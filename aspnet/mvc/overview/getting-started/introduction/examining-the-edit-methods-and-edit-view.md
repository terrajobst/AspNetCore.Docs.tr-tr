---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: "Düzenleme görünümü ve düzenleme yöntemler inceleniyor | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: d7e1ba503b8aa815cebf431d2f5ffc9436b3575b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>Düzenleme görünümü ve düzenleme yöntemler inceleniyor
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

Bu bölümde, oluşturulan inceleyeceğiz `Edit` eylem yöntemleri ve görünümler film denetleyicisi. Ancak daha iyi Ara yayın tarihi yapmak için kısa bir değişiklik ilk alacaktır. Açık *Models\Movie.cs* dosya ve aşağıda gösterilen vurgulanan satırları ekleyin:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

Bu gibi belirli tarih kültür de yapabilirsiniz:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Şu konulara değineceğiz [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) sonraki öğreticide. [Görüntülemek](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) özniteliği ne bir alanın adını (Bu durumda "ReleaseDate" yerine "yayın tarihi") için görüntülenecek belirtir. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) özniteliği veri türünü belirtir, alanda depolanan saat bilgisi gösterilmesi için bu durumda bir tarih. [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) özniteliği tarih biçimleri hatalı işler Chrome tarayıcıda bir hata için gereklidir.

Uygulamayı çalıştırın ve Gözat `Movies` denetleyicisi. Fare işaretçisini tutun bir **Düzenle** bağlandığı URL'yi görmek için bağlantı.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Düzenle** bağlantı tarafından üretilen `Html.ActionLink` yönteminde *Views\Movies\Index.cshtml* görünümü:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Nesnesi üzerinde bir özelliği kullanılarak kullanıma sunulan bir yardımcı olan [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) temel sınıfı. `ActionLink` Yardımcı yöntemini eylem yöntemlerine denetleyicilerde bağlantı HTML köprüler dinamik olarak oluşturulacak kolaylaştırır. İlk bağımsız değişken `ActionLink` yöntemdir işlemek için bağlantı metni (örneğin, `<a>Edit Me</a>`). İkinci bağımsız değişkeni çağırılacak eylem yönteminin adıdır (Bu durumda, `Edit` eylem). Son bağımsız değişken bir [anonim nesneyi](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) (Bu durumda, 4 kimliği) rota verilerini oluşturur.

Önceki görüntüde gösterildiği oluşturulan bağlantı `http://localhost:1234/Movies/Edit/4`. Varsayılan yol (oluşturulmuş *uygulama\_Start\RouteConfig.cs*) URL deseni alır `{controller}/{action}/{id}`. Bu nedenle, ASP.NET çevirir `http://localhost:1234/Movies/Edit/4` bir istek içine `Edit` eylem yöntemi `Movies` parametresiyle denetleyicisi `ID` 4 eşittir. Aşağıdaki kod inceleyin *uygulama\_Start\RouteConfig.cs* dosya. [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) yöntemi, HTTP isteklerini doğru denetleyici ve eylem yöntemine yönlendirmek ve isteğe bağlı ID parametresi sağlamak için kullanılır. [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) yöntemi tarafından kullanılan de [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) gibi `ActionLink` verilen denetleyici, eylem yöntemi ve herhangi bir rota veri URL üretmek için.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

Eylem yöntemi parametrelerini bir sorgu dizesi kullanarak da geçirebilirsiniz. Örneğin, URL `http://localhost:1234/Movies/Edit?ID=3` parametresi de geçirir `ID` için 3'ün `Edit` eylem yöntemi `Movies` denetleyicisi.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Açık `Movies` denetleyicisi. İki `Edit` eylem yöntemleri aşağıda gösterilmektedir.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

