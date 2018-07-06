---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Görünüm ekleme | Microsoft Docs
author: shanselman
description: ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: e3b923aa5572781261c5a75546700faf223703d4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834510"
---
<a name="adding-a-view"></a>Görünüm ekleme
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

> ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız. Ziyaret [ASP.NET MVC eğitim Merkezi](../../../index.md) diğer ASP.NET MVC, öğreticilerimiz ve örneklerimizden bulunacak.


Bu bölümde nasıl biz düzgün bir şekilde oluşturma HTML yanıtları istemciye kapsüllemek için bir görünüm şablon dosyası kullanmak bizim HelloWorldController sınıfına sahip olabilir aramak için kullanacağız.

Bizim dizin yöntemi ile bir şablonu görüntüleme kullanarak başlayalım. Index bizim yöntemi çağrılır ve HelloWorldController içinde olur. Şu anda bizim İNDİS() yöntemi Controller sınıfı içinde kodlanmış olduğunu belirten bir ileti ile bir dize döndürür.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Şimdi bunun yerine şuna benzeyecektir dizin yöntemi değiştirelim:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Artık bir şablonu görüntüleme için bizim İNDİS() yöntemi kullanabiliriz Projemizin ekleyelim. Bunu yapmak için dizin yöntemi ortasında yere farenizle sağ tıklatıp Görünüm Ekle...

![görüntü](getting-started-with-mvc-part3/_static/image1.png)

Bu bize nasıl bizim dizin yöntemi tarafından kullanılan bir görünüm şablonu oluşturmak istediğimiz bazı seçenekler sağlayan "Görünüm Ekle" iletişim kutusu çıkarır. Şimdilik, yoksa herhangi bir ayarı değiştirmek ve Ekle düğmesine tıklamanız yeterlidir.

