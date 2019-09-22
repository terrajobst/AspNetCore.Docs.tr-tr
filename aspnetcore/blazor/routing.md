---
title: ASP.NET Core Blazor yönlendirme
author: guardrex
description: Uygulamalardaki istekleri yönlendirme ve gezinti bağlantısı bileşeni hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2019
uid: blazor/routing
ms.openlocfilehash: d6fb3f03be94ff99ac3ed434265e6cd6b752c625
ms.sourcegitcommit: 04ce94b3c1b01d167f30eed60c1c95446dfe759d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/21/2019
ms.locfileid: "71176393"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="a7cc2-103">ASP.NET Core Blazor yönlendirme</span><span class="sxs-lookup"><span data-stu-id="a7cc2-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="a7cc2-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a7cc2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="a7cc2-105">İsteklerin nasıl yönlendirileceğini ve Blazor uygulamalarında gezinti bağlantıları oluşturmak için `NavLink` bileşenin nasıl kullanılacağını öğrenin.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="a7cc2-106">Uç nokta yönlendirme tümleştirmesi ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a7cc2-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="a7cc2-107">Blazor sunucusu [ASP.NET Core uç nokta yönlendirme](xref:fundamentals/routing)ile tümleşiktir.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-107">Blazor Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="a7cc2-108">ASP.NET Core bir uygulama, `MapBlazorHub` içindeki `Startup.Configure`etkileşimli bileşenler için gelen bağlantıları kabul edecek şekilde yapılandırılmıştır:</span><span class="sxs-lookup"><span data-stu-id="a7cc2-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="a7cc2-109">En yaygın yapılandırma, tüm istekleri Razor sayfasına yönlendirmesidir ve bu, Blazor Server uygulamasının sunucu tarafı bölümü için ana bilgisayar görevi görür.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-109">The most typical configuration is to route all requests to a Razor page, which acts as the host for the server-side part of the Blazor Server app.</span></span> <span data-ttu-id="a7cc2-110">Kural gereği, *ana bilgisayar* sayfası genellikle *_host. cshtml*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-110">By convention, the *host* page is usually named *_Host.cshtml*.</span></span> <span data-ttu-id="a7cc2-111">Ana bilgisayar dosyasında belirtilen yol, yol eşleştirilirken düşük bir öncelik ile çalıştığından bir *geri dönüş yolu* olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-111">The route specified in the host file is called a *fallback route* because it operates with a low priority in route matching.</span></span> <span data-ttu-id="a7cc2-112">Geri dönüş yolu, diğer yollar eşleşmediği zaman kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-112">The fallback route is considered when other routes don't match.</span></span> <span data-ttu-id="a7cc2-113">Bu, uygulamanın Blazor sunucu uygulamasıyla kesintiye uğramadan diğer denetleyicileri ve sayfaları kullanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-113">This allows the app to use others controllers and pages without interfering with the Blazor Server app.</span></span>

## <a name="route-templates"></a><span data-ttu-id="a7cc2-114">Rota şablonları</span><span class="sxs-lookup"><span data-stu-id="a7cc2-114">Route templates</span></span>

<span data-ttu-id="a7cc2-115">Bileşeni `Router` , belirtilen bir rota ile her bileşene yönlendirmeyi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-115">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="a7cc2-116">Bileşen App. Razor dosyasında görünür: `Router`</span><span class="sxs-lookup"><span data-stu-id="a7cc2-116">The `Router` component appears in the *App.razor* file:</span></span>

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

<span data-ttu-id="a7cc2-117">Bir`@page` yönergeyle bir *. Razor* dosyası derlendiğinde, oluşturulan sınıf, yol şablonunu belirten bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-117">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="a7cc2-118">Çalışma zamanında, `RouteView` bileşen:</span><span class="sxs-lookup"><span data-stu-id="a7cc2-118">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="a7cc2-119">' İ istediğiniz parametrelerle birlikte alır. `Router` `RouteData`</span><span class="sxs-lookup"><span data-stu-id="a7cc2-119">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="a7cc2-120">Belirtilen parametreleri kullanarak belirtilen bileşeni düzeniyle (veya isteğe bağlı bir varsayılan düzende) işler.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-120">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="a7cc2-121">İsteğe bağlı olarak, bir `DefaultLayout` düzen belirtmeyen bileşenler için kullanılacak düzen sınıfıyla bir parametre belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-121">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="a7cc2-122">Varsayılan Blazor şablonları `MainLayout` bileşeni belirler.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-122">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="a7cc2-123">*Mainlayout. Razor* , şablon projenin *paylaşılan* klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-123">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="a7cc2-124">Düzenler hakkında daha fazla bilgi için bkz <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-124">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="a7cc2-125">Birden çok yol şablonu, bir bileşene uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-125">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="a7cc2-126">Aşağıdaki bileşen ve `/BlazorRoute` `/DifferentBlazorRoute`için isteklere yanıt verir:</span><span class="sxs-lookup"><span data-stu-id="a7cc2-126">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="a7cc2-127">URL 'lerin doğru şekilde çözülmesi için `<base>` , uygulamanın, `href` özniteliğinde belirtilen uygulama temel yolu ile *Wwwroot/index.html* File (Blazor webassembly) veya *Pages/_host. cshtml* dosyasında (Blazor Server) bir etiketi içermesi gerekir (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="a7cc2-127">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="a7cc2-128">Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-128">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="a7cc2-129">İçerik bulunamadığında özel içerik sağla</span><span class="sxs-lookup"><span data-stu-id="a7cc2-129">Provide custom content when content isn't found</span></span>

