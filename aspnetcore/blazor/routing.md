---
title: ASP.NET Core Blazor yönlendirme
author: guardrex
description: Uygulamalardaki istekleri yönlendirme ve gezinti bağlantısı bileşeni hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/routing
ms.openlocfilehash: d4b76c00f79f333884fa7e30b27eadc6e36de287
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72589931"
---
# <a name="aspnet-core-opno-locblazor-routing"></a><span data-ttu-id="f47b0-103">ASP.NET Core Blazor yönlendirme</span><span class="sxs-lookup"><span data-stu-id="f47b0-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="f47b0-104">[Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="f47b0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="f47b0-105">İstekleri yönlendirmeyi ve Blazor uygulamalarda gezinti bağlantıları oluşturmak için `NavLink` bileşenini kullanmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="f47b0-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="f47b0-106">Uç nokta yönlendirme tümleştirmesi ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f47b0-106">ASP.NET Core endpoint routing integration</span></span>

Blazor<span data-ttu-id="f47b0-107"> sunucusu [ASP.NET Core uç nokta yönlendirmesinde](xref:fundamentals/routing)tümleşiktir.</span><span class="sxs-lookup"><span data-stu-id="f47b0-107"> Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="f47b0-108">ASP.NET Core bir uygulama, `Startup.Configure` ' de `MapBlazorHub` ile etkileşimli bileşenlere yönelik gelen bağlantıları kabul edecek şekilde yapılandırılmıştır:</span><span class="sxs-lookup"><span data-stu-id="f47b0-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="f47b0-109">En yaygın yapılandırma, tüm istekleri Razor sayfasına yönlendirmesidir ve bu, Blazor sunucu uygulamasının sunucu tarafı bölümü için ana bilgisayar görevi görür.</span><span class="sxs-lookup"><span data-stu-id="f47b0-109">The most typical configuration is to route all requests to a Razor page, which acts as the host for the server-side part of the Blazor Server app.</span></span> <span data-ttu-id="f47b0-110">Kural gereği, *ana bilgisayar* sayfası genellikle *_host. cshtml*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="f47b0-110">By convention, the *host* page is usually named *_Host.cshtml*.</span></span> <span data-ttu-id="f47b0-111">Ana bilgisayar dosyasında belirtilen yol, yol eşleştirilirken düşük bir öncelik ile çalıştığından bir *geri dönüş yolu* olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="f47b0-111">The route specified in the host file is called a *fallback route* because it operates with a low priority in route matching.</span></span> <span data-ttu-id="f47b0-112">Geri dönüş yolu, diğer yollar eşleşmediği zaman kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="f47b0-112">The fallback route is considered when other routes don't match.</span></span> <span data-ttu-id="f47b0-113">Bu, uygulamanın Blazor sunucu uygulamasıyla kesintiye uğramadan diğer denetleyicileri ve sayfaları kullanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="f47b0-113">This allows the app to use others controllers and pages without interfering with the Blazor Server app.</span></span>

## <a name="route-templates"></a><span data-ttu-id="f47b0-114">Rota şablonları</span><span class="sxs-lookup"><span data-stu-id="f47b0-114">Route templates</span></span>

<span data-ttu-id="f47b0-115">@No__t_0 bileşeni, belirtilen bir rota ile her bileşene yönlendirmeyi sağlar.</span><span class="sxs-lookup"><span data-stu-id="f47b0-115">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="f47b0-116">@No__t_0 bileşeni *app. Razor* dosyasında görünür:</span><span class="sxs-lookup"><span data-stu-id="f47b0-116">The `Router` component appears in the *App.razor* file:</span></span>

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

