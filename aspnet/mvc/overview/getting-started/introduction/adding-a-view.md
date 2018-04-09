---
title: Bir MVC uygulamasına bir görünümü ekleme
author: Rick-Anderson
description: Bir MVC uygulamasına bir görünümü ekleme
ms.author: riande
manager: wpickett
ms.date: 09/1721/2017
ms.topic: article
ms.technology: dotnet-mvc
ms.prod: .net-framework
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 21db97e635b5db580df31f46ca7f8b60a80d6f94
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-view"></a>Bir görünümü ekleme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Bu bölümde değiştirme oluşturacağız `HelloWorldController` sınıfı şablon dosyalarını düzgün bir şekilde kapsülleyen bir istemci HTML yanıtlarını oluşturma işlemine görünümünü kullanın. 

Görünüm şablonu kullanarak bir dosyaya oluşturacaksınız [Razor görüntüleme altyapısı](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md). Razor tabanlı görünümü şablonları bir *.cshtml* dosya uzantısı ve C# kullanarak çıktısını HTML oluşturmak için zarif bir yolunu sağlar. Razor karakter ve bir görünüm şablon yazarken gereken tuş vuruşları sayısını en aza indirir ve iş akışı kodlama hızlı, sıvı sağlar.

Şu anda `Index` yöntemi controller sınıfında sabit kodlanmış olduğunu belirten bir ileti ile bir dize döndürür. Değişiklik `Index` döndürülecek yöntemi bir `View` nesnesi, aşağıdaki kodda gösterildiği gibi:

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