<span data-ttu-id="a7cc2-130">`Router` Bileşen, istenen rota için içerik bulunmazsa uygulamanın özel içerik belirtmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-130">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="a7cc2-131">*App. Razor* dosyasında, `NotFound` `Router` bileşenin Şablon parametresinde özel içerik ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="a7cc2-131">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

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

<span data-ttu-id="a7cc2-132">`<NotFound>` Etiketlerin içeriği, diğer etkileşimli bileşenler gibi rastgele öğeler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-132">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="a7cc2-133">`NotFound` İçeriğe varsayılan bir düzen uygulamak için bkz <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-133">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="a7cc2-134">Birden çok derlemeden bileşenlere rota</span><span class="sxs-lookup"><span data-stu-id="a7cc2-134">Route to components from multiple assemblies</span></span>

<span data-ttu-id="a7cc2-135">Bileşen için yönlendirilebilir bileşenleri ararken dikkate alınması gereken ek derlemeleri belirtmek için parametresinikullanın.`AdditionalAssemblies` `Router`</span><span class="sxs-lookup"><span data-stu-id="a7cc2-135">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="a7cc2-136">Belirtilen derlemeler, `AppAssembly`belirtilen derlemeye ek olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-136">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="a7cc2-137">Aşağıdaki örnekte, `Component1` başvurulan bir sınıf kitaplığında tanımlanan yönlendirilebilir bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-137">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="a7cc2-138">Aşağıdaki `AdditionalAssemblies` örnek, için `Component1`yönlendirme desteği ile sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="a7cc2-138">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

<span data-ttu-id="a7cc2-139">< yönlendirici AppAssembly = "typeof (program). Derleme "AdditionalAssemblies =" New [] {typeof (Component1). Bütünleştirilmiş kod} >...</Router></span><span class="sxs-lookup"><span data-stu-id="a7cc2-139"><Router AppAssembly="typeof(Program).Assembly" AdditionalAssemblies="new[] { typeof(Component1).Assembly }> ... </Router></span></span>

## <a name="route-parameters"></a><span data-ttu-id="a7cc2-140">Rota parametreleri</span><span class="sxs-lookup"><span data-stu-id="a7cc2-140">Route parameters</span></span>

<span data-ttu-id="a7cc2-141">Yönlendirici, karşılık gelen bileşen parametrelerini aynı ada (büyük/küçük harfe duyarsız) doldurmak için yol parametrelerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="a7cc2-141">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="a7cc2-142">ASP.NET Core 3,0 önizlemesinde Blazor uygulamaları için isteğe bağlı parametreler desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-142">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="a7cc2-143">Önceki `@page` örnekte iki yönergeler uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-143">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="a7cc2-144">İlki, bir parametre olmadan bileşene gezinmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-144">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="a7cc2-145">İkinci `@page` yönerge, `{text}` yol parametresini alır ve değeri `Text` özelliğine atar.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-145">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="a7cc2-146">Yol kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="a7cc2-146">Route constraints</span></span>

