---
title: Razor bileşenleri yönlendirme
author: guardrex
description: Uygulamalar ve NavLink bileşenle ilgili istekleri yönlendirmeyi öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/01/2019
uid: razor-components/routing
ms.openlocfilehash: 5c648ba1bb3846f5baa515e808a98a3b33f81438
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159468"
---
# <a name="razor-components-routing"></a><span data-ttu-id="4066d-103">Razor bileşenleri yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4066d-103">Razor Components routing</span></span>

<span data-ttu-id="4066d-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4066d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4066d-105">Uygulamalar ve NavLink bileşenle ilgili istekleri yönlendirmeyi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="4066d-105">Learn how to route requests in apps and about the NavLink component.</span></span>

<span data-ttu-id="4066d-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="4066d-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="4066d-107">Bkz: [başlama](xref:razor-components/get-started) Önkoşullar için konu.</span><span class="sxs-lookup"><span data-stu-id="4066d-107">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

## <a name="route-templates"></a><span data-ttu-id="4066d-108">Rota şablonlarının</span><span class="sxs-lookup"><span data-stu-id="4066d-108">Route templates</span></span>

<span data-ttu-id="4066d-109">`<Router>` Bileşen yönlendirme sağlar ve erişilebilir her bileşeni için bir rota şablonu sağlanır.</span><span class="sxs-lookup"><span data-stu-id="4066d-109">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="4066d-110">`<Router>` Bileşeni görünür *App.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="4066d-110">The `<Router>` component appears in the *App.cshtml* file:</span></span>

```cshtml
<Router AppAssembly=typeof(Program).Assembly />
```

<span data-ttu-id="4066d-111">Olduğunda bir  *\*.cshtml* ile dosya bir `@page` yönergesi derlendiğinde, oluşturulan sınıfın belirli bir [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) belirten rota şablonu.</span><span class="sxs-lookup"><span data-stu-id="4066d-111">When a *\*.cshtml* file with an `@page` directive is compiled, the generated class is given a [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) specifying the route template.</span></span> <span data-ttu-id="4066d-112">Çalışma zamanında bileşen sınıfları ile yönlendirici arar bir `RouteAttribute` ve hangi bileşen istenen URL ile eşleşen bir rota şablonuna sahip işler.</span><span class="sxs-lookup"><span data-stu-id="4066d-112">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="4066d-113">Bir bileşenin birden çok yol şablonu uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="4066d-113">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="4066d-114">İçinde [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), aşağıdaki bileşen isteklerine yanıt veren `/BlazorRoute` ve `/DifferentBlazorRoute`.</span><span class="sxs-lookup"><span data-stu-id="4066d-114">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), the following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`.</span></span>

<span data-ttu-id="4066d-115">*Pages/BlazorRoute.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="4066d-115">*Pages/BlazorRoute.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

<span data-ttu-id="4066d-116">`<Router>` İstenen yol, işleme için bir geri dönüş bileşen ayarı destekler çözülmüş değildir.</span><span class="sxs-lookup"><span data-stu-id="4066d-116">`<Router>` supports setting a fallback component for rendering when a requested route isn't resolved.</span></span> <span data-ttu-id="4066d-117">Bu katılımı ayarlayarak senaryoyu `FallbackComponent` geri dönüş bileşen sınıfı türü parametresi.</span><span class="sxs-lookup"><span data-stu-id="4066d-117">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="4066d-118">Aşağıdaki örnek, bir bileşen içinde tanımlanan ayarlar *Pages/MyFallbackRazorComponent.cshtml* geri dönüş bileşeni için bir `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="4066d-118">The following example sets a component defined in *Pages/MyFallbackRazorComponent.cshtml* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="4066d-119">Yollar düzgün bir şekilde oluşturmak için uygulamayı içermelidir bir `<base>` içindeki kendi *wwwroot/index.html* belirtilen uygulama temel yolu dosyasıyla `href` özniteliği (`<base href="/" />`).</span><span class="sxs-lookup"><span data-stu-id="4066d-119">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/" />`).</span></span> <span data-ttu-id="4066d-120">Daha fazla bilgi için [konak ve dağıtın: Uygulama temel yolu](xref:host-and-deploy/razor-components/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="4066d-120">For more information, see [Host and deploy: App base path](xref:host-and-deploy/razor-components/index#app-base-path).</span></span>

