---
title: Razor bileşenleri yönlendirme
author: guardrex
description: Uygulamalar ve NavLink bileşenle ilgili istekleri yönlendirmeyi öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: razor-components/routing
ms.openlocfilehash: 39039c306a0ac0d9838e3c98815a6b1aade8863b
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978364"
---
# <a name="razor-components-routing"></a><span data-ttu-id="9428c-103">Razor bileşenleri yönlendirme</span><span class="sxs-lookup"><span data-stu-id="9428c-103">Razor Components routing</span></span>

<span data-ttu-id="9428c-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9428c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9428c-105">Uygulamalar ve NavLink bileşenle ilgili istekleri yönlendirmeyi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="9428c-105">Learn how to route requests in apps and about the NavLink component.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="9428c-106">ASP.NET Core uç noktası yönlendirme tümleştirmesi</span><span class="sxs-lookup"><span data-stu-id="9428c-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="9428c-107">Razor bileşenleri tümleştirilir [ASP.NET Core yönlendirme](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="9428c-107">Razor Components are integrated into [ASP.NET Core routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="9428c-108">ASP.NET Core uygulaması ile etkileşimli Razor bileşenleri için gelen bağlantıları kabul edecek şekilde yapılandırılmış `MapComponentHub<TComponent>` içinde `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9428c-108">An ASP.NET Core app is configured to accept incoming connections for interactive Razor Components with `MapComponentHub<TComponent>` in `Startup.Configure`.</span></span> <span data-ttu-id="9428c-109">`MapComponentHub` belirten kök bileşeni `App` Seçici eşleşen bir DOM öğesi içinde işleneceğini `app`:</span><span class="sxs-lookup"><span data-stu-id="9428c-109">`MapComponentHub` specifies that the root component `App` should be rendered within a DOM element matching the selector `app`:</span></span>

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapComponentHub<App>("app");
});
```

## <a name="route-templates"></a><span data-ttu-id="9428c-110">Rota şablonlarının</span><span class="sxs-lookup"><span data-stu-id="9428c-110">Route templates</span></span>

<span data-ttu-id="9428c-111">`<Router>` Bileşen yönlendirme sağlar ve erişilebilir her bileşeni için bir rota şablonu sağlanır.</span><span class="sxs-lookup"><span data-stu-id="9428c-111">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="9428c-112">`<Router>` Bileşeni görünür *Components/App.razor* dosyası:</span><span class="sxs-lookup"><span data-stu-id="9428c-112">The `<Router>` component appears in the *Components/App.razor* file:</span></span>

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

<span data-ttu-id="9428c-113">Olduğunda bir *.razor* veya *.cshtml* ile dosya bir `@page` yönergesi derlendiğinde, oluşturulan sınıfın belirli bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> belirten rota şablonu.</span><span class="sxs-lookup"><span data-stu-id="9428c-113">When a *.razor* or *.cshtml* file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="9428c-114">Çalışma zamanında bileşen sınıfları ile yönlendirici arar bir `RouteAttribute` ve hangi bileşen istenen URL ile eşleşen bir rota şablonuna sahip işler.</span><span class="sxs-lookup"><span data-stu-id="9428c-114">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="9428c-115">Bir bileşenin birden çok yol şablonu uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="9428c-115">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="9428c-116">Aşağıdaki bileşen isteklerine yanıt veren `/BlazorRoute` ve `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="9428c-116">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute&highlight=1-2)]

