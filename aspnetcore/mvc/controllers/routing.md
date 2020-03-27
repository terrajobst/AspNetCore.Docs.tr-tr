---
title: ASP.NET Core denetleyici eylemlerine yönlendirme
author: rick-anderson
description: ASP.NET Core MVC 'nin, gelen isteklerin URL 'Lerini eşleştirmek ve bunları eylemlerle eşlemek için yönlendirme ara yazılımını nasıl kullandığını öğrenin.
ms.author: riande
ms.date: 3/25/2020
uid: mvc/controllers/routing
ms.openlocfilehash: c1c0d978714718af1de0f627e50a54f66ed391ed
ms.sourcegitcommit: 4b166b49ec557a03f99f872dd069ca5e56faa524
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80362657"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a>ASP.NET Core denetleyici eylemlerine yönlendirme

[Ryan şimdi ak](https://github.com/rynowak), [Kirk Larkabağı](https://twitter.com/serpent5)ve [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core denetleyicileri, gelen isteklerin URL 'Leriyle eşleştirmek ve bunları [eylemlerle](#action)eşlemek için yönlendirme [Ara yazılımını](xref:fundamentals/middleware/index) kullanır.  Rota şablonları:

* , Başlangıç kodunda veya özniteliklerde tanımlanmıştır.
* URL yollarının [eylemlerle](#action)nasıl eşleştiğini betimleyen.
* Bağlantıların URL 'Leri oluşturmak için kullanılır. Oluşturulan bağlantılar genellikle yanıtlar halinde döndürülür.

Eylemler [genel olarak-yönlendirildi](#cr) veya [Attribute-yönlendirildi](#ar)' dir. Bir yolun denetleyiciye veya [eyleme](#action) yerleştirilmesi, BT özniteliğinin yönlendirilmesini sağlar. Daha fazla bilgi için bkz. [karma yönlendirme](#routing-mixed-ref-label) .

Bu belge:

* MVC ve yönlendirme arasındaki etkileşimleri açıklar:
  * Tipik MVC uygulamalarının yönlendirme özelliklerini kullanma şekli.
  * Her ikisini de içerir:
    * Genellikle denetleyiciler ve görünümlerle kullanılan [genel olarak yönlendirme](#cr) .
    * REST API 'Leri ile kullanılan *öznitelik yönlendirme* . Birincil olarak REST API 'Leri için yönlendirme ile ilgileniyorsanız, [REST API 'leri Için öznitelik yönlendirme](#ar) bölümüne atlayın.
  * Bkz. Gelişmiş yönlendirme ayrıntıları için [yönlendirme](xref:fundamentals/routing) .
* ASP.NET Core 3,0 ' de eklenen varsayılan yönlendirme sisteminin Endpoint Routing olarak adlandırıldığını gösterir. Uyumluluk amaçlarıyla önceki yönlendirme sürümü ile denetleyicileri kullanmak mümkündür. Yönergeler için [2.2-3.0 geçiş kılavuzuna](xref:migration/22-to-30) bakın. Eski yönlendirme sistemindeki başvuru malzemeleri için [Bu belgenin 2,2 sürümüne](xref:mvc/controllers/routing?view=aspnetcore-2.2) bakın.

<a name="cr"></a>

## <a name="set-up-conventional-route"></a>Geleneksel rotayı ayarlama

`Startup.Configure` [geleneksel yönlendirme](#crd)kullanılırken, genellikle aşağıdakine benzer bir kod içerir:

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet)]

<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>çağrısının içinde, tek bir yol oluşturmak için <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> kullanılır. Tek yol `default` yol olarak adlandırılır. Denetleyiciler ve görünümler içeren çoğu uygulama, `default` yoluna benzer bir yol şablonu kullanır. REST API 'Leri [öznitelik yönlendirmeyi](#ar)kullanmalıdır.

Yol şablonu `"{controller=Home}/{action=Index}/{id?}"`:

* `/Products/Details/5` gibi bir URL yoluyla eşleşir
* Yolu simgeleyerek `{ controller = Products, action = Details, id = 5 }` yol değerlerini ayıklar. Uygulamanın `ProductsController` adlı bir denetleyici ve `Details` eylemi varsa yol değerlerinin ayıklanması bir eşleşme ile sonuçlanır:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippetA)]

 [Mydisplayrouteınfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) yöntemi [örnek karşıdan yüklemeye](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) dahildir ve yönlendirme bilgilerini görüntülemek için kullanılır.

  * `/Products/Details/5` model `id` parametresini `5`olarak ayarlamak için `id = 5` değerini bağlar. Daha fazla ayrıntı için bkz. [model bağlama](xref:mvc/models/model-binding) .
* `{controller=Home}` varsayılan `controller`olarak `Home` tanımlar.
* `{action=Index}` varsayılan `action`olarak `Index` tanımlar.
*  `{id?}` `?` karakteri `id` isteğe bağlı olarak tanımlar.
  * Bir eşleşme için URL yolunda varsayılan ve isteğe bağlı yol parametrelerinin mevcut olması gerekmez. Yol şablonu sözdiziminin ayrıntılı açıklaması için bkz. [route Template Reference](xref:fundamentals/routing#route-template-reference) .
* URL yolu `/`eşleşir.
* `{ controller = Home, action = Index }`yol değerlerini üretir.
* [Mydisplayrouteınfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) yöntemi [örnek karşıdan yüklemeye](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) dahildir ve yönlendirme bilgilerini görüntülemek için kullanılır.

`controller` ve `action` değerleri varsayılan değerleri kullanır. URL yolunda karşılık gelen bir kesim olmadığından `id` bir değer oluşturmuyor. `/` yalnızca bir `HomeController` ve `Index` eylemi varsa eşleşir:

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

Önceki denetleyici tanımı ve yönlendirme şablonunu kullanarak, aşağıdaki URL yolları için `HomeController.Index` eylemi çalıştırılır:

* `/Home/Index/17`
* `/Home/Index`
* `/Home`
* `/`

`/` URL yolu, yönlendirme şablonu varsayılan `Home` denetleyicilerini ve `Index` eylemini kullanır. URL yolu `/Home` yol şablonu varsayılan `Index` eylemini kullanır.

Kolaylık yöntemi <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>:

```csharp
endpoints.MapDefaultControllerRoute();
```

Yerine

```csharp
endpoints.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

Yönlendirme, <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> ve <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> ara yazılımı kullanılarak yapılandırılır. Denetleyicileri kullanmak için:

* [Öznitelik yönlendirmeli](#ar) denetleyicileri eşlemek için `UseEndpoints` içinde <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> çağırın.
* [Genel olarak yönlendirmeli](#cr) denetleyicileri eşlemek için <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> veya <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>çağırın.

<a name="routing-conventional-ref-label"></a>
<a name="crd"></a>

## <a name="conventional-routing"></a>Geleneksel yönlendirme

Geleneksel yönlendirme, denetleyiciler ve görünümlerle kullanılır. `default` yolu:

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet2)]

, *geleneksel yönlendirmeye*bir örnektir. Bu, URL yolları için bir *kural* oluşturduğundan *geleneksel yönlendirme* olarak adlandırılır:

* İlk yol segmenti `{controller=Home}`, denetleyicinin adıyla eşlenir.
* İkinci kesim `{action=Index}`, [eylem](#action) adıyla eşlenir.
* Üçüncü segment `{id?}`, isteğe bağlı bir `id`için kullanılır. `{id?}` `?`, isteğe bağlı hale getirir. `id`, bir model varlığına eşlemek için kullanılır.

Bu `default` yolunu kullanarak, URL yolu:

* `/Products/List` `ProductsController.List` eylemine eşlenir.
* `/Blog/Article/17` `BlogController.Article` eşlenir ve genellikle model `id` parametresini 17 ' ye bağlar.

Bu eşleme:

* **Yalnızca**denetleyiciyi ve [eylem](#action) adlarını temel alır.
* Ad alanlarını, kaynak dosya konumlarını veya yöntem parametrelerini temel alan değildir.

Varsayılan yol ile geleneksel yönlendirmeyi kullanmak, her eylem için yeni bir URL düzeniyle karşılaşmadan uygulamanın oluşturulmasına olanak sağlar. [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) stilinde eylemlere sahip bir uygulama için, denetleyiciler arasında URL 'ler için tutarlılık vardır:

* Kodu basitleştirmeye yardımcı olur.
* Kullanıcı arabirimini daha öngörülebilir hale getirir.

> [!WARNING]
> Yukarıdaki koddaki `id`, yol şablonu tarafından isteğe bağlı olarak tanımlanmıştır. Eylemler, URL 'nin bir parçası olarak belirtilen isteğe bağlı KIMLIK olmadan çalıştırılabilir. Genellikle, URL 'den`id` atlandığında:
>
> * `id` model bağlamaya göre `0` olarak ayarlanır.
> * Veritabanında eşleşen `id == 0`varlık bulunamadı.
>
> [Öznitelik yönlendirme](#ar) , kimliği bazı eylemler için gerekli hale getirmek için ayrıntılı denetim sağlar ve diğerleri için değildir. Kurala göre belgeler, doğru kullanımlarda görünmeme olasılığı olan `id` gibi isteğe bağlı parametreler içerir.

Çoğu uygulama, URL 'Lerin okunabilir ve anlamlı olması için temel ve açıklayıcı bir yönlendirme şeması seçmelidir. Varsayılan geleneksel yol `{controller=Home}/{action=Index}/{id?}`:

* Temel ve açıklayıcı bir yönlendirme düzenini destekler.
* , UI tabanlı uygulamalar için kullanışlı bir başlangıç noktasıdır.
* Birçok Web UI uygulaması için tek yol şablonu gereklidir. Daha büyük Web Kullanıcı arabirimi uygulamaları için, sık sık gerekli olan [alanlarda](#areas) başka bir yol kullanın.

<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> ve <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*>:

* , Çağrıldığı sıraya göre kendi uç noktalarına otomatik olarak bir **sipariş** değeri atayın.

ASP.NET Core 3,0 ve üzeri için uç nokta yönlendirme:

* Bir yol kavramı yoktur.
* , Genişletilebilirlik yürütülmesi için sıralama garantisi sağlamaz, tüm uç noktalar bir kerede işlenir.

<xref:Microsoft.AspNetCore.Routing.Route>, istekleri eşleştirme gibi yerleşik yönlendirme uygulamalarının nasıl yapıldığını görmek için [günlük kaydını](xref:fundamentals/logging/index) etkinleştirin.

[Öznitelik yönlendirme](#ar) bu belgenin ilerleyen kısımlarında açıklanmıştır.

<a name="mr"></a>

### <a name="multiple-conventional-routes"></a>Birden çok geleneksel yollar

<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> ve <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>daha fazla çağrı eklenerek `UseEndpoints` içine birden çok [geleneksel yol](#cr) eklenebilir. Bunun yapılması, birden çok kural tanımlamayı veya belirli bir [eyleme](#action)adanmış geleneksel yollar eklemeyi sağlar; örneğin:

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<a name="dcr"></a>

Yukarıdaki koddaki `blog` yolu, **adanmış bir geleneksel yoldur**. Ayrılmış bir geleneksel yol olarak adlandırılır çünkü:

* [Geleneksel yönlendirme](#cr)kullanır.
* Belirli bir [eyleme](#action)ayrılmıştır.

`controller` ve `action`, yol şablonunda `"blog/{*article}"` parametre olarak görünmüyor:

* Yalnızca varsayılan değerlere sahip olabilirler `{ controller = "Blog", action = "Article" }`.
* Bu yol her zaman `BlogController.Article`işlem ile eşlenir.

`/Blog`, `/Blog/Article`ve `/Blog/{any-string}`, blog rotası ile eşleşen tek URL yollarıdır.

Önceki örnek:

* `blog` yol, ilk eklendiği için `default` rotasına kıyasla daha yüksek önceliğe sahiptir.
* Ve, URL 'nin bir parçası olarak bir makale adı olması gereken tipik bir [başlık](https://developer.mozilla.org/docs/Glossary/Slug) stili yönlendirme örneğidir.

> [!WARNING]
> ASP.NET Core 3,0 ve üzeri sürümlerde yönlendirme:
> * *Yol*adlı bir kavram tanımlayın. `UseRouting`, ara yazılım ardışık düzenine eşleşen rota ekler. `UseRouting` ara yazılımı uygulamada tanımlanan uç noktalar kümesine bakar ve isteğe bağlı olarak en iyi uç nokta eşleşmesini seçer.
> * <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> veya <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>gibi genişletilebilirlik yürütme sırası hakkında garanti sağlar.
>
>Bkz. yönlendirme ile ilgili başvuru malzemeleri için [yönlendirme](xref:fundamentals/routing) .

<a name="cro"></a>

### <a name="conventional-routing-order"></a>Geleneksel yönlendirme sırası

Geleneksel yönlendirme yalnızca uygulama tarafından tanımlanan eylem ve denetleyicinin bir bileşimiyle eşleşir. Bu, geleneksel yolların çakıştığı durumları basitleştirmek için tasarlanmıştır.
<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>ve <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> kullanarak yollar eklendiğinde, çağrıldığı sıraya göre uç noktalarına otomatik olarak bir Order değeri atanır. Daha önce görüntülenen bir rotadaki eşleşmelerin önceliği daha yüksektir. Geleneksel yönlendirme sıra bağımlıdır. Genel olarak, alanlar içeren rotalar daha önce bir alan olmadan rotalardan daha belirgin olduklarından yerleştirilmelidir. `{*article}` gibi tüm rota parametrelerini yakala ile [adanmış geleneksel yollar](#dcr) , bir [yolun çok fazla](xref:fundamentals/routing#greedy)ve diğer yollarla eşleştirilmesini amaçladığı URL 'lerle eşleşmesi anlamına gelir. Doyumsuz yollarını daha sonra yol tablosuna yerleştirerek doyumsuz eşleşmelerini önleyin.

<a name="best"></a>

### <a name="resolving-ambiguous-actions"></a>Belirsiz eylemleri çözme

Yönlendirme ile iki uç nokta eşleşmesi durumunda, yönlendirme aşağıdakilerden birini yapmanız gerekir:

* En iyi aday ' ı seçin.
* Bir özel durum oluşturur.

Örneğin:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet9)]

Yukarıdaki denetleyici, eşleşen iki eylemi tanımlar:

* URL yolu `/Products33/Edit/17`
* Veri yolu `{ controller = Products33, action = Edit, id = 17 }`.

Bu, MVC denetleyicileri için tipik bir modeldir:

* `Edit(int)` bir ürünü düzenlemek için bir form görüntüler.
* `Edit(int, Product)`, postalanan formu işler.

Doğru yolu çözümlemek için:

* istek bir HTTP `POST`olduğunda `Edit(int, Product)` seçilir.
* [http fiili](#verb) başka bir şey olduğunda `Edit(int)` seçilir. `Edit(int)` genellikle `GET`aracılığıyla çağrılır.

<xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, `[HttpPost]`, isteğin HTTP yöntemine göre seçim yapabilmesi için yönlendirme için verilmiştir. `HttpPostAttribute`, `Edit(int)`kıyasla `Edit(int, Product)` daha uygun hale getirir.

`HttpPostAttribute`gibi özniteliklerin rol olduğunu anlamak önemlidir. Benzer öznitelikler diğer [http fiilleri](#verb)için tanımlanmıştır. [Geleneksel yönlendirmesinde](#cr), bir görüntüleme formu, gönder formu iş akışının bir parçası olduklarında aynı eylem adını kullanmak yaygın olarak kullanılan bir işlemdir. Örneğin, bkz. [Iki düzenleme eylemi yöntemini İnceleme](xref:tutorials/first-mvc-app/controller-methods-views#get-post).

Yönlendirme en iyi bir aday seçebilirse, birden fazla eşleşen uç noktayı listeleyerek bir <xref:System.Reflection.AmbiguousMatchException> oluşturulur.

<a name="routing-route-name-ref-label"></a>

### <a name="conventional-route-names"></a>Geleneksel yol adları

Aşağıdaki örneklerde `"blog"` ve `"default"` dizeler geleneksel yol adlarıdır:

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

Yol adları, yola mantıksal bir ad verir. Adlandırılmış yol, URL oluşturmak için kullanılabilir. Adlandırılmış bir yol kullanmak, yolların sıralaması URL oluşturma karmaşık hale geldiğinde URL oluşturmayı basitleştirir. Yol adları benzersiz uygulama genelinde olmalıdır.

Yol adları:

* İsteklerin URL 'siyle eşleşmesi veya işlenmesi üzerinde hiçbir etkisi yoktur.
* Yalnızca URL oluşturmak için kullanılır.

Yol adı kavramı [ıendpointnamemetadata](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata)olarak yönlendirme ile temsil edilir. Terimler **yol adı** ve **uç nokta adı**:

* Değiştirilebilir.
* Belgede kullanılan ve kod, açıklanan API 'ye bağlıdır.

<a name="attribute-routing-ref-label"></a>
<a name="ar"></a>

## <a name="attribute-routing-for-rest-apis"></a>REST API 'Leri için öznitelik yönlendirme

REST API 'Leri, uygulamanın işlevselliğini [http fiilleri](#verb)tarafından temsil edilen bir kaynak kümesi olarak modellemek için öznitelik yönlendirmeyi kullanmalıdır.

Öznitelik yönlendirme eylemleri doğrudan yönlendirme şablonlarına eşlemek için bir öznitelik kümesi kullanır. Aşağıdaki `StartUp.Configure` kodu bir REST API için tipik bir sonraki örnekte kullanılır:

[!code-csharp[](routing/samples/3.x/main/StartupApi.cs?name=snippet)]

Yukarıdaki kodda, öznitelik yönlendirmeli denetleyicileri eşlemek için `UseEndpoints` içinde <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> çağırılır.

Aşağıdaki örnekte:

* Yukarıdaki `Configure` yöntemi kullanılır.
* `HomeController`, varsayılan geleneksel yol `{controller=Home}/{action=Index}/{id?}` eşleşenleri ile benzer bir URL kümesiyle eşleşir.

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

`HomeController.Index` eylemi, `/`, `/Home`, `/Home/Index`veya `/Home/Index/3`URL yollarından herhangi biri için çalıştırılır.

Bu örnek, öznitelik yönlendirme ve [geleneksel yönlendirme](#cr)arasında bir temel programlama farkı vurgulamaktadır. Öznitelik yönlendirme için bir yol belirtmek için daha fazla giriş gerekir. Geleneksel varsayılan yol, yönlendirmeleri daha succinctly işler. Ancak, öznitelik yönlendirme izin verir ve her [eyleme](#action)hangi rota şablonlarının uygulanacağını kesin olarak denetler.

Öznitelik yönlendirme ile, denetleyici adı ve eylem adları, eylem ile eşleşen **hiçbir** rol oynar. Aşağıdaki örnek, önceki örnekle aynı URL 'Lerle eşleşir:

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

Aşağıdaki kod `action` ve `controller`için belirteç değişimini kullanır:

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet22)]

Aşağıdaki kod denetleyiciye `[Route("[controller]/[action]")]` geçerlidir:

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

Yukarıdaki kodda `Index` yöntemi şablonları, yol şablonlarına sonuna `/` veya `~/` eklenmelidir. `/` veya `~/` ile başlayan bir eyleme uygulanan yol şablonları denetleyiciye uygulanan yol şablonları ile birleştirilmemelidir.

Rota şablonu seçimi hakkında bilgi için bkz. [route Template önceliği](xref:fundamentals/routing#rtp) .

## <a name="reserved-routing-names"></a>Ayrılmış yönlendirme adları

Aşağıdaki anahtar sözcükler, denetleyiciler veya Razor Pages kullanılırken ayrılmış yol parametresi adlarıdır:

* `action`
* `area`
* `controller`
* `handler`
* `page`

Öznitelik yönlendirme ile yol parametresi olarak `page` kullanmak yaygın bir hatadır. Bunun yapılması, URL oluşturma ile tutarsız ve kafa karıştırıcı davranışa neden olur.

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo2Controller.cs?name=snippet)]

Özel parametre adları, URL oluşturma işleminin bir Razor sayfasına mı yoksa bir denetleyiciye mi başvurduğunu öğrenmek için URL oluşturma tarafından kullanılır.

<a name="verb"></a>

## <a name="http-verb-templates"></a>HTTP fiili şablonları

ASP.NET Core aşağıdaki HTTP fiili şablonlarına sahiptir:

* [HttpGet](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)
* [HttpPost](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)
* [[HttpPut]](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)
* [[HttpDelete]](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)
* [[HttpHead]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)
* [[HttpPatch]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)

<a name="rt"></a>

### <a name="route-templates"></a>Rota şablonları

ASP.NET Core aşağıdaki yol şablonlarına sahiptir:

* Tüm [http fiili şablonları](#verb) rota şablonlarıdır.
* [Yolu](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)

<a name="arx"></a>

### <a name="attribute-routing-with-http-verb-attributes"></a>Http fiili öznitelikleriyle öznitelik yönlendirme

Aşağıdaki denetleyiciyi göz önünde bulundurun:

[!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet)]

Önceki kodda:

* Her eylem yalnızca HTTP GET istekleriyle eşleştirmeyi kısıtlayan `[HttpGet]` özniteliğini içerir.
* `GetProduct` eylemi `"{id}"` şablonunu içerir, bu nedenle `id` denetleyicideki `"api/[controller]"` şablonuna eklenir. Yöntemler şablonu `"api/[controller]/"{id}""`. Bu nedenle bu eylem yalnızca form `/api/test2/xyz`,`/api/test2/123`,`/api/test2/{any string}`vb. için GET istekleri ile eşleşir.
  [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet2)]
* `GetIntProduct` eylemi `"int/{id:int}")` şablonunu içerir. Şablonun `:int` kısmı, `id` yol değerlerini bir tamsayıya dönüştürülebilen dizelere kısıtlar. `/api/test2/int/abc`için bir GET isteği:
  * Bu eylemle eşleşmiyor.
  * Bir [404 bulunamadı](https://developer.mozilla.org/docs/Web/HTTP/Status/404) hatası döndürür.
    [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet3)]
* `GetInt2Product` eylemi şablonda `{id}` içerir, ancak `id` bir tamsayıya dönüştürülemeyen değerlerle kısıtlamaz. `/api/test2/int2/abc`için bir GET isteği:
  * Bu rota ile eşleşir.
  * Model bağlama `abc` tamsayıya dönüştüremiyor. Metodun `id` parametresi tamsayıdır.
  * Model bağlama`abc` tamsayıya dönüştürülemediğinden, [Hatalı bir istek 400](https://developer.mozilla.org/docs/Web/HTTP/Status/400) döndürür.
      [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet4)]

Öznitelik yönlendirme <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, <xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute>ve <xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute>gibi <xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute> öznitelikleri kullanabilir. Tüm [http fiili](#verb) öznitelikleri bir yol şablonunu kabul eder. Aşağıdaki örnekte, aynı yol şablonuyla eşleşen iki eylem gösterilmektedir:

[!code-csharp[](routing/samples/3.x/main/Controllers/MyProductsController.cs?name=snippet1)]

URL yolu `/products3`kullanılıyor:

* `MyProductsController.ListProducts` eylemi, [http fiili](#verb) `GET`olduğunda çalışır.
* `MyProductsController.CreateProduct` eylemi, [http fiili](#verb) `POST`olduğunda çalışır.

Bir REST API oluştururken, eylem tüm HTTP yöntemlerini kabul ettiğinden bir eylem yönteminde `[Route(...)]` kullanmanız gerekecektir. API 'nizin neleri desteklediği hakkında kesin olması için, daha özel [http fiili özniteliği](#verb) kullanmak daha iyidir. REST API 'lerinin istemcileri, hangi yolların ve HTTP fiillerinin belirli mantıksal işlemlere eşlendiğini bilmelidir.

REST API 'Leri, uygulamanın işlevselliğini HTTP fiilleri tarafından temsil edilen bir kaynak kümesi olarak modellemek için öznitelik yönlendirmeyi kullanmalıdır. Bu, örneğin, aynı mantıksal kaynaktaki al ve postala gibi birçok işlemin aynı URL 'YI kullanması anlamına gelir. Öznitelik yönlendirme, bir API 'nin Genel uç nokta yerleşimini dikkatle tasarlamak için gereken bir denetim düzeyi sağlar.

Bir öznitelik yolu belirli bir eyleme uyguladığı için, yol şablonu tanımının bir parçası olarak gerekli parametreleri yapmak kolaydır. Aşağıdaki örnekte, URL yolunun bir parçası olarak `id` gereklidir:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

`Products2ApiController.GetProduct(int)` eylemi:

* `/products2/3` gibi URL yoluyla çalıştırılır
* URL yolu `/products2`ile çalıştırılmadı.

[[Tüketir]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>) özniteliği, desteklenen istek içerik türlerini sınırlama eylemi sağlar. Daha fazla bilgi için bkz. [desteklenen istek içerik türlerini tüketir özniteliğiyle tanımlama](xref:web-api/index#consumes).

 Yol şablonlarının ve ilgili seçeneklerin tam açıklaması için bkz. [yönlendirme](xref:fundamentals/routing) .

`[ApiController]`hakkında daha fazla bilgi için bkz. [Apicontroller özniteliği](xref:web-api/index##apicontroller-attribute).

## <a name="route-name"></a>Yönlendirme adı

Aşağıdaki kod `Products_List`yol adını tanımlar:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

Yol adları, belirli bir yolu temel alan bir URL oluşturmak için kullanılabilir. Yol adları:

* Yönlendirmenin URL eşleşen davranışı üzerinde hiçbir etkisi yoktur.
* Yalnızca URL oluşturma için kullanılır.

Yol adları, uygulama genelinde benzersiz olmalıdır.

Önceki kodu, `id` parametresini isteğe bağlı (`{id?}`) olarak tanımlayan geleneksel varsayılan rotayla kontrast. API 'Leri tam olarak belirtme özelliği, `/products` ve `/products/5` farklı eylemlere dağıtılması gibi avantajlar sağlar.

<a name="routing-combining-ref-label"></a>

## <a name="combining-attribute-routes"></a>Öznitelik yollarını birleştirme

Öznitelik yönlendirmeyi daha az tekrarlı hale getirmek için, denetleyicideki yol öznitelikleri, bireysel eylemlerdeki rota öznitelikleriyle birleştirilir. Denetleyicide tanımlanan tüm yol şablonları, eylemlerdeki rota şablonlarına eklenir. Bir Route özniteliğinin denetleyiciye yerleştirilmesi, denetleyicideki **Tüm** eylemlerin öznitelik yönlendirme kullanmasını sağlar.

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet)]

Yukarıdaki örnekte:

* URL yolu `/products` eşleşiyor `ProductsApi.ListProducts`
* URL yolu `/products/5` `ProductsApi.GetProduct(int)`ile eşleştirebilir.

Bu eylemlerin her ikisi de `[HttpGet]` özniteliğiyle işaretlendiğinden HTTP `GET` eşleşir.

`/` veya `~/` ile başlayan bir eyleme uygulanan yol şablonları denetleyiciye uygulanan yol şablonları ile birleştirilmemelidir. Aşağıdaki örnek, varsayılan rotaya benzer bir URL yolları kümesiyle eşleşir.

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet)]

Aşağıdaki tabloda, önceki koddaki `[Route]` öznitelikleri açıklanmaktadır:

| Öznitelik               | `[Route("Home")]` ile birleştirir | Rota şablonunu tanımlar |
| ----------------- | ------------ | --------- |
| `[Route("")]` | Yes | `"Home"` |
| `[Route("Index")]` | Yes | `"Home/Index"` |
| `[Route("/")]` | **Hayır** | `""` |
| `[Route("About")]` | Yes | `"Home/About"` |

<a name="routing-ordering-ref-label"></a>
<a name="oar"></a>

### <a name="attribute-route-order"></a>Öznitelik yolu sırası

Yönlendirme bir ağaç oluşturur ve aynı anda tüm uç noktaları eşleştirir:

* Yol girdileri ideal bir sıralamaya yerleştirilmiş gibi davranır.
* En özel yolların, daha genel yollardan önce yürütülmesi şansınız vardır.

Örneğin, `blog/search/{topic}` gibi bir öznitelik yolu `blog/{*article}`gibi bir öznitelik rotasına göre daha özgüdür. Daha belirgin olduğundan, `blog/search/{topic}` yolu, varsayılan olarak daha yüksek önceliğe sahiptir. [Geleneksel yönlendirmeyi](#cr)kullanarak, yolları istenen sırada yerleştirmekten geliştirici sorumludur.

Öznitelik yolları <xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order> özelliğini kullanarak bir sıra yapılandırabilir. Sunulan tüm Framework [yol öznitelikleri](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) `Order` içerir. Yollar `Order` özelliğinin artan sıralamasına göre işlenir. Varsayılan sıra `0`. `Order = -1` kullanarak bir yolu ayarlama, bir sipariş ayarlamadığı rotalardan önce çalışır. `Order = 1` kullanarak bir yolu ayarlama varsayılan yol sıralaması sonrasında çalışır.

`Order`bağlı olmadığından **kaçının** . Bir uygulamanın URL 'SI alanı, doğru şekilde yönlendirmek için açık sıra değerleri gerektiriyorsa, bu durumda istemciler de kafa karıştırıcı olabilir. Genel olarak, öznitelik yönlendirme URL ile eşleşen doğru yolu seçer. URL oluşturma için kullanılan varsayılan sıra çalışmıyorsa, geçersiz kılma olarak bir yol adı kullanılması genellikle `Order` özelliğini uygulamaktan daha basittir.

Her ikisi de `/home`eşleşen yolu tanımlayan aşağıdaki iki denetleyicisi göz önünde bulundurun:

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

Yukarıdaki kodla `/home` istemek aşağıdakine benzer bir özel durum oluşturur:

```text
AmbiguousMatchException: The request matched multiple endpoints. Matches:

 WebMvcRouting.Controllers.HomeController.Index
 WebMvcRouting.Controllers.MyDemoController.MyIndex
```

Yol özniteliklerinden birine `Order` eklemek belirsizlik çözer:

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo3Controller.cs?name=snippet3& highlight=2)]

Yukarıdaki kodla `/home` `HomeController.Index` uç noktasını çalıştırır. `MyDemoController.MyIndex`almak için `/home/MyIndex`isteyin. **Not**:

* Yukarıdaki kod bir örnek veya kötü yönlendirme tasarımdır. `Order` özelliğini göstermek için kullanılmıştı.
* `Order` özelliği yalnızca belirsizlik çözümleniyor, bu şablon eşleştirilemez. `[Route("Home")]` şablonunu kaldırmak daha iyi olacaktır.

Bkz. [Razor Pages yol ve uygulama kuralları:](xref:razor-pages/razor-pages-conventions#route-order) rota sıralaması hakkında bilgiler için Razor Pages.

Bazı durumlarda, belirsiz yollarla bir HTTP 500 hatası döndürülür. `AmbiguousMatchException`hangi uç noktaların olduğunu görmek için [günlüğe kaydetmeyi](xref:fundamentals/logging/index) kullanın.

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>Yönlendirme şablonlarında belirteç değiştirme [denetleyici], [eylem], [alan]

Daha kolay olması için, öznitelik rotaları, aşağıdakilerden birine bir belirteç ekleyerek ayrılmış yol parametrelerine yönelik belirteç değişimini destekler:

* Köşeli ayraç: `[]`
* Küme ayraçları: `{}`

`[action]`, `[area]`ve `[controller]` belirteçleri, yolun tanımlandığı eylemden eylem adı, alan adı ve denetleyici adı değerleriyle değiştirilmiştir:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet)]

Önceki kodda:

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet10)]

  * Eşleşen `/Products0/List`

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet11)]

  * Eşleşen `/Products0/Edit/{id}`

Belirteç değişikliği, öznitelik yollarının oluşturulması için son adım olarak gerçekleşir. Yukarıdaki örnek aşağıdaki kodla aynı şekilde davranır:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet20)]

[!INCLUDE[](~/includes/MTcomments.md)]

Öznitelik rotaları de devralma ile birleştirilebilir. Bu, belirteç değiştirme ile güçlü bir şekilde birleştirilir. Belirteç değişikliği, öznitelik rotaları tarafından tanımlanan yol adları için de geçerlidir.
`[Route("[controller]/[action]", Name="[controller]_[action]")]`her eylem için benzersiz bir yol adı üretir:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet5)]

Belirteç değişikliği, öznitelik rotaları tarafından tanımlanan yol adları için de geçerlidir.
`[Route("[controller]/[action]", Name="[controller]_[action]")]`
Her eylem için benzersiz bir yol adı üretir.

Sabit belirteç değiştirme sınırlayıcısı `[` veya `]`eşleştirmek için, karakteri (`[[` veya `]]`) tekrarlayarak kaçış.

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a>Belirteç değişimini özelleştirmek için bir parametre transformatörü kullanın

Belirteç değiştirme, bir parametre transformatörü kullanılarak özelleştirilebilir. Bir parametre transformatörü <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> uygular ve parametrelerin değerini dönüştürür. Örneğin, özel bir `SlugifyParameterTransformer` parametresi transformatörü `SubscriptionManagement` Route değerini `subscription-management`olarak değiştirir:

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet2)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention>, şu şekilde bir uygulama modeli kuralıdır:

* Bir uygulamadaki tüm öznitelik yollarına bir parametre transformatörü uygular.
* Öznitelik yol belirteci değerlerini değiştirildikleri gibi özelleştirir.

[!code-csharp[](routing/samples/3.x/main/Controllers/SubscriptionManagementController.cs?name=snippet)]

Yukarıdaki `ListAll` yöntemi `/subscription-management/list-all`eşleşir.

`RouteTokenTransformerConvention`, `ConfigureServices`bir seçenek olarak kaydedilir.

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet)]

Bilgi tanımı için bkz. [Mdıdn Web docs](https://developer.mozilla.org/docs/Glossary/Slug) for the başlık.

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-attribute-routes"></a>Birden çok öznitelik yolu

Öznitelik yönlendirme, aynı eyleme ulaşan birden çok yolun tanımlanmasını destekler. Bunun en yaygın kullanımları, aşağıdaki örnekte gösterildiği gibi varsayılan geleneksel yolun davranışını taklit etmek olur:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6x)]

Denetleyiciye birden çok yol özniteliği koymak, her birinin eylem yöntemlerinde yol özniteliklerinin her biri ile birleştiribileceği anlamına gelir:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6)]

Tüm [http fiili](#verb) yol kısıtlamaları `IActionConstraint`uygular.

<xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> uygulayan birden çok yol özniteliği bir eyleme yerleştirildiğinde:

* Her eylem kısıtlaması, denetleyiciye uygulanan rota şablonuyla birlikte birleşir.

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet7)]

Eylemlerde birden çok yolun kullanılması yararlı ve güçlü görünebilir, uygulamanızın URL alanını temel ve iyi tanımlanmış tutmanız daha iyidir. **Yalnızca** gerektiğinde, var olan istemcileri desteklemek için, eylemlerde birden çok yol kullanın.

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>Öznitelik rotası isteğe bağlı parametreler, varsayılan değerler ve kısıtlamalar belirtme

Öznitelik yolları, isteğe bağlı parametreleri, varsayılan değerleri ve kısıtlamaları belirtmek için geleneksel yollarla aynı satır içi sözdizimini destekler.

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet8&highlight=3)]

Yukarıdaki kodda `[HttpPost("product/{id:int}")]` bir yol kısıtlaması uygular. `ProductsController.ShowProduct` eylemi yalnızca `/product/3`gibi URL yollarıyla eşleştirilir. Yol şablonu bölümü, bu segmenti yalnızca tamsayılarla kısıtlar `{id:int}`.

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

Yol şablonu sözdiziminin ayrıntılı açıklaması için bkz. [route Template Reference](xref:fundamentals/routing#route-template-reference) .

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>Iroutetemplateprovider kullanarak özel yol öznitelikleri

Tüm [Rota öznitelikleri](#rt) <xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider>uygular. ASP.NET Core çalışma zamanı:

* Uygulama başladığında denetleyici sınıflarında ve eylem yöntemlerinde öznitelikler arar.
* İlk yol kümesini oluşturmak için `IRouteTemplateProvider` uygulayan öznitelikleri kullanır.

Özel yol özniteliklerini tanımlamak için `IRouteTemplateProvider` uygulayın. Her `IRouteTemplateProvider`, özel bir yol şablonu, sırası ve adı ile tek bir yol tanımlamanızı sağlar:

[!code-csharp[](routing/samples/3.x/main/Controllers/MyTestApiController.cs?name=snippet&highlight=1-10)]

Önceki `Get` yöntemi `Order = 2, Template = api/MyTestApi`döndürür.

<a name="routing-app-model-ref-label"></a>

### <a name="use-application-model-to-customize-attribute-routes"></a>Öznitelik yollarını özelleştirmek için uygulama modelini kullanın

Uygulama modeli:

* , Başlangıçta oluşturulan bir nesne modelidir.
* Bir uygulamadaki eylemleri yönlendirmek ve yürütmek için ASP.NET Core tarafından kullanılan tüm meta verileri içerir.

Uygulama modeli, yol özniteliklerinden toplanan tüm verileri içerir. Yol özniteliklerinden alınan veriler `IRouteTemplateProvider` uygulama tarafından sağlanır. Adlandır

* , Yönlendirmenin nasıl davranacağını özelleştirmek için uygulama modelini değiştirmek üzere yazılabilir.
* Uygulama başlangıcında okundu.

Bu bölümde, uygulama modeli kullanılarak yönlendirmeyi özelleştirmenin temel bir örneği gösterilmektedir. Aşağıdaki kod, rotaları projenin klasör yapısıyla kabaca bir şekilde oluşturur.

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet)]

Aşağıdaki kod, `namespace` kuralının, öznitelik yönlendirilen denetleyicilere uygulanmasını engeller:

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet2)]

Örneğin, aşağıdaki denetleyici `NamespaceRoutingConvention`kullanmaz:

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/ManagersController.cs?name=snippet&highlight=1)]

`NamespaceRoutingConvention.Apply` yöntemi:

* Denetleyici öznitelik yönlendirmemişse hiçbir şey yapmaz.
* `namespace`temel `namespace` kaldırılacak şekilde denetleyiciler şablonunu ayarlar.

`NamespaceRoutingConvention` `Startup.ConfigureServices`uygulanabilir:

[!code-csharp[](routing/samples/3.x/nsrc/Startup.cs?name=snippet&highlight=1,14-18)]

Örneğin, aşağıdaki denetleyiciyi göz önünde bulundurun:

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/UsersController.cs)]

Önceki kodda:

* Temel `namespace` `My.Application`.
* Önceki denetleyicinin tam adı `My.Application.Admin.Controllers.UsersController`.
* `NamespaceRoutingConvention`, denetleyiciler şablonunu `Admin/Controllers/Users/[action]/{id?`olarak ayarlar.

`NamespaceRoutingConvention` bir denetleyiciye bir öznitelik olarak da uygulanabilir:

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/TestController.cs?name=snippet&highlight=1)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>Karma yönlendirme: öznitelik yönlendirme vs geleneksel yönlendirme

ASP.NET Core uygulamalar geleneksel yönlendirme ve öznitelik yönlendirmenin kullanımını karıştırabilir. Tarayıcılar için HTML sayfalarına hizmet veren denetleyiciler için geleneksel yollar ve REST API 'Lerine hizmet veren denetleyiciler için öznitelik yönlendirme kullanılması normaldir.

Eylemler genel olarak Dolaştırılan veya Attribute olarak yönlendirilir. Bir yolu denetleyiciye koymak veya eylemi, BT özniteliği yönlendirilmesini sağlar. Öznitelik yollarını tanımlayan eylemlere geleneksel yollar üzerinden ulaşılamıyor ve bunun tersi de geçerlidir. Denetleyicideki **herhangi bir** rota özniteliği, denetleyici özniteliğindeki **Tüm** eylemlerin yönlendirilmesini sağlar.

Öznitelik yönlendirme ve geleneksel yönlendirme aynı yönlendirme altyapısını kullanır.

<a name="routing-url-gen-ref-label"></a>
<a name="ambient"></a>

## <a name="url-generation-and-ambient-values"></a>URL oluşturma ve ortam değerleri

Uygulamalar, eylemlere URL bağlantıları oluşturmak için yönlendirme URL 'SI oluşturma özelliklerini kullanabilir. URL 'Leri oluşturmak, kod daha sağlam ve sürdürülebilir hale getirmek için sorunsuz kodlama URL 'Lerini ortadan kaldırır. Bu bölüm, MVC tarafından sunulan URL oluşturma özelliklerine odaklanır ve yalnızca URL oluşturma özelliğinin nasıl çalıştığına ilişkin temel bilgileri kapsar. URL oluşturma hakkında ayrıntılı bir açıklama için bkz. [yönlendirme](xref:fundamentals/routing) .

<xref:Microsoft.AspNetCore.Mvc.IUrlHelper> arabirimi, URL oluşturma için MVC ve yönlendirme arasındaki altyapının temel öğesidir. `IUrlHelper` örneği, denetleyiciler, görünümler ve görünüm bileşenlerinde `Url` özelliği aracılığıyla kullanılabilir.

Aşağıdaki örnekte `IUrlHelper` arabirimi, başka bir eyleme yönelik bir URL oluşturmak için `Controller.Url` özelliği aracılığıyla kullanılır.

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

Uygulama varsayılan geleneksel yolu kullanıyorsa, `url` değişkenin değeri `/UrlGeneration/Destination`URL yol dizesidir. Bu URL yolu, yönlendirme tarafından birleştirerek oluşturulur:

* Geçerli istekten, **ortam değerleri**olarak adlandırılan rota değerleri.
* `Url.Action` geçirilen değerler ve bu değerleri yol şablonuna değiştirme:

``` text
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

Yol şablonundaki her bir rota parametresinin değeri, değerler ve ortam değerleri ile eşleşen adlara sahip olacak şekilde değiştirilir. Bir değere sahip olmayan bir rota parametresi şunları yapabilir:

* Bir varsayılan değer varsa, bu değeri kullanın.
* İsteğe bağlı ise atlanır. Örneğin, yol şablonundan `id` `{controller}/{action}/{id?}`.

Herhangi bir gerekli yol parametresi karşılık gelen bir değere sahip değilse, URL oluşturma başarısız olur. Bir yol için URL oluşturma başarısız olursa, tüm yollar Denenene veya bir eşleşme bulunana kadar sonraki yol denenir.

Önceki `Url.Action` örneği [geleneksel yönlendirmeyi](#cr)varsayar. URL oluşturma, [öznitelik yönlendirme](#ar)ile benzer şekilde çalışır, ancak kavramlar farklıdır. Geleneksel yönlendirme ile:

* Rota değerleri bir şablonu genişletmek için kullanılır.
* `controller` ve `action` için yol değerleri genellikle bu şablonda görüntülenir. Bu, yönlendirme ile eşleşen URL 'Ler bir kurala bağlı olduğundan, bu işe yarar.

Aşağıdaki örnek öznitelik yönlendirme kullanır:

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationAttrController.cs?name=snippet_1)]

Yukarıdaki koddaki `Source` eylemi `custom/url/to/destination`oluşturur.

<xref:Microsoft.AspNetCore.Routing.LinkGenerator>, `IUrlHelper`alternatif olarak ASP.NET Core 3,0 eklenmiştir. `LinkGenerator` benzer ancak daha esnek işlevler sunar. `IUrlHelper` yöntemlerin her biri, `LinkGenerator` ilgili bir yöntem ailesine da sahiptir.

### <a name="generating-urls-by-action-name"></a>Eylem adına göre URL 'Leri oluşturma

[URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), [Linkgenerator. GetPathByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*)ve tüm ilgili aşırı yüklemeler, bir denetleyici adı ve eylem adı belirtilerek hedef uç noktası oluşturmak için tasarlanmıştır.

`Url.Action`kullanırken, `controller` ve `action` için geçerli yol değerleri çalışma zamanı tarafından sağlanır:

* `controller` ve `action` değeri hem [ortam değerlerinin](#ambient) hem de değerlerinin bir parçasıdır. `Url.Action` yöntemi her zaman `action` ve `controller` geçerli değerlerini kullanır ve geçerli eyleme yönlendiren bir URL yolu oluşturur.

Yönlendirme, bir URL oluştururken sağlanmayan bilgileri doldurmanızı sağlamak için çevresel değerlerde değerleri kullanmaya çalışır. Çevresel değerler `{ a = Alice, b = Bob, c = Carol, d = David }``{a}/{b}/{c}/{d}` gibi bir yol düşünün:

* Yönlendirmenin ek değer olmadan bir URL oluşturmak için yeterli bilgi vardır.
* Tüm rota parametrelerinin bir değeri olduğundan, yönlendirmeye yeterli bilgi vardır.

Değer `{ d = Donovan }` eklenirse:

* `{ d = David }` değeri yoksayıldı.
* Oluşturulan URL yolu `Alice/Bob/Carol/Donovan`.

**Uyarı**: URL yolları hiyerarşiktir. Yukarıdaki örnekte, değer `{ c = Cheryl }` eklenirse:

* Değerlerin her ikisi de `{ c = Carol, d = David }` yok sayılır.
* `d` için artık bir değer yoktur ve URL oluşturma başarısız olur.
* `c` ve `d` istenen değerleri bir URL oluşturmak için belirtilmelidir.  

`{controller}/{action}/{id?}`varsayılan yol ile bu sorunu daha fazla beklemeniz gerekebilir. `Url.Action` her zaman açık bir `controller` ve `action` değerini belirttiğinden bu sorun pratikte bu soruna neden olur.

URL 'nin birkaç aşırı yüklemesi [. işlem](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) , `controller` ve `action`dışındaki rota parametreleri için değerler sağlamak üzere bir yol değerleri nesnesi alır. Yol değerleri nesnesi genellikle `id`ile kullanılır. Örneğin, `Url.Action("Buy", "Products", new { id = 17 })`. Yol değerleri nesnesi:

* Kural gereği genellikle anonim türdeki bir nesnedir.
* `IDictionary<>` veya [poco](https://wikipedia.org/wiki/Plain_old_CLR_object)olabilir).

Yol parametreleriyle eşleşmeyen ek rota değerleri sorgu dizesine konur.

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet)]

Yukarıdaki kod `/Products/Buy/17?color=red`oluşturur.

Aşağıdaki kod mutlak URL oluşturur:

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet2)]