## <a name="route-parameters"></a><span data-ttu-id="4066d-121">Yol parametreleri</span><span class="sxs-lookup"><span data-stu-id="4066d-121">Route parameters</span></span>

<span data-ttu-id="4066d-122">Yönlendirici, aynı adı (büyük küçük harfe duyarlı) karşılık gelen bileşen parametrelerle doldurmak için rota parametrelerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="4066d-122">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive).</span></span>

<span data-ttu-id="4066d-123">*Pages/RouteParameter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="4066d-123">*Pages/RouteParameter.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=8)]

<span data-ttu-id="4066d-124">İsteğe bağlı parametreler henüz desteklenmeyen böylece iki `@page` yönergeleri, yukarıdaki örnekte uygulanır.</span><span class="sxs-lookup"><span data-stu-id="4066d-124">Optional parameters aren't supported yet, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="4066d-125">İlk Gezinti parametresi olmadan bileşenine izin verir.</span><span class="sxs-lookup"><span data-stu-id="4066d-125">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="4066d-126">İkinci `@page` yönergesi gereken `{text}` rota parametresi ve değeri atar `Text` özelliği.</span><span class="sxs-lookup"><span data-stu-id="4066d-126">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="4066d-127">Rota kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="4066d-127">Route constraints</span></span>

<span data-ttu-id="4066d-128">Türü bir bileşeni için bir yol kesimi üzerinde eşleşen bir rota kısıtlaması zorlar.</span><span class="sxs-lookup"><span data-stu-id="4066d-128">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="4066d-129">Aşağıdaki örnekte, kullanıcılar bileşen yolu yalnızca, eşleşen:</span><span class="sxs-lookup"><span data-stu-id="4066d-129">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="4066d-130">Bir `Id` yol kesimi istek URL'si hakkındaki varsa.</span><span class="sxs-lookup"><span data-stu-id="4066d-130">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="4066d-131">`Id` Segmenttir tamsayı (`int`).</span><span class="sxs-lookup"><span data-stu-id="4066d-131">The `Id` segment is an integer (`int`).</span></span>

```cshtml
@page "/Users/{Id:int}"

<h1>The user Id is @Id!</h1>

@functions {
    [Parameter]
    private int Id { get; set; }
}
```

<span data-ttu-id="4066d-132">Aşağıdaki tabloda gösterilen rota kısıtlamalarını kullanılabilir duruma gelir.</span><span class="sxs-lookup"><span data-stu-id="4066d-132">The route constraints shown in the following table are available for use.</span></span> <span data-ttu-id="4066d-133">Sabit kültür ile eşleşen rota kısıtlamaları için daha fazla bilgi için tablonun altındaki bir uyarı görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="4066d-133">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="4066d-134">Kısıtlama</span><span class="sxs-lookup"><span data-stu-id="4066d-134">Constraint</span></span> | <span data-ttu-id="4066d-135">Örnek</span><span class="sxs-lookup"><span data-stu-id="4066d-135">Example</span></span>           | <span data-ttu-id="4066d-136">Örnek eşleşmeleri</span><span class="sxs-lookup"><span data-stu-id="4066d-136">Example Matches</span></span>                                                                  | <span data-ttu-id="4066d-137">Değişmez değer</span><span class="sxs-lookup"><span data-stu-id="4066d-137">Invariant</span></span><br><span data-ttu-id="4066d-138">kültür</span><span class="sxs-lookup"><span data-stu-id="4066d-138">culture</span></span><br><span data-ttu-id="4066d-139">eşleştirme</span><span class="sxs-lookup"><span data-stu-id="4066d-139">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="4066d-140">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="4066d-140">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="4066d-141">Hayır</span><span class="sxs-lookup"><span data-stu-id="4066d-141">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="4066d-142">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="4066d-142">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="4066d-143">Evet</span><span class="sxs-lookup"><span data-stu-id="4066d-143">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="4066d-144">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="4066d-144">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="4066d-145">Evet</span><span class="sxs-lookup"><span data-stu-id="4066d-145">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="4066d-146">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="4066d-146">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="4066d-147">Evet</span><span class="sxs-lookup"><span data-stu-id="4066d-147">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="4066d-148">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="4066d-148">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="4066d-149">Evet</span><span class="sxs-lookup"><span data-stu-id="4066d-149">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="4066d-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="4066d-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="4066d-151">Hayır</span><span class="sxs-lookup"><span data-stu-id="4066d-151">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="4066d-152">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="4066d-152">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="4066d-153">Evet</span><span class="sxs-lookup"><span data-stu-id="4066d-153">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="4066d-154">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="4066d-154">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="4066d-155">Evet</span><span class="sxs-lookup"><span data-stu-id="4066d-155">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="4066d-156">Rota kısıtlamalarını URL'yi doğrulayın ve CLR türüne dönüştürülür (gibi `int` veya `DateTime`) her zaman sabit kültürü kullanır.</span><span class="sxs-lookup"><span data-stu-id="4066d-156">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="4066d-157">Bu kısıtlamalar, URL yerelleştirilemeyen olduğunu varsayın.</span><span class="sxs-lookup"><span data-stu-id="4066d-157">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="4066d-158">NavLink bileşeni</span><span class="sxs-lookup"><span data-stu-id="4066d-158">NavLink component</span></span>

