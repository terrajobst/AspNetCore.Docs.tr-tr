---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Düzenleme metotlarını ve düzenleme görünümünü İnceleme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 950af5fad99bebc5f9d49f30c2ce1e9e4e5c0f25
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755811"
---
<a name="examining-the-edit-methods-and-edit-view"></a>Düzenleme metotlarını ve düzenleme görünümünü İnceleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Bu bölümde, oluşturulan inceleyeceğiz `Edit` eylem metotları ve görünümleri film denetleyicisi. Ancak önce daha iyi Ara yayın tarihi yapmak için kısa bir değişiklik olacak. Açık *Models\Movie.cs* dosya ve aşağıda vurgulanan satırları ekleyin:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

Bu gibi belirli tarih kültürü de yapabilirsiniz:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Şu konulara değineceğiz [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) sonraki öğreticide. [Görüntüleme](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) öznitelik adı (Bu durumda "ReleaseDate" yerine "yayın tarihi") bir alan için görüntülenecek öğeleri belirtir. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) alanında depolanan saat bilgisi görüntülenmez bu durumda, bir tarih olduğundan, öznitelik veri türü belirtir. [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) özniteliği yanlış tarih biçimleri işleyen Chrome tarayıcıda bir hata için gereklidir.

Uygulamayı çalıştırmak ve göz atın `Movies` denetleyicisi. Fare işaretçisini tutun bir **Düzenle** bağlantı URL'sini görmek için bağlantı.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Düzenle** bağlantı tarafından oluşturulan `Html.ActionLink` yönteminde *Views\Movies\Index.cshtml* görüntüle:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Nesnesi üzerinde bir özelliği kullanılarak açığa çıkarılır bir yardımcı olan [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) temel sınıfı. `ActionLink` Yardımcı yöntemini dinamik olarak HTML köprüler, eylem yöntemlerine denetleyicilerde bağlantı oluşturmak kolaylaştırır. İlk bağımsız değişkeni `ActionLink` yöntemdir işlemek için bağlantı metni (örneğin, `<a>Edit Me</a>`). Çağırılacak eylem yönteminin ikinci bağımsız değişkeni addır (Bu durumda, `Edit` eylem). Son bağımsız değişken bir [anonim nesne](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) , rota verilerini (Bu durumda 4 kimliği) oluşturur.

Önceki görüntüde verilen oluşturulan bağlantı `http://localhost:1234/Movies/Edit/4`. Varsayılan yol (içinde belirlenen *uygulama\_Start\RouteConfig.cs*) URL deseni alan `{controller}/{action}/{id}`. Bu nedenle, ASP.NET çevirir `http://localhost:1234/Movies/Edit/4` bir istek halinde `Edit` eylem yöntemi `Movies` denetleyicisi parametresiyle `ID` 4 eşittir. Aşağıdaki kod İnceleme *uygulama\_Start\RouteConfig.cs* dosya. [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) yöntemi, HTTP isteklerini doğru denetleyici ve eylem yöntemi için rota ve isteğe bağlı ID parametresi kullanılır. [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) yöntemi tarafından da kullanılır [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) gibi `ActionLink` verilen denetleyici, eylem yöntemi ve herhangi bir rota veri URL'lerini oluşturmak için.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

Sorgu dizesi kullanarak bir eylem yöntemi parametrelerine de geçirebilirsiniz. Örneğin, URL `http://localhost:1234/Movies/Edit?ID=3` ayrıca parametresini geçirir `ID` için 3 `Edit` eylem yöntemi `Movies` denetleyicisi.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Açık `Movies` denetleyicisi. İki `Edit` eylem yöntemleri aşağıda gösterilmektedir.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

