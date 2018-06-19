---
title: Visual Studio yayımlama profilleri ASP.NET Core uygulama dağıtımı için
author: rick-anderson
description: Oluşturmayı öğrenin Visual Studio'da yayımlama profillerini ve ASP.NET Core uygulama dağıtımlarını çeşitli hedeflere yönetmek için kullanın.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483376"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Visual Studio yayımlama profilleri ASP.NET Core uygulama dağıtımı için

Tarafından [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu belgeyi oluşturmak ve kullanmak için Visual Studio 2017 kullanmaya odaklanır yayımlama profilleri. Visual Studio ile oluşturulan yayımlama profilleri MSBuild ve Visual Studio 2017 çalıştırabilirsiniz. Bkz: [Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) yayımlama için yönergeler için.

Aşağıdaki proje dosyası komutuyla oluşturulan `dotnet new mvc`:

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

`<Project>` Öğenin `Sdk` özniteliği aşağıdaki görevleri gerçekleştirir:

* Özellikler dosyasından içeri aktarır *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* başında.
* Hedef dosyadan içeri aktarır *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* sonunda.

Varsayılan konumu `MSBuildSDKsPath` (Visual Studio 2017 kuruluş ile) *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* klasör.

`Microsoft.NET.Sdk.Web` SDK bağlıdır:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Aşağıdaki özellikler ve içeri aktarılacak hedefleri neden olur:

* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*

Kullanılan Yayımla yöntemine göre hedefler hakkını ayarlayın hedefleri alma yayımlayın.

MSBuild veya Visual Studio Proje yüklediğinde, aşağıdaki üst düzey eylemler gerçekleşir:

* Proje derleme
* Yayımlanacak dosyaları işlem
* Dosyaları hedefe yayımlama

## <a name="compute-project-items"></a>Proje öğeleri işlem

Proje yüklendiğinde, proje öğeleri (dosyaları) hesaplanır. `item type` Özniteliği dosya nasıl işleneceğini belirler. Varsayılan olarak, *.cs* dosyaları dahil edilmiştir `Compile` öğe listesi. Dosyalar `Compile` öğe listesi derlenir.

`Content` Öğesi listesinin yanı sıra yapı çıkışları yayımlanan dosyaları içerir. Varsayılan olarak, dosyaları desen eşleştirme `wwwroot/**` içinde yer alan `Content` öğesi. `wwwroot/\*\*` [Genelleme düzeni](https://gruntjs.com/configuring-tasks#globbing-patterns) tüm dosyalarda eşleşen *wwwroot* klasörü **ve** alt klasörler. Açıkça Yayımla listesine bir dosya eklemek için doğrudan dosyasına ekleyin *.csproj* dosya gösterildiği gibi [dosyaları içerir](#include-files).

Seçerken **Yayımla** düğmesi Visual Studio'da veya komut satırından yayımlama sırasında:

* Özellikler/öğeleri hesaplanır (oluşturmak için gereken dosyaları).
* **Yalnızca Visual Studio**: NuGet paketleri geri yüklenir. (Geri yükleme CLI kullanıcı tarafından açık olması gerekir.)
* Proje oluşturur.
* Yayımla öğeleri hesaplanır (yayımlama için gerekli olan dosyalar).
* Proje yayımlanan (hesaplanan dosyalar Yayımla hedefe kopyalanır).

ASP.NET Core projesinde başvurduğunda `Microsoft.NET.Sdk.Web` proje dosyasında bir *app_offline.htm* dosyası, web uygulama dizini kökünde yerleştirilir. Dosyanın mevcut olduğunda, ASP.NET Core modülü düzgün biçimde uygulamasını kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya. Daha fazla bilgi için bkz: [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).

## <a name="basic-command-line-publishing"></a>Temel komut satırı yayımlama

Komut satırı yayımlama tüm .NET Core desteklenen platformlarda çalışır ve Visual Studio gerektirmez. Aşağıdaki örnekte [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu proje dizinden çalıştırın (içeren *.csproj* dosyası). Aksi durumda proje klasöründe açıkça proje dosya yolunda geçirin. Örneğin:

```console
dotnet publish C:\Webs\Web1
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

[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu, aşağıdakine benzer bir çıktı üretir:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

Varsayılan klasör yayımlama olduğu `bin\$(Configuration)\netcoreapp<version>\publish`. İçin varsayılan `$(Configuration)` olan *hata ayıklama*. Önceki örnekte, `<TargetFramework>` olan `netcoreapp2.0`.

`dotnet publish -h` bilgi yayımlama için yardımı görüntüler.

Aşağıdaki komut belirtir bir `Release` oluşturma ve yayımlama dizini:

```console
dotnet publish -c Release -o C:\MyWebs\test
```

[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) çağrıları çağırır MSBuild komut `Publish` hedef. Herhangi bir parametre geçirilen `dotnet publish` MSBuild için geçirilir. `-c` Parametresi eşlendiğini `Configuration` MSBuild özelliği. `-o` Parametresi eşlendiğini `OutputPath`.

MSBuild özellikleri aşağıdaki biçimlerden birini kullanarak geçirilebilir:

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Aşağıdaki komutu yayımlayan bir `Release` bir ağ paylaşımına oluşturun:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Ağ paylaşımı eğik çizgi ile belirtilir (*//r8/*) ve tüm desteklenen .NET Core platformlarında çalışır.

Yayımlanan uygulama dağıtımı için çalışmayan onaylayın. Dosyalar *yayımlama* uygulama çalışırken klasörü kilitlenir. Dağıtım kilitli olduğundan dosyaları kopyalanamaz gerçekleşemez.

## <a name="publish-profiles"></a>Yayımlama profilleri

Bu bölümde, bir yayımlama profili oluşturmak için Visual Studio 2017 kullanır. Visual Studio veya komut satırı yayımlama oluşturulduktan sonra kullanılabilir.

Yayımlama profilleri yayımlama işlemini basitleştirmek ve herhangi bir sayıda profilleri bulunabilir. Bir yayımlama profili Visual Studio aşağıdaki yollardan birini seçerek oluşturun:

* Çözüm Gezgini'nde projeye sağ tıklayıp **Yayımla**.
* Seçin **Yayımla &lt;project_name&gt;**  gelen **yapı** menüsü.

**Yayımla** uygulama kapasiteleri sayfasında görüntülenir. Bir yayımlama profili proje sahip değilse, aşağıdaki sayfayı görüntülenir:

![Uygulama kapasiteleri sayfasında Yayımla sekmesi](visual-studio-publish-profiles/_static/az.png)

Zaman **klasörü** olduğunu belirlenirse, yayımlanan varlıkları depolamak için bir klasör yolu belirtin. Varsayılan klasör *bin\Release\PublishOutput*. Tıklatın **profili oluştur** tamamlanması düğmesi.

Bir yayımlama profili oluşturulduktan sonra **Yayımla** sekmesinde değişiklikleri. Yeni oluşturulan profil açılır listede görüntülenir. Tıklatın **yeni profil oluşturmak** başka bir yeni profili oluşturmak için.

![FolderProfile gösteren uygulama kapasiteleri sayfasına Yayımla sekmesi](visual-studio-publish-profiles/_static/create_new.png)

Yayımlama Sihirbazı'nı aşağıdaki Yayımla hedefleri destekler:

* Azure uygulama hizmeti
* Azure sanal makineler
* IIS, FTP, vb. (için herhangi bir web sunucusu)
* Klasör
* Profilini içeri aktarma

Daha fazla bilgi için bkz: [hangi yayımlama seçeneklerini benim için en uygun](/visualstudio/ide/not-in-toc/web-publish-options).

Bir yayımlama profili Visual Studio ile oluştururken bir *özellikleri/PublishProfiles/&lt;Profil_adı&gt;.pubxml* MSBuild dosyası oluşturulur. *.Pubxml* dosyası MSBuild dosyadır ve içeren yapılandırma ayarlarını yayımlayın. Bu dosya, yapı özelleştirmek ve işlem yayımlamak için değiştirilebilir. Bu dosya yayımlama işlemi tarafından okunur. `<LastUsedBuildConfiguration>` Genel bir özellik olduğundan ve yapı alınan herhangi bir dosya olmamalıdır özeldir. Bkz: [MSBuild: Yapılandırma özelliğinin nasıl ayarlandığını](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) daha fazla bilgi için.

Bir Azure hedefine yayımlarken *.pubxml* dosyası, Azure abonelik tanımlayıcısı içeriyor. Bu hedef türü ile bu dosya kaynak denetimine ekleme önerilmez. Bir Azure olmayan hedefine yayımlarken iade güvenli *.pubxml* dosya.

(Yayımla parola gibi) hassas bilgileri şifrelenir bir kullanıcı/makine gerçekleştiriliyordu. İçinde depolanan *özellikleri/PublishProfiles/&lt;Profil_adı&gt;. pubxml.user* dosya. Bu dosya hassas bilgileri depolayabileceğiniz için kaynak denetimine iade döndürmemelidir.

ASP.NET Core üzerinde bir web uygulaması yayımlama genel bakış için bkz: [konak dağıtıp](xref:host-and-deploy/index). MSBuild görevleri ve ASP.NET Core uygulama yayımlamak için gerekli hedefleri açık kaynak adresindeki https://github.com/aspnet/websdk.

`dotnet publish` klasörü, MSDeploy, kullanabilirsiniz ve [Kudu](https://github.com/projectkudu/kudu/wiki) yayımlama profilleri:

Klasör (platformlar arası çalışır):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy (şu anda bu yalnızca çalışır MSDeploy platformlar arası değil bu yana Windows'ta):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

MSDeploy paketi (şu anda bu yalnızca çalışır MSDeploy platformlar arası değil bu yana Windows'ta):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

Önceki örnekte **yok** geçirmek `deployonbuild` için `dotnet publish`.

Daha fazla bilgi için bkz: [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` Kudu herhangi bir platform için Azure yayımlama için API'ler destekler. Visual Studio yayımlama Kudu API'ları, ancak desteklenen tarafından WebSDK platformlar arası yayımlama için Azure'a destekler.

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

Yayımla içerikleri zip ve Kudu API'lerini kullanarak Azure yayımlamak için aşağıdaki komutu çalıştırın:

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

Bir yayımlama profili kullanırken aşağıdaki MSBuild özellikleri ayarlayın:

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

Adlı bir profille yayımlarken *FolderProfile*, aşağıdaki komutlardan birini çalıştırılabilir:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Çağrılırken, [dotnet yapı](/dotnet/core/tools/dotnet-build), çağırır `msbuild` yapı çalıştırın ve işlem yayınlamak için. Ya da çağırma `dotnet build` veya `msbuild` bir klasör profilinde geçirilirken eşdeğerdir. MSBuild doğrudan Windows çağrılırken MSBuild .NET Framework sürümü kullanılır. MSDeploy, Windows makineler yayımlama için şu anda sınırlıdır. Çağırma `dotnet build` klasörde olmayan profili MSBuild çağırır ve MSBuild klasörü olmayan profilleri MSDeploy kullanır. Çağırma `dotnet build` bir klasör olmayan profilindeki (MSDeploy kullanarak) MSBuild çağırır ve (bile Windows platformu üzerinde çalışırken) bir hatayla sonuçlanır. Bir klasör olmayan profili ile yayımlamak için MSBuild doğrudan çağırabilir.

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

Not `<LastUsedBuildConfiguration>` ayarlanır `Release`. Visual Studio'dan yayımlarken `<LastUsedBuildConfiguration>` yapılandırma özellik değeri, yayımlama işlemi başlatıldığında değeri kullanılarak ayarlanır. `<LastUsedBuildConfiguration>` Yapılandırma özelliği özeldir ve içeri aktarılan bir MSBuild dosyasında geçersiz kılınmış döndürmemelidir. Bu özellik, komut satırında geçersiz kılınabilir.

.NET Core CLI kullanarak:

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

MSBuild kullanma:

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>MSDeploy uç noktasına komut satırından yayımlama

Yayımlama .NET Core CLI veya MSBuild kullanılarak gerçekleştirilebilir. `dotnet publish` .NET Core bağlamında çalışır. `msbuild` Komutu Windows ortamları için sınırlar .NET Framework gerektirir.

En kolay yolu MSDeploy ile yayımlamak için önce bir yayımlama profili Visual Studio 2017 içinde oluşturmak ve komut satırından profil kullanmaktır.

Aşağıdaki örnekte, bir ASP.NET Core web uygulaması oluşturuldu (kullanarak `dotnet new mvc`), ve bir Azure yayımlama profili Visual Studio ile eklenir.

Çalıştırma `msbuild` gelen bir **VS 2017 için geliştirici komut istemi**. Geliştirici komut istemi doğru sahip *msbuild.exe* yolundaki bazı MSBuild değişkenler kümesine sahip.

MSBuild aşağıdaki söz dizimini kullanır:

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

Alma `Password` gelen  *\<Yayımla adı >. PublishSettings* dosya. Karşıdan *. PublishSettings* ya da dosyasından:

* Çözüm Gezgini: Web uygulamasında sağ tıklatın ve seçin **yayımlama profili indirme**.
* Azure portal: tıklatın **Get yayımlama profili** Web uygulamanızın üzerinde **genel bakış** paneli.

`Username` Yayımlama profili bulunabilir.

Aşağıdaki örnek kullanır *Web11112 - Web dağıtımı* yayımlama profili:

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a>Dosyaları dışarıda bırak

ASP.NET Core web uygulamaları, derleme yapıları ve içeriğini yayımlarken *wwwroot* klasörü dahil. `msbuild` destekleyen [genelleme desenleri](https://gruntjs.com/configuring-tasks#globbing-patterns). Örneğin, aşağıdaki `<Content>` öğesi hariç tüm metni (*.txt*) dosyaları buradan *wwwroot/içerik* klasör ve tüm alt klasörlerini.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Önceki biçimlendirme bir yayımlama profili eklenebilir veya *.csproj* dosya. Eklendiğinde *.csproj* dosya, kural eklenmiş olup projedeki tüm yayımlama profilleri.

Aşağıdaki `<MsDeploySkipRules>` öğesi hariç tüm dosyaları *wwwroot/içerik* klasörü:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` silmez *atla* dağıtım sitesinden hedefler. `<Content>` hedef dosyalar ve klasörler dağıtım site veritabanından silinir. Örneğin, aşağıdaki dosyaları dağıtılan web uygulamasının vardı varsayın:

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Aşağıdaki `<MsDeploySkipRules>` öğeleri eklendiğinde, bu dosyaları dağıtım sitesinde silinmiş olmayacaktır.

```xml
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

Yukarıdaki `<MsDeploySkipRules>` öğeleri engellemek *atlandı* dağıtılan dosyaları. Dağıtmış sonra bu dosyaları silmez.

Aşağıdaki `<Content>` öğesi dağıtım sitede hedeflenen dosya siler:

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Önceki komut satırı dağıtım kullanarak `<Content>` öğesi şu çıkışı üretir:

```console
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

## <a name="include-files"></a>Dosyaları dahil etme

Aşağıdaki biçimlendirme içeren bir *görüntüleri* klasörünün dışında proje dizinine *wwwroot/görüntüleri* Yayımla sitesinin klasörüne:

```xml
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

## <a name="run-a-target-before-or-after-publishing"></a>Bir hedef önce veya sonra yayımlama çalıştırın

Yerleşik `BeforePublish` ve `AfterPublish` hedefleri önce veya sonra yayımlama hedefi bir hedef yürütün. Yayımlama profili öncesinde ve sonrasında yayımlama konsol iletilerini günlüğe kaydetmek için aşağıdaki öğeleri ekleyin:

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>Güvenilmeyen bir sertifika kullanarak bir sunucuya yayımlayın

Ekleme `<AllowUntrustedCertificate>` özellik değerini `True` yayımlama profili için:

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Kudu hizmeti

Azure App Service web uygulama dağıtımı dosyaları görüntülemek için kullanın [Kudu hizmet](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Append `scm` web uygulaması adı belirteci. Örneğin:

| URL                                    | Sonuç       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Web uygulaması      |
| `http://mysite.scm.azurewebsites.net/` | Kudu hizmeti |

Seçin [hata ayıklama Konsolu'nda](https://github.com/projectkudu/kudu/wiki/Kudu-console) görüntüleme, düzenleme, silme veya dosyaları eklemek için kullanılan menü öğesi.

## <a name="additional-resources"></a>Ek kaynaklar

* [Web dağıtımı](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) web uygulamaları ve IIS sunucuları için Web siteleri dağıtımını basitleştirir.
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Dosya sorunları ve dağıtım için özelliklerini isteyin.