<span data-ttu-id="4066d-159">Bir NavLink bileşeni yerine HTML kullanan  **\<bir >** gezinme bağlantıları oluşturulurken öğeleri.</span><span class="sxs-lookup"><span data-stu-id="4066d-159">Use a NavLink component in place of HTML **\<a>** elements when creating navigation links.</span></span> <span data-ttu-id="4066d-160">NavLink bileşeni gibi davranan bir  **\<bir >** öğesi, onu değiştirir dışında bir `active` CSS sınıfı bağlı kendi `href` geçerli URL ile eşleşen.</span><span class="sxs-lookup"><span data-stu-id="4066d-160">A NavLink component behaves like an **\<a>** element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="4066d-161">`active` Sınıfı, bir kullanıcının hangi sayfa etkin sayfa görüntülenen gezinti bağlantıları arasında olduğunu anlamak yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="4066d-161">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="4066d-162">NavMenu bileşeni [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) oluşturur bir [önyükleme](https://getbootstrap.com/docs/) gezinti çubuğunda, NavLink bileşenlerinin nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="4066d-162">The NavMenu component in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) creates a [Bootstrap](https://getbootstrap.com/docs/) nav bar that demonstrates how to use NavLink components.</span></span> <span data-ttu-id="4066d-163">Aşağıdaki biçimlendirme içinde ilk iki NavLinks gösterir *Shared/NavMenu.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="4066d-163">The following markup shows the first two NavLinks in the *Shared/NavMenu.cshtml* file.</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?start=13&end=24&highlight=4-6,9-11)]

<span data-ttu-id="4066d-164">İki `NavLinkMatch` seçenekleri:</span><span class="sxs-lookup"><span data-stu-id="4066d-164">There are two `NavLinkMatch` options:</span></span>

* <span data-ttu-id="4066d-165">`NavLinkMatch.All` &ndash; Tüm geçerli URL eşleştiğinde NavLink etkin olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="4066d-165">`NavLinkMatch.All` &ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* <span data-ttu-id="4066d-166">`NavLinkMatch.Prefix` &ndash; Geçerli URL herhangi bir önek eşleştiğinde NavLink etkin olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="4066d-166">`NavLinkMatch.Prefix` &ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="4066d-167">Yukarıdaki örnekte, giriş NavLink (`href=""`) tüm URL'lerle eşleşir ve her zaman alan `active` CSS sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4066d-167">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="4066d-168">İkinci NavLink yalnızca alan `active` sınıfı kullanıcı BlazorRoute bileşen ziyaret ettiğinde (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="4066d-168">The second NavLink only receives the `active` class when the user visits the BlazorRoute component (`href="BlazorRoute"`).</span></span>