<span data-ttu-id="9428c-117">`<Router>` İstenen yol, işleme için bir geri dönüş bileşen ayarı destekler çözülmüş değildir.</span><span class="sxs-lookup"><span data-stu-id="9428c-117">`<Router>` supports setting a fallback component for rendering when a requested route isn't resolved.</span></span> <span data-ttu-id="9428c-118">Bu katılımı ayarlayarak senaryoyu `FallbackComponent` geri dönüş bileşen sınıfı türü parametresi.</span><span class="sxs-lookup"><span data-stu-id="9428c-118">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="9428c-119">Aşağıdaki örnek, bir bileşen içinde tanımlanan ayarlar *Pages/MyFallbackRazorComponent.cshtml* geri dönüş bileşeni için bir `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="9428c-119">The following example sets a component defined in *Pages/MyFallbackRazorComponent.cshtml* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="9428c-120">Yollar düzgün bir şekilde oluşturmak için uygulamayı içermelidir bir `<base>` içindeki kendi *wwwroot/index.html* belirtilen uygulama temel yolu dosyasıyla `href` özniteliği (`<base href="/" />`).</span><span class="sxs-lookup"><span data-stu-id="9428c-120">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/" />`).</span></span> <span data-ttu-id="9428c-121">Daha fazla bilgi için bkz. <xref:host-and-deploy/razor-components/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="9428c-121">For more information, see <xref:host-and-deploy/razor-components/index#app-base-path>.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="9428c-122">Yol parametreleri</span><span class="sxs-lookup"><span data-stu-id="9428c-122">Route parameters</span></span>

<span data-ttu-id="9428c-123">Yönlendirici, aynı adı (büyük küçük harfe duyarlı) karşılık gelen bileşen parametrelerle doldurmak için rota parametreleri kullanır:</span><span class="sxs-lookup"><span data-stu-id="9428c-123">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="9428c-124">İsteğe bağlı parametreler henüz desteklenmeyen böylece iki `@page` yönergeleri, yukarıdaki örnekte uygulanır.</span><span class="sxs-lookup"><span data-stu-id="9428c-124">Optional parameters aren't supported yet, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="9428c-125">İlk Gezinti parametresi olmadan bileşenine izin verir.</span><span class="sxs-lookup"><span data-stu-id="9428c-125">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="9428c-126">İkinci `@page` yönergesi gereken `{text}` rota parametresi ve değeri atar `Text` özelliği.</span><span class="sxs-lookup"><span data-stu-id="9428c-126">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="9428c-127">Rota kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="9428c-127">Route constraints</span></span>

<span data-ttu-id="9428c-128">Türü bir bileşeni için bir yol kesimi üzerinde eşleşen bir rota kısıtlaması zorlar.</span><span class="sxs-lookup"><span data-stu-id="9428c-128">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="9428c-129">Aşağıdaki örnekte, kullanıcılar bileşen yolu yalnızca, eşleşen:</span><span class="sxs-lookup"><span data-stu-id="9428c-129">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="9428c-130">Bir `Id` yol kesimi istek URL'si hakkındaki varsa.</span><span class="sxs-lookup"><span data-stu-id="9428c-130">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="9428c-131">`Id` Segmenttir tamsayı (`int`).</span><span class="sxs-lookup"><span data-stu-id="9428c-131">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.cshtml?highlight=1)]