<span data-ttu-id="a7cc2-147">Yol kısıtlaması bir yönlendirme segmentinde bir bileşene tür eşleştirmeyi zorlar.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-147">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="a7cc2-148">Aşağıdaki örnekte, `Users` bileşen yolu yalnızca şu durumlarda eşleşir:</span><span class="sxs-lookup"><span data-stu-id="a7cc2-148">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="a7cc2-149">İstek `Id` URL 'sinde bir yol kesimi var.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-149">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="a7cc2-150">Segment bir tamsayıdır (`int`). `Id`</span><span class="sxs-lookup"><span data-stu-id="a7cc2-150">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="a7cc2-151">Aşağıdaki tabloda gösterilen yol kısıtlamaları mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-151">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="a7cc2-152">Sabit kültür ile eşleşen yol kısıtlamaları için daha fazla bilgi için tablonun altındaki uyarıya bakın.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-152">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="a7cc2-153">Kısıtlaması</span><span class="sxs-lookup"><span data-stu-id="a7cc2-153">Constraint</span></span> | <span data-ttu-id="a7cc2-154">Örnek</span><span class="sxs-lookup"><span data-stu-id="a7cc2-154">Example</span></span>           | <span data-ttu-id="a7cc2-155">Örnek eşleşmeler</span><span class="sxs-lookup"><span data-stu-id="a7cc2-155">Example Matches</span></span>                                                                  | <span data-ttu-id="a7cc2-156">Bilmesi</span><span class="sxs-lookup"><span data-stu-id="a7cc2-156">Invariant</span></span><br><span data-ttu-id="a7cc2-157">kültür</span><span class="sxs-lookup"><span data-stu-id="a7cc2-157">culture</span></span><br><span data-ttu-id="a7cc2-158">eşleştirme</span><span class="sxs-lookup"><span data-stu-id="a7cc2-158">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="a7cc2-159">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="a7cc2-159">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="a7cc2-160">Hayır</span><span class="sxs-lookup"><span data-stu-id="a7cc2-160">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="a7cc2-161">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="a7cc2-161">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="a7cc2-162">Evet</span><span class="sxs-lookup"><span data-stu-id="a7cc2-162">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="a7cc2-163">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="a7cc2-163">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="a7cc2-164">Evet</span><span class="sxs-lookup"><span data-stu-id="a7cc2-164">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="a7cc2-165">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="a7cc2-165">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="a7cc2-166">Evet</span><span class="sxs-lookup"><span data-stu-id="a7cc2-166">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="a7cc2-167">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="a7cc2-167">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="a7cc2-168">Evet</span><span class="sxs-lookup"><span data-stu-id="a7cc2-168">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="a7cc2-169">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="a7cc2-169">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="a7cc2-170">Hayır</span><span class="sxs-lookup"><span data-stu-id="a7cc2-170">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="a7cc2-171">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="a7cc2-171">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="a7cc2-172">Evet</span><span class="sxs-lookup"><span data-stu-id="a7cc2-172">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="a7cc2-173">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="a7cc2-173">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="a7cc2-174">Evet</span><span class="sxs-lookup"><span data-stu-id="a7cc2-174">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="a7cc2-175">URL 'yi doğrulayan ve bir clr türüne ( `int` veya `DateTime`gibi) dönüştürülen yol kısıtlamaları, her zaman sabit kültürü kullanır.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-175">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="a7cc2-176">Bu kısıtlamalar, URL 'nin yerelleştirilemeyen olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-176">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="a7cc2-177">Noktalar içeren URL 'lerle yönlendirme</span><span class="sxs-lookup"><span data-stu-id="a7cc2-177">Routing with URLs that contain dots</span></span>

