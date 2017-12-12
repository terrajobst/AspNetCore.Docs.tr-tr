---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: "Bir görünüm (VB) ekleme | Microsoft Docs"
author: Rick-Anderson
description: "Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7e8564c743510780b93d56bc1215f4c5b1faeb43
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view-vb"></a>Bir görünüm (VB) ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek. Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun. Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> VB.NET kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir. [VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). C# tercih ederseniz, geçiş [C# sürümü](../cs/adding-a-view.md) Bu öğreticinin.


Bu bölümde değiştirmek için yapacağız `HelloWorldController` sınıfı bir görünüm şablonu dosyasına düzgün bir şekilde kullanmak için bir istemci HTML yanıtlarını oluşturma işlemine yalıtma.

Şablonu görüntüleme kullanarak başlayalım `Index` yönteminde `HelloWorldController` sınıfı. Şu anda `Index` yöntemi bir iletiyle içinde denetleyici sınıfı sabit kodlanmış bir dize döndürür. Değişiklik `Index` döndürülecek yöntemi bir `View` nesnesi, aşağıda gösterildiği gibi:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Şimdi bir görünüm şablonu ile çağırabileceği bizim projesine ekleyelim `Index` yöntemi. Bunu yapmak için sağ tıklatın içinde `Index` yöntemi ve tıklatın **Görünüm Ekle**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

**Görünüm Ekle** iletişim kutusu görüntülenir. Varsayılan girişleri bırakabilir ve tıklatın **Ekle** düğmesi.

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

*MvcMovie\Views\HelloWorld* klasör ve *MvcMovie\Views\HelloWorld\Index.vbhtml* dosyası oluşturulur. Bunları görebilirsiniz **Çözüm Gezgini**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Bazı HTML altında eklemek `<h2>` etiketi. Değiştirilen *MvcMovie\Views\HelloWorld\Index.vbhtml* dosya aşağıda gösterilmektedir.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Uygulamayı çalıştırın ve Gözat &quot;Merhaba Dünya&quot; denetleyici (`http://localhost:xxxx/HelloWorld`). `Index` Denetleyicinizi yönteminde kadar iş yapmak olmadı; yalnızca bir deyim çalışan `return View()`, biz istemci yanıta işlemek için bir görünüm şablon dosyası kullanmak istediğinizi gösterilir. Kullanılacak görünüm şablonu dosyasının adı biz açıkça belirtmediğinden, ASP.NET MVC kullanarak varsayılan *Index.vbhtml* görünüm dosyası içinde *\Views\HelloWorld* klasör. Aşağıdaki görüntü görünümünde sabit kodlanmış dize gösterir.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Oldukça iyi görünür. Ancak, tarayıcının başlık çubuğunda yazmadığını fark &quot;dizin&quot; ve büyük başlık sayfasında diyor &quot;MVC Uygulamam.&quot; Bu değiştirelim.

## <a name="changing-views-and-layout-pages"></a>Görünümleri ve Düzen sayfaları değiştirme

İlk olarak, metin değiştirelim &quot;MVC Uygulamam.&quot; Bu metin paylaşılır ve her sayfada görüntülenir. Uygulamamızı her sayfasında olsa bile gerçekte bizim projesinde, yalnızca tek bir yerde görünür. Git */görünümler/paylaşılan* klasöründe **Çözüm Gezgini** açarak  *\_Layout.vbhtml* dosya. Bu dosyayı bir düzen sayfası olarak adlandırılır ve paylaşılan ise &quot;Kabuk&quot; , diğer tüm sayfalar kullanın.

Not `@RenderBody()` dosyasının altına kod satırı. `RenderBody`Burada oluşturduğunuz tüm sayfalar gösterir, bir yer tutucudur &quot;Sarmalanan&quot; düzeni sayfasında. Değişiklik `<h1>` gelen başlık  **&quot;**  MVC Uygulamam&quot; için &quot;MVC film uygulaması&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Uygulamayı çalıştırın ve şimdi yazacaktır Not &quot;MVC film uygulaması&quot;. Tıklatın **hakkında** bağlantısı ve sayfa gösterir &quot;MVC film uygulaması&quot;, çok.

Tam  *\_Layout.vbhtml* dosya aşağıda gösterilmektedir:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Şimdi, dizin sayfası (Görünüm) başlığı değiştirelim.

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Açık *MvcMovie\Views\HelloWorld\Index.vbhtml*. Bir değişiklik yapmak için iki yerde vardır: ilk olarak, metin görünür tarayıcının başlık ve ardından ikincil üstbilgisinde ( `<h2>` öğesi). Hangi bölümünün uygulamanın hangi bit kod değişiklikleri görebilmeleri biz biraz farklı yapmanız.

