---
title: ASP.NET Core denetleyici eylemlerine yönlendirme
author: rick-anderson
description: ASP.NET Core MVC 'nin, gelen isteklerin URL 'Lerini eşleştirmek ve bunları eylemlerle eşlemek için yönlendirme ara yazılımını nasıl kullandığını öğrenin.
ms.author: riande
ms.date: 3/25/2020
uid: mvc/controllers/routing
ms.openlocfilehash: be7da9eeaf64c2f52c095b5179ccc22db43d57c3
ms.sourcegitcommit: 99e71ae03319ab386baf2ebde956fc2d511df8b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80242587"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a><span data-ttu-id="4884b-103">ASP.NET Core denetleyici eylemlerine yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4884b-103">Routing to controller actions in ASP.NET Core</span></span>

<span data-ttu-id="4884b-104">[Ryan şimdi ak](https://github.com/rynowak), [Kirk Larkabağı](https://twitter.com/serpent5)ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4884b-104">By [Ryan Nowak](https://github.com/rynowak), [Kirk Larkin](https://twitter.com/serpent5), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4884b-105">ASP.NET Core denetleyicileri, gelen isteklerin URL 'Leriyle eşleştirmek ve bunları [eylemlerle](#action)eşlemek için yönlendirme [Ara yazılımını](xref:fundamentals/middleware/index) kullanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-105">ASP.NET Core controllers use the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to [actions](#action).</span></span>  <span data-ttu-id="4884b-106">Rota şablonları:</span><span class="sxs-lookup"><span data-stu-id="4884b-106">Routes templates:</span></span>

* <span data-ttu-id="4884b-107">, Başlangıç kodunda veya özniteliklerde tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4884b-107">Are defined in startup code or attributes.</span></span>
* <span data-ttu-id="4884b-108">URL yollarının [eylemlerle](#action)nasıl eşleştiğini betimleyen.</span><span class="sxs-lookup"><span data-stu-id="4884b-108">Describe how URL paths are matched to [actions](#action).</span></span>
* <span data-ttu-id="4884b-109">Bağlantıların URL 'Leri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-109">Are used to generate URLs for links.</span></span> <span data-ttu-id="4884b-110">Oluşturulan bağlantılar genellikle yanıtlar halinde döndürülür.</span><span class="sxs-lookup"><span data-stu-id="4884b-110">The generated links are typically returned in responses.</span></span>

<span data-ttu-id="4884b-111">Eylemler [genel olarak-yönlendirildi](#cr) veya [Attribute-yönlendirildi](#ar)' dir.</span><span class="sxs-lookup"><span data-stu-id="4884b-111">Actions are either [conventionally-routed](#cr) or [attribute-routed](#ar).</span></span> <span data-ttu-id="4884b-112">Bir yolun denetleyiciye veya [eyleme](#action) yerleştirilmesi, BT özniteliğinin yönlendirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-112">Placing a route on the controller or [action](#action) makes it attribute-routed.</span></span> <span data-ttu-id="4884b-113">Daha fazla bilgi için bkz. [karma yönlendirme](#routing-mixed-ref-label) .</span><span class="sxs-lookup"><span data-stu-id="4884b-113">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="4884b-114">Bu belge:</span><span class="sxs-lookup"><span data-stu-id="4884b-114">This document:</span></span>

* <span data-ttu-id="4884b-115">MVC ve yönlendirme arasındaki etkileşimleri açıklar:</span><span class="sxs-lookup"><span data-stu-id="4884b-115">Explains the interactions between MVC and routing:</span></span>
  * <span data-ttu-id="4884b-116">Tipik MVC uygulamalarının yönlendirme özelliklerini kullanma şekli.</span><span class="sxs-lookup"><span data-stu-id="4884b-116">How typical MVC apps make use of routing features.</span></span>
  * <span data-ttu-id="4884b-117">Her ikisini de içerir:</span><span class="sxs-lookup"><span data-stu-id="4884b-117">Covers both:</span></span>
    * <span data-ttu-id="4884b-118">Genellikle denetleyiciler ve görünümlerle kullanılan [genel olarak yönlendirme](#cr) .</span><span class="sxs-lookup"><span data-stu-id="4884b-118">[Conventionally routing](#cr) typically used with controllers and views.</span></span>
    * <span data-ttu-id="4884b-119">REST API 'Leri ile kullanılan *öznitelik yönlendirme* .</span><span class="sxs-lookup"><span data-stu-id="4884b-119">*Attribute routing* used with REST APIs.</span></span> <span data-ttu-id="4884b-120">Birincil olarak REST API 'Leri için yönlendirme ile ilgileniyorsanız, [REST API 'leri Için öznitelik yönlendirme](#ar) bölümüne atlayın.</span><span class="sxs-lookup"><span data-stu-id="4884b-120">If you're primarily interested in routing for REST APIs, jump to the [Attribute routing for REST APIs](#ar) section.</span></span>
  * <span data-ttu-id="4884b-121">Bkz. Gelişmiş yönlendirme ayrıntıları için [yönlendirme](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="4884b-121">See [Routing](xref:fundamentals/routing) for advanced routing details.</span></span>
* <span data-ttu-id="4884b-122">ASP.NET Core 3,0 ' de eklenen varsayılan yönlendirme sisteminin Endpoint Routing olarak adlandırıldığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="4884b-122">Refers to the default routing system added in ASP.NET Core 3.0, called endpoint routing.</span></span> <span data-ttu-id="4884b-123">Uyumluluk amaçlarıyla önceki yönlendirme sürümü ile denetleyicileri kullanmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4884b-123">It's possible to use controllers with the previous version of routing for compatibility purposes.</span></span> <span data-ttu-id="4884b-124">Yönergeler için [2.2-3.0 geçiş kılavuzuna](xref:migration/22-to-30) bakın.</span><span class="sxs-lookup"><span data-stu-id="4884b-124">See the [2.2-3.0 migration guide](xref:migration/22-to-30) for instructions.</span></span> <span data-ttu-id="4884b-125">Eski yönlendirme sistemindeki başvuru malzemeleri için [Bu belgenin 2,2 sürümüne](xref:mvc/controllers/routing?view=aspnetcore-2.2) bakın.</span><span class="sxs-lookup"><span data-stu-id="4884b-125">Refer to the [2.2 version of this document](xref:mvc/controllers/routing?view=aspnetcore-2.2) for reference material on the legacy routing system.</span></span>

<a name="cr"></a>

## <a name="set-up-conventional-route"></a><span data-ttu-id="4884b-126">Geleneksel rotayı ayarlama</span><span class="sxs-lookup"><span data-stu-id="4884b-126">Set up conventional route</span></span>

<span data-ttu-id="4884b-127">`Startup.Configure` [geleneksel yönlendirme](#crd)kullanılırken, genellikle aşağıdakine benzer bir kod içerir:</span><span class="sxs-lookup"><span data-stu-id="4884b-127">`Startup.Configure` typically has code similar to the following when using [conventional routing](#crd):</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet)]

<span data-ttu-id="4884b-128"><xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>çağrısının içinde, tek bir yol oluşturmak için <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-128">Inside the call to <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> is used to create a single route.</span></span> <span data-ttu-id="4884b-129">Tek yol `default` yol olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-129">The single route is named `default` route.</span></span> <span data-ttu-id="4884b-130">Denetleyiciler ve görünümler içeren çoğu uygulama, `default` yoluna benzer bir yol şablonu kullanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-130">Most apps with controllers and views use a route template similar to the `default` route.</span></span> <span data-ttu-id="4884b-131">REST API 'Leri [öznitelik yönlendirmeyi](#ar)kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-131">REST APIs should use [attribute routing](#ar).</span></span>

<span data-ttu-id="4884b-132">Yol şablonu `"{controller=Home}/{action=Index}/{id?}"`:</span><span class="sxs-lookup"><span data-stu-id="4884b-132">The route template `"{controller=Home}/{action=Index}/{id?}"`:</span></span>

* <span data-ttu-id="4884b-133">`/Products/Details/5` gibi bir URL yoluyla eşleşir</span><span class="sxs-lookup"><span data-stu-id="4884b-133">Matches a URL path like `/Products/Details/5`</span></span>
* <span data-ttu-id="4884b-134">Yolu simgeleyerek `{ controller = Products, action = Details, id = 5 }` yol değerlerini ayıklar.</span><span class="sxs-lookup"><span data-stu-id="4884b-134">Extracts the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="4884b-135">Uygulamanın `ProductsController` adlı bir denetleyici ve `Details` eylemi varsa yol değerlerinin ayıklanması bir eşleşme ile sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="4884b-135">The extraction of route values results in a match if the app has a controller named `ProductsController` and a `Details` action:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippetA)]

 <span data-ttu-id="4884b-136">[Mydisplayrouteınfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) yöntemi [örnek karşıdan yüklemeye](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) dahildir ve yönlendirme bilgilerini görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-136">The [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) method is included in the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) and is used to display routing information.</span></span>

  * <span data-ttu-id="4884b-137">`/Products/Details/5` model `id` parametresini `5`olarak ayarlamak için `id = 5` değerini bağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-137">`/Products/Details/5` model binds the value of `id = 5` to set the `id` parameter to `5`.</span></span> <span data-ttu-id="4884b-138">Daha fazla ayrıntı için bkz. [model bağlama](xref:mvc/models/model-binding) .</span><span class="sxs-lookup"><span data-stu-id="4884b-138">See [Model Binding](xref:mvc/models/model-binding) for more details.</span></span>
* <span data-ttu-id="4884b-139">`{controller=Home}` varsayılan `controller`olarak `Home` tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-139">`{controller=Home}` defines `Home` as the default `controller`.</span></span>
* <span data-ttu-id="4884b-140">`{action=Index}` varsayılan `action`olarak `Index` tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-140">`{action=Index}` defines `Index` as the default `action`.</span></span>
*  <span data-ttu-id="4884b-141">`{id?}` `?` karakteri `id` isteğe bağlı olarak tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-141">The `?` character in `{id?}` defines `id` as optional.</span></span>
  * <span data-ttu-id="4884b-142">Bir eşleşme için URL yolunda varsayılan ve isteğe bağlı yol parametrelerinin mevcut olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4884b-142">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="4884b-143">Yol şablonu sözdiziminin ayrıntılı açıklaması için bkz. [route Template Reference](xref:fundamentals/routing#route-template-reference) .</span><span class="sxs-lookup"><span data-stu-id="4884b-143">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>
* <span data-ttu-id="4884b-144">URL yolu `/`eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-144">Matches the URL path `/`.</span></span>
* <span data-ttu-id="4884b-145">`{ controller = Home, action = Index }`yol değerlerini üretir.</span><span class="sxs-lookup"><span data-stu-id="4884b-145">Produces the route values `{ controller = Home, action = Index }`.</span></span>
* <span data-ttu-id="4884b-146">[Mydisplayrouteınfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) yöntemi [örnek karşıdan yüklemeye](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) dahildir ve yönlendirme bilgilerini görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-146">The [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) method is included in the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) and is used to display routing information.</span></span>

<span data-ttu-id="4884b-147">`controller` ve `action` değerleri varsayılan değerleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-147">The values for `controller` and `action` make use of the default values.</span></span> <span data-ttu-id="4884b-148">URL yolunda karşılık gelen bir kesim olmadığından `id` bir değer oluşturmuyor.</span><span class="sxs-lookup"><span data-stu-id="4884b-148">`id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="4884b-149">`/` yalnızca bir `HomeController` ve `Index` eylemi varsa eşleşir:</span><span class="sxs-lookup"><span data-stu-id="4884b-149">`/` only matches if there exists a `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="4884b-150">Önceki denetleyici tanımı ve yönlendirme şablonunu kullanarak, aşağıdaki URL yolları için `HomeController.Index` eylemi çalıştırılır:</span><span class="sxs-lookup"><span data-stu-id="4884b-150">Using the preceding controller definition and route template, the `HomeController.Index` action is run for the following URL paths:</span></span>

* `/Home/Index/17`
* `/Home/Index`
* `/Home`
* `/`

<span data-ttu-id="4884b-151">`/` URL yolu, yönlendirme şablonu varsayılan `Home` denetleyicilerini ve `Index` eylemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-151">The URL path `/` uses the route template default `Home` controllers and `Index` action.</span></span> <span data-ttu-id="4884b-152">URL yolu `/Home` yol şablonu varsayılan `Index` eylemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-152">The URL path `/Home` uses the route template default `Index` action.</span></span>

<span data-ttu-id="4884b-153">Kolaylık yöntemi <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>:</span><span class="sxs-lookup"><span data-stu-id="4884b-153">The convenience method <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>:</span></span>

```csharp
endpoints.MapDefaultControllerRoute();
```

<span data-ttu-id="4884b-154">Yerine</span><span class="sxs-lookup"><span data-stu-id="4884b-154">Replaces:</span></span>

```csharp
endpoints.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="4884b-155">Yönlendirme, <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> ve <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> ara yazılımı kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-155">Routing is configured using the <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> middleware.</span></span> <span data-ttu-id="4884b-156">Denetleyicileri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="4884b-156">To use controllers:</span></span>

* <span data-ttu-id="4884b-157">[Öznitelik yönlendirmeli](#ar) denetleyicileri eşlemek için `UseEndpoints` içinde <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="4884b-157">Call <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> inside `UseEndpoints` to map [attribute routed](#ar) controllers.</span></span>
* <span data-ttu-id="4884b-158">[Genel olarak yönlendirmeli](#cr) denetleyicileri eşlemek için <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> veya <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>çağırın.</span><span class="sxs-lookup"><span data-stu-id="4884b-158">Call <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> or <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>, to map [conventionally routed](#cr) controllers.</span></span>

<a name="routing-conventional-ref-label"></a>
<a name="crd"></a>

## <a name="conventional-routing"></a><span data-ttu-id="4884b-159">Geleneksel yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4884b-159">Conventional routing</span></span>

<span data-ttu-id="4884b-160">Geleneksel yönlendirme, denetleyiciler ve görünümlerle kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-160">Conventional routing is used with controllers and views.</span></span> <span data-ttu-id="4884b-161">`default` yolu:</span><span class="sxs-lookup"><span data-stu-id="4884b-161">The `default` route:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet2)]

<span data-ttu-id="4884b-162">, *geleneksel yönlendirmeye*bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="4884b-162">is an example of a *conventional routing*.</span></span> <span data-ttu-id="4884b-163">Bu, URL yolları için bir *kural* oluşturduğundan *geleneksel yönlendirme* olarak adlandırılır:</span><span class="sxs-lookup"><span data-stu-id="4884b-163">It's called *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="4884b-164">İlk yol segmenti `{controller=Home}`, denetleyicinin adıyla eşlenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-164">The first path segment, `{controller=Home}`, maps to the controller name.</span></span>
* <span data-ttu-id="4884b-165">İkinci kesim `{action=Index}`, [eylem](#action) adıyla eşlenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-165">The second segment, `{action=Index}`, maps to the [action](#action) name.</span></span>
* <span data-ttu-id="4884b-166">Üçüncü segment `{id?}`, isteğe bağlı bir `id`için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-166">The third segment, `{id?}` is used for an optional `id`.</span></span> <span data-ttu-id="4884b-167">`{id?}` `?`, isteğe bağlı hale getirir.</span><span class="sxs-lookup"><span data-stu-id="4884b-167">The `?` in `{id?}` makes it optional.</span></span> <span data-ttu-id="4884b-168">`id`, bir model varlığına eşlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-168">`id` is used to map to a model entity.</span></span>

<span data-ttu-id="4884b-169">Bu `default` yolunu kullanarak, URL yolu:</span><span class="sxs-lookup"><span data-stu-id="4884b-169">Using this `default` route, the URL path:</span></span>

* <span data-ttu-id="4884b-170">`/Products/List` `ProductsController.List` eylemine eşlenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-170">`/Products/List` maps to the `ProductsController.List` action.</span></span>
* <span data-ttu-id="4884b-171">`/Blog/Article/17` `BlogController.Article` eşlenir ve genellikle model `id` parametresini 17 ' ye bağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-171">`/Blog/Article/17` maps to `BlogController.Article` and typically model binds the `id` parameter to 17.</span></span>

<span data-ttu-id="4884b-172">Bu eşleme:</span><span class="sxs-lookup"><span data-stu-id="4884b-172">This mapping:</span></span>

* <span data-ttu-id="4884b-173">**Yalnızca**denetleyiciyi ve [eylem](#action) adlarını temel alır.</span><span class="sxs-lookup"><span data-stu-id="4884b-173">Is based on the controller and [action](#action) names **only**.</span></span>
* <span data-ttu-id="4884b-174">Ad alanlarını, kaynak dosya konumlarını veya yöntem parametrelerini temel alan değildir.</span><span class="sxs-lookup"><span data-stu-id="4884b-174">Isn't based on namespaces, source file locations, or method parameters.</span></span>

<span data-ttu-id="4884b-175">Varsayılan yol ile geleneksel yönlendirmeyi kullanmak, her eylem için yeni bir URL düzeniyle karşılaşmadan uygulamanın oluşturulmasına olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-175">Using conventional routing with the default route allows creating the app without having to come up with a new URL pattern for each action.</span></span> <span data-ttu-id="4884b-176">[CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) stilinde eylemlere sahip bir uygulama için, denetleyiciler arasında URL 'ler için tutarlılık vardır:</span><span class="sxs-lookup"><span data-stu-id="4884b-176">For an app with [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) style actions, having consistency for the URLs across controllers:</span></span>

* <span data-ttu-id="4884b-177">Kodu basitleştirmeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="4884b-177">Helps simplify the code.</span></span>
* <span data-ttu-id="4884b-178">Kullanıcı arabirimini daha öngörülebilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="4884b-178">Makes the UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="4884b-179">Yukarıdaki koddaki `id`, yol şablonu tarafından isteğe bağlı olarak tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4884b-179">The `id` in the preceding code is defined as optional by the route template.</span></span> <span data-ttu-id="4884b-180">Eylemler, URL 'nin bir parçası olarak belirtilen isteğe bağlı KIMLIK olmadan çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-180">Actions can execute without the optional ID provided as part of the URL.</span></span> <span data-ttu-id="4884b-181">Genellikle, URL 'den`id` atlandığında:</span><span class="sxs-lookup"><span data-stu-id="4884b-181">Generally, when`id` is omitted from the URL:</span></span>
>
> * <span data-ttu-id="4884b-182">`id` model bağlamaya göre `0` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-182">`id` is set to `0` by model binding.</span></span>
> * <span data-ttu-id="4884b-183">Veritabanında eşleşen `id == 0`varlık bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="4884b-183">No entity is found in the database matching `id == 0`.</span></span>
>
> <span data-ttu-id="4884b-184">[Öznitelik yönlendirme](#ar) , kimliği bazı eylemler için gerekli hale getirmek için ayrıntılı denetim sağlar ve diğerleri için değildir.</span><span class="sxs-lookup"><span data-stu-id="4884b-184">[Attribute routing](#ar) provides fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="4884b-185">Kurala göre belgeler, doğru kullanımlarda görünmeme olasılığı olan `id` gibi isteğe bağlı parametreler içerir.</span><span class="sxs-lookup"><span data-stu-id="4884b-185">By convention, the documentation includes optional parameters like `id` when they're likely to appear in correct usage.</span></span>

<span data-ttu-id="4884b-186">Çoğu uygulama, URL 'Lerin okunabilir ve anlamlı olması için temel ve açıklayıcı bir yönlendirme şeması seçmelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-186">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="4884b-187">Varsayılan geleneksel yol `{controller=Home}/{action=Index}/{id?}`:</span><span class="sxs-lookup"><span data-stu-id="4884b-187">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="4884b-188">Temel ve açıklayıcı bir yönlendirme düzenini destekler.</span><span class="sxs-lookup"><span data-stu-id="4884b-188">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="4884b-189">, UI tabanlı uygulamalar için kullanışlı bir başlangıç noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-189">Is a useful starting point for UI-based apps.</span></span>
* <span data-ttu-id="4884b-190">Birçok Web UI uygulaması için tek yol şablonu gereklidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-190">Is the only route template needed for many web UI apps.</span></span> <span data-ttu-id="4884b-191">Daha büyük Web Kullanıcı arabirimi uygulamaları için, sık sık gerekli olan [alanlarda](#areas) başka bir yol kullanın.</span><span class="sxs-lookup"><span data-stu-id="4884b-191">For larger web UI apps, another route using [Areas](#areas) if frequently all that's needed.</span></span>

<span data-ttu-id="4884b-192"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> ve <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*>:</span><span class="sxs-lookup"><span data-stu-id="4884b-192"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> and <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> :</span></span>

* <span data-ttu-id="4884b-193">, Çağrıldığı sıraya göre kendi uç noktalarına otomatik olarak bir **sipariş** değeri atayın.</span><span class="sxs-lookup"><span data-stu-id="4884b-193">Automatically assign an **order** value to their endpoints based on the order they are invoked.</span></span>

<span data-ttu-id="4884b-194">ASP.NET Core 3,0 ve üzeri için uç nokta yönlendirme:</span><span class="sxs-lookup"><span data-stu-id="4884b-194">Endpoint routing in ASP.NET Core 3.0 and later:</span></span>

* <span data-ttu-id="4884b-195">Bir yol kavramı yoktur.</span><span class="sxs-lookup"><span data-stu-id="4884b-195">Doesn't have a concept of routes.</span></span>
* <span data-ttu-id="4884b-196">, Genişletilebilirlik yürütülmesi için sıralama garantisi sağlamaz, tüm uç noktalar bir kerede işlenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-196">Doesn't provide ordering guarantees for the execution of extensibility,  all endpoints are processed at once.</span></span>

<span data-ttu-id="4884b-197"><xref:Microsoft.AspNetCore.Routing.Route>, istekleri eşleştirme gibi yerleşik yönlendirme uygulamalarının nasıl yapıldığını görmek için [günlük kaydını](xref:fundamentals/logging/index) etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="4884b-197">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

<span data-ttu-id="4884b-198">[Öznitelik yönlendirme](#ar) bu belgenin ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4884b-198">[Attribute routing](#ar) is explained later in this document.</span></span>

<a name="mr"></a>

### <a name="multiple-conventional-routes"></a><span data-ttu-id="4884b-199">Birden çok geleneksel yollar</span><span class="sxs-lookup"><span data-stu-id="4884b-199">Multiple conventional routes</span></span>

<span data-ttu-id="4884b-200"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> ve <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>daha fazla çağrı eklenerek `UseEndpoints` içine birden çok [geleneksel yol](#cr) eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-200">Multiple [conventional routes](#cr) can be added inside `UseEndpoints` by adding more calls to <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> and <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>.</span></span> <span data-ttu-id="4884b-201">Bunun yapılması, birden çok kural tanımlamayı veya belirli bir [eyleme](#action)adanmış geleneksel yollar eklemeyi sağlar; örneğin:</span><span class="sxs-lookup"><span data-stu-id="4884b-201">Doing so allows defining multiple conventions, or to adding conventional routes that are dedicated to a specific [action](#action), such as:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<a name="dcr"></a>

<span data-ttu-id="4884b-202">Yukarıdaki koddaki `blog` yolu, **adanmış bir geleneksel yoldur**.</span><span class="sxs-lookup"><span data-stu-id="4884b-202">The `blog` route in the preceding code is a **dedicated conventional route**.</span></span> <span data-ttu-id="4884b-203">Ayrılmış bir geleneksel yol olarak adlandırılır çünkü:</span><span class="sxs-lookup"><span data-stu-id="4884b-203">It's called a dedicated conventional route because:</span></span>

* <span data-ttu-id="4884b-204">[Geleneksel yönlendirme](#cr)kullanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-204">It uses [conventional routing](#cr).</span></span>
* <span data-ttu-id="4884b-205">Belirli bir [eyleme](#action)ayrılmıştır.</span><span class="sxs-lookup"><span data-stu-id="4884b-205">It's dedicated to a specific [action](#action).</span></span>

<span data-ttu-id="4884b-206">`controller` ve `action`, yol şablonunda `"blog/{*article}"` parametre olarak görünmüyor:</span><span class="sxs-lookup"><span data-stu-id="4884b-206">Because `controller` and `action` don't appear in the route template `"blog/{*article}"` as parameters:</span></span>

* <span data-ttu-id="4884b-207">Yalnızca varsayılan değerlere sahip olabilirler `{ controller = "Blog", action = "Article" }`.</span><span class="sxs-lookup"><span data-stu-id="4884b-207">They can only have the default values `{ controller = "Blog", action = "Article" }`.</span></span>
* <span data-ttu-id="4884b-208">Bu yol her zaman `BlogController.Article`işlem ile eşlenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-208">This route always maps to the action `BlogController.Article`.</span></span>

<span data-ttu-id="4884b-209">`/Blog`, `/Blog/Article`ve `/Blog/{any-string}`, blog rotası ile eşleşen tek URL yollarıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-209">`/Blog`, `/Blog/Article`, and `/Blog/{any-string}` are the only URL paths that match the blog route.</span></span>

<span data-ttu-id="4884b-210">Önceki örnek:</span><span class="sxs-lookup"><span data-stu-id="4884b-210">The preceding example:</span></span>

* <span data-ttu-id="4884b-211">`blog` yol, ilk eklendiği için `default` rotasına kıyasla daha yüksek önceliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4884b-211">`blog` route has a higher priority for matches than the `default` route because it is added first.</span></span>
* <span data-ttu-id="4884b-212">Ve, URL 'nin bir parçası olarak bir makale adı olması gereken tipik bir [başlık](https://developer.mozilla.org/docs/Glossary/Slug) stili yönlendirme örneğidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-212">Is and example of [Slug](https://developer.mozilla.org/docs/Glossary/Slug) style routing where it's typical to have an article name as part of the URL.</span></span>

> [!WARNING]
> <span data-ttu-id="4884b-213">ASP.NET Core 3,0 ve üzeri sürümlerde yönlendirme:</span><span class="sxs-lookup"><span data-stu-id="4884b-213">In ASP.NET Core 3.0 and later, routing doesn't:</span></span>
> * <span data-ttu-id="4884b-214">*Yol*adlı bir kavram tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="4884b-214">Define a concept called a *route*.</span></span> <span data-ttu-id="4884b-215">`UseRouting`, ara yazılım ardışık düzenine eşleşen rota ekler.</span><span class="sxs-lookup"><span data-stu-id="4884b-215">`UseRouting` adds route matching to the middleware pipeline.</span></span> <span data-ttu-id="4884b-216">`UseRouting` ara yazılımı uygulamada tanımlanan uç noktalar kümesine bakar ve isteğe bağlı olarak en iyi uç nokta eşleşmesini seçer.</span><span class="sxs-lookup"><span data-stu-id="4884b-216">The `UseRouting` middleware looks at the set of endpoints defined in the app, and selects the best endpoint match based on the request.</span></span>
> * <span data-ttu-id="4884b-217"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> veya <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>gibi genişletilebilirlik yürütme sırası hakkında garanti sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-217">Provide guarantees about the execution order of extensibility like <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> or <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>.</span></span>
>
><span data-ttu-id="4884b-218">Bkz. yönlendirme ile ilgili başvuru malzemeleri için [yönlendirme](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="4884b-218">See [Routing](xref:fundamentals/routing) for reference material on routing.</span></span>

<a name="cro"></a>

### <a name="conventional-routing-order"></a><span data-ttu-id="4884b-219">Geleneksel yönlendirme sırası</span><span class="sxs-lookup"><span data-stu-id="4884b-219">Conventional routing order</span></span>

<span data-ttu-id="4884b-220">Geleneksel yönlendirme yalnızca uygulama tarafından tanımlanan eylem ve denetleyicinin bir bileşimiyle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-220">Conventional routing only matches a combination of action and controller that are defined by the app.</span></span> <span data-ttu-id="4884b-221">Bu, geleneksel yolların çakıştığı durumları basitleştirmek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4884b-221">This is intended to simplify cases where conventional routes overlap.</span></span>
<span data-ttu-id="4884b-222"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>ve <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> kullanarak yollar eklendiğinde, çağrıldığı sıraya göre uç noktalarına otomatik olarak bir Order değeri atanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-222">Adding routes using <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>, and <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> automatically assign an order value to their endpoints based on the order they are invoked.</span></span> <span data-ttu-id="4884b-223">Daha önce görüntülenen bir rotadaki eşleşmelerin önceliği daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="4884b-223">Matches from a route that appears earlier have a higher priority.</span></span> <span data-ttu-id="4884b-224">Geleneksel yönlendirme sıra bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-224">Conventional routing is order-dependent.</span></span> <span data-ttu-id="4884b-225">Genel olarak, alanlar içeren rotalar daha önce bir alan olmadan rotalardan daha belirgin olduklarından yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-225">In general, routes with areas should be placed earlier as they're more specific than routes without an area.</span></span> <span data-ttu-id="4884b-226">`{*article}` gibi tüm rota parametrelerini yakala ile [adanmış geleneksel yollar](#dcr) , bir [yolun çok fazla](xref:fundamentals/routing#greedy)ve diğer yollarla eşleştirilmesini amaçladığı URL 'lerle eşleşmesi anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4884b-226">[Dedicated conventional routes](#dcr) with catch all route parameters like `{*article}` can make a route too [greedy](xref:fundamentals/routing#greedy), meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="4884b-227">Doyumsuz yollarını daha sonra yol tablosuna yerleştirerek doyumsuz eşleşmelerini önleyin.</span><span class="sxs-lookup"><span data-stu-id="4884b-227">Put the greedy routes later in the route table to prevent greedy matches.</span></span>

<a name="best"></a>

### <a name="resolving-ambiguous-actions"></a><span data-ttu-id="4884b-228">Belirsiz eylemleri çözme</span><span class="sxs-lookup"><span data-stu-id="4884b-228">Resolving ambiguous actions</span></span>

<span data-ttu-id="4884b-229">Yönlendirme ile iki uç nokta eşleşmesi durumunda, yönlendirme aşağıdakilerden birini yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="4884b-229">When two endpoints match through routing, routing must do one of the following:</span></span>

* <span data-ttu-id="4884b-230">En iyi aday ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="4884b-230">Choose the best candidate.</span></span>
* <span data-ttu-id="4884b-231">Bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4884b-231">Throw an exception.</span></span>

<span data-ttu-id="4884b-232">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4884b-232">For example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet9)]

<span data-ttu-id="4884b-233">Yukarıdaki denetleyici, eşleşen iki eylemi tanımlar:</span><span class="sxs-lookup"><span data-stu-id="4884b-233">The preceding controller defines two actions that match:</span></span>

* <span data-ttu-id="4884b-234">URL yolu `/Products33/Edit/17`</span><span class="sxs-lookup"><span data-stu-id="4884b-234">The URL path `/Products33/Edit/17`</span></span>
* <span data-ttu-id="4884b-235">Veri yolu `{ controller = Products33, action = Edit, id = 17 }`.</span><span class="sxs-lookup"><span data-stu-id="4884b-235">Route data `{ controller = Products33, action = Edit, id = 17 }`.</span></span>

<span data-ttu-id="4884b-236">Bu, MVC denetleyicileri için tipik bir modeldir:</span><span class="sxs-lookup"><span data-stu-id="4884b-236">This is a typical pattern for MVC controllers:</span></span>

* <span data-ttu-id="4884b-237">`Edit(int)` bir ürünü düzenlemek için bir form görüntüler.</span><span class="sxs-lookup"><span data-stu-id="4884b-237">`Edit(int)` displays a form to edit a product.</span></span>
* <span data-ttu-id="4884b-238">`Edit(int, Product)`, postalanan formu işler.</span><span class="sxs-lookup"><span data-stu-id="4884b-238">`Edit(int, Product)` processes  the posted form.</span></span>

<span data-ttu-id="4884b-239">Doğru yolu çözümlemek için:</span><span class="sxs-lookup"><span data-stu-id="4884b-239">To resolve the correct route:</span></span>

* <span data-ttu-id="4884b-240">istek bir HTTP `POST`olduğunda `Edit(int, Product)` seçilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-240">`Edit(int, Product)` is selected when the request is an HTTP `POST`.</span></span>
* <span data-ttu-id="4884b-241">[http fiili](#verb) başka bir şey olduğunda `Edit(int)` seçilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-241">`Edit(int)` is selected when the [HTTP verb](#verb) is anything else.</span></span> <span data-ttu-id="4884b-242">`Edit(int)` genellikle `GET`aracılığıyla çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-242">`Edit(int)` is generally called via `GET`.</span></span>

<span data-ttu-id="4884b-243"><xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, `[HttpPost]`, isteğin HTTP yöntemine göre seçim yapabilmesi için yönlendirme için verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4884b-243">The <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, `[HttpPost]`, is provided to routing so that it can choose based on the HTTP method of the request.</span></span> <span data-ttu-id="4884b-244">`HttpPostAttribute`, `Edit(int)`kıyasla `Edit(int, Product)` daha uygun hale getirir.</span><span class="sxs-lookup"><span data-stu-id="4884b-244">The `HttpPostAttribute` makes `Edit(int, Product)` a better match than `Edit(int)`.</span></span>

<span data-ttu-id="4884b-245">`HttpPostAttribute`gibi özniteliklerin rol olduğunu anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-245">It's important to understand the role of attributes like `HttpPostAttribute`.</span></span> <span data-ttu-id="4884b-246">Benzer öznitelikler diğer [http fiilleri](#verb)için tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4884b-246">Similar attributes are defined for other [HTTP verbs](#verb).</span></span> <span data-ttu-id="4884b-247">[Geleneksel yönlendirmesinde](#cr), bir görüntüleme formu, gönder formu iş akışının bir parçası olduklarında aynı eylem adını kullanmak yaygın olarak kullanılan bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="4884b-247">In [conventional routing](#cr), it's common for actions to use the same action name when they're part of a show form, submit form workflow.</span></span> <span data-ttu-id="4884b-248">Örneğin, bkz. [Iki düzenleme eylemi yöntemini İnceleme](xref:tutorials/first-mvc-app/controller-methods-views#get-post).</span><span class="sxs-lookup"><span data-stu-id="4884b-248">For example, see [Examine the two Edit action methods](xref:tutorials/first-mvc-app/controller-methods-views#get-post).</span></span>

<span data-ttu-id="4884b-249">Yönlendirme en iyi bir aday seçebilirse, birden fazla eşleşen uç noktayı listeleyerek bir <xref:System.Reflection.AmbiguousMatchException> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4884b-249">If routing can't choose a best candidate, an <xref:System.Reflection.AmbiguousMatchException> is thrown, listing the multiple matched endpoints.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="conventional-route-names"></a><span data-ttu-id="4884b-250">Geleneksel yol adları</span><span class="sxs-lookup"><span data-stu-id="4884b-250">Conventional route names</span></span>

<span data-ttu-id="4884b-251">Aşağıdaki örneklerde `"blog"` ve `"default"` dizeler geleneksel yol adlarıdır:</span><span class="sxs-lookup"><span data-stu-id="4884b-251">The strings  `"blog"` and `"default"` in the following examples are conventional route names:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<span data-ttu-id="4884b-252">Yol adları, yola mantıksal bir ad verir.</span><span class="sxs-lookup"><span data-stu-id="4884b-252">The route names give the route a logical name.</span></span> <span data-ttu-id="4884b-253">Adlandırılmış yol, URL oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-253">The named route can be used for URL generation.</span></span> <span data-ttu-id="4884b-254">Adlandırılmış bir yol kullanmak, yolların sıralaması URL oluşturma karmaşık hale geldiğinde URL oluşturmayı basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="4884b-254">Using a named route simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="4884b-255">Yol adları benzersiz uygulama genelinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-255">Route names must be unique application wide.</span></span>

<span data-ttu-id="4884b-256">Yol adları:</span><span class="sxs-lookup"><span data-stu-id="4884b-256">Route names:</span></span>

* <span data-ttu-id="4884b-257">İsteklerin URL 'siyle eşleşmesi veya işlenmesi üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="4884b-257">Have no impact on URL matching or handling of requests.</span></span>
* <span data-ttu-id="4884b-258">Yalnızca URL oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-258">Are used only for URL generation.</span></span>

<span data-ttu-id="4884b-259">Yol adı kavramı [ıendpointnamemetadata](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata)olarak yönlendirme ile temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-259">The route name concept is represented in routing as [IEndpointNameMetadata](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata).</span></span> <span data-ttu-id="4884b-260">Terimler **yol adı** ve **uç nokta adı**:</span><span class="sxs-lookup"><span data-stu-id="4884b-260">The terms **route name** and **endpoint name**:</span></span>

* <span data-ttu-id="4884b-261">Değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-261">Are interchangeable.</span></span>
* <span data-ttu-id="4884b-262">Belgede kullanılan ve kod, açıklanan API 'ye bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-262">Which one is used in documentation and code depends on the API being described.</span></span>

<a name="attribute-routing-ref-label"></a>
<a name="ar"></a>

## <a name="attribute-routing-for-rest-apis"></a><span data-ttu-id="4884b-263">REST API 'Leri için öznitelik yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4884b-263">Attribute routing for REST APIs</span></span>

<span data-ttu-id="4884b-264">REST API 'Leri, uygulamanın işlevselliğini [http fiilleri](#verb)tarafından temsil edilen bir kaynak kümesi olarak modellemek için öznitelik yönlendirmeyi kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-264">REST APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by [HTTP verbs](#verb).</span></span>

<span data-ttu-id="4884b-265">Öznitelik yönlendirme eylemleri doğrudan yönlendirme şablonlarına eşlemek için bir öznitelik kümesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-265">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="4884b-266">Aşağıdaki `StartUp.Configure` kodu bir REST API için tipik bir sonraki örnekte kullanılır:</span><span class="sxs-lookup"><span data-stu-id="4884b-266">The following `StartUp.Configure` code is typical for a REST API and is used in the next sample:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupApi.cs?name=snippet)]

<span data-ttu-id="4884b-267">Yukarıdaki kodda, öznitelik yönlendirmeli denetleyicileri eşlemek için `UseEndpoints` içinde <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> çağırılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-267">In the preceding code, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> is called inside `UseEndpoints` to map attribute routed controllers.</span></span>

<span data-ttu-id="4884b-268">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="4884b-268">In the following example:</span></span>

* <span data-ttu-id="4884b-269">Yukarıdaki `Configure` yöntemi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-269">The preceding `Configure` method is used.</span></span>
* <span data-ttu-id="4884b-270">`HomeController`, varsayılan geleneksel yol `{controller=Home}/{action=Index}/{id?}` eşleşenleri ile benzer bir URL kümesiyle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-270">`HomeController` matches a set of URLs similar to what the default conventional route `{controller=Home}/{action=Index}/{id?}` matches.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

<span data-ttu-id="4884b-271">`HomeController.Index` eylemi, `/`, `/Home`, `/Home/Index`veya `/Home/Index/3`URL yollarından herhangi biri için çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-271">The `HomeController.Index` action is run for any of the URL paths `/`, `/Home`, `/Home/Index`, or `/Home/Index/3`.</span></span>

<span data-ttu-id="4884b-272">Bu örnek, öznitelik yönlendirme ve [geleneksel yönlendirme](#cr)arasında bir temel programlama farkı vurgulamaktadır.</span><span class="sxs-lookup"><span data-stu-id="4884b-272">This example highlights a key programming difference between attribute routing and [conventional routing](#cr).</span></span> <span data-ttu-id="4884b-273">Öznitelik yönlendirme için bir yol belirtmek için daha fazla giriş gerekir.</span><span class="sxs-lookup"><span data-stu-id="4884b-273">Attribute routing requires more input to specify a route.</span></span> <span data-ttu-id="4884b-274">Geleneksel varsayılan yol, yönlendirmeleri daha succinctly işler.</span><span class="sxs-lookup"><span data-stu-id="4884b-274">The conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="4884b-275">Ancak, öznitelik yönlendirme izin verir ve her [eyleme](#action)hangi rota şablonlarının uygulanacağını kesin olarak denetler.</span><span class="sxs-lookup"><span data-stu-id="4884b-275">However, attribute routing allows and requires precise control of which route templates apply to each [action](#action).</span></span>

<span data-ttu-id="4884b-276">Öznitelik yönlendirme ile, denetleyici adı ve eylem adları, eylem ile eşleşen **hiçbir** rol oynar.</span><span class="sxs-lookup"><span data-stu-id="4884b-276">With attribute routing, the controller name and action names play **no** role in which action is matched.</span></span> <span data-ttu-id="4884b-277">Aşağıdaki örnek, önceki örnekle aynı URL 'Lerle eşleşir:</span><span class="sxs-lookup"><span data-stu-id="4884b-277">The following example matches the same URLs as the previous example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

<span data-ttu-id="4884b-278">Aşağıdaki kod `action` ve `controller`için belirteç değişimini kullanır:</span><span class="sxs-lookup"><span data-stu-id="4884b-278">The following code uses token replacement for `action` and `controller`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet22)]

<span data-ttu-id="4884b-279">Aşağıdaki kod denetleyiciye `[Route("[controller]/[action]")]` geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="4884b-279">The following code applies `[Route("[controller]/[action]")]` to the controller:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

<span data-ttu-id="4884b-280">Yukarıdaki kodda `Index` yöntemi şablonları, yol şablonlarına sonuna `/` veya `~/` eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-280">In the preceding code, the `Index` method templates must prepend `/` or `~/` to the route templates.</span></span> <span data-ttu-id="4884b-281">`/` veya `~/` ile başlayan bir eyleme uygulanan yol şablonları denetleyiciye uygulanan yol şablonları ile birleştirilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-281">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span>

<span data-ttu-id="4884b-282">Rota şablonu seçimi hakkında bilgi için bkz. [route Template önceliği](xref:fundamentals/routing#rtp) .</span><span class="sxs-lookup"><span data-stu-id="4884b-282">See [Route template precedence](xref:fundamentals/routing#rtp) for information on route template selection.</span></span>

## <a name="reserved-routing-names"></a><span data-ttu-id="4884b-283">Ayrılmış yönlendirme adları</span><span class="sxs-lookup"><span data-stu-id="4884b-283">Reserved routing names</span></span>

<span data-ttu-id="4884b-284">Aşağıdaki anahtar sözcükler, denetleyiciler veya Razor Pages kullanılırken ayrılmış yol parametresi adlarıdır:</span><span class="sxs-lookup"><span data-stu-id="4884b-284">The following keywords are reserved route parameter names when using Controllers or Razor Pages:</span></span>

* `action`
* `area`
* `controller`
* `handler`
* `page`

<span data-ttu-id="4884b-285">Öznitelik yönlendirme ile yol parametresi olarak `page` kullanmak yaygın bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="4884b-285">Using `page` as a route parameter with attribute routing is a common error.</span></span> <span data-ttu-id="4884b-286">Bunun yapılması, URL oluşturma ile tutarsız ve kafa karıştırıcı davranışa neden olur.</span><span class="sxs-lookup"><span data-stu-id="4884b-286">Doing that results in inconsistent and confusing behavior with URL generation.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo2Controller.cs?name=snippet)]

<span data-ttu-id="4884b-287">Özel parametre adları, URL oluşturma işleminin bir Razor sayfasına mı yoksa bir denetleyiciye mi başvurduğunu öğrenmek için URL oluşturma tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-287">The special parameter names are used by the URL generation to determine if a URL generation operation refers to a Razor Page or to a Controller.</span></span>

<a name="verb"></a>

## <a name="http-verb-templates"></a><span data-ttu-id="4884b-288">HTTP fiili şablonları</span><span class="sxs-lookup"><span data-stu-id="4884b-288">HTTP verb templates</span></span>

<span data-ttu-id="4884b-289">ASP.NET Core aşağıdaki HTTP fiili şablonlarına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="4884b-289">ASP.NET Core has the following HTTP verb templates:</span></span>

* <span data-ttu-id="4884b-290">[HttpGet](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)</span><span class="sxs-lookup"><span data-stu-id="4884b-290">[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)</span></span>
* <span data-ttu-id="4884b-291">[HttpPost](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)</span><span class="sxs-lookup"><span data-stu-id="4884b-291">[[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)</span></span>
* <span data-ttu-id="4884b-292">[[HttpPut]](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)</span><span class="sxs-lookup"><span data-stu-id="4884b-292">[[HttpPut]](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)</span></span>
* <span data-ttu-id="4884b-293">[[HttpDelete]](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)</span><span class="sxs-lookup"><span data-stu-id="4884b-293">[[HttpDelete]](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)</span></span>
* <span data-ttu-id="4884b-294">[[HttpHead]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)</span><span class="sxs-lookup"><span data-stu-id="4884b-294">[[HttpHead]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)</span></span>
* <span data-ttu-id="4884b-295">[[HttpPatch]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)</span><span class="sxs-lookup"><span data-stu-id="4884b-295">[[HttpPatch]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)</span></span>

<a name="rt"></a>

### <a name="route-templates"></a><span data-ttu-id="4884b-296">Rota şablonları</span><span class="sxs-lookup"><span data-stu-id="4884b-296">Route templates</span></span>

<span data-ttu-id="4884b-297">ASP.NET Core aşağıdaki yol şablonlarına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="4884b-297">ASP.NET Core has the following route templates:</span></span>

* <span data-ttu-id="4884b-298">Tüm [http fiili şablonları](#verb) rota şablonlarıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-298">All the [HTTP verb templates](#verb) are route templates.</span></span>
* <span data-ttu-id="4884b-299">[Yolu](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="4884b-299">[[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)</span></span>

<a name="arx"></a>

### <a name="attribute-routing-with-http-verb-attributes"></a><span data-ttu-id="4884b-300">Http fiili öznitelikleriyle öznitelik yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4884b-300">Attribute routing with Http verb attributes</span></span>

<span data-ttu-id="4884b-301">Aşağıdaki denetleyiciyi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="4884b-301">Consider the following controller:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="4884b-302">Önceki kodda:</span><span class="sxs-lookup"><span data-stu-id="4884b-302">In the preceding code:</span></span>

* <span data-ttu-id="4884b-303">Her eylem yalnızca HTTP GET istekleriyle eşleştirmeyi kısıtlayan `[HttpGet]` özniteliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="4884b-303">Each action contains the `[HttpGet]` attribute, which constrains matching to HTTP GET requests only.</span></span>
* <span data-ttu-id="4884b-304">`GetProduct` eylemi `"{id}"` şablonunu içerir, bu nedenle `id` denetleyicideki `"api/[controller]"` şablonuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-304">The `GetProduct` action includes the `"{id}"` template, therefore `id` is appended to the `"api/[controller]"` template on the controller.</span></span> <span data-ttu-id="4884b-305">Yöntemler şablonu `"api/[controller]/"{id}""`.</span><span class="sxs-lookup"><span data-stu-id="4884b-305">The methods template is `"api/[controller]/"{id}""`.</span></span> <span data-ttu-id="4884b-306">Bu nedenle bu eylem yalnızca form `/api/test2/xyz`,`/api/test2/123`,`/api/test2/{any string}`vb. için GET istekleri ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-306">Therefore this action only matches GET requests of for the form `/api/test2/xyz`,`/api/test2/123`,`/api/test2/{any string}`, etc.</span></span>
  [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet2)]
* <span data-ttu-id="4884b-307">`GetIntProduct` eylemi `"int/{id:int}")` şablonunu içerir.</span><span class="sxs-lookup"><span data-stu-id="4884b-307">The `GetIntProduct` action contains the `"int/{id:int}")` template.</span></span> <span data-ttu-id="4884b-308">Şablonun `:int` kısmı, `id` yol değerlerini bir tamsayıya dönüştürülebilen dizelere kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-308">The `:int` portion of the template constrains the `id` route values to strings that can be converted to an integer.</span></span> <span data-ttu-id="4884b-309">`/api/test2/int/abc`için bir GET isteği:</span><span class="sxs-lookup"><span data-stu-id="4884b-309">A GET request to `/api/test2/int/abc`:</span></span>
  * <span data-ttu-id="4884b-310">Bu eylemle eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="4884b-310">Doesn't match this action.</span></span>
  * <span data-ttu-id="4884b-311">Bir [404 bulunamadı](https://developer.mozilla.org/docs/Web/HTTP/Status/404) hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="4884b-311">Returns a [404 Not Found](https://developer.mozilla.org/docs/Web/HTTP/Status/404) error.</span></span>
    [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet3)]
* <span data-ttu-id="4884b-312">`GetInt2Product` eylemi şablonda `{id}` içerir, ancak `id` bir tamsayıya dönüştürülemeyen değerlerle kısıtlamaz.</span><span class="sxs-lookup"><span data-stu-id="4884b-312">The `GetInt2Product` action contains `{id}` in the template, but doesn't constrain `id` to values that can be converted to an integer.</span></span> <span data-ttu-id="4884b-313">`/api/test2/int2/abc`için bir GET isteği:</span><span class="sxs-lookup"><span data-stu-id="4884b-313">A GET request to `/api/test2/int2/abc`:</span></span>
  * <span data-ttu-id="4884b-314">Bu rota ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-314">Matches this route.</span></span>
  * <span data-ttu-id="4884b-315">Model bağlama `abc` tamsayıya dönüştüremiyor.</span><span class="sxs-lookup"><span data-stu-id="4884b-315">Model binding fails to convert `abc` to an integer.</span></span> <span data-ttu-id="4884b-316">Metodun `id` parametresi tamsayıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-316">The `id` parameter of the method is integer.</span></span>
  * <span data-ttu-id="4884b-317">Model bağlama`abc` tamsayıya dönüştürülemediğinden, [Hatalı bir istek 400](https://developer.mozilla.org/docs/Web/HTTP/Status/400) döndürür.</span><span class="sxs-lookup"><span data-stu-id="4884b-317">Returns a [400 Bad Request](https://developer.mozilla.org/docs/Web/HTTP/Status/400) because model binding failed to convert`abc` to an integer.</span></span>
      [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet4)]

<span data-ttu-id="4884b-318">Öznitelik yönlendirme <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, <xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute>ve <xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute>gibi <xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute> öznitelikleri kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-318">Attribute routing can use <xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute> attributes such as <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, <xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute>, and <xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute>.</span></span> <span data-ttu-id="4884b-319">Tüm [http fiili](#verb) öznitelikleri bir yol şablonunu kabul eder.</span><span class="sxs-lookup"><span data-stu-id="4884b-319">All of the [HTTP verb](#verb) attributes accept a route template.</span></span> <span data-ttu-id="4884b-320">Aşağıdaki örnekte, aynı yol şablonuyla eşleşen iki eylem gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="4884b-320">The following example shows two actions that match the same route template:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyProductsController.cs?name=snippet1)]

<span data-ttu-id="4884b-321">URL yolu `/products3`kullanılıyor:</span><span class="sxs-lookup"><span data-stu-id="4884b-321">Using the URL path `/products3`:</span></span>

* <span data-ttu-id="4884b-322">`MyProductsController.ListProducts` eylemi, [http fiili](#verb) `GET`olduğunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="4884b-322">The `MyProductsController.ListProducts` action runs when the [HTTP verb](#verb) is `GET`.</span></span>
* <span data-ttu-id="4884b-323">`MyProductsController.CreateProduct` eylemi, [http fiili](#verb) `POST`olduğunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="4884b-323">The `MyProductsController.CreateProduct` action runs when the [HTTP verb](#verb) is `POST`.</span></span>

<span data-ttu-id="4884b-324">Bir REST API oluştururken, eylem tüm HTTP yöntemlerini kabul ettiğinden bir eylem yönteminde `[Route(...)]` kullanmanız gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="4884b-324">When building a REST API, it's rare that you'll need to use `[Route(...)]` on an action method because the action accepts all HTTP methods.</span></span> <span data-ttu-id="4884b-325">API 'nizin neleri desteklediği hakkında kesin olması için, daha özel [http fiili özniteliği](#verb) kullanmak daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-325">It's better to use the more specific [HTTP verb attribute](#verb) to be precise about what your API supports.</span></span> <span data-ttu-id="4884b-326">REST API 'lerinin istemcileri, hangi yolların ve HTTP fiillerinin belirli mantıksal işlemlere eşlendiğini bilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-326">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="4884b-327">REST API 'Leri, uygulamanın işlevselliğini HTTP fiilleri tarafından temsil edilen bir kaynak kümesi olarak modellemek için öznitelik yönlendirmeyi kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-327">REST APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="4884b-328">Bu, örneğin, aynı mantıksal kaynaktaki al ve postala gibi birçok işlemin aynı URL 'YI kullanması anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4884b-328">This means that many operations, for example, GET and POST on the same logical resource use the same URL.</span></span> <span data-ttu-id="4884b-329">Öznitelik yönlendirme, bir API 'nin Genel uç nokta yerleşimini dikkatle tasarlamak için gereken bir denetim düzeyi sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-329">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

<span data-ttu-id="4884b-330">Bir öznitelik yolu belirli bir eyleme uyguladığı için, yol şablonu tanımının bir parçası olarak gerekli parametreleri yapmak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="4884b-330">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="4884b-331">Aşağıdaki örnekte, URL yolunun bir parçası olarak `id` gereklidir:</span><span class="sxs-lookup"><span data-stu-id="4884b-331">In the following example, `id` is required as part of the URL path:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

<span data-ttu-id="4884b-332">`Products2ApiController.GetProduct(int)` eylemi:</span><span class="sxs-lookup"><span data-stu-id="4884b-332">The `Products2ApiController.GetProduct(int)` action:</span></span>

* <span data-ttu-id="4884b-333">`/products2/3` gibi URL yoluyla çalıştırılır</span><span class="sxs-lookup"><span data-stu-id="4884b-333">Is run with URL path like `/products2/3`</span></span>
* <span data-ttu-id="4884b-334">URL yolu `/products2`ile çalıştırılmadı.</span><span class="sxs-lookup"><span data-stu-id="4884b-334">Isn't run with the URL path `/products2`.</span></span>

<span data-ttu-id="4884b-335">[[Tüketir]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>) özniteliği, desteklenen istek içerik türlerini sınırlama eylemi sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-335">The [[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>) attribute allows an action to limit the supported request content types.</span></span> <span data-ttu-id="4884b-336">Daha fazla bilgi için bkz. [desteklenen istek içerik türlerini tüketir özniteliğiyle tanımlama](xref:web-api/index#consumes).</span><span class="sxs-lookup"><span data-stu-id="4884b-336">For more information, see [Define supported request content types with the Consumes attribute](xref:web-api/index#consumes).</span></span>

 <span data-ttu-id="4884b-337">Yol şablonlarının ve ilgili seçeneklerin tam açıklaması için bkz. [yönlendirme](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="4884b-337">See [Routing](xref:fundamentals/routing) for a full description of route templates and related options.</span></span>

<span data-ttu-id="4884b-338">`[ApiController]`hakkında daha fazla bilgi için bkz. [Apicontroller özniteliği](xref:web-api/index##apicontroller-attribute).</span><span class="sxs-lookup"><span data-stu-id="4884b-338">For more information on `[ApiController]`, see [ApiController attribute](xref:web-api/index##apicontroller-attribute).</span></span>

## <a name="route-name"></a><span data-ttu-id="4884b-339">Yönlendirme adı</span><span class="sxs-lookup"><span data-stu-id="4884b-339">Route name</span></span>

<span data-ttu-id="4884b-340">Aşağıdaki kod `Products_List`yol adını tanımlar:</span><span class="sxs-lookup"><span data-stu-id="4884b-340">The following code  defines a route name of `Products_List`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

<span data-ttu-id="4884b-341">Yol adları, belirli bir yolu temel alan bir URL oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-341">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="4884b-342">Yol adları:</span><span class="sxs-lookup"><span data-stu-id="4884b-342">Route names:</span></span>

* <span data-ttu-id="4884b-343">Yönlendirmenin URL eşleşen davranışı üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="4884b-343">Have no impact on the URL matching behavior of routing.</span></span>
* <span data-ttu-id="4884b-344">Yalnızca URL oluşturma için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-344">Are only used for URL generation.</span></span>

<span data-ttu-id="4884b-345">Yol adları, uygulama genelinde benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-345">Route names must be unique application-wide.</span></span>

<span data-ttu-id="4884b-346">Önceki kodu, `id` parametresini isteğe bağlı (`{id?}`) olarak tanımlayan geleneksel varsayılan rotayla kontrast.</span><span class="sxs-lookup"><span data-stu-id="4884b-346">Contrast the preceding code with the conventional default route, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="4884b-347">API 'Leri tam olarak belirtme özelliği, `/products` ve `/products/5` farklı eylemlere dağıtılması gibi avantajlar sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-347">The ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

## <a name="combining-attribute-routes"></a><span data-ttu-id="4884b-348">Öznitelik yollarını birleştirme</span><span class="sxs-lookup"><span data-stu-id="4884b-348">Combining attribute routes</span></span>

<span data-ttu-id="4884b-349">Öznitelik yönlendirmeyi daha az tekrarlı hale getirmek için, denetleyicideki yol öznitelikleri, bireysel eylemlerdeki rota öznitelikleriyle birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-349">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="4884b-350">Denetleyicide tanımlanan tüm yol şablonları, eylemlerdeki rota şablonlarına eklenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-350">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="4884b-351">Bir Route özniteliğinin denetleyiciye yerleştirilmesi, denetleyicideki **Tüm** eylemlerin öznitelik yönlendirme kullanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-351">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet)]

<span data-ttu-id="4884b-352">Yukarıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="4884b-352">In the preceding example:</span></span>

* <span data-ttu-id="4884b-353">URL yolu `/products` eşleşiyor `ProductsApi.ListProducts`</span><span class="sxs-lookup"><span data-stu-id="4884b-353">The URL path `/products` can match `ProductsApi.ListProducts`</span></span>
* <span data-ttu-id="4884b-354">URL yolu `/products/5` `ProductsApi.GetProduct(int)`ile eşleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-354">The URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span>

<span data-ttu-id="4884b-355">Bu eylemlerin her ikisi de `[HttpGet]` özniteliğiyle işaretlendiğinden HTTP `GET` eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-355">Both of these actions only match HTTP `GET` because they're marked with the `[HttpGet]` attribute.</span></span>

<span data-ttu-id="4884b-356">`/` veya `~/` ile başlayan bir eyleme uygulanan yol şablonları denetleyiciye uygulanan yol şablonları ile birleştirilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-356">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="4884b-357">Aşağıdaki örnek, varsayılan rotaya benzer bir URL yolları kümesiyle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-357">The following example matches a set of URL paths similar to the default route.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet)]

<span data-ttu-id="4884b-358">Aşağıdaki tabloda, önceki koddaki `[Route]` öznitelikleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="4884b-358">The following table explains the `[Route]` attributes in the preceding code:</span></span>

| <span data-ttu-id="4884b-359">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="4884b-359">Attribute</span></span>               | <span data-ttu-id="4884b-360">`[Route("Home")]` ile birleştirir</span><span class="sxs-lookup"><span data-stu-id="4884b-360">Combines with `[Route("Home")]`</span></span> | <span data-ttu-id="4884b-361">Rota şablonunu tanımlar</span><span class="sxs-lookup"><span data-stu-id="4884b-361">Defines route template</span></span> |
| ----------------- | ------------ | --------- |
| `[Route("")]` | <span data-ttu-id="4884b-362">Evet</span><span class="sxs-lookup"><span data-stu-id="4884b-362">Yes</span></span> | `"Home"` |
| `[Route("Index")]` | <span data-ttu-id="4884b-363">Evet</span><span class="sxs-lookup"><span data-stu-id="4884b-363">Yes</span></span> | `"Home/Index"` |
| `[Route("/")]` | <span data-ttu-id="4884b-364">**Hayır**</span><span class="sxs-lookup"><span data-stu-id="4884b-364">**No**</span></span> | `""` |
| `[Route("About")]` | <span data-ttu-id="4884b-365">Evet</span><span class="sxs-lookup"><span data-stu-id="4884b-365">Yes</span></span> | `"Home/About"` |

<a name="routing-ordering-ref-label"></a>
<a name="oar"></a>

### <a name="attribute-route-order"></a><span data-ttu-id="4884b-366">Öznitelik yolu sırası</span><span class="sxs-lookup"><span data-stu-id="4884b-366">Attribute route order</span></span>

<span data-ttu-id="4884b-367">Yönlendirme bir ağaç oluşturur ve aynı anda tüm uç noktaları eşleştirir:</span><span class="sxs-lookup"><span data-stu-id="4884b-367">Routing builds a tree and matches all endpoints simultaneously:</span></span>

* <span data-ttu-id="4884b-368">Yol girdileri ideal bir sıralamaya yerleştirilmiş gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="4884b-368">The route entries behave as if placed in an ideal ordering.</span></span>
* <span data-ttu-id="4884b-369">En özel yolların, daha genel yollardan önce yürütülmesi şansınız vardır.</span><span class="sxs-lookup"><span data-stu-id="4884b-369">The most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="4884b-370">Örneğin, `blog/search/{topic}` gibi bir öznitelik yolu `blog/{*article}`gibi bir öznitelik rotasına göre daha özgüdür.</span><span class="sxs-lookup"><span data-stu-id="4884b-370">For example, an attribute route like `blog/search/{topic}` is more specific than an attribute route like `blog/{*article}`.</span></span> <span data-ttu-id="4884b-371">Daha belirgin olduğundan, `blog/search/{topic}` yolu, varsayılan olarak daha yüksek önceliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4884b-371">The `blog/search/{topic}` route has higher priority, by default, because it's more specific.</span></span> <span data-ttu-id="4884b-372">[Geleneksel yönlendirmeyi](#cr)kullanarak, yolları istenen sırada yerleştirmekten geliştirici sorumludur.</span><span class="sxs-lookup"><span data-stu-id="4884b-372">Using [conventional routing](#cr), the developer is responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="4884b-373">Öznitelik yolları <xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order> özelliğini kullanarak bir sıra yapılandırabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-373">Attribute routes can configure an order using the <xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order> property.</span></span> <span data-ttu-id="4884b-374">Sunulan tüm Framework [yol öznitelikleri](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) `Order` içerir.</span><span class="sxs-lookup"><span data-stu-id="4884b-374">All of the framework provided [route attributes](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) include `Order` .</span></span> <span data-ttu-id="4884b-375">Yollar `Order` özelliğinin artan sıralamasına göre işlenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-375">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="4884b-376">Varsayılan sıra `0`.</span><span class="sxs-lookup"><span data-stu-id="4884b-376">The default order is `0`.</span></span> <span data-ttu-id="4884b-377">`Order = -1` kullanarak bir yolu ayarlama, bir sipariş ayarlamadığı rotalardan önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="4884b-377">Setting a route using `Order = -1` runs before routes that don't set an order.</span></span> <span data-ttu-id="4884b-378">`Order = 1` kullanarak bir yolu ayarlama varsayılan yol sıralaması sonrasında çalışır.</span><span class="sxs-lookup"><span data-stu-id="4884b-378">Setting a route using `Order = 1` runs after default route ordering.</span></span>

<span data-ttu-id="4884b-379">`Order`bağlı olmadığından **kaçının** .</span><span class="sxs-lookup"><span data-stu-id="4884b-379">**Avoid** depending on `Order`.</span></span> <span data-ttu-id="4884b-380">Bir uygulamanın URL 'SI alanı, doğru şekilde yönlendirmek için açık sıra değerleri gerektiriyorsa, bu durumda istemciler de kafa karıştırıcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-380">If an app's URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="4884b-381">Genel olarak, öznitelik yönlendirme URL ile eşleşen doğru yolu seçer.</span><span class="sxs-lookup"><span data-stu-id="4884b-381">In general, attribute routing selects the correct route with URL matching.</span></span> <span data-ttu-id="4884b-382">URL oluşturma için kullanılan varsayılan sıra çalışmıyorsa, geçersiz kılma olarak bir yol adı kullanılması genellikle `Order` özelliğini uygulamaktan daha basittir.</span><span class="sxs-lookup"><span data-stu-id="4884b-382">If the default order used for URL generation isn't working, using a route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="4884b-383">Her ikisi de `/home`eşleşen yolu tanımlayan aşağıdaki iki denetleyicisi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="4884b-383">Consider the following two controllers which both define the route matching `/home`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

<span data-ttu-id="4884b-384">Yukarıdaki kodla `/home` istemek aşağıdakine benzer bir özel durum oluşturur:</span><span class="sxs-lookup"><span data-stu-id="4884b-384">Requesting `/home` with the preceding code throws an exception similar to the following:</span></span>

```text
AmbiguousMatchException: The request matched multiple endpoints. Matches:

 WebMvcRouting.Controllers.HomeController.Index
 WebMvcRouting.Controllers.MyDemoController.MyIndex
```

<span data-ttu-id="4884b-385">Yol özniteliklerinden birine `Order` eklemek belirsizlik çözer:</span><span class="sxs-lookup"><span data-stu-id="4884b-385">Adding `Order` to one of the route attributes resolves the ambiguity:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo3Controller.cs?name=snippet3& highlight=2)]

<span data-ttu-id="4884b-386">Yukarıdaki kodla `/home` `HomeController.Index` uç noktasını çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="4884b-386">With the preceding code, `/home` runs the `HomeController.Index` endpoint.</span></span> <span data-ttu-id="4884b-387">`MyDemoController.MyIndex`almak için `/home/MyIndex`isteyin.</span><span class="sxs-lookup"><span data-stu-id="4884b-387">To get to the `MyDemoController.MyIndex`, request `/home/MyIndex`.</span></span> <span data-ttu-id="4884b-388">**Not**:</span><span class="sxs-lookup"><span data-stu-id="4884b-388">**Note**:</span></span>

* <span data-ttu-id="4884b-389">Yukarıdaki kod bir örnek veya kötü yönlendirme tasarımdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-389">The preceding code is an example or poor routing design.</span></span> <span data-ttu-id="4884b-390">`Order` özelliğini göstermek için kullanılmıştı.</span><span class="sxs-lookup"><span data-stu-id="4884b-390">It was used to illustrate the `Order` property.</span></span>
* <span data-ttu-id="4884b-391">`Order` özelliği yalnızca belirsizlik çözümleniyor, bu şablon eşleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="4884b-391">The `Order` property only resolves the ambiguity, that template cannot be matched.</span></span> <span data-ttu-id="4884b-392">`[Route("Home")]` şablonunu kaldırmak daha iyi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="4884b-392">It would be better to remove the `[Route("Home")]` template.</span></span>

<span data-ttu-id="4884b-393">Bkz. [Razor Pages yol ve uygulama kuralları:](xref:razor-pages/razor-pages-conventions#route-order) rota sıralaması hakkında bilgiler için Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="4884b-393">See [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order) for information on route order with Razor Pages.</span></span>

<span data-ttu-id="4884b-394">Bazı durumlarda, belirsiz yollarla bir HTTP 500 hatası döndürülür.</span><span class="sxs-lookup"><span data-stu-id="4884b-394">In some cases, an HTTP 500 error is returned with ambiguous routes.</span></span> <span data-ttu-id="4884b-395">`AmbiguousMatchException`hangi uç noktaların olduğunu görmek için [günlüğe kaydetmeyi](xref:fundamentals/logging/index) kullanın.</span><span class="sxs-lookup"><span data-stu-id="4884b-395">Use [logging](xref:fundamentals/logging/index) to see which endpoints caused the `AmbiguousMatchException`.</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="4884b-396">Yönlendirme şablonlarında belirteç değiştirme [denetleyici], [eylem], [alan]</span><span class="sxs-lookup"><span data-stu-id="4884b-396">Token replacement in route templates [controller], [action], [area]</span></span>

<span data-ttu-id="4884b-397">Daha kolay olması için, öznitelik rotaları, aşağıdakilerden birine bir belirteç ekleyerek ayrılmış yol parametrelerine yönelik belirteç değişimini destekler:</span><span class="sxs-lookup"><span data-stu-id="4884b-397">For convenience, attribute routes support token replacement for reserved route parameters by enclosing a token in one of the following:</span></span>

* <span data-ttu-id="4884b-398">Köşeli ayraç: `[]`</span><span class="sxs-lookup"><span data-stu-id="4884b-398">Square braces: `[]`</span></span>
* <span data-ttu-id="4884b-399">Küme ayraçları: `{}`</span><span class="sxs-lookup"><span data-stu-id="4884b-399">Curly braces: `{}`</span></span>

<span data-ttu-id="4884b-400">`[action]`, `[area]`ve `[controller]` belirteçleri, yolun tanımlandığı eylemden eylem adı, alan adı ve denetleyici adı değerleriyle değiştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4884b-400">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="4884b-401">Önceki kodda:</span><span class="sxs-lookup"><span data-stu-id="4884b-401">In the preceding code:</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet10)]

  * <span data-ttu-id="4884b-402">Eşleşen `/Products0/List`</span><span class="sxs-lookup"><span data-stu-id="4884b-402">Matches `/Products0/List`</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet11)]

  * <span data-ttu-id="4884b-403">Eşleşen `/Products0/Edit/{id}`</span><span class="sxs-lookup"><span data-stu-id="4884b-403">Matches `/Products0/Edit/{id}`</span></span>

<span data-ttu-id="4884b-404">Belirteç değişikliği, öznitelik yollarının oluşturulması için son adım olarak gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-404">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="4884b-405">Yukarıdaki örnek aşağıdaki kodla aynı şekilde davranır:</span><span class="sxs-lookup"><span data-stu-id="4884b-405">The preceding example behaves the same as the following code:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet20)]

[!INCLUDE[](~/includes/MTcomments.md)]

<span data-ttu-id="4884b-406">Öznitelik rotaları de devralma ile birleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-406">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="4884b-407">Bu, belirteç değiştirme ile güçlü bir şekilde birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-407">This is powerful combined with token replacement.</span></span> <span data-ttu-id="4884b-408">Belirteç değişikliği, öznitelik rotaları tarafından tanımlanan yol adları için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-408">Token replacement also applies to route names defined by attribute routes.</span></span>
<span data-ttu-id="4884b-409">`[Route("[controller]/[action]", Name="[controller]_[action]")]`her eylem için benzersiz bir yol adı üretir:</span><span class="sxs-lookup"><span data-stu-id="4884b-409">`[Route("[controller]/[action]", Name="[controller]_[action]")]`generates a unique route name for each action:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet5)]

<span data-ttu-id="4884b-410">Belirteç değişikliği, öznitelik rotaları tarafından tanımlanan yol adları için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-410">Token replacement also applies to route names defined by attribute routes.</span></span>
`[Route("[controller]/[action]", Name="[controller]_[action]")]`
<span data-ttu-id="4884b-411">Her eylem için benzersiz bir yol adı üretir.</span><span class="sxs-lookup"><span data-stu-id="4884b-411">generates a unique route name for each action.</span></span>

<span data-ttu-id="4884b-412">Sabit belirteç değiştirme sınırlayıcısı `[` veya `]`eşleştirmek için, karakteri (`[[` veya `]]`) tekrarlayarak kaçış.</span><span class="sxs-lookup"><span data-stu-id="4884b-412">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="4884b-413">Belirteç değişimini özelleştirmek için bir parametre transformatörü kullanın</span><span class="sxs-lookup"><span data-stu-id="4884b-413">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="4884b-414">Belirteç değiştirme, bir parametre transformatörü kullanılarak özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-414">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="4884b-415">Bir parametre transformatörü <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> uygular ve parametrelerin değerini dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="4884b-415">A parameter transformer implements <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> and transforms the value of parameters.</span></span> <span data-ttu-id="4884b-416">Örneğin, özel bir `SlugifyParameterTransformer` parametresi transformatörü `SubscriptionManagement` Route değerini `subscription-management`olarak değiştirir:</span><span class="sxs-lookup"><span data-stu-id="4884b-416">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet2)]

<span data-ttu-id="4884b-417"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention>, şu şekilde bir uygulama modeli kuralıdır:</span><span class="sxs-lookup"><span data-stu-id="4884b-417">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention> is an application model convention that:</span></span>

* <span data-ttu-id="4884b-418">Bir uygulamadaki tüm öznitelik yollarına bir parametre transformatörü uygular.</span><span class="sxs-lookup"><span data-stu-id="4884b-418">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="4884b-419">Öznitelik yol belirteci değerlerini değiştirildikleri gibi özelleştirir.</span><span class="sxs-lookup"><span data-stu-id="4884b-419">Customizes the attribute route token values as they are replaced.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/SubscriptionManagementController.cs?name=snippet)]

<span data-ttu-id="4884b-420">Yukarıdaki `ListAll` yöntemi `/subscription-management/list-all`eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-420">The preceding `ListAll` method matches `/subscription-management/list-all`.</span></span>

<span data-ttu-id="4884b-421">`RouteTokenTransformerConvention`, `ConfigureServices`bir seçenek olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-421">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet)]

<span data-ttu-id="4884b-422">Bilgi tanımı için bkz. [Mdıdn Web docs](https://developer.mozilla.org/docs/Glossary/Slug) for the başlık.</span><span class="sxs-lookup"><span data-stu-id="4884b-422">See [MDN web docs on Slug](https://developer.mozilla.org/docs/Glossary/Slug) for the definition of Slug.</span></span>

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-attribute-routes"></a><span data-ttu-id="4884b-423">Birden çok öznitelik yolu</span><span class="sxs-lookup"><span data-stu-id="4884b-423">Multiple attribute routes</span></span>

<span data-ttu-id="4884b-424">Öznitelik yönlendirme, aynı eyleme ulaşan birden çok yolun tanımlanmasını destekler.</span><span class="sxs-lookup"><span data-stu-id="4884b-424">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="4884b-425">Bunun en yaygın kullanımları, aşağıdaki örnekte gösterildiği gibi varsayılan geleneksel yolun davranışını taklit etmek olur:</span><span class="sxs-lookup"><span data-stu-id="4884b-425">The most common usage of this is to mimic the behavior of the default conventional route as shown in the following example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6x)]

<span data-ttu-id="4884b-426">Denetleyiciye birden çok yol özniteliği koymak, her birinin eylem yöntemlerinde yol özniteliklerinin her biri ile birleştiribileceği anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="4884b-426">Putting multiple route attributes on the controller means that each one combines with each of the route attributes on the action methods:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6)]

<span data-ttu-id="4884b-427">Tüm [http fiili](#verb) yol kısıtlamaları `IActionConstraint`uygular.</span><span class="sxs-lookup"><span data-stu-id="4884b-427">All the [HTTP verb](#verb) route constraints implement `IActionConstraint`.</span></span>

<span data-ttu-id="4884b-428"><xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> uygulayan birden çok yol özniteliği bir eyleme yerleştirildiğinde:</span><span class="sxs-lookup"><span data-stu-id="4884b-428">When multiple route attributes that implement <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> are placed on an action:</span></span>

* <span data-ttu-id="4884b-429">Her eylem kısıtlaması, denetleyiciye uygulanan rota şablonuyla birlikte birleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-429">Each action constraint combines with the route template applied to the controller.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet7)]

<span data-ttu-id="4884b-430">Eylemlerde birden çok yolun kullanılması yararlı ve güçlü görünebilir, uygulamanızın URL alanını temel ve iyi tanımlanmış tutmanız daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-430">Using multiple routes on actions might seem useful and powerful, it's better to keep your app's URL space basic and well defined.</span></span> <span data-ttu-id="4884b-431">**Yalnızca** gerektiğinde, var olan istemcileri desteklemek için, eylemlerde birden çok yol kullanın.</span><span class="sxs-lookup"><span data-stu-id="4884b-431">Use multiple routes on actions **only** where needed, for example, to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="4884b-432">Öznitelik rotası isteğe bağlı parametreler, varsayılan değerler ve kısıtlamalar belirtme</span><span class="sxs-lookup"><span data-stu-id="4884b-432">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="4884b-433">Öznitelik yolları, isteğe bağlı parametreleri, varsayılan değerleri ve kısıtlamaları belirtmek için geleneksel yollarla aynı satır içi sözdizimini destekler.</span><span class="sxs-lookup"><span data-stu-id="4884b-433">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet8&highlight=3)]

<span data-ttu-id="4884b-434">Yukarıdaki kodda `[HttpPost("product/{id:int}")]` bir yol kısıtlaması uygular.</span><span class="sxs-lookup"><span data-stu-id="4884b-434">In the preceding code, `[HttpPost("product/{id:int}")]` applies a route constraint.</span></span> <span data-ttu-id="4884b-435">`ProductsController.ShowProduct` eylemi yalnızca `/product/3`gibi URL yollarıyla eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-435">The `ProductsController.ShowProduct` action is matched only by URL paths like `/product/3`.</span></span> <span data-ttu-id="4884b-436">Yol şablonu bölümü, bu segmenti yalnızca tamsayılarla kısıtlar `{id:int}`.</span><span class="sxs-lookup"><span data-stu-id="4884b-436">The route template portion `{id:int}` constrains that segment to only integers.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

<span data-ttu-id="4884b-437">Yol şablonu sözdiziminin ayrıntılı açıklaması için bkz. [route Template Reference](xref:fundamentals/routing#route-template-reference) .</span><span class="sxs-lookup"><span data-stu-id="4884b-437">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="4884b-438">Iroutetemplateprovider kullanarak özel yol öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="4884b-438">Custom route attributes using IRouteTemplateProvider</span></span>

<span data-ttu-id="4884b-439">Tüm [Rota öznitelikleri](#rt) <xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider>uygular.</span><span class="sxs-lookup"><span data-stu-id="4884b-439">All of the [route attributes](#rt) implement <xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider>.</span></span> <span data-ttu-id="4884b-440">ASP.NET Core çalışma zamanı:</span><span class="sxs-lookup"><span data-stu-id="4884b-440">The ASP.NET Core runtime:</span></span>

* <span data-ttu-id="4884b-441">Uygulama başladığında denetleyici sınıflarında ve eylem yöntemlerinde öznitelikler arar.</span><span class="sxs-lookup"><span data-stu-id="4884b-441">Looks for attributes on controller classes and action methods when the app starts.</span></span>
* <span data-ttu-id="4884b-442">İlk yol kümesini oluşturmak için `IRouteTemplateProvider` uygulayan öznitelikleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-442">Uses the attributes that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="4884b-443">Özel yol özniteliklerini tanımlamak için `IRouteTemplateProvider` uygulayın.</span><span class="sxs-lookup"><span data-stu-id="4884b-443">Implement `IRouteTemplateProvider` to define custom route attributes.</span></span> <span data-ttu-id="4884b-444">Her `IRouteTemplateProvider`, özel bir yol şablonu, sırası ve adı ile tek bir yol tanımlamanızı sağlar:</span><span class="sxs-lookup"><span data-stu-id="4884b-444">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyTestApiController.cs?name=snippet&highlight=1-10)]

<span data-ttu-id="4884b-445">Önceki `Get` yöntemi `Order = 2, Template = api/MyTestApi`döndürür.</span><span class="sxs-lookup"><span data-stu-id="4884b-445">The preceding `Get` method returns `Order = 2, Template = api/MyTestApi`.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="use-application-model-to-customize-attribute-routes"></a><span data-ttu-id="4884b-446">Öznitelik yollarını özelleştirmek için uygulama modelini kullanın</span><span class="sxs-lookup"><span data-stu-id="4884b-446">Use application model to customize attribute routes</span></span>

<span data-ttu-id="4884b-447">Uygulama modeli:</span><span class="sxs-lookup"><span data-stu-id="4884b-447">The application model:</span></span>

* <span data-ttu-id="4884b-448">, Başlangıçta oluşturulan bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-448">Is an object model created at startup.</span></span>
* <span data-ttu-id="4884b-449">Bir uygulamadaki eylemleri yönlendirmek ve yürütmek için ASP.NET Core tarafından kullanılan tüm meta verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="4884b-449">Contains all of the metadata used by ASP.NET Core to route and execute the actions in an app.</span></span>

<span data-ttu-id="4884b-450">Uygulama modeli, yol özniteliklerinden toplanan tüm verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="4884b-450">The application model includes all of the data gathered from route attributes.</span></span> <span data-ttu-id="4884b-451">Yol özniteliklerinden alınan veriler `IRouteTemplateProvider` uygulama tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-451">The data from route attributes is provided by the `IRouteTemplateProvider` implementation.</span></span> <span data-ttu-id="4884b-452">Adlandır</span><span class="sxs-lookup"><span data-stu-id="4884b-452">Conventions:</span></span>

* <span data-ttu-id="4884b-453">, Yönlendirmenin nasıl davranacağını özelleştirmek için uygulama modelini değiştirmek üzere yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-453">Can be written to modify the application model to customize how routing behaves.</span></span>
* <span data-ttu-id="4884b-454">Uygulama başlangıcında okundu.</span><span class="sxs-lookup"><span data-stu-id="4884b-454">Are read at app startup.</span></span>

<span data-ttu-id="4884b-455">Bu bölümde, uygulama modeli kullanılarak yönlendirmeyi özelleştirmenin temel bir örneği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="4884b-455">This section shows a basic example of customizing routing using application model.</span></span> <span data-ttu-id="4884b-456">Aşağıdaki kod, rotaları projenin klasör yapısıyla kabaca bir şekilde oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4884b-456">The following code makes routes roughly line up with the folder structure of the project.</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet)]

<span data-ttu-id="4884b-457">Aşağıdaki kod, `namespace` kuralının, öznitelik yönlendirilen denetleyicilere uygulanmasını engeller:</span><span class="sxs-lookup"><span data-stu-id="4884b-457">The following code prevents the `namespace` convention from being applied to controllers that are attribute routed:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet2)]

<span data-ttu-id="4884b-458">Örneğin, aşağıdaki denetleyici `NamespaceRoutingConvention`kullanmaz:</span><span class="sxs-lookup"><span data-stu-id="4884b-458">For example, the following controller doesn't use `NamespaceRoutingConvention`:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/ManagersController.cs?name=snippet&highlight=1)]

<span data-ttu-id="4884b-459">`NamespaceRoutingConvention.Apply` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="4884b-459">The `NamespaceRoutingConvention.Apply` method:</span></span>

* <span data-ttu-id="4884b-460">Denetleyici öznitelik yönlendirmemişse hiçbir şey yapmaz.</span><span class="sxs-lookup"><span data-stu-id="4884b-460">Does nothing if the controller is attribute routed.</span></span>
* <span data-ttu-id="4884b-461">`namespace`temel `namespace` kaldırılacak şekilde denetleyiciler şablonunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-461">Sets the controllers template based on the `namespace`, with the base `namespace` removed.</span></span>

<span data-ttu-id="4884b-462">`NamespaceRoutingConvention` `Startup.ConfigureServices`uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="4884b-462">The `NamespaceRoutingConvention` can be applied in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Startup.cs?name=snippet&highlight=1,14-18)]

<span data-ttu-id="4884b-463">Örneğin, aşağıdaki denetleyiciyi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="4884b-463">For example, consider the following controller:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/UsersController.cs)]

<span data-ttu-id="4884b-464">Önceki kodda:</span><span class="sxs-lookup"><span data-stu-id="4884b-464">In the preceding code:</span></span>

* <span data-ttu-id="4884b-465">Temel `namespace` `My.Application`.</span><span class="sxs-lookup"><span data-stu-id="4884b-465">The base `namespace` is `My.Application`.</span></span>
* <span data-ttu-id="4884b-466">Önceki denetleyicinin tam adı `My.Application.Admin.Controllers.UsersController`.</span><span class="sxs-lookup"><span data-stu-id="4884b-466">The full name of the preceding controller is `My.Application.Admin.Controllers.UsersController`.</span></span>
* <span data-ttu-id="4884b-467">`NamespaceRoutingConvention`, denetleyiciler şablonunu `Admin/Controllers/Users/[action]/{id?`olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-467">The `NamespaceRoutingConvention` sets the controllers template to `Admin/Controllers/Users/[action]/{id?`.</span></span>

<span data-ttu-id="4884b-468">`NamespaceRoutingConvention` bir denetleyiciye bir öznitelik olarak da uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="4884b-468">The `NamespaceRoutingConvention` can also be applied as an attribute on a controller:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/TestController.cs?name=snippet&highlight=1)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="4884b-469">Karma yönlendirme: öznitelik yönlendirme vs geleneksel yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4884b-469">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="4884b-470">ASP.NET Core uygulamalar geleneksel yönlendirme ve öznitelik yönlendirmenin kullanımını karıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-470">ASP.NET Core apps can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="4884b-471">Tarayıcılar için HTML sayfalarına hizmet veren denetleyiciler için geleneksel yollar ve REST API 'Lerine hizmet veren denetleyiciler için öznitelik yönlendirme kullanılması normaldir.</span><span class="sxs-lookup"><span data-stu-id="4884b-471">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="4884b-472">Eylemler genel olarak Dolaştırılan veya Attribute olarak yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-472">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="4884b-473">Bir yolu denetleyiciye koymak veya eylemi, BT özniteliği yönlendirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-473">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="4884b-474">Öznitelik yollarını tanımlayan eylemlere geleneksel yollar üzerinden ulaşılamıyor ve bunun tersi de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-474">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="4884b-475">Denetleyicideki **herhangi bir** rota özniteliği, denetleyici özniteliğindeki **Tüm** eylemlerin yönlendirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-475">**Any** route attribute on the controller makes **all** actions in the controller attribute routed.</span></span>

<span data-ttu-id="4884b-476">Öznitelik yönlendirme ve geleneksel yönlendirme aynı yönlendirme altyapısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-476">Attribute routing and conventional routing use the same routing engine.</span></span>

<a name="routing-url-gen-ref-label"></a>
<a name="ambient"></a>

## <a name="url-generation-and-ambient-values"></a><span data-ttu-id="4884b-477">URL oluşturma ve ortam değerleri</span><span class="sxs-lookup"><span data-stu-id="4884b-477">URL Generation and ambient values</span></span>

<span data-ttu-id="4884b-478">Uygulamalar, eylemlere URL bağlantıları oluşturmak için yönlendirme URL 'SI oluşturma özelliklerini kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-478">Apps can use routing URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="4884b-479">URL 'Leri oluşturmak, kod daha sağlam ve sürdürülebilir hale getirmek için sorunsuz kodlama URL 'Lerini ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="4884b-479">Generating URLs eliminates hardcoding URLs, making code more robust and maintainable.</span></span> <span data-ttu-id="4884b-480">Bu bölüm, MVC tarafından sunulan URL oluşturma özelliklerine odaklanır ve yalnızca URL oluşturma özelliğinin nasıl çalıştığına ilişkin temel bilgileri kapsar.</span><span class="sxs-lookup"><span data-stu-id="4884b-480">This section focuses on the URL generation features provided by MVC and only cover basics of how URL generation works.</span></span> <span data-ttu-id="4884b-481">URL oluşturma hakkında ayrıntılı bir açıklama için bkz. [yönlendirme](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="4884b-481">See [Routing](xref:fundamentals/routing) for a detailed description of URL generation.</span></span>

<span data-ttu-id="4884b-482"><xref:Microsoft.AspNetCore.Mvc.IUrlHelper> arabirimi, URL oluşturma için MVC ve yönlendirme arasındaki altyapının temel öğesidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-482">The <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> interface is the underlying element of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="4884b-483">`IUrlHelper` örneği, denetleyiciler, görünümler ve görünüm bileşenlerinde `Url` özelliği aracılığıyla kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-483">An instance of `IUrlHelper` is available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="4884b-484">Aşağıdaki örnekte `IUrlHelper` arabirimi, başka bir eyleme yönelik bir URL oluşturmak için `Controller.Url` özelliği aracılığıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-484">In the following example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="4884b-485">Uygulama varsayılan geleneksel yolu kullanıyorsa, `url` değişkenin değeri `/UrlGeneration/Destination`URL yol dizesidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-485">If the app is using the default conventional route, the value of the `url` variable is the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="4884b-486">Bu URL yolu, yönlendirme tarafından birleştirerek oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="4884b-486">This URL path is created by routing by combining:</span></span>

* <span data-ttu-id="4884b-487">Geçerli istekten, **ortam değerleri**olarak adlandırılan rota değerleri.</span><span class="sxs-lookup"><span data-stu-id="4884b-487">The route values from the current request, which are called **ambient values**.</span></span>
* <span data-ttu-id="4884b-488">`Url.Action` geçirilen değerler ve bu değerleri yol şablonuna değiştirme:</span><span class="sxs-lookup"><span data-stu-id="4884b-488">The values passed to `Url.Action` and substituting those values into the route template:</span></span>

``` text
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="4884b-489">Yol şablonundaki her bir rota parametresinin değeri, değerler ve ortam değerleri ile eşleşen adlara sahip olacak şekilde değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-489">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="4884b-490">Bir değere sahip olmayan bir rota parametresi şunları yapabilir:</span><span class="sxs-lookup"><span data-stu-id="4884b-490">A route parameter that doesn't have a value can:</span></span>

* <span data-ttu-id="4884b-491">Bir varsayılan değer varsa, bu değeri kullanın.</span><span class="sxs-lookup"><span data-stu-id="4884b-491">Use a default value if it has one.</span></span>
* <span data-ttu-id="4884b-492">İsteğe bağlı ise atlanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-492">Be skipped if it's optional.</span></span> <span data-ttu-id="4884b-493">Örneğin, yol şablonundan `id` `{controller}/{action}/{id?}`.</span><span class="sxs-lookup"><span data-stu-id="4884b-493">For example, the `id` from the  route template `{controller}/{action}/{id?}`.</span></span>

<span data-ttu-id="4884b-494">Herhangi bir gerekli yol parametresi karşılık gelen bir değere sahip değilse, URL oluşturma başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4884b-494">URL generation fails if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="4884b-495">Bir yol için URL oluşturma başarısız olursa, tüm yollar Denenene veya bir eşleşme bulunana kadar sonraki yol denenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-495">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="4884b-496">Önceki `Url.Action` örneği [geleneksel yönlendirmeyi](#cr)varsayar.</span><span class="sxs-lookup"><span data-stu-id="4884b-496">The preceding example of `Url.Action` assumes [conventional routing](#cr).</span></span> <span data-ttu-id="4884b-497">URL oluşturma, [öznitelik yönlendirme](#ar)ile benzer şekilde çalışır, ancak kavramlar farklıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-497">URL generation works similarly with [attribute routing](#ar), though the concepts are different.</span></span> <span data-ttu-id="4884b-498">Geleneksel yönlendirme ile:</span><span class="sxs-lookup"><span data-stu-id="4884b-498">With conventional routing:</span></span>

* <span data-ttu-id="4884b-499">Rota değerleri bir şablonu genişletmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-499">The route values are used to expand a template.</span></span>
* <span data-ttu-id="4884b-500">`controller` ve `action` için yol değerleri genellikle bu şablonda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-500">The route values for `controller` and `action` usually appear in that template.</span></span> <span data-ttu-id="4884b-501">Bu, yönlendirme ile eşleşen URL 'Ler bir kurala bağlı olduğundan, bu işe yarar.</span><span class="sxs-lookup"><span data-stu-id="4884b-501">This works because the URLs matched by routing adhere to a convention.</span></span>

<span data-ttu-id="4884b-502">Aşağıdaki örnek öznitelik yönlendirme kullanır:</span><span class="sxs-lookup"><span data-stu-id="4884b-502">The following example uses attribute routing:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationAttrController.cs?name=snippet_1)]

<span data-ttu-id="4884b-503">Yukarıdaki koddaki `Source` eylemi `custom/url/to/destination`oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4884b-503">The `Source` action in the preceding code generates `custom/url/to/destination`.</span></span>

<span data-ttu-id="4884b-504"><xref:Microsoft.AspNetCore.Routing.LinkGenerator>, `IUrlHelper`alternatif olarak ASP.NET Core 3,0 eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="4884b-504"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> was added in ASP.NET Core 3.0 as an alternative to `IUrlHelper`.</span></span> <span data-ttu-id="4884b-505">`LinkGenerator` benzer ancak daha esnek işlevler sunar.</span><span class="sxs-lookup"><span data-stu-id="4884b-505">`LinkGenerator` offers similar but more flexible functionality.</span></span> <span data-ttu-id="4884b-506">`IUrlHelper` yöntemlerin her biri, `LinkGenerator` ilgili bir yöntem ailesine da sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4884b-506">Each other the methods on `IUrlHelper` has a corresponding family of methods on `LinkGenerator` as well.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="4884b-507">Eylem adına göre URL 'Leri oluşturma</span><span class="sxs-lookup"><span data-stu-id="4884b-507">Generating URLs by action name</span></span>

<span data-ttu-id="4884b-508">[URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), [Linkgenerator. GetPathByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*)ve tüm ilgili aşırı yüklemeler, bir denetleyici adı ve eylem adı belirtilerek hedef uç noktası oluşturmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4884b-508">[Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), [LinkGenerator.GetPathByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*), and all related overloads all are designed to generate the target endpoint by specifying a controller name and action name.</span></span>

<span data-ttu-id="4884b-509">`Url.Action`kullanırken, `controller` ve `action` için geçerli yol değerleri çalışma zamanı tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="4884b-509">When using `Url.Action`, the current route values for `controller` and `action` are provided by the runtime:</span></span>

* <span data-ttu-id="4884b-510">`controller` ve `action` değeri hem [ortam değerlerinin](#ambient) hem de değerlerinin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-510">The value of `controller` and `action` are part of both [ambient values](#ambient) and values.</span></span> <span data-ttu-id="4884b-511">`Url.Action` yöntemi her zaman `action` ve `controller` geçerli değerlerini kullanır ve geçerli eyleme yönlendiren bir URL yolu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4884b-511">The method `Url.Action` always uses the current values of `action` and `controller` and generates a URL path that routes to the current action.</span></span>

<span data-ttu-id="4884b-512">Yönlendirme, bir URL oluştururken sağlanmayan bilgileri doldurmanızı sağlamak için çevresel değerlerde değerleri kullanmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="4884b-512">Routing attempts to use the values in ambient values to fill in information that wasn't provided when generating a URL.</span></span> <span data-ttu-id="4884b-513">Çevresel değerler `{ a = Alice, b = Bob, c = Carol, d = David }``{a}/{b}/{c}/{d}` gibi bir yol düşünün:</span><span class="sxs-lookup"><span data-stu-id="4884b-513">Consider a route like `{a}/{b}/{c}/{d}` with ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`:</span></span>

* <span data-ttu-id="4884b-514">Yönlendirmenin ek değer olmadan bir URL oluşturmak için yeterli bilgi vardır.</span><span class="sxs-lookup"><span data-stu-id="4884b-514">Routing has enough information to generate a URL without any additional values.</span></span>
* <span data-ttu-id="4884b-515">Tüm rota parametrelerinin bir değeri olduğundan, yönlendirmeye yeterli bilgi vardır.</span><span class="sxs-lookup"><span data-stu-id="4884b-515">Routing has enough information because all route parameters have a value.</span></span>

<span data-ttu-id="4884b-516">Değer `{ d = Donovan }` eklenirse:</span><span class="sxs-lookup"><span data-stu-id="4884b-516">If the value `{ d = Donovan }` is added:</span></span>

* <span data-ttu-id="4884b-517">`{ d = David }` değeri yoksayıldı.</span><span class="sxs-lookup"><span data-stu-id="4884b-517">The value `{ d = David }` is ignored.</span></span>
* <span data-ttu-id="4884b-518">Oluşturulan URL yolu `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="4884b-518">The generated URL path is `Alice/Bob/Carol/Donovan`.</span></span>

<span data-ttu-id="4884b-519">**Uyarı**: URL yolları hiyerarşiktir.</span><span class="sxs-lookup"><span data-stu-id="4884b-519">**Warning**: URL paths are hierarchical.</span></span> <span data-ttu-id="4884b-520">Yukarıdaki örnekte, değer `{ c = Cheryl }` eklenirse:</span><span class="sxs-lookup"><span data-stu-id="4884b-520">In the preceding example, if the value `{ c = Cheryl }` is added:</span></span>

* <span data-ttu-id="4884b-521">Değerlerin her ikisi de `{ c = Carol, d = David }` yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-521">Both of the values `{ c = Carol, d = David }` are ignored.</span></span>
* <span data-ttu-id="4884b-522">`d` için artık bir değer yoktur ve URL oluşturma başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4884b-522">There is no longer a value for `d` and URL generation fails.</span></span>
* <span data-ttu-id="4884b-523">`c` ve `d` istenen değerleri bir URL oluşturmak için belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-523">The desired values of `c` and `d` must be specified to generate a URL.</span></span>  

<span data-ttu-id="4884b-524">`{controller}/{action}/{id?}`varsayılan yol ile bu sorunu daha fazla beklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-524">You might expect to hit this problem with the default route `{controller}/{action}/{id?}`.</span></span> <span data-ttu-id="4884b-525">`Url.Action` her zaman açık bir `controller` ve `action` değerini belirttiğinden bu sorun pratikte bu soruna neden olur.</span><span class="sxs-lookup"><span data-stu-id="4884b-525">This problem is rare in practice because `Url.Action` always explicitly specifies a `controller` and `action` value.</span></span>

<span data-ttu-id="4884b-526">URL 'nin birkaç aşırı yüklemesi [. işlem](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) , `controller` ve `action`dışındaki rota parametreleri için değerler sağlamak üzere bir yol değerleri nesnesi alır.</span><span class="sxs-lookup"><span data-stu-id="4884b-526">Several overloads of [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) take a route values object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="4884b-527">Yol değerleri nesnesi genellikle `id`ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-527">The route values object is frequently used with `id`.</span></span> <span data-ttu-id="4884b-528">Örneğin, `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="4884b-528">For example, `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="4884b-529">Yol değerleri nesnesi:</span><span class="sxs-lookup"><span data-stu-id="4884b-529">The route values object:</span></span>

* <span data-ttu-id="4884b-530">Kural gereği genellikle anonim türdeki bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="4884b-530">By convention is usually an object of anonymous type.</span></span>
* <span data-ttu-id="4884b-531">`IDictionary<>` veya [poco](https://wikipedia.org/wiki/Plain_old_CLR_object)olabilir).</span><span class="sxs-lookup"><span data-stu-id="4884b-531">Can be an `IDictionary<>` or a [POCO](https://wikipedia.org/wiki/Plain_old_CLR_object)).</span></span>

<span data-ttu-id="4884b-532">Yol parametreleriyle eşleşmeyen ek rota değerleri sorgu dizesine konur.</span><span class="sxs-lookup"><span data-stu-id="4884b-532">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="4884b-533">Yukarıdaki kod `/Products/Buy/17?color=red`oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4884b-533">The preceding code generates `/Products/Buy/17?color=red`.</span></span>

<span data-ttu-id="4884b-534">Aşağıdaki kod mutlak URL oluşturur:</span><span class="sxs-lookup"><span data-stu-id="4884b-534">The following code generates an absolute URL:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet2)]

<span data-ttu-id="4884b-535">Mutlak URL oluşturmak için, aşağıdakilerden birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="4884b-535">To create an absolute URL, use one of the following:</span></span>

* <span data-ttu-id="4884b-536">`protocol`kabul eden aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="4884b-536">An overload that accepts a `protocol`.</span></span> <span data-ttu-id="4884b-537">Örneğin, yukarıdaki kod.</span><span class="sxs-lookup"><span data-stu-id="4884b-537">For example, the preceding code.</span></span>
* <span data-ttu-id="4884b-538">Varsayılan olarak mutlak URI 'Ler üreten [Linkgenerator. GetUriByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*).</span><span class="sxs-lookup"><span data-stu-id="4884b-538">[LinkGenerator.GetUriByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*), which generates absolute URIs by default.</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generate-urls-by-route"></a><span data-ttu-id="4884b-539">Yola göre URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="4884b-539">Generate URLs by route</span></span>

<span data-ttu-id="4884b-540">Yukarıdaki kod, denetleyiciyi ve eylem adını geçirerek bir URL oluşturmayı göstermiştir.</span><span class="sxs-lookup"><span data-stu-id="4884b-540">The preceding code demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="4884b-541">`IUrlHelper` Ayrıca, [URL. RouteUrl](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*) ailesi yöntemlerin de sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-541">`IUrlHelper` also provides the [Url.RouteUrl](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*) family of methods.</span></span> <span data-ttu-id="4884b-542">Bu yöntemler [URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*)ile benzerdir, ancak `action` ve `controller` geçerli değerlerini rota değerlerine kopyalamaz.</span><span class="sxs-lookup"><span data-stu-id="4884b-542">These methods are similar to [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="4884b-543">`Url.RouteUrl`en yaygın kullanımı:</span><span class="sxs-lookup"><span data-stu-id="4884b-543">The most common usage of `Url.RouteUrl`:</span></span>

* <span data-ttu-id="4884b-544">URL oluşturmak için bir yol adı belirtir.</span><span class="sxs-lookup"><span data-stu-id="4884b-544">Specifies a route name to generate the URL.</span></span>
* <span data-ttu-id="4884b-545">Genellikle bir denetleyici veya eylem adı belirtmez.</span><span class="sxs-lookup"><span data-stu-id="4884b-545">Generally doesn't specify a controller or action name.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGeneration2Controller.cs?name=snippet_1)]

<span data-ttu-id="4884b-546">Aşağıdaki Razor dosyası `Destination_Route`bir HTML bağlantısı oluşturur:</span><span class="sxs-lookup"><span data-stu-id="4884b-546">The following Razor file generates an HTML link to the `Destination_Route`:</span></span>

[!code-cshtml[](routing/samples/3.x/main/Views/Shared/MyLink.cshtml)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generate-urls-in-html-and-razor"></a><span data-ttu-id="4884b-547">HTML ve Razor 'de URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="4884b-547">Generate URLs in HTML and Razor</span></span>

<span data-ttu-id="4884b-548"><xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper>, `<form>` ve `<a>` öğeleri oluşturmak için [HTML. BeginForm](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*) ve [html. ActionLink](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*) <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper> yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-548"><xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper> provides the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper> methods [Html.BeginForm](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*) and [Html.ActionLink](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*) to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="4884b-549">Bu yöntemler bir URL oluşturmak için [URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) yöntemini kullanır ve benzer bağımsız değişkenleri kabul ederler.</span><span class="sxs-lookup"><span data-stu-id="4884b-549">These methods use the [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="4884b-550">`HtmlHelper` için `Url.RouteUrl` compan, benzer işlevlere sahip `Html.BeginRouteForm` ve `Html.RouteLink`.</span><span class="sxs-lookup"><span data-stu-id="4884b-550">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="4884b-551">Taghelmakalar, `form` TagHelper ve `<a>` TagHelper aracılığıyla URL 'Ler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4884b-551">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="4884b-552">Bunların her ikisi de kendi uygulamaları için `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="4884b-552">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="4884b-553">Daha fazla bilgi için bkz. [Formlardaki etiket yardımcıları](xref:mvc/views/working-with-forms) .</span><span class="sxs-lookup"><span data-stu-id="4884b-553">See [Tag Helpers in forms](xref:mvc/views/working-with-forms) for more information.</span></span>

<span data-ttu-id="4884b-554">Görünümler içinde `IUrlHelper`, yukarıdaki herhangi bir geçici URL nesli için `Url` özelliği aracılığıyla kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-554">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="url-generation-in-action-results"></a><span data-ttu-id="4884b-555">Eylem sonuçlarında URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="4884b-555">URL generation in Action Results</span></span>

<span data-ttu-id="4884b-556">Yukarıdaki örneklerde `IUrlHelper` bir denetleyicide kullanılması gösterildi.</span><span class="sxs-lookup"><span data-stu-id="4884b-556">The preceding examples showed using `IUrlHelper` in a controller.</span></span> <span data-ttu-id="4884b-557">Bir denetleyicideki en yaygın kullanım, bir eylem sonucunun parçası olarak bir URL oluşturmak olur.</span><span class="sxs-lookup"><span data-stu-id="4884b-557">The most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="4884b-558"><xref:Microsoft.AspNetCore.Mvc.ControllerBase> ve <xref:Microsoft.AspNetCore.Mvc.Controller> Taban sınıfları, başka bir eyleme başvuruda bulunan eylem sonuçları için kolay yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-558">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase> and <xref:Microsoft.AspNetCore.Mvc.Controller> base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="4884b-559">Kullanıcı girişini kabul ettikten sonra, yaygın olarak kullanılan bir kullanım yeniden yönlendirilmelidir:</span><span class="sxs-lookup"><span data-stu-id="4884b-559">One typical usage is to redirect after accepting user input:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/CustomerController.cs?name=snippet)]

<span data-ttu-id="4884b-560">Eylem sonuçları <xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction*> ve <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> gibi Fabrika yöntemleri `IUrlHelper`yöntemler için de benzer bir desenler izler.</span><span class="sxs-lookup"><span data-stu-id="4884b-560">The action results factory methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="4884b-561">Adanmış geleneksel yollar için özel durum</span><span class="sxs-lookup"><span data-stu-id="4884b-561">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="4884b-562">[Geleneksel yönlendirme](#cr) , [adanmış geleneksel yol](#dcr)olarak adlandırılan özel bir yol tanımı türünü kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-562">[Conventional routing](#cr) can use a special kind of route definition called a [dedicated conventional route](#dcr).</span></span> <span data-ttu-id="4884b-563">Aşağıdaki örnekte, `blog` adlı yol adanmış bir geleneksel yoldur:</span><span class="sxs-lookup"><span data-stu-id="4884b-563">In the following example, the route named `blog` is a dedicated conventional route:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<span data-ttu-id="4884b-564">Önceki yol tanımlarını kullanarak, `Url.Action("Index", "Home")` `/` URL yolunu `default` yolunu kullanarak oluşturur, ancak neden?</span><span class="sxs-lookup"><span data-stu-id="4884b-564">Using the preceding route definitions, `Url.Action("Index", "Home")` generates the URL path `/` using the `default` route, but why?</span></span> <span data-ttu-id="4884b-565">Yol değerlerini tahmin edebilirsiniz `{ controller = Home, action = Index }` `blog`kullanarak URL oluşturmak için yeterli olacaktır ve sonuç `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="4884b-565">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="4884b-566">[Adanmış geleneksel yollar](#dcr) , karşılık gelen bir rota parametresi olmayan varsayılan değerlerin özel bir davranışına dayanır ve bu da yol, URL oluşturma ile çok fazla [dodan](xref:fundamentals/routing#greedy) yararlanın.</span><span class="sxs-lookup"><span data-stu-id="4884b-566">[Dedicated conventional routes](#dcr) rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being too [greedy](xref:fundamentals/routing#greedy) with URL generation.</span></span> <span data-ttu-id="4884b-567">Bu durumda, varsayılan değerler `{ controller = Blog, action = Article }`ve ne `controller` ne de `action` yol parametresi olarak görünmez.</span><span class="sxs-lookup"><span data-stu-id="4884b-567">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="4884b-568">Yönlendirme URL oluşturma işlemi gerçekleştirdiğinde, belirtilen değerler varsayılan değerlerle eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-568">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="4884b-569">Değerler `{ controller = Home, action = Index }` `{ controller = Blog, action = Article }`eşleşmediğinden `blog` kullanarak URL oluşturma başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4884b-569">URL generation using `blog` fails because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="4884b-570">Ardından yönlendirme `default`denemeye geri döner ve başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="4884b-570">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="4884b-571">Alanlar</span><span class="sxs-lookup"><span data-stu-id="4884b-571">Areas</span></span>

<span data-ttu-id="4884b-572">[Bölgeler](xref:mvc/controllers/areas) , ilgili işlevselliği ayrı olarak bir grup içinde düzenlemek için kullanılan bir MVC özelliğidir:</span><span class="sxs-lookup"><span data-stu-id="4884b-572">[Areas](xref:mvc/controllers/areas) are an MVC feature used to organize related functionality into a group as a separate:</span></span>

* <span data-ttu-id="4884b-573">Denetleyici eylemleri için yönlendirme ad alanı.</span><span class="sxs-lookup"><span data-stu-id="4884b-573">Routing namespace for controller actions.</span></span>
* <span data-ttu-id="4884b-574">Görünümler için klasör yapısı.</span><span class="sxs-lookup"><span data-stu-id="4884b-574">Folder structure for views.</span></span>

<span data-ttu-id="4884b-575">Alanların kullanılması, farklı alanlara sahip oldukları sürece bir uygulamanın aynı ada sahip birden çok denetleyicisi olmasına olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-575">Using areas allows an app to have multiple controllers with the same name, as long as they have different areas.</span></span> <span data-ttu-id="4884b-576">Alanların kullanılması, başka bir yol parametresi ekleyerek yönlendirme amacına yönelik bir hiyerarşi oluşturur, `controller` ve `action``area`.</span><span class="sxs-lookup"><span data-stu-id="4884b-576">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="4884b-577">Bu bölümde yönlendirmenin alanlarla nasıl etkileşim kurduğu açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4884b-577">This section discusses how routing interacts with areas.</span></span> <span data-ttu-id="4884b-578">Alanların görünümlerle nasıl kullanıldığı hakkında ayrıntılar için bkz. [alanlara](xref:mvc/controllers/areas) bakın.</span><span class="sxs-lookup"><span data-stu-id="4884b-578">See [Areas](xref:mvc/controllers/areas) for details about how areas are used with views.</span></span>

<span data-ttu-id="4884b-579">Aşağıdaki örnek, MVC 'yi varsayılan geleneksel yolu kullanacak şekilde yapılandırır `Blog`adlı bir `area` için `area` yolu.</span><span class="sxs-lookup"><span data-stu-id="4884b-579">The following example configures MVC to use the default conventional route and an `area` route for an `area` named `Blog`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="4884b-580">Yukarıdaki kodda, `"blog_route"`oluşturmak için <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> çağırılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-580">In the preceding code, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> is called to create the `"blog_route"`.</span></span> <span data-ttu-id="4884b-581">`"Blog"`ikinci parametresi, alan adıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-581">The second parameter, `"Blog"`, is the area name.</span></span>

<span data-ttu-id="4884b-582">`/Manage/Users/AddUser`gibi bir URL yolu eşleştirilirken `"blog_route"` yolu `{ area = Blog, controller = Users, action = AddUser }`yol değerlerini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4884b-582">When matching a URL path like `/Manage/Users/AddUser`, the `"blog_route"` route generates the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="4884b-583">`area` yol değeri, `area`için varsayılan bir değer tarafından üretilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-583">The `area` route value is produced by a default value for `area`.</span></span> <span data-ttu-id="4884b-584">`MapAreaControllerRoute` tarafından oluşturulan yol, aşağıdaki ile eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="4884b-584">The route created by `MapAreaControllerRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup2.cs?name=snippet2)]

<span data-ttu-id="4884b-585">`MapAreaControllerRoute`, `area` için hem varsayılan değer hem de kısıtlama (Bu durumda `Blog`) kullanarak bir yol oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4884b-585">`MapAreaControllerRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="4884b-586">Varsayılan değer, yolun her zaman `{ area = Blog, ... }`üretmesini sağlar, kısıtlama, URL oluşturma için `{ area = Blog, ... }` değer gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4884b-586">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

<span data-ttu-id="4884b-587">Geleneksel yönlendirme sıra bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-587">Conventional routing is order-dependent.</span></span> <span data-ttu-id="4884b-588">Genel olarak, alanlar içeren rotalar daha önce bir alan olmadan rotalardan daha belirgin olduklarından yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-588">In general, routes with areas should be placed earlier as they're more specific than routes without an area.</span></span>

<span data-ttu-id="4884b-589">Önceki örneği kullanarak, yol değerleri `{ area = Blog, controller = Users, action = AddUser }` aşağıdaki eylemle eşleşir:</span><span class="sxs-lookup"><span data-stu-id="4884b-589">Using the preceding example, the route values `{ area = Blog, controller = Users, action = AddUser }` match the following action:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="4884b-590">[[Area]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) özniteliği, bir alanın parçası olarak denetleyiciyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="4884b-590">The [[Area]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute is what denotes a controller as part of an area.</span></span> <span data-ttu-id="4884b-591">Bu denetleyici `Blog` alanıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-591">This controller is in the `Blog` area.</span></span> <span data-ttu-id="4884b-592">`[Area]` özniteliği olmayan denetleyiciler hiçbir alanın üyesi değildir ve `area` yol değeri yönlendirme tarafından **sağlandığında eşleşmez.**</span><span class="sxs-lookup"><span data-stu-id="4884b-592">Controllers without an `[Area]` attribute are not members of any area, and do **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="4884b-593">Aşağıdaki örnekte, yalnızca listelenen ilk denetleyici `{ area = Blog, controller = Users, action = AddUser }`rota değerleriyle eşleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-593">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

<span data-ttu-id="4884b-594">Her denetleyicinin ad alanı, tamamlanma açısından burada gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4884b-594">The namespace of each controller is shown here for completeness.</span></span> <span data-ttu-id="4884b-595">Yukarıdaki denetleyiciler aynı ad alanını kullanıyorsa, bir derleyici hatası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4884b-595">If the preceding controllers uses the same namespace, a compiler error would be generated.</span></span> <span data-ttu-id="4884b-596">Sınıf ad alanlarının MVC 'nin yönlendirme üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="4884b-596">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="4884b-597">İlk iki denetleyici alanların üyeleridir ve yalnızca ilgili alan adı `area` rota değeri tarafından sağlandığında eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-597">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="4884b-598">Üçüncü denetleyici hiçbir alanın üyesi değildir ve yalnızca Yönlendirme tarafından `area` hiçbir değer sağlanmıyorsa eşleşemez.</span><span class="sxs-lookup"><span data-stu-id="4884b-598">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

<a name="aa"></a>

<span data-ttu-id="4884b-599">*Değer olmadan*eşleşme açısından, `area` değerinin yokluğu, `area` değeri null ya da boş dize olarak aynıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-599">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="4884b-600">Bir alan içinde bir eylem yürütürken, `area` için rota değeri, yönlendirme için, URL oluşturma için kullanılacak [çevresel bir değer](#ambient) olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-600">When executing an action inside an area, the route value for `area` is available as an [ambient value](#ambient) for routing to use for URL generation.</span></span> <span data-ttu-id="4884b-601">Bu, varsayılan olarak, aşağıdaki örnekte gösterildiği gibi, URL oluşturma için *yapışkan* olarak hareket ettiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4884b-601">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup3.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<span data-ttu-id="4884b-602">Aşağıdaki kod `/Zebra/Users/AddUser`bir URL oluşturur:</span><span class="sxs-lookup"><span data-stu-id="4884b-602">The following code generates a URL to `/Zebra/Users/AddUser`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/HomeController.cs?name=snippet)]

<a name="action"></a>

## <a name="action-definition"></a><span data-ttu-id="4884b-603">Eylem tanımı</span><span class="sxs-lookup"><span data-stu-id="4884b-603">Action definition</span></span>

<span data-ttu-id="4884b-604">Bir denetleyicide, [Nonactıon](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute) özniteliğine sahip olanlar hariç genel yöntemler eylemlerdir.</span><span class="sxs-lookup"><span data-stu-id="4884b-604">Public methods on a controller, except those with the [NonAction](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute) attribute, are actions.</span></span>

## <a name="sample-code"></a><span data-ttu-id="4884b-605">Örnek kod</span><span class="sxs-lookup"><span data-stu-id="4884b-605">Sample code</span></span>

 * <span data-ttu-id="4884b-606">[Mydisplayrouteınfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) yöntemi [örnek karşıdan yüklemeye](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) dahildir ve yönlendirme bilgilerini görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-606">The [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) method is included in the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) and is used to display routing information.</span></span>
* <span data-ttu-id="4884b-607">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4884b-607">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4884b-608">ASP.NET Core MVC, gelen isteklerin URL 'Leriyle eşleştirmek ve bunları eylemlerle eşlemek için yönlendirme [Ara yazılımını](xref:fundamentals/middleware/index) kullanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-608">ASP.NET Core MVC uses the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="4884b-609">Yollar başlangıç kodunda veya özniteliklerde tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4884b-609">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="4884b-610">Yollar URL yollarının eylemlerle nasıl eşleştirileceği açıklanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-610">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="4884b-611">Yollar, yanıt olarak gönderilen URL 'Leri (bağlantılar için) oluşturmak için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-611">Routes are also used to generate URLs (for links) sent out in responses.</span></span>

<span data-ttu-id="4884b-612">Eylemler genel olarak Dolaştırılan veya Attribute olarak yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-612">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="4884b-613">Bir yolu denetleyiciye koymak veya eylemi, BT özniteliği yönlendirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-613">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="4884b-614">Daha fazla bilgi için bkz. [karma yönlendirme](#routing-mixed-ref-label) .</span><span class="sxs-lookup"><span data-stu-id="4884b-614">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="4884b-615">Bu belge, MVC ve yönlendirme arasındaki etkileşimleri ve tipik MVC uygulamalarının yönlendirme özelliklerini nasıl kullandığını açıklar.</span><span class="sxs-lookup"><span data-stu-id="4884b-615">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="4884b-616">Gelişmiş yönlendirme hakkında ayrıntılar için bkz. [yönlendirme](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="4884b-616">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="4884b-617">Yönlendirme ara yazılımını ayarlama</span><span class="sxs-lookup"><span data-stu-id="4884b-617">Setting up Routing Middleware</span></span>

<span data-ttu-id="4884b-618">*Yapılandırma* yönteminde şuna benzer bir kod görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4884b-618">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="4884b-619">`UseMvc`çağrısının içinde `MapRoute`, `default` rota olarak başvurabileceğiniz tek bir yol oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-619">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="4884b-620">Çoğu MVC uygulaması, `default` yoluna benzer bir şablon içeren bir yol kullanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-620">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="4884b-621">Yol şablonu `"{controller=Home}/{action=Index}/{id?}"` `/Products/Details/5` gibi bir URL yoluyla eşleştirebilir ve yolu simgeleştirerek `{ controller = Products, action = Details, id = 5 }` yol değerlerini ayıklar.</span><span class="sxs-lookup"><span data-stu-id="4884b-621">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="4884b-622">MVC, `ProductsController` adlı bir denetleyiciyi bulmaya çalışır ve `Details`eylemi çalıştırır:</span><span class="sxs-lookup"><span data-stu-id="4884b-622">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="4884b-623">Bu örnekte model bağlamanın, bu eylemi çağırırken `id` parametresini `5` olarak ayarlamak için `id = 5` değerini kullanabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4884b-623">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="4884b-624">Daha fazla ayrıntı için [model bağlamaya](../models/model-binding.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="4884b-624">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="4884b-625">`default` yolunu kullanma:</span><span class="sxs-lookup"><span data-stu-id="4884b-625">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="4884b-626">Yol şablonu:</span><span class="sxs-lookup"><span data-stu-id="4884b-626">The route template:</span></span>

* <span data-ttu-id="4884b-627">`{controller=Home}` varsayılan olarak `Home` tanımlar `controller`</span><span class="sxs-lookup"><span data-stu-id="4884b-627">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="4884b-628">`{action=Index}` varsayılan olarak `Index` tanımlar `action`</span><span class="sxs-lookup"><span data-stu-id="4884b-628">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="4884b-629">`{id?}` `id` isteğe bağlı olarak tanımlar</span><span class="sxs-lookup"><span data-stu-id="4884b-629">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="4884b-630">Bir eşleşme için URL yolunda varsayılan ve isteğe bağlı yol parametrelerinin mevcut olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4884b-630">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="4884b-631">Yol şablonu sözdiziminin ayrıntılı açıklaması için bkz. [route Template Reference](xref:fundamentals/routing#route-template-reference) .</span><span class="sxs-lookup"><span data-stu-id="4884b-631">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="4884b-632">`"{controller=Home}/{action=Index}/{id?}"`, `/` URL yolu ile eşleştirebilir ve `{ controller = Home, action = Index }`yol değerlerini üretecektir.</span><span class="sxs-lookup"><span data-stu-id="4884b-632">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="4884b-633">`controller` ve `action` değerleri varsayılan değerleri kullanır `id`, URL yolunda karşılık gelen bir kesim olmadığından, bu değer oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="4884b-633">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="4884b-634">MVC bu yol değerlerini kullanarak `HomeController` ve `Index` eylemini seçer:</span><span class="sxs-lookup"><span data-stu-id="4884b-634">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="4884b-635">Bu denetleyici tanımı ve yönlendirme şablonunu kullanarak, aşağıdaki URL yollarından herhangi biri için `HomeController.Index` eylemi yürütülür:</span><span class="sxs-lookup"><span data-stu-id="4884b-635">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="4884b-636">Kolaylık yöntemi `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="4884b-636">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="4884b-637">Değiştirmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="4884b-637">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="4884b-638">`UseMvc` ve `UseMvcWithDefaultRoute` ara yazılım ardışık düzenine bir `RouterMiddleware` örneği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4884b-638">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="4884b-639">MVC, doğrudan ara yazılım ile etkileşime girmez ve istekleri işlemek için yönlendirmeyi kullanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-639">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="4884b-640">MVC bir `MvcRouteHandler`örneği aracılığıyla yollara bağlanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-640">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="4884b-641">`UseMvc` içindeki kod aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="4884b-641">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="4884b-642">`UseMvc` doğrudan hiçbir yol tanımlamıyor, yol koleksiyonuna `attribute` yolu için bir yer tutucu ekler.</span><span class="sxs-lookup"><span data-stu-id="4884b-642">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="4884b-643">Aşırı yükleme `UseMvc(Action<IRouteBuilder>)` kendi rotalarınızı eklemenize ve öznitelik yönlendirmeyi de desteklemenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-643">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="4884b-644">`UseMvc` ve tüm çeşitlemeleri, `UseMvc`yapılandırma şeklinden bağımsız olarak her zaman kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-644">`UseMvc` and all of its variations add a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="4884b-645">`UseMvcWithDefaultRoute` varsayılan bir yol tanımlar ve öznitelik yönlendirmeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="4884b-645">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="4884b-646">[Öznitelik yönlendirme](#attribute-routing-ref-label) bölümü öznitelik yönlendirme hakkında daha fazla ayrıntı içerir.</span><span class="sxs-lookup"><span data-stu-id="4884b-646">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="4884b-647">Geleneksel yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4884b-647">Conventional routing</span></span>

<span data-ttu-id="4884b-648">`default` yolu:</span><span class="sxs-lookup"><span data-stu-id="4884b-648">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="4884b-649">Yukarıdaki kod geleneksel yönlendirmeye bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="4884b-649">The preceding code is an example of a conventional routing.</span></span> <span data-ttu-id="4884b-650">Bu stil, URL yolları için bir *kural* oluşturduğundan geleneksel yönlendirme olarak adlandırılır:</span><span class="sxs-lookup"><span data-stu-id="4884b-650">This style is called conventional routing because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="4884b-651">İlk yol segmenti, denetleyicinin adıyla eşlenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-651">The first path segment maps to the controller name.</span></span>
* <span data-ttu-id="4884b-652">İkincisi eylem adıyla eşlenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-652">The second maps to the action name.</span></span>
* <span data-ttu-id="4884b-653">Üçüncü segment, isteğe bağlı bir `id`için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-653">The third segment is used for an optional `id`.</span></span> <span data-ttu-id="4884b-654">`id` bir model varlığına eşlenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-654">`id` maps to a model entity.</span></span>

<span data-ttu-id="4884b-655">Bu `default` yolunu kullanarak, URL yolu `/Products/List` `ProductsController.List` eylemine eşlenir ve `/Blog/Article/17` eşlenir `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="4884b-655">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="4884b-656">Bu eşleme **yalnızca** denetleyiciye ve eylem adlarına dayalıdır ve ad alanları, kaynak dosya konumları veya yöntem parametrelerine göre değildir.</span><span class="sxs-lookup"><span data-stu-id="4884b-656">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="4884b-657">Varsayılan yol ile geleneksel yönlendirmeyi kullanmak, tanımladığınız her eylem için yeni bir URL düzeniyle karşılaşmanıza gerek kalmadan uygulamayı hızlı bir şekilde oluşturmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-657">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="4884b-658">CRUD stilinde eylemlere sahip bir uygulama için denetleyicilerinizdeki URL 'Lerin tutarlılığı, kodunuzun basitleştirilmesine ve Kullanıcı arabiriminizi daha öngörülebilir hale getirmenize yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-658">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="4884b-659">`id`, yol şablonu tarafından isteğe bağlı olarak tanımlanır ve bu, eylemlerinizin URL 'nin bir parçası olarak sağlanmadan yürütebileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4884b-659">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="4884b-660">Genellikle, URL 'den `id` atlandığında ne olur, bu durum model bağlama tarafından `0` olarak ayarlanır ve sonuç olarak veritabanında eşleşen `id == 0`hiçbir varlık bulunamacaktır.</span><span class="sxs-lookup"><span data-stu-id="4884b-660">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="4884b-661">Öznitelik yönlendirme, bazı eylemler için gereken KIMLIĞI, diğerleri için değil, daha ayrıntılı bir denetim sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-661">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="4884b-662">Kurala göre belgeler, doğru kullanımlarda görünebilecekleri `id` gibi isteğe bağlı parametreleri de içerecektir.</span><span class="sxs-lookup"><span data-stu-id="4884b-662">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="4884b-663">Birden çok yol</span><span class="sxs-lookup"><span data-stu-id="4884b-663">Multiple routes</span></span>

<span data-ttu-id="4884b-664">`MapRoute`daha fazla çağrı ekleyerek `UseMvc` içine birden çok yol ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4884b-664">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="4884b-665">Bunun yapılması, birden çok kural tanımlamanızı veya belirli bir eyleme adanmış geleneksel yollar eklemenizi sağlar; örneğin:</span><span class="sxs-lookup"><span data-stu-id="4884b-665">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="4884b-666">Buradaki `blog` yol, geleneksel bir *geleneksel yoldur*, yani geleneksel yönlendirme sistemini kullanır, ancak belirli bir eyleme ayrılmıştır.</span><span class="sxs-lookup"><span data-stu-id="4884b-666">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="4884b-667">`controller` ve `action` yol şablonunda parametre olarak görünmadığından, bu yol yalnızca varsayılan değerlere sahip olabilir ve bu nedenle bu yol her zaman eylem `BlogController.Article`eşlenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-667">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="4884b-668">Rota koleksiyonundaki yollar sıralanır ve eklendikleri sırada işlenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-668">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="4884b-669">Bu örnekte, `blog` yolu `default` rotadan önce denenecek.</span><span class="sxs-lookup"><span data-stu-id="4884b-669">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="4884b-670">*Adanmış geleneksel yollar* genellıkle, URL yolunun kalan kısmını yakalamak için `{*article}` gibi **catch-all** Route parametrelerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-670">*Dedicated conventional routes* often use **catch-all** route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="4884b-671">Bu, ' çok Greedy ' yolunu diğer yollarla eşleştirirken hedeflediğiniz URL 'Lerle eşleşen bir yol haline getirir.</span><span class="sxs-lookup"><span data-stu-id="4884b-671">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="4884b-672">Bunu çözümlemek için ' Greedy ' yollarını daha sonra yol tablosuna koyun.</span><span class="sxs-lookup"><span data-stu-id="4884b-672">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="4884b-673">Dönüş</span><span class="sxs-lookup"><span data-stu-id="4884b-673">Fallback</span></span>

<span data-ttu-id="4884b-674">İstek işlemenin bir parçası olarak, MVC, uygulamanızdaki bir denetleyiciyi ve eylemi bulmak için yol değerlerinin kullanılabileceğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="4884b-674">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="4884b-675">Rota değerleri bir eylemle eşleşmezse, yol eşleşme olarak kabul edilmez ve sonraki rota denenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-675">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="4884b-676">Buna *geri dönüş*denir ve geleneksel yolların çakıştığı durumları basitleştirmek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4884b-676">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="4884b-677">Kesinleştirme eylemleri</span><span class="sxs-lookup"><span data-stu-id="4884b-677">Disambiguating actions</span></span>

<span data-ttu-id="4884b-678">İki eylem yönlendirme aracılığıyla eşleşiyorsa, MVC ' en iyi ' adayı seçmek için bir özel durum oluşturması veya bir özel durum oluşturmak için, MVC 'nin belirsizliğini</span><span class="sxs-lookup"><span data-stu-id="4884b-678">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="4884b-679">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4884b-679">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="4884b-680">Bu denetleyici, URL yolu `/Products/Edit/17` ile eşleşen iki eylemi tanımlar ve verileri `{ controller = Products, action = Edit, id = 17 }`yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="4884b-680">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="4884b-681">Bu, `Edit(int)` bir ürünü düzenlemek üzere bir form gösterdiği ve `Edit(int, Product)` postalanan formu işleyen MVC denetleyicileri için tipik bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="4884b-681">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="4884b-682">Bunu yapmak için bu olası MVC, istek bir HTTP `POST` olduğunda `Edit(int, Product)` ve HTTP fiili başka bir şey olduğunda `Edit(int)` ' ı seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="4884b-682">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="4884b-683">`HttpPostAttribute` (`[HttpPost]`), yalnızca HTTP fiili `POST`olduğunda eylemin seçili olmasını sağlayacak `IActionConstraint` uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-683">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="4884b-684">`IActionConstraint` olması, `Edit(int, Product)` ' daha iyi bir eşleşme `Edit(int)`, bu nedenle önce `Edit(int, Product)` denenmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-684">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="4884b-685">Yalnızca özelleştirilmiş senaryolarda özel `IActionConstraint` uygulamalar yazmanız gerekir, ancak diğer HTTP fiilleri için `HttpPostAttribute` benzer öznitelikler gibi özniteliklerin rol olduğunu anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-685">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="4884b-686">Geleneksel yönlendirmesinde, eylemler bir `show form -> submit form` iş akışının parçası olduğunda aynı eylem adını kullanmak yaygındır.</span><span class="sxs-lookup"><span data-stu-id="4884b-686">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="4884b-687">Bu düzenin rahatlığı, [ıactionconstraint 'ı anlama](#understanding-iactionconstraint) bölümünde daha sonra görünür hale gelir.</span><span class="sxs-lookup"><span data-stu-id="4884b-687">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="4884b-688">Birden çok yol eşleşirse ve MVC ' en iyi ' yolu bulamazsa, bir `AmbiguousActionException`oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4884b-688">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="4884b-689">Yol adları</span><span class="sxs-lookup"><span data-stu-id="4884b-689">Route names</span></span>

<span data-ttu-id="4884b-690">Aşağıdaki örneklerde `"blog"` ve `"default"` dizeler yol adlarıdır:</span><span class="sxs-lookup"><span data-stu-id="4884b-690">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="4884b-691">Yol adları, URL oluşturma için adlandırılmış yolun kullanılabilmesi için yola mantıksal bir ad verir.</span><span class="sxs-lookup"><span data-stu-id="4884b-691">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="4884b-692">Bu, yolların sıralaması URL oluşturma karmaşık hale geldiğinde URL oluşturmayı büyük ölçüde basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="4884b-692">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="4884b-693">Yol adları, uygulama genelinde benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-693">Route names must be unique application-wide.</span></span>

<span data-ttu-id="4884b-694">Yol adları, isteklerin URL 'SI ile eşleşmesini veya işlenmesini etkilemez; Bunlar yalnızca URL oluşturma için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-694">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="4884b-695">[Yönlendirme](xref:fundamentals/routing) , MVC 'ye özgü yardımcılardaki URL oluşturma da dahil olmak üzere URL oluşturma hakkında daha ayrıntılı bilgiler içerir.</span><span class="sxs-lookup"><span data-stu-id="4884b-695">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="4884b-696">Öznitelik yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4884b-696">Attribute routing</span></span>

<span data-ttu-id="4884b-697">Öznitelik yönlendirme eylemleri doğrudan yönlendirme şablonlarına eşlemek için bir öznitelik kümesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-697">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="4884b-698">Aşağıdaki örnekte, `app.UseMvc();` `Configure` yönteminde kullanılır ve hiçbir yol geçirilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-698">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="4884b-699">`HomeController`, varsayılan yol `{controller=Home}/{action=Index}/{id?}` eşleşeceğinize benzer bir URL kümesiyle eşleşir:</span><span class="sxs-lookup"><span data-stu-id="4884b-699">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

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

<span data-ttu-id="4884b-700">`HomeController.Index()` eylem, `/`, `/Home`veya `/Home/Index`URL yollarından herhangi biri için yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4884b-700">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="4884b-701">Bu örnek, öznitelik yönlendirme ve geleneksel yönlendirme arasında bir temel programlama farkı vurgulamaktadır.</span><span class="sxs-lookup"><span data-stu-id="4884b-701">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="4884b-702">Öznitelik yönlendirme, bir yol belirtmek için daha fazla giriş gerektirir; geleneksel varsayılan yol, yönlendirmeleri daha succinctly işler.</span><span class="sxs-lookup"><span data-stu-id="4884b-702">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="4884b-703">Ancak, öznitelik yönlendirme (ve gerektirir) her eylem için hangi rota şablonlarının uygulanacağını kesin olarak denetler.</span><span class="sxs-lookup"><span data-stu-id="4884b-703">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="4884b-704">Öznitelik yönlendirme ile, denetleyici adı ve eylem adları, **herhangi** bir eylem seçildiği hiçbir rol oynar.</span><span class="sxs-lookup"><span data-stu-id="4884b-704">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="4884b-705">Bu örnek, önceki örnekle aynı URL 'Lerle eşleştirecektir.</span><span class="sxs-lookup"><span data-stu-id="4884b-705">This example will match the same URLs as the previous example.</span></span>

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
> <span data-ttu-id="4884b-706">Yukarıdaki yol şablonları `action`, `area`ve `controller`için yol parametreleri tanımlamaz.</span><span class="sxs-lookup"><span data-stu-id="4884b-706">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="4884b-707">Aslında, öznitelik rotalarında bu yol parametrelerine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="4884b-707">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="4884b-708">Yol şablonu bir eylemle zaten ilişkili olduğundan, URL 'den eylem adını ayrıştırmak mantıklı değildir.</span><span class="sxs-lookup"><span data-stu-id="4884b-708">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="4884b-709">Http [fiil] öznitelikleriyle öznitelik yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4884b-709">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="4884b-710">Öznitelik yönlendirme Ayrıca, `HttpPostAttribute`gibi `Http[Verb]` özniteliklerini de kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-710">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="4884b-711">Bu özniteliklerin hepsi bir yol şablonunu kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-711">All of these attributes can accept a route template.</span></span> <span data-ttu-id="4884b-712">Bu örnekte, aynı rota şablonuyla eşleşen iki eylem gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="4884b-712">This example shows two actions that match the same route template:</span></span>

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

<span data-ttu-id="4884b-713">`/products` gibi bir URL yolu için, HTTP fiili `GET` olduğunda `ProductsApi.ListProducts` eylemi yürütülür ve HTTP fiili `POST`olduğunda `ProductsApi.CreateProduct` yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4884b-713">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="4884b-714">Öznitelik yönlendirme öncelikle URL ile yol öznitelikleri tarafından tanımlanan yol şablonları kümesine göre eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-714">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="4884b-715">Bir rota şablonu eşleştiğinde, hangi eylemlerin yürütüleceğini belirleyen `IActionConstraint` kısıtlamalar uygulanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-715">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="4884b-716">Bir REST API oluştururken, eylem tüm HTTP yöntemlerini kabul edecek şekilde bir eylem yönteminde `[Route(...)]` kullanmak isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="4884b-716">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method as the action will accept all HTTP methods.</span></span> <span data-ttu-id="4884b-717">API 'nizin neleri desteklediği hakkında kesin olması için daha özel `Http*Verb*Attributes` kullanmak daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-717">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="4884b-718">REST API 'lerinin istemcileri, hangi yolların ve HTTP fiillerinin belirli mantıksal işlemlere eşlendiğini bilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-718">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="4884b-719">Bir öznitelik yolu belirli bir eyleme uyguladığı için, yol şablonu tanımının bir parçası olarak gerekli parametreleri yapmak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="4884b-719">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="4884b-720">Bu örnekte, URL yolunun bir parçası olarak `id` gereklidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-720">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="4884b-721">`ProductsApi.GetProduct(int)` eylemi, `/products/3` gibi bir URL yolu için yürütülür, ancak `/products`gibi bir URL yolu için değil.</span><span class="sxs-lookup"><span data-stu-id="4884b-721">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="4884b-722">Yol şablonlarının ve ilgili seçeneklerin tam açıklaması için bkz. [yönlendirme](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="4884b-722">See [Routing](xref:fundamentals/routing) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="4884b-723">Yol adı</span><span class="sxs-lookup"><span data-stu-id="4884b-723">Route Name</span></span>

<span data-ttu-id="4884b-724">Aşağıdaki kod `Products_List`*yol adını* tanımlar:</span><span class="sxs-lookup"><span data-stu-id="4884b-724">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="4884b-725">Yol adları, belirli bir yolu temel alan bir URL oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-725">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="4884b-726">Rota adlarının, yönlendirmenin URL eşleştirme davranışına etkisi yoktur ve yalnızca URL oluşturma için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-726">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="4884b-727">Yol adları, uygulama genelinde benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-727">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="4884b-728">Bunu, `id` parametresini isteğe bağlı (`{id?}`) olarak tanımlayan geleneksel *varsayılan rotayla*karşıtın.</span><span class="sxs-lookup"><span data-stu-id="4884b-728">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="4884b-729">API 'Leri tam olarak belirtme özelliği, `/products` ve `/products/5` farklı eylemlere dağıtılması gibi avantajlar sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-729">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="4884b-730">Yolları birleştirme</span><span class="sxs-lookup"><span data-stu-id="4884b-730">Combining routes</span></span>

<span data-ttu-id="4884b-731">Öznitelik yönlendirmeyi daha az tekrarlı hale getirmek için, denetleyicideki yol öznitelikleri, bireysel eylemlerdeki rota öznitelikleriyle birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-731">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="4884b-732">Denetleyicide tanımlanan tüm yol şablonları, eylemlerdeki rota şablonlarına eklenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-732">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="4884b-733">Bir Route özniteliğinin denetleyiciye yerleştirilmesi, denetleyicideki **Tüm** eylemlerin öznitelik yönlendirme kullanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-733">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

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

<span data-ttu-id="4884b-734">Bu örnekte, URL yolu `/products` `ProductsApi.ListProducts`ile eşleştirebilir ve URL yolu `/products/5` `ProductsApi.GetProduct(int)`eşleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-734">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="4884b-735">Bu eylemlerin her ikisi de, `HttpGetAttribute`olarak işaretlendiğinden HTTP `GET` eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-735">Both of these actions only match HTTP `GET` because they're marked with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="4884b-736">`/` veya `~/` ile başlayan bir eyleme uygulanan yol şablonları denetleyiciye uygulanan yol şablonları ile birleştirilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-736">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="4884b-737">Bu örnek, *varsayılan rotaya*benzer bir URL yolları kümesiyle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-737">This example matches a set of URL paths similar to the *default route*.</span></span>

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

### <a name="ordering-attribute-routes"></a><span data-ttu-id="4884b-738">Öznitelik yollarını sıralama</span><span class="sxs-lookup"><span data-stu-id="4884b-738">Ordering attribute routes</span></span>

<span data-ttu-id="4884b-739">Tanımlı bir düzende yürütülen geleneksel yolların aksine, öznitelik yönlendirme bir ağaç oluşturur ve tüm yollarla aynı anda eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-739">In contrast to conventional routes, which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="4884b-740">Bu, yol girişleri ideal bir sıralamaya yerleştirildiyse olduğu gibi davranır; en özel yolların, daha genel yollardan önce yürütülmesi şansınız vardır.</span><span class="sxs-lookup"><span data-stu-id="4884b-740">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="4884b-741">Örneğin, `blog/search/{topic}` gibi bir yol `blog/{*article}`gibi bir yol daha özgüdür.</span><span class="sxs-lookup"><span data-stu-id="4884b-741">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="4884b-742">İlk olarak ' çalıştırmaları ' `blog/search/{topic}` yolu için, varsayılan olarak, tek yapmanız gereken tek bir sıralama olduğundan mantıksal olarak konuşun.</span><span class="sxs-lookup"><span data-stu-id="4884b-742">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="4884b-743">Geleneksel yönlendirmeyi kullanarak, yolları istenen sırada yerleştirmekten geliştirici sorumludur.</span><span class="sxs-lookup"><span data-stu-id="4884b-743">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="4884b-744">Öznitelik yolları, tüm Framework yol özniteliklerinin `Order` özelliğini kullanarak bir sıra yapılandırabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-744">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="4884b-745">Yollar `Order` özelliğinin artan sıralamasına göre işlenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-745">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="4884b-746">Varsayılan sıra `0`.</span><span class="sxs-lookup"><span data-stu-id="4884b-746">The default order is `0`.</span></span> <span data-ttu-id="4884b-747">`Order = -1` kullanarak bir yolun ayarlanması, bir sipariş ayarlamadan önce çalıştırılacak rotalardan önce çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-747">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="4884b-748">`Order = 1` kullanarak bir yolun ayarlanması, varsayılan yol sıralaması sonrasında çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="4884b-748">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="4884b-749">`Order`bağlı olmadığından kaçının.</span><span class="sxs-lookup"><span data-stu-id="4884b-749">Avoid depending on `Order`.</span></span> <span data-ttu-id="4884b-750">URL alanınız, doğru sıralama değerlerinin doğru şekilde yönlendirilmesini gerektiriyorsa, istemciler de kafa karıştırıcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-750">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="4884b-751">Genel öznitelik yönlendirme ' de, URL eşleştirme ile doğru yolu seçer.</span><span class="sxs-lookup"><span data-stu-id="4884b-751">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="4884b-752">URL oluşturma için kullanılan varsayılan sıra çalışmıyorsa, yol adının bir geçersiz kılma olarak kullanılması genellikle `Order` özelliğini uygulamaktan daha basittir.</span><span class="sxs-lookup"><span data-stu-id="4884b-752">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="4884b-753">Razor Pages yönlendirme ve MVC denetleyici yönlendirme bir uygulamayı paylaşır.</span><span class="sxs-lookup"><span data-stu-id="4884b-753">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="4884b-754">Razor Pages konularındaki yol siparişi hakkında bilgiler [Razor Pages yol ve uygulama kuralları: yol sıralaması](xref:razor-pages/razor-pages-conventions#route-order)' nda bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-754">Information on route order in the Razor Pages topics is available at [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order).</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="4884b-755">Yol şablonlarında belirteç değiştirme ([denetleyici], [eylem], [alan])</span><span class="sxs-lookup"><span data-stu-id="4884b-755">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="4884b-756">Özellik yolları, bir belirteci köşeli ayraç içine alarak *belirteç değişimini* destekler (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="4884b-756">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="4884b-757">`[action]`, `[area]`ve `[controller]` belirteçleri, yolun tanımlandığı eylemden eylem adı, alan adı ve denetleyici adı değerleriyle değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4884b-757">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="4884b-758">Aşağıdaki örnekte, Eylemler, açıklamalarda açıklandığı gibi URL yollarıyla eşleşir:</span><span class="sxs-lookup"><span data-stu-id="4884b-758">In the following example, the actions match URL paths as described in the comments:</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="4884b-759">Belirteç değişikliği, öznitelik yollarının oluşturulması için son adım olarak gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-759">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="4884b-760">Yukarıdaki örnek aşağıdaki kodla aynı şekilde davranır:</span><span class="sxs-lookup"><span data-stu-id="4884b-760">The above example will behave the same as the following code:</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="4884b-761">Öznitelik rotaları de devralma ile birleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-761">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="4884b-762">Bu özellikle, belirteç değiştirme ile güçlü bir şekilde birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-762">This is particularly powerful combined with token replacement.</span></span>

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

<span data-ttu-id="4884b-763">Belirteç değişikliği, öznitelik rotaları tarafından tanımlanan yol adları için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-763">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="4884b-764">`[Route("[controller]/[action]", Name="[controller]_[action]")]` her eylem için benzersiz bir yol adı üretir.</span><span class="sxs-lookup"><span data-stu-id="4884b-764">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generates a unique route name for each action.</span></span>

<span data-ttu-id="4884b-765">Sabit belirteç değiştirme sınırlayıcısı `[` veya `]`eşleştirmek için, karakteri (`[[` veya `]]`) tekrarlayarak kaçış.</span><span class="sxs-lookup"><span data-stu-id="4884b-765">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="4884b-766">Belirteç değişimini özelleştirmek için bir parametre transformatörü kullanın</span><span class="sxs-lookup"><span data-stu-id="4884b-766">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="4884b-767">Belirteç değiştirme, bir parametre transformatörü kullanılarak özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-767">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="4884b-768">Bir parametre transformatörü `IOutboundParameterTransformer` uygular ve parametrelerin değerini dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="4884b-768">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="4884b-769">Örneğin, özel bir `SlugifyParameterTransformer` parametresi transformatörü `SubscriptionManagement` Route değerini `subscription-management`olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4884b-769">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="4884b-770">`RouteTokenTransformerConvention`, şu şekilde bir uygulama modeli kuralıdır:</span><span class="sxs-lookup"><span data-stu-id="4884b-770">The `RouteTokenTransformerConvention` is an application model convention that:</span></span>

* <span data-ttu-id="4884b-771">Bir uygulamadaki tüm öznitelik yollarına bir parametre transformatörü uygular.</span><span class="sxs-lookup"><span data-stu-id="4884b-771">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="4884b-772">Öznitelik yol belirteci değerlerini değiştirildikleri gibi özelleştirir.</span><span class="sxs-lookup"><span data-stu-id="4884b-772">Customizes the attribute route token values as they are replaced.</span></span>

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

<span data-ttu-id="4884b-773">`RouteTokenTransformerConvention`, `ConfigureServices`bir seçenek olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-773">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

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

### <a name="multiple-routes"></a><span data-ttu-id="4884b-774">Birden çok yol</span><span class="sxs-lookup"><span data-stu-id="4884b-774">Multiple Routes</span></span>

<span data-ttu-id="4884b-775">Öznitelik yönlendirme, aynı eyleme ulaşan birden çok yolun tanımlanmasını destekler.</span><span class="sxs-lookup"><span data-stu-id="4884b-775">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="4884b-776">Bunun en yaygın kullanımları, aşağıdaki örnekte gösterildiği gibi *varsayılan geleneksel yolun* davranışını taklit etmek olur:</span><span class="sxs-lookup"><span data-stu-id="4884b-776">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="4884b-777">Denetleyiciye birden çok yol özniteliği koymak, her birinin eylem yöntemlerinde yol özniteliklerinin her biriyle birleşmesi anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4884b-777">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

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

<span data-ttu-id="4884b-778">Birden çok yol özniteliği (`IActionConstraint`uygulayan) bir eyleme yerleştirildiğinde, her eylem kısıtlaması, onu tanımlayan öznitelikten yol şablonuyla birleştirir.</span><span class="sxs-lookup"><span data-stu-id="4884b-778">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

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
> <span data-ttu-id="4884b-779">Eylemlerde birden çok yolun kullanılması güçlü görünse de, uygulamanızın URL alanının basit ve iyi tanımlanmış tutulması daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-779">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="4884b-780">Yalnızca gerektiğinde eylemler üzerinde birden çok yol kullanın, örneğin mevcut istemcileri desteklemek için.</span><span class="sxs-lookup"><span data-stu-id="4884b-780">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="4884b-781">Öznitelik rotası isteğe bağlı parametreler, varsayılan değerler ve kısıtlamalar belirtme</span><span class="sxs-lookup"><span data-stu-id="4884b-781">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="4884b-782">Öznitelik yolları, isteğe bağlı parametreleri, varsayılan değerleri ve kısıtlamaları belirtmek için geleneksel yollarla aynı satır içi sözdizimini destekler.</span><span class="sxs-lookup"><span data-stu-id="4884b-782">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="4884b-783">Yol şablonu sözdiziminin ayrıntılı açıklaması için bkz. [route Template Reference](xref:fundamentals/routing#route-template-reference) .</span><span class="sxs-lookup"><span data-stu-id="4884b-783">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="4884b-784">`IRouteTemplateProvider` kullanarak özel yol öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="4884b-784">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="4884b-785">Çerçevede (`[Route(...)]`, `[HttpGet(...)]`, vb.) sunulan yol özniteliklerinin hepsi `IRouteTemplateProvider` arabirimini uygular.</span><span class="sxs-lookup"><span data-stu-id="4884b-785">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="4884b-786">MVC, uygulama başlatıldığında denetleyici sınıflarında ve eylem yöntemlerinde öznitelikler arar ve ilk yol kümesini oluşturmak için `IRouteTemplateProvider` uygulayan uygulamaları kullanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-786">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="4884b-787">Kendi yol öznitelerinizi tanımlamak için `IRouteTemplateProvider` uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4884b-787">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="4884b-788">Her `IRouteTemplateProvider`, özel bir yol şablonu, sırası ve adı ile tek bir yol tanımlamanızı sağlar:</span><span class="sxs-lookup"><span data-stu-id="4884b-788">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="4884b-789">Yukarıdaki örnekteki özniteliği, `[MyApiController]` uygulandığında otomatik olarak `Template` `"api/[controller]"` olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-789">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="4884b-790">Öznitelik yollarını özelleştirmek için uygulama modelini kullanma</span><span class="sxs-lookup"><span data-stu-id="4884b-790">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="4884b-791">*Uygulama modeli* , MVC tarafından eylemlerinizi yönlendirmek ve yürütmek için kullanılan tüm meta veriler ile başlangıçta oluşturulan bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-791">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="4884b-792">*Uygulama modeli* , yol özniteliklerinden toplanan tüm verileri içerir (`IRouteTemplateProvider`aracılığıyla).</span><span class="sxs-lookup"><span data-stu-id="4884b-792">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="4884b-793">Yönlendirme işleminin nasıl davranacağını özelleştirmek için, Başlangıç zamanında uygulama modelini değiştirmek üzere *kurallar* yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4884b-793">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="4884b-794">Bu bölümde, uygulama modeli kullanılarak yönlendirmeyi özelleştirmenin basit bir örneği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="4884b-794">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[](routing/samples/2.x/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="4884b-795">Karma yönlendirme: öznitelik yönlendirme vs geleneksel yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4884b-795">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="4884b-796">MVC uygulamaları, geleneksel yönlendirme ve öznitelik yönlendirmenin kullanımını karıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-796">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="4884b-797">Tarayıcılar için HTML sayfalarına hizmet veren denetleyiciler için geleneksel yollar ve REST API 'Lerine hizmet veren denetleyiciler için öznitelik yönlendirme kullanılması normaldir.</span><span class="sxs-lookup"><span data-stu-id="4884b-797">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="4884b-798">Eylemler genel olarak Dolaştırılan veya Attribute olarak yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-798">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="4884b-799">Bir yolu denetleyiciye koymak veya eylemi, BT özniteliği yönlendirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-799">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="4884b-800">Öznitelik yollarını tanımlayan eylemlere geleneksel yollar üzerinden ulaşılamıyor ve bunun tersi de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-800">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="4884b-801">Denetleyicideki **herhangi bir** rota özniteliği, denetleyici özniteliğindeki tüm eylemlerin yönlendirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-801">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="4884b-802">İki tür yönlendirme sisteminin ayırt edilmesini ne kadar ayırt eden, bir URL bir yol şablonuyla eşleştirdikten sonra uygulanan işlemdir.</span><span class="sxs-lookup"><span data-stu-id="4884b-802">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="4884b-803">Geleneksel yönlendirmesinde, eşleşmeden yol değerleri, tüm geleneksel yönlendirilmiş eylemlerin arama tablosundan eylemi ve denetleyiciyi seçmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-803">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="4884b-804">Öznitelik yönlendirmesinde, her şablon zaten bir eylemle ilişkilendirilir ve başka bir arama gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4884b-804">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="4884b-805">Karmaşık segmentler</span><span class="sxs-lookup"><span data-stu-id="4884b-805">Complex segments</span></span>

<span data-ttu-id="4884b-806">Karmaşık segmentler (örneğin, `[Route("/dog{token}cat")]`), sabit değerli olmayan değişmez değerler ile sağdan sola eşleştirilirken işlenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-806">Complex segments (for example, `[Route("/dog{token}cat")]`), are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="4884b-807">Bir açıklama için bkz. [kaynak kodu](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) .</span><span class="sxs-lookup"><span data-stu-id="4884b-807">See [the source code](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) for a description.</span></span> <span data-ttu-id="4884b-808">Daha fazla bilgi için [Bu soruna](https://github.com/dotnet/AspNetCore.Docs/issues/8197)bakın.</span><span class="sxs-lookup"><span data-stu-id="4884b-808">For more information, see [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/8197).</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="4884b-809">URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="4884b-809">URL Generation</span></span>

<span data-ttu-id="4884b-810">MVC uygulamaları, eylemlere URL bağlantıları oluşturmak için yönlendirmenin URL oluşturma özelliklerini kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-810">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="4884b-811">URL oluşturma, kodlarınızın daha sağlam ve sürdürülebilir hale getirilmesi için sorunsuz kodlama URL 'Lerini ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="4884b-811">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="4884b-812">Bu bölüm, MVC tarafından sunulan URL oluşturma özelliklerine odaklanır ve yalnızca URL oluşturmanın nasıl çalıştığına ilişkin temel bilgileri kapsar.</span><span class="sxs-lookup"><span data-stu-id="4884b-812">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="4884b-813">URL oluşturma hakkında ayrıntılı bir açıklama için bkz. [yönlendirme](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="4884b-813">See [Routing](xref:fundamentals/routing) for a detailed description of URL generation.</span></span>

<span data-ttu-id="4884b-814">`IUrlHelper` arabirimi, URL oluşturma için MVC ve yönlendirme arasındaki temel altyapı parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-814">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="4884b-815">Denetleyiciler, görünümler ve görünüm bileşenlerinde `Url` özelliği aracılığıyla kullanılabilen bir `IUrlHelper` örneğini bulacaksınız.</span><span class="sxs-lookup"><span data-stu-id="4884b-815">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="4884b-816">Bu örnekte `IUrlHelper` arabirimi, başka bir eyleme yönelik bir URL oluşturmak için `Controller.Url` özelliği aracılığıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-816">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="4884b-817">Uygulama varsayılan geleneksel rotayı kullanıyorsa, `url` değişkenin değeri `/UrlGeneration/Destination`URL yol dizesi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="4884b-817">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="4884b-818">Bu URL yolu, yönlendirme değerlerini, geçerli istekten (çevresel değerler), `Url.Action` aktarılan değerlerle ve bu değerleri yol şablonuna geçirerek birleştirerek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4884b-818">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="4884b-819">Yol şablonundaki her bir rota parametresinin değeri, değerler ve ortam değerleri ile eşleşen adlara sahip olacak şekilde değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-819">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="4884b-820">Bir değere sahip olmayan bir rota parametresi, varsa varsayılan bir değer kullanabilir veya isteğe bağlı ise (Bu örnekteki `id` olduğu gibi) atlanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-820">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="4884b-821">Gerekli yol parametresinin karşılık gelen bir değeri yoksa, URL oluşturma başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4884b-821">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="4884b-822">Bir yol için URL oluşturma başarısız olursa, tüm yollar Denenene veya bir eşleşme bulunana kadar sonraki yol denenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-822">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="4884b-823">Yukarıdaki `Url.Action` örneği geleneksel yönlendirmeyi varsayar, ancak URL oluşturma, öznitelik yönlendirimiyle benzer şekilde çalışır, ancak kavramlar farklıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-823">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="4884b-824">Geleneksel yönlendirme ile, bir şablonu genişletmek için yol değerleri kullanılır ve `controller` ve `action` için rota değerleri genellikle bu şablonda görünür-bu, yönlendirme ile eşleşen URL 'Ler bir *kurala*bağlı olduğundan, bu işe yarar.</span><span class="sxs-lookup"><span data-stu-id="4884b-824">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="4884b-825">Öznitelik yönlendirmesinde, `controller` ve `action` için yol değerlerinin şablonda görünmesine izin verilmez; bunun yerine kullanılacak şablonu aramak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-825">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="4884b-826">Bu örnek öznitelik yönlendirme kullanır:</span><span class="sxs-lookup"><span data-stu-id="4884b-826">This example uses attribute routing:</span></span>

[!code-csharp[](routing/samples/2.x/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="4884b-827">MVC, tüm öznitelik yönlendirilmiş eylemlerinin bir arama tablosunu oluşturur ve URL oluşturma için kullanılacak yol şablonunu seçmek üzere `controller` ve `action` değerleriyle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-827">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="4884b-828">Yukarıdaki örnekte `custom/url/to/destination` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4884b-828">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="4884b-829">Eylem adına göre URL 'Leri oluşturma</span><span class="sxs-lookup"><span data-stu-id="4884b-829">Generating URLs by action name</span></span>

<span data-ttu-id="4884b-830">`Url.Action` (`IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="4884b-830">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="4884b-831">`Action`) ve tüm ilgili aşırı yüklemeler, bir denetleyici adı ve eylem adı belirterek ne bağlandığınızı belirtmek istediğinizi temel alır.</span><span class="sxs-lookup"><span data-stu-id="4884b-831">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="4884b-832">`Url.Action`kullanırken, `controller` ve `action` için geçerli yol değerleri sizin için belirtilir; `controller` değeri, `action` hem *ortam değerlerinin* **hem** de *değerlerinin*bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-832">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="4884b-833">`Url.Action`yöntemi her zaman `action` ve `controller` geçerli değerlerini kullanır ve geçerli eyleme yönlendiren bir URL yolu oluşturacaktır.</span><span class="sxs-lookup"><span data-stu-id="4884b-833">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="4884b-834">Yönlendirme, bir URL oluştururken sağlamadığınız bilgileri doldurmanızı sağlamak için çevresel değerlerde değerleri kullanmayı dener.</span><span class="sxs-lookup"><span data-stu-id="4884b-834">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="4884b-835">Yönlendirme parametrelerinin bir değere sahip olduğundan, `{a}/{b}/{c}/{d}` ve çevresel değerler `{ a = Alice, b = Bob, c = Carol, d = David }`gibi bir yol kullanarak yönlendirme için ek değer olmadan bir URL oluşturmaya yetecek kadar bilgi vardır.</span><span class="sxs-lookup"><span data-stu-id="4884b-835">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="4884b-836">Değer `{ d = Donovan }`eklediyseniz, `{ d = David }` değeri yok sayılır ve oluşturulan URL yolu `Alice/Bob/Carol/Donovan`olur.</span><span class="sxs-lookup"><span data-stu-id="4884b-836">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="4884b-837">URL yolları hiyerarşiktir.</span><span class="sxs-lookup"><span data-stu-id="4884b-837">URL paths are hierarchical.</span></span> <span data-ttu-id="4884b-838">Yukarıdaki örnekte, değeri `{ c = Cheryl }`eklediyseniz her iki değer de `{ c = Carol, d = David }` yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="4884b-838">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="4884b-839">Bu durumda artık `d` için bir değer yoktur ve URL oluşturma başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4884b-839">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="4884b-840">İstediğiniz `c` ve `d`değerini belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="4884b-840">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="4884b-841">Bu sorunu varsayılan yol (`{controller}/{action}/{id?}`) ile () beklemeniz gerekebilir; ancak, `Url.Action` her zaman açıkça bir `controller` ve `action` değeri belirtmesi gibi uygulamada bu davranış hakkında nadiren karşılaşacaksınız.</span><span class="sxs-lookup"><span data-stu-id="4884b-841">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="4884b-842">Daha uzun `Url.Action` aşırı yüklemeleri, `controller` ve `action`dışındaki rota parametreleri için değerler sağlamak üzere ek bir *yol değerleri* nesnesi de alır.</span><span class="sxs-lookup"><span data-stu-id="4884b-842">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="4884b-843">Bu, en yaygın olarak `Url.Action("Buy", "Products", new { id = 17 })`gibi `id` kullanıldığını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="4884b-843">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="4884b-844">Kurala göre *yol değerleri* nesnesi genellikle anonim türdeki bir nesnedir, ancak bir `IDictionary<>` veya *düz bir .net nesnesi*de olabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-844">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="4884b-845">Yol parametreleriyle eşleşmeyen ek rota değerleri sorgu dizesine konur.</span><span class="sxs-lookup"><span data-stu-id="4884b-845">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="4884b-846">Mutlak URL oluşturmak için, `protocol`kabul eden bir aşırı yükleme kullanın: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="4884b-846">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="4884b-847">Rotaya göre URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="4884b-847">Generating URLs by route</span></span>

<span data-ttu-id="4884b-848">Yukarıdaki kod, denetleyiciyi ve eylem adını geçirerek bir URL oluşturmayı göstermiştir.</span><span class="sxs-lookup"><span data-stu-id="4884b-848">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="4884b-849">`IUrlHelper` Ayrıca `Url.RouteUrl` Yöntem ailesini da sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-849">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="4884b-850">Bu yöntemler `Url.Action`benzerdir, ancak `action` ve `controller` geçerli değerlerini rota değerlerine kopyalamaz.</span><span class="sxs-lookup"><span data-stu-id="4884b-850">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="4884b-851">En yaygın kullanım, genellikle bir denetleyici veya eylem *adı belirtmeden,* URL oluşturmak için belirli bir yolu kullanmak üzere bir yol adı belirtmektir.</span><span class="sxs-lookup"><span data-stu-id="4884b-851">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="4884b-852">HTML 'de URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="4884b-852">Generating URLs in HTML</span></span>

<span data-ttu-id="4884b-853">`IHtmlHelper`, `<form>` ve `<a>` öğeleri oluşturmak için `HtmlHelper` yöntemleri `Html.BeginForm` ve `Html.ActionLink` sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-853">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="4884b-854">Bu yöntemler bir URL oluşturmak için `Url.Action` yöntemini kullanır ve benzer bağımsız değişkenleri kabul ederler.</span><span class="sxs-lookup"><span data-stu-id="4884b-854">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="4884b-855">`HtmlHelper` için `Url.RouteUrl` compan, benzer işlevlere sahip `Html.BeginRouteForm` ve `Html.RouteLink`.</span><span class="sxs-lookup"><span data-stu-id="4884b-855">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="4884b-856">Taghelmakalar, `form` TagHelper ve `<a>` TagHelper aracılığıyla URL 'Ler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4884b-856">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="4884b-857">Bunların her ikisi de kendi uygulamaları için `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="4884b-857">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="4884b-858">Daha fazla bilgi için bkz. [formlarla çalışma](../views/working-with-forms.md) .</span><span class="sxs-lookup"><span data-stu-id="4884b-858">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="4884b-859">Görünümler içinde `IUrlHelper`, yukarıdaki herhangi bir geçici URL nesli için `Url` özelliği aracılığıyla kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-859">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="4884b-860">Eylem sonuçlarında URL oluşturma</span><span class="sxs-lookup"><span data-stu-id="4884b-860">Generating URLS in Action Results</span></span>

<span data-ttu-id="4884b-861">Yukarıdaki örnekler, bir denetleyicide `IUrlHelper` kullanılarak gösterilmektedir, ancak denetleyicideki en yaygın kullanım, bir eylem sonucunun parçası olarak bir URL oluşturmak olur.</span><span class="sxs-lookup"><span data-stu-id="4884b-861">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="4884b-862">`ControllerBase` ve `Controller` Taban sınıfları, başka bir eyleme başvuruda bulunan eylem sonuçları için kolay yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-862">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="4884b-863">Tipik bir kullanım, Kullanıcı girişi kabul edildikten sonra yeniden yönlendirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-863">One typical usage is to redirect after accepting user input.</span></span>

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

<span data-ttu-id="4884b-864">Eylem sonuçları Fabrika yöntemleri `IUrlHelper`yöntemlere benzer bir model izler.</span><span class="sxs-lookup"><span data-stu-id="4884b-864">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="4884b-865">Adanmış geleneksel yollar için özel durum</span><span class="sxs-lookup"><span data-stu-id="4884b-865">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="4884b-866">Geleneksel yönlendirme, *adanmış geleneksel yol*olarak adlandırılan özel bir yol tanımı türünü kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-866">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="4884b-867">Aşağıdaki örnekte, `blog` adlı yol adanmış bir geleneksel yoldur.</span><span class="sxs-lookup"><span data-stu-id="4884b-867">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="4884b-868">`Url.Action("Index", "Home")`, bu yol tanımlarını kullanarak `/` URL yolunu `default` rotası ile oluşturacak, ancak neden?</span><span class="sxs-lookup"><span data-stu-id="4884b-868">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="4884b-869">Yol değerlerini tahmin edebilirsiniz `{ controller = Home, action = Index }` `blog`kullanarak URL oluşturmak için yeterli olacaktır ve sonuç `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="4884b-869">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="4884b-870">Adanmış geleneksel yollar, URL oluşturmayla "çok Greedy" olmasını önleyen karşılık gelen bir yol parametresi olmayan varsayılan değerlerin özel bir davranışına bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-870">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="4884b-871">Bu durumda, varsayılan değerler `{ controller = Blog, action = Article }`ve ne `controller` ne de `action` yol parametresi olarak görünmez.</span><span class="sxs-lookup"><span data-stu-id="4884b-871">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="4884b-872">Yönlendirme URL oluşturma işlemi gerçekleştirdiğinde, belirtilen değerler varsayılan değerlerle eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-872">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="4884b-873">`blog` kullanılarak URL oluşturma başarısız olur çünkü değerler `{ controller = Home, action = Index }` `{ controller = Blog, action = Article }`eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="4884b-873">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="4884b-874">Ardından yönlendirme `default`denemeye geri döner ve başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="4884b-874">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="4884b-875">Alanlar</span><span class="sxs-lookup"><span data-stu-id="4884b-875">Areas</span></span>

<span data-ttu-id="4884b-876">[Bölgeler](areas.md) , ilgili işlevselliği ayrı bir yönlendirme-ad alanı (denetleyici eylemleri için) ve klasör yapısı (görünümler için) olarak bir grupla düzenlemek için kullanılan bir MVC özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-876">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="4884b-877">Alanların kullanılması, bir uygulamanın farklı *alanlara*sahip oldukları sürece aynı ada sahip birden çok denetleyicisi olmasına olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="4884b-877">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="4884b-878">Alanların kullanılması, başka bir yol parametresi ekleyerek yönlendirme amacına yönelik bir hiyerarşi oluşturur, `controller` ve `action``area`.</span><span class="sxs-lookup"><span data-stu-id="4884b-878">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="4884b-879">Bu bölüm, yönlendirmenin alanlarla nasıl etkileşime gireceğini tartışır. alanların görünümlerle nasıl kullanıldığı hakkında ayrıntılar için bkz. [alanlara](areas.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="4884b-879">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="4884b-880">Aşağıdaki örnek, MVC 'yi, `Blog`adlı bir alan için varsayılan geleneksel yolu ve bir *alan yolunu* kullanacak şekilde yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="4884b-880">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="4884b-881">`/Manage/Users/AddUser`gibi bir URL yolu eşleştirilirken, ilk yol `{ area = Blog, controller = Users, action = AddUser }`yol değerlerini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4884b-881">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="4884b-882">`area` yol değeri, `area`için varsayılan bir değer tarafından üretilir, aslında `MapAreaRoute` tarafından oluşturulan yol, aşağıdaki değere eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="4884b-882">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="4884b-883">`MapAreaRoute`, `area` için hem varsayılan değer hem de kısıtlama (Bu durumda `Blog`) kullanarak bir yol oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4884b-883">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="4884b-884">Varsayılan değer, yolun her zaman `{ area = Blog, ... }`üretmesini sağlar, kısıtlama, URL oluşturma için `{ area = Blog, ... }` değer gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4884b-884">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="4884b-885">Geleneksel yönlendirme sıra bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-885">Conventional routing is order-dependent.</span></span> <span data-ttu-id="4884b-886">Genel olarak, alanlar içeren rotalar, alan olmayan rotalardan daha belirgin olduklarından daha önce rota tablosuna yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-886">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="4884b-887">Yukarıdaki örneği kullanarak, yol değerleri aşağıdaki eylemle eşleşir:</span><span class="sxs-lookup"><span data-stu-id="4884b-887">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="4884b-888">`AreaAttribute`, bir alanın parçası olarak denetleyiciyi belirtir, bu denetleyicinin `Blog` alanında olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="4884b-888">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="4884b-889">`[Area]` özniteliği olmayan denetleyiciler hiçbir alanın üyesi değildir ve `area` yol değeri yönlendirme tarafından sağlandığında **eşleşmeyecektir** .</span><span class="sxs-lookup"><span data-stu-id="4884b-889">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="4884b-890">Aşağıdaki örnekte, yalnızca listelenen ilk denetleyici `{ area = Blog, controller = Users, action = AddUser }`rota değerleriyle eşleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-890">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="4884b-891">Her denetleyicinin ad alanı, tamamlanma için burada gösterilir. Aksi takdirde, denetleyicilerde adlandırma çakışması olur ve derleyici hatası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4884b-891">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="4884b-892">Sınıf ad alanlarının MVC 'nin yönlendirme üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="4884b-892">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="4884b-893">İlk iki denetleyici alanların üyeleridir ve yalnızca ilgili alan adı `area` rota değeri tarafından sağlandığında eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-893">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="4884b-894">Üçüncü denetleyici hiçbir alanın üyesi değildir ve yalnızca Yönlendirme tarafından `area` hiçbir değer sağlanmıyorsa eşleşemez.</span><span class="sxs-lookup"><span data-stu-id="4884b-894">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="4884b-895">*Değer olmadan*eşleşme açısından, `area` değerinin yokluğu, `area` değeri null ya da boş dize olarak aynıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-895">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="4884b-896">Bir alan içinde bir eylem yürütürken, `area` için rota değeri, yönlendirme için, URL oluşturma için kullanılacak *çevresel bir değer* olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-896">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="4884b-897">Bu, varsayılan olarak, aşağıdaki örnekte gösterildiği gibi, URL oluşturma için *yapışkan* olarak hareket ettiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4884b-897">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>
[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="4884b-898">Iactionconstraint 'i anlama</span><span class="sxs-lookup"><span data-stu-id="4884b-898">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="4884b-899">Bu bölüm, Framework iç işlevleri hakkında ayrıntılı bir bakış ve MVC 'nin yürütülecek eylemi nasıl seçtiği.</span><span class="sxs-lookup"><span data-stu-id="4884b-899">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="4884b-900">Tipik bir uygulama özel bir `IActionConstraint` gerektirmez</span><span class="sxs-lookup"><span data-stu-id="4884b-900">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="4884b-901">Büyük olasılıkla, arabirime tanıdık olmasanız bile `IActionConstraint` zaten kullandık.</span><span class="sxs-lookup"><span data-stu-id="4884b-901">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="4884b-902">`[HttpGet]` özniteliği ve benzer `[Http-VERB]` öznitelikleri, bir eylem yönteminin yürütülmesini sınırlandırmak için `IActionConstraint` uygular.</span><span class="sxs-lookup"><span data-stu-id="4884b-902">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="4884b-903">Varsayılan geleneksel yolun kabul edilmesinden, URL yolunun `/Products/Edit`, burada gösterilen eylemlerle **her ikisi de** eşleşen değerler `{ controller = Products, action = Edit }`üretecektir.</span><span class="sxs-lookup"><span data-stu-id="4884b-903">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="4884b-904">`IActionConstraint` terminolojisinde, her ikisi de rota verileriyle eşleştiğinden, bu eylemlerin her ikisi de aday olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-904">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="4884b-905">`HttpGetAttribute` yürütüldüğünde, *Edit ()* , *Get* için bir EŞLEŞMEDIR ve diğer http fiili için bir eşleşme değildir.</span><span class="sxs-lookup"><span data-stu-id="4884b-905">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="4884b-906">`Edit(...)` eyleminde tanımlı kısıtlama yok ve bu nedenle herhangi bir HTTP fiili ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4884b-906">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="4884b-907">Bu nedenle, yalnızca `POST` bir `Edit(...)` eşleştiğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="4884b-907">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="4884b-908">Ancak `GET` için her iki eylem de eşleşemez, ancak `IActionConstraint` bir eylem, olmadan bir eylemden en *iyi* şekilde değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-908">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="4884b-909">Bu nedenle `Edit()`, `[HttpGet]` daha belirgin olarak değerlendirilir ve her iki eylemin da eşleşeceğinden seçilecek.</span><span class="sxs-lookup"><span data-stu-id="4884b-909">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="4884b-910">Kavramsal olarak, `IActionConstraint` *aşırı yükleme*biçimidir, ancak aynı ada sahip yöntemlerin aşırı yüklenmesi yerıne aynı URL ile eşleşen eylemler arasında aşırı yüklenir.</span><span class="sxs-lookup"><span data-stu-id="4884b-910">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="4884b-911">Öznitelik yönlendirme `IActionConstraint` de kullanır ve farklı denetleyicilerden gelen eylemlere her ikisi de aday olarak kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="4884b-911">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="4884b-912">Iactionconstraint uygulama</span><span class="sxs-lookup"><span data-stu-id="4884b-912">Implementing IActionConstraint</span></span>

<span data-ttu-id="4884b-913">`IActionConstraint` kullanmanın en kolay yolu, `System.Attribute` türetilmiş bir sınıf oluşturmaktır ve bunları eylemleriniz ve denetleyicilerinize yerleştirmelidir.</span><span class="sxs-lookup"><span data-stu-id="4884b-913">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="4884b-914">MVC, öznitelik olarak uygulanan `IActionConstraint` otomatik olarak bulur.</span><span class="sxs-lookup"><span data-stu-id="4884b-914">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="4884b-915">Kısıtlama uygulamak için uygulama modelini kullanabilirsiniz ve bu, büyük olasılıkla en esnek yaklaşımdır ve bu sayede, nasıl uygulanabileceğini meta programlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4884b-915">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="4884b-916">Aşağıdaki örnekte, bir kısıtlama yol verilerinden bir *ülke kodunu* temel alan bir eylem seçer.</span><span class="sxs-lookup"><span data-stu-id="4884b-916">In the following example, a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="4884b-917">[GitHub 'daki tam örnek](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="4884b-917">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

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

<span data-ttu-id="4884b-918">`Accept` yöntemi uygulamaktan ve kısıtlamanın yürütülmesi için bir ' Order ' seçmeye sorumlusunuz.</span><span class="sxs-lookup"><span data-stu-id="4884b-918">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="4884b-919">Bu durumda `Accept` yöntemi, `country` rota değeri eşleştiğinde eylemin bir eşleşme olduğunu göstermek için `true` döndürür.</span><span class="sxs-lookup"><span data-stu-id="4884b-919">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="4884b-920">Bu, varolmayan bir eyleme geri dönüş sağlayan bir `RouteValueAttribute` farklıdır.</span><span class="sxs-lookup"><span data-stu-id="4884b-920">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="4884b-921">Örnek, bir `en-US` eylemi tanımlarsanız `fr-FR` gibi bir ülke kodunun `[CountrySpecific(...)]` uygulanmamış daha genel bir denetleyiciye geri dönemeyeceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="4884b-921">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="4884b-922">`Order` özelliği, kısıtlamanın parçası olan *aşamayı* belirler.</span><span class="sxs-lookup"><span data-stu-id="4884b-922">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="4884b-923">Eylem kısıtlamaları `Order`göre gruplar halinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="4884b-923">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="4884b-924">Örneğin, tüm Framework tarafından sunulan HTTP yöntemi öznitelikleri aynı aşamada çalışacak şekilde aynı `Order` değerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="4884b-924">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="4884b-925">İstediğiniz ilkeleri uygulamak için ihtiyacınız olan çok sayıda aşamaya sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4884b-925">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="4884b-926">`Order` bir değere karar vermek için, kısıtlamalarınızın HTTP yöntemlerinden önce uygulanıp uygulanmayacağı hakkında düşünün.</span><span class="sxs-lookup"><span data-stu-id="4884b-926">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="4884b-927">Daha az sayı önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="4884b-927">Lower numbers run first.</span></span>

::: moniker-end
