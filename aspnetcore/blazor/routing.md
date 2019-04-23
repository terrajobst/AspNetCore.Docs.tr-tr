---
title: Blazor yönlendirme
author: guardrex
description: Uygulamalar ve NavLink bileşenle ilgili istekleri yönlendirmeyi öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/18/2019
uid: blazor/routing
ms.openlocfilehash: d3356ceccd5a6ed3375b7eada9cac295ef7ad53b
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982868"
---
# <a name="blazor-routing"></a><span data-ttu-id="4e872-103">Blazor yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4e872-103">Blazor routing</span></span>

<span data-ttu-id="4e872-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4e872-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4e872-105">Uygulamalar ve NavLink bileşenle ilgili istekleri yönlendirmeyi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="4e872-105">Learn how to route requests in apps and about the NavLink component.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="4e872-106">ASP.NET Core uç noktası yönlendirme tümleştirmesi</span><span class="sxs-lookup"><span data-stu-id="4e872-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="4e872-107">Sunucu tarafı Blazor bütünleştirilmiştir [ASP.NET Core uç noktası yönlendirme](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="4e872-107">Blazor server-side is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="4e872-108">ASP.NET Core uygulaması ile etkileşimli bileşenleri için gelen bağlantıları kabul edecek şekilde yapılandırılmış `MapBlazorHub` içinde `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="4e872-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapBlazorHub();
    endpoints.MapFallbackToPage("/_Host");
});
```

## <a name="route-templates"></a><span data-ttu-id="4e872-109">Rota şablonlarının</span><span class="sxs-lookup"><span data-stu-id="4e872-109">Route templates</span></span>

<span data-ttu-id="4e872-110">`<Router>` Bileşen yönlendirme sağlar ve erişilebilir her bileşeni için bir rota şablonu sağlanır.</span><span class="sxs-lookup"><span data-stu-id="4e872-110">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="4e872-111">`<Router>` Bileşeni görünür *App.razor* dosyası:</span><span class="sxs-lookup"><span data-stu-id="4e872-111">The `<Router>` component appears in the *App.razor* file:</span></span>

<span data-ttu-id="4e872-112">Bir Blazor sunucu-tarafı uygulaması için:</span><span class="sxs-lookup"><span data-stu-id="4e872-112">For a Blazor server-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly" />
```

<span data-ttu-id="4e872-113">Bir Blazor istemci-tarafı uygulaması için:</span><span class="sxs-lookup"><span data-stu-id="4e872-113">For a Blazor client-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

