---
title: ASP.NET Core uygulama dağıtımı için Visual Studio yayımlama profilleri
author: rick-anderson
description: Visual Studio 'da yayımlama profilleri oluşturmayı ve bunları çeşitli hedeflere ASP.NET Core uygulama dağıtımlarını yönetmek için kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/21/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: fd08a5ebe5b85dcddcec4ef3e57d326a44ce2f2d
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080857"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>ASP.NET Core uygulama dağıtımı için Visual Studio yayımlama profilleri

[Sayed Ibrampahashve](https://github.com/sayedihashimi) [Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

Bu belge, yayımlama profillerinin oluşturulması ve kullanılması için Visual Studio 2019 veya sonraki bir sürümü kullanılarak odaklanmıştır. Visual Studio ile oluşturulan yayımlama profilleri MSBuild ve Visual Studio ile birlikte kullanılabilir. Azure 'da yayımlama yönergeleri için bkz <xref:tutorials/publish-to-azure-webapp-using-vs>.

Komut `dotnet new mvc` , aşağıdaki kök düzeyi [ \<Proje > öğesini](/visualstudio/msbuild/project-element-msbuild)içeren bir proje dosyası üretir:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
</Project>
```

`<Project>` Önceki [](/visualstudio/msbuild/msbuild-targets) [](/visualstudio/msbuild/msbuild-properties) öğenin özniteliği, $ (msbuildsdkspath) \Microsoft.net.SDK.Web\Sdk\Sdk.props ve $ (msbuildsdkspath) \ adresinden MSBuild özelliklerini ve hedeflerini içeri aktarır `Sdk`  *Sırasıyla Microsoft. NET. SDK. Web\sdk\sdk.exe hedefleri*. İçin `$(MSBuildSDKsPath)` varsayılan konum (Visual Studio 2019 Enterprise ile) *% ProgramFiles (x86)% \ Microsoft Visual studio\2019\enterprise\msbuild\sdk* klasörüdür.

`Microsoft.NET.Sdk.Web`(Web SDK) (.NET Core SDK) ve `Microsoft.NET.Sdk` `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)) dahil diğer SDK 'lara bağlıdır. Her bağımlı SDK ile ilişkili MSBuild özellikleri ve hedefleri içeri aktarılır. Yayımlama hedefleri, kullanılan Yayımla yöntemine göre uygun hedef kümesini içeri aktarır.

MSBuild veya Visual Studio bir projeyi yüklediğinde, aşağıdaki üst düzey eylemler gerçekleşir:

* Projeyi oluştur
* Yayımlanacak işlem dosyaları
* Dosyaları hedefe Yayımla

## <a name="compute-project-items"></a>İşlem projesi öğeleri

Proje yüklendiğinde, [MSBuild proje öğeleri](/visualstudio/msbuild/common-msbuild-project-items) (dosyalar) hesaplanır. Öğe türü, dosyanın nasıl işlendiğini belirler. Varsayılan olarak, *. cs* dosyaları `Compile` öğe listesine eklenir. `Compile` Öğe listesindeki dosyalar derlenir.

`Content` Öğe listesi, derleme çıktılarına ek olarak yayımlanan dosyaları içerir. Varsayılan `wwwroot\**`olarak, `**\*.config` ve`**\*.json`desenleriyle eşleşen dosyalar öğelistesinedahiledilir.`Content` Örneğin, `wwwroot\**` [Glob deseninin](https://gruntjs.com/configuring-tasks#globbing-patterns) *Wwwroot* klasörü ve alt klasörlerindeki tüm dosyalar eşleşir.

::: moniker range=">= aspnetcore-3.0"

Web SDK 'Sı [Razor SDK 'sını](xref:razor-pages/sdk)içeri aktarır. Sonuç olarak, desenlerle `**\*.cshtml` eşleşen dosyalar ve `**\*.razor` `Content` öğe listesine de dahildir.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Web SDK 'Sı [Razor SDK 'sını](xref:razor-pages/sdk)içeri aktarır. Sonuç olarak, `**\*.cshtml` düzeniyle eşleşen dosyalar da `Content` öğe listesine dahil edilir.

::: moniker-end

