---
title: ASP.NET Core yük/stres testi
author: Jeremy-Meng
description: Birkaç önemli araçlar ve yük testi ve ASP.NET Core uygulamalarını stres yaklaşımlar hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: test/loadtests
ms.openlocfilehash: 0a8449ea2c9df0f2ac93058f03af0a1a2aa66508
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068189"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="d74f8-103">ASP.NET Core yük/stres testi</span><span class="sxs-lookup"><span data-stu-id="d74f8-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="d74f8-104">Yük testi ve stres testi yüksek performanslı bir web uygulaması olduğundan emin olmak önemli ve ölçeklenebilir.</span><span class="sxs-lookup"><span data-stu-id="d74f8-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="d74f8-105">Benzer testleri genellikle paylaştıkları olsa bile hedeflerine farklıdır.</span><span class="sxs-lookup"><span data-stu-id="d74f8-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="d74f8-106">**Yük testleri** &ndash; uygulama hala yanıt hedefi uyarlarken kullanıcıların belirli bir senaryo için belirtilen bir yük işleyebilir olup olmadığını test edin.</span><span class="sxs-lookup"><span data-stu-id="d74f8-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="d74f8-107">Uygulama, normal koşullar altında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="d74f8-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="d74f8-108">**Stres testleri** &ndash; genellikle uzun bir süre için aşırı koşullar altında çalıştırıldığı sırada uygulamanın kararlılığını Test.</span><span class="sxs-lookup"><span data-stu-id="d74f8-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="d74f8-109">Testleri, uygulamanın yüksek kullanıcı yükü, ani veya aşamalı artan yük yerleştirin veya bunlar uygulamanın bilgi işlem kaynaklarını sınırlayın.</span><span class="sxs-lookup"><span data-stu-id="d74f8-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="d74f8-110">Yük testi, yük altında uygulama ve hatadan kurtarmak için beklenen davranışı düzgün biçimde döndürür belirleyin.</span><span class="sxs-lookup"><span data-stu-id="d74f8-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="d74f8-111">Baskı altında uygulama normal koşullar altında çalıştırma değil.</span><span class="sxs-lookup"><span data-stu-id="d74f8-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="d74f8-112">Visual Studio 2019 son Visual Studio Yük testi özellikleriyle sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="d74f8-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="d74f8-113">Yük testi araçları gelecekte isteyen müşteriler için Apache JMeter ve Akamai CloudTest BlazeMeter gibi diğer araçları öneririz.</span><span class="sxs-lookup"><span data-stu-id="d74f8-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="d74f8-114">Daha fazla bilgi için [Visual Studio 2019 sürüm notları](/visualstudio/releases/2019/release-notes#test-tools).</span><span class="sxs-lookup"><span data-stu-id="d74f8-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes#test-tools).</span></span>

<span data-ttu-id="d74f8-115">Yük testi hizmetinin Azure DevOps 2020'de sona eriyor.</span><span class="sxs-lookup"><span data-stu-id="d74f8-115">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="d74f8-116">Daha fazla bilgi için [bulut tabanlı yük test etme hizmeti sona erecek](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="d74f8-116">For more information, see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="d74f8-117">Visual Studio Araçları</span><span class="sxs-lookup"><span data-stu-id="d74f8-117">Visual Studio tools</span></span>

<span data-ttu-id="d74f8-118">Visual Studio oluşturma, geliştirme ve web performansı ve yük testleri hata ayıklama olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="d74f8-118">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="d74f8-119">Bir seçenek, bir web tarayıcısında eylemlerini kaydederek testleri oluşturmak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d74f8-119">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="d74f8-120">Oluşturma, yapılandırma ve projeleri Visual Studio 2017'yi kullanarak yük testi çalıştırma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Bir yük testi projesi oluşturma](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span><span class="sxs-lookup"><span data-stu-id="d74f8-120">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span> <span data-ttu-id="d74f8-121">Daha fazla bilgi için [ek kaynaklar](#additional-resources) bölümü.</span><span class="sxs-lookup"><span data-stu-id="d74f8-121">For more information, see the [Additional resources](#additional-resources) section.</span></span>

<span data-ttu-id="d74f8-122">Yük testleri, şirket içi veya çalışma Azure DevOps kullanarak bulutta çalıştırmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d74f8-122">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="d74f8-123">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="d74f8-123">Azure DevOps</span></span>

<span data-ttu-id="d74f8-124">Yük testi çalışmaları kullanılarak başlatılabilir [Azure DevOps Test planları](/azure/devops/test/load-test/index?view=vsts) hizmeti.</span><span class="sxs-lookup"><span data-stu-id="d74f8-124">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![Azure DevOps yük testi giriş sayfası](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="d74f8-126">Hizmet şu test biçimlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="d74f8-126">The service supports the following test formats:</span></span>

* <span data-ttu-id="d74f8-127">Visual Studio &ndash; Visual Studio'da oluşturulan Web testi.</span><span class="sxs-lookup"><span data-stu-id="d74f8-127">Visual Studio &ndash; Web test created in Visual Studio.</span></span>
* <span data-ttu-id="d74f8-128">HTTP arşivi &ndash; arşiv içinde yakalanan HTTP trafiğini yeniden test sırasında.</span><span class="sxs-lookup"><span data-stu-id="d74f8-128">HTTP Archive &ndash; Captured HTTP traffic inside archive is replayed during testing.</span></span>
* <span data-ttu-id="d74f8-129">[URL tabanlı](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; yük testi, istek türleri, üst bilgiler ve sorgu dizeleri için URL'leri belirtme sağlar.</span><span class="sxs-lookup"><span data-stu-id="d74f8-129">[URL-based](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="d74f8-130">Süresi gibi parametreleri ayarlanıyor çalıştırın, yük düzeni ve kullanıcı sayısı yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d74f8-130">Run setting parameters such as duration, load pattern, and number of users can be configured.</span></span>
* <span data-ttu-id="d74f8-131">[Apache JMeter](https://jmeter.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="d74f8-131">[Apache JMeter](https://jmeter.apache.org/).</span></span>

## <a name="azure-portal"></a><span data-ttu-id="d74f8-132">Azure portal</span><span class="sxs-lookup"><span data-stu-id="d74f8-132">Azure portal</span></span>

<span data-ttu-id="d74f8-133">[Azure portal sağlar ayarlayarak ve web uygulamalarına yük testi çalıştırarak](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) doğrudan **performans** Azure portalında App Service için sekmesinde.</span><span class="sxs-lookup"><span data-stu-id="d74f8-133">[Azure portal allows setting up and running load testing of web apps](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the **Performance** tab of the App Service in Azure portal.</span></span>

![Azure portalında Azure App Service](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="d74f8-135">Belirtilen URL ya da birden fazla URL test edebilirsiniz bir Visual Studio Web testi dosyası ile el ile test, test olabilir.</span><span class="sxs-lookup"><span data-stu-id="d74f8-135">The test can be a manual test with a specified URL or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![Azure portalında yeni performans testi sayfası](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="d74f8-137">Testin sonunda, oluşturulan raporlar uygulama performans özellikleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="d74f8-137">At end of the test, generated reports show the performance characteristics of the app.</span></span> <span data-ttu-id="d74f8-138">Örnek istatistikleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="d74f8-138">Example statistics include:</span></span>

* <span data-ttu-id="d74f8-139">Ortalama yanıt süresi</span><span class="sxs-lookup"><span data-stu-id="d74f8-139">Average response time</span></span>
* <span data-ttu-id="d74f8-140">En fazla aktarım hızı: saniye başına istek sayısı</span><span class="sxs-lookup"><span data-stu-id="d74f8-140">Max throughput: requests per second</span></span>
* <span data-ttu-id="d74f8-141">Başarısızlık yüzdesi</span><span class="sxs-lookup"><span data-stu-id="d74f8-141">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="d74f8-142">Üçüncü taraf araçları</span><span class="sxs-lookup"><span data-stu-id="d74f8-142">Third-party tools</span></span>

<span data-ttu-id="d74f8-143">Aşağıdaki liste, çeşitli özellik kümeleri ile üçüncü taraf web performans araçları içerir:</span><span class="sxs-lookup"><span data-stu-id="d74f8-143">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="d74f8-144">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="d74f8-144">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="d74f8-145">ApacheBench (ab)</span><span class="sxs-lookup"><span data-stu-id="d74f8-145">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="d74f8-146">Gatling</span><span class="sxs-lookup"><span data-stu-id="d74f8-146">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="d74f8-147">Locust</span><span class="sxs-lookup"><span data-stu-id="d74f8-147">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="d74f8-148">Batı Rüzgar WebSurge</span><span class="sxs-lookup"><span data-stu-id="d74f8-148">West Wind WebSurge</span></span>](http://websurge.west-wind.com/)
* [<span data-ttu-id="d74f8-149">Netling</span><span class="sxs-lookup"><span data-stu-id="d74f8-149">Netling</span></span>](https://github.com/hallatore/Netling)

## <a name="additional-resources"></a><span data-ttu-id="d74f8-150">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d74f8-150">Additional resources</span></span>

* [<span data-ttu-id="d74f8-151">Yük testi blog serisi</span><span class="sxs-lookup"><span data-stu-id="d74f8-151">Load Test blog series</span></span>](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/)
