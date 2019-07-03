---
title: ASP.NET Core Blazor yönlendirme
author: guardrex
description: Uygulamalar ve NavLink bileşenle ilgili istekleri yönlendirmeyi öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/routing
ms.openlocfilehash: d2f0ce608d7368871f508754d7bbe4f75cc9701f
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538525"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="20118-103">ASP.NET Core Blazor yönlendirme</span><span class="sxs-lookup"><span data-stu-id="20118-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="20118-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="20118-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="20118-105">İstek yönlendirme hakkında ve nasıl kullanılacağını öğrenin `NavLink` Blazor uygulamalarda gezinme bağlantıları oluşturmak için bileşen.</span><span class="sxs-lookup"><span data-stu-id="20118-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="20118-106">ASP.NET Core uç noktası yönlendirme tümleştirmesi</span><span class="sxs-lookup"><span data-stu-id="20118-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="20118-107">Sunucu tarafı Blazor bütünleştirilmiştir [ASP.NET Core uç noktası yönlendirme](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="20118-107">Blazor server-side is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="20118-108">ASP.NET Core uygulaması ile etkileşimli bileşenleri için gelen bağlantıları kabul edecek şekilde yapılandırılmış `MapBlazorHub` içinde `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="20118-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a><span data-ttu-id="20118-109">Rota şablonlarının</span><span class="sxs-lookup"><span data-stu-id="20118-109">Route templates</span></span>

<span data-ttu-id="20118-110">`Router` Bileşen yönlendirme sağlar ve erişilebilir her bileşeni için bir rota şablonu sağlanır.</span><span class="sxs-lookup"><span data-stu-id="20118-110">The `Router` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="20118-111">`Router` Bileşeni görünür *App.razor* dosyası:</span><span class="sxs-lookup"><span data-stu-id="20118-111">The `Router` component appears in the *App.razor* file:</span></span>

<span data-ttu-id="20118-112">Blazor sunucu tarafı uygulamasında:</span><span class="sxs-lookup"><span data-stu-id="20118-112">In a Blazor server-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly" />
```

<span data-ttu-id="20118-113">Bir Blazor istemci-tarafı uygulaması:</span><span class="sxs-lookup"><span data-stu-id="20118-113">In a Blazor client-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

<span data-ttu-id="20118-114">Olduğunda bir *.razor* ile dosya bir `@page` yönergesi derlendiğinde, oluşturulan sınıfın sağlanan bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> belirten rota şablonu.</span><span class="sxs-lookup"><span data-stu-id="20118-114">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="20118-115">Çalışma zamanında bileşen sınıfları ile yönlendirici arar bir `RouteAttribute` ve istenen URL ile eşleşen bir rota şablonuyla bileşeni işler.</span><span class="sxs-lookup"><span data-stu-id="20118-115">At runtime, the router looks for component classes with a `RouteAttribute` and renders the component with a route template that matches the requested URL.</span></span>

<span data-ttu-id="20118-116">Bir bileşenin birden çok yol şablonu uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="20118-116">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="20118-117">Aşağıdaki bileşen isteklerine yanıt veren `/BlazorRoute` ve `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="20118-117">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="20118-118">Yollar düzgün bir şekilde oluşturmak için uygulamayı içermelidir bir `<base>` içindeki kendi *wwwroot/index.html* (Blazor istemci-tarafı) dosya veya *Pages/_Host.cshtml* dosyası (Blazor sunucu-tarafı) ile uygulama temel yolu Belirtilen `href` özniteliği (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="20118-118">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor client-side) or *Pages/_Host.cshtml* file (Blazor server-side) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="20118-119">Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/client-side#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="20118-119">For more information, see <xref:host-and-deploy/blazor/client-side#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="20118-120">İçerik bulunamadığında, özel içerik sağlayın</span><span class="sxs-lookup"><span data-stu-id="20118-120">Provide custom content when content isn't found</span></span>

<span data-ttu-id="20118-121">`Router` Bileşen, uygulama içeriği için istenen yol bulunamazsa özel içeriği belirtmek sağlar.</span><span class="sxs-lookup"><span data-stu-id="20118-121">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="20118-122">İçinde *App.razor* dosyası içinde özel içerik kümesi `<NotFoundContent>` öğesinin `Router` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="20118-122">In the *App.razor* file, set custom content in the `<NotFoundContent>` element of the `Router` component:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <NotFoundContent>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFoundContent>
</Router>
```

<span data-ttu-id="20118-123">İçeriği `<NotFoundContent>` etkileşimli diğer bileşenler gibi rastgele öğeler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="20118-123">The content of `<NotFoundContent>` can include arbitrary items, such as other interactive components.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="20118-124">Yol parametreleri</span><span class="sxs-lookup"><span data-stu-id="20118-124">Route parameters</span></span>

<span data-ttu-id="20118-125">Yönlendirici, aynı adı (büyük küçük harfe duyarlı) karşılık gelen bileşen parametrelerle doldurmak için rota parametreleri kullanır:</span><span class="sxs-lookup"><span data-stu-id="20118-125">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="20118-126">İsteğe bağlı parametreler, ASP.NET Core 3.0 Önizleme Blazor uygulamalar için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="20118-126">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="20118-127">İki `@page` yönergeleri, önceki örnekte uygulanır.</span><span class="sxs-lookup"><span data-stu-id="20118-127">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="20118-128">İlk Gezinti parametresi olmadan bileşenine izin verir.</span><span class="sxs-lookup"><span data-stu-id="20118-128">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="20118-129">İkinci `@page` yönergesi gereken `{text}` rota parametresi ve değeri atar `Text` özelliği.</span><span class="sxs-lookup"><span data-stu-id="20118-129">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="20118-130">Rota kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="20118-130">Route constraints</span></span>

<span data-ttu-id="20118-131">Türü bir bileşeni için bir yol kesimi üzerinde eşleşen bir rota kısıtlaması zorlar.</span><span class="sxs-lookup"><span data-stu-id="20118-131">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="20118-132">Aşağıdaki örnekte, rotaya `Users` bileşen yalnızca eşleşip eşleşmediğini:</span><span class="sxs-lookup"><span data-stu-id="20118-132">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="20118-133">Bir `Id` yol kesimi istek URL'si hakkındaki varsa.</span><span class="sxs-lookup"><span data-stu-id="20118-133">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="20118-134">`Id` Segmenttir tamsayı (`int`).</span><span class="sxs-lookup"><span data-stu-id="20118-134">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="20118-135">Aşağıdaki tabloda gösterilen rota kısıtlamalarını kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="20118-135">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="20118-136">Sabit kültür ile eşleşen rota kısıtlamaları için daha fazla bilgi için tablonun altındaki bir uyarı görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="20118-136">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="20118-137">Kısıtlama</span><span class="sxs-lookup"><span data-stu-id="20118-137">Constraint</span></span> | <span data-ttu-id="20118-138">Örnek</span><span class="sxs-lookup"><span data-stu-id="20118-138">Example</span></span>           | <span data-ttu-id="20118-139">Örnek eşleşmeleri</span><span class="sxs-lookup"><span data-stu-id="20118-139">Example Matches</span></span>                                                                  | <span data-ttu-id="20118-140">Değişmez değer</span><span class="sxs-lookup"><span data-stu-id="20118-140">Invariant</span></span><br><span data-ttu-id="20118-141">kültür</span><span class="sxs-lookup"><span data-stu-id="20118-141">culture</span></span><br><span data-ttu-id="20118-142">eşleştirme</span><span class="sxs-lookup"><span data-stu-id="20118-142">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="20118-143">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="20118-143">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="20118-144">Hayır</span><span class="sxs-lookup"><span data-stu-id="20118-144">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="20118-145">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="20118-145">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="20118-146">Evet</span><span class="sxs-lookup"><span data-stu-id="20118-146">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="20118-147">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="20118-147">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="20118-148">Evet</span><span class="sxs-lookup"><span data-stu-id="20118-148">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="20118-149">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="20118-149">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="20118-150">Evet</span><span class="sxs-lookup"><span data-stu-id="20118-150">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="20118-151">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="20118-151">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="20118-152">Evet</span><span class="sxs-lookup"><span data-stu-id="20118-152">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="20118-153">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="20118-153">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="20118-154">Hayır</span><span class="sxs-lookup"><span data-stu-id="20118-154">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="20118-155">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="20118-155">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="20118-156">Evet</span><span class="sxs-lookup"><span data-stu-id="20118-156">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="20118-157">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="20118-157">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="20118-158">Evet</span><span class="sxs-lookup"><span data-stu-id="20118-158">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="20118-159">Rota kısıtlamalarını URL'yi doğrulayın ve CLR türüne dönüştürülür (gibi `int` veya `DateTime`) her zaman sabit kültürü kullanır.</span><span class="sxs-lookup"><span data-stu-id="20118-159">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="20118-160">Bu kısıtlamalar, URL yerelleştirilemeyen olduğunu varsayın.</span><span class="sxs-lookup"><span data-stu-id="20118-160">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="20118-161">NavLink bileşeni</span><span class="sxs-lookup"><span data-stu-id="20118-161">NavLink component</span></span>

<span data-ttu-id="20118-162">Kullanım bir `NavLink` HTML Köprü öğeleri yerine bileşen (`<a>`) gezinme bağlantıları oluşturulurken.</span><span class="sxs-lookup"><span data-stu-id="20118-162">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="20118-163">A `NavLink` bileşeni davranacağını gibi bir `<a>` öğesi, onu değiştirir dışında bir `active` CSS sınıfı bağlı kendi `href` geçerli URL ile eşleşen.</span><span class="sxs-lookup"><span data-stu-id="20118-163">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="20118-164">`active` Sınıfı, bir kullanıcının hangi sayfa etkin sayfa görüntülenen gezinti bağlantıları arasında olduğunu anlamak yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="20118-164">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="20118-165">Aşağıdaki `NavMenu` bileşeni oluşturur bir [önyükleme](https://getbootstrap.com/docs/) nasıl kullanılacağını gösteren bir gezinti çubuğu `NavLink` bileşenler:</span><span class="sxs-lookup"><span data-stu-id="20118-165">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="20118-166">İki `NavLinkMatch` atayabileceğiniz seçenekleri `Match` özniteliği `<NavLink>` öğesi:</span><span class="sxs-lookup"><span data-stu-id="20118-166">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="20118-167">`NavLinkMatch.All` &ndash; `NavLink` Tüm geçerli URL eşleştiğinde etkindir.</span><span class="sxs-lookup"><span data-stu-id="20118-167">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="20118-168">`NavLinkMatch.Prefix` (*varsayılan*) &ndash; `NavLink` geçerli URL herhangi bir önek eşleştiğinde etkindir.</span><span class="sxs-lookup"><span data-stu-id="20118-168">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="20118-169">Yukarıdaki örnekte, giriş `NavLink` `href=""` giriş URL ile eşleşen ve yalnızca alan `active` CSS sınıfı uygulamanın varsayılan temel yol URL'si (örneğin, `https://localhost:5001/`).</span><span class="sxs-lookup"><span data-stu-id="20118-169">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="20118-170">İkinci `NavLink` alır `active` kullanıcı herhangi bir URL ile ziyaret ettiğinde sınıfı bir `MyComponent` önek (örneğin, `https://localhost:5001/MyComponent` ve `https://localhost:5001/MyComponent/AnotherSegment`).</span><span class="sxs-lookup"><span data-stu-id="20118-170">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="20118-171">URI ve gezinti durumu Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="20118-171">URI and navigation state helpers</span></span>

<span data-ttu-id="20118-172">Kullanım `Microsoft.AspNetCore.Components.IUriHelper` gezintisi ve bir URI'leri ile çalışmak için C# kod.</span><span class="sxs-lookup"><span data-stu-id="20118-172">Use `Microsoft.AspNetCore.Components.IUriHelper` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="20118-173">`IUriHelper` Olay ve aşağıdaki tabloda gösterilen yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="20118-173">`IUriHelper` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="20118-174">Üye</span><span class="sxs-lookup"><span data-stu-id="20118-174">Member</span></span> | <span data-ttu-id="20118-175">Açıklama</span><span class="sxs-lookup"><span data-stu-id="20118-175">Description</span></span> |
| ------ | ----------- |
| `GetAbsoluteUri` | <span data-ttu-id="20118-176">Geçerli bir mutlak URI alır.</span><span class="sxs-lookup"><span data-stu-id="20118-176">Gets the current absolute URI.</span></span> |
| `GetBaseUri` | <span data-ttu-id="20118-177">(Eğik ile) bir mutlak URI oluşturmak için göreli URI yolları başına temel URI'sini alır.</span><span class="sxs-lookup"><span data-stu-id="20118-177">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="20118-178">Genellikle, `GetBaseUri` karşılık gelen `href` belgenin özniteliği `<base>` öğesinde *wwwroot/index.html* (Blazor istemci-tarafı) veya *Pages/_Host.cshtml* () Blazor sunucu tarafı).</span><span class="sxs-lookup"><span data-stu-id="20118-178">Typically, `GetBaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor client-side) or *Pages/_Host.cshtml* (Blazor server-side).</span></span> |
| `NavigateTo` | <span data-ttu-id="20118-179">Belirtilen URI'ye gider.</span><span class="sxs-lookup"><span data-stu-id="20118-179">Navigates to the specified URI.</span></span> <span data-ttu-id="20118-180">Varsa `forceLoad` olduğu `true`:</span><span class="sxs-lookup"><span data-stu-id="20118-180">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="20118-181">İstemci tarafı yönlendirmesi atlanır.</span><span class="sxs-lookup"><span data-stu-id="20118-181">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="20118-182">URI genellikle istemci tarafı yönlendirici tarafından işlenen olup olmadığını tarayıcı sunucusundan yeni sayfa yükleme zorlanır.</span><span class="sxs-lookup"><span data-stu-id="20118-182">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `OnLocationChanged` | <span data-ttu-id="20118-183">Gezinti konumu değiştirildiğinde ateşlenir olay.</span><span class="sxs-lookup"><span data-stu-id="20118-183">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="20118-184">Bir göreli URİ'yi mutlak bir URI dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="20118-184">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="20118-185">Belirtilen temel URI (örneğin, bir URI daha önce döndürülen tarafından `GetBaseUri`), temel URI'si ön ek göreli bir URI mutlak URİ'ye dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="20118-185">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="20118-186">Uygulamanın aşağıdaki bileşen gider `Counter` düğme seçildiğinde bileşeni:</span><span class="sxs-lookup"><span data-stu-id="20118-186">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

```cshtml
@page "/navigate"
@using Microsoft.AspNetCore.Components
@inject IUriHelper UriHelper

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="@NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        UriHelper.NavigateTo("counter");
    }
}
```
