---
title: "Denetleyici eylemleri için yönlendirme"
author: rick-anderson
description: 
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 26250a4d-bf62-4d45-8549-26801cf956e9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/routing
ms.openlocfilehash: d6e230351eb2f4c8549b54d75fd8e345718e6109
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="routing-to-controller-actions"></a>Denetleyici eylemleri için yönlendirme

Tarafından [Ryan Nowak](https://github.com/rynowak) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core MVC kullanan yönlendirme [ara yazılım](../../fundamentals/middleware.md) gelen istekleri URL'lerini eşleşen ve eylemlere eşlemek için. Yollar başlatma kodunu veya öznitelikleri tanımlanır. Yollar URL yollarını eylemler için nasıl eşleştirilmesi gerektiğini açıklar. Yollar yanıtları gönderilen (Bağlantılar) URL'lerini oluşturmak için de kullanılır. 

Öznitelik yönlendirilebilir veya Eylemler işleme, genel yönlendirilir. Bir rota denetleyici veya eylem getirir, yönlendirilmiş özniteliği. Bkz: [yönlendirme karma](#routing-mixed-ref-label) daha fazla bilgi için.

Bu belge, MVC ve Yönlendirme ve nasıl tipik MVC uygulamaları oluşturma arasındaki etkileşimler anlatılmıştır yönlendirme özelliklerini kullanın. Bkz: [yönlendirme](xref:fundamentals/routing) Gelişmiş yönlendirme ile ilgili ayrıntılar için.

## <a name="setting-up-routing-middleware"></a>Ara yazılım yönlendirme ayarlama

İçinde *yapılandırma* yöntemi için benzer bir kod da karşılaşabilirsiniz:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Çağrısı içinde `UseMvc`, `MapRoute` olarak adlandırılan tek bir yol oluşturmak için kullanılan `default` rota. Çoğu MVC uygulamaları için benzer bir şablonla bir yol kullanacak `default` rota.

Rota şablonu `"{controller=Home}/{action=Index}/{id?}"` gibi bir URL yolu eşleştirebilirsiniz `/Products/Details/5` ve rota değerlerini ayıklar `{ controller = Products, action = Details, id = 5 }` yolu tokenizing tarafından. MVC bulmaya çalışır adlı bir denetleyicisi `ProductsController` eylemi çalıştırıp `Details`:

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

Bu örnekte, değeri, model bağlama kullanırsınız Not `id = 5` ayarlamak için `id` parametresi `5` bu eylemi çağrılırken. Bkz: [Model bağlama](../models/model-binding.md) daha fazla ayrıntı için.

Kullanarak `default` yol:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

Rota şablonu:

* `{controller=Home}`tanımlar `Home` varsayılan olarak`controller`

* `{action=Index}`tanımlar `Index` varsayılan olarak`action`

* `{id?}`tanımlar `id` isteğe bağlı olarak

Varsayılan ve isteğe bağlı rota parametrelerinin eşleşen bir URL yolu bulunması gerekmez. Bkz: [rota şablon başvurusu](../../fundamentals/routing.md#route-template-reference) rota şablon söz dizimi ayrıntılı bir açıklaması.

`"{controller=Home}/{action=Index}/{id?}"`URL yolunu eşleştirebilirsiniz `/` ve rota değerleri üretecektir `{ controller = Home, action = Index }`. Değerleri `controller` ve `action` olun varsayılan değerini kullanmak `id` URL yolu ilgili segment yok olduğundan bir değeri oluşturmuyor. MVC kullandığınız bu rota değerlerini seçmek için `HomeController` ve `Index` eylem:

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

Bu denetleyici tanımı ve rota şablonu kullanarak `HomeController.Index` eylem yürütülebilir herhangi aşağıdaki URL yolu için:

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

Kolaylık metodunun `UseMvcWithDefaultRoute`:

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

`UseMvc`ve `UseMvcWithDefaultRoute` bir örnek ekler `RouterMiddleware` ara yazılım ardışık düzenini için. MVC doğrudan ara yazılımı ile etkileşim değil ve yönlendirme isteklerini işlemek için kullanır. MVC örneği ile yolların bağlı `MvcRouteHandler`. Kod içinde `UseMvc` aşağıdakine benzer:

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

`UseMvc`tüm yollar doğrudan tanımlamıyor rota koleksiyonu için bir yer tutucu ekler `attribute` rota. Aşırı `UseMvc(Action<IRouteBuilder>)` kendi yollar eklemenize olanak sağlayan ve ayrıca özniteliği yönlendirmeyi destekler.  `UseMvc`ve tüm alt çeşitleri ekler özniteliği yol için bir yer tutucu - özniteliği yönlendirme her zaman nasıl yapılandırdığınıza bakmaksızın kullanılabilir `UseMvc`. `UseMvcWithDefaultRoute`Varsayılan rota tanımlar ve öznitelik yönlendirmeyi destekler. [Özniteliği yönlendirme](#attribute-routing-ref-label) bölüm özniteliği yönlendirme hakkında daha fazla ayrıntı içerir.

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a>Geleneksel yönlendirme

`default` Yol:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

örneği bir *geleneksel yönlendirme*. Bu stili diyoruz *geleneksel yönlendirme* , çünkü bir *kuralı* URL yolları için:

* İlk yol kesimi Denetleyici adı eşlemeleri

* Eylem adı ikinci eşlenir.

* Üçüncü segment isteğe için kullanılan `id` modeli varlığa eşlemek için kullanılır

Bu kullanarak `default` yol, URL yolunu `/Products/List` eşlendiği `ProductsController.List` eylemi ve `/Blog/Article/17` eşlendiği `BlogController.Article`. Bu eşleme denetleyici ve eylem adları temelinde **yalnızca** ve ad alanları, kaynak dosya konumları veya yöntem parametreleri göre değil.

> [!TIP]
> Geleneksel olan varsayılan yol yönlendirme kullanmak için tanımladığınız her eylem için yeni bir URL düzendeki gündeme gerek kalmadan uygulama hızlı bir şekilde oluşturmanızı sağlar. CRUD stili eylemleri içeren bir uygulama için tutarlılık için URL'leri denetleyicilerinizi arasında sahip olmak, kodunuzu basitleştirerek UI daha öngörülebilir hale yardımcı olabilir.

> [!WARNING]
> `id` İsteğe bağlı olarak eylemlerinizi URL'SİNİN bir parçası sağlanan kimliği olmadan yürütebilir anlamı rota şablonu tarafından tanımlanır. Genellikle ne olursa olacağını `id` URL'den atlanmış onu ayarlanacak emin olan `0` model bağlama tarafından ve hiçbir varlık veritabanı eşleşen bulunabilir sonuç olarak `id == 0`. Öznitelik yönlendirme bazı eylemler için ve başkaları için gerekli kimlik yapmak için hassas bir denetim verebilirsiniz. İsteğe bağlı parametreler belgelere içereceği kurala göre ister `id` olduklarında büyük bir olasılıkla doğru kullanımı görünür.

## <a name="multiple-routes"></a>Birden çok yol

İçinde birden çok yol ekleyebilirsiniz `UseMvc` daha fazla aramaları ekleyerek `MapRoute`. Bunun yapılması, birden çok kuralları tanımlamak ya da belirli bir eylemi gibi ayrılmış geleneksel yollar eklemeniz sağlar:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`blog` Burada yol bir *ayrılmış geleneksel rota*, onu geleneksel yönlendirme sistem kullanır ancak belirli bir eylemi ayrılmış anlamına gelir. Bu yana `controller` ve `action` rota şablonu parametreleri olarak görünmez, yalnızca varsayılan değerleri olabilir ve bu nedenle bu rota için eylem her zaman eşler `BlogController.Article`.

Rota koleksiyonu yollar sıralanır ve eklendikleri sırayla işlenir. Bu örnekte, bunu `blog` rota çalıştı önce `default` rota.

> [!NOTE]
> *Geleneksel yollar ayrılmış* catch tüm rota parametrelerinin gibi sık kullandığınız `{*article}` URL yolunu geri kalan bölümü yakalama için. Bu bir rota 'çok doyumsuz' yapabilirsiniz diğer yollar eşleştirilmesini hedeflenen URL'leri eşleşen anlamına gelir. 'Doyumsuz' yollar bunu çözmek için daha sonra rota tablosunda yerleştirin.

### <a name="fallback"></a>Geri dönüş

İstek işleme bir parçası olarak, MVC, rota değerleri, uygulamanızda bir denetleyici ve eylem bulmak için kullanılabilir doğrular. Rota değerleri bir eylem eşleşmiyorsa sonra rota bir eşleşme olarak kabul edilmez ve sonraki yol denenir. Bu adlı *geri dönüş*, ve geleneksel yollar çakıştığı durumlarda basitleştirmek hedeflenen.

### <a name="disambiguating-actions"></a>Belirsizliği Eylemler

İki eylem Yönlendirme aracılığıyla eşleştiğinde, MVC 'en iyi' adayı seçin veya başka bir özel durum için ayırt etmek gerekir. Örneğin:

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

Bu denetleyici URL yolunu eşleşir iki eylemleri tanımlar `/Products/Edit/17` ve rota verilerini `{ controller = Products, action = Edit, id = 17 }`. MVC denetleyicileri için genel bir desen budur nerede `Edit(int)` bir ürünü düzenlemek için bir form gösterir ve `Edit(int, Product)` gönderilen formu işler. Bu olası MVC seçmek yapmanız `Edit(int, Product)` bir HTTP istek olduğunda `POST` ve `Edit(int)` HTTP fiili olduğunda başka bir şey.

`HttpPostAttribute` ( `[HttpPost]` ) Uygulamasıdır `IActionConstraint` , yalnızca izni verdiği HTTP fiili olduğunda, seçili eylem `POST`. Varlığını bir `IActionConstraint` yapar `Edit(int, Product)` 'daha iyi' eşleşen daha `Edit(int)`, bu nedenle `Edit(int, Product)` ilk olarak denenir.

Yalnızca özel yazma gerekir `IActionConstraint` özel senaryoları, ancak uygulamalarında gibi özniteliklere rolünü anlamak önemlidir `HttpPostAttribute` -benzer öznitelikleri için diğer HTTP fiilleri tanımlanır. Geleneksel yönlendirme parçası olduğunda aynı eylem adı kullanmak eylemler için yaygındır bir `show form -> submit form` iş akışı. Bu desen kolaylık gözden geçirdikten sonra daha belirgin hale gelecek [anlama IActionConstraint](#understanding-iactionconstraint) bölümü.

Birden çok yol eşleşen ve MVC 'en iyi' yolu bulunamıyor, throw bir `AmbiguousActionException`.

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a>Yol adları

Dizeleri `"blog"` ve `"default"` aşağıdaki örneklerde rota adları şunlardır:


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Rota adları, rota mantıksal bir ad verin adlandırılmış rota URL üretmek için kullanılabilir. Yolların sıralama URL nesil karmaşık hale getirebilecek olduğunda bu URL oluşturma büyük ölçüde basitleştirir. Rota adları benzersiz uygulama kapsamında olması gerekir.

Rota adları eşleşen veya istekleri işleme URL üzerinde bir etkisi yok; Bunlar yalnızca URL'si oluşturmak için kullanılır. [Yönlendirme](xref:fundamentals/routing) MVC özgü Yardımcıları URL oluşturma dahil olmak üzere URL oluşturma hakkında ayrıntılı bilgi vardır.

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a>Öznitelik yönlendirme

Öznitelik yönlendirme eylemleri doğrudan rota şablonlarının eşlemek için öznitelikler kümesi kullanır. Aşağıdaki örnekte, `app.UseMvc();` kullanılan `Configure` yöntemi ve hiçbir rota geçirilir. `HomeController` URL'leri hangi varsayılan yol için benzer bir dizi eşleşir `{controller=Home}/{action=Index}/{id?}` eşleşir:

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

`HomeController.Index()` Herhangi bir URL yolu için eylem yürütülür `/`, `/Home`, veya `/Home/Index`.

> [!NOTE]
> Bu örnekte, öznitelik Yönlendirme ve geleneksel yönlendirme anahtar programlama birbirinden vurgular. Öznitelik yönlendirme bir yol belirtmek için daha fazla girdi gerektirir; Geleneksel varsayılan yol yolları daha temellerini işler. Bununla birlikte, öznitelik yönlendirme sağlar (ve gerektirir) hangi rota şablonlarının her işlem için geçerli kesin bir denetim.

Denetleyici adı ve eylem adları yönlendirme özniteliğiyle yürütmek **hiçbir** rol eylem seçilidir. Bu örnek önceki örnekteki gibi aynı URL'ler ile eşleşir.

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
> Rota şablonlarının yukarıdaki rota parametrelerini tanımlayın yok `action`, `area`, ve `controller`. Aslında, bu rota parametrelerinin öznitelik rotaları izin verilmiyor. Rota şablonu zaten bir eylem ile ilişkili olduğundan, bu eylem adı URL'den ayrıştırmak için anlamlı olmayacaktır.

## <a name="attribute-routing-with-httpverb-attributes"></a>HTTP [eylem] özniteliklerle yönlendirme özniteliği

Öznitelik yönlendirme de yapabilirsiniz kullanımı `Http[Verb]` gibi öznitelikleri `HttpPostAttribute`. Tüm bu özniteliklerin bir rota şablonu kabul edebilir. Bu örnek aynı yol şablonu eşleşen iki eylemleri gösterir:

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

Gibi bir URL yolu için `/products` `ProductsApi.ListProducts` HTTP fiili olduğunda eylem yürütülür `GET` ve `ProductsApi.CreateProduct` HTTP fiili olduğunda yürütülecek `POST`. Öznitelik ilk yönlendirme URL rota öznitelikleri tarafından tanımlanan rota şablonlarının dizi eşleşir. Eşleşen bir rota şablonu sonra `IActionConstraint` kısıtlamaları hangi eylemleri yürütülebilecek belirlemek için uygulanır.

> [!TIP]
> Bir REST API oluştururken, kullanmak istediğiniz nadir `[Route(...)]` bir eylem yöntemi. Daha fazla özel kullanmak en iyisidir `Http*Verb*Attributes` API'nizi destekleyen hakkında kesin olarak. REST API'leri istemcileri için belirli mantıksal işlemler ne yolları ve HTTP fiillerini eşleme bilmeniz beklenir.

Öznitelik rotası belirli bir eylem için geçerli olduğundan, rota şablonu tanımının bir parçası gerekli parametreleri yapmak kolaydır. Bu örnekte, `id` URL yolu bir parçası olarak gerekli değildir.

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

`ProductsApi.GetProduct(int)` Eylem, URL yolu gibi yürütülür `/products/3` ancak gibi bir URL yolu için `/products`. Bkz: [yönlendirme](../../fundamentals/routing.md) rota şablonlarının ve ilgili seçenekleri tam bir açıklaması.

## <a name="route-name"></a>Rota adı

Aşağıdaki kodu tanımlayan bir *rota adı* , `Products_List`:

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

Rota adları, belirli bir rotaya bağlı bir URL'yi oluşturmak için kullanılabilir. Rota adları, yönlendirme davranışını eşleşen URL üzerinde hiçbir etkisi ve yalnızca URL üretmek için kullanılır. Rota adları benzersiz uygulama kapsamında olması gerekir.

> [!NOTE]
> Bunu geleneksel ile karşılaştırın *varsayılan yol*, tanımlayan `id` parametre isteğe bağlı olarak (`{id?}`). Tam olarak API'leri belirtmek için bu özelliği izin verme gibi avantaj `/products` ve `/products/5` farklı eylemler için gönderilecek.

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a>Birleştirme yolları

Öznitelik yönlendirme daha az yinelenen yapmak için rota denetleyici özniteliklerinden yola rotası özniteliklerinin ayrı Eylemler ile birleştirilir. Denetleyicide tanımlanan tüm rota şablonlarının eylemlere rota şablonlarının $a. Yol özniteliği denetleyiciye yerleştirmek yapar **tüm** denetleyici eylemlerini kullanmak özniteliği yönlendirme.

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

Bu örnekte URL yolunu `/products` eşleşebilir `ProductsApi.ListProducts`ve URL yolunu `/products/5` eşleşebilir `ProductsApi.GetProduct(int)`. Bu eylemlerin her ikisini de yalnızca HTTP eşleşen `GET` ile donatılmış olduğundan `HttpGetAttribute`.

Rota şablonları ile başlayan bir eyleme uygulanan bir `/` denetleyiciye uygulanan rota şablonlarının birlikte değil. Bu örnek URL yollarını benzer birtakım eşleşen *varsayılan yol*.

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Does not combine, defines the route template ""
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

Tanımsız bir sırayla yürütmek geleneksel yollar aksine özniteliği yönlendirme bir ağaç oluşturur ve tüm yollar aynı anda eşleştirir. Bu olarak davranır-rota girişleri bir ideal sıralamada; yerleştirilmişse en belirli yollar daha genel yolları önce yürütülecek şansına sahip olabilirsiniz.

Örneğin, bir rota gibi `blog/search/{topic}` gibi bir yol daha fazla özel `blog/{*article}`. Mantıksal olarak konuşarak `blog/search/{topic}` rota 'çalışır' ilk olarak, varsayılan olarak, yalnızca duyarlı sıralama olduğundan. Geleneksel yönlendirme kullanarak, geliştirici yollar sıralamada yerleştirmek için sorumludur.

Öznitelik rotaları bir sipariş yapılandırabilir kullanarak `Order` tüm sağlanan çerçevesi rota özniteliklerini özelliği. Yollar, artan göre işlenir tür `Order` özelliği. Varsayılan sıra `0`. Bir rota kullanarak ayarlama `Order = -1` sipariş ayarlamazsanız yolları önce çalışır. Bir rota kullanarak ayarlama `Order = 1` varsayılan rota sıralandıktan sonra çalışır.

> [!TIP]
> Bağlı olarak kaçının `Order`. Ardından, URL alanınızın doğru yönlendirmek için açık sırası değerlerinin gerektiriyorsa, istemcilere de büyük olasılıkla kafa karıştırıcı. Genel özniteliği yönlendirme URL eşleşen doğru yolu seçer. URL üretmek için kullanılan varsayılan sırası çalışmıyorsa, bir geçersiz kılma uygulamaktan daha genellikle daha basit olduğu rota adı kullanılarak `Order` özelliği.

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>Simge rota şablonlarındaki değiştirme ([denetleyicisi], [action] [alanı])

Öznitelik rotaları kolaylık sağlamak için destek *belirteci değiştirme* köşeli ayraçlar içinde bir belirteç kapsayan tarafından (`[`, `]`). Belirteçleri `[action]`, `[area]`, ve `[controller]` eylem adı, alan adı ve Denetleyici adı rota tanımlandığı eylemden değerleri ile değiştirilecek. Bu örnekte açıklamaları açıklandığı gibi eylemleri URL yollarını eşleşebilir:

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

Belirteç değiştirme öznitelik rotaları oluşturmanın son adım olarak gerçekleşir. Yukarıdaki örnekte, aşağıdaki kod ile aynı davranır:

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

Öznitelik rotaları da devralma ile birleştirilebilir. Bu belirteç değiştirme ile birleştirilmiş özellikle güçlüdür.

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPost("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

Belirteç değiştirme öznitelik rotaları tarafından tanımlanan rota adları için de geçerlidir. `[Route("[controller]/[action]", Name="[controller]_[action]")]`Her eylem için bir benzersiz rota adı oluşturur.

Değişmez değer belirteci değiştirme sınırlayıcı eşleşecek şekilde `[` veya `]`, karakteri tekrarlayarak kaçış (`[[` veya `]]`).

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a>Birden çok yol

Aynı eylemi ulaşmak birden çok yol tanımlama yönlendirme desteklediği öznitelik. Davranışını taklit etmek için bu en yaygın kullanımı olan *varsayılan geleneksel yol* aşağıdaki örnekte gösterildiği gibi:

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

Birden çok yol özniteliğine denetleyicisinde her biri her eylem yöntemlerini rotası özniteliklerinin birleştirir toplamaktır.

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

Zaman birden çok yol özniteliğine (uygulayan `IActionConstraint`) bir eylem, her eylem kısıtlaması tanımlanmış özniteliğindeki rota şablonu birleştirir sonra yerleştirilir.

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
> Birden çok yol eylemleri kullanarak güçlü görünebilir, ancak uygulamanızın URL alanı basit ve iyi tanımlanmış durumda tutmak daha iyidir. Birden çok yol yalnızca gerektiğinde, örneğin mevcut istemcileri desteklemek için eylemleri kullanın.

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>Öznitelik yol isteğe bağlı parametreler, varsayılan değerleri ile kısıtlamaları belirtme

Öznitelik rotaları isteğe bağlı parametreler, varsayılan değerleri ile kısıtlamaları belirtmek için geleneksel yollar aynı satır içi söz dizimini destekler.

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

Bkz: [rota şablon başvurusu](../../fundamentals/routing.md#route-template-reference) rota şablon söz dizimi ayrıntılı bir açıklaması.

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>Özel rota öznitelikleri kullanma`IRouteTemplateProvider`

Tüm framework sağlanan rota özniteliklerini ( `[Route(...)]`, `[HttpGet(...)]` , vs.) uygulama `IRouteTemplateProvider` arabirimi. MVC arar denetleyicisi sınıfları ve eylem yöntemleri özniteliklerinde uygulamayı başlatır ve uygulama olanları kullandığında `IRouteTemplateProvider` rota ilk kümesini oluşturmak için.

Uygulayabileceğiniz `IRouteTemplateProvider` kendi rota öznitelikleri tanımlamak için. Her `IRouteTemplateProvider` özel rota şablonuyla tek bir yol tanımlamanıza olanak verir sipariş ve adlandırın:

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

Yukarıdaki örnekte özniteliğinden otomatik olarak ayarlar `Template` için `"api/[controller]"` zaman `[MyApiController]` uygulanır.

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a>Öznitelik rotaları özelleştirmek için uygulama modelini kullanarak

*Uygulama modeli* Başlangıçta tüm yönlendirmek ve eylemleri yürütmek için MVC tarafından kullanılan meta verileri ile oluşturulan bir nesne model. *Uygulama modeli* rota özniteliklerini toplanan verilerin tümünü içerir (aracılığıyla `IRouteTemplateProvider`). Yazabileceğiniz *kuralları* yönlendirme biçimini özelleştirmek için başlangıç sırasında uygulama modeli değiştirmek için. Bu bölümde, uygulama modeli kullanarak yönlendirme özelleştirme basit bir örnek gösterilmiştir.

[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>Yönlendirme karma: vs geleneksel yönlendirmesi özniteliği

MVC uygulamaları geleneksel Yönlendirme ve öznitelik yönlendirme kullanımını karıştırabilirsiniz. Tarayıcılar için HTML sayfalarını sunmadan denetleyicileri için geleneksel rotaları kullanır ve REST API'leri hizmet veren denetleyicileri için yönlendirme öznitelik için genel bir durumdur.

Öznitelik yönlendirilebilir veya Eylemler işleme, genel yönlendirilir. Bir rota denetleyici veya eylem getirir, yönlendirilmiş özniteliği. Öznitelik rotaları tanımlayan eylemlerini geleneksel yolları ve tam tersini erişilemiyor. **Tüm** yol özniteliği denetleyicisinde yönlendirilen denetleyicisi özniteliğinde tüm eylemleri yapar.

> [!NOTE]
> Ne yönlendirme sistemleri iki tür ayıran bir URL eşleşen bir rota şablonu sonra uygulanan işlemidir. Geleneksel yönlendirme içinde eşleşme rota değerleri için eylem ve denetleyici tüm geleneksel yönlendirilmiş eylemleri bir arama tablosu seçmek için kullanılır. Öznitelik akışındaki her bir şablon zaten bir eylem ile ilişkili ve başka hiçbir arama gereklidir.

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a>URL oluşturma

MVC uygulamaları 's yönlendirme URL nesil özellikleri Eylemler URL bağlantılarını oluşturmak için kullanabilirsiniz. URL'ler oluşturulurken cmdlet'e kod URL'ler, kodunuzu daha sağlam ve korunabilir hale ortadan kaldırır. Bu bölümde, MVC tarafından sağlanan URL nesil özelliklere odaklanır ve sadece URL nesil nasıl çalıştığını temel kavramları kapsar. Bkz: [yönlendirme](../../fundamentals/routing.md) URL nesil ayrıntılı bir açıklaması.

`IUrlHelper` Arabirimidir MVC arasındaki URL üretmek için yönlendirme altyapısını temel parçası. Bir örneğini bulabilirsiniz `IUrlHelper` aracılığıyla kullanılabilen `Url` denetleyicileri, görünümleri ve görünüm bileşenleri bir özellik.

Bu örnekte, `IUrlHelper` arabirimi aracılığıyla kullanılır `Controller.Url` başka bir eylem URL'si oluşturmak için özellik.

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

Uygulama geleneksel varsayılan kullanıyorsa, yol, değeri `url` değişkeni URL yolu dizesi olacaktır `/UrlGeneration/Destination`. Bu URL yolu geçerli istek (ortam değerleri), rota değerlerini birleştirerek yönlendirme tarafından geçirilen değerlerle oluşturulur `Url.Action` ve bu değerler rota şablonuna değiştirerek:

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

Rota şablonu her yol parametresinde ve ortam değerleri ile eşleşen adları yerine kendi değere sahiptir. Varsa veya isteğe bağlı ise atlanması durumunda varsayılan bir değer bir değere sahip olmayan bir rota parametresini kullanabilirsiniz (olarak durumunda `id` Bu örnekte). Tüm gerekli rota parametresini karşılık gelen bir değer yoksa, URL oluşturma başarısız olur. Bir rota için URL oluşturma başarısız olursa, sonraki yol tüm yollar çalıştı veya herhangi bir eşleşme kadar denenir.

Örnek `Url.Action` kavramları farklı ancak geleneksel yönlendirme, ancak URL nesil works benzer şekilde yönlendirmesi özniteliği, yukarıdaki varsayar. Geleneksel yönlendirme ile rota değerleri bir şablon ve rota değerleri için genişletmek için kullanılan `controller` ve `action` genellikle görünür bu şablonda - yönlendirerek eşleşen URL'leri uygun olduğundan işlediğine bir *kuralı*. Öznitelik yönlendirmeye, rota değerleri için `controller` ve `action` görünmesi şablonda - bunlar bunun yerine kullanılan şablonun kullanılacak aramak için izin verilmiyor.

Bu örnekte, öznitelik yönlendirme kullanır:

[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC tüm yönlendirilen özniteliği eylemlerin bir arama tablosu oluşturur ve eşleşir `controller` ve `action` değerleri URL üretmek için kullanılacak rota şablonunu seçin. Yukarıdaki örnekteki `custom/url/to/destination` oluşturulur.

### <a name="generating-urls-by-action-name"></a>Eylem adına göre URL'ler oluşturulurken

`Url.Action` (`IUrlHelper` . `Action`) ve ilişkili tüm tüm temel ne bir denetleyici adı ve eylem adı belirterek bağlantıyı yaptığınız belirtmek istediğiniz bu fikir aşırı.

> [!NOTE]
> Kullanırken `Url.Action`, geçerli rota değerleri için `controller` ve `action` sizin için - değerini belirtilen `controller` ve `action` her ikisi de parçası olan *ortam değerleri* **ve** *değerleri*. Yöntem `Url.Action`, her zaman geçerli değerlerini kullanır `action` ve `controller` ve geçerli eylem yönlendiren bir URL yolu oluşturur.

Yönlendirme bir URL oluşturulurken sağlamadı bilgilerinde doldurmak için ortam değerleri değerleri kullanmayı dener. Gibi bir yol kullanarak `{a}/{b}/{c}/{d}` ve ortam değerleri `{ a = Alice, b = Bob, c = Carol, d = David }`, Yönlendirme ek değer içermeyen bir URL'yi oluşturmak için yeterli bilgiye sahip - tüm rota beri parametreler bir değere sahip. Değer eklediyseniz `{ d = Donovan }`, değer `{ d = David }` yok sayılırdı, ve oluşturulan URL yolu olacağını `Alice/Bob/Carol/Donovan`.

> [!WARNING]
> URL yollarını hiyerarşik. Eğer yukarıdaki örnekte değer katan `{ c = Cheryl }`, değerlerin her ikisi de `{ c = Carol, d = David }` göz ardı. İçin bir değer artık bu durumda sahibiz `d` ve URL oluşturma başarısız olur. İstenen değeri belirtmek zorunda kalmaz `c` ve `d`.  Bu sorun varsayılan yol isabet bekleyebilirsiniz (`{controller}/{action}/{id?}`)- ancak uygulama olarak bu davranışı nadiren karşılaşır `Url.Action` her zaman açık olarak belirleyeceksiniz bir `controller` ve `action` değeri.

Uzun aşırı `Url.Action` de ek bir ele *rota değerleri* dışında rota parametrelerinin değerlerini sağlamak için nesne `controller` ve `action`. Bu ile kullanılan en yaygın olarak görürsünüz `id` gibi `Url.Action("Buy", "Products", new { id = 17 })`. Kural tarafından *rota değerleri* nesne, genellikle anonim türünde bir nesne ancak aynı zamanda olabilir bir `IDictionary<>` veya *düz eski .NET nesne*. Rota parametrelerine eşleşmeyen herhangi bir ek rota değeri sorgu dizesinde yerleştirilir.

[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> Mutlak bir URL oluşturmak için kabul eden bir aşırı yüklemesini kullanın bir `protocol`:`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a>Rota tarafından URL'ler oluşturulurken

Yukarıdaki kod, denetleyici ve eylem adı geçirerek bir URL oluşturmanın gösterilmektedir. `IUrlHelper`Ayrıca sağlar `Url.RouteUrl` yöntemlerin ailesi. Bu yöntemlere benzer `Url.Action`, ancak geçerli değerlerini kopyalamayın `action` ve `controller` rota değerleri için. Belirli bir yolu URL'yi genellikle oluşturmak için kullanılacak rota adı belirtmek için en yaygın kullanımdır *olmadan* bir denetleyici veya eylem adı belirterek.

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a>HTML dosyasındaki URL'ler oluşturulurken

`IHtmlHelper`sağlar `HtmlHelper` yöntemleri `Html.BeginForm` ve `Html.ActionLink` oluşturmak için `<form>` ve `<a>` öğeleri sırasıyla. Bu yöntemleri kullanmak `Url.Action` bir URL üretmek için yöntemi ve bunlar benzer bağımsız değişkenlerini kabul eder. `Url.RouteUrl` İçin arkadaşlarımız `HtmlHelper` olan `Html.BeginRouteForm` ve `Html.RouteLink` benzer işlevlere sahiptir.

TagHelpers oluşturmak URL'ler aracılığıyla `form` TagHelper ve `<a>` TagHelper. Bunların her ikisi de kullanmak `IUrlHelper` kendi uygulama. Bkz: [formlarla çalışmak](../views/working-with-forms.md) daha fazla bilgi için.

Görünümler içinde `IUrlHelper` aracılığıyla kullanılabilir `Url` özelliği yukarıdaki tarafından kapsanmayan tüm geçici URL üretmek için.

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a>Eylem sonuçlarında URL'ler oluşturulurken

Yukarıdaki örneklerde kullanarak göstermiştir `IUrlHelper` bir denetleyicide en yaygın kullanımı bir eylem sonucu bir parçası olarak bir URL üretmek için olsa da bir denetleyicide.

`ControllerBase` Ve `Controller` kullanışlı yöntemler başka bir eylem başvuru eylem sonuçlarına yönelik temel sınıflar sağlar. Bir genel kullanıcı girişi kabul ettikten sonra yeniden yönlendirmek için kullanımdır.

```csharp
public Task<IActionResult> Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
}
```

Eylem sonuçlarını Fabrika yöntemleri yöntemlere benzer bir desen izleyin `IUrlHelper`.

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>Ayrılmış geleneksel rotalar için özel durum

Geleneksel yönlendirme route tanımını adlı özel bir tür kullanabileceğiniz bir *ayrılmış geleneksel rota*. Aşağıdaki örnekte, yol adında `blog` adanmış bir geleneksel yoldur.

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Bu rota tanımları kullanarak `Url.Action("Index", "Home")` URL yolu oluşturur `/` ile `default` neden ancak rota? Rota değerleri tahmin `{ controller = Home, action = Index }` bir URL'yi oluşturmak için yeterli kullanıyor `blog`, ve sonucu olacağını `/blog?action=Index&controller=Home`.

Özel bir davranış rota engeller karşılık gelen bir yol parametresi yok varsayılan değerlerin Bel ayrılmış geleneksel yolları "çok hızlı" URL oluşturma ile. Bu durumda varsayılan değerler `{ controller = Blog, action = Article }`ve hiçbiri `controller` ya da `action` bir rota parametresi olarak görünür. Yönlendirme URL nesil gerçekleştirdiğinde, sağlanan değerler varsayılan değerler eşleşmelidir. Nesil URL'yi kullanarak `blog` başarısız olur değerleri `{ controller = Home, action = Index }` eşleşmeyen `{ controller = Blog, action = Article }`. Yönlendirme sonra kalan tekrar denemek için `default`, hangi başarılı olur.

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>Alanları

[Alanları](areas.md) ilgili işlevselliği, ayrı yönlendirme-için ad alanı (denetleyici eylemleri) ve klasör yapısı (için görünümler) olarak bir gruba düzenlemek için kullanılan bir MVC özelliğidir. Alanları kullanarak sağlar - aynı ada sahip birden çok denetleyicilerine sahip bir uygulama farklı olduğu sürece *alanları*. Alanları kullanarak başka bir rota parametresini ekleyerek yönlendirme amacıyla bir hiyerarşi oluşturur `area` için `controller` ve `action`. Bu bölümde alanlarla - yönlendirme nasıl etkileşim ele alınacaktır bkz [alanları](areas.md) alanları görünümlerle nasıl kullanıldığı hakkında ayrıntılar için.

Aşağıdaki örnek MVC varsayılan geleneksel yol kullanacak şekilde yapılandırır ve bir *alanı rota* adlı bir alan için `Blog`:

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

Bir URL yolu gibi eşleştirirken `/Manage/Users/AddUser`, ilk rota rota değerlerini üretecektir `{ area = Blog, controller = Users, action = AddUser }`. `area` Rota değeri için varsayılan bir değer tarafından oluşturulur `area`, aslında tarafından oluşturulan rota `MapAreaRoute` şuna eşdeğerdir:

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute`Varsayılan değer hem için kısıtlamayı kullanan bir yol oluşturur `area` sağlanan alan adı, bu durumda kullanarak `Blog`. Varsayılan değer rota her zaman üretir sağlar `{ area = Blog, ... }`, değer kısıtlaması gerektirir `{ area = Blog, ... }` URL üretmek için.

> [!TIP]
> Geleneksel yönlendirme sipariş bağlıdır. Genel olarak, bir alan olmadan yolları daha ayrıntılı olarak alanları yollar önceki rota tablosunda yerleştirilmelidir.

Yukarıdaki örneği kullanarak, rota değerlerini aşağıdaki eylemi eşleşir:

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

`AreaAttribute` Ne bir denetleyici gösterir olan bir alan bir parçası olarak, bu denetleyicisi olduğundan emin olan dediğimiz `Blog` alanı. Denetleyicileri olmadan bir `[Area]` özniteliği herhangi bir alan üyesi olmayan ve olacak **değil** ne zaman eşleşen `area` rota değeri yönlendirme tarafından sağlanır. Aşağıdaki örnekte, rota değerleri yalnızca listelenen ilk denetleyicisi eşleştirebilirsiniz `{ area = Blog, controller = Users, action = AddUser }`.

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> Her denetleyici ad alanı bütünlük açısından buraya gösterilen - çakışan ve derleme hatasına neden adlandırma denetleyicileri Aksi durumda olurdu. Sınıf ad alanları MVC'ın yönlendirme üzerinde hiçbir etkisi yoktur.

İlk iki denetleyicileri alanları üyeleri olan ve ilgili alan adlarının tarafından sağlandığında yalnızca eşleşen `area` rota değeri. Üçüncü denetleyicisi hiçbir alan ve can yalnızca eşleşme için hiçbir değer olduğunda üyesi olmayan `area` yönlendirme tarafından sağlanır.

> [!NOTE]
> Eşleşen bakımından *hiçbir değer*, yokluğu `area` değerdir aynı gibi değeri `area` null ya da boş dize.

Bir alanı içinde bir eylemi yürütürken rota değeri için `area` olarak kullanılabilecek bir *ortam değeri* URL üretmek için kullanılacak yönlendirme. Varsayılan olarak alanları hareket yani *Yapışkan* aşağıdaki örneği tarafından gösterildiği gibi URL üretmek için.

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a>Anlama IActionConstraint

> [!NOTE]
> Bu bölüm, derin Dalış framework iç Ayrıntılar ve MVC yürütmek için bir eylem nasıl seçtiği değildir. Özel bir genel uygulama gerekmez`IActionConstraint`

Büyük olasılıkla zaten kullanmış `IActionConstraint` arabirimiyle hakkında bilgi sahibi olmadığınız halde. `[HttpGet]` Özniteliği ve benzer `[Http-VERB]` öznitelikleri uygulamak `IActionConstraint` bir eylem yönteminin yürütülmesi sınırlamak için.

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

Varsayılan geleneksel yol, URL yolunu varsayılarak `/Products/Edit` değerleri oluşturur `{ controller = Products, action = Edit }`, hangi eşleşir **her ikisi de** burada gösterilen eylem. İçinde `IActionConstraint` dediğimiz bu eylemlerin her ikisini de adayları - kabul edilen her ikisi de rota verilerini eşleşecek şekilde terminolojisi.

Zaman `HttpGetAttribute` yürütür, sorar *Edit()* bir eşleşen *almak* ve başka bir HTTP fiili yönelik bir eşleşme değil. `Edit(...)` Eylem tanımlı kısıtlamalar yok ve bu nedenle herhangi bir HTTP fiili ile eşleşir. Bu nedenle varsayarak bir `POST` - yalnızca `Edit(...)` eşleşir. Ancak, bir `GET` hem Eylemler hala - ancak, bir eylem ile eşleşebilir bir `IActionConstraint` her zaman kabul *daha iyi* bir eylem daha. Bu nedenle çünkü `Edit()` sahip `[HttpGet]` daha belirgin olarak kabul edilir ve her iki Eylemler eşleşebilmesi durumunda seçilir.

Kavramsal olarak, `IActionConstraint` biçimidir *aşırı yüklemesi*, ancak aşırı yükleme yöntemleri aynı ada sahip yerine onu aynı URL'ye eşleşen eylemler arasında aşırı yüklemesi. Öznitelik yönlendirme de kullanır `IActionConstraint` ve farklı denetleyicileri eylemlerden her iki kabul adayları neden olabilir.

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a>IActionConstraint uygulama

En basit yolu uygulamak için bir `IActionConstraint` türetilmiş bir sınıf oluşturmak için `System.Attribute` ve eylemlerin ve denetleyicilerin üzerinde yerleştirin. MVC otomatik olarak bulmak herhangi `IActionConstraint` öznitelikleri olarak uygulanır. Uygulama modeli kısıtlamaları uygulamak için kullanabileceğiniz ve nasıl uygulandığını metaprogram izin verdiği kadar bu büyük olasılıkla en esnek bir yaklaşımdır.

Aşağıdaki örnekte bir kısıtlama dayalı bir eylem seçtiği bir *ülke kodu* rota verileri. [Github'da tam örnek](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).

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

Uygulamak için sorumlu `Accept` yöntemi ve 'yürütmek için sipariş' kısıtlaması için seçme. Bu durumda, `Accept` yöntemi döndürür `true` eylemi bir eşleşme olduğunu belirtmek için zaman `country` değeri eşleşmeleri rota. Bu farklıdır bir `RouteValueAttribute` öznitelikli olmayan bir eylem için geri dönüş olanak sağlar. Tanımlarsanız, gösteren örnek bir `en-US` eylemi ardından bir ülke kodu gibi `fr-FR` sahip olmayan daha genel bir denetleyiciye döner `[CountrySpecific(...)]` uygulanır.

`Order` Özelliği, karar *aşama* kısıtlaması bir parçasıdır. Eylem kısıtlamaları çalıştırmak göre gruplarındaki `Order`. Örneğin, tüm framework'ün HTTP yöntem öznitelikleri kullanmak aynı sağlanan `Order` aynı aşamasında çalışabilmesi değeri. İstenen ilkelerinizi uygulamak gerektiği kadar aşama olabilir.

> [!TIP]
> İçin bir değer karar vermek için `Order` olsun veya olmasın, kısıtlama HTTP yöntemleri önce uygulanması gereken düşünün. Düşük numaraların ilk çalıştırın.
