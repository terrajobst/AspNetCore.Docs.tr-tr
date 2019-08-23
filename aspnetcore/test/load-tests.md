---
title: Yük/stres testini ASP.NET Core
author: Jeremy-Meng
description: Uygulamalar ASP.NET Core yük testi ve stres testi için birkaç önemli araç ve yaklaşım hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: 7a9dfc1fedf747ab26daa573b61ed01c31709058
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975239"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="188f3-103">Yük/stres testini ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="188f3-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="188f3-104">Yük testi ve stres testi, Web uygulamasının performansı ve ölçeklenebilir olmasını sağlamak için önemlidir.</span><span class="sxs-lookup"><span data-stu-id="188f3-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="188f3-105">Genellikle benzer testleri paylaşsalar bile hedefleri farklıdır.</span><span class="sxs-lookup"><span data-stu-id="188f3-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="188f3-106">**Yük testleri** &ndash; Uygulamanın belirli bir kullanıcı yükünü, yanıt hedefini karşılarken, belirli bir senaryo için işleyebilip işleyemeyeceğini test edin.</span><span class="sxs-lookup"><span data-stu-id="188f3-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="188f3-107">Uygulama normal koşullarda çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="188f3-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="188f3-108">**Stres testleri** &ndash; Genellikle uzun bir süre boyunca çok fazla koşullarda çalışırken uygulamanın kararlılığını test edin.</span><span class="sxs-lookup"><span data-stu-id="188f3-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="188f3-109">Testler, uygulamanın yoğun veya aşamalı olarak yükünü artırır ya da uygulamanın bilgi işlem kaynaklarını sınırlar.</span><span class="sxs-lookup"><span data-stu-id="188f3-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="188f3-110">Stres testleri, stres kapsamındaki bir uygulamanın hatadan kurtulacağını ve düzgün biçimde beklenen davranışa geri döneceğini denetler.</span><span class="sxs-lookup"><span data-stu-id="188f3-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="188f3-111">Stres altında, uygulama normal koşullarda çalıştırılmamaları.</span><span class="sxs-lookup"><span data-stu-id="188f3-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="188f3-112">Visual Studio 2019, Visual Studio 'nun yük testi özellikleriyle son sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="188f3-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="188f3-113">Gelecekte yük testi araçları gerektiren müşteriler için Apache JMeter, Akamai CloudTest ve BlazeMeter gibi alternatif araçlar önerilir.</span><span class="sxs-lookup"><span data-stu-id="188f3-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="188f3-114">Daha fazla bilgi için bkz. [Visual Studio 2019 sürüm notları](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span><span class="sxs-lookup"><span data-stu-id="188f3-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span></span>

