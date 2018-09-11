---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Sürekli tümleştirme ve sürekli teslim (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) | Microsoft Docs
author: MikeWasson
description: Gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. Bu, 13 desenler ve kendisi için uygulamalar açıklanmaktadır...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 7a92a68ce8bbeec604a22e082975d33f9f3377c1
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340075"
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Sürekli tümleştirme ve sürekli teslim (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma)
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitabı indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile** e-kitap, Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. 13 desenleri açıklar ve web uygulamaları bulut için geliştirme başarılı yardımcı olabilecek uygulamalar. E-kitabı hakkında daha fazla bilgi için bkz. [ilk bölüm](introduction.md).


İlk iki önerilen geliştirme işlem desenleri [her şeyi otomatikleştirin](automate-everything.md) ve [kaynak denetimi](source-control.md), ve üçüncü işlem deseni bunları birleştirir. Sürekli Tümleştirme (CI), bir geliştirici kaynak deponuza kod iade her bir yapı otomatik olarak tetiklenen anlamına gelir. Sürekli teslim (CD) bunu bir adım daha fazla sürer: bir derleme ve birim testleri otomatikleştirmek başarılı olduktan sonra uygulamayı nerede yapabileceğiniz daha fazla ayrıntılı testler bir ortama otomatik olarak dağıtmak.

Bulut, kullanmakta olduğunuz sürece, yalnızca ortam kaynaklar için ödeme çünkü test ortamı bakım maliyetini en aza indirmek sağlar. CD işleminizi test ortamını ihtiyacınız ve işiniz bittiğinde, ortamı uygulayabileceğiniz ayarlayabilirsiniz sınama.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Sürekli tümleştirme ve sürekli teslim iş akışı

Genellikle, geliştirme ve hazırlık ortamları sürekli teslim yapmanız önerilir. Hatta Microsoft'ta çoğu takım için üretim dağıtımını el ile inceleme ve onay sürecinden gerektirir. Geliştirme ekibinin anahtar kişiler destek için veya düşük trafikli dönemleri sırasında kullanılabilir olduğunda bir üretim için dağıtım emin olmak isteyebilirsiniz'olmuyor. Ancak tüm yapmak için bir geliştirici sahip olacak şekilde bir değişikliği ve bir ortam iade tamamen geliştirme ve test ortamlarınızı otomatikleştirme gelen önlemek için hiçbir şey kabul testi için ayarlanır.

