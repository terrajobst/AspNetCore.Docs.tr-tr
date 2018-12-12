---
title: İzleme ve hata ayıklama - ASP.NET Core ve Azure ile DevOps
author: CamSoper
description: İzleme ve ASP.NET Core ve Azure ile DevOps çözümün bir parçası kodunuzun hatalarını ayıklama
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/monitor
ms.openlocfilehash: 00489bd92dfff8fd80bd24c2e60193d32031d7c4
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284376"
---
# <a name="monitor-and-debug"></a><span data-ttu-id="e266d-103">İzleme ve hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="e266d-103">Monitor and debug</span></span>

<span data-ttu-id="e266d-104">Uygulamanın dağıtılan ve yerleşik olarak DevOps işlem hattı, izleme ve sorun giderme uygulama anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="e266d-104">Having deployed the app and built a DevOps pipeline, it's important to understand how to monitor and troubleshoot the app.</span></span>

<span data-ttu-id="e266d-105">Bu bölümde, aşağıdaki görevleri tamamlamanız:</span><span class="sxs-lookup"><span data-stu-id="e266d-105">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="e266d-106">Temel izleme ve sorun giderme Azure portalında veri Bul</span><span class="sxs-lookup"><span data-stu-id="e266d-106">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="e266d-107">Azure İzleyici ölçümleri daha ayrıntılı göz tüm Azure Hizmetleri genelinde nasıl sağladığını öğrenin</span><span class="sxs-lookup"><span data-stu-id="e266d-107">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="e266d-108">Uygulama profil oluşturma için Application Insights ile web uygulamasına bağlama</span><span class="sxs-lookup"><span data-stu-id="e266d-108">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="e266d-109">Günlük özelliğini açar ve günlükleri indirmek öğrenin</span><span class="sxs-lookup"><span data-stu-id="e266d-109">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="e266d-110">Stream gerçek zamanlı olarak günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="e266d-110">Stream logs in real time</span></span>
* <span data-ttu-id="e266d-111">Uyarıları ayarlamak öğrenin</span><span class="sxs-lookup"><span data-stu-id="e266d-111">Learn where to set up alerts</span></span>
* <span data-ttu-id="e266d-112">Uzaktan hata ayıklama Azure App Service web apps hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="e266d-112">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="basic-monitoring-and-troubleshooting"></a><span data-ttu-id="e266d-113">Temel izleme ve sorun giderme</span><span class="sxs-lookup"><span data-stu-id="e266d-113">Basic monitoring and troubleshooting</span></span>

<span data-ttu-id="e266d-114">App Service web uygulamalarını kolayca gerçek zamanlı olarak izlenir.</span><span class="sxs-lookup"><span data-stu-id="e266d-114">App Service web apps are easily monitored in real time.</span></span> <span data-ttu-id="e266d-115">Azure portal, ölçümleri anlaşılması kolay grafikler ve graflar, işler.</span><span class="sxs-lookup"><span data-stu-id="e266d-115">The Azure portal renders metrics in easy-to-understand charts and graphs.</span></span>

