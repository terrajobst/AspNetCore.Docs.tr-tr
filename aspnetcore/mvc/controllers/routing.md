---
title: ASP.NET core'da denetleyici eylemlerine yönlendirme
author: rick-anderson
description: Ara yazılım yönlendirme ASP.NET Core MVC URL'leri gelen isteklerin aynı ve onları eylemleri eşlemek için nasıl kullandığını öğrenin.
ms.author: riande
ms.date: 01/24/2019
uid: mvc/controllers/routing
ms.openlocfilehash: f5104bc53581a41fa8c25d8c67e08e038c275391
ms.sourcegitcommit: c6db8b14521814f1f7e528d7aa06e474e4c04a1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2019
ms.locfileid: "55065015"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a><span data-ttu-id="abd6b-103">ASP.NET core'da denetleyici eylemlerine yönlendirme</span><span class="sxs-lookup"><span data-stu-id="abd6b-103">Routing to controller actions in ASP.NET Core</span></span>

<span data-ttu-id="abd6b-104">Tarafından [Ryan Nowak](https://github.com/rynowak) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="abd6b-104">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="abd6b-105">ASP.NET Core MVC yönlendirme kullanan [ara yazılım](xref:fundamentals/middleware/index) URL'leri gelen isteklerin aynı ve eylemlere eşleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="abd6b-105">ASP.NET Core MVC uses the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="abd6b-106">Yollar, başlatma kodu veya öznitelikleri tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-106">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="abd6b-107">Yollar, URL yolu için Eylemler nasıl eşleştirilmesi gerektiğini açıklar.</span><span class="sxs-lookup"><span data-stu-id="abd6b-107">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="abd6b-108">Yolları (için bağlantılar) gönderilen yanıtlarını URL'lerini oluşturmak için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-108">Routes are also used to generate URLs (for links) sent out in responses.</span></span>

<span data-ttu-id="abd6b-109">Eylemler genel yönlendirilir veya öznitelik yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-109">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="abd6b-110">Bir rota denetleyici veya eylem getirir, yönlendirilmiş özniteliği.</span><span class="sxs-lookup"><span data-stu-id="abd6b-110">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="abd6b-111">Bkz: [yönlendirme karma](#routing-mixed-ref-label) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="abd6b-111">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="abd6b-112">Bu belge, MVC ve Yönlendirme ve nasıl tipik MVC uygulamaları oluşturma arasındaki etkileşimler açıklayacak yönlendirme özelliklerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="abd6b-112">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="abd6b-113">Bkz: [yönlendirme](xref:fundamentals/routing) Gelişmiş yönlendirme hakkında bilgi.</span><span class="sxs-lookup"><span data-stu-id="abd6b-113">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="abd6b-114">Ara yazılım yönlendirme ayarlama</span><span class="sxs-lookup"><span data-stu-id="abd6b-114">Setting up Routing Middleware</span></span>

<span data-ttu-id="abd6b-115">İçinde *yapılandırma* yöntemi için benzer bir kod ile karşılaşabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="abd6b-115">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="abd6b-116">Çağrısı içinde `UseMvc`, `MapRoute` biz olarak başvuracağınız tek bir yol oluşturmak için kullanılan `default` rota.</span><span class="sxs-lookup"><span data-stu-id="abd6b-116">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="abd6b-117">Çoğu MVC uygulamaları için benzer bir şablonla bir yol kullanır `default` rota.</span><span class="sxs-lookup"><span data-stu-id="abd6b-117">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="abd6b-118">Rota şablonu `"{controller=Home}/{action=Index}/{id?}"` gibi bir URL yolu eşleşebilir `/Products/Details/5` ve rota değerlerini ayıklar `{ controller = Products, action = Details, id = 5 }` yolu belirteç oluşturma tarafından.</span><span class="sxs-lookup"><span data-stu-id="abd6b-118">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="abd6b-119">MVC adlı bir denetleyici bulmaya çalışır `ProductsController` ve çalıştırma eylemi `Details`:</span><span class="sxs-lookup"><span data-stu-id="abd6b-119">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="abd6b-120">Bu örnekte, model bağlama değerini kullanırsınız Not `id = 5` ayarlanacak `id` parametresi `5` bu eylemi çağrılırken.</span><span class="sxs-lookup"><span data-stu-id="abd6b-120">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="abd6b-121">Bkz: [Model bağlama](../models/model-binding.md) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="abd6b-121">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="abd6b-122">Kullanarak `default` yolu:</span><span class="sxs-lookup"><span data-stu-id="abd6b-122">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="abd6b-123">Rota şablonu:</span><span class="sxs-lookup"><span data-stu-id="abd6b-123">The route template:</span></span>

* <span data-ttu-id="abd6b-124">`{controller=Home}` tanımlar `Home` varsayılan olarak `controller`</span><span class="sxs-lookup"><span data-stu-id="abd6b-124">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="abd6b-125">`{action=Index}` tanımlar `Index` varsayılan olarak `action`</span><span class="sxs-lookup"><span data-stu-id="abd6b-125">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="abd6b-126">`{id?}` tanımlar `id` isteğe bağlı olarak</span><span class="sxs-lookup"><span data-stu-id="abd6b-126">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="abd6b-127">Varsayılan ve isteğe bağlı yol parametreleri, URL yolu için bir eşleşme bulunması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="abd6b-127">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="abd6b-128">Bkz: [rota şablonu başvurusu](../../fundamentals/routing.md#route-template-reference) rota şablonu söz dizimi ayrıntılı bir açıklaması.</span><span class="sxs-lookup"><span data-stu-id="abd6b-128">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="abd6b-129">`"{controller=Home}/{action=Index}/{id?}"` URL yolu eşleşebilir `/` ve rota değerleri oluşturacak `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-129">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="abd6b-130">Değerleri `controller` ve `action` yapmak için varsayılan değerleri kullanmak `id` URL yolunda yok ilgili segment olduğundan değer üretemez.</span><span class="sxs-lookup"><span data-stu-id="abd6b-130">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="abd6b-131">MVC kullandığınız bu rota değerlerini seçmek için `HomeController` ve `Index` eylem:</span><span class="sxs-lookup"><span data-stu-id="abd6b-131">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="abd6b-132">Bu denetleyici tanımı ve rota şablonu kullanarak `HomeController.Index` eylem yürütülebilir herhangi biri, aşağıdaki URL yolu için:</span><span class="sxs-lookup"><span data-stu-id="abd6b-132">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="abd6b-133">Kolaylık yöntemi `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="abd6b-133">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="abd6b-134">Değiştirmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="abd6b-134">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="abd6b-135">`UseMvc` ve `UseMvcWithDefaultRoute` bir örneğini ekleme `RouterMiddleware` ara yazılım ardışık düzenini için.</span><span class="sxs-lookup"><span data-stu-id="abd6b-135">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="abd6b-136">MVC doğrudan Ara yazılımla etkileşim kurmaz ve yönlendirme isteklerini işlemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-136">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="abd6b-137">MVC yolları örneği üzerinden bağlı olduğu `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-137">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="abd6b-138">Kod içinde `UseMvc` aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="abd6b-138">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="abd6b-139">`UseMvc` tüm rotalar doğrudan tanımlamıyor rota koleksiyonu için bir yer tutucu ekler `attribute` rota.</span><span class="sxs-lookup"><span data-stu-id="abd6b-139">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="abd6b-140">Aşırı yükleme `UseMvc(Action<IRouteBuilder>)` kendi yönlendirmelerinizi eklemenize olanak sağlayan ve de öznitelik Yönlendirme'yi destekler.</span><span class="sxs-lookup"><span data-stu-id="abd6b-140">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="abd6b-141">`UseMvc` ve tüm türevlerini ekler özniteliği yol için bir yer tutucu - öznitelik yönlendirme her zaman nasıl yapılandırdığınıza bakmaksızın kullanılabilir `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-141">`UseMvc` and all of its variations adds a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="abd6b-142">`UseMvcWithDefaultRoute` Varsayılan bir yol tanımlar ve öznitelik yönlendirme destekler.</span><span class="sxs-lookup"><span data-stu-id="abd6b-142">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="abd6b-143">[Öznitelik yönlendirme](#attribute-routing-ref-label) bölüm öznitelik yönlendirme hakkında daha fazla ayrıntı içerir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-143">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="abd6b-144">Geleneksel yönlendirme</span><span class="sxs-lookup"><span data-stu-id="abd6b-144">Conventional routing</span></span>

<span data-ttu-id="abd6b-145">`default` Yolu:</span><span class="sxs-lookup"><span data-stu-id="abd6b-145">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="abd6b-146">bir örneğini bir *geleneksel yönlendirme*.</span><span class="sxs-lookup"><span data-stu-id="abd6b-146">is an example of a *conventional routing*.</span></span> <span data-ttu-id="abd6b-147">Bu stil diyoruz *geleneksel yönlendirme* , çünkü bir *kuralı* URL yolu için:</span><span class="sxs-lookup"><span data-stu-id="abd6b-147">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="abd6b-148">İlk yol kesimi denetleyici adıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-148">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="abd6b-149">Eylem adı ikinci eşlenir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-149">the second maps to the action name.</span></span>

* <span data-ttu-id="abd6b-150">Üçüncü kesim için isteğe bağlı olarak kullanılan `id` modeli varlığa eşlemek için kullanılır</span><span class="sxs-lookup"><span data-stu-id="abd6b-150">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="abd6b-151">Bunu kullanarak `default` rota, URL yolu `/Products/List` eşlendiği `ProductsController.List` eylem ve `/Blog/Article/17` eşlendiği `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-151">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="abd6b-152">Bu eşleme denetleyici ve eylem adları bağlıdır **yalnızca** ve ad alanları, kaynak dosya konumlarının veya yöntem parametreleri göre değil.</span><span class="sxs-lookup"><span data-stu-id="abd6b-152">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="abd6b-153">Geleneksel olan varsayılan yol yönlendirme kullanarak tanımladığınız her eylem için yeni bir URL deseni ile gündeme gerek kalmadan hızla uygulama oluşturmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="abd6b-153">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="abd6b-154">CRUD stili eylemleri ile bir uygulama için tutarlılık için URL'leri denetleyicilerinizi arasında olması, kodunuzu basitleştirerek ve kullanıcı Arabirimi daha öngörülebilir hale yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-154">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="abd6b-155">`id` İsteğe bağlı olarak eylemlerinizi URL'SİNİN bir parçası belirtilen kimliği olmadan yürütebilir anlamına gelir, bir rota şablonu tarafından tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-155">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="abd6b-156">Genellikle ne olacağını `id` URL'den atlanırsa, ayarlanacak emin olan `0` model bağlama ve varlık içinde eşleşen veritabanı bulunabilir sonucunda `id == 0`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-156">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="abd6b-157">Öznitelik yönlendirme, bazı eylemler için ve başkaları için gerekli kimliği olmak için ayrıntılı denetim verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="abd6b-157">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="abd6b-158">İsteğe bağlı parametreler belgeleri içerecektir kurala göre ister `id` zaman bunlar büyük olasılıkla doğru kullanımı görünür.</span><span class="sxs-lookup"><span data-stu-id="abd6b-158">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="abd6b-159">Birden fazla rota</span><span class="sxs-lookup"><span data-stu-id="abd6b-159">Multiple routes</span></span>

<span data-ttu-id="abd6b-160">İçinde birden çok yol ekleyebilirsiniz `UseMvc` daha fazla çağrıları ekleyerek `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-160">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="abd6b-161">Bunun yapılması, birden çok kuralları tanımlamak için veya belirli bir eylem için ayrılmış gibi geleneksel yollar eklemeniz sağlar:</span><span class="sxs-lookup"><span data-stu-id="abd6b-161">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="abd6b-162">`blog` Yolu Buraya bir *adanmış geleneksel rota*, bu geleneksel yönlendirme sistem kullanır ancak belirli bir eylem için ayrılmış anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-162">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="abd6b-163">Bu yana `controller` ve `action` yol şablonunda parametreler olarak görünmez, yalnızca varsayılan değerleri olabilir ve bu nedenle bu rota için eylem her zaman eşleyecektir `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-163">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="abd6b-164">Rota koleksiyonu yolları sıralanır ve bunların eklenen sırayla işlenir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-164">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="abd6b-165">Bu örnekte, bu nedenle `blog` rota çalıştığınız önce `default` rota.</span><span class="sxs-lookup"><span data-stu-id="abd6b-165">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="abd6b-166">*Geleneksel rotaları dedicated* tümünü rota parametrelerinin gibi sık kullandığınız `{*article}` URL yolu kalan kısmı yakalamak için.</span><span class="sxs-lookup"><span data-stu-id="abd6b-166">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="abd6b-167">Bu bir yol 'çok doyumsuz' yapabilirsiniz diğer yollar eşlenmesi yönelik URL'ler eşleştiğini anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-167">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="abd6b-168">'Doyumsuz' yolları, bunu çözmek için daha sonra yolu tabloya yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="abd6b-168">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="abd6b-169">Geri dönüş</span><span class="sxs-lookup"><span data-stu-id="abd6b-169">Fallback</span></span>

<span data-ttu-id="abd6b-170">İstek işleme bir parçası olarak, MVC, rota değerleri, uygulamanızda bir denetleyici ve eylem bulmak için kullanılabilir doğrular.</span><span class="sxs-lookup"><span data-stu-id="abd6b-170">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="abd6b-171">Rota değerlerini bir eylem eşleşmezse ardından yol bir eşleşme olarak değil ve sonraki yol denenir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-171">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="abd6b-172">Bu adlandırılır *geri dönüş*, ve geleneksel yollar çakıştığı çalışmaları basitleştirmek kullanılmaya.</span><span class="sxs-lookup"><span data-stu-id="abd6b-172">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="abd6b-173">Belirsizliğini eylemleri</span><span class="sxs-lookup"><span data-stu-id="abd6b-173">Disambiguating actions</span></span>

<span data-ttu-id="abd6b-174">İki eylem yönlendirme ile eşleşiyorsa, MVC 'en iyi' adayı seçin veya başka bir özel durum için ayırt etmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-174">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="abd6b-175">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="abd6b-175">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="abd6b-176">Bu denetleyici URL yolu BC iki eylem tanımlar `/Products/Edit/17` ve rota verilerini `{ controller = Products, action = Edit, id = 17 }`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-176">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="abd6b-177">MVC denetleyicileri için genel bir desen olmasıdır burada `Edit(int)` bir ürün düzenlemek için bir form gösterir ve `Edit(int, Product)` gönderilen bir formu işler.</span><span class="sxs-lookup"><span data-stu-id="abd6b-177">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="abd6b-178">Bunu mümkün hale getirmek için MVC seçin gerekir `Edit(int, Product)` bir HTTP istek olduğunda `POST` ve `Edit(int)` HTTP fiili olduğunda başka bir şey.</span><span class="sxs-lookup"><span data-stu-id="abd6b-178">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="abd6b-179">`HttpPostAttribute` ( `[HttpPost]` ) Uygulamasıdır `IActionConstraint` yalnızca izin veren HTTP fiili olduğunda, seçili eylem `POST`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-179">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="abd6b-180">Varlığı bir `IActionConstraint` yapar `Edit(int, Product)` 'daha iyi' eşleşen daha `Edit(int)`, bu nedenle `Edit(int, Product)` ilk kez yeniden denenecek.</span><span class="sxs-lookup"><span data-stu-id="abd6b-180">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="abd6b-181">Yalnızca özel yazmanız gerekecektir `IActionConstraint` özel senaryoları, ancak uygulamalar gibi öznitelikleri rolünü anlamak önemlidir `HttpPostAttribute` -benzer öznitelikleri için diğer HTTP fiilleri tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-181">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="abd6b-182">Geleneksel yönlendirme parçası olduklarında aynı eylem adı kullanacak şekilde eylemler için yaygındır bir `show form -> submit form` iş akışı.</span><span class="sxs-lookup"><span data-stu-id="abd6b-182">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="abd6b-183">Bu düzen uygun gözden geçirdikten sonra daha belirgin hale gelir [anlama IActionConstraint](#understanding-iactionconstraint) bölümü.</span><span class="sxs-lookup"><span data-stu-id="abd6b-183">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="abd6b-184">Birden çok yol eşleşen ve MVC 'en iyi' yolu bulunamıyor, throw bir `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-184">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="abd6b-185">Rota adları</span><span class="sxs-lookup"><span data-stu-id="abd6b-185">Route names</span></span>

<span data-ttu-id="abd6b-186">Dizeleri `"blog"` ve `"default"` aşağıdaki örneklerde, rota adlarıdır:</span><span class="sxs-lookup"><span data-stu-id="abd6b-186">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="abd6b-187">Böylece adlı rota URL üretmek için kullanılan rota adlarını rota mantıksal bir ad verin.</span><span class="sxs-lookup"><span data-stu-id="abd6b-187">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="abd6b-188">URL üretimi karmaşık yollarını sıralama yapabilir, bu URL oluşturmayı büyük ölçüde kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-188">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="abd6b-189">Rota adları, uygulama genelinde benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-189">Route names must be unique application-wide.</span></span>

<span data-ttu-id="abd6b-190">Rota adları eşleşen veya işleme isteklerinin URL'yi bir etkisi yok; Bunlar yalnızca URL üretmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-190">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="abd6b-191">[Yönlendirme](xref:fundamentals/routing) MVC özgü Yardımcılar olarak URL üretimi dahil olmak üzere URL oluşturma hakkında ayrıntılı bilgi vardır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-191">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="abd6b-192">Öznitelik yönlendirme</span><span class="sxs-lookup"><span data-stu-id="abd6b-192">Attribute routing</span></span>

<span data-ttu-id="abd6b-193">Öznitelik yönlendirme eylemleri doğrudan rota şablonlarının eşlemek için bir öznitelik kümesini kullanır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-193">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="abd6b-194">Aşağıdaki örnekte, `app.UseMvc();` kullanılır `Configure` yöntemi ve yol geçirilir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-194">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="abd6b-195">`HomeController` URL'ler için hangi varsayılan rota benzer birtakım eşleşecektir `{controller=Home}/{action=Index}/{id?}` eşleşir:</span><span class="sxs-lookup"><span data-stu-id="abd6b-195">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

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

<span data-ttu-id="abd6b-196">`HomeController.Index()` Herhangi bir URL yolu için eylem yürütülecek `/`, `/Home`, veya `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-196">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="abd6b-197">Bu örnekte, öznitelik Yönlendirme ve geleneksel yönlendirme anahtar programlama birbirinden vurgular.</span><span class="sxs-lookup"><span data-stu-id="abd6b-197">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="abd6b-198">Öznitelik yönlendirme bir yol belirtmek için daha fazla giriş gerektirir; Geleneksel varsayılan yol temellerini daha yollarını işler.</span><span class="sxs-lookup"><span data-stu-id="abd6b-198">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="abd6b-199">Bununla birlikte, öznitelik yönlendirme sağlar (ve gerektirir) rota şablonlarının her eylem için geçerli kesin bir denetim.</span><span class="sxs-lookup"><span data-stu-id="abd6b-199">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="abd6b-200">Denetleyici adı ve eylem adları yönlendirme özniteliğiyle play **hiçbir** eylem seçildiğinde rol.</span><span class="sxs-lookup"><span data-stu-id="abd6b-200">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="abd6b-201">Bu örnek önceki örnekteki gibi aynı URL'leri eşleşir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-201">This example will match the same URLs as the previous example.</span></span>

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
> <span data-ttu-id="abd6b-202">Rota şablonlarının rota parametrelerinin tanımlama yok `action`, `area`, ve `controller`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-202">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="abd6b-203">Aslında, bu rota parametreleri öznitelik rotaları izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="abd6b-203">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="abd6b-204">Rota şablonu zaten bir eylem ile ilişkili olduğundan bu eylem adını URL'den ayrıştırılacak anlamsız olmasını.</span><span class="sxs-lookup"><span data-stu-id="abd6b-204">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="abd6b-205">HTTP [eylem] özniteliği ile yönlendirme özniteliği</span><span class="sxs-lookup"><span data-stu-id="abd6b-205">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="abd6b-206">Öznitelik yönlendirme de yapabilirsiniz kullanım `Http[Verb]` gibi öznitelikleri `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-206">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="abd6b-207">Tüm bu öznitelikler, bir rota şablonu kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-207">All of these attributes can accept a route template.</span></span> <span data-ttu-id="abd6b-208">Bu örnek aynı rota şablonu eşleşen iki eylemleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="abd6b-208">This example shows two actions that match the same route template:</span></span>

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

<span data-ttu-id="abd6b-209">Gibi bir URL yolu için `/products` `ProductsApi.ListProducts` eylemi, HTTP fiili olduğunda yürütülecek `GET` ve `ProductsApi.CreateProduct` HTTP fiili olduğunda yürütülecek `POST`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-209">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="abd6b-210">İlk öznitelik yönlendirme URL rota öznitelikleri tarafından tanımlanan rota şablonlarının kümesini karşı eşleşir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-210">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="abd6b-211">Eşleşen bir rota şablonu sonra `IActionConstraint` kısıtlamaları, hangi eylemlerin yürütülebilecek belirlemek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-211">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="abd6b-212">REST API oluştururken, kullanmak istediğiniz nadir `[Route(...)]` bir eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="abd6b-212">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method.</span></span> <span data-ttu-id="abd6b-213">Daha fazla özel kullanmak daha iyidir `Http*Verb*Attributes` API'NİZİN desteklediği hakkında kesin olarak.</span><span class="sxs-lookup"><span data-stu-id="abd6b-213">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="abd6b-214">REST API'leri istemcileri belirli bir mantıksal işlemler için ne yollarını ve HTTP fiilleri harita bilmeniz beklenir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-214">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="abd6b-215">Öznitelik rotası belirli bir eylem için geçerli olduğundan, yol şablon tanımının bir parçası gerekli parametreleri sağlamak kolay bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-215">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="abd6b-216">Bu örnekte, `id` URL yolu bir parçası olarak gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-216">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="abd6b-217">`ProductsApi.GetProduct(int)` Gibi bir URL yolu için eylem yürütülecek `/products/3` gibi bir URL yolu için `/products`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-217">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="abd6b-218">Bkz: [yönlendirme](../../fundamentals/routing.md) rota şablonlarının ve ilgili seçeneklerinin tam bir açıklaması için.</span><span class="sxs-lookup"><span data-stu-id="abd6b-218">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="abd6b-219">Rota adı</span><span class="sxs-lookup"><span data-stu-id="abd6b-219">Route Name</span></span>

<span data-ttu-id="abd6b-220">Aşağıdaki kodu tanımlayan bir *rota adı* , `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="abd6b-220">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="abd6b-221">Rota adları, belirli bir rotaya bağlı bir URL'yi oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-221">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="abd6b-222">Rota adları, URL yönlendirme davranışı eşleştirme üzerinde hiçbir etkisi ve yalnızca URL üretmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-222">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="abd6b-223">Rota adları, uygulama genelinde benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-223">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="abd6b-224">Bu, geleneksel Karşıtlık *varsayılan rota*, tanımlayan `id` parametre olarak isteğe bağlı (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="abd6b-224">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="abd6b-225">Bu özelliği tam olarak API'leri belirtmek için izin verme gibi avantaj `/products` ve `/products/5` farklı eylemler için gönderilmesi.</span><span class="sxs-lookup"><span data-stu-id="abd6b-225">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="abd6b-226">Yolları birleştirme</span><span class="sxs-lookup"><span data-stu-id="abd6b-226">Combining routes</span></span>

<span data-ttu-id="abd6b-227">Öznitelik yönlendirme daha az tekrarlı yapmak için rota denetleyici özniteliklerinden yola rotası özniteliklerinin bireysel işlemlere ile birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-227">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="abd6b-228">Rota şablonlarının eylemleri için denetleyicide tanımlanan tüm rota şablonlarının tanımlandıkları.</span><span class="sxs-lookup"><span data-stu-id="abd6b-228">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="abd6b-229">Rota özniteliğinin denetleyicisinde yerleştirerek yapar **tüm** denetleyici eylemlerini kullanmak öznitelik yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="abd6b-229">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

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

<span data-ttu-id="abd6b-230">Bu örnekte URL yolu `/products` eşleşebilir `ProductsApi.ListProducts`ve URL yolunu `/products/5` eşleşebilir `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-230">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="abd6b-231">Bu eylemlerin her ikisi de yalnızca HTTP eşleşen `GET` ile belirtilmiş çünkü `HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-231">Both of these actions only match HTTP `GET` because they're decorated with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="abd6b-232">Rota ile başlayan bir eyleme uygulanan şablonlar `/` veya `~/` rota şablonuyla denetleyiciye uygulanan birleştirilmiş yok.</span><span class="sxs-lookup"><span data-stu-id="abd6b-232">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="abd6b-233">URL yolu için benzer bir dizi şu örnekle eşleşir *varsayılan rota*.</span><span class="sxs-lookup"><span data-stu-id="abd6b-233">This example matches a set of URL paths similar to the *default route*.</span></span>

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

### <a name="ordering-attribute-routes"></a><span data-ttu-id="abd6b-234">Öznitelik rotaları sıralama</span><span class="sxs-lookup"><span data-stu-id="abd6b-234">Ordering attribute routes</span></span>

<span data-ttu-id="abd6b-235">Tanımlanan bir sırayla yürütmek geleneksel yollar aksine öznitelik yönlendirme bir ağaç oluşturur ve aynı anda tüm yollar eşleşir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-235">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="abd6b-236">Bu gibi davranır-rota girişleri bir ideal sıralamada; yerleştirilmişse en belirgin yolları önce genel daha yollarını yürütme şansına sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="abd6b-236">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="abd6b-237">Örneğin, bir yol gibi `blog/search/{topic}` bir yol gibi daha fazla özel `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-237">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="abd6b-238">Mantıksal olarak konuşma `blog/search/{topic}` yol 'çalışır' ilk olarak, varsayılan olarak, yalnızca duyarlı sıralama olduğundan.</span><span class="sxs-lookup"><span data-stu-id="abd6b-238">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="abd6b-239">Geleneksel yönlendirme kullanarak Geliştirici yollar sıralamada yerleştirmek için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="abd6b-239">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="abd6b-240">Öznitelik rotaları, bir sipariş yapılandırabilir kullanarak `Order` tüm framework sağlanan rota özniteliklerini özelliği.</span><span class="sxs-lookup"><span data-stu-id="abd6b-240">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="abd6b-241">Yollar, artan göre işlenir tür `Order` özelliği.</span><span class="sxs-lookup"><span data-stu-id="abd6b-241">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="abd6b-242">Varsayılan sıra `0`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-242">The default order is `0`.</span></span> <span data-ttu-id="abd6b-243">Yolu kullanarak bir ayarı `Order = -1` sipariş ayarlamamanız yolları önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-243">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="abd6b-244">Yolu kullanarak bir ayarı `Order = 1` varsayılan rota sıralandıktan sonra çalışır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-244">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="abd6b-245">Yapılandırmanıza bağlı olarak önlemek `Order`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-245">Avoid depending on `Order`.</span></span> <span data-ttu-id="abd6b-246">Sonra URL alanını doğru bir şekilde yönlendirmek için Açık sipariş değerleri gerektiriyorsa, istemcilere de büyük olasılıkla karmaşık olur.</span><span class="sxs-lookup"><span data-stu-id="abd6b-246">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="abd6b-247">Genel öznitelik yönlendirme ile URL ile eşleşen doğru yolu seçer.</span><span class="sxs-lookup"><span data-stu-id="abd6b-247">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="abd6b-248">URL üretimi için kullanılan varsayılan düzenini çalışmıyorsa, bir geçersiz kılma uygulama daha genellikle daha basit olduğu rota adı kullanılarak `Order` özelliği.</span><span class="sxs-lookup"><span data-stu-id="abd6b-248">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="abd6b-249">Yönlendirme razor sayfaları ve MVC denetleyicisi yönlendirme paylaşım uygulaması.</span><span class="sxs-lookup"><span data-stu-id="abd6b-249">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="abd6b-250">Rota sırası Razor sayfaları konularında bilgi şu adreste [Razor sayfaları yol ve uygulama kuralları: Rota sırası](xref:razor-pages/razor-pages-conventions#route-order).</span><span class="sxs-lookup"><span data-stu-id="abd6b-250">Information on route order in the Razor Pages topics is available at [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order).</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="abd6b-251">Belirteç değiştirme rota şablonlarındaki ([controller] [action] [alanı])</span><span class="sxs-lookup"><span data-stu-id="abd6b-251">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="abd6b-252">Kolaylık olması için öznitelik rotaları Destek *belirteç değiştirme* tarafından bir belirteç köşeli ayraç içinde kapsayan (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="abd6b-252">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="abd6b-253">Belirteçleri `[action]`, `[area]`, ve `[controller]` eylem adı, alan adı ve denetleyici adını, rota tanımlandığı eyleminin değerleri ile değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-253">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="abd6b-254">Aşağıdaki örnekte, URL yolu yorumlar bölümünde anlatıldığı gibi eylemleri eşleşmesi:</span><span class="sxs-lookup"><span data-stu-id="abd6b-254">In the following example, the actions match URL paths as described in the comments:</span></span>

[!code-csharp[](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="abd6b-255">Belirteç değiştirme öznitelik rotaları oluşturma işleminin son adımında gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-255">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="abd6b-256">Yukarıdaki örnek aşağıdaki kodla aynı davranır:</span><span class="sxs-lookup"><span data-stu-id="abd6b-256">The above example will behave the same as the following code:</span></span>

[!code-csharp[](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="abd6b-257">Öznitelik rotaları da devralma ile birleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-257">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="abd6b-258">Bu belirteç değiştirme ile birleştirilmiş özellikle güçlüdür.</span><span class="sxs-lookup"><span data-stu-id="abd6b-258">This is particularly powerful combined with token replacement.</span></span>

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

<span data-ttu-id="abd6b-259">Belirteç değiştirme öznitelik rotaları tarafından tanımlanan rota adları için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-259">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="abd6b-260">`[Route("[controller]/[action]", Name="[controller]_[action]")]` Her eylem için benzersiz bir rota adı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="abd6b-260">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generates a unique route name for each action.</span></span>

<span data-ttu-id="abd6b-261">Sabit belirteç değiştirme sınırlayıcı eşleştirilecek `[` veya `]`, karakter tekrarlayarak kaçış (`[[` veya `]]`).</span><span class="sxs-lookup"><span data-stu-id="abd6b-261">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

::: moniker range=">= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="abd6b-262">Belirteç değiştirme özelleştirmek için bir parametre transformer kullanma</span><span class="sxs-lookup"><span data-stu-id="abd6b-262">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="abd6b-263">Belirteç değiştirme parametresi transformer kullanılarak özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-263">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="abd6b-264">Bir parametre transformer uygulayan `IOutboundParameterTransformer` ve parametre değerine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="abd6b-264">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="abd6b-265">Örneğin, bir özel `SlugifyParameterTransformer` parametre transformer değişiklikleri `SubscriptionManagement` yönlendirmek için değer `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-265">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="abd6b-266">`RouteTokenTransformerConvention` Bir uygulama modeli kuralına göre:</span><span class="sxs-lookup"><span data-stu-id="abd6b-266">The `RouteTokenTransformerConvention` is an application model convention that:</span></span>

* <span data-ttu-id="abd6b-267">Bir uygulamadaki tüm öznitelik rotaları parametresi transformer uygular.</span><span class="sxs-lookup"><span data-stu-id="abd6b-267">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="abd6b-268">Bunlar gibi öznitelik rotası belirteci değerleri özelleştirir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-268">Customizes the attribute route token values as they are replaced.</span></span>

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

<span data-ttu-id="abd6b-269">`RouteTokenTransformerConvention` Bir seçenek olarak kayıtlı `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-269">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

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

### <a name="multiple-routes"></a><span data-ttu-id="abd6b-270">Birden fazla rota</span><span class="sxs-lookup"><span data-stu-id="abd6b-270">Multiple Routes</span></span>

<span data-ttu-id="abd6b-271">Aynı eylemi ulaşan birden çok yol tanımlayan yönlendirme desteklediği öznitelik.</span><span class="sxs-lookup"><span data-stu-id="abd6b-271">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="abd6b-272">Davranışını taklit etmek için bu en yaygın kullanımı olan *varsayılan geleneksel yolu* aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="abd6b-272">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="abd6b-273">Birden çok yol özniteliğine denetleyicisinde koyma, her biri her eylem yöntemlerini rotası özniteliklerinin birleştirecek anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-273">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

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

<span data-ttu-id="abd6b-274">Zaman birden çok yol özniteliğine (uygulayan `IActionConstraint`) bir eylem, her eylem kısıtlaması ile tanımlanan özniteliğindeki rota şablonu birleştirir sonra yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-274">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

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
> <span data-ttu-id="abd6b-275">Birden çok yol eylemleri kullanarak güçlü görünebilir, ancak uygulamanızın URL'si alanı basit ve iyi tanımlanmış durumda tutmak daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-275">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="abd6b-276">Eylemler yalnızca gerektiğinde, örneğin mevcut istemcileri desteklemek için birden çok yol kullanın.</span><span class="sxs-lookup"><span data-stu-id="abd6b-276">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="abd6b-277">Öznitelik yönlendirme isteğe bağlı parametreleri, varsayılan değerleri ile kısıtlamaları belirtme</span><span class="sxs-lookup"><span data-stu-id="abd6b-277">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="abd6b-278">Öznitelik rotaları isteğe bağlı parametreler, varsayılan değerleri ile kısıtlamaları belirtmek için geleneksel rotalar aynı satır içi söz dizimini destekler.</span><span class="sxs-lookup"><span data-stu-id="abd6b-278">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="abd6b-279">Bkz: [rota şablonu başvurusu](../../fundamentals/routing.md#route-template-reference) rota şablonu söz dizimi ayrıntılı bir açıklaması.</span><span class="sxs-lookup"><span data-stu-id="abd6b-279">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="abd6b-280">Özel rota özniteliklerini kullanma `IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="abd6b-280">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="abd6b-281">Tüm framework içinde sağlanan rota özniteliklerini ( `[Route(...)]`, `[HttpGet(...)]` , vs.) uygulama `IRouteTemplateProvider` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="abd6b-281">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="abd6b-282">MVC arar denetleyici sınıflarına ve eylem yöntemlerine özniteliklerinde uygulama başladığında ve uygulama olanları kullanan `IRouteTemplateProvider` ilk rota kümesini oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="abd6b-282">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="abd6b-283">Uygulayabileceğiniz `IRouteTemplateProvider` kendi rota özniteliklerini tanımlamak için.</span><span class="sxs-lookup"><span data-stu-id="abd6b-283">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="abd6b-284">Her `IRouteTemplateProvider` sipariş tek bir rota özel rota şablonuyla tanımlamanıza olanak sağlar ve adlandırın:</span><span class="sxs-lookup"><span data-stu-id="abd6b-284">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="abd6b-285">Yukarıdaki örnekte öznitelik otomatik olarak ayarlar `Template` için `"api/[controller]"` olduğunda `[MyApiController]` uygulanır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-285">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="abd6b-286">Öznitelik rotaları özelleştirmek için uygulama modelini kullanarak</span><span class="sxs-lookup"><span data-stu-id="abd6b-286">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="abd6b-287">*Uygulama modeli* Başlangıçta tüm MVC tarafından yönlendirmek ve eylemlerinizi yürütmek için kullanılan meta veriler ile oluşturulan bir nesne modeli.</span><span class="sxs-lookup"><span data-stu-id="abd6b-287">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="abd6b-288">*Uygulama modeli* rota özniteliklerinden toplanan verileri içerir (aracılığıyla `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="abd6b-288">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="abd6b-289">Yazabileceğiniz *kuralları* nasıl yönlendirme davranışını özelleştirmek için başlangıç zamanında uygulama modeli değiştirmek için.</span><span class="sxs-lookup"><span data-stu-id="abd6b-289">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="abd6b-290">Bu bölümde, uygulama modelini kullanarak yönlendirme özelleştirilmesine basit bir örnek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-290">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="abd6b-291">Karma yönlendirme: VS geleneksel yönlendirmesi özniteliği</span><span class="sxs-lookup"><span data-stu-id="abd6b-291">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="abd6b-292">MVC uygulamaları geleneksel Yönlendirme ve öznitelik yönlendirme karıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="abd6b-292">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="abd6b-293">Tarayıcılar için HTML sayfalarını sunmadan denetleyicileri için geleneksel yollar kullanın ve REST API'leri sunan denetleyicileri için yönlendirme özniteliği için tipik bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="abd6b-293">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="abd6b-294">Eylemler genel yönlendirilir veya öznitelik yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-294">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="abd6b-295">Bir rota denetleyici veya eylem getirir, yönlendirilmiş özniteliği.</span><span class="sxs-lookup"><span data-stu-id="abd6b-295">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="abd6b-296">Öznitelik rotaları tanımlamak Eylemler, geleneksel yolları ve tersi ulaşılamıyor.</span><span class="sxs-lookup"><span data-stu-id="abd6b-296">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="abd6b-297">**Tüm** rota özniteliğinin denetleyicisinde yönlendirilen denetleyicisi özniteliğinde tüm eylemleri yapar.</span><span class="sxs-lookup"><span data-stu-id="abd6b-297">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="abd6b-298">Ne yönlendirme sistemleri iki tür ayıran bir rota şablonuna bir URL ile eşleşen sonra uygulanan işlemidir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-298">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="abd6b-299">Yönlendirme Geleneksel, eşleşme rota değerleri için eylem ve denetleyici tüm geleneksel yönlendirilmiş eylemleri bir arama tablosundan seçmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-299">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="abd6b-300">Öznitelik yönlendirme, her bir şablon zaten bir eylem ile ilişkilendirilir ve başka hiçbir arama gereklidir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-300">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="abd6b-301">Karmaşık segmentleri</span><span class="sxs-lookup"><span data-stu-id="abd6b-301">Complex segments</span></span>

<span data-ttu-id="abd6b-302">Karmaşık bir segment (örneğin, `[Route("/dog{token}cat")]`), değişmez değerlerden soldan sağa'kurmak doyumsuz olmayan bir yolla eşleşen tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-302">Complex segments (for example, `[Route("/dog{token}cat")]`), are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="abd6b-303">Bkz: [kaynak kodu](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) için bir açıklama.</span><span class="sxs-lookup"><span data-stu-id="abd6b-303">See [the source code](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) for a description.</span></span> <span data-ttu-id="abd6b-304">Daha fazla bilgi için [bu sorunu](https://github.com/aspnet/Docs/issues/8197).</span><span class="sxs-lookup"><span data-stu-id="abd6b-304">For more information, see [this issue](https://github.com/aspnet/Docs/issues/8197).</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="abd6b-305">URL üretimi</span><span class="sxs-lookup"><span data-stu-id="abd6b-305">URL Generation</span></span>

<span data-ttu-id="abd6b-306">MVC uygulamaları ait yönlendirme URL'si oluşturma özellikleri eylemleri URL bağlantılarını oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="abd6b-306">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="abd6b-307">URL'ler oluşturulurken, runbook'a kod URL'ler, kodunuzu daha sağlam ve sürdürülebilir hale ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-307">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="abd6b-308">Bu bölümde, MVC tarafından sağlanan URL üretimi özelliklere odaklanır ve yalnızca URL üretimi nasıl çalıştığına ilişkin temel bilgileri ele alınacaktır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-308">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="abd6b-309">Bkz: [yönlendirme](../../fundamentals/routing.md) URL üretimi ayrıntılı bir açıklaması.</span><span class="sxs-lookup"><span data-stu-id="abd6b-309">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="abd6b-310">`IUrlHelper` Temel alınan altyapı MVC arasındaki URL üretmek için yönlendirme parçası arabirimidir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-310">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="abd6b-311">Bir örneğini bulabilirsiniz `IUrlHelper` aracılığıyla `Url` denetleyicileri, görünümleri ve görünüm bileşenleri özelliği.</span><span class="sxs-lookup"><span data-stu-id="abd6b-311">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="abd6b-312">Bu örnekte, `IUrlHelper` arabirimi aracılığıyla kullanılır `Controller.Url` özelliği başka bir eylem için bir URL oluşturun.</span><span class="sxs-lookup"><span data-stu-id="abd6b-312">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="abd6b-313">Geleneksel varsayılan uygulamayı kullanıyorsa, yol, değerini `url` değişkeni URL yol dizesi olacaktır `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-313">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="abd6b-314">Bu URL yolu geçerli istek (ortam değerler), rota değerleri birleştirerek yönlendirme tarafından geçirilen değerlerle oluşturulur `Url.Action` ve bu değerler rota şablonuna değiştirme:</span><span class="sxs-lookup"><span data-stu-id="abd6b-314">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="abd6b-315">Rota şablonu her yol parametresi değerleri ve ortam değerleri ile eşleşen adları yerine değeri vardır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-315">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="abd6b-316">Bir değere sahip olmayan bir rota parametresini varsa veya isteğe bağlı ise atlanması durumunda varsayılan bir değer kullanabilirsiniz (olarak durumunda `id` Bu örnekte).</span><span class="sxs-lookup"><span data-stu-id="abd6b-316">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="abd6b-317">Tüm gerekli bir rota parametresini karşılık gelen bir değer yoksa, URL üretimi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="abd6b-317">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="abd6b-318">Bir rota için URL üretimi başarısız olursa, sonraki yol, tüm yolları denediniz mi veya bir eşleşme bulunduğu kadar denenir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-318">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="abd6b-319">Örnek `Url.Action` kavramları farklı olsa yukarıdaki geleneksel yönlendirme varsayılmaktadır, ancak öznitelik yönlendirme ile benzer şekilde URL üretimi çalışır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-319">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="abd6b-320">Geleneksel yönlendirme ile rota değerlerini bir şablon ve rota değerleri için genişletmek için kullanılan `controller` ve `action` genellikle görünür şablon içinde - yönlendirerek eşleşen URL'leri bağlı olduğundan bu çalışır bir *kuralı*.</span><span class="sxs-lookup"><span data-stu-id="abd6b-320">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="abd6b-321">Öznitelik yönlendirme, rota değerleri için `controller` ve `action` görüntülenecek şablonda - yerine kullandıkları hangi şablonun kullanılacağını aramak için izin verilmiyor.</span><span class="sxs-lookup"><span data-stu-id="abd6b-321">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="abd6b-322">Bu örnekte, öznitelik yönlendirme kullanır:</span><span class="sxs-lookup"><span data-stu-id="abd6b-322">This example uses attribute routing:</span></span>

[!code-csharp[](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="abd6b-323">MVC tüm öznitelik yönlendirilen eylemlerin arama tablosu oluşturur ve eşleştirir `controller` ve `action` URL üretmek için kullanılacak rota şablonu seçmek için değerler.</span><span class="sxs-lookup"><span data-stu-id="abd6b-323">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="abd6b-324">Yukarıdaki örnekteki `custom/url/to/destination` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="abd6b-324">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="abd6b-325">Eylem adına göre URL'ler oluşturulurken</span><span class="sxs-lookup"><span data-stu-id="abd6b-325">Generating URLs by action name</span></span>

<span data-ttu-id="abd6b-326">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="abd6b-326">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="abd6b-327">`Action`) ve tüm ilgili ne bir denetleyici adı ve eylem adı belirterek bağlantı kurduğunuz belirtmek istiyorsanız bu fikri dayalı tüm aşırı yüklemeler.</span><span class="sxs-lookup"><span data-stu-id="abd6b-327">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="abd6b-328">Kullanırken `Url.Action`, geçerli rota değerleri için `controller` ve `action` - değerini belirtilir `controller` ve `action` hem de bir parçası olan *ortam değerlerini* **ve** *değerleri*.</span><span class="sxs-lookup"><span data-stu-id="abd6b-328">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="abd6b-329">Yöntem `Url.Action`, her zaman geçerli değerleri kullanan `action` ve `controller` ve geçerli eylem için yönlendiren bir URL yolu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="abd6b-329">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="abd6b-330">Yönlendirme değerleri, bir URL oluşturulurken sağlamadı bilgilerinde doldurmak için ortam değerleri kullanmayı dener.</span><span class="sxs-lookup"><span data-stu-id="abd6b-330">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="abd6b-331">Gibi bir yol kullanılarak `{a}/{b}/{c}/{d}` ve ortam değerlerini `{ a = Alice, b = Bob, c = Carol, d = David }`, yönlendirme, ek değer içermeyen bir URL'yi oluşturmak için yeterli bilgiye sahip - tüm yol olduğundan parametre bir değere sahip.</span><span class="sxs-lookup"><span data-stu-id="abd6b-331">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="abd6b-332">Değer eklediyseniz `{ d = Donovan }`, değer `{ d = David }` yoksayıldı, ve oluşturulan URL yolu `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-332">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="abd6b-333">URL yolu hiyerarşiktir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-333">URL paths are hierarchical.</span></span> <span data-ttu-id="abd6b-334">Yukarıdaki örnekte değer katan `{ c = Cheryl }`, değerlerin her ikisini de `{ c = Carol, d = David }` yok.</span><span class="sxs-lookup"><span data-stu-id="abd6b-334">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="abd6b-335">Bir değer için artık bu durumda sahibiz `d` ve URL üretimi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="abd6b-335">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="abd6b-336">İstenen değeri belirtmek zorunda kalmaz `c` ve `d`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-336">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="abd6b-337">Varsayılan yol ile bu sorun isabet beklenebilir (`{controller}/{action}/{id?}`)- ancak bu davranış uygulamada nadiren karşınıza çıkacak `Url.Action` her zaman açıkça belirten bir `controller` ve `action` değeri.</span><span class="sxs-lookup"><span data-stu-id="abd6b-337">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="abd6b-338">Aşırı uzun `Url.Action` da ek bir göz *rota değerleri* dışında rota parametrelerinin değerlerini sağlamak nesne `controller` ve `action`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-338">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="abd6b-339">En yaygın olarak kullanılan bu görürsünüz `id` gibi `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-339">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="abd6b-340">Kural gereği *rota değerleri* nesnedir genellikle anonim türde bir nesne, ancak ayrıca olabilir bir `IDictionary<>` veya *düz eski .NET nesne*.</span><span class="sxs-lookup"><span data-stu-id="abd6b-340">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="abd6b-341">Rota parametrelerine eşleşmeyen herhangi bir ek rota değerleri, sorgu dizesinde yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-341">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="abd6b-342">Mutlak bir URL oluşturmak için kabul eden bir aşırı yüklemesini kullanın. bir `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="abd6b-342">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="abd6b-343">Yol tarafından URL'ler oluşturulurken</span><span class="sxs-lookup"><span data-stu-id="abd6b-343">Generating URLs by route</span></span>

<span data-ttu-id="abd6b-344">Yukarıdaki kod, denetleyici ve eylem adı geçirerek bir URL oluşturmanın gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-344">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="abd6b-345">`IUrlHelper` Ayrıca sağlar `Url.RouteUrl` yöntemlerin ailesi.</span><span class="sxs-lookup"><span data-stu-id="abd6b-345">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="abd6b-346">Bu yöntemlere benzer `Url.Action`, ancak bunlar geçerli değerleri kopyalamayın `action` ve `controller` rota değerleri için.</span><span class="sxs-lookup"><span data-stu-id="abd6b-346">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="abd6b-347">Belirli bir yol genellikle URL'yi oluşturmak için kullanılacak rota adı belirtmek için en yaygın kullanımı olan *olmadan* bir denetleyici veya eylem adı belirterek.</span><span class="sxs-lookup"><span data-stu-id="abd6b-347">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="abd6b-348">HTML URL'ler oluşturulurken</span><span class="sxs-lookup"><span data-stu-id="abd6b-348">Generating URLs in HTML</span></span>

<span data-ttu-id="abd6b-349">`IHtmlHelper` sağlar `HtmlHelper` yöntemleri `Html.BeginForm` ve `Html.ActionLink` oluşturulacak `<form>` ve `<a>` öğeleri sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="abd6b-349">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="abd6b-350">Bu yöntemleri `Url.Action` bir URL'yi oluşturmak için gereken yöntemini ve benzer bağımsız değişkenleri kabul.</span><span class="sxs-lookup"><span data-stu-id="abd6b-350">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="abd6b-351">`Url.RouteUrl` İçin arkadaşlarımız `HtmlHelper` olan `Html.BeginRouteForm` ve `Html.RouteLink` benzer işlevselliği olan.</span><span class="sxs-lookup"><span data-stu-id="abd6b-351">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="abd6b-352">TagHelpers URL'ler aracılığıyla oluşturmak `form` TagHelper ve `<a>` TagHelper.</span><span class="sxs-lookup"><span data-stu-id="abd6b-352">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="abd6b-353">Bunların her ikisi de kullanın `IUrlHelper` uygulamaları için.</span><span class="sxs-lookup"><span data-stu-id="abd6b-353">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="abd6b-354">Bkz: [formlarla çalışma](../views/working-with-forms.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="abd6b-354">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="abd6b-355">Görünümler içinde `IUrlHelper` aracılığıyla kullanılabilir `Url` yukarıdaki tarafından kapsanmayan tüm geçici URL üretmek için özellik.</span><span class="sxs-lookup"><span data-stu-id="abd6b-355">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="abd6b-356">' De eylem sonuçları URL'ler oluşturulurken</span><span class="sxs-lookup"><span data-stu-id="abd6b-356">Generating URLS in Action Results</span></span>

<span data-ttu-id="abd6b-357">Yukarıdaki örnekleri kullanarak gösterilmesini `IUrlHelper` bir eylem sonucu bir parçası olarak bir URL'yi oluşturmak için en yaygın kullanımı bir denetleyicide olmakla birlikte bir denetleyicide.</span><span class="sxs-lookup"><span data-stu-id="abd6b-357">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="abd6b-358">`ControllerBase` Ve `Controller` temel sınıflar, eylem sonuçlarını, başka bir eylem başvurmak için kullanışlı yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="abd6b-358">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="abd6b-359">Tipik bir kullanımı, kullanıcı girişi kabul ettikten sonra yeniden yönlendirme sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-359">One typical usage is to redirect after accepting user input.</span></span>

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

<span data-ttu-id="abd6b-360">Eylem sonuçlarını Fabrika yöntemleri yöntemlere benzer bir desen izleyin `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-360">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="abd6b-361">Adanmış geleneksel rotalar için özel durum</span><span class="sxs-lookup"><span data-stu-id="abd6b-361">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="abd6b-362">Geleneksel yönlendirme adlı rota tanımı özel bir tür kullanabileceğiniz bir *adanmış geleneksel rota*.</span><span class="sxs-lookup"><span data-stu-id="abd6b-362">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="abd6b-363">Aşağıdaki örnekte adlı rota `blog` adanmış geleneksel yoldur.</span><span class="sxs-lookup"><span data-stu-id="abd6b-363">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="abd6b-364">Bu yönlendirme tanımları kullanarak `Url.Action("Index", "Home")` URL yolu oluşturur `/` ile `default` yolu, ama neden?</span><span class="sxs-lookup"><span data-stu-id="abd6b-364">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="abd6b-365">Rota değerleri tahmin `{ controller = Home, action = Index }` bir URL'yi oluşturmak için yeterli kullanıyor `blog`, ve sonucu `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-365">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="abd6b-366">Adanmış geleneksel rotalar kullanan özel bir davranışa yol engeller karşılık gelen bir rota parametresini sahip değilseniz varsayılan değerler "çok greedy" URL üretimi ile.</span><span class="sxs-lookup"><span data-stu-id="abd6b-366">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="abd6b-367">Bu durumda varsayılan değerler `{ controller = Blog, action = Article }`ve hiçbiri `controller` ya da `action` bir rota parametresini görünür.</span><span class="sxs-lookup"><span data-stu-id="abd6b-367">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="abd6b-368">Yönlendirme URL üretimi gerçekleştirdiğinde, sağlanan değerler varsayılan değerlerin eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-368">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="abd6b-369">URL üretimi kullanarak `blog` başarısız olur değerleri `{ controller = Home, action = Index }` eşleşmeyen `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-369">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="abd6b-370">Yönlendirme ardından gördükleri denemek `default`, hangi başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="abd6b-370">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="abd6b-371">Alanları</span><span class="sxs-lookup"><span data-stu-id="abd6b-371">Areas</span></span>

<span data-ttu-id="abd6b-372">[Alanları](areas.md) ilgili işlevleri ayrı yönlendirme-için ad alanı (denetleyici eylemleri) ve klasör yapısını (için görünümler) bir gruba düzenlemek için kullanılan bir MVC özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-372">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="abd6b-373">Alanlara kullanarak sağlar - aynı ada sahip birden çok denetleyicilerine sahip bir uygulama farklı sahip oldukları sürece *alanları*.</span><span class="sxs-lookup"><span data-stu-id="abd6b-373">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="abd6b-374">Alanlara kullanarak başka bir rota parametresini ekleyerek yönlendirme amacıyla hiyerarşi oluşturur `area` için `controller` ve `action`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-374">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="abd6b-375">Bu bölümde, yönlendirme - alanları ile nasıl etkileştiğini ele alınacaktır bkz [alanları](areas.md) alanları görünümlerle nasıl kullanıldığı hakkında ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="abd6b-375">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="abd6b-376">Aşağıdaki örnek, varsayılan geleneksel yolu kullanmak için MVC yapılandırır ve bir *alan yolu* adlı bir alan için `Blog`:</span><span class="sxs-lookup"><span data-stu-id="abd6b-376">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="abd6b-377">Bir URL yolu gibi eşleştirirken `/Manage/Users/AddUser`, ilk rota, rota değerleri oluşturacak `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-377">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="abd6b-378">`area` Rota değeri için varsayılan bir değer tarafından üretilen `area`, aslında tarafından oluşturulan rota `MapAreaRoute` aşağıdakine eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="abd6b-378">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="abd6b-379">`MapAreaRoute` Varsayılan değer ve kısıtlamasını kullanarak bir yol oluşturur `area` bu durumda, belirtilen alan adını kullanarak `Blog`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-379">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="abd6b-380">Varsayılan değer yol her zaman üretir sağlar `{ area = Blog, ... }`, değer kısıtlaması gerekli `{ area = Blog, ... }` URL üretmek için.</span><span class="sxs-lookup"><span data-stu-id="abd6b-380">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="abd6b-381">Geleneksel yönlendirme sipariş bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-381">Conventional routing is order-dependent.</span></span> <span data-ttu-id="abd6b-382">Genel olarak, bunlar olmadan bir alan yolları daha ayrıntılı olarak alanları rotalarla önceki rota tablosunda yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-382">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="abd6b-383">Rota değerleri, yukarıdaki örneği kullanarak, aşağıdaki eylemi eşleşir:</span><span class="sxs-lookup"><span data-stu-id="abd6b-383">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="abd6b-384">`AreaAttribute` Ne bir denetleyici gösterir olan bir alan bir parçası olarak, bu denetleyici olduğunu olan dediğimiz `Blog` alan.</span><span class="sxs-lookup"><span data-stu-id="abd6b-384">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="abd6b-385">Denetleyicileri olmadan bir `[Area]` öznitelik herhangi bir alanı üyesi olmayan ve olacak **değil** ne zaman eşleşen `area` rota değeri yönlendirme tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-385">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="abd6b-386">Aşağıdaki örnekte, yalnızca listelenen ilk denetleyici, rota değerlerini eşleşebilir `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-386">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="abd6b-387">Her denetleyici ad alanı, bütünlük açısından buraya gösterilir; Aksi takdirde denetleyicileri adlandırma çakışması ve derleme hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="abd6b-387">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="abd6b-388">Sınıfı ad alanları MVC'nin yönlendirme üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="abd6b-388">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="abd6b-389">İlk iki denetleyici alanları üyeleri ve ilgili alan adlarının tarafından sağlandığında yalnızca eşleşen `area` rota değeri.</span><span class="sxs-lookup"><span data-stu-id="abd6b-389">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="abd6b-390">Üçüncü denetleyicisi hiçbir alan ve can yalnızca eşleşme için hiçbir değer üyesi olmayan `area` yönlendirme tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-390">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="abd6b-391">Eşleştirme bakımından *hiçbir değer*, olmaması `area` değeri aynı olduğu gibi değeri `area` null veya boş dize.</span><span class="sxs-lookup"><span data-stu-id="abd6b-391">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="abd6b-392">Yol alanı içinde bir eylemi yürütmede zaman değeri `area` olarak kullanılabilir bir *ortam değeri* yönlendirme URL'sini oluşturmak için kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="abd6b-392">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="abd6b-393">Varsayılan olarak alanları hareket yani *Yapışkan* tarafından aşağıdaki örnekte gösterildiği gibi URL üretmek için.</span><span class="sxs-lookup"><span data-stu-id="abd6b-393">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="abd6b-394">IActionConstraint anlama</span><span class="sxs-lookup"><span data-stu-id="abd6b-394">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="abd6b-395">Bu bölüm bir derinlemesine framework iç Ayrıntılar ve MVC yürütülecek bir eylem nasıl seçtiği yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-395">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="abd6b-396">Tipik bir uygulaması, özel bir gerekmez `IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="abd6b-396">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="abd6b-397">Büyük olasılıkla zaten kullanmış `IActionConstraint` arabirimi ile ilgili bilgi sahibi olmasanız bile.</span><span class="sxs-lookup"><span data-stu-id="abd6b-397">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="abd6b-398">`[HttpGet]` Özniteliği ve benzer `[Http-VERB]` öznitelikleri uygulamak `IActionConstraint` bir eylem yönteminin yürütülmesi sınırlamak için.</span><span class="sxs-lookup"><span data-stu-id="abd6b-398">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="abd6b-399">Varsayılan geleneksel yolu, URL yolu varsayılarak `/Products/Edit` değerleri oluşturur `{ controller = Products, action = Edit }`, hangi eşleşir **hem** burada gösterilen işlemlerin.</span><span class="sxs-lookup"><span data-stu-id="abd6b-399">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="abd6b-400">İçinde `IActionConstraint` terminolojisi dediğimiz bu eylemlerin her ikisi de adayları - değerlendirilir her ikisi de rota verilerini eşleşme olarak işle.</span><span class="sxs-lookup"><span data-stu-id="abd6b-400">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="abd6b-401">Zaman `HttpGetAttribute` yürütür, Yazar *Edit()* için bir eşleştirme olup *alma* ve bir eşleşme için herhangi bir HTTP fiili değil.</span><span class="sxs-lookup"><span data-stu-id="abd6b-401">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="abd6b-402">`Edit(...)` Eylem tanımlanmış kısıtlamalar yoksa ve bu nedenle herhangi bir HTTP fiili eşleşir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-402">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="abd6b-403">Bu nedenle varsayılarak bir `POST` - yalnızca `Edit(...)` eşleşir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-403">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="abd6b-404">Ancak, bir `GET` hem Eylemler hala - ancak bir eylem ile eşleşebilir bir `IActionConstraint` her zaman değerlendirilir *daha iyi* bir eylem daha.</span><span class="sxs-lookup"><span data-stu-id="abd6b-404">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="abd6b-405">Bu nedenle çünkü `Edit()` sahip `[HttpGet]` daha özel olarak kabul edilir ve her iki eylem durumunda seçilir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-405">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="abd6b-406">Kavramsal olarak, `IActionConstraint` bir biçimidir *aşırı yükleme*, ancak aynı ada sahip yöntem aşırı yükleme yerine onu aynı URL ile eşleşmek eylemler arasında aşırı yüklemesi.</span><span class="sxs-lookup"><span data-stu-id="abd6b-406">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="abd6b-407">Öznitelik yönlendirme de kullanır `IActionConstraint` ve farklı denetleyicileri eylemlerden her iki kabul adayları neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-407">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="abd6b-408">IActionConstraint uygulama</span><span class="sxs-lookup"><span data-stu-id="abd6b-408">Implementing IActionConstraint</span></span>

<span data-ttu-id="abd6b-409">Uygulamak için en kolay yolu bir `IActionConstraint` türetilen bir sınıf oluşturmak `System.Attribute` ve eylemlerin ve denetleyicilerin üzerinde yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="abd6b-409">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="abd6b-410">MVC otomatik olarak bulur herhangi `IActionConstraint` öznitelik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-410">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="abd6b-411">İçin nasıl uygulanacağını metaprogram sağladığından muhtemelen en esnek bir yaklaşım budur ve kısıtlamalar uygulamak için uygulama modeli kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="abd6b-411">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="abd6b-412">Aşağıdaki örnekte bir kısıtlama dayalı bir eylem seçtiği bir *ülke kodu* rota verileri.</span><span class="sxs-lookup"><span data-stu-id="abd6b-412">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="abd6b-413">[Tam örneğine Github'dan](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="abd6b-413">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

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

<span data-ttu-id="abd6b-414">Uygulamak için sorumlu olduğunuz `Accept` yöntemi ve 'yürütmek için bir sipariş' kısıtlaması için seçme.</span><span class="sxs-lookup"><span data-stu-id="abd6b-414">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="abd6b-415">Bu durumda, `Accept` yöntemi döndürür `true` eylemi bir eşleşme olduğunu göstermek için zaman `country` değer eşleşme yol.</span><span class="sxs-lookup"><span data-stu-id="abd6b-415">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="abd6b-416">Bu farklıdır bir `RouteValueAttribute` öznitelikli olmayan bir eylem için geri dönüş olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="abd6b-416">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="abd6b-417">Tanımlarsanız, gösteren örnek bir `en-US` eylemi ardından bir ülke kodu gibi `fr-FR` yok daha genel bir denetleyiciye döner `[CountrySpecific(...)]` uygulanır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-417">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="abd6b-418">`Order` Özelliği, karar *aşama* kısıtlaması bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-418">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="abd6b-419">Eylemi kısıtlamalarını Çalıştır göre gruplar halinde `Order`.</span><span class="sxs-lookup"><span data-stu-id="abd6b-419">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="abd6b-420">Örneğin, tüm framework'ün HTTP yöntemi öznitelikleri aynı kullanmak sağlanan `Order` böylece aynı aşamadaki çalıştırdığı değeri.</span><span class="sxs-lookup"><span data-stu-id="abd6b-420">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="abd6b-421">İstenen ilkelerinizi uygulamak gerektiği kadar aşama olabilir.</span><span class="sxs-lookup"><span data-stu-id="abd6b-421">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="abd6b-422">Bir değeri karar vermek üzere `Order` önce HTTP yöntemleri, kısıtlama uygulanması gereken olup olmadığını düşünün.</span><span class="sxs-lookup"><span data-stu-id="abd6b-422">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="abd6b-423">Düşük rakamlı ilk önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="abd6b-423">Lower numbers run first.</span></span>
