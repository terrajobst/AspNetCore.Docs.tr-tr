---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Görünüm (VB) ekleme | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack, 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 840f925aea40963c45787a8b5af20e99ff31f3d8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386792"
---
<a name="adding-a-view-vb"></a>Görünüm (VB) ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz bir Microsoft Visual Studio sürümü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun. Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Bu konuya eşlik etmek üzere bir Visual Web Developer proje VB.NET kaynak koduyla birlikte kullanılabilir. [VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). C# tercih ederseniz, geçiş [C# sürümü](../cs/adding-a-view.md) Bu öğreticinin.


Bu bölümde bunu değiştirmek için dağıtacağız `HelloWorldController` düzenleyebilmeleri için Görünüm şablonu dosyasını kullanmak için sınıf kapsülleyen bir istemci HTML yanıtlarını oluşturma işlemi.

Görünüm şablonu ile kullanarak başlayalım `Index` yönteminde `HelloWorldController` sınıfı. Şu anda `Index` yöntemi controller sınıfı içinde sabit kodlanmış olduğunu belirten bir ileti ile bir dize döndürür. Değişiklik `Index` döndürülecek yöntemi bir `View` aşağıda gösterildiği gibi nesne:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Artık bir şablonu görüntüleme ile çağırabileceği Projemizin için ekleyelim `Index` yöntemi. İçinde Bunu yapmak için sağ `Index` yöntemi ve tıklatın **Görünüm Ekle**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

**Görünüm Ekle** iletişim kutusu görüntülenir. Varsayılan girişleri bırakabilir ve tıklayın **Ekle** düğmesi.

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

*MvcMovie\Views\HelloWorld* klasörü ve *MvcMovie\Views\HelloWorld\Index.vbhtml* dosyası oluşturulur. Bunları gördüğünüz **Çözüm Gezgini**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Bazı HTML altında ekleme `<h2>` etiketi. Değiştirilmiş *MvcMovie\Views\HelloWorld\Index.vbhtml* dosya aşağıda gösterilmektedir.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Uygulamayı çalıştırmak ve göz atın &quot;Merhaba Dünya&quot; denetleyici (`http://localhost:xxxx/HelloWorld`). `Index` Denetleyicinizin yöntemi kadar iş yapmak istemediğiniz; yalnızca bir deyim çalıştırdığınız `return View()`, hangi belirtilen bir yanıtı istemciye işlemek için bir görünüm şablon dosyası kullanılacak istedik. Biz kullanılacak görünüm şablon dosyası adı açıkça belirtmediği için ASP.NET MVC kullanarak varsayılan *Index.vbhtml* görünüm dosyası içinde *\Views\HelloWorld* klasör. Aşağıdaki resimde, görünümünde sabit kodlanmış bir dize göstermez.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Oldukça iyi görünüyor. Ancak, tarayıcınızın başlık çubuğunda gösterdiğini fark &quot;dizin&quot; ve büyük başlığı sayfasında diyor &quot;MVC Uygulamam.&quot; Bu değiştirelim.

## <a name="changing-views-and-layout-pages"></a>Görünümlere ve sayfalara düzenini değiştirme

İlk olarak, metin değiştirelim &quot;MVC Uygulamam.&quot; Bu metin, paylaşılan ve her sayfada görüntülenir. Uygulamamızı her sayfasında olsa bile gerçekten Projemizin, yalnızca tek bir yerde görünür. Git */görünümler/paylaşılan* klasöründe **Çözüm Gezgini** açın  *\_Layout.vbhtml* dosya. Bu dosya bir düzen sayfası olarak adlandırılır ve paylaşılan olan &quot;Kabuk&quot; , diğer tüm sayfalar kullanın.

Not `@RenderBody()` dosyasının alt kısmına yakın bir kod satırı. `RenderBody` Burada oluşturduğunuz tüm sayfalar gösterir, bir yer tutucudur &quot;sarmalanmış&quot; Düzen sayfasında. Değişiklik `<h1>` gelen başlık **&quot;** MVC Uygulamam&quot; için &quot;MVC film uygulaması&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Uygulamayı çalıştırmak ve artık diyor Not &quot;MVC film uygulaması&quot;. Tıklayın **hakkında** bağlantısı ve sayfa gösterir &quot;MVC film uygulaması&quot;, çok.

Tam  *\_Layout.vbhtml* dosya aşağıda gösterilmektedir:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Şimdi, dizin sayfası (view) başlığının değiştirelim.

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Açık *MvcMovie\Views\HelloWorld\Index.vbhtml*. Değişiklik yapmak için iki yerde vardır: ilk olarak, metni görüntülenir tarayıcı başlık ve ikincil üst bilgisindeki ( `<h2>` öğesi). Uygulamanın hangi parçası hangi bit kod değişiklikleri görebilmeniz için bunları biraz farklı oluşturacağız.

