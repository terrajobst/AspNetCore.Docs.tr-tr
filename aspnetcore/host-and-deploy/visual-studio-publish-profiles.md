---
title: "Visual Studio yayımlama profilleri ASP.NET Core uygulama dağıtımı için"
author: rick-anderson
description: "Nasıl oluşturulacağını Bul Visual Studio'da yayımlama profillerini ASP.NET Core uygulamaları için."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 138b60d0e7c2a3d8848d534ffed854feaf0f5661
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Visual Studio yayımlama profilleri ASP.NET Core uygulama dağıtımı için

Tarafından [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu makalede oluşturmak için Visual Studio 2017 kullanmaya odaklanır yayımlama profilleri. Visual Studio ile oluşturulan yayımlama profilleri MSBuild ve Visual Studio 2017 çalıştırabilirsiniz. Makale yayımlama işleminin ayrıntıları sağlar. Bkz: [Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) yayımlama için yönergeler için.

Aşağıdaki *.csproj* dosyası komutuyla oluşturuldu `dotnet new mvc`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

`Sdk` Özniteliğini `<Project>` öğesi (ilk satırı) yukarıdaki biçimlendirme aşağıdakileri yapar:

* Özellikler dosyasından içeri aktarır *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* başında.
* Hedef dosyadan içeri aktarır *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* sonunda.

Varsayılan konumu `MSBuildSDKsPath` (Visual Studio 2017 kuruluş ile) *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* klasör.

`Microsoft.NET.Sdk.Web`aşağıdakilere bağlıdır:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Aşağıdaki özellikler ve içeri aktarılacak hedefleri neden olur:

* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets

Kullanılan Yayımla yöntemine göre hedefler hakkını ayarlayın hedefleri alma yayımlayın.

MSBuild veya Visual Studio Proje yüklediğinde, aşağıdaki yüksek düzey eylemleri gerçekleştirilir:

* Proje derleme
* Yayımlanacak dosyaları işlem
* Dosyaları hedefe yayımlama

### <a name="compute-project-items"></a>Proje öğeleri işlem

Proje yüklendiğinde, proje öğeleri (dosyaları) hesaplanır. `item type` Özniteliği dosya nasıl işleneceğini belirler. Varsayılan olarak, *.cs* dosyaları dahil edilmiştir `Compile` öğe listesi. Dosyalar `Compile` öğe listesi derlenir.

`Content` Öğesi listesinin yanı sıra yapı çıkışları yayımlanan dosyaları içerir. Varsayılan olarak, dosyaları desen eşleştirme `wwwroot/**` içinde yer alan `Content` öğesi. [wwwroot /\* \* genelleme deseni](https://gruntjs.com/configuring-tasks#globbing-patterns) tüm dosyalarda belirtir *wwwroot* klasörü **ve** alt klasörler. Açıkça Yayımla listesine bir dosya eklemek için doğrudan dosyasına ekleyin *.csproj* dosya gösterildiği gibi [dosyaları dahil](#including-files).

Seçerken **Yayımla** düğmesi Visual Studio'da veya komut satırından yayımlama sırasında:

* Özellikler/öğeleri hesaplanır (oluşturmak için gereken dosyaları).
* Yalnızca Visual Studio: NuGet paketleri geri yüklenir. (Geri yükleme CLI kullanıcı tarafından açık olması gerekir.)
* Proje oluşturur.
* Yayımla öğeleri hesaplanır (yayımlama için gerekli olan dosyalar).
* Proje yayımlanır. (Hesaplanan dosyalar Yayımla hedefe kopyalanır.)

ASP.NET Core projesinde başvurduğunda `Microsoft.NET.Sdk.Web` proje dosyasında bir *app_offline.htm* dosyası, web uygulama dizini kökünde yerleştirilir. Dosyanın mevcut olduğunda, ASP.NET Core modülü düzgün biçimde uygulamasını kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya. Daha fazla bilgi için bkz: [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#appofflinehtm).

## <a name="basic-command-line-publishing"></a>Temel komut satırı yayımlama

Komut satırı yayımlama tüm .NET Core desteklenen platformlarda çalışır ve Visual Studio gerektirmez. Aşağıdaki örnekte `dotnet publish` komutu proje dizinden çalıştırın (içeren *.csproj* dosyası). Aksi durumda proje klasöründe açıkça proje dosya yolunda geçirin. Örneğin:

```console
dotnet publish c:/webs/web1
```

Oluşturma ve bir web uygulaması yayımlama için aşağıdaki komutları çalıştırın:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

`dotnet publish` Aşağıdakine benzer bir çıktı üretir:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

Varsayılan klasör yayımlama olduğu `bin\$(Configuration)\netcoreapp<version>\publish`. İçin varsayılan `$(Configuration)` Hata Ayıkla'dır. Yukarıdaki örnekteki `<TargetFramework>` olan `netcoreapp2.0`.

`dotnet publish -h`bilgi yayımlama için yardımı görüntüler.

Aşağıdaki komut belirtir bir `Release` oluşturma ve yayımlama dizini:

```console
dotnet publish -c Release -o C:/MyWebs/test
```

`dotnet publish` Komutu çağıran çağıran MSBuild `Publish` hedef. Herhangi bir parametre geçirilen `dotnet publish` MSBuild için geçirilir. `-c` Parametresi eşlendiğini `Configuration` MSBuild özelliği. `-o` Parametresi eşlendiğini `OutputPath`.

MSBuild özellikleri aşağıdaki biçimlerden birini kullanarak geçirilebilir:

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Aşağıdaki komutu yayımlayan bir `Release` bir ağ paylaşımına oluşturun:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Ağ paylaşımı eğik çizgi ile belirtilir (*//r8/*) ve tüm desteklenen .NET Core platformlarında çalışır.

Yayımlanan uygulama dağıtımı için çalışmayan onaylayın. Dosyalar *yayımlama* uygulama çalışırken klasörü kilitlenir. Dağıtım kilitli olduğundan dosyaları kopyalanamaz gerçekleşemez.

## <a name="publish-profiles"></a>Yayımlama profilleri

Bu bölümde Visual Studio 2017 ve daha yüksek yayımlama profillerini oluşturmak için kullanır. Visual Studio veya komut satırı yayımlama oluşturulduktan sonra kullanılabilir.

Yayımlama profilleri yayımlama işlemini basitleştirir. Birden fazla yayımlama profili bulunabilir. Visual Studio'da bir yayımlama profili oluşturmak için Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Yayımla**. Alternatif olarak, seçin **Yayımla \<proje adı >** yapı menüsünde. **Yayımla** uygulama kapasiteleri sayfasında görüntülenir. Proje bir yayımlama profili içermiyorsa, aşağıdaki sayfayı görüntülenir:

![Azure, IIS, FTB, Azure klasörüyle gösteren uygulama kapasiteleri sayfası Yayımla sekmesi seçili. Ayrıca gösterir yeni oluşturabilir ve Exiting radyo düğmeleri seçin](visual-studio-publish-profiles/_static/az.png)

Zaman **klasörü** seçili **Yayımla** düğmesi, bir klasör oluşturur yayımlama profili ve yayımlar.

![** Gösteren Azure, IIS, FTB, klasör uygulama kapasiteleri sayfasının sekmesinde Yayımla **](visual-studio-publish-profiles/_static/pub1.png)

Bir yayımlama profili oluşturulduktan sonra **Yayımla** sekmesinde değişiklikleri ve seçin **yeni profil oluşturmak** yeni bir profil oluşturmak için.

![** FolderProfile gösteren uygulama kapasiteleri sayfasının sekmesinde Yayımla **-oluşturulan son adımı ve yeni profil bağlantısını oluşturma](visual-studio-publish-profiles/_static/create_new.png)

Yayımlama Sihirbazı'nı aşağıdaki Yayımla hedefleri destekler:

* Microsoft Azure uygulama hizmeti
* IIS, FTP, vb. (için herhangi bir web sunucusu)
* Klasör
* Profilini içeri aktarma (Profil alma izin verir).
* Microsoft Azure sanal makineler

Bkz: [hangi yayımlama seçeneklerini benim için en uygun?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) daha fazla bilgi için.

Bir yayımlama profili Visual Studio ile oluştururken bir *özellikleri/PublishProfiles/\<Yayımla adı > .pubxml* MSBuild dosyası oluşturulur. Bu *.pubxml* dosyası MSBuild dosyadır ve içeren yapılandırma ayarlarını yayımlayın. Bu dosya, yapı özelleştirmek ve işlem yayımlamak için değiştirilebilir. Bu dosya yayımlama işlemi tarafından okunur. `<LastUsedBuildConfiguration>`Genel bir özellik olduğundan ve yapı alınan herhangi bir dosya olmamalıdır özeldir. Bkz: [MSBuild: Yapılandırma özelliğinin nasıl ayarlandığını](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) daha fazla bilgi için.
*.Pubxml* dosya döndürmemelidir kontrol edileceği kaynak denetimine bağımlı olduğundan dolayı *.kullanıcı* dosya. *.Kullanıcı* dosya hiç işaretlenmelidir kaynak denetimine hassas bilgileri içerebileceği ve yalnızca bir kullanıcı ve makine için geçerli olur.

(Yayımla parola gibi) hassas bilgileri şifrelenir bir kullanıcı/makine düzeyi ve depolanan başına *özellikleri/PublishProfiles/\<Yayımla adı >. pubxml.user* dosya. Bu dosya hassas bilgileri içerdiğinden gereken **değil** kaynak denetimine iade edilmelidir.

ASP.NET Core üzerinde bir web uygulaması yayımlama genel bakış için bkz: [konak dağıtıp](index.md). [Ana bilgisayar ve dağıtma](index.md) https://github.com/aspnet/websdk adresindeki bir açık kaynaklı proje.

`dotnet publish`klasörü, MSDeploy, kullanabilirsiniz ve [KUDU](https://github.com/projectkudu/kudu/wiki) yayımlama profilleri:
 
Klasör (platformlar arası çalışır):`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`

MSDeploy (şu anda bu yalnızca çalışır MSDeploy platformlar arası değil bu yana Windows'ta):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`

MSDeploy paketi (şu anda bu yalnızca çalışır MSDeploy platformlar arası değil bu yana Windows'ta):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`

Önceki örnekte **yok** geçirmek `deployonbuild` için `dotnet publish`.

Daha fazla bilgi için bkz: [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish`herhangi bir platform için Azure yayımlama için KUDU API'lerini destekler. Visual Studio KUDU API'leri ancak desteklenen websdk tarafından çapraz plat yayımlama için Azure'a mu destek yayımlayın.

Bir yayımlama profili Ekle *özellikleri/PublishProfiles* klasöründe aşağıdaki içeriğe sahip:

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

Aşağıdaki komutu çalıştırarak Yayımla içerikleri zips ve KUDU API'lerini kullanarak Azure yayımlama:

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

Bir yayımlama profili kullanırken aşağıdaki MSBuild özellikleri ayarlayın:

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

Adlı bir profille yayımlarken *FolderProfile*, aşağıdaki komutlardan birini çalıştırılabilir:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Çağrılırken, `dotnet build`, çağırır `msbuild` yapı çalıştırın ve işlem yayınlamak için. Çağırma `dotnet build` veya `msbuild` bir klasör profilinde geçirilirken temelde eşdeğerdir. MSBuild doğrudan Windows çağrılırken MSBuild .NET Framework sürümü kullanılır. MSDeploy, Windows makineler yayımlama için şu anda sınırlıdır. Çağırma `dotnet build` klasörde olmayan profili MSBuild çağırır ve MSBuild klasörü olmayan profilleri MSDeploy kullanır. Çağırma `dotnet build` bir klasör olmayan profilindeki (MSDeploy kullanarak) MSBuild çağırır ve (bile Windows platformu üzerinde çalışırken) bir hatayla sonuçlanır. Bir klasör olmayan profili ile yayımlamak için MSBuild doğrudan çağırabilir.

Aşağıdaki klasörü yayımlama profili Visual Studio ile oluşturulmuş ve bir ağ paylaşımına yayımlar:

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

Not `<LastUsedBuildConfiguration>` ayarlanır `Release`. Visual Studio'dan yayımlarken `<LastUsedBuildConfiguration>` yapılandırma özellik değeri, yayımlama işlemi başlatıldığında değeri kullanılarak ayarlanır. `<LastUsedBuildConfiguration>` Yapılandırma özelliği özeldir ve içeri aktarılan bir MSBuild dosyasında geçersiz kılınmış döndürmemelidir. Bu özellik, komut satırında geçersiz kılınabilir. Örneğin:

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

MSBuild kullanma:

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>MSDeploy uç noktasına komut satırından yayımlama

Daha önce belirtildiği gibi yayımlama kullanılarak gerçekleştirilebilir `dotnet publish` veya `msbuild` komutu. `dotnet publish`.NET Core bağlamında çalışır. `msbuild` Komutu .NET framework gerektirir ve bu nedenle Windows ortamları sınırlıdır.

En kolay yolu MSDeploy ile yayımlamak için önce bir yayımlama profili Visual Studio 2017 içinde oluşturmak ve komut satırından profil kullanmaktır.

Aşağıdaki örnekte, bir ASP.NET Core web uygulaması oluşturuldu (kullanarak `dotnet new mvc`), ve bir Azure yayımlama profili Visual Studio ile eklenir.

Çalıştırma `msbuild` gelen bir **VS 2017 için geliştirici komut istemi**. Geliştirici komut istemi doğru sahip *msbuild.exe* yolundaki bazı MSBuild değişkenler kümesine sahip.

MSBuild aşağıdaki söz dizimini kullanır:

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

Alma `Password` gelen  *\<Yayımla adı >. PublishSettings* dosya. Karşıdan *. PublishSettings* ya da dosyasından:

* Çözüm Gezgini: Web uygulamasında sağ tıklatın ve seçin **yayımlama profili indirme**.
* Azure Yönetim Portalı: Seçin **Get yayımlama profili** Web uygulaması dikey penceresinden.

`Username`Yayımlama profili bulunabilir.

Aşağıdaki örnek kullanır "Web11112 – Web Dağıtımı" yayımlama profili:

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a>Dosyaları dışlama

ASP.NET Core web uygulamaları, derleme yapıları ve içeriğini yayımlarken *wwwroot* klasörü dahil. `msbuild`destekleyen [genelleme desenleri](https://gruntjs.com/configuring-tasks#globbing-patterns). Örneğin, aşağıdaki `<Content>` öğesi biçimlendirme hariç tüm metni (*.txt*) dosyaları buradan *wwwroot/içerik* klasör ve tüm alt klasörlerini.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Yukarıdaki biçimlendirme bir yayımlama profili eklenebilir veya *.csproj* dosya. Eklendiğinde *.csproj* dosya, kural eklenmiş olup projedeki tüm yayımlama profilleri.

Aşağıdaki `<MsDeploySkipRules>` öğesi biçimlendirme içerenleri tüm dosyaları *wwwroot/içerik* klasörü:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>`silmez *atla* dağıtım sitesinden hedefler. `<Content>`hedef dosyalar ve klasörler dağıtım site veritabanından silinir. Örneğin, aşağıdaki dosyaları dağıtılan web uygulamasının vardı varsayın:

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Aşağıdaki `<MsDeploySkipRules>` biçimlendirme eklenir, dağıtım sitesinde bu dosyaların silinmesi olmayacaktır.

``` xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` Yukarıda gösterilen biçimlendirme engeller *atlandı* depoyed engeller dosyaları dağıtmış sonra bu dosyaları silmez ancak.

Aşağıdaki `<Content>` biçimlendirme dağıtım sitede hedeflenen dosya siler:

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Komut satırı dağıtımı ile kullanarak `<Content>` aşağıdakine benzer bir çıktı sonuçlarında yukarıda biçimlendirme:

``` console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="including-files"></a>Dosyaları dahil olmak üzere

Aşağıdaki biçimlendirme içeren bir *görüntüleri* klasörünün dışında proje dizinine *wwwroot/görüntüleri* Yayımla sitesinin klasörüne:

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

İşaretleme eklenebilir *.csproj* dosya veya yayımlama profili. İçin eklediyseniz *.csproj* dosyası, onu eklendi projesinde her yayımlama profilinde.

Aşağıdaki biçimlendirme gösterir nasıl vurgulanmış için:

* Projeye dışında dosyasından kopyalama *wwwroot* klasör.
* Dışlama *wwwroot\Content* klasör.
* Dışlama *Views\Home\About2.cshtml*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

Bkz: [WebSDK Benioku](https://github.com/aspnet/websdk) daha fazla dağıtım örnekleri için.

### <a name="run-a-target-before-or-after-publishing"></a>Bir hedef önce veya sonra yayımlama çalıştırın

Yerleşik `BeforePublish` ve `AfterPublish` hedefleri, bir hedef önce veya sonra yayımlama hedefi yürütmek için kullanılabilir. Aşağıdaki biçimlendirmede önce ve sonra yayımlama konsol çıktısı iletilerini günlüğe kaydetmek için yayımlama profili eklenebilir:

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a>Kudu hizmeti

Dosyaları görüntülemek için bir Azure uygulama hizmeti web uygulama dağıtımı, kullanım [Kudu hizmet](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Append `scm` belirteci web uygulamasının adı. Örneğin:

| URL                                    | Sonuç      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | Web uygulaması     |
| `http://mysite.scm.azurewebsites.net/` | Kudu hizmeti |

Seçin [hata ayıklama Konsolu'nda](https://github.com/projectkudu/kudu/wiki/Kudu-console) dosyaları görünüm/düzenleme/silme/Ekle menü öğesine.

## <a name="additional-resources"></a>Ek kaynaklar

* [Web dağıtımı](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) web uygulamaları ve IIS sunucuları için Web siteleri dağıtımını basitleştirir.
* [https://github.com/ASPNET/websdk](https://github.com/aspnet/websdk/issues): dosya sorunları ve dağıtım için özelliklerini isteyin.
