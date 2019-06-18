---
title: Visual Studio yayımlama profilleri için ASP.NET Core uygulaması dağıtımı
author: rick-anderson
description: Oluşturmayı Visual Studio'da yayımlama profilleri ve ASP.NET Core uygulama dağıtımlarını çeşitli hedeflere yönetmek için kullanın.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/18/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 7006b73ea9a424a3c9530cf526727bc71a88838f
ms.sourcegitcommit: 516f166c5f7cec54edf3d9c71e6e2ba53fb3b0e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67196308"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Visual Studio yayımlama profilleri için ASP.NET Core uygulaması dağıtımı

Tarafından [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu belge, Visual Studio 2017 kullanarak veya daha sonra oluşturma ve kullanma odaklanır yayımlama profilleri. Visual Studio ile oluşturulan yayımlama profillerine MSBuild ve Visual Studio çalıştırabilirsiniz. Bkz: [Visual Studio kullanarak Azure App Service'e bir ASP.NET Core web uygulaması yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) Azure'da yayımlamak için yönergeler.

`dotnet new mvc` Komutu, aşağıdaki üst düzey içeren bir proje dosyası üretir `<Project>` öğesi:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
</Project>
```

Önceki `<Project>` öğenin `Sdk` özniteliği alır MSBuild [özellikleri](/visualstudio/msbuild/msbuild-properties) ve [hedefleri](/visualstudio/msbuild/msbuild-targets) gelen *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\ SDK.props* ve *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*sırasıyla. Varsayılan konumu `$(MSBuildSDKsPath)` (Visual Studio 2019 Enterprise ile) *% programfiles (x86) %\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* klasör.

`Microsoft.NET.Sdk.Web` (Web SDK'sı) dahil olmak üzere diğer SDK'lar üzerinde bağlıdır `Microsoft.NET.Sdk` (.NET Core SDK) ve `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)). Bağımlı her SDK ile ilişkili hedefler ve MSBuild özellikleri içeri aktarılır. Hedefler içe hedefleri kullanılan Yayımla yöntemine göre uygun kümesini yayımlayın.

MSBuild veya Visual Studio bir projeyi yüklediğinde, aşağıdaki üst düzey eylemler gerçekleşir:

* Proje derleme
* Yayımlamak için dosyaların işlem
* Hedef dosya yayımlama

## <a name="compute-project-items"></a>Proje öğeleri işlem

Proje yüklendiğinde [MSBuild proje öğeleri](/visualstudio/msbuild/common-msbuild-project-items) (dosyalar) hesaplanır. Öğe türü, dosyanın nasıl işleneceğini belirler. Varsayılan olarak, *.cs* dosyaları dahil edilecek `Compile` öğe listesi. Dosyalar `Compile` öğe listesi derlenir.

