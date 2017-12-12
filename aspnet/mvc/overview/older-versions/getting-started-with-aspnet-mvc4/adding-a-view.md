---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: "Bir görünümü ekleme | Microsoft Docs"
author: Rick-Anderson
description: "Not: Bu öğreticide güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu daha güvenli, daha kolay izleyin ve gösteri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 4309290294b28d4c177e0193719bcff4b3f2a8cf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view"></a>Bir görünümü ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğretici güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Daha güvenli, izlemek çok daha kolaydır ve daha fazla özelliklerini gösterir.


Bu bölümde değiştirme oluşturacağız `HelloWorldController` sınıfı şablon dosyalarını düzgün bir şekilde kapsülleyen bir istemci HTML yanıtlarını oluşturma işlemine görünümünü kullanın.

Görünüm şablonu kullanarak bir dosyaya oluşturacaksınız [Razor görüntüleme altyapısı](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) ASP.NET MVC 3 ile sunulmuştur. Razor tabanlı görünümü şablonları bir *.cshtml* dosya uzantısı ve C# kullanarak çıktısını HTML oluşturmak için zarif bir yolunu sağlar. Razor karakter ve bir görünüm şablon yazarken gereken tuş vuruşları sayısını en aza indirir ve iş akışı kodlama hızlı, sıvı sağlar.

Şu anda `Index` yöntemi controller sınıfında sabit kodlanmış olduğunu belirten bir ileti ile bir dize döndürür. Değişiklik `Index` döndürülecek yöntemi bir `View` nesnesi, aşağıdaki kodda gösterildiği gibi:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

