---
title: ASP.NET core'da denetleyici eylemlerine yönlendirme
author: rick-anderson
description: Ara yazılım yönlendirme ASP.NET Core MVC URL'leri gelen isteklerin aynı ve onları eylemleri eşlemek için nasıl kullandığını öğrenin.
ms.author: riande
ms.date: 09/17/2018
uid: mvc/controllers/routing
ms.openlocfilehash: 2f6328a5efaa96fd8e4f0cafdbde77dd63a1548f
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477650"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a>ASP.NET core'da denetleyici eylemlerine yönlendirme

Tarafından [Ryan Nowak](https://github.com/rynowak) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core MVC yönlendirme kullanan [ara yazılım](xref:fundamentals/middleware/index) URL'leri gelen isteklerin aynı ve eylemlere eşleştirebilirsiniz. Yollar, başlatma kodu veya öznitelikleri tanımlanır. Yollar, URL yolu için Eylemler nasıl eşleştirilmesi gerektiğini açıklar. Yolları (için bağlantılar) gönderilen yanıtlarını URL'lerini oluşturmak için de kullanılır. 

Eylemler genel yönlendirilir veya öznitelik yönlendirilir. Bir rota denetleyici veya eylem getirir, yönlendirilmiş özniteliği. Bkz: [yönlendirme karma](#routing-mixed-ref-label) daha fazla bilgi için.

Bu belge, MVC ve Yönlendirme ve nasıl tipik MVC uygulamaları oluşturma arasındaki etkileşimler açıklayacak yönlendirme özelliklerini kullanın. Bkz: [yönlendirme](xref:fundamentals/routing) Gelişmiş yönlendirme hakkında bilgi.

## <a name="setting-up-routing-middleware"></a>Ara yazılım yönlendirme ayarlama

İçinde *yapılandırma* yöntemi için benzer bir kod ile karşılaşabilirsiniz:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Çağrısı içinde `UseMvc`, `MapRoute` biz olarak başvuracağınız tek bir yol oluşturmak için kullanılan `default` rota. Çoğu MVC uygulamaları için benzer bir şablonla bir yol kullanır `default` rota.

Rota şablonu `"{controller=Home}/{action=Index}/{id?}"` gibi bir URL yolu eşleşebilir `/Products/Details/5` ve rota değerlerini ayıklar `{ controller = Products, action = Details, id = 5 }` yolu belirteç oluşturma tarafından. MVC adlı bir denetleyici bulmaya çalışır `ProductsController` ve çalıştırma eylemi `Details`:

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

Bu örnekte, model bağlama değerini kullanırsınız Not `id = 5` ayarlanacak `id` parametresi `5` bu eylemi çağrılırken. Bkz: [Model bağlama](../models/model-binding.md) daha fazla ayrıntı için.

Kullanarak `default` yolu:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

Rota şablonu:

* `{controller=Home}` tanımlar `Home` varsayılan olarak `controller`

* `{action=Index}` tanımlar `Index` varsayılan olarak `action`

* `{id?}` tanımlar `id` isteğe bağlı olarak

Varsayılan ve isteğe bağlı yol parametreleri, URL yolu için bir eşleşme bulunması gerekmez. Bkz: [rota şablonu başvurusu](../../fundamentals/routing.md#route-template-reference) rota şablonu söz dizimi ayrıntılı bir açıklaması.

`"{controller=Home}/{action=Index}/{id?}"` URL yolu eşleşebilir `/` ve rota değerleri oluşturacak `{ controller = Home, action = Index }`. Değerleri `controller` ve `action` yapmak için varsayılan değerleri kullanmak `id` URL yolunda yok ilgili segment olduğundan değer üretemez. MVC kullandığınız bu rota değerlerini seçmek için `HomeController` ve `Index` eylem:

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

Bu denetleyici tanımı ve rota şablonu kullanarak `HomeController.Index` eylem yürütülebilir herhangi biri, aşağıdaki URL yolu için:

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

`UseMvc` ve `UseMvcWithDefaultRoute` bir örneğini ekleme `RouterMiddleware` ara yazılım ardışık düzenini için. MVC doğrudan Ara yazılımla etkileşim kurmaz ve yönlendirme isteklerini işlemek için kullanır. MVC yolları örneği üzerinden bağlı olduğu `MvcRouteHandler`. Kod içinde `UseMvc` aşağıdakine benzer:

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

`UseMvc` tüm rotalar doğrudan tanımlamıyor rota koleksiyonu için bir yer tutucu ekler `attribute` rota. Aşırı yükleme `UseMvc(Action<IRouteBuilder>)` kendi yönlendirmelerinizi eklemenize olanak sağlayan ve de öznitelik Yönlendirme'yi destekler.  `UseMvc` ve tüm türevlerini ekler özniteliği yol için bir yer tutucu - öznitelik yönlendirme her zaman nasıl yapılandırdığınıza bakmaksızın kullanılabilir `UseMvc`. `UseMvcWithDefaultRoute` Varsayılan bir yol tanımlar ve öznitelik yönlendirme destekler. [Öznitelik yönlendirme](#attribute-routing-ref-label) bölüm öznitelik yönlendirme hakkında daha fazla ayrıntı içerir.

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a>Geleneksel yönlendirme

`default` Yolu:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

bir örneğini bir *geleneksel yönlendirme*. Bu stil diyoruz *geleneksel yönlendirme* , çünkü bir *kuralı* URL yolu için:

* İlk yol kesimi denetleyici adıyla eşleşir.

* Eylem adı ikinci eşlenir.

* Üçüncü kesim için isteğe bağlı olarak kullanılan `id` modeli varlığa eşlemek için kullanılır

Bunu kullanarak `default` rota, URL yolu `/Products/List` eşlendiği `ProductsController.List` eylem ve `/Blog/Article/17` eşlendiği `BlogController.Article`. Bu eşleme denetleyici ve eylem adları bağlıdır **yalnızca** ve ad alanları, kaynak dosya konumlarının veya yöntem parametreleri göre değil.

> [!TIP]
> Geleneksel olan varsayılan yol yönlendirme kullanarak tanımladığınız her eylem için yeni bir URL deseni ile gündeme gerek kalmadan hızla uygulama oluşturmanızı sağlar. CRUD stili eylemleri ile bir uygulama için tutarlılık için URL'leri denetleyicilerinizi arasında olması, kodunuzu basitleştirerek ve kullanıcı Arabirimi daha öngörülebilir hale yardımcı olabilir.

> [!WARNING]
> `id` İsteğe bağlı olarak eylemlerinizi URL'SİNİN bir parçası belirtilen kimliği olmadan yürütebilir anlamına gelir, bir rota şablonu tarafından tanımlanır. Genellikle ne olacağını `id` URL'den atlanırsa, ayarlanacak emin olan `0` model bağlama ve varlık içinde eşleşen veritabanı bulunabilir sonucunda `id == 0`. Öznitelik yönlendirme, bazı eylemler için ve başkaları için gerekli kimliği olmak için ayrıntılı denetim verebilirsiniz. İsteğe bağlı parametreler belgeleri içerecektir kurala göre ister `id` zaman bunlar büyük olasılıkla doğru kullanımı görünür.

## <a name="multiple-routes"></a>Birden fazla rota

İçinde birden çok yol ekleyebilirsiniz `UseMvc` daha fazla çağrıları ekleyerek `MapRoute`. Bunun yapılması, birden çok kuralları tanımlamak için veya belirli bir eylem için ayrılmış gibi geleneksel yollar eklemeniz sağlar:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`blog` Yolu Buraya bir *adanmış geleneksel rota*, bu geleneksel yönlendirme sistem kullanır ancak belirli bir eylem için ayrılmış anlamına gelir. Bu yana `controller` ve `action` yol şablonunda parametreler olarak görünmez, yalnızca varsayılan değerleri olabilir ve bu nedenle bu rota için eylem her zaman eşleyecektir `BlogController.Article`.

Rota koleksiyonu yolları sıralanır ve bunların eklenen sırayla işlenir. Bu örnekte, bu nedenle `blog` rota çalıştığınız önce `default` rota.

> [!NOTE]
> *Geleneksel rotaları dedicated* tümünü rota parametrelerinin gibi sık kullandığınız `{*article}` URL yolu kalan kısmı yakalamak için. Bu bir yol 'çok doyumsuz' yapabilirsiniz diğer yollar eşlenmesi yönelik URL'ler eşleştiğini anlamına gelir. 'Doyumsuz' yolları, bunu çözmek için daha sonra yolu tabloya yerleştirin.

### <a name="fallback"></a>Geri dönüş

İstek işleme bir parçası olarak, MVC, rota değerleri, uygulamanızda bir denetleyici ve eylem bulmak için kullanılabilir doğrular. Rota değerlerini bir eylem eşleşmezse ardından yol bir eşleşme olarak değil ve sonraki yol denenir. Bu adlandırılır *geri dönüş*, ve geleneksel yollar çakıştığı çalışmaları basitleştirmek kullanılmaya.

### <a name="disambiguating-actions"></a>Belirsizliğini eylemleri

İki eylem yönlendirme ile eşleşiyorsa, MVC 'en iyi' adayı seçin veya başka bir özel durum için ayırt etmek gerekir. Örneğin:

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

Bu denetleyici URL yolu BC iki eylem tanımlar `/Products/Edit/17` ve rota verilerini `{ controller = Products, action = Edit, id = 17 }`. MVC denetleyicileri için genel bir desen olmasıdır burada `Edit(int)` bir ürün düzenlemek için bir form gösterir ve `Edit(int, Product)` gönderilen bir formu işler. Bunu mümkün hale getirmek için MVC seçin gerekir `Edit(int, Product)` bir HTTP istek olduğunda `POST` ve `Edit(int)` HTTP fiili olduğunda başka bir şey.

`HttpPostAttribute` ( `[HttpPost]` ) Uygulamasıdır `IActionConstraint` yalnızca izin veren HTTP fiili olduğunda, seçili eylem `POST`. Varlığı bir `IActionConstraint` yapar `Edit(int, Product)` 'daha iyi' eşleşen daha `Edit(int)`, bu nedenle `Edit(int, Product)` ilk kez yeniden denenecek.

Yalnızca özel yazmanız gerekecektir `IActionConstraint` özel senaryoları, ancak uygulamalar gibi öznitelikleri rolünü anlamak önemlidir `HttpPostAttribute` -benzer öznitelikleri için diğer HTTP fiilleri tanımlanır. Geleneksel yönlendirme parçası olduklarında aynı eylem adı kullanacak şekilde eylemler için yaygındır bir `show form -> submit form` iş akışı. Bu düzen uygun gözden geçirdikten sonra daha belirgin hale gelir [anlama IActionConstraint](#understanding-iactionconstraint) bölümü.

Birden çok yol eşleşen ve MVC 'en iyi' yolu bulunamıyor, throw bir `AmbiguousActionException`.

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a>Rota adları

Dizeleri `"blog"` ve `"default"` aşağıdaki örneklerde, rota adlarıdır:


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Böylece adlı rota URL üretmek için kullanılan rota adlarını rota mantıksal bir ad verin. URL üretimi karmaşık yollarını sıralama yapabilir, bu URL oluşturmayı büyük ölçüde kolaylaştırır. Rota adları, uygulama genelinde benzersiz olmalıdır.

Rota adları eşleşen veya işleme isteklerinin URL'yi bir etkisi yok; Bunlar yalnızca URL üretmek için kullanılır. [Yönlendirme](xref:fundamentals/routing) MVC özgü Yardımcılar olarak URL üretimi dahil olmak üzere URL oluşturma hakkında ayrıntılı bilgi vardır.

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a>Öznitelik yönlendirme

Öznitelik yönlendirme eylemleri doğrudan rota şablonlarının eşlemek için bir öznitelik kümesini kullanır. Aşağıdaki örnekte, `app.UseMvc();` kullanılır `Configure` yöntemi ve yol geçirilir. `HomeController` URL'ler için hangi varsayılan rota benzer birtakım eşleşecektir `{controller=Home}/{action=Index}/{id?}` eşleşir:

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

`HomeController.Index()` Herhangi bir URL yolu için eylem yürütülecek `/`, `/Home`, veya `/Home/Index`.

> [!NOTE]
> Bu örnekte, öznitelik Yönlendirme ve geleneksel yönlendirme anahtar programlama birbirinden vurgular. Öznitelik yönlendirme bir yol belirtmek için daha fazla giriş gerektirir; Geleneksel varsayılan yol temellerini daha yollarını işler. Bununla birlikte, öznitelik yönlendirme sağlar (ve gerektirir) rota şablonlarının her eylem için geçerli kesin bir denetim.

Denetleyici adı ve eylem adları yönlendirme özniteliğiyle play **hiçbir** eylem seçildiğinde rol. Bu örnek önceki örnekteki gibi aynı URL'leri eşleşir.

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
> Rota şablonlarının rota parametrelerinin tanımlama yok `action`, `area`, ve `controller`. Aslında, bu rota parametreleri öznitelik rotaları izin verilmez. Rota şablonu zaten bir eylem ile ilişkili olduğundan bu eylem adını URL'den ayrıştırılacak anlamsız olmasını.

## <a name="attribute-routing-with-httpverb-attributes"></a>HTTP [eylem] özniteliği ile yönlendirme özniteliği

Öznitelik yönlendirme de yapabilirsiniz kullanım `Http[Verb]` gibi öznitelikleri `HttpPostAttribute`. Tüm bu öznitelikler, bir rota şablonu kabul edebilir. Bu örnek aynı rota şablonu eşleşen iki eylemleri gösterir:

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

Gibi bir URL yolu için `/products` `ProductsApi.ListProducts` eylemi, HTTP fiili olduğunda yürütülecek `GET` ve `ProductsApi.CreateProduct` HTTP fiili olduğunda yürütülecek `POST`. İlk öznitelik yönlendirme URL rota öznitelikleri tarafından tanımlanan rota şablonlarının kümesini karşı eşleşir. Eşleşen bir rota şablonu sonra `IActionConstraint` kısıtlamaları, hangi eylemlerin yürütülebilecek belirlemek için uygulanır.

> [!TIP]
> REST API oluştururken, kullanmak istediğiniz nadir `[Route(...)]` bir eylem yöntemi. Daha fazla özel kullanmak daha iyidir `Http*Verb*Attributes` API'NİZİN desteklediği hakkında kesin olarak. REST API'leri istemcileri belirli bir mantıksal işlemler için ne yollarını ve HTTP fiilleri harita bilmeniz beklenir.

Öznitelik rotası belirli bir eylem için geçerli olduğundan, yol şablon tanımının bir parçası gerekli parametreleri sağlamak kolay bir işlemdir. Bu örnekte, `id` URL yolu bir parçası olarak gerekli değildir.

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

`ProductsApi.GetProduct(int)` Gibi bir URL yolu için eylem yürütülecek `/products/3` gibi bir URL yolu için `/products`. Bkz: [yönlendirme](../../fundamentals/routing.md) rota şablonlarının ve ilgili seçeneklerinin tam bir açıklaması için.

## <a name="route-name"></a>Rota adı

Aşağıdaki kodu tanımlayan bir *rota adı* , `Products_List`:

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

Rota adları, belirli bir rotaya bağlı bir URL'yi oluşturmak için kullanılabilir. Rota adları, URL yönlendirme davranışı eşleştirme üzerinde hiçbir etkisi ve yalnızca URL üretmek için kullanılır. Rota adları, uygulama genelinde benzersiz olmalıdır.

> [!NOTE]
> Bu, geleneksel Karşıtlık *varsayılan rota*, tanımlayan `id` parametre olarak isteğe bağlı (`{id?}`). Bu özelliği tam olarak API'leri belirtmek için izin verme gibi avantaj `/products` ve `/products/5` farklı eylemler için gönderilmesi.

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a>Yolları birleştirme

Öznitelik yönlendirme daha az tekrarlı yapmak için rota denetleyici özniteliklerinden yola rotası özniteliklerinin bireysel işlemlere ile birleştirilir. Rota şablonlarının eylemleri için denetleyicide tanımlanan tüm rota şablonlarının tanımlandıkları. Rota özniteliğinin denetleyicisinde yerleştirerek yapar **tüm** denetleyici eylemlerini kullanmak öznitelik yönlendirme.

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

Bu örnekte URL yolu `/products` eşleşebilir `ProductsApi.ListProducts`ve URL yolunu `/products/5` eşleşebilir `ProductsApi.GetProduct(int)`. Bu eylemlerin her ikisi de yalnızca HTTP eşleşen `GET` ile belirtilmiş çünkü `HttpGetAttribute`.

Rota ile başlayan bir eyleme uygulanan şablonlar bir `/` rota şablonuyla denetleyiciye uygulanan birleştirilmiş yok. URL yolu için benzer bir dizi şu örnekle eşleşir *varsayılan rota*.

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

### <a name="ordering-attribute-routes"></a>Öznitelik rotaları sıralama

Tanımlanan bir sırayla yürütmek geleneksel yollar aksine öznitelik yönlendirme bir ağaç oluşturur ve aynı anda tüm yollar eşleşir. Bu gibi davranır-rota girişleri bir ideal sıralamada; yerleştirilmişse en belirgin yolları önce genel daha yollarını yürütme şansına sahip olabilirsiniz.

Örneğin, bir yol gibi `blog/search/{topic}` bir yol gibi daha fazla özel `blog/{*article}`. Mantıksal olarak konuşma `blog/search/{topic}` yol 'çalışır' ilk olarak, varsayılan olarak, yalnızca duyarlı sıralama olduğundan. Geleneksel yönlendirme kullanarak Geliştirici yollar sıralamada yerleştirmek için sorumludur.

Öznitelik rotaları, bir sipariş yapılandırabilir kullanarak `Order` tüm framework sağlanan rota özniteliklerini özelliği. Yollar, artan göre işlenir tür `Order` özelliği. Varsayılan sıra `0`. Yolu kullanarak bir ayarı `Order = -1` sipariş ayarlamamanız yolları önce çalışır. Yolu kullanarak bir ayarı `Order = 1` varsayılan rota sıralandıktan sonra çalışır.

> [!TIP]
> Yapılandırmanıza bağlı olarak önlemek `Order`. Sonra URL alanını doğru bir şekilde yönlendirmek için Açık sipariş değerleri gerektiriyorsa, istemcilere de büyük olasılıkla karmaşık olur. Genel öznitelik yönlendirme ile URL ile eşleşen doğru yolu seçer. URL üretimi için kullanılan varsayılan düzenini çalışmıyorsa, bir geçersiz kılma uygulama daha genellikle daha basit olduğu rota adı kullanılarak `Order` özelliği.

Yönlendirme razor sayfaları ve MVC denetleyicisi yönlendirme paylaşım uygulaması. Rota sırası Razor sayfaları konularında bilgi şu adreste [Razor sayfaları yol ve uygulama kuralları: rota sırasını](xref:razor-pages/razor-pages-conventions#route-order).

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>Belirteç değiştirme rota şablonlarındaki ([controller] [action] [alanı])

Kolaylık olması için öznitelik rotaları Destek *belirteç değiştirme* tarafından bir belirteç köşeli ayraç içinde kapsayan (`[`, `]`). Belirteçleri `[action]`, `[area]`, ve `[controller]` eylem adı, alan adı ve denetleyici adını, rota tanımlandığı eyleminin değerleri ile değiştirilir. Aşağıdaki örnekte, URL yolu yorumlar bölümünde anlatıldığı gibi eylemleri eşleşmesi:

[!code-csharp[](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

Belirteç değiştirme öznitelik rotaları oluşturma işleminin son adımında gerçekleşir. Yukarıdaki örnek aşağıdaki kodla aynı davranır:

[!code-csharp[](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

Öznitelik rotaları da devralma ile birleştirilebilir. Bu belirteç değiştirme ile birleştirilmiş özellikle güçlüdür.

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

Belirteç değiştirme öznitelik rotaları tarafından tanımlanan rota adları için de geçerlidir. `[Route("[controller]/[action]", Name="[controller]_[action]")]` Her eylem için benzersiz bir rota adı oluşturur.

Sabit belirteç değiştirme sınırlayıcı eşleştirilecek `[` veya `]`, karakter tekrarlayarak kaçış (`[[` veya `]]`).

::: moniker range=">= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a>Belirteç değiştirme özelleştirmek için bir parametre transformer kullanma

Belirteç değiştirme parametresi transformer kullanılarak özelleştirilebilir. Bir parametre transformer uygulayan `IOutboundParameterTransformer` ve parametre değerine dönüştürür. Örneğin, bir özel `SlugifyParameterTransformer` parametre transformer değişiklikleri `SubscriptionManagement` yönlendirmek için değer `subscription-management`.

`RouteTokenTransformerConvention` Bir uygulama modeli kuralına göre:

* Bir uygulamadaki tüm öznitelik rotaları parametresi transformer uygular.
* Bunlar gibi öznitelik rotası belirteci değerleri özelleştirir.

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

`RouteTokenTransformerConvention` Bir seçenek olarak kayıtlı `ConfigureServices`.

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

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a>Birden fazla rota

Aynı eylemi ulaşan birden çok yol tanımlayan yönlendirme desteklediği öznitelik. Davranışını taklit etmek için bu en yaygın kullanımı olan *varsayılan geleneksel yolu* aşağıdaki örnekte gösterildiği gibi:

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

Birden çok yol özniteliğine denetleyicisinde koyma, her biri her eylem yöntemlerini rotası özniteliklerinin birleştirecek anlamına gelir.

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

Zaman birden çok yol özniteliğine (uygulayan `IActionConstraint`) bir eylem, her eylem kısıtlaması ile tanımlanan özniteliğindeki rota şablonu birleştirir sonra yerleştirilir.

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
> Birden çok yol eylemleri kullanarak güçlü görünebilir, ancak uygulamanızın URL'si alanı basit ve iyi tanımlanmış durumda tutmak daha iyidir. Eylemler yalnızca gerektiğinde, örneğin mevcut istemcileri desteklemek için birden çok yol kullanın.

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>Öznitelik yönlendirme isteğe bağlı parametreleri, varsayılan değerleri ile kısıtlamaları belirtme

Öznitelik rotaları isteğe bağlı parametreler, varsayılan değerleri ile kısıtlamaları belirtmek için geleneksel rotalar aynı satır içi söz dizimini destekler.

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

Bkz: [rota şablonu başvurusu](../../fundamentals/routing.md#route-template-reference) rota şablonu söz dizimi ayrıntılı bir açıklaması.

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>Özel rota özniteliklerini kullanma `IRouteTemplateProvider`

Tüm framework içinde sağlanan rota özniteliklerini ( `[Route(...)]`, `[HttpGet(...)]` , vs.) uygulama `IRouteTemplateProvider` arabirimi. MVC arar denetleyici sınıflarına ve eylem yöntemlerine özniteliklerinde uygulama başladığında ve uygulama olanları kullanan `IRouteTemplateProvider` ilk rota kümesini oluşturmak için.

Uygulayabileceğiniz `IRouteTemplateProvider` kendi rota özniteliklerini tanımlamak için. Her `IRouteTemplateProvider` sipariş tek bir rota özel rota şablonuyla tanımlamanıza olanak sağlar ve adlandırın:

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

Yukarıdaki örnekte öznitelik otomatik olarak ayarlar `Template` için `"api/[controller]"` olduğunda `[MyApiController]` uygulanır.

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a>Öznitelik rotaları özelleştirmek için uygulama modelini kullanarak

*Uygulama modeli* Başlangıçta tüm MVC tarafından yönlendirmek ve eylemlerinizi yürütmek için kullanılan meta veriler ile oluşturulan bir nesne modeli. *Uygulama modeli* rota özniteliklerinden toplanan verileri içerir (aracılığıyla `IRouteTemplateProvider`). Yazabileceğiniz *kuralları* nasıl yönlendirme davranışını özelleştirmek için başlangıç zamanında uygulama modeli değiştirmek için. Bu bölümde, uygulama modelini kullanarak yönlendirme özelleştirilmesine basit bir örnek gösterilmektedir.

[!code-csharp[](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>Karma yönlendirme: vs geleneksel yönlendirmesi özniteliği

MVC uygulamaları geleneksel Yönlendirme ve öznitelik yönlendirme karıştırabilirsiniz. Tarayıcılar için HTML sayfalarını sunmadan denetleyicileri için geleneksel yollar kullanın ve REST API'leri sunan denetleyicileri için yönlendirme özniteliği için tipik bir durumdur.

Eylemler genel yönlendirilir veya öznitelik yönlendirilir. Bir rota denetleyici veya eylem getirir, yönlendirilmiş özniteliği. Öznitelik rotaları tanımlamak Eylemler, geleneksel yolları ve tersi ulaşılamıyor. **Tüm** rota özniteliğinin denetleyicisinde yönlendirilen denetleyicisi özniteliğinde tüm eylemleri yapar.

> [!NOTE]
> Ne yönlendirme sistemleri iki tür ayıran bir rota şablonuna bir URL ile eşleşen sonra uygulanan işlemidir. Yönlendirme Geleneksel, eşleşme rota değerleri için eylem ve denetleyici tüm geleneksel yönlendirilmiş eylemleri bir arama tablosundan seçmek için kullanılır. Öznitelik yönlendirme, her bir şablon zaten bir eylem ile ilişkilendirilir ve başka hiçbir arama gereklidir.

## <a name="complex-segments"></a>Karmaşık segmentleri

Karmaşık bir segment (örneğin, `[Route("/dog{token}cat")]`), değişmez değerlerden soldan sağa'kurmak doyumsuz olmayan bir yolla eşleşen tarafından işlenir. Bkz: [kaynak kodu](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) için bir açıklama. Daha fazla bilgi için [bu sorunu](https://github.com/aspnet/Docs/issues/8197).

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a>URL üretimi

MVC uygulamaları ait yönlendirme URL'si oluşturma özellikleri eylemleri URL bağlantılarını oluşturmak için kullanabilirsiniz. URL'ler oluşturulurken, runbook'a kod URL'ler, kodunuzu daha sağlam ve sürdürülebilir hale ortadan kaldırır. Bu bölümde, MVC tarafından sağlanan URL üretimi özelliklere odaklanır ve yalnızca URL üretimi nasıl çalıştığına ilişkin temel bilgileri ele alınacaktır. Bkz: [yönlendirme](../../fundamentals/routing.md) URL üretimi ayrıntılı bir açıklaması.

`IUrlHelper` Temel alınan altyapı MVC arasındaki URL üretmek için yönlendirme parçası arabirimidir. Bir örneğini bulabilirsiniz `IUrlHelper` aracılığıyla `Url` denetleyicileri, görünümleri ve görünüm bileşenleri özelliği.

Bu örnekte, `IUrlHelper` arabirimi aracılığıyla kullanılır `Controller.Url` özelliği başka bir eylem için bir URL oluşturun.

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

Geleneksel varsayılan uygulamayı kullanıyorsa, yol, değerini `url` değişkeni URL yol dizesi olacaktır `/UrlGeneration/Destination`. Bu URL yolu geçerli istek (ortam değerler), rota değerleri birleştirerek yönlendirme tarafından geçirilen değerlerle oluşturulur `Url.Action` ve bu değerler rota şablonuna değiştirme:

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

Rota şablonu her yol parametresi değerleri ve ortam değerleri ile eşleşen adları yerine değeri vardır. Bir değere sahip olmayan bir rota parametresini varsa veya isteğe bağlı ise atlanması durumunda varsayılan bir değer kullanabilirsiniz (olarak durumunda `id` Bu örnekte). Tüm gerekli bir rota parametresini karşılık gelen bir değer yoksa, URL üretimi başarısız olur. Bir rota için URL üretimi başarısız olursa, sonraki yol, tüm yolları denediniz mi veya bir eşleşme bulunduğu kadar denenir.

Örnek `Url.Action` kavramları farklı olsa yukarıdaki geleneksel yönlendirme varsayılmaktadır, ancak öznitelik yönlendirme ile benzer şekilde URL üretimi çalışır. Geleneksel yönlendirme ile rota değerlerini bir şablon ve rota değerleri için genişletmek için kullanılan `controller` ve `action` genellikle görünür şablon içinde - yönlendirerek eşleşen URL'leri bağlı olduğundan bu çalışır bir *kuralı*. Öznitelik yönlendirme, rota değerleri için `controller` ve `action` görüntülenecek şablonda - yerine kullandıkları hangi şablonun kullanılacağını aramak için izin verilmiyor.

Bu örnekte, öznitelik yönlendirme kullanır:

[!code-csharp[](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC tüm öznitelik yönlendirilen eylemlerin arama tablosu oluşturur ve eşleştirir `controller` ve `action` URL üretmek için kullanılacak rota şablonu seçmek için değerler. Yukarıdaki örnekteki `custom/url/to/destination` oluşturulur.

### <a name="generating-urls-by-action-name"></a>Eylem adına göre URL'ler oluşturulurken

`Url.Action` (`IUrlHelper` . `Action`) ve tüm ilgili ne bir denetleyici adı ve eylem adı belirterek bağlantı kurduğunuz belirtmek istiyorsanız bu fikri dayalı tüm aşırı yüklemeler.

> [!NOTE]
> Kullanırken `Url.Action`, geçerli rota değerleri için `controller` ve `action` - değerini belirtilir `controller` ve `action` hem de bir parçası olan *ortam değerlerini* **ve** *değerleri*. Yöntem `Url.Action`, her zaman geçerli değerleri kullanan `action` ve `controller` ve geçerli eylem için yönlendiren bir URL yolu oluşturur.

Yönlendirme değerleri, bir URL oluşturulurken sağlamadı bilgilerinde doldurmak için ortam değerleri kullanmayı dener. Gibi bir yol kullanılarak `{a}/{b}/{c}/{d}` ve ortam değerlerini `{ a = Alice, b = Bob, c = Carol, d = David }`, yönlendirme, ek değer içermeyen bir URL'yi oluşturmak için yeterli bilgiye sahip - tüm yol olduğundan parametre bir değere sahip. Değer eklediyseniz `{ d = Donovan }`, değer `{ d = David }` yoksayıldı, ve oluşturulan URL yolu `Alice/Bob/Carol/Donovan`.

> [!WARNING]
> URL yolu hiyerarşiktir. Yukarıdaki örnekte değer katan `{ c = Cheryl }`, değerlerin her ikisini de `{ c = Carol, d = David }` yok. Bir değer için artık bu durumda sahibiz `d` ve URL üretimi başarısız olur. İstenen değeri belirtmek zorunda kalmaz `c` ve `d`.  Varsayılan yol ile bu sorun isabet beklenebilir (`{controller}/{action}/{id?}`)- ancak bu davranış uygulamada nadiren karşınıza çıkacak `Url.Action` her zaman açıkça belirten bir `controller` ve `action` değeri.

Aşırı uzun `Url.Action` da ek bir göz *rota değerleri* dışında rota parametrelerinin değerlerini sağlamak nesne `controller` ve `action`. En yaygın olarak kullanılan bu görürsünüz `id` gibi `Url.Action("Buy", "Products", new { id = 17 })`. Kural gereği *rota değerleri* nesnedir genellikle anonim türde bir nesne, ancak ayrıca olabilir bir `IDictionary<>` veya *düz eski .NET nesne*. Rota parametrelerine eşleşmeyen herhangi bir ek rota değerleri, sorgu dizesinde yerleştirilir.

[!code-csharp[](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> Mutlak bir URL oluşturmak için kabul eden bir aşırı yüklemesini kullanın. bir `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a>Yol tarafından URL'ler oluşturulurken

Yukarıdaki kod, denetleyici ve eylem adı geçirerek bir URL oluşturmanın gösterilmiştir. `IUrlHelper` Ayrıca sağlar `Url.RouteUrl` yöntemlerin ailesi. Bu yöntemlere benzer `Url.Action`, ancak bunlar geçerli değerleri kopyalamayın `action` ve `controller` rota değerleri için. Belirli bir yol genellikle URL'yi oluşturmak için kullanılacak rota adı belirtmek için en yaygın kullanımı olan *olmadan* bir denetleyici veya eylem adı belirterek.

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a>HTML URL'ler oluşturulurken

`IHtmlHelper` sağlar `HtmlHelper` yöntemleri `Html.BeginForm` ve `Html.ActionLink` oluşturulacak `<form>` ve `<a>` öğeleri sırasıyla. Bu yöntemleri `Url.Action` bir URL'yi oluşturmak için gereken yöntemini ve benzer bağımsız değişkenleri kabul. `Url.RouteUrl` İçin arkadaşlarımız `HtmlHelper` olan `Html.BeginRouteForm` ve `Html.RouteLink` benzer işlevselliği olan.

TagHelpers URL'ler aracılığıyla oluşturmak `form` TagHelper ve `<a>` TagHelper. Bunların her ikisi de kullanın `IUrlHelper` uygulamaları için. Bkz: [formlarla çalışma](../views/working-with-forms.md) daha fazla bilgi için.

Görünümler içinde `IUrlHelper` aracılığıyla kullanılabilir `Url` yukarıdaki tarafından kapsanmayan tüm geçici URL üretmek için özellik.

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a>' De eylem sonuçları URL'ler oluşturulurken

Yukarıdaki örnekleri kullanarak gösterilmesini `IUrlHelper` bir eylem sonucu bir parçası olarak bir URL'yi oluşturmak için en yaygın kullanımı bir denetleyicide olmakla birlikte bir denetleyicide.

`ControllerBase` Ve `Controller` temel sınıflar, eylem sonuçlarını, başka bir eylem başvurmak için kullanışlı yöntemler sağlar. Tipik bir kullanımı, kullanıcı girişi kabul ettikten sonra yeniden yönlendirme sağlamaktır.

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

Eylem sonuçlarını Fabrika yöntemleri yöntemlere benzer bir desen izleyin `IUrlHelper`.

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>Adanmış geleneksel rotalar için özel durum

Geleneksel yönlendirme adlı rota tanımı özel bir tür kullanabileceğiniz bir *adanmış geleneksel rota*. Aşağıdaki örnekte adlı rota `blog` adanmış geleneksel yoldur.

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Bu yönlendirme tanımları kullanarak `Url.Action("Index", "Home")` URL yolu oluşturur `/` ile `default` yolu, ama neden? Rota değerleri tahmin `{ controller = Home, action = Index }` bir URL'yi oluşturmak için yeterli kullanıyor `blog`, ve sonucu `/blog?action=Index&controller=Home`.

Adanmış geleneksel rotalar kullanan özel bir davranışa yol engeller karşılık gelen bir rota parametresini sahip değilseniz varsayılan değerler "çok greedy" URL üretimi ile. Bu durumda varsayılan değerler `{ controller = Blog, action = Article }`ve hiçbiri `controller` ya da `action` bir rota parametresini görünür. Yönlendirme URL üretimi gerçekleştirdiğinde, sağlanan değerler varsayılan değerlerin eşleşmesi gerekir. URL üretimi kullanarak `blog` başarısız olur değerleri `{ controller = Home, action = Index }` eşleşmeyen `{ controller = Blog, action = Article }`. Yönlendirme ardından gördükleri denemek `default`, hangi başarılı olur.

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>Alanları

[Alanları](areas.md) ilgili işlevleri ayrı yönlendirme-için ad alanı (denetleyici eylemleri) ve klasör yapısını (için görünümler) bir gruba düzenlemek için kullanılan bir MVC özelliğidir. Alanlara kullanarak sağlar - aynı ada sahip birden çok denetleyicilerine sahip bir uygulama farklı sahip oldukları sürece *alanları*. Alanlara kullanarak başka bir rota parametresini ekleyerek yönlendirme amacıyla hiyerarşi oluşturur `area` için `controller` ve `action`. Bu bölümde, yönlendirme - alanları ile nasıl etkileştiğini ele alınacaktır bkz [alanları](areas.md) alanları görünümlerle nasıl kullanıldığı hakkında ayrıntılar için.

Aşağıdaki örnek, varsayılan geleneksel yolu kullanmak için MVC yapılandırır ve bir *alan yolu* adlı bir alan için `Blog`:

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

Bir URL yolu gibi eşleştirirken `/Manage/Users/AddUser`, ilk rota, rota değerleri oluşturacak `{ area = Blog, controller = Users, action = AddUser }`. `area` Rota değeri için varsayılan bir değer tarafından üretilen `area`, aslında tarafından oluşturulan rota `MapAreaRoute` aşağıdakine eşdeğerdir:

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute` Varsayılan değer ve kısıtlamasını kullanarak bir yol oluşturur `area` bu durumda, belirtilen alan adını kullanarak `Blog`. Varsayılan değer yol her zaman üretir sağlar `{ area = Blog, ... }`, değer kısıtlaması gerekli `{ area = Blog, ... }` URL üretmek için.

> [!TIP]
> Geleneksel yönlendirme sipariş bağlıdır. Genel olarak, bunlar olmadan bir alan yolları daha ayrıntılı olarak alanları rotalarla önceki rota tablosunda yerleştirilmelidir.

Rota değerleri, yukarıdaki örneği kullanarak, aşağıdaki eylemi eşleşir:

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

`AreaAttribute` Ne bir denetleyici gösterir olan bir alan bir parçası olarak, bu denetleyici olduğunu olan dediğimiz `Blog` alan. Denetleyicileri olmadan bir `[Area]` öznitelik herhangi bir alanı üyesi olmayan ve olacak **değil** ne zaman eşleşen `area` rota değeri yönlendirme tarafından sağlanır. Aşağıdaki örnekte, yalnızca listelenen ilk denetleyici, rota değerlerini eşleşebilir `{ area = Blog, controller = Users, action = AddUser }`.

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> Her denetleyici ad alanı, bütünlük açısından buraya gösterilir; Aksi takdirde denetleyicileri adlandırma çakışması ve derleme hatasına neden olur. Sınıfı ad alanları MVC'nin yönlendirme üzerinde hiçbir etkisi yoktur.

İlk iki denetleyici alanları üyeleri ve ilgili alan adlarının tarafından sağlandığında yalnızca eşleşen `area` rota değeri. Üçüncü denetleyicisi hiçbir alan ve can yalnızca eşleşme için hiçbir değer üyesi olmayan `area` yönlendirme tarafından sağlanır.

> [!NOTE]
> Eşleştirme bakımından *hiçbir değer*, olmaması `area` değeri aynı olduğu gibi değeri `area` null veya boş dize.

Yol alanı içinde bir eylemi yürütmede zaman değeri `area` olarak kullanılabilir bir *ortam değeri* yönlendirme URL'sini oluşturmak için kullanılacak. Varsayılan olarak alanları hareket yani *Yapışkan* tarafından aşağıdaki örnekte gösterildiği gibi URL üretmek için.

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a>IActionConstraint anlama

> [!NOTE]
> Bu bölüm bir derinlemesine framework iç Ayrıntılar ve MVC yürütülecek bir eylem nasıl seçtiği yöneliktir. Tipik bir uygulaması, özel bir gerekmez `IActionConstraint`

Büyük olasılıkla zaten kullanmış `IActionConstraint` arabirimi ile ilgili bilgi sahibi olmasanız bile. `[HttpGet]` Özniteliği ve benzer `[Http-VERB]` öznitelikleri uygulamak `IActionConstraint` bir eylem yönteminin yürütülmesi sınırlamak için.

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

Varsayılan geleneksel yolu, URL yolu varsayılarak `/Products/Edit` değerleri oluşturur `{ controller = Products, action = Edit }`, hangi eşleşir **hem** burada gösterilen işlemlerin. İçinde `IActionConstraint` terminolojisi dediğimiz bu eylemlerin her ikisi de adayları - değerlendirilir her ikisi de rota verilerini eşleşme olarak işle.

Zaman `HttpGetAttribute` yürütür, Yazar *Edit()* için bir eşleştirme olup *alma* ve bir eşleşme için herhangi bir HTTP fiili değil. `Edit(...)` Eylem tanımlanmış kısıtlamalar yoksa ve bu nedenle herhangi bir HTTP fiili eşleşir. Bu nedenle varsayılarak bir `POST` - yalnızca `Edit(...)` eşleşir. Ancak, bir `GET` hem Eylemler hala - ancak bir eylem ile eşleşebilir bir `IActionConstraint` her zaman değerlendirilir *daha iyi* bir eylem daha. Bu nedenle çünkü `Edit()` sahip `[HttpGet]` daha özel olarak kabul edilir ve her iki eylem durumunda seçilir.

Kavramsal olarak, `IActionConstraint` bir biçimidir *aşırı yükleme*, ancak aynı ada sahip yöntem aşırı yükleme yerine onu aynı URL ile eşleşmek eylemler arasında aşırı yüklemesi. Öznitelik yönlendirme de kullanır `IActionConstraint` ve farklı denetleyicileri eylemlerden her iki kabul adayları neden olabilir.

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a>IActionConstraint uygulama

Uygulamak için en kolay yolu bir `IActionConstraint` türetilen bir sınıf oluşturmak `System.Attribute` ve eylemlerin ve denetleyicilerin üzerinde yerleştirin. MVC otomatik olarak bulur herhangi `IActionConstraint` öznitelik olarak uygulanır. İçin nasıl uygulanacağını metaprogram sağladığından muhtemelen en esnek bir yaklaşım budur ve kısıtlamalar uygulamak için uygulama modeli kullanabilirsiniz.

Aşağıdaki örnekte bir kısıtlama dayalı bir eylem seçtiği bir *ülke kodu* rota verileri. [Tam örneğine Github'dan](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).

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

Uygulamak için sorumlu olduğunuz `Accept` yöntemi ve 'yürütmek için bir sipariş' kısıtlaması için seçme. Bu durumda, `Accept` yöntemi döndürür `true` eylemi bir eşleşme olduğunu göstermek için zaman `country` değer eşleşme yol. Bu farklıdır bir `RouteValueAttribute` öznitelikli olmayan bir eylem için geri dönüş olanak sağlar. Tanımlarsanız, gösteren örnek bir `en-US` eylemi ardından bir ülke kodu gibi `fr-FR` yok daha genel bir denetleyiciye döner `[CountrySpecific(...)]` uygulanır.

`Order` Özelliği, karar *aşama* kısıtlaması bir parçasıdır. Eylemi kısıtlamalarını Çalıştır göre gruplar halinde `Order`. Örneğin, tüm framework'ün HTTP yöntemi öznitelikleri aynı kullanmak sağlanan `Order` böylece aynı aşamadaki çalıştırdığı değeri. İstenen ilkelerinizi uygulamak gerektiği kadar aşama olabilir.

> [!TIP]
> Bir değeri karar vermek üzere `Order` önce HTTP yöntemleri, kısıtlama uygulanması gereken olup olmadığını düşünün. Düşük rakamlı ilk önce çalışır.
