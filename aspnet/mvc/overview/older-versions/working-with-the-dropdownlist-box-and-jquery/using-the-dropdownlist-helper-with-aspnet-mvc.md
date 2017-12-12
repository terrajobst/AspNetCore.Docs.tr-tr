---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: "DropDownList Yardımcısı ASP.NET MVC ile kullanma | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: b8393e1503cb562a46a00f49b51c0cb64ff2cfdc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>ASP.NET MVC ile DropDownList Yardımcısını kullanma
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

Bu öğretici ile çalışma temellerini öğretmek [DropDownList](https://msdn.microsoft.com/en-us/library/dd492948.aspx) Yardımcısı ve [ListBox](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.selectextensions.listbox.aspx) bir ASP.NET MVC Web uygulaması yok. Microsoft Visual Web Developer 2010 Express Service Pack öğreticiyi izlemek için Microsoft Visual Studio ücretsiz sürümü olan 1, kullanabilirsiniz. Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun. Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:

- [Visual Studio Web Developer Express SP1 Önkoşullar](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)<a id="post"></a>
- [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)

Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). Bu öğretici tamamladığınızdan varsayar [ASP.NET MVC giriş](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) öğretici veya[ASP.NET MVC müzik deposu](../mvc-music-store/mvc-music-store-part-1.md) öğretici veya ile ASP.NET MVC geliştirme hakkında bilgi sahibi. Bu öğretici değiştirilmiş bir proje ile başlayan [ASP.NET MVC müzik deposu](../mvc-music-store/mvc-music-store-part-1.md) Öğreticisi. Aşağıdaki bağlantıda starter projeyle indirebilirsiniz [C# sürümü](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

C# kaynak kodu tamamlanmış öğretici Visual Web Developer projeyle bu konuya eşlik etmek kullanılabilir. [Karşıdan](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Ne oluşturacağınız

Eylem yöntemleri ve kullandığınız görünümleri oluşturacaksınız [DropDownList](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) Yardımcısı bir kategori seçin. Ayrıca kullanacağınız **jQuery** (örneğin, tarz veya sanatçı) yeni bir kategori gerektiğinde, kullanılabilen bir INSERT kategori iletişim kutusu eklemek için. Yeni bir tarzını eklemek ve yeni sanatçı eklemek için bağlantıları gösteren Oluştur görünümünün ekran görüntüsü aşağıdadır.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Bilgi edineceksiniz yetenekleri

Öğrenecekleriniz aşağıda verilmiştir:

- Nasıl kullanılacağını [DropDownList](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) Yardımcısı kategori verileri seçin.
- Nasıl ekleneceği bir **jQuery** yeni kategoriler eklemek için iletişim kutusu.

### <a name="getting-started"></a>Başlarken

Başlangıç aşağıdaki bağlantıyı starter projeyle yükleyerek [karşıdan](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). Windows Gezgini'nde, sağ tıklayın *DDL\_Starter.zip* dosya ve Özellikler'i seçin. İçinde **DDL\_Starter.zip özellikleri** iletişim kutusu, select Engellemeyi Kaldır.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

DDL sağ tıklayın\_Starter.zip dosya ve select **tümünü Ayıkla** dosyanın sıkıştırmasını açmak için. Açık *StartMusicStore.sln* Visual Web Developer 2010 Express ("Visual Web Developer" veya kısaca "VWD") veya Visual Studio 2010 ile dosya.

Uygulamayı çalıştırın ve'ı tıklatın CTRL + F5'e basın **Test** bağlantı.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Seçin **film kategorisi seçin (Basit)** bağlantı. Bir film türünü seçin, seçili değer Komedi ile listelenir.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Tarayıcı ve kaynağı Görüntüle'yi seçin, sağ tıklayın. HTML sayfası görüntülenir. Aşağıdaki kod, HTML select öğesi için gösterir.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Seçim listesindeki her öğe bir değer (eylemi için 0, ekranda için 1, 2 Komedi için ve Bilim Kurgu için 3) ve görünen ad (eylem, ekranda, Komedi ve Bilim Kurgu) olduğunu görebilirsiniz. Yukarıdaki kod, seçim listesi için standart HTML'dir.

Seçim listesi ekranda isabet geçip **gönderme** düğmesi. URL tarayıcıda `http://localhost:2468/Home/CategoryChosen?MovieType=1` ve sayfasını görüntüler **seçtiğiniz: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Açık *Controllers\HomeController.cs* dosya ve inceleyin `SelectCategory` yöntemi.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

[DropDownList](https://msdn.microsoft.com/en-us/library/dd492738.aspx) bir HTML select listesindeki oluşturmak için kullanılan yardımcı gerektiren bir **IEnumerable&lt;Selectlistıtem &gt;** , açıkça veya örtük. Diğer bir deyişle, geçirebilirsiniz **IEnumerable&lt;Selectlistıtem &gt;**  açıkça [DropDownList](https://msdn.microsoft.com/en-us/library/dd492738.aspx) yardımcı veya ekleyebilir **IEnumerable&lt; Selectlistıtem &gt;**  için [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) için aynı adı kullanarak **Selectlistıtem** model özelliği olarak. Tümleştirilmesidir **Selectlistıtem** örtük olarak ve açıkça öğreticinin sonraki bölümünde ele alınmıştır. Yukarıdaki kod oluşturma için olası en basit yolu gösteren bir **IEnumerable&lt;Selectlistıtem &gt;**  ve metin ve değerleri ile doldurabilirsiniz. Not `Comedy` [Selectlistıtem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) sahip [seçili](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.selected.aspx) özelliğini **true;** bu göstermek işlenen seçim listesi neden olacak **Komedi** listesinde seçili öğe olarak.

**IEnumerable&lt;Selectlistıtem &gt;**  oluşturulan yukarıdaki eklenen [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) MovieType ada sahip. Biz nasıl geçirmek budur **IEnumerable&lt;Selectlistıtem &gt;**  örtük olarak çok [DropDownList](https://msdn.microsoft.com/en-us/library/dd492738.aspx) aşağıda gösterilen yardımcı.

Açık *Views\Home\SelectCategory.cshtml* dosya ve biçimlendirme inceleyin.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

Üçüncü satır üzerinde düzeni görünümler/paylaşılan ayarlarız/\_basit\_standart düzeni dosyasını basitleştirilmiş bir sürümü Layout.cshtml. Biz görüntü tutmak için bunu ve HTML basit çizilir.

Biz kullanarak verileri gönderir şekilde bu örnekte biz uygulamasının durumu değiştirmiyorsanız bir **HTTP GET**değil **HTTP POST**. W3C bölümüne bakın [seçme HTTP GET veya POST için hızlı denetim listesi](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Olduğundan biz olmayan uygulama değiştirme ve form gönderme kullanırız [Html.BeginForm](https://msdn.microsoft.com/en-us/library/dd460344.aspx) bize eylem yöntemi, denetleyici ve form yöntemini belirtmek izin veren aşırı yüklemesi (**HTTP POST** veya **HTTP GET**). Genellikle görünümleri içeren [Html.BeginForm](https://msdn.microsoft.com/en-us/library/dd505244.aspx) parametre almayan aşırı. Hiçbir parametre sürüm aynı eylem yöntemi ve denetleyicinin POST sürümüne form verileri göndermek için varsayılan olarak ayarlanır.

Aşağıdaki satır

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

bir dize bağımsız değişkeni geçirir **DropDownList** Yardımcısı. Bu dize, örneğimizde "MovieType" iki işlemi yapar:

- İçin anahtar sağlar **DropDownList** bulmaya yardımcı bir **IEnumerable&lt;Selectlistıtem &gt;**  içinde **ViewBag**.
- Bu verilere MovieType form öğesi için bağlı. Gönderme yöntemi ise **HTTP GET**, `MovieType` bir sorgu dizesi olacaktır. Gönderme yöntemi ise **HTTP POST**, `MovieType` ileti gövdesine eklenir. Aşağıdaki resimde, sorgu dizesi değeri 1 olan gösterir.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

Aşağıdaki kodda gösterildiği `CategoryChosen` form için gönderildiği yöntemi.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Test sayfasına geri gidin ve seçin **HTML SelectList** bağlantı. HTML sayfası basit ASP.NET MVC test sayfası için benzer bir select öğesi işler. Tarayıcı penceresinin sağ tıklayın ve **kaynağı görüntüle**. Seçim listesi için HTML biçimlendirmesini temelde aynıdır. Test HTML sayfası, ASP.NET MVC eylem yöntemi ve daha önce test görünümü gibi çalışır.

### <a name="improving-the-movie-select-list-with-enums"></a>Film seçim listesi numaralandırmaları ile geliştirme

Uygulamanızı kategorilerde sabit ve değişmez, kodunuzu daha sağlam ve genişletmek daha basit hale getirmek için numaralandırmaları yararlanabilirsiniz. Yeni bir kategori eklediğinizde, doğru kategori değeri oluşturulur. Yeni bir kategori ekleyin, ancak kategori değeri güncelleştirmek unuttunuz durumlarda Kopyala ve Yapıştır hatalarını önler.

Açık *Controllers\HomeController.cs* dosya ve aşağıdaki kodu inceleyin:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

[Enum](https://msdn.microsoft.com/en-us/library/sbbt4032(VS.80).aspx) `eMovieCategories` dört film türleri yakalar. `SetViewBagMovieType` Yöntemi oluşturur **IEnumerable&lt;Selectlistıtem &gt;**  gelen `eMovieCategories` **enum**ve ayarlar `Selected` özelliği`selectedMovie` parametresi. `SelectCategoryEnum` Eylem yöntemini kullanan aynı görünüm olarak `SelectCategory` eylem yöntemi.

Test sayfasına gidin ve tıklayın `Select Movie Category (Enum)` bağlantı. Bu süre, görüntülenen bir değeri yerine (sayı), enum temsil eden bir dize görüntülenir.

### <a name="posting-enum-values"></a>Enum değerleri gönderme

HTML formları genellikle sunucunun veri göndermek için kullanılır. Aşağıdaki kodda gösterildiği `HTTP GET` ve `HTTP POST` sürümleri `SelectCategoryEnumPost` yöntemi.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Geçirerek bir `eMovieCategories` için enum `POST` yöntemi, biz enum değeri ve enum dize ayıklayabilirsiniz. Örneği çalıştırmak ve Test sayfasına gidin. Tıklayın `Select Movie Category(Enum Post)` bağlantı. Bir filmi türü seçin ve Gönder düğmesine basın. Görüntü hem değer hem de film türünün adını gösterir.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Birden çok bir bölümü Select öğesi oluşturma

[ListBox](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.selectextensions.listbox.aspx) HTML Yardımcısı işler HTML `<select>` öğeyle `multiple` birden çok seçimin yapmalarını sağlayan özniteliği. Test bağlantısı gidin ve ardından **çoklu seçin Ülke** bağlantı. İşlenmiş UI birden fazla ülkede seçmenize olanak sağlar. Aşağıdaki resimde, Kanada ve Çin seçilir.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>MultiSelectCountry kodunu İnceleme

Aşağıdaki kod inceleyin *Controllers\HomeController.cs* dosya.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

`GetCountries` Yöntemi ülkelerin listesi oluşturur, ardından buna ileten `MultiSelectList` Oluşturucusu. `MultiSelectList` Oluşturucusu aşırı kullanılan `GetCountries` yöntemi yukarıdaki dört parametreleri alır:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *öğeleri*: bir [IEnumerable](https://msdn.microsoft.com/en-us/library/system.collections.ienumerable.aspx) listedeki öğeleri içeren. Örnekte, ülkelerin listesinin üstünde.
2. *dataValueField*: bir özellik adı **IEnumerable** değeri içeren liste. Yukarıdaki örnekte `ID` özelliği.
3. *dataTextField*: bir özellik adı **IEnumerable** görüntülemek için bilgi içeren liste. Yukarıdaki örnekte `name` özelliği.
4. *selectedValues*: Seçili değerler listesi.

Yukarıdaki örnekte `MultiSelectCountry` yöntemi geçişleri bir `null` UI görüntülendiğinde hiçbir ülkelerde seçilir şekilde Seçilen ülkelere değeri. Aşağıdaki kod işlemek için kullanılan Razor biçimlendirmeyi gösterir `MultiSelectCountry` görünümü.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

HTML Yardımcısı [ListBox](https://msdn.microsoft.com/en-us/library/dd470200.aspx) yöntemi iki parametre, model bağlama için özellik adını Al kullanılan ve [MultiSelectList](https://msdn.microsoft.com/en-us/library/system.web.mvc.multiselectlist.aspx) seçenekleri belirleyin ve değerlerini içeren. `ViewBag.YouSelected` Yukarıdaki kod formu gönderdiğinde seçtiğiniz ülkelerde değerlerini görüntülemek için kullanılır. HTTP POST aşırı yüklemesini inceleyin `MultiSelectCountry` yöntemi.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

`ViewBag.YouSelected` Dinamik özellik içeriyor için elde Seçilen ülkelere `Countries` form koleksiyonu girişi. Bu sürümde GetCountries yöntemi seçili ülkelerin listesi geçirilen böyle olduğunda `MultiSelectCountry` görünümü görüntülenir, Seçilen ülkelere kullanıcı Arabiriminde seçilir.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Bir seçin öğesi kolay toplama seçilen jQuery eklentisi ile yapma

Toplama [seçilen](http://harvesthq.github.com/chosen/) jQuery eklentisi için bir HTML eklenebilir &lt;seçin&gt; öğesi bir kullanıcı oluşturmak için kolay kullanıcı Arabirimi. Toplama görüntüleri göstermek [seçilen](http://harvesthq.github.com/chosen/) jQuery eklentisiyle `MultiSelectCountry` görünümü.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

Aşağıdaki iki görüntülerinde **Kanada** seçilir.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

Yukarıdaki resimde Kanada seçilir ve içerdiği bir **x** seçimini kaldırmak için tıklatın. Japonya seçili ve aşağıdaki görüntü Kanada, Çin'de gösterir.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Toplama seçilen jQuery eklentisi takma

Aşağıdaki bölümde, jQuery bazı deneyimiyle varsa izleyin kolaydır. JQuery önce hiç kullanmadıysanız, aşağıdaki jQuery öğretilerden birini deneyin isteyebilirsiniz.

- [JQuery nasıl çalıştığını](http://docs.jquery.com/Tutorials:How_jQuery_Works) tarafından [John Resig](http://ejohn.org/)
- [JQuery ile çalışmaya başlama](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) tarafından [Jörn Zaefferer](http://bassistance.de/)
- [JQuery örnekleri canlı](http://codylindley.com/blogstuff/js/jquery/#) tarafından [Cody Lindley](http://codylindley.com/)

Seçilen eklenti başlatıcı ve bu öğreticinin eşlik tamamlanan örnek projeler dahil edilmiştir. Bu öğretici için yalnızca kadar UI bağlanmanıza jQuery kullanmanız gerekecektir. ASP.NET MVC projesinde toplama seçilen jQuery eklentisini kullanmak için şunları yapmalısınız:

1. Seçilen eklentisi gelen indirme [github](https://github.com/harvesthq/chosen/). Bu adım sizin için yapılmıştır.
2. Seçilen klasör, ASP.NET MVC projenize ekleyin. Seçilen klasör için önceki adımda indirdiğiniz seçilen eklentisi varlıklar ekleme. Bu adım sizin için yapılmıştır.
3. Seçilen eklentisi bağlanacağını **DropDownList** veya **ListBox** HTML Yardımcısı.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>MultiSelectCountry görüntülemek için seçilen eklentisi takma.

Açık *Views\Home\MultiSelectCountry.cshtml* dosya ve ekleme bir `htmlAttributes` parametresi `Html.ListBox`. Seçim listesi için bir sınıf adı, ekleyecek parametresi içerir (`@class = "chzn-select"`). Tamamlanan kodu aşağıda gösterilmiştir:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

Yukarıdaki kod öznitelik değeri ve HTML öznitelik ekliyoruz `class = "chzn-select"`. @ Karakteri sınıfı önceki Razor görüntüleme altyapısı ile ilgisi vardır. `class`olan bir [C# anahtar sözcüğü](https://msdn.microsoft.com/en-us/library/x53a06bb.aspx). @ Önek olarak içerirler sürece C# anahtar sözcükleri tanımlayıcıları kullanılamaz. Yukarıdaki örnekte `@class` geçerli bir tanımlayıcı değil ancak **sınıfı** değil çünkü **sınıfı** bir anahtar sözcüktür.

Başvurular ekleyin *Chosen/chosen.jquery.js* ve *Chosen/chosen.css* dosyaları. *Chosen/chosen.jquery.js* ve uygulayan seçilen eklentisi, işlevsel olarak. *Chosen/chosen.css* dosyası stil sağlar. Altındaki bu başvuruları ekleyin *Views\Home\MultiSelectCountry.cshtml* dosya. Aşağıdaki kod, seçilen eklentisi başvuru gösterilmektedir.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Kullanılan sınıf adı kullanılarak seçilen eklentisini etkinleştirmek **Html.ListBox** kodu. Yukarıdaki örnekte, sınıf adı olan `chzn-select`. ' In altına aşağıdaki satırı ekleyin *Views\Home\MultiSelectCountry.cshtml* görünüm dosyası. Bu satırı seçilen eklentisi etkinleştirir.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

Sınıf adı ile DOM öğesi seçer jQuery hazır işlevi çağırmak için söz dizimi aşağıdaki çizgidir `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Sarmalanan yukarıdaki çağrısından döndürülen ayarlamak seçtiğiniz yöntemi uygular (`.chosen();`), seçilen eklentisi kanca oluşturur.

Aşağıdaki kod tamamlanmış gösterir *Views\Home\MultiSelectCountry.cshtml* görünüm dosyası.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Uygulamayı çalıştırın ve gidin `MultiSelectCountry` görünümü. Eklemeyi deneyin ve ülkelerde silme. Sağlanan örnek indirme de içeren bir `MultiCountryVM` yöntemi ve bir görünümü kullanarak MultiSelectCountry işlevselliğini hayata Geçiren görünüm model yerine bir **ViewBag**.

Sonraki bölümde ile ASP.NET MVC yapı iskelesi mekanizması nasıl çalıştığını görürsünüz **DropDownList** Yardımcısı.

>[!div class="step-by-step"]
[Sonraki](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
