---
title: ASP.NET Core Razor Pages yol ve uygulama kuralları
author: rick-anderson
description: Yönlendirme ve uygulama modeli sağlayıcısı kurallarının sayfa yönlendirmeyi, bulmayı ve işlemeyi denetlemenize nasıl yardımcı olduğunu öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: f45e327051aba54d1cab67148eb540fb1a5cc149
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667862"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="d2c66-103">ASP.NET Core Razor Pages yol ve uygulama kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d2c66-104">Razor Pages uygulamalarında sayfa yönlendirmeyi, bulmayı ve işlemeyi denetlemek için sayfa [yolu ve uygulama modeli sağlayıcısı kurallarını](xref:mvc/controllers/application-model#conventions) nasıl kullanacağınızı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-104">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="d2c66-105">Ayrı sayfalar için özel sayfa yolları yapılandırmanız gerektiğinde, bu konunun ilerleyen kısımlarında açıklanan [Addpageroute kuralına](#configure-a-page-route) sahip sayfalara yönlendirmeyi yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-105">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="d2c66-106">Bir sayfa yolu belirtmek, yol kesimleri eklemek veya bir rotaya parametreler eklemek için, sayfanın `@page` yönergesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-106">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="d2c66-107">Daha fazla bilgi için bkz. [özel rotalar](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="d2c66-107">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="d2c66-108">Yol kesimleri veya parametre adları olarak kullanılamayan ayrılmış sözcükler vardır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-108">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="d2c66-109">Daha fazla bilgi için bkz. [Yönlendirme: ayrılmış yönlendirme adları](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="d2c66-109">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="d2c66-110">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d2c66-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="d2c66-111">Senaryo</span><span class="sxs-lookup"><span data-stu-id="d2c66-111">Scenario</span></span> | <span data-ttu-id="d2c66-112">Örnek gösterilmektedir...</span><span class="sxs-lookup"><span data-stu-id="d2c66-112">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="d2c66-113">Model kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-113">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="d2c66-114">Kurallar. Add</span><span class="sxs-lookup"><span data-stu-id="d2c66-114">Conventions.Add</span></span><ul><li><span data-ttu-id="d2c66-115">Ipageroutemodelconvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-115">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="d2c66-116">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-116">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="d2c66-117">Ipagehandlermodelconvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-117">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="d2c66-118">Uygulamanın sayfalarına bir yol şablonu ve üst bilgi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-118">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="d2c66-119">Sayfa yolu eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-119">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="d2c66-120">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-120">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="d2c66-121">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-121">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="d2c66-122">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="d2c66-122">AddPageRoute</span></span></li></ul> | <span data-ttu-id="d2c66-123">Bir klasördeki sayfalara ve tek bir sayfaya rota şablonu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-123">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="d2c66-124">Sayfa modeli eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-124">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="d2c66-125">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-125">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="d2c66-126">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-126">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="d2c66-127">ConfigureFilter (filtre sınıfı, lambda ifadesi veya filtre fabrikası)</span><span class="sxs-lookup"><span data-stu-id="d2c66-127">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="d2c66-128">Bir klasördeki sayfalara üst bilgi ekleyin, tek bir sayfaya üst bilgi ekleyin ve bir [filtre fabrikası](xref:mvc/controllers/filters#ifilterfactory) yapılandırarak uygulamanın sayfalarına üst bilgi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-128">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="d2c66-129">Razor Pages kuralları, `Startup` sınıfında hizmet koleksiyonuna <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> uzantısı yöntemi kullanılarak eklenir ve yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-129">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="d2c66-130">Aşağıdaki kural örnekleri bu konunun ilerleyen kısımlarında açıklanmıştır:</span><span class="sxs-lookup"><span data-stu-id="d2c66-130">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="d2c66-131">Rota sırası</span><span class="sxs-lookup"><span data-stu-id="d2c66-131">Route order</span></span>

<span data-ttu-id="d2c66-132">Rotalar işleme için bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> belirtir (rota eşleştirme).</span><span class="sxs-lookup"><span data-stu-id="d2c66-132">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="d2c66-133">Sipariş verme</span><span class="sxs-lookup"><span data-stu-id="d2c66-133">Order</span></span>            | <span data-ttu-id="d2c66-134">Davranış</span><span class="sxs-lookup"><span data-stu-id="d2c66-134">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="d2c66-135">-1</span><span class="sxs-lookup"><span data-stu-id="d2c66-135">-1</span></span>               | <span data-ttu-id="d2c66-136">Yol, diğer rotalar işlenmeden önce işlenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-136">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="d2c66-137">0</span><span class="sxs-lookup"><span data-stu-id="d2c66-137">0</span></span>                | <span data-ttu-id="d2c66-138">Sıra belirtilmemiş (varsayılan değer).</span><span class="sxs-lookup"><span data-stu-id="d2c66-138">Order isn't specified (default value).</span></span> <span data-ttu-id="d2c66-139">`Order` atanmazsa (`Order = null`), yolun işlenmek üzere 0 (sıfır) olarak `Order`.</span><span class="sxs-lookup"><span data-stu-id="d2c66-139">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="d2c66-140">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="d2c66-140">1, 2, &hellip; n</span></span> | <span data-ttu-id="d2c66-141">Yol işleme sırasını belirtir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-141">Specifies the route processing order.</span></span> |

<span data-ttu-id="d2c66-142">Yol işleme, kurala göre belirlenir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-142">Route processing is established by convention:</span></span>

* <span data-ttu-id="d2c66-143">Yollar sıralı sırada işlenir (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="d2c66-143">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="d2c66-144">Yolların aynı `Order`olduğunda, en belirli yol önce daha az özel yollarla eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-144">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="d2c66-145">Aynı `Order` ve aynı parametre sayısı ile rotalar bir istek URL 'siyle eşleşiyorsa, rotalar <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>eklendiği sırada işlenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-145">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="d2c66-146">Mümkünse, belirlenen bir yol işleme sırasına bağlı olarak kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="d2c66-146">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="d2c66-147">Genellikle Yönlendirme, URL eşleştirme ile doğru yolu seçer.</span><span class="sxs-lookup"><span data-stu-id="d2c66-147">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="d2c66-148">İstekleri doğru yönlendirmek için yol `Order` özelliklerini ayarlamanız gerekiyorsa, uygulamanın yönlendirme şeması büyük olasılıkla istemciler için kafa karıştırıcı olur ve bakımını yapmak için kırıcı olur.</span><span class="sxs-lookup"><span data-stu-id="d2c66-148">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="d2c66-149">Uygulamanın yönlendirme şemasını basitleştirecek şekilde arama yapın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-149">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="d2c66-150">Örnek uygulama, tek bir uygulama kullanarak birkaç yönlendirme senaryosunu göstermek için açık bir yol işleme sırası gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-150">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="d2c66-151">Ancak, üretim uygulamalarında rota `Order` ayarlama uygulamalarından kaçınmaya çalışın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-151">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="d2c66-152">Razor Pages yönlendirme ve MVC denetleyici yönlendirme bir uygulamayı paylaşır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-152">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="d2c66-153">MVC konularındaki yol sırasıyla ilgili bilgiler, [Denetleyici eylemlerine yönlendirme sırasında mevcuttur: öznitelik yollarını sıralama](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="d2c66-153">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="d2c66-154">Model kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-154">Model conventions</span></span>

<span data-ttu-id="d2c66-155">Razor Pages için uygulanan [model kuralları](xref:mvc/controllers/application-model#conventions) eklemek üzere <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> için bir temsilci ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-155">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="d2c66-156">Tüm sayfalara bir rota modeli kuralı ekleme</span><span class="sxs-lookup"><span data-stu-id="d2c66-156">Add a route model convention to all pages</span></span>

<span data-ttu-id="d2c66-157">Sayfa yönlendirme modeli oluşturma sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnekleri koleksiyonuna bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-157">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="d2c66-158">Örnek uygulama, uygulamadaki tüm sayfalara bir `{globalTemplate?}` Route şablonu ekler:</span><span class="sxs-lookup"><span data-stu-id="d2c66-158">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="d2c66-159"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği `1`olarak ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-159">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="d2c66-160">Bu, örnek uygulamada aşağıdaki yol eşleştirme davranışını sağlar:</span><span class="sxs-lookup"><span data-stu-id="d2c66-160">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="d2c66-161">`TheContactPage/{text?}` için bir yol şablonu konuya daha sonra eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-161">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="d2c66-162">Iletişim sayfası yolu, `null` (`Order = 0`) varsayılan sırasına sahiptir, bu nedenle `{globalTemplate?}` Route şablonundan önce eşleşir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-162">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="d2c66-163">Konunun ilerleyen kısımlarında `{aboutTemplate?}` yol şablonu eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-163">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="d2c66-164">`{aboutTemplate?}` şablonuna `2``Order` verilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-164">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="d2c66-165">`/About/RouteDataValue`sayfası istendiğinde,`Order = 2`özelliğinin ayarlanması nedeniyle "RouteDataValue" `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["aboutTemplate"]` (`Order`) değil.</span><span class="sxs-lookup"><span data-stu-id="d2c66-165">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="d2c66-166">Konunun ilerleyen kısımlarında `{otherPagesTemplate?}` yol şablonu eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-166">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="d2c66-167">`{otherPagesTemplate?}` şablonuna `2``Order` verilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-167">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="d2c66-168">*Sayfalar/diğer sayfalar* klasöründeki herhangi bir sayfa bir yol parametresiyle (örneğin, `/OtherPages/Page1/RouteDataValue`) istendiğinde,`Order = 2`özelliğinin ayarlanması nedeniyle "routedatavalue", `RouteData.Values["otherPagesTemplate"]` (`Order`) değil `RouteData.Values["globalTemplate"]` (`Order = 1`) olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-168">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="d2c66-169">Mümkün olan yerlerde, `Order``Order = 0`sonuç olarak ayarlanmayın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-169">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="d2c66-170">Doğru yolu seçmek için yönlendirmeyi güvenin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-170">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="d2c66-171"><xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>ekleme gibi Razor Pages seçenekler, MVC `Startup.ConfigureServices`hizmet koleksiyonuna eklendiğinde eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-171">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d2c66-172">Örnek için bkz. [örnek uygulama](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="d2c66-172">For an example, see the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="d2c66-173">`localhost:5000/About/GlobalRouteValue` 'de örneğin hakkında sayfasını isteyin ve sonucu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-173">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![Hakkında sayfası, GlobalRouteValue 'un rota segmentiyle istenir.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="d2c66-176">Tüm sayfalara uygulama modeli kuralı ekleme</span><span class="sxs-lookup"><span data-stu-id="d2c66-176">Add an app model convention to all pages</span></span>

<span data-ttu-id="d2c66-177"><xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> kullanarak, sayfa uygulama modeli oluşturma sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnekleri koleksiyonuna bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-177">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="d2c66-178">Bu ve diğer kuralları konunun ilerleyen kısımlarında göstermek için, örnek uygulama bir `AddHeaderAttribute` sınıfı içerir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-178">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="d2c66-179">Sınıf Oluşturucusu bir `name` dize ve bir `values` dize dizisi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="d2c66-179">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="d2c66-180">Bu değerler, yanıt üst bilgisini ayarlamak için `OnResultExecuting` yönteminde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-180">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="d2c66-181">Tam sınıf, konusunun ilerleyen kısımlarında [sayfa modeli eylem kuralları](#page-model-action-conventions) bölümünde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-181">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="d2c66-182">Örnek uygulama, uygulamadaki tüm sayfalara bir başlık, `GlobalHeader`eklemek için `AddHeaderAttribute` sınıfını kullanır:</span><span class="sxs-lookup"><span data-stu-id="d2c66-182">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="d2c66-183">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d2c66-183">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="d2c66-184">`localhost:5000/About` sırasında örneğin hakkında daha fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-184">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Hakkında sayfasının yanıt üstbilgileri GlobalHeader 'ın eklendiğini gösterir.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="d2c66-186">Tüm sayfalara bir işleyici modeli kuralı ekleme</span><span class="sxs-lookup"><span data-stu-id="d2c66-186">Add a handler model convention to all pages</span></span>

<span data-ttu-id="d2c66-187">Sayfa işleyicisi modelinin oluşturulması sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnekleri koleksiyonuna bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-187">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="d2c66-188">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d2c66-188">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="d2c66-189">Sayfa yolu eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-189">Page route action conventions</span></span>

<span data-ttu-id="d2c66-190"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> türetilen varsayılan yol modeli sağlayıcısı, sayfa yollarını yapılandırmak için genişletilebilirlik noktaları sağlamak üzere tasarlanan kuralları çağırır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-190">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="d2c66-191">Klasör Yönlendirme modeli kuralı</span><span class="sxs-lookup"><span data-stu-id="d2c66-191">Folder route model convention</span></span>

<span data-ttu-id="d2c66-192">Belirtilen klasör altındaki tüm sayfalar için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> bir eylemi çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-192">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="d2c66-193">Örnek uygulama, *diğer sayfalar* klasöründeki sayfalara bir `{otherPagesTemplate?}` yol şablonu eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> kullanır:</span><span class="sxs-lookup"><span data-stu-id="d2c66-193">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="d2c66-194"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği `2`olarak ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-194">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="d2c66-195">Bu, tek bir rota değeri sağlandığında `{globalTemplate?}` (konuda daha önce `1`olarak ayarlanan) şablonunun ilk yol veri değeri konumu için öncelik verilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="d2c66-195">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="d2c66-196">*Sayfalar/otherpages* klasöründeki bir sayfa bir yol parametresi değeri (örneğin, `/OtherPages/Page1/RouteDataValue`) ile isteniyorsa,`Order = 2`özelliğinin ayarlanması nedeniyle "routedatavalue", `RouteData.Values["otherPagesTemplate"]` (`Order`) değil `RouteData.Values["globalTemplate"]` (`Order = 1`) olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-196">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="d2c66-197">Mümkün olan yerlerde, `Order``Order = 0`sonuç olarak ayarlanmayın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-197">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="d2c66-198">Doğru yolu seçmek için yönlendirmeyi güvenin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-198">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="d2c66-199">Örnekteki Sayfa1 sayfasını `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` isteyin ve sonucu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-199">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Diğer sayfalar klasöründeki Sayfa1, GlobalRouteValue ve OtherPagesRouteValue yol kesimiyle istenir.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="d2c66-202">Sayfa yönlendirme modeli kuralı</span><span class="sxs-lookup"><span data-stu-id="d2c66-202">Page route model convention</span></span>

<span data-ttu-id="d2c66-203">Belirtilen ada sahip sayfanın <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> bir eylemi çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-203">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="d2c66-204">Örnek uygulama, hakkında sayfasına bir `{aboutTemplate?}` yol şablonu eklemek için `AddPageRouteModelConvention` kullanır:</span><span class="sxs-lookup"><span data-stu-id="d2c66-204">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="d2c66-205"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği `2`olarak ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-205">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="d2c66-206">Bu, tek bir rota değeri sağlandığında `{globalTemplate?}` (konuda daha önce `1`olarak ayarlanan) şablonunun ilk yol veri değeri konumu için öncelik verilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="d2c66-206">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="d2c66-207">`/About/RouteDataValue`sayfasında yol parametresi değeri varsa,`Order = 2`özelliğinin ayarlanması nedeniyle "RouteDataValue" `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["aboutTemplate"]` (`Order`) değil.</span><span class="sxs-lookup"><span data-stu-id="d2c66-207">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="d2c66-208">Mümkün olan yerlerde, `Order``Order = 0`sonuç olarak ayarlanmayın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-208">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="d2c66-209">Doğru yolu seçmek için yönlendirmeyi güvenin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-209">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="d2c66-210">`localhost:5000/About/GlobalRouteValue/AboutRouteValue` 'de örneğin hakkında sayfasını isteyin ve sonucu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-210">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![GlobalRouteValue ve AboutRouteValue için rota kesimlerle ilgili sayfa hakkında bilgi istenir.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="d2c66-213">Sayfa yollarını özelleştirmek için bir parametre transformatörü kullanın</span><span class="sxs-lookup"><span data-stu-id="d2c66-213">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="d2c66-214">ASP.NET Core tarafından oluşturulan sayfa yolları, bir parametre transformatörü kullanılarak özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-214">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="d2c66-215">Bir parametre transformatörü `IOutboundParameterTransformer` uygular ve parametrelerin değerini dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="d2c66-215">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="d2c66-216">Örneğin, özel bir `SlugifyParameterTransformer` parametresi transformatörü `SubscriptionManagement` Route değerini `subscription-management`olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-216">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="d2c66-217">`PageRouteTransformerConvention` Page Route model kuralı, bir uygulamadaki otomatik olarak oluşturulan sayfa yollarının klasör ve dosya adı kesimlerine bir parametre transformatörü uygular.</span><span class="sxs-lookup"><span data-stu-id="d2c66-217">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="d2c66-218">Örneğin, */Pages/subscriptionmanagement/viewAll.exe* konumundaki Razor Pages dosyasında yol `/SubscriptionManagement/ViewAll` `/subscription-management/view-all`olarak yeniden yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-218">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="d2c66-219">`PageRouteTransformerConvention`, yalnızca Razor Pages klasöründen ve dosya adından gelen bir sayfa yolunun otomatik olarak oluşturulan segmentlerini dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="d2c66-219">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="d2c66-220">`@page` yönergesiyle eklenen yol kesimlerini dönüştürmez.</span><span class="sxs-lookup"><span data-stu-id="d2c66-220">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="d2c66-221">Kural, <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>eklenen yolları da dönüştürmez.</span><span class="sxs-lookup"><span data-stu-id="d2c66-221">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="d2c66-222">`PageRouteTransformerConvention`, `Startup.ConfigureServices`bir seçenek olarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-222">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

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

## <a name="configure-a-page-route"></a><span data-ttu-id="d2c66-223">Sayfa yolu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d2c66-223">Configure a page route</span></span>

<span data-ttu-id="d2c66-224">Belirtilen sayfa yolundaki bir sayfaya bir yol yapılandırmak için <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-224">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="d2c66-225">Sayfa için oluşturulan bağlantılar belirtilen rotayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-225">Generated links to the page use your specified route.</span></span> <span data-ttu-id="d2c66-226">`AddPageRoute`, yolu oluşturmak için `AddPageRouteModelConvention` kullanır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-226">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="d2c66-227">Örnek uygulama, *Contact. cshtml*için `/TheContactPage` bir yol oluşturur:</span><span class="sxs-lookup"><span data-stu-id="d2c66-227">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="d2c66-228">Iletişim sayfasına, varsayılan yolu aracılığıyla `/Contact` de erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-228">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="d2c66-229">Örnek uygulamanın kişi sayfasına özel yolu, isteğe bağlı `text` yol segmentine (`{text?}`) izin verir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-229">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="d2c66-230">Bu sayfa, ziyaretçinin `/Contact` rotasında sayfaya erişmesi durumunda `@page` yönergesinde bu isteğe bağlı segmenti de içerir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-230">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="d2c66-231">İşlenmiş sayfadaki **kişi** bağlantısı IÇIN oluşturulan URL 'nin güncelleştirilmiş yolu yansıttığını unutmayın:</span><span class="sxs-lookup"><span data-stu-id="d2c66-231">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Gezinti çubuğundaki örnek uygulama Iletişim bağlantısı](razor-pages-conventions/_static/contact-link.png)

![İşlenmiş HTML 'deki kişi bağlantısının inceleniyor href 'in '/theContact tpage ' olarak ayarlandığını belirtir](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="d2c66-234">`/Contact`, kendi sıradan yönlendirmekte olan kişi sayfasını ziyaret edin, veya özel yol `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="d2c66-234">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="d2c66-235">Ek bir `text` yol kesimi sağlarsanız, sayfada sağladığınız HTML kodlu segment görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-235">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Microsoft Edge tarayıcı, URL 'de ' TextValue ' öğesinin isteğe bağlı ' text ' yol segmentini sağlamaya yönelik örnek.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="d2c66-238">Sayfa modeli eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-238">Page model action conventions</span></span>

<span data-ttu-id="d2c66-239"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> uygulayan varsayılan sayfa modeli sağlayıcısı, sayfa modellerini yapılandırmak için genişletilebilirlik noktaları sağlamak üzere tasarlanan kuralları çağırır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-239">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="d2c66-240">Bu kurallar sayfa bulma ve işleme senaryolarını oluştururken ve değiştirirken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-240">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="d2c66-241">Bu bölümdeki örneklerde örnek uygulama, yanıt üst bilgisini uygulayan bir <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>`AddHeaderAttribute` sınıfı kullanır:</span><span class="sxs-lookup"><span data-stu-id="d2c66-241">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="d2c66-242">Kurallar kullanılarak, örnek bir klasördeki tüm sayfalara ve tek bir sayfaya özniteliğin nasıl uygulanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-242">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="d2c66-243">**Klasör uygulama modeli kuralı**</span><span class="sxs-lookup"><span data-stu-id="d2c66-243">**Folder app model convention**</span></span>

<span data-ttu-id="d2c66-244">Belirtilen klasör altındaki tüm sayfalar için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> örneklerine bir eylem çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-244">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="d2c66-245">Örnek, uygulamanın *diğer sayfalar* klasörünün içindeki sayfalara bir başlık, `OtherPagesHeader`ekleyerek `AddFolderApplicationModelConvention` kullanımını gösterir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-245">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="d2c66-246">Örnekteki Sayfa1 sayfasını `localhost:5000/OtherPages/Page1` isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-246">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Diğer sayfalar/Sayfa1 sayfasının yanıt üstbilgileri, OtherPagesHeader öğesinin eklendiğini gösterir.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="d2c66-248">**Sayfa uygulama modeli kuralı**</span><span class="sxs-lookup"><span data-stu-id="d2c66-248">**Page app model convention**</span></span>

<span data-ttu-id="d2c66-249">Belirtilen ada sahip sayfanın <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> bir eylemi çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-249">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="d2c66-250">Örnek, hakkında sayfasına `AboutHeader`bir başlık ekleyerek `AddPageApplicationModelConvention` kullanımını gösterir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-250">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="d2c66-251">`localhost:5000/About` sırasında örneğin hakkında daha fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-251">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Hakkında sayfasının yanıt üstbilgileri AboutHeader eklendiğini gösterir.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="d2c66-253">**Filtre yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="d2c66-253">**Configure a filter**</span></span>

<span data-ttu-id="d2c66-254"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>, belirtilen filtreyi uygulamak üzere yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-254"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="d2c66-255">Bir filtre sınıfı uygulayabilirsiniz, ancak örnek uygulama bir lambda ifadesinde bir filtrenin nasıl uygulanacağını gösterir, bu da bir filtre döndüren bir fabrika olarak arka planda uygulandı:</span><span class="sxs-lookup"><span data-stu-id="d2c66-255">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="d2c66-256">Sayfa uygulama modeli, *diğer sayfalar* klasöründeki Page2 sayfasına yol açan parçaların göreli yolunu denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-256">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="d2c66-257">Koşul geçerse, bir üst bilgi eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-257">If the condition passes, a header is added.</span></span> <span data-ttu-id="d2c66-258">Aksi takdirde, `EmptyFilter` uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-258">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="d2c66-259">`EmptyFilter` bir [eylem filtresidir](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="d2c66-259">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="d2c66-260">Eylem filtreleri Razor Pages tarafından yoksayıldığından, yolun `OtherPages/Page2`içermiyorsa `EmptyFilter` hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="d2c66-260">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="d2c66-261">`localhost:5000/OtherPages/Page2` 'de örneğin Page2 sayfasını isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-261">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header, Page2 için yanıta eklenir.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="d2c66-263">**Filtre fabrikası yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="d2c66-263">**Configure a filter factory**</span></span>

<span data-ttu-id="d2c66-264"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>, belirtilen fabrikayı tüm Razor Pages [filtre](xref:mvc/controllers/filters) uygulayacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-264"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="d2c66-265">Örnek uygulama, uygulamanın sayfalarına iki değer içeren `FilterFactoryHeader`bir üst bilgi ekleyerek bir [filtre fabrikası](xref:mvc/controllers/filters#ifilterfactory) kullanılmasına bir örnek sağlar:</span><span class="sxs-lookup"><span data-stu-id="d2c66-265">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="d2c66-266">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="d2c66-266">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="d2c66-267">`localhost:5000/About` sırasında örneğin hakkında daha fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-267">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Hakkında sayfasının yanıt üstbilgileri iki FilterFactoryHeader üst bilgisinin eklendiğini gösterir.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="d2c66-269">MVC filtreleri ve sayfa filtresi (ıpagefilter)</span><span class="sxs-lookup"><span data-stu-id="d2c66-269">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="d2c66-270">Razor Pages işleyici yöntemleri kullandığından, MVC [eylem filtreleri](xref:mvc/controllers/filters#action-filters) Razor Pages tarafından yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-270">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="d2c66-271">Diğer MVC filtresi türleri şunlardır: [Yetkilendirme](xref:mvc/controllers/filters#authorization-filters), [özel durum](xref:mvc/controllers/filters#exception-filters), [kaynak](xref:mvc/controllers/filters#resource-filters)ve [sonuç](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="d2c66-271">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="d2c66-272">Daha fazla bilgi için [Filtreler](xref:mvc/controllers/filters) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-272">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="d2c66-273">Sayfa filtresi (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) Razor Pages için geçerli bir filtredir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-273">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="d2c66-274">Daha fazla bilgi için bkz. [Razor Pages Için filtre yöntemleri](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="d2c66-274">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d2c66-275">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d2c66-275">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="d2c66-276">Razor Pages uygulamalarında sayfa yönlendirmeyi, bulmayı ve işlemeyi denetlemek için sayfa [yolu ve uygulama modeli sağlayıcısı kurallarını](xref:mvc/controllers/application-model#conventions) nasıl kullanacağınızı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-276">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="d2c66-277">Ayrı sayfalar için özel sayfa yolları yapılandırmanız gerektiğinde, bu konunun ilerleyen kısımlarında açıklanan [Addpageroute kuralına](#configure-a-page-route) sahip sayfalara yönlendirmeyi yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-277">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="d2c66-278">Bir sayfa yolu belirtmek, yol kesimleri eklemek veya bir rotaya parametreler eklemek için, sayfanın `@page` yönergesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-278">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="d2c66-279">Daha fazla bilgi için bkz. [özel rotalar](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="d2c66-279">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="d2c66-280">Yol kesimleri veya parametre adları olarak kullanılamayan ayrılmış sözcükler vardır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-280">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="d2c66-281">Daha fazla bilgi için bkz. [Yönlendirme: ayrılmış yönlendirme adları](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="d2c66-281">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="d2c66-282">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d2c66-282">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="d2c66-283">Senaryo</span><span class="sxs-lookup"><span data-stu-id="d2c66-283">Scenario</span></span> | <span data-ttu-id="d2c66-284">Örnek gösterilmektedir...</span><span class="sxs-lookup"><span data-stu-id="d2c66-284">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="d2c66-285">Model kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-285">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="d2c66-286">Kurallar. Add</span><span class="sxs-lookup"><span data-stu-id="d2c66-286">Conventions.Add</span></span><ul><li><span data-ttu-id="d2c66-287">Ipageroutemodelconvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-287">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="d2c66-288">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-288">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="d2c66-289">Ipagehandlermodelconvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-289">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="d2c66-290">Uygulamanın sayfalarına bir yol şablonu ve üst bilgi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-290">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="d2c66-291">Sayfa yolu eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-291">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="d2c66-292">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-292">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="d2c66-293">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-293">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="d2c66-294">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="d2c66-294">AddPageRoute</span></span></li></ul> | <span data-ttu-id="d2c66-295">Bir klasördeki sayfalara ve tek bir sayfaya rota şablonu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-295">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="d2c66-296">Sayfa modeli eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-296">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="d2c66-297">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-297">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="d2c66-298">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-298">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="d2c66-299">ConfigureFilter (filtre sınıfı, lambda ifadesi veya filtre fabrikası)</span><span class="sxs-lookup"><span data-stu-id="d2c66-299">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="d2c66-300">Bir klasördeki sayfalara üst bilgi ekleyin, tek bir sayfaya üst bilgi ekleyin ve bir [filtre fabrikası](xref:mvc/controllers/filters#ifilterfactory) yapılandırarak uygulamanın sayfalarına üst bilgi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-300">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="d2c66-301">Razor Pages kuralları, `Startup` sınıfında hizmet koleksiyonuna <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> uzantısı yöntemi kullanılarak eklenir ve yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-301">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="d2c66-302">Aşağıdaki kural örnekleri bu konunun ilerleyen kısımlarında açıklanmıştır:</span><span class="sxs-lookup"><span data-stu-id="d2c66-302">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="d2c66-303">Rota sırası</span><span class="sxs-lookup"><span data-stu-id="d2c66-303">Route order</span></span>

<span data-ttu-id="d2c66-304">Rotalar işleme için bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> belirtir (rota eşleştirme).</span><span class="sxs-lookup"><span data-stu-id="d2c66-304">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="d2c66-305">Sipariş verme</span><span class="sxs-lookup"><span data-stu-id="d2c66-305">Order</span></span>            | <span data-ttu-id="d2c66-306">Davranış</span><span class="sxs-lookup"><span data-stu-id="d2c66-306">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="d2c66-307">-1</span><span class="sxs-lookup"><span data-stu-id="d2c66-307">-1</span></span>               | <span data-ttu-id="d2c66-308">Yol, diğer rotalar işlenmeden önce işlenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-308">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="d2c66-309">0</span><span class="sxs-lookup"><span data-stu-id="d2c66-309">0</span></span>                | <span data-ttu-id="d2c66-310">Sıra belirtilmemiş (varsayılan değer).</span><span class="sxs-lookup"><span data-stu-id="d2c66-310">Order isn't specified (default value).</span></span> <span data-ttu-id="d2c66-311">`Order` atanmazsa (`Order = null`), yolun işlenmek üzere 0 (sıfır) olarak `Order`.</span><span class="sxs-lookup"><span data-stu-id="d2c66-311">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="d2c66-312">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="d2c66-312">1, 2, &hellip; n</span></span> | <span data-ttu-id="d2c66-313">Yol işleme sırasını belirtir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-313">Specifies the route processing order.</span></span> |

<span data-ttu-id="d2c66-314">Yol işleme, kurala göre belirlenir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-314">Route processing is established by convention:</span></span>

* <span data-ttu-id="d2c66-315">Yollar sıralı sırada işlenir (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="d2c66-315">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="d2c66-316">Yolların aynı `Order`olduğunda, en belirli yol önce daha az özel yollarla eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-316">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="d2c66-317">Aynı `Order` ve aynı parametre sayısı ile rotalar bir istek URL 'siyle eşleşiyorsa, rotalar <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>eklendiği sırada işlenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-317">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="d2c66-318">Mümkünse, belirlenen bir yol işleme sırasına bağlı olarak kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="d2c66-318">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="d2c66-319">Genellikle Yönlendirme, URL eşleştirme ile doğru yolu seçer.</span><span class="sxs-lookup"><span data-stu-id="d2c66-319">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="d2c66-320">İstekleri doğru yönlendirmek için yol `Order` özelliklerini ayarlamanız gerekiyorsa, uygulamanın yönlendirme şeması büyük olasılıkla istemciler için kafa karıştırıcı olur ve bakımını yapmak için kırıcı olur.</span><span class="sxs-lookup"><span data-stu-id="d2c66-320">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="d2c66-321">Uygulamanın yönlendirme şemasını basitleştirecek şekilde arama yapın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-321">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="d2c66-322">Örnek uygulama, tek bir uygulama kullanarak birkaç yönlendirme senaryosunu göstermek için açık bir yol işleme sırası gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-322">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="d2c66-323">Ancak, üretim uygulamalarında rota `Order` ayarlama uygulamalarından kaçınmaya çalışın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-323">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="d2c66-324">Razor Pages yönlendirme ve MVC denetleyici yönlendirme bir uygulamayı paylaşır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-324">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="d2c66-325">MVC konularındaki yol sırasıyla ilgili bilgiler, [Denetleyici eylemlerine yönlendirme sırasında mevcuttur: öznitelik yollarını sıralama](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="d2c66-325">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="d2c66-326">Model kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-326">Model conventions</span></span>

<span data-ttu-id="d2c66-327">Razor Pages için uygulanan [model kuralları](xref:mvc/controllers/application-model#conventions) eklemek üzere <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> için bir temsilci ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-327">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="d2c66-328">Tüm sayfalara bir rota modeli kuralı ekleme</span><span class="sxs-lookup"><span data-stu-id="d2c66-328">Add a route model convention to all pages</span></span>

<span data-ttu-id="d2c66-329">Sayfa yönlendirme modeli oluşturma sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnekleri koleksiyonuna bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-329">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="d2c66-330">Örnek uygulama, uygulamadaki tüm sayfalara bir `{globalTemplate?}` Route şablonu ekler:</span><span class="sxs-lookup"><span data-stu-id="d2c66-330">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="d2c66-331"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği `1`olarak ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-331">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="d2c66-332">Bu, örnek uygulamada aşağıdaki yol eşleştirme davranışını sağlar:</span><span class="sxs-lookup"><span data-stu-id="d2c66-332">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="d2c66-333">`TheContactPage/{text?}` için bir yol şablonu konuya daha sonra eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-333">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="d2c66-334">Iletişim sayfası yolu, `null` (`Order = 0`) varsayılan sırasına sahiptir, bu nedenle `{globalTemplate?}` Route şablonundan önce eşleşir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-334">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="d2c66-335">Konunun ilerleyen kısımlarında `{aboutTemplate?}` yol şablonu eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-335">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="d2c66-336">`{aboutTemplate?}` şablonuna `2``Order` verilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-336">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="d2c66-337">`/About/RouteDataValue`sayfası istendiğinde,`Order = 2`özelliğinin ayarlanması nedeniyle "RouteDataValue" `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["aboutTemplate"]` (`Order`) değil.</span><span class="sxs-lookup"><span data-stu-id="d2c66-337">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="d2c66-338">Konunun ilerleyen kısımlarında `{otherPagesTemplate?}` yol şablonu eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-338">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="d2c66-339">`{otherPagesTemplate?}` şablonuna `2``Order` verilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-339">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="d2c66-340">*Sayfalar/diğer sayfalar* klasöründeki herhangi bir sayfa bir yol parametresiyle (örneğin, `/OtherPages/Page1/RouteDataValue`) istendiğinde,`Order = 2`özelliğinin ayarlanması nedeniyle "routedatavalue", `RouteData.Values["otherPagesTemplate"]` (`Order`) değil `RouteData.Values["globalTemplate"]` (`Order = 1`) olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-340">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="d2c66-341">Mümkün olan yerlerde, `Order``Order = 0`sonuç olarak ayarlanmayın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-341">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="d2c66-342">Doğru yolu seçmek için yönlendirmeyi güvenin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-342">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="d2c66-343"><xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>ekleme gibi Razor Pages seçenekler, MVC `Startup.ConfigureServices`hizmet koleksiyonuna eklendiğinde eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-343">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d2c66-344">Örnek için bkz. [örnek uygulama](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="d2c66-344">For an example, see the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="d2c66-345">`localhost:5000/About/GlobalRouteValue` 'de örneğin hakkında sayfasını isteyin ve sonucu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-345">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![Hakkında sayfası, GlobalRouteValue 'un rota segmentiyle istenir.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="d2c66-348">Tüm sayfalara uygulama modeli kuralı ekleme</span><span class="sxs-lookup"><span data-stu-id="d2c66-348">Add an app model convention to all pages</span></span>

<span data-ttu-id="d2c66-349"><xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> kullanarak, sayfa uygulama modeli oluşturma sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnekleri koleksiyonuna bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-349">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="d2c66-350">Bu ve diğer kuralları konunun ilerleyen kısımlarında göstermek için, örnek uygulama bir `AddHeaderAttribute` sınıfı içerir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-350">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="d2c66-351">Sınıf Oluşturucusu bir `name` dize ve bir `values` dize dizisi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="d2c66-351">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="d2c66-352">Bu değerler, yanıt üst bilgisini ayarlamak için `OnResultExecuting` yönteminde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-352">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="d2c66-353">Tam sınıf, konusunun ilerleyen kısımlarında [sayfa modeli eylem kuralları](#page-model-action-conventions) bölümünde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-353">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="d2c66-354">Örnek uygulama, uygulamadaki tüm sayfalara bir başlık, `GlobalHeader`eklemek için `AddHeaderAttribute` sınıfını kullanır:</span><span class="sxs-lookup"><span data-stu-id="d2c66-354">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="d2c66-355">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d2c66-355">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="d2c66-356">`localhost:5000/About` sırasında örneğin hakkında daha fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-356">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Hakkında sayfasının yanıt üstbilgileri GlobalHeader 'ın eklendiğini gösterir.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="d2c66-358">Tüm sayfalara bir işleyici modeli kuralı ekleme</span><span class="sxs-lookup"><span data-stu-id="d2c66-358">Add a handler model convention to all pages</span></span>

<span data-ttu-id="d2c66-359">Sayfa işleyicisi modelinin oluşturulması sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnekleri koleksiyonuna bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-359">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="d2c66-360">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d2c66-360">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="d2c66-361">Sayfa yolu eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-361">Page route action conventions</span></span>

<span data-ttu-id="d2c66-362"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> türetilen varsayılan yol modeli sağlayıcısı, sayfa yollarını yapılandırmak için genişletilebilirlik noktaları sağlamak üzere tasarlanan kuralları çağırır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-362">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="d2c66-363">Klasör Yönlendirme modeli kuralı</span><span class="sxs-lookup"><span data-stu-id="d2c66-363">Folder route model convention</span></span>

<span data-ttu-id="d2c66-364">Belirtilen klasör altındaki tüm sayfalar için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> bir eylemi çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-364">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="d2c66-365">Örnek uygulama, *diğer sayfalar* klasöründeki sayfalara bir `{otherPagesTemplate?}` yol şablonu eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> kullanır:</span><span class="sxs-lookup"><span data-stu-id="d2c66-365">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="d2c66-366"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği `2`olarak ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-366">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="d2c66-367">Bu, tek bir rota değeri sağlandığında `{globalTemplate?}` (konuda daha önce `1`olarak ayarlanan) şablonunun ilk yol veri değeri konumu için öncelik verilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="d2c66-367">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="d2c66-368">*Sayfalar/otherpages* klasöründeki bir sayfa bir yol parametresi değeri (örneğin, `/OtherPages/Page1/RouteDataValue`) ile isteniyorsa,`Order = 2`özelliğinin ayarlanması nedeniyle "routedatavalue", `RouteData.Values["otherPagesTemplate"]` (`Order`) değil `RouteData.Values["globalTemplate"]` (`Order = 1`) olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-368">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="d2c66-369">Mümkün olan yerlerde, `Order``Order = 0`sonuç olarak ayarlanmayın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-369">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="d2c66-370">Doğru yolu seçmek için yönlendirmeyi güvenin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-370">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="d2c66-371">Örnekteki Sayfa1 sayfasını `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` isteyin ve sonucu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-371">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Diğer sayfalar klasöründeki Sayfa1, GlobalRouteValue ve OtherPagesRouteValue yol kesimiyle istenir.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="d2c66-374">Sayfa yönlendirme modeli kuralı</span><span class="sxs-lookup"><span data-stu-id="d2c66-374">Page route model convention</span></span>

<span data-ttu-id="d2c66-375">Belirtilen ada sahip sayfanın <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> bir eylemi çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-375">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="d2c66-376">Örnek uygulama, hakkında sayfasına bir `{aboutTemplate?}` yol şablonu eklemek için `AddPageRouteModelConvention` kullanır:</span><span class="sxs-lookup"><span data-stu-id="d2c66-376">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="d2c66-377"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği `2`olarak ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-377">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="d2c66-378">Bu, tek bir rota değeri sağlandığında `{globalTemplate?}` (konuda daha önce `1`olarak ayarlanan) şablonunun ilk yol veri değeri konumu için öncelik verilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="d2c66-378">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="d2c66-379">`/About/RouteDataValue`sayfasında yol parametresi değeri varsa,`Order = 2`özelliğinin ayarlanması nedeniyle "RouteDataValue" `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["aboutTemplate"]` (`Order`) değil.</span><span class="sxs-lookup"><span data-stu-id="d2c66-379">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="d2c66-380">Mümkün olan yerlerde, `Order``Order = 0`sonuç olarak ayarlanmayın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-380">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="d2c66-381">Doğru yolu seçmek için yönlendirmeyi güvenin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-381">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="d2c66-382">`localhost:5000/About/GlobalRouteValue/AboutRouteValue` 'de örneğin hakkında sayfasını isteyin ve sonucu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-382">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![GlobalRouteValue ve AboutRouteValue için rota kesimlerle ilgili sayfa hakkında bilgi istenir.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="d2c66-385">Sayfa yollarını özelleştirmek için bir parametre transformatörü kullanın</span><span class="sxs-lookup"><span data-stu-id="d2c66-385">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="d2c66-386">ASP.NET Core tarafından oluşturulan sayfa yolları, bir parametre transformatörü kullanılarak özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-386">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="d2c66-387">Bir parametre transformatörü `IOutboundParameterTransformer` uygular ve parametrelerin değerini dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="d2c66-387">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="d2c66-388">Örneğin, özel bir `SlugifyParameterTransformer` parametresi transformatörü `SubscriptionManagement` Route değerini `subscription-management`olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-388">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="d2c66-389">`PageRouteTransformerConvention` Page Route model kuralı, bir uygulamadaki otomatik olarak oluşturulan sayfa yollarının klasör ve dosya adı kesimlerine bir parametre transformatörü uygular.</span><span class="sxs-lookup"><span data-stu-id="d2c66-389">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="d2c66-390">Örneğin, */Pages/subscriptionmanagement/viewAll.exe* konumundaki Razor Pages dosyasında yol `/SubscriptionManagement/ViewAll` `/subscription-management/view-all`olarak yeniden yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-390">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="d2c66-391">`PageRouteTransformerConvention`, yalnızca Razor Pages klasöründen ve dosya adından gelen bir sayfa yolunun otomatik olarak oluşturulan segmentlerini dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="d2c66-391">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="d2c66-392">`@page` yönergesiyle eklenen yol kesimlerini dönüştürmez.</span><span class="sxs-lookup"><span data-stu-id="d2c66-392">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="d2c66-393">Kural, <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>eklenen yolları da dönüştürmez.</span><span class="sxs-lookup"><span data-stu-id="d2c66-393">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="d2c66-394">`PageRouteTransformerConvention`, `Startup.ConfigureServices`bir seçenek olarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-394">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

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

## <a name="configure-a-page-route"></a><span data-ttu-id="d2c66-395">Sayfa yolu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d2c66-395">Configure a page route</span></span>

<span data-ttu-id="d2c66-396">Belirtilen sayfa yolundaki bir sayfaya bir yol yapılandırmak için <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-396">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="d2c66-397">Sayfa için oluşturulan bağlantılar belirtilen rotayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-397">Generated links to the page use your specified route.</span></span> <span data-ttu-id="d2c66-398">`AddPageRoute`, yolu oluşturmak için `AddPageRouteModelConvention` kullanır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-398">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="d2c66-399">Örnek uygulama, *Contact. cshtml*için `/TheContactPage` bir yol oluşturur:</span><span class="sxs-lookup"><span data-stu-id="d2c66-399">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="d2c66-400">Iletişim sayfasına, varsayılan yolu aracılığıyla `/Contact` de erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-400">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="d2c66-401">Örnek uygulamanın kişi sayfasına özel yolu, isteğe bağlı `text` yol segmentine (`{text?}`) izin verir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-401">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="d2c66-402">Bu sayfa, ziyaretçinin `/Contact` rotasında sayfaya erişmesi durumunda `@page` yönergesinde bu isteğe bağlı segmenti de içerir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-402">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="d2c66-403">İşlenmiş sayfadaki **kişi** bağlantısı IÇIN oluşturulan URL 'nin güncelleştirilmiş yolu yansıttığını unutmayın:</span><span class="sxs-lookup"><span data-stu-id="d2c66-403">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Gezinti çubuğundaki örnek uygulama Iletişim bağlantısı](razor-pages-conventions/_static/contact-link.png)

![İşlenmiş HTML 'deki kişi bağlantısının inceleniyor href 'in '/theContact tpage ' olarak ayarlandığını belirtir](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="d2c66-406">`/Contact`, kendi sıradan yönlendirmekte olan kişi sayfasını ziyaret edin, veya özel yol `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="d2c66-406">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="d2c66-407">Ek bir `text` yol kesimi sağlarsanız, sayfada sağladığınız HTML kodlu segment görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-407">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Microsoft Edge tarayıcı, URL 'de ' TextValue ' öğesinin isteğe bağlı ' text ' yol segmentini sağlamaya yönelik örnek.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="d2c66-410">Sayfa modeli eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-410">Page model action conventions</span></span>

<span data-ttu-id="d2c66-411"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> uygulayan varsayılan sayfa modeli sağlayıcısı, sayfa modellerini yapılandırmak için genişletilebilirlik noktaları sağlamak üzere tasarlanan kuralları çağırır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-411">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="d2c66-412">Bu kurallar sayfa bulma ve işleme senaryolarını oluştururken ve değiştirirken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-412">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="d2c66-413">Bu bölümdeki örneklerde örnek uygulama, yanıt üst bilgisini uygulayan bir <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>`AddHeaderAttribute` sınıfı kullanır:</span><span class="sxs-lookup"><span data-stu-id="d2c66-413">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="d2c66-414">Kurallar kullanılarak, örnek bir klasördeki tüm sayfalara ve tek bir sayfaya özniteliğin nasıl uygulanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-414">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="d2c66-415">**Klasör uygulama modeli kuralı**</span><span class="sxs-lookup"><span data-stu-id="d2c66-415">**Folder app model convention**</span></span>

<span data-ttu-id="d2c66-416">Belirtilen klasör altındaki tüm sayfalar için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> örneklerine bir eylem çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-416">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="d2c66-417">Örnek, uygulamanın *diğer sayfalar* klasörünün içindeki sayfalara bir başlık, `OtherPagesHeader`ekleyerek `AddFolderApplicationModelConvention` kullanımını gösterir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-417">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="d2c66-418">Örnekteki Sayfa1 sayfasını `localhost:5000/OtherPages/Page1` isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-418">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Diğer sayfalar/Sayfa1 sayfasının yanıt üstbilgileri, OtherPagesHeader öğesinin eklendiğini gösterir.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="d2c66-420">**Sayfa uygulama modeli kuralı**</span><span class="sxs-lookup"><span data-stu-id="d2c66-420">**Page app model convention**</span></span>

<span data-ttu-id="d2c66-421">Belirtilen ada sahip sayfanın <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> bir eylemi çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-421">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="d2c66-422">Örnek, hakkında sayfasına `AboutHeader`bir başlık ekleyerek `AddPageApplicationModelConvention` kullanımını gösterir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-422">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="d2c66-423">`localhost:5000/About` sırasında örneğin hakkında daha fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-423">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Hakkında sayfasının yanıt üstbilgileri AboutHeader eklendiğini gösterir.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="d2c66-425">**Filtre yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="d2c66-425">**Configure a filter**</span></span>

<span data-ttu-id="d2c66-426"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>, belirtilen filtreyi uygulamak üzere yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-426"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="d2c66-427">Bir filtre sınıfı uygulayabilirsiniz, ancak örnek uygulama bir lambda ifadesinde bir filtrenin nasıl uygulanacağını gösterir, bu da bir filtre döndüren bir fabrika olarak arka planda uygulandı:</span><span class="sxs-lookup"><span data-stu-id="d2c66-427">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="d2c66-428">Sayfa uygulama modeli, *diğer sayfalar* klasöründeki Page2 sayfasına yol açan parçaların göreli yolunu denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-428">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="d2c66-429">Koşul geçerse, bir üst bilgi eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-429">If the condition passes, a header is added.</span></span> <span data-ttu-id="d2c66-430">Aksi takdirde, `EmptyFilter` uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-430">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="d2c66-431">`EmptyFilter` bir [eylem filtresidir](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="d2c66-431">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="d2c66-432">Eylem filtreleri Razor Pages tarafından yoksayıldığından, yolun `OtherPages/Page2`içermiyorsa `EmptyFilter` hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="d2c66-432">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="d2c66-433">`localhost:5000/OtherPages/Page2` 'de örneğin Page2 sayfasını isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-433">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header, Page2 için yanıta eklenir.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="d2c66-435">**Filtre fabrikası yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="d2c66-435">**Configure a filter factory**</span></span>

<span data-ttu-id="d2c66-436"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>, belirtilen fabrikayı tüm Razor Pages [filtre](xref:mvc/controllers/filters) uygulayacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-436"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="d2c66-437">Örnek uygulama, uygulamanın sayfalarına iki değer içeren `FilterFactoryHeader`bir üst bilgi ekleyerek bir [filtre fabrikası](xref:mvc/controllers/filters#ifilterfactory) kullanılmasına bir örnek sağlar:</span><span class="sxs-lookup"><span data-stu-id="d2c66-437">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="d2c66-438">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="d2c66-438">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="d2c66-439">`localhost:5000/About` sırasında örneğin hakkında daha fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-439">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Hakkında sayfasının yanıt üstbilgileri iki FilterFactoryHeader üst bilgisinin eklendiğini gösterir.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="d2c66-441">MVC filtreleri ve sayfa filtresi (ıpagefilter)</span><span class="sxs-lookup"><span data-stu-id="d2c66-441">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="d2c66-442">Razor Pages işleyici yöntemleri kullandığından, MVC [eylem filtreleri](xref:mvc/controllers/filters#action-filters) Razor Pages tarafından yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-442">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="d2c66-443">Diğer MVC filtresi türleri şunlardır: [Yetkilendirme](xref:mvc/controllers/filters#authorization-filters), [özel durum](xref:mvc/controllers/filters#exception-filters), [kaynak](xref:mvc/controllers/filters#resource-filters)ve [sonuç](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="d2c66-443">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="d2c66-444">Daha fazla bilgi için [Filtreler](xref:mvc/controllers/filters) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-444">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="d2c66-445">Sayfa filtresi (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) Razor Pages için geçerli bir filtredir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-445">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="d2c66-446">Daha fazla bilgi için bkz. [Razor Pages Için filtre yöntemleri](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="d2c66-446">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d2c66-447">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d2c66-447">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="d2c66-448">Razor Pages uygulamalarında sayfa yönlendirmeyi, bulmayı ve işlemeyi denetlemek için sayfa [yolu ve uygulama modeli sağlayıcısı kurallarını](xref:mvc/controllers/application-model#conventions) nasıl kullanacağınızı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-448">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="d2c66-449">Ayrı sayfalar için özel sayfa yolları yapılandırmanız gerektiğinde, bu konunun ilerleyen kısımlarında açıklanan [Addpageroute kuralına](#configure-a-page-route) sahip sayfalara yönlendirmeyi yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-449">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="d2c66-450">Bir sayfa yolu belirtmek, yol kesimleri eklemek veya bir rotaya parametreler eklemek için, sayfanın `@page` yönergesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-450">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="d2c66-451">Daha fazla bilgi için bkz. [özel rotalar](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="d2c66-451">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="d2c66-452">Yol kesimleri veya parametre adları olarak kullanılamayan ayrılmış sözcükler vardır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-452">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="d2c66-453">Daha fazla bilgi için bkz. [Yönlendirme: ayrılmış yönlendirme adları](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="d2c66-453">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="d2c66-454">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d2c66-454">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="d2c66-455">Senaryo</span><span class="sxs-lookup"><span data-stu-id="d2c66-455">Scenario</span></span> | <span data-ttu-id="d2c66-456">Örnek gösterilmektedir...</span><span class="sxs-lookup"><span data-stu-id="d2c66-456">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="d2c66-457">Model kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-457">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="d2c66-458">Kurallar. Add</span><span class="sxs-lookup"><span data-stu-id="d2c66-458">Conventions.Add</span></span><ul><li><span data-ttu-id="d2c66-459">Ipageroutemodelconvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-459">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="d2c66-460">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-460">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="d2c66-461">Ipagehandlermodelconvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-461">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="d2c66-462">Uygulamanın sayfalarına bir yol şablonu ve üst bilgi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-462">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="d2c66-463">Sayfa yolu eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-463">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="d2c66-464">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-464">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="d2c66-465">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-465">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="d2c66-466">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="d2c66-466">AddPageRoute</span></span></li></ul> | <span data-ttu-id="d2c66-467">Bir klasördeki sayfalara ve tek bir sayfaya rota şablonu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-467">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="d2c66-468">Sayfa modeli eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-468">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="d2c66-469">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-469">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="d2c66-470">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="d2c66-470">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="d2c66-471">ConfigureFilter (filtre sınıfı, lambda ifadesi veya filtre fabrikası)</span><span class="sxs-lookup"><span data-stu-id="d2c66-471">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="d2c66-472">Bir klasördeki sayfalara üst bilgi ekleyin, tek bir sayfaya üst bilgi ekleyin ve bir [filtre fabrikası](xref:mvc/controllers/filters#ifilterfactory) yapılandırarak uygulamanın sayfalarına üst bilgi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-472">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="d2c66-473">Razor Pages kuralları, `Startup` sınıfında hizmet koleksiyonuna <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> uzantısı yöntemi kullanılarak eklenir ve yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-473">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="d2c66-474">Aşağıdaki kural örnekleri bu konunun ilerleyen kısımlarında açıklanmıştır:</span><span class="sxs-lookup"><span data-stu-id="d2c66-474">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="d2c66-475">Rota sırası</span><span class="sxs-lookup"><span data-stu-id="d2c66-475">Route order</span></span>

<span data-ttu-id="d2c66-476">Rotalar işleme için bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> belirtir (rota eşleştirme).</span><span class="sxs-lookup"><span data-stu-id="d2c66-476">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="d2c66-477">Sipariş verme</span><span class="sxs-lookup"><span data-stu-id="d2c66-477">Order</span></span>            | <span data-ttu-id="d2c66-478">Davranış</span><span class="sxs-lookup"><span data-stu-id="d2c66-478">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="d2c66-479">-1</span><span class="sxs-lookup"><span data-stu-id="d2c66-479">-1</span></span>               | <span data-ttu-id="d2c66-480">Yol, diğer rotalar işlenmeden önce işlenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-480">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="d2c66-481">0</span><span class="sxs-lookup"><span data-stu-id="d2c66-481">0</span></span>                | <span data-ttu-id="d2c66-482">Sıra belirtilmemiş (varsayılan değer).</span><span class="sxs-lookup"><span data-stu-id="d2c66-482">Order isn't specified (default value).</span></span> <span data-ttu-id="d2c66-483">`Order` atanmazsa (`Order = null`), yolun işlenmek üzere 0 (sıfır) olarak `Order`.</span><span class="sxs-lookup"><span data-stu-id="d2c66-483">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="d2c66-484">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="d2c66-484">1, 2, &hellip; n</span></span> | <span data-ttu-id="d2c66-485">Yol işleme sırasını belirtir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-485">Specifies the route processing order.</span></span> |

<span data-ttu-id="d2c66-486">Yol işleme, kurala göre belirlenir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-486">Route processing is established by convention:</span></span>

* <span data-ttu-id="d2c66-487">Yollar sıralı sırada işlenir (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="d2c66-487">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="d2c66-488">Yolların aynı `Order`olduğunda, en belirli yol önce daha az özel yollarla eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-488">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="d2c66-489">Aynı `Order` ve aynı parametre sayısı ile rotalar bir istek URL 'siyle eşleşiyorsa, rotalar <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>eklendiği sırada işlenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-489">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="d2c66-490">Mümkünse, belirlenen bir yol işleme sırasına bağlı olarak kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="d2c66-490">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="d2c66-491">Genellikle Yönlendirme, URL eşleştirme ile doğru yolu seçer.</span><span class="sxs-lookup"><span data-stu-id="d2c66-491">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="d2c66-492">İstekleri doğru yönlendirmek için yol `Order` özelliklerini ayarlamanız gerekiyorsa, uygulamanın yönlendirme şeması büyük olasılıkla istemciler için kafa karıştırıcı olur ve bakımını yapmak için kırıcı olur.</span><span class="sxs-lookup"><span data-stu-id="d2c66-492">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="d2c66-493">Uygulamanın yönlendirme şemasını basitleştirecek şekilde arama yapın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-493">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="d2c66-494">Örnek uygulama, tek bir uygulama kullanarak birkaç yönlendirme senaryosunu göstermek için açık bir yol işleme sırası gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-494">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="d2c66-495">Ancak, üretim uygulamalarında rota `Order` ayarlama uygulamalarından kaçınmaya çalışın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-495">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="d2c66-496">Razor Pages yönlendirme ve MVC denetleyici yönlendirme bir uygulamayı paylaşır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-496">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="d2c66-497">MVC konularındaki yol sırasıyla ilgili bilgiler, [Denetleyici eylemlerine yönlendirme sırasında mevcuttur: öznitelik yollarını sıralama](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="d2c66-497">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="d2c66-498">Model kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-498">Model conventions</span></span>

<span data-ttu-id="d2c66-499">Razor Pages için uygulanan [model kuralları](xref:mvc/controllers/application-model#conventions) eklemek üzere <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> için bir temsilci ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-499">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="d2c66-500">Tüm sayfalara bir rota modeli kuralı ekleme</span><span class="sxs-lookup"><span data-stu-id="d2c66-500">Add a route model convention to all pages</span></span>

<span data-ttu-id="d2c66-501">Sayfa yönlendirme modeli oluşturma sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnekleri koleksiyonuna bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-501">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="d2c66-502">Örnek uygulama, uygulamadaki tüm sayfalara bir `{globalTemplate?}` Route şablonu ekler:</span><span class="sxs-lookup"><span data-stu-id="d2c66-502">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="d2c66-503"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği `1`olarak ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-503">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="d2c66-504">Bu, örnek uygulamada aşağıdaki yol eşleştirme davranışını sağlar:</span><span class="sxs-lookup"><span data-stu-id="d2c66-504">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="d2c66-505">`TheContactPage/{text?}` için bir yol şablonu konuya daha sonra eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-505">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="d2c66-506">Iletişim sayfası yolu, `null` (`Order = 0`) varsayılan sırasına sahiptir, bu nedenle `{globalTemplate?}` Route şablonundan önce eşleşir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-506">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="d2c66-507">Konunun ilerleyen kısımlarında `{aboutTemplate?}` yol şablonu eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-507">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="d2c66-508">`{aboutTemplate?}` şablonuna `2``Order` verilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-508">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="d2c66-509">`/About/RouteDataValue`sayfası istendiğinde,`Order = 2`özelliğinin ayarlanması nedeniyle "RouteDataValue" `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["aboutTemplate"]` (`Order`) değil.</span><span class="sxs-lookup"><span data-stu-id="d2c66-509">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="d2c66-510">Konunun ilerleyen kısımlarında `{otherPagesTemplate?}` yol şablonu eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-510">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="d2c66-511">`{otherPagesTemplate?}` şablonuna `2``Order` verilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-511">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="d2c66-512">*Sayfalar/diğer sayfalar* klasöründeki herhangi bir sayfa bir yol parametresiyle (örneğin, `/OtherPages/Page1/RouteDataValue`) istendiğinde,`Order = 2`özelliğinin ayarlanması nedeniyle "routedatavalue", `RouteData.Values["otherPagesTemplate"]` (`Order`) değil `RouteData.Values["globalTemplate"]` (`Order = 1`) olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-512">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="d2c66-513">Mümkün olan yerlerde, `Order``Order = 0`sonuç olarak ayarlanmayın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-513">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="d2c66-514">Doğru yolu seçmek için yönlendirmeyi güvenin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-514">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="d2c66-515"><xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>ekleme gibi Razor Pages seçenekler, MVC `Startup.ConfigureServices`hizmet koleksiyonuna eklendiğinde eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-515">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d2c66-516">Örnek için bkz. [örnek uygulama](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="d2c66-516">For an example, see the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="d2c66-517">`localhost:5000/About/GlobalRouteValue` 'de örneğin hakkında sayfasını isteyin ve sonucu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-517">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![Hakkında sayfası, GlobalRouteValue 'un rota segmentiyle istenir.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="d2c66-520">Tüm sayfalara uygulama modeli kuralı ekleme</span><span class="sxs-lookup"><span data-stu-id="d2c66-520">Add an app model convention to all pages</span></span>

<span data-ttu-id="d2c66-521"><xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> kullanarak, sayfa uygulama modeli oluşturma sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnekleri koleksiyonuna bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-521">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="d2c66-522">Bu ve diğer kuralları konunun ilerleyen kısımlarında göstermek için, örnek uygulama bir `AddHeaderAttribute` sınıfı içerir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-522">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="d2c66-523">Sınıf Oluşturucusu bir `name` dize ve bir `values` dize dizisi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="d2c66-523">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="d2c66-524">Bu değerler, yanıt üst bilgisini ayarlamak için `OnResultExecuting` yönteminde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-524">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="d2c66-525">Tam sınıf, konusunun ilerleyen kısımlarında [sayfa modeli eylem kuralları](#page-model-action-conventions) bölümünde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-525">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="d2c66-526">Örnek uygulama, uygulamadaki tüm sayfalara bir başlık, `GlobalHeader`eklemek için `AddHeaderAttribute` sınıfını kullanır:</span><span class="sxs-lookup"><span data-stu-id="d2c66-526">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="d2c66-527">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d2c66-527">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="d2c66-528">`localhost:5000/About` sırasında örneğin hakkında daha fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-528">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Hakkında sayfasının yanıt üstbilgileri GlobalHeader 'ın eklendiğini gösterir.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="d2c66-530">Tüm sayfalara bir işleyici modeli kuralı ekleme</span><span class="sxs-lookup"><span data-stu-id="d2c66-530">Add a handler model convention to all pages</span></span>

<span data-ttu-id="d2c66-531">Sayfa işleyicisi modelinin oluşturulması sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnekleri koleksiyonuna bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-531">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="d2c66-532">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d2c66-532">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="d2c66-533">Sayfa yolu eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-533">Page route action conventions</span></span>

<span data-ttu-id="d2c66-534"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> türetilen varsayılan yol modeli sağlayıcısı, sayfa yollarını yapılandırmak için genişletilebilirlik noktaları sağlamak üzere tasarlanan kuralları çağırır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-534">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="d2c66-535">Klasör Yönlendirme modeli kuralı</span><span class="sxs-lookup"><span data-stu-id="d2c66-535">Folder route model convention</span></span>

<span data-ttu-id="d2c66-536">Belirtilen klasör altındaki tüm sayfalar için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> bir eylemi çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-536">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="d2c66-537">Örnek uygulama, *diğer sayfalar* klasöründeki sayfalara bir `{otherPagesTemplate?}` yol şablonu eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> kullanır:</span><span class="sxs-lookup"><span data-stu-id="d2c66-537">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="d2c66-538"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği `2`olarak ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-538">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="d2c66-539">Bu, tek bir rota değeri sağlandığında `{globalTemplate?}` (konuda daha önce `1`olarak ayarlanan) şablonunun ilk yol veri değeri konumu için öncelik verilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="d2c66-539">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="d2c66-540">*Sayfalar/otherpages* klasöründeki bir sayfa bir yol parametresi değeri (örneğin, `/OtherPages/Page1/RouteDataValue`) ile isteniyorsa,`Order = 2`özelliğinin ayarlanması nedeniyle "routedatavalue", `RouteData.Values["otherPagesTemplate"]` (`Order`) değil `RouteData.Values["globalTemplate"]` (`Order = 1`) olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-540">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="d2c66-541">Mümkün olan yerlerde, `Order``Order = 0`sonuç olarak ayarlanmayın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-541">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="d2c66-542">Doğru yolu seçmek için yönlendirmeyi güvenin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-542">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="d2c66-543">Örnekteki Sayfa1 sayfasını `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` isteyin ve sonucu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-543">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Diğer sayfalar klasöründeki Sayfa1, GlobalRouteValue ve OtherPagesRouteValue yol kesimiyle istenir.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="d2c66-546">Sayfa yönlendirme modeli kuralı</span><span class="sxs-lookup"><span data-stu-id="d2c66-546">Page route model convention</span></span>

<span data-ttu-id="d2c66-547">Belirtilen ada sahip sayfanın <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> bir eylemi çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-547">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="d2c66-548">Örnek uygulama, hakkında sayfasına bir `{aboutTemplate?}` yol şablonu eklemek için `AddPageRouteModelConvention` kullanır:</span><span class="sxs-lookup"><span data-stu-id="d2c66-548">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="d2c66-549"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği `2`olarak ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-549">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="d2c66-550">Bu, tek bir rota değeri sağlandığında `{globalTemplate?}` (konuda daha önce `1`olarak ayarlanan) şablonunun ilk yol veri değeri konumu için öncelik verilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="d2c66-550">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="d2c66-551">`/About/RouteDataValue`sayfasında yol parametresi değeri varsa,`Order = 2`özelliğinin ayarlanması nedeniyle "RouteDataValue" `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["aboutTemplate"]` (`Order`) değil.</span><span class="sxs-lookup"><span data-stu-id="d2c66-551">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="d2c66-552">Mümkün olan yerlerde, `Order``Order = 0`sonuç olarak ayarlanmayın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-552">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="d2c66-553">Doğru yolu seçmek için yönlendirmeyi güvenin.</span><span class="sxs-lookup"><span data-stu-id="d2c66-553">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="d2c66-554">`localhost:5000/About/GlobalRouteValue/AboutRouteValue` 'de örneğin hakkında sayfasını isteyin ve sonucu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-554">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![GlobalRouteValue ve AboutRouteValue için rota kesimlerle ilgili sayfa hakkında bilgi istenir.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a><span data-ttu-id="d2c66-557">Sayfa yolu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d2c66-557">Configure a page route</span></span>

<span data-ttu-id="d2c66-558">Belirtilen sayfa yolundaki bir sayfaya bir yol yapılandırmak için <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-558">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="d2c66-559">Sayfa için oluşturulan bağlantılar belirtilen rotayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-559">Generated links to the page use your specified route.</span></span> <span data-ttu-id="d2c66-560">`AddPageRoute`, yolu oluşturmak için `AddPageRouteModelConvention` kullanır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-560">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="d2c66-561">Örnek uygulama, *Contact. cshtml*için `/TheContactPage` bir yol oluşturur:</span><span class="sxs-lookup"><span data-stu-id="d2c66-561">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="d2c66-562">Iletişim sayfasına, varsayılan yolu aracılığıyla `/Contact` de erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-562">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="d2c66-563">Örnek uygulamanın kişi sayfasına özel yolu, isteğe bağlı `text` yol segmentine (`{text?}`) izin verir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-563">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="d2c66-564">Bu sayfa, ziyaretçinin `/Contact` rotasında sayfaya erişmesi durumunda `@page` yönergesinde bu isteğe bağlı segmenti de içerir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-564">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="d2c66-565">İşlenmiş sayfadaki **kişi** bağlantısı IÇIN oluşturulan URL 'nin güncelleştirilmiş yolu yansıttığını unutmayın:</span><span class="sxs-lookup"><span data-stu-id="d2c66-565">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Gezinti çubuğundaki örnek uygulama Iletişim bağlantısı](razor-pages-conventions/_static/contact-link.png)

![İşlenmiş HTML 'deki kişi bağlantısının inceleniyor href 'in '/theContact tpage ' olarak ayarlandığını belirtir](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="d2c66-568">`/Contact`, kendi sıradan yönlendirmekte olan kişi sayfasını ziyaret edin, veya özel yol `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="d2c66-568">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="d2c66-569">Ek bir `text` yol kesimi sağlarsanız, sayfada sağladığınız HTML kodlu segment görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-569">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Microsoft Edge tarayıcı, URL 'de ' TextValue ' öğesinin isteğe bağlı ' text ' yol segmentini sağlamaya yönelik örnek.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="d2c66-572">Sayfa modeli eylem kuralları</span><span class="sxs-lookup"><span data-stu-id="d2c66-572">Page model action conventions</span></span>

<span data-ttu-id="d2c66-573"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> uygulayan varsayılan sayfa modeli sağlayıcısı, sayfa modellerini yapılandırmak için genişletilebilirlik noktaları sağlamak üzere tasarlanan kuralları çağırır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-573">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="d2c66-574">Bu kurallar sayfa bulma ve işleme senaryolarını oluştururken ve değiştirirken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-574">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="d2c66-575">Bu bölümdeki örneklerde örnek uygulama, yanıt üst bilgisini uygulayan bir <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>`AddHeaderAttribute` sınıfı kullanır:</span><span class="sxs-lookup"><span data-stu-id="d2c66-575">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="d2c66-576">Kurallar kullanılarak, örnek bir klasördeki tüm sayfalara ve tek bir sayfaya özniteliğin nasıl uygulanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-576">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="d2c66-577">**Klasör uygulama modeli kuralı**</span><span class="sxs-lookup"><span data-stu-id="d2c66-577">**Folder app model convention**</span></span>

<span data-ttu-id="d2c66-578">Belirtilen klasör altındaki tüm sayfalar için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> örneklerine bir eylem çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-578">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="d2c66-579">Örnek, uygulamanın *diğer sayfalar* klasörünün içindeki sayfalara bir başlık, `OtherPagesHeader`ekleyerek `AddFolderApplicationModelConvention` kullanımını gösterir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-579">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="d2c66-580">Örnekteki Sayfa1 sayfasını `localhost:5000/OtherPages/Page1` isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-580">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Diğer sayfalar/Sayfa1 sayfasının yanıt üstbilgileri, OtherPagesHeader öğesinin eklendiğini gösterir.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="d2c66-582">**Sayfa uygulama modeli kuralı**</span><span class="sxs-lookup"><span data-stu-id="d2c66-582">**Page app model convention**</span></span>

<span data-ttu-id="d2c66-583">Belirtilen ada sahip sayfanın <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> bir eylemi çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-583">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="d2c66-584">Örnek, hakkında sayfasına `AboutHeader`bir başlık ekleyerek `AddPageApplicationModelConvention` kullanımını gösterir:</span><span class="sxs-lookup"><span data-stu-id="d2c66-584">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="d2c66-585">`localhost:5000/About` sırasında örneğin hakkında daha fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-585">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Hakkında sayfasının yanıt üstbilgileri AboutHeader eklendiğini gösterir.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="d2c66-587">**Filtre yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="d2c66-587">**Configure a filter**</span></span>

<span data-ttu-id="d2c66-588"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>, belirtilen filtreyi uygulamak üzere yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-588"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="d2c66-589">Bir filtre sınıfı uygulayabilirsiniz, ancak örnek uygulama bir lambda ifadesinde bir filtrenin nasıl uygulanacağını gösterir, bu da bir filtre döndüren bir fabrika olarak arka planda uygulandı:</span><span class="sxs-lookup"><span data-stu-id="d2c66-589">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="d2c66-590">Sayfa uygulama modeli, *diğer sayfalar* klasöründeki Page2 sayfasına yol açan parçaların göreli yolunu denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-590">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="d2c66-591">Koşul geçerse, bir üst bilgi eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-591">If the condition passes, a header is added.</span></span> <span data-ttu-id="d2c66-592">Aksi takdirde, `EmptyFilter` uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-592">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="d2c66-593">`EmptyFilter` bir [eylem filtresidir](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="d2c66-593">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="d2c66-594">Eylem filtreleri Razor Pages tarafından yoksayıldığından, yolun `OtherPages/Page2`içermiyorsa `EmptyFilter` hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="d2c66-594">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="d2c66-595">`localhost:5000/OtherPages/Page2` 'de örneğin Page2 sayfasını isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-595">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header, Page2 için yanıta eklenir.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="d2c66-597">**Filtre fabrikası yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="d2c66-597">**Configure a filter factory**</span></span>

<span data-ttu-id="d2c66-598"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>, belirtilen fabrikayı tüm Razor Pages [filtre](xref:mvc/controllers/filters) uygulayacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-598"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="d2c66-599">Örnek uygulama, uygulamanın sayfalarına iki değer içeren `FilterFactoryHeader`bir üst bilgi ekleyerek bir [filtre fabrikası](xref:mvc/controllers/filters#ifilterfactory) kullanılmasına bir örnek sağlar:</span><span class="sxs-lookup"><span data-stu-id="d2c66-599">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="d2c66-600">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="d2c66-600">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="d2c66-601">`localhost:5000/About` sırasında örneğin hakkında daha fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:</span><span class="sxs-lookup"><span data-stu-id="d2c66-601">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Hakkında sayfasının yanıt üstbilgileri iki FilterFactoryHeader üst bilgisinin eklendiğini gösterir.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="d2c66-603">MVC filtreleri ve sayfa filtresi (ıpagefilter)</span><span class="sxs-lookup"><span data-stu-id="d2c66-603">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="d2c66-604">Razor Pages işleyici yöntemleri kullandığından, MVC [eylem filtreleri](xref:mvc/controllers/filters#action-filters) Razor Pages tarafından yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="d2c66-604">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="d2c66-605">Diğer MVC filtresi türleri şunlardır: [Yetkilendirme](xref:mvc/controllers/filters#authorization-filters), [özel durum](xref:mvc/controllers/filters#exception-filters), [kaynak](xref:mvc/controllers/filters#resource-filters)ve [sonuç](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="d2c66-605">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="d2c66-606">Daha fazla bilgi için [Filtreler](xref:mvc/controllers/filters) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="d2c66-606">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="d2c66-607">Sayfa filtresi (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) Razor Pages için geçerli bir filtredir.</span><span class="sxs-lookup"><span data-stu-id="d2c66-607">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="d2c66-608">Daha fazla bilgi için bkz. [Razor Pages Için filtre yöntemleri](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="d2c66-608">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d2c66-609">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d2c66-609">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end
