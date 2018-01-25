---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: "Sürekli tümleştirme ve kesintisiz teslim (Azure ile gerçek bulut uygulamaları derleme) | Microsoft Docs"
author: MikeWasson
description: "Yapı gerçek dünya ile bulut uygulamaları Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunu temel alır. 13 desenleri ve kendisi için yöntemler açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 4a5433a7dd70e27b59163822ba427b026c3f4ce0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Sürekli tümleştirme ve kesintisiz teslim (Azure ile gerçek bulut uygulamaları derleme)
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitap indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünya bulut uygulamalarını Azure ile** e-kitap Scott Guthrie tarafından geliştirilen bir sunu dayanır. 13 desenleri açıklar ve yardımcı olacak yöntemler bulutu için web uygulamaları geliştirme başarılı. E-kitap hakkında daha fazla bilgi için bkz: [ilk bölüm](introduction.md).


İlk iki önerilen geliştirme işlemi desenleri [her şeyi otomatikleştirmek](automate-everything.md) ve [kaynak denetimi](source-control.md), ve üçüncü işlem düzeni bunları birleştirir. Sürekli Tümleştirme (CI) kaynak deposu kodu bir geliştirici denetler her bir yapı otomatik olarak tetiklenir anlamına gelir. Kesintisiz teslim (CD) bunu bir adım daha fazla sürer: bir yapı ve otomatikleştirilmiş testler başarılı olduktan sonra otomatik olarak burada yapabileceğiniz daha fazla ayrıntılı sınama ortamında dağıtırsınız.

Bulut, kullanmakta olduğunuz sürece ortam kaynakları için yalnızca ödeme için bir test ortamı bakım maliyetini en aza indirmek sağlar. CD işleminizi test ortamını ihtiyacınız ve tamamladığınızda ortamını alabilir ayarlayabilirsiniz sınama.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Sürekli tümleştirme ve kesintisiz teslim iş akışı

Genellikle, geliştirme ve hazırlık ortamları için kesintisiz teslim yapmanızı öneririz. Hatta Microsoft'taki çoğu takımlar Üretim dağıtımı için el ile gözden geçirin ve onay işlemi gerektirir. Üretim için geliştirme ekibi anahtar kişiler desteği veya düşük trafiği dönemlerinde kullanılabilir emin olmak isteyebilirsiniz dağıtım olur. Ancak tüm yapmak için bir geliştirici sahip olmasını sağlamak bir değişiklik ve ortam denetimi tamamen geliştirme ve test ortamları otomatikleştirme gelen önlemek için hiçbir şey kabul testi için ayarlanır.