<span data-ttu-id="4e872-114">Olduğunda bir *.razor* ile dosya bir `@page` yönergesi derlendiğinde, oluşturulan sınıfın belirli bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> belirten rota şablonu.</span><span class="sxs-lookup"><span data-stu-id="4e872-114">When a *.razor* file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="4e872-115">Çalışma zamanında bileşen sınıfları ile yönlendirici arar bir `RouteAttribute` ve hangi bileşen istenen URL ile eşleşen bir rota şablonuna sahip işler.</span><span class="sxs-lookup"><span data-stu-id="4e872-115">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="4e872-116">Bir bileşenin birden çok yol şablonu uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="4e872-116">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="4e872-117">Aşağıdaki bileşen isteklerine yanıt veren `/BlazorRoute` ve `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="4e872-117">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

<span data-ttu-id="4e872-118">`<Router>` İstenen yol, işleme için bir geri dönüş bileşen ayarı destekler çözülmüş değildir.</span><span class="sxs-lookup"><span data-stu-id="4e872-118">`<Router>` supports setting a fallback component for rendering when a requested route isn't resolved.</span></span> <span data-ttu-id="4e872-119">Bu katılımı ayarlayarak senaryoyu `FallbackComponent` geri dönüş bileşen sınıfı türü parametresi.</span><span class="sxs-lookup"><span data-stu-id="4e872-119">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="4e872-120">Aşağıdaki örnek, bir bileşen içinde tanımlanan ayarlar *Pages/MyFallbackRazorComponent.razor* geri dönüş bileşeni için bir `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="4e872-120">The following example sets a component defined in *Pages/MyFallbackRazorComponent.razor* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="4e872-121">Yollar düzgün bir şekilde oluşturmak için uygulamayı içermelidir bir `<base>` içindeki kendi *wwwroot/index.html* belirtilen uygulama temel yolu dosyasıyla `href` özniteliği (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="4e872-121">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="4e872-122">Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/client-side#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="4e872-122">For more information, see <xref:host-and-deploy/blazor/client-side#app-base-path>.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="4e872-123">Yol parametreleri</span><span class="sxs-lookup"><span data-stu-id="4e872-123">Route parameters</span></span>

<span data-ttu-id="4e872-124">Yönlendirici, aynı adı (büyük küçük harfe duyarlı) karşılık gelen bileşen parametrelerle doldurmak için rota parametreleri kullanır:</span><span class="sxs-lookup"><span data-stu-id="4e872-124">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="4e872-125">İsteğe bağlı parametreler henüz desteklenmeyen böylece iki `@page` yönergeleri, yukarıdaki örnekte uygulanır.</span><span class="sxs-lookup"><span data-stu-id="4e872-125">Optional parameters aren't supported yet, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="4e872-126">İlk Gezinti parametresi olmadan bileşenine izin verir.</span><span class="sxs-lookup"><span data-stu-id="4e872-126">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="4e872-127">İkinci `@page` yönergesi gereken `{text}` rota parametresi ve değeri atar `Text` özelliği.</span><span class="sxs-lookup"><span data-stu-id="4e872-127">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="4e872-128">Rota kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="4e872-128">Route constraints</span></span>

<span data-ttu-id="4e872-129">Türü bir bileşeni için bir yol kesimi üzerinde eşleşen bir rota kısıtlaması zorlar.</span><span class="sxs-lookup"><span data-stu-id="4e872-129">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="4e872-130">Aşağıdaki örnekte, kullanıcılar bileşen yolu yalnızca, eşleşen:</span><span class="sxs-lookup"><span data-stu-id="4e872-130">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="4e872-131">Bir `Id` yol kesimi istek URL'si hakkındaki varsa.</span><span class="sxs-lookup"><span data-stu-id="4e872-131">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="4e872-132">`Id` Segmenttir tamsayı (`int`).</span><span class="sxs-lookup"><span data-stu-id="4e872-132">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="4e872-133">Aşağıdaki tabloda gösterilen rota kısıtlamalarını kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4e872-133">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="4e872-134">Sabit kültür ile eşleşen rota kısıtlamaları için daha fazla bilgi için tablonun altındaki bir uyarı görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="4e872-134">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="4e872-135">Kısıtlama</span><span class="sxs-lookup"><span data-stu-id="4e872-135">Constraint</span></span> | <span data-ttu-id="4e872-136">Örnek</span><span class="sxs-lookup"><span data-stu-id="4e872-136">Example</span></span>           | <span data-ttu-id="4e872-137">Örnek eşleşmeleri</span><span class="sxs-lookup"><span data-stu-id="4e872-137">Example Matches</span></span>                                                                  | <span data-ttu-id="4e872-138">Değişmez değer</span><span class="sxs-lookup"><span data-stu-id="4e872-138">Invariant</span></span><br><span data-ttu-id="4e872-139">kültür</span><span class="sxs-lookup"><span data-stu-id="4e872-139">culture</span></span><br><span data-ttu-id="4e872-140">eşleştirme</span><span class="sxs-lookup"><span data-stu-id="4e872-140">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="4e872-141">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="4e872-141">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="4e872-142">Hayır</span><span class="sxs-lookup"><span data-stu-id="4e872-142">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="4e872-143">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="4e872-143">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="4e872-144">Evet</span><span class="sxs-lookup"><span data-stu-id="4e872-144">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="4e872-145">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="4e872-145">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="4e872-146">Evet</span><span class="sxs-lookup"><span data-stu-id="4e872-146">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="4e872-147">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="4e872-147">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="4e872-148">Evet</span><span class="sxs-lookup"><span data-stu-id="4e872-148">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="4e872-149">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="4e872-149">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="4e872-150">Evet</span><span class="sxs-lookup"><span data-stu-id="4e872-150">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="4e872-151">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="4e872-151">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="4e872-152">Hayır</span><span class="sxs-lookup"><span data-stu-id="4e872-152">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="4e872-153">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="4e872-153">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="4e872-154">Evet</span><span class="sxs-lookup"><span data-stu-id="4e872-154">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="4e872-155">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="4e872-155">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="4e872-156">Evet</span><span class="sxs-lookup"><span data-stu-id="4e872-156">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="4e872-157">Rota kısıtlamalarını URL'yi doğrulayın ve CLR türüne dönüştürülür (gibi `int` veya `DateTime`) her zaman sabit kültürü kullanır.</span><span class="sxs-lookup"><span data-stu-id="4e872-157">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="4e872-158">Bu kısıtlamalar, URL yerelleştirilemeyen olduğunu varsayın.</span><span class="sxs-lookup"><span data-stu-id="4e872-158">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="4e872-159">NavLink bileşeni</span><span class="sxs-lookup"><span data-stu-id="4e872-159">NavLink component</span></span>

<span data-ttu-id="4e872-160">Bir NavLink bileşeni yerine HTML kullanan `<a>` gezinme bağlantıları oluşturulurken öğeleri.</span><span class="sxs-lookup"><span data-stu-id="4e872-160">Use a NavLink component in place of HTML `<a>` elements when creating navigation links.</span></span> <span data-ttu-id="4e872-161">NavLink bileşeni gibi davranan bir `<a>` öğesi, onu değiştirir dışında bir `active` CSS sınıfı bağlı kendi `href` geçerli URL ile eşleşen.</span><span class="sxs-lookup"><span data-stu-id="4e872-161">A NavLink component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="4e872-162">`active` Sınıfı, bir kullanıcının hangi sayfa etkin sayfa görüntülenen gezinti bağlantıları arasında olduğunu anlamak yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="4e872-162">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="4e872-163">Aşağıdaki NavMenu bileşeni oluşturur bir [önyükleme](https://getbootstrap.com/docs/) gezinti çubuğunda, NavLink bileşenleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="4e872-163">The following NavMenu component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use NavLink components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.razor?name=snippet_NavLinks&highlight=4-6,9-11)]

<span data-ttu-id="4e872-164">İki `NavLinkMatch` seçenekleri:</span><span class="sxs-lookup"><span data-stu-id="4e872-164">There are two `NavLinkMatch` options:</span></span>

* <span data-ttu-id="4e872-165">`NavLinkMatch.All` &ndash; Tüm geçerli URL eşleştiğinde NavLink etkin olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="4e872-165">`NavLinkMatch.All` &ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* <span data-ttu-id="4e872-166">`NavLinkMatch.Prefix` &ndash; Geçerli URL herhangi bir önek eşleştiğinde NavLink etkin olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="4e872-166">`NavLinkMatch.Prefix` &ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="4e872-167">Yukarıdaki örnekte, giriş NavLink (`href=""`) tüm URL'lerle eşleşir ve her zaman alan `active` CSS sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4e872-167">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="4e872-168">İkinci NavLink yalnızca alan `active` sınıfı kullanıcı Blazor rota bileşen ziyaret ettiğinde (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="4e872-168">The second NavLink only receives the `active` class when the user visits the Blazor Route component (`href="BlazorRoute"`).</span></span>