Uygulamayı çalıştırın ve Gözat`http://localhost:xx/HelloWorld`. Tarayıcı başlığı, birincil başlık ve ikincil başlıklar değişmiş dikkat edin. Büyük küçük değişikliklerle, uygulamanızdaki bir görünüme değişiklik kolaydır. (Değişiklikleri tarayıcıda görmüyorsanız, önbelleğe alınmış içeriği görüntülüyor olabilirsiniz. Yüklenecek sunucudan yanıt zorlamak için tarayıcınızda CTRL + F5'e basın.)

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Bizim az biti &quot;veri&quot; (Bu durumda &quot;Hello World!&quot; ileti) sabit kodlanmış, ancak değil. MVC uygulamamız V (görünümler) vardır ve biz C (denetleyicileri), ancak henüz hiçbir M (modeli) olduğuna. Kısa bir süre içinde biz nasıl adım geçireceğiz bir veritabanı oluşturun ve model verileri alabilirsiniz.

## <a name="passing-data-from-the-controller-to-the-view"></a>Denetleyici geçirme verileri görüntülemek için

Bir veritabanına gidin ve modelleri hakkında konuşun önce ancak şimdi ilk bilgi denetleyicisinden bir görünüme geçirme hakkında konuşun. Şablonu görüntüleme bir istemci için bir HTML yanıt işlemek için gerektirdiği istiyoruz. Bu nesneler genelde oluşturulur ve bir görünüm şablonu için bir denetleyici sınıfı tarafından geçirilen ve şablonu görüntüleme gerektiren veri içermelidir — ve daha fazla.

Daha önce ile `HelloWorldController` sınıfı, `Welcome` eylem yöntemine geçen bir `name` ve `numTimes` parametresi ve parametre değerleri tarayıcıya sonra çıktı. Bunun yerine bu yanıt doğrudan işlemeye devam denetleyiciniz daha şimdi bunun yerine Biz bu verileri bir görünüm için Çantası. Denetleyicileri ve görünümleri kullanabilir bir `ViewBag` verileri tutacak nesne. Bir görünüm şablonu otomatik olarak geçirilen ve olması veri paketi içeriğini kullanarak HTML yanıtı işlemek için kullanılan. Böylece denetleyicisi tek şey ve başka bir görünüm şablonu ile ilgilenir — bize temiz korumak etkinleştirme &quot;sorunları ayrılması&quot; uygulama içinde.

Alternatif olarak, biz özel bir sınıf tanımlama, ardından söz konusu nesne örneği bizim kendi, verilerle doldurmak oluşturup görünüme iletmek. Özel bir Model görünüm için olduğundan, genellikle bir ViewModel denir. Küçük miktardaki veriler için ancak ViewBag iyi bir uygulamadır.

Geri dönüp *HelloWorldController.vb* dosya değişikliği `Welcome` NumTimes ve ileti ViewBag yerleştirilecek denetleyicisi içinde yöntemi. Görünüm Paketi dinamik bir nesnedir. Ne olursa olsun, istediğiniz koyabilirsiniz anlamına gelir. İçindeki bir şey put kadar görünüm paketi tanımlı özelliği yok.

Tam `HelloWorldController.vb` aynı dosyada yeni sınıf ile.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Şimdi bizim ViewBag görünümüne otomatik olarak geçirilir veri içeriyor. Biz beğendiğinizi, yeniden alternatif olarak Biz bu gibi kendi nesnesindeki geçmiş:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

İhtiyacımız artık bir `WelcomeView` şablonu! Yeni kod derlenmiş şekilde uygulamayı çalıştırın. Tarayıcıyı kapatın, içinde sağ `Welcome` yöntemi ve ardından **Görünüm Ekle**.

İşte, **Görünüm Ekle** gibi iletişim kutusu görünür.

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Altında aşağıdaki kodu ekleyin `<h2>` öğesinde yeni *Hoş Geldiniz.* vbhtml dosyası. Biz döngü olmak ve söyleyin &quot;Hello&quot; sayıda kullanıcı biz gerektiğini bildiren!

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Uygulamayı çalıştırın ve göz atın`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Artık veriler URL'den gerçekleştirilecek ve denetleyiciye otomatik olarak geçirildi. Denetleyici paketler veri bir `Model` nesnesini ve nesne görünümüne geçirir. Verileri HTML olarak kullanıcıya görüntüler daha görüntüleyin.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Bir tür iyi, bir &quot;M&quot; modeli, ancak veritabanı türü değil. Ne biz öğrendiğinize ve film bir veritabanı oluşturmak atalım.

>[!div class="step-by-step"]
[Önceki](adding-a-controller.md)
[sonraki](adding-a-model.md)