`Content` Öğesi listesinin yanı sıra derleme çıktılarını yayımlanan dosyaları içerir. Varsayılan olarak, dosyaları desenlerle eşleşen `wwwroot\**`, `**\*.config`, ve `**\*.json` dahil `Content` öğe listesi. Örneğin, `wwwroot\**` [Glob deseni](https://gruntjs.com/configuring-tasks#globbing-patterns) eşleşen tüm dosyaları *wwwroot* klasör **ve** klasörlerinden.

::: moniker range=">= aspnetcore-3.0"

Web SDK'sı aktarır [Razor SDK](xref:razor-pages/sdk). Bunun sonucunda, dosyaları desenlerle eşleşen `**\*.cshtml` ve `**\*.razor` de dahil edilir `Content` öğe listesi.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Web SDK'sı aktarır [Razor SDK](xref:razor-pages/sdk). Bunun sonucunda, dosyaları eşleşen `**\*.cshtml` düzeni de dahil edilecek `Content` öğe listesi.

::: moniker-end

Açıkça Yayımla listesine bir dosya eklemek için dosyanın doğrudan ekleme *.csproj* gösterildiği gibi dosya [dosyaları içerir](#include-files) bölümü.

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

```console
dotnet new mvc
dotnet publish
```

[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komut aşağıdakine benzer bir çıktı üretir:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version {version} for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 36.81 ms for C:\Webs\Web1\Web1.csproj.
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\Web1.Views.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\publish\
```

Varsayılan publish klasörüne olan `bin\$(Configuration)\netcoreapp<version>\publish`. İçin varsayılan `$(Configuration)` olduğu *hata ayıklama*. Önceki örnekte, `<TargetFramework>` olduğu `netcoreapp{X.Y}`.

`dotnet publish -h` bilgi yayımlama için yardımı görüntüler.

Aşağıdaki komut belirtir bir `Release` derleme ve yayımlama dizini:

```console
dotnet publish -c Release -o C:\MyWebs\test
```

[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) çağrıları çağıran MSBuild komut `Publish` hedef. Herhangi bir parametre geçirilen `dotnet publish` MSBuild'e geçirilir. `-c` Parametre eşlenen `Configuration` MSBuild özelliği. `-o` Parametre eşlenen `OutputPath`.

MSBuild özellikleri, aşağıdaki biçimlerden birini kullanarak geçirilebilir:

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Aşağıdaki komutu yayımlar bir `Release` bir ağ paylaşımına oluşturun:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Ağ paylaşımı eğik çizgi ile belirtilen ( *//r8/* ) ve .NET Core desteklenen tüm platformlarda çalışır.

Yayımlanan uygulama dağıtımı için çalışmadığından emin onaylayın. Dosyalar *yayımlama* klasörü, uygulama çalışırken kilitlenir. Dağıtım kilitli olduğundan dosya kopyalanamadı oluşamaz.

## <a name="publish-profiles"></a>Yayımlama profilleri

Bu bölümde Visual Studio 2017 veya sonraki bir yayımlama profili oluşturmak için kullanılır. Visual Studio ya da komut satırından yayımlama profili oluşturulduktan sonra kullanılabilir.

Yayımlama profilleri yayımlama işlemini basitleştirmek ve herhangi bir sayıda profilleri bulunabilir. Bir yayımlama profili, aşağıdaki yollardan birini seçerek Visual Studio'da oluşturun:

* Projeye sağ **Çözüm Gezgini** seçip **Yayımla**.
* Seçin **yayımlama {proje adı}** gelen **derleme** menüsü.

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

Visual Studio ile bir yayımlama profili oluştururken, bir *özellikleri/PublishProfiles / {PROFİL adı} .pubxml* MSBuild dosyası oluşturulur. *.Pubxml* dosyanın MSBuild dosyası ve içeren yapılandırma ayarlarını yayımlayın. Bu dosya, yapı özelleştirme ve yayımlama işlemi için değiştirilebilir. Bu dosya yayımlama işlemi tarafından okunur. `<LastUsedBuildConfiguration>` Genel bir özellik olduğundan ve yapı alınan herhangi bir dosya olmamalıdır özeldir. Bkz: [MSBuild: Yapılandırma özelliğinin nasıl ayarlandığını](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) daha fazla bilgi için.

Azure bir hedefe, yayımlama sırasında *.pubxml* dosyası, Azure abonelik tanımlayıcısı içeriyor. Bu dosya kaynak denetimine eklemeye, hedef türüyle önerilmez. Azure dışı hedef yayımlama sırasında iade etmeye güvenli *.pubxml* dosya.

(Yayımlama parola gibi) hassas bilgiler şifreli bir kullanıcı/makine düzeyinde başına. İçinde depolanan *özellikleri/PublishProfiles / {PROFİL adı}.pubxml.user* dosya. Bu dosya, hassas bilgileri depolayabileceğiniz için kaynak denetimine iade olmamalıdır.

Nasıl bir ASP.NET Core web uygulaması yayımlamak genel bakış için bkz. [konak dağıtıp](xref:host-and-deploy/index). MSBuild görevleri ve hedefleri ASP.NET Core uygulaması yayımlamak için gerekli açık kaynaklı, [aspnet/websdk depo](https://github.com/aspnet/websdk).

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

Yukarıdaki örneklerde geçirmezseniz `deployonbuild` için `dotnet publish`.

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
* `PublishProfile={PUBLISH PROFILE}`

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

Önceki örnekte `<LastUsedBuildConfiguration>` ayarlanır `Release`. Visual Studio'dan yayımlama sırasında `<LastUsedBuildConfiguration>` yapılandırma özellik değeri, yayımlama işlemi başlatıldığında değeri kullanılarak ayarlanır. `<LastUsedBuildConfiguration>` Yapılandırma özelliği özeldir ve içeri aktarılan bir MSBuild dosyasında geçersiz kılınan olmamalıdır. Bu özellik komut satırından geçersiz kılınabilir.

.NET Core CLI kullanarak:

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

MSBuild kullanarak:

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Komut satırından bir MSDeploy uç noktasına yayımlama

Aşağıdaki örnekte adlı Visual Studio tarafından oluşturulan bir ASP.NET Core web uygulaması *AzureWebApp*. Azure uygulamaları yayımlama profili Visual Studio ile eklenir. Profil oluşturma hakkında daha fazla bilgi için bkz. [yayımlama profillerini](#publish-profiles) bölümü.

Bir yayımlama profili kullanarak uygulama dağıtmak için yürütme `msbuild` Visual Studio'dan komutunu **Geliştirici komut istemi**. Komut istemi kullanılabilir *Visual Studio* klasörü **Başlat** Windows görev çubuğundaki menü. Daha kolay erişim için komut satırına ekleyebilirsiniz **Araçları** Visual Studio'daki menü. Daha fazla bilgi için [Visual Studio için geliştirici komut istemi](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).

MSBuild, şu komut söz dizimini kullanır:

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* {PATH} &ndash; Uygulamanın proje dosyasının yolu.
* {} PROFİLİ &ndash; Yayımlama profilinin adı.
* {USERNAME} &ndash; MSDeploy kullanıcı adı. {USERNAME} yayımlama profilinde bulunabilir.
* {PASSWORD} &ndash; MSDeploy parola. {PASSWORD} almak *{profili}. PublishSettings* dosya. İndirme *. PublishSettings* dosyasından ya da:
  * Çözüm Gezgini için: Seçin **görünümü** > **Cloud Explorer**. Azure aboneliğinizle bağlanın. Açık **uygulama hizmetleri**. Uygulamaya sağ tıklayın. Seçin **yayımlama profili indir**.
  * Azure portalı: Seçin **yayımlama profili Al** web uygulamasının **genel bakış** paneli.

Aşağıdaki örnekte adlı bir yayımlama profili *AzureWebApp - Web dağıtımı*:

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

Bir yayımlama profili, .NET Core CLI ile de kullanılabilir [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) bir Windows komut isteminden komutu:

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!NOTE]
> [Dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) komut kullanılabilir platformlar arası ve macOS ve Linux'ta ASP.NET Core uygulamaları derleyebilirsiniz. Ancak, MSBuild MacOS ve Linux Azure veya başka bir MSDeploy uç noktası için bir uygulama dağıtmaya yeteneğine sahip değildir. MSDeploy, yalnızca Windows üzerinde kullanılabilir.

## <a name="set-the-environment"></a>Ortamı ayarlama

Dahil `<EnvironmentName>` yayımlama profilini özelliğinde ( *.pubxml*) veya uygulamanın ayarlamak için proje dosyasını [ortam](xref:fundamentals/environments):

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

Gerektiriyorsa *web.config* Dönüşümleri (yapılandırma, profili veya ortama göre örneğin, ortam değişkenlerini ayarlama), bkz: <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="exclude-files"></a>Dosyaları dışarıda bırak

ASP.NET Core web uygulamaları yayımlarken, aşağıdaki varlıklar dahildir:

* Derleme yapıları
* Dosya ve klasörleri aşağıdaki Glob desenlerinin eşleşen:
  * `**\*.config` (örneğin, *web.config*)
  * `**\*.json` (örneğin, *appsettings.json*)
  * `wwwroot\**`

MSBuild destekler [Glob desenlerinin](https://gruntjs.com/configuring-tasks#globbing-patterns). Örneğin, aşağıdaki `<Content>` öğesi metnin kopyalanmasını engeller ( *.txt*) dosyalarını *wwwroot\content* klasör ve alt klasörleri:

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Önceki işaretleme için bir yayımlama profili eklenebilir veya *.csproj* dosya. Eklenen *.csproj* dosya, kural için eklenir projedeki tüm yayımlama profilleri.

Aşağıdaki `<MsDeploySkipRules>` öğe tüm dosyaları dışlar *wwwroot\content* klasörü:

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
* <xref:host-and-deploy/iis/transform-webconfig>
