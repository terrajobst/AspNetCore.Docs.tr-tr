---
title: İzleme ve hata ayıklama - ASP.NET Core ve Azure ile DevOps
author: CamSoper
description: İzleme ve ASP.NET Core ve Azure ile DevOps çözümün bir parçası kodunuzun hatalarını ayıklama
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 07/10/2019
uid: azure/devops/monitor
ms.openlocfilehash: 1d8ed99f4387dbc99929164c558cc2ce14bd9ea0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659504"
---
# <a name="monitor-and-debug"></a>İzleme ve hata ayıklama

Uygulamanın dağıtılan ve yerleşik olarak DevOps işlem hattı, izleme ve sorun giderme uygulama anlamak önemlidir.

Bu bölümde, aşağıdaki görevleri tamamlamanız:

* Temel izleme ve sorun giderme Azure portalında veri Bul
* Azure İzleyici ölçümleri daha ayrıntılı göz tüm Azure Hizmetleri genelinde nasıl sağladığını öğrenin
* Uygulama profil oluşturma için Application Insights ile web uygulamasına bağlama
* Günlük özelliğini açar ve günlükleri indirmek öğrenin
* Stream gerçek zamanlı olarak günlüğe kaydeder.
* Uyarıları ayarlamak öğrenin
* Uzaktan hata ayıklama Azure App Service web apps hakkında bilgi edinin.

## <a name="basic-monitoring-and-troubleshooting"></a>Temel izleme ve sorun giderme

App Service web uygulamalarını kolayca gerçek zamanlı olarak izlenir. Azure portal, ölçümleri anlaşılması kolay grafikler ve graflar, işler.

