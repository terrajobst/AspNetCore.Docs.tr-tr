---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: '1. Kısım: Genel bakış ve Yeni Proje -> Dosya | Microsoft Docs'
author: jongalloway
description: Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 1 kapak genel bakış ve Dosya -> Yeni proje.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2082927d18c95563893da199d60347fa15952446
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="part-1-overview-and-file-new-project"></a>1. Kısım: Genel bakış ve Yeni Proje -> Dosya
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.  
>   
> MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.  
>   
> Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 1 kapsayan genel bakış ve dosya -&gt;yeni proje.


## <a name="overview"></a>Genel Bakış

MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Web Developer web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır. Biz yavaş başlatma, başlangıç düzeyi web geliştirme deneyimi için uygundur.

Biz oluşturmakta uygulama basit müzik deposudur. Uygulamayı üç ana bölümü vardır: alışveriş, kullanıma alma ve yönetim.

![](mvc-music-store-part-1/_static/image1.jpg)

Ziyaretçilerin Tarz albümlerini göz atabilirsiniz:

![](mvc-music-store-part-1/_static/image2.jpg)

Tek bir albümü görüntüleyebilir ve bunların Sepete Ekle:

![](mvc-music-store-part-1/_static/image3.jpg)

Bunlar artık istedikleri öğeleri kaldırma kendi Sepeti gözden geçirebilirsiniz:

![](mvc-music-store-part-1/_static/image4.jpg)

Checkout geçmeden oturum açmak veya kaydetmek için bir kullanıcı hesabı için bunları ister.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Bir hesap oluşturduktan sonra bunların sırası aktarma ve ödeme bilgilerini doldurarak tamamlayabilirsiniz. Örneği basit tutmak için size harika bir yükseltme çalıştırıyorsanız: her şeyi promosyon kodu "Serbest" girerseniz ücretsizdir!

![](mvc-music-store-part-1/_static/image5.jpg)

Sıralandıktan sonra bunlar bir basit onay ekranı bakın:

![](mvc-music-store-part-1/_static/image6.jpg)

Müşteri faceing sayfalarına ek olarak Biz ayrıca, yöneticiler oluşturabilir, albümleri düzenleme, bir listesini gösterir bir yönetici bölümü oluşturmak ve albümleri silin:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. Dosya -&gt; yeni proje

### <a name="installing-the-software"></a>Yazılım yükleme

Bu öğretici ücretsiz Visual Web Developer 2010 (olan ücretsiz) Express kullanarak yeni bir ASP.NET MVC 3 projesi oluşturarak başlar ve ardından tam çalışan bir uygulamayı oluşturmak için özellikleri artımlı olarak ekleyeceğiz. Yol boyunca biz veritabanı erişimi, form nakil senaryoları, veri doğrulama, ana sayfalar için tutarlı sayfa düzeni, AJAX Sayfa güncelleştirmelerini ve doğrulama, kullanıcı oturum açma ve daha fazla bilgi için kullanarak ele alacağız.

Adım adım izleyebilirsiniz veya tamamlanmış uygulamadan indirebilirsiniz [MVC müzik deposu](https://github.com/evilDave/MVC-Music-Store).

Uygulamanızı oluşturmak için Visual Studio 2010 SP1 veya Visual Web Developer 2010 Express SP1 (Visual Studio 2010 'un boş bir sürüm) kullanabilirsiniz. Biz SQL Server Compact (de serbest) veritabanını barındırmak için kullanırsınız. Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun.


- [Visual Studio Web Developer Express SP1 Önkoşullar]
- [ASP.NET MVC 3 araçları güncelleştirme]
- [SQL Server Compact 4.0] - çalışma zamanı ve Araçlar desteği dahil olmak üzere


### <a name="creating-a-new-aspnet-mvc-3-project"></a>Yeni bir ASP.NET MVC 3 projesi oluşturma

Visual Web Developer Dosya menüsünden "Yeni Proje" seçerek başlayacağız. Yeni Proje iletişim kutusunu açar.

![](mvc-music-store-part-1/_static/image5.png)

Biz Visual C# - seçersiniz&gt; Web şablonları grup sol tarafta, sonra merkezi sütununda "ASP.NET MVC 3 Web uygulaması" şablonunu seçin. MvcMusicStore projenizi adlandırın ve Tamam düğmesine basın.

![](mvc-music-store-part-1/_static/image8.jpg)

Bu bizim proje için bazı MVC belirli ayarları hale getirmemize sağlayan ikincil bir iletişim kutusu görüntüler. Aşağıdakileri seçin:

Şablonu proje - boş seçin

Altyapısı görüntüleme - Razor seçin

HTML5 anlamsal biçimlendirme - işaretli kullanın

Ayarlarınızı aşağıda gösterildiği gibi ardından Tamam düğmesine basın doğrulayın.

![](mvc-music-store-part-1/_static/image9.jpg)

Bu bizim projesi oluşturacaksınız. Uygulamamız sağ tarafında Çözüm Gezgini'nde eklenmiş klasörleri bir göz atalım.

![](mvc-music-store-part-1/_static/image10.jpg)

Boş MVC 3 şablon tamamen boş değil – temel klasör yapısı ekler:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC klasör adları için bazı temel adlandırma kuralları kullanır:

| **Klasör** | **Amaç** |
| --- | --- |
| **/ Denetleyicileri** | Denetleyicileri yanıt tarayıcıdan giriş, kendisiyle yapın ve yanıtın kullanıcıya dönmek karar vermek için. |
| **/ Görünümleri** | UI şablonlarımız görünümleri tutun |
| **/ Modelleri** | Modelleri basılı tutun ve verileri işlemek |
| **/ İçeriği** | Bu klasör bizim görüntüleri, CSS ve diğer statik içeriği tutar |
| **/ Komut dosyaları** | Bu klasör bizim JavaScript dosyaları tutar |

ASP.NET MVC çerçevesi varsayılan olarak "kuralı yapılandırması üzerinden" bir yaklaşım kullanır ve klasör adlandırma kurallarına göre bazı varsayılan varsayımlar yapar çünkü bu klasörleri bile bir boş ASP.NET MVC uygulamasındaki dahil edilir. Örneğin, denetleyicileri görünümler klasöründe görünümlerinde varsayılan olarak bu kodunuzda açıkça belirtmek zorunda kalmadan arayın. Varsayılan kuralları ile kalmanız yazmak için gereken kod miktarını azaltır ve aynı zamanda, projenizin anlamak diğer geliştiriciler için kolaylaştırabilir. Bu kuralları biz uygulamamız yapı gibi daha fazla açıklayacağız.

> [!div class="step-by-step"]
> [Next](mvc-music-store-part-2.md)
