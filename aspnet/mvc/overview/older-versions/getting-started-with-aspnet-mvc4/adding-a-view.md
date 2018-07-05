---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Görünüm ekleme | Microsoft Docs
author: Rick-Anderson
description: 'Not: Bu öğretici güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu, daha güvenli ve izleyin ve tanıtım çok daha kolay...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 0974cd2e06ed86c736214944a29a5a1eab8af50b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391515"
---
<a name="adding-a-view"></a>Görünüm ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğreticide güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Bu, daha güvenli ve izlemek çok daha kolay ve daha fazla özelliklerini gösterir.


Bu bölümde, değiştirilecek yedekleyeceksiniz `HelloWorldController` sınıfının şablon dosyalarını indrebilirsiniz kapsülleyen bir istemci HTML yanıtlarını oluşturma işlemi görünümünü kullanın.

Görünüm şablonu kullanarak dosyanın oluşturacaksınız [Razor görünüm altyapısı](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) ASP.NET MVC 3 ile sunulan. Razor tabanlı bir görünüm şablonları bir *.cshtml* dosya uzantısı ve C# kullanarak çıktısını HTML oluşturmak için zarif bir yol sağlar. Razor karakterler ve bir şablonu görüntüleme yazarken gerekli tuş vuruşları sayısını en aza indirir ve iş akışı kodlama daha hızlı bir akış sağlar.

Şu anda `Index` yöntemi controller sınıfında sabit kodlu olduğunu belirten bir ileti içeren bir dize döndürür. Değişiklik `Index` döndürülecek yöntemi bir `View` aşağıdaki kodda gösterildiği gibi nesne:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