Aşağıdaki diyagramda gelen [bir Microsoft Patterns and Practices e-kitap sürekli teslim hakkında](http://aka.ms/ReleasePipeline) tipik bir iş akışı gösterilmektedir. Özgün bağlamında tam boyutlu görüntüyü görmek için tıklayın.

[![Sürekli teslim iş akışı](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Bulut Hesaplı CI ve CD nasıl sağlar?

Azure'da bu işlemleri otomatik hale getirmek kolaydır. Her şey bulutta çalıştırdığınız için satın alma veya sunucularını, derlemeleri veya test ortamlarınızı yönetme gerekmez. Ve bir sunucu üzerinde testi yapmak kullanılabilir olmasını beklemek zorunda değilsiniz. Bunu her derleme ile Otomasyon betiğinizi, çalışma kabul testleri veya bunlara karşı daha fazla ayrıntılı test kullanarak azure'da bir test ortamını hızla çalıştırın ve yalnızca işiniz bittiğinde sonra geri yıkın. Ve yalnızca sunucu için 2 saat veya 8 saat veya gün çalıştırırsanız, yalnızca bir makine gerçekten çalıştığı süre için ödeme için ödeme yapmanıza gerek para miktarını en az olmasıdır. Örneğin, ortam ücretsiz düzeyden bir katman gidebilir, uygulama, temelde saat başına yaklaşık 1 Sent maliyetleri düzeltme için gereklidir. Bir defada yalnızca saat ortamı çalışmışsa faturaya bir ay boyunca, test ortamınızda büyük olasılıkla kahve satın aldığınız bir latte değerinden maliyeti.

## <a name="azure-devops-services"></a>Azure DevOps Hizmetleri 

Azure DevOps Services ile uygulama geliştirme için dağıtıma yardımcı olacak özellikler sağlar.

- Hem Git (dağıtılmış) ve de (Merkezi) TFVC kaynak denetimi destekler.
- Dinamik olarak zaman, derleme sunucuları oluşturur ve ne zaman, işinizi bitirince götüren bir esnek derleme hizmeti sunar. Bir derleme, birisi kaynak kod değişikliklerini kontrol ve sahip ayırmak ve çoğu zaman boşta kalan kendi derleme sunucularınızı için ödeme gerekmez otomatik olarak başlatabilir. Derleme hizmeti, belirli bir derleme sayısı aşmamak sürece ücretsizdir. Yüksek hacimli yapıları yapmak bekliyorsanız, ayrılmış derleme sunucuları için küçük bir ek ödeme yapabilirsiniz.
- Bu, azure'a sürekli teslimi destekler.
- Bu, otomatik yük testi destekler. Yük testi, bulut uygulaması için kritik öneme sahiptir, ancak çok geç kadar sık ihmal. Benzetim ağır olarak kullanan bir uygulama tarafından binlerce kullanıcıya, böylece performans sorunlarını bulmak ve performansı iyileştirmek yük testi — üretim için uygulamayı yayımlamadan önce.
- Bu, gerçek zamanlı iletişim ve işbirliği küçük Çevik ekipler için kolaylaştıran takım odası işbirliği destekler.
- Bu, Çevik proje yönetimi destekler.


Sürekli tümleştirme ve teslim özellikleri Azure DevOps hizmetleri hakkında daha fazla bilgi için bkz. [Azure DevOps belgeleri](/azure/devops/index).

Anahtar teslim proje yönetimi için arıyorsanız, takım işbirliği ve kaynak denetimi çözümü, Azure DevOps hizmetlerini denetleyin. Adresinde kaydolun [Azure DevOps Hizmetleri](https://dev.azure.com/).

## <a name="summary"></a>Özet

İlk üç bulut geliştirme desenleri nasıl uygulanacağını düşük döngü süresi ile bir tekrarlanabilir, güvenilir ve öngörülebilir bir geliştirme süreci hakkında olmuştur. İçinde [sonraki bölümde](web-development-best-practices.md) mimari ve kodlama desenleri Ara başlayın.

## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için [Azure App Service'te bir web uygulaması dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Ayrıca aşağıdaki kaynaklara bakın:

- [Team Foundation Server 2012 ile yayın işlem hattı oluşturma](http://aka.ms/ReleasePipeline). E-kitap, uygulamalı laboratuvarlar ve Microsoft Patterns and Practices, örnek kodla sürekli teslim ayrıntılı bir giriş sağlar. Visual Studio Laboratuvar Yönetimi ve Visual Studio Release Management kullanımı kapsar.
- [ALM Rangers DevOps araçları ve rehberlik](https://aka.ms/vsarsolutions/). ALM Rangers işbirliği desenleri ile yol gösterici pratik bilgiler ve DevOps Workbench örnek Kılavuzu çözümü sunulan &amp; uygulamalar kitabı *TFS 2012 ile yayın işlem hattı oluşturma*, olarak başlamak için harika bir yoldur DevOps kavramları öğrenme &amp; TFS 2012 için ve incelemek için Release Management. Kılavuzu bir kez oluşturun ve birden çok ortama dağıtma işlemi gösterilmektedir.
- [Visual Studio 2012 ile sürekli teslimat testi](https://msdn.microsoft.com/library/jj159345.aspx). E-kitabı Microsoft Patterns and Practices, tarafından nasıl tümleştireceğinizi otomatik ile sürekli teslimat testi açıklar.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). TFS (bir etikete göre), bir derlemeden yakalamak için tasarlanmış bir aracı için kaynak kodu derleyin, paketi, belirli yönlerini yapılandırmak için DevOps rolü izin vermek ve Azure'a göndermek. Araç, "daha önceden dağıtılan bir sürümüne geri almak" işlemlerini etkinleştirmek için dağıtım işlemini izler. Araç, dış bağımlılıkları yoktur ve tek başına TFS API'leri ve Azure SDK'sını kullanarak çalışabilir.
- [Sürekli teslim: Güvenilir yazılım sürümleri derleme, Test ve dağıtım Otomasyon](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Kitap Jez Humble tarafından.
- [Bırakın! Tasarım ve üretim ortamına hazır yazılım dağıtım](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Kitap Michael t Nygard tarafından.

> [!div class="step-by-step"]
> [Önceki](source-control.md)
> [İleri](web-development-best-practices.md)
