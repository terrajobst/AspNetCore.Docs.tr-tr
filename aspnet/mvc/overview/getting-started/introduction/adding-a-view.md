---
title: Bir MVC uygulaması için bir görünüm ekleme
author: Rick-Anderson
description: Bir MVC uygulaması için bir görünüm ekleme
ms.author: riande
ms.date: 09/1721/2017
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 56c00d5992a95971f48bb6e1ec30d63706948997
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578243"
---
<a name="adding-a-view"></a>Görünüm ekleme
====================
Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Bu bölümde, değiştirilecek yedekleyeceksiniz `HelloWorldController` sınıfının şablon dosyalarını indrebilirsiniz kapsülleyen bir istemci HTML yanıtlarını oluşturma işlemi görünümünü kullanın. 

Görünüm şablonu kullanarak dosyanın oluşturacaksınız [Razor görünüm altyapısı](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md). Razor tabanlı bir görünüm şablonları bir *.cshtml* dosya uzantısı ve C# kullanarak çıktısını HTML oluşturmak için zarif bir yol sağlar. Razor karakterler ve bir şablonu görüntüleme yazarken gerekli tuş vuruşları sayısını en aza indirir ve iş akışı kodlama daha hızlı bir akış sağlar.

Şu anda `Index` yöntemi controller sınıfında sabit kodlu olduğunu belirten bir ileti içeren bir dize döndürür. Değişiklik `Index` döndürülecek yöntemi bir `View` aşağıdaki kodda gösterildiği gibi nesne:

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

