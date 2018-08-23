---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
title: Düzenleme metotlarını ve düzenleme görünümünü (VB) İnceleme | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack, 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 5cb3c59b-1e96-464b-b3a8-c55607201872
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 13acaa13d5e696928fbc9834eb355c630ced7162
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753800"
---
<a name="examining-the-edit-methods-and-edit-view-vb"></a>Düzenleme metotlarını ve düzenleme görünümünü (VB) İnceleme
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
> Bu konuya eşlik etmek üzere bir Visual Web Developer proje VB.NET kaynak koduyla birlikte kullanılabilir. [VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). C# tercih ederseniz, geçiş [C# sürümü](../cs/examining-the-edit-methods-and-edit-view.md) Bu öğreticinin.


Bu bölümde, oluşturulan eylem metotları ve görünümleri film denetleyicisi inceleyeceğiz. Ardından, özel arama sayfası ekleyeceksiniz.

Uygulamayı çalıştırmak ve göz atın `Movies` ekleyerek denetleyicisi */Movies* tarayıcınızın adres çubuğundaki URL. Fare işaretçisini tutun bir **Düzenle** bağlantı URL'sini görmek için bağlantı.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Düzenle** bağlantı tarafından oluşturulan `Html.ActionLink` yönteminde *Views\Movies\Index.vbhtml* görüntüle:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image3.png)](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Nesnesi üzerinde bir özelliği kullanılarak açığa çıkarılır bir yardımcı olan `WebViewPage` temel sınıfı. `ActionLink` Yardımcı yöntemini dinamik olarak HTML köprüler, eylem yöntemlerine denetleyicilerde bağlantı oluşturmak kolaylaştırır. İlk bağımsız değişkeni `ActionLink` yöntemdir işlemek için bağlantı metni (örneğin, `<a>Edit Me</a>`). İkinci bağımsız değişkeni çağrılacak eylem yöntemi adıdır. Son bağımsız değişken bir [anonim nesne](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) , rota verilerini (Bu durumda 4 kimliği) oluşturur.

Önceki görüntüde verilen oluşturulan bağlantı `http://localhost:xxxxx/Movies/Edit/4`. URL deseni varsayılan rota alır `{controller}/{action}/{id}`. Bu nedenle, ASP.NET çevirir `http://localhost:xxxxx/Movies/Edit/4` bir istek halinde `Edit` eylem yöntemi `Movies` denetleyicisi parametresiyle `ID` 4 eşittir.

Sorgu dizesi kullanarak bir eylem yöntemi parametrelerine de geçirebilirsiniz. Örneğin, URL `http://localhost:xxxxx/Movies/Edit?ID=4` ayrıca parametresini geçirir `ID` için 4'ün `Edit` eylem yöntemi `Movies` denetleyicisi.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image5.png)](examining-the-edit-methods-and-edit-view/_static/image4.png)

Açık `Movies` denetleyicisi. İki `Edit` eylem yöntemleri aşağıda gösterilmektedir.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample3.vb)]

İkinci fark `Edit` eylem yöntemine öncesinde `HttpPost` özniteliği. Bu öznitelik Bu aşırı yüklemesini belirtir `Edit` yöntemi yalnızca POST istekleri için çağrılan. Geçerli olabilir `HttpGet` ilk özniteliği Düzenle yöntemi, ancak bu gerekli değildir, varsayılan olduğundan. (Biz örtük olarak atanmış olan eylem yöntemleri için başvuracağınız `HttpGet` olarak özniteliği `HttpGet` yöntemler.)

`HttpGet` `Edit` Yöntemi film kimliği parametre alır, Entity Framework kullanarak filmi arar `Find` yöntemi ve düzenleme görünümü seçili film döndürür. Yapı iskelesi sistem düzenleme görünümü oluşturduğunuzda, onu incelenirken `Movie` sınıfı ve işlemek için oluşturulan kodu `<label>` ve `<input>` sınıfın her bir özellik için öğeleri. Aşağıdaki örnek, oluşturulan düzenleme görünümünü gösterir:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.vbhtml)]

Şablonu Görüntüle nasıl olduğunu fark bir `@ModelType MvcMovie.Models.Movie` deyimini dosyanın üst — bu görünüm model görünüm şablonu türünde olmasını bekliyor belirtir `Movie`.

