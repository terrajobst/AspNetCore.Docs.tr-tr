---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: Düzenleme metotlarını ve düzenleme görünümünü İnceleme | Microsoft Docs
author: Rick-Anderson
description: 'Not: Bu öğretici güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu, daha güvenli ve izleyin ve tanıtım çok daha kolay...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: ea8156fc2ea334be37e7c3f02ac15d168cfc6843
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752382"
---
<a name="examining-the-edit-methods-and-edit-view"></a>Düzenleme metotlarını ve düzenleme görünümünü İnceleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğreticide güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Bu, daha güvenli ve izlemek çok daha kolay ve daha fazla özelliklerini gösterir.


Bu bölümde, oluşturulan eylem metotları ve görünümleri film denetleyicisi inceleyeceğiz. Ardından, özel arama sayfası ekleyeceksiniz.

Uygulamayı çalıştırmak ve göz atın `Movies` ekleyerek denetleyicisi */Movies* tarayıcınızın adres çubuğundaki URL. Fare işaretçisini tutun bir **Düzenle** bağlantı URL'sini görmek için bağlantı.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Düzenle** bağlantı tarafından oluşturulan `Html.ActionLink` yönteminde *Views\Movies\Index.cshtml* görüntüle:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Nesnesi üzerinde bir özelliği kullanılarak açığa çıkarılır bir yardımcı olan [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) temel sınıfı. `ActionLink` Yardımcı yöntemini dinamik olarak HTML köprüler, eylem yöntemlerine denetleyicilerde bağlantı oluşturmak kolaylaştırır. İlk bağımsız değişkeni `ActionLink` yöntemdir işlemek için bağlantı metni (örneğin, `<a>Edit Me</a>`). İkinci bağımsız değişkeni çağrılacak eylem yöntemi adıdır. Son bağımsız değişken bir [anonim nesne](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) , rota verilerini (Bu durumda 4 kimliği) oluşturur.

Önceki görüntüde verilen oluşturulan bağlantı `http://localhost:xxxxx/Movies/Edit/4`. Varsayılan yol (içinde belirlenen *uygulama\_Start\RouteConfig.cs*) URL deseni alan `{controller}/{action}/{id}`. Bu nedenle, ASP.NET çevirir `http://localhost:xxxxx/Movies/Edit/4` bir istek halinde `Edit` eylem yöntemi `Movies` denetleyicisi parametresiyle `ID` 4 eşittir. Aşağıdaki kod İnceleme *uygulama\_Start\RouteConfig.cs* dosya.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Sorgu dizesi kullanarak bir eylem yöntemi parametrelerine de geçirebilirsiniz. Örneğin, URL `http://localhost:xxxxx/Movies/Edit?ID=4` ayrıca parametresini geçirir `ID` için 4'ün `Edit` eylem yöntemi `Movies` denetleyicisi.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Açık `Movies` denetleyicisi. İki `Edit` eylem yöntemleri aşağıda gösterilmektedir.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

İkinci fark `Edit` eylem yöntemine öncesinde `HttpPost` özniteliği. Bu öznitelik Bu aşırı yüklemesini belirtir `Edit` yöntemi yalnızca POST istekleri için çağrılan. Geçerli olabilir `HttpGet` ilk özniteliği Düzenle yöntemi, ancak bu gerekli değildir, varsayılan olduğundan. (Biz örtük olarak atanmış olan eylem yöntemleri için başvuracağınız `HttpGet` olarak özniteliği `HttpGet` yöntemler.)