Mutlak URL oluşturmak için, aşağıdakilerden birini kullanın:

* `protocol`kabul eden aşırı yükleme. Örneğin, yukarıdaki kod.
* Varsayılan olarak mutlak URI 'Ler üreten [Linkgenerator. GetUriByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*).

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generate-urls-by-route"></a>Yola göre URL oluşturma

Yukarıdaki kod, denetleyiciyi ve eylem adını geçirerek bir URL oluşturmayı göstermiştir. `IUrlHelper` Ayrıca, [URL. RouteUrl](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*) ailesi yöntemlerin de sağlar. Bu yöntemler [URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*)ile benzerdir, ancak `action` ve `controller` geçerli değerlerini rota değerlerine kopyalamaz. `Url.RouteUrl`en yaygın kullanımı:

* URL oluşturmak için bir yol adı belirtir.
* Genellikle bir denetleyici veya eylem adı belirtmez.

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGeneration2Controller.cs?name=snippet_1)]

Aşağıdaki Razor dosyası `Destination_Route`bir HTML bağlantısı oluşturur:

[!code-cshtml[](routing/samples/3.x/main/Views/Shared/MyLink.cshtml)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generate-urls-in-html-and-razor"></a>HTML ve Razor 'de URL oluşturma

<xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper>, `<form>` ve `<a>` öğeleri oluşturmak için [HTML. BeginForm](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*) ve [html. ActionLink](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*) <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper> yöntemler sağlar. Bu yöntemler bir URL oluşturmak için [URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) yöntemini kullanır ve benzer bağımsız değişkenleri kabul ederler. `HtmlHelper` için `Url.RouteUrl` compan, benzer işlevlere sahip `Html.BeginRouteForm` ve `Html.RouteLink`.

Taghelmakalar, `form` TagHelper ve `<a>` TagHelper aracılığıyla URL 'Ler oluşturur. Bunların her ikisi de kendi uygulamaları için `IUrlHelper`. Daha fazla bilgi için bkz. [Formlardaki etiket yardımcıları](xref:mvc/views/working-with-forms) .

Görünümler içinde `IUrlHelper`, yukarıdaki herhangi bir geçici URL nesli için `Url` özelliği aracılığıyla kullanılabilir.

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="url-generation-in-action-results"></a>Eylem sonuçlarında URL oluşturma

Yukarıdaki örneklerde `IUrlHelper` bir denetleyicide kullanılması gösterildi. Bir denetleyicideki en yaygın kullanım, bir eylem sonucunun parçası olarak bir URL oluşturmak olur.

<xref:Microsoft.AspNetCore.Mvc.ControllerBase> ve <xref:Microsoft.AspNetCore.Mvc.Controller> Taban sınıfları, başka bir eyleme başvuruda bulunan eylem sonuçları için kolay yöntemler sağlar. Kullanıcı girişini kabul ettikten sonra, yaygın olarak kullanılan bir kullanım yeniden yönlendirilmelidir:

[!code-csharp[](routing/samples/3.x/main/Controllers/CustomerController.cs?name=snippet)]

Eylem sonuçları <xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction*> ve <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> gibi Fabrika yöntemleri `IUrlHelper`yöntemler için de benzer bir desenler izler.

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>Adanmış geleneksel yollar için özel durum

[Geleneksel yönlendirme](#cr) , [adanmış geleneksel yol](#dcr)olarak adlandırılan özel bir yol tanımı türünü kullanabilir. Aşağıdaki örnekte, `blog` adlı yol adanmış bir geleneksel yoldur:

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

Önceki yol tanımlarını kullanarak, `Url.Action("Index", "Home")` `/` URL yolunu `default` yolunu kullanarak oluşturur, ancak neden? Yol değerlerini tahmin edebilirsiniz `{ controller = Home, action = Index }` `blog`kullanarak URL oluşturmak için yeterli olacaktır ve sonuç `/blog?action=Index&controller=Home`.

[Adanmış geleneksel yollar](#dcr) , karşılık gelen bir rota parametresi olmayan varsayılan değerlerin özel bir davranışına dayanır ve bu da yol, URL oluşturma ile çok fazla [dodan](xref:fundamentals/routing#greedy) yararlanın. Bu durumda, varsayılan değerler `{ controller = Blog, action = Article }`ve ne `controller` ne de `action` yol parametresi olarak görünmez. Yönlendirme URL oluşturma işlemi gerçekleştirdiğinde, belirtilen değerler varsayılan değerlerle eşleşmelidir. Değerler `{ controller = Home, action = Index }` `{ controller = Blog, action = Article }`eşleşmediğinden `blog` kullanarak URL oluşturma başarısız olur. Ardından yönlendirme `default`denemeye geri döner ve başarılı olur.

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>Alanlar

[Bölgeler](xref:mvc/controllers/areas) , ilgili işlevselliği ayrı olarak bir grup içinde düzenlemek için kullanılan bir MVC özelliğidir:

* Denetleyici eylemleri için yönlendirme ad alanı.
* Görünümler için klasör yapısı.

Alanların kullanılması, farklı alanlara sahip oldukları sürece bir uygulamanın aynı ada sahip birden çok denetleyicisi olmasına olanak sağlar. Alanların kullanılması, başka bir yol parametresi ekleyerek yönlendirme amacına yönelik bir hiyerarşi oluşturur, `controller` ve `action``area`. Bu bölümde yönlendirmenin alanlarla nasıl etkileşim kurduğu açıklanmaktadır. Alanların görünümlerle nasıl kullanıldığı hakkında ayrıntılar için bkz. [alanlara](xref:mvc/controllers/areas) bakın.

Aşağıdaki örnek, MVC 'yi varsayılan geleneksel yolu kullanacak şekilde yapılandırır `Blog`adlı bir `area` için `area` yolu.

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

Yukarıdaki kodda, `"blog_route"`oluşturmak için <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> çağırılır. `"Blog"`ikinci parametresi, alan adıdır.

`/Manage/Users/AddUser`gibi bir URL yolu eşleştirilirken `"blog_route"` yolu `{ area = Blog, controller = Users, action = AddUser }`yol değerlerini oluşturur. `area` yol değeri, `area`için varsayılan bir değer tarafından üretilir. `MapAreaControllerRoute` tarafından oluşturulan yol, aşağıdaki ile eşdeğerdir:

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup2.cs?name=snippet2)]

`MapAreaControllerRoute`, `area` için hem varsayılan değer hem de kısıtlama (Bu durumda `Blog`) kullanarak bir yol oluşturur. Varsayılan değer, yolun her zaman `{ area = Blog, ... }`üretmesini sağlar, kısıtlama, URL oluşturma için `{ area = Blog, ... }` değer gerektirir.

Geleneksel yönlendirme sıra bağımlıdır. Genel olarak, alanlar içeren rotalar daha önce bir alan olmadan rotalardan daha belirgin olduklarından yerleştirilmelidir.

Önceki örneği kullanarak, yol değerleri `{ area = Blog, controller = Users, action = AddUser }` aşağıdaki eylemle eşleşir:

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[[Area]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) özniteliği, bir alanın parçası olarak denetleyiciyi belirtir. Bu denetleyici `Blog` alanıdır. `[Area]` özniteliği olmayan denetleyiciler hiçbir alanın üyesi değildir ve `area` yol değeri yönlendirme tarafından **sağlandığında eşleşmez.** Aşağıdaki örnekte, yalnızca listelenen ilk denetleyici `{ area = Blog, controller = Users, action = AddUser }`rota değerleriyle eşleştirebilir.

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

Her denetleyicinin ad alanı, tamamlanma açısından burada gösterilmiştir. Yukarıdaki denetleyiciler aynı ad alanını kullanıyorsa, bir derleyici hatası oluşturulur. Sınıf ad alanlarının MVC 'nin yönlendirme üzerinde hiçbir etkisi yoktur.

İlk iki denetleyici alanların üyeleridir ve yalnızca ilgili alan adı `area` rota değeri tarafından sağlandığında eşleşir. Üçüncü denetleyici hiçbir alanın üyesi değildir ve yalnızca Yönlendirme tarafından `area` hiçbir değer sağlanmıyorsa eşleşemez.

<a name="aa"></a>

*Değer olmadan*eşleşme açısından, `area` değerinin yokluğu, `area` değeri null ya da boş dize olarak aynıdır.

Bir alan içinde bir eylem yürütürken, `area` için rota değeri, yönlendirme için, URL oluşturma için kullanılacak [çevresel bir değer](#ambient) olarak kullanılabilir. Bu, varsayılan olarak, aşağıdaki örnekte gösterildiği gibi, URL oluşturma için *yapışkan* olarak hareket ettiği anlamına gelir.

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup3.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

Aşağıdaki kod `/Zebra/Users/AddUser`bir URL oluşturur:

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/HomeController.cs?name=snippet)]

