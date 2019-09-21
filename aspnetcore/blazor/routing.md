---
title: ASP.NET Core Blazor yönlendirme
author: guardrex
description: Uygulamalardaki istekleri yönlendirme ve gezinti bağlantısı bileşeni hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/routing
ms.openlocfilehash: 6d9d1614b6e0cc9f4711de0db4513ada4841809f
ms.sourcegitcommit: e5a74f882c14eaa0e5639ff082355e130559ba83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168182"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="a10d3-103">ASP.NET Core Blazor yönlendirme</span><span class="sxs-lookup"><span data-stu-id="a10d3-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="a10d3-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a10d3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="a10d3-105">İsteklerin nasıl yönlendirileceğini ve Blazor uygulamalarında gezinti bağlantıları oluşturmak için `NavLink` bileşenin nasıl kullanılacağını öğrenin.</span><span class="sxs-lookup"><span data-stu-id="a10d3-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="a10d3-106">Uç nokta yönlendirme tümleştirmesi ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a10d3-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="a10d3-107">Blazor sunucusu [ASP.NET Core uç nokta yönlendirme](xref:fundamentals/routing)ile tümleşiktir.</span><span class="sxs-lookup"><span data-stu-id="a10d3-107">Blazor Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="a10d3-108">ASP.NET Core bir uygulama, `MapBlazorHub` içindeki `Startup.Configure`etkileşimli bileşenler için gelen bağlantıları kabul edecek şekilde yapılandırılmıştır:</span><span class="sxs-lookup"><span data-stu-id="a10d3-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a><span data-ttu-id="a10d3-109">Rota şablonları</span><span class="sxs-lookup"><span data-stu-id="a10d3-109">Route templates</span></span>

<span data-ttu-id="a10d3-110">Bileşeni `Router` , belirtilen bir rota ile her bileşene yönlendirmeyi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a10d3-110">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="a10d3-111">Bileşen App. Razor dosyasında görünür: `Router`</span><span class="sxs-lookup"><span data-stu-id="a10d3-111">The `Router` component appears in the *App.razor* file:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <p>Sorry, there's nothing at this address.</p>
    </NotFound>
</Router>
```

<span data-ttu-id="a10d3-112">Bir`@page` yönergeyle bir *. Razor* dosyası derlendiğinde, oluşturulan sınıf, yol şablonunu belirten bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a10d3-112">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="a10d3-113">Çalışma zamanında, `RouteView` bileşen:</span><span class="sxs-lookup"><span data-stu-id="a10d3-113">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="a10d3-114">' İ istediğiniz parametrelerle birlikte alır. `Router` `RouteData`</span><span class="sxs-lookup"><span data-stu-id="a10d3-114">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="a10d3-115">Belirtilen parametreleri kullanarak belirtilen bileşeni düzeniyle (veya isteğe bağlı bir varsayılan düzende) işler.</span><span class="sxs-lookup"><span data-stu-id="a10d3-115">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="a10d3-116">İsteğe bağlı olarak, bir `DefaultLayout` düzen belirtmeyen bileşenler için kullanılacak düzen sınıfıyla bir parametre belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a10d3-116">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="a10d3-117">Varsayılan Blazor şablonları `MainLayout` bileşeni belirler.</span><span class="sxs-lookup"><span data-stu-id="a10d3-117">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="a10d3-118">*Mainlayout. Razor* , şablon projenin *paylaşılan* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="a10d3-118">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="a10d3-119">Düzenler hakkında daha fazla bilgi için bkz <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="a10d3-119">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="a10d3-120">Birden çok yol şablonu, bir bileşene uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="a10d3-120">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="a10d3-121">Aşağıdaki bileşen ve `/BlazorRoute` `/DifferentBlazorRoute`için isteklere yanıt verir:</span><span class="sxs-lookup"><span data-stu-id="a10d3-121">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="a10d3-122">URL 'lerin doğru şekilde çözülmesi için `<base>` , uygulamanın, `href` özniteliğinde belirtilen uygulama temel yolu ile *Wwwroot/index.html* File (Blazor webassembly) veya *Pages/_host. cshtml* dosyasında (Blazor Server) bir etiketi içermesi gerekir (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="a10d3-122">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="a10d3-123">Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="a10d3-123">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="a10d3-124">İçerik bulunamadığında özel içerik sağla</span><span class="sxs-lookup"><span data-stu-id="a10d3-124">Provide custom content when content isn't found</span></span>

<span data-ttu-id="a10d3-125">`Router` Bileşen, istenen rota için içerik bulunmazsa uygulamanın özel içerik belirtmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="a10d3-125">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="a10d3-126">*App. Razor* dosyasında, `NotFound` `Router` bileşenin Şablon parametresinde özel içerik ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="a10d3-126">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFound>
</Router>
```

