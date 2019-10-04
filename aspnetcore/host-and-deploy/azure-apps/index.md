---
title: ASP.NET Core uygulamalarını Azure App Service dağıtma
author: guardrex
description: Bu makale, Azure ana bilgisayarına bağlantılar ve kaynakları dağıtmak için bağlantı içerir.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/02/2019
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: bda4923adb0f9769f883ef64f7902c8650308222
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71924896"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a>ASP.NET Core uygulamalarını Azure App Service dağıtma

[Azure App Service](https://azure.microsoft.com/services/app-service/) , Web uygulamalarını barındırmak için ASP.NET Core dahil olmak üzere bir [Microsoft bulut bilgi işlem platformu hizmetidir](https://azure.microsoft.com/) .

## <a name="useful-resources"></a>Faydalı kaynaklar

[App Service belge](/azure/app-service/) , Azure Apps belgelerinin, öğreticilerin, örneklerin, nasıl yapılır kılavuzlarının ve diğer kaynakların ana adresidir. Barındırma ASP.NET Core uygulamaları ile ilgili iki önemli öğretici şunlardır:

[Azure 'da ASP.NET Core Web uygulaması oluşturma](/azure/app-service/app-service-web-get-started-dotnet)  
ASP.NET Core bir Web uygulamasını Windows üzerinde Azure App Service oluşturmak ve dağıtmak için Visual Studio 'Yu kullanın.

[Linux üzerinde App Service ASP.NET Core uygulama oluşturma](/azure/app-service/containers/quickstart-dotnetcore)  
Linux üzerinde Azure App Service için bir ASP.NET Core Web uygulaması oluşturup dağıtmak üzere komut satırını kullanın.

Azure App Service 'te bulunan ASP.NET Core sürümü için [App Service panosundaki ASP.NET Core](https://aspnetcoreon.azurewebsites.net/) bakın.

Aşağıdaki makaleler ASP.NET Core belgelerinde sunulmaktadır:

<xref:tutorials/publish-to-azure-webapp-using-vs>  
Visual Studio kullanarak Azure App Service'e bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin.

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
Visual Studio kullanarak ASP.NET Core bir Web uygulaması oluşturmayı ve sürekli dağıtım için git 'i kullanarak Azure App Service nasıl dağıtacağınızı öğrenin.

[İlk işlem hattınızı oluşturma](/azure/devops/pipelines/get-started-yaml)  
ASP.NET Core bir uygulama için CI derlemesi ayarlayın ve Azure App Service için sürekli bir dağıtım sürümü oluşturun.

[Azure Web uygulaması korumalı alanı](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Azure Apps platformu tarafından zorlanan Azure App Service çalışma zamanı yürütme sınırlamalarını bulun.

<xref:test/troubleshoot>  
ASP.NET Core projelerle uyarıları ve hataları anlayın ve sorun giderin.

## <a name="application-configuration"></a>Uygulama yapılandırması

### <a name="platform"></a>Platform

::: moniker range=">= aspnetcore-2.2"

64-bit (x64) ve 32-bit (x86) uygulamalarının çalışma zamanları Azure App Service vardır. App Service kullanılabilir [.NET Core SDK](/dotnet/core/sdk) 32 bittir, ancak [kudu](https://github.com/projectkudu/kudu/wiki) konsolunu veya Visual Studio 'daki Yayımla işlemini kullanarak yerel olarak oluşturulan 64 bit uygulamaları dağıtabilirsiniz. Daha fazla bilgi için, [uygulamayı yayımlama ve dağıtma](#publish-and-deploy-the-app) bölümüne bakın.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Yerel bağımlılıklara sahip uygulamalar için, 32-bit (x86) uygulamalarının çalışma zamanları Azure App Service vardır. App Service kullanılabilir [.NET Core SDK](/dotnet/core/sdk) 32 bitlik bir değer.

::: moniker-end

.NET Core Framework bileşenleri ve dağıtım yöntemleri hakkında daha fazla bilgi için, .NET Core çalışma zamanı ve .NET Core SDK hakkında bilgi gibi bkz [. .NET Core hakkında: Oluşturma](/dotnet/core/about#composition).

### <a name="packages"></a>Paketler

Azure App Service dağıtılan uygulamalar için otomatik günlük oluşturma özellikleri sağlamak üzere aşağıdaki NuGet paketlerini ekleyin:

* [Microsoft. AspNetCore. AzureAppServices. hostingstartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) , Azure App Service ile ASP.NET Core hafif tümleştirme sağlamak Için [ıhostingstartup](xref:fundamentals/configuration/platform-specific-configuration) kullanır. Eklenen günlük özellikleri, `Microsoft.AspNetCore.AzureAppServicesIntegration` paket tarafından sağlanır.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.
* [Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) , Azure App Service tanılama günlüklerini ve günlük akışı özelliklerini desteklemek için günlükçü uygulamaları sağlar.

Önceki paketlere [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)tarafından ulaşılabilir. `Microsoft.AspNetCore.App` Metapackage 'i .NET Framework veya başvurusunu hedefleyen uygulamalar, uygulamanın proje dosyasındaki ayrı paketlere açık olarak başvurmalıdır.

## <a name="override-app-configuration-using-the-azure-portal"></a>Azure portalını kullanarak uygulama yapılandırmasını geçersiz kılma

Azure portalındaki uygulama ayarları, uygulamanın ortam değişkenlerini ayarlamanıza olanak sağlar. Ortam değişkenleri, [ortam değişkenleri yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider)tarafından tüketilebilir.

Azure portalında bir uygulama ayarı oluşturulduğunda veya değiştirildiğinde ve **Kaydet** düğmesi seçildiğinde, Azure uygulaması yeniden başlatılır. Ortam değişkeni, hizmet yeniden başlatıldıktan sonra uygulama için kullanılabilir.

::: moniker range=">= aspnetcore-3.0"

Bir uygulama [genel ana bilgisayarı](xref:fundamentals/host/generic-host)kullandığında, ortam değişkenleri varsayılan olarak bir uygulamanın yapılandırmasına yüklenmez ve yapılandırma sağlayıcısı geliştirici tarafından eklenmelidir. Geliştirici, yapılandırma sağlayıcısı eklendiğinde ortam değişkeni önekini belirler. Daha fazla bilgi için bkz <xref:fundamentals/host/generic-host> . ve [ortam değişkenleri yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bir uygulama, [Webhost. createdefaultbuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)kullanarak Konağı oluşturduğunda, Konağı yapılandıran ortam değişkenleri `ASPNETCORE_` ön eki kullanır. Daha fazla bilgi için bkz <xref:fundamentals/host/web-host> . ve [ortam değişkenleri yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Ara sunucu ve yük dengeleyici senaryoları

[İşlem dışı](xref:host-and-deploy/iis/index#out-of-process-hosting-model)barındırma sırasında Iletilen üstbilgiler ara yazılımını yapılandıran ve ASP.NET Core modülü DÜZENI (http/https) ve isteğin BAŞLATıLDıĞı uzak IP adresini iletecek şekilde yapılandırılmış [IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components). . Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir. Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>İzleme ve günlüğe kaydetme

::: moniker range=">= aspnetcore-3.0"

App Service dağıtılan ASP.NET Core uygulamalar, bir App Service uzantısı **ASP.NET Core günlüğe kaydetme tümleştirmesi**otomatik olarak alır. Uzantı, Azure App Service ASP.NET Core uygulamalar için günlüğe kaydetme tümleştirmesi imkanı sunar.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

App Service dağıtılan ASP.NET Core uygulamalar, bir App Service uzantısı **ASP.NET Core günlüğe kaydetme uzantısı**otomatik olarak alır. Uzantı, Azure App Service ASP.NET Core uygulamalar için günlüğe kaydetme tümleştirmesi imkanı sunar.

::: moniker-end

İzleme, günlüğe kaydetme ve sorun giderme bilgileri için aşağıdaki makalelere bakın:

[Azure App Service uygulamaları izleme](/azure/app-service/web-sites-monitor)  
Uygulamalar ve App Service planları için kotaları ve ölçümleri incelemeyi öğrenin.

[Azure App Service uygulamalar için tanılama günlüğünü etkinleştirme](/azure/app-service/web-sites-enable-diagnostic-log)  
HTTP durum kodları, başarısız istekler ve Web sunucusu etkinliği için tanılama günlüğü 'nün nasıl etkinleştirileceğini ve erişebileceğini öğrenin.

<xref:fundamentals/error-handling>  
ASP.NET Core uygulamalarında hataları işlemeye yönelik yaygın yaklaşımları anlayın.

<xref:test/troubleshoot-azure-iis>  
ASP.NET Core uygulamalarla Azure App Service dağıtımlarla ilgili sorunları tanılamayı öğrenin.

<xref:host-and-deploy/azure-iis-errors-reference>  
Sorun giderme önerisi ile Azure App Service/IIS tarafından barındırılan uygulamalar için ortak dağıtım yapılandırma hatalarına bakın.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Veri koruma anahtar halkası ve dağıtım Yuvaları

[Veri koruma anahtarları](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) *%Home%\ASP.NET\DataProtection-Keys* klasöründe kalıcı hale getirilir. Bu klasör, ağ depolama tarafından desteklenir ve uygulamayı barındıran tüm makinelerde eşitlenir. Anahtarlar bekleyen bir şekilde korunmuyor. Bu klasör, bir uygulamanın tüm örneklerine tek bir dağıtım yuvasında anahtar halkasını sağlar. Hazırlama ve üretim gibi ayrı dağıtım yuvaları, anahtar halkasını paylaşmaz.

Dağıtım yuvaları arasında takas edildiğinde, veri koruma kullanan tüm sistem, önceki yuva içindeki anahtar halkasını kullanarak depolanan verilerin şifresini çözemeyecektir. ASP.NET tanımlama bilgisi ara yazılımı, tanımlama bilgilerini korumak için veri koruma kullanır. Bu, kullanıcılara standart ASP.NET tanımlama bilgisi ara yazılımı kullanan bir uygulamanın oturumunu kapatmakta olduğunu gösterir. Yuvada bağımsız bir anahtar halka çözümü için, bir dış anahtar halka sağlayıcısı kullanın, örneğin:

* Azure Blob Depolama
* Azure Key Vault
* SQL Mağazası
* Redsıs önbelleği

Daha fazla bilgi için bkz. <xref:security/data-protection/implementation/key-storage-providers>.
<a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>
<!-- revert this after 3.0 supported
## Deploy ASP.NET Core preview release to Azure App Service

Use one of the following approaches if the app relies on a preview release of .NET Core:

* [Install the preview site extension](#install-the-preview-site-extension).
* [Deploy a self-contained preview app](#deploy-a-self-contained-preview-app).
* [Use Docker with Web Apps for containers](#use-docker-with-web-apps-for-containers).
-->
## <a name="deploy-aspnet-core-30-to-azure-app-service"></a>Azure App Service ASP.NET Core 3,0 dağıtma

Yakında Azure App Service ASP.NET Core 3,0 kullanılabilir olmasını umuyoruz.

Uygulama .NET Core 3,0 ' i temel alıyorsa aşağıdaki yaklaşımlardan birini kullanın:

* [Önizleme sitesi uzantısını yükler](#install-the-preview-site-extension).
* [Kendi kendine içerilen bir önizleme uygulaması dağıtın](#deploy-a-self-contained-preview-app).
* [Kapsayıcılar için Web Apps Docker kullanın](#use-docker-with-web-apps-for-containers).

### <a name="install-the-preview-site-extension"></a>Önizleme sitesi uzantısını yükler

Önizleme sitesi uzantısı kullanılarak bir sorun oluşursa, bir [ASPNET/AspNetCore sorunu](https://github.com/aspnet/AspNetCore/issues)açın.

1. Azure portalından App Service gidin.
1. Web uygulamasını seçin.
1. "Uzantıları" filtrelemek için arama kutusuna "Ex" yazın veya yönetim araçları listesini aşağı kaydırın.
1. **Uzantıları**seçin.
1. **Add (Ekle)** seçeneğini belirleyin.
1. `{x64|x86}` Listeden`{X.Y}` ASP.NET Core önizleme sürümü olduğu ve platformu belirten **ASP.NET Core {X. Y} ({x64 | x86}) çalışma zamanı** uzantısını seçin.
1. Yasal koşulları kabul etmek için **Tamam ' ı** seçin.
1. Uzantıyı yüklemek için **Tamam ' ı** seçin.

İşlem tamamlandığında, en son .NET Core önizlemesi yüklenir. Yüklemeyi doğrulayın:

1. **Gelişmiş Araçlar**' ı seçin.
1. **Gelişmiş araçlarda** **Git** ' i seçin.
1. **Hata ayıklama konsolu** > **PowerShell** menü öğesini seçin.
1. PowerShell komut isteminde aşağıdaki komutu yürütün. Komutu için `{X.Y}` ASP.NET Core çalışma zamanı sürümünü ve `{PLATFORM}` platformunu değiştirin:

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```

   Bu komut x64 `True` önizlemesi çalışma zamanı yüklendiğinde döndürür.

> [!NOTE]
> Uygulama hizmetleri uygulamasının platform mimarisi (x86/x64), bir dizi işlem veya daha iyi barındırma katmanında barındırılan uygulamalar için Azure portalındaki uygulama ayarlarında ayarlanır. Uygulama, işlem içi modda çalışıyorsa ve platform mimarisi 64-bit (x64) için yapılandırılmışsa ASP.NET Core modülü, varsa 64 bit önizleme çalışma zamanını kullanır. **ASP.NET Core {X. Y} (x64) çalışma zamanı** uzantısını yükler.
>
> X64 Önizleme çalışma zamanını yükledikten sonra, yüklemenin doğrulanması için kudu PowerShell komut penceresinde aşağıdaki komutu çalıştırın. Komutu için `{X.Y}` ASP.NET Core çalışma zamanı sürümünü yerine koyun:
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
>
> Bu komut x64 `True` önizlemesi çalışma zamanı yüklendiğinde döndürür.

> [!NOTE]
> **ASP.NET Core uzantıları** , Azure Uygulama Hizmetleri 'nde Azure günlük kaydı etkinleştirme gibi ek ASP.NET Core işlevler sunar. Uzantı Visual Studio 'dan dağıtıldığında otomatik olarak yüklenir. Uzantı yüklü değilse, uygulama için bu uygulamayı yükleme.

**Bir ARM şablonuyla önizleme sitesi uzantısını kullanma**

Uygulama oluşturmak ve dağıtmak için bir ARM şablonu kullanılıyorsa, `siteextensions` kaynak türü bir Web uygulamasına site uzantısını eklemek için kullanılabilir. Örneğin:

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-a-self-contained-preview-app"></a>Kendi içinde bir önizleme uygulaması dağıtma

Bir önizleme çalışma zamanını hedefleyen [kendinden bağımsız bir dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) , dağıtımdaki Önizleme çalışma zamanını taşır.

Kendi kendine içerilen bir uygulama dağıtımında:

* Azure App Service sitesi, [Önizleme sitesi uzantısını](#install-the-preview-site-extension)gerektirmez.
* Uygulamanın, [çerçeveye bağımlı dağıtım (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd)için yayımlarken farklı bir yaklaşımdan sonra yayımlanması gerekir.

[Uygulamanın kendi Içinde dağıtımı](#deploy-the-app-self-contained) bölümünde yer alan yönergeleri izleyin.

### <a name="use-docker-with-web-apps-for-containers"></a>Kapsayıcılar için Web Apps Docker kullanma

[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) , en son önizleme Docker görüntülerini içerir. Görüntüler, temel görüntü olarak kullanılabilir. Görüntüyü kullanın ve kapsayıcılar için normal olarak Web Apps için dağıtın.

## <a name="publish-and-deploy-the-app"></a>Uygulamayı yayımlama ve dağıtma

### <a name="deploy-the-app-framework-dependent"></a>Uygulama çerçevesine bağımlı dağıtım

::: moniker range=">= aspnetcore-2.2"

64 bitlik bir [çerçeveye bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd)için:

* 64 bit uygulama derlemek için 64 bit .NET Core SDK kullanın.
* App Service **yapılandırma** genel > ayarlarında platformu **64 bit** olarak ayarlayın. Uygulamanın, platform bit özelliğini tercih etmek için temel veya daha yüksek bir hizmet planı kullanması gerekir.

::: moniker-end

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Visual Studio araç çubuğundan **derleme** > **yayımlaması {Application Name}** öğesini seçin veya **Çözüm Gezgini** ' de projeye sağ tıklayıp **Yayımla**' yı seçin.
1. **Bir yayımlama hedefi seç** iletişim kutusunda **App Service** seçili olduğunu onaylayın.
1. **Gelişmiş**'i seçin. **Yayımla** iletişim kutusu açılır.
1. İçinde **Yayımla** iletişim:
   * **Yayın** yapılandırmasının seçili olduğunu doğrulayın.
   * **Dağıtım modu** açılır listesini açın ve **çerçeveye bağımlı**' ı seçin.
   * **Hedef çalışma zamanı**olarak **Taşınabilir** öğesini seçin.
   * Dağıtımdan sonra ek dosyaları kaldırmanız gerekiyorsa, **dosya yayımlama seçenekleri** ' ni açın ve hedefteki ek dosyaları kaldırmak için onay kutusunu işaretleyin.
   * **Kaydet**’i seçin.
1. Yayımla sihirbazının kalan istemlerini izleyerek yeni bir site oluşturun veya var olan bir siteyi güncelleştirin.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. Proje dosyasında, bir [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog)belirtmeyin.

1. Bir komut kabuğundan, uygulamayı sürüm yapılandırmasında [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutuyla yayımlayın. Aşağıdaki örnekte, uygulama çerçeveye bağlı bir uygulama olarak yayımlanır:

   ```console
   dotnet publish --configuration Release
   ```

1. *Bin/Release/{Target Framework}/Publish* dizininin içeriğini App Service sitesinde siteye taşıyın. *Klasör içeriğini* yerel sabit sürücünüzden veya ağ paylaşımınızdan, [kudu](https://github.com/projectkudu/kudu/wiki) konsolundaki App Service doğrudan sürüklerseniz, `D:\home\site\wwwroot` dosyaları kudu konsolundaki klasöre sürükleyin.

---

### <a name="deploy-the-app-self-contained"></a>Uygulamayı kendi içinde dağıtma

Kendi içinde dağıtım için Visual Studio 'Yu veya komut satırı arabirimi (CLı) araçlarını kullanın [(SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Visual Studio araç çubuğundan **derleme** > **yayımlaması {Application Name}** öğesini seçin veya **Çözüm Gezgini** ' de projeye sağ tıklayıp **Yayımla**' yı seçin.
1. **Bir yayımlama hedefi seç** iletişim kutusunda **App Service** seçili olduğunu onaylayın.
1. **Gelişmiş**'i seçin. **Yayımla** iletişim kutusu açılır.
1. İçinde **Yayımla** iletişim:
   * **Yayın** yapılandırmasının seçili olduğunu doğrulayın.
   * **Dağıtım modu** açılır listesini açın ve **kendinden bağımsız**' i seçin.
   * Hedef **çalışma** zamanı açılır listesinden hedef çalışma zamanını seçin. Varsayılan, `win-x86` değeridir.
   * Dağıtımdan sonra ek dosyaları kaldırmanız gerekiyorsa, **dosya yayımlama seçenekleri** ' ni açın ve hedefteki ek dosyaları kaldırmak için onay kutusunu işaretleyin.
   * **Kaydet**’i seçin.
1. Yayımla sihirbazının kalan istemlerini izleyerek yeni bir site oluşturun veya var olan bir siteyi güncelleştirin.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. Proje dosyasında bir veya daha fazla [çalışma zamanı tanımlayıcısı (RID 'ler)](/dotnet/core/rid-catalog)belirtin. Tek `<RuntimeIdentifier>` bir RID için (tekil) kullanın veya (plural `<RuntimeIdentifiers>` ) kullanarak, noktalı virgülle ayrılmış RID 'ler listesini sağlayın. Aşağıdaki örnekte, `win-x86` RID belirtilir:

   ```xml
   <PropertyGroup>
     <TargetFramework>{TARGET FRAMEWORK}</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```

1. Bir komut kabuğundan, uygulamayı, [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutuyla konağın çalışma zamanının sürüm yapılandırmasında yayımlayın. Aşağıdaki örnekte, uygulama `win-x86` RID için yayımlanır. `--runtime` Seçeneğe sağlanan RID, proje dosyasındaki `<RuntimeIdentifier>` (veya `<RuntimeIdentifiers>`) özelliğinde sağlanmalıdır.

   ```console
   dotnet publish --configuration Release --runtime win-x86 --self-contained
   ```

1. *Bin/Release/{Target Framework}/{RUNTIME Identifier}/Publish* dizininin içeriğini App Service sitesinde siteye taşıyın. *Klasör içeriğini* yerel sabit sürücünüzden veya ağ paylaşımınızdan, kudu konsolundaki App Service doğrudan sürüklerseniz, dosyaları `D:\home\site\wwwroot` kudu konsolundaki klasöre sürükleyin.

---

## <a name="protocol-settings-https"></a>Protokol ayarları (HTTPS)

Güvenli Protokol bağlamaları, HTTPS üzerinden isteklere yanıt verme sırasında kullanılacak bir sertifika belirtmenize olanak tanır. Bağlama, belirli bir ana bilgisayar adı için verilen geçerli bir özel sertifika ( *. pfx*) gerektirir. Daha fazla bilgi için bkz [. Öğretici: Var olan bir özel SSL sertifikasını Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl)bağlayın.

## <a name="transform-webconfig"></a>Web.config’i dönüştürme

*Web. config* 'i yayımlama sırasında dönüştürmeniz gerekiyorsa (örneğin, yapılandırma, profil veya ortama göre ortam değişkenlerini ayarlayın), bkz <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="additional-resources"></a>Ek kaynaklar

* [App Service Genel Bakış](/azure/app-service/app-service-web-overview)
* [Azure App Service: .NET uygulamalarınızı barındırmak için En Iyi yer (55 dakikalık genel bakış videosu)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Cuma: Azure App Service tanılama ve sorun giderme deneyimi (12 dakikalık video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Azure App Service tanılamada genel bakış](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

Windows Server 'da Azure App Service, [Internet Information Services (IIS)](https://www.iis.net/)kullanır. Aşağıdaki konular, temel alınan IIS teknolojisine yöneliktir:

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [Windows Server-geçerli ve önceki sürümler için BT Yöneticisi içeriği](/windows-server/windows-server-versions)
