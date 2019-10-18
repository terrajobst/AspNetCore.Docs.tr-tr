---
title: ASP.NET Core Blazor yönlendirme
author: guardrex
description: Uygulamalardaki istekleri yönlendirme ve gezinti bağlantısı bileşeni hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/routing
ms.openlocfilehash: d9f81c8aa2cf07f8bfaede65efcb7328088f55b9
ms.sourcegitcommit: ce2bfb01f2cc7dd83f8a97da0689d232c71bcdc4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72531142"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="a56a1-103">ASP.NET Core Blazor yönlendirme</span><span class="sxs-lookup"><span data-stu-id="a56a1-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="a56a1-104">[Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="a56a1-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="a56a1-105">İsteklerin nasıl yönlendirileceğini ve Blazor uygulamalarında gezinti bağlantıları oluşturmak için `NavLink` bileşeninin nasıl kullanılacağını öğrenin.</span><span class="sxs-lookup"><span data-stu-id="a56a1-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="a56a1-106">Uç nokta yönlendirme tümleştirmesi ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a56a1-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="a56a1-107">Blazor sunucusu [ASP.NET Core uç nokta yönlendirme](xref:fundamentals/routing)ile tümleşiktir.</span><span class="sxs-lookup"><span data-stu-id="a56a1-107">Blazor Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="a56a1-108">ASP.NET Core bir uygulama, `Startup.Configure` ' de `MapBlazorHub` ile etkileşimli bileşenlere yönelik gelen bağlantıları kabul edecek şekilde yapılandırılmıştır:</span><span class="sxs-lookup"><span data-stu-id="a56a1-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="a56a1-109">En yaygın yapılandırma, tüm istekleri Razor sayfasına yönlendirmesidir ve bu, Blazor Server uygulamasının sunucu tarafı bölümü için ana bilgisayar görevi görür.</span><span class="sxs-lookup"><span data-stu-id="a56a1-109">The most typical configuration is to route all requests to a Razor page, which acts as the host for the server-side part of the Blazor Server app.</span></span> <span data-ttu-id="a56a1-110">Kural gereği, *ana bilgisayar* sayfası genellikle *_host. cshtml*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="a56a1-110">By convention, the *host* page is usually named *_Host.cshtml*.</span></span> <span data-ttu-id="a56a1-111">Ana bilgisayar dosyasında belirtilen yol, yol eşleştirilirken düşük bir öncelik ile çalıştığından bir *geri dönüş yolu* olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="a56a1-111">The route specified in the host file is called a *fallback route* because it operates with a low priority in route matching.</span></span> <span data-ttu-id="a56a1-112">Geri dönüş yolu, diğer yollar eşleşmediği zaman kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="a56a1-112">The fallback route is considered when other routes don't match.</span></span> <span data-ttu-id="a56a1-113">Bu, uygulamanın Blazor sunucu uygulamasıyla kesintiye uğramadan diğer denetleyicileri ve sayfaları kullanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="a56a1-113">This allows the app to use others controllers and pages without interfering with the Blazor Server app.</span></span>

## <a name="route-templates"></a><span data-ttu-id="a56a1-114">Rota şablonları</span><span class="sxs-lookup"><span data-stu-id="a56a1-114">Route templates</span></span>

<span data-ttu-id="a56a1-115">@No__t-0 bileşeni, belirtilen bir rota ile her bileşene yönlendirmeyi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a56a1-115">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="a56a1-116">@No__t-0 bileşeni *app. Razor* dosyasında görünür:</span><span class="sxs-lookup"><span data-stu-id="a56a1-116">The `Router` component appears in the *App.razor* file:</span></span>

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

<span data-ttu-id="a56a1-117">@No__t-1 yönergesi içeren bir *. Razor* dosyası derlendiğinde, oluşturulan sınıf, yol şablonunu belirten bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="a56a1-117">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="a56a1-118">Çalışma zamanında `RouteView` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="a56a1-118">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="a56a1-119">@No__t-1 ' den istenen parametrelerle birlikte `RouteData` alır.</span><span class="sxs-lookup"><span data-stu-id="a56a1-119">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="a56a1-120">Belirtilen parametreleri kullanarak belirtilen bileşeni düzeniyle (veya isteğe bağlı bir varsayılan düzende) işler.</span><span class="sxs-lookup"><span data-stu-id="a56a1-120">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="a56a1-121">İsteğe bağlı olarak, düzen belirtmeyen bileşenler için kullanılacak düzen sınıfıyla birlikte `DefaultLayout` parametresini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a56a1-121">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="a56a1-122">Varsayılan Blazor şablonları `MainLayout` bileşenini belirtir.</span><span class="sxs-lookup"><span data-stu-id="a56a1-122">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="a56a1-123">*Mainlayout. Razor* , şablon projenin *paylaşılan* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="a56a1-123">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="a56a1-124">Düzenler hakkında daha fazla bilgi için bkz. <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="a56a1-124">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="a56a1-125">Birden çok yol şablonu, bir bileşene uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="a56a1-125">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="a56a1-126">Aşağıdaki bileşen `/BlazorRoute` ve `/DifferentBlazorRoute` isteklerine yanıt verir:</span><span class="sxs-lookup"><span data-stu-id="a56a1-126">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="a56a1-127">URL 'Lerin doğru şekilde çözülmesi için, uygulamanın, `href` özniteliğinde (`<base href="/">`) belirtilen uygulama temel yolu ile *Wwwroot/index.html* dosyasına (Blazor WebAssembly) veya *Pages/_host. cshtml* dosyasına (Blazor Server) bir `<base>` etiketi içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="a56a1-127">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="a56a1-128">Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="a56a1-128">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="a56a1-129">İçerik bulunamadığında özel içerik sağla</span><span class="sxs-lookup"><span data-stu-id="a56a1-129">Provide custom content when content isn't found</span></span>

<span data-ttu-id="a56a1-130">@No__t-0 bileşeni, istenen rota için içerik bulunmazsa uygulamanın özel içerik belirtmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="a56a1-130">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="a56a1-131">*App. Razor* dosyasında, `Router` bileşeninin `NotFound` Şablon parametresinde özel içerik ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="a56a1-131">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

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

<span data-ttu-id="a56a1-132">@No__t-0 etiketlerinin içeriği, diğer etkileşimli bileşenler gibi rastgele öğeler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="a56a1-132">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="a56a1-133">@No__t-0 içeriğine varsayılan bir düzen uygulamak için bkz. <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="a56a1-133">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="a56a1-134">Birden çok derlemeden bileşenlere rota</span><span class="sxs-lookup"><span data-stu-id="a56a1-134">Route to components from multiple assemblies</span></span>

<span data-ttu-id="a56a1-135">@No__t-1 bileşeninin yönlendirilebilir bileşenleri ararken dikkate alınması için ek derlemeler belirtmek üzere `AdditionalAssemblies` parametresini kullanın.</span><span class="sxs-lookup"><span data-stu-id="a56a1-135">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="a56a1-136">Belirtilen derlemeler @no__t -0-belirtilen derlemeye ek olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="a56a1-136">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="a56a1-137">Aşağıdaki örnekte, `Component1`, başvurulan bir sınıf kitaplığında tanımlanan yönlendirilebilir bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="a56a1-137">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="a56a1-138">Aşağıdaki `AdditionalAssemblies` örneği, `Component1` için yönlendirme desteğiyle sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="a56a1-138">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

```cshtml
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a><span data-ttu-id="a56a1-139">Rota parametreleri</span><span class="sxs-lookup"><span data-stu-id="a56a1-139">Route parameters</span></span>

<span data-ttu-id="a56a1-140">Yönlendirici, karşılık gelen bileşen parametrelerini aynı ada (büyük/küçük harfe duyarsız) doldurmak için yol parametrelerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="a56a1-140">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="a56a1-141">ASP.NET Core 3,0 ' deki Blazor uygulamaları için isteğe bağlı parametreler desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="a56a1-141">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0.</span></span> <span data-ttu-id="a56a1-142">Önceki örnekte iki `@page` yönergesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a56a1-142">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="a56a1-143">İlki, bir parametre olmadan bileşene gezinmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="a56a1-143">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="a56a1-144">İkinci `@page` yönergesi `{text}` yol parametresini alır ve değeri `Text` özelliğine atar.</span><span class="sxs-lookup"><span data-stu-id="a56a1-144">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="a56a1-145">Yol kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="a56a1-145">Route constraints</span></span>

<span data-ttu-id="a56a1-146">Yol kısıtlaması bir yönlendirme segmentinde bir bileşene tür eşleştirmeyi zorlar.</span><span class="sxs-lookup"><span data-stu-id="a56a1-146">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="a56a1-147">Aşağıdaki örnekte, `Users` bileşeninin yolu yalnızca şu durumlarda eşleşir:</span><span class="sxs-lookup"><span data-stu-id="a56a1-147">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="a56a1-148">İstek URL 'sinde bir `Id` yol kesimi var.</span><span class="sxs-lookup"><span data-stu-id="a56a1-148">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="a56a1-149">@No__t-0 segmenti bir tamsayıdır (`int`).</span><span class="sxs-lookup"><span data-stu-id="a56a1-149">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="a56a1-150">Aşağıdaki tabloda gösterilen yol kısıtlamaları mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="a56a1-150">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="a56a1-151">Sabit kültür ile eşleşen yol kısıtlamaları için daha fazla bilgi için tablonun altındaki uyarıya bakın.</span><span class="sxs-lookup"><span data-stu-id="a56a1-151">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="a56a1-152">Kısıtlaması</span><span class="sxs-lookup"><span data-stu-id="a56a1-152">Constraint</span></span> | <span data-ttu-id="a56a1-153">Örnek</span><span class="sxs-lookup"><span data-stu-id="a56a1-153">Example</span></span>           | <span data-ttu-id="a56a1-154">Örnek eşleşmeler</span><span class="sxs-lookup"><span data-stu-id="a56a1-154">Example Matches</span></span>                                                                  | <span data-ttu-id="a56a1-155">Bilmesi</span><span class="sxs-lookup"><span data-stu-id="a56a1-155">Invariant</span></span><br><span data-ttu-id="a56a1-156">kültür</span><span class="sxs-lookup"><span data-stu-id="a56a1-156">culture</span></span><br><span data-ttu-id="a56a1-157">eşleştirme</span><span class="sxs-lookup"><span data-stu-id="a56a1-157">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="a56a1-158">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="a56a1-158">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="a56a1-159">Hayır</span><span class="sxs-lookup"><span data-stu-id="a56a1-159">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="a56a1-160">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="a56a1-160">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="a56a1-161">Evet</span><span class="sxs-lookup"><span data-stu-id="a56a1-161">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="a56a1-162">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="a56a1-162">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="a56a1-163">Evet</span><span class="sxs-lookup"><span data-stu-id="a56a1-163">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="a56a1-164">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="a56a1-164">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="a56a1-165">Evet</span><span class="sxs-lookup"><span data-stu-id="a56a1-165">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="a56a1-166">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="a56a1-166">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="a56a1-167">Evet</span><span class="sxs-lookup"><span data-stu-id="a56a1-167">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="a56a1-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="a56a1-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="a56a1-169">Hayır</span><span class="sxs-lookup"><span data-stu-id="a56a1-169">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="a56a1-170">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="a56a1-170">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="a56a1-171">Evet</span><span class="sxs-lookup"><span data-stu-id="a56a1-171">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="a56a1-172">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="a56a1-172">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="a56a1-173">Evet</span><span class="sxs-lookup"><span data-stu-id="a56a1-173">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="a56a1-174">URL 'YI doğrulayan ve bir CLR türüne (`int` veya `DateTime`) dönüştürülen yol kısıtlamaları her zaman sabit kültürü kullanır.</span><span class="sxs-lookup"><span data-stu-id="a56a1-174">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="a56a1-175">Bu kısıtlamalar, URL 'nin yerelleştirilemeyen olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="a56a1-175">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="a56a1-176">Noktalar içeren URL 'lerle yönlendirme</span><span class="sxs-lookup"><span data-stu-id="a56a1-176">Routing with URLs that contain dots</span></span>

<span data-ttu-id="a56a1-177">Blazor Server uygulamalarında, *_Host. cshtml* içindeki varsayılan yol `/` (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="a56a1-177">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="a56a1-178">URL bir dosya isteyecek şekilde göründüğünden, nokta (`.`) içeren bir istek URL 'SI varsayılan yol tarafından eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="a56a1-178">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="a56a1-179">Bir Blazor uygulaması mevcut olmayan bir statik dosya için *404-Found* yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="a56a1-179">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="a56a1-180">Bir nokta içeren yolları kullanmak için *_Host. cshtml* 'yi aşağıdaki yol şablonuyla yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="a56a1-180">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="a56a1-181">@No__t-0 şablonu şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="a56a1-181">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="a56a1-182">Çift yıldız işareti (`**`), eğik çizgi (`/`) kodlaması olmadan birden çok klasör sınırlarındaki *yolu yakalar.*</span><span class="sxs-lookup"><span data-stu-id="a56a1-182">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="a56a1-183">@No__t-0 rota parametresi adı.</span><span class="sxs-lookup"><span data-stu-id="a56a1-183">A `path` route parameter name.</span></span>

<span data-ttu-id="a56a1-184">Daha fazla bilgi için bkz. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="a56a1-184">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="a56a1-185">Gezinti bağlantısı bileşeni</span><span class="sxs-lookup"><span data-stu-id="a56a1-185">NavLink component</span></span>

<span data-ttu-id="a56a1-186">Gezinti bağlantıları oluştururken, HTML köprü öğelerinin yerine `NavLink` bileşeni kullanın (`<a>`).</span><span class="sxs-lookup"><span data-stu-id="a56a1-186">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="a56a1-187">@No__t-0 bileşeni, `href` ' in geçerli URL ile eşleşip eşleşmediğini temel alan `active` CSS sınıfına geçiş yaparken, bir `<a>` öğesi gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="a56a1-187">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="a56a1-188">@No__t-0 sınıfı, bir kullanıcının hangi sayfanın etkin sayfa olduğunu anlamasına yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="a56a1-188">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="a56a1-189">Aşağıdaki `NavMenu` bileşeni, `NavLink` bileşenlerinin nasıl kullanılacağını gösteren bir [önyükleme](https://getbootstrap.com/docs/) gezinti çubuğu oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a56a1-189">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="a56a1-190">@No__t-2 öğesinin `Match` özniteliğine atayabilmeniz için iki `NavLinkMatch` seçeneği vardır:</span><span class="sxs-lookup"><span data-stu-id="a56a1-190">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="a56a1-191">`NavLinkMatch.All` &ndash; `NavLink` geçerli URL ile eşleştiğinde etkin olur.</span><span class="sxs-lookup"><span data-stu-id="a56a1-191">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="a56a1-192">`NavLinkMatch.Prefix` (*varsayılan*) &ndash;, geçerli URL 'nin herhangi bir önekiyle eşleştiğinde `NavLink` etkin olur.</span><span class="sxs-lookup"><span data-stu-id="a56a1-192">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="a56a1-193">Yukarıdaki örnekte, `NavLink` `href=""` giriş URL 'siyle eşleşir ve yalnızca uygulamanın varsayılan temel yol URL 'sindeki `active` CSS sınıfını alır (örneğin, `https://localhost:5001/`).</span><span class="sxs-lookup"><span data-stu-id="a56a1-193">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="a56a1-194">İkinci `NavLink`, Kullanıcı `MyComponent` ön ekine sahip herhangi bir URL 'YI ziyaret ettiğinde `active` sınıfını alır (örneğin, `https://localhost:5001/MyComponent` ve `https://localhost:5001/MyComponent/AnotherSegment`).</span><span class="sxs-lookup"><span data-stu-id="a56a1-194">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="a56a1-195">Ek `NavLink` bileşen öznitelikleri, işlenen tutturucu etiketine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a56a1-195">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="a56a1-196">Aşağıdaki örnekte, `NavLink` bileşeni `target` özniteliğini içerir:</span><span class="sxs-lookup"><span data-stu-id="a56a1-196">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="a56a1-197">Aşağıdaki HTML biçimlendirmesi işlenir:</span><span class="sxs-lookup"><span data-stu-id="a56a1-197">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="a56a1-198">URI ve gezinti durumu yardımcıları</span><span class="sxs-lookup"><span data-stu-id="a56a1-198">URI and navigation state helpers</span></span>

<span data-ttu-id="a56a1-199">URI ile çalışmak için `Microsoft.AspNetCore.Components.NavigationManager` kullanın ve C# kodda gezinme yapın.</span><span class="sxs-lookup"><span data-stu-id="a56a1-199">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="a56a1-200">`NavigationManager`, aşağıdaki tabloda gösterilen olay ve yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="a56a1-200">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="a56a1-201">Üye</span><span class="sxs-lookup"><span data-stu-id="a56a1-201">Member</span></span> | <span data-ttu-id="a56a1-202">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a56a1-202">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="a56a1-203">Geçerli mutlak URI 'yi alır.</span><span class="sxs-lookup"><span data-stu-id="a56a1-203">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="a56a1-204">Mutlak bir URI oluşturmak için göreli URI yollarına eklenebilir olan temel URI 'yi (sondaki eğik çizgiyle birlikte) alır.</span><span class="sxs-lookup"><span data-stu-id="a56a1-204">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="a56a1-205">Genellikle `BaseUri`, belgenin *Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_host. cshtml* (Blazor Server) içindeki `<base>` öğesinde `href` özniteliğine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a56a1-205">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> |
| `NavigateTo` | <span data-ttu-id="a56a1-206">Belirtilen URI 'ye gider.</span><span class="sxs-lookup"><span data-stu-id="a56a1-206">Navigates to the specified URI.</span></span> <span data-ttu-id="a56a1-207">@No__t-0 `true` ise:</span><span class="sxs-lookup"><span data-stu-id="a56a1-207">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="a56a1-208">İstemci tarafı yönlendirme atlanır.</span><span class="sxs-lookup"><span data-stu-id="a56a1-208">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="a56a1-209">Bu tarayıcı, URI 'nin normalde istemci tarafı yönlendirici tarafından işlenip işlenmediğini sunucudan yeni sayfayı yüklemeye zorlanır.</span><span class="sxs-lookup"><span data-stu-id="a56a1-209">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="a56a1-210">Gezinti konumu değiştiğinde harekete gelen bir olay.</span><span class="sxs-lookup"><span data-stu-id="a56a1-210">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="a56a1-211">Göreli bir URI 'yi mutlak bir URI 'ye dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="a56a1-211">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="a56a1-212">Bir temel URI (örneğin, daha önce `GetBaseUri` tarafından döndürülen bir URI) verildiğinde, mutlak bir URI 'yi taban URI ön ekine göre bir URI 'ye dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="a56a1-212">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="a56a1-213">Aşağıdaki bileşen, düğme seçildiğinde uygulamanın `Counter` bileşenine gider:</span><span class="sxs-lookup"><span data-stu-id="a56a1-213">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

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
