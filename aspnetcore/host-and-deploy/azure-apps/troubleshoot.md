---
title: Azure App Service'te ASP.NET Core sorunlarını giderme
author: guardrex
description: ASP.NET Core Azure App Service dağıtım sorunlarını tanılamayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 03/05/2019
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: c3732bfab362ec034248eb3912d4b1337c94216e
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665434"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Azure App Service'te ASP.NET Core sorunlarını giderme

Tarafından [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

Bu makalede, Azure App Service'nın tanılama araçlarını kullanarak uygulama başlatma çıkış nasıl bir ASP.NET Core tanılamak yönergeler sağlar. Ek sorun giderme öneriler için bkz. [Azure App Service tanılama genel bakış](/azure/app-service/app-service-diagnostics) ve [nasıl yapılır: Azure App Service'te uygulamaları izleme](/azure/app-service/web-sites-monitor) Azure belgeleri.

## <a name="app-startup-errors"></a>Uygulama başlatma hataları

**502.5 işlem hatası**  
Çalışan işlemi başarısız olur. Uygulama başlamaz.

[ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) başlatmak çalışan işlemi ancak başlatma girişimleri başarısız. Genellikle uygulama olay günlüğünü inceleyerek bu tür bir sorun gidermenize yardımcı olur. Günlük erişme bölümünde açıklanmıştır [uygulama olay günlüğüne](#application-event-log) bölümü.

*502.5 işlem hatası* hata sayfası, yanlış yapılandırılmış bir uygulama çalışan işleminin başarısız olmasına neden olduğunda döndürülür:

![502.5 işlem hatası sayfasını gösteren bir tarayıcı penceresi](troubleshoot/_static/process-failure-page.png)

**500 İç sunucu hatası**  
Uygulamayı başlatır, ancak bir hata sunucu isteği yerine getirmesini önler.

Bu hata, başlatma sırasında veya bir yanıt oluşturulurken uygulamanın kod içinde oluşur. Yanıtın içerik içerebilir veya yanıt olarak görünebilir bir *500 İç sunucu hatası* tarayıcıda. Uygulama olay günlüğü, genellikle uygulama normal şekilde çalışmaya belirtir. Sunucunun açısından bakıldığında, doğru olmasıdır. Uygulama başladı, ancak geçerli bir yanıt oluşturulamıyor. [Uygulamayı çalıştırın Kudu konsolunda](#run-the-app-in-the-kudu-console) veya [ASP.NET Core modülü stdout günlüğünü etkinleştir](#aspnet-core-module-stdout-log) sorunu gidermek için.

**Bağlantı sıfırlama**

Üst bilgileri gönderildiğinde sonra bir hata oluşursa, sunucunun göndermek çok geç bir **500 İç sunucu hatası** bir hata oluştuğunda. Bu durum, genellikle bir yanıt için karmaşık nesne serileştirme sırasında bir hata oluştuğunda gerçekleşir. Bu tür olarak görünür bir *bağlantı sıfırlama* istemci üzerinde hata. [Uygulama günlüğü](xref:fundamentals/logging/index) bu tür hataları gidermeye yardımcı olabilir.

## <a name="default-startup-limits"></a>Varsayılan başlangıç sınırları

ASP.NET Core modülü ile bir varsayılan yapılandırılmış *startupTimeLimit* 120 saniye. Varsayılan değer olarak sol uygulama modülü bir işlem hatası oturum önce başlatmak için iki dakika sürebilir. Modül yapılandırma hakkında daha fazla bilgi için bkz. [aspNetCore öğenin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Uygulama başlatma hatalarını giderme

### <a name="application-event-log"></a>Uygulama olay günlüğü

Uygulama olay günlüğüne erişmek için **Tanıla ve problemleri çözmenize** Azure portalındaki dikey penceresinde:

1. Azure portalında, uygulamada açmak **uygulama hizmetleri**.
1. Seçin **tanılayın ve sorunlarını çözmek**.
1. Seçin **tanılama araçları** başlığı.
1. Altında **Destek Araçları**seçin **uygulama olayları** düğmesi.
1. Tarafından sağlanan en son hatasını inceleyin *IIS AspNetCoreModule* veya *IIS AspNetCoreModule V2* girişi **kaynak** sütun.

Kullanmaya alternatif **tanılayın ve sorunlarını çözmek** dikey olan kullanarak doğrudan uygulama olay günlüğü dosyasını incelemek için [Kudu](https://github.com/projectkudu/kudu/wiki):

1. Açık **Gelişmiş Araçlar** içinde **geliştirme araçları** alan. Seçin **Git&rarr;**  düğmesi. Kudu konsolunu bir yeni tarayıcı sekmesinde veya penceresinde açılır.
1. Sayfanın en üstünde gezinti çubuğunu kullanarak açmak **hata ayıklama konsoluna** seçip **CMD**.
1. Açık **LogFiles** klasör.
1. Kalem simgesini seçin *eventlog.xml* dosya.
1. Günlüğünü inceleyin. En son olayları görmek için günlüğü alt kısmına kaydırın.

### <a name="run-the-app-in-the-kudu-console"></a>Kudu konsolunda uygulamayı çalıştırma

Başlatma hataları birçok yararlı bilgiler uygulama olay günlüğü'ndeki üretmediği. Uygulamayı çalıştırabilirsiniz [Kudu](https://github.com/projectkudu/kudu/wiki) hatayı bulmak için uzaktan yürütme konsolu:

1. Açık **Gelişmiş Araçlar** içinde **geliştirme araçları** alan. Seçin **Git&rarr;**  düğmesi. Kudu konsolunu bir yeni tarayıcı sekmesinde veya penceresinde açılır.
1. Sayfanın en üstünde gezinti çubuğunu kullanarak açmak **hata ayıklama konsoluna** seçip **CMD**.

#### <a name="test-a-32-bit-x86-app"></a>Bir 32-bit (x 86) uygulamayı test etme

##### <a name="current-release"></a>Geçerli yayın

1. `cd d:\home\site\wwwroot`
1. Uygulamayı çalıştırın:
   * Uygulama ise bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

     ```console
     dotnet .\{ASSEMBLY NAME}.dll
     ```
   * Uygulama ise bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd):

     ```console
     {ASSEMBLY NAME}.exe
     ```
   
Konsol çıkışını herhangi bir hata gösteren uygulamadan Kudu konsoluna cmdlet'iyle yönetilir.
   
##### <a name="framework-depdendent-deployment-running-on-a-preview-release"></a>Bir önizleme sürümü üzerinde çalışan framework depdendent dağıtım

*ASP.NET Core {VERSION} (x86) yükleme gerektirir çalışma zamanı site uzantısı.*

1. `cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` çalışma zamanı sürümü)
1. Uygulamayı çalıştırın: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`

Konsol çıkışını herhangi bir hata gösteren uygulamadan Kudu konsoluna cmdlet'iyle yönetilir.

#### <a name="test-a-64-bit-x64-app"></a>Bir 64-bit (x 64) uygulamayı test etme

##### <a name="current-release"></a>Geçerli yayın

* Uygulamanın bir 64-bit (x64) olup olmadığını [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd):
  1. `cd D:\Program Files\dotnet`
  1. Uygulamayı çalıştırın: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`
* Uygulama ise bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd):
  1. `cd D:\home\site\wwwroot`
  1. Uygulamayı çalıştırın: `{ASSEMBLY NAME}.exe`

Konsol çıkışını herhangi bir hata gösteren uygulamadan Kudu konsoluna cmdlet'iyle yönetilir.

##### <a name="framework-depdendent-deployment-running-on-a-preview-release"></a>Bir önizleme sürümü üzerinde çalışan framework depdendent dağıtım

*ASP.NET Core {VERSION} (x64) yükleme gerektirir çalışma zamanı site uzantısı.*

1. `cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` çalışma zamanı sürümü)
1. Uygulamayı çalıştırın: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`

Konsol çıkışını herhangi bir hata gösteren uygulamadan Kudu konsoluna cmdlet'iyle yönetilir.

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET Core modülü stdout günlüğü

ASP.NET Core modülü stdout günlük genellikle uygulama olay günlüğüne bulunamadı kullanışlı hata iletileri kaydeder. Stdout günlükleri görüntülemek ve etkinleştirmek için:

1. Gidin **Tanıla ve problemleri çözmenize** Azure portalındaki dikey penceresinde.
1. Altında **sorun KATEGORİSİ seçin**seçin **Web uygulaması kapalı** düğmesi.
1. Altında **önerilen çözümleri** > **Stdout günlük yeniden yönlendirmeyi etkinleştirmek**, düğmeyi seçin **Web.config'i düzenlemek zorunda açık Kudu Konsolu**.
1. Kudu içinde **tanılama Konsolu**, yoluna Klasör Aç **site** > **wwwroot**. Kaydırarak aşağı açığa çıkar *web.config* listenin altındaki dosya.
1. Kalem simgesine tıklayın *web.config* dosya.
1. Ayarlama **stdoutLogEnabled** için `true` değiştirip **stdoutLogFile** yolu: `\\?\%home%\LogFiles\stdout`.
1. Seçin **Kaydet** güncelleştirilmiş kaydetmek için *web.config* dosya.
1. Uygulamaya bir istek oluşturun.
1. Azure portalına dönün. Seçin **Gelişmiş Araçlar** dikey penceresinde **geliştirme araçları** alan. Seçin **Git&rarr;**  düğmesi. Kudu konsolunu bir yeni tarayıcı sekmesinde veya penceresinde açılır.
1. Sayfanın en üstünde gezinti çubuğunu kullanarak açmak **hata ayıklama konsoluna** seçip **CMD**.
1. Seçin **LogFiles** klasör.
1. İnceleme **değiştirilen** son değiştirilme tarihi ile oturum sütun ve stdout düzenlemek için kalem simgesini seçin.
1. Günlük dosyası açıldığında bir hata görüntülenmiyor.

Sorun giderme işlemi tamamlandıktan sonra stdout günlüğü devre dışı bırakın:

1. Kudu içinde **tanılama Konsolu**, dönüş yoluna **site** > **wwwroot** açığa çıkarmak için *web.config* dosya. Açık **web.config** kalem simgesini seçerek yeniden dosya.
1. Ayarlama **stdoutLogEnabled** için `false`.
1. Seçin **Kaydet** dosyayı kaydetmek için.

> [!WARNING]
> Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir. Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur. Yalnızca uygulama başlatma sorunlarını gidermek için günlüğü stdout kullanın.
>
> Genel ASP.NET Core uygulaması başlatma işleminden sonra için günlüğü, günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın. Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log"></a>ASP.NET Core modülü hata ayıklama günlüğü

ASP.NET Core modülü hata ayıklama günlüğünü ek, daha ayrıntılı günlük kaydı, ASP.NET Core modülü sağlar. Stdout günlükleri görüntülemek ve etkinleştirmek için:

1. Gelişmiş Tanılama Günlüğü etkinleştirmek için aşağıdakilerden birini gerçekleştirin:
   * Bölümündeki yönergeleri [Gelişmiş tanılama günlükleri](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) uygulama için Gelişmiş tanılama günlük kaydını yapılandırmak için. Uygulamayı yeniden dağıtın.
   * Ekleme `<handlerSettings>` gösterilen [Gelişmiş tanılama günlükleri](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) için Canlı uygulamanın *web.config* Kudu konsolunu kullanarak dosya:
     1. Açık **Gelişmiş Araçlar** içinde **geliştirme araçları** alan. Seçin **Git&rarr;**  düğmesi. Kudu konsolunu bir yeni tarayıcı sekmesinde veya penceresinde açılır.
     1. Sayfanın en üstünde gezinti çubuğunu kullanarak açmak **hata ayıklama konsoluna** seçip **CMD**.
     1. Klasör yolu açın **site** > **wwwroot**. Düzen *web.config* Kalem düğmesine seçerek dosya. Ekleme `<handlerSettings>` bölümünde gösterildiği gibi [Gelişmiş tanılama günlükleri](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs). **Kaydet** düğmesini seçin.
1. Açık **Gelişmiş Araçlar** içinde **geliştirme araçları** alan. Seçin **Git&rarr;**  düğmesi. Kudu konsolunu bir yeni tarayıcı sekmesinde veya penceresinde açılır.
1. Sayfanın en üstünde gezinti çubuğunu kullanarak açmak **hata ayıklama konsoluna** seçip **CMD**.
1. Klasör yolu açın **site** > **wwwroot**. İçin bir yol belirtmediyseniz, *aspnetcore debug.log* dosya, dosya, listede görünür. Bir yol belirtilirse, günlük dosyasının konumuna gidin.
1. Dosya adının yanındaki kalem düğmesine ile günlük dosyasını açın.

Sorun giderme işlemi tamamlandıktan sonra hata ayıklama günlüğü devre dışı bırakın:

1. Gelişmiş hata ayıklama günlüğü, genellikle aşağıdakilerden birini devre dışı bırakmak için:
   * Kaldırma `<handlerSettings>` gelen *web.config* dosyasını yerel ve uygulamayı yeniden dağıtın.
   * Düzenlemek için Kudu Konsolu kullanın *web.config* dosya ve kaldırma `<handlerSettings>` bölümü. Dosyayı kaydedin.

> [!WARNING]
> Uygulama veya sunucu başarısızlığı için hata ayıklama günlüğünü devre dışı bırakmak için hata neden olabilir. Günlük dosyası boyutu sınırı yoktur. Yalnızca uygulama başlatma sorunlarını gidermek için hata ayıklama günlüğü kullanın.
>
> Genel ASP.NET Core uygulaması başlatma işleminden sonra için günlüğü, günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın. Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).

::: moniker-end

## <a name="common-startup-errors"></a>Ortak başlatma hataları

Bkz. <xref:host-and-deploy/azure-iis-errors-reference>. Uygulama başlatma önleyen yaygın sorunların çoğunu başvuru konusunda ele alınmaktadır.

## <a name="slow-or-hanging-app"></a>Yavaş veya asılı uygulama

Bir uygulama yavaş yanıt ya da bir isteği yanıt vermemeye başlıyor bkz [sorun giderme yavaş web uygulaması performans sorunlarını Azure App Service'te](/azure/app-service/app-service-web-troubleshoot-performance-degradation) Kılavuzu hata ayıklama.

## <a name="remote-debugging"></a>Uzaktan hata ayıklama

Aşağıdaki konulara bakın:

* [Uzaktan hata ayıklama Web uygulamaları Visual Studio kullanarak Azure App Service web uygulamasında sorun giderme bölümünü](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (Azure belgeleri)
* [Azure'da Visual Studio 2017'de IIS üzerinde uzaktan hata ayıklama ASP.NET Core](/visualstudio/debugger/remote-debugging-azure) (Visual Studio belgeleri)

## <a name="application-insights"></a>Application Insights

[Application Insights](https://azure.microsoft.com/services/application-insights/) tan alınan telemetri verilerini günlüğe kaydetme ve raporlama özellikleri hata dahil olmak üzere Azure App Service'te barındırılan uygulamalar sağlar. Application Insights, yalnızca uygulamanın günlüğe kaydetme özelliklerini kullanılabilir olduğunda uygulama başladıktan sonra gerçekleşen hataları üzerinde rapor edebilirsiniz. Daha fazla bilgi için [ASP.NET Core için Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="monitoring-blades"></a>İzleme dikey pencereleri

İzleme dikey pencereler, sorun giderme deneyimini konusunda daha önce açıklanan yöntemlerine bir alternatif sunar. Bu dikey 500 serisi hataları tanılamak için kullanılabilir.

ASP.NET Core uzantılar yüklü olduğunu doğrulayın. Uzantılar yüklü değilse, bunları el ile yükleyin:

1. İçinde **geliştirme araçları** dikey penceresi bölümünden **uzantıları** dikey penceresi.
1. **ASP.NET Core uzantıları** listede görünmesi gerekir.
1. Uzantılar yüklü değilse seçin **Ekle** düğmesi.
1. Seçin **ASP.NET Core uzantıları** listeden.
1. Seçin **Tamam** yasal koşulları kabul etmek için.
1. Seçin **Tamam** üzerinde **uzantısı ekleme** dikey penceresi.
1. Ne zaman uzantılar başarıyla yüklendiğinde bilgilendirici bir açılır ileti gösterir.

STDOUT günlük kaydı etkin değilse, aşağıdaki adımları izleyin:

1. Azure portalında **Gelişmiş Araçlar** dikey penceresinde **geliştirme araçları** alan. Seçin **Git&rarr;**  düğmesi. Kudu konsolunu bir yeni tarayıcı sekmesinde veya penceresinde açılır.
1. Sayfanın en üstünde gezinti çubuğunu kullanarak açmak **hata ayıklama konsoluna** seçip **CMD**.
1. Klasör yolu açın **site** > **wwwroot** açığa çıkar aşağı kaydırarak *web.config* listenin altındaki dosya.
1. Kalem simgesine tıklayın *web.config* dosya.
1. Ayarlama **stdoutLogEnabled** için `true` değiştirip **stdoutLogFile** yolu: `\\?\%home%\LogFiles\stdout`.
1. Seçin **Kaydet** güncelleştirilmiş kaydetmek için *web.config* dosya.

Tanılama günlüğüne kaydetmeyi etkinleştirmek için devam edin:

1. Azure portalında **tanılama günlükleri** dikey penceresi.
1. Seçin **üzerinde** için geçiş **uygulama günlüğü (dosya sistemi)** ve **ayrıntılı hata iletileri**. Seçin **Kaydet** dikey penceresinin üstünde düğme.
1. Başarısız istek olay arabelleğe alma (FREB) günlük olarak da bilinir, başarısız istek izleme eklemek için işaretleyin **üzerinde** için geçiş **başarısız istek izleme**. 
1. Seçin **günlük akışı** hemen altında listelenen dikey penceresinde **tanılama günlükleri** portaldaki dikey pencere.
1. Uygulamaya bir istek oluşturun.
1. Hatanın nedenini günlük akış verilerini içinde belirtilir.

Sorun giderme işlemi tamamlandıktan sonra stdout günlüğünü devre dışı emin olun. Yönergelere bakın [ASP.NET Core modülü stdout günlük](#aspnet-core-module-stdout-log) bölümü.

Başarısız istek izleme günlükleri (FREB günlükleri) görüntülemek için:

1. Gidin **Tanıla ve problemleri çözmenize** Azure portalındaki dikey penceresinde.
1. Seçin **başarısız istek günlükleri izleme** gelen **Destek Araçları** kenar alanı.

Bkz: [başarısız istek konuda Azure App Service web apps için etkinleştirme tanılama günlüğünü bölümünü izler](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) ve [azure'daki Web uygulamaları için uygulama performansı ile ilgili SSS: Başarısız istek izlemeyi nasıl kapatırım? ](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) daha fazla bilgi için.

Daha fazla bilgi için [Azure App Service'te web apps için tanılama günlüğünü etkinleştirme](/azure/app-service/web-sites-enable-diagnostic-log).

> [!WARNING]
> Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir. Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.
>
> ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın. Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Visual Studio kullanarak Azure App Service'te bir web uygulaması sorunlarını giderme](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* ["502 hatalı ağ geçidi" ve "503 Hizmet kullanılamıyor" Azure web uygulamalarınızda HTTP hatalarını giderme](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Azure App Service'te yavaş web uygulaması performans sorunlarını giderme](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Azure Web Apps için uygulama performansı ile ilgili SSS](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure Web uygulama korumalı alanı (App Service çalışma zamanı yürütme sınırlamaları)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [Azure Friday: Azure App Service tanılama ve sorun giderme deneyimini (12 dakikalık video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