Yayınlama listesine açıkça bir dosya eklemek için, dosyayı, [dosyaları dahil et](#include-files) bölümünde gösterildiği gibi doğrudan *. csproj* dosyasına ekleyin.

Visual Studio 'da veya komut satırından yayımlarken **Yayımla** düğmesini seçerken:

* Özellikler/öğeler hesaplanır (oluşturmak için gereken dosyalar).
* **Yalnızca Visual Studio**: NuGet paketleri geri yüklendi. (Geri yüklemenin CLı üzerinde kullanıcı tarafından açık olması gerekir.)
* Proje oluşturulur.
* Yayımlama öğeleri hesaplanır (yayımlamak için gereken dosyalar).
* Proje yayımlandı (hesaplanan dosyalar yayımlama hedefine kopyalanır).

Proje dosyasına bir ASP.NET Core projesi `Microsoft.NET.Sdk.Web` başvurduğunda, Web uygulaması dizininin köküne bir *app_offline. htm* dosyası yerleştirilir. Dosya varsa, ASP.NET Core modülü düzgün bir şekilde uygulamayı kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya. Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).

## <a name="basic-command-line-publishing"></a>Temel komut satırı yayımlama

Komut satırı yayımlama, .NET Core tarafından desteklenen tüm platformlarda çalışmaktadır ve Visual Studio 'Yu gerektirmez. Aşağıdaki örneklerde .NET Core CLI [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutu proje dizininden çalıştırılır ( *. csproj* dosyasını içerir). Proje klasörü geçerli çalışma dizini değilse, proje dosyası yolunda açıkça geçiş yapın. Örneğin:

```dotnetcli
dotnet publish C:\Webs\Web1
```

Bir Web uygulaması oluşturmak ve yayımlamak için aşağıdaki komutları çalıştırın:

```dotnetcli
dotnet new mvc
dotnet publish
```