Uygulamayı çalıştırmak ve göz atın`http://localhost:xx/HelloWorld`. Tarayıcı başlığı, başlığı birincil ve ikincil başlıklar değiştirildi dikkat edin. Bir görünüme uygulamanızda küçük değişiklikler ile büyük bir değişiklik yapmak kolay bir işlemdir. (Tarayıcı değişiklikleri görmüyorsanız, önbelleğe alınmış içerikleri görüntülüyor olabilirsiniz. Tarayıcınızda yüklenmesine sunucudan yanıt zorlamak için CTRL + F5'e basın.)

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Bizim küçük bit &quot;veri&quot; (Bu durumda &quot;Merhaba Dünya!&quot; ileti) sabit kodlanmıştır, ancak olduğu. MVC uygulamamıza V (görünümleri) varsa ve C (denetleyiciler), ancak henüz hiçbir M (modeli) yapılandırdığımıza göre. Kısa bir süre sonra nasıl alacağız bir veritabanı oluşturur ve model verileri alabilirsiniz.

## <a name="passing-data-from-the-controller-to-the-view"></a>Görünüm denetleyicisinden veri geçirme

Bir veritabanına gidin ve modelleri hakkında konuşmak önce ilk görünümü denetleyicisi bilgi geçirme hakkında konuşalım. Şablonu Görüntüle bir HTML yanıtı istemciye işlemek için gerektirdiği istiyoruz. Bu nesneler genellikle oluşturulur ve bir görünüm şablonu için bir denetleyici sınıfı tarafından geçirilen ve görünüm şablonu gerektiren veri içermesi gereken — fazlasını.

Daha önce ile `HelloWorldController` sınıfı `Welcome` eylem yöntemine geçen bir `name` ve `numTimes` parametre ve parametre değerleri tarayıcıya sonra çıktı. Yerine bu yanıt doğrudan işlemeye devam denetleyiciniz daha şimdi bunun yerine size bu verileri bir Torba içinde görünümünü giriyorum. Denetleyicileri ve görünümleri yeniden kullanabileceğiniz bir `ViewBag` verileri tutacak nesne. Bir görünüm şablona geçirilen otomatik olarak ve paket içeriğini kullanarak verileri HTML yanıtını işlemek için kullanılan. Bu şekilde bir şey ve başka bir görünüm şablonu ile ilgilenir denetleyici — bize temiz korumak etkinleştirme &quot;görev ayrımı nettir&quot; uygulama içinde.

Alternatif olarak, biz özel bir sınıf tanımlayın, ardından bizim kendi söz konusu nesne örneği oluşturun, verilerle doldurmak ve görünüme iletmek. Özel bir Model görünüm için olduğundan, genellikle bir ViewModel olarak adlandırılır. Bununla birlikte, küçük miktarlarda veri için ViewBag mükemmel çalışıyor.

Geri dönüp *HelloWorldController.vb* dosya değişikliği `Welcome` NumTimes ve ileti ViewBag yerleştirmek için denetleyici içinde yöntemi. Görünüm Paketi dinamik bir nesnedir. İnovasyonunuz ne olursa olsun istediğiniz koyabilirsiniz anlamına gelir. İçindeki Office'te kadar ViewBag, tanımlanmış özelliği yok.

Tam `HelloWorldController.vb` yeni sınıfı aynı dosyada ile.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Artık bizim ViewBag görünümüne otomatik olarak geçirilecek veri içerir. Biz beğenmediğinizi varsa yeniden alternatif olarak Biz bu gibi kendi nesne geçirilen:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Şimdi yapmamız gereken bir `WelcomeView` şablon! Yeni kod derlenir için uygulamayı çalıştırın. Tarayıcıyı kapatın, içinde sağ `Welcome` yöntemi ve ardından **Görünüm Ekle**.

İşte, **Görünüm Ekle** gibi iletişim kutusu görünür.

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Altında aşağıdaki kodu ekleyin `<h2>` yeni öğe <em>Hoş Geldiniz.</em> vbhtml dosyası. Biz bir döngü yapmak ve söyleyin &quot;Hello&quot; sayıda kullanıcı gelmiş Yazan!

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Uygulamayı çalıştırmak ve göz atın `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Artık veriler URL'den gerçekleştirilen ve denetleyiciye otomatik olarak geçirildi. Denetleyici paketleri verileri bir `Model` nesne ve Görünüm nesnesi geçer. Veriler kullanıcıya HTML olarak görüntülüyor görüntüleyin.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Bir gelen kutusu, bir &quot;M&quot; modeli, ancak veritabanı türü değil. Ne biz öğrendiniz ve film veritabanı oluşturma ele alalım.

> [!div class="step-by-step"]
> [Önceki](adding-a-controller.md)
> [İleri](adding-a-model.md)
