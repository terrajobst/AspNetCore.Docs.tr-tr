---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: "Yeni bir ASP.NET MVC projesi oluşturun | Microsoft Docs"
author: microsoft
description: "1. adım temel NerdDinner uygulama yapısı yerleştirdiniz gösterilmiştir."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 4d30a6803b1478014a2afb814ac317df27394446
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
<a name="create-a-new-aspnet-mvc-project"></a>Yeni bir ASP.NET MVC projesi oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Adım 1 / ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , yetenekte küçük bir yapı ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulamasına nasıl aracılığıyla.
> 
> 1. adım temel NerdDinner uygulama yapısı yerleştirdiniz gösterilmiştir.
> 
> ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner adım 1: Dosya -&gt;yeni proje

Biz seçerek NerdDinner uygulamamız başlarsınız **dosya -&gt;yeni proje** menü öğesi içinde Visual Studio 2008 veya ücretsiz Visual Web Developer 2008 Express.

Bu, "Yeni Proje" iletişim kutusunu getirir. Yeni bir ASP.NET MVC uygulaması oluşturmak için iletişim kutusunun sol taraftaki "Web" düğümünü seçin ve ardından sağdaki "ASP.NET MVC Web uygulaması" proje şablonu seçin:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Önemli: indirilir ve ASP.NET yeni proje iletişim kutusunda görüntülenmeyecektir MVC - Aksi takdirde yüklü olduğundan emin olun. V2 olarak kullanabileceğiniz [Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx) , onu henüz yüklemediyseniz (ASP.NET MVC içinde kullanılabilir "Web Platformu -&gt;çerçeveler ve çalışma zamanları" bölümü).*

Biz, biz "NerdDinner" oluşturabilir ve oluşturmak için "Tamam" düğmesini tıklatın olacak yeni proje adı.

Biz "Tamam" düğmesini tıklatın, Visual Studio ister bize isteğe bağlı olarak da yeni uygulama için birim testi projesi oluşturmak için ek bir iletişim kutusu getirir. Bu birim testi projesi bizi uygulamamız davranışını ve işlevlerinin doğrulayın otomatikleştirilmiş test oluşturmak sağlar (bir şey kapak nasıl daha sonra Bu öğreticide Yapılacaklar).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

Yukarıdaki iletişim kutusunda "Test framework" açılır makinede yüklü tüm kullanılabilir ASP.NET MVC birim testi projesi şablonları ile doldurulur. Sürümleri NUnit, MBUnit ve XUnit indirilebilir. Yerleşik Visual Studio birim testi çerçevesi da desteklenir.

*Not: Visual Studio Birim Test Çerçevesi yalnızca Visual Studio 2008 Professional ve daha sonraki sürümler ile kullanılabilir. VS 2008 Standard Edition veya Visual Web Developer 2008 Express kullanıyorsanız yükleyip NUnit, MBUnit veya XUnit uzantıları için ASP.NET MVC gösterilmesi için bu iletişim kutusu için sırayla gerekecektir. Yüklü herhangi bir test çerçeve yoksa iletişim kutusunda görüntülenmez.*

Biz oluşturuyoruz test projesi için varsayılan "NerdDinner.Tests" adı kullanın ve "Visual Studio Birim Test" framework seçeneğini kullanın. Biz tıklattığınızda "Tamam" düğmesine Visual Studio çözüm bize bu - web uygulamamız için diğeri bizim birim testleri için iki proje ile oluşturacak:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>NerdDinner dizin yapısını inceleniyor

Visual Studio ile yeni bir ASP.NET MVC uygulaması oluşturduğunuzda, projeye otomatik olarak bir dizi dosyaları ve dizinleri ekler:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

Varsayılan olarak ASP.NET MVC projeleri altı en üst düzey dizinleri sahiptir:

| **Dizin** | **Amaç** |
| --- | --- |
| **/ Denetleyicileri** | Burada URL isteklerini işleyen denetleyici sınıfları yerleştirme |
| **/ Modelleri** | Burada temsil eder ve verileri işlemek sınıfları yerleştirme |
| **/ Görünümleri** | Burada işleme çıktısı için sorumlu kullanıcı Arabirimi şablon dosyalarını yerleştirme |
| **/ Komut dosyaları** | Burada JavaScript kitaplığı dosyaları ve komut dosyaları (.js) yerleştirin |
| **/ İçeriği** | Burada, CSS ve görüntü dosyaları ve diğer olmayan-dinamik/olmayan-JavaScript içeriği yerleştirme |
| **/ Uygulama\_veri** | Veri dosyalarını depolamak burada okuma/yazma istiyorsunuz. |

