---
title: Azure App Service'te ASP.NET Core barındırma
author: guardrex
description: Faydalı kaynaklara bağlantılarla Azure App Service'te ASP.NET Core uygulamaları barındırmak nasıl keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 07/24/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 9a7d20378cac597b748d8a60eb0f0bf17c9ba082
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "41756288"
---
# <a name="host-aspnet-core-on-azure-app-service"></a>Azure App Service'te ASP.NET Core barındırma

[Azure App Service](https://azure.microsoft.com/services/app-service/) olduğu bir [Microsoft bulut platformu hizmet](https://azure.microsoft.com/) ASP.NET Core web uygulamalarını barındırmak için dahil olmak üzere.

## <a name="useful-resources"></a>Faydalı kaynaklar

Azure [Web Apps belgeleri](/azure/app-service/) Azure uygulamaları belgeler, öğreticiler, örnekler, nasıl yapılır kılavuzları ve diğer kaynaklar için platformdur. ASP.NET Core uygulamaları barındırmak için ilgilidir iki önemli öğreticiler şunlardır:

[Hızlı Başlangıç: Azure'da bir ASP.NET Core web uygulaması oluşturma](/azure/app-service/app-service-web-get-started-dotnet)  
Visual Studio, oluşturmak ve Windows üzerinde Azure App Service'e bir ASP.NET Core web uygulaması dağıtmak için kullanın.

[Hızlı Başlangıç: Linux üzerinde App Service'te .NET Core web uygulaması oluşturma](/azure/app-service/containers/quickstart-dotnetcore)  
Oluşturmak ve Linux üzerinde Azure App Service'e bir ASP.NET Core web uygulaması dağıtmak için komut satırını kullanın.

ASP.NET Core belgelerinde aşağıdaki makalelere kullanılabilir:

[Visual Studio ile Azure'a yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs)  
Visual Studio kullanarak Azure App Service'e bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin.

[CLI araçları ile Azure’a yayımlama](xref:tutorials/publish-to-azure-webapp-using-cli)  
Azure App Service'e Git komut satırı istemcisini kullanarak bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin.

[Visual Studio ve Git ile Azure’a sürekli dağıtım](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
Visual Studio kullanarak ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtım için Azure App Service'e dağıtın.

[VSTS ile Azure’a sürekli dağıtım](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
ASP.NET Core uygulaması için bir CI derlemesi ayarlayın ve ardından bir Azure App Service'e sürekli dağıtım yayın oluşturun.

[Azure Web uygulama korumalı alanı](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Azure App Service Azure uygulama platformu tarafından zorlanan çalışma zamanı yürütme sınırlamaları keşfedin.

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a>Uygulama yapılandırması

ASP.NET Core 2.0 veya sonraki sürümlerde, aşağıdaki NuGet paketlerini Azure App Service'e dağıtılan uygulamalar için otomatik günlük tutma özellikleri sağlar:

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) kullanan [Ihostingstartup](xref:fundamentals/configuration/platform-specific-configuration) Azure App Service ile ASP.NET Core açık yukarı tümleştirmesi sağlamak için. Eklenen günlüğe kaydetme özelliklerini tarafından sağlanan `Microsoft.AspNetCore.AzureAppServicesIntegration` paket.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) Günlükçü uygulamalarını Azure App Service tanılama günlüklerini ve günlük özellikleri akışı desteklemek için sağlar.

.NET Core'u hedefleyen ve bunlara başvurma [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), paketleri zaten dahildir. Eksik paketleri yeni gelen [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app). .NET Framework'ü hedefleyen veya başvuru `Microsoft.AspNetCore.App` metapackage, tek tek günlük paketleri başvuru.

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Ara sunucu ve yük dengeleyici senaryoları

İletilen üstbilgileri ara yazılım ve ASP.NET Core modülü yapılandırır IIS tümleştirme Ara şema (HTTP/HTTPS) ve isteğin geldiği uzak IP adresine iletecek şekilde yapılandırılır. Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir. Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>İzleme ve günlüğe kaydetme

İzleme, günlüğe kaydetme ve sorun giderme bilgileri için aşağıdaki makalelere bakın:

[Nasıl yapılır: Azure App Service'te uygulamaları izleme](/azure/app-service/web-sites-monitor)  
Kotalar ve uygulamaları ve App Service planları için ölçümleri gözden geçirmeyi öğrenin.

[Azure App Service'te web apps için tanılama günlüğünü etkinleştirme](/azure/app-service/web-sites-enable-diagnostic-log)  
HTTP durum kodları, başarısız istekler ve web sunucusu etkinliğini için tanılama günlüğüne kaydetme erişimi nasıl etkinleştirileceği keşfedin.

[Hata ASP.NET çekirdek işleme giriş](xref:fundamentals/error-handling)  
ASP.NET Core uygulamalarında hata işleme için genel yaklaşımları anlayın.

[Azure App Service’te uygulama sorunlarını giderme](xref:host-and-deploy/azure-apps/troubleshoot)  
ASP.NET Core uygulamaları ile Azure App Service dağıtımı ile ilgili sorunları tanılamayı öğrenin.

[Azure App Service ve IIS ile ASP.NET Core için sık karşılaşılan hatalar başvurusu](xref:host-and-deploy/azure-iis-errors-reference)  
Sık karşılaşılan dağıtım yapılandırma hatalarını sorun giderme önerilerine ile Azure App Service/IIS tarafından barındırılan uygulamalar için bkz.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Veri koruma anahtarı halka ve dağıtım yuvaları

[Veri koruma anahtarları](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) için kalıcı *%HOME%\ASP.NET\DataProtection-Keys* klasör. Bu klasör, ağ depolaması tarafından desteklenir ve uygulamayı barındıran tüm makinelerde eşitlenir. Anahtarlar, bekleme sırasında korunmayan. Bu klasör, anahtar halkası tek dağıtım yuvasındaki bir uygulamanın tüm örnekleri sağlar. Hazırlama ve üretim gibi farklı dağıtım yuvaları, anahtar halkası paylaşmayın.

Dağıtım yuvası arasında değiştirme, veri korumayı kullanarak herhangi bir sistem önceki yuvasına anahtar halkası kullanarak depolanan verilerin şifresini çözmek mümkün olmayacaktır. ASP.NET tanımlama bilgisi ara yazılım, veri koruma, tanımlama bilgilerini korumak için kullanır. Bu, standart ASP.NET tanımlama bilgisi Ara kullanan bir uygulamanın oturumunu kapatmadan kullanıcılara yol açar. Yuva bağımsız anahtar halkası çözümü için bir dış anahtar halkası sağlayıcısı gibi kullanın:

* Azure Blob Depolama
* Azure anahtar kasası
* SQL depolama
* Redis önbelleği

Daha fazla bilgi için [anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers).

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>ASP.NET Core Önizleme sürümü, Azure App Service'e dağıtma

ASP.NET Core Önizleme uygulamalarını Azure App Service için aşağıdaki yaklaşımlardan ile dağıtılabilir:

* [Önizleme sitesi uzantısını yükle](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) -->
* [Docker, kapsayıcılar için Web Apps ile kullanma](#use-docker-with-web-apps-for-containers)

Önizleme site uzantısı kullanarak bir sorun ortaya çıkarsa, bir sorun açın [GitHub](https://github.com/aspnet/azureintegration/issues/new).

### <a name="install-the-preview-site-extension"></a>Önizleme sitesi uzantısını yükle

1. Azure portalda, App Service dikey penceresine gidin.
1. Web uygulamasını seçin.
1. Arama kutusuna "ör" girin veya yönetim bölmelerini listesini aşağı kaydırın **geliştirme araçları**.
1. Seçin **geliştirme araçları** > **uzantıları**.
1. Seçin **ekleme**.

   ![Önceki adımları ile azure uygulama dikey penceresi](index/_static/x1.png)

1. Seçin **ASP.NET Core uzantıları**.
1. Seçin **Tamam** yasal koşulları kabul etmek için.
1. Seçin **Tamam** uzantıyı yüklemek için.

Ekleme işlemleri tamamladığınızda, en son .NET Core Önizleme yüklenir. Çalıştırarak yüklemeyi doğrulayın `dotnet --info` konsolunda. Gelen **App Service** dikey penceresinde:

1. "Arama kutusu veya kaydırma yönetim bölmeleri serilerini için con" girin **geliştirme araçları**.
1. Seçin **geliştirme araçları** > **konsol**.
1. Girin `dotnet --info` konsolunda.

Varsa sürüm `2.1.300-preview1-008174` en son Önizleme sürümü olduğundan aşağıdaki çıktıyı çalıştırarak elde edilen `dotnet --info` komut isteminde:

![Önceki adımları ile azure uygulama dikey penceresi](index/_static/cons.png)

ASP.NET Core önceki görüntüde gösterilen sürümünü `2.1.300-preview1-008174`, bir örnek verilmiştir. ASP.NET Core site uzantısını yapılandırılmış zaman en son önizleme sürümünü yürüttüğünüzde görünür `dotnet --info`.

`dotnet --info` Önizleme burada yüklenmiş site uzantısı yolunu görüntüler. Uygulama yerine varsayılan site uzantısını çalıştırılmasının gösterir *ProgramFiles* konumu. Görürseniz *ProgramFiles*, siteyi yeniden başlatın ve çalıştırın `dotnet --info`.

**Bir ARM şablonu ile Önizleme site uzantısı kullanma**

Bir ARM şablonu, uygulamaları oluşturup dağıtmak için kullanılıyorsa `siteextensions` kaynak türü, bir web uygulamasına site uzantısı eklemek için kullanılabilir. Örneğin:

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a>Docker, kapsayıcılar için Web Apps ile kullanma

[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) en son içeren Docker görüntüleri önizleme. Görüntü, temel görüntü olarak kullanılabilir. Görüntüyü kullanın ve kapsayıcılar için Web Apps için normal olarak dağıtın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Web Apps'e genel bakış (5 dakikalık genel bakış videosu)](/azure/app-service/app-service-web-overview)
* [Azure App Service: Konak için en iyi, .NET uygulamalarınızı (55 dakikalık genel bakış videosu) yerleştirin.](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday: Azure App Service tanılama ve sorun giderme deneyimini (12 dakikalık video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Azure App Service tanılama genel bakış](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

Windows Server üzerinde Azure App Service kullanan [Internet Information Services (IIS)](https://www.iis.net/). Aşağıdaki konular temel alınan IIS teknolojiye ilgilidir:

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [Microsoft TechNet Kitaplığı'de: Windows Server](/windows-server/windows-server-versions)
