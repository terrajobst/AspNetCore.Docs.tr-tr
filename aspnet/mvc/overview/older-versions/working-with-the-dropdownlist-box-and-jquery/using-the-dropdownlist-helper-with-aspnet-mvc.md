---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: ASP.NET MVC ile DropDownList Yardımcısını kullanma | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 6dbffe715990de5c0b3b834e354379e414925816
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219062"
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>ASP.NET MVC ile DropDownList Yardımcısını kullanma
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

Bu öğretici ile çalışmanın temel bilgileri sağlanır [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) Yardımcısı ve [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) bir ASP.NET MVC Web uygulaması yok. Microsoft Visual Web Developer 2010 Express Service Pack öğreticiyi uygulamak için Microsoft Visual Studio ücretsiz bir sürümü olan 1, kullanabilirsiniz. Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun. Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:

- [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack) <a id="post"></a>
- [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)

Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). Bu öğreticide bölümünü tamamladığınız varsayılır [ASP.NET MVC'ye giriş](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) öğretici veya[ASP.NET MVC müzik Store](../mvc-music-store/mvc-music-store-part-1.md) öğretici veya ASP.NET MVC geliştirmeyle. Bu öğreticide değiştirilmiş bir projeden başlayan [ASP.NET MVC müzik Store](../mvc-music-store/mvc-music-store-part-1.md) öğretici. Aşağıdaki bağlantıda başlangıç projesiyle indirebileceğiniz [C# sürümü indirme](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Tamamlanan öğreticide C# kaynak kodu içeren bir Visual Web Developer proje, bu konuya eşlik etmek üzere kullanılabilir. [İndirme](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Ne oluşturacaksınız

Eylem yöntemleri ve kullanan görünümlerde oluşturacaksınız [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) Yardımcısı bir kategori seçin. Ayrıca kullanacağınız **jQuery** (örneğin, türe veya sanatçı) yeni bir kategori gerektiğinde kullanılabilecek bir ekleme kategorisi iletişim eklemek için. Yeni türe ekleyin ve yeni bir sanatçının eklemek için bağlantıları gösteren Oluştur görünümünün ekran aşağıdadır.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Beceriler hakkında bilgi edineceksiniz

Öğrenecekleriniz aşağıda verilmiştir:

- Nasıl kullanılacağını [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) Yardımcısı kategori verileri seçin.
- Nasıl ekleneceği bir **jQuery** yeni kategori eklemek için iletişim.

### <a name="getting-started"></a>Başlarken

Aşağıdaki bağlantıda başlangıç projesiyle indirerek başlayın [indirme](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). Windows Gezgini'nde sağ tıklayarak *DDL\_Starter.zip* dosya ve Özellikler'i seçin. İçinde **DDL\_Starter.zip özellikleri** iletişim kutusu, select Engellemeyi Kaldır.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

DDL sağ tıklayın\_Starter.zip dosya ve select **tümünü Ayıkla** dosyasının sıkıştırması. Açık *StartMusicStore.sln* Visual Web Developer 2010 Express'in ("Visual Web Developer" veya kısaca "VWD") veya Visual Studio 2010 ile dosya.

Tıklayın ve uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın **Test** bağlantı.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Seçin **film kategorisi seçin (Basit)** bağlantı. Seçilen değeri Komedi ile film türünü seçin. bir liste görüntülenir.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Tarayıcı ve kaynağı Görüntüle'yi seçin, sağ tıklayın. HTML sayfası görüntülenir. Aşağıdaki kod, bir HTML select öğesi için gösterir.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Listedeki her öğe bir değer (0 eylem için DRAM için 1, Komedi için 2 ve 3 Bilim Kurgu için) ve bir görünen ad (eylem, DRAM, Komedi ve Bilim Kurgu) olduğunu görebilirsiniz. Yukarıdaki kod, seçme listesi için standart HTML'dir.

Seçim listesi DRAM isabet geçip **Gönder** düğmesi. URL tarayıcıda `http://localhost:2468/Home/CategoryChosen?MovieType=1` ve sayfasını görüntüler **seçtiğiniz: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Açık *Controllers\HomeController.cs* inceleyin ve dosya `SelectCategory` yöntemi.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

[DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) bir HTML select listesindeki oluşturmak için kullanılan bir yardımcı gerektiriyor bir **IEnumerable&lt;Selectlistıtem &gt;** , açıkça veya örtülü olarak. Diğer bir deyişle, geçirebilirsiniz **IEnumerable&lt;Selectlistıtem &gt;**  açıkça [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) Yardımcısı veya ekleyebilir **IEnumerable&lt; Selectlistıtem &gt;**  için [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) için aynı adı kullanarak **Selectlistıtem** model özelliği olarak. Öğesinde geçen **Selectlistıtem** örtük ve açık bir şekilde öğreticinin sonraki bölümünde ele alınmıştır. Yukarıdaki kod oluşturmak için olası en basit yolu gösteren bir **IEnumerable&lt;Selectlistıtem &gt;**  ve metin ve değerleri ile doldurabilirsiniz. Not `Comedy` [Selectlistıtem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) sahip [seçili](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) özelliğini **true;** bu göstermek oluşturulan seçim listesi neden **Komedi** listesinde seçili öğe.

**IEnumerable&lt;Selectlistıtem &gt;**  oluşturulan yukarıdaki eklenir [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) MovieType ada sahip. Bu size nasıl geçirmek, **IEnumerable&lt;Selectlistıtem &gt;**  için örtük olarak [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) aşağıda gösterilen yardımcı.

Açık *Views\Home\SelectCategory.cshtml* dosya ve biçimlendirme inceleyin.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

Üçüncü satırda düzenini görünümler/paylaşılan ayarladığımız/\_basit\_basitleştirilmiş bir sürümünü Standart Düzen dosyası olan Layout.cshtml. Biz görünen tutmak için bunu ve HTML basit çizilir.

Biz kullanarak verileri gönderir, böylece bu örnekte biz uygulamanın durumunu değiştirmiyorsanız bir **HTTP GET**değil **HTTP POST**. W3C bakın [seçme HTTP GET veya POST için hızlı bir denetim listesi](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Biz değil uygulamayı değiştirme ve form gönderme kullandığımız [Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) kurmamızı eylem yöntemi, denetleyici ve form yöntemi belirtmek aşırı yükleme (**HTTP POST** veya **HTTP GET**). Genellikle görünümleri içeren [Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) parametre almayan aşırı yükleme. Hiçbir parametre sürümü aynı eylem yöntemi ve denetleyicinin POST sürümüne form verileri gönderme için varsayılan olarak ayarlanır.

Aşağıdaki satır

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

bir dize bağımsız değişkeni geçirir **DropDownList** Yardımcısı. Bu dize, Bizim örneğimizde "MovieType" iki şey yapar:

- Anahtar için sağladığı **DropDownList** bulmaya yardımcı bir **IEnumerable&lt;Selectlistıtem &gt;**  içinde **ViewBag**.
- Bu verilere MovieType form öğesi için bağlı. Gönderme yöntem ise **HTTP GET**, `MovieType` bir sorgu dizesi olacaktır. Gönderme yöntem ise **HTTP POST**, `MovieType` ileti gövdesine eklenir. Aşağıdaki resimde, sorgu dizesi değeri 1 ile gösterilmektedir.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

Aşağıdaki kodda gösterildiği `CategoryChosen` yöntemi form için gönderildi.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Test sayfasına geri dönün ve seçin **HTML SelectList** bağlantı. HTML sayfasını bir select öğesindeki benzeyen basit bir ASP.NET MVC test sayfası olarak işler. Tarayıcı penceresinin sağ tıklatın ve seçin **kaynağı görüntüle**. Seçim listesi için HTML biçimlendirmesini temelde aynıdır. Test HTML sayfası, ASP.NET MVC eylem yöntemi ve daha önce test görünümü gibi çalışır.

### <a name="improving-the-movie-select-list-with-enums"></a>Film seçim listesi numaralandırmalar ile geliştirme

Uygulama kategorileri sabit ve değişmez, kodunuzu daha sağlam ve genişletmek daha basit hale getirmek için numaralandırmalar yararlanabilir. Doğru kategori değer eklediğinizde, yeni bir kategori oluşturulur. Yeni bir kategori ekleyin, ancak kategori değerini unutursanız, kopyalama ve yapıştırma hataları önler.

Açık *Controllers\HomeController.cs* dosyasını açıp aşağıdaki kodu inceleyin:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

[Enum](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` dört film türleri yakalar. `SetViewBagMovieType` Yöntemi oluşturur **IEnumerable&lt;Selectlistıtem &gt;**  gelen `eMovieCategories` **enum**ve ayarlar `Selected` özelliği`selectedMovie` parametresi. `SelectCategoryEnum` Eylem yöntemini kullanır aynı görünüm olarak `SelectCategory` eylem yöntemi.

Test sayfasına gidin ve tıklayarak `Select Movie Category (Enum)` bağlantı. Bu süre, sabit temsil eden bir dize görüntülenen bir değeri yerine (sayı) görüntülenir.

### <a name="posting-enum-values"></a>Sabit listesi değerlerinin gönderme

HTML formu genellikle sunucunun veri göndermek için kullanılır. Aşağıdaki kodda gösterildiği `HTTP GET` ve `HTTP POST` sürümlerini `SelectCategoryEnumPost` yöntemi.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Geçirerek bir `eMovieCategories` numaralandırmaya `POST` yöntemi, biz hem sabit listesi değeri hem de sabit dize ayıklayabilirsiniz. Örneği çalıştırmak ve Test sayfasına gidin. Tıklayarak `Select Movie Category(Enum Post)` bağlantı. Bir film türü seçin ve ardından Gönder düğmesine basın. Hem değer hem de film türün adını gösterir.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Birden çok bölüm Select öğesi oluşturma

[ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) HTML Yardımcısı işler HTML `<select>` öğeyle `multiple` özniteliği, kullanıcıların birden fazla seçim yapmanızı sağlar. Test bağlantısı gidin ve ardından **çoklu seçin Ülke** bağlantı. İşlenen kullanıcı Arabirimi birden fazla ülkede seçmenizi sağlar. Aşağıdaki görüntüde, Kanada ve Çin seçilir.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>MultiSelectCountry kod İnceleme

Aşağıdaki kod İnceleme *Controllers\HomeController.cs* dosya.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

`GetCountries` Yöntemi ülkelerin listesi oluşturur ve ardından buna ileten `MultiSelectList` Oluşturucusu. `MultiSelectList` Oluşturucu aşırı kullanılan `GetCountries` yukarıdaki yöntemi dört parametre alır:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *öğeleri*: bir [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) listesindeki öğeleri içeren. Örnekte, ülkelerin listesinin üstünde.
2. *dataValueField*: özelliğin adını **IEnumerable** değeri içeren liste. Yukarıdaki örnekte `ID` özelliği.
3. *dataTextField*: özelliğin adını **IEnumerable** görüntülenecek bilgileri içeren liste. Yukarıdaki örnekte `name` özelliği.
4. *selectedValues*: seçilen değerlerin listesi.

Yukarıdaki örnekte `MultiSelectCountry` yöntemi geçişleri bir `null` kullanıcı Arabiriminde görüntülendiğinde hiçbir ülkeler seçili için seçilen ülke için değer. Aşağıdaki kod oluşturmak için kullanılan Razor işaretlemesi gösterir `MultiSelectCountry` görünümü.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

HTML Yardımcısı [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) yöntemi, iki parametre, model bağlama için özellik adını Al kullanılan ve [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) seçenekleri belirleyin ve değerlerini içeren. `ViewBag.YouSelected` Yukarıdaki kod, formu gönderdiğinde, seçili ülkelerde değerleri görüntülemek için kullanılır. HTTP POST aşırı yüklemesini inceleyin `MultiSelectCountry` yöntemi.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

`ViewBag.YouSelected` Dinamik özellik içeren için elde edilen seçili ülkelerde `Countries` form koleksiyonu girişi. Bu sürümde seçili ülkelerin listesi GetCountries yöntemi geçirilir bu nedenle `MultiSelectCountry` görünümü gösterilir, kullanıcı Arabiriminde seçili ülkelerde seçilir.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Bir Seç öğesi kolay Harvest seçilen jQuery eklentisi ile yapma

Toplama [seçilen](http://harvesthq.github.com/chosen/) jQuery eklentisi için bir HTML eklenebilir &lt;seçin&gt; öğesi bir kullanıcı oluşturmak için kolay kullanıcı Arabirimi. Aşağıdaki görüntülerin Harvest göstermek [seçilen](http://harvesthq.github.com/chosen/) jQuery eklentisi ile `MultiSelectCountry` görünümü.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

Aşağıdaki iki resimlerdeki **Kanada** seçilir.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

Yukarıdaki görüntüde, Kanada seçilir ve içerdiği bir **x** seçimini kaldırmak için tıklayın. Kanada, Çin'de, aşağıdaki resimde gösterilmektedir ve Japonya seçili.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Seçilen toplama jQuery eklentisi takma

Aşağıdaki bölümde, jQuery biraz deneyim olması durumunda daha kolay olur. JQuery önce hiç kullanmadıysanız, aşağıdaki jQuery öğreticilerden birine denemek isteyebilirsiniz.

- [JQuery nasıl çalıştığını](http://docs.jquery.com/Tutorials:How_jQuery_Works) tarafından [John Resig](http://ejohn.org/)
- [JQuery ile çalışmaya başlama](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) tarafından [Jörn Zaefferer](http://bassistance.de/)
- [JQuery örnekleri canlı](http://codylindley.com/blogstuff/js/jquery/#) tarafından [Cody Lindley](http://codylindley.com/)

Seçilen eklentisi, başlangıç ve eşlik eden Bu öğreticinin tamamlanan örnek projeleri dahildir. Bu öğretici için yalnızca kullanıcı arabirimini yeteneklerinizi jQuery kullanmanız gerekecektir. ASP.NET MVC projesinde Harvest seçilen jQuery eklentisini kullanmak için yapmanız gerekir:

1. Seçilen eklentisini indirin [github](https://github.com/harvesthq/chosen/). Bu adım, sizin için yapılmıştır.
2. Seçilen klasörü, ASP.NET MVC projenize ekleyin. Seçilen klasör için önceki adımda indirdiğiniz seçilen eklentisi varlıkları ekleyin. Bu adım, sizin için yapılmıştır.
3. Seçili eklenti için bağlama **DropDownList** veya **ListBox** HTML Yardımcısı.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Seçili eklenti MultiSelectCountry görünümüne olaylara bağlama.

Açık *Views\Home\MultiSelectCountry.cshtml* dosya ve ekleme bir `htmlAttributes` parametresi `Html.ListBox`. Seçim listesi için bir sınıf adı ekleyeceğiniz parametresi içerir (`@class = "chzn-select"`). Tamamlanan kodu aşağıda gösterilmiştir:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

Yukarıdaki kod HTML öznitelik ve öznitelik değeri ekliyoruz `class = "chzn-select"`. \@ Önceki sınıfı Razor görünüm altyapısı ile ilgisi olan karakter. `class` olan bir [C# anahtar sözcüğünü](https://msdn.microsoft.com/library/x53a06bb.aspx). C# anahtar sözcükleri içerirler sürece tanımlayıcı olarak kullanılamaz \@ öneki olarak. Yukarıdaki örnekte `@class` geçerli bir tanıtıcı ancak **sınıfı** değil çünkü **sınıfı** bir anahtar sözcüktür.

Başvuruları Ekle *Chosen/chosen.jquery.js* ve *Chosen/chosen.css* dosyaları. *Chosen/chosen.jquery.js* ve seçilen eklentisi, işlevsel olarak. *Chosen/chosen.css* dosyası stil sağlar. Alt kısmındaki bu başvuruları ekleyin *Views\Home\MultiSelectCountry.cshtml* dosya. Aşağıdaki kod, seçilen eklentisi başvuru gösterilmektedir.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Kullanılan sınıf adını kullanarak seçilen eklentisini etkinleştirmeniz **Html.ListBox** kod. Yukarıdaki örnekte, sınıf adıdır `chzn-select`. ' In altına aşağıdaki satırı ekleyin *Views\Home\MultiSelectCountry.cshtml* görünüm dosyası. Bu satırı seçilen eklentiyi etkinleştirir.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

Aşağıdaki satırı sınıf adı DOM öğesiyle seçer jQuery hazır işlevini çağırmak için sözdizimi aşağıdaki gibidir `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Sarmalanan yukarıdaki çağrısından döndürülen ayarlayın seçtiğiniz yöntemi uygular (`.chosen();`), seçilen eklentisi kancaları.

Aşağıdaki kod tamamlanmış gösterir *Views\Home\MultiSelectCountry.cshtml* görünüm dosyası.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Uygulamayı çalıştırmak ve gidin `MultiSelectCountry` görünümü. Eklemeyi deneyin ve ülkeler siliniyor. Sağlanan örnek indirme de içeren bir `MultiCountryVM` yöntemi ve bir görüntü kullanarak MultiSelectCountry işlevselliğini uygular görünüm modeli yerine bir **ViewBag**.

Sonraki bölümde, ASP.NET MVC yapı iskelesi mekanizması ile nasıl çalıştığı görürsünüz **DropDownList** Yardımcısı.

> [!div class="step-by-step"]
> [Next](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