`dotnet publish` Komut aşağıdaki çıkışın bir çeşidini üretir:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version {VERSION} for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 36.81 ms for C:\Webs\Web1\Web1.csproj.
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\Web1.Views.dll
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\publish\
```

Varsayılan yayımlama klasörü biçimi *\\bin\Debug {Target Framework bilinen ad\\} \publish*şeklindedir. Örneğin, *\\Bin\debug\netcoreapp2,2\publish*.

Aşağıdaki komut bir `Release` derlemeyi ve yayımlama dizinini belirtir:

```dotnetcli
dotnet publish -c Release -o C:\MyWebs\test
```

Komutu, `Publish` hedefi çağıran MSBuild 'i çağırır. `dotnet publish` Öğesine `dotnet publish` geçirilen parametreler MSBuild 'e geçirilir. `-c` Ve `-o` parametrelerisırasıyla`OutputPath` MSBuild veözelliklerileeşlenir.`Configuration`

MSBuild özellikleri aşağıdaki biçimlerden birini kullanarak geçirilebilir:

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Örneğin, aşağıdaki komut bir ağ paylaşımında bir `Release` derlemeyi yayımlar. Ağ paylaşma, eğik çizgiler (/*saat*) ile belirtilir ve tüm .NET Core desteklenen platformlarda çalışmaktadır.

```dotnetcli
dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb
```

Dağıtım için yayımlanan uygulamanın çalışmadığından emin olun. *Yayımla* klasöründeki dosyalar, uygulama çalışırken kilitlenir. Kilitli dosyalar kopyalanamadığından dağıtım gerçekleştirilemez.

## <a name="publish-profiles"></a>Yayımlama profilleri

Bu bölüm, bir yayımlama profili oluşturmak için Visual Studio 2019 veya üstünü kullanır. Profil oluşturulduktan sonra, Visual Studio 'dan veya komut satırından yayımlama kullanılabilir. Yayımlama profilleri Yayımlama sürecini basitleştirebilir ve herhangi bir sayıda profil bulunabilir.

Aşağıdaki yollardan birini seçerek Visual Studio 'da bir yayımlama profili oluşturun:

* **Çözüm Gezgini** projeye sağ tıklayın ve **Yayımla**' yı seçin.
* **Build** menüsünden **{Project Name} Yayımla** ' yı seçin.

Uygulama özellikleri sayfasının **Yayımla** sekmesi görüntülenir. Projenin bir yayımlama profili yoksa, **bir yayımlama hedefi seçin** sayfası görüntülenir. Aşağıdaki yayımlama hedeflerinden birini seçmeniz istenir:

* Azure uygulama hizmeti
* Linux üzerinde Azure App Service
* Azure sanal makineleri
* Klasör
* IIS, FTP, Web Dağıtımı (herhangi bir Web sunucusu için)
* Profili içeri aktar

En uygun yayımlama hedefini belirlemek için, [hangi yayımlama seçeneklerinin benim için](/visualstudio/ide/not-in-toc/web-publish-options)uygun olduğunu öğrenin.

Hedef Yayımla **klasörü** seçildiğinde, yayımlanmış varlıkları depolamak için bir klasör yolu belirtin. Varsayılan klasör yolu, *bin\\{Project CONFIGURATION}\\{Target Framework bilinen adı} \publish\\* şeklindedir. Örneğin, *\\Bin\release\netcoreapp2,2\publish*. Tamamlanacak **Profil oluştur** düğmesini seçin.

Bir yayımlama profili oluşturulduktan sonra, **Yayımla** sekmesinin içeriği değişir. Yeni oluşturulan profil bir açılan listede görüntülenir. Aşağı açılan listenin altında **Yeni profil** oluştur ' u seçerek yeni bir profil oluşturun.

Visual Studio 'nun yayımlama aracı, yayımlama profilini açıklayan bir *Özellikler/PublishProfiles/{PROFILE Name}. pubxml* MSBuild dosyası oluşturuyor. *. Pubxml* dosyası:

* Yayımlama yapılandırma ayarlarını içerir ve yayımlama işlemi tarafından kullanılır.
* Derleme ve yayımlama işlemini özelleştirmek için değiştirilebilir.

Azure hedefine yayımlarken, *. pubxml* dosyası Azure abonelik tanımlarınızı içerir. Bu hedef türünde, bu dosyayı kaynak denetimine eklemek önerilmez. Azure olmayan bir hedefe yayımlarken, *. pubxml* dosyasını denetlemek güvenlidir.

Gizli bilgiler (yayımlama parolası gibi) Kullanıcı/makine düzeyinde şifrelenir. *Özellikler/PublishProfiles/{PROFILE Name}. pubxml. User* dosyasında depolanır. Bu dosya hassas bilgileri depolayabildiğinden, kaynak denetimine denetlenmemelidir.

ASP.NET Core Web uygulamasının nasıl yayımlanacağı hakkında genel bir bakış için, bkz <xref:host-and-deploy/index>. ASP.NET Core Web uygulaması yayımlamak için gereken MSBuild görevleri ve hedefleri, [ASPNET/WebSDK deposunda](https://github.com/aspnet/websdk)açık kaynaktır.

Komut, klasörü, MSDeploy ve kudu yayımlama profillerini kullanabilir. [](https://github.com/projectkudu/kudu/wiki) `dotnet publish` MSDeploy platformlar arası destek olmadığından, aşağıdaki MSDeploy seçenekleri yalnızca Windows 'da desteklenir.

**Klasör (platformlar arası):**

```dotnetcli
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

**MSDeploy**

```dotnetcli
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

**MSDeploy paketi:**

```dotnetcli
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

Yukarıdaki örneklerde öğesine `deployonbuild` `dotnet publish`geçmeyin.