ASP.NET MVC bu yapı gerektirmez. Aslında, büyük uygulamalar üzerinde çalışan geliştiriciler genellikle uygulama yukarı daha kolay yönetilebilmesi için birden çok projede bölüm (örneğin: veri modeli sınıflarını genellikle web uygulamasından ayrı sınıf kitaplığında Git). Varsayılan proje yapısı, ancak bizim uygulama sorunları temiz tutma kullanırız iyi varsayılan dizin kuralını sağlar.

Biz /Controllers dizin genişlettiğinizde Biz Visual Studio varsayılan olarak iki denetleyicisi sınıf – HomeController ve AccountController – projeye eklenen bulabilirsiniz:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Biz /Views dizin genişlettiğinizde, biz üç alt dizinleri – /Home, ApplicationTier/account ve /Shared – yanı sıra birkaç şablonu içindeki dosyalara Ayrıca varsayılan projeye eklenen bulabilirsiniz:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Biz/scripts dizinler ve/Content genişlettiğinizde, biz ASP.NET AJAX ve jQuery etkinleştirebilirsiniz JavaScript kitaplıklarını yanı sıra tüm HTML sitesinde stilini belirlemek için kullanılan bir Site.css dosya uygulama içerisinden destek bulabilirsiniz:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Biz NerdDinner.Tests proje genişlettiğinizde biz bizim denetleyicisi sınıfları için birim testleri içeren iki sınıf bulabilirsiniz:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Visual Studio tarafından eklenen bu varsayılan dosyalar, bize çalışan bir uygulama - sayfasında, hesap oturum açma/oturum kapatma/kayıt sayfaları ve bir işlenmemiş bir hata sayfası (tüm kablolu yukarı ve kutunun dışında çalışma) hakkında giriş sayfası için basit bir yapı ile sağlayın.

### <a name="running-the-nerddinner-application"></a>NerdDinner uygulamayı çalıştırma

Biz ya da seçerek proje çalıştırabilirsiniz **hata ayıklama -&gt;hata ayıklamayı Başlat** veya **hata ayıklama -&gt;hata ayıklama olmadan Başlat** menü öğeleri:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Bu yerleşik ASP.NET Web-Visual Studio ile birlikte gelen sunucusunu başlatmaya ve uygulamamızı çalıştırın:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Yeni Projemizin giriş sayfasını aşağıdadır (URL: "/") çalıştığında:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

"Hakkında" sekmesini tıklatarak görüntüleyen bir sayfa hakkında (URL: "/ ev/hakkında"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Sağ üst tarafında "Oturum Aç" bağlantısını tıklatarak alır bize bir oturum açma sayfasına (URL: "/ Account/oturum açma")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Biz kayıt bağlantıyı tıklatabilir bir oturum açma hesabı yoksa (URL: "/ Account/Register") oluşturmak için:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Oturum kapatma ve hakkında yukarıdaki Giriş uygulamak için kodu Register işlevselliği, biz bizim yeni bir proje oluşturduğunuzda varsayılan olarak eklendi. Uygulama başlangıç noktası olarak bunu kullanacağız.

### <a name="testing-the-nerddinner-application"></a>NerdDinner uygulamayı test etme

Biz Professional Edition veya Visual Studio 2008'in daha yüksek sürümünü kullanıyorsanız, biz IDE desteği Visual Studio'dan Test yerleşik birimi projeyi test etmek için kullanabilirsiniz:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Yukarıdaki seçeneklerden birini seçerek IDE içinde "Test sonuçlarını" bölmesini açın ve bize yerleşik işlevselliği kapak 27 birim testlerini bizim yeni projeye dahil geçişi/başarısız durumuna sağlar:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Daha sonra Bu öğreticide biz otomatikleştirilmiş testleri hakkında daha fazla konuşun ve biz uygulamak uygulama işlevsellikler içeren ek birim testleri ekleme.

### <a name="next-step"></a>Sonraki adım

Temel uygulama yapısı artık yerinde açıyor. Şimdi, şimdi [bizim uygulama verilerini depolamak için bir veritabanı oluşturun](create-a-database.md).

>[!div class="step-by-step"]
[Önceki](introducing-the-nerddinner-tutorial.md)
[sonraki](create-a-database.md)
