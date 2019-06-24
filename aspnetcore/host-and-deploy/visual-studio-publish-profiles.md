---
title: Visual Studio yayımlama profilleri için ASP.NET Core uygulaması dağıtımı
author: rick-anderson
description: Oluşturmayı Visual Studio'da yayımlama profilleri ve ASP.NET Core uygulama dağıtımlarını çeşitli hedeflere yönetmek için kullanın.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/21/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 50be5a20f6d927270ef2d9dbc6c1cbf24196978f
ms.sourcegitcommit: 28646e8ca62fb094db1557b5c0c02d5b45531824
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2019
ms.locfileid: "67333418"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Visual Studio yayımlama profilleri için ASP.NET Core uygulaması dağıtımı

Tarafından [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu belge, Visual Studio 2019 kullanma veya daha sonra oluşturma ve kullanma odaklanır yayımlama profilleri. Visual Studio ile oluşturulan yayımlama profillerine, MSBuild ve Visual Studio ile kullanılabilir. Azure'da yayımlamak için yönergeler için bkz: <xref:tutorials/publish-to-azure-webapp-using-vs>.

`dotnet new mvc` Komutu, aşağıdaki kök düzeyinde içeren bir proje dosyası üretir [ \<Proje > öğesi](/visualstudio/msbuild/project-element-msbuild):

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

`Content` Öğesi listesinin yanı sıra derleme çıktılarını yayımlanan dosyaları içerir. Varsayılan olarak, dosyaları desenlerle eşleşen `wwwroot\**`, `**\*.config`, ve `**\*.json` dahil `Content` öğe listesi. Örneğin, `wwwroot\**` [Glob deseni](https://gruntjs.com/configuring-tasks#globbing-patterns) eşleşen tüm dosyaları *wwwroot* klasör ve alt klasörleri.

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

Komut satırı yayımlama, .NET Core tarafından desteklenen tüm platformlarda çalışır ve Visual Studio gerektirmez. Aşağıdaki örneklerde, .NET Core CLI's [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu proje dizininden çalıştırın (içeren *.csproj* dosyası). Proje klasörü geçerli çalışma dizini değilse, proje dosyası yolu açıkça geçirebilirsiniz. Örneğin:

```console
dotnet publish C:\Webs\Web1
```

Oluşturma ve bir web uygulaması yayımlamak için aşağıdaki komutları çalıştırın:

```console
dotnet new mvc
dotnet publish
```

`dotnet publish` Komutu, bir çeşitlemesi aşağıdaki çıktıyı üretir:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version {VERSION} for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 36.81 ms for C:\Webs\Web1\Web1.csproj.
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\Web1.Views.dll
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\publish\
```

Varsayılan klasör yayımlama biçimi *bin\Debug\\{hedef çerçeve adı} \publish\\* . Örneğin, *bin\Debug\netcoreapp2.2\publish\\* .

Aşağıdaki komut belirtir bir `Release` derleme ve yayımlama dizini:

```console
dotnet publish -c Release -o C:\MyWebs\test
```

`dotnet publish` Çağrıları çağıran MSBuild komut `Publish` hedef. Herhangi bir parametre geçirilen `dotnet publish` MSBuild'e geçirilir. `-c` Ve `-o` MSBuild'e ait eşleme parametreleri `Configuration` ve `OutputPath` özellikleri, sırasıyla.

MSBuild özellikleri, aşağıdaki biçimlerden birini kullanarak geçirilebilir:

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Örneğin, aşağıdaki komutu yayımlar bir `Release` bir ağ paylaşımına oluşturun. Ağ paylaşımı eğik çizgi ile belirtilen ( *//r8/* ) ve .NET Core desteklenen tüm platformlarda çalışır.

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Yayımlanan uygulama dağıtımı için çalışmadığından emin onaylayın. Dosyalar *yayımlama* klasörü, uygulama çalışırken kilitlenir. Dağıtım kilitli olduğundan dosya kopyalanamadı oluşamaz.

## <a name="publish-profiles"></a>Yayımlama profilleri

Bu bölümde Visual Studio 2019 veya üzeri bir yayımlama profili oluşturmak için kullanılır. Visual Studio ya da komut satırından yayımlama profili oluşturulduktan sonra kullanılabilir. Yayımlama profilleri yayımlama işlemini basitleştirmek ve herhangi bir sayıda profilleri bulunabilir.

Bir yayımlama profili, aşağıdaki yollardan birini seçerek Visual Studio'da oluşturun:

* Projeye sağ **Çözüm Gezgini** seçip **Yayımla**.
* Seçin **yayımlama {proje adı}** gelen **derleme** menüsü.

**Yayımla** uygulama özellikleri sayfasının sekmesi görüntülenir. Proje bir yayımlama profili yoksa **yayımlama hedefi seçin** sayfası görüntülenir. Aşağıdaki yayımlama hedeflerinden birini seçin istenir:

* Azure uygulama hizmeti
* Linux üzerinde Azure App Service
* Azure sanal makineleri
* Klasör
* IIS, FTP, Web dağıtımı (için herhangi bir web sunucusu)
* Profili içeri aktar

En uygun yayımlama hedef belirlemek için bkz: [hangi Yayımlama seçenekleri benim için en uygun](/visualstudio/ide/not-in-toc/web-publish-options).

Zaman **klasör** yayımlama hedef seçildiğinde, yayımlanan varlıkları depolamak için bir klasör yolu belirtin. Varsayılan klasör yolu *bin\\{PROJECT Yapılandırması}\\{hedef çerçeve adı} \publish\\* . Örneğin, *bin\Release\netcoreapp2.2\publish\\* . Seçin **profili oluştur** düğmesini tamamlayın.

Bir yayımlama profili oluşturulduktan sonra **Yayımla** sekmenin içerik değişiklikleri. Yeni oluşturulan profil aşağı açılan listede görünür. Açılır listede seçin **yeni profil oluşturma** başka bir yeni profili oluşturmak için.

Visual Studio'nun Yayımla aracı üreten bir *özellikleri/PublishProfiles / {PROFİL adı} .pubxml* yayımlama profili açıklayan MSBuild dosyası. *.Pubxml* dosyası:

* İçeren yapılandırma ayarlarını yayımlayın ve yayımlama işlemi tarafından kullanılır.
* Yapı özelleştirme ve yayımlama işlemi için değiştirilebilir.

Azure bir hedefe, yayımlama sırasında *.pubxml* dosyası, Azure abonelik tanımlayıcısı içeriyor. Bu dosya kaynak denetimine eklemeye, hedef türüyle önerilmez. Azure dışı hedef yayımlama sırasında iade etmeye güvenli *.pubxml* dosya.

(Yayımlama parola gibi) hassas bilgiler şifreli bir kullanıcı/makine düzeyinde başına. İçinde depolanan *özellikleri/PublishProfiles / {PROFİL adı}.pubxml.user* dosya. Bu dosya, hassas bilgileri depolayabileceğiniz için kaynak denetimine iade olmamalıdır.

Nasıl bir ASP.NET Core web uygulaması yayımlamak genel bakış için bkz. <xref:host-and-deploy/index>. Açık kaynaklı bir ASP.NET Core web uygulaması yayımlamak için gerekli olan hedefler ve MSBuild görevleri sırasında [aspnet/websdk depo](https://github.com/aspnet/websdk).

`dotnet publish` Komutu, klasör, MSDeploy, kullanabilir ve [Kudu](https://github.com/projectkudu/kudu/wiki) yayımlama profilleri. Platformlar arası destek MSDeploy olmadığı için aşağıdaki MSDeploy seçenekleri yalnızca Windows üzerinde desteklenir.

**(Çalışan platformlar arası) klasörü:**

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

**MSDeploy:**

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

**MSDeploy paketi:**

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

Yukarıdaki örneklerde geçirmezseniz `deployonbuild` için `dotnet publish`.

Daha fazla bilgi için [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` her platformda dizinden Azure'da yayımlamak için Kudu API'leri destekler. Visual Studio yayımlama Kudu API'leri, ancak desteklenen WebSDK ile platformlar arası yayımlamak için Azure'a destekler.

Projenin bir yayımlama profili Ekle *özellikleri/PublishProfiles* klasöründe aşağıdaki içeriğe sahip:

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

.NET Core CLI'ın [dotnet derleme](/dotnet/core/tools/dotnet-build) komut çağrıları `msbuild` yapıyı çalıştırmak ve işlem yayınlamak için. `dotnet build` Ve `msbuild` klasör profilinde geçerken komutlar eşdeğerdir. Çağrılırken `msbuild` doğrudan Windows üzerinde MSBuild .NET Framework sürümü kullanılır. Çağırma `dotnet build` klasörü olmayan profilindeki:

* Çağıran `msbuild`, MSDeploy kullanır.
* (Hatta Windows üzerinde çalışırken) bir hatayla sonuçlanır. Bir klasörü olmayan profili ile yayımlamak için çağrı `msbuild` doğrudan.

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

Önceki örnekte:

* `<ExcludeApp_Data>` Özelliği yalnızca bir XML Şeması gereksinimi karşılamak için mevcut. `<ExcludeApp_Data>` Özellik üzerinde hiçbir etkisi yayımlama işlemi olsa bile bir *App_Data* proje kök klasöründe. *App_Data* ASP.NET 4.x projelerinde olduğu gibi klasör özel olarak değerlendirilmesi alma değil.

* `<LastUsedBuildConfiguration>` Özelliği `Release`. Visual Studio'dan değerini yayımlarken `<LastUsedBuildConfiguration>` yayımlama işlemi başlatıldığında değeri kullanılarak yapılır. `<LastUsedBuildConfiguration>` Özel ve içeri aktarılan bir MSBuild dosyasında geçersiz kılınan olmamalıdır. Bu özellik ancak olabilir aşağıdaki yaklaşımlardan birini kullanarak komut satırından geçersiz kılındı.
  * .NET Core CLI kullanarak:

    ```console
    dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
    ```

  * MSBuild kullanarak:

    ```console
    msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
    ```

  Daha fazla bilgi için [MSBuild: Yapılandırma özelliğinin nasıl ayarlandığını](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).

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
  * **Çözüm Gezgini**: Seçin **görünümü** > **Cloud Explorer**. Azure aboneliğinizle bağlanın. Açık **uygulama hizmetleri**. Uygulamaya sağ tıklayın. Seçin **yayımlama profili indir**.
  * Azure portalı: Seçin **yayımlama profili Al** web uygulamasının **genel bakış** paneli.

Aşağıdaki örnekte adlı bir yayımlama profili *AzureWebApp - Web dağıtımı*:

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

Bir yayımlama profili, .NET Core CLI ile de kullanılabilir [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) bir Windows komut kabuğu komutunu:

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!IMPORTANT]
> `dotnet msbuild` Komut bir platformlar arası komut ve macOS ve Linux'ta ASP.NET Core uygulamaları derleyebilirsiniz. Ancak, MSBuild MacOS ve Linux Azure veya diğer MSDeploy uç noktalar için uygulama dağıtma uyumlu değil.

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

Önceki komut satırı dağıtımı kullanarak `<Content>` öğesi çeşitlemesi aşağıdaki çıktıyı üretir:

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\{TARGET FRAMEWORK MONIKER}\PubTmp\Web1.SourceManifest.
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

Aşağıdaki bölümlerde anahat farklı yaklaşımlara dosya eklemek için zaman yayımlayın. [Genel dosya ekleme](#general-file-inclusion) bölümünde kullanan `DotNetPublishFiles` Yayımla hedefleri dosyasında Web SDK'sı tarafından sağlanan öğesi. [Seçici dosya eklemeyi](#selective-file-inclusion) bölümünde kullanır `ResolvedFileToPublish` Yayımla hedefleri dosyasında .NET Core SDK'sı tarafından sağlanan öğesi. Web SDK'sı üzerinde .NET Core SDK'sı bağlı olduğundan, bir ASP.NET Core projesi içinde her iki öğe kullanılabilir.

### <a name="general-file-inclusion"></a>Genel dosya ekleme

Aşağıdaki örnekteki `<ItemGroup>` öğesi yayımlanmış siteyi klasöre proje dizininin dışında bulunan bir klasöre kopyalama gösterir. Aşağıdaki biçimlendirme için kullanıcının eklenen dosyaları `<ItemGroup>` varsayılan olarak eklenir.

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotNetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotNetPublishFiles>
</ItemGroup>
```

Önceki işaretlemesi:

* Eklenebilir *.csproj* dosya veya yayımlama profili. Kümeye eklenirse *.csproj* dosyası, onu eklendi projedeki her yayımlama profilinde.
* Bildiren bir `_CustomFiles` depolamak için öğe dosyaları eşleşen `Include` özniteliğin Glob deseni. *Görüntüleri* düzende başvurulan klasörü, proje dizininin dışında bulunur. A [ayrılmış özelliği](/visualstudio/msbuild/msbuild-reserved-and-well-known-properties), adlandırılmış `$(MSBuildProjectDirectory)`, proje dosyasının mutlak yolu çözümler.
* Dosyaları bir listesini sağlar `DotNetPublishFiles` öğesi. Varsayılan olarak, öğenin ait `<DestinationRelativePath>` öğesi boş. Varsayılan değer kullanır ve biçimlendirme içinde geçersiz [tanınmış öğe meta verileri](/visualstudio/msbuild/msbuild-well-known-item-metadata) gibi `%(RecursiveDir)`. İç metni temsil eder *wwwroot/görüntülerinden* yayımlanmış siteyi klasörü.

### <a name="selective-file-inclusion"></a>Seçici dosya ekleme

Aşağıdaki örnekte vurgulanmış biçimlendirmeyi gösterir:

* Projenin dışında yayımlanan site içinde bulunan bir dosya kopyalama *wwwroot* klasör. Dosya adını *ReadMe2.md* korunur.
* Hariç *wwwroot\Content* klasör.
* Hariç *Views\Home\About2.cshtml*.

[!code-xml[](visual-studio-publish-profiles/samples/Web1.pubxml?highlight=18-23)]

Önceki örnekte `ResolvedFileToPublish` , varsayılan davranış, sağlanan dosyaları her zaman Kopyala öğesini `Include` özniteliği için yayımlanmış siteyi. Dahil ederek varsayılan davranışın üzerine bir `<CopyToPublishDirectory>` iç metni ya da alt öğesiyle `Never` veya `PreserveNewest`. Örneğin:

```xml
<ResolvedFileToPublish Include="..\ReadMe2.md">
  <RelativePath>wwwroot\ReadMe2.md</RelativePath>
  <CopyToPublishDirectory>PreserveNewest</CopyToPublishDirectory>
</ResolvedFileToPublish>
```

Daha fazla dağıtım örneği için bkz. [Web SDK'sı depoya Benioku](https://github.com/aspnet/websdk).

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
* [Web SDK'sı GitHub deposu](https://github.com/aspnet/websdk/issues): Dosya sorunları ve istek için dağıtım özellikleri.
* [Visual Studio'dan Azure VM için bir ASP.NET Web uygulaması yayımlama](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