Daha fazla bilgi için bkz. [Microsoft. net. SDK. Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish`Azure 'da herhangi bir platformda yayımlanacak kudu API 'Lerini destekler. Visual Studio yayımlama, kudu API 'Lerini destekler, ancak Azure 'da platformlar arası yayımlama için WebSDK tarafından desteklenir.

Projenin *Properties/PublishProfiles* klasörüne aşağıdaki içeriğe sahip bir yayımlama profili ekleyin:

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

Yayımlama içeriğini bağlamak ve kudu API 'Lerini kullanarak Azure 'da yayımlamak için aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

Bir yayımlama profili kullanırken aşağıdaki MSBuild özelliklerini ayarlayın:

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

*Folderprofile*adlı bir profille yayımlarken, aşağıdaki komutlardan biri yürütülebilir:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

.NET Core CLI [DotNet Build](/dotnet/core/tools/dotnet-build) komutu, derleme ve `msbuild` yayımlama işlemini çalıştırmak için çağırır. `dotnet build` Ve`msbuild` komutları, bir klasör profilinde geçirilerek eşdeğerdir. Doğrudan Windows `msbuild` üzerinde çağrılırken, MSBuild 'in .NET Framework sürümü kullanılır. Klasör `dotnet build` olmayan bir profilde çağırma:

* MSDeploy `msbuild`kullanan çağırır.
* Hataya neden olur (Windows üzerinde çalışırken bile). Klasör olmayan bir profille yayımlamak için doğrudan çağırın `msbuild` .

Aşağıdaki klasör yayımlama profili, Visual Studio ile oluşturulmuştur ve bir ağ paylaşımında yayımlar:

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

Yukarıdaki örnekte:

* `<ExcludeApp_Data>` Özelliği yalnızca bir XML şeması gereksinimini karşılamak için vardır. Proje kökünde *App_Data* klasörü olsa bile, özelliğinyayımlamaişlemiüzerindehiçbiretkisiyoktur.`<ExcludeApp_Data>` *App_Data* klasörü, ASP.NET 4. x projelerinde olduğu gibi özel bir işleme almaz.

* `<LastUsedBuildConfiguration>` Özelliği olarak`Release`ayarlanır. Visual Studio 'dan yayımlarken değeri `<LastUsedBuildConfiguration>` , yayımlama işlemi başlatıldığında değeri kullanılarak ayarlanır. `<LastUsedBuildConfiguration>`özeldir ve içeri aktarılan MSBuild dosyasında geçersiz kılınmamalıdır. Ancak, bu özellik aşağıdaki yaklaşımlardan birini kullanarak komut satırından geçersiz kılınabilir.
  * .NET Core CLI kullanarak:

    ```dotnetcli
    dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
    ```

  * MSBuild 'i kullanma:

    ```console
    msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
    ```

  Daha fazla bilgi için bkz. [MSBuild: yapılandırma özelliğini ayarlama](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Komut satırından bir MSDeploy uç noktasına yayımlama

Aşağıdaki örnek, *AzureWebApp*adlı Visual Studio tarafından oluşturulan bir ASP.NET Core Web uygulamasını kullanır. Visual Studio ile bir Azure Apps yayımlama profili eklenir. Profil oluşturma hakkında daha fazla bilgi için, [Yayımlama profilleri](#publish-profiles) bölümüne bakın.

Uygulamayı bir yayımlama profili kullanarak dağıtmak için, bir Visual Studio `msbuild` **Geliştirici komut istemi**komutunu yürütün. Komut istemi, Windows görev çubuğundaki **Başlat** menüsünün *Visual Studio* klasöründe bulunur. Daha kolay erişim için, Visual Studio 'daki **Araçlar** menüsüne komut istemi ekleyebilirsiniz. Daha fazla bilgi için bkz. [Visual Studio için geliştirici komut istemi](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).

MSBuild aşağıdaki komut sözdizimini kullanır:

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* Yolun &ndash; Uygulamanın proje dosyasının yolu.
* PROFILINIZI &ndash; Yayımlama profilinin adı.
* NITELEN &ndash; MSDeploy Kullanıcı adı. {USERNAME}, yayımlama profilinde bulunabilir.
* PAROLAYı &ndash; MSDeploy parolası. {PROFILE} öğesinden {PASSWORD} öğesini edinin *. PublishSettings* dosyası. ' Nı indirin *. PublishSettings* dosyası şunlardan biri:
  * **Çözüm Gezgini**: **Bulut Gezginini** **görüntüle** > ' yi seçin. Azure aboneliğinize bağlanın. **Uygulama hizmetleri**'ni açın. Uygulamaya sağ tıklayın. **Yayımlama profilini indir**' i seçin.
  * Azure portal: Web uygulamasının **genel bakış** panelinde **Yayımlama profilini al** ' ı seçin.

Aşağıdaki örnek, *AzureWebApp-Web dağıtımı*adlı bir yayımlama profili kullanır:

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

Bir yayımlama profili, Windows komut kabuğu 'ndan .NET Core CLI [DotNet MSBuild](/dotnet/core/tools/dotnet-msbuild) komutuyla da kullanılabilir:

```dotnetcli
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!IMPORTANT]
> Komut `dotnet msbuild` , platformlar arası bir komuttur ve MacOS ve Linux üzerinde ASP.NET Core uygulamalar derleyebilir. Ancak, macOS ve Linux 'ta MSBuild, bir uygulamayı Azure 'a veya diğer MSDeploy uç noktalarına dağıtmıyor.

