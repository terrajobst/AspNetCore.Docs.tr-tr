---
title: ASP.NET Core Razor Pages yol ve uygulama kuralları
author: guardrex
description: Yönlendirme ve uygulama modeli sağlayıcısı kurallarının sayfa yönlendirmeyi, bulmayı ve işlemeyi denetlemenize nasıl yardımcı olduğunu öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/22/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: a0a1eda69da34d1865fd11ef464c3697bcd01eff
ms.sourcegitcommit: 810d5831169770ee240d03207d6671dabea2486e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2019
ms.locfileid: "72779211"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="eb463-103">ASP.NET Core Razor Pages yol ve uygulama kuralları</span><span class="sxs-lookup"><span data-stu-id="eb463-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="eb463-104">[Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="eb463-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="eb463-105">Razor Pages uygulamalarında sayfa yönlendirmeyi, bulmayı ve işlemeyi denetlemek için sayfa [yolu ve uygulama modeli sağlayıcısı kurallarını](xref:mvc/controllers/application-model#conventions) nasıl kullanacağınızı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="eb463-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="eb463-106">Ayrı sayfalar için özel sayfa yolları yapılandırmanız gerektiğinde, bu konunun ilerleyen kısımlarında açıklanan [Addpageroute kuralına](#configure-a-page-route) sahip sayfalara yönlendirmeyi yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="eb463-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="eb463-107">Bir sayfa yolu belirtmek, yol kesimleri eklemek veya bir rotaya parametreler eklemek için, sayfanın `@page` yönergesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="eb463-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="eb463-108">Daha fazla bilgi için bkz. [özel rotalar](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="eb463-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="eb463-109">Yol kesimleri veya parametre adları olarak kullanılamayan ayrılmış sözcükler vardır.</span><span class="sxs-lookup"><span data-stu-id="eb463-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="eb463-110">Daha fazla bilgi için bkz. [Yönlendirme: ayrılmış yönlendirme adları](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="eb463-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="eb463-111">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="eb463-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="eb463-112">Senaryo</span><span class="sxs-lookup"><span data-stu-id="eb463-112">Scenario</span></span> | <span data-ttu-id="eb463-113">Örnek gösterilmektedir...</span><span class="sxs-lookup"><span data-stu-id="eb463-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="eb463-114">Model kuralları</span><span class="sxs-lookup"><span data-stu-id="eb463-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="eb463-115">Kurallar. Add</span><span class="sxs-lookup"><span data-stu-id="eb463-115">Conventions.Add</span></span><ul><li><span data-ttu-id="eb463-116">Ipageroutemodelconvention</span><span class="sxs-lookup"><span data-stu-id="eb463-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="eb463-117">Ipageapplicationmodelconvention</span><span class="sxs-lookup"><span data-stu-id="eb463-117">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="eb463-118">Ipagehandlermodelconvention</span><span class="sxs-lookup"><span data-stu-id="eb463-118">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="eb463-119">Uygulamanın sayfalarına bir yol şablonu ve üst bilgi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="eb463-119">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="eb463-120">Sayfa yolu eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="eb463-120">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="eb463-121">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="eb463-121">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="eb463-122">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="eb463-122">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="eb463-123">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="eb463-123">AddPageRoute</span></span></li></ul> | <span data-ttu-id="eb463-124">Bir klasördeki sayfalara ve tek bir sayfaya rota şablonu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="eb463-124">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="eb463-125">Sayfa modeli eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="eb463-125">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="eb463-126">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="eb463-126">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="eb463-127">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="eb463-127">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="eb463-128">ConfigureFilter (filtre sınıfı, lambda ifadesi veya filtre fabrikası)</span><span class="sxs-lookup"><span data-stu-id="eb463-128">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="eb463-129">Bir klasördeki sayfalara üst bilgi ekleyin, tek bir sayfaya üst bilgi ekleyin ve bir [filtre fabrikası](xref:mvc/controllers/filters#ifilterfactory) yapılandırarak uygulamanın sayfalarına üst bilgi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="eb463-129">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="eb463-130">Razor Pages kuralları, `Startup` sınıfında hizmet koleksiyonuna <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> uzantısı yöntemi kullanılarak eklenir ve yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="eb463-130">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="eb463-131">Aşağıdaki kural örnekleri bu konunun ilerleyen kısımlarında açıklanmıştır:</span><span class="sxs-lookup"><span data-stu-id="eb463-131">The following convention examples are explained later in this topic:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

::: moniker-end

## <a name="route-order"></a><span data-ttu-id="eb463-132">Rota sırası</span><span class="sxs-lookup"><span data-stu-id="eb463-132">Route order</span></span>

<span data-ttu-id="eb463-133">Rotalar işleme için bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> belirtir (rota eşleştirme).</span><span class="sxs-lookup"><span data-stu-id="eb463-133">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="eb463-134">Siparişi</span><span class="sxs-lookup"><span data-stu-id="eb463-134">Order</span></span>            | <span data-ttu-id="eb463-135">Davranış</span><span class="sxs-lookup"><span data-stu-id="eb463-135">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="eb463-136">-1</span><span class="sxs-lookup"><span data-stu-id="eb463-136">-1</span></span>               | <span data-ttu-id="eb463-137">Yol, diğer rotalar işlenmeden önce işlenir.</span><span class="sxs-lookup"><span data-stu-id="eb463-137">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="eb463-138">0</span><span class="sxs-lookup"><span data-stu-id="eb463-138">0</span></span>                | <span data-ttu-id="eb463-139">Sıra belirtilmemiş (varsayılan değer).</span><span class="sxs-lookup"><span data-stu-id="eb463-139">Order isn't specified (default value).</span></span> <span data-ttu-id="eb463-140">@No__t_0 atanmazsa (`Order = null`), yolun işlenmek üzere 0 (sıfır) olarak `Order`.</span><span class="sxs-lookup"><span data-stu-id="eb463-140">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="eb463-141">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="eb463-141">1, 2, &hellip; n</span></span> | <span data-ttu-id="eb463-142">Yol işleme sırasını belirtir.</span><span class="sxs-lookup"><span data-stu-id="eb463-142">Specifies the route processing order.</span></span> |

<span data-ttu-id="eb463-143">Yol işleme, kurala göre belirlenir:</span><span class="sxs-lookup"><span data-stu-id="eb463-143">Route processing is established by convention:</span></span>

* <span data-ttu-id="eb463-144">Yollar sıralı sırada işlenir (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="eb463-144">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="eb463-145">Yolların aynı `Order` olduğunda, en belirli yol önce daha az özel yollarla eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="eb463-145">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="eb463-146">Aynı `Order` ve aynı parametre sayısı ile rotalar bir istek URL 'siyle eşleşiyorsa, rotalar <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection> eklendiği sırada işlenir.</span><span class="sxs-lookup"><span data-stu-id="eb463-146">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="eb463-147">Mümkünse, belirlenen bir yol işleme sırasına bağlı olarak kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="eb463-147">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="eb463-148">Genellikle Yönlendirme, URL eşleştirme ile doğru yolu seçer.</span><span class="sxs-lookup"><span data-stu-id="eb463-148">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="eb463-149">İstekleri doğru yönlendirmek için yol `Order` özelliklerini ayarlamanız gerekiyorsa, uygulamanın yönlendirme şeması büyük olasılıkla istemciler için kafa karıştırıcı olur ve bakımını yapmak için kırıcı olur.</span><span class="sxs-lookup"><span data-stu-id="eb463-149">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="eb463-150">Uygulamanın yönlendirme şemasını basitleştirecek şekilde arama yapın.</span><span class="sxs-lookup"><span data-stu-id="eb463-150">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="eb463-151">Örnek uygulama, tek bir uygulama kullanarak birkaç yönlendirme senaryosunu göstermek için açık bir yol işleme sırası gerektirir.</span><span class="sxs-lookup"><span data-stu-id="eb463-151">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="eb463-152">Ancak, üretim uygulamalarında rota `Order` ayarlama uygulamalarından kaçınmaya çalışın.</span><span class="sxs-lookup"><span data-stu-id="eb463-152">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="eb463-153">Razor Pages yönlendirme ve MVC denetleyici yönlendirme bir uygulamayı paylaşır.</span><span class="sxs-lookup"><span data-stu-id="eb463-153">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="eb463-154">MVC konularındaki yol sırasıyla ilgili bilgiler, [Denetleyici eylemlerine yönlendirme sırasında mevcuttur: öznitelik yollarını sıralama](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="eb463-154">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="eb463-155">Model kuralları</span><span class="sxs-lookup"><span data-stu-id="eb463-155">Model conventions</span></span>

<span data-ttu-id="eb463-156">Razor Pages için uygulanan [model kuralları](xref:mvc/controllers/application-model#conventions) eklemek üzere <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> için bir temsilci ekleyin.</span><span class="sxs-lookup"><span data-stu-id="eb463-156">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="eb463-157">Tüm sayfalara bir rota modeli kuralı ekleme</span><span class="sxs-lookup"><span data-stu-id="eb463-157">Add a route model convention to all pages</span></span>

<span data-ttu-id="eb463-158">Sayfa yönlendirme modeli oluşturma sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnekleri koleksiyonuna bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> kullanın.</span><span class="sxs-lookup"><span data-stu-id="eb463-158">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="eb463-159">Örnek uygulama, uygulamadaki tüm sayfalara bir `{globalTemplate?}` Route şablonu ekler:</span><span class="sxs-lookup"><span data-stu-id="eb463-159">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="eb463-160">@No__t_1 için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği `1` olarak ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="eb463-160">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="eb463-161">Bu, örnek uygulamada aşağıdaki yol eşleştirme davranışını sağlar:</span><span class="sxs-lookup"><span data-stu-id="eb463-161">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="eb463-162">@No__t_0 için bir yol şablonu konuya daha sonra eklenir.</span><span class="sxs-lookup"><span data-stu-id="eb463-162">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="eb463-163">Iletişim sayfası yolu, `null` (`Order = 0`) varsayılan sırasına sahiptir, bu nedenle `{globalTemplate?}` Route şablonundan önce eşleşir.</span><span class="sxs-lookup"><span data-stu-id="eb463-163">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="eb463-164">Konunun ilerleyen kısımlarında `{aboutTemplate?}` yol şablonu eklenir.</span><span class="sxs-lookup"><span data-stu-id="eb463-164">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="eb463-165">@No__t_0 şablonuna `2` `Order` verilir.</span><span class="sxs-lookup"><span data-stu-id="eb463-165">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="eb463-166">@No__t_0 sayfası istendiğinde, `Order = 2` özelliğinin ayarlanması nedeniyle "RouteDataValue" `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["aboutTemplate"]` (`Order`) değil.</span><span class="sxs-lookup"><span data-stu-id="eb463-166">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="eb463-167">Konunun ilerleyen kısımlarında `{otherPagesTemplate?}` yol şablonu eklenir.</span><span class="sxs-lookup"><span data-stu-id="eb463-167">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="eb463-168">@No__t_0 şablonuna `2` `Order` verilir.</span><span class="sxs-lookup"><span data-stu-id="eb463-168">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="eb463-169">*Sayfalar/diğer sayfalar* klasöründeki herhangi bir sayfa bir yol parametresiyle (örneğin, `/OtherPages/Page1/RouteDataValue`) istendiğinde, `Order = 2` özelliğinin ayarlanması nedeniyle "routedatavalue", `RouteData.Values["otherPagesTemplate"]` (`Order`) değil `RouteData.Values["globalTemplate"]` (`Order = 1`) olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="eb463-169">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="eb463-170">Mümkün olan yerlerde, `Order` `Order = 0` sonuç olarak ayarlanmayın.</span><span class="sxs-lookup"><span data-stu-id="eb463-170">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="eb463-171">Doğru yolu seçmek için yönlendirmeyi güvenin.</span><span class="sxs-lookup"><span data-stu-id="eb463-171">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="eb463-172">@No__t_0 ekleme gibi Razor Pages seçenekler, MVC `Startup.ConfigureServices` hizmet koleksiyonuna eklendiğinde eklenir.</span><span class="sxs-lookup"><span data-stu-id="eb463-172">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="eb463-173">Örnek için bkz. [örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="eb463-173">For an example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="eb463-174">@No__t_0 'de örneğin hakkında sayfasını isteyin ve sonucu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="eb463-174">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![Hakkında sayfası, GlobalRouteValue 'un rota segmentiyle istenir.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="eb463-177">Tüm sayfalara uygulama modeli kuralı ekleme</span><span class="sxs-lookup"><span data-stu-id="eb463-177">Add an app model convention to all pages</span></span>

<span data-ttu-id="eb463-178">@No__t_0 kullanarak, sayfa uygulama modeli oluşturma sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnekleri koleksiyonuna bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ekleyin.</span><span class="sxs-lookup"><span data-stu-id="eb463-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="eb463-179">Bu ve diğer kuralları konunun ilerleyen kısımlarında göstermek için, örnek uygulama bir `AddHeaderAttribute` sınıfı içerir.</span><span class="sxs-lookup"><span data-stu-id="eb463-179">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="eb463-180">Sınıf Oluşturucusu bir `name` dize ve bir `values` dize dizisi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="eb463-180">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="eb463-181">Bu değerler, yanıt üst bilgisini ayarlamak için `OnResultExecuting` yönteminde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="eb463-181">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="eb463-182">Tam sınıf, konusunun ilerleyen kısımlarında [sayfa modeli eylem kuralları](#page-model-action-conventions) bölümünde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="eb463-182">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="eb463-183">Örnek uygulama, uygulamadaki tüm sayfalara bir başlık, `GlobalHeader` eklemek için `AddHeaderAttribute` sınıfını kullanır:</span><span class="sxs-lookup"><span data-stu-id="eb463-183">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="eb463-184">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="eb463-184">*Startup.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="eb463-185">@No__t_0 sırasında örneğin hakkında daha fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="eb463-185">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Hakkında sayfasının yanıt üstbilgileri GlobalHeader 'ın eklendiğini gösterir.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="eb463-187">Tüm sayfalara bir işleyici modeli kuralı ekleme</span><span class="sxs-lookup"><span data-stu-id="eb463-187">Add a handler model convention to all pages</span></span>

<span data-ttu-id="eb463-188">Sayfa işleyicisi modelinin oluşturulması sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnekleri koleksiyonuna bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> kullanın.</span><span class="sxs-lookup"><span data-stu-id="eb463-188">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="eb463-189">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="eb463-189">*Startup.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

## <a name="page-route-action-conventions"></a><span data-ttu-id="eb463-190">Sayfa yolu eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="eb463-190">Page route action conventions</span></span>

<span data-ttu-id="eb463-191">@No__t_0 türetilen varsayılan yol modeli sağlayıcısı, sayfa yollarını yapılandırmak için genişletilebilirlik noktaları sağlamak üzere tasarlanan kuralları çağırır.</span><span class="sxs-lookup"><span data-stu-id="eb463-191">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="eb463-192">Klasör Yönlendirme modeli kuralı</span><span class="sxs-lookup"><span data-stu-id="eb463-192">Folder route model convention</span></span>

<span data-ttu-id="eb463-193">Belirtilen klasör altındaki tüm sayfalar için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> bir eylemi çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="eb463-193">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="eb463-194">Örnek uygulama, *diğer sayfalar* klasöründeki sayfalara bir `{otherPagesTemplate?}` yol şablonu eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> kullanır:</span><span class="sxs-lookup"><span data-stu-id="eb463-194">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="eb463-195">@No__t_1 için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği `2` olarak ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="eb463-195">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="eb463-196">Bu, tek bir rota değeri sağlandığında `{globalTemplate?}` (konuda daha önce `1` olarak ayarlanan) şablonunun ilk yol veri değeri konumu için öncelik verilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="eb463-196">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="eb463-197">*Sayfalar/otherpages* klasöründeki bir sayfa bir yol parametresi değeri (örneğin, `/OtherPages/Page1/RouteDataValue`) ile isteniyorsa, `Order = 2` özelliğinin ayarlanması nedeniyle "routedatavalue", `RouteData.Values["otherPagesTemplate"]` (`Order`) değil `RouteData.Values["globalTemplate"]` (`Order = 1`) olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="eb463-197">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="eb463-198">Mümkün olan yerlerde, `Order` `Order = 0` sonuç olarak ayarlanmayın.</span><span class="sxs-lookup"><span data-stu-id="eb463-198">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="eb463-199">Doğru yolu seçmek için yönlendirmeyi güvenin.</span><span class="sxs-lookup"><span data-stu-id="eb463-199">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="eb463-200">Örnekteki Sayfa1 sayfasını `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` isteyin ve sonucu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="eb463-200">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Diğer sayfalar klasöründeki Sayfa1, GlobalRouteValue ve OtherPagesRouteValue yol kesimiyle istenir.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="eb463-203">Sayfa yönlendirme modeli kuralı</span><span class="sxs-lookup"><span data-stu-id="eb463-203">Page route model convention</span></span>

<span data-ttu-id="eb463-204">Belirtilen ada sahip sayfanın <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> bir eylemi çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="eb463-204">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="eb463-205">Örnek uygulama, hakkında sayfasına bir `{aboutTemplate?}` yol şablonu eklemek için `AddPageRouteModelConvention` kullanır:</span><span class="sxs-lookup"><span data-stu-id="eb463-205">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

<span data-ttu-id="eb463-206">@No__t_1 için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği `2` olarak ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="eb463-206">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="eb463-207">Bu, tek bir rota değeri sağlandığında `{globalTemplate?}` (konuda daha önce `1` olarak ayarlanan) şablonunun ilk yol veri değeri konumu için öncelik verilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="eb463-207">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="eb463-208">@No__t_0 sayfasında yol parametresi değeri varsa, `Order = 2` özelliğinin ayarlanması nedeniyle "RouteDataValue" `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["aboutTemplate"]` (`Order`) değil.</span><span class="sxs-lookup"><span data-stu-id="eb463-208">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="eb463-209">Mümkün olan yerlerde, `Order` `Order = 0` sonuç olarak ayarlanmayın.</span><span class="sxs-lookup"><span data-stu-id="eb463-209">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="eb463-210">Doğru yolu seçmek için yönlendirmeyi güvenin.</span><span class="sxs-lookup"><span data-stu-id="eb463-210">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="eb463-211">@No__t_0 'de örneğin hakkında sayfasını isteyin ve sonucu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="eb463-211">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![GlobalRouteValue ve AboutRouteValue için rota kesimlerle ilgili sayfa hakkında bilgi istenir.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="eb463-214">Sayfa yollarını özelleştirmek için bir parametre transformatörü kullanın</span><span class="sxs-lookup"><span data-stu-id="eb463-214">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="eb463-215">ASP.NET Core tarafından oluşturulan sayfa yolları, bir parametre transformatörü kullanılarak özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="eb463-215">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="eb463-216">Bir parametre transformatörü `IOutboundParameterTransformer` uygular ve parametrelerin değerini dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="eb463-216">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="eb463-217">Örneğin, özel bir `SlugifyParameterTransformer` parametresi transformatörü `SubscriptionManagement` Route değerini `subscription-management` olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="eb463-217">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="eb463-218">@No__t_0 Page Route model kuralı, bir uygulamadaki otomatik olarak oluşturulan sayfa yollarının klasör ve dosya adı kesimlerine bir parametre transformatörü uygular.</span><span class="sxs-lookup"><span data-stu-id="eb463-218">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="eb463-219">Örneğin, */Pages/subscriptionmanagement/viewAll.exe* konumundaki Razor Pages dosyasında yol `/SubscriptionManagement/ViewAll` `/subscription-management/view-all` olarak yeniden yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="eb463-219">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="eb463-220">`PageRouteTransformerConvention`, yalnızca Razor Pages klasöründen ve dosya adından gelen bir sayfa yolunun otomatik olarak oluşturulan segmentlerini dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="eb463-220">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="eb463-221">@No__t_0 yönergesiyle eklenen yol kesimlerini dönüştürmez.</span><span class="sxs-lookup"><span data-stu-id="eb463-221">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="eb463-222">Kural, <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> eklenen yolları da dönüştürmez.</span><span class="sxs-lookup"><span data-stu-id="eb463-222">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="eb463-223">@No__t_0, `Startup.ConfigureServices` bir seçenek olarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="eb463-223">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
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

::: moniker range="= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
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

## <a name="configure-a-page-route"></a><span data-ttu-id="eb463-224">Sayfa yolu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="eb463-224">Configure a page route</span></span>

<span data-ttu-id="eb463-225">Belirtilen sayfa yolundaki bir sayfaya bir yol yapılandırmak için <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="eb463-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="eb463-226">Sayfa için oluşturulan bağlantılar belirtilen rotayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="eb463-226">Generated links to the page use your specified route.</span></span> <span data-ttu-id="eb463-227">`AddPageRoute`, yolu oluşturmak için `AddPageRouteModelConvention` kullanır.</span><span class="sxs-lookup"><span data-stu-id="eb463-227">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="eb463-228">Örnek uygulama, *Contact. cshtml*için `/TheContactPage` bir yol oluşturur:</span><span class="sxs-lookup"><span data-stu-id="eb463-228">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

<span data-ttu-id="eb463-229">Iletişim sayfasına, varsayılan yolu aracılığıyla `/Contact` de erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="eb463-229">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="eb463-230">Örnek uygulamanın kişi sayfasına özel yolu, isteğe bağlı `text` yol segmentine (`{text?}`) izin verir.</span><span class="sxs-lookup"><span data-stu-id="eb463-230">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="eb463-231">Bu sayfa, ziyaretçinin `/Contact` rotasında sayfaya erişmesi durumunda `@page` yönergesinde bu isteğe bağlı segmenti de içerir:</span><span class="sxs-lookup"><span data-stu-id="eb463-231">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

<span data-ttu-id="eb463-232">İşlenmiş sayfadaki **kişi** bağlantısı IÇIN oluşturulan URL 'nin güncelleştirilmiş yolu yansıttığını unutmayın:</span><span class="sxs-lookup"><span data-stu-id="eb463-232">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Gezinti çubuğundaki örnek uygulama Iletişim bağlantısı](razor-pages-conventions/_static/contact-link.png)

![İşlenmiş HTML 'deki kişi bağlantısının inceleniyor href 'in '/theContact tpage ' olarak ayarlandığını belirtir](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="eb463-235">@No__t_0, kendi sıradan yönlendirmekte olan kişi sayfasını ziyaret edin, veya özel yol `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="eb463-235">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="eb463-236">Ek bir `text` yol kesimi sağlarsanız, sayfada sağladığınız HTML kodlu segment görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="eb463-236">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Microsoft Edge tarayıcı, URL 'de ' TextValue ' öğesinin isteğe bağlı ' text ' yol segmentini sağlamaya yönelik örnek.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="eb463-239">Sayfa modeli eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="eb463-239">Page model action conventions</span></span>

<span data-ttu-id="eb463-240">@No__t_0 uygulayan varsayılan sayfa modeli sağlayıcısı, sayfa modellerini yapılandırmak için genişletilebilirlik noktaları sağlamak üzere tasarlanan kuralları çağırır.</span><span class="sxs-lookup"><span data-stu-id="eb463-240">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="eb463-241">Bu kurallar sayfa bulma ve işleme senaryolarını oluştururken ve değiştirirken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="eb463-241">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="eb463-242">Bu bölümdeki örneklerde örnek uygulama, yanıt üst bilgisini uygulayan bir <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute> `AddHeaderAttribute` sınıfı kullanır:</span><span class="sxs-lookup"><span data-stu-id="eb463-242">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="eb463-243">Kurallar kullanılarak, örnek bir klasördeki tüm sayfalara ve tek bir sayfaya özniteliğin nasıl uygulanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="eb463-243">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="eb463-244">**Klasör uygulama modeli kuralı**</span><span class="sxs-lookup"><span data-stu-id="eb463-244">**Folder app model convention**</span></span>

<span data-ttu-id="eb463-245">Belirtilen klasör altındaki tüm sayfalar için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> örneklerine bir eylem çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="eb463-245">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="eb463-246">Örnek, uygulamanın *diğer sayfalar* klasörünün içindeki sayfalara bir başlık, `OtherPagesHeader` ekleyerek `AddFolderApplicationModelConvention` kullanımını gösterir:</span><span class="sxs-lookup"><span data-stu-id="eb463-246">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

<span data-ttu-id="eb463-247">Örnekteki Sayfa1 sayfasını `localhost:5000/OtherPages/Page1` isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="eb463-247">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Diğer sayfalar/Sayfa1 sayfasının yanıt üstbilgileri, OtherPagesHeader öğesinin eklendiğini gösterir.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="eb463-249">**Sayfa uygulama modeli kuralı**</span><span class="sxs-lookup"><span data-stu-id="eb463-249">**Page app model convention**</span></span>

<span data-ttu-id="eb463-250">Belirtilen ada sahip sayfanın <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> bir eylemi çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="eb463-250">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="eb463-251">Örnek, hakkında sayfasına `AboutHeader` bir başlık ekleyerek `AddPageApplicationModelConvention` kullanımını gösterir:</span><span class="sxs-lookup"><span data-stu-id="eb463-251">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

<span data-ttu-id="eb463-252">@No__t_0 sırasında örneğin hakkında daha fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="eb463-252">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Hakkında sayfasının yanıt üstbilgileri AboutHeader eklendiğini gösterir.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="eb463-254">**Filtre yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="eb463-254">**Configure a filter**</span></span>

<span data-ttu-id="eb463-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>, belirtilen filtreyi uygulamak üzere yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="eb463-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="eb463-256">Bir filtre sınıfı uygulayabilirsiniz, ancak örnek uygulama bir lambda ifadesinde bir filtrenin nasıl uygulanacağını gösterir, bu da bir filtre döndüren bir fabrika olarak arka planda uygulandı:</span><span class="sxs-lookup"><span data-stu-id="eb463-256">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

<span data-ttu-id="eb463-257">Sayfa uygulama modeli, *diğer sayfalar* klasöründeki Page2 sayfasına yol açan parçaların göreli yolunu denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="eb463-257">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="eb463-258">Koşul geçerse, bir üst bilgi eklenir.</span><span class="sxs-lookup"><span data-stu-id="eb463-258">If the condition passes, a header is added.</span></span> <span data-ttu-id="eb463-259">Aksi takdirde, `EmptyFilter` uygulanır.</span><span class="sxs-lookup"><span data-stu-id="eb463-259">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="eb463-260">`EmptyFilter` bir [eylem filtresidir](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="eb463-260">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="eb463-261">Eylem filtreleri Razor Pages tarafından yoksayıldığından, yolun `OtherPages/Page2` içermiyorsa `EmptyFilter` hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="eb463-261">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="eb463-262">@No__t_0 'de örneğin Page2 sayfasını isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="eb463-262">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header, Page2 için yanıta eklenir.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="eb463-264">**Filtre fabrikası yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="eb463-264">**Configure a filter factory**</span></span>

<span data-ttu-id="eb463-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>, belirtilen fabrikayı tüm Razor Pages [filtre](xref:mvc/controllers/filters) uygulayacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="eb463-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="eb463-266">Örnek uygulama, uygulamanın sayfalarına iki değer içeren `FilterFactoryHeader` bir üst bilgi ekleyerek bir [filtre fabrikası](xref:mvc/controllers/filters#ifilterfactory) kullanılmasına bir örnek sağlar:</span><span class="sxs-lookup"><span data-stu-id="eb463-266">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

<span data-ttu-id="eb463-267">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="eb463-267">*AddHeaderWithFactory.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="eb463-268">@No__t_0 sırasında örneğin hakkında daha fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="eb463-268">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Hakkında sayfasının yanıt üstbilgileri iki FilterFactoryHeader üst bilgisinin eklendiğini gösterir.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="eb463-270">MVC filtreleri ve sayfa filtresi (ıpagefilter)</span><span class="sxs-lookup"><span data-stu-id="eb463-270">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="eb463-271">Razor Pages işleyici yöntemleri kullandığından, MVC [eylem filtreleri](xref:mvc/controllers/filters#action-filters) Razor Pages tarafından yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="eb463-271">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="eb463-272">Diğer MVC filtresi türleri şunlardır: [Yetkilendirme](xref:mvc/controllers/filters#authorization-filters), [özel durum](xref:mvc/controllers/filters#exception-filters), [kaynak](xref:mvc/controllers/filters#resource-filters)ve [sonuç](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="eb463-272">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="eb463-273">Daha fazla bilgi için [Filtreler](xref:mvc/controllers/filters) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="eb463-273">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="eb463-274">Sayfa filtresi (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) Razor Pages için geçerli bir filtredir.</span><span class="sxs-lookup"><span data-stu-id="eb463-274">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="eb463-275">Daha fazla bilgi için bkz. [Razor Pages Için filtre yöntemleri](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="eb463-275">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="eb463-276">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="eb463-276">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
