---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Görünüm (C#) ekleme | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack, 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 9fc8c6cad44016511c462b4206c28ea3449ff97e
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576618"
---
<a name="adding-a-view-c"></a>Görünüm (C#) ekleme
====================
Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Bu öğreticide güncelleştirilmiş bir sürümü kullanılabilir [burada](../../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Bu, daha güvenli ve izlemek çok daha kolay ve daha fazla özelliklerini gösterir.
> 
> 
> Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz bir Microsoft Visual Studio sürümü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun. Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> C# kaynak kodu içeren bir Visual Web Developer proje, bu konuya eşlik etmek üzere kullanılabilir. [C# sürümü indirme](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Visual Basic tercih ederseniz, geçiş [Visual Basic sürümü](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.


Bu bölümde, değiştirilecek yedekleyeceksiniz `HelloWorldController` sınıfının şablon dosyalarını indrebilirsiniz kapsülleyen bir istemci HTML yanıtlarını oluşturma işlemi görünümünü kullanın.

Kullanarak yeni bir görünüm şablon dosyası oluşturacaksınız [Razor görünüm altyapısı](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) ASP.NET MVC 3 ile sunulan. Razor tabanlı bir görünüm şablonları bir *.cshtml* dosya uzantısı ve C# kullanarak çıktısını HTML oluşturmak için zarif bir yol sağlar. Razor karakterler ve bir şablonu görüntüleme yazarken gerekli tuş vuruşları sayısını en aza indirir ve iş akışı kodlama daha hızlı bir akış sağlar.

Başlangıç görünümü şablonuyla kullanarak `Index` yönteminde `HelloWorldController` sınıfı. Şu anda `Index` yöntemi controller sınıfında sabit kodlu olduğunu belirten bir ileti içeren bir dize döndürür. Değişiklik `Index` döndürülecek yöntemi bir `View` aşağıda gösterildiği gibi nesne:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Bu kod, tarayıcının bir HTML yanıtı oluşturmak için bir şablonu görüntüleme kullanır. Proje ile kullanabileceğiniz bir şablonu görüntüleme Ekle `Index` yöntemi. İçinde Bunu yapmak için sağ `Index` yöntemi ve tıklatın **Görünüm Ekle**.

![](adding-a-view/_static/image1.png)

**Görünüm Ekle** iletişim kutusu görüntülenir. Bunlar ve tıklayın gibi varsayılan değerleri bırakın **Ekle** düğmesi:

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld* klasörü ve *MvcMovie\Views\HelloWorld\Index.cshtml* dosyası oluşturulur. Bunları gördüğünüz **Çözüm Gezgini**:

![](adding-a-view/_static/image3.png)

Aşağıdaki gösterildiği *Index.cshtml* oluşturulan dosya:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Bazı HTML altında ekleme `<h2>` etiketi. Değiştirilmiş *MvcMovie\Views\HelloWorld\Index.cshtml* dosya aşağıda gösterilmektedir.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Uygulamayı çalıştırmak ve göz atın `HelloWorld` denetleyici (`http://localhost:xxxx/HelloWorld`). `Index` Denetleyicinizin yöntemi kadar iş yapmak istemediğiniz; yalnızca bir deyim çalıştırdığınız `return View()`, hangi belirtilen tarayıcı yanıt işlemek için yöntemi bir görünüm şablon dosyası kullanmanız gerekir. Kullanılacak görünüm şablon dosyası adı açıkça belirtmediğiniz için ASP.NET MVC kullanarak varsayılan *Index.cshtml* görünümü dosyasında *\Views\HelloWorld* klasör. Aşağıdaki resimde, görünümünde sabit kodlanmış bir dize göstermez.

![](adding-a-view/_static/image6.png)

Oldukça iyi görünüyor. Ancak, "Index" tarayıcının başlık çubuğunda diyor ve büyük başlığı sayfasında "MVC Uygulamam." ifadesini içeren dikkat edin Bu değiştirelim.

## <a name="changing-views-and-layout-pages"></a>Görünümlere ve sayfalara düzenini değiştirme

İlk olarak, sayfanın üstündeki "MVC Uygulamam" başlığını değiştirmek istiyorsunuz. Metnin her sayfaya yaygındır. Uygulamadaki her sayfada görünmesine rağmen gerçekte projesinde, yalnızca tek bir yerde uygulanır. Git */görünümler/paylaşılan* klasöründe **Çözüm Gezgini** açın  *\_Layout.cshtml* dosya. Bu dosya adında bir *düzen sayfası* ve "diğer tüm sayfalar kullanan paylaşılan Kabuk" olur.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Düzen şablonları, tek bir yerde sitenizin HTML kapsayıcı düzenini belirtin ve sitenizde birden çok sayfada geçerli olanak sağlar. Not `@RenderBody()` dosyasının alt kısmına yakın bir satır. `RenderBody` Burada oluşturduğunuz tüm görünüm özgü sayfa, "Düzen sayfası içinde sarmalanmış" gösteri bir yer tutucudur. Başlık başlığı Düzen şablonunu "My MVC uygulamasında" için "MVC film uygulaması" olarak değiştirin.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Uygulamayı çalıştırmak ve artık "MVC film uygulaması" yazacaktır dikkat edin. Tıklayın **hakkında** bağlantı ve bkz bu sayfa "MVC film uygulaması" çok gösterilmektedir. Biz Düzen şablonda değişiklik kez sunmayı başarabilseydiniz nasıl olurdu ve sahip sitesindeki tüm sayfalara yansıtan yeni başlığı.

![](adding-a-view/_static/image9.png)

Tam  *\_Layout.cshtml* dosya aşağıda gösterilmektedir:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Şimdi, dizin sayfası (view) başlığının değiştirelim.

Açık *MvcMovie\Views\HelloWorld\Index.cshtml*. Değişiklik yapmak için iki yerde vardır: ilk olarak, metni görüntülenir tarayıcı başlık ve ikincil üst bilgisindeki ( `<h2>` öğesi). Uygulamanın hangi parçası hangi bit kod değişiklikleri görebilmeniz için biraz daha farklı yapmanız.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

HTML Başlığı görüntülemek için kod kümeleri üstündeki belirtmek için bir `Title` özelliği `ViewBag` nesne (içinde *Index.cshtml* şablonu görüntüleme). Düzen şablonunu kaynak koda geri bakarsanız, şablon bu değeri kullandığını fark edeceksiniz `<title>` öğesi bir parçası olarak `<head>` HTML bölümü. Bu yaklaşımı kullanarak, kolayca diğer parametreler şablonu görüntüleme ve düzeni, dosyanızı arasında geçirebilirsiniz.

Uygulamayı çalıştırmak ve göz atın `http://localhost:xx/HelloWorld`. Tarayıcı başlığı, başlığı birincil ve ikincil başlıklar değiştirildi dikkat edin. (Tarayıcı değişiklikleri görmüyorsanız, önbelleğe alınmış içerikleri görüntülüyor olabilirsiniz. Tarayıcınızda yüklenmesine sunucudan yanıt zorlamak için CTRL + F5'e basın.)

Ayrıca dikkat edin nasıl içeriği *Index.cshtml* görünüm şablonu ile birleştirilmiş  *\_Layout.cshtml* şablonu görüntüleme ve tek bir HTML yanıtını tarayıcıya gönderilen. Düzen şablonları gerçekten tüm uygulamanızdaki sayfaların uygulanacak değişiklikler yapmak kolaylaştırır.

![](adding-a-view/_static/image10.png)

(Bu durumda "Hello bizim görünümü şablondan!", "veri" az bizim bit ileti) sabit kodlanmıştır, ancak. MVC uygulaması bir "V" (view) ve "C" (denetleyicisi), ancak hiçbir "M" (modeli) henüz kendinizi. Kısa bir süre sonra nasıl alacağız bir veritabanı oluşturur ve model verileri alabilirsiniz.

## <a name="passing-data-from-the-controller-to-the-view"></a>Görünüm denetleyicisinden veri geçirme

Bir veritabanına gidin ve modelleri hakkında konuşmak önce ilk görünümü denetleyicisi bilgi geçirme hakkında konuşalım. Denetleyici sınıflarına gelen bir URL isteğine yanıt olarak çağrılır. Burada gelen parametreleri işler, bir veritabanından verileri alır ve sonuçta ne tür bir tarayıcıya gönderilecek yanıt verirse kodu yazdığınız bir denetleyici sınıftır. Görünüm şablonları oluşturmak ve bir HTML yanıtını tarayıcıya biçimlendirmek için bir denetleyiciden sonra kullanılabilir.

Denetleyicileri, seçtiğiniz veri veya nesneleri tarayıcı yanıt işlemek bir şablonu görüntüleme için sırayla gerekli sağlamaktan sorumludur. Şablonu Görüntüle hiçbir zaman iş mantığını gerçekleştirmek veya bir veritabanıyla doğrudan etkileşim gerekir. Bunun yerine, yalnızca kendisine denetleyici tarafından sağlanan verilerle çalışmalıdır. Bu "ayrımı" bakımı, kodunuzu temiz ve daha sürdürülebilir tutmaya yardımcı olur.

Şu anda `Welcome` eylem yönteminde `HelloWorldController` sınıfı alır bir `name` ve `numTimes` parametresi ve çıkışları doğrudan tarayıcıya değerleri. Bu yanıt dize olarak işleme denetleyiciniz yerine denetleyici görünüm şablonu kullanmayı değiştirelim. Şablonu Görüntüle yanıtı oluşturmak için uygun veri bitleri denetleyicisinden görünüme iletmek gerektiği anlamına gelir dinamik bir yanıt oluşturur. Şablonu görüntüleme, gereken dinamik verilerinizden denetleyicisi sağlayarak bunu yapabilirsiniz bir `ViewBag` görünüm şablonu erişebiliyorsa nesne.

Geri dönüp *HelloWorldController.cs* dosya ve değiştirme `Welcome` yöntemi eklemek için bir `Message` ve `NumTimes` değerini `ViewBag` nesne. `ViewBag` inovasyonunuz ne olursa olsun istediğiniz şekilde koyabilirsiniz anlamına gelir. bir dinamik Nesne sınıflandırabilirsiniz. `ViewBag` nesnesi içine yerleştirin kadar tanımlı hiçbir özellik sahiptir. Tam *HelloWorldController.cs* dosya şu şekilde görünür:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Artık `ViewBag` nesnesi görünüme otomatik olarak iletilen verileri içerir.

Ardından, bir şablonu Hoş Geldiniz görüntüleme gerekir! İçinde **hata ayıklama** menüsünde **derleme MvcMovie** proje derlendiğinde emin olmak için.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

İçine sağ tıklayın `Welcome` yöntemi ve tıklatın **Görünüm Ekle**. İşte **Görünüm Ekle** gibi görünen iletişim kutusunda:

![](adding-a-view/_static/image13.png)

Tıklayın **Ekle**ve ardından aşağıdaki kodu ekleyin `<h2>` yeni öğe *Welcome.cshtml* dosya. "Hello" ifadesini içeren birden çok kez olarak kullanıcı,'ın belirttiği gibi bir döngü oluşturursunuz. Tam *Welcome.cshtml* dosya aşağıda gösterilmektedir.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Uygulamayı çalıştırın ve aşağıdaki URL'ye gidin:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Artık veriler URL'den gerçekleştirilen ve denetleyiciye otomatik olarak geçirildi. Denetleyici verileri paketleri bir `ViewBag` nesne ve Görünüm nesnesi geçer. Görünüm ardından veriler kullanıcıya HTML olarak görüntüler.

![](adding-a-view/_static/image14.png)

İyi modeli, ancak veritabanı türü için "M" bir tür olan. Ne biz öğrendiniz ve film veritabanı oluşturma ele alalım.

> [!div class="step-by-step"]
> [Önceki](adding-a-controller.md)
> [İleri](adding-a-model.md)
