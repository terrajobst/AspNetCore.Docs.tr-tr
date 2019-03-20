---
title: ASP.NET Core uygulamalarını Azure App Service'e dağıtma
author: guardrex
description: 'Bu makalede, Azure konak bağlantı içerir ve kaynakları dağıtma.'
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: host-and-deploy/azure-apps/index
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a>ASP.NET Core uygulamalarını Azure App Service'e dağıtma

[Azure App Service](https://azure.microsoft.com/services/app-service/) olduğu bir [Microsoft bulut platformu hizmet](https://azure.microsoft.com/) ASP.NET Core web uygulamalarını barındırmak için dahil olmak üzere.

## <a name="useful-resources"></a>Faydalı kaynaklar

Azure [Web Apps belgeleri](/azure/app-service/) Azure uygulamaları belgeler, öğreticiler, örnekler, nasıl yapılır kılavuzları ve diğer kaynaklar için platformdur. ASP.NET Core uygulamaları barındırmak için ilgilidir iki önemli öğreticiler şunlardır:

[Azure'da ASP.NET Core web uygulaması oluşturma](/azure/app-service/app-service-web-get-started-dotnet)  
Visual Studio, oluşturmak ve Windows üzerinde Azure App Service'e bir ASP.NET Core web uygulaması dağıtmak için kullanın.

[Linux üzerinde App Service'te bir ASP.NET Core uygulaması oluşturma](/azure/app-service/containers/quickstart-dotnetcore)  
Oluşturmak ve Linux üzerinde Azure App Service'e bir ASP.NET Core web uygulaması dağıtmak için komut satırını kullanın.

ASP.NET Core belgelerinde aşağıdaki makalelere kullanılabilir:

<xref:tutorials/publish-to-azure-webapp-using-vs>  
Visual Studio kullanarak Azure App Service'e bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin.

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
Visual Studio kullanarak ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtım için Azure App Service'e dağıtın.

[Azure işlem hattı ile ilk işlem hattınızı oluşturun](/azure/devops/pipelines/get-started-yaml)  
ASP.NET Core uygulaması için bir CI derlemesi ayarlayın ve ardından bir Azure App Service'e sürekli dağıtım yayın oluşturun.

[Azure Web uygulama korumalı alanı](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Azure App Service Azure uygulama platformu tarafından zorlanan çalışma zamanı yürütme sınırlamaları keşfedin.

## <a name="application-configuration"></a>Uygulama yapılandırması

### <a name="platform"></a>Platform

::: moniker range=">= aspnetcore-2.2"

Çalışma zamanları (x64) 64-bit ve 32-bit (x 86) uygulamaları için Azure App Service üzerinde yok. [.NET Core SDK'sı](/dotnet/core/sdk) App Service'te 32-bit kullanılabilir, ancak kullanarak 64-bit uygulamaları dağıtabileceğiniz [Kudu](https://github.com/projectkudu/kudu/wiki) konsol veya aracılığıyla [Visual Studio ile MSDeploy yayımlama profilini veya CLI komutunu](xref:host-and-deploy/visual-studio-publish-profiles).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Yerel bağımlılıkları olan uygulamalar için çalışma zamanları 32-bit (x 86) uygulamaları için Azure App Service üzerinde yok. [.NET Core SDK'sı](/dotnet/core/sdk) 32-bit App Service'teki kullanılabilir.

::: moniker-end

### <a name="packages"></a>Paketler

Azure App Service'e dağıtılan uygulamalar için otomatik günlük kaydını özellikler sağlamak için aşağıdaki NuGet paketlerini içerir:

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) kullanan [Ihostingstartup](xref:fundamentals/configuration/platform-specific-configuration) Azure App Service ile ASP.NET Core açık yukarı tümleştirmesi sağlamak için. Eklenen günlüğe kaydetme özelliklerini tarafından sağlanan `Microsoft.AspNetCore.AzureAppServicesIntegration` paket.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) Günlükçü uygulamalarını Azure App Service tanılama günlüklerini ve günlük özellikleri akışı desteklemek için sağlar.

Yukarıdaki paketleri kullanılabilir olmayan [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app). .NET Framework veya başvuru hedefleyen uygulamaları `Microsoft.AspNetCore.App` metapackage tek paketlerden uygulamanın proje dosyasında açıkça başvurmalıdır.

## <a name="override-app-configuration-using-the-azure-portal"></a>Azure portalını kullanarak uygulama yapılandırması geçersiz kıl

Azure Portalı'nda uygulama ayarlarını uygulaması için ortam değişkenlerini ayarlamak için izin verir. Ortam değişkenleri tarafından kullanılabilir [ortam değişkenlerini yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

Ne zaman bir uygulama ayarı oluşturulduğunda veya Azure Portalı'nda değiştirildiğinde ve **Kaydet** düğmesi seçildiğinde, Azure uygulamasını yeniden başlatılır. Hizmet yeniden başlatıldıktan sonra uygulamaya ortam değişkeni kullanılabilir.

Bir uygulama kullandığında [Web ana bilgisayarı](xref:fundamentals/host/web-host) kullanarak ana bilgisayar oluşturur [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), ana bilgisayar yapılandırma ortam değişkenlerini kullanma `ASPNETCORE_` önek. Daha fazla bilgi için <xref:fundamentals/host/web-host> ve [ortam değişkenlerini yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

Bir uygulama kullandığında [genel ana bilgisayar](xref:fundamentals/host/generic-host), ortam değişkenleri uygulamanın yapılandırmasını varsayılan olarak yüklü değildir ve geliştirici tarafından yapılandırma sağlayıcısı eklenmesi gerekir. Yapılandırma sağlayıcısı eklendiğinde Geliştirici ortamı değişken önek belirler. Daha fazla bilgi için <xref:fundamentals/host/generic-host> ve [ortam değişkenlerini yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Ara sunucu ve yük dengeleyici senaryoları

İletilen üstbilgileri ara yazılım ve ASP.NET Core modülü yapılandırır IIS tümleştirme Ara şema (HTTP/HTTPS) ve isteğin geldiği uzak IP adresine iletecek şekilde yapılandırılır. Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir. Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>İzleme ve günlüğe kaydetme

App Service için otomatik olarak dağıtılan bir ASP.NET Core uygulamaları bir App Service uzantısını alma **ASP.NET Core günlüğü uzantıları**. Uzantı Azure günlük kaydını etkinleştirir.

İzleme, günlüğe kaydetme ve sorun giderme bilgileri için aşağıdaki makalelere bakın:

[Nasıl yapılır: Azure App Service'te uygulamaları izleme](/azure/app-service/web-sites-monitor)  
Kotalar ve uygulamaları ve App Service planları için ölçümleri gözden geçirmeyi öğrenin.

[Azure App Service'te web apps için tanılama günlüğünü etkinleştirme](/azure/app-service/web-sites-enable-diagnostic-log)  
HTTP durum kodları, başarısız istekler ve web sunucusu etkinliğini için tanılama günlüğüne kaydetme erişimi nasıl etkinleştirileceği keşfedin.

<xref:fundamentals/error-handling>  
ASP.NET Core uygulamalarında hata işleme için genel yaklaşımları anlayın.

<xref:host-and-deploy/azure-apps/troubleshoot>  
ASP.NET Core uygulamaları ile Azure App Service dağıtımı ile ilgili sorunları tanılamayı öğrenin.

<xref:host-and-deploy/azure-iis-errors-reference>  
Sık karşılaşılan dağıtım yapılandırma hatalarını sorun giderme önerilerine ile Azure App Service/IIS tarafından barındırılan uygulamalar için bkz.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Veri koruma anahtarı halka ve dağıtım yuvaları

[Veri koruma anahtarları](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) için kalıcı *%HOME%\ASP.NET\DataProtection-Keys* klasör. Bu klasör, ağ depolaması tarafından desteklenir ve uygulamayı barındıran tüm makinelerde eşitlenir. Anahtarlar, bekleme sırasında korunmayan. Bu klasör, anahtar halkası tek dağıtım yuvasındaki bir uygulamanın tüm örnekleri sağlar. Hazırlama ve üretim gibi farklı dağıtım yuvaları, anahtar halkası paylaşmayın.

Dağıtım yuvası arasında değiştirme, veri korumayı kullanarak herhangi bir sistem önceki yuvasına anahtar halkası kullanarak depolanan verilerin şifresini çözmek mümkün olmayacaktır. ASP.NET tanımlama bilgisi ara yazılım, veri koruma, tanımlama bilgilerini korumak için kullanır. Bu, standart ASP.NET tanımlama bilgisi Ara kullanan bir uygulamanın oturumunu kapatmadan kullanıcılara yol açar. Yuva bağımsız anahtar halkası çözümü için bir dış anahtar halkası sağlayıcısı gibi kullanın:

* Azure Blob Depolama
* Azure Key Vault
* SQL depolama
* Redis önbelleği

Daha fazla bilgi için bkz. <xref:security/data-protection/implementation/key-storage-providers>.

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>ASP.NET Core Önizleme sürümü, Azure App Service'e dağıtma

Aşağıdaki yaklaşımlardan birini kullanın:

* [Önizleme site uzantısını yüklemek](#install-the-preview-site-extension).
* [Kendi içinde uygulama dağıtmak](#deploy-the-app-self-contained).
* [Docker, kapsayıcılar için Web Apps ile kullanma](#use-docker-with-web-apps-for-containers).

### <a name="install-the-preview-site-extension"></a>Önizleme sitesi uzantısını yükle

Önizleme site uzantısı kullanarak bir sorun meydana gelirse, açık bir [aspnet/AspNetCore sorunu](https://github.com/aspnet/AspNetCore/issues).

1. Azure Portalı'ndan uygulama hizmetine gidin.
1. Web uygulamasını seçin.
1. Tür "ör" veya "Uzantıları" kaydırma için filtrelemek için arama kutusuna Yönetim Araçları listesini aşağı.
1. Seçin **uzantıları**.
1. **Add (Ekle)** seçeneğini belirleyin.
1. Seçin **ASP.NET Core {X.Y} ({x64 | x86}) çalışma zamanı** uzantısı listeden burada `{X.Y}` ASP.NET Core Önizleme sürümüdür ve `{x64|x86}` platformunu belirtir.
1. Seçin **Tamam** yasal koşulları kabul etmek için.
1. Seçin **Tamam** uzantıyı yüklemek için.

İşlem tamamlandığında, en son .NET Core Önizleme yüklenir. Yüklemeyi doğrulama:

1. Seçin **Gelişmiş Araçlar**.
1. Seçin **Git** içinde **Gelişmiş Araçlar**.
1. Seçin **hata ayıklama konsoluna** > **PowerShell** menü öğesi.
1. PowerShell komut isteminde aşağıdaki komutu yürütün. ASP.NET Core çalışma zamanı sürümü yerine `{X.Y}` ve platformu `{PLATFORM}` komutta:

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```

   Komut döndürür `True` olduğunda önizlemesi çalışma zamanı yüklü x64.

> [!NOTE]
> Uygulama Hizmetleri uygulama platformu mimarisi (x86/x64), uygulama ayarlarında, bir A serisi işlem üzerinde barındırılan ya da daha iyi katmanını barındıran uygulamalar için Azure Portalı'nda ayarlanır. Uygulama, işlem içi modunda çalıştırıldığında ve platform mimarisi için 64-bit (x64) yapılandırılmışsa, ASP.NET Core modülü varsa, 64-bit önizlemesi çalışma zamanı kullanır. Yükleme **ASP.NET Core {X.Y} (x64) çalışma zamanı** uzantısı.
>
> X64 yükledikten sonra çalışma zamanı Önizleme, yüklemeyi doğrulamak için Kudu PowerShell komut penceresinde aşağıdaki komutu çalıştırın. ASP.NET Core çalışma zamanı sürümü yerine `{X.Y}` komutta:
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
> Komut döndürür `True` olduğunda önizlemesi çalışma zamanı yüklü x64.

> [!NOTE]
> **ASP.NET Core uzantıları** Azure günlük kaydı etkinleştirme gibi ek işlevler üzerinde Azure App Services, ASP.NET Core sağlar. Uzantı, Visual Studio'dan dağıtım yaparken otomatik olarak yüklenir. Uzantı yüklü değilse, uygulamayı yükleyin.

**Bir ARM şablonu ile Önizleme site uzantısı kullanma**

Bir ARM şablonu, uygulamaları oluşturup dağıtmak için kullanılıyorsa `siteextensions` kaynak türü, bir web uygulamasına site uzantısı eklemek için kullanılabilir. Örneğin:

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a>Kendi içinde uygulama dağıtma

A [müstakil dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) önizlemesini hedefleyen çalışma zamanı dağıtımda önizlemesi çalışma zamanı taşır.

Kendi içinde uygulama dağıtırken:

* Azure App Service'te site gerektirmeyen [Önizleme site uzantısı](#install-the-preview-site-extension).
* Uygulama yayımlama olduğunda daha farklı bir yaklaşım izleyerek yayımlanmalıdır bir [framework bağımlı dağıtım (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).

#### <a name="publish-from-visual-studio"></a>Visual Studio'dan yayımlama

1. Seçin **derleme** > **yayımlama {uygulama-adı}** Visual Studio araç çubuğundan.
1. İçinde **yayımlama hedefi seçin** iletişim kutusunda onaylayın **App Service** seçilir.
1. **Gelişmiş**'i seçin. **Yayımla** iletişim kutusu açılır.
1. İçinde **Yayımla** iletişim:
   * Onaylayın **yayın** yapılandırması seçili.
   * Açık **dağıtım modu** aşağı açılan listesinden **müstakil**.
   * Hedef çalışma zamanını şuradan seçin **hedef çalışma zamanı** aşağı açılan listesi. Varsayılan, `win-x86` değeridir.
   * Ek dosyaları dağıtım kaldırmanız gerekirse, açık **dosya yayımlama seçeneği** ve hedefteki ek dosyaları kaldırmak için bu onay kutusunu seçin.
   * **Kaydet**’i seçin.
1. Yeni bir site veya mevcut bir site Yayımlama Sihirbazı'nın kalan istemleri izleyerek güncelleştirilemiyor.

#### <a name="publish-using-command-line-interface-cli-tools"></a>Komut satırı arabirimi (CLI) araçlarını kullanarak yayımla

1. Proje dosyasında bir veya daha fazla belirtin [çalışma zamanı tanımlayıcılarının (RID'ler)](/dotnet/core/rid-catalog). Kullanma `<RuntimeIdentifier>` tek RID veya kullanın (tekil) `<RuntimeIdentifiers>` RID'ler noktalı virgülle ayrılmış bir listesini sağlamak üzere (çoğul). Aşağıdaki örnekte, `win-x86` RID belirtilir:

   ```xml
   <PropertyGroup>
     <TargetFramework>netcoreapp2.1</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```

1. Bir komut kabuğu'ndan, sürüm yapılandırmasında ana bilgisayarın çalışma zamanı ile uygulamayı yayımlama [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu. Aşağıdaki örnekte, uygulama için yayımlanan `win-x86` RID. Sağlanan RID `--runtime` seçeneği sağlanmalıdır `<RuntimeIdentifier>` (veya `<RuntimeIdentifiers>`) proje dosyasındaki özellik.

   ```console
   dotnet publish --configuration Release --runtime win-x86
   ```

1. İçeriği Taşı *bin/bırakma / {TARGET FRAMEWORK} / {çalışma zamanı TANIMLAYICISI} / publish* App Service'te bir siteye dizin.

### <a name="use-docker-with-web-apps-for-containers"></a>Docker, kapsayıcılar için Web Apps ile kullanma

[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) en son içeren Docker görüntüleri önizleme. Görüntü, temel görüntü olarak kullanılabilir. Görüntüyü kullanın ve kapsayıcılar için Web Apps için normal olarak dağıtın.

## <a name="protocol-settings-https"></a>Protokol ayarları (HTTPS)

Güvenli protokol bağlamalar HTTPS üzerinden isteklerine yanıt verirken kullanmak üzere bir sertifika belirtin izin verir. Bağlama, geçerli özel sertifikaları gerektirir (*.pfx*) belirli ana bilgisayar adı için verilmiş. Daha fazla bilgi için [Öğreticisi: Azure Web Apps'e mevcut özel bir SSL sertifikası bağlama](/azure/app-service/app-service-web-tutorial-custom-ssl).

## <a name="transform-webconfig"></a>Web.config’i dönüştürme

Dönüştürmeniz gerekirse *web.config* (örneğin, ortam değişkenlerini ayarlama, yapılandırma, profili veya ortama göre), bkz: yayımlama sırasında <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="additional-resources"></a>Ek kaynaklar

* [Web Apps'e genel bakış (5 dakikalık genel bakış videosu)](/azure/app-service/app-service-web-overview)
* [Azure uygulama hizmeti: En iyi .NET uygulamalarınızı (55 dakikalık genel bakış videosu) konağa yerleştirin.](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday: Azure App Service tanılama ve sorun giderme deneyimini (12 dakikalık video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Azure App Service tanılama genel bakış](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

Windows Server üzerinde Azure App Service kullanan [Internet Information Services (IIS)](https://www.iis.net/). Aşağıdaki konular temel alınan IIS teknolojiye ilgilidir:

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [Microsoft TechNet Kitaplığı: Windows Server](/windows-server/windows-server-versions)