İskele kurulan kodu birkaç kullanan *yardımcı yöntemler* HTML biçimlendirmeyi basitleştirmek için. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Yardımcı, alan adını görüntüler (&quot;başlık&quot;, &quot;ReleaseDate&quot;, &quot;Tarz&quot;, veya &quot;fiyat &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Görüntüleyen bir HTML Yardımcısı `<input>` öğesi. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Yardımcısı, bu özellikle ilişkili herhangi bir doğrulama iletisi görüntüler.

Uygulamayı çalıştırmak ve gidin */Movies* URL'si. ' A tıklayın bir **Düzenle** bağlantı. Tarayıcıda, sayfa için kaynağı görüntüleyin. HTML sayfasında, aşağıdaki örnekteki gibi görünür. (Menü biçimlendirme açıklık için dışarıda bırakıldı.)

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html)]

`<input>` Öğeleri olan bir HTML `<form>` öğesi olan `action` özniteliğinin ayarlanmış gönderinin yayımlanacağı */filmler/düzenleme* URL'si. Form verileri sunucuya yayımlanacak olduğunda **Düzenle** düğmesine tıklandığında.

## <a name="processing-the-post-request"></a>POST isteğini işleme

Aşağıdaki liste gösterildiği `HttpPost` sürümünü `Edit` eylem yöntemi.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample6.vb)]

ASP.NET framework model bağlayıcı gönderilen form değerlerini alır ve oluşturan bir `Movie` olarak geçirilen nesne `movie` parametresi. `ModelState.IsValid` Kodu iade doğrular biçiminde gönderilen veriler değiştirmek için kullanılabilir bir `Movie` nesne. Veriler geçerliyse, kod film verileri kaydeder `Movies` koleksiyonunu `MovieDBContext` örneği. Kodu daha sonra yeni film verileri veritabanına çağırarak kaydeder `SaveChanges` yöntemi `MovieDBContext`, veritabanına değişiklikleri sürdürür. Verileri kaydettikten sonra kodu kullanıcı için yönlendiren `Index` eylem yöntemi `MoviesController` sınıfını filmler listesinde görüntülenecek güncelleştirilmiş film neden olur.

Gönderilen değerlerden geçerli değilse, formda yeniden görüntülenir. `Html.ValidationMessageFor` Yardımcılar olarak *Edit.vbhtml* görünüm şablonu uygun hata iletilerini görüntüleme ilgileniriz.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image7.png)](examining-the-edit-methods-and-edit-view/_static/image6.png)

> **Yerel ayarlar hakkında daha fazla Not** normalde İngilizce dışında bir yerel ayar ile çalışıyorsanız, bkz. [İngilizce olmayan yerel ayarlar ile ASP.NET MVC 3 doğrulama destekleme.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Düzenleme yöntemi daha sağlam hale getirme

`HttpGet` `Edit` Yapı iskelesi sistem tarafından oluşturulan yöntem kendisine geçirilen Kimliğinin geçerli olduğundan emin denetleyin değil. Bir kullanıcı kimliği kesim URL'den kaldırır, (`http://localhost:xxxxx/Movies/Edit`), şu hata görüntülenir:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image9.png)](examining-the-edit-methods-and-edit-view/_static/image8.png)

Bir kullanıcı gibi veritabanında mevcut olmayan bir kimliği de geçirebiliriz `http://localhost:xxxxx/Movies/Edit/1234`. İçin iki değişiklik yapabilirsiniz `HttpGet` `Edit` bu sınırlamaya yönelik eylem yöntemi. İlk olarak değiştirmek `ID` bağlandığımızda sıfır varsayılan değeri açıkça geçirilmiş olmayan bir kimliği parametre. Ayrıca, denetleyebilirsiniz `Find` yöntem gerçekten görünümü şablona Film nesnesini döndürmeden önce bir filmi bulunamadı. Güncelleştirilmiş `Edit` yöntemi aşağıda gösterilmektedir.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample7.vb)]

Hiçbir film bulunursa `HttpNotFound` yöntemi çağrılır.