İkinci fark `Edit` tarafından eylem yöntemi öncesinde `HttpPost` özniteliği. Bu öznitelik belirleyen aşırı yüklemesini `Edit` yöntemi yalnızca POST istekleri için çağrılan. Geçerli olabilir `HttpGet` ilk öznitelik Düzenle yöntemi, ancak bu gerekli değildir, varsayılan olduğundan. (Örtük olarak atanmış olan eylem yöntemlerine bakın `HttpGet` olarak özniteliği `HttpGet` yöntemlerini.) [Bağlamak](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) veri modeliniz için aşırı gönderim bilgisayar korsanlarının tutar başka bir önemli güvenlik mekanizması bir özniteliktir. Değiştirmek istediğiniz bağlama özniteliğinde özellikleri yalnızca içermesi gerekir. Overposting ve bağ özniteliğinde hakkında bilgi edinebilirsiniz my [güvenlik notu overposting](https://go.microsoft.com/fwlink/?LinkId=317598). Bu öğreticide kullanılan basit modelde biz modeldeki tüm veri bağlama. [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) özniteliği istek sahteciliğini önlemek için kullanılır ve ile eşleştirilmiş `@Html.AntiForgeryToken()` düzenleme görünümü dosyasındaki (*Views\Movies\Edit.cshtml*), bir bölümü aşağıda verilmiştir:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()`içinde eşleşmelidir bir gizli form sahteciliğe karşı koruma belirteci oluşturur `Edit` yöntemi `Movies` denetleyicisi. Daha fazla bilgiyi hakkında siteler arası istek sahteciliği (XSRF veya CSRF olarak da bilinir) my öğreticideki [XSRF/CSRF önleme mvc'de](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

`HttpGet` `Edit` Yöntemi film ID parametresi alır, Entity Framework kullanarak filmi arar `Find` yöntemi ve seçili film düzenleme görünümü döndürür. Bir filmi bulunamazsa [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) döndürülür. Yapı iskelesi sistem düzenleme görünümü oluşturduğunuzda, incelenmesi `Movie` sınıfı ve işlemek için oluşturulan kodu `<label>` ve `<input>` sınıfın her bir özellik için öğeleri. Aşağıdaki örnek, visual studio yapı iskelesi sistem tarafından oluşturulan düzenleme görünümü gösterir:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Şablonu görüntüleme nasıl sahip fark bir `@model MvcMovie.Models.Movie` deyimini dosyanın üst — bu görünüm model türünde olmasını şablonu görüntüleme için beklediğini belirtir `Movie`.

İskele kurulmuş kodu birkaç kullanan *yardımcı yöntemler* HTML biçimlendirmesi kolaylaştırmak için. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Yardımcı alanın adını görüntüler (&quot;başlık&quot;, &quot;ReleaseDate&quot;, &quot;Tarz&quot;, veya &quot;fiyatı &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Yardımcı işleyen bir HTML `<input>` öğesi. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Yardımcısı bu özellik ile ilişkili herhangi bir doğrulama iletisi görüntüler.

Uygulamayı çalıştırın ve gidin */Movies* URL. Tıklatın bir **Düzenle** bağlantı. Tarayıcıda, sayfa için kaynağı görüntüleyin. HTML form öğesi için aşağıda gösterilmiştir.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

`<input>` Öğeleridir bir HTML `<form>` öğesi, `action` özniteliği postalamak için ayarlanmış */filmler/düzenleme* URL. Form verileri sunucuya nakledilir zaman **kaydetmek** düğmesine tıklandığında. İkinci satır gizli gösterir [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) tarafından oluşturulan belirteç `@Html.AntiForgeryToken()` çağırın.

## <a name="processing-the-post-request"></a>POST isteği işleme

Aşağıdaki liste gösterildiği `HttpPost` sürümü `Edit` eylem yöntemi.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

[ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) özniteliği doğrular [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) tarafından oluşturulan belirteç `@Html.AntiForgeryToken()` görünümünde çağırın.

[ASP.NET MVC model bağlayıcı](https://msdn.microsoft.com/library/dd410405.aspx) gönderilen form değerleri alır ve oluşturan bir `Movie` olarak geçirilen nesne `movie` parametresi. `ModelState.IsValid` Yöntemi doğrular biçiminde gönderilen veriler (düzenleme veya güncelleştirme) değiştirmek için kullanılabilir bir `Movie` nesnesi. Veriler geçerliyse, film verileri kaydedilir `Movies` koleksiyonu `db(MovieDBContext` örnek). Yeni film verileri çağırarak veritabanına kaydedilir `SaveChanges` yöntemi `MovieDBContext`. Veriler kaydedildikten sonra kodu kullanıcı için yönlendiren `Index` eylem yöntemi `MoviesController` yaptığınız değişiklikler dahil film koleksiyon görüntüler sınıfı.

Bir alan değerlerini geçerli olmayan istemci tarafı doğrulama belirler hemen bir hata iletisi görüntülenir. JavaScript devre dışı bırakırsanız, istemci tarafı doğrulama olmaz ancak sunucu algılar gönderilen değerler geçerli değildir ve form değerleri hata iletileri ile görünürler. Öğreticinin ilerleyen bölümlerinde daha ayrıntılı doğrulama inceleyeceğiz.

`Html.ValidationMessageFor` Yardımcıları içinde *Edit.cshtml* şablonu uygun hata iletilerini görüntüleme ilgilenebilmek görünümü.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Tüm `HttpGet` benzer bir desen yöntemleri uygulayın. Film nesnesini alın (veya durumunda nesnelerin listesini `Index`) ve görünüm model geçirin. `Create` Yöntemi bir boş film nesnesi oluşturma görünümüne geçirir. Bu nedenle oluşturmak, düzenlemek, silmek veya aksi halde verileri değiştirme tüm yöntemleri yapmak `HttpPost` yönteminin. Bir HTTP GET yöntemi verileri değiştirme olan bir güvenlik riski blog gönderisine girişi açıklandığı gibi [ASP.NET MVC ipucu #46 – güvenlik açıklarını oluşturduğundan bağlantılarını sil kullanmayan](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). GET yöntemi verileri değiştirme de ihlal HTTP en iyi yöntemler ve mimari [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) desen, GET istekleri uygulamanızın durumunu değiştirmemeniz gerektiğini belirtir. Diğer bir deyişle, bir GET işlemi gerçekleştirilirken hiçbir yan etkisi olan ve kalıcı verilerinizi değiştirmeyen güvenli bir işlem olmalıdır.

ABD İngilizcesi bilgisayar kullanıyorsanız, bu bölüm atlayın ve sonraki öğretici gidin. Bu öğretici Globalize sürümünü indirebilirsiniz [burada](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Bir mükemmel iki bölümlü öğretici için uluslararası duruma getirme hakkında bkz [Nadeem'ın ASP.NET MVC 5 uluslararası](http://afana.me/post/aspnet-mvc-internationalization.aspx).


> [!NOTE]
> bir virgül İngilizce dışındaki yerel ayarlar için jQuery doğrulamasına desteklemek için (&quot;,&quot;) ondalık ve ABD İngilizcesi dışındaki tarih biçimleri için içermelidir *globalize.js* ve özel  *cultures/globalize.cultures.js* dosyası (gelen [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) ve kullanmak için JavaScript'i `Globalize.parseFloat`. JQuery İngilizce olmayan doğrulama Nuget'ten alabilirsiniz. (Bir İngilizce yerel ayar kullanıyorsanız Globalize yüklemeyin.)


1. Gelen **Araçları** menüsünü tıklatın **NuGetLibrary Paket Yöneticisi**ve ardından **çözüm için NuGet paketlerini Yönet**.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. Sol bölmede seçin **Gözat*. *** (aşağıdaki görüntü bakın.)
3. Giriş kutusuna * Globalize **.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png)Seçin `jQuery.Validation.Globalize`, seçin `MvcMovie` tıklatıp **yükleme**. *Scripts\jquery.globalize\globalize.js* dosyayı projenize eklenir. *Scripts\jquery.globalize\cultures\* klasörü birçok kültür JavaScript dosyaları içerir. Not: Bu paket yüklemek için beş dakika sürebilir.

 Aşağıdaki kod Views\Movies\Edit.cshtml dosya için yapılan değişiklikleri gösterir: 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Bu kodu her düzenleme görünümü, yinelenen önlemek için Düzen dosyasına taşıyabilirsiniz. Komut dosyası indirme en iyi duruma getirme my öğretici bkz [paketleme ve küçültme](../../performance/bundling-and-minification.md).

Daha fazla bilgi için bkz: [ASP.NET MVC 3 uluslararası hale getirme](http://afana.me/post/aspnet-mvc-internationalization.aspx) ve [ASP.NET MVC 3 uluslararası - Kısım 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Geçici bir düzeltme olarak yerel çalışma doğrulama alınamıyor, bilgisayarınızı ABD İngilizcesi kullanacak şekilde zorlayabilirsiniz veya tarayıcınızın JavaScript devre dışı bırakabilirsiniz. Bilgisayarınızı ABD İngilizcesi kullanacak şekilde zorlamak için projeleri kök Genelleştirme öğesi ekleyebilirsiniz. *web.config* dosya. Aşağıdaki kod, ABD İngilizcesi olarak ayarlamak kültür ile Genelleştirme öğesini gösterir.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a>Sonraki öğreticide biz arama işlevini uygulamanız.

>[!div class="step-by-step"]
[Önceki](accessing-your-models-data-from-a-controller.md)
[sonraki](adding-search.md)