<span data-ttu-id="f47b0-117">@No__t_1 yönergesine sahip bir *. Razor* dosyası derlendiğinde, oluşturulan sınıf yol şablonunu belirten bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> sağlanır.</span><span class="sxs-lookup"><span data-stu-id="f47b0-117">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="f47b0-118">Çalışma zamanında `RouteView` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="f47b0-118">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="f47b0-119">Tüm istenen parametrelerle birlikte `Router` `RouteData` alır.</span><span class="sxs-lookup"><span data-stu-id="f47b0-119">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="f47b0-120">Belirtilen parametreleri kullanarak belirtilen bileşeni düzeniyle (veya isteğe bağlı bir varsayılan düzende) işler.</span><span class="sxs-lookup"><span data-stu-id="f47b0-120">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="f47b0-121">İsteğe bağlı olarak, düzen belirtmeyen bileşenler için kullanılacak düzen sınıfıyla birlikte `DefaultLayout` parametresini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f47b0-121">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="f47b0-122">Varsayılan Blazor şablonları `MainLayout` bileşenini belirtir.</span><span class="sxs-lookup"><span data-stu-id="f47b0-122">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="f47b0-123">*Mainlayout. Razor* , şablon projenin *paylaşılan* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="f47b0-123">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="f47b0-124">Düzenler hakkında daha fazla bilgi için bkz. <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="f47b0-124">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="f47b0-125">Birden çok yol şablonu, bir bileşene uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="f47b0-125">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="f47b0-126">Aşağıdaki bileşen `/BlazorRoute` ve `/DifferentBlazorRoute` isteklerine yanıt verir:</span><span class="sxs-lookup"><span data-stu-id="f47b0-126">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="f47b0-127">URL 'Lerin doğru şekilde çözülmesi için, uygulamanın, `href` özniteliğinde belirtilen uygulama temel yolu ile *Wwwroot/index.html* dosyası (Blazor WebAssembly) veya *Pages/_host. cshtml* dosyasında (Blazor Server) bir `<base>` etiketi içermesi gerekir (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="f47b0-127">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="f47b0-128">Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="f47b0-128">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="f47b0-129">İçerik bulunamadığında özel içerik sağla</span><span class="sxs-lookup"><span data-stu-id="f47b0-129">Provide custom content when content isn't found</span></span>

<span data-ttu-id="f47b0-130">@No__t_0 bileşeni, istenen rota için içerik bulunmazsa uygulamanın özel içerik belirtmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="f47b0-130">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="f47b0-131">*App. Razor* dosyasında, `Router` bileşeninin `NotFound` Şablon parametresinde özel içerik ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="f47b0-131">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

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

<span data-ttu-id="f47b0-132">@No__t_0 etiketlerinin içeriği, diğer etkileşimli bileşenler gibi rastgele öğeler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="f47b0-132">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="f47b0-133">@No__t_0 içeriğe varsayılan bir düzen uygulamak için bkz. <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="f47b0-133">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="f47b0-134">Birden çok derlemeden bileşenlere rota</span><span class="sxs-lookup"><span data-stu-id="f47b0-134">Route to components from multiple assemblies</span></span>

<span data-ttu-id="f47b0-135">@No__t_1 bileşenin yönlendirilebilir bileşenleri ararken dikkate alınması için ek derlemeler belirtmek üzere `AdditionalAssemblies` parametresini kullanın.</span><span class="sxs-lookup"><span data-stu-id="f47b0-135">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="f47b0-136">Belirtilen derlemeler `AppAssembly` belirtilen derlemeye ek olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="f47b0-136">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="f47b0-137">Aşağıdaki örnekte, `Component1`, başvurulan bir sınıf kitaplığında tanımlanan yönlendirilebilir bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="f47b0-137">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="f47b0-138">Aşağıdaki `AdditionalAssemblies` örneği, `Component1` için yönlendirme desteğiyle sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="f47b0-138">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