`Index` Yukarıdaki yöntemi, bir tarayıcıya HTML yanıtı oluşturmak için bir görünüm şablonu kullanır. Denetleyici yöntemlerinde (olarak da bilinen [eylem yöntemleri](http://rachelappel.com/asp.net-mvc-actionresults-explained)), gibi `Index` yukarıdaki yöntemi genellikle iade bir [actionresult öğesini](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (veya türetilmiş bir sınıf [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), gibi dize olmayan ilkel türler.

Sağ tıklayın *Views\HelloWorld* klasörü ve tıklatın **Ekle**, ardından **MVC 5 görünüm sayfası (Razor)**.
  
![](adding-a-view/_static/image1.png)   
  
İçinde **öğe için ad belirtmek** iletişim kutusuna *dizin*ve ardından **Tamam**.  
  
![](adding-a-view/_static/image2.png)  
  
İçinde **yerleşim sayfası seçin** iletişim kutusunda varsayılan değerleri kabul  **\_Layout.cshtml** tıklatıp **Tamam**.  
  
![](adding-a-view/_static/image3.png)  
  
Yukarıdaki iletişim *görünümler/paylaşılan* sol bölmede seçili klasör. Özel düzen dosyası başka bir klasöre olsaydı seçebilirsiniz. Öğreticinin ilerleyen bölümlerinde Düzen dosyası hakkında konuşacağız

*MvcMovie\Views\HelloWorld\Index.cshtml* dosyası oluşturulur.

![](adding-a-view/_static/image4.png)

Vurgulanan aşağıdaki işaretlemeyi ekleyin.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Sağ tıklayın *Index.cshtml* seçin ve dosya **tarayıcıda görüntüle**.

![PI](adding-a-view/_static/image5.png)

Sağ tıklayabilir *Index.cshtml* seçin ve dosya **sayfa denetçisi görünümünde.** Bkz: [sayfa denetçisi öğretici](../../views/using-page-inspector-in-aspnet-mvc.md) daha fazla bilgi için.

Alternatif olarak, uygulamayı çalıştırmak ve göz atın `HelloWorld` denetleyici (`http://localhost:xxxx/HelloWorld`). `Index` Denetleyicinizin yöntemi kadar iş yapmak istemediğiniz; yalnızca bir deyim çalıştırdığınız `return View()`, hangi belirtilen tarayıcı yanıt işlemek için yöntemi bir görünüm şablon dosyası kullanmanız gerekir. Kullanılacak görünüm şablon dosyası adı açıkça belirtmediğiniz için ASP.NET MVC kullanarak varsayılan *Index.cshtml* görünümü dosyasında *\Views\HelloWorld* klasör. Dize aşağıdaki resimde gösterilmektedir &quot;bizim görünümü şablondan Merhaba!&quot; görünümünde sabit kodlanmış.

![](adding-a-view/_static/image6.png)

Oldukça iyi görünüyor. Tarayıcının başlık çubuğunda gösterilir, ancak, fark &quot;Index - My ASP.NET Uy "ve büyük bağlantı sayfanın üstündeki"Uygulama adı"diyor. Tarayıcı pencerenizin nasıl küçük yaptığınız bağlı olarak görmek için sağ üst köşedeki üç çubuğa tıklayın gerekebilir için **giriş**, **hakkında**, **kişi**, **Kaydetme** ve **oturum** bağlantıları.

## <a name="changing-views-and-layout-pages"></a>Görünümlere ve sayfalara düzenini değiştirme

İlk olarak, değiştirmek istediğiniz &quot;uygulama adı&quot; sayfanın üstündeki bağlantısı. Metnin her sayfaya yaygındır. Uygulamadaki her sayfada görünmesine rağmen gerçekte projesinde, yalnızca tek bir yerde uygulanır. Git */görünümler/paylaşılan* klasöründe **Çözüm Gezgini** açın  *\_Layout.cshtml* dosya. Bu dosya adında bir *düzen sayfası* ve diğer tüm sayfalar kullanan paylaşılan klasöründe bulunur.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Düzen şablonları, tek bir yerde sitenizin HTML kapsayıcı düzenini belirtin ve sitenizde birden çok sayfada geçerli olanak sağlar. Bulma `@RenderBody()` satır. `RenderBody` olduğundan burada tüm görünüm özel sayfalar, yer tutucu oluşturma Göster, &quot;sarmalanmış&quot; Düzen sayfasında. Örneğin, **hakkında** bağlantı *Views\Home\About.cshtml* görünümü içinde işlenir `RenderBody` yöntemi.

Title öğesi içeriğini değiştirin. Değişiklik [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx) Düzen şablonu içinde &quot;uygulama adı&quot; için &quot;MVC film&quot; ve denetleyicisinden `Home` için `Movies`. Tam Düzen dosyası aşağıda gösterilmiştir:

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Artık diyor dikkat edin ve uygulama çalıştırma &quot;MVC film &quot;. Tıklayın **hakkında** bağlantı ve bkz bu sayfaya nasıl görüneceğini &quot;MVC film&quot;, çok. Biz Düzen şablonda değişiklik kez sunmayı başarabilseydiniz nasıl olurdu ve sahip sitesindeki tüm sayfalara yansıtan yeni başlığı.

![](adding-a-view/_static/image8.png)

Ne zaman önce oluşturduğumuz *Views\HelloWorld\Index.cshtml* dosya, aşağıdaki kodda yer:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Razor kodu açıkça düzen sayfası ayarlıyor. İncelemek *görünümleri\\_ViewStart.cshtml* dosya, tam olarak aynı Razor işaretlemesi içerir. *[Görünümleri\\_ViewStart.cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* dosyası, tüm görünümlere kullanacağı ortak düzenini tanımlar, bu nedenle, out ya da bu koddan kaldırdığınızdan açıklama satırı haline getirebilirsiniz *Views\HelloWorld\ Index.cshtml* dosya.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

Kullanabileceğiniz `Layout` farklı düzeni görünümünde veya ayarlamak, özellik `null` hiçbir Düzen dosyası yeniden kullanılacak şekilde.

Şimdi, dizin görünümünün başlık değiştirelim.

Açık *MvcMovie\Views\HelloWorld\Index.cshtml*. Değişiklik yapmak için iki yerde vardır: ilk olarak, metni görüntülenir tarayıcı başlık ve ikincil üst bilgisindeki ( `<h2>` öğesi). Uygulamanın hangi parçası hangi bit kod değişiklikleri görebilmeniz için biraz daha farklı yapmanız.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

HTML Başlığı görüntülemek için kod kümeleri üstündeki belirtmek için bir `Title` özelliği `ViewBag` nesne (içinde *Index.cshtml* şablonu görüntüleme). Dikkat Düzen şablonunu ( *görünümler/paylaşılan\\_Layout.cshtml* ) bu değeri kullanır `<title>` öğesi bir parçası olarak `<head>` bölümünde daha önce değiştirilen HTML.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

Bunu kullanarak `ViewBag` yaklaşımı, kolayca geçirebilirsiniz diğer parametreler, Düzen dosyası şablonu görüntüleme arasında.

Uygulamayı çalıştırın. Tarayıcı başlığı, başlığı birincil ve ikincil başlıklar değiştirildi dikkat edin. (Tarayıcı değişiklikleri görmüyorsanız, önbelleğe alınmış içerikleri görüntülüyor olabilirsiniz. Tarayıcınızda yüklenmesine sunucudan yanıt zorlamak için CTRL + F5'e basın.) Tarayıcı başlığı ile oluşturulan `ViewBag.Title` biz kümesinde *Index.cshtml* şablonu ve ek görüntüleme &quot;-film uygulaması&quot; Düzen dosyasında eklendi.

Ayrıca dikkat edin nasıl içeriği *Index.cshtml* görünüm şablonu ile birleştirilmiş  *\_Layout.cshtml* şablonu görüntüleme ve tek bir HTML yanıtını tarayıcıya gönderilen. Düzen şablonları gerçekten tüm uygulamanızdaki sayfaların uygulanacak değişiklikler yapmak kolaylaştırır.

![](adding-a-view/_static/image9.png)

Bizim küçük bit &quot;veri&quot; (Bu durumda &quot;bizim görünümü şablondan Merhaba!&quot; ileti) sabit kodlanmıştır, ancak olduğu. MVC uygulama bir &quot;V&quot; (view) ve kendinizi bir &quot;C&quot; (denetleyicisi), ancak hiçbir &quot;M&quot; (henüz model). Kısa bir süre içinde bir veritabanı oluşturun ve ondan model verileri almak nasıl gösterilecektir.

## <a name="passing-data-from-the-controller-to-the-view"></a>Görünüm denetleyicisinden veri geçirme

Bir veritabanına gidin ve modelleri hakkında konuşmak önce ilk görünümü denetleyicisi bilgi geçirme hakkında konuşalım. Denetleyici sınıflarına gelen bir URL isteğine yanıt olarak çağrılır. Denetleyici sınıfı Burada, istekleri, bir veritabanından veri alır ve sonuçta ne tür bir tarayıcıya gönderilecek yanıt verirse gelen tarayıcı işleyen kodu yazdığınız yerdedir. Görünüm şablonları oluşturmak ve bir HTML yanıtını tarayıcıya biçimlendirmek için bir denetleyiciden sonra kullanılabilir.

Denetleyicileri, seçtiğiniz veri veya nesneleri tarayıcı yanıt işlemek bir şablonu görüntüleme için sırayla gerekli sağlamaktan sorumludur. En iyi yöntem: **şablonu görüntüle hiçbir zaman iş mantığını gerçekleştirebilirsiniz veya bir veritabanıyla doğrudan etkileşim**. Bunun yerine, bir şablonu görüntüleme için denetleyici tarafından sağlanan veri ile çalışması gerekir. Bu koruma &quot;görev ayrımı nettir&quot; yardımcı olur, kodunuzu temiz, test edilebilir ve daha sürdürülebilir tutun.

Şu anda `Welcome` eylem yönteminde `HelloWorldController` sınıfı alır bir `name` ve `numTimes` parametresi ve çıkışları doğrudan tarayıcıya değerleri. Bu yanıt dize olarak işleme denetleyiciniz yerine denetleyici görünüm şablonu kullanmayı değiştirelim. Şablonu Görüntüle yanıtı oluşturmak için uygun veri bitleri denetleyicisinden görünüme iletmek gerektiği anlamına gelir dinamik bir yanıt oluşturur. Şablonu görüntüleme, gereken dinamik (Parametreler) verilerinizden denetleyicisi sağlayarak bunu yapabilirsiniz bir `ViewBag` görünüm şablonu erişebiliyorsa nesne.

Geri dönüp *HelloWorldController.cs* dosya ve değiştirme `Welcome` yöntemi eklemek için bir `Message` ve `NumTimes` değerini `ViewBag` nesne. `ViewBag` inovasyonunuz ne olursa olsun istediğiniz şekilde koyabilirsiniz anlamına gelir. bir dinamik Nesne sınıflandırabilirsiniz. `ViewBag` nesnesi içine yerleştirin kadar tanımlı hiçbir özellik sahiptir. [ASP.NET MVC model bağlama sistemi](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) adlandırılmış parametreleri otomatik olarak eşlenir (`name` ve `numTimes`) Sorgu dizesinden yönteminizi parametrelere adres çubuğundaki. Tam *HelloWorldController.cs* dosya şu şekilde görünür:

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

Artık `ViewBag` nesnesi görünüme otomatik olarak iletilen verileri içerir. Ardından, bir şablonu Hoş Geldiniz görüntüleme gerekir! İçinde **derleme** menüsünde **Çözümü Derle** (veya Ctrl + Shift + B) proje derlendiğinde emin olmak için. Sağ tıklayın *Views\HelloWorld* klasörü ve tıklatın **Ekle**, ardından **MVC 5 görünüm sayfası (Razor)**.
  
![](adding-a-view/_static/image10.png)   
  
İçinde **öğe için ad belirtmek** iletişim kutusuna *Hoş Geldiniz*ve ardından **Tamam**.   
  
İçinde **yerleşim sayfası seçin** iletişim kutusunda varsayılan değerleri kabul  **\_Layout.cshtml** tıklatıp **Tamam**.  
  
![](adding-a-view/_static/image11.png)   

*MvcMovie\Views\HelloWorld\Welcome.cshtml* dosyası oluşturulur.

Biçimlendirmeyi Değiştir *Welcome.cshtml* dosya. Bildiren bir döngü oluşturursunuz &quot;Hello&quot; sayıda kullanıcı olması gerektiği söyler. Tam *Welcome.cshtml* dosya aşağıda gösterilmektedir.

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Uygulamayı çalıştırın ve aşağıdaki URL'ye gidin:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Şimdi veri URL'den gerçekleştirilen ve denetleyicisi kullanarak geçirilen [model bağlayıcı](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Denetleyici verileri paketleri bir `ViewBag` nesne ve Görünüm nesnesi geçer. Görünüm ardından veriler kullanıcıya HTML olarak görüntüler.

![](adding-a-view/_static/image12.png)

Yukarıdaki örnekte kullandık bir `ViewBag` denetleyicisinden bir görünüme veri iletmek için nesne. Öğreticinin sonraki bölümlerinde bir denetleyiciden bir görünüme veri iletmek için bir görünüm modeli kullanacağız. Veri geçirme görünüm modeli yaklaşım görünümü paketi üzerinde genellikle çok tercih edilen yaklaşımdır. Blog girişine bakın [dinamik V kesin türü belirtilmiş görünümleri](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) daha fazla bilgi için. 

Bir gelen kutusu, bir &quot;M&quot; modeli, ancak veritabanı türü değil. Ne biz öğrendiniz ve film veritabanı oluşturma ele alalım.

> [!div class="step-by-step"]
> [Önceki](adding-a-controller.md)
> [İleri](adding-a-model.md)