Tüm `HttpGet` benzer bir desen yöntemleri uygulayın. Bir film nesnesi aldıkları (veya durumunda bir nesneler listesi `Index`) ve model görünümüne geçirin. `Create` Yöntemi bir boş film nesne oluşturma görünümüne geçirir. Bu nedenle, oluşturmak, düzenlemek, silmek veya aksi halde verileri değiştiren tüm yöntemler yapmak `HttpPost` yöntemi aşırı yüklemesi. Bir HTTP GET yöntemi verileri değiştirmek, bir güvenlik riski blog gönderisi girişi açıklandığı [ASP.NET MVC ipucu #46 – bunlar güvenlik açıkları oluşturduğundan Sil bağlantılarını kullanmayın](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Bir GET yöntemi verilerde değişiklik de HTTP en iyi yöntemleri ve GET istekleri, uygulamanızın durumunu değiştirmemesi gerekir belirtir mimari REST desenini ihlal ediyor. Diğer bir deyişle, bir GET işlemi gerçekleştirilirken yan etkileri olan güvenli bir işlem olmalıdır.

## <a name="adding-a-search-method-and-search-view"></a>Bir arama yöntemi ve arama görünümü ekleme

Bu bölümde, ekleyeceksiniz bir `SearchIndex` olanak sağlayan bir eylem yöntemi filmler Tarz veya adı arayın. Bu kullanılarak kullanılabilir olacaktır */filmler/SearchIndex* URL'si. İstek, bir kullanıcı için bir filmi aramak amacıyla doldurabilirsiniz giriş öğelerini içeren bir HTML formu görüntülenir. Kullanıcı formu gönderdiğinde, eylem yöntemi, kullanıcı tarafından gönderilen arama değerlerini alın ve veritabanı aramak için değerleri kullanın.

![SearchIndx_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

## <a name="displaying-the-searchindex-form"></a>SearchIndex Form görüntüleme

Başlangıç ekleyerek bir `SearchIndex` mevcut eylem yöntemine `MoviesController` sınıfı. Yöntemi, bir HTML formu içeren bir görünümü döndürülür. Kod aşağıdaki gibidir:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample8.vb)]

İlk satırı `SearchIndex` yöntemi oluşturur aşağıdaki [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) filmler seçmek için sorgu:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample9.vb)]

Sorgu, bu noktada tanımlanır, ancak veri deposuna karşı henüz çalıştırılmadı.

Varsa `searchString` parametresi içeren bir dize, filmler sorgu aşağıdaki kodu kullanarak, bir arama dizesi değerini temel filtrelemek için değiştirilir:

Aksi durumda String.IsNullOrEmpty(searchString) sonra   
 filmler filmler =. Burada (s.Title.Contains(searchString)) işlevlere   
 End IF

LINQ sorguları tanımlandıkları veya bunlar bir yöntemi çağırarak değiştirildiğinde yürütülmez `Where` veya `OrderBy`. Bunun yerine, sorgu yürütmesi, üzerinden gerçekleştirilen değeri gerçekten yinelendiğinde kadar bir ifade değerlendirmesi ertelendi anlamına gelir ertelenmiştir veya [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) yöntemi çağrılır. İçinde `SearchIndex` örnek SearchIndex Görünümü'nde sorgu yürütülür. Ertelenen sorgu yürütme hakkında daha fazla bilgi için bkz. [sorgu yürütme](https://msdn.microsoft.com/library/bb738633.aspx).

Şimdi, uygulayabileceğiniz `SearchIndex` kullanıcıya form görüntüleyecek görünümü. İçinde sağ `SearchIndex` yöntemi ve ardından **Görünüm Ekle**. İçinde **Görünüm Ekle** iletişim kutusunda, geçirilecek gideceğinizi belirtin bir `Movie` model sınıfı olarak görünümü şablon nesnesi. İçinde **İskele şablon** listesinde **listesi**, ardından **Ekle**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Tıkladığınızda **Ekle** düğmesi *Views\Movies\SearchIndex.vbhtml* görünüm şablonu oluşturulur. Seçtiğiniz çünkü **listesi** içinde **İskele şablon** listesinde, otomatik olarak oluşturulan Visual Web Developer (iskele kurulmuş) bazı varsayılan içerik görünümünde. Bir HTML formuna yapı iskelesi oluşturulur. Denetlenen `Movie` sınıfı ve işlemek için oluşturulan kodu `<label>` sınıfın her bir özellik için öğeleri. Listenin altında oluşturulan Oluştur görünümünün gösterir:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.vbhtml)]

Uygulamayı çalıştırmak ve gidin */filmler/SearchIndex*. Bir sorgu dizesi gibi ekleme `?searchString=ghost` URL'si. Filtrelenmiş filmler görüntülenir.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

