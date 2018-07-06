---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Yeni bir ASP.NET MVC projesi oluşturun | Microsoft Docs
author: microsoft
description: 1. adım yerleştirdiniz temel NerdDinner uygulama yapısı gösterilmektedir.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 88ef850503725cc57c92a11952729b4bd2205a69
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835100"
---
<a name="create-a-new-aspnet-mvc-project"></a>Yeni bir ASP.NET MVC projesi oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Adım 1 / ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , Yürüyüşü nasıl küçük bir derleme, ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulaması aracılığıyla.
> 
> 1. adım yerleştirdiniz temel NerdDinner uygulama yapısı gösterilmektedir.
> 
> ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner adım 1: Dosya -&gt;yeni proje

Biz seçerek NerdDinner uygulamamız başlarsınız **File -&gt;yeni proje** Visual Studio 2008 veya ücretsiz Visual Web Developer 2008 Express menü öğesi.

Bu, "Yeni Proje" iletişim kutusu getirir. Yeni bir ASP.NET MVC uygulaması oluşturmak için biz iletişim kutusunun sol taraftaki "Web" düğümü seçin ve ardından sağdaki "ASP.NET MVC Web uygulaması" proje şablonu seçin:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Önemli: indirilen ve ASP.NET yeni proje iletişim kutusunda görünmez MVC - aksi yüklü olduğundan emin olun. V2, kullanabileceğiniz [Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx) henüz yüklemediyseniz, (ASP.NET MVC, içinde kullanılabilir "Web Platformu -&gt;çerçeveleri ve çalışma zamanları" bölümü).*

Biz "NerdDinner" oluşturup oluşturmak için "Tamam" düğmesini tıklatıp kullanacağız yeni proje adı.

Biz "Tamam" düğmesine tıklayın, Visual Studio ister bize isteğe bağlı olarak birim testi projesi için de yeni uygulama oluşturmak için ek bir iletişim kutusu çıkarır. Bu birim testi projesi uygulamamız davranışını ve işlevsellik doğrulamak otomatik testler oluşturma sağlıyor (bir şey ele nasıl Bu öğreticinin sonraki adımlarında Yapılacaklar).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

"Test Çerçevesi" açılan yukarıdaki iletişim kutusunda, makinede yüklü tüm kullanılabilir ASP.NET MVC birim test projesi şablonları ile doldurulur. NUnit, MBUnit ve XUnit sürümleri indirilebilir. Yerleşik Visual Studio birim testi çerçevesi de desteklenir.

*Not: Visual Studio birim testi çerçevesi yalnızca Visual Studio 2008 Professional ve üzeri sürümlerde kullanılabilir. VS 2008 Standard Edition veya Visual Web Developer 2008 Express kullanıyorsanız indirip sırada gösterilecek bu iletişim için ASP.NET MVC NUnit, MBUnit veya XUnit uzantıları yüklemeniz gerekir. Tüm test çerçeveleri yüklü değilse, iletişim kutusu görüntülenmez.*

Biz oluştururuz test projesi için varsayılan "NerdDinner.Tests" adını kullanın ve "Visual Studio birim testi" framework seçeneğini kullanın. Biz tıkladığınızda Visual Studio "Tamam" düğmesine bir çözüm bizim için - bir web uygulamamız için ve bir birim testlerimiz için iki proje oluşturur:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>NerdDinner dizin yapısını İnceleme

Visual Studio ile yeni bir ASP.NET MVC uygulaması oluşturduğunuzda, projeye otomatik olarak bir dizi dosyaları ve dizinleri ekler:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

ASP.NET MVC projeleri varsayılan olarak altı en üst düzey dizinleriniz bulunmaktadır:

| **Dizin** | **Amaç** |
| --- | --- |
| **/ Denetleyicileri** | URL isteklerini işleyen denetleyici sınıflarına yerleştirdiğiniz yere |
| **/ Modelleri** | Verileri işlemek ve temsil eden sınıflar yerleştirdiğiniz yere |
| **/ Görünümler** | Oluşturma çıkışı için sorumlu kullanıcı Arabirimi şablon dosyaları yerleştirdiğiniz burada |
| **/ Komut dosyaları** | JavaScript kitaplığı dosyaları ve komut dosyaları (.js) yerleştirdiğiniz yere |
| **/ İçerik** | Burada, CSS ve resim dosyaları ve diğer olmayan-dinamik/olmayan-JavaScript içeriği yerleştirin |
| **/ Uygulama\_veri** | Burada veri dosyalarını depolamak okuma/yazma istersiniz. |