<span data-ttu-id="a7cc2-178">Blazor Server uygulamalarında, *_Host. cshtml* `/` içindeki varsayılan yol (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="a7cc2-178">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="a7cc2-179">URL bir dosya isteyecek şekilde göründüğünden, nokta`.`() içeren bir istek URL 'si varsayılan yol tarafından eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-179">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="a7cc2-180">Bir Blazor uygulaması mevcut olmayan bir statik dosya için *404-Found* yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-180">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="a7cc2-181">Bir nokta içeren yolları kullanmak için *_Host. cshtml* 'yi aşağıdaki yol şablonuyla yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="a7cc2-181">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="a7cc2-182">`"/{**path}"` Şablon şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="a7cc2-182">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="a7cc2-183">Ters eğik çizgi (`/`) kodlaması olmadan birden`**`çok klasör sınırlarındaki yolu yakalamak için çift yıldız *catch-all* söz dizimi ().</span><span class="sxs-lookup"><span data-stu-id="a7cc2-183">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="a7cc2-184">`path` Rota parametresi adı.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-184">A `path` route parameter name.</span></span>

<span data-ttu-id="a7cc2-185">Daha fazla bilgi için bkz. <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-185">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="a7cc2-186">Gezinti bağlantısı bileşeni</span><span class="sxs-lookup"><span data-stu-id="a7cc2-186">NavLink component</span></span>

<span data-ttu-id="a7cc2-187">Gezinti bağlantıları `NavLink` oluştururken, HTML köprü öğelerinin (`<a>`) yerine bir bileşen kullanın.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-187">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="a7cc2-188">Bir `NavLink` bileşen `<a>` ,geçerli`href` URL ile eşleşip eşleşmediğini temel alarak bir CSSsınıfınageçişyaptığısürecebiröğesigibidavranır.`active`</span><span class="sxs-lookup"><span data-stu-id="a7cc2-188">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="a7cc2-189">`active` Sınıfı, bir kullanıcının hangi sayfanın etkin sayfa olduğunu anladığı gezinti bağlantıları arasında yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-189">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="a7cc2-190">Aşağıdaki `NavMenu` bileşen, bileşenlerin nasıl [](https://getbootstrap.com/docs/) kullanılacağını `NavLink` gösteren bir önyükleme gezinti çubuğu oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a7cc2-190">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="a7cc2-191">Öğesinin özniteliğine atayabilmeniz için kullanabileceğiniz iki `NavLinkMatch` `Match`seçenekvardır `<NavLink>` :</span><span class="sxs-lookup"><span data-stu-id="a7cc2-191">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="a7cc2-192">`NavLinkMatch.All`Tüm geçerli URL ile eşleştiğinde etkin olur.`NavLink` &ndash;</span><span class="sxs-lookup"><span data-stu-id="a7cc2-192">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="a7cc2-193">`NavLinkMatch.Prefix`(*varsayılan*) Geçerli URL 'nin herhangi bir önekiyle eşleştiğinde etkin olur.`NavLink` &ndash;</span><span class="sxs-lookup"><span data-stu-id="a7cc2-193">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="a7cc2-194">`NavLink` Yukarıdaki örnekte, ana `href=""` giriş `active` URL 'siyle eşleşir ve yalnızca uygulamanın varsayılan temel yol URL 'sindeki CSS sınıfını alır (örneğin, `https://localhost:5001/`).</span><span class="sxs-lookup"><span data-stu-id="a7cc2-194">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="a7cc2-195">`NavLink` İkincisi, Kullanıcı ön `active` eki olan herhangi bir `MyComponent` URL 'yi ziyaret ettiğinde sınıfı alır (örneğin, `https://localhost:5001/MyComponent` ve `https://localhost:5001/MyComponent/AnotherSegment`).</span><span class="sxs-lookup"><span data-stu-id="a7cc2-195">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="a7cc2-196">Ek `NavLink` bileşen öznitelikleri, işlenen tutturucu etiketine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-196">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="a7cc2-197">Aşağıdaki örnekte, `NavLink` bileşen `target` özniteliğini içerir:</span><span class="sxs-lookup"><span data-stu-id="a7cc2-197">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="a7cc2-198">Aşağıdaki HTML biçimlendirmesi işlenir:</span><span class="sxs-lookup"><span data-stu-id="a7cc2-198">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="a7cc2-199">URI ve gezinti durumu yardımcıları</span><span class="sxs-lookup"><span data-stu-id="a7cc2-199">URI and navigation state helpers</span></span>

<span data-ttu-id="a7cc2-200">Kod `Microsoft.AspNetCore.Components.NavigationManager` içinde C# URI ve gezinme ile çalışmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-200">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="a7cc2-201">`NavigationManager`Aşağıdaki tabloda gösterilen olay ve yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-201">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="a7cc2-202">Üye</span><span class="sxs-lookup"><span data-stu-id="a7cc2-202">Member</span></span> | <span data-ttu-id="a7cc2-203">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a7cc2-203">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="a7cc2-204">Geçerli mutlak URI 'yi alır.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-204">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="a7cc2-205">Mutlak bir URI oluşturmak için göreli URI yollarına eklenebilir olan temel URI 'yi (sondaki eğik çizgiyle birlikte) alır.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-205">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="a7cc2-206">Genellikle, `BaseUri` *Wwwroot/index.html* (Blazor `href` webassembly) veya `<base>` *Pages/_host. cshtml* (Blazor Server) içindeki belge öğesindeki özniteliğe karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-206">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> |
| `NavigateTo` | <span data-ttu-id="a7cc2-207">Belirtilen URI 'ye gider.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-207">Navigates to the specified URI.</span></span> <span data-ttu-id="a7cc2-208">`forceLoad` Şu`true`ise:</span><span class="sxs-lookup"><span data-stu-id="a7cc2-208">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="a7cc2-209">İstemci tarafı yönlendirme atlanır.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-209">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="a7cc2-210">Bu tarayıcı, URI 'nin normalde istemci tarafı yönlendirici tarafından işlenip işlenmediğini sunucudan yeni sayfayı yüklemeye zorlanır.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-210">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="a7cc2-211">Gezinti konumu değiştiğinde harekete gelen bir olay.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-211">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="a7cc2-212">Göreli bir URI 'yi mutlak bir URI 'ye dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-212">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="a7cc2-213">Temel URI (örneğin, daha önce tarafından `GetBaseUri`döndürülen bir URI) verildiğinde, mutlak bir URI 'yi taban URI önekine göre bir URI 'ye dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="a7cc2-213">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="a7cc2-214">Aşağıdaki bileşen, düğme seçildiğinde uygulamanın `Counter` bileşenine gider:</span><span class="sxs-lookup"><span data-stu-id="a7cc2-214">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

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
