---
title: ASP.NET Core yük/stres testi
author: Jeremy-Meng
description: Birkaç önemli araçlar ve yük testi ve ASP.NET Core uygulamalarını stres yaklaşımları açıklanmaktadır.
ms.author: riande
ms.custom: mvc
ms.date: 01/04/2019
uid: test/loadtests
ms.openlocfilehash: 0a53405cba19435a74b398ba42a05456c50bdc72
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099541"
---
# <a name="load-and-stress-testing-aspnet-core"></a><span data-ttu-id="7ec06-103">Yük ve stres testi ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ec06-103">Load and stress testing ASP.NET Core</span></span>

<span data-ttu-id="7ec06-104">Yük testi ve stres testi yüksek performanslı bir web uygulaması olduğundan emin olmak önemli ve ölçeklenebilir.</span><span class="sxs-lookup"><span data-stu-id="7ec06-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="7ec06-105">Hedeflerine farklı benzer testleri genellikle bile paylaşırlar.</span><span class="sxs-lookup"><span data-stu-id="7ec06-105">Their goals are different even they often share similar tests.</span></span>

<span data-ttu-id="7ec06-106">**Yük testleri**: Uygulamayı hala yanıt hedefi uyarlarken kullanıcıların belirli bir senaryo için belirtilen bir yük işleyebilir olup olmadığını sınar.</span><span class="sxs-lookup"><span data-stu-id="7ec06-106">**Load tests**: Tests whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="7ec06-107">Uygulama, normal koşullar altında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="7ec06-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="7ec06-108">**Stres testleri**: Altında aşırı koşullar ve genellikle bir uzun süre çalışan testler uygulama kararlılık:</span><span class="sxs-lookup"><span data-stu-id="7ec06-108">**Stress tests**: Tests app stability when running under extreme conditions and often a long period of time:</span></span>

* <span data-ttu-id="7ec06-109">Yüksek kullanıcı yükü – ani veya kademeli olarak artırma.</span><span class="sxs-lookup"><span data-stu-id="7ec06-109">High user load – either spikes or gradually increasing.</span></span>
* <span data-ttu-id="7ec06-110">Sınırlı bilgi işlem kaynakları.</span><span class="sxs-lookup"><span data-stu-id="7ec06-110">Limited computing resources.</span></span>  

<span data-ttu-id="7ec06-111">Baskı altında uygulama hatasından kurtarabilmek ve düzgün bir şekilde beklenen bir davranış döndürür?</span><span class="sxs-lookup"><span data-stu-id="7ec06-111">Under stress, can the app recover from failure and gracefully return to expected behavior?</span></span> <span data-ttu-id="7ec06-112">Uygulama, normal koşullar altında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="7ec06-112">The app is run under normal conditions.</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="7ec06-113">Visual Studio Araçları</span><span class="sxs-lookup"><span data-stu-id="7ec06-113">Visual Studio Tools</span></span>

<span data-ttu-id="7ec06-114">Visual Studio oluşturma, geliştirme ve web performansı ve yük testleri hata ayıklama olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ec06-114">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="7ec06-115">Web tarayıcısında eylemlerini kaydederek testleri oluşturmak bir seçenek kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ec06-115">An option is available to create tests by recording actions in web browser.</span></span>

<span data-ttu-id="7ec06-116">[Hızlı Başlangıç: Bir yük testi projesi oluşturma](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) oluşturmanızı, yapılandırmanızı ve projeleri Visual Studio 2017'yi kullanarak yük testi çalıştırma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7ec06-116">[Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) shows how to create, configure, and run a load test projects using Visual Studio 2017.</span></span>