## <a name="set-the-environment"></a>Ortamı ayarlama

Uygulamanın ortamını ayarlamak için Publish profile ( *. pubxml*) veya proje dosyasına [](xref:fundamentals/environments) özelliğiekleyin:`<EnvironmentName>`

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

*Web. config* dönüştürmelerine ihtiyaç duyuyorsanız (örneğin, yapılandırma, profil veya ortama göre ortam değişkenlerini ayarlamak), bkz <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="exclude-files"></a>Dosyaları Dışla

ASP.NET Core Web Apps yayımlandığında, aşağıdaki varlıklar dahil edilmiştir:

* Yapı yapıtları
* Aşağıdaki glob desenleriyle eşleşen klasörler ve dosyalar:
  * `**\*.config`(örneğin, *Web. config*)
  * `**\*.json`(örneğin, *appSettings. JSON*)
  * `wwwroot\**`

MSBuild, [Glob desenlerini](https://gruntjs.com/configuring-tasks#globbing-patterns)destekler. Örneğin, aşağıdaki `<Content>` öğe, metin ( *. txt*) dosyalarının *wwwroot\content* klasörü ve alt klasörlerinde kopyalanmasını bastırır:

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Önceki biçimlendirme bir yayımlama profiline veya *. csproj* dosyasına eklenebilir. *. Csproj* dosyasına eklendiğinde, kural projedeki tüm yayımlama profillerine eklenir.

Aşağıdaki `<MsDeploySkipRules>` öğe, tüm dosyaları *wwwroot\content* klasöründen dışlar:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>`dağıtım sitesinden *atlama* hedeflerini silmez. `<Content>`hedeflenen dosya ve klasörler dağıtım sitesinden silinir. Örneğin, dağıtılan bir Web uygulamasının aşağıdaki dosyalar olduğunu varsayalım:

* *Görünümler/Home/about1. cshtml*
* *Görünümler/Home/About2. cshtml*
* *Görünümler/Home/About3. cshtml*

Aşağıdaki `<MsDeploySkipRules>` öğeler eklenirse, bu dosyalar dağıtım sitesinde silinmez.

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

Önceki `<MsDeploySkipRules>` öğeler *Atlanan* dosyaların dağıtılmasını engeller. Dağıtıldıktan sonra bu dosyaları silmez.

Aşağıdaki `<Content>` öğe, dağıtım sitesindeki hedeflenen dosyaları siler:

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Önceki `<Content>` öğeyle komut satırı dağıtımı kullanmak, aşağıdaki çıkışın bir varyasyonunu verir:

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

## <a name="include-files"></a>İçerme dosyaları

Aşağıdaki bölümlerde, yayımlama zamanında dosya ekleme için farklı yaklaşımlar ana hatlarıyla verilmiştir. [Genel dosya ekleme](#general-file-inclusion) bölümü, Web SDK `DotNetPublishFiles` 'sında bir Yayımla hedefi dosyası tarafından belirtilen öğesini kullanır. [Seçmeli dosya ekleme](#selective-file-inclusion) bölümü, .NET Core SDK bir `ResolvedFileToPublish` Yayımla hedefi dosyası tarafından belirtilen öğesini kullanır. Web SDK .NET Core SDK bağlı olduğundan, her iki öğe bir ASP.NET Core projesinde kullanılabilir.

### <a name="general-file-inclusion"></a>Genel dosya ekleme

Aşağıdaki örnek `<ItemGroup>` öğesi, proje dizininin dışında bulunan bir klasörü yayımlanmış sitenin bir klasörüne kopyalamayı gösterir. Aşağıdaki biçimlendirmeye `<ItemGroup>` eklenen tüm dosyalar varsayılan olarak dahil edilir.

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotNetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotNetPublishFiles>
</ItemGroup>
```

Önceki işaretlemesi:

* *. Csproj* dosyasına veya yayımlama profiline eklenebilir. *. Csproj* dosyasına eklenirse, bu, projedeki her bir yayımlama profiline eklenir.
* `Include` Özniteliğin glob `_CustomFiles` düzeniyle eşleşen dosyaları depolamak için bir öğe bildirir. Düzende başvurulan *görüntüler* klasörü, proje dizininin dışında bulunur. Adlı`$(MSBuildProjectDirectory)` [ayrılmış bir özellik](/visualstudio/msbuild/msbuild-reserved-and-well-known-properties), proje dosyasının mutlak yoluna çözümlenir.
* `DotNetPublishFiles` Öğe için dosyaların bir listesini sağlar. Varsayılan olarak, öğenin `<DestinationRelativePath>` öğesi boştur. Varsayılan değer, biçimlendirmede geçersiz kılınır ve gibi [iyi bilinen öğe meta verilerini](/visualstudio/msbuild/msbuild-well-known-item-metadata) `%(RecursiveDir)`kullanır. İç metin, yayımlanan sitenin *Wwwroot/görüntüler* klasörünü temsil eder.

### <a name="selective-file-inclusion"></a>Seçmeli dosya ekleme

Aşağıdaki örnekte vurgulanan biçimlendirme şunları göstermektedir:

* Projenin dışında bulunan bir dosyayı yayınlanan sitenin *Wwwroot* klasörüne kopyalama. *ReadMe2.MD* dosyasının adı korunur.
* *Wwwroot\content* klasörü dışlanıyor.
* *Views\home\about2,cshtml*hariç tutulanıyor.

[!code-xml[](visual-studio-publish-profiles/samples/Web1.pubxml?highlight=18-23)]

Yukarıdaki örnek, varsayılan davranışı `ResolvedFileToPublish` `Include` özniteliğinde belirtilen dosyaları her zaman yayımlanan siteye kopyalamak için olan öğesini kullanır. Ya `<CopyToPublishDirectory>` `Never` da 'ıniçmetniylebiraltöğeekleyerekvarsayılandavranışıgeçersizkılın.`PreserveNewest` Örneğin:

```xml
<ResolvedFileToPublish Include="..\ReadMe2.md">
  <RelativePath>wwwroot\ReadMe2.md</RelativePath>
  <CopyToPublishDirectory>PreserveNewest</CopyToPublishDirectory>
</ResolvedFileToPublish>
```

Daha fazla dağıtım örneği için bkz. [Web SDK deposu Benioku dosyası](https://github.com/aspnet/websdk).

## <a name="run-a-target-before-or-after-publishing"></a>Yayımlamadan önce veya sonra bir hedef Çalıştır

Yerleşik `BeforePublish` ve`AfterPublish` hedefler, yayımlama hedefinden önce veya sonra bir hedef yürütür. Aşağıdaki öğeleri yayımlama profiline, yayımlamadan önce ve sonra da konsol iletilerini günlüğe kaydetmek için ekleyin:

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>Güvenilmeyen bir sertifikayı kullanarak bir sunucuya yayımlama

Değerini yayımlama profiline değeri `<AllowUntrustedCertificate>` `True` olan özelliği ekleyin:

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Kudu hizmeti

Azure App Service Web uygulaması dağıtımında dosyaları görüntülemek için [kudu hizmetini](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service)kullanın. `scm` Belirteci Web uygulaması adına ekleyin. Örneğin:

| URL                                    | Sonuç       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Web uygulaması      |
| `http://mysite.scm.azurewebsites.net/` | Kudu hizmeti |

Dosyaları görüntülemek, düzenlemek, silmek veya eklemek için [hata ayıklama konsolu](https://github.com/projectkudu/kudu/wiki/Kudu-console) menü öğesini seçin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Web dağıtımı](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy), IIS sunucularına Web uygulamaları ve Web siteleri dağıtımını basitleştirir.
* [Web SDK GitHub deposu](https://github.com/aspnet/websdk/issues): Dosya sorunları ve dağıtım için istek özellikleri.
* [Visual Studio 'dan bir Azure VM 'de ASP.NET Web uygulaması yayımlama](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