`Index` Yukarıdaki yöntemi tarayıcıya bir HTML yanıtı oluşturmak için bir görünüm şablonu kullanır. Denetleyici yöntemlerine (olarak da bilinen [eylem yöntemleri](http://rachelappel.com/asp.net-mvc-actionresults-explained)), gibi `Index` yukarıdaki genellikle döndürme bir [ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx) (veya türetilmiş bir sınıf [ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx)), olmayan ilkel türler, string ister.

Proje ile birlikte kullanabileceğiniz bir görünüm şablonu Ekle `Index` yöntemi. Bunu yapmak için sağ tıklatın içinde `Index` yöntemi ve tıklatın **Görünüm Ekle**.

![](adding-a-view/_static/image1.png)

**Görünüm Ekle** iletişim kutusu görüntülenir. Varsayılan değerleri olan ve'ı tıklatın gibi bırakın **Ekle** düğmesi:

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld* klasör ve *MvcMovie\Views\HelloWorld\Index.cshtml* dosyası oluşturulur. Bunları görebilirsiniz **Çözüm Gezgini**:

![](adding-a-view/_static/image3.png)

Aşağıdaki gösterildiği *Index.cshtml* oluşturulmuş dosyası:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Aşağıdaki HTML altında eklemek `<h2>` etiketi.

[!code-html[Main](adding-a-view/samples/sample2.html)]

Tam *MvcMovie\Views\HelloWorld\Index.cshtml* dosya aşağıda gösterilmektedir.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Çözüm Gezgini'nde Visual Studio 2012 kullanıyorsanız, sağ tıklatın *Index.cshtml* dosya ve seçin **sayfa denetçisi görünümünde**.

![PI](adding-a-view/_static/image5.png)

[Sayfa denetçisi öğretici](../../views/using-page-inspector-in-aspnet-mvc.md) bu yeni araç hakkında daha fazla bilgi yer almaktadır.

Alternatif olarak, uygulamayı çalıştırın ve Gözat `HelloWorld` denetleyici (`http://localhost:xxxx/HelloWorld`). `Index` Denetleyicinizi yönteminde kadar iş yapmak olmadı; yalnızca bir deyim çalışan `return View()`, hangi belirtilen tarayıcı yanıta işlemek için yöntemi bir görünüm şablon dosyası kullanmanız gerekir. Kullanılacak görünüm şablonu dosyasının adı açıkça belirtmediğiniz olduğundan, ASP.NET MVC kullanarak varsayılan *Index.cshtml* görünüm dosyasında *\Views\HelloWorld* klasör. Aşağıdaki görüntü dizesini gösterir &quot;bizim görünüm şablondan Hello!&quot; görünümünde sabit kodlanmış.

![](adding-a-view/_static/image6.png)

Oldukça iyi görünür. Ancak, tarayıcının başlık çubuğunda gösterdiğine dikkat edin &quot;My ASP.NET bir dizin&quot; ve sayfanın üst kısmında büyük bağlantı diyor &quot;buraya logonuz konacak.&quot; Aşağıda &quot;, logo buraya.&quot; bağlantı kayıt ve günlük bağlantılar ve giriş, bu bağlantıları aşağıda hakkında ve iletişim sayfaları. Bunlardan bazıları değiştirelim.

## <a name="changing-views-and-layout-pages"></a>Görünümleri ve Düzen sayfaları değiştirme

İlk olarak, değiştirmek istediğiniz &quot;, logo buraya.&quot; sayfanın üst kısmındaki başlığı. Metnin her sayfaya yaygındır. Uygulamadaki her sayfada görünse bile projesinde, yalnızca tek bir yerde gerçekten uygulanır. Git */görünümler/paylaşılan* klasöründe **Çözüm Gezgini** açarak  *\_Layout.cshtml* dosya. Bu dosya adında bir *düzen sayfası* ve paylaşılan &quot;Kabuk&quot; , diğer tüm sayfalar kullanın.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Düzen şablonları, tek bir yerde, sitenizin HTML kapsayıcı düzeni belirtin ve sitenizin birden çok sayfada üzerinden uygulanan olanak sağlar. Bul `@RenderBody()` satır. `RenderBody`olan burada tüm görünüm özgü sayfaları, bir yer tutucu oluşturmak Göster, &quot;Sarmalanan&quot; düzeni sayfasında. Örneğin, hakkında bağlantısını seçerseniz *Views\Home\About.cshtml* görünümü içinde işlenir `RenderBody` yöntemi.

Düzen şablonu sitesi title başlığına değiştirme &quot;buraya logonuz konacak&quot; için &quot;MVC film&quot;.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Title öğesi içeriğini aşağıdaki biçimlendirme ile değiştirin:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Uygulama ve şimdi yazacaktır bildirimi çalıştırmak &quot;MVC film &quot;. Tıklatın **hakkında** bağlantı ve bakın nasıl bu sayfada görüntülenir &quot;MVC film&quot;, çok. Düzen şablonda değişiklik kez bulduk ve sahip sitesindeki tüm sayfalara yansıtacak yeni başlığı.

![](adding-a-view/_static/image8.png)

Şimdi, dizin görünümünün başlığı değiştirelim.

Açık *MvcMovie\Views\HelloWorld\Index.cshtml*. Bir değişiklik yapmak için iki yerde vardır: ilk olarak, metin görünür tarayıcının başlık ve ardından ikincil üstbilgisinde ( `<h2>` öğesi). Hangi bölümünün uygulamanın hangi bit kod değişiklikleri görebilmeleri biraz farklı yapmanız.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

HTML Başlığı görüntülemek için ayarlar yukarıdaki kodu belirtmek için bir `Title` özelliği `ViewBag` nesne (olduğu içinde *Index.cshtml* şablonu görüntüle). Düzen şablonunu kaynak kodu geri bakarsanız, şablonu bu değeri kullanır fark edeceksiniz `<title>` öğesi bir parçası olarak `<head>` bölümünde daha önce değiştirilmiş HTML. Bu kullanarak `ViewBag` yaklaşımı, kolayca iletebilir diğer parametreleri şablonu görüntüleme ve düzeni dosyanız arasında.

Uygulamayı çalıştırın ve Gözat `http://localhost:xx/HelloWorld`. Tarayıcı başlığı, birincil başlık ve ikincil başlıklar değişmiş dikkat edin. (Değişiklikleri tarayıcıda görmüyorsanız, önbelleğe alınmış içeriği görüntülüyor olabilirsiniz. Yüklenecek sunucudan yanıt zorlamak için tarayıcınızda CTRL + F5'e basın.) Tarayıcı başlığı ile oluşturulan `ViewBag.Title` biz kümesinde *Index.cshtml* görüntülemek şablonu ve ek &quot;-film uygulaması&quot; düzeni dosyasına eklendi.

Ayrıca dikkat edin nasıl içerik *Index.cshtml* görünüm şablonu birleştirilmiş ile  *\_Layout.cshtml* şablonu görüntüleme ve tek bir HTML yanıtını tarayıcıya gönderildi. Düzen şablonları, uygulamanızdaki sayfaların tümünü uygulamak değişiklik gerçekten kolay hale getirir.

![](adding-a-view/_static/image9.png)

Bizim az biti &quot;veri&quot; (Bu durumda &quot;bizim görünüm şablondan Hello!&quot; ileti) sabit kodlanmış, ancak olduğu. MVC uygulaması bir &quot;V&quot; (Görünüm) ve aldığınız bir &quot;C&quot; (denetleyicisi), ancak hiçbir &quot;M&quot; (henüz model). Kısa bir süre içinde biz nasıl adım geçireceğiz bir veritabanı oluşturun ve model verileri alabilirsiniz.

## <a name="passing-data-from-the-controller-to-the-view"></a>Denetleyici geçirme verileri görüntülemek için

Bir veritabanına gidin ve modelleri hakkında konuşun önce ancak şimdi ilk bilgi denetleyicisinden bir görünüme geçirme hakkında konuşun. Denetleyici sınıfları, gelen bir URL isteğine yanıt olarak çağrılır. Denetleyici gelen tarayıcı işleyen kodu istekleri, bir veritabanından veri alır ve sonuçta ne tür bir tarayıcıya göndermek için yanıt verirse yazma burada sınıfıdır. Görünüm şablonları daha sonra bir denetleyicisinden oluşturur ve tarayıcıya bir HTML yanıtını biçimlendirmek için kullanılabilir.

Denetleyicileri sırada tarayıcı yanıta işlemek bir şablonu görüntüleme için hangi veri veya nesneler gerekli sağlamak için sorumludur. En iyi yöntem: **şablonu görüntüleme hiçbir zaman iş mantığı gerçekleştirmek veya bir veritabanı ile doğrudan etkileşim**. Bunun yerine, şablonu görüntüleme için denetleyici tarafından sağlanan verileri ile çalışması gerekir. Bu koruma &quot;sorunları ayrılması&quot; tutan kodunuzu temiz, test edilebilir ve daha korunabilir.

Şu anda `Welcome` eylem yönteminde `HelloWorldController` sınıfını alır bir `name` ve `numTimes` parametresi ve çıkışları doğrudan tarayıcıya değerleri. Bu yanıt dize olarak işleme denetleyiciniz yerine bir görünüm şablonu kullanmayı denetleyicisi değiştirelim. Şablonu görüntüleme yanıtı oluşturmak için uygun veri bitleri denetleyicisinden görünüme iletmek gerektiği anlamına gelir dinamik bir yanıt oluşturur. Bu şablonu görüntüleme gereksinimlerinize dinamik veri (parametre) put denetleyicisi sağlayarak yapmak bir `ViewBag` şablonu görüntüle daha sonra erişebilirsiniz nesnesi.

Geri dönüp *HelloWorldController.cs* dosya ve değişiklik `Welcome` ekleme yöntemi bir `Message` ve `NumTimes` değeri `ViewBag` nesnesi. `ViewBag`istediğiniz ona koyabilirsiniz yani dinamik bir nesne değil; `ViewBag` nesnesi içindeki put kadar tanımlı hiçbir özellik sahiptir. [ASP.NET MVC model bağlama sistem](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) adlandırılmış parametreleri otomatik olarak eşlenir (`name` ve `numTimes`) Sorgu dizesinden yönteminizi parametrelere adres çubuğundaki. Tam *HelloWorldController.cs* dosya şu şekilde görünür:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Şimdi `ViewBag` nesnesi görünümüne otomatik olarak geçirilen verileri içerir.

Ardından, bir Hoş Geldiniz görünüm şablonu gerekiyor! İçinde **yapı** menüsünde, select **yapı MvcMovie** proje derlenmiş emin olmak için.

İçinde sağ `Welcome` yöntemi ve tıklatın **Görünüm Ekle**.

![](adding-a-view/_static/image10.png)

İşte **Görünüm Ekle** iletişim kutusu görünür gibi:

![](adding-a-view/_static/image11.png)

Tıklatın **Ekle**ve ardından aşağıdaki kodu ekleyin `<h2>` öğesinde yeni *Welcome.cshtml* dosya. Bildiren bir döngü oluşturacaksınız &quot;Hello&quot; sayıda kullanıcı onu gerektiğini söyler. Tam *Welcome.cshtml* dosya aşağıda gösterilmektedir.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Uygulamayı çalıştırın ve aşağıdaki URL'ye gidin:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Şimdi veri URL'den alınır ve denetleyici kullanmaya geçirilen [model Bağlayıcısı](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Verileri denetleyicisi paketleri bir `ViewBag` nesnesini ve nesne görünümüne geçirir. Görünümü sonra verileri HTML olarak kullanıcıya görüntüler.

![](adding-a-view/_static/image12.png)

Yukarıdaki örnekte, kullandık bir `ViewBag` denetleyicisinden bir görünüme veri iletmek için nesne. Sonraki öğreticide, bir görünüm modeli bir denetleyicisinden bir görünüme veri iletmek için kullanacağız. Veri geçirme görünüm modeli yaklaşım görünüm paketi genellikle çok tercih edilen yaklaşımdır. Blog girişine bakın [dinamik V kesin türü belirtilmiş görünümleri](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) daha fazla bilgi için.

Bir tür iyi, bir &quot;M&quot; modeli, ancak veritabanı türü değil. Ne biz öğrendiğinize ve film bir veritabanı oluşturmak atalım.

>[!div class="step-by-step"]
[Önceki](adding-a-controller.md)
[sonraki](adding-a-model.md)