```cshtml
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a><span data-ttu-id="f47b0-139">Rota parametreleri</span><span class="sxs-lookup"><span data-stu-id="f47b0-139">Route parameters</span></span>

<span data-ttu-id="f47b0-140">Yönlendirici, karşılık gelen bileşen parametrelerini aynı ada (büyük/küçük harfe duyarsız) doldurmak için yol parametrelerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="f47b0-140">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="f47b0-141">ASP.NET Core 3,0 ' deki Blazor uygulamalar için isteğe bağlı parametreler desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="f47b0-141">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0.</span></span> <span data-ttu-id="f47b0-142">Önceki örnekte iki `@page` yönergesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="f47b0-142">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="f47b0-143">İlki, bir parametre olmadan bileşene gezinmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="f47b0-143">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="f47b0-144">İkinci `@page` yönergesi `{text}` yol parametresini alır ve değeri `Text` özelliğine atar.</span><span class="sxs-lookup"><span data-stu-id="f47b0-144">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="f47b0-145">Yol kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="f47b0-145">Route constraints</span></span>

<span data-ttu-id="f47b0-146">Yol kısıtlaması bir yönlendirme segmentinde bir bileşene tür eşleştirmeyi zorlar.</span><span class="sxs-lookup"><span data-stu-id="f47b0-146">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="f47b0-147">Aşağıdaki örnekte, `Users` bileşeninin yolu yalnızca şu durumlarda eşleşir:</span><span class="sxs-lookup"><span data-stu-id="f47b0-147">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="f47b0-148">İstek URL 'sinde bir `Id` yol kesimi var.</span><span class="sxs-lookup"><span data-stu-id="f47b0-148">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="f47b0-149">@No__t_0 segmenti bir tamsayıdır (`int`).</span><span class="sxs-lookup"><span data-stu-id="f47b0-149">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="f47b0-150">Aşağıdaki tabloda gösterilen yol kısıtlamaları mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="f47b0-150">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="f47b0-151">Sabit kültür ile eşleşen yol kısıtlamaları için daha fazla bilgi için tablonun altındaki uyarıya bakın.</span><span class="sxs-lookup"><span data-stu-id="f47b0-151">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="f47b0-152">Kısıtlaması</span><span class="sxs-lookup"><span data-stu-id="f47b0-152">Constraint</span></span> | <span data-ttu-id="f47b0-153">Örnek</span><span class="sxs-lookup"><span data-stu-id="f47b0-153">Example</span></span>           | <span data-ttu-id="f47b0-154">Örnek eşleşmeler</span><span class="sxs-lookup"><span data-stu-id="f47b0-154">Example Matches</span></span>                                                                  | <span data-ttu-id="f47b0-155">Bilmesi</span><span class="sxs-lookup"><span data-stu-id="f47b0-155">Invariant</span></span><br><span data-ttu-id="f47b0-156">kültür</span><span class="sxs-lookup"><span data-stu-id="f47b0-156">culture</span></span><br><span data-ttu-id="f47b0-157">eşleştirme</span><span class="sxs-lookup"><span data-stu-id="f47b0-157">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="f47b0-158">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="f47b0-158">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="f47b0-159">Hayır</span><span class="sxs-lookup"><span data-stu-id="f47b0-159">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="f47b0-160">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="f47b0-160">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="f47b0-161">Evet</span><span class="sxs-lookup"><span data-stu-id="f47b0-161">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="f47b0-162">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="f47b0-162">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="f47b0-163">Evet</span><span class="sxs-lookup"><span data-stu-id="f47b0-163">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="f47b0-164">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="f47b0-164">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="f47b0-165">Evet</span><span class="sxs-lookup"><span data-stu-id="f47b0-165">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="f47b0-166">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="f47b0-166">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="f47b0-167">Evet</span><span class="sxs-lookup"><span data-stu-id="f47b0-167">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="f47b0-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="f47b0-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="f47b0-169">Hayır</span><span class="sxs-lookup"><span data-stu-id="f47b0-169">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="f47b0-170">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="f47b0-170">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="f47b0-171">Evet</span><span class="sxs-lookup"><span data-stu-id="f47b0-171">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="f47b0-172">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="f47b0-172">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="f47b0-173">Evet</span><span class="sxs-lookup"><span data-stu-id="f47b0-173">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="f47b0-174">URL 'YI doğrulayan ve bir CLR türüne (`int` veya `DateTime`) dönüştürülen yol kısıtlamaları her zaman sabit kültürü kullanır.</span><span class="sxs-lookup"><span data-stu-id="f47b0-174">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="f47b0-175">Bu kısıtlamalar, URL 'nin yerelleştirilemeyen olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="f47b0-175">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="f47b0-176">Noktalar içeren URL 'lerle yönlendirme</span><span class="sxs-lookup"><span data-stu-id="f47b0-176">Routing with URLs that contain dots</span></span>

<span data-ttu-id="f47b0-177">@No__t_0 Server uygulamalarında, *_Host. cshtml* içindeki varsayılan yol `/` (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="f47b0-177">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="f47b0-178">URL bir dosya isteyecek şekilde göründüğünden, nokta (`.`) içeren bir istek URL 'SI varsayılan yol tarafından eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="f47b0-178">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="f47b0-179">@No__t_0 bir uygulama, var olmayan bir statik dosya için *404-Found* yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="f47b0-179">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="f47b0-180">Bir nokta içeren yolları kullanmak için *_Host. cshtml* 'yi aşağıdaki yol şablonuyla yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="f47b0-180">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="f47b0-181">@No__t_0 şablonu şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="f47b0-181">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="f47b0-182">Çift yıldız işareti (`**`), eğik çizgi (`/`) kodlaması olmadan birden çok klasör sınırlarındaki *yolu yakalar.*</span><span class="sxs-lookup"><span data-stu-id="f47b0-182">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="f47b0-183">@No__t_0 yol parametresi adı.</span><span class="sxs-lookup"><span data-stu-id="f47b0-183">A `path` route parameter name.</span></span>

<span data-ttu-id="f47b0-184">Daha fazla bilgi için bkz. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="f47b0-184">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="f47b0-185">Gezinti bağlantısı bileşeni</span><span class="sxs-lookup"><span data-stu-id="f47b0-185">NavLink component</span></span>

<span data-ttu-id="f47b0-186">Gezinti bağlantıları oluştururken, HTML köprü öğelerinin yerine `NavLink` bileşeni kullanın (`<a>`).</span><span class="sxs-lookup"><span data-stu-id="f47b0-186">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="f47b0-187">Bir `NavLink` bileşeni, `href` geçerli URL ile eşleşip eşleşmediğini temel alan `active` CSS sınıfına geçiş yaparken, bir `<a>` öğesi gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="f47b0-187">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="f47b0-188">@No__t_0 sınıfı, bir kullanıcının hangi sayfanın etkin sayfa olduğunu anlamasına yardımcı olan gezinti bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="f47b0-188">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="f47b0-189">Aşağıdaki `NavMenu` bileşeni, `NavLink` bileşenlerinin nasıl kullanılacağını gösteren bir [önyükleme](https://getbootstrap.com/docs/) gezinti çubuğu oluşturur:</span><span class="sxs-lookup"><span data-stu-id="f47b0-189">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="f47b0-190">@No__t_2 öğesinin `Match` özniteliğine atayabilmeniz için iki `NavLinkMatch` seçeneği vardır:</span><span class="sxs-lookup"><span data-stu-id="f47b0-190">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="f47b0-191">`NavLinkMatch.All` &ndash; `NavLink` geçerli URL ile eşleştiğinde etkin olur.</span><span class="sxs-lookup"><span data-stu-id="f47b0-191">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="f47b0-192">`NavLinkMatch.Prefix` (*varsayılan*) &ndash;, geçerli URL 'nin herhangi bir önekiyle eşleştiğinde `NavLink` etkin olur.</span><span class="sxs-lookup"><span data-stu-id="f47b0-192">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="f47b0-193">Yukarıdaki örnekte, `NavLink` `href=""` giriş URL 'siyle eşleşir ve yalnızca uygulamanın varsayılan temel yol URL 'sindeki `active` CSS sınıfını alır (örneğin, `https://localhost:5001/`).</span><span class="sxs-lookup"><span data-stu-id="f47b0-193">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="f47b0-194">İkinci `NavLink`, Kullanıcı `MyComponent` ön ekine sahip herhangi bir URL 'YI ziyaret ettiğinde `active` sınıfını alır (örneğin, `https://localhost:5001/MyComponent` ve `https://localhost:5001/MyComponent/AnotherSegment`).</span><span class="sxs-lookup"><span data-stu-id="f47b0-194">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="f47b0-195">Ek `NavLink` bileşen öznitelikleri, işlenen tutturucu etiketine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="f47b0-195">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="f47b0-196">Aşağıdaki örnekte, `NavLink` bileşeni `target` özniteliğini içerir:</span><span class="sxs-lookup"><span data-stu-id="f47b0-196">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="f47b0-197">Aşağıdaki HTML biçimlendirmesi işlenir:</span><span class="sxs-lookup"><span data-stu-id="f47b0-197">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="f47b0-198">URI ve gezinti durumu yardımcıları</span><span class="sxs-lookup"><span data-stu-id="f47b0-198">URI and navigation state helpers</span></span>