1. <span data-ttu-id="e266d-116">Açık [Azure portalında](https://portal.azure.com)ve ardından gidin *mywebapp şeklindedir\<unique_number\>*  App Service.</span><span class="sxs-lookup"><span data-stu-id="e266d-116">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>

1. <span data-ttu-id="e266d-117">**Genel bakış** sekmesi son ölçümleri gösteren grafikler de dahil olmak üzere faydalı "bir bakışta" bilgiler görüntüler.</span><span class="sxs-lookup"><span data-stu-id="e266d-117">The **Overview** tab displays useful "at-a-glance" information, including graphs displaying recent metrics.</span></span>

    ![Ekran gösteren genel bakış paneli](./media/monitoring/overview.png)

    * <span data-ttu-id="e266d-119">**HTTP 5xx**: Sunucu tarafı hataları, genellikle ASP.NET Core kod özel durum sayısı.</span><span class="sxs-lookup"><span data-stu-id="e266d-119">**Http 5xx**: Count of server-side errors, usually exceptions in ASP.NET Core code.</span></span>
    * <span data-ttu-id="e266d-120">**Verileri**: Web uygulamanıza gelen veri girişi.</span><span class="sxs-lookup"><span data-stu-id="e266d-120">**Data In**: Data ingress coming into your web app.</span></span>
    * <span data-ttu-id="e266d-121">**Veri çıkışı**: İstemcilere web uygulamanızdan veri çıkışı.</span><span class="sxs-lookup"><span data-stu-id="e266d-121">**Data Out**: Data egress from your web app to clients.</span></span>
    * <span data-ttu-id="e266d-122">**İstekleri**: HTTP isteği sayısı.</span><span class="sxs-lookup"><span data-stu-id="e266d-122">**Requests**: Count of HTTP requests.</span></span>
    * <span data-ttu-id="e266d-123">**Ortalama yanıt süresi**: HTTP isteklerine yanıt vermek web uygulaması için ortalama süre.</span><span class="sxs-lookup"><span data-stu-id="e266d-123">**Average Response Time**: Average time for the web app to respond to HTTP requests.</span></span>

    <span data-ttu-id="e266d-124">Sorun giderme ve en iyi duruma getirme için Self Servis çeşitli araçlar Ayrıca bu sayfada bulunur.</span><span class="sxs-lookup"><span data-stu-id="e266d-124">Several self-service tools for troubleshooting and optimization are also found on this page.</span></span>

    ![Self Servis gösteren araçları ekran](./media/monitoring/wizards.png)

    * <span data-ttu-id="e266d-126">**Sorunları tanılama ve çözme** olan bir Self Servis sorun giderici.</span><span class="sxs-lookup"><span data-stu-id="e266d-126">**Diagnose and solve problems** is a self-service troubleshooter.</span></span>
    * <span data-ttu-id="e266d-127">**Application Insights** performans ve uygulama davranışını profil oluşturma için olan ve daha sonra bu bölümde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="e266d-127">**Application Insights** is for profiling performance and app behavior, and is discussed later in this section.</span></span>
    * <span data-ttu-id="e266d-128">**App Service Danışmanı** uygulama deneyiminizi ayarlamak için öneriler sağlar.</span><span class="sxs-lookup"><span data-stu-id="e266d-128">**App Service Advisor** makes recommendations to tune your app experience.</span></span>

## <a name="advanced-monitoring"></a><span data-ttu-id="e266d-129">Gelişmiş izleme</span><span class="sxs-lookup"><span data-stu-id="e266d-129">Advanced monitoring</span></span>

<span data-ttu-id="e266d-130">[Azure İzleyici](/azure/monitoring-and-diagnostics/) tüm ölçümleri izleme ve Azure Hizmetleri genelinde uyarılar ayarlanması için merkezi hizmetidir.</span><span class="sxs-lookup"><span data-stu-id="e266d-130">[Azure Monitor](/azure/monitoring-and-diagnostics/) is the centralized service for monitoring all metrics and setting alerts across Azure services.</span></span> <span data-ttu-id="e266d-131">Azure İzleyici'içinde yöneticileri hedefle performansını izleyebilir ve eğilimleri belirleyin.</span><span class="sxs-lookup"><span data-stu-id="e266d-131">Within Azure Monitor, administrators can granularly track performance and identify trends.</span></span> <span data-ttu-id="e266d-132">Her bir Azure hizmeti, kendi sunar [ölçüm kümesini](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) Azure İzleyicisi.</span><span class="sxs-lookup"><span data-stu-id="e266d-132">Each Azure service offers its own [set of metrics](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) to Azure Monitor.</span></span>

## <a name="profile-with-application-insights"></a><span data-ttu-id="e266d-133">Application Insights ile profili</span><span class="sxs-lookup"><span data-stu-id="e266d-133">Profile with Application Insights</span></span>

<span data-ttu-id="e266d-134">[Application Insights](/azure/application-insights/app-insights-overview) web uygulamaları ve kullanıcıların bunları nasıl kullanılacağını kararlılığı ve performansı analiz etmek için bir Azure hizmetidir.</span><span class="sxs-lookup"><span data-stu-id="e266d-134">[Application Insights](/azure/application-insights/app-insights-overview) is an Azure service for analyzing the performance and stability of web apps and how users use them.</span></span> <span data-ttu-id="e266d-135">Application Insights verilerini, daha geniş ve Azure İzleyici'den daha derin.</span><span class="sxs-lookup"><span data-stu-id="e266d-135">The data from Application Insights is broader and deeper than that of Azure Monitor.</span></span> <span data-ttu-id="e266d-136">Veriler, geliştiricilerin ve yöneticilerin uygulamaları geliştirmek için anahtar bilgilerini sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="e266d-136">The data can provide developers and administrators with key information for improving apps.</span></span> <span data-ttu-id="e266d-137">Application ınsights'ı bir Azure App Service kaynak kod değişikliği yapmadan eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="e266d-137">Application Insights can be added to an Azure App Service resource without code changes.</span></span>

1. <span data-ttu-id="e266d-138">Açık [Azure portalında](https://portal.azure.com)ve ardından gidin *mywebapp şeklindedir\<unique_number\>*  App Service.</span><span class="sxs-lookup"><span data-stu-id="e266d-138">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="e266d-139">Gelen **genel bakış** sekmesinde **Application Insights** Döşe.</span><span class="sxs-lookup"><span data-stu-id="e266d-139">From the **Overview** tab, click the **Application Insights** tile.</span></span>

    ![Application Insights kutucuğunu](./media/monitoring/app-insights.png)

1. <span data-ttu-id="e266d-141">Seçin **yeni kaynak Oluştur** radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="e266d-141">Select the **Create new resource** radio button.</span></span> <span data-ttu-id="e266d-142">Varsayılan kaynak adı kullanın ve Application Insights kaynağı için konum seçin.</span><span class="sxs-lookup"><span data-stu-id="e266d-142">Use the default resource name, and select the location for the Application Insights resource.</span></span> <span data-ttu-id="e266d-143">Konum, web uygulamanızın eşleşmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="e266d-143">The location doesn't need to match that of your web app.</span></span>

    ![Application Insights Kurulumu](./media/monitoring/new-app-insights.png)

1. <span data-ttu-id="e266d-145">İçin **çalışma zamanı/Framework**seçin **ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e266d-145">For **Runtime/Framework**, select **ASP.NET Core**.</span></span> <span data-ttu-id="e266d-146">Varsayılan ayarları kabul edin.</span><span class="sxs-lookup"><span data-stu-id="e266d-146">Accept the default settings.</span></span>
1. <span data-ttu-id="e266d-147">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="e266d-147">Select **OK**.</span></span> <span data-ttu-id="e266d-148">Onaylamanız istendiğinde belirleyin **devam**.</span><span class="sxs-lookup"><span data-stu-id="e266d-148">If prompted to confirm, select **Continue**.</span></span>
1. <span data-ttu-id="e266d-149">Kaynak oluşturulduktan sonra Application Insights sayfasına doğrudan gitmek için Application Insights kaynağı adına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e266d-149">After the resource has been created, click the name of Application Insights resource to navigate directly to the Application Insights page.</span></span>

    ![Yeni Application Insights kaynağı hazır](./media/monitoring/new-app-insights-done.png)

<span data-ttu-id="e266d-151">Uygulama kullanıldıkça, verileri toplanır.</span><span class="sxs-lookup"><span data-stu-id="e266d-151">As the app is used, data accumulates.</span></span> <span data-ttu-id="e266d-152">Seçin **Yenile** dikey penceresinde yeni verilerle yeniden yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="e266d-152">Select **Refresh** to reload the blade with new data.</span></span>

![Application Insights genel bakış sekmesi](./media/monitoring/app-insights-overview.png)

<span data-ttu-id="e266d-154">Application ınsights'ı hiçbir ek yapılandırma yararlı sunucu tarafı bilgiler sağlar.</span><span class="sxs-lookup"><span data-stu-id="e266d-154">Application Insights provides useful server-side information with no additional configuration.</span></span> <span data-ttu-id="e266d-155">Uygulama anlayışları'ndan en fazla değeri elde etmek [uygulamanızı Application Insights SDK'sı ile izleme](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="e266d-155">To get the most value from Application Insights, [instrument your app with the Application Insights SDK](/azure/application-insights/app-insights-asp-net-core).</span></span> <span data-ttu-id="e266d-156">Düzgün bir şekilde yapılandırıldığında, istemci tarafında performans dahil olmak üzere, tarayıcı ve web sunucusu arasında uçtan uca izleme hizmetidir.</span><span class="sxs-lookup"><span data-stu-id="e266d-156">When properly configured, the service provides end-to-end monitoring across the web server and browser, including client-side performance.</span></span> <span data-ttu-id="e266d-157">Daha fazla bilgi için [Application Insights belgeleri](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="e266d-157">For more information, see the [Application Insights documentation](/azure/application-insights/app-insights-overview).</span></span>

## <a name="logging"></a><span data-ttu-id="e266d-158">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="e266d-158">Logging</span></span>

<span data-ttu-id="e266d-159">Web sunucusu ve uygulama günlüklerini Azure App Service'te varsayılan olarak devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="e266d-159">Web server and app logs are disabled by default in Azure App Service.</span></span> <span data-ttu-id="e266d-160">Aşağıdaki adımlarla günlüklerini etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="e266d-160">Enable the logs with the following steps:</span></span>

1. <span data-ttu-id="e266d-161">Açık [Azure portalında](https://portal.azure.com)gidin *mywebapp şeklindedir\<unique_number\>*  App Service.</span><span class="sxs-lookup"><span data-stu-id="e266d-161">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="e266d-162">Sol menüde, aşağı kaydırarak **izleme** bölümü.</span><span class="sxs-lookup"><span data-stu-id="e266d-162">In the menu to the left, scroll down to the **Monitoring** section.</span></span> <span data-ttu-id="e266d-163">Seçin **tanılama günlükleri**.</span><span class="sxs-lookup"><span data-stu-id="e266d-163">Select **Diagnostics logs**.</span></span>

    ![Tanılama günlükleri bağlantı](./media/monitoring/logging.png)

1. <span data-ttu-id="e266d-165">Açma **uygulama günlüğü (dosya sistemi)**.</span><span class="sxs-lookup"><span data-stu-id="e266d-165">Turn on **Application Logging (Filesystem)**.</span></span> <span data-ttu-id="e266d-166">İstenirse, web uygulamasında oturum uygulamasını etkinleştirmek için uzantıları yüklemek için kutusuna tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e266d-166">If prompted, click the box to install the extensions to enable app logging in the web app.</span></span>
1. <span data-ttu-id="e266d-167">Ayarlama **Web sunucusu günlüğü** için **dosya sistemi**.</span><span class="sxs-lookup"><span data-stu-id="e266d-167">Set **Web server logging** to **File System**.</span></span>
1. <span data-ttu-id="e266d-168">Girin **saklama süresi** gün.</span><span class="sxs-lookup"><span data-stu-id="e266d-168">Enter the **Retention Period** in days.</span></span> <span data-ttu-id="e266d-169">Örneğin, 30.</span><span class="sxs-lookup"><span data-stu-id="e266d-169">For example, 30.</span></span>
1. <span data-ttu-id="e266d-170">**Kaydet**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e266d-170">Click **Save**.</span></span>

<span data-ttu-id="e266d-171">Web uygulaması için ASP.NET Core ve web sunucusu (App Service) günlükleri üretilir.</span><span class="sxs-lookup"><span data-stu-id="e266d-171">ASP.NET Core and web server (App Service) logs are generated for the web app.</span></span> <span data-ttu-id="e266d-172">Görüntülenen bilgileri FTP/FTPS kullanarak yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="e266d-172">They can be downloaded using the FTP/FTPS information displayed.</span></span> <span data-ttu-id="e266d-173">Parola, bu kılavuzda daha önce oluşturduğunuz dağıtım kimlik bilgileri ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="e266d-173">The password is the same as the deployment credentials created earlier in this guide.</span></span> <span data-ttu-id="e266d-174">Günlükleri olabilir [PowerShell veya Azure CLI ile yerel makinenize doğrudan akışla](/azure/app-service/web-sites-enable-diagnostic-log#download).</span><span class="sxs-lookup"><span data-stu-id="e266d-174">The logs can be [streamed directly to your local machine with PowerShell or Azure CLI](/azure/app-service/web-sites-enable-diagnostic-log#download).</span></span> <span data-ttu-id="e266d-175">Günlükleri de olabilir [uygulama anlayışları'nda görüntülenen](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).</span><span class="sxs-lookup"><span data-stu-id="e266d-175">Logs can also be [viewed in Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).</span></span>

## <a name="log-streaming"></a><span data-ttu-id="e266d-176">Günlük akışı</span><span class="sxs-lookup"><span data-stu-id="e266d-176">Log streaming</span></span>

<span data-ttu-id="e266d-177">Uygulama ve web sunucusu günlükleri, portal üzerinden gerçek zamanlı aktarılabilir.</span><span class="sxs-lookup"><span data-stu-id="e266d-177">App and web server logs can be streamed in real time through the portal.</span></span>

1. <span data-ttu-id="e266d-178">Açık [Azure portalında](https://portal.azure.com)gidin *mywebapp şeklindedir\<unique_number\>*  App Service.</span><span class="sxs-lookup"><span data-stu-id="e266d-178">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="e266d-179">Sol menüde, aşağı kaydırarak **izleme** seçin ve bölüm **günlük akışı**.</span><span class="sxs-lookup"><span data-stu-id="e266d-179">In the menu to the left, scroll down to the **Monitoring** section and select **Log stream**.</span></span>

    ![Ekran gösteren günlük akış bağlantısı](./media/monitoring/log-stream.png)

<span data-ttu-id="e266d-181">Günlükleri de olabilir [Azure CLI veya Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs)de dahil olmak üzere Cloud Shell aracılığıyla.</span><span class="sxs-lookup"><span data-stu-id="e266d-181">Logs can also be [streamed via Azure CLI or Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), including through the Cloud Shell.</span></span>

## <a name="alerts"></a><span data-ttu-id="e266d-182">Uyarılar</span><span class="sxs-lookup"><span data-stu-id="e266d-182">Alerts</span></span>

<span data-ttu-id="e266d-183">Azure İzleyici ayrıca sağlar [gerçek zamanlı uyarılar](/azure/monitoring-and-diagnostics/insights-alerts-portal) ölçümleri, yönetim olayları ve diğer ölçütlere göre.</span><span class="sxs-lookup"><span data-stu-id="e266d-183">Azure Monitor also provides [real time alerts](/azure/monitoring-and-diagnostics/insights-alerts-portal) based on metrics, administrative events, and other criteria.</span></span>

> <span data-ttu-id="e266d-184">*Not: Şu anda üzerinde web uygulaması ölçümleri uyarı yalnızca uyarılar (Klasik) hizmeti kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="e266d-184">*Note: Currently alerting on web app metrics is only available in the Alerts (classic) service.*</span></span>

<span data-ttu-id="e266d-185">[Uyarılar (Klasik) hizmeti](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) Azure İzleyicisi'nde veya altında bulunabilir **izleme** App Service ayarları bölümü.</span><span class="sxs-lookup"><span data-stu-id="e266d-185">The [Alerts (classic) service](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) can be found in Azure Monitor or under the **Monitoring** section of the App Service settings.</span></span>

![Uyarılar (Klasik) bağlantısı](./media/monitoring/alerts.png)

## <a name="live-debugging"></a><span data-ttu-id="e266d-187">Canlı hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="e266d-187">Live debugging</span></span>

<span data-ttu-id="e266d-188">Azure App Service olabilir [ile Visual Studio uzaktan hata ayıklama](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) zaman günlükleri sağlamadığınızdan yeterli bilgi.</span><span class="sxs-lookup"><span data-stu-id="e266d-188">Azure App Service can be [debugged remotely with Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) when logs don't provide enough information.</span></span> <span data-ttu-id="e266d-189">Ancak, uzaktan hata ayıklama, hata ayıklama sembolleriyle derlenmiş uygulamanın gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e266d-189">However, remote debugging requires the app to be compiled with debug symbols.</span></span> <span data-ttu-id="e266d-190">Hata ayıklama üretimde dışında son çare olarak yapılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e266d-190">Debugging shouldn't be done in production, except as a last resort.</span></span>

## <a name="conclusion"></a><span data-ttu-id="e266d-191">Sonuç</span><span class="sxs-lookup"><span data-stu-id="e266d-191">Conclusion</span></span>

<span data-ttu-id="e266d-192">Bu bölümde, aşağıdaki görevleri tamamlandı:</span><span class="sxs-lookup"><span data-stu-id="e266d-192">In this section, you completed the following tasks:</span></span>

* <span data-ttu-id="e266d-193">Temel izleme ve sorun giderme Azure portalında veri Bul</span><span class="sxs-lookup"><span data-stu-id="e266d-193">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="e266d-194">Azure İzleyici ölçümleri daha ayrıntılı göz tüm Azure Hizmetleri genelinde nasıl sağladığını öğrenin</span><span class="sxs-lookup"><span data-stu-id="e266d-194">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="e266d-195">Uygulama profil oluşturma için Application Insights ile web uygulamasına bağlama</span><span class="sxs-lookup"><span data-stu-id="e266d-195">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="e266d-196">Günlük özelliğini açar ve günlükleri indirmek öğrenin</span><span class="sxs-lookup"><span data-stu-id="e266d-196">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="e266d-197">Stream gerçek zamanlı olarak günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="e266d-197">Stream logs in real time</span></span>
* <span data-ttu-id="e266d-198">Uyarıları ayarlamak öğrenin</span><span class="sxs-lookup"><span data-stu-id="e266d-198">Learn where to set up alerts</span></span>
* <span data-ttu-id="e266d-199">Uzaktan hata ayıklama Azure App Service web apps hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="e266d-199">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="e266d-200">Ek okuma</span><span class="sxs-lookup"><span data-stu-id="e266d-200">Additional reading</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="e266d-201">Application Insights ile Azure web uygulaması performansını izleme</span><span class="sxs-lookup"><span data-stu-id="e266d-201">Monitor Azure web app performance with Application Insights</span></span>](/azure/application-insights/app-insights-azure-web-apps)
* [<span data-ttu-id="e266d-202">Azure App Service'te web apps için tanılama günlüğünü etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="e266d-202">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)
* [<span data-ttu-id="e266d-203">Visual Studio kullanarak Azure App Service'te bir web uygulaması sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="e266d-203">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="e266d-204">Klasik ölçüm uyarıları, Azure İzleyici'de Azure Hizmetleri için - oluşturun. Azure portalı</span><span class="sxs-lookup"><span data-stu-id="e266d-204">Create classic metric alerts in Azure Monitor for Azure services - Azure portal</span></span>](/azure/monitoring-and-diagnostics/insights-alerts-portal)