<span data-ttu-id="a10d3-127">`<NotFound>` Etiketlerin içeriği, diğer etkileşimli bileşenler gibi rastgele öğeler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="a10d3-127">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="a10d3-128">`NotFound` İçeriğe varsayılan bir düzen uygulamak için bkz <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="a10d3-128">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="a10d3-129">Birden çok derlemeden bileşenlere rota</span><span class="sxs-lookup"><span data-stu-id="a10d3-129">Route to components from multiple assemblies</span></span>

<span data-ttu-id="a10d3-130">Bileşen için yönlendirilebilir bileşenleri ararken dikkate alınması gereken ek derlemeleri belirtmek için parametresinikullanın.`AdditionalAssemblies` `Router`</span><span class="sxs-lookup"><span data-stu-id="a10d3-130">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="a10d3-131">Belirtilen derlemeler, `AppAssembly`belirtilen derlemeye ek olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="a10d3-131">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="a10d3-132">Aşağıdaki örnekte, `Component1` başvurulan bir sınıf kitaplığında tanımlanan yönlendirilebilir bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="a10d3-132">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="a10d3-133">Aşağıdaki `AdditionalAssemblies` örnek, için `Component1`yönlendirme desteği ile sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="a10d3-133">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

<span data-ttu-id="a10d3-134">< yönlendirici AppAssembly = "typeof (program). Derleme "AdditionalAssemblies =" New [] {typeof (Component1). Bütünleştirilmiş kod} >...</Router></span><span class="sxs-lookup"><span data-stu-id="a10d3-134"><Router AppAssembly="typeof(Program).Assembly" AdditionalAssemblies="new[] { typeof(Component1).Assembly }> ... </Router></span></span>

## <a name="route-parameters"></a><span data-ttu-id="a10d3-135">Rota parametreleri</span><span class="sxs-lookup"><span data-stu-id="a10d3-135">Route parameters</span></span>

<span data-ttu-id="a10d3-136">Yönlendirici, karşılık gelen bileşen parametrelerini aynı ada (büyük/küçük harfe duyarsız) doldurmak için yol parametrelerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="a10d3-136">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="a10d3-137">ASP.NET Core 3,0 önizlemesinde Blazor uygulamaları için isteğe bağlı parametreler desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="a10d3-137">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="a10d3-138">Önceki `@page` örnekte iki yönergeler uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a10d3-138">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="a10d3-139">İlki, bir parametre olmadan bileşene gezinmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="a10d3-139">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="a10d3-140">İkinci `@page` yönerge, `{text}` yol parametresini alır ve değeri `Text` özelliğine atar.</span><span class="sxs-lookup"><span data-stu-id="a10d3-140">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="a10d3-141">Yol kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="a10d3-141">Route constraints</span></span>