<span data-ttu-id="7ec06-117">Bkz: [ek kaynaklar](#add) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="7ec06-117">See [Additional Resources](#add) for more information.</span></span>

<span data-ttu-id="7ec06-118">Yük testleri, şirket içinde çalıştırın veya Azure DevOps kullanarak bulutta çalıştırmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ec06-118">Load tests can be configured to run in on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="7ec06-119">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="7ec06-119">Azure DevOps</span></span>

<span data-ttu-id="7ec06-120">Yük testi çalışmaları kullanılarak başlatılabilir [Azure DevOps Test planları](/azure/devops/test/load-test/index?view=vsts) hizmeti.</span><span class="sxs-lookup"><span data-stu-id="7ec06-120">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="7ec06-121">Hizmeti test biçimi aşağıdaki türlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="7ec06-121">The service supports the following types of test format:</span></span>

- <span data-ttu-id="7ec06-122">Visual Studio testi – Visual Studio'da oluşturulan web testi.</span><span class="sxs-lookup"><span data-stu-id="7ec06-122">Visual Studio test – web test created in Visual Studio.</span></span>
- <span data-ttu-id="7ec06-123">Arşiv içinde yakalanan HTTP trafiğini HTTP arşivi tabanlı test, test sırasında tekrarlanır.</span><span class="sxs-lookup"><span data-stu-id="7ec06-123">HTTP Archive-based test – captured HTTP traffic inside archive is replayed during testing.</span></span>
- <span data-ttu-id="7ec06-124">[URL tabanlı test](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) – yük testi, istek türleri, üst bilgiler ve sorgu dizeleri için URL'leri belirtmeye izin verir.</span><span class="sxs-lookup"><span data-stu-id="7ec06-124">[URL-based test](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) – allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="7ec06-125">Süresi gibi parametreleri ayarlanıyor çalıştırın, yük düzeni, kullanıcılar, vb. sayısı yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7ec06-125">Run setting parameters such as duration, load pattern, number of users, etc., can be configured.</span></span>
- <span data-ttu-id="7ec06-126">[Apache JMeter](https://jmeter.apache.org/) test edin.</span><span class="sxs-lookup"><span data-stu-id="7ec06-126">[Apache JMeter](https://jmeter.apache.org/) test.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="7ec06-127">Azure portal</span><span class="sxs-lookup"><span data-stu-id="7ec06-127">Azure portal</span></span>

<span data-ttu-id="7ec06-128">[Azure portal sağlar ayarlayarak ve Web Apps yük testi çalıştırarak](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) doğrudan Azure portalında App Service'nın performans sekmesinden.</span><span class="sxs-lookup"><span data-stu-id="7ec06-128">[Azure portal allows setting up and running load testing of Web Apps,](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the Performance tab of the App Service in Azure portal.</span></span>

![](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="7ec06-129">Belirtilen URL ya da birden fazla URL test edebilirsiniz bir Visual Studio Web testi dosyası ile el ile test, test olabilir.</span><span class="sxs-lookup"><span data-stu-id="7ec06-129">The test can be a manual test with a specified URL, or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="7ec06-130">Testin sonunda, uygulama performans özelliklerini göstermek için raporlar oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7ec06-130">At end of the test, reports are generated to show the performance characteristics of the app.</span></span> <span data-ttu-id="7ec06-131">Örnek istatistikleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="7ec06-131">Example statistics include:</span></span>

- <span data-ttu-id="7ec06-132">Ortalama yanıt süresi</span><span class="sxs-lookup"><span data-stu-id="7ec06-132">Average response time</span></span>
- <span data-ttu-id="7ec06-133">En fazla aktarım hızı: saniye başına istek sayısı</span><span class="sxs-lookup"><span data-stu-id="7ec06-133">Max throughput: requests per second</span></span>
- <span data-ttu-id="7ec06-134">Başarısızlık yüzdesi</span><span class="sxs-lookup"><span data-stu-id="7ec06-134">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="7ec06-135">Üçüncü taraf araçları</span><span class="sxs-lookup"><span data-stu-id="7ec06-135">Third-party Tools</span></span>

<span data-ttu-id="7ec06-136">Aşağıdaki liste, çeşitli özellik kümeleri ile üçüncü taraf web performans araçları içerir:</span><span class="sxs-lookup"><span data-stu-id="7ec06-136">The following list contains third-party web performance tools with various feature sets:</span></span>

- <span data-ttu-id="7ec06-137">[Apache JMeter](https://jmeter.apache.org/) : Yük Test Etme Araçları tam özellikli paketi.</span><span class="sxs-lookup"><span data-stu-id="7ec06-137">[Apache JMeter](https://jmeter.apache.org/) : Full featured suite of load testing tools.</span></span> <span data-ttu-id="7ec06-138">İş parçacığı bağlı: kullanıcı başına tek bir iş parçacığı gerekir.</span><span class="sxs-lookup"><span data-stu-id="7ec06-138">Thread-bound: need one thread per user.</span></span>
- [<span data-ttu-id="7ec06-139">AB - Apache HTTP server değerlendirmesi aracı</span><span class="sxs-lookup"><span data-stu-id="7ec06-139">ab - Apache HTTP server benchmarking tool</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
- <span data-ttu-id="7ec06-140">[Gatling](https://gatling.io/) : Masaüstü aracı ile bir GUI ve test Kaydedici.</span><span class="sxs-lookup"><span data-stu-id="7ec06-140">[Gatling](https://gatling.io/) : Desktop tool with a GUI and test recorders.</span></span> <span data-ttu-id="7ec06-141">Daha fazla yüksek performanslı JMeter daha.</span><span class="sxs-lookup"><span data-stu-id="7ec06-141">More performant than JMeter.</span></span>
- <span data-ttu-id="7ec06-142">[Locust.io](https://locust.io/) : İş parçacıkları tarafından sınırlanan değil.</span><span class="sxs-lookup"><span data-stu-id="7ec06-142">[Locust.io](https://locust.io/) : Not bounded by threads.</span></span>

<a name="add"></a>
## <a name="additional-resources"></a><span data-ttu-id="7ec06-143">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7ec06-143">Additional Resources</span></span>

<span data-ttu-id="7ec06-144">[Yük testi blog dizisini](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) Charles Sterling'le tarafından.</span><span class="sxs-lookup"><span data-stu-id="7ec06-144">[Load Test blog series](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) by Charles Sterling.</span></span> <span data-ttu-id="7ec06-145">Tarihli ancak konuları çoğu yine de ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="7ec06-145">Dated but most of the topics are still relevant.</span></span>