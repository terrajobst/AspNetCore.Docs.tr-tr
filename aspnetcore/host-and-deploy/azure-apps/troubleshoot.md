---
title: Azure uygulama Hizmeti'nde ASP.NET Core sorun giderme
author: guardrex
description: ASP.NET Core Azure uygulama hizmeti dağıtım sorunlarını tanılamaya öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2018
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: f9918e9162329f4c5dbd1ff18e30fce0db24e651
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272730"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Azure uygulama Hizmeti'nde ASP.NET Core sorun giderme

Tarafından [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

Bu makalede, Azure App Service'nın tanılama araçlarını kullanarak uygulama başlatma sorunu ASP.NET Core Tanılama hakkında yönergeler sağlar. Ek sorun giderme öneriler için bkz: [Azure App Service tanılama genel bakış](/azure/app-service/app-service-diagnostics) ve [nasıl yapılır: Azure uygulama hizmetinde uygulamaları izleme](/azure/app-service/web-sites-monitor) Azure belgelerinde.

## <a name="app-startup-errors"></a>Uygulama başlatma hataları

**502.5 işlem hatası**  
Çalışan işlemi başarısız olur. Uygulama başlamıyor.

[ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) , ancak çalışan işlemi başlatmak için denemeleri başarısız başlatmak. Genellikle uygulama olay günlüğünü inceleyerek, bu tür bir sorun gidermenize yardımcı olur. Günlük erişimi içinde açıklanan [uygulama olay günlüğü](#application-event-log) bölümü.

*502.5 işlem hatası* yanlış yapılandırılmış bir uygulama çalışan işleminin başarısız olmasına neden olduğunda hata sayfası döndürdü:

![502.5 işlem hatası sayfasını gösteren bir tarayıcı penceresi](troubleshoot/_static/process-failure-page.png)

**500 İç sunucu hatası**  
Uygulamayı başlatır, ancak bir hata, isteği yerine getirmesini sunucu engeller.

Bu hata uygulamanın kodunu içinde başlatma sırasında veya bir yanıt oluşturma sırasında oluşur. Yanıtın içerik içerebilir veya yanıt olarak görünebilir bir *500 İç sunucu hatası* tarayıcıda. Uygulama olay günlüğüne genellikle uygulamanın normal olarak başlatıldığını belirtir. Sunucunun açısından bakıldığında, doğru olmasıdır. Uygulamayı başlatmak, ancak geçerli bir yanıt oluşturulamıyor. [Uygulama Kudu konsolunda çalıştırma](#run-the-app-in-the-kudu-console) veya [ASP.NET Core modül stdout günlüğünü etkinleştirir](#aspnet-core-module-stdout-log) sorunu gidermek için.

**Bağlantı sıfırlama**

Üstbilgileri gönderildikten sonra bir hata oluşursa, sunucunun göndermek için çok geç bir **500 İç sunucu hatası** bir hata oluştuğunda. Bu durum, genellikle yanıt karmaşık nesne seri hale getirme sırasında bir hata ortaya çıktığında oluşur. Bu tür hatalara görünür bir *bağlantı sıfırlama* istemci hatası. [Uygulama günlüğü](xref:fundamentals/logging/index) bu tür hataların gidermenize yardımcı olacak.

## <a name="default-startup-limits"></a>Varsayılan başlangıç sınırları

ASP.NET çekirdeği modülü ile bir varsayılan yapılandırılmış *startupTimeLimit* 120 saniye. Varsayılan değer olarak bırakılırsa, bir uygulama modülü bir işlem hatası oturum önce başlatmak için iki dakika sürebilir. Modül yapılandırma hakkında daha fazla bilgi için bkz: [aspNetCore öğesinin özniteliklerini](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Uygulama başlangıç hatalarında sorun giderme

### <a name="application-event-log"></a>Uygulama olay günlüğü

Uygulama olay günlüğüne erişmek için **Tanıla ve sorunları** Azure portalında dikey penceresinde:

1. Azure Portal'da uygulamanızın dikey penceresinde açın **uygulama hizmetleri** dikey.
1. Seçin **Tanıla ve sorunları** dikey.
1. Altında **sorun KATEGORİSİ seçin**seçin **Web uygulaması aşağı** düğmesi.
1. Altında **önerilen çözümler**, bölmesini açmak **uygulama olay günlüklerini açmak**. Seçin **açık uygulama olay günlüklerini** düğmesi.
1. Tarafından sağlanan en son hatasını inceleyin *IIS AspNetCoreModule* içinde **kaynak** sütun.

Kullanmaya alternatif **Tanıla ve sorunları** dikey penceresini kullanarak doğrudan uygulama olay günlüğü dosyasını inceleyin etmektir [Kudu](https://github.com/projectkudu/kudu/wiki):

1. Seçin **Gelişmiş Araçlar** dikey penceresinde **geliştirme araçları** alanı. Seçin **Git&rarr;**  düğmesi. Kudu konsol, yeni bir tarayıcı sekmesinde veya penceresinde açılır.
1. Sayfanın en üstünde gezinti çubuğunu kullanarak açmak **hata ayıklama konsoluna** seçip **CMD**.
1. Açık **LogFiles** klasör.
1. Kalem simgesini seçin *eventlog.xml* dosya.
1. Günlüğünü inceleyin. En son olayların görmek için günlüğü altına gidin.

### <a name="run-the-app-in-the-kudu-console"></a>Uygulama Kudu konsolunda çalıştırma

Birçok başlatma hataları yararlı bilgiler uygulama olay günlüğü'ndeki üretmediği. Uygulamayı çalıştırabilirsiniz [Kudu](https://github.com/projectkudu/kudu/wiki) hata bulmak için uzaktan yürütme konsolu:

1. Seçin **Gelişmiş Araçlar** dikey penceresinde **geliştirme araçları** alanı. Seçin **Git&rarr;**  düğmesi. Kudu konsol, yeni bir tarayıcı sekmesinde veya penceresinde açılır.
1. Sayfanın en üstünde gezinti çubuğunu kullanarak açmak **hata ayıklama konsoluna** seçip **CMD**.
1. Yolun klasörlere açmak **site** > **wwwroot**.
1. Konsolda, uygulamanın derleme yürüterek uygulamayı çalıştırın.
   * Uygulama ise bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd), uygulamanın derleme çalıştırmak *dotnet.exe*. Aşağıdaki komutta, uygulamanın derleme adı yerine `<assembly_name>`: `dotnet .\<assembly_name>.dll`
   * Uygulama ise bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd)çalıştırın uygulama yürütülebilir. Aşağıdaki komutta, uygulamanın derleme adı yerine `<assembly_name>`: `<assembly_name>.exe`
1. Herhangi bir hata gösteren uygulamadan çıktı konsol Kudu konsola varsayılır.

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET Core modül stdout günlük

ASP.NET Core modül stdout günlük genellikle uygulama olay günlüğüne bulunamadı yararlı hata iletileri kaydeder. Etkinleştirme ve stdout günlükleri görüntülemek için:

1. Gidin **Tanıla ve sorunları** Azure portaldaki dikey pencere.
1. Altında **sorun KATEGORİSİ seçin**seçin **Web uygulaması aşağı** düğmesi.
1. Altında **önerilen çözümler** > **Stdout günlük yeniden yönlendirmeyi etkinleştirmek**, düğme seçin **Web.Config düzenlemek için açık Kudu Konsolu**.
1. Kudu, **tanılama Konsolu**, yolun klasörlere açmak **site** > **wwwroot**. Kaydırarak aşağı kaydırın açığa *web.config* listesinin altındaki dosya.
1. Kalem simgesine tıklayın *web.config* dosya.
1. Ayarlama **stdoutLogEnabled** için `true` değiştirip **stdoutLogFile** yoluna: `\\?\%home%\LogFiles\stdout`.
1. Seçin **kaydetmek** güncelleştirilmiş kaydetmek için *web.config* dosya.
1. Bir istek uygulamaya olun.
1. Azure portalına geri dönün. Seçin **Gelişmiş Araçlar** dikey penceresinde **geliştirme araçları** alanı. Seçin **Git&rarr;**  düğmesi. Kudu konsol, yeni bir tarayıcı sekmesinde veya penceresinde açılır.
1. Sayfanın en üstünde gezinti çubuğunu kullanarak açmak **hata ayıklama konsoluna** seçip **CMD**.
1. Seçin **LogFiles** klasör.
1. İnceleme **değiştirilen** en son değiştirilme tarihini ile oturum sütun ve stdout düzenlemek için Kurşun Kalem simgesini seçin.
1. Günlük dosyası açıldığında hata görüntülenir.

**Önemli!** Sorun giderme tamamlandığında stdout günlüğü devre dışı bırakın.

1. Kudu, **tanılama Konsolu**, yolun dönün **site** > **wwwroot** ortaya çıkarmak üzere *web.config* dosya. Açık **web.config** Kurşun Kalem simgesini seçerek yeniden dosyası.
1. Ayarlama **stdoutLogEnabled** için `false`.
1. Seçin **kaydetmek** dosyayı kaydetmek için.

> [!WARNING]
> Stdout günlüğünü devre dışı bırakmak için uygulama veya sunucu başarısızlığı açabilir. Günlük dosyası boyutu bir sınırlama yoktur veya oluşturulan günlük dosyalarını sayısı yoktur. Yalnızca uygulama başlatma sorunlarını gidermek için günlüğü stdout kullanın.
>
> Genel bir ASP.NET Core uygulamada başlatma işleminden sonra için günlüğü, günlük dosyası boyutu sınırlar ve günlükleri döndüğü bir günlük kitaplığını kullanın. Daha fazla bilgi için bkz: [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="common-startup-errors"></a>Ortak başlatma hataları 

Bkz: [ASP.NET Core ortak hataları referans](xref:host-and-deploy/azure-iis-errors-reference). Uygulama başlangıç önlemek yaygın sorunların çoğunu başvuru konuda ele alınmıştır.

## <a name="slow-or-hanging-app"></a>Yavaş ya da asılı uygulama

Bir uygulama yavaş yanıt ya da bir istekte askıda bkz [sorun giderme yavaş web uygulaması performans sorunlarını Azure App Service'te](/azure/app-service/app-service-web-troubleshoot-performance-degradation) Kılavuzu hata ayıklama için.

## <a name="remote-debugging"></a>Uzaktan hata ayıklama

Aşağıdaki konulara bakın:

* [Uzaktan hata ayıklama Web uygulamaları sorunlarını giderme Visual Studio kullanarak Azure App Service web uygulamasında bölümünü](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (Azure belgelerine)
* [Visual Studio 2017 Azure'da IIS üzerinde uzaktan hata ayıklama ASP.NET Core](/visualstudio/debugger/remote-debugging-azure) (Visual Studio belgelerinde)

## <a name="application-insights"></a>Application Insights

[Application Insights](https://azure.microsoft.com/services/application-insights/) hata günlüğü ve raporlama özellikleri dahil olmak üzere Azure App Service'te barındırılan uygulamalar telemetrisini sağlar. Application Insights, yalnızca uygulamanın günlüğe kaydetme özelliklerini kullanılabilir olduğunda uygulama başladıktan sonra oluşan hataları bildirebilirsiniz. Daha fazla bilgi için bkz: [ASP.NET Core için Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="monitoring-blades"></a>İzleme dikey pencereleri

İzleme Kanatlar sorun giderme deneyimini konuda daha önce açıklanan yöntemlerden için bir alternatif sunar. Bu dikey, 500-serisi hataları tanılamak için kullanılabilir.

ASP.NET Core uzantıları yüklü olduğunu doğrulayın. Uzantıları yüklü değilse, bunları el ile yükleyin:

1. İçinde **geliştirme araçları** dikey bölümde, select **uzantıları** dikey.
1. **ASP.NET Core uzantıları** listede görünmelidir.
1. Uzantıları yüklü değilse, seçin **Ekle** düğmesi.
1. Seçin **ASP.NET Core uzantıları** listeden.
1. Seçin **Tamam** yasal koşulları kabul etmek için.
1. Seçin **Tamam** üzerinde **uzantısı Ekle** dikey.
1. Açılır ileti bilgilendirme zaman uzantıları yüklenene gösterir.

STDOUT günlük kaydı etkin değilse, aşağıdaki adımları izleyin:

1. Azure portalında seçin **Gelişmiş Araçlar** dikey penceresinde **geliştirme araçları** alanı. Seçin **Git&rarr;**  düğmesi. Kudu konsol, yeni bir tarayıcı sekmesinde veya penceresinde açılır.
1. Sayfanın en üstünde gezinti çubuğunu kullanarak açmak **hata ayıklama konsoluna** seçip **CMD**.
1. Yolun klasörlere açmak **site** > **wwwroot** ve kaydırma aşağıya doğru açığa *web.config* listesinin altındaki dosya.
1. Kalem simgesine tıklayın *web.config* dosya.
1. Ayarlama **stdoutLogEnabled** için `true` değiştirip **stdoutLogFile** yoluna: `\\?\%home%\LogFiles\stdout`.
1. Seçin **kaydetmek** güncelleştirilmiş kaydetmek için *web.config* dosya.

Tanılama günlük kaydını etkinleştirmek için devam edin:

1. Azure portalında seçin **tanılama günlükleri** dikey.
1. Seçin **üzerinde** için geçiş **uygulama günlüğü (dosya sistemi)** ve **ayrıntılı hata iletileri**. Seçin **kaydetmek** dikey pencerenin üstündeki düğmesi.
1. Başarısız istek izleme başarısız istek olay arabelleğini (FREB) günlük olarak da bilinen, dahil etmek istiyorsanız seçin **üzerinde** için geçiş **başarısız istek izleme**. 
1. Seçin **günlük akışı** hemen altında listelenen dikey **tanılama günlükleri** portaldaki dikey pencere.
1. Bir istek uygulamaya olun.
1. Günlük veri akışı içinde hatanın nedenini belirtilir.

**Önemli!** Sorun giderme tamamlandığında stdout günlüğü devre dışı bıraktığınızdan emin olun. Deki yönergelere bakın [ASP.NET Core modül stdout günlük](#aspnet-core-module-stdout-log) bölümü.

Başarısız istek izleme günlükleri (FREB günlükleri) görüntülemek için:

1. Gidin **Tanıla ve sorunları** Azure portaldaki dikey pencere.
1. Seçin **istek izleme günlüklerini başarısız** gelen **Destek Araçları** kenar alanı.

Bkz: [başarısız istek izler Azure App Service konudaki web uygulamaları için etkinleştir tanılama günlüğünü bölümünü](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) ve [Azure Web uygulamaları için uygulama performansı ile ilgili SSS: nasıl başarısız istek izlemeyi kapatırım?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) Daha fazla bilgi için.

Daha fazla bilgi için bkz: [Azure App Service'te web uygulamalarını için tanılama günlüğünü etkinleştirme](/azure/app-service/web-sites-enable-diagnostic-log).

> [!WARNING]
> Stdout günlüğünü devre dışı bırakmak için uygulama veya sunucu başarısızlığı açabilir. Günlük dosyası boyutu bir sınırlama yoktur veya oluşturulan günlük dosyalarını sayısı yoktur.
>
> Bir ASP.NET Core uygulamada rutin günlüğü için günlük dosyası boyutu sınırlar ve günlükleri döndüğü bir günlük kitaplığını kullanın. Daha fazla bilgi için bkz: [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="additional-resources"></a>Ek kaynaklar

* [Hata ASP.NET çekirdek işleme giriş](xref:fundamentals/error-handling)
* [Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu](xref:host-and-deploy/azure-iis-errors-reference)
* [Visual Studio kullanarak Azure App Service web uygulamasında sorun giderme](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* ["502 hatalı ağ geçidi" ve "503 Hizmet kullanılamıyor", Azure web uygulamalarında HTTP hatalarını giderme](/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Azure App Service'te yavaş web uygulaması performans sorunlarını giderme](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Azure Web uygulamaları için uygulama performansı ile ilgili SSS](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure Web uygulaması sandbox (uygulama hizmeti çalışma zamanı yürütme sınırlamaları)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [Azure Friday: Azure uygulama hizmeti tanılama ve sorun giderme deneyimini (12 dakikalık video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