<span data-ttu-id="a10d3-142">Yol kısıtlaması bir yönlendirme segmentinde bir bileşene tür eşleştirmeyi zorlar.</span><span class="sxs-lookup"><span data-stu-id="a10d3-142">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="a10d3-143">Aşağıdaki örnekte, `Users` bileşen yolu yalnızca şu durumlarda eşleşir:</span><span class="sxs-lookup"><span data-stu-id="a10d3-143">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="a10d3-144">İstek `Id` URL 'sinde bir yol kesimi var.</span><span class="sxs-lookup"><span data-stu-id="a10d3-144">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="a10d3-145">Segment bir tamsayıdır (`int`). `Id`</span><span class="sxs-lookup"><span data-stu-id="a10d3-145">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="a10d3-146">Aşağıdaki tabloda gösterilen yol kısıtlamaları mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="a10d3-146">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="a10d3-147">Sabit kültür ile eşleşen yol kısıtlamaları için daha fazla bilgi için tablonun altındaki uyarıya bakın.</span><span class="sxs-lookup"><span data-stu-id="a10d3-147">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="a10d3-148">Kısıtlaması</span><span class="sxs-lookup"><span data-stu-id="a10d3-148">Constraint</span></span> | <span data-ttu-id="a10d3-149">Örnek</span><span class="sxs-lookup"><span data-stu-id="a10d3-149">Example</span></span>           | <span data-ttu-id="a10d3-150">Örnek eşleşmeler</span><span class="sxs-lookup"><span data-stu-id="a10d3-150">Example Matches</span></span>                                                                  | <span data-ttu-id="a10d3-151">Bilmesi</span><span class="sxs-lookup"><span data-stu-id="a10d3-151">Invariant</span></span><br><span data-ttu-id="a10d3-152">kültür</span><span class="sxs-lookup"><span data-stu-id="a10d3-152">culture</span></span><br><span data-ttu-id="a10d3-153">eşleştirme</span><span class="sxs-lookup"><span data-stu-id="a10d3-153">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="a10d3-154">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="a10d3-154">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="a10d3-155">Hayır</span><span class="sxs-lookup"><span data-stu-id="a10d3-155">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="a10d3-156">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="a10d3-156">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="a10d3-157">Evet</span><span class="sxs-lookup"><span data-stu-id="a10d3-157">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="a10d3-158">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="a10d3-158">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="a10d3-159">Evet</span><span class="sxs-lookup"><span data-stu-id="a10d3-159">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="a10d3-160">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="a10d3-160">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="a10d3-161">Evet</span><span class="sxs-lookup"><span data-stu-id="a10d3-161">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="a10d3-162">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="a10d3-162">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="a10d3-163">Evet</span><span class="sxs-lookup"><span data-stu-id="a10d3-163">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="a10d3-164">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="a10d3-164">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="a10d3-165">Hayır</span><span class="sxs-lookup"><span data-stu-id="a10d3-165">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="a10d3-166">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="a10d3-166">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="a10d3-167">Evet</span><span class="sxs-lookup"><span data-stu-id="a10d3-167">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="a10d3-168">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="a10d3-168">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="a10d3-169">Evet</span><span class="sxs-lookup"><span data-stu-id="a10d3-169">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="a10d3-170">URL 'yi doğrulayan ve bir clr türüne ( `int` veya `DateTime`gibi) dönüştürülen yol kısıtlamaları, her zaman sabit kültürü kullanır.</span><span class="sxs-lookup"><span data-stu-id="a10d3-170">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="a10d3-171">Bu kısıtlamalar, URL 'nin yerelleştirilemeyen olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="a10d3-171">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="a10d3-172">Noktalar içeren URL 'lerle yönlendirme</span><span class="sxs-lookup"><span data-stu-id="a10d3-172">Routing with URLs that contain dots</span></span>

