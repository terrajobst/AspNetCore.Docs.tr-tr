---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: '1. Bölüm: Genel bakış ve Dosya -> Yeni Proje | Microsoft Docs'
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. Bölüm 1 kapak genel bakış ve Dosya -> Yeni proje.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 57856a4a78a650e4abe872004e5be5f8f3b2dbcd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816349"
---
<a name="part-1-overview-and-file-new-project"></a>1. Bölüm: Genel bakış ve Dosya -> Yeni Proje
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.  
>   
> MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.  
>   
> Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 1. Bölüm kapsayan genel bakış ve dosya -&gt;yeni bir proje.


## <a name="overview"></a>Genel Bakış

MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Web Developer web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır. Biz, yavaş başlatma, başlangıç düzeyi bir web geliştirme deneyimi için uygundur.

Biz oluşturuyor olacaksınız basit müzik deposu uygulamasıdır. Uygulamayı üç ana bölümü vardır: alışveriş, kullanıma alma ve yönetim.

![](mvc-music-store-part-1/_static/image1.jpg)

Ziyaretçi Tarz albümlerini göz atabilirsiniz:

![](mvc-music-store-part-1/_static/image2.jpg)

Tek bir albümü görüntülemek ve bunların sepetinize ekleyin:

![](mvc-music-store-part-1/_static/image3.jpg)

Bunlar, artık istedikleri tüm öğeleri kaldırma, kendi Sepeti gözden geçirebilirsiniz:

![](mvc-music-store-part-1/_static/image4.jpg)

Kullanıma alma için devam etmeden bunları bir kullanıcı hesabına kaydolun veya oturum açma için ister.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Bir hesap oluşturduktan sonra bunların sırası sevkıyat ve ödeme bilgilerini doldurarak tamamlayabilirsiniz. Örneği basit tutmak için biz harika bir yükseltme çalıştırıyorsanız: her şeyi promosyon kodunu "Ücretsiz" olacaklardır ücretsizdir!

![](mvc-music-store-part-1/_static/image5.jpg)

Sıralandıktan sonra bunlar basit bir onay ekranı görürsünüz:

![](mvc-music-store-part-1/_static/image6.jpg)

Ayrıca müşteri faceing sayfalarına ek olarak albümleri, yöneticiler oluşturabilir, düzenleme, listesini gösteren bir yönetici bölümü oluşturun ve albümleri Sil:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. Dosya -&gt; yeni proje

### <a name="installing-the-software"></a>Yazılım yükleme

Bu öğreticide, ücretsiz Visual Web Developer 2010 (ücretsiz olan) Express kullanarak yeni bir ASP.NET MVC 3 projesi oluşturarak başlar ve ardından eksiksiz, çalışan bir uygulamayı oluşturmak için özellikler artımlı olarak ekleyeceğiz. Ana sayfalar tutarlı sayfa düzeni için AJAX Sayfa güncelleştirmelerini ve doğrulama, kullanıcı oturum açma ve daha fazlasını kullanarak, süreç boyunca size veritabanı erişimi, formu posta senaryolar ve veri doğrulama değineceğiz.

Adım adım takip edebilirsiniz veya tamamlanmış uygulamayı karşıdan yükleyebileceğiniz [MVC müzik Store](https://github.com/evilDave/MVC-Music-Store).

Uygulamayı oluşturmak için Visual Studio 2010 SP1 veya Visual Web Developer 2010 Express SP1 (Visual Studio 2010 ücretsiz bir sürümü) kullanabilirsiniz. SQL Server Compact (da ücretsiz) veritabanını barındırmak için kullanacağız. Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun.


- [Visual Studio Web Developer Express SP1 Önkoşullar]
- [ASP.NET MVC 3 araçları güncelleştirme]
- [SQL Server Compact 4.0] - hem çalışma zamanı ve araçları desteği dahil olmak üzere


### <a name="creating-a-new-aspnet-mvc-3-project"></a>Yeni bir ASP.NET MVC 3 projesi oluşturma

Visual Web Developer Dosya menüsünden "Yeni Proje" seçerek başlayacağız. Bu, yeni proje iletişim kutusunu açar.

![](mvc-music-store-part-1/_static/image5.png)

Visual C# - seçeneğini belirleyeceğiz&gt; Web şablonları grup sol tarafta, ardından Orta sütunda "ASP.NET MVC 3 Web uygulaması" şablonu seçin. MvcMusicStore projenizi adlandırın ve Tamam düğmesine basın.

![](mvc-music-store-part-1/_static/image8.jpg)

Bu MVC belirli ayarlara Projemizin olmak bize izin veren bir ikincil bir iletişim kutusu görüntüler. Aşağıdakileri seçin:

Şablon proje - boş seçin

Altyapısı görüntüleme - Razor seçin

Kontrol anlam biçimlendirme HTML5 - kullanın

Ayarlarınızı aşağıda gösterildiği gibi ardından Tamam düğmesine basın doğrulayın.

![](mvc-music-store-part-1/_static/image9.jpg)

Bu, bizim projesi oluşturur. Çözüm Gezgini'nde sağ tarafındaki uygulamamız eklenmiş klasörleri bir göz atalım.

![](mvc-music-store-part-1/_static/image10.jpg)

Boş MVC 3 şablonu tamamen boş değildir – temel klasör yapısı ekler:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC klasör adları için bazı temel adlandırma kurallarını kullanır:

| **Klasör** | **Amaç** |
| --- | --- |
| **/ Denetleyicileri** | Denetleyicileri yanıt tarayıcıdan giriş, ne ile bunu ve kullanıcıya bir yanıt döndürür. |
| **/ Görünümler** | UI şablonlarımızı görünümleri tutun |
| **/ Modelleri** | Modelleri basılı tutun ve verilerini işleme |
| **/ İçerik** | Bu klasör görüntülerimizi, CSS ve diğer statik içeriklerdeki tutar. |
| **/ Komut dosyaları** | Bu klasör tutan sunduğumuz JavaScript dosyaları |

ASP.NET MVC çerçevesi varsayılan olarak "kuralı yapılandırmanız üzerinde" bir yaklaşım kullanır ve klasör adlandırma kurallarına dayalı bazı varsayılan varsayımlarda bulunur çünkü bu klasörleri bile bir boş ASP.NET MVC uygulamasındaki dahil edilir. Örneği için denetleyicileri görünümleri görünümleri klasöründe varsayılan olarak bu kodunuzda açıkça belirtmeniz gerek kalmadan arayın. Varsayılan kuralları ile kalmanız yazmak için gereken kod miktarını azaltır ve ayrıca, projenizi anlamak diğer geliştiriciler için kolaylaştırabilir. Bu kuralları uygulamamız ekleriz gibi daha fazla açıklayacağız.

> [!div class="step-by-step"]
> [Next](mvc-music-store-part-2.md)
