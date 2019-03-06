---
title: ASP.NET Core yük/stres testi
author: Jeremy-Meng
description: Birkaç önemli araçlar ve yük testi ve ASP.NET Core uygulamalarını stres yaklaşımları açıklanmaktadır.
ms.author: riande
ms.custom: mvc
ms.date: 01/04/2019
uid: test/loadtests
ms.openlocfilehash: 587df6e216943d3eeec779df4d0554dd0fc2fda0
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345434"
---
# <a name="load-and-stress-testing-aspnet-core"></a>Yük ve stres testi ASP.NET Core

Yük testi ve stres testi yüksek performanslı bir web uygulaması olduğundan emin olmak önemli ve ölçeklenebilir. Hedeflerine farklı benzer testleri genellikle bile paylaşırlar.

**Yük testleri**: Uygulamayı hala yanıt hedefi uyarlarken kullanıcıların belirli bir senaryo için belirtilen bir yük işleyebilir olup olmadığını sınar. Uygulama, normal koşullar altında çalıştırılır.

**Stres testleri**: Altında aşırı koşullar ve genellikle bir uzun süre çalışan testler uygulama kararlılık:

* Yüksek kullanıcı yükü – ani veya kademeli olarak artırma.
* Sınırlı bilgi işlem kaynakları.  

Baskı altında uygulama hatasından kurtarabilmek ve düzgün bir şekilde beklenen bir davranış döndürür? Baskı altında uygulamadır *değil* normal koşullar altında çalıştırın.

Yük test etme özelliklerine sahip son Visual Studio sürümü, Visual Studio 2019 olacaktır. Yük test etme araçlarına ihtiyaç duyan müşteriler için Apache JMeter, Akamai CloudTest, Blazemeter gibi alternatif yük test etme araçlarının kullanılmasını öneririz. Daha fazla bilgi için [Visual Studio 2019 Preview sürüm notları](/visualstudio/releases/2019/release-notes-preview#test-tools).

Yük testi hizmetinin Azure DevOps 2020'de sona eriyor. Daha fazla bilgi için [bulut tabanlı yük test etme hizmeti sona erecek](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).

## <a name="visual-studio-tools"></a>Visual Studio Araçları

Visual Studio oluşturma, geliştirme ve web performansı ve yük testleri hata ayıklama olanağı sağlar. Web tarayıcısında eylemlerini kaydederek testleri oluşturmak bir seçenek kullanılabilir.

[Hızlı Başlangıç: Bir yük testi projesi oluşturma](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) oluşturmanızı, yapılandırmanızı ve projeleri Visual Studio 2017'yi kullanarak yük testi çalıştırma işlemi gösterilmektedir.

Bkz: [ek kaynaklar](#add) daha fazla bilgi için.

Yük testleri, şirket içinde çalıştırın veya Azure DevOps kullanarak bulutta çalıştırmak için yapılandırılabilir.

## <a name="azure-devops"></a>Azure DevOps

Yük testi çalışmaları kullanılarak başlatılabilir [Azure DevOps Test planları](/azure/devops/test/load-test/index?view=vsts) hizmeti.

![](./load-tests/_static/azure-devops-load-test.png)

Hizmeti test biçimi aşağıdaki türlerini destekler:

- Visual Studio testi – Visual Studio'da oluşturulan web testi.
- Arşiv içinde yakalanan HTTP trafiğini HTTP arşivi tabanlı test, test sırasında tekrarlanır.
- [URL tabanlı test](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) – yük testi, istek türleri, üst bilgiler ve sorgu dizeleri için URL'leri belirtmeye izin verir. Süresi gibi parametreleri ayarlanıyor çalıştırın, yük düzeni, kullanıcılar, vb. sayısı yapılandırılabilir.
- [Apache JMeter](https://jmeter.apache.org/) test edin.

## <a name="azure-portal"></a>Azure portal

[Azure portal sağlar ayarlayarak ve Web Apps yük testi çalıştırarak](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) doğrudan Azure portalında App Service'nın performans sekmesinden.

![](./load-tests/_static/azure-appservice-perf-test.png)

Belirtilen URL ya da birden fazla URL test edebilirsiniz bir Visual Studio Web testi dosyası ile el ile test, test olabilir.

![](./load-tests/_static/azure-appservice-perf-test-config.png)

Testin sonunda, uygulama performans özelliklerini göstermek için raporlar oluşturulur. Örnek istatistikleri şunlardır:

- Ortalama yanıt süresi
- En fazla aktarım hızı: saniye başına istek sayısı
- Başarısızlık yüzdesi

## <a name="third-party-tools"></a>Üçüncü taraf araçları

Aşağıdaki liste, çeşitli özellik kümeleri ile üçüncü taraf web performans araçları içerir:

- [Apache JMeter](https://jmeter.apache.org/) : Yük Test Etme Araçları tam özellikli paketi. İş parçacığı bağlı: kullanıcı başına tek bir iş parçacığı gerekir.
- [AB - Apache HTTP server değerlendirmesi aracı](https://httpd.apache.org/docs/2.4/programs/ab.html)
- [Gatling](https://gatling.io/) : Masaüstü aracı ile bir GUI ve test Kaydedici. Daha fazla yüksek performanslı JMeter daha.
- [Locust.io](https://locust.io/) : İş parçacıkları tarafından sınırlanan değil.

<a name="add"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

[Yük testi blog dizisini](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) Charles Sterling'le tarafından. Tarihli ancak konuları çoğu yine de ilgilidir.
