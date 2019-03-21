---
title: ASP.NET Core yük/stres testi
author: Jeremy-Meng
description: Birkaç önemli araçlar ve yük testi ve ASP.NET Core uygulamalarını stres yaklaşımları açıklanmaktadır.
ms.author: riande
ms.custom: mvc
ms.date: 01/04/2019
uid: test/loadtests
ms.openlocfilehash: 39632af2c92dac548c03e24d35a5e8a03e00890d
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209839"
---
# <a name="load-and-stress-testing-aspnet-core"></a><span data-ttu-id="af55b-103">Yük ve stres testi ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="af55b-103">Load and stress testing ASP.NET Core</span></span>

<span data-ttu-id="af55b-104">Yük testi ve stres testi yüksek performanslı bir web uygulaması olduğundan emin olmak önemli ve ölçeklenebilir.</span><span class="sxs-lookup"><span data-stu-id="af55b-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="af55b-105">Hedeflerine farklı benzer testleri genellikle bile paylaşırlar.</span><span class="sxs-lookup"><span data-stu-id="af55b-105">Their goals are different even they often share similar tests.</span></span>

<span data-ttu-id="af55b-106">**Yük testleri**: Uygulamayı hala yanıt hedefi uyarlarken kullanıcıların belirli bir senaryo için belirtilen bir yük işleyebilir olup olmadığını sınar.</span><span class="sxs-lookup"><span data-stu-id="af55b-106">**Load tests**: Tests whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="af55b-107">Uygulama, normal koşullar altında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="af55b-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="af55b-108">**Stres testleri**: Altında aşırı koşullar ve genellikle bir uzun süre çalışan testler uygulama kararlılık:</span><span class="sxs-lookup"><span data-stu-id="af55b-108">**Stress tests**: Tests app stability when running under extreme conditions and often a long period of time:</span></span>

* <span data-ttu-id="af55b-109">Yüksek kullanıcı yükü – ani veya kademeli olarak artırma.</span><span class="sxs-lookup"><span data-stu-id="af55b-109">High user load – either spikes or gradually increasing.</span></span>
* <span data-ttu-id="af55b-110">Sınırlı bilgi işlem kaynakları.</span><span class="sxs-lookup"><span data-stu-id="af55b-110">Limited computing resources.</span></span>

<span data-ttu-id="af55b-111">Baskı altında uygulama hatasından kurtarabilmek ve düzgün bir şekilde beklenen bir davranış döndürür?</span><span class="sxs-lookup"><span data-stu-id="af55b-111">Under stress, can the app recover from failure and gracefully return to expected behavior?</span></span> <span data-ttu-id="af55b-112">Baskı altında uygulamadır *değil* normal koşullar altında çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="af55b-112">Under stress, the app is *not* run under normal conditions.</span></span>