<span data-ttu-id="a10d3-173">Blazor Server uygulamalarında, *_Host. cshtml* `/` içindeki varsayılan yol (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="a10d3-173">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="a10d3-174">URL bir dosya isteyecek şekilde göründüğünden, nokta`.`() içeren bir istek URL 'si varsayılan yol tarafından eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="a10d3-174">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="a10d3-175">Bir Blazor uygulaması mevcut olmayan bir statik dosya için *404-Found* yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="a10d3-175">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="a10d3-176">Bir nokta içeren yolları kullanmak için *_Host. cshtml* 'yi aşağıdaki yol şablonuyla yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="a10d3-176">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="a10d3-177">`"/{**path}"` Şablon şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="a10d3-177">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="a10d3-178">Ters eğik çizgi (`/`) kodlaması olmadan birden`**`çok klasör sınırlarındaki yolu yakalamak için çift yıldız *catch-all* söz dizimi ().</span><span class="sxs-lookup"><span data-stu-id="a10d3-178">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="a10d3-179">`path` Rota parametresi adı.</span><span class="sxs-lookup"><span data-stu-id="a10d3-179">A `path` route parameter name.</span></span>

<span data-ttu-id="a10d3-180">Daha fazla bilgi için bkz. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="a10d3-180">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="a10d3-181">Gezinti bağlantısı bileşeni</span><span class="sxs-lookup"><span data-stu-id="a10d3-181">NavLink component</span></span>

<span data-ttu-id="a10d3-182">Gezinti bağlantıları `NavLink` oluştururken, HTML köprü öğelerinin (`<a>`) yerine bir bileşen kullanın.</span><span class="sxs-lookup"><span data-stu-id="a10d3-182">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="a10d3-183">Bir `NavLink` bileşen `<a>` ,geçerli`href` URL ile eşleşip eşleşmediğini temel alarak bir CSSsınıfınageçişyaptığısürecebiröğesigibidavranır.`active`</span><span class="sxs-lookup"><span data-stu-id="a10d3-183">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="a10d3-184">`active` Sınıfı, bir kullanıcının hangi sayfanın etkin sayfa olduğunu anladığı gezinti bağlantıları arasında yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="a10d3-184">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="a10d3-185">Aşağıdaki `NavMenu` bileşen, bileşenlerin nasıl [](https://getbootstrap.com/docs/) kullanılacağını `NavLink` gösteren bir önyükleme gezinti çubuğu oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a10d3-185">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="a10d3-186">Öğesinin özniteliğine atayabilmeniz için kullanabileceğiniz iki `NavLinkMatch` `Match`seçenekvardır `<NavLink>` :</span><span class="sxs-lookup"><span data-stu-id="a10d3-186">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="a10d3-187">`NavLinkMatch.All`Tüm geçerli URL ile eşleştiğinde etkin olur.`NavLink` &ndash;</span><span class="sxs-lookup"><span data-stu-id="a10d3-187">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="a10d3-188">`NavLinkMatch.Prefix`(*varsayılan*) Geçerli URL 'nin herhangi bir önekiyle eşleştiğinde etkin olur.`NavLink` &ndash;</span><span class="sxs-lookup"><span data-stu-id="a10d3-188">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="a10d3-189">`NavLink` Yukarıdaki örnekte, ana `href=""` giriş `active` URL 'siyle eşleşir ve yalnızca uygulamanın varsayılan temel yol URL 'sindeki CSS sınıfını alır (örneğin, `https://localhost:5001/`).</span><span class="sxs-lookup"><span data-stu-id="a10d3-189">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="a10d3-190">`NavLink` İkincisi, Kullanıcı ön `active` eki olan herhangi bir `MyComponent` URL 'yi ziyaret ettiğinde sınıfı alır (örneğin, `https://localhost:5001/MyComponent` ve `https://localhost:5001/MyComponent/AnotherSegment`).</span><span class="sxs-lookup"><span data-stu-id="a10d3-190">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="a10d3-191">Ek `NavLink` bileşen öznitelikleri, işlenen tutturucu etiketine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a10d3-191">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="a10d3-192">Aşağıdaki örnekte, `NavLink` bileşen `target` özniteliğini içerir:</span><span class="sxs-lookup"><span data-stu-id="a10d3-192">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="a10d3-193">Aşağıdaki HTML biçimlendirmesi işlenir:</span><span class="sxs-lookup"><span data-stu-id="a10d3-193">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="a10d3-194">URI ve gezinti durumu yardımcıları</span><span class="sxs-lookup"><span data-stu-id="a10d3-194">URI and navigation state helpers</span></span>

<span data-ttu-id="a10d3-195">Kod `Microsoft.AspNetCore.Components.NavigationManager` içinde C# URI ve gezinme ile çalışmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="a10d3-195">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="a10d3-196">`NavigationManager`Aşağıdaki tabloda gösterilen olay ve yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="a10d3-196">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="a10d3-197">Üye</span><span class="sxs-lookup"><span data-stu-id="a10d3-197">Member</span></span> | <span data-ttu-id="a10d3-198">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a10d3-198">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="a10d3-199">Geçerli mutlak URI 'yi alır.</span><span class="sxs-lookup"><span data-stu-id="a10d3-199">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="a10d3-200">Mutlak bir URI oluşturmak için göreli URI yollarına eklenebilir olan temel URI 'yi (sondaki eğik çizgiyle birlikte) alır.</span><span class="sxs-lookup"><span data-stu-id="a10d3-200">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="a10d3-201">Genellikle, `BaseUri` *Wwwroot/index.html* (Blazor `href` webassembly) veya `<base>` *Pages/_host. cshtml* (Blazor Server) içindeki belge öğesindeki özniteliğe karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a10d3-201">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> |
| `NavigateTo` | <span data-ttu-id="a10d3-202">Belirtilen URI 'ye gider.</span><span class="sxs-lookup"><span data-stu-id="a10d3-202">Navigates to the specified URI.</span></span> <span data-ttu-id="a10d3-203">`forceLoad` Şu`true`ise:</span><span class="sxs-lookup"><span data-stu-id="a10d3-203">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="a10d3-204">İstemci tarafı yönlendirme atlanır.</span><span class="sxs-lookup"><span data-stu-id="a10d3-204">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="a10d3-205">Bu tarayıcı, URI 'nin normalde istemci tarafı yönlendirici tarafından işlenip işlenmediğini sunucudan yeni sayfayı yüklemeye zorlanır.</span><span class="sxs-lookup"><span data-stu-id="a10d3-205">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="a10d3-206">Gezinti konumu değiştiğinde harekete gelen bir olay.</span><span class="sxs-lookup"><span data-stu-id="a10d3-206">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="a10d3-207">Göreli bir URI 'yi mutlak bir URI 'ye dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="a10d3-207">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="a10d3-208">Temel URI (örneğin, daha önce tarafından `GetBaseUri`döndürülen bir URI) verildiğinde, mutlak bir URI 'yi taban URI önekine göre bir URI 'ye dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="a10d3-208">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="a10d3-209">Aşağıdaki bileşen, düğme seçildiğinde uygulamanın `Counter` bileşenine gider:</span><span class="sxs-lookup"><span data-stu-id="a10d3-209">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

```cshtml
@page "/navigate"
@inject NavigationManager NavigationManager

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        NavigationManager.NavigateTo("counter");
    }
}
```