`HttpGet` `Edit` Yöntemi film kimliği parametre alır, Entity Framework kullanarak filmi arar `Find` yöntemi ve düzenleme görünümü seçili film döndürür. ID parametresi belirtir bir [varsayılan değer](https://msdn.microsoft.com/library/dd264739.aspx) biri sıfır `Edit` yöntemi, bir parametre olmadan çağrılır. Bir filmi bulunamazsa [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) döndürülür. Yapı iskelesi sistem düzenleme görünümü oluşturduğunuzda, onu incelenirken `Movie` sınıfı ve işlemek için oluşturulan kodu `<label>` ve `<input>` sınıfın her bir özellik için öğeleri. Aşağıdaki örnek, oluşturulan düzenleme görünümünü gösterir:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

Şablonu Görüntüle nasıl olduğunu fark bir `@model MvcMovie.Models.Movie` deyimini dosyanın üst — bu görünüm model görünüm şablonu türünde olmasını bekliyor belirtir `Movie`.

İskele kurulan kodu birkaç kullanan *yardımcı yöntemler* HTML biçimlendirmeyi basitleştirmek için. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Yardımcı, alan adını görüntüler (&quot;başlık&quot;, &quot;ReleaseDate&quot;, &quot;Tarz&quot;, veya &quot;fiyat &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) İşleyen bir HTML Yardımcısı `<input>` öğesi. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Yardımcısı, bu özellikle ilişkili herhangi bir doğrulama iletisi görüntüler.

Uygulamayı çalıştırmak ve gidin */Movies* URL'si. ' A tıklayın bir **Düzenle** bağlantı. Tarayıcıda, sayfa için kaynağı görüntüleyin. HTML form öğesi için aşağıda gösterilmiştir.

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

`<input>` Öğeleri olan bir HTML `<form>` öğesi olan `action` özniteliğinin ayarlanmış gönderinin yayımlanacağı */filmler/düzenleme* URL'si. Form verileri sunucuya yayımlanacak olduğunda **Düzenle** düğmesine tıklandığında.

## <a name="processing-the-post-request"></a>POST isteğini işleme

Aşağıdaki liste gösterildiği `HttpPost` sürümünü `Edit` eylem yöntemi.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

[ASP.NET MVC model bağlayıcı](https://msdn.microsoft.com/magazine/hh781022.aspx) gönderilen form değerlerini alır ve oluşturur bir `Movie` olarak geçirilen nesne `movie` parametresi. `ModelState.IsValid` Yöntemi doğrular (düzenleme veya güncelleştirme) değiştirileceğini biçiminde gönderilen veriler kullanılabilir bir `Movie` nesne. Veriler geçerliyse, film verileri kaydedilen `Movies` koleksiyonunu `db(MovieDBContext` örnek). Çağrı yaparak yeni film verileri veritabanına kaydedilir `SaveChanges` yöntemi `MovieDBContext`. Verileri kaydettikten sonra kodu kullanıcı için yönlendiren `Index` eylem yöntemi `MoviesController` sınıfı, görüntüleyen yaptığınız değişiklikleri içeren film koleksiyonun.

Gönderilen değerlerden geçerli değilse, formda yeniden görüntülenir. `Html.ValidationMessageFor` Yardımcılar olarak *Edit.cshtml* görünüm şablonu uygun hata iletilerini görüntüleme ilgileniriz.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> jQuery doğrulama virgül İngilizce olmayan yerel ayara yönelik desteği için (&quot;,&quot;) için bir ondalık noktası eklemeniz gerekir *globalize.js* ve size özgü *cultures/globalize.cultures.js* dosyası (gelen [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) ve kullanmak için JavaScript'i `Globalize.parseFloat`. Aşağıdaki kod ile çalışmak için Views\Movies\Edit.cshtml dosya için yapılan değişiklikleri gösterir &quot;fr-FR&quot; kültür:


[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Ondalık alanın, virgül, ondalık noktası gerektirebilir. Geçici bir düzeltme projelerin kök web.config dosyasının Genelleştirme öğe ekleyebilirsiniz. Aşağıdaki kodu, ABD İngilizcesi olarak ayarlamak kültürüyle Genelleştirme öğeyi gösterir.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

Tüm `HttpGet` benzer bir desen yöntemleri uygulayın. Bir film nesnesi aldıkları (veya durumunda bir nesneler listesi `Index`) ve model görünümüne geçirin. `Create` Yöntemi bir boş film nesne oluşturma görünümüne geçirir. Bu nedenle, oluşturmak, düzenlemek, silmek veya aksi halde verileri değiştiren tüm yöntemler yapmak `HttpPost` yöntemi aşırı yüklemesi. Bir HTTP GET yöntemi verileri değiştirmek, bir güvenlik riski blog gönderisi girişi açıklandığı [ASP.NET MVC ipucu #46 – bunlar güvenlik açıkları oluşturduğundan Sil bağlantılarını kullanmayın](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Bir GET yöntemi verilerde değişiklik de ihlal HTTP en iyi yöntemler ve mimari [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) desen, GET istekleri, uygulamanızın durumunu değiştirmemesi gerekir belirtir. Diğer bir deyişle, bir GET işlemi gerçekleştirilirken yan etkileri olan ve verilerinizi kalıcı değiştirmez güvenli bir işlem olmalıdır.

## <a name="adding-a-search-method-and-search-view"></a>Bir arama yöntemi ve arama görünümü ekleme

Bu bölümde, ekleyeceksiniz bir `SearchIndex` olanak sağlayan bir eylem yöntemi filmler Tarz veya adı arayın. Bu kullanılarak kullanılabilir olacaktır */filmler/SearchIndex* URL'si. İstek, bir kullanıcı için bir filmi aramak amacıyla girebilir, giriş öğelerini içeren bir HTML formu görüntülenir. Kullanıcı formu gönderdiğinde, eylem yöntemi, kullanıcı tarafından gönderilen arama değerlerini alın ve veritabanı aramak için değerleri kullanın.

## <a name="displaying-the-searchindex-form"></a>SearchIndex Form görüntüleme

Başlangıç ekleyerek bir `SearchIndex` mevcut eylem yöntemine `MoviesController` sınıfı. Yöntemi, bir HTML formu içeren bir görünümü döndürülür. Kod aşağıdaki gibidir:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

İlk satırı `SearchIndex` yöntemi oluşturur aşağıdaki [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) filmler seçmek için sorgu:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

Sorgu, bu noktada tanımlanır, ancak veri deposuna karşı henüz çalıştırılmadı.

Varsa `searchString` parametresi içeren bir dize, filmler sorgu aşağıdaki kodu kullanarak, bir arama dizesi değerini temel filtrelemek için değiştirilir:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

`s => s.Title` Kodu yukarıdaki bir [Lambda ifadesi](https://msdn.microsoft.com/library/bb397687.aspx). Lambda ifadeleri yöntem tabanlı kullanılan [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) gibi sorgularında standart sorgu işleci yöntemlerinin bağımsız değişkenleri olarak [burada](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) Yukarıdaki kod içinde kullanılan yöntem. LINQ sorguları tanımlandıkları veya bunlar bir yöntemi çağırarak değiştirildiğinde yürütülmez `Where` veya `OrderBy`. Bunun yerine, sorgu yürütmesi, üzerinden gerçekleştirilen değeri gerçekten yinelendiğinde kadar bir ifade değerlendirmesi ertelendi anlamına gelir ertelenmiştir veya [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) yöntemi çağrılır. İçinde `SearchIndex` örnek SearchIndex Görünümü'nde sorgu yürütülür. Ertelenen sorgu yürütme hakkında daha fazla bilgi için bkz. [sorgu yürütme](https://msdn.microsoft.com/library/bb738633.aspx).

Şimdi, uygulayabileceğiniz `SearchIndex` kullanıcıya form görüntüleyecek görünümü. İçinde sağ `SearchIndex` yöntemi ve ardından **Görünüm Ekle**. İçinde **Görünüm Ekle** iletişim kutusunda, geçirilecek gideceğinizi belirtin bir `Movie` model sınıfı olarak görünümü şablon nesnesi. İçinde **İskele şablon** listesinde **listesi**, ardından **Ekle**.

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

Tıkladığınızda **Ekle** düğmesi *Views\Movies\SearchIndex.cshtml* görünüm şablonu oluşturulur. Seçtiğiniz çünkü **listesi** içinde **İskele şablon** listesinde, otomatik olarak oluşturulan Visual Studio (iskele kurulmuş) görünümünde bazı varsayılan biçimlendirme. Bir HTML formuna yapı iskelesi oluşturulur. Denetlenen `Movie` sınıfı ve işlemek için oluşturulan kodu `<label>` sınıfın her bir özellik için öğeleri. Listenin altında oluşturulan Oluştur görünümünün gösterir:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

Uygulamayı çalıştırmak ve gidin */filmler/SearchIndex*. Bir sorgu dizesi gibi ekleme `?searchString=ghost` URL'si. Filtrelenmiş filmler görüntülenir.

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

İmzası değiştirirseniz `SearchIndex` adlı bir parametreye sahip yöntemi `id`, `id` parametresi eşleşir `{id}` varsayılan için yer tutucu yönlendiren kümesinde *Global.asax* dosya.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

Özgün `SearchIndex` yöntemi aşağıdaki gibi görünür:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

Değiştirilmiş `SearchIndex` yöntemi şu şekilde görünür:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

Arama başlığı olarak bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine artık geçirebilirsiniz.

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

Ancak, bunlar bir filmi için arama yapmak istediğiniz her zaman URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz. Artık, yardımcı olmak için kullanıcı Arabirimi ekleyeceksiniz filmler filtreleyebilirsiniz. İmzası değiştirdiyseniz `SearchIndex` metodu rota bağlı ID parametresine geçirilecek nasıl test etmek için değiştirme geri böylece, `SearchIndex` yöntemi adlı bir dize parametresi alır `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

Açık *Views\Movies\SearchIndex.cshtml* dosyasını açın ve sonra yalnızca `@Html.ActionLink("Create New", "Create")`, aşağıdakileri ekleyin:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

Aşağıdaki örnek bir bölümü gösterilmektedir *Views\Movies\SearchIndex.cshtml* dosyasıyla eklenen filtreleme biçimlendirme.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

`Html.BeginForm` Yardımcısı oluşturur açılış `<form>` etiketi. `Html.BeginForm` Yardımcısı neden tıklayarak kullanıcı formu gönderdiğinde, kendisine göndermek form **filtre** düğmesi.

Uygulamayı çalıştırmak ve bir filmi için arama yapmayı deneyin.

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

Var olan hiçbir `HttpPost` aşırı yükünü `SearchIndex` yöntemi. Yöntemi uygulama durumunu değiştirme değildir çünkü, yalnızca verileri filtreleme gerekmez.

Aşağıdaki ekleyebilirsiniz `HttpPost SearchIndex` yöntemi. Bu durumda, eylem çağırıcısını BC `HttpPost SearchIndex` yöntemi ve `HttpPost SearchIndex` yöntemi aşağıdaki resimde gösterildiği gibi çalıştırın.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

Ancak, bu eklediğiniz bile `HttpPost` sürümünü `SearchIndex` yöntemi nasıl bu tüm uygulanmıştır içinde bir sınırlama yoktur. Belirli bir arama yer işareti koymak istediğiniz veya bunlar filmler aynı filtrelenmiş listesini görmek için tıklayabilirsiniz arkadaşlarınıza bağlantı göndermek istediğiniz düşünün. HTTP POST isteği URL'sini GET isteği (localhost:xxxxx filmler/SearchIndex) URL'si ile aynıdır; URL içinde hiçbir arama bilgisi dikkat edin. Sağ artık, arama dizesi bilgilerini sunucuya biçiminde alan değeri gönderilir. Bu, yer işareti veya bir URL içinde arkadaş göndermek için bu arama bilgileri yakalayamazsınız anlamına gelir.

Çözüm bir aşırı yüklemesini kullanmaktır `BeginForm` POST isteği URL'sini arama bilgilerini eklemeniz gerekir ve HttpGet sürümüne yönlendirileceğini belirtir `SearchIndex` yöntemi. Parametresiz varolan `BeginForm` aşağıdaki yöntemi:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

Artık bir arama gönderdiğinizde, URL'yi bir arama sorgu dizesi içerir. Arama de gider için `HttpGet SearchIndex` eylem yöntemi, varsa bile bir `HttpPost SearchIndex` yöntemi.

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>Türe göre arama ekleme

Eklediyseniz `HttpPost` sürümünü `SearchIndex` yöntemi, şimdi silin.

Ardından, kullanıcıların filmler için türe göre arama bir özellik ekleyeceksiniz. Değiştirin `SearchIndex` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

Bu sürümü `SearchIndex` yöntemi alır ek bir parametre, yani `movieGenre`. İlk birkaç kod satırlarının bir `List` film türleri veritabanından tutacak nesne.

Tüm türleri veritabanından alır bir LINQ Sorgu kodudur.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

Kod `AddRange` genel yöntemini `List` farklı türleri listesine eklemek için koleksiyonu. (Olmadan `Distinct` değiştiricisi, yinelenen tür eklenecek — Örneğin, iki kez örneğimizi Komedi eklenir). Kod içinde türleri listesini ardından depolar `ViewBag` nesne.

Aşağıdaki kod nasıl kontrol edileceğini göstermektedir `movieGenre` parametresi. Boş değilse, belirtilen türe seçili film sınırlamak için filmler sorgu kodu daha ayrıntılı kısıtlar.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Türe göre ara desteklemek için SearchIndex görünümü biçimlendirme ekleme

Ekleme bir `Html.DropDownList` yardımcıya *Views\Movies\SearchIndex.cshtml* hemen önce dosya `TextBox` Yardımcısı. Tamamlanan biçimlendirme aşağıda gösterilmiştir:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

Uygulamayı çalıştırmak ve göz atın */filmler/SearchIndex*. Türe, film adı ve her iki ölçütlere göre bir arama deneyin.

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

Bu bölümde CRUD eylem yöntemleri ve framework tarafından oluşturulan görünümler incelenir. Bir arama eylem yöntemi ve kullanıcıların filmi Tarz arama görünümü oluşturduğunuz. Sonraki bölümde, bir özelliğin ekleneceği nasıl şu konuları inceleyeceğiz `Movie` modeli ve bir test veritabanı otomatik olarak oluşturacak bir başlatıcı ekleme.

> [!div class="step-by-step"]
> [Önceki](accessing-your-models-data-from-a-controller.md)
> [İleri](adding-a-new-field-to-the-movie-model-and-table.md)