`Index` Yukarıdaki yöntemi, bir tarayıcıya HTML yanıtı oluşturmak için bir görünüm şablonu kullanır. Denetleyici yöntemlerinde (olarak da bilinen [eylem yöntemleri](http://rachelappel.com/asp.net-mvc-actionresults-explained)), gibi `Index` yukarıdaki yöntemi genellikle iade bir [actionresult öğesini](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (veya türetilmiş bir sınıf [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), gibi dize olmayan ilkel türler.

Proje ile kullanabileceğiniz bir şablonu görüntüleme Ekle `Index` yöntemi. İçinde Bunu yapmak için sağ `Index` yöntemi ve tıklatın **Görünüm Ekle**.

![](adding-a-view/_static/image1.png)

**Görünüm Ekle** iletişim kutusu görüntülenir. Bunlar ve tıklayın gibi varsayılan değerleri bırakın **Ekle** düğmesi:

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld* klasörü ve *MvcMovie\Views\HelloWorld\Index.cshtml* dosyası oluşturulur. Bunları gördüğünüz **Çözüm Gezgini**:

![](adding-a-view/_static/image3.png)

Aşağıdaki gösterildiği *Index.cshtml* oluşturulan dosya:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Aşağıdaki HTML'yi altında ekleme `<h2>` etiketi.

[!code-html[Main](adding-a-view/samples/sample2.html)]

Tam *MvcMovie\Views\HelloWorld\Index.cshtml* dosya aşağıda gösterilmektedir.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Çözüm Gezgini'nde Visual Studio 2012 kullanıyorsanız, sağ tıklayın *Index.cshtml* seçin ve dosya **sayfa denetçisi görünümünde**.

![PI](adding-a-view/_static/image5.png)

[Sayfa denetçisi öğretici](../../views/using-page-inspector-in-aspnet-mvc.md) bu yeni aracı hakkında daha fazla bilgi yer almaktadır.

Alternatif olarak, uygulamayı çalıştırmak ve göz atın `HelloWorld` denetleyici (`http://localhost:xxxx/HelloWorld`). `Index` Denetleyicinizin yöntemi kadar iş yapmak istemediğiniz; yalnızca bir deyim çalıştırdığınız `return View()`, hangi belirtilen tarayıcı yanıt işlemek için yöntemi bir görünüm şablon dosyası kullanmanız gerekir. Kullanılacak görünüm şablon dosyası adı açıkça belirtmediğiniz için ASP.NET MVC kullanarak varsayılan *Index.cshtml* görünümü dosyasında *\Views\HelloWorld* klasör. Dize aşağıdaki resimde gösterilmektedir &quot;bizim görünümü şablondan Merhaba!&quot; görünümünde sabit kodlanmış.

![](adding-a-view/_static/image6.png)

Oldukça iyi görünüyor. Tarayıcının başlık çubuğunda gösterilir, ancak, fark &quot;My ASP.NET bir dizin&quot; ve sayfanın üstündeki büyük bağlantı diyor &quot;logonuz buraya gelir.&quot; Aşağıda &quot;, logo burada.&quot; bağlantı kayıt ve günlük bağlantılar ve giriş, bağlanan aşağıda hakkında ve kişi sayfaları. Bunlardan bazıları değiştirelim.

## <a name="changing-views-and-layout-pages"></a>Görünümlere ve sayfalara düzenini değiştirme

İlk olarak, değiştirmek istediğiniz &quot;, logo burada.&quot; sayfanın üst kısmındaki başlık. Metnin her sayfaya yaygındır. Uygulamadaki her sayfada görünmesine rağmen gerçekte projesinde, yalnızca tek bir yerde uygulanır. Git */görünümler/paylaşılan* klasöründe **Çözüm Gezgini** açın  *\_Layout.cshtml* dosya. Bu dosya adında bir *düzen sayfası* ve paylaşılan &quot;Kabuk&quot; , diğer tüm sayfalar kullanın.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Düzen şablonları, tek bir yerde sitenizin HTML kapsayıcı düzenini belirtin ve sitenizde birden çok sayfada geçerli olanak sağlar. Bulma `@RenderBody()` satır. `RenderBody` olduğundan burada tüm görünüm özel sayfalar, yer tutucu oluşturma Göster, &quot;sarmalanmış&quot; Düzen sayfasında. Örneğin, hakkında bağlantısını seçerseniz *Views\Home\About.cshtml* görünümü içinde işlenir `RenderBody` yöntemi.

Düzen şablonu site başlığı başlığı değiştirmek &quot;buraya logonuz&quot; için &quot;MVC film&quot;.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Title öğesi içeriğini aşağıdaki biçimlendirme ile değiştirin:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Artık diyor dikkat edin ve uygulama çalıştırma &quot;MVC film &quot;. Tıklayın **hakkında** bağlantı ve bkz bu sayfaya nasıl görüneceğini &quot;MVC film&quot;, çok. Biz Düzen şablonda değişiklik kez sunmayı başarabilseydiniz nasıl olurdu ve sahip sitesindeki tüm sayfalara yansıtan yeni başlığı.

![](adding-a-view/_static/image8.png)

Şimdi, dizin görünümünün başlık değiştirelim.

Açık *MvcMovie\Views\HelloWorld\Index.cshtml*. Değişiklik yapmak için iki yerde vardır: ilk olarak, metni görüntülenir tarayıcı başlık ve ikincil üst bilgisindeki ( `<h2>` öğesi). Uygulamanın hangi parçası hangi bit kod değişiklikleri görebilmeniz için biraz daha farklı yapmanız.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

HTML Başlığı görüntülemek için kod kümeleri üstündeki belirtmek için bir `Title` özelliği `ViewBag` nesne (içinde *Index.cshtml* şablonu görüntüleme). Düzen şablonunu kaynak koda geri bakarsanız, şablon bu değeri kullandığını fark edeceksiniz `<title>` öğesi bir parçası olarak `<head>` bölümünde daha önce değiştirilen HTML. Bunu kullanarak `ViewBag` yaklaşımı, kolayca geçirebilirsiniz diğer parametreler, Düzen dosyası şablonu görüntüleme arasında.

Uygulamayı çalıştırmak ve göz atın `http://localhost:xx/HelloWorld`. Tarayıcı başlığı, başlığı birincil ve ikincil başlıklar değiştirildi dikkat edin. (Tarayıcı değişiklikleri görmüyorsanız, önbelleğe alınmış içerikleri görüntülüyor olabilirsiniz. Tarayıcınızda yüklenmesine sunucudan yanıt zorlamak için CTRL + F5'e basın.) Tarayıcı başlığı ile oluşturulan `ViewBag.Title` biz kümesinde *Index.cshtml* şablonu ve ek görüntüleme &quot;-film uygulaması&quot; Düzen dosyasında eklendi.

Ayrıca dikkat edin nasıl içeriği *Index.cshtml* görünüm şablonu ile birleştirilmiş  *\_Layout.cshtml* şablonu görüntüleme ve tek bir HTML yanıtını tarayıcıya gönderilen. Düzen şablonları gerçekten tüm uygulamanızdaki sayfaların uygulanacak değişiklikler yapmak kolaylaştırır.

![](adding-a-view/_static/image9.png)

Bizim küçük bit &quot;veri&quot; (Bu durumda &quot;bizim görünümü şablondan Merhaba!&quot; ileti) sabit kodlanmıştır, ancak olduğu. MVC uygulama bir &quot;V&quot; (view) ve kendinizi bir &quot;C&quot; (denetleyicisi), ancak hiçbir &quot;M&quot; (henüz model). Kısa bir süre sonra nasıl alacağız bir veritabanı oluşturur ve model verileri alabilirsiniz.

## <a name="passing-data-from-the-controller-to-the-view"></a>Görünüm denetleyicisinden veri geçirme

Bir veritabanına gidin ve modelleri hakkında konuşmak önce ilk görünümü denetleyicisi bilgi geçirme hakkında konuşalım. Denetleyici sınıflarına gelen bir URL isteğine yanıt olarak çağrılır. Denetleyici sınıfı Burada, istekleri, bir veritabanından veri alır ve sonuçta ne tür bir tarayıcıya gönderilecek yanıt verirse gelen tarayıcı işleyen kodu yazdığınız yerdedir. Görünüm şablonları oluşturmak ve bir HTML yanıtını tarayıcıya biçimlendirmek için bir denetleyiciden sonra kullanılabilir.

Denetleyicileri, seçtiğiniz veri veya nesneleri tarayıcı yanıt işlemek bir şablonu görüntüleme için sırayla gerekli sağlamaktan sorumludur. En iyi yöntem: **şablonu görüntüle hiçbir zaman iş mantığını gerçekleştirebilirsiniz veya bir veritabanıyla doğrudan etkileşim**. Bunun yerine, bir şablonu görüntüleme için denetleyici tarafından sağlanan veri ile çalışması gerekir. Bu koruma &quot;görev ayrımı nettir&quot; yardımcı olur, kodunuzu temiz, test edilebilir ve daha sürdürülebilir tutun.

Şu anda `Welcome` eylem yönteminde `HelloWorldController` sınıfı alır bir `name` ve `numTimes` parametresi ve çıkışları doğrudan tarayıcıya değerleri. Bu yanıt dize olarak işleme denetleyiciniz yerine denetleyici görünüm şablonu kullanmayı değiştirelim. Şablonu Görüntüle yanıtı oluşturmak için uygun veri bitleri denetleyicisinden görünüme iletmek gerektiği anlamına gelir dinamik bir yanıt oluşturur. Şablonu görüntüleme, gereken dinamik (Parametreler) verilerinizden denetleyicisi sağlayarak bunu yapabilirsiniz bir `ViewBag` görünüm şablonu erişebiliyorsa nesne.

Geri dönüp *HelloWorldController.cs* dosya ve değiştirme `Welcome` yöntemi eklemek için bir `Message` ve `NumTimes` değerini `ViewBag` nesne. `ViewBag` inovasyonunuz ne olursa olsun istediğiniz şekilde koyabilirsiniz anlamına gelir. bir dinamik Nesne sınıflandırabilirsiniz. `ViewBag` nesnesi içine yerleştirin kadar tanımlı hiçbir özellik sahiptir. [ASP.NET MVC model bağlama sistemi](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) adlandırılmış parametreleri otomatik olarak eşlenir (`name` ve `numTimes`) Sorgu dizesinden yönteminizi parametrelere adres çubuğundaki. Tam *HelloWorldController.cs* dosya şu şekilde görünür:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Artık `ViewBag` nesnesi görünüme otomatik olarak iletilen verileri içerir.

Ardından, bir şablonu Hoş Geldiniz görüntüleme gerekir! İçinde **derleme** menüsünde **derleme MvcMovie** proje derlendiğinde emin olmak için.

İçine sağ tıklayın `Welcome` yöntemi ve tıklatın **Görünüm Ekle**.

![](adding-a-view/_static/image10.png)

İşte **Görünüm Ekle** gibi görünen iletişim kutusunda:

![](adding-a-view/_static/image11.png)

Tıklayın **Ekle**ve ardından aşağıdaki kodu ekleyin `<h2>` yeni öğe *Welcome.cshtml* dosya. Bildiren bir döngü oluşturursunuz &quot;Hello&quot; sayıda kullanıcı olması gerektiği söyler. Tam *Welcome.cshtml* dosya aşağıda gösterilmektedir.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Uygulamayı çalıştırın ve aşağıdaki URL'ye gidin:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Şimdi veri URL'den gerçekleştirilen ve denetleyicisi kullanarak geçirilen [model bağlayıcı](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Denetleyici verileri paketleri bir `ViewBag` nesne ve Görünüm nesnesi geçer. Görünüm ardından veriler kullanıcıya HTML olarak görüntüler.

![](adding-a-view/_static/image12.png)

Yukarıdaki örnekte kullandık bir `ViewBag` denetleyicisinden bir görünüme veri iletmek için nesne. İkinci öğreticide, bir görünüm modeli bir denetleyiciden bir görünüme veri iletmek için kullanacağız. Veri geçirme görünüm modeli yaklaşım görünümü paketi üzerinde genellikle çok tercih edilen yaklaşımdır. Blog girişine bakın [dinamik V kesin türü belirtilmiş görünümleri](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) daha fazla bilgi için.

Bir gelen kutusu, bir &quot;M&quot; modeli, ancak veritabanı türü değil. Ne biz öğrendiniz ve film veritabanı oluşturma ele alalım.

> [!div class="step-by-step"]
> [Önceki](adding-a-controller.md)
> [İleri](adding-a-model.md)