İmzası değiştirirseniz `SearchIndex` adlı bir parametreye sahip yöntemi `id`, `id` parametresi eşleşir `{id}` varsayılan için yer tutucu yönlendiren kümesinde *Global.asax* dosya.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

Değiştirilmiş `SearchIndex` yöntemi şu şekilde görünür:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample12.vb)]

Arama başlığı olarak bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine artık geçirebilirsiniz.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Ancak, bunlar bir filmi için arama yapmak istediğiniz her zaman URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz. Artık, yardımcı olmak için kullanıcı Arabirimi ekleyeceksiniz filmler filtreleyebilirsiniz. İmzası değiştirdiyseniz `SearchIndex` metodu rota bağlı ID parametresine geçirilecek nasıl test etmek için değiştirme geri böylece, `SearchIndex` yöntemi adlı bir dize parametresi alır `searchString`:

Açık *Views\Movies\SearchIndex.vbhtml* dosyasını açın ve sonra yalnızca `@Html.ActionLink("Create New", "Create")`, aşağıdakileri ekleyin:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample13.vbhtml)]

`Html.BeginForm` Yardımcısı oluşturur açılış `<form>` etiketi. `Html.BeginForm` Yardımcısı neden tıklayarak kullanıcı formu gönderdiğinde, kendisine göndermek form **filtre** düğmesi.

Uygulamayı çalıştırmak ve bir filmi için arama yapmayı deneyin.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

Var olan hiçbir `HttpPost` aşırı yükünü `SearchIndex` yöntemi. Yöntemi uygulama durumunu değiştirme değildir çünkü, yalnızca verileri filtreleme gerekmez. Aşağıdaki eklediyseniz `HttpPost` `SearchIndex` yöntemi, eylem çağırıcısını eşleşir `HttpPost` `SearchIndex` yöntemi ve `HttpPost` `SearchIndex` yöntemi aşağıdaki resimde gösterildiği gibi çalıştırın.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample14.vb)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

## <a name="adding-search-by-genre"></a>Türe göre arama ekleme

Eklediyseniz `HttpPost` sürümünü `SearchIndex` yöntemi, şimdi silin.

Ardından, kullanıcıların filmler için türe göre arama bir özellik ekleyeceksiniz. Değiştirin `SearchIndex` yöntemini aşağıdaki kod ile:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample15.vb)]

Bu sürümü `SearchIndex` yöntemi alır ek bir parametre, yani `movieGenre`. İlk birkaç kod satırlarının bir `List` film türleri veritabanından tutacak nesne.

Tüm türleri veritabanından alır bir LINQ Sorgu kodudur.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample16.vb)]

Kod `AddRange` genel yöntemini `List` farklı türleri listesine eklemek için koleksiyonu. (Olmadan `Distinct` değiştiricisi, yinelenen tür eklenecek — Örneğin, iki kez örneğimizi Komedi eklenir). Kod içinde türleri listesini ardından depolar `ViewBag` nesne.

Aşağıdaki kod nasıl kontrol edileceğini göstermektedir `movieGenre` parametresi. Boş değilse, belirtilen türe seçili film sınırlamak için filmler sorgu kodu daha ayrıntılı kısıtlar.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample17.vb)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Türe göre ara desteklemek için SearchIndex görünümü biçimlendirme ekleme

Ekleme bir `Html.DropDownList` yardımcıya *Views\Movies\SearchIndex.vbhtml* hemen önce dosya `TextBox` Yardımcısı. Tamamlanan biçimlendirme aşağıda gösterilmiştir:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.vbhtml)]

Uygulamayı çalıştırmak ve göz atın */filmler/SearchIndex*. Türe, film adı ve her iki ölçütlere göre bir arama deneyin.

Bu bölümde CRUD eylem yöntemleri ve framework tarafından oluşturulan görünümler incelenir. Bir arama eylem yöntemi ve kullanıcıların filmi Tarz arama görünümü oluşturduğunuz. Sonraki bölümde, bir özelliğin ekleneceği nasıl şu konuları inceleyeceğiz `Movie` modeli ve bir test veritabanı otomatik olarak oluşturacak bir başlatıcı ekleme.

> [!div class="step-by-step"]
> [Önceki](accessing-your-models-data-from-a-controller.md)
> [İleri](adding-a-new-field.md)
