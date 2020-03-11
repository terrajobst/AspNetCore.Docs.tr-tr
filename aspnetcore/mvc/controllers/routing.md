---
title: ASP.NET Core denetleyici eylemlerine yönlendirme
author: rick-anderson
description: ASP.NET Core MVC 'nin, gelen isteklerin URL 'Lerini eşleştirmek ve bunları eylemlerle eşlemek için yönlendirme ara yazılımını nasıl kullandığını öğrenin.
ms.author: riande
ms.date: 12/05/2019
uid: mvc/controllers/routing
ms.openlocfilehash: 1116cc699f749a137638b75095a7172ad0d4858a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663725"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a><span data-ttu-id="e5f28-103">ASP.NET Core denetleyici eylemlerine yönlendirme</span><span class="sxs-lookup"><span data-stu-id="e5f28-103">Routing to controller actions in ASP.NET Core</span></span>

<span data-ttu-id="e5f28-104">[Ryan şimdi ak](https://github.com/rynowak) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e5f28-104">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e5f28-105">ASP.NET Core MVC, gelen isteklerin URL 'Leriyle eşleştirmek ve bunları eylemlerle eşlemek için yönlendirme [Ara yazılımını](xref:fundamentals/middleware/index) kullanır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-105">ASP.NET Core MVC uses the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="e5f28-106">Yollar başlangıç kodunda veya özniteliklerde tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-106">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="e5f28-107">Yollar URL yollarının eylemlerle nasıl eşleştirileceği açıklanır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-107">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="e5f28-108">Yollar, yanıt olarak gönderilen URL 'Leri (bağlantılar için) oluşturmak için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-108">Routes are also used to generate URLs (for links) sent out in responses.</span></span>

<span data-ttu-id="e5f28-109">Eylemler genel olarak Dolaştırılan veya Attribute olarak yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-109">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="e5f28-110">Bir yolu denetleyiciye koymak veya eylemi, BT özniteliği yönlendirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-110">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="e5f28-111">Daha fazla bilgi için bkz. [karma yönlendirme](#routing-mixed-ref-label) .</span><span class="sxs-lookup"><span data-stu-id="e5f28-111">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="e5f28-112">Bu belge, MVC ve yönlendirme arasındaki etkileşimleri ve tipik MVC uygulamalarının yönlendirme özelliklerini nasıl kullandığını açıklar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-112">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="e5f28-113">Gelişmiş yönlendirme hakkında ayrıntılar için bkz. [yönlendirme](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="e5f28-113">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="e5f28-114">Yönlendirme ara yazılımını ayarlama</span><span class="sxs-lookup"><span data-stu-id="e5f28-114">Setting up Routing Middleware</span></span>

<span data-ttu-id="e5f28-115">*Yapılandırma* yönteminde şuna benzer bir kod görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e5f28-115">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="e5f28-116">`UseMvc`çağrısının içinde `MapRoute`, `default` rota olarak başvurabileceğiniz tek bir yol oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-116">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="e5f28-117">Çoğu MVC uygulaması, `default` yoluna benzer bir şablon içeren bir yol kullanır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-117">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="e5f28-118">Yol şablonu `"{controller=Home}/{action=Index}/{id?}"` `/Products/Details/5` gibi bir URL yoluyla eşleştirebilir ve yolu simgeleştirerek `{ controller = Products, action = Details, id = 5 }` yol değerlerini ayıklar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-118">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="e5f28-119">MVC, `ProductsController` adlı bir denetleyiciyi bulmaya çalışır ve `Details`eylemi çalıştırır:</span><span class="sxs-lookup"><span data-stu-id="e5f28-119">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="e5f28-120">Bu örnekte model bağlamanın, bu eylemi çağırırken `id` parametresini `5` olarak ayarlamak için `id = 5` değerini kullanabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="e5f28-120">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="e5f28-121">Daha fazla ayrıntı için [model bağlamaya](../models/model-binding.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="e5f28-121">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="e5f28-122">`default` yolunu kullanma:</span><span class="sxs-lookup"><span data-stu-id="e5f28-122">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="e5f28-123">Yol şablonu:</span><span class="sxs-lookup"><span data-stu-id="e5f28-123">The route template:</span></span>

* <span data-ttu-id="e5f28-124">`{controller=Home}` varsayılan olarak `Home` tanımlar `controller`</span><span class="sxs-lookup"><span data-stu-id="e5f28-124">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="e5f28-125">`{action=Index}` varsayılan olarak `Index` tanımlar `action`</span><span class="sxs-lookup"><span data-stu-id="e5f28-125">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="e5f28-126">`{id?}` `id` isteğe bağlı olarak tanımlar</span><span class="sxs-lookup"><span data-stu-id="e5f28-126">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="e5f28-127">Bir eşleşme için URL yolunda varsayılan ve isteğe bağlı yol parametrelerinin mevcut olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="e5f28-127">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="e5f28-128">Yol şablonu sözdiziminin ayrıntılı açıklaması için bkz. [route Template Reference](../../fundamentals/routing.md#route-template-reference) .</span><span class="sxs-lookup"><span data-stu-id="e5f28-128">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="e5f28-129">`"{controller=Home}/{action=Index}/{id?}"`, `/` URL yolu ile eşleştirebilir ve `{ controller = Home, action = Index }`yol değerlerini üretecektir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-129">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="e5f28-130">`controller` ve `action` değerleri varsayılan değerleri kullanır `id`, URL yolunda karşılık gelen bir kesim olmadığından, bu değer oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="e5f28-130">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="e5f28-131">MVC bu yol değerlerini kullanarak `HomeController` ve `Index` eylemini seçer:</span><span class="sxs-lookup"><span data-stu-id="e5f28-131">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="e5f28-132">Bu denetleyici tanımı ve yönlendirme şablonunu kullanarak, aşağıdaki URL yollarından herhangi biri için `HomeController.Index` eylemi yürütülür:</span><span class="sxs-lookup"><span data-stu-id="e5f28-132">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="e5f28-133">Kolaylık yöntemi `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="e5f28-133">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="e5f28-134">Değiştirmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="e5f28-134">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="e5f28-135">`UseMvc` ve `UseMvcWithDefaultRoute` ara yazılım ardışık düzenine bir `RouterMiddleware` örneği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e5f28-135">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="e5f28-136">MVC, doğrudan ara yazılım ile etkileşime girmez ve istekleri işlemek için yönlendirmeyi kullanır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-136">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="e5f28-137">MVC bir `MvcRouteHandler`örneği aracılığıyla yollara bağlanır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-137">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="e5f28-138">`UseMvc` içindeki kod aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="e5f28-138">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="e5f28-139">`UseMvc` doğrudan hiçbir yol tanımlamıyor, yol koleksiyonuna `attribute` yolu için bir yer tutucu ekler.</span><span class="sxs-lookup"><span data-stu-id="e5f28-139">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="e5f28-140">Aşırı yükleme `UseMvc(Action<IRouteBuilder>)` kendi rotalarınızı eklemenize ve öznitelik yönlendirmeyi de desteklemenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-140">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="e5f28-141">`UseMvc` ve tüm çeşitlemeleri, `UseMvc`yapılandırma şeklinden bağımsız olarak her zaman kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-141">`UseMvc` and all of its variations add a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="e5f28-142">`UseMvcWithDefaultRoute` varsayılan bir yol tanımlar ve öznitelik yönlendirmeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="e5f28-142">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="e5f28-143">[Öznitelik yönlendirme](#attribute-routing-ref-label) bölümü öznitelik yönlendirme hakkında daha fazla ayrıntı içerir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-143">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="e5f28-144">Geleneksel yönlendirme</span><span class="sxs-lookup"><span data-stu-id="e5f28-144">Conventional routing</span></span>

<span data-ttu-id="e5f28-145">`default` yolu:</span><span class="sxs-lookup"><span data-stu-id="e5f28-145">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="e5f28-146">, *geleneksel yönlendirmeye*bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-146">is an example of a *conventional routing*.</span></span> <span data-ttu-id="e5f28-147">Bu stil *geleneksel yönlendirmeyi* , URL yolları için bir *kural* oluşturduğundan çağırıyoruz:</span><span class="sxs-lookup"><span data-stu-id="e5f28-147">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="e5f28-148">ilk yol kesimi denetleyicinin adıyla eşlenir</span><span class="sxs-lookup"><span data-stu-id="e5f28-148">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="e5f28-149">İkincisi eylem adıyla eşlenir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-149">the second maps to the action name.</span></span>

* <span data-ttu-id="e5f28-150">üçüncü segment bir model varlığına eşlemek için kullanılan isteğe bağlı `id` kullanılır</span><span class="sxs-lookup"><span data-stu-id="e5f28-150">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="e5f28-151">Bu `default` yolunu kullanarak, URL yolu `/Products/List` `ProductsController.List` eylemine eşlenir ve `/Blog/Article/17` eşlenir `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="e5f28-151">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="e5f28-152">Bu eşleme **yalnızca** denetleyiciye ve eylem adlarına dayalıdır ve ad alanları, kaynak dosya konumları veya yöntem parametrelerine göre değildir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-152">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="e5f28-153">Varsayılan yol ile geleneksel yönlendirmeyi kullanmak, tanımladığınız her eylem için yeni bir URL düzeniyle karşılaşmanıza gerek kalmadan uygulamayı hızlı bir şekilde oluşturmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-153">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="e5f28-154">CRUD stilinde eylemlere sahip bir uygulama için denetleyicilerinizdeki URL 'Lerin tutarlılığı, kodunuzun basitleştirilmesine ve Kullanıcı arabiriminizi daha öngörülebilir hale getirmenize yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-154">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="e5f28-155">`id`, yol şablonu tarafından isteğe bağlı olarak tanımlanır ve bu, eylemlerinizin URL 'nin bir parçası olarak sağlanmadan yürütebileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-155">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="e5f28-156">Genellikle, URL 'den `id` atlandığında ne olur, bu durum model bağlama tarafından `0` olarak ayarlanır ve sonuç olarak veritabanında eşleşen `id == 0`hiçbir varlık bulunamacaktır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-156">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="e5f28-157">Öznitelik yönlendirme, bazı eylemler için gereken KIMLIĞI, diğerleri için değil, daha ayrıntılı bir denetim sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-157">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="e5f28-158">Kurala göre belgeler, doğru kullanımlarda görünebilecekleri `id` gibi isteğe bağlı parametreleri de içerecektir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-158">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="e5f28-159">Birden çok yol</span><span class="sxs-lookup"><span data-stu-id="e5f28-159">Multiple routes</span></span>

<span data-ttu-id="e5f28-160">`MapRoute`daha fazla çağrı ekleyerek `UseMvc` içine birden çok yol ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5f28-160">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="e5f28-161">Bunun yapılması, birden çok kural tanımlamanızı veya belirli bir eyleme adanmış geleneksel yollar eklemenizi sağlar; örneğin:</span><span class="sxs-lookup"><span data-stu-id="e5f28-161">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="e5f28-162">Buradaki `blog` yol, geleneksel bir *geleneksel yoldur*, yani geleneksel yönlendirme sistemini kullanır, ancak belirli bir eyleme ayrılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-162">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="e5f28-163">`controller` ve `action` yol şablonunda parametre olarak görünmadığından, bu yol yalnızca varsayılan değerlere sahip olabilir ve bu nedenle bu yol her zaman eylem `BlogController.Article`eşlenir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-163">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="e5f28-164">Rota koleksiyonundaki yollar sıralanır ve eklendikleri sırada işlenir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-164">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="e5f28-165">Bu örnekte, `blog` yolu `default` rotadan önce denenecek.</span><span class="sxs-lookup"><span data-stu-id="e5f28-165">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="e5f28-166">*Adanmış geleneksel yollar* genellıkle, URL yolunun kalan kısmını yakalamak için `{*article}` gibi catch-all Route parametrelerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-166">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="e5f28-167">Bu, ' çok Greedy ' yolunu diğer yollarla eşleştirirken hedeflediğiniz URL 'Lerle eşleşen bir yol haline getirir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-167">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="e5f28-168">Bunu çözümlemek için ' Greedy ' yollarını daha sonra yol tablosuna koyun.</span><span class="sxs-lookup"><span data-stu-id="e5f28-168">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="e5f28-169">Dönüş</span><span class="sxs-lookup"><span data-stu-id="e5f28-169">Fallback</span></span>

<span data-ttu-id="e5f28-170">İstek işlemenin bir parçası olarak, MVC, uygulamanızdaki bir denetleyiciyi ve eylemi bulmak için yol değerlerinin kullanılabileceğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="e5f28-170">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="e5f28-171">Rota değerleri bir eylemle eşleşmezse, yol eşleşme olarak kabul edilmez ve sonraki rota denenir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-171">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="e5f28-172">Buna *geri dönüş*denir ve geleneksel yolların çakıştığı durumları basitleştirmek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-172">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="e5f28-173">Kesinleştirme eylemleri</span><span class="sxs-lookup"><span data-stu-id="e5f28-173">Disambiguating actions</span></span>

<span data-ttu-id="e5f28-174">İki eylem yönlendirme aracılığıyla eşleşiyorsa, MVC ' en iyi ' adayı seçmek için bir özel durum oluşturması veya bir özel durum oluşturmak için, MVC 'nin belirsizliğini</span><span class="sxs-lookup"><span data-stu-id="e5f28-174">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="e5f28-175">Örnek:</span><span class="sxs-lookup"><span data-stu-id="e5f28-175">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="e5f28-176">Bu denetleyici, URL yolu `/Products/Edit/17` ile eşleşen iki eylemi tanımlar ve verileri `{ controller = Products, action = Edit, id = 17 }`yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-176">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="e5f28-177">Bu, `Edit(int)` bir ürünü düzenlemek üzere bir form gösterdiği ve `Edit(int, Product)` postalanan formu işleyen MVC denetleyicileri için tipik bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-177">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="e5f28-178">Bunu yapmak için bu olası MVC, istek bir HTTP `POST` olduğunda `Edit(int, Product)` ve HTTP fiili başka bir şey olduğunda `Edit(int)` ' ı seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-178">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="e5f28-179">`HttpPostAttribute` (`[HttpPost]`), yalnızca HTTP fiili `POST`olduğunda eylemin seçili olmasını sağlayacak `IActionConstraint` uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-179">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="e5f28-180">`IActionConstraint` olması, `Edit(int, Product)` ' daha iyi bir eşleşme `Edit(int)`, bu nedenle önce `Edit(int, Product)` denenmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-180">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="e5f28-181">Yalnızca özelleştirilmiş senaryolarda özel `IActionConstraint` uygulamalar yazmanız gerekir, ancak diğer HTTP fiilleri için `HttpPostAttribute` benzer öznitelikler gibi özniteliklerin rol olduğunu anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-181">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="e5f28-182">Geleneksel yönlendirmesinde, eylemler bir `show form -> submit form` iş akışının parçası olduğunda aynı eylem adını kullanmak yaygındır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-182">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="e5f28-183">Bu düzenin rahatlığı, [ıactionconstraint 'ı anlama](#understanding-iactionconstraint) bölümünde daha sonra görünür hale gelir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-183">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="e5f28-184">Birden çok yol eşleşirse ve MVC ' en iyi ' yolu bulamazsa, bir `AmbiguousActionException`oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-184">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="e5f28-185">Yol adları</span><span class="sxs-lookup"><span data-stu-id="e5f28-185">Route names</span></span>

<span data-ttu-id="e5f28-186">Aşağıdaki örneklerde `"blog"` ve `"default"` dizeler yol adlarıdır:</span><span class="sxs-lookup"><span data-stu-id="e5f28-186">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="e5f28-187">Yol adları, URL oluşturma için adlandırılmış yolun kullanılabilmesi için yola mantıksal bir ad verir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-187">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="e5f28-188">Bu, yolların sıralaması URL oluşturma karmaşık hale geldiğinde URL oluşturmayı büyük ölçüde basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-188">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="e5f28-189">Yol adları, uygulama genelinde benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-189">Route names must be unique application-wide.</span></span>

<span data-ttu-id="e5f28-190">Yol adları, isteklerin URL 'SI ile eşleşmesini veya işlenmesini etkilemez; Bunlar yalnızca URL oluşturma için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-190">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="e5f28-191">[Yönlendirme](xref:fundamentals/routing) , MVC 'ye özgü yardımcılardaki URL oluşturma da dahil olmak üzere URL oluşturma hakkında daha ayrıntılı bilgiler içerir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-191">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="e5f28-192">Öznitelik yönlendirme</span><span class="sxs-lookup"><span data-stu-id="e5f28-192">Attribute routing</span></span>

<span data-ttu-id="e5f28-193">Öznitelik yönlendirme eylemleri doğrudan yönlendirme şablonlarına eşlemek için bir öznitelik kümesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-193">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="e5f28-194">Aşağıdaki örnekte, `app.UseMvc();` `Configure` yönteminde kullanılır ve hiçbir yol geçirilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-194">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="e5f28-195">`HomeController`, varsayılan yol `{controller=Home}/{action=Index}/{id?}` eşleşeceğinize benzer bir URL kümesiyle eşleşir:</span><span class="sxs-lookup"><span data-stu-id="e5f28-195">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

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

<span data-ttu-id="e5f28-196">`HomeController.Index()` eylem, `/`, `/Home`veya `/Home/Index`URL yollarından herhangi biri için yürütülür.</span><span class="sxs-lookup"><span data-stu-id="e5f28-196">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="e5f28-197">Bu örnek, öznitelik yönlendirme ve geleneksel yönlendirme arasında bir temel programlama farkı vurgulamaktadır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-197">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="e5f28-198">Öznitelik yönlendirme, bir yol belirtmek için daha fazla giriş gerektirir; geleneksel varsayılan yol, yönlendirmeleri daha succinctly işler.</span><span class="sxs-lookup"><span data-stu-id="e5f28-198">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="e5f28-199">Ancak, öznitelik yönlendirme (ve gerektirir) her eylem için hangi rota şablonlarının uygulanacağını kesin olarak denetler.</span><span class="sxs-lookup"><span data-stu-id="e5f28-199">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="e5f28-200">Öznitelik yönlendirme ile, denetleyici adı ve eylem adları, **herhangi** bir eylem seçildiği hiçbir rol oynar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-200">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="e5f28-201">Bu örnek, önceki örnekle aynı URL 'Lerle eşleştirecektir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-201">This example will match the same URLs as the previous example.</span></span>

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
> <span data-ttu-id="e5f28-202">Yukarıdaki yol şablonları `action`, `area`ve `controller`için yol parametreleri tanımlamaz.</span><span class="sxs-lookup"><span data-stu-id="e5f28-202">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="e5f28-203">Aslında, öznitelik rotalarında bu yol parametrelerine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="e5f28-203">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="e5f28-204">Yol şablonu bir eylemle zaten ilişkili olduğundan, URL 'den eylem adını ayrıştırmak mantıklı değildir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-204">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="e5f28-205">Http [fiil] öznitelikleriyle öznitelik yönlendirme</span><span class="sxs-lookup"><span data-stu-id="e5f28-205">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="e5f28-206">Öznitelik yönlendirme Ayrıca, `HttpPostAttribute`gibi `Http[Verb]` özniteliklerini de kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-206">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="e5f28-207">Bu özniteliklerin hepsi bir yol şablonunu kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-207">All of these attributes can accept a route template.</span></span> <span data-ttu-id="e5f28-208">Bu örnekte, aynı rota şablonuyla eşleşen iki eylem gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="e5f28-208">This example shows two actions that match the same route template:</span></span>

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

<span data-ttu-id="e5f28-209">`/products` gibi bir URL yolu için, HTTP fiili `GET` olduğunda `ProductsApi.ListProducts` eylemi yürütülür ve HTTP fiili `POST`olduğunda `ProductsApi.CreateProduct` yürütülür.</span><span class="sxs-lookup"><span data-stu-id="e5f28-209">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="e5f28-210">Öznitelik yönlendirme öncelikle URL ile yol öznitelikleri tarafından tanımlanan yol şablonları kümesine göre eşleşir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-210">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="e5f28-211">Bir rota şablonu eşleştiğinde, hangi eylemlerin yürütüleceğini belirleyen `IActionConstraint` kısıtlamalar uygulanır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-211">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="e5f28-212">Bir REST API oluştururken, eylem tüm HTTP yöntemlerini kabul edecek şekilde bir eylem yönteminde `[Route(...)]` kullanmak isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="e5f28-212">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method as the action will accept all HTTP methods.</span></span> <span data-ttu-id="e5f28-213">API 'nizin neleri desteklediği hakkında kesin olması için daha özel `Http*Verb*Attributes` kullanmak daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-213">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="e5f28-214">REST API 'lerinin istemcileri, hangi yolların ve HTTP fiillerinin belirli mantıksal işlemlere eşlendiğini bilmelidir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-214">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="e5f28-215">Bir öznitelik yolu belirli bir eyleme uyguladığı için, yol şablonu tanımının bir parçası olarak gerekli parametreleri yapmak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-215">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="e5f28-216">Bu örnekte, URL yolunun bir parçası olarak `id` gereklidir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-216">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="e5f28-217">`ProductsApi.GetProduct(int)` eylemi, `/products/3` gibi bir URL yolu için yürütülür, ancak `/products`gibi bir URL yolu için değil.</span><span class="sxs-lookup"><span data-stu-id="e5f28-217">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="e5f28-218">Yol şablonlarının ve ilgili seçeneklerin tam açıklaması için bkz. [yönlendirme](../../fundamentals/routing.md) .</span><span class="sxs-lookup"><span data-stu-id="e5f28-218">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="e5f28-219">Yol adı</span><span class="sxs-lookup"><span data-stu-id="e5f28-219">Route Name</span></span>

<span data-ttu-id="e5f28-220">Aşağıdaki kod `Products_List`*yol adını* tanımlar:</span><span class="sxs-lookup"><span data-stu-id="e5f28-220">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="e5f28-221">Yol adları, belirli bir yolu temel alan bir URL oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-221">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="e5f28-222">Rota adlarının, yönlendirmenin URL eşleştirme davranışına etkisi yoktur ve yalnızca URL oluşturma için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-222">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="e5f28-223">Yol adları, uygulama genelinde benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-223">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="e5f28-224">Bunu, `id` parametresini isteğe bağlı (`{id?}`) olarak tanımlayan geleneksel *varsayılan rotayla*karşıtın.</span><span class="sxs-lookup"><span data-stu-id="e5f28-224">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="e5f28-225">API 'Leri tam olarak belirtme özelliği, `/products` ve `/products/5` farklı eylemlere dağıtılması gibi avantajlar sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-225">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="e5f28-226">Yolları birleştirme</span><span class="sxs-lookup"><span data-stu-id="e5f28-226">Combining routes</span></span>

<span data-ttu-id="e5f28-227">Öznitelik yönlendirmeyi daha az tekrarlı hale getirmek için, denetleyicideki yol öznitelikleri, bireysel eylemlerdeki rota öznitelikleriyle birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-227">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="e5f28-228">Denetleyicide tanımlanan tüm yol şablonları, eylemlerdeki rota şablonlarına eklenir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-228">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="e5f28-229">Bir Route özniteliğinin denetleyiciye yerleştirilmesi, denetleyicideki **Tüm** eylemlerin öznitelik yönlendirme kullanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-229">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

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

<span data-ttu-id="e5f28-230">Bu örnekte, URL yolu `/products` `ProductsApi.ListProducts`ile eşleştirebilir ve URL yolu `/products/5` `ProductsApi.GetProduct(int)`eşleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-230">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="e5f28-231">Bu eylemlerin her ikisi de, `HttpGetAttribute`olarak işaretlendiğinden HTTP `GET` eşleşir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-231">Both of these actions only match HTTP `GET` because they're marked with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="e5f28-232">`/` veya `~/` ile başlayan bir eyleme uygulanan yol şablonları denetleyiciye uygulanan yol şablonları ile birleştirilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-232">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="e5f28-233">Bu örnek, *varsayılan rotaya*benzer bir URL yolları kümesiyle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-233">This example matches a set of URL paths similar to the *default route*.</span></span>

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

### <a name="ordering-attribute-routes"></a><span data-ttu-id="e5f28-234">Öznitelik yollarını sıralama</span><span class="sxs-lookup"><span data-stu-id="e5f28-234">Ordering attribute routes</span></span>

<span data-ttu-id="e5f28-235">Tanımlı sırada yürütülen geleneksel yolların aksine, öznitelik yönlendirme bir ağaç oluşturur ve tüm yollarla aynı anda eşleşir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-235">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="e5f28-236">Bu, yol girişleri ideal bir sıralamaya yerleştirildiyse olduğu gibi davranır; en özel yolların, daha genel yollardan önce yürütülmesi şansınız vardır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-236">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="e5f28-237">Örneğin, `blog/search/{topic}` gibi bir yol `blog/{*article}`gibi bir yol daha özgüdür.</span><span class="sxs-lookup"><span data-stu-id="e5f28-237">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="e5f28-238">İlk olarak ' çalıştırmaları ' `blog/search/{topic}` yolu için, varsayılan olarak, tek yapmanız gereken tek bir sıralama olduğundan mantıksal olarak konuşun.</span><span class="sxs-lookup"><span data-stu-id="e5f28-238">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="e5f28-239">Geleneksel yönlendirmeyi kullanarak, yolları istenen sırada yerleştirmekten geliştirici sorumludur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-239">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="e5f28-240">Öznitelik yolları, tüm Framework yol özniteliklerinin `Order` özelliğini kullanarak bir sıra yapılandırabilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-240">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="e5f28-241">Yollar `Order` özelliğinin artan sıralamasına göre işlenir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-241">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="e5f28-242">Varsayılan sıra `0`.</span><span class="sxs-lookup"><span data-stu-id="e5f28-242">The default order is `0`.</span></span> <span data-ttu-id="e5f28-243">`Order = -1` kullanarak bir yolun ayarlanması, bir sipariş ayarlamadan önce çalıştırılacak rotalardan önce çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-243">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="e5f28-244">`Order = 1` kullanarak bir yolun ayarlanması, varsayılan yol sıralaması sonrasında çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-244">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="e5f28-245">`Order`bağlı olmadığından kaçının.</span><span class="sxs-lookup"><span data-stu-id="e5f28-245">Avoid depending on `Order`.</span></span> <span data-ttu-id="e5f28-246">URL alanınız, doğru sıralama değerlerinin doğru şekilde yönlendirilmesini gerektiriyorsa, istemciler de kafa karıştırıcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-246">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="e5f28-247">Genel öznitelik yönlendirme ' de, URL eşleştirme ile doğru yolu seçer.</span><span class="sxs-lookup"><span data-stu-id="e5f28-247">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="e5f28-248">URL oluşturma için kullanılan varsayılan sıra çalışmıyorsa, yol adının bir geçersiz kılma olarak kullanılması genellikle `Order` özelliğini uygulamaktan daha basittir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-248">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="e5f28-249">Razor Pages yönlendirme ve MVC denetleyici yönlendirme bir uygulamayı paylaşır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-249">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="e5f28-250">Razor Pages konularındaki yol siparişi hakkında bilgiler [Razor Pages yol ve uygulama kuralları: yol sıralaması](xref:razor-pages/razor-pages-conventions#route-order)' nda bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-250">Information on route order in the Razor Pages topics is available at [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order).</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="e5f28-251">Yol şablonlarında belirteç değiştirme ([denetleyici], [eylem], [alan])</span><span class="sxs-lookup"><span data-stu-id="e5f28-251">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="e5f28-252">Özellik yolları, bir belirteci köşeli ayraç içine alarak *belirteç değişimini* destekler (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="e5f28-252">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="e5f28-253">`[action]`, `[area]`ve `[controller]` belirteçleri, yolun tanımlandığı eylemden eylem adı, alan adı ve denetleyici adı değerleriyle değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-253">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="e5f28-254">Aşağıdaki örnekte, Eylemler, açıklamalarda açıklandığı gibi URL yollarıyla eşleşir:</span><span class="sxs-lookup"><span data-stu-id="e5f28-254">In the following example, the actions match URL paths as described in the comments:</span></span>

[!code-csharp[](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="e5f28-255">Belirteç değişikliği, öznitelik yollarının oluşturulması için son adım olarak gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-255">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="e5f28-256">Yukarıdaki örnek aşağıdaki kodla aynı şekilde davranır:</span><span class="sxs-lookup"><span data-stu-id="e5f28-256">The above example will behave the same as the following code:</span></span>

[!code-csharp[](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="e5f28-257">Öznitelik rotaları de devralma ile birleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-257">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="e5f28-258">Bu özellikle, belirteç değiştirme ile güçlü bir şekilde birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-258">This is particularly powerful combined with token replacement.</span></span>

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

<span data-ttu-id="e5f28-259">Belirteç değişikliği, öznitelik rotaları tarafından tanımlanan yol adları için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-259">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="e5f28-260">`[Route("[controller]/[action]", Name="[controller]_[action]")]` her eylem için benzersiz bir yol adı üretir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-260">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generates a unique route name for each action.</span></span>

<span data-ttu-id="e5f28-261">Sabit belirteç değiştirme sınırlayıcısı `[` veya `]`eşleştirmek için, karakteri (`[[` veya `]]`) tekrarlayarak kaçış.</span><span class="sxs-lookup"><span data-stu-id="e5f28-261">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

::: moniker range=">= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="e5f28-262">Belirteç değişimini özelleştirmek için bir parametre transformatörü kullanın</span><span class="sxs-lookup"><span data-stu-id="e5f28-262">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="e5f28-263">Belirteç değiştirme, bir parametre transformatörü kullanılarak özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-263">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="e5f28-264">Bir parametre transformatörü `IOutboundParameterTransformer` uygular ve parametrelerin değerini dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="e5f28-264">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="e5f28-265">Örneğin, özel bir `SlugifyParameterTransformer` parametresi transformatörü `SubscriptionManagement` Route değerini `subscription-management`olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-265">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="e5f28-266">`RouteTokenTransformerConvention`, şu şekilde bir uygulama modeli kuralıdır:</span><span class="sxs-lookup"><span data-stu-id="e5f28-266">The `RouteTokenTransformerConvention` is an application model convention that:</span></span>

* <span data-ttu-id="e5f28-267">Bir uygulamadaki tüm öznitelik yollarına bir parametre transformatörü uygular.</span><span class="sxs-lookup"><span data-stu-id="e5f28-267">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="e5f28-268">Öznitelik yol belirteci değerlerini değiştirildikleri gibi özelleştirir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-268">Customizes the attribute route token values as they are replaced.</span></span>

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

<span data-ttu-id="e5f28-269">`RouteTokenTransformerConvention`, `ConfigureServices`bir seçenek olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-269">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

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

### <a name="multiple-routes"></a><span data-ttu-id="e5f28-270">Birden çok yol</span><span class="sxs-lookup"><span data-stu-id="e5f28-270">Multiple Routes</span></span>

<span data-ttu-id="e5f28-271">Öznitelik yönlendirme, aynı eyleme ulaşan birden çok yolun tanımlanmasını destekler.</span><span class="sxs-lookup"><span data-stu-id="e5f28-271">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="e5f28-272">Bunun en yaygın kullanımları, aşağıdaki örnekte gösterildiği gibi *varsayılan geleneksel yolun* davranışını taklit etmek olur:</span><span class="sxs-lookup"><span data-stu-id="e5f28-272">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="e5f28-273">Denetleyiciye birden çok yol özniteliği koymak, her birinin eylem yöntemlerinde yol özniteliklerinin her biriyle birleşmesi anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-273">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

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

<span data-ttu-id="e5f28-274">Birden çok yol özniteliği (`IActionConstraint`uygulayan) bir eyleme yerleştirildiğinde, her eylem kısıtlaması, onu tanımlayan öznitelikten yol şablonuyla birleştirir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-274">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

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
> <span data-ttu-id="e5f28-275">Eylemlerde birden çok yolun kullanılması güçlü görünse de, uygulamanızın URL alanının basit ve iyi tanımlanmış tutulması daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-275">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="e5f28-276">Yalnızca gerektiğinde eylemler üzerinde birden çok yol kullanın, örneğin mevcut istemcileri desteklemek için.</span><span class="sxs-lookup"><span data-stu-id="e5f28-276">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="e5f28-277">Öznitelik rotası isteğe bağlı parametreler, varsayılan değerler ve kısıtlamalar belirtme</span><span class="sxs-lookup"><span data-stu-id="e5f28-277">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="e5f28-278">Öznitelik yolları, isteğe bağlı parametreleri, varsayılan değerleri ve kısıtlamaları belirtmek için geleneksel yollarla aynı satır içi sözdizimini destekler.</span><span class="sxs-lookup"><span data-stu-id="e5f28-278">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="e5f28-279">Yol şablonu sözdiziminin ayrıntılı açıklaması için bkz. [route Template Reference](../../fundamentals/routing.md#route-template-reference) .</span><span class="sxs-lookup"><span data-stu-id="e5f28-279">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="e5f28-280">`IRouteTemplateProvider` kullanarak özel yol öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="e5f28-280">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="e5f28-281">Çerçevede (`[Route(...)]`, `[HttpGet(...)]`, vb.) sunulan yol özniteliklerinin hepsi `IRouteTemplateProvider` arabirimini uygular.</span><span class="sxs-lookup"><span data-stu-id="e5f28-281">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="e5f28-282">MVC, uygulama başlatıldığında denetleyici sınıflarında ve eylem yöntemlerinde öznitelikler arar ve ilk yol kümesini oluşturmak için `IRouteTemplateProvider` uygulayan uygulamaları kullanır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-282">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="e5f28-283">Kendi yol öznitelerinizi tanımlamak için `IRouteTemplateProvider` uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5f28-283">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="e5f28-284">Her `IRouteTemplateProvider`, özel bir yol şablonu, sırası ve adı ile tek bir yol tanımlamanızı sağlar:</span><span class="sxs-lookup"><span data-stu-id="e5f28-284">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="e5f28-285">Yukarıdaki örnekteki özniteliği, `[MyApiController]` uygulandığında otomatik olarak `Template` `"api/[controller]"` olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-285">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="e5f28-286">Öznitelik yollarını özelleştirmek için uygulama modelini kullanma</span><span class="sxs-lookup"><span data-stu-id="e5f28-286">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="e5f28-287">*Uygulama modeli* , MVC tarafından eylemlerinizi yönlendirmek ve yürütmek için kullanılan tüm meta veriler ile başlangıçta oluşturulan bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-287">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="e5f28-288">*Uygulama modeli* , yol özniteliklerinden toplanan tüm verileri içerir (`IRouteTemplateProvider`aracılığıyla).</span><span class="sxs-lookup"><span data-stu-id="e5f28-288">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="e5f28-289">Yönlendirme işleminin nasıl davranacağını özelleştirmek için, Başlangıç zamanında uygulama modelini değiştirmek üzere *kurallar* yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5f28-289">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="e5f28-290">Bu bölümde, uygulama modeli kullanılarak yönlendirmeyi özelleştirmenin basit bir örneği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-290">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="e5f28-291">Karma yönlendirme: öznitelik yönlendirme vs geleneksel yönlendirme</span><span class="sxs-lookup"><span data-stu-id="e5f28-291">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="e5f28-292">MVC uygulamaları, geleneksel yönlendirme ve öznitelik yönlendirmenin kullanımını karıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-292">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="e5f28-293">Tarayıcılar için HTML sayfalarına hizmet veren denetleyiciler için geleneksel yollar ve REST API 'Lerine hizmet veren denetleyiciler için öznitelik yönlendirme kullanılması normaldir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-293">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="e5f28-294">Eylemler genel olarak Dolaştırılan veya Attribute olarak yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-294">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="e5f28-295">Bir yolu denetleyiciye koymak veya eylemi, BT özniteliği yönlendirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-295">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="e5f28-296">Öznitelik yollarını tanımlayan eylemlere geleneksel yollar üzerinden ulaşılamıyor ve bunun tersi de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-296">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="e5f28-297">Denetleyicideki **herhangi bir** rota özniteliği, denetleyici özniteliğindeki tüm eylemlerin yönlendirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-297">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="e5f28-298">İki tür yönlendirme sisteminin ayırt edilmesini ne kadar ayırt eden, bir URL bir yol şablonuyla eşleştirdikten sonra uygulanan işlemdir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-298">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="e5f28-299">Geleneksel yönlendirmesinde, eşleşmeden yol değerleri, tüm geleneksel yönlendirilmiş eylemlerin arama tablosundan eylemi ve denetleyiciyi seçmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-299">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="e5f28-300">Öznitelik yönlendirmesinde, her şablon zaten bir eylemle ilişkilendirilir ve başka bir arama gerekmez.</span><span class="sxs-lookup"><span data-stu-id="e5f28-300">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="e5f28-301">Karmaşık segmentler</span><span class="sxs-lookup"><span data-stu-id="e5f28-301">Complex segments</span></span>

<span data-ttu-id="e5f28-302">Karmaşık segmentler (örneğin, `[Route("/dog{token}cat")]`), sabit değerli olmayan değişmez değerler ile sağdan sola eşleştirilirken işlenir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-302">Complex segments (for example, `[Route("/dog{token}cat")]`), are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="e5f28-303">Bir açıklama için bkz. [kaynak kodu](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) .</span><span class="sxs-lookup"><span data-stu-id="e5f28-303">See [the source code](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) for a description.</span></span> <span data-ttu-id="e5f28-304">Daha fazla bilgi için [Bu soruna](https://github.com/dotnet/AspNetCore.Docs/issues/8197)bakın.</span><span class="sxs-lookup"><span data-stu-id="e5f28-304">For more information, see [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/8197).</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="e5f28-305">URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="e5f28-305">URL Generation</span></span>

<span data-ttu-id="e5f28-306">MVC uygulamaları, eylemlere URL bağlantıları oluşturmak için yönlendirmenin URL oluşturma özelliklerini kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-306">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="e5f28-307">URL oluşturma, kodlarınızın daha sağlam ve sürdürülebilir hale getirilmesi için sorunsuz kodlama URL 'Lerini ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-307">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="e5f28-308">Bu bölüm, MVC tarafından sunulan URL oluşturma özelliklerine odaklanır ve yalnızca URL oluşturmanın nasıl çalıştığına ilişkin temel bilgileri kapsar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-308">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="e5f28-309">URL oluşturma hakkında ayrıntılı bir açıklama için bkz. [yönlendirme](../../fundamentals/routing.md) .</span><span class="sxs-lookup"><span data-stu-id="e5f28-309">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="e5f28-310">`IUrlHelper` arabirimi, URL oluşturma için MVC ve yönlendirme arasındaki temel altyapı parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-310">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="e5f28-311">Denetleyiciler, görünümler ve görünüm bileşenlerinde `Url` özelliği aracılığıyla kullanılabilen bir `IUrlHelper` örneğini bulacaksınız.</span><span class="sxs-lookup"><span data-stu-id="e5f28-311">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="e5f28-312">Bu örnekte `IUrlHelper` arabirimi, başka bir eyleme yönelik bir URL oluşturmak için `Controller.Url` özelliği aracılığıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-312">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="e5f28-313">Uygulama varsayılan geleneksel rotayı kullanıyorsa, `url` değişkenin değeri `/UrlGeneration/Destination`URL yol dizesi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-313">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="e5f28-314">Bu URL yolu, yönlendirme değerlerini, geçerli istekten (çevresel değerler), `Url.Action` aktarılan değerlerle ve bu değerleri yol şablonuna geçirerek birleştirerek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-314">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="e5f28-315">Yol şablonundaki her bir rota parametresinin değeri, değerler ve ortam değerleri ile eşleşen adlara sahip olacak şekilde değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-315">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="e5f28-316">Bir değere sahip olmayan bir rota parametresi, varsa varsayılan bir değer kullanabilir veya isteğe bağlı ise (Bu örnekteki `id` olduğu gibi) atlanır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-316">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="e5f28-317">Gerekli yol parametresinin karşılık gelen bir değeri yoksa, URL oluşturma başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-317">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="e5f28-318">Bir yol için URL oluşturma başarısız olursa, tüm yollar Denenene veya bir eşleşme bulunana kadar sonraki yol denenir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-318">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="e5f28-319">Yukarıdaki `Url.Action` örneği geleneksel yönlendirmeyi varsayar, ancak URL oluşturma, öznitelik yönlendirimiyle benzer şekilde çalışır, ancak kavramlar farklıdır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-319">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="e5f28-320">Geleneksel yönlendirme ile, bir şablonu genişletmek için yol değerleri kullanılır ve `controller` ve `action` için rota değerleri genellikle bu şablonda görünür-bu, yönlendirme ile eşleşen URL 'Ler bir *kurala*bağlı olduğundan, bu işe yarar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-320">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="e5f28-321">Öznitelik yönlendirmesinde, `controller` ve `action` için yol değerlerinin şablonda görünmesine izin verilmez; bunun yerine kullanılacak şablonu aramak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-321">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="e5f28-322">Bu örnek öznitelik yönlendirme kullanır:</span><span class="sxs-lookup"><span data-stu-id="e5f28-322">This example uses attribute routing:</span></span>

[!code-csharp[](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="e5f28-323">MVC, tüm öznitelik yönlendirilmiş eylemlerinin bir arama tablosunu oluşturur ve URL oluşturma için kullanılacak yol şablonunu seçmek üzere `controller` ve `action` değerleriyle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-323">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="e5f28-324">Yukarıdaki örnekte `custom/url/to/destination` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-324">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="e5f28-325">Eylem adına göre URL 'Leri oluşturma</span><span class="sxs-lookup"><span data-stu-id="e5f28-325">Generating URLs by action name</span></span>

<span data-ttu-id="e5f28-326">`Url.Action` (`IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="e5f28-326">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="e5f28-327">`Action`) ve tüm ilgili aşırı yüklemeler, bir denetleyici adı ve eylem adı belirterek ne bağlandığınızı belirtmek istediğinizi temel alır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-327">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="e5f28-328">`Url.Action`kullanırken, `controller` ve `action` için geçerli yol değerleri sizin için belirtilir; `controller` değeri, `action` hem *ortam değerlerinin* **hem** de *değerlerinin*bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-328">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="e5f28-329">`Url.Action`yöntemi her zaman `action` ve `controller` geçerli değerlerini kullanır ve geçerli eyleme yönlendiren bir URL yolu oluşturacaktır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-329">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="e5f28-330">Yönlendirme, bir URL oluştururken sağlamadığınız bilgileri doldurmanızı sağlamak için çevresel değerlerde değerleri kullanmayı dener.</span><span class="sxs-lookup"><span data-stu-id="e5f28-330">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="e5f28-331">Yönlendirme parametrelerinin bir değere sahip olduğundan, `{a}/{b}/{c}/{d}` ve çevresel değerler `{ a = Alice, b = Bob, c = Carol, d = David }`gibi bir yol kullanarak yönlendirme için ek değer olmadan bir URL oluşturmaya yetecek kadar bilgi vardır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-331">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="e5f28-332">Değer `{ d = Donovan }`eklediyseniz, `{ d = David }` değeri yok sayılır ve oluşturulan URL yolu `Alice/Bob/Carol/Donovan`olur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-332">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="e5f28-333">URL yolları hiyerarşiktir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-333">URL paths are hierarchical.</span></span> <span data-ttu-id="e5f28-334">Yukarıdaki örnekte, değeri `{ c = Cheryl }`eklediyseniz her iki değer de `{ c = Carol, d = David }` yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-334">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="e5f28-335">Bu durumda artık `d` için bir değer yoktur ve URL oluşturma başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-335">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="e5f28-336">İstediğiniz `c` ve `d`değerini belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-336">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="e5f28-337">Bu sorunu varsayılan yol (`{controller}/{action}/{id?}`) ile () beklemeniz gerekebilir; ancak, `Url.Action` her zaman açıkça bir `controller` ve `action` değeri belirtmesi gibi uygulamada bu davranış hakkında nadiren karşılaşacaksınız.</span><span class="sxs-lookup"><span data-stu-id="e5f28-337">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="e5f28-338">Daha uzun `Url.Action` aşırı yüklemeleri, `controller` ve `action`dışındaki rota parametreleri için değerler sağlamak üzere ek bir *yol değerleri* nesnesi de alır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-338">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="e5f28-339">Bu, en yaygın olarak `Url.Action("Buy", "Products", new { id = 17 })`gibi `id` kullanıldığını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="e5f28-339">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="e5f28-340">Kurala göre *yol değerleri* nesnesi genellikle anonim türdeki bir nesnedir, ancak bir `IDictionary<>` veya *düz bir .net nesnesi*de olabilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-340">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="e5f28-341">Yol parametreleriyle eşleşmeyen ek rota değerleri sorgu dizesine konur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-341">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="e5f28-342">Mutlak URL oluşturmak için, `protocol`kabul eden bir aşırı yükleme kullanın: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="e5f28-342">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="e5f28-343">Rotaya göre URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="e5f28-343">Generating URLs by route</span></span>

<span data-ttu-id="e5f28-344">Yukarıdaki kod, denetleyiciyi ve eylem adını geçirerek bir URL oluşturmayı göstermiştir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-344">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="e5f28-345">`IUrlHelper` Ayrıca `Url.RouteUrl` Yöntem ailesini da sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-345">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="e5f28-346">Bu yöntemler `Url.Action`benzerdir, ancak `action` ve `controller` geçerli değerlerini rota değerlerine kopyalamaz.</span><span class="sxs-lookup"><span data-stu-id="e5f28-346">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="e5f28-347">En yaygın kullanım, genellikle bir denetleyici veya eylem *adı belirtmeden,* URL oluşturmak için belirli bir yolu kullanmak üzere bir yol adı belirtmektir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-347">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="e5f28-348">HTML 'de URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="e5f28-348">Generating URLs in HTML</span></span>

<span data-ttu-id="e5f28-349">`IHtmlHelper`, `<form>` ve `<a>` öğeleri oluşturmak için `HtmlHelper` yöntemleri `Html.BeginForm` ve `Html.ActionLink` sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-349">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="e5f28-350">Bu yöntemler bir URL oluşturmak için `Url.Action` yöntemini kullanır ve benzer bağımsız değişkenleri kabul ederler.</span><span class="sxs-lookup"><span data-stu-id="e5f28-350">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="e5f28-351">`HtmlHelper` için `Url.RouteUrl` compan, benzer işlevlere sahip `Html.BeginRouteForm` ve `Html.RouteLink`.</span><span class="sxs-lookup"><span data-stu-id="e5f28-351">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="e5f28-352">Taghelmakalar, `form` TagHelper ve `<a>` TagHelper aracılığıyla URL 'Ler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-352">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="e5f28-353">Bunların her ikisi de kendi uygulamaları için `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="e5f28-353">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="e5f28-354">Daha fazla bilgi için bkz. [formlarla çalışma](../views/working-with-forms.md) .</span><span class="sxs-lookup"><span data-stu-id="e5f28-354">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="e5f28-355">Görünümler içinde `IUrlHelper`, yukarıdaki herhangi bir geçici URL nesli için `Url` özelliği aracılığıyla kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-355">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="e5f28-356">Eylem sonuçlarında URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="e5f28-356">Generating URLS in Action Results</span></span>

<span data-ttu-id="e5f28-357">Yukarıdaki örnekler, bir denetleyicide `IUrlHelper` kullanılarak gösterilmektedir, ancak denetleyicideki en yaygın kullanım, bir eylem sonucunun parçası olarak bir URL oluşturmak olur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-357">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="e5f28-358">`ControllerBase` ve `Controller` Taban sınıfları, başka bir eyleme başvuruda bulunan eylem sonuçları için kolay yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-358">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="e5f28-359">Tipik bir kullanım, Kullanıcı girişi kabul edildikten sonra yeniden yönlendirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-359">One typical usage is to redirect after accepting user input.</span></span>

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

<span data-ttu-id="e5f28-360">Eylem sonuçları Fabrika yöntemleri `IUrlHelper`yöntemlere benzer bir model izler.</span><span class="sxs-lookup"><span data-stu-id="e5f28-360">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="e5f28-361">Adanmış geleneksel yollar için özel durum</span><span class="sxs-lookup"><span data-stu-id="e5f28-361">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="e5f28-362">Geleneksel yönlendirme, *adanmış geleneksel yol*olarak adlandırılan özel bir yol tanımı türünü kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-362">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="e5f28-363">Aşağıdaki örnekte, `blog` adlı yol adanmış bir geleneksel yoldur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-363">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="e5f28-364">`Url.Action("Index", "Home")`, bu yol tanımlarını kullanarak `/` URL yolunu `default` rotası ile oluşturacak, ancak neden?</span><span class="sxs-lookup"><span data-stu-id="e5f28-364">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="e5f28-365">Yol değerlerini tahmin edebilirsiniz `{ controller = Home, action = Index }` `blog`kullanarak URL oluşturmak için yeterli olacaktır ve sonuç `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="e5f28-365">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="e5f28-366">Adanmış geleneksel yollar, URL oluşturmayla "çok Greedy" olmasını önleyen karşılık gelen bir yol parametresi olmayan varsayılan değerlerin özel bir davranışına bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-366">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="e5f28-367">Bu durumda, varsayılan değerler `{ controller = Blog, action = Article }`ve ne `controller` ne de `action` yol parametresi olarak görünmez.</span><span class="sxs-lookup"><span data-stu-id="e5f28-367">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="e5f28-368">Yönlendirme URL oluşturma işlemi gerçekleştirdiğinde, belirtilen değerler varsayılan değerlerle eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-368">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="e5f28-369">`blog` kullanılarak URL oluşturma başarısız olur çünkü değerler `{ controller = Home, action = Index }` `{ controller = Blog, action = Article }`eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="e5f28-369">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="e5f28-370">Ardından yönlendirme `default`denemeye geri döner ve başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-370">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="e5f28-371">Alanlar</span><span class="sxs-lookup"><span data-stu-id="e5f28-371">Areas</span></span>

<span data-ttu-id="e5f28-372">[Bölgeler](areas.md) , ilgili işlevselliği ayrı bir yönlendirme-ad alanı (denetleyici eylemleri için) ve klasör yapısı (görünümler için) olarak bir grupla düzenlemek için kullanılan bir MVC özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-372">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="e5f28-373">Alanların kullanılması, bir uygulamanın farklı *alanlara*sahip oldukları sürece aynı ada sahip birden çok denetleyicisi olmasına olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5f28-373">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="e5f28-374">Alanların kullanılması, başka bir yol parametresi ekleyerek yönlendirme amacına yönelik bir hiyerarşi oluşturur, `controller` ve `action``area`.</span><span class="sxs-lookup"><span data-stu-id="e5f28-374">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="e5f28-375">Bu bölüm, yönlendirmenin alanlarla nasıl etkileşime gireceğini tartışır. alanların görünümlerle nasıl kullanıldığı hakkında ayrıntılar için bkz. [alanlara](areas.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="e5f28-375">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="e5f28-376">Aşağıdaki örnek, MVC 'yi, `Blog`adlı bir alan için varsayılan geleneksel yolu ve bir *alan yolunu* kullanacak şekilde yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="e5f28-376">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="e5f28-377">`/Manage/Users/AddUser`gibi bir URL yolu eşleştirilirken, ilk yol `{ area = Blog, controller = Users, action = AddUser }`yol değerlerini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-377">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="e5f28-378">`area` yol değeri, `area`için varsayılan bir değer tarafından üretilir, aslında `MapAreaRoute` tarafından oluşturulan yol, aşağıdaki değere eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="e5f28-378">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="e5f28-379">`MapAreaRoute`, `area` için hem varsayılan değer hem de kısıtlama (Bu durumda `Blog`) kullanarak bir yol oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-379">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="e5f28-380">Varsayılan değer, yolun her zaman `{ area = Blog, ... }`üretmesini sağlar, kısıtlama, URL oluşturma için `{ area = Blog, ... }` değer gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-380">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="e5f28-381">Geleneksel yönlendirme sıra bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-381">Conventional routing is order-dependent.</span></span> <span data-ttu-id="e5f28-382">Genel olarak, alanlar içeren rotalar, alan olmayan rotalardan daha belirgin olduklarından daha önce rota tablosuna yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-382">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="e5f28-383">Yukarıdaki örneği kullanarak, yol değerleri aşağıdaki eylemle eşleşir:</span><span class="sxs-lookup"><span data-stu-id="e5f28-383">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="e5f28-384">`AreaAttribute`, bir alanın parçası olarak denetleyiciyi belirtir, bu denetleyicinin `Blog` alanında olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="e5f28-384">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="e5f28-385">`[Area]` özniteliği olmayan denetleyiciler hiçbir alanın üyesi değildir ve `area` yol değeri yönlendirme tarafından sağlandığında **eşleşmeyecektir** .</span><span class="sxs-lookup"><span data-stu-id="e5f28-385">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="e5f28-386">Aşağıdaki örnekte, yalnızca listelenen ilk denetleyici `{ area = Blog, controller = Users, action = AddUser }`rota değerleriyle eşleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-386">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="e5f28-387">Her denetleyicinin ad alanı, tamamlanma için burada gösterilir. Aksi takdirde, denetleyicilerde adlandırma çakışması olur ve derleyici hatası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-387">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="e5f28-388">Sınıf ad alanlarının MVC 'nin yönlendirme üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-388">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="e5f28-389">İlk iki denetleyici alanların üyeleridir ve yalnızca ilgili alan adı `area` rota değeri tarafından sağlandığında eşleşir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-389">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="e5f28-390">Üçüncü denetleyici hiçbir alanın üyesi değildir ve yalnızca Yönlendirme tarafından `area` hiçbir değer sağlanmıyorsa eşleşemez.</span><span class="sxs-lookup"><span data-stu-id="e5f28-390">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="e5f28-391">*Değer olmadan*eşleşme açısından, `area` değerinin yokluğu, `area` değeri null ya da boş dize olarak aynıdır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-391">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="e5f28-392">Bir alan içinde bir eylem yürütürken, `area` için rota değeri, yönlendirme için, URL oluşturma için kullanılacak *çevresel bir değer* olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-392">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="e5f28-393">Bu, varsayılan olarak, aşağıdaki örnekte gösterildiği gibi, URL oluşturma için *yapışkan* olarak hareket ettiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-393">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="e5f28-394">Iactionconstraint 'i anlama</span><span class="sxs-lookup"><span data-stu-id="e5f28-394">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="e5f28-395">Bu bölüm, Framework iç işlevleri hakkında ayrıntılı bir bakış ve MVC 'nin yürütülecek eylemi nasıl seçtiği.</span><span class="sxs-lookup"><span data-stu-id="e5f28-395">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="e5f28-396">Tipik bir uygulama özel bir `IActionConstraint` gerektirmez</span><span class="sxs-lookup"><span data-stu-id="e5f28-396">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="e5f28-397">Büyük olasılıkla, arabirime tanıdık olmasanız bile `IActionConstraint` zaten kullandık.</span><span class="sxs-lookup"><span data-stu-id="e5f28-397">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="e5f28-398">`[HttpGet]` özniteliği ve benzer `[Http-VERB]` öznitelikleri, bir eylem yönteminin yürütülmesini sınırlandırmak için `IActionConstraint` uygular.</span><span class="sxs-lookup"><span data-stu-id="e5f28-398">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="e5f28-399">Varsayılan geleneksel yolun kabul edilmesinden, URL yolunun `/Products/Edit`, burada gösterilen eylemlerle **her ikisi de** eşleşen değerler `{ controller = Products, action = Edit }`üretecektir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-399">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="e5f28-400">`IActionConstraint` terminolojisinde, her ikisi de rota verileriyle eşleştiğinden, bu eylemlerin her ikisi de aday olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-400">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="e5f28-401">`HttpGetAttribute` yürütüldüğünde, *Edit ()* , *Get* için bir EŞLEŞMEDIR ve diğer http fiili için bir eşleşme değildir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-401">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="e5f28-402">`Edit(...)` eyleminde tanımlı kısıtlama yok ve bu nedenle herhangi bir HTTP fiili ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-402">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="e5f28-403">Bu nedenle, yalnızca `POST` bir `Edit(...)` eşleştiğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="e5f28-403">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="e5f28-404">Ancak `GET` için her iki eylem de eşleşemez, ancak `IActionConstraint` bir eylem, olmadan bir eylemden en *iyi* şekilde değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-404">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="e5f28-405">Bu nedenle `Edit()`, `[HttpGet]` daha belirgin olarak değerlendirilir ve her iki eylemin da eşleşeceğinden seçilecek.</span><span class="sxs-lookup"><span data-stu-id="e5f28-405">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="e5f28-406">Kavramsal olarak, `IActionConstraint` *aşırı yükleme*biçimidir, ancak aynı ada sahip yöntemlerin aşırı yüklenmesi yerıne aynı URL ile eşleşen eylemler arasında aşırı yüklenir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-406">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="e5f28-407">Öznitelik yönlendirme `IActionConstraint` de kullanır ve farklı denetleyicilerden gelen eylemlere her ikisi de aday olarak kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-407">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="e5f28-408">Iactionconstraint uygulama</span><span class="sxs-lookup"><span data-stu-id="e5f28-408">Implementing IActionConstraint</span></span>

<span data-ttu-id="e5f28-409">`IActionConstraint` kullanmanın en kolay yolu, `System.Attribute` türetilmiş bir sınıf oluşturmaktır ve bunları eylemleriniz ve denetleyicilerinize yerleştirmelidir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-409">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="e5f28-410">MVC, öznitelik olarak uygulanan `IActionConstraint` otomatik olarak bulur.</span><span class="sxs-lookup"><span data-stu-id="e5f28-410">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="e5f28-411">Kısıtlama uygulamak için uygulama modelini kullanabilirsiniz ve bu, büyük olasılıkla en esnek yaklaşımdır ve bu sayede, nasıl uygulanabileceğini meta programlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5f28-411">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="e5f28-412">Aşağıdaki örnekte bir kısıtlama, rota verilerinden bir *ülke kodunu* temel alan bir eylem seçer.</span><span class="sxs-lookup"><span data-stu-id="e5f28-412">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="e5f28-413">[GitHub 'daki tam örnek](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="e5f28-413">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

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

<span data-ttu-id="e5f28-414">`Accept` yöntemi uygulamaktan ve kısıtlamanın yürütülmesi için bir ' Order ' seçmeye sorumlusunuz.</span><span class="sxs-lookup"><span data-stu-id="e5f28-414">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="e5f28-415">Bu durumda `Accept` yöntemi, `country` rota değeri eşleştiğinde eylemin bir eşleşme olduğunu göstermek için `true` döndürür.</span><span class="sxs-lookup"><span data-stu-id="e5f28-415">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="e5f28-416">Bu, varolmayan bir eyleme geri dönüş sağlayan bir `RouteValueAttribute` farklıdır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-416">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="e5f28-417">Örnek, bir `en-US` eylemi tanımlarsanız `fr-FR` gibi bir ülke kodunun `[CountrySpecific(...)]` uygulanmamış daha genel bir denetleyiciye geri dönemeyeceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="e5f28-417">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="e5f28-418">`Order` özelliği, kısıtlamanın parçası olan *aşamayı* belirler.</span><span class="sxs-lookup"><span data-stu-id="e5f28-418">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="e5f28-419">Eylem kısıtlamaları `Order`göre gruplar halinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-419">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="e5f28-420">Örneğin, tüm Framework tarafından sunulan HTTP yöntemi öznitelikleri aynı aşamada çalışacak şekilde aynı `Order` değerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-420">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="e5f28-421">İstediğiniz ilkeleri uygulamak için ihtiyacınız olan çok sayıda aşamaya sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5f28-421">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="e5f28-422">`Order` bir değere karar vermek için, kısıtlamalarınızın HTTP yöntemlerinden önce uygulanıp uygulanmayacağı hakkında düşünün.</span><span class="sxs-lookup"><span data-stu-id="e5f28-422">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="e5f28-423">Daha az sayı önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="e5f28-423">Lower numbers run first.</span></span>