[![Görünüm Ekle iletişim kutusu](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Ekle'ye tıklayın, sonra yeni bir klasör ve yeni bir dosya çözüm klasöründe, burada görüldüğü gibi görünür. Artık bir HelloWorld klasörü altında görünümleri ve Index.aspx dosyası bu klasörün içinde sahibim.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

Yeni dizin dosyası ayrıca zaten açık ve düzenlemek için hazır. İlk metin ekleyin &lt;h2&gt;dizin&lt;H2&gt; ister "Hello World."

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Uygulamanızı çalıştırın ve ziyaret [ `http://localhost:xx/HelloWorld` ](http://localhostxx) tarayıcınızda yeniden. Denetleyici bu örnekteki dizin yöntemi herhangi bir iş başarmadık, ancak "hangi istemcisine bir yanıt işlemek için bir görünüm şablon dosyası kullanılacak istedik belirtilen dönüş View()" çağrısı. Biz kullanılacak görünüm şablon dosyası adı açıkça belirtmediği için ASP.NET MVC \Views\HelloWorld klasördeki Index.aspx görünüm dosyası kullanmayı varsayılan olarak. Şimdi biz bizim görünümünde kodlanmış dize görüyoruz.

[![Dizin - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Oldukça iyi görünüyor. Ancak, "Index" tarayıcının başlık diyor ve büyük başlığı sayfasında "MVC Uygulamam." ifadesini içeren dikkat edin Bu değiştirelim.

### <a name="changing-views-and-master-pages"></a>Değişen görünümleri ve ana sayfalar

İlk olarak, "MVC Uygulamam." metin değiştirelim Bu metin, paylaşılan ve her sayfada görüntülenir. Uygulamamızı her sayfasında olsa bile gerçekten kodumuz, yalnızca tek bir yerde görünür. Çözüm Gezgini'nde /Views/Shared klasörüne gidin ve Site.Master dosyasını açın. Bu dosya bir ana sayfa olarak adlandırılır ve "diğer tüm sayfalarımızın kullanan paylaşılan Kabuk" olur.

ContentPlaceholder "MainContent" ifadesini içeren metin bu dosyada dikkat edin.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Burada oluşturduğunuz tüm sayfaları, "ana sayfasında Sarmalanan" gösterilir, tutucudur. Uygulamanızı çalıştırın ve birden çok sayfalarını ziyaret başlık değiştirmeyi deneyin. Bir değişikliğiniz birden çok sayfada göründüğünü fark edersiniz.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Her sayfanın birincil başlık - başlıkları olan H1 - "My MVC film uygulaması." Bu sayfalar arasında paylaşılan üst var. beyaz metinli işler.

Değiştirilen başlık ile tamamen Site.Master şöyledir:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Şimdi, dizin sayfası başlığı değiştirelim.

Open /HelloWorld/Index.aspx. Değiştirmek için iki yerde yoktur. İlk olarak, başlık, tarayıcı, daha sonra H2 - de ikincil üstbilgi - başlığında görüntülenir. Bunların her uygulamanın hangi parçası hangi bit kod görebilmeniz için biraz daha farklı değişiklikler yapacaksınız.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Uygulamanızı çalıştırın ve /Movies ziyaret edin. Tarayıcı başlığı, başlığı birincil ve ikincil başlıklar değiştirildi dikkat edin. Büyük değişiklikler görünümünüzü uygulamanızda küçük değişiklikler yapmak kolay bir işlemdir.

[![Film listesi - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

(Bu durumda "Hello World!", "veri" az bizim bit ileti), ancak kodlanmış zordu. V (görünümleri) yapılandırdığımıza göre ve C (denetleyiciler), ancak henüz hiçbir M (modeli) yapılandırdığımıza göre. Kısa bir süre içinde nasıl alacağız bir veritabanı oluşturur ve model verileri alabilirsiniz.

## <a name="passing-a-viewmodel"></a>Bir ViewModel geçirme

Bir veritabanına gidin ve modelleri hakkında konuşmak önce ilk "Viewmodel'lar" hakkında konuşalım Şablonu Görüntüle bir HTML yanıtı istemciye işlenecek gerektirir temsil eden nesneleri şunlardır. Bunlar genellikle oluşturulur ve bir görünüm şablonu için bir denetleyici sınıfı tarafından geçirilen ve yalnızca görünüm şablonu gerektirir - veri ve artık içermelidir.

Daha önce HelloWorld örneğimizi bizim Welcome() eylem yöntemi bir ad ve bir numTimes parametre sürdü ve tarayıcıya çıktı. Bu yanıt doğrudan işlemeye devam denetleyiciniz yerine, bunun yerine verileri tutmak ve ardından bunu kullanarak HTML yanıtını geri işlemek için bir görünüm şablonu geçirin için küçük bir sınıf olalım. Bu şekilde denetleyici bir şey ve şablonu görüntüleme ile ilgili başka-us "ayrılmasına" uygulamamız içinde korumak etkinleştiriliyor.

HelloWorldController.cs dosyasına dönün ve "WelcomeViewModel" yeni bir sınıf ekleyin ve sonra da denetleyicinizin Hoş Geldiniz bir yöntemde değiştirin. Yeni sınıfı aynı dosyada ile tam HelloWorldController.cs aşağıda verilmiştir.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Bunu birden çok satırda olsa da, bizim Hoş Geldiniz aslında yalnızca iki kod deyimlerini yöntemidir. İlk deyim elde edilen nesnenin üzerine görünümü ViewModel nesnesi ve ikinci geçiş iki bizim parametrelerini paketler.

Şimdi bir Hoş Geldiniz görünüm şablonu gerekiyor! Hoş Geldiniz yönteminde sağ tıklayın ve Ekle görünümü seçin. Bu kez, biz "Kesin türü belirtilmiş Görünüm Oluştur" denetleyin ve açılır listeden bizim WelcomeViewModel sınıfı seçin. Bu yeni görünüm yalnızca WelcomeViewModels ve diğer hiçbir türde nesneler hakkında bilgi sahibi olacaktır.

> *Not: Bir kez, WelcomeViewModel için aşağı açılan listeden görünmesini ekledikten sonra derlediğiniz gerekecektir.*


Burada, Görünüm Ekle iletişim aşağıdaki gibi görünmelidir. Ekle düğmesine tıklayın. ![Görünüm daire içinde ekleyin](getting-started-with-mvc-part3/_static/image10.png)

Bu kod ekleme &lt;h2&gt; , yeni Welcome.aspx içinde. Biz bir döngü yapmak ve sayıda olarak kullanıcı gelmiş diyor Merhaba deyin!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Ayrıca, çünkü Biz bu görünümü WelcomeViewModel hakkında söyledi, yazarken dikkat edin (unutmayın, Evli oldukları?) her zaman, biz başvuru bizim Model nesnesi olarak aşağıdaki ekran görüntüsünde görülen yararlı IntelliSense aldığımız:

[![NumTime kaynak kodu](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Uygulamanızı çalıştırın ve ziyaret `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` yeniden. Data URL'deki yönlendiriyoruz artık Denetleyicimizin otomatik olarak geçirilir, denetleyici bir ViewModel verileri paketleri ve bizim görünümü bu nesneye geçirir. Veriler kullanıcıya HTML olarak görüntülüyor görüntüleyin.

[![Hoş Geldiniz - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

De, bir tür için Model "M", ancak veritabanı türü oluştu. Ne biz öğrendiniz ve film veritabanı oluşturma ele alalım.

> [!div class="step-by-step"]
> [Önceki](getting-started-with-mvc-part2.md)
> [İleri](getting-started-with-mvc-part4.md)
