---
title: Visual Studio yayımlama profilleri için ASP.NET Core uygulaması dağıtımı
author: rick-anderson
description: Oluşturmayı Visual Studio'da yayımlama profilleri ve ASP.NET Core uygulama dağıtımlarını çeşitli hedeflere yönetmek için kullanın.
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 751f25f74a0e24eb9ce4f2bd6b2fa462ccb03ecb
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538407"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Visual Studio yayımlama profilleri için ASP.NET Core uygulaması dağıtımı

Tarafından [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu belge oluşturma ve kullanma için Visual Studio 2017'yi kullanmaya odaklanmıştır yayımlama profilleri. Visual Studio ile oluşturulan yayımlama profillerine MSBuild ve Visual Studio 2017 ' çalıştırabilirsiniz. Bkz: [Visual Studio kullanarak Azure App Service'e bir ASP.NET Core web uygulaması yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) Azure'da yayımlamak için yönergeler.

Aşağıdaki proje dosyasını komutu ile oluşturulmuş `dotnet new mvc`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.1.4" />
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

* Özellikleri dosyasından içeri aktarır *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* başında.
* Hedefleri dosyasından içeri aktarır *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* sonunda.

Varsayılan konumu `MSBuildSDKsPath` (Visual Studio 2017 Enterprise ile) *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* klasör.

`Microsoft.NET.Sdk.Web` SDK bağlıdır:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Neden olan aşağıdaki özellikleri ve içeri aktarılacak hedefler:

* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*

Kullanılan yayımlama yöntemi temel hedefleri sağ kümesini hedefler içe yayımlayın.

MSBuild veya Visual Studio bir projeyi yüklediğinde, aşağıdaki üst düzey eylemler gerçekleşir:

* Proje derleme
* Yayımlamak için dosyaların işlem
* Hedef dosya yayımlama

## <a name="compute-project-items"></a>Proje öğeleri işlem

Proje yüklendiğinde, proje öğeler (dosyalar) hesaplanır. `item type` Özniteliği dosyanın nasıl işleneceğini belirler. Varsayılan olarak, *.cs* dosyaları dahil edilecek `Compile` öğe listesi. Dosyalar `Compile` öğe listesi derlenir.

`Content` Öğesi listesinin yanı sıra derleme çıktılarını yayımlanan dosyaları içerir. Varsayılan olarak, dosyaları desen eşleştirme `wwwroot/**` dahil `Content` öğesi. `wwwroot/\*\*` [Glob deseni](https://gruntjs.com/configuring-tasks#globbing-patterns) eşleşen tüm dosyaları *wwwroot* klasör **ve** alt. Açıkça Yayımla listesine bir dosya eklemek için dosyanın doğrudan ekleme *.csproj* gösterildiği gibi dosya [dosyaları içerir](#include-files).

Seçerken **Yayımla** düğme Visual Studio'da veya komut satırından yayımlama sırasında:

* Özellikler/öğeleri hesaplanır (oluşturmak için gereken dosyaları).
* **Yalnızca Visual Studio**: NuGet paketlerini geri yüklenir. (Geri yükleme CLI kullanıcı tarafından açık olması gerekir.)
* Projeyi oluşturur.
* Yayımlama öğeleri hesaplanır (yayımlama için gerekli dosyaları).
* Proje yayımlandığında (hesaplanan dosyalar Yayımla hedefe kopyalanır).

Bir ASP.NET Core projesi başvurduğunda `Microsoft.NET.Sdk.Web` proje dosyasında bir *app_offline.htm* dosya, web uygulama dizini kökünde yerleştirilir. Dosya varsa, ASP.NET Core modülü düzgün bir şekilde uygulamayı kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya. Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).

## <a name="basic-command-line-publishing"></a>Temel bir komut satırı yayımlama

Komut satırı yayımlama, .NET Core tarafından desteklenen tüm platformlarda çalışır ve Visual Studio gerektirmez. Aşağıdaki örnekte [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu proje dizininden çalıştırın (içeren *.csproj* dosyası). Aksi durumda proje klasöründe proje dosya yolu açıkça geçirebilirsiniz. Örneğin:

```console
dotnet publish C:\Webs\Web1
```

Oluşturma ve bir web uygulaması yayımlamak için aşağıdaki komutları çalıştırın:

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

[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komut aşağıdakine benzer bir çıktı üretir:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

Varsayılan publish klasörüne olan `bin\$(Configuration)\netcoreapp<version>\publish`. İçin varsayılan `$(Configuration)` olduğu *hata ayıklama*. Önceki örnekte, `<TargetFramework>` olduğu `netcoreapp2.0`.

`dotnet publish -h` bilgi yayımlama için yardımı görüntüler.

Aşağıdaki komut belirtir bir `Release` derleme ve yayımlama dizini:

```console
dotnet publish -c Release -o C:\MyWebs\test
```

[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) çağrıları çağıran MSBuild komut `Publish` hedef. Herhangi bir parametre geçirilen `dotnet publish` MSBuild'e geçirilir. `-c` Parametre eşlenen `Configuration` MSBuild özelliği. `-o` Parametre eşlenen `OutputPath`.

MSBuild özellikleri, aşağıdaki biçimlerden birini kullanarak geçirilebilir:

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Aşağıdaki komutu yayımlar bir `Release` bir ağ paylaşımına oluşturun:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Ağ paylaşımı eğik çizgi ile belirtilen (*//r8/*) ve .NET Core desteklenen tüm platformlarda çalışır.

Yayımlanan uygulama dağıtımı için çalışmadığından emin onaylayın. Dosyalar *yayımlama* klasörü, uygulama çalışırken kilitlenir. Dağıtım kilitli olduğundan dosya kopyalanamadı oluşamaz.

## <a name="publish-profiles"></a>Yayımlama profilleri

Bu bölümde, bir yayımlama profili oluşturmak için Visual Studio 2017 kullanılır. Visual Studio ya da komut satırından yayımlama oluşturulduktan sonra kullanılabilir.

Yayımlama profilleri yayımlama işlemini basitleştirmek ve herhangi bir sayıda profilleri bulunabilir. Bir yayımlama profili, aşağıdaki yollardan birini seçerek Visual Studio'da oluşturun:

* Çözüm Gezgini'nde projeye sağ tıklayıp **Yayımla**.
* Seçin **Yayımla &lt;project_name&gt;**  gelen **derleme** menüsü.

**Yayımla** uygulama kapasiteler sayfasının sekmesi görüntülenir. Proje bir yayımlama profili sahip değilse, aşağıdaki sayfası görüntülenir:

![Yayımlama sekmesindeki uygulama kapasiteler sayfasının](visual-studio-publish-profiles/_static/az.png)

Zaman **klasör** olduğu belirlenirse, yayımlanan varlıkları depolamak için bir klasör yolu belirtin. Varsayılan klasör *bin\Release\PublishOutput*. Tıklayın **profili oluştur** düğmesini tamamlayın.

Bir yayımlama profili oluşturulduktan sonra **Yayımla** sekmesinde değişiklikler. Yeni oluşturulan profil aşağı açılan listede görünür. Tıklayın **yeni profil oluşturma** başka bir yeni profili oluşturmak için.

![Yayımlama sekmesindeki FolderProfile gösteren uygulama kapasiteler sayfasının](visual-studio-publish-profiles/_static/create_new.png)

Yayımlama Sihirbazı'nı aşağıdaki Yayımla hedeflerini destekler:

* Azure uygulama hizmeti
* Azure sanal makineleri
* IIS, FTP, vb. (için herhangi bir web sunucusu)
* Klasör
* Profili içeri aktar

Daha fazla bilgi için [hangi Yayımlama seçenekleri benim için en uygun](/visualstudio/ide/not-in-toc/web-publish-options).

Visual Studio ile bir yayımlama profili oluştururken, bir *özellikleri/PublishProfiles/&lt;Profil_adı&gt;.pubxml* MSBuild dosyası oluşturulur. *.Pubxml* dosyanın MSBuild dosyası ve içeren yapılandırma ayarlarını yayımlayın. Bu dosya, yapı özelleştirme ve yayımlama işlemi için değiştirilebilir. Bu dosya yayımlama işlemi tarafından okunur. `<LastUsedBuildConfiguration>` Genel bir özellik olduğundan ve yapı alınan herhangi bir dosya olmamalıdır özeldir. Bkz: [MSBuild: Yapılandırma özelliğinin nasıl ayarlandığını](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) daha fazla bilgi için.

Azure bir hedefe, yayımlama sırasında *.pubxml* dosyası, Azure abonelik tanımlayıcısı içeriyor. Bu dosya kaynak denetimine eklemeye, hedef türüyle önerilmez. Azure dışı hedef yayımlama sırasında iade etmeye güvenli *.pubxml* dosya.

(Yayımlama parola gibi) hassas bilgiler şifreli bir kullanıcı/makine düzeyinde başına. İçinde depolanan *özellikleri/PublishProfiles/&lt;Profil_adı&gt;. pubxml.user* dosya. Bu dosya, hassas bilgileri depolayabileceğiniz için kaynak denetimine iade olmamalıdır.

Nasıl bir ASP.NET Core web uygulaması yayımlamak genel bakış için bkz. [konak dağıtıp](xref:host-and-deploy/index). MSBuild görevleri ve hedefleri ASP.NET Core uygulaması yayımlamak için gerekli açık kaynaklı, https://github.com/aspnet/websdk.

`dotnet publish` MSDeploy, klasörü kullanabilirsiniz ve [Kudu](https://github.com/projectkudu/kudu/wiki) yayımlama profilleri:

(Çalışan platformlar arası) klasörü:

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy (şu anda bu tek çalışır platformlar arası MSDeploy olmadığından bu yana Windows):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

MSDeploy paket (şu anda bu tek çalışır platformlar arası MSDeploy olmadığından bu yana Windows):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

Önceki örneklerdeki **yoksa** geçirmek `deployonbuild` için `dotnet publish`.

Daha fazla bilgi için [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` her platformda dizinden Azure'da yayımlamak için Kudu API'leri destekler. Visual Studio yayımlama Kudu API'leri, ancak desteklenen WebSDK ile platformlar arası yayımlamak için Azure'a destekler.

Eklemek için bir yayımlama profili *özellikleri/PublishProfiles* klasöründe aşağıdaki içeriğe sahip:

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

Yayımla içerikleri zip ve Kudu API'lerini kullanarak Azure'da yayımlamak için aşağıdaki komutu çalıştırın:

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

Bir yayımlama profili kullanırken aşağıdaki MSBuild özellikleri ayarlayın:

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

Adlı bir profille yayımlarken *FolderProfile*, aşağıdaki komutlardan birini yürütülebilir:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Çağrılırken [dotnet derleme](/dotnet/core/tools/dotnet-build), çağrı `msbuild` yapıyı çalıştırmak ve işlem yayınlamak için. Ya da çağırma `dotnet build` veya `msbuild` klasör profilinde geçerken eşdeğerdir. MSBuild doğrudan Windows üzerinde çağrıldığında, MSBuild .NET Framework sürümü kullanılır. MSDeploy yayımlama için Windows makineleri için şu anda sınırlıdır. Çağırma `dotnet build` klasördeki olmayan profili çağırır MSBuild ve MSBuild olmayan klasör profillerde MSDeploy kullanır. Çağırma `dotnet build` klasörü olmayan profilinde (MSDeploy kullanarak) MSBuild çağırır ve (hatta Windows platformunda çalışırken) içinde bir hata oluşur. Bir klasörü olmayan profili ile yayımlamak için MSBuild doğrudan çağırabilir.

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

Not `<LastUsedBuildConfiguration>` ayarlanır `Release`. Visual Studio'dan yayımlama sırasında `<LastUsedBuildConfiguration>` yapılandırma özellik değeri, yayımlama işlemi başlatıldığında değeri kullanılarak ayarlanır. `<LastUsedBuildConfiguration>` Yapılandırma özelliği özeldir ve içeri aktarılan bir MSBuild dosyasında geçersiz kılınan olmamalıdır. Bu özellik komut satırından geçersiz kılınabilir.

.NET Core CLI kullanarak:

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

MSBuild kullanarak:

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Komut satırından bir MSDeploy uç noktasına yayımlama

Yayımlama, .NET Core CLI veya MSBuild'ı kullanarak gerçekleştirilebilir. `dotnet publish` .NET Core bağlamında çalışır. `msbuild` Komutu Windows ortamları için sınırlar .NET Framework gerektirir.

MSDeploy yayımlama için en kolay yolu, ilk Visual Studio 2017'de bir yayımlama profili oluşturun ve komut satırından profil sağlamaktır.

Aşağıdaki örnekte, bir ASP.NET Core web uygulaması oluşturulur (kullanarak `dotnet new mvc`), ve bir Azure yayımlama profili, Visual Studio ile eklenir.

Çalıştırma `msbuild` gelen bir **VS 2017 için geliştirici komut istemi**. Geliştirici komut istemi doğru sahip *msbuild.exe* yolundaki bazı MSBuild değişkenleri kümesi.

MSBuild aşağıdaki sözdizimini kullanır:

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

Alma `Password` gelen  *\<Yayımla adı >. PublishSettings* dosya. İndirme *. PublishSettings* dosyasından ya da:

* Çözüm Gezgini: Web uygulaması üzerinde sağ tıklayın ve **yayımlama profili indirme**.
* Azure portalı: tıklayın **yayımlama profili Al** Web uygulamasının üzerinde **genel bakış** paneli.

`Username` Yayımlama profilinde bulunabilir.

Aşağıdaki örnek kullanımları *Web11112 - Web dağıtımı* yayımlama profili:

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a>Dosyaları dışarıda bırak

ASP.NET Core web uygulamaları, yapılar ve içeriğini yayımlarken *wwwroot* klasörü dahil edilir. `msbuild` destekleyen [Glob desenlerinin](https://gruntjs.com/configuring-tasks#globbing-patterns). Örneğin, aşağıdaki `<Content>` öğesi hariç tüm metni (*.txt*) dosyalarını *wwwroot/içerik* klasör ve tüm alt klasörleri.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Önceki işaretleme için bir yayımlama profili eklenebilir veya *.csproj* dosya. Eklenen *.csproj* dosya, kural için eklenir projedeki tüm yayımlama profilleri.

Aşağıdaki `<MsDeploySkipRules>` öğe tüm dosyaları dışlar *wwwroot/içerik* klasörü:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` silmez *atla* dağıtım sitesinden hedefler. `<Content>` hedef dosyalar ve klasörler dağıtım site veritabanından silinir. Örneğin, aşağıdaki dosyaları dağıtılan web uygulaması olduğu varsayalım:

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Aşağıdaki `<MsDeploySkipRules>` öğeleri eklenir, dağıtım sitesinde bu dosyaları silseniz mıydı.

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

Önceki `<MsDeploySkipRules>` öğeleri önlemek *atlandı* dosyalarının dağıtılıyor. Bunlar dağıttıktan sonra bu dosyaları silinmez.

Aşağıdaki `<Content>` öğesi dağıtım sitede hedeflenen dosyaları siler:

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Önceki komut satırı dağıtımı kullanarak `<Content>` öğesi aşağıdaki çıktıyı üretir:

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

## <a name="include-files"></a>Dosyaları Ekle

Aşağıdaki biçimlendirme içeren bir *görüntüleri* klasörü için proje dizininin dışına *wwwroot/görüntülerinden* Yayımla sitenin klasörü:

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

Biçimlendirme eklenebilir *.csproj* dosya veya yayımlama profili. Kümeye eklenirse *.csproj* dosyası, onu eklendi projedeki her yayımlama profilinde.

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

Bkz: [WebSDK Benioku](https://github.com/aspnet/websdk) daha fazla dağıtım örneği için.

## <a name="run-a-target-before-or-after-publishing"></a>Bir hedef önce veya sonra yayımlama çalıştırın

Yerleşik `BeforePublish` ve `AfterPublish` hedefleri yürütmenizi hedef önce veya sonra yayımlama hedefi. Yayımlama profili öncesinde ve sonrasında yayımlama konsolu iletilerini günlüğe kaydetmek için aşağıdaki öğeleri ekleyin:

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>Güvenilmeyen bir sertifika kullanarak bir sunucuda yayımlayın

Ekleme `<AllowUntrustedCertificate>` özellik değeriyle `True` yayımlama profili için:

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Kudu hizmeti

Bir Azure App Service web uygulaması dağıtımı ' dosyaları görüntülemek için kullanın [Kudu hizmet](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Append `scm` belirteç için web uygulaması adı. Örneğin:

| URL                                    | Sonuç       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Web uygulaması      |
| `http://mysite.scm.azurewebsites.net/` | Kudu hizmeti |

Seçin [hata ayıklama konsolunu](https://github.com/projectkudu/kudu/wiki/Kudu-console) görüntülemek, düzenlemek, silmek veya dosya eklemek için menü öğesi.

## <a name="additional-resources"></a>Ek kaynaklar

* [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) web uygulamaları ve IIS sunucuları için Web siteleri dağıtımı (MSDeploy) basitleştirir.
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Dosya sorunları ve istek için dağıtım özellikleri.
* [Visual Studio'dan Azure VM için bir ASP.NET Web uygulaması yayımlama](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