İkinci fark `Edit` eylem yöntemine öncesinde `HttpPost` özniteliği. Bu öznitelik belirten aşırı yüklemesini `Edit` yöntemi yalnızca POST istekleri için çağrılan. Geçerli olabilir `HttpGet` ilk özniteliği Düzenle yöntemi, ancak bu gerekli değildir, varsayılan olduğundan. (Biz örtük olarak atanmış olan eylem yöntemleri için başvuracağınız `HttpGet` olarak özniteliği `HttpGet` yöntemler.) [Bağlama](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) özniteliktir veri modeliniz için fazladan yayınlayarak bilgisayar korsanlarının tutan başka bir önemli güvenlik mekanizması. Yalnızca, özelliklerini değiştirmek istediğiniz bind özniteliği içermelidir. Overposting ve bağlama özniteliğinde okuyabilirsiniz my [güvenlik notu overposting](https://go.microsoft.com/fwlink/?LinkId=317598). Bu öğreticide kullanılan basit model biz modelinde tüm veri bağlama. [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) özniteliği istek sahteciliğini önlemek için kullanılır ve ile eşleştirilmiş `@Html.AntiForgeryToken()` düzenleme görünümü dosyasında (*Views\Movies\Edit.cshtml*), bir bölümü aşağıda verilmiştir:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` ile eşleşen bir gizli form sahteciliğe karşı koruma belirteci oluşturan `Edit` yöntemi `Movies` denetleyicisi. Daha fazla bilgi edinebilirsiniz hakkında siteler arası istek sahteciliği (XSRF veya CSRF olarak da bilinir) my öğreticide [MVC XSRF/CSRF önleme](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

`HttpGet` `Edit` Yöntemi film kimliği parametre alır, Entity Framework kullanarak filmi arar `Find` yöntemi ve düzenleme görünümü seçili film döndürür. Bir filmi bulunamazsa [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) döndürülür. Yapı iskelesi sistem düzenleme görünümü oluşturduğunuzda, onu incelenirken `Movie` sınıfı ve işlemek için oluşturulan kodu `<label>` ve `<input>` sınıfın her bir özellik için öğeleri. Aşağıdaki örnek, visual studio yapı iskelesi sistem tarafından oluşturulan düzenleme görünümünü gösterir:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Şablonu Görüntüle nasıl olduğunu fark bir `@model MvcMovie.Models.Movie` deyimini dosyanın üst — bu görünüm model görünüm şablonu türünde olmasını bekliyor belirtir `Movie`.

İskele kurulan kodu birkaç kullanan *yardımcı yöntemler* HTML biçimlendirmeyi basitleştirmek için. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Yardımcı, alan adını görüntüler (&quot;başlık&quot;, &quot;ReleaseDate&quot;, &quot;Tarz&quot;, veya &quot;fiyat &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) İşleyen bir HTML Yardımcısı `<input>` öğesi. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Yardımcısı, bu özellikle ilişkili herhangi bir doğrulama iletisi görüntüler.

Uygulamayı çalıştırmak ve gidin */Movies* URL'si. ' A tıklayın bir **Düzenle** bağlantı. Tarayıcıda, sayfa için kaynağı görüntüleyin. HTML form öğesi için aşağıda gösterilmiştir.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

`<input>` Öğeleri olan bir HTML `<form>` öğesi olan `action` özniteliğinin ayarlanmış gönderinin yayımlanacağı */filmler/düzenleme* URL'si. Form verileri sunucuya yayımlanacak olduğunda **Kaydet** düğmesine tıklandığında. İkinci satır gizli gösterir [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) tarafından oluşturulan belirteç `@Html.AntiForgeryToken()` çağırın.

## <a name="processing-the-post-request"></a>POST isteğini işleme

Aşağıdaki liste gösterildiği `HttpPost` sürümünü `Edit` eylem yöntemi.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

[ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) özniteliği doğrular [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) tarafından oluşturulan belirteç `@Html.AntiForgeryToken()` Görünümü'nde arayın.

[ASP.NET MVC model bağlayıcı](https://msdn.microsoft.com/library/dd410405.aspx) gönderilen form değerlerini alır ve oluşturur bir `Movie` olarak geçirilen nesne `movie` parametresi. `ModelState.IsValid` Yöntemi doğrular (düzenleme veya güncelleştirme) değiştirileceğini biçiminde gönderilen veriler kullanılabilir bir `Movie` nesne. Veriler geçerliyse, film verileri kaydedilen `Movies` koleksiyonunu `db(MovieDBContext` örnek). Çağrı yaparak yeni film verileri veritabanına kaydedilir `SaveChanges` yöntemi `MovieDBContext`. Verileri kaydettikten sonra kodu kullanıcı için yönlendiren `Index` eylem yöntemi `MoviesController` yaptığınız değişiklikleri içeren film koleksiyonu görüntüleyen sınıfı.

Bir alan değerleri geçerli olmayan istemci tarafı doğrulama belirler hemen sonra bir hata iletisi görüntülenir. JavaScript devre dışı bırakırsanız, istemci tarafı doğrulama olmaz ancak sunucu algılar gönderilen değerler geçerli değil ve form değerleri, hata iletileri ile yeniden. Öğreticinin ilerleyen bölümlerinde daha ayrıntılı bir doğrulama inceleyeceğiz.

`Html.ValidationMessageFor` Yardımcılar olarak *Edit.cshtml* görünüm şablonu uygun hata iletilerini görüntüleme ilgileniriz.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Tüm `HttpGet` benzer bir desen yöntemleri uygulayın. Bir film nesnesi aldıkları (veya durumunda bir nesneler listesi `Index`) ve model görünümüne geçirin. `Create` Yöntemi bir boş film nesne oluşturma görünümüne geçirir. Bu nedenle, oluşturmak, düzenlemek, silmek veya aksi halde verileri değiştiren tüm yöntemler yapmak `HttpPost` yöntemi aşırı yüklemesi. Bir HTTP GET yöntemi verileri değiştirmek, bir güvenlik riski blog gönderisi girişi açıklandığı [ASP.NET MVC ipucu #46 – bunlar güvenlik açıkları oluşturduğundan Sil bağlantılarını kullanmayın](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Bir GET yöntemi verilerde değişiklik de ihlal HTTP en iyi yöntemler ve mimari [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) desen, GET istekleri, uygulamanızın durumunu değiştirmemesi gerekir belirtir. Diğer bir deyişle, bir GET işlemi gerçekleştirilirken yan etkileri olan ve verilerinizi kalıcı değiştirmez güvenli bir işlem olmalıdır.

ABD İngilizcesi bilgisayar kullanıyorsanız, bu bölümü atlayın ve sonraki öğreticiye geçin. Bu öğreticide Globalize sürümünü indirebilirsiniz [burada](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Bir mükemmel iki bölümlü öğreticisi için uluslararası duruma getirme bkz [Nadeem'ın ASP.NET MVC 5 uluslararası duruma getirme](http://afana.me/post/aspnet-mvc-internationalization.aspx).


> [!NOTE]
> JQuery doğrulama virgül İngilizce olmayan yerel ayara yönelik desteği için (&quot;,&quot;) ondalık ve ABD İngilizce olmayan tarih biçimleri için içermelidir *globalize.js* ve size özgü  *cultures/globalize.cultures.js* dosyası (gelen [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) ve kullanmak için JavaScript'i `Globalize.parseFloat`. JQuery İngilizce olmayan doğrulama Nuget'ten alabilirsiniz. (Bir İngilizce yerel ayarı kullanıyorsanız Globalize yüklemeyin.)


1. Gelen **Araçları** menüsünü tıklatın **NuGetLibrary Paket Yöneticisi**ve ardından **çözüm için NuGet paketlerini Yönet**.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. Sol bölmeden <strong>Gözat *.</strong>* (Aşağıdaki resme bakın.)
3. Giriş kutusuna girin * Globalize **.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) Seçin `jQuery.Validation.Globalize`, seçin `MvcMovie` tıklatıp **yükleme**. *Scripts\jquery.globalize\globalize.js* dosyası projenize eklenir. *Scripts\jquery.globalize\cultures\* klasör birçok kültür JavaScript dosyaları içerir. Unutmayın, bu paketi yüklemek için beş dakika sürebilir.

   Aşağıdaki kod Views\Movies\Edit.cshtml dosya için yapılan değişiklikleri gösterir: 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Bu kod her düzenleme Görünümü'nde tekrarlamayı önlemek üzere Düzen dosyası için taşıyabilirsiniz. Komut dosyası indirme en iyi duruma getirmek için benim öğreticiye bakın [paketleme ve küçültme](../../performance/bundling-and-minification.md).

Daha fazla bilgi için [ASP.NET MVC 3 uluslararası duruma getirme](http://afana.me/post/aspnet-mvc-internationalization.aspx) ve [ASP.NET MVC 3 uluslararası - 2. Bölüm (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Geçici bir düzeltme doğrulama yerel çalışma alınamıyor, bilgisayarınızı ABD İngilizcesi kullanmaya zorlayabilir veya tarayıcınızda JavaScript devre dışı bırakabilirsiniz. Bilgisayarınızı ABD İngilizcesi kullanmaya zorlamak için proje kök dizinine Genelleştirme öğe ekleyebilirsiniz. *web.config* dosya. Aşağıdaki kodu, ABD İngilizcesi olarak ayarlamak kültürüyle Genelleştirme öğeyi gösterir.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a> Sonraki öğreticide, biz arama işlevini uygulayacaksınız.

> [!div class="step-by-step"]
> [Önceki](accessing-your-models-data-from-a-controller.md)
> [İleri](adding-search.md)