1. [Azure Portal](https://portal.azure.com)açın ve sonra *mywebapp\<unique_number\>* App Service gidin.

1. **Genel bakış** sekmesi, son ölçümleri görüntüleyen grafikler dahil yararlı "bir bakışta" bilgileri görüntüler.

    ![Ekran gösteren genel bakış paneli](./media/monitoring/overview.png)

    * **Http 5xx**: sunucu tarafı hatalarının sayısı, genellikle ASP.NET Core kodundaki özel durumlar.
    * **Veri girişi**: Web uygulamanıza gelen veri girişi.
    * **Giden veri**: Web uygulamanızdan istemcilere giden veri çıkışı.
    * **İstekler**: http isteklerinin sayısı.
    * **Ortalama yanıt süresi**: Web uygulamasının http isteklerine yanıt vermesi için geçen ortalama süre.

    Sorun giderme ve en iyi duruma getirme için Self Servis çeşitli araçlar Ayrıca bu sayfada bulunur.

    ![Self Servis gösteren araçları ekran](./media/monitoring/wizards.png)

    * **Sorunları tanılama ve çözme** , bir self servis sorun gidericidir.
    * **Application Insights** , profil oluşturma performansı ve uygulama davranışına yöneliktir ve bu bölümün ilerleyen kısımlarında ele alınmıştır.
    * **App Service Danışmanı** , uygulama deneyiminizi ayarlama önerilerini sağlar.

## <a name="advanced-monitoring"></a>Gelişmiş izleme

[Azure izleyici](/azure/monitoring-and-diagnostics/) , tüm ölçümleri Izlemek ve Azure hizmetleri genelinde uyarı ayarlamak için merkezi bir hizmettir. Azure İzleyici'içinde yöneticileri hedefle performansını izleyebilir ve eğilimleri belirleyin. Her Azure hizmeti, Azure Izleyici 'ye kendi [ölçüm kümesini](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) sunar.

## <a name="profile-with-application-insights"></a>Application Insights ile profili

[Application Insights](/azure/application-insights/app-insights-overview) , Web uygulamalarının performansını ve kararlılığını ve kullanıcıların bunları nasıl kullandığını analiz eden bir Azure hizmetidir. Application Insights verilerini, daha geniş ve Azure İzleyici'den daha derin. Veriler, geliştiricilerin ve yöneticilerin uygulamaları geliştirmek için anahtar bilgilerini sağlayabilir. Application ınsights'ı bir Azure App Service kaynak kod değişikliği yapmadan eklenebilir.

1. [Azure Portal](https://portal.azure.com)açın ve sonra *mywebapp\<unique_number\>* App Service gidin.
1. **Genel bakış** sekmesinden **Application Insights** kutucuğuna tıklayın.

    ![Application Insights kutucuğunu](./media/monitoring/app-insights.png)

1. **Yeni kaynak oluştur** radyo düğmesini seçin. Varsayılan kaynak adı kullanın ve Application Insights kaynağı için konum seçin. Konum, web uygulamanızın eşleşmesi gerekmez.

    ![Application Insights Kurulumu](./media/monitoring/new-app-insights.png)

1. **Çalışma zamanı/çerçeve**için **ASP.NET Core**seçin. Varsayılan ayarları kabul edin.
1. **Tamam**’ı seçin. Onaylamanız istenirse **devam**' ı seçin.
1. Kaynak oluşturulduktan sonra Application Insights sayfasına doğrudan gitmek için Application Insights kaynağı adına tıklayın.

    ![Yeni Application Insights kaynağı hazır](./media/monitoring/new-app-insights-done.png)

Uygulama kullanıldıkça, verileri toplanır. Yeni verilerle dikey pencereyi yeniden yüklemek için **Yenile** ' yi seçin.

![Application Insights genel bakış sekmesi](./media/monitoring/app-insights-overview.png)

Application ınsights'ı hiçbir ek yapılandırma yararlı sunucu tarafı bilgiler sağlar. Application Insights en fazla değeri almak için [uygulamanızı APPLICATION INSIGHTS SDK ile işaretleyin](/azure/application-insights/app-insights-asp-net-core). Düzgün bir şekilde yapılandırıldığında, istemci tarafında performans dahil olmak üzere, tarayıcı ve web sunucusu arasında uçtan uca izleme hizmetidir. Daha fazla bilgi için [Application Insights belgelerine](/azure/application-insights/app-insights-overview)bakın.

## <a name="logging"></a>Günlüğe kaydetme

Web sunucusu ve uygulama günlüklerini Azure App Service'te varsayılan olarak devre dışı bırakılır. Aşağıdaki adımlarla günlüklerini etkinleştirin:

1. [Azure Portal](https://portal.azure.com)açın ve *mywebapp\<unique_number\>* App Service gidin.
1. Soldaki menüde, **izleme** bölümüne gidin. **Tanılama günlükleri**' ni seçin.

    ![Tanılama günlükleri bağlantı](./media/monitoring/logging.png)

1. **Uygulama günlüğünü açın (dosya sistemi)** . İstenirse, web uygulamasında oturum uygulamasını etkinleştirmek için uzantıları yüklemek için kutusuna tıklayın.
1. **Web sunucusu günlüğünü** **dosya sistemine**ayarlayın.
1. **Saklama süresini** gün olarak girin. Örneğin, 30.
1. **Kaydet** düğmesine tıklayın.

Web uygulaması için ASP.NET Core ve web sunucusu (App Service) günlükleri üretilir. Görüntülenen bilgileri FTP/FTPS kullanarak yüklenebilir. Parola, bu kılavuzda daha önce oluşturduğunuz dağıtım kimlik bilgileri ile aynıdır. Günlükler [doğrudan PowerShell veya Azure CLI ile yerel makinenize akışla](/azure/app-service/web-sites-enable-diagnostic-log#download)eklenebilir. Günlükler, Application Insights de [görüntülenebilir](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).

## <a name="log-streaming"></a>Günlük akışı

Uygulama ve web sunucusu günlükleri, portal üzerinden gerçek zamanlı aktarılabilir.

1. [Azure Portal](https://portal.azure.com)açın ve *mywebapp\<unique_number\>* App Service gidin.
1. Soldaki menüde, **izleme** bölümüne gidin ve **günlük akışı**' nı seçin.

    ![Ekran gösteren günlük akış bağlantısı](./media/monitoring/log-stream.png)

Günlükler, Cloud Shell dahil olmak üzere [Azure CLI veya Azure PowerShell aracılığıyla da akışla](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs)eklenebilir.

## <a name="alerts"></a>Uyarılar

Azure Izleyici Ayrıca ölçümler, yönetim olayları ve diğer ölçütlere göre [gerçek zamanlı uyarılar](/azure/monitoring-and-diagnostics/insights-alerts-portal) sağlar.

> *Note: Şu anda Web uygulaması ölçümlerinde uyarı verme yalnızca uyarılar (klasik) hizmetinde kullanılabilir.*

[Uyarılar (klasik) hizmeti](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) Azure izleyici 'de veya App Service ayarlarının **izleme** bölümünde bulunabilir.

![Uyarılar (Klasik) bağlantısı](./media/monitoring/alerts.png)

## <a name="live-debugging"></a>Canlı hata ayıklama

Günlüklerde yeterli bilgi sağlamadığında, [Visual Studio ile Azure App Service uzaktan hata ayıklanabilir](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) . Ancak, uzaktan hata ayıklama, hata ayıklama sembolleriyle derlenmiş uygulamanın gerektirir. Hata ayıklama üretimde dışında son çare olarak yapılması gerekir.

## <a name="conclusion"></a>Sonuç

Bu bölümde, aşağıdaki görevleri tamamlandı:

* Temel izleme ve sorun giderme Azure portalında veri Bul
* Azure İzleyici ölçümleri daha ayrıntılı göz tüm Azure Hizmetleri genelinde nasıl sağladığını öğrenin
* Uygulama profil oluşturma için Application Insights ile web uygulamasına bağlama
* Günlük özelliğini açar ve günlükleri indirmek öğrenin
* Stream gerçek zamanlı olarak günlüğe kaydeder.
* Uyarıları ayarlamak öğrenin
* Uzaktan hata ayıklama Azure App Service web apps hakkında bilgi edinin.

## <a name="additional-reading"></a>Ek okuma

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Application Insights ile Azure Web uygulaması performansını izleme](/azure/application-insights/app-insights-azure-web-apps)
* [Azure App Service Web Apps için tanılama günlüğünü etkinleştirme](/azure/app-service/web-sites-enable-diagnostic-log)
* [Visual Studio 'Yu kullanarak Azure App Service bir Web uygulamasının sorunlarını giderme](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Azure hizmetleri için Azure Izleyici 'de klasik ölçüm uyarıları oluşturma-Azure portal](/azure/monitoring-and-diagnostics/insights-alerts-portal)
