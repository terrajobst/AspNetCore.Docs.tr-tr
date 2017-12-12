---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: "Bölüm 1: Dosya -> Yeni Proje | Microsoft Docs"
author: JoeStagner
description: "Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 1 genel bakış ve dosya/yeni proje kapsar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: bd840f9f3f5d723e6bc1bb35955a7770634e9483
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="part-1-file--new-project"></a>Bölüm 1: Dosya -> Yeni Proje
====================
tarafından [CAN Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks .NET platformu için güçlü, ölçeklendirilebilir uygulamalar oluşturun nasıl alıyoruz basit gerçekleştiğini gösterir. Alışveriş, kullanıma alma ve yönetim dahil olmak üzere bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.
> 
> Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 1 genel bakış ve dosya/yeni proje kapsar.


## <a id="_Toc260221666"></a>Genel bakış

Bu öğretici ASP.NET WebForms giriş ' dir. Biz yavaş başlatma, başlangıç düzeyi web geliştirme deneyimi için uygundur.

Biz oluşturma basit bir çevrimiçi mağaza uygulamasıdır.

![](tailspin-spyworks-part-1/_static/image1.jpg)


Ziyaretçilerin ürünleri kategoriye göre göz atabilirsiniz:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Tek bir ürün görüntüleyebilir ve bunların Sepete Ekle:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Bunlar artık istedikleri öğeleri kaldırma kendi Sepeti gözden geçirebilirsiniz:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Checkout izlemeye devam etmeden bunları ister

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Sıralandıktan sonra bunlar bir basit onay ekranı bakın:

![](tailspin-spyworks-part-1/_static/image7.jpg)


Visual Studio 2010'da yeni bir ASP.NET WebForms projesi oluşturarak başlamadan ve tam çalışan bir uygulamayı oluşturmak için özellikleri artımlı olarak ekleyeceğiz. Yol boyunca biz veritabanı erişimi, liste ve kılavuz görünümleri, veri güncelleştirme sayfaları, veri doğrulama tutarlı sayfa düzeni, AJAX, doğrulama, kullanıcı üyeliği ve daha fazla bilgi için ana sayfalar kullanarak ele alacağız.

Adım adım izleyebilirsiniz veya tamamlanmış uygulamadan indirebilirsiniz [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Visual Studio 2010 veya ücretsiz Visual Web Developer 2010'dan kullanabileceğiniz [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/). Uygulama oluşturmak için SQL Server veya ücretsiz SQL Server Express ana veritabanı için kullanabilirsiniz.

## <a id="_Toc260221667"></a>Dosya / yeni proje

Dosya menüsünde Visual Studio'da yeni proje seçerek başlayacağız. Yeni Proje iletişim kutusunu açar.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Biz Visual C# seçersiniz / Web şablonları grup sol tarafta ve ardından merkezi sütununda "ASP.NET Web uygulaması" şablonunu seçin. TailspinSpyworks projenizi adlandırın ve Tamam düğmesine basın.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Bu bizim projesi oluşturacaksınız. Sağ taraftaki Çözüm Gezgini'nde uygulamamız içinde yer alan klasörleri bir göz atalım.

![](tailspin-spyworks-part-1/_static/image10.jpg)

Boş çözüm tamamen boş olmayan – temel klasör yapısı ekler:

![](tailspin-spyworks-part-1/_static/image1.png)

ASP.NET 4 varsayılan proje şablonu tarafından uygulanan kuralları unutmayın.

- "Hesap" klasörü ASP için bir temel kullanıcı arabirimini uygular. NET'in üyelik alt sistemi.
- İstemci tarafı JavaScript dosyaları için depo "Scripts" klasörü görür ve çekirdek jQuery .js dosyaları varsayılan olarak kullanılabilir hale getirilir.
- "Stilleri" klasörü bizim web sitesi görselleri (CSS stil sayfaları) düzenlemek için kullanılır

Biz, biz uygulamamızı çalıştırmak ve default.aspx sayfasında işlemek için F5 tuşuna bastığınızda aşağıdakilere bakın.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Bizim ilk uygulama geliştirme, CSS sınıfları ve Tailspin Spyworks uygulamamız için istiyoruz visual asthetics kılacak ilişkili görüntü dosyaları varsayılan WebForms şablondan Style.css dosyasını değiştirmek için olacaktır.

Bunu yaptıktan sonra default.aspx sayfamızı şu şekilde işler.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Üst görüntü bağlantılar fark sağında sayfa ve ana sayfaya eklenen menü öğeleri. (Varsayılan şablon tarafından oluşturulan) mevcut sayfaları ve biz uygulamamız yapı olarak biz gerçekleştireceksiniz sayfaları geri kalanı için yalnızca "Oturum Aç" ve "Hesap" bağlantıları gelin.

Ayrıca ana sayfa stilleri dizinine taşıyabilir yapacağız. Bu yalnızca bir tercih olsa uygulamamız gelecekte "skinable" olun karar verirseniz şeyler biraz kolaylaştırabilir.

Ana sayfa değiştirmek için gerekir yaptıktan sonra tüm .aspx dosyalarını başvurularında ASP.NET WebForms sayfaları varsayılan olarak oluşturulur.

>[!div class="step-by-step"]
[Sonraki](tailspin-spyworks-part-2.md)
