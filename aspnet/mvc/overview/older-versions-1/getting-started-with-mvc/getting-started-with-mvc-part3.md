---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: "Bir görünümü ekleme | Microsoft Docs"
author: shanselman
description: "ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 509dd301eef7c00431eae194a0df69d70e6d80f8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view"></a>Bir görünümü ekleme
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

> ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız. Ziyaret [ASP.NET MVC öğrenme Merkezi](../../../index.md) diğer ASP.NET MVC öğreticiler ve örnekleri bulunamadı.


Bu bölümde biz nasıl biz düzgün bir şekilde bir istemciye oluşturma HTML yanıtları kapsülleyen bir görünüm şablon dosyası kullanmak bizim HelloWorldController sınıfı olabilir aramak için adımıdır.

Bizim dizin yöntemi ile bir görünüm şablonu kullanarak başlayalım. Bizim yöntemi dizin adı verilir ve bu HelloWorldController. Şu anda bizim İNDİS() yöntemi Controller sınıfı içinde kodlanmış olduğunu belirten bir ileti ile bir dize döndürür.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Şimdi bunun yerine şuna için dizin yöntemi değiştirelim:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Şimdi bir görünüm şablonu bizim İNDİS() yöntemi için kullanırız bizim projesine ekleyelim. Bunu yapmak için dizin yöntemi ortasında bir yerde fareyi sağ tıklatıp Görünüm Ekle...

![görüntü](getting-started-with-mvc-part3/_static/image1.png)

Bu, bize nasıl bizim dizin yöntemi tarafından kullanılan bir görünüm şablonu oluşturmak istiyoruz için bazı seçenekler sağlayan "Görünümü Ekle" iletişim kutusunu getirir. Şimdilik, yoksa değişikliği ve Ekle düğmesine tıklamanız yeterlidir.

