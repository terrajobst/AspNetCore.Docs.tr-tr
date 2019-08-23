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
# <a name="aspnet-core-loadstress-testing"></a>Yük/stres testini ASP.NET Core

Yük testi ve stres testi, Web uygulamasının performansı ve ölçeklenebilir olmasını sağlamak için önemlidir. Genellikle benzer testleri paylaşsalar bile hedefleri farklıdır.

**Yük testleri** &ndash; Uygulamanın belirli bir kullanıcı yükünü, yanıt hedefini karşılarken, belirli bir senaryo için işleyebilip işleyemeyeceğini test edin. Uygulama normal koşullarda çalıştırılır.

**Stres testleri** &ndash; Genellikle uzun bir süre boyunca çok fazla koşullarda çalışırken uygulamanın kararlılığını test edin. Testler, uygulamanın yoğun veya aşamalı olarak yükünü artırır ya da uygulamanın bilgi işlem kaynaklarını sınırlar.

Stres testleri, stres kapsamındaki bir uygulamanın hatadan kurtulacağını ve düzgün biçimde beklenen davranışa geri döneceğini denetler. Stres altında, uygulama normal koşullarda çalıştırılmamaları.

Visual Studio 2019, Visual Studio 'nun yük testi özellikleriyle son sürümüdür. Gelecekte yük testi araçları gerektiren müşteriler için Apache JMeter, Akamai CloudTest ve BlazeMeter gibi alternatif araçlar önerilir. Daha fazla bilgi için bkz. [Visual Studio 2019 sürüm notları](/visualstudio/releases/2019/release-notes-v16.0#test-tools).

Azure DevOps 'daki yük testi hizmeti 2020 ' de sona eriyor. Daha fazla bilgi için bkz. [bulut tabanlı yük testi hizmeti yaşam sonu](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).

## <a name="visual-studio-tools"></a>Visual Studio Araçları

Visual Studio, kullanıcıların Web performans ve yük testleri oluşturmalarına, geliştirmesine ve hata ayıklamasına olanak tanır. Bir Web tarayıcısında eylemleri kaydederek testler oluşturmak için bir seçenek mevcuttur.

Visual Studio 2017 kullanarak yük testi projeleri oluşturma, yapılandırma ve çalıştırma hakkında daha fazla bilgi için bkz [. hızlı başlangıç: Yük testi projesi](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017)oluşturun.

Yük testleri, şirket içinde çalışacak veya Azure DevOps kullanılarak bulutta çalıştırılacak şekilde yapılandırılabilir.

## <a name="azure-devops"></a>Azure DevOps

Yük testi çalıştırmaları [Azure DevOps test Plans](/azure/devops/test/load-test/index?view=vsts) hizmeti kullanılarak başlatılabilir.

![Azure DevOps yük testi giriş sayfası](./load-tests/_static/azure-devops-load-test.png)

Hizmet aşağıdaki test biçimlerini destekler:

* Visual Studio &ndash; Web testi Visual Studio 'da oluşturuldu.
* Http Arşivi &ndash; arşiv içinde yakalanan HTTP trafiği test sırasında yeniden yürütülür.
* [URL tabanlı](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Yük testine, istek türlerine, üstbilgilere ve sorgu dizelerine yönelik URL 'lerin belirtilmesine izin verir. Süre, yük stili ve Kullanıcı sayısı gibi çalışma ayarı parametrelerini yapılandırabilirsiniz.
* [Apache JMeter](https://jmeter.apache.org/).

## <a name="azure-portal"></a>Azure portal

[Azure Portal, Web uygulamalarının yük testini](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) doğrudan Azure Portal App Service **performans** sekmesinden ayarlamayı ve çalıştırmayı sağlar.

![Azure portal Azure App Service](./load-tests/_static/azure-appservice-perf-test.png)

Test, belirtilen URL veya Visual Studio Web test dosyası ile birden çok URL 'yi test eden el ile test olabilir.

![Azure portal yeni performans test sayfası](./load-tests/_static/azure-appservice-perf-test-config.png)

Testin sonunda, oluşturulan raporlar uygulamanın performans özelliklerini gösterir. Örnek istatistikler şunları içerir:

* Ortalama yanıt süresi
* En fazla aktarım hızı: saniye başına istek
* Hata yüzdesi

## <a name="third-party-tools"></a>Üçüncü taraf araçları

Aşağıdaki listede, çeşitli özellik kümelerine sahip üçüncü taraf Web performans araçları yer almaktadır:

* [Apache JMeter](https://jmeter.apache.org/)
* [ApacheBench (AB)](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/)
* [Locust](https://locust.io/)
* [Batı rüzgar Web dalgalanma](https://websurge.west-wind.com/)
* [Netling](https://github.com/hallatore/Netling)
* [Vegeta](https://github.com/tsenart/vegeta)