ASP.NET MVC, bu yapı gerektirmez. Aslında, büyük uygulamalar üzerinde çalışan geliştiriciler genellikle uygulamayı oluşturan daha kolay yönetilebilir hale getirmek için birden çok projede bölüm (örneğin: veri modeli sınıfları genellikle ayrı bir sınıf kitaplığı projesinde bir web uygulamasından gidin). Varsayılan proje yapısı, ancak uygulama önde gelen kaygılarımızdan düzenli tutmak için kullanabileceğiniz bir iyi varsayılan dizin kuralı sunar.

Biz /Controllers dizini genişlettiğinizde Biz Visual Studio varsayılan olarak iki denetleyici sınıflarına – HomeController ve AccountController – projeye eklenen bulabilirsiniz:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Biz /Views dizini genişlettiğinizde, biz üç alt dizinleri – /Home/Account ve /Shared – yanı, varsayılan olarak içindeki dosyalara da projeye eklenen birkaç şablon bulabilirsiniz:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Biz/scripts dizinleri ve/Content genişlettiğinizde, biz olanak veren ASP.NET AJAX ve jQuery JavaScript kitaplıklarını yanı sıra tüm HTML sitesinde biçimlendirmek için kullanılan bir Site.css dosyası içinde uygulama destek bulacaksınız:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Biz NerdDinner.Tests proje genişlettiğinizde biz bizim denetleyici sınıflarına için birim testleri içeren iki sınıfınız bulabilirsiniz:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Visual Studio tarafından eklenen bu varsayılan dosyalar, bize için çalışan bir uygulama - ile giriş sayfası, sayfayı, hesap oturum açma/kapatma/kayıt sayfaları ve bir işlenmeyen bir hata sayfası (tüm kablolu yukarı ve yepyeni çalışma) hakkında temel bir yapı sağlar.

### <a name="running-the-nerddinner-application"></a>NerdDinner uygulamayı çalıştırma

Biz ya da seçerek proje çalıştırabilirsiniz **hata ayıklama -&gt;hata ayıklamayı Başlat** veya **hata ayıklama -&gt;hata ayıklama olmadan Başlat** menü öğeleri:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Bu yerleşik ASP.NET Web-Visual Studio ile birlikte gelen sunucusunu başlatmak ve uygulamamızı çalıştırın:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Giriş sayfasına yeni Projemizin aşağıda verilmiştir (URL: "/") çalıştığında:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

"Hakkında" sekmesini tıklatarak görüntüleyen bir sayfa hakkında (URL: "/ Home/About"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Sağ üst kısmında "Oturum Aç" bağlantısına tıklayarak alan bize bir oturum açma sayfasına (URL: "/ hesabı/oturum açma")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Biz Kaydet bağlantısına tıklayabilirsiniz bir oturum açma hesabı yoksa (URL: "/ hesabı/Register") oluşturmak için:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Oturum kapatma ve, yukarıdaki Giriş uygulamak için kod / register işlevleri size sunduğumuz yeni proje oluşturulduğunda varsayılan olarak eklenmiştir. Uygulamamızı başlangıç noktası olarak kullanacağız.

### <a name="testing-the-nerddinner-application"></a>NerdDinner uygulamayı test etme

Professional sürümü veya üzeri bir sürüm Visual Studio 2008'in kullanıyoruz, yerleşik birim testi Visual Studio içinde IDE desteği projeyi test etmek için kullanabiliriz:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Yukarıdaki seçeneklerden birini seçerek IDE içinde "Test sonuçlarını" bölmesini açın ve yerleşik işlevi kapsayacak yeni Projemizin dahil 27 birim testleri geçer/başarısız durumuna sahip sağlayın:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Bu öğreticinin ilerleyen bölümlerinde biz otomatik testi hakkında daha fazla konuşacak ve biz uygulamak uygulama işlevsellikler ek bir birim testleri ekleme.

### <a name="next-step"></a>Sonraki adım

Temel uygulama yapısı artık yerinde açıyor. Haydi şimdi [bizim uygulama verilerini depolamak için bir veritabanı oluşturma](create-a-database.md).

> [!div class="step-by-step"]
> [Önceki](introducing-the-nerddinner-tutorial.md)
> [İleri](create-a-database.md)
