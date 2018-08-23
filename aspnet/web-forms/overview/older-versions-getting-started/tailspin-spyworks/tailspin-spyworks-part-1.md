---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: '1. Bölüm: Dosya -> Yeni Proje | Microsoft Docs'
author: JoeStagner
description: Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır. 1. Bölüm genel bakış ve dosya/yeni proje kapsar.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: d2e4b36c9029e86eea9b09974839e96e9aa39ced
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755917"
---
<a name="part-1-file--new-project"></a>1. Bölüm: Dosya -> Yeni Proje
====================
tarafından [ALi Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks .NET platformu için güçlü, ölçeklenebilir uygulamalar oluşturmak için nasıl çok basit olduğunu gösterir. Bu, alışveriş, kullanıma alma ve yönetim gibi bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.
> 
> Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır. 1. Bölüm genel bakış ve dosya/yeni proje kapsar.


## <a id="_Toc260221666"></a>  Genel bakış

Bu öğreticide, ASP.NET WebForms giriş niteliğindedir. Biz, yavaş başlatma, başlangıç düzeyi bir web geliştirme deneyimi için uygundur.

Biz oluşturuyor olacaksınız basit bir çevrimiçi mağaza uygulamasıdır.

![](tailspin-spyworks-part-1/_static/image1.jpg)


Ziyaretçi ürünleri kategoriye göre göz atabilirsiniz:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Tek bir ürün görüntülemek ve bunların sepetinize ekleyin:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Bunlar, artık istedikleri tüm öğeleri kaldırma, kendi Sepeti gözden geçirebilirsiniz:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Kullanıma alma için devam etmeden bunları ister.

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Sıralandıktan sonra bunlar basit bir onay ekranı görürsünüz:

![](tailspin-spyworks-part-1/_static/image7.jpg)


Biz Visual Studio 2010'da yeni bir ASP.NET WebForms projesi oluşturarak başlarsınız ve eksiksiz, çalışan bir uygulamayı oluşturmak için özellikler artımlı olarak ekleyeceğiz. Bu doğrultuda, biz veritabanı erişimi, liste ve kılavuz görünümleri, veri güncelleştirme sayfaları, veri doğrulama tutarlı sayfa düzeni, AJAX, doğrulama, kullanıcının üyelik ve daha fazla bilgi için ana sayfaları kullanarak ele alacağız.

Adım adım takip edebilirsiniz veya tamamlanmış uygulamayı karşıdan yükleme [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Visual Studio 2010 veya ücretsiz Visual Web Developer 2010'dan kullanabileceğiniz [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/). Uygulamayı oluşturmak için SQL Server veya ücretsiz SQL Server Express veritabanı barındırmak için kullanabilirsiniz.

## <a id="_Toc260221667"></a>  Dosya / yeni proje

Yeni Proje Visual Studio'da Dosya menüsü seçerek başlayacağız. Bu, yeni proje iletişim kutusunu açar.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Visual C# seçeneğini belirleyeceğiz / Web şablonları Grup solda ve ardından Orta sütundaki "ASP.NET Web uygulaması" şablonu seçin. TailspinSpyworks projenizi adlandırın ve Tamam düğmesine basın.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Bu, bizim projesi oluşturur. Uygulamamızı Çözüm Gezgini'nde sağ tarafta bulunan klasörleri bir göz atalım.

![](tailspin-spyworks-part-1/_static/image10.jpg)

Boş çözüm tamamen boş değildir – temel klasör yapısı ekler:

![](tailspin-spyworks-part-1/_static/image1.png)

ASP.NET 4 varsayılan proje şablonu tarafından uygulanan kuralları unutmayın.

- "Hesap" klasörü, ASP temel kullanıcı arabirimini uygular. NET üyeliği alt sistemi.
- İstemci tarafı JavaScript dosyaları için depo "Betikler" klasörüne görür ve çekirdek jQuery .js dosyaları varsayılan olarak sunulur.
- "Stilleri" klasörü, web sitesi görsellerimize (CSS stil sayfaları) düzenlemek için kullanılır

Biz default.aspx sayfayı oluşturmak ve uygulamamızı çalıştırmak için F5 tuşuna bastığınızda seçtiğimizde aşağıdaki seçenekleri görüyoruz.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Tailspin Spyworks uygulamamız için istediğimiz visual asthetics işlenir ilişkili görüntü dosyaları ve CSS sınıfları varsayılan WebForms şablondan Style.css dosyasını değiştirmek için ilk uygulama geliştirme olacaktır.

Bunu yaptıktan sonra default.aspx sayfamızı şu şekilde işler.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Üst görüntü bağlantılarının fark sağında sayfası ve ana sayfaya eklenen menü öğeleri. Yalnızca "Oturum açma" ve "Hesap" bağlantıları (varsayılan şablon tarafından oluşturulan) mevcut sayfalar ve kalan uygulamamız ekleriz gibi biz uygulayacak sayfalarında işaret.

Bunu ana sayfa stilleri dizine dışında yeniden konumlandırmakta dağıtacağız. Yalnızca bir tercih budur ancak biz uygulamamız "skinable" gelecekte yapmaya karar verirseniz şeyler biraz kolaylaştırabilir.

Biz ana sayfaya değiştirmeniz gerekir Bunu yaptıktan sonra tüm .aspx dosyalarını başvuruları ASP.NET WebForms sayfaları varsayılan olarak oluşturulur.

> [!div class="step-by-step"]
> [Next](tailspin-spyworks-part-2.md)
