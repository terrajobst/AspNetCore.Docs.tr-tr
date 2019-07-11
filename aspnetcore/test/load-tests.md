---
title: ASP.NET Core yük/stres testi
author: Jeremy-Meng
description: Birkaç önemli araçlar ve yük testi ve ASP.NET Core uygulamalarını stres yaklaşımlar hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: test/loadtests
ms.openlocfilehash: 4b07dd1af7e0c1d3ce9baa167b69fd8f80df204a
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815168"
---
# <a name="aspnet-core-loadstress-testing"></a>ASP.NET Core yük/stres testi

Yük testi ve stres testi yüksek performanslı bir web uygulaması olduğundan emin olmak önemli ve ölçeklenebilir. Benzer testleri genellikle paylaştıkları olsa bile hedeflerine farklıdır.

**Yük testleri** &ndash; uygulama hala yanıt hedefi uyarlarken kullanıcıların belirli bir senaryo için belirtilen bir yük işleyebilir olup olmadığını test edin. Uygulama, normal koşullar altında çalıştırılır.

**Stres testleri** &ndash; genellikle uzun bir süre için aşırı koşullar altında çalıştırıldığı sırada uygulamanın kararlılığını Test. Testleri, uygulamanın yüksek kullanıcı yükü, ani veya aşamalı artan yük yerleştirin veya bunlar uygulamanın bilgi işlem kaynaklarını sınırlayın.

Yük testi, yük altında uygulama ve hatadan kurtarmak için beklenen davranışı düzgün biçimde döndürür belirleyin. Baskı altında uygulama normal koşullar altında çalıştırma değil.

Visual Studio 2019 son Visual Studio Yük testi özellikleriyle sürümüdür. Yük testi araçları gelecekte isteyen müşteriler için Apache JMeter ve Akamai CloudTest BlazeMeter gibi diğer araçları öneririz. Daha fazla bilgi için [Visual Studio 2019 sürüm notları](/visualstudio/releases/2019/release-notes-v16.0#test-tools).

Yük testi hizmetinin Azure DevOps 2020'de sona eriyor. Daha fazla bilgi için [bulut tabanlı yük test etme hizmeti sona erecek](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).

## <a name="visual-studio-tools"></a>Visual Studio Araçları

Visual Studio oluşturma, geliştirme ve web performansı ve yük testleri hata ayıklama olanağı sağlar. Bir seçenek, bir web tarayıcısında eylemlerini kaydederek testleri oluşturmak kullanılabilir.

Oluşturma, yapılandırma ve projeleri Visual Studio 2017'yi kullanarak yük testi çalıştırma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Bir yük testi projesi oluşturma](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017). Daha fazla bilgi için [ek kaynaklar](#additional-resources) bölümü.

Yük testleri, şirket içi veya çalışma Azure DevOps kullanarak bulutta çalıştırmak için yapılandırılabilir.

## <a name="azure-devops"></a>Azure DevOps

Yük testi çalışmaları kullanılarak başlatılabilir [Azure DevOps Test planları](/azure/devops/test/load-test/index?view=vsts) hizmeti.

![Azure DevOps yük testi giriş sayfası](./load-tests/_static/azure-devops-load-test.png)

Hizmet şu test biçimlerini destekler:

* Visual Studio &ndash; Visual Studio'da oluşturulan Web testi.
* HTTP arşivi &ndash; arşiv içinde yakalanan HTTP trafiğini yeniden test sırasında.
* [URL tabanlı](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; yük testi, istek türleri, üst bilgiler ve sorgu dizeleri için URL'leri belirtme sağlar. Süresi gibi parametreleri ayarlanıyor çalıştırın, yük düzeni ve kullanıcı sayısı yapılandırılabilir.
* [Apache JMeter](https://jmeter.apache.org/).

## <a name="azure-portal"></a>Azure portal

[Azure portal sağlar ayarlayarak ve web uygulamalarına yük testi çalıştırarak](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) doğrudan **performans** Azure portalında App Service için sekmesinde.

![Azure portalında Azure App Service](./load-tests/_static/azure-appservice-perf-test.png)

Belirtilen URL ya da birden fazla URL test edebilirsiniz bir Visual Studio Web testi dosyası ile el ile test, test olabilir.

![Azure portalında yeni performans testi sayfası](./load-tests/_static/azure-appservice-perf-test-config.png)

Testin sonunda, oluşturulan raporlar uygulama performans özellikleri gösterir. Örnek istatistikleri şunlardır:

* Ortalama yanıt süresi
* En fazla aktarım hızı: saniye başına istek sayısı
* Başarısızlık yüzdesi

## <a name="third-party-tools"></a>Üçüncü taraf araçları

Aşağıdaki liste, çeşitli özellik kümeleri ile üçüncü taraf web performans araçları içerir:

* [Apache JMeter](https://jmeter.apache.org/)
* [ApacheBench (ab)](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/)
* [Locust](https://locust.io/)
* [Batı Rüzgar WebSurge](https://websurge.west-wind.com/)
* [Netling](https://github.com/hallatore/Netling)
* [Vegeta](https://github.com/tsenart/vegeta)

## <a name="additional-resources"></a>Ek kaynaklar

* [Yük testi blog serisi](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/)