Aşağıdaki diyagramda [bir Microsoft Patterns and Practices e-kitap sürekli teslimi hakkında](http://aka.ms/ReleasePipeline) normal bir iş akışı gösterilmektedir. Görüntünün görmek için özgün bağlamında tam boyutlu'ı tıklatın.

[![Kesintisiz teslim iş akışı](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Düşük maliyetli CI ve CD buluta nasıl sağlar

Bu işlemler Azure otomatikleştirme kolaydır. Her şeyi bulutta çalıştırdığınız için satın alma veya derlemeleriniz veya test ortamları için sunucuları yönetmek yok. Ve bir sunucu üzerinde testi yapmak kullanılabilir olmasını beklemek zorunda değilsiniz. Bunu her bir yapı ile bir Azure Otomasyonu kodunuzu, çalışma kabul ya da daha fazla ayrıntılı testleri karşısında kullanarak test ortamında dönmesi ve yalnızca bittiğinde sonra onu kesmeden. Ve yalnızca bu sunucu için 2 saat veya 8 saat veya günde çalıştırırsanız, yalnızca bir makine çalıştırdığını zaman için ödeme nedeniyle için ödeme yapmanız para miktarını, kısadır. Örneğin, ortam, bir katman ücretsiz düzeyden giderseniz, uygulama temelde saat başına yaklaşık 1 Sent maliyetleri düzeltme için gereklidir. Bir kerede yalnızca saat ortamı çalıştıracaksanız, bir ay süresince, test ortamınızda büyük olasılıkla Starbucks satın latte değerinden maliyeti.

## <a name="visual-studio-team-services-vsts"></a>Visual Studio Team Services (VSTS)

VSTS, uygulama geliştirme için dağıtım planlama gelen yardımcı olmak üzere birçok özellik sağlar.

- Git (dağıtılmış) ve (Merkezi) TFVC'yi kaynak denetimini destekler.
- Dinamik olarak bunlar gerekmediğinde yapı sunucuları oluşturur ve ne zaman bunlar işiniz bittiğinde götüren anlamına gelir bir esnek yapı hizmeti sunar. Otomatik olarak devre dışı bir yapı birisi kaynak kod değişikliklerini denetler ve sahip ayırmak ve çoğu zaman boşta kalan kendi yapı sunucuları için ödeme gerekmez tetiklersiniz. Belirli bir sayıda derlemeleri aşmayan sürece yapı hizmeti ücretsizdir. Derlemeleri yüksek hacimli yapmak bekliyorsanız, ayrılmış yapı sunucuları için çok az ek ödeme.
- Azure için sürekli teslim destekler.
- Bu, otomatik yük testi destekler. Yük testi bulut uygulama için kritik öneme sahiptir, ancak çok geç kadar sık ihmal. Taklit eden bir uygulama tarafından engelleri bulun ve performansı iyileştirmek etkinleştirme kullanıcılar, binlerce kullanımına ağırlık yük testi — üretim uygulamaya bırakmadan önce.
- Gerçek zamanlı iletişim ve işbirliği küçük Çevik ekipleri için kolaylaştıran takım odası işbirliği destekler.
- Çevik proje yönetimi destekler.


VSTS teslimat özelliklerini ve sürekli tümleştirme hakkında daha fazla bilgi için bkz: [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

Bir anahtar teslim proje yönetimi, ekip işbirliği ve kaynak denetimi çözümü kullanıma VSTS arıyorsanız. En fazla 5 kullanıcılar için ücretsiz bir hizmettir ve yerinde için kaydolabilirsiniz [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

## <a name="summary"></a>Özet

İlk üç bulut geliştirme desenlerinden düşük döngü süresi yinelenebilir, güvenilir ve tahmin edilebilir geliştirme işlemine gerçekleştirme hakkında olmuştur. İçinde [sonraki bölümde](web-development-best-practices.md) biz mimari ve kodlama desenleri aramak Başlat.

## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için bkz: [Azure App Service'te bir web uygulaması dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Ayrıca aşağıdaki kaynaklara bakın:

- [Team Foundation Server 2012 ile yayın işlem hattı oluşturma](http://aka.ms/ReleasePipeline). E-kitap, uygulamalı Laboratuarları ve örnek kod Microsoft Patterns and Practices, tarafından sürekli teslim kapsamlı bir giriş sağlar. Visual Studio Laboratuvar Yönetimi ve Visual Studio yayın Yönetimi kullanımı kapsar.
- [Tooling ALM Rangers DevOps ve kılavuz](https://aka.ms/vsarsolutions/). DevOps çalışma ekranı örnek yardımcı çözümü ve desenler işbirliğiyle pratik bir kılavuz ALM Rangers sunulan &amp; yöntemler kitap *TFS 2012 ile yayın işlem hattı oluşturma*, başlatmak için harika bir yolu olarak DevOps kavramlarını öğrenme &amp; yayın Yönetimi TFS 2012 için ve özelliklerini tetiklersiniz. Kılavuzu, bir kez oluşturmak ve dağıtmak için birden çok ortamı gösterilmektedir.
- [Visual Studio 2012 ile sürekli teslimat için test etme](https://msdn.microsoft.com/library/jj159345.aspx). E-kitap Microsoft Patterns and Practices, tarafından otomatikleştirilmiş testleri sürekli teslimat ile tümleştirmek nasıl açıklanmaktadır.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). (Bir etikete göre) TFS derleme yakalamak için tasarlanmış bir araç için kaynak kodu, yapı, paketi, belirli yönlerini yapılandırmak için DevOps rolünde izin vermek ve Azure'da anında. Aracı dağıtım işlemi "önceden dağıtılmış bir sürüme geri almak" işlemlerini etkinleştirmek için izler. Aracı dış bağımlılıkları yoktur ve tek başına TFS API'ları ve Azure SDK'sını kullanarak çalışabilir.
- [Kesintisiz teslim: Güvenilir yazılım güncelleştirmeleri derleme, Test ve dağıtım Otomasyon](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Defteri Jez Humble tarafından.
- [Serbest! Tasarım ve üretime hazır yazılım dağıtım](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Michael T. Nygard ile rehberi.

>[!div class="step-by-step"]
[Önceki](source-control.md)
[sonraki](web-development-best-practices.md)