`Index` Yukarıdaki yöntemi tarayıcıya bir HTML yanıtı oluşturmak için bir görünüm şablonu kullanır. Denetleyici yöntemlerine (olarak da bilinen [eylem yöntemleri](http://rachelappel.com/asp.net-mvc-actionresults-explained)), gibi `Index` yukarıdaki genellikle döndürme bir [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (veya türetilmiş bir sınıf [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), olmayan ilkel türler, string ister.

Sağ tıklayın *Views\HelloWorld* klasörü ve tıklatın **Ekle**, ardından **MVC 5 Düzen (Razor) olan görünüm sayfası**.
  
![](adding-a-view/_static/image1.png)   
  
İçinde **öğesi için ad belirtmek** iletişim kutusunda, girin *dizin*ve ardından **Tamam**.  
  
![](adding-a-view/_static/image2.png)  
  
İçinde **düzen sayfası seçin** iletişim kutusunda, varsayılanı kabul  **\_Layout.cshtml** tıklatıp **Tamam**.  
  
![](adding-a-view/_static/image3.png)  
  
Yukarıdaki iletişim *görünümler/paylaşılan* klasörü, sol bölmede seçilidir. Başka bir klasöre özel yerleşim dosya'i seçebilirsiniz. Biz Düzen dosyası hakkında daha sonra öğreticide konuşun

*MvcMovie\Views\HelloWorld\Index.cshtml* dosyası oluşturulur.

![](adding-a-view/_static/image4.png)

Aşağıdaki vurgulanmış biçimlendirmeyi ekleyin.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Sağ tıklayın *Index.cshtml* dosya ve seçin **tarayıcıda görüntüle**.

![PI](adding-a-view/_static/image5.png)

Ayrıca sağ tıklayabilir, *Index.cshtml* dosya ve seçin **sayfa denetçisi görünümünde.** Bkz: [sayfa denetçisi öğretici](../../views/using-page-inspector-in-aspnet-mvc.md) daha fazla bilgi için.

Alternatif olarak, uygulamayı çalıştırın ve Gözat `HelloWorld` denetleyici (`http://localhost:xxxx/HelloWorld`). `Index` Denetleyicinizi yönteminde kadar iş yapmak olmadı; yalnızca bir deyim çalışan `return View()`, hangi belirtilen tarayıcı yanıta işlemek için yöntemi bir görünüm şablon dosyası kullanmanız gerekir. Kullanılacak görünüm şablonu dosyasının adı açıkça belirtmediğiniz olduğundan, ASP.NET MVC kullanarak varsayılan *Index.cshtml* görünüm dosyasında *\Views\HelloWorld* klasör. Aşağıdaki görüntü dizesini gösterir &quot;bizim görünüm şablondan Hello!&quot; görünümünde sabit kodlanmış.

![](adding-a-view/_static/image6.png)

Oldukça iyi görünür. Ancak, tarayıcının başlık çubuğunda gösterdiğine dikkat edin &quot;Index - My ASP.NET Uy "ve"Uygulama adı"büyük bağlantı sayfanın üst kısmında diyor Tarayıcı pencerenizi nasıl küçük yaptığınız bağlı olarak üç çubukları görmek için üst sağ tıklatın gerekebilir için **giriş**, **hakkında**, **kişi**, **Kaydetmek** ve **oturum** bağlantılar.

## <a name="changing-views-and-layout-pages"></a>Görünümleri ve Düzen sayfaları değiştirme

İlk olarak, değiştirmek istediğiniz &quot;uygulama adı&quot; sayfanın üst kısmındaki bağlantı. Metnin her sayfaya yaygındır. Uygulamadaki her sayfada görünse bile projesinde, yalnızca tek bir yerde gerçekten uygulanır. Git */görünümler/paylaşılan* klasöründe **Çözüm Gezgini** açarak  *\_Layout.cshtml* dosya. Bu dosya adında bir *düzen sayfası* ve paylaşılan klasörde diğer tüm sayfalar kullanın.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Düzen şablonları, tek bir yerde, sitenizin HTML kapsayıcı düzeni belirtin ve sitenizin birden çok sayfada üzerinden uygulanan olanak sağlar. Bul `@RenderBody()` satır. `RenderBody` olan burada tüm görünüm özgü sayfaları, bir yer tutucu oluşturmak Göster, &quot;Sarmalanan&quot; düzeni sayfasında. Örneğin, **hakkında** bağlantı *Views\Home\About.cshtml* görünümü içinde işlenir `RenderBody` yöntemi.

Title öğesi içeriğini değiştirin. Değişiklik [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx) Düzen şablonu içinde &quot;uygulama adı&quot; için &quot;MVC film&quot; ve denetleyicisinden `Home` için `Movies`. Tam yerleşimi dosya aşağıda gösterilmiştir:

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Uygulama ve şimdi yazacaktır bildirimi çalıştırmak &quot;MVC film &quot;. Tıklatın **hakkında** bağlantı ve bakın nasıl bu sayfada görüntülenir &quot;MVC film&quot;, çok. Düzen şablonda değişiklik kez bulduk ve sahip sitesindeki tüm sayfalara yansıtacak yeni başlığı.

![](adding-a-view/_static/image8.png)

Ne zaman önce oluşturduğumuz *Views\HelloWorld\Index.cshtml* dosya, aşağıdaki kod içeriyor:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Yukarıdaki Razor kod açıkça düzen sayfası ayarlıyor. İncelemek *görünümleri\\_ViewStart.cshtml* dosyası tam aynı Razor biçimlendirme içeriyor. *[Görünümleri\\_ViewStart.cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* dosyası tüm görünümleri kullanacağı ortak yerleşim tanımlar, out veya bu koddan kaldırma bu nedenle yorum yapabileceği *Views\HelloWorld\ Index.cshtml* dosya.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

Kullanabileceğiniz `Layout` farklı bir görünümü ayarlayın veya ayarlamak özellik `null` herhangi bir düzen dosyası kullanılacak şekilde.

Şimdi, dizin görünümünün başlığı değiştirelim.

Open *MvcMovie\Views\HelloWorld\Index.cshtml*. Bir değişiklik yapmak için iki yerde vardır: ilk olarak, metin görünür tarayıcının başlık ve ardından ikincil üstbilgisinde ( `<h2>` öğesi). Hangi bölümünün uygulamanın hangi bit kod değişiklikleri görebilmeleri biraz farklı yapmanız.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

HTML Başlığı görüntülemek için ayarlar yukarıdaki kodu belirtmek için bir `Title` özelliği `ViewBag` nesne (olduğu içinde *Index.cshtml* şablonu görüntüle). Dikkat Düzen şablonunu ( *görünümler/paylaşılan\\_Layout.cshtml* ) bu değeri kullanır `<title>` öğesi bir parçası olarak `<head>` bölümünde daha önce değiştirilmiş HTML.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

Bu kullanarak `ViewBag` yaklaşımı, kolayca iletebilir diğer parametreleri şablonu görüntüleme ve düzeni dosyanız arasında.

Uygulamayı çalıştırın. Tarayıcı başlığı, birincil başlık ve ikincil başlıklar değişmiş dikkat edin. (Değişiklikleri tarayıcıda görmüyorsanız, önbelleğe alınmış içeriği görüntülüyor olabilirsiniz. Yüklenecek sunucudan yanıt zorlamak için tarayıcınızda CTRL + F5'e basın.) Tarayıcı başlığı ile oluşturulan `ViewBag.Title` biz kümesinde *Index.cshtml* görüntülemek şablonu ve ek &quot;-film uygulaması&quot; düzeni dosyasına eklendi.

Ayrıca dikkat edin nasıl içerik *Index.cshtml* görünüm şablonu birleştirilmiş ile  *\_Layout.cshtml* şablonu görüntüleme ve tek bir HTML yanıtını tarayıcıya gönderildi. Düzen şablonları, uygulamanızdaki sayfaların tümünü uygulamak değişiklik gerçekten kolay hale getirir.

![](adding-a-view/_static/image9.png)

Bizim az biti &quot;veri&quot; (Bu durumda &quot;bizim görünüm şablondan Hello!&quot; ileti) sabit kodlanmış, ancak olduğu. MVC uygulaması bir &quot;V&quot; (Görünüm) ve aldığınız bir &quot;C&quot; (denetleyicisi), ancak hiçbir &quot;M&quot; (henüz model). Kısa bir süre sonra size bir veritabanı oluşturun ve ondan model verileri almak nasıl yol.

## <a name="passing-data-from-the-controller-to-the-view"></a>Denetleyici geçirme verileri görüntülemek için

Bir veritabanına gidin ve modelleri hakkında konuşun önce ancak şimdi ilk bilgi denetleyicisinden bir görünüme geçirme hakkında konuşun. Denetleyici sınıfları, gelen bir URL isteğine yanıt olarak çağrılır. Denetleyici gelen tarayıcı işleyen kodu istekleri, bir veritabanından veri alır ve sonuçta ne tür bir tarayıcıya göndermek için yanıt verirse yazma burada sınıfıdır. Görünüm şablonları daha sonra bir denetleyicisinden oluşturur ve tarayıcıya bir HTML yanıtını biçimlendirmek için kullanılabilir.

Denetleyicileri sırada tarayıcı yanıta işlemek bir şablonu görüntüleme için hangi veri veya nesneler gerekli sağlamak için sorumludur. En iyi yöntem: **şablonu görüntüleme hiçbir zaman iş mantığı gerçekleştirmek veya bir veritabanı ile doğrudan etkileşim**. Bunun yerine, şablonu görüntüleme için denetleyici tarafından sağlanan verileri ile çalışması gerekir. Bu koruma &quot;sorunları ayrılması&quot; tutan kodunuzu temiz, test edilebilir ve daha korunabilir.

Şu anda `Welcome` eylem yönteminde `HelloWorldController` sınıfını alır bir `name` ve `numTimes` parametresi ve çıkışları doğrudan tarayıcıya değerleri. Bu yanıt dize olarak işleme denetleyiciniz yerine bir görünüm şablonu kullanmayı denetleyicisi değiştirelim. Şablonu görüntüleme yanıtı oluşturmak için uygun veri bitleri denetleyicisinden görünüme iletmek gerektiği anlamına gelir dinamik bir yanıt oluşturur. Bu şablonu görüntüleme gereksinimlerinize dinamik veri (parametre) put denetleyicisi sağlayarak yapmak bir `ViewBag` şablonu görüntüle daha sonra erişebilirsiniz nesnesi.

Geri dönüp *HelloWorldController.cs* dosya ve değişiklik `Welcome` ekleme yöntemi bir `Message` ve `NumTimes` değeri `ViewBag` nesnesi. `ViewBag` istediğiniz ona koyabilirsiniz yani dinamik bir nesne değil; `ViewBag` nesnesi içindeki put kadar tanımlı hiçbir özellik sahiptir. [ASP.NET MVC model bağlama sistem](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) adlandırılmış parametreleri otomatik olarak eşlenir (`name` ve `numTimes`) Sorgu dizesinden yönteminizi parametrelere adres çubuğundaki. Tam *HelloWorldController.cs* dosya şu şekilde görünür:

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

Şimdi `ViewBag` nesnesi görünümüne otomatik olarak geçirilen verileri içerir. Ardından, bir Hoş Geldiniz görünüm şablonu gerekiyor! İçinde **yapı** menüsünde, select **yapı çözümü** (veya Ctrl + Shift + B) proje derlenmiş emin olmak için. Sağ tıklayın *Views\HelloWorld* klasörü ve tıklatın **Ekle**, ardından **MVC 5 Düzen (Razor) olan görünüm sayfası**.
  
![](adding-a-view/_static/image10.png)   
  
İçinde **öğesi için ad belirtmek** iletişim kutusunda, girin *Hoş Geldiniz*ve ardından **Tamam**.   
  
İçinde **düzen sayfası seçin** iletişim kutusunda, varsayılanı kabul  **\_Layout.cshtml** tıklatıp **Tamam**.  
  
![](adding-a-view/_static/image11.png)   

*MvcMovie\Views\HelloWorld\Welcome.cshtml* dosyası oluşturulur.

Biçimlendirme Değiştir *Welcome.cshtml* dosya. Bildiren bir döngü oluşturacaksınız &quot;Hello&quot; sayıda kullanıcı onu gerektiğini söyler. Tam *Welcome.cshtml* dosya aşağıda gösterilmektedir.

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Uygulamayı çalıştırın ve aşağıdaki URL'ye gidin:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Şimdi veri URL'den alınır ve denetleyici kullanmaya geçirilen [model Bağlayıcısı](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Verileri denetleyicisi paketleri bir `ViewBag` nesnesini ve nesne görünümüne geçirir. Görünümü sonra verileri HTML olarak kullanıcıya görüntüler.

![](adding-a-view/_static/image12.png)

Yukarıdaki örnekte, kullandık bir `ViewBag` denetleyicisinden bir görünüme veri iletmek için nesne. Öğreticide daha sonra bir denetleyicisinden bir görünüme veri iletmek için bir görünüm modeli kullanacağız. Veri geçirme görünüm modeli yaklaşım görünüm paketi genellikle çok tercih edilen yaklaşımdır. Blog girişine bakın [dinamik V kesin türü belirtilmiş görünümleri](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) daha fazla bilgi için. 

Bir tür iyi, bir &quot;M&quot; modeli, ancak veritabanı türü değil. Ne biz öğrendiğinize ve film bir veritabanı oluşturmak atalım.

> [!div class="step-by-step"]
> [Önceki](adding-a-controller.md)
> [sonraki](adding-a-model.md)