<a name="action"></a>

## <a name="action-definition"></a>Eylem tanımı

Bir denetleyicide, [Nonactıon](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute) özniteliğine sahip olanlar hariç genel yöntemler eylemlerdir.

## <a name="sample-code"></a>Örnek kod

 * [Mydisplayrouteınfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) yöntemi [örnek karşıdan yüklemeye](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) dahildir ve yönlendirme bilgilerini görüntülemek için kullanılır.
* [Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

[!INCLUDE[](~/includes/dbg-route.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core MVC, gelen isteklerin URL 'Leriyle eşleştirmek ve bunları eylemlerle eşlemek için yönlendirme [Ara yazılımını](xref:fundamentals/middleware/index) kullanır. Yollar başlangıç kodunda veya özniteliklerde tanımlanmıştır. Yollar URL yollarının eylemlerle nasıl eşleştirileceği açıklanır. Yollar, yanıt olarak gönderilen URL 'Leri (bağlantılar için) oluşturmak için de kullanılır.

Eylemler genel olarak Dolaştırılan veya Attribute olarak yönlendirilir. Bir yolu denetleyiciye koymak veya eylemi, BT özniteliği yönlendirilmesini sağlar. Daha fazla bilgi için bkz. [karma yönlendirme](#routing-mixed-ref-label) .

Bu belge, MVC ve yönlendirme arasındaki etkileşimleri ve tipik MVC uygulamalarının yönlendirme özelliklerini nasıl kullandığını açıklar. Gelişmiş yönlendirme hakkında ayrıntılar için bkz. [yönlendirme](xref:fundamentals/routing) .

## <a name="setting-up-routing-middleware"></a>Yönlendirme ara yazılımını ayarlama

*Yapılandırma* yönteminde şuna benzer bir kod görebilirsiniz:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`UseMvc`çağrısının içinde `MapRoute`, `default` rota olarak başvurabileceğiniz tek bir yol oluşturmak için kullanılır. Çoğu MVC uygulaması, `default` yoluna benzer bir şablon içeren bir yol kullanır.

Yol şablonu `"{controller=Home}/{action=Index}/{id?}"` `/Products/Details/5` gibi bir URL yoluyla eşleştirebilir ve yolu simgeleştirerek `{ controller = Products, action = Details, id = 5 }` yol değerlerini ayıklar. MVC, `ProductsController` adlı bir denetleyiciyi bulmaya çalışır ve `Details`eylemi çalıştırır:

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

Bu örnekte model bağlamanın, bu eylemi çağırırken `id` parametresini `5` olarak ayarlamak için `id = 5` değerini kullanabileceğini unutmayın. Daha fazla ayrıntı için [model bağlamaya](../models/model-binding.md) bakın.

`default` yolunu kullanma:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

Yol şablonu:

* `{controller=Home}` varsayılan olarak `Home` tanımlar `controller`

* `{action=Index}` varsayılan olarak `Index` tanımlar `action`

* `{id?}` `id` isteğe bağlı olarak tanımlar

Bir eşleşme için URL yolunda varsayılan ve isteğe bağlı yol parametrelerinin mevcut olması gerekmez. Yol şablonu sözdiziminin ayrıntılı açıklaması için bkz. [route Template Reference](xref:fundamentals/routing#route-template-reference) .

`"{controller=Home}/{action=Index}/{id?}"`, `/` URL yolu ile eşleştirebilir ve `{ controller = Home, action = Index }`yol değerlerini üretecektir. `controller` ve `action` değerleri varsayılan değerleri kullanır `id`, URL yolunda karşılık gelen bir kesim olmadığından, bu değer oluşturmaz. MVC bu yol değerlerini kullanarak `HomeController` ve `Index` eylemini seçer:

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

Bu denetleyici tanımı ve yönlendirme şablonunu kullanarak, aşağıdaki URL yollarından herhangi biri için `HomeController.Index` eylemi yürütülür:

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

Kolaylık yöntemi `UseMvcWithDefaultRoute`:

```csharp
app.UseMvcWithDefaultRoute();
```

Değiştirmek için kullanılabilir:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`UseMvc` ve `UseMvcWithDefaultRoute` ara yazılım ardışık düzenine bir `RouterMiddleware` örneği ekleyin. MVC, doğrudan ara yazılım ile etkileşime girmez ve istekleri işlemek için yönlendirmeyi kullanır. MVC bir `MvcRouteHandler`örneği aracılığıyla yollara bağlanır. `UseMvc` içindeki kod aşağıdaki gibidir:

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

`UseMvc` doğrudan hiçbir yol tanımlamıyor, yol koleksiyonuna `attribute` yolu için bir yer tutucu ekler. Aşırı yükleme `UseMvc(Action<IRouteBuilder>)` kendi rotalarınızı eklemenize ve öznitelik yönlendirmeyi de desteklemenizi sağlar.  `UseMvc` ve tüm çeşitlemeleri, `UseMvc`yapılandırma şeklinden bağımsız olarak her zaman kullanılabilir. `UseMvcWithDefaultRoute` varsayılan bir yol tanımlar ve öznitelik yönlendirmeyi destekler. [Öznitelik yönlendirme](#attribute-routing-ref-label) bölümü öznitelik yönlendirme hakkında daha fazla ayrıntı içerir.

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a>Geleneksel yönlendirme

`default` yolu:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

Yukarıdaki kod geleneksel yönlendirmeye bir örnektir. Bu stil, URL yolları için bir *kural* oluşturduğundan geleneksel yönlendirme olarak adlandırılır:

* İlk yol segmenti, denetleyicinin adıyla eşlenir.
* İkincisi eylem adıyla eşlenir.
* Üçüncü segment, isteğe bağlı bir `id`için kullanılır. `id` bir model varlığına eşlenir.

Bu `default` yolunu kullanarak, URL yolu `/Products/List` `ProductsController.List` eylemine eşlenir ve `/Blog/Article/17` eşlenir `BlogController.Article`. Bu eşleme **yalnızca** denetleyiciye ve eylem adlarına dayalıdır ve ad alanları, kaynak dosya konumları veya yöntem parametrelerine göre değildir.

> [!TIP]
> Varsayılan yol ile geleneksel yönlendirmeyi kullanmak, tanımladığınız her eylem için yeni bir URL düzeniyle karşılaşmanıza gerek kalmadan uygulamayı hızlı bir şekilde oluşturmanıza olanak tanır. CRUD stilinde eylemlere sahip bir uygulama için denetleyicilerinizdeki URL 'Lerin tutarlılığı, kodunuzun basitleştirilmesine ve Kullanıcı arabiriminizi daha öngörülebilir hale getirmenize yardımcı olabilir.

> [!WARNING]
> `id`, yol şablonu tarafından isteğe bağlı olarak tanımlanır ve bu, eylemlerinizin URL 'nin bir parçası olarak sağlanmadan yürütebileceği anlamına gelir. Genellikle, URL 'den `id` atlandığında ne olur, bu durum model bağlama tarafından `0` olarak ayarlanır ve sonuç olarak veritabanında eşleşen `id == 0`hiçbir varlık bulunamacaktır. Öznitelik yönlendirme, bazı eylemler için gereken KIMLIĞI, diğerleri için değil, daha ayrıntılı bir denetim sağlayabilir. Kurala göre belgeler, doğru kullanımlarda görünebilecekleri `id` gibi isteğe bağlı parametreleri de içerecektir.

## <a name="multiple-routes"></a>Birden çok yol

`MapRoute`daha fazla çağrı ekleyerek `UseMvc` içine birden çok yol ekleyebilirsiniz. Bunun yapılması, birden çok kural tanımlamanızı veya belirli bir eyleme adanmış geleneksel yollar eklemenizi sağlar; örneğin:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Buradaki `blog` yol, geleneksel bir *geleneksel yoldur*, yani geleneksel yönlendirme sistemini kullanır, ancak belirli bir eyleme ayrılmıştır. `controller` ve `action` yol şablonunda parametre olarak görünmadığından, bu yol yalnızca varsayılan değerlere sahip olabilir ve bu nedenle bu yol her zaman eylem `BlogController.Article`eşlenir.

Rota koleksiyonundaki yollar sıralanır ve eklendikleri sırada işlenir. Bu örnekte, `blog` yolu `default` rotadan önce denenecek.

> [!NOTE]
> *Adanmış geleneksel yollar* genellıkle, URL yolunun kalan kısmını yakalamak için `{*article}` gibi **catch-all** Route parametrelerini kullanır. Bu, ' çok Greedy ' yolunu diğer yollarla eşleştirirken hedeflediğiniz URL 'Lerle eşleşen bir yol haline getirir. Bunu çözümlemek için ' Greedy ' yollarını daha sonra yol tablosuna koyun.

### <a name="fallback"></a>Dönüş

İstek işlemenin bir parçası olarak, MVC, uygulamanızdaki bir denetleyiciyi ve eylemi bulmak için yol değerlerinin kullanılabileceğini doğrular. Rota değerleri bir eylemle eşleşmezse, yol eşleşme olarak kabul edilmez ve sonraki rota denenir. Buna *geri dönüş*denir ve geleneksel yolların çakıştığı durumları basitleştirmek için tasarlanmıştır.

### <a name="disambiguating-actions"></a>Kesinleştirme eylemleri

İki eylem yönlendirme aracılığıyla eşleşiyorsa, MVC ' en iyi ' adayı seçmek için bir özel durum oluşturması veya bir özel durum oluşturmak için, MVC 'nin belirsizliğini Örneğin:

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

Bu denetleyici, URL yolu `/Products/Edit/17` ile eşleşen iki eylemi tanımlar ve verileri `{ controller = Products, action = Edit, id = 17 }`yönlendirir. Bu, `Edit(int)` bir ürünü düzenlemek üzere bir form gösterdiği ve `Edit(int, Product)` postalanan formu işleyen MVC denetleyicileri için tipik bir modeldir. Bunu yapmak için bu olası MVC, istek bir HTTP `POST` olduğunda `Edit(int, Product)` ve HTTP fiili başka bir şey olduğunda `Edit(int)` ' ı seçmeniz gerekir.

`HttpPostAttribute` (`[HttpPost]`), yalnızca HTTP fiili `POST`olduğunda eylemin seçili olmasını sağlayacak `IActionConstraint` uygulamasıdır. `IActionConstraint` olması, `Edit(int, Product)` ' daha iyi bir eşleşme `Edit(int)`, bu nedenle önce `Edit(int, Product)` denenmesini sağlar.

Yalnızca özelleştirilmiş senaryolarda özel `IActionConstraint` uygulamalar yazmanız gerekir, ancak diğer HTTP fiilleri için `HttpPostAttribute` benzer öznitelikler gibi özniteliklerin rol olduğunu anlamak önemlidir. Geleneksel yönlendirmesinde, eylemler bir `show form -> submit form` iş akışının parçası olduğunda aynı eylem adını kullanmak yaygındır. Bu düzenin rahatlığı, [ıactionconstraint 'ı anlama](#understanding-iactionconstraint) bölümünde daha sonra görünür hale gelir.

Birden çok yol eşleşirse ve MVC ' en iyi ' yolu bulamazsa, bir `AmbiguousActionException`oluşturur.

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a>Yol adları

Aşağıdaki örneklerde `"blog"` ve `"default"` dizeler yol adlarıdır:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Yol adları, URL oluşturma için adlandırılmış yolun kullanılabilmesi için yola mantıksal bir ad verir. Bu, yolların sıralaması URL oluşturma karmaşık hale geldiğinde URL oluşturmayı büyük ölçüde basitleştirir. Yol adları, uygulama genelinde benzersiz olmalıdır.

Yol adları, isteklerin URL 'SI ile eşleşmesini veya işlenmesini etkilemez; Bunlar yalnızca URL oluşturma için kullanılır. [Yönlendirme](xref:fundamentals/routing) , MVC 'ye özgü yardımcılardaki URL oluşturma da dahil olmak üzere URL oluşturma hakkında daha ayrıntılı bilgiler içerir.

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a>Öznitelik yönlendirme

Öznitelik yönlendirme eylemleri doğrudan yönlendirme şablonlarına eşlemek için bir öznitelik kümesi kullanır. Aşağıdaki örnekte, `app.UseMvc();` `Configure` yönteminde kullanılır ve hiçbir yol geçirilir. `HomeController`, varsayılan yol `{controller=Home}/{action=Index}/{id?}` eşleşeceğinize benzer bir URL kümesiyle eşleşir:

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

`HomeController.Index()` eylem, `/`, `/Home`veya `/Home/Index`URL yollarından herhangi biri için yürütülür.

> [!NOTE]
> Bu örnek, öznitelik yönlendirme ve geleneksel yönlendirme arasında bir temel programlama farkı vurgulamaktadır. Öznitelik yönlendirme, bir yol belirtmek için daha fazla giriş gerektirir; geleneksel varsayılan yol, yönlendirmeleri daha succinctly işler. Ancak, öznitelik yönlendirme (ve gerektirir) her eylem için hangi rota şablonlarının uygulanacağını kesin olarak denetler.

Öznitelik yönlendirme ile, denetleyici adı ve eylem adları, **herhangi** bir eylem seçildiği hiçbir rol oynar. Bu örnek, önceki örnekle aynı URL 'Lerle eşleştirecektir.

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> Yukarıdaki yol şablonları `action`, `area`ve `controller`için yol parametreleri tanımlamaz. Aslında, öznitelik rotalarında bu yol parametrelerine izin verilmez. Yol şablonu bir eylemle zaten ilişkili olduğundan, URL 'den eylem adını ayrıştırmak mantıklı değildir.

## <a name="attribute-routing-with-httpverb-attributes"></a>Http [fiil] öznitelikleriyle öznitelik yönlendirme

Öznitelik yönlendirme Ayrıca, `HttpPostAttribute`gibi `Http[Verb]` özniteliklerini de kullanabilir. Bu özniteliklerin hepsi bir yol şablonunu kabul edebilir. Bu örnekte, aynı rota şablonuyla eşleşen iki eylem gösterilmektedir:

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

`/products` gibi bir URL yolu için, HTTP fiili `GET` olduğunda `ProductsApi.ListProducts` eylemi yürütülür ve HTTP fiili `POST`olduğunda `ProductsApi.CreateProduct` yürütülür. Öznitelik yönlendirme öncelikle URL ile yol öznitelikleri tarafından tanımlanan yol şablonları kümesine göre eşleşir. Bir rota şablonu eşleştiğinde, hangi eylemlerin yürütüleceğini belirleyen `IActionConstraint` kısıtlamalar uygulanır.

> [!TIP]
> Bir REST API oluştururken, eylem tüm HTTP yöntemlerini kabul edecek şekilde bir eylem yönteminde `[Route(...)]` kullanmak isteyeceksiniz. API 'nizin neleri desteklediği hakkında kesin olması için daha özel `Http*Verb*Attributes` kullanmak daha iyidir. REST API 'lerinin istemcileri, hangi yolların ve HTTP fiillerinin belirli mantıksal işlemlere eşlendiğini bilmelidir.

Bir öznitelik yolu belirli bir eyleme uyguladığı için, yol şablonu tanımının bir parçası olarak gerekli parametreleri yapmak kolaydır. Bu örnekte, URL yolunun bir parçası olarak `id` gereklidir.

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

`ProductsApi.GetProduct(int)` eylemi, `/products/3` gibi bir URL yolu için yürütülür, ancak `/products`gibi bir URL yolu için değil. Yol şablonlarının ve ilgili seçeneklerin tam açıklaması için bkz. [yönlendirme](xref:fundamentals/routing) .

## <a name="route-name"></a>Yol adı

Aşağıdaki kod `Products_List`*yol adını* tanımlar:

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

Yol adları, belirli bir yolu temel alan bir URL oluşturmak için kullanılabilir. Rota adlarının, yönlendirmenin URL eşleştirme davranışına etkisi yoktur ve yalnızca URL oluşturma için kullanılır. Yol adları, uygulama genelinde benzersiz olmalıdır.

> [!NOTE]
> Bunu, `id` parametresini isteğe bağlı (`{id?}`) olarak tanımlayan geleneksel *varsayılan rotayla*karşıtın. API 'Leri tam olarak belirtme özelliği, `/products` ve `/products/5` farklı eylemlere dağıtılması gibi avantajlar sağlar.

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a>Yolları birleştirme

Öznitelik yönlendirmeyi daha az tekrarlı hale getirmek için, denetleyicideki yol öznitelikleri, bireysel eylemlerdeki rota öznitelikleriyle birleştirilir. Denetleyicide tanımlanan tüm yol şablonları, eylemlerdeki rota şablonlarına eklenir. Bir Route özniteliğinin denetleyiciye yerleştirilmesi, denetleyicideki **Tüm** eylemlerin öznitelik yönlendirme kullanmasını sağlar.

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

Bu örnekte, URL yolu `/products` `ProductsApi.ListProducts`ile eşleştirebilir ve URL yolu `/products/5` `ProductsApi.GetProduct(int)`eşleştirebilir. Bu eylemlerin her ikisi de, `HttpGetAttribute`olarak işaretlendiğinden HTTP `GET` eşleşir.

`/` veya `~/` ile başlayan bir eyleme uygulanan yol şablonları denetleyiciye uygulanan yol şablonları ile birleştirilmemelidir. Bu örnek, *varsayılan rotaya*benzer bir URL yolları kümesiyle eşleşir.

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Doesn't combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name="routing-ordering-ref-label"></a>

### <a name="ordering-attribute-routes"></a>Öznitelik yollarını sıralama

Tanımlı bir düzende yürütülen geleneksel yolların aksine, öznitelik yönlendirme bir ağaç oluşturur ve tüm yollarla aynı anda eşleşir. Bu, yol girişleri ideal bir sıralamaya yerleştirildiyse olduğu gibi davranır; en özel yolların, daha genel yollardan önce yürütülmesi şansınız vardır.

Örneğin, `blog/search/{topic}` gibi bir yol `blog/{*article}`gibi bir yol daha özgüdür. İlk olarak ' çalıştırmaları ' `blog/search/{topic}` yolu için, varsayılan olarak, tek yapmanız gereken tek bir sıralama olduğundan mantıksal olarak konuşun. Geleneksel yönlendirmeyi kullanarak, yolları istenen sırada yerleştirmekten geliştirici sorumludur.

Öznitelik yolları, tüm Framework yol özniteliklerinin `Order` özelliğini kullanarak bir sıra yapılandırabilir. Yollar `Order` özelliğinin artan sıralamasına göre işlenir. Varsayılan sıra `0`. `Order = -1` kullanarak bir yolun ayarlanması, bir sipariş ayarlamadan önce çalıştırılacak rotalardan önce çalıştırılır. `Order = 1` kullanarak bir yolun ayarlanması, varsayılan yol sıralaması sonrasında çalışacaktır.

> [!TIP]
> `Order`bağlı olmadığından kaçının. URL alanınız, doğru sıralama değerlerinin doğru şekilde yönlendirilmesini gerektiriyorsa, istemciler de kafa karıştırıcı olabilir. Genel öznitelik yönlendirme ' de, URL eşleştirme ile doğru yolu seçer. URL oluşturma için kullanılan varsayılan sıra çalışmıyorsa, yol adının bir geçersiz kılma olarak kullanılması genellikle `Order` özelliğini uygulamaktan daha basittir.

Razor Pages yönlendirme ve MVC denetleyici yönlendirme bir uygulamayı paylaşır. Razor Pages konularındaki yol siparişi hakkında bilgiler [Razor Pages yol ve uygulama kuralları: yol sıralaması](xref:razor-pages/razor-pages-conventions#route-order)' nda bulunabilir.

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>Yol şablonlarında belirteç değiştirme ([denetleyici], [eylem], [alan])

Özellik yolları, bir belirteci köşeli ayraç içine alarak *belirteç değişimini* destekler (`[`, `]`). `[action]`, `[area]`ve `[controller]` belirteçleri, yolun tanımlandığı eylemden eylem adı, alan adı ve denetleyici adı değerleriyle değiştirilmiştir. Aşağıdaki örnekte, Eylemler, açıklamalarda açıklandığı gibi URL yollarıyla eşleşir:

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

Belirteç değişikliği, öznitelik yollarının oluşturulması için son adım olarak gerçekleşir. Yukarıdaki örnek aşağıdaki kodla aynı şekilde davranır:

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

Öznitelik rotaları de devralma ile birleştirilebilir. Bu özellikle, belirteç değiştirme ile güçlü bir şekilde birleştirilir.

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPut("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

Belirteç değişikliği, öznitelik rotaları tarafından tanımlanan yol adları için de geçerlidir. `[Route("[controller]/[action]", Name="[controller]_[action]")]` her eylem için benzersiz bir yol adı üretir.

Sabit belirteç değiştirme sınırlayıcısı `[` veya `]`eşleştirmek için, karakteri (`[[` veya `]]`) tekrarlayarak kaçış.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a>Belirteç değişimini özelleştirmek için bir parametre transformatörü kullanın

Belirteç değiştirme, bir parametre transformatörü kullanılarak özelleştirilebilir. Bir parametre transformatörü `IOutboundParameterTransformer` uygular ve parametrelerin değerini dönüştürür. Örneğin, özel bir `SlugifyParameterTransformer` parametresi transformatörü `SubscriptionManagement` Route değerini `subscription-management`olarak değiştirir.

`RouteTokenTransformerConvention`, şu şekilde bir uygulama modeli kuralıdır:

* Bir uygulamadaki tüm öznitelik yollarına bir parametre transformatörü uygular.
* Öznitelik yol belirteci değerlerini değiştirildikleri gibi özelleştirir.

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

`RouteTokenTransformerConvention`, `ConfigureServices`bir seçenek olarak kaydedilir.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options =>
    {
        options.Conventions.Add(new RouteTokenTransformerConvention(
                                     new SlugifyParameterTransformer()));
    });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end


::: moniker range="< aspnetcore-3.0"
<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a>Birden çok yol

Öznitelik yönlendirme, aynı eyleme ulaşan birden çok yolun tanımlanmasını destekler. Bunun en yaygın kullanımları, aşağıdaki örnekte gösterildiği gibi *varsayılan geleneksel yolun* davranışını taklit etmek olur:

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

Denetleyiciye birden çok yol özniteliği koymak, her birinin eylem yöntemlerinde yol özniteliklerinin her biriyle birleşmesi anlamına gelir.

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

Birden çok yol özniteliği (`IActionConstraint`uygulayan) bir eyleme yerleştirildiğinde, her eylem kısıtlaması, onu tanımlayan öznitelikten yol şablonuyla birleştirir.

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> Eylemlerde birden çok yolun kullanılması güçlü görünse de, uygulamanızın URL alanının basit ve iyi tanımlanmış tutulması daha iyidir. Yalnızca gerektiğinde eylemler üzerinde birden çok yol kullanın, örneğin mevcut istemcileri desteklemek için.

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>Öznitelik rotası isteğe bağlı parametreler, varsayılan değerler ve kısıtlamalar belirtme

Öznitelik yolları, isteğe bağlı parametreleri, varsayılan değerleri ve kısıtlamaları belirtmek için geleneksel yollarla aynı satır içi sözdizimini destekler.

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

Yol şablonu sözdiziminin ayrıntılı açıklaması için bkz. [route Template Reference](xref:fundamentals/routing#route-template-reference) .

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>`IRouteTemplateProvider` kullanarak özel yol öznitelikleri

Çerçevede (`[Route(...)]`, `[HttpGet(...)]`, vb.) sunulan yol özniteliklerinin hepsi `IRouteTemplateProvider` arabirimini uygular. MVC, uygulama başlatıldığında denetleyici sınıflarında ve eylem yöntemlerinde öznitelikler arar ve ilk yol kümesini oluşturmak için `IRouteTemplateProvider` uygulayan uygulamaları kullanır.

Kendi yol öznitelerinizi tanımlamak için `IRouteTemplateProvider` uygulayabilirsiniz. Her `IRouteTemplateProvider`, özel bir yol şablonu, sırası ve adı ile tek bir yol tanımlamanızı sağlar:

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

Yukarıdaki örnekteki özniteliği, `[MyApiController]` uygulandığında otomatik olarak `Template` `"api/[controller]"` olarak ayarlar.

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a>Öznitelik yollarını özelleştirmek için uygulama modelini kullanma

*Uygulama modeli* , MVC tarafından eylemlerinizi yönlendirmek ve yürütmek için kullanılan tüm meta veriler ile başlangıçta oluşturulan bir nesne modelidir. *Uygulama modeli* , yol özniteliklerinden toplanan tüm verileri içerir (`IRouteTemplateProvider`aracılığıyla). Yönlendirme işleminin nasıl davranacağını özelleştirmek için, Başlangıç zamanında uygulama modelini değiştirmek üzere *kurallar* yazabilirsiniz. Bu bölümde, uygulama modeli kullanılarak yönlendirmeyi özelleştirmenin basit bir örneği gösterilmektedir.

[!code-csharp[](routing/samples/2.x/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>Karma yönlendirme: öznitelik yönlendirme vs geleneksel yönlendirme

MVC uygulamaları, geleneksel yönlendirme ve öznitelik yönlendirmenin kullanımını karıştırabilir. Tarayıcılar için HTML sayfalarına hizmet veren denetleyiciler için geleneksel yollar ve REST API 'Lerine hizmet veren denetleyiciler için öznitelik yönlendirme kullanılması normaldir.

Eylemler genel olarak Dolaştırılan veya Attribute olarak yönlendirilir. Bir yolu denetleyiciye koymak veya eylemi, BT özniteliği yönlendirilmesini sağlar. Öznitelik yollarını tanımlayan eylemlere geleneksel yollar üzerinden ulaşılamıyor ve bunun tersi de geçerlidir. Denetleyicideki **herhangi bir** rota özniteliği, denetleyici özniteliğindeki tüm eylemlerin yönlendirilmesini sağlar.

> [!NOTE]
> İki tür yönlendirme sisteminin ayırt edilmesini ne kadar ayırt eden, bir URL bir yol şablonuyla eşleştirdikten sonra uygulanan işlemdir. Geleneksel yönlendirmesinde, eşleşmeden yol değerleri, tüm geleneksel yönlendirilmiş eylemlerin arama tablosundan eylemi ve denetleyiciyi seçmek için kullanılır. Öznitelik yönlendirmesinde, her şablon zaten bir eylemle ilişkilendirilir ve başka bir arama gerekmez.

## <a name="complex-segments"></a>Karmaşık segmentler

Karmaşık segmentler (örneğin, `[Route("/dog{token}cat")]`), sabit değerli olmayan değişmez değerler ile sağdan sola eşleştirilirken işlenir. Bir açıklama için bkz. [kaynak kodu](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) . Daha fazla bilgi için [Bu soruna](https://github.com/dotnet/AspNetCore.Docs/issues/8197)bakın.

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a>URL oluşturma

MVC uygulamaları, eylemlere URL bağlantıları oluşturmak için yönlendirmenin URL oluşturma özelliklerini kullanabilir. URL oluşturma, kodlarınızın daha sağlam ve sürdürülebilir hale getirilmesi için sorunsuz kodlama URL 'Lerini ortadan kaldırır. Bu bölüm, MVC tarafından sunulan URL oluşturma özelliklerine odaklanır ve yalnızca URL oluşturmanın nasıl çalıştığına ilişkin temel bilgileri kapsar. URL oluşturma hakkında ayrıntılı bir açıklama için bkz. [yönlendirme](xref:fundamentals/routing) .

`IUrlHelper` arabirimi, URL oluşturma için MVC ve yönlendirme arasındaki temel altyapı parçasıdır. Denetleyiciler, görünümler ve görünüm bileşenlerinde `Url` özelliği aracılığıyla kullanılabilen bir `IUrlHelper` örneğini bulacaksınız.

Bu örnekte `IUrlHelper` arabirimi, başka bir eyleme yönelik bir URL oluşturmak için `Controller.Url` özelliği aracılığıyla kullanılır.

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

Uygulama varsayılan geleneksel rotayı kullanıyorsa, `url` değişkenin değeri `/UrlGeneration/Destination`URL yol dizesi olacaktır. Bu URL yolu, yönlendirme değerlerini, geçerli istekten (çevresel değerler), `Url.Action` aktarılan değerlerle ve bu değerleri yol şablonuna geçirerek birleştirerek oluşturulur.

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

Yol şablonundaki her bir rota parametresinin değeri, değerler ve ortam değerleri ile eşleşen adlara sahip olacak şekilde değiştirilir. Bir değere sahip olmayan bir rota parametresi, varsa varsayılan bir değer kullanabilir veya isteğe bağlı ise (Bu örnekteki `id` olduğu gibi) atlanır. Gerekli yol parametresinin karşılık gelen bir değeri yoksa, URL oluşturma başarısız olur. Bir yol için URL oluşturma başarısız olursa, tüm yollar Denenene veya bir eşleşme bulunana kadar sonraki yol denenir.

Yukarıdaki `Url.Action` örneği geleneksel yönlendirmeyi varsayar, ancak URL oluşturma, öznitelik yönlendirimiyle benzer şekilde çalışır, ancak kavramlar farklıdır. Geleneksel yönlendirme ile, bir şablonu genişletmek için yol değerleri kullanılır ve `controller` ve `action` için rota değerleri genellikle bu şablonda görünür-bu, yönlendirme ile eşleşen URL 'Ler bir *kurala*bağlı olduğundan, bu işe yarar. Öznitelik yönlendirmesinde, `controller` ve `action` için yol değerlerinin şablonda görünmesine izin verilmez; bunun yerine kullanılacak şablonu aramak için kullanılır.

Bu örnek öznitelik yönlendirme kullanır:

[!code-csharp[](routing/samples/2.x/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC, tüm öznitelik yönlendirilmiş eylemlerinin bir arama tablosunu oluşturur ve URL oluşturma için kullanılacak yol şablonunu seçmek üzere `controller` ve `action` değerleriyle eşleşir. Yukarıdaki örnekte `custom/url/to/destination` oluşturulur.

### <a name="generating-urls-by-action-name"></a>Eylem adına göre URL 'Leri oluşturma

`Url.Action` (`IUrlHelper`. `Action`) ve tüm ilgili aşırı yüklemeler, bir denetleyici adı ve eylem adı belirterek ne bağlandığınızı belirtmek istediğinizi temel alır.

> [!NOTE]
> `Url.Action`kullanırken, `controller` ve `action` için geçerli yol değerleri sizin için belirtilir; `controller` değeri, `action` hem *ortam değerlerinin* **hem** de *değerlerinin*bir parçasıdır. `Url.Action`yöntemi her zaman `action` ve `controller` geçerli değerlerini kullanır ve geçerli eyleme yönlendiren bir URL yolu oluşturacaktır.

Yönlendirme, bir URL oluştururken sağlamadığınız bilgileri doldurmanızı sağlamak için çevresel değerlerde değerleri kullanmayı dener. Yönlendirme parametrelerinin bir değere sahip olduğundan, `{a}/{b}/{c}/{d}` ve çevresel değerler `{ a = Alice, b = Bob, c = Carol, d = David }`gibi bir yol kullanarak yönlendirme için ek değer olmadan bir URL oluşturmaya yetecek kadar bilgi vardır. Değer `{ d = Donovan }`eklediyseniz, `{ d = David }` değeri yok sayılır ve oluşturulan URL yolu `Alice/Bob/Carol/Donovan`olur.

> [!WARNING]
> URL yolları hiyerarşiktir. Yukarıdaki örnekte, değeri `{ c = Cheryl }`eklediyseniz her iki değer de `{ c = Carol, d = David }` yok sayılır. Bu durumda artık `d` için bir değer yoktur ve URL oluşturma başarısız olur. İstediğiniz `c` ve `d`değerini belirtmeniz gerekir.  Bu sorunu varsayılan yol (`{controller}/{action}/{id?}`) ile () beklemeniz gerekebilir; ancak, `Url.Action` her zaman açıkça bir `controller` ve `action` değeri belirtmesi gibi uygulamada bu davranış hakkında nadiren karşılaşacaksınız.

Daha uzun `Url.Action` aşırı yüklemeleri, `controller` ve `action`dışındaki rota parametreleri için değerler sağlamak üzere ek bir *yol değerleri* nesnesi de alır. Bu, en yaygın olarak `Url.Action("Buy", "Products", new { id = 17 })`gibi `id` kullanıldığını görürsünüz. Kurala göre *yol değerleri* nesnesi genellikle anonim türdeki bir nesnedir, ancak bir `IDictionary<>` veya *düz bir .net nesnesi*de olabilir. Yol parametreleriyle eşleşmeyen ek rota değerleri sorgu dizesine konur.

[!code-csharp[](routing/samples/2.x/main/Controllers/TestController.cs)]

> [!TIP]
> Mutlak URL oluşturmak için, `protocol`kabul eden bir aşırı yükleme kullanın: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a>Rotaya göre URL oluşturma

Yukarıdaki kod, denetleyiciyi ve eylem adını geçirerek bir URL oluşturmayı göstermiştir. `IUrlHelper` Ayrıca `Url.RouteUrl` Yöntem ailesini da sağlar. Bu yöntemler `Url.Action`benzerdir, ancak `action` ve `controller` geçerli değerlerini rota değerlerine kopyalamaz. En yaygın kullanım, genellikle bir denetleyici veya eylem *adı belirtmeden,* URL oluşturmak için belirli bir yolu kullanmak üzere bir yol adı belirtmektir.

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a>HTML 'de URL oluşturma

`IHtmlHelper`, `<form>` ve `<a>` öğeleri oluşturmak için `HtmlHelper` yöntemleri `Html.BeginForm` ve `Html.ActionLink` sağlar. Bu yöntemler bir URL oluşturmak için `Url.Action` yöntemini kullanır ve benzer bağımsız değişkenleri kabul ederler. `HtmlHelper` için `Url.RouteUrl` compan, benzer işlevlere sahip `Html.BeginRouteForm` ve `Html.RouteLink`.

Taghelmakalar, `form` TagHelper ve `<a>` TagHelper aracılığıyla URL 'Ler oluşturur. Bunların her ikisi de kendi uygulamaları için `IUrlHelper`. Daha fazla bilgi için bkz. [formlarla çalışma](../views/working-with-forms.md) .

Görünümler içinde `IUrlHelper`, yukarıdaki herhangi bir geçici URL nesli için `Url` özelliği aracılığıyla kullanılabilir.

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a>Eylem sonuçlarında URL oluşturma

Yukarıdaki örnekler, bir denetleyicide `IUrlHelper` kullanılarak gösterilmektedir, ancak denetleyicideki en yaygın kullanım, bir eylem sonucunun parçası olarak bir URL oluşturmak olur.

`ControllerBase` ve `Controller` Taban sınıfları, başka bir eyleme başvuruda bulunan eylem sonuçları için kolay yöntemler sağlar. Tipik bir kullanım, Kullanıcı girişi kabul edildikten sonra yeniden yönlendirilmelidir.

```csharp
public IActionResult Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
    return View(customer);
}
```

Eylem sonuçları Fabrika yöntemleri `IUrlHelper`yöntemlere benzer bir model izler.

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>Adanmış geleneksel yollar için özel durum

Geleneksel yönlendirme, *adanmış geleneksel yol*olarak adlandırılan özel bir yol tanımı türünü kullanabilir. Aşağıdaki örnekte, `blog` adlı yol adanmış bir geleneksel yoldur.

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`Url.Action("Index", "Home")`, bu yol tanımlarını kullanarak `/` URL yolunu `default` rotası ile oluşturacak, ancak neden? Yol değerlerini tahmin edebilirsiniz `{ controller = Home, action = Index }` `blog`kullanarak URL oluşturmak için yeterli olacaktır ve sonuç `/blog?action=Index&controller=Home`.

Adanmış geleneksel yollar, URL oluşturmayla "çok Greedy" olmasını önleyen karşılık gelen bir yol parametresi olmayan varsayılan değerlerin özel bir davranışına bağımlıdır. Bu durumda, varsayılan değerler `{ controller = Blog, action = Article }`ve ne `controller` ne de `action` yol parametresi olarak görünmez. Yönlendirme URL oluşturma işlemi gerçekleştirdiğinde, belirtilen değerler varsayılan değerlerle eşleşmelidir. `blog` kullanılarak URL oluşturma başarısız olur çünkü değerler `{ controller = Home, action = Index }` `{ controller = Blog, action = Article }`eşleşmiyor. Ardından yönlendirme `default`denemeye geri döner ve başarılı olur.

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>Alanlar

[Bölgeler](areas.md) , ilgili işlevselliği ayrı bir yönlendirme-ad alanı (denetleyici eylemleri için) ve klasör yapısı (görünümler için) olarak bir grupla düzenlemek için kullanılan bir MVC özelliğidir. Alanların kullanılması, bir uygulamanın farklı *alanlara*sahip oldukları sürece aynı ada sahip birden çok denetleyicisi olmasına olanak sağlar. Alanların kullanılması, başka bir yol parametresi ekleyerek yönlendirme amacına yönelik bir hiyerarşi oluşturur, `controller` ve `action``area`. Bu bölüm, yönlendirmenin alanlarla nasıl etkileşime gireceğini tartışır. alanların görünümlerle nasıl kullanıldığı hakkında ayrıntılar için bkz. [alanlara](areas.md) bakın.

Aşağıdaki örnek, MVC 'yi, `Blog`adlı bir alan için varsayılan geleneksel yolu ve bir *alan yolunu* kullanacak şekilde yapılandırır:

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

`/Manage/Users/AddUser`gibi bir URL yolu eşleştirilirken, ilk yol `{ area = Blog, controller = Users, action = AddUser }`yol değerlerini oluşturur. `area` yol değeri, `area`için varsayılan bir değer tarafından üretilir, aslında `MapAreaRoute` tarafından oluşturulan yol, aşağıdaki değere eşdeğerdir:

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute`, `area` için hem varsayılan değer hem de kısıtlama (Bu durumda `Blog`) kullanarak bir yol oluşturur. Varsayılan değer, yolun her zaman `{ area = Blog, ... }`üretmesini sağlar, kısıtlama, URL oluşturma için `{ area = Blog, ... }` değer gerektirir.

> [!TIP]
> Geleneksel yönlendirme sıra bağımlıdır. Genel olarak, alanlar içeren rotalar, alan olmayan rotalardan daha belirgin olduklarından daha önce rota tablosuna yerleştirilmelidir.

Yukarıdaki örneği kullanarak, yol değerleri aşağıdaki eylemle eşleşir:

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

`AreaAttribute`, bir alanın parçası olarak denetleyiciyi belirtir, bu denetleyicinin `Blog` alanında olduğunu varsayalım. `[Area]` özniteliği olmayan denetleyiciler hiçbir alanın üyesi değildir ve `area` yol değeri yönlendirme tarafından sağlandığında **eşleşmeyecektir** . Aşağıdaki örnekte, yalnızca listelenen ilk denetleyici `{ area = Blog, controller = Users, action = AddUser }`rota değerleriyle eşleştirebilir.

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> Her denetleyicinin ad alanı, tamamlanma için burada gösterilir. Aksi takdirde, denetleyicilerde adlandırma çakışması olur ve derleyici hatası oluşturur. Sınıf ad alanlarının MVC 'nin yönlendirme üzerinde hiçbir etkisi yoktur.

İlk iki denetleyici alanların üyeleridir ve yalnızca ilgili alan adı `area` rota değeri tarafından sağlandığında eşleşir. Üçüncü denetleyici hiçbir alanın üyesi değildir ve yalnızca Yönlendirme tarafından `area` hiçbir değer sağlanmıyorsa eşleşemez.

> [!NOTE]
> *Değer olmadan*eşleşme açısından, `area` değerinin yokluğu, `area` değeri null ya da boş dize olarak aynıdır.

Bir alan içinde bir eylem yürütürken, `area` için rota değeri, yönlendirme için, URL oluşturma için kullanılacak *çevresel bir değer* olarak kullanılabilir. Bu, varsayılan olarak, aşağıdaki örnekte gösterildiği gibi, URL oluşturma için *yapışkan* olarak hareket ettiği anlamına gelir.
[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a>Iactionconstraint 'i anlama

> [!NOTE]
> Bu bölüm, Framework iç işlevleri hakkında ayrıntılı bir bakış ve MVC 'nin yürütülecek eylemi nasıl seçtiği. Tipik bir uygulama özel bir `IActionConstraint` gerektirmez

Büyük olasılıkla, arabirime tanıdık olmasanız bile `IActionConstraint` zaten kullandık. `[HttpGet]` özniteliği ve benzer `[Http-VERB]` öznitelikleri, bir eylem yönteminin yürütülmesini sınırlandırmak için `IActionConstraint` uygular.

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

Varsayılan geleneksel yolun kabul edilmesinden, URL yolunun `/Products/Edit`, burada gösterilen eylemlerle **her ikisi de** eşleşen değerler `{ controller = Products, action = Edit }`üretecektir. `IActionConstraint` terminolojisinde, her ikisi de rota verileriyle eşleştiğinden, bu eylemlerin her ikisi de aday olarak kabul edilir.

`HttpGetAttribute` yürütüldüğünde, *Edit ()* , *Get* için bir EŞLEŞMEDIR ve diğer http fiili için bir eşleşme değildir. `Edit(...)` eyleminde tanımlı kısıtlama yok ve bu nedenle herhangi bir HTTP fiili ile eşleşir. Bu nedenle, yalnızca `POST` bir `Edit(...)` eşleştiğini kabul eder. Ancak `GET` için her iki eylem de eşleşemez, ancak `IActionConstraint` bir eylem, olmadan bir eylemden en *iyi* şekilde değerlendirilir. Bu nedenle `Edit()`, `[HttpGet]` daha belirgin olarak değerlendirilir ve her iki eylemin da eşleşeceğinden seçilecek.

Kavramsal olarak, `IActionConstraint` *aşırı yükleme*biçimidir, ancak aynı ada sahip yöntemlerin aşırı yüklenmesi yerıne aynı URL ile eşleşen eylemler arasında aşırı yüklenir. Öznitelik yönlendirme `IActionConstraint` de kullanır ve farklı denetleyicilerden gelen eylemlere her ikisi de aday olarak kabul edilebilir.

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a>Iactionconstraint uygulama

`IActionConstraint` kullanmanın en kolay yolu, `System.Attribute` türetilmiş bir sınıf oluşturmaktır ve bunları eylemleriniz ve denetleyicilerinize yerleştirmelidir. MVC, öznitelik olarak uygulanan `IActionConstraint` otomatik olarak bulur. Kısıtlama uygulamak için uygulama modelini kullanabilirsiniz ve bu, büyük olasılıkla en esnek yaklaşımdır ve bu sayede, nasıl uygulanabileceğini meta programlayabilirsiniz.

Aşağıdaki örnekte, bir kısıtlama yol verilerinden bir *ülke kodunu* temel alan bir eylem seçer. [GitHub 'daki tam örnek](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

`Accept` yöntemi uygulamaktan ve kısıtlamanın yürütülmesi için bir ' Order ' seçmeye sorumlusunuz. Bu durumda `Accept` yöntemi, `country` rota değeri eşleştiğinde eylemin bir eşleşme olduğunu göstermek için `true` döndürür. Bu, varolmayan bir eyleme geri dönüş sağlayan bir `RouteValueAttribute` farklıdır. Örnek, bir `en-US` eylemi tanımlarsanız `fr-FR` gibi bir ülke kodunun `[CountrySpecific(...)]` uygulanmamış daha genel bir denetleyiciye geri dönemeyeceğini gösterir.

`Order` özelliği, kısıtlamanın parçası olan *aşamayı* belirler. Eylem kısıtlamaları `Order`göre gruplar halinde çalışır. Örneğin, tüm Framework tarafından sunulan HTTP yöntemi öznitelikleri aynı aşamada çalışacak şekilde aynı `Order` değerini kullanır. İstediğiniz ilkeleri uygulamak için ihtiyacınız olan çok sayıda aşamaya sahip olabilirsiniz.

> [!TIP]
> `Order` bir değere karar vermek için, kısıtlamalarınızın HTTP yöntemlerinden önce uygulanıp uygulanmayacağı hakkında düşünün. Daha az sayı önce çalışır.

::: moniker-end