<span data-ttu-id="af55b-113">Yük test etme özelliklerine sahip son Visual Studio sürümü, Visual Studio 2019 olacaktır.</span><span class="sxs-lookup"><span data-stu-id="af55b-113">Visual Studio 2019 will be the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="af55b-114">Yük test etme araçlarına ihtiyaç duyan müşteriler için Apache JMeter, Akamai CloudTest, Blazemeter gibi alternatif yük test etme araçlarının kullanılmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="af55b-114">For customers requiring load testing tools, we recommend using alternate load testing tools such as Apache JMeter, Akamai CloudTest, Blazemeter.</span></span> <span data-ttu-id="af55b-115">Daha fazla bilgi için [Visual Studio 2019 Preview sürüm notları](/visualstudio/releases/2019/release-notes-preview#test-tools).</span><span class="sxs-lookup"><span data-stu-id="af55b-115">For more information, see the [Visual Studio 2019 Preview Release Notes](/visualstudio/releases/2019/release-notes-preview#test-tools).</span></span>

<span data-ttu-id="af55b-116">Yük testi hizmetinin Azure DevOps 2020'de sona eriyor.</span><span class="sxs-lookup"><span data-stu-id="af55b-116">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="af55b-117">Daha fazla bilgi için [bulut tabanlı yük test etme hizmeti sona erecek](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="af55b-117">For more information see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="af55b-118">Visual Studio Araçları</span><span class="sxs-lookup"><span data-stu-id="af55b-118">Visual Studio Tools</span></span>

<span data-ttu-id="af55b-119">Visual Studio oluşturma, geliştirme ve web performansı ve yük testleri hata ayıklama olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="af55b-119">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="af55b-120">Web tarayıcısında eylemlerini kaydederek testleri oluşturmak bir seçenek kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="af55b-120">An option is available to create tests by recording actions in web browser.</span></span>

<span data-ttu-id="af55b-121">[Hızlı Başlangıç: Bir yük testi projesi oluşturma](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) oluşturmanızı, yapılandırmanızı ve projeleri Visual Studio 2017'yi kullanarak yük testi çalıştırma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="af55b-121">[Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) shows how to create, configure, and run a load test projects using Visual Studio 2017.</span></span>

<span data-ttu-id="af55b-122">Bkz: [ek kaynaklar](#add) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="af55b-122">See [Additional Resources](#add) for more information.</span></span>

<span data-ttu-id="af55b-123">Yük testleri, şirket içinde çalıştırın veya Azure DevOps kullanarak bulutta çalıştırmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="af55b-123">Load tests can be configured to run in on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="af55b-124">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="af55b-124">Azure DevOps</span></span>

<span data-ttu-id="af55b-125">Yük testi çalışmaları kullanılarak başlatılabilir [Azure DevOps Test planları](/azure/devops/test/load-test/index?view=vsts) hizmeti.</span><span class="sxs-lookup"><span data-stu-id="af55b-125">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![Azure DevOps yük testi giriş sayfası](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="af55b-127">Hizmeti test biçimi aşağıdaki türlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="af55b-127">The service supports the following types of test format:</span></span>

* <span data-ttu-id="af55b-128">Visual Studio testi – Visual Studio'da oluşturulan web testi.</span><span class="sxs-lookup"><span data-stu-id="af55b-128">Visual Studio test – web test created in Visual Studio.</span></span>
* <span data-ttu-id="af55b-129">Arşiv içinde yakalanan HTTP trafiğini HTTP arşivi tabanlı test, test sırasında tekrarlanır.</span><span class="sxs-lookup"><span data-stu-id="af55b-129">HTTP Archive-based test – captured HTTP traffic inside archive is replayed during testing.</span></span>
* <span data-ttu-id="af55b-130">[URL tabanlı test](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) – yük testi, istek türleri, üst bilgiler ve sorgu dizeleri için URL'leri belirtmeye izin verir.</span><span class="sxs-lookup"><span data-stu-id="af55b-130">[URL-based test](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) – allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="af55b-131">Süresi gibi parametreleri ayarlanıyor çalıştırın, yük düzeni, kullanıcılar, vb. sayısı yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="af55b-131">Run setting parameters such as duration, load pattern, number of users, etc., can be configured.</span></span>
* <span data-ttu-id="af55b-132">[Apache JMeter](https://jmeter.apache.org/) test edin.</span><span class="sxs-lookup"><span data-stu-id="af55b-132">[Apache JMeter](https://jmeter.apache.org/) test.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="af55b-133">Azure portal</span><span class="sxs-lookup"><span data-stu-id="af55b-133">Azure portal</span></span>

<span data-ttu-id="af55b-134">[Azure portal sağlar ayarlayarak ve Web Apps yük testi çalıştırarak](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) doğrudan Azure portalında App Service'nın performans sekmesinden.</span><span class="sxs-lookup"><span data-stu-id="af55b-134">[Azure portal allows setting up and running load testing of Web Apps,](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the Performance tab of the App Service in Azure portal.</span></span>

![Azure portalında Azure uygulama hizmeti](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="af55b-136">Belirtilen URL ya da birden fazla URL test edebilirsiniz bir Visual Studio Web testi dosyası ile el ile test, test olabilir.</span><span class="sxs-lookup"><span data-stu-id="af55b-136">The test can be a manual test with a specified URL, or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![Azure portalında yeni performans testi sayfası](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="af55b-138">Testin sonunda, uygulama performans özelliklerini göstermek için raporlar oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="af55b-138">At end of the test, reports are generated to show the performance characteristics of the app.</span></span> <span data-ttu-id="af55b-139">Örnek istatistikleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="af55b-139">Example statistics include:</span></span>

* <span data-ttu-id="af55b-140">Ortalama yanıt süresi</span><span class="sxs-lookup"><span data-stu-id="af55b-140">Average response time</span></span>
* <span data-ttu-id="af55b-141">En fazla aktarım hızı: saniye başına istek sayısı</span><span class="sxs-lookup"><span data-stu-id="af55b-141">Max throughput: requests per second</span></span>
* <span data-ttu-id="af55b-142">Başarısızlık yüzdesi</span><span class="sxs-lookup"><span data-stu-id="af55b-142">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="af55b-143">Üçüncü taraf araçları</span><span class="sxs-lookup"><span data-stu-id="af55b-143">Third-party Tools</span></span>

<span data-ttu-id="af55b-144">Aşağıdaki liste, çeşitli özellik kümeleri ile üçüncü taraf web performans araçları içerir:</span><span class="sxs-lookup"><span data-stu-id="af55b-144">The following list contains third-party web performance tools with various feature sets:</span></span>

* <span data-ttu-id="af55b-145">[Apache JMeter](https://jmeter.apache.org/) : Yük Test Etme Araçları tam özellikli paketi.</span><span class="sxs-lookup"><span data-stu-id="af55b-145">[Apache JMeter](https://jmeter.apache.org/) : Full featured suite of load testing tools.</span></span> <span data-ttu-id="af55b-146">İş parçacığı bağlı: kullanıcı başına tek bir iş parçacığı gerekir.</span><span class="sxs-lookup"><span data-stu-id="af55b-146">Thread-bound: need one thread per user.</span></span>
* [<span data-ttu-id="af55b-147">AB - Apache HTTP server değerlendirmesi aracı</span><span class="sxs-lookup"><span data-stu-id="af55b-147">ab - Apache HTTP server benchmarking tool</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* <span data-ttu-id="af55b-148">[Gatling](https://gatling.io/) : Masaüstü aracı ile bir GUI ve test Kaydedici.</span><span class="sxs-lookup"><span data-stu-id="af55b-148">[Gatling](https://gatling.io/) : Desktop tool with a GUI and test recorders.</span></span> <span data-ttu-id="af55b-149">Daha fazla yüksek performanslı JMeter daha.</span><span class="sxs-lookup"><span data-stu-id="af55b-149">More performant than JMeter.</span></span>
* <span data-ttu-id="af55b-150">[Locust.io](https://locust.io/) : İş parçacıkları tarafından sınırlanan değil.</span><span class="sxs-lookup"><span data-stu-id="af55b-150">[Locust.io](https://locust.io/) : Not bounded by threads.</span></span>

<a name="add"></a>

## <a name="additional-resources"></a><span data-ttu-id="af55b-151">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="af55b-151">Additional Resources</span></span>

<span data-ttu-id="af55b-152">[Yük testi blog dizisini](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) Charles Sterling'le tarafından.</span><span class="sxs-lookup"><span data-stu-id="af55b-152">[Load Test blog series](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) by Charles Sterling.</span></span> <span data-ttu-id="af55b-153">Tarihli ancak konuları çoğu yine de ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="af55b-153">Dated but most of the topics are still relevant.</span></span>