<span data-ttu-id="188f3-115">Azure DevOps 'daki yük testi hizmeti 2020 ' de sona eriyor.</span><span class="sxs-lookup"><span data-stu-id="188f3-115">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="188f3-116">Daha fazla bilgi için bkz. [bulut tabanlı yük testi hizmeti yaşam sonu](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="188f3-116">For more information, see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="188f3-117">Visual Studio Araçları</span><span class="sxs-lookup"><span data-stu-id="188f3-117">Visual Studio tools</span></span>

<span data-ttu-id="188f3-118">Visual Studio, kullanıcıların Web performans ve yük testleri oluşturmalarına, geliştirmesine ve hata ayıklamasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="188f3-118">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="188f3-119">Bir Web tarayıcısında eylemleri kaydederek testler oluşturmak için bir seçenek mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="188f3-119">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="188f3-120">Visual Studio 2017 kullanarak yük testi projeleri oluşturma, yapılandırma ve çalıştırma hakkında daha fazla bilgi için bkz [. hızlı başlangıç: Yük testi projesi](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017)oluşturun.</span><span class="sxs-lookup"><span data-stu-id="188f3-120">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span>

<span data-ttu-id="188f3-121">Yük testleri, şirket içinde çalışacak veya Azure DevOps kullanılarak bulutta çalıştırılacak şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="188f3-121">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="188f3-122">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="188f3-122">Azure DevOps</span></span>

<span data-ttu-id="188f3-123">Yük testi çalıştırmaları [Azure DevOps test Plans](/azure/devops/test/load-test/index?view=vsts) hizmeti kullanılarak başlatılabilir.</span><span class="sxs-lookup"><span data-stu-id="188f3-123">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![Azure DevOps yük testi giriş sayfası](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="188f3-125">Hizmet aşağıdaki test biçimlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="188f3-125">The service supports the following test formats:</span></span>

* <span data-ttu-id="188f3-126">Visual Studio &ndash; Web testi Visual Studio 'da oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="188f3-126">Visual Studio &ndash; Web test created in Visual Studio.</span></span>
* <span data-ttu-id="188f3-127">Http Arşivi &ndash; arşiv içinde yakalanan HTTP trafiği test sırasında yeniden yürütülür.</span><span class="sxs-lookup"><span data-stu-id="188f3-127">HTTP Archive &ndash; Captured HTTP traffic inside archive is replayed during testing.</span></span>
* <span data-ttu-id="188f3-128">[URL tabanlı](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Yük testine, istek türlerine, üstbilgilere ve sorgu dizelerine yönelik URL 'lerin belirtilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="188f3-128">[URL-based](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="188f3-129">Süre, yük stili ve Kullanıcı sayısı gibi çalışma ayarı parametrelerini yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="188f3-129">Run setting parameters such as duration, load pattern, and number of users can be configured.</span></span>
* <span data-ttu-id="188f3-130">[Apache JMeter](https://jmeter.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="188f3-130">[Apache JMeter](https://jmeter.apache.org/).</span></span>

## <a name="azure-portal"></a><span data-ttu-id="188f3-131">Azure portal</span><span class="sxs-lookup"><span data-stu-id="188f3-131">Azure portal</span></span>

<span data-ttu-id="188f3-132">[Azure Portal, Web uygulamalarının yük testini](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) doğrudan Azure Portal App Service **performans** sekmesinden ayarlamayı ve çalıştırmayı sağlar.</span><span class="sxs-lookup"><span data-stu-id="188f3-132">[Azure portal allows setting up and running load testing of web apps](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the **Performance** tab of the App Service in Azure portal.</span></span>

![Azure portal Azure App Service](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="188f3-134">Test, belirtilen URL veya Visual Studio Web test dosyası ile birden çok URL 'yi test eden el ile test olabilir.</span><span class="sxs-lookup"><span data-stu-id="188f3-134">The test can be a manual test with a specified URL or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![Azure portal yeni performans test sayfası](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="188f3-136">Testin sonunda, oluşturulan raporlar uygulamanın performans özelliklerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="188f3-136">At end of the test, generated reports show the performance characteristics of the app.</span></span> <span data-ttu-id="188f3-137">Örnek istatistikler şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="188f3-137">Example statistics include:</span></span>

* <span data-ttu-id="188f3-138">Ortalama yanıt süresi</span><span class="sxs-lookup"><span data-stu-id="188f3-138">Average response time</span></span>
* <span data-ttu-id="188f3-139">En fazla aktarım hızı: saniye başına istek</span><span class="sxs-lookup"><span data-stu-id="188f3-139">Max throughput: requests per second</span></span>
* <span data-ttu-id="188f3-140">Hata yüzdesi</span><span class="sxs-lookup"><span data-stu-id="188f3-140">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="188f3-141">Üçüncü taraf araçları</span><span class="sxs-lookup"><span data-stu-id="188f3-141">Third-party tools</span></span>

<span data-ttu-id="188f3-142">Aşağıdaki listede, çeşitli özellik kümelerine sahip üçüncü taraf Web performans araçları yer almaktadır:</span><span class="sxs-lookup"><span data-stu-id="188f3-142">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="188f3-143">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="188f3-143">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="188f3-144">ApacheBench (AB)</span><span class="sxs-lookup"><span data-stu-id="188f3-144">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="188f3-145">Gatling</span><span class="sxs-lookup"><span data-stu-id="188f3-145">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="188f3-146">Locust</span><span class="sxs-lookup"><span data-stu-id="188f3-146">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="188f3-147">Batı rüzgar Web dalgalanma</span><span class="sxs-lookup"><span data-stu-id="188f3-147">West Wind WebSurge</span></span>](https://websurge.west-wind.com/)
* [<span data-ttu-id="188f3-148">Netling</span><span class="sxs-lookup"><span data-stu-id="188f3-148">Netling</span></span>](https://github.com/hallatore/Netling)
* [<span data-ttu-id="188f3-149">Vegeta</span><span class="sxs-lookup"><span data-stu-id="188f3-149">Vegeta</span></span>](https://github.com/tsenart/vegeta)