[![Görünüm Ekle iletişim kutusu](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Ekle tıkladıktan sonra yeni bir klasör ve yeni bir dosya çözüm klasöründe, burada görüldüğü gibi görünür. Şimdi bir HelloWorld klasör görünümleri ve Index.aspx dosyası altında bu klasöre sahibim.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

Yeni dizin dosyası da zaten açılmış ve düzenleme için hazır. İlk altında bazı metin eklemek &lt;h2&gt;dizin&lt;H2&gt; ister "Hello World."

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Uygulamanızı çalıştırın ve ziyaret [ `http://localhost:xx/HelloWorld` ](http://localhostxx) tarayıcınızda yeniden. Bu örnekte, denetleyici dizin yönteminde işlerinizi yapmak alamadık, ancak ", biz istemcisine geri yanıt işlemek için bir görünüm şablon dosyası kullanmak istediğinizi gösterilen dönüş View()" çağrısı. Kullanılacak görünüm şablonu dosyasının adı biz açıkça belirtmediğinden, ASP.NET MVC \Views\HelloWorld klasör içinde Index.aspx görünüm dosyası kullanarak varsayılan. Şimdi biz bizim görünümünde kodlanmış dize bakın.

[![Index - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Oldukça iyi görünür. Ancak, "Index" tarayıcının başlık diyor ve büyük başlık sayfasında "MVC Uygulamam." diyor dikkat edin Bu değiştirelim.

### <a name="changing-views-and-master-pages"></a>Görünümleri ve ana sayfalar değiştirme

İlk olarak, "MVC Uygulamam." metin değiştirelim Bu metin paylaşılır ve her sayfada görüntülenir. Bizim uygulamasında her sayfada olsa bile gerçekte bizim kodda, yalnızca tek bir yerde görünür. Çözüm Gezgini'nde /Views/Shared klasörüne gidin ve Site.Master dosyasını açın. Bu dosyayı bir ana sayfa adı verilir ve "bizim diğer sayfalarını kullanma paylaşılan Kabuğu" olur.

ContentPlaceholder "MainContent" diyor bazı metinleri bu dosyada dikkat edin.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Burada oluşturduğunuz tüm sayfaları, "ana sayfa Sarmalanan" gösterecektir bu tutucudur. Uygulamanızı çalıştırmak ve birden çok sayfaları ziyaret başlık değiştirmeyi deneyin. Bir değişiklik birden çok sayfada göründüğünü fark edeceksiniz.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Her sayfada birincil başlık - sahip artık H1 - "Benim MVC film uygulaması." olmasıdır. Tüm sayfalar arasında paylaşılan üst var. beyaz metin işler.

Bizim değiştirilen başlıkla bütünüyle Site.Master şöyledir:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Şimdi, dizin sayfasının başlığını değiştirelim.

/HelloWorld/Index.aspx açın. Değiştirmek için iki yerde yoktur. İlk olarak, başlık tarayıcı, daha sonra H2 - da ikincil üstbilgisini - başlığında görüntülenir. Bunların her hangi bit kod görebilmeniz için biraz farklı uygulama hangi kısmına değişiklikler yapacağız.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Uygulamanızı çalıştırın ve /Movies ziyaret edin. Tarayıcı başlığı, birincil başlık ve ikincil başlıklar değişmiş dikkat edin. Büyük küçük değişikliklerle birlikte, uygulamanızda görünümünüze değişiklik kolaydır.

[![Film listesi - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Bizim az bitlik (Bu durumda "Hello World!" "veri" ileti), ancak kodlanmış sabit. Biz V (görünümler) aldınız ve biz C (denetleyicileri), ancak henüz hiçbir M (modeli) olduğuna. Size kısa süre içinde nasıl adım geçireceğiz bir veritabanı oluşturun ve model verileri alabilirsiniz.

## <a name="passing-a-viewmodel"></a>Bir ViewModel geçirme

Bir veritabanına gidin ve modelleri hakkında konuşun önce ancak şimdi ilk "ViewModels" hakkında konuşun Bunlar bir görünüm şablonu bir istemciye bir HTML yanıtı işlemek için gerektirir temsil eden nesnelerdir. Bunlar genellikle oluşturulur ve bir görünüm şablonu için bir denetleyici sınıfı tarafından geçirilen ve görünüm şablonu gerektirir - veri ve artık yalnızca içermelidir.

Daha önce bizim HelloWorld örnek bizim Welcome() eylem yöntemi bir ad ve bir numTimes parametre sürdü ve tarayıcıya çıktı. Bu yanıt doğrudan işlemeye devam denetleyiciniz yerine, bunun yerine bu verileri tutun ve ardından bu geri kullanmadan HTML yanıtı işlemek için bir görünüm şablonu geçirin için küçük bir sınıf olalım. Denetleyici tek şey ve görünüm şablonu ile ilgili bu şekilde başka – bize temiz "ayrılması sorunları" uygulamamız içinde korumak etkinleştirme.

HelloWorldController.cs dosyaya dönmek ve yeni bir "WelcomeViewModel" sınıf ekleyin ve Hoş Geldiniz yönteminin denetleyicinizi içinde değiştirin. Aynı dosyada yeni sınıf ile tam HelloWorldController.cs aşağıdadır.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Birden çok satırda olmasına rağmen bizim Hoş Geldiniz gerçekten yalnızca iki kod deyimleri yöntemidir. İlk ifade ViewModel nesnesi ve ikinci geçişleri iki bizim parametrelerini görünümü elde edilen nesneye paketler.

Şimdi bir Hoş Geldiniz görünüm şablonu gerekiyor! Hoş Geldiniz yönteminde sağ tıklatın ve eklemek görünümü seçin. Bu süre, biz "Kesin türü belirtilmiş Görünüm Oluştur" denetleyin ve açılır listeden bizim WelcomeViewModel sınıfı seçin. Bu yeni görünüm yalnızca WelcomeViewModels ve diğer hiçbir nesne türlerini hakkında bilirsiniz.

> *Not: Bir kez, WelcomeViewModel için aşağı açılan listeden görünmesine ekledikten sonra derlenmiş gerekir.*


İşte, Görünüm Ekle iletişim kutusu aşağıdaki gibi görünmelidir. Ekle düğmesine tıklayın. ![Görünüm yuvarlak içine alınmıştır ekleme](getting-started-with-mvc-part3/_static/image10.png)

Bu kodu altında ekleyin &lt;h2&gt; yeni Welcome.aspx içinde. Biz döngü yapmak ve sayıda olarak size gereken kullanıcı diyor Merhaba deyin!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Ayrıca, biz bu görünümü WelcomeViewModel hakkında söylediyse olduğundan, yazarken dikkat edin (Evli, unutmayın?) biz her zaman, biz başvuru bizim Model nesnesi ekran görüntüsü görülen olarak yararlı IntelliSense alın:

[![NumTime kaynak kodu](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Uygulamanızı çalıştırın ve ziyaret `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` yeniden. Veri URL'den yönlendiriyoruz artık bizim Denetleyicisi'nde otomatik olarak geçirilir, Denetleyicimizin bir ViewModel verileri paketleri ve söz konusu nesne bizim görünüm üzerine geçirir. Verileri HTML olarak kullanıcıya görüntüler daha görüntüleyin.

[![Hoş Geldiniz - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

De, bir Model için bir "M" tür ancak veritabanı türü oluştu. Ne biz öğrendiğinize ve film bir veritabanı oluşturmak atalım.

>[!div class="step-by-step"]
[Önceki](getting-started-with-mvc-part2.md)
[sonraki](getting-started-with-mvc-part4.md)