<span data-ttu-id="f47b0-199">URI ile çalışmak için `Microsoft.AspNetCore.Components.NavigationManager` kullanın ve C# kodda gezinme yapın.</span><span class="sxs-lookup"><span data-stu-id="f47b0-199">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="f47b0-200">`NavigationManager`, aşağıdaki tabloda gösterilen olay ve yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="f47b0-200">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="f47b0-201">Üye</span><span class="sxs-lookup"><span data-stu-id="f47b0-201">Member</span></span> | <span data-ttu-id="f47b0-202">Açıklama</span><span class="sxs-lookup"><span data-stu-id="f47b0-202">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="f47b0-203">Geçerli mutlak URI 'yi alır.</span><span class="sxs-lookup"><span data-stu-id="f47b0-203">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="f47b0-204">Mutlak bir URI oluşturmak için göreli URI yollarına eklenebilir olan temel URI 'yi (sondaki eğik çizgiyle birlikte) alır.</span><span class="sxs-lookup"><span data-stu-id="f47b0-204">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="f47b0-205">Genellikle `BaseUri`, belgenin *Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_host. cshtml* (Blazor Server) içindeki `<base>` öğesinde `href` özniteliğine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="f47b0-205">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> |
| `NavigateTo` | <span data-ttu-id="f47b0-206">Belirtilen URI 'ye gider.</span><span class="sxs-lookup"><span data-stu-id="f47b0-206">Navigates to the specified URI.</span></span> <span data-ttu-id="f47b0-207">@No__t_0 `true`:</span><span class="sxs-lookup"><span data-stu-id="f47b0-207">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="f47b0-208">İstemci tarafı yönlendirme atlanır.</span><span class="sxs-lookup"><span data-stu-id="f47b0-208">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="f47b0-209">Bu tarayıcı, URI 'nin normalde istemci tarafı yönlendirici tarafından işlenip işlenmediğini sunucudan yeni sayfayı yüklemeye zorlanır.</span><span class="sxs-lookup"><span data-stu-id="f47b0-209">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="f47b0-210">Gezinti konumu değiştiğinde harekete gelen bir olay.</span><span class="sxs-lookup"><span data-stu-id="f47b0-210">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="f47b0-211">Göreli bir URI 'yi mutlak bir URI 'ye dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="f47b0-211">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="f47b0-212">Bir temel URI (örneğin, daha önce `GetBaseUri` tarafından döndürülen bir URI) verildiğinde, mutlak bir URI 'yi taban URI ön ekine göre bir URI 'ye dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="f47b0-212">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="f47b0-213">Aşağıdaki bileşen, düğme seçildiğinde uygulamanın `Counter` bileşenine gider:</span><span class="sxs-lookup"><span data-stu-id="f47b0-213">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

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
