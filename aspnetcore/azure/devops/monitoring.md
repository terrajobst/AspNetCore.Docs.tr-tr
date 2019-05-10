---
title: İzleme ve hata ayıklama - ASP.NET Core ve Azure ile DevOps
author: CamSoper
description: İzleme ve ASP.NET Core ve Azure ile DevOps çözümün bir parçası kodunuzun hatalarını ayıklama
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/monitor
ms.openlocfilehash: 00489bd92dfff8fd80bd24c2e60193d32031d7c4
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64899381"
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

1. Açık [Azure portalında](https://portal.azure.com)ve ardından gidin *mywebapp şeklindedir\<unique_number\>*  App Service.

1. **Genel bakış** sekmesi son ölçümleri gösteren grafikler de dahil olmak üzere faydalı "bir bakışta" bilgiler görüntüler.

    ![Ekran gösteren genel bakış paneli](./media/monitoring/overview.png)

    * **HTTP 5xx**: Sunucu tarafı hataları, genellikle ASP.NET Core kod özel durum sayısı.
    * **Verileri**: Web uygulamanıza gelen veri girişi.
    * **Veri çıkışı**: İstemcilere web uygulamanızdan veri çıkışı.
    * **İstekleri**: HTTP isteği sayısı.
    * **Ortalama yanıt süresi**: HTTP isteklerine yanıt vermek web uygulaması için ortalama süre.

    Sorun giderme ve en iyi duruma getirme için Self Servis çeşitli araçlar Ayrıca bu sayfada bulunur.

    ![Self Servis gösteren araçları ekran](./media/monitoring/wizards.png)

    * **Sorunları tanılama ve çözme** olan bir Self Servis sorun giderici.
    * **Application Insights** performans ve uygulama davranışını profil oluşturma için olan ve daha sonra bu bölümde ele alınmıştır.
    * **App Service Danışmanı** uygulama deneyiminizi ayarlamak için öneriler sağlar.

## <a name="advanced-monitoring"></a>Gelişmiş izleme

[Azure İzleyici](/azure/monitoring-and-diagnostics/) tüm ölçümleri izleme ve Azure Hizmetleri genelinde uyarılar ayarlanması için merkezi hizmetidir. Azure İzleyici'içinde yöneticileri hedefle performansını izleyebilir ve eğilimleri belirleyin. Her bir Azure hizmeti, kendi sunar [ölçüm kümesini](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) Azure İzleyicisi.

## <a name="profile-with-application-insights"></a>Application Insights ile profili

[Application Insights](/azure/application-insights/app-insights-overview) web uygulamaları ve kullanıcıların bunları nasıl kullanılacağını kararlılığı ve performansı analiz etmek için bir Azure hizmetidir. Application Insights verilerini, daha geniş ve Azure İzleyici'den daha derin. Veriler, geliştiricilerin ve yöneticilerin uygulamaları geliştirmek için anahtar bilgilerini sağlayabilir. Application ınsights'ı bir Azure App Service kaynak kod değişikliği yapmadan eklenebilir.

1. Açık [Azure portalında](https://portal.azure.com)ve ardından gidin *mywebapp şeklindedir\<unique_number\>*  App Service.
1. Gelen **genel bakış** sekmesinde **Application Insights** Döşe.

    ![Application Insights kutucuğunu](./media/monitoring/app-insights.png)

1. Seçin **yeni kaynak Oluştur** radyo düğmesi. Varsayılan kaynak adı kullanın ve Application Insights kaynağı için konum seçin. Konum, web uygulamanızın eşleşmesi gerekmez.

    ![Application Insights Kurulumu](./media/monitoring/new-app-insights.png)

1. İçin **çalışma zamanı/Framework**seçin **ASP.NET Core**. Varsayılan ayarları kabul edin.
1. **Tamam**’ı seçin. Onaylamanız istendiğinde belirleyin **devam**.
1. Kaynak oluşturulduktan sonra Application Insights sayfasına doğrudan gitmek için Application Insights kaynağı adına tıklayın.

    ![Yeni Application Insights kaynağı hazır](./media/monitoring/new-app-insights-done.png)

Uygulama kullanıldıkça, verileri toplanır. Seçin **Yenile** dikey penceresinde yeni verilerle yeniden yüklemek için.

![Application Insights genel bakış sekmesi](./media/monitoring/app-insights-overview.png)

Application ınsights'ı hiçbir ek yapılandırma yararlı sunucu tarafı bilgiler sağlar. Uygulama anlayışları'ndan en fazla değeri elde etmek [uygulamanızı Application Insights SDK'sı ile izleme](/azure/application-insights/app-insights-asp-net-core). Düzgün bir şekilde yapılandırıldığında, istemci tarafında performans dahil olmak üzere, tarayıcı ve web sunucusu arasında uçtan uca izleme hizmetidir. Daha fazla bilgi için [Application Insights belgeleri](/azure/application-insights/app-insights-overview).

## <a name="logging"></a>Günlüğe Kaydetme

Web sunucusu ve uygulama günlüklerini Azure App Service'te varsayılan olarak devre dışı bırakılır. Aşağıdaki adımlarla günlüklerini etkinleştirin:

1. Açık [Azure portalında](https://portal.azure.com)gidin *mywebapp şeklindedir\<unique_number\>*  App Service.
1. Sol menüde, aşağı kaydırarak **izleme** bölümü. Seçin **tanılama günlükleri**.

    ![Tanılama günlükleri bağlantı](./media/monitoring/logging.png)

1. Açma **uygulama günlüğü (dosya sistemi)**. İstenirse, web uygulamasında oturum uygulamasını etkinleştirmek için uzantıları yüklemek için kutusuna tıklayın.
1. Ayarlama **Web sunucusu günlüğü** için **dosya sistemi**.
1. Girin **saklama süresi** gün. Örneğin, 30.
1. **Kaydet**'e tıklayın.

Web uygulaması için ASP.NET Core ve web sunucusu (App Service) günlükleri üretilir. Görüntülenen bilgileri FTP/FTPS kullanarak yüklenebilir. Parola, bu kılavuzda daha önce oluşturduğunuz dağıtım kimlik bilgileri ile aynıdır. Günlükleri olabilir [PowerShell veya Azure CLI ile yerel makinenize doğrudan akışla](/azure/app-service/web-sites-enable-diagnostic-log#download). Günlükleri de olabilir [uygulama anlayışları'nda görüntülenen](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).

## <a name="log-streaming"></a>Günlük akışı

Uygulama ve web sunucusu günlükleri, portal üzerinden gerçek zamanlı aktarılabilir.

1. Açık [Azure portalında](https://portal.azure.com)gidin *mywebapp şeklindedir\<unique_number\>*  App Service.
1. Sol menüde, aşağı kaydırarak **izleme** seçin ve bölüm **günlük akışı**.

    ![Ekran gösteren günlük akış bağlantısı](./media/monitoring/log-stream.png)

Günlükleri de olabilir [Azure CLI veya Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs)de dahil olmak üzere Cloud Shell aracılığıyla.

## <a name="alerts"></a>Uyarılar

Azure İzleyici ayrıca sağlar [gerçek zamanlı uyarılar](/azure/monitoring-and-diagnostics/insights-alerts-portal) ölçümleri, yönetim olayları ve diğer ölçütlere göre.

> *Not: Şu anda üzerinde web uygulaması ölçümleri uyarı yalnızca uyarılar (Klasik) hizmeti kullanılabilir.*

[Uyarılar (Klasik) hizmeti](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) Azure İzleyicisi'nde veya altında bulunabilir **izleme** App Service ayarları bölümü.

![Uyarılar (Klasik) bağlantısı](./media/monitoring/alerts.png)

## <a name="live-debugging"></a>Canlı hata ayıklama

Azure App Service olabilir [ile Visual Studio uzaktan hata ayıklama](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) zaman günlükleri sağlamadığınızdan yeterli bilgi. Ancak, uzaktan hata ayıklama, hata ayıklama sembolleriyle derlenmiş uygulamanın gerektirir. Hata ayıklama üretimde dışında son çare olarak yapılması gerekir.

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

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Application Insights ile Azure web uygulaması performansını izleme](/azure/application-insights/app-insights-azure-web-apps)
* [Azure App Service'te web apps için tanılama günlüğünü etkinleştirme](/azure/app-service/web-sites-enable-diagnostic-log)
* [Visual Studio kullanarak Azure App Service'te bir web uygulaması sorunlarını giderme](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Klasik ölçüm uyarıları, Azure İzleyici'de Azure Hizmetleri için - oluşturun. Azure portalı](/azure/monitoring-and-diagnostics/insights-alerts-portal)