<span data-ttu-id="9428c-132">Aşağıdaki tabloda gösterilen rota kısıtlamalarını kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9428c-132">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="9428c-133">Sabit kültür ile eşleşen rota kısıtlamaları için daha fazla bilgi için tablonun altındaki bir uyarı görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="9428c-133">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="9428c-134">Kısıtlama</span><span class="sxs-lookup"><span data-stu-id="9428c-134">Constraint</span></span> | <span data-ttu-id="9428c-135">Örnek</span><span class="sxs-lookup"><span data-stu-id="9428c-135">Example</span></span>           | <span data-ttu-id="9428c-136">Örnek eşleşmeleri</span><span class="sxs-lookup"><span data-stu-id="9428c-136">Example Matches</span></span>                                                                  | <span data-ttu-id="9428c-137">Değişmez değer</span><span class="sxs-lookup"><span data-stu-id="9428c-137">Invariant</span></span><br><span data-ttu-id="9428c-138">kültür</span><span class="sxs-lookup"><span data-stu-id="9428c-138">culture</span></span><br><span data-ttu-id="9428c-139">eşleştirme</span><span class="sxs-lookup"><span data-stu-id="9428c-139">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="9428c-140">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="9428c-140">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="9428c-141">Hayır</span><span class="sxs-lookup"><span data-stu-id="9428c-141">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="9428c-142">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="9428c-142">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="9428c-143">Evet</span><span class="sxs-lookup"><span data-stu-id="9428c-143">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="9428c-144">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="9428c-144">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="9428c-145">Evet</span><span class="sxs-lookup"><span data-stu-id="9428c-145">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="9428c-146">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="9428c-146">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="9428c-147">Evet</span><span class="sxs-lookup"><span data-stu-id="9428c-147">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="9428c-148">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="9428c-148">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="9428c-149">Evet</span><span class="sxs-lookup"><span data-stu-id="9428c-149">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="9428c-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="9428c-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="9428c-151">Hayır</span><span class="sxs-lookup"><span data-stu-id="9428c-151">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="9428c-152">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="9428c-152">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="9428c-153">Evet</span><span class="sxs-lookup"><span data-stu-id="9428c-153">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="9428c-154">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="9428c-154">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="9428c-155">Evet</span><span class="sxs-lookup"><span data-stu-id="9428c-155">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="9428c-156">Rota kısıtlamalarını URL'yi doğrulayın ve CLR türüne dönüştürülür (gibi `int` veya `DateTime`) her zaman sabit kültürü kullanır.</span><span class="sxs-lookup"><span data-stu-id="9428c-156">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="9428c-157">Bu kısıtlamalar, URL yerelleştirilemeyen olduğunu varsayın.</span><span class="sxs-lookup"><span data-stu-id="9428c-157">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="9428c-158">NavLink bileşeni</span><span class="sxs-lookup"><span data-stu-id="9428c-158">NavLink component</span></span>

<span data-ttu-id="9428c-159">Bir NavLink bileşeni yerine HTML kullanan `<a>` gezinme bağlantıları oluşturulurken öğeleri.</span><span class="sxs-lookup"><span data-stu-id="9428c-159">Use a NavLink component in place of HTML `<a>` elements when creating navigation links.</span></span> <span data-ttu-id="9428c-160">NavLink bileşeni gibi davranan bir `<a>` öğesi, onu değiştirir dışında bir `active` CSS sınıfı bağlı kendi `href` geçerli URL ile eşleşen.</span><span class="sxs-lookup"><span data-stu-id="9428c-160">A NavLink component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="9428c-161">`active` Sınıfı, bir kullanıcının hangi sayfa etkin sayfa görüntülenen gezinti bağlantıları arasında olduğunu anlamak yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="9428c-161">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="9428c-162">Aşağıdaki NavMenu bileşeni oluşturur bir [önyükleme](https://getbootstrap.com/docs/) gezinti çubuğunda, NavLink bileşenleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="9428c-162">The following NavMenu component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use NavLink components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?name=snippet_NavLinks&highlight=4-6,9-11)]

<span data-ttu-id="9428c-163">İki `NavLinkMatch` seçenekleri:</span><span class="sxs-lookup"><span data-stu-id="9428c-163">There are two `NavLinkMatch` options:</span></span>

* <span data-ttu-id="9428c-164">`NavLinkMatch.All` &ndash; Tüm geçerli URL eşleştiğinde NavLink etkin olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="9428c-164">`NavLinkMatch.All` &ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* <span data-ttu-id="9428c-165">`NavLinkMatch.Prefix` &ndash; Geçerli URL herhangi bir önek eşleştiğinde NavLink etkin olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="9428c-165">`NavLinkMatch.Prefix` &ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="9428c-166">Yukarıdaki örnekte, giriş NavLink (`href=""`) tüm URL'lerle eşleşir ve her zaman alan `active` CSS sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9428c-166">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="9428c-167">İkinci NavLink yalnızca alan `active` sınıfı kullanıcı Blazor rota bileşen ziyaret ettiğinde (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="9428c-167">The second NavLink only receives the `active` class when the user visits the Blazor Route component (`href="BlazorRoute"`).</span></span>
