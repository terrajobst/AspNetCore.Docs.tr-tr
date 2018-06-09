---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Bir görünüm (C#) ekleme | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 50ce4a2024ffd9e2bbb5526717052d486689ff38
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30873176"
---
<a name="adding-a-view-c"></a>Bir görünüm (C#) ekleme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğretici güncelleştirilmiş bir sürümü kullanılabilir [burada](../../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Daha güvenli, izlemek çok daha kolaydır ve daha fazla özelliklerini gösterir.
> 
> 
> Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek. Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun. Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> C# kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir. [C# sürümü](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Visual Basic tercih ederseniz, geçiş [Visual Basic sürüm](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.


Bu bölümde değiştirme oluşturacağız `HelloWorldController` sınıfı şablon dosyalarını düzgün bir şekilde kapsülleyen bir istemci HTML yanıtlarını oluşturma işlemine görünümünü kullanın.

Kullanarak yeni bir görünüm şablon dosyası oluşturacaksınız [Razor görüntüleme altyapısı](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) ASP.NET MVC 3 ile sunulmuştur. Razor tabanlı görünümü şablonları bir *.cshtml* dosya uzantısı ve C# kullanarak çıktısını HTML oluşturmak için zarif bir yolunu sağlar. Razor karakter ve bir görünüm şablon yazarken gereken tuş vuruşları sayısını en aza indirir ve iş akışı kodlama hızlı, sıvı sağlar.

Başlangıç şablonu görüntüleme kullanarak `Index` yönteminde `HelloWorldController` sınıfı. Şu anda `Index` yöntemi controller sınıfında sabit kodlanmış olduğunu belirten bir ileti ile bir dize döndürür. Değişiklik `Index` döndürülecek yöntemi bir `View` nesnesi, aşağıda gösterildiği gibi:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Bu kodu tarayıcının bir HTML yanıtı oluşturmak için bir görünüm şablonu kullanır. Proje ile birlikte kullanabileceğiniz bir görünüm şablonu Ekle `Index` yöntemi. Bunu yapmak için sağ tıklatın içinde `Index` yöntemi ve tıklatın **Görünüm Ekle**.

![](adding-a-view/_static/image1.png)

**Görünüm Ekle** iletişim kutusu görüntülenir. Varsayılan değerleri olan ve'ı tıklatın gibi bırakın **Ekle** düğmesi:

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld* klasör ve *MvcMovie\Views\HelloWorld\Index.cshtml* dosyası oluşturulur. Bunları görebilirsiniz **Çözüm Gezgini**:

![](adding-a-view/_static/image3.png)

Aşağıdaki gösterildiği *Index.cshtml* oluşturulmuş dosyası:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Bazı HTML altında eklemek `<h2>` etiketi. Değiştirilen *MvcMovie\Views\HelloWorld\Index.cshtml* dosya aşağıda gösterilmektedir.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Uygulamayı çalıştırın ve Gözat `HelloWorld` denetleyici (`http://localhost:xxxx/HelloWorld`). `Index` Denetleyicinizi yönteminde kadar iş yapmak olmadı; yalnızca bir deyim çalışan `return View()`, hangi belirtilen tarayıcı yanıta işlemek için yöntemi bir görünüm şablon dosyası kullanmanız gerekir. Kullanılacak görünüm şablonu dosyasının adı açıkça belirtmediğiniz olduğundan, ASP.NET MVC kullanarak varsayılan *Index.cshtml* görünüm dosyasında *\Views\HelloWorld* klasör. Aşağıdaki görüntü görünümünde sabit kodlanmış dize gösterir.

![](adding-a-view/_static/image6.png)

Oldukça iyi görünür. Ancak, "Index" tarayıcının başlık çubuğunda diyor ve büyük başlık sayfasında "MVC Uygulamam." diyor dikkat edin Bu değiştirelim.

## <a name="changing-views-and-layout-pages"></a>Görünümleri ve Düzen sayfaları değiştirme

İlk olarak, sayfanın üst kısmındaki "MVC Uygulamam" başlığı değiştirmek istediğiniz. Metnin her sayfaya yaygındır. Uygulamadaki her sayfada görünse bile projesinde, yalnızca tek bir yerde gerçekten uygulanır. Git */görünümler/paylaşılan* klasöründe **Çözüm Gezgini** açarak  *\_Layout.cshtml* dosya. Bu dosya adında bir *düzen sayfası* ve "diğer tüm sayfalar kullanmak paylaşılan Kabuğu" olur.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Düzen şablonları, tek bir yerde, sitenizin HTML kapsayıcı düzeni belirtin ve sitenizin birden çok sayfada üzerinden uygulanan olanak sağlar. Not `@RenderBody()` satır dosyanın sonuna yakın. `RenderBody` Burada oluşturduğunuz tüm görünüm özgü sayfaları, "Düzen sayfası Sarmalanan" gösteri bir yer tutucudur. "MVC Uygulamam" "MVC film uygulaması" için Düzen şablonu başlık başlığına değiştirin.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Uygulamayı çalıştırın ve şimdi "MVC film uygulaması" yazacaktır dikkat edin. Tıklatın **hakkında** bağlantı ve bakın nasıl bu sayfa "MVC film uygulaması" çok gösterir. Düzen şablonda değişiklik kez bulduk ve sahip sitesindeki tüm sayfalara yansıtacak yeni başlığı.

![](adding-a-view/_static/image9.png)

Tam  *\_Layout.cshtml* dosya aşağıda gösterilmektedir:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Şimdi, dizin sayfası (Görünüm) başlığı değiştirelim.

Açık *MvcMovie\Views\HelloWorld\Index.cshtml*. Bir değişiklik yapmak için iki yerde vardır: ilk olarak, metin görünür tarayıcının başlık ve ardından ikincil üstbilgisinde ( `<h2>` öğesi). Hangi bölümünün uygulamanın hangi bit kod değişiklikleri görebilmeleri biraz farklı yapmanız.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

HTML Başlığı görüntülemek için ayarlar yukarıdaki kodu belirtmek için bir `Title` özelliği `ViewBag` nesne (olduğu içinde *Index.cshtml* şablonu görüntüle). Düzen şablonunu kaynak kodu geri bakarsanız, şablonu bu değeri kullanır fark edeceksiniz `<title>` öğesi bir parçası olarak `<head>` HTML bölümü. Bu yaklaşımı kullanarak, kolayca diğer parametreleri şablonu görüntüleme ve düzeni dosyanız arasında geçirebilirsiniz.

Uygulamayı çalıştırın ve Gözat `http://localhost:xx/HelloWorld`. Tarayıcı başlığı, birincil başlık ve ikincil başlıklar değişmiş dikkat edin. (Değişiklikleri tarayıcıda görmüyorsanız, önbelleğe alınmış içeriği görüntülüyor olabilirsiniz. Yüklenecek sunucudan yanıt zorlamak için tarayıcınızda CTRL + F5'e basın.)

Ayrıca dikkat edin nasıl içerik *Index.cshtml* görünüm şablonu birleştirilmiş ile  *\_Layout.cshtml* şablonu görüntüleme ve tek bir HTML yanıtını tarayıcıya gönderildi. Düzen şablonları, uygulamanızdaki sayfaların tümünü uygulamak değişiklik gerçekten kolay hale getirir.

![](adding-a-view/_static/image10.png)

Bizim az bitlik (Bu durumda "Hello bizim görünüm şablondan!" "veri" ileti), sabit kodlanmış, ancak. MVC uygulama bir "V" (Görünüm) vardır ve bir "C" (denetleyicisi), ancak hiçbir "M" (modeli) henüz var. Kısa bir süre içinde biz nasıl adım geçireceğiz bir veritabanı oluşturun ve model verileri alabilirsiniz.

## <a name="passing-data-from-the-controller-to-the-view"></a>Denetleyici geçirme verileri görüntülemek için

Bir veritabanına gidin ve modelleri hakkında konuşun önce ancak şimdi ilk bilgi denetleyicisinden bir görünüme geçirme hakkında konuşun. Denetleyici sınıfları, gelen bir URL isteğine yanıt olarak çağrılır. Denetleyici gelen parametreleri işler, bir veritabanından veri alır ve sonuçta ne tür bir tarayıcıya göndermek için yanıt verirse kodu yazma burada sınıfıdır. Görünüm şablonları daha sonra bir denetleyicisinden oluşturur ve tarayıcıya bir HTML yanıtını biçimlendirmek için kullanılabilir.

Denetleyicileri sırada tarayıcı yanıta işlemek bir şablonu görüntüleme için hangi veri veya nesneler gerekli sağlamak için sorumludur. Şablonu görüntüleme hiç iş mantığı gerçekleştirmek veya bir veritabanı ile doğrudan etkileşim gerekir. Bunun yerine, yalnızca denetleyici tarafından sağlanan verilerle çalışmalıdır. "Sorunları ayrımı" Bakımı kodunuzu temiz ve daha rahat tutmaya yardımcı olur.

Şu anda `Welcome` eylem yönteminde `HelloWorldController` sınıfını alır bir `name` ve `numTimes` parametresi ve çıkışları doğrudan tarayıcıya değerleri. Bu yanıt dize olarak işleme denetleyiciniz yerine bir görünüm şablonu kullanmayı denetleyicisi değiştirelim. Şablonu görüntüleme yanıtı oluşturmak için uygun veri bitleri denetleyicisinden görünüme iletmek gerektiği anlamına gelir dinamik bir yanıt oluşturur. Bu şablonu görüntüleme gereksinimlerinize dinamik veri put denetleyicisi sağlayarak yapmak bir `ViewBag` şablonu görüntüle daha sonra erişebilirsiniz nesnesi.

Geri dönüp *HelloWorldController.cs* dosya ve değişiklik `Welcome` ekleme yöntemi bir `Message` ve `NumTimes` değeri `ViewBag` nesnesi. `ViewBag` istediğiniz ona koyabilirsiniz yani dinamik bir nesne değil; `ViewBag` nesnesi içindeki put kadar tanımlı hiçbir özellik sahiptir. Tam *HelloWorldController.cs* dosya şu şekilde görünür:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Şimdi `ViewBag` nesnesi görünümüne otomatik olarak geçirilen verileri içerir.

Ardından, bir Hoş Geldiniz görünüm şablonu gerekiyor! İçinde **hata ayıklama** menüsünde, select **yapı MvcMovie** proje derlenmiş emin olmak için.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

İçinde sağ `Welcome` yöntemi ve tıklatın **Görünüm Ekle**. İşte **Görünüm Ekle** iletişim kutusu görünür gibi:

![](adding-a-view/_static/image13.png)

Tıklatın **Ekle**ve ardından aşağıdaki kodu ekleyin `<h2>` öğesinde yeni *Welcome.cshtml* dosya. "Hello" sayıda olarak gerektiği kullanıcı diyor bildiren bir döngü oluşturacaksınız. Tam *Welcome.cshtml* dosya aşağıda gösterilmektedir.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Uygulamayı çalıştırın ve aşağıdaki URL'ye gidin:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Artık veriler URL'den gerçekleştirilecek ve denetleyiciye otomatik olarak geçirildi. Verileri denetleyicisi paketleri bir `ViewBag` nesnesini ve nesne görünümüne geçirir. Görünümü sonra verileri HTML olarak kullanıcıya görüntüler.

![](adding-a-view/_static/image14.png)

De, "M" türde bir model, ancak veritabanı türü için oluştu. Ne biz öğrendiğinize ve film bir veritabanı oluşturmak atalım.

> [!div class="step-by-step"]
> [Önceki](adding-a-controller.md)
> [sonraki](adding-a-model.md)
