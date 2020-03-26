---
title: ASP.NET Core Razor bileşenleri oluşturma ve kullanma
author: guardrex
description: Veri bağlama, olayları işleme ve bileşen yaşam döngülerini yönetme dahil Razor bileşenleri oluşturmayı ve kullanmayı öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/25/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: bc1d07aef9cd60b89343a034168daa6754f4421b
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306504"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="28e46-103">ASP.NET Core Razor bileşenleri oluşturma ve kullanma</span><span class="sxs-lookup"><span data-stu-id="28e46-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="28e46-104">, [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="28e46-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="28e46-105">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="28e46-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

Blazor<span data-ttu-id="28e46-106"> uygulamalar, *bileşenleri*kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="28e46-106"> apps are built using *components*.</span></span> <span data-ttu-id="28e46-107">Bir bileşen, bir sayfa, iletişim veya form gibi bir kullanıcı arabirimi (UI) öbekidir.</span><span class="sxs-lookup"><span data-stu-id="28e46-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="28e46-108">Bir bileşen, veri eklemek veya UI olaylarına yanıt vermek için gereken HTML işaretlemesini ve işleme mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="28e46-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="28e46-109">Bileşenler esnek ve hafif.</span><span class="sxs-lookup"><span data-stu-id="28e46-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="28e46-110">Bunlar, iç içe geçmiş, yeniden kullanılabilir ve projeler arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="28e46-111">Bileşen sınıfları</span><span class="sxs-lookup"><span data-stu-id="28e46-111">Component classes</span></span>

<span data-ttu-id="28e46-112">Bileşenler, C# ve HTML Işaretlemesi kullanılarak [Razor](xref:mvc/views/razor) bileşen dosyalarında ( *. Razor*) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="28e46-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="28e46-113">Blazor bir bileşen, bir *Razor bileşeni*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="28e46-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="28e46-114">Bir bileşenin adı, büyük harfle başlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="28e46-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="28e46-115">Örneğin, *mycoolcomponent. Razor* geçerlidir ve *mycoolcomponent. Razor* geçersizdir.</span><span class="sxs-lookup"><span data-stu-id="28e46-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="28e46-116">Bir bileşen için Kullanıcı arabirimi HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="28e46-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="28e46-117">Dinamik işleme mantığı (örneğin, döngüler, koşullar, ifadeler) C# [Razor](xref:mvc/views/razor)adlı gömülü bir sözdizimi kullanılarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="28e46-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="28e46-118">Bir uygulama derlendiğinde, HTML biçimlendirme ve C# işleme mantığı bir bileşen sınıfına dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="28e46-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="28e46-119">Oluşturulan sınıfın adı, dosyanın adıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="28e46-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="28e46-120">Bileşen sınıfının üyeleri bir `@code` bloğunda tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="28e46-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="28e46-121">`@code` bloğunda, bileşen durumu (özellikler, alanlar) olay işleme yöntemleriyle veya diğer bileşen mantığını tanımlamaya yönelik yöntemlerle belirtilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="28e46-122">Birden fazla `@code` bloğu izin verilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-122">More than one `@code` block is permissible.</span></span>

<span data-ttu-id="28e46-123">Bileşen üyeleri, `@`ile başlayan ifadeleri kullanarak C# bileşenin işleme mantığının bir parçası olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-123">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="28e46-124">Örneğin bir C# alan, alan adına `@` önek olarak işlenerek işlenir.</span><span class="sxs-lookup"><span data-stu-id="28e46-124">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="28e46-125">Aşağıdaki örnek değerlendirilir ve işler:</span><span class="sxs-lookup"><span data-stu-id="28e46-125">The following example evaluates and renders:</span></span>

* <span data-ttu-id="28e46-126">`font-style`için CSS özellik değerine `_headingFontStyle`.</span><span class="sxs-lookup"><span data-stu-id="28e46-126">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="28e46-127">`<h1>` öğesinin içeriğine `_headingText`.</span><span class="sxs-lookup"><span data-stu-id="28e46-127">`_headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="28e46-128">Bileşen ilk olarak işlendikten sonra, bileşen işleme ağacını olaylara yanıt olarak yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="28e46-128">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> Blazor<span data-ttu-id="28e46-129">, yeni işleme ağacını öncekiyle karşılaştırır ve tarayıcının Belge Nesne Modeli (DOM) üzerinde herhangi bir değişiklik uygular.</span><span class="sxs-lookup"><span data-stu-id="28e46-129"> then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="28e46-130">Bileşenler sıradan C# sınıflardır ve bir proje içinde herhangi bir yere yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-130">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="28e46-131">Web sayfalarını üreten bileşenler genellikle *Sayfalar* klasöründe bulunur.</span><span class="sxs-lookup"><span data-stu-id="28e46-131">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="28e46-132">Sayfa olmayan bileşenler sıklıkla *paylaşılan* klasöre veya projeye eklenen özel bir klasöre yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-132">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span>

<span data-ttu-id="28e46-133">Genellikle, bir bileşenin ad alanı uygulamanın kök ad alanından ve uygulamanın içindeki konum (klasör) ile türetilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-133">Typically, a component's namespace is derived from the app's root namespace and the component's location (folder) within the app.</span></span> <span data-ttu-id="28e46-134">Uygulamanın kök ad alanı `BlazorApp` ve `Counter` bileşeni *Sayfalar* klasöründe yer alıyorsa:</span><span class="sxs-lookup"><span data-stu-id="28e46-134">If the app's root namespace is `BlazorApp` and the `Counter` component resides in the *Pages* folder:</span></span>

* <span data-ttu-id="28e46-135">`Counter` bileşenin ad alanı `BlazorApp.Pages`.</span><span class="sxs-lookup"><span data-stu-id="28e46-135">The `Counter` component's namespace is `BlazorApp.Pages`.</span></span>
* <span data-ttu-id="28e46-136">Bileşenin tam nitelikli tür adı `BlazorApp.Pages.Counter`.</span><span class="sxs-lookup"><span data-stu-id="28e46-136">The fully qualified type name of the component is `BlazorApp.Pages.Counter`.</span></span>

<span data-ttu-id="28e46-137">Daha fazla bilgi için [bileşenleri Içeri aktarma](#import-components) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="28e46-137">For more information, see the [Import components](#import-components) section.</span></span>

<span data-ttu-id="28e46-138">Özel bir klasör kullanmak için, özel klasörün ad alanını üst bileşene ya da uygulamanın *_Imports. Razor* dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="28e46-138">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="28e46-139">Örneğin, aşağıdaki ad alanı, uygulamanın kök ad alanı `BlazorApp`olduğunda *Bileşenler* klasöründeki bileşenleri kullanılabilir yapar:</span><span class="sxs-lookup"><span data-stu-id="28e46-139">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `BlazorApp`:</span></span>

```razor
@using BlazorApp.Components
```

## <a name="static-assets"></a><span data-ttu-id="28e46-140">Statik varlıklar</span><span class="sxs-lookup"><span data-stu-id="28e46-140">Static assets</span></span>

Blazor<span data-ttu-id="28e46-141">, projenin [Web root (Wwwroot) klasörü](xref:fundamentals/index#web-root)altına statik varlıklar yerleştirmekte olan ASP.NET Core uygulamaların kuralını izler.</span><span class="sxs-lookup"><span data-stu-id="28e46-141"> follows the convention of ASP.NET Core apps placing static assets under the project's [web root (wwwroot) folder](xref:fundamentals/index#web-root).</span></span>

<span data-ttu-id="28e46-142">Statik bir varlık için Web köküne başvurmak üzere temel göreli bir yol (`/`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="28e46-142">Use a base-relative path (`/`) to refer to the web root for a static asset.</span></span> <span data-ttu-id="28e46-143">Aşağıdaki örnekte, *logo. png* fiziksel olarak *{Project root}/Wwwroot/Images* klasöründe bulunur:</span><span class="sxs-lookup"><span data-stu-id="28e46-143">In the following example, *logo.png* is physically located in the *{PROJECT ROOT}/wwwroot/images* folder:</span></span>

```razor
<img alt="Company logo" src="/images/logo.png" />
```

<span data-ttu-id="28e46-144">Razor bileşenleri, tilde işareti gösterimini (`~/` **) desteklemez.**</span><span class="sxs-lookup"><span data-stu-id="28e46-144">Razor components do **not** support tilde-slash notation (`~/`).</span></span>

<span data-ttu-id="28e46-145">Uygulamanın temel yolunu ayarlama hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="28e46-145">For information on setting an app's base path, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="tag-helpers-arent-supported-in-components"></a><span data-ttu-id="28e46-146">Etiket Yardımcıları bileşenlerde desteklenmiyor</span><span class="sxs-lookup"><span data-stu-id="28e46-146">Tag Helpers aren't supported in components</span></span>

<span data-ttu-id="28e46-147">Razor bileşenlerinde ( *. Razor* dosyaları) [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="28e46-147">[Tag Helpers](xref:mvc/views/tag-helpers/intro) aren't supported in Razor components (*.razor* files).</span></span> <span data-ttu-id="28e46-148">Blazor' de etiket Yardımcısı benzeri işlevsellik sağlamak için, etiket Yardımcısı ile aynı işlevselliğe sahip bir bileşen oluşturun ve bunun yerine bileşeni kullanın.</span><span class="sxs-lookup"><span data-stu-id="28e46-148">To provide Tag Helper-like functionality in Blazor, create a component with the same functionality as the Tag Helper and use the component instead.</span></span>

## <a name="use-components"></a><span data-ttu-id="28e46-149">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="28e46-149">Use components</span></span>

<span data-ttu-id="28e46-150">Bileşenler, HTML öğesi söz dizimini kullanarak bildirerek diğer bileşenleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-150">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="28e46-151">Bir bileşeni kullanmak için biçimlendirme, etiket adının bileşen türü olduğu bir HTML etiketi gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="28e46-151">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="28e46-152">*Index. Razor* dosyasında aşağıdaki biçimlendirme `HeadingComponent` örneği işler:</span><span class="sxs-lookup"><span data-stu-id="28e46-152">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="28e46-153">*Bileşenler/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="28e46-153">*Components/HeadingComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="28e46-154">Bir bileşen bir bileşen adıyla eşleşmeyen büyük harfle yazılmış bir HTML öğesi içeriyorsa, öğenin beklenmeyen bir adı olduğunu gösteren bir uyarı yayınlanır.</span><span class="sxs-lookup"><span data-stu-id="28e46-154">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="28e46-155">Bileşenin ad alanı için `@using` yönergesi eklemek, bileşeni, uyarıyı çözen şekilde kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="28e46-155">Adding an `@using` directive for the component's namespace makes the component available, which resolves the warning.</span></span>

## <a name="routing"></a><span data-ttu-id="28e46-156">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="28e46-156">Routing</span></span>

<span data-ttu-id="28e46-157">Blazor yönlendirme, uygulamadaki her erişilebilir bileşene bir rota şablonu sağlayarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-157">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="28e46-158">`@page` yönergesine sahip bir Razor dosyası derlendiğinde, oluşturulan sınıfa yol şablonunu belirten bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> verilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-158">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="28e46-159">Çalışma zamanında, yönlendirici bileşen sınıflarını bir `RouteAttribute` arar ve hangi bileşenin istenen URL ile eşleşen bir rota şablonuna sahip olduğunu işler.</span><span class="sxs-lookup"><span data-stu-id="28e46-159">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

```razor
@page "/ParentComponent"

...
```

<span data-ttu-id="28e46-160">Daha fazla bilgi için bkz. <xref:blazor/routing>.</span><span class="sxs-lookup"><span data-stu-id="28e46-160">For more information, see <xref:blazor/routing>.</span></span>

## <a name="parameters"></a><span data-ttu-id="28e46-161">Parametreler</span><span class="sxs-lookup"><span data-stu-id="28e46-161">Parameters</span></span>

### <a name="route-parameters"></a><span data-ttu-id="28e46-162">Rota parametreleri</span><span class="sxs-lookup"><span data-stu-id="28e46-162">Route parameters</span></span>

<span data-ttu-id="28e46-163">Bileşenler, `@page` yönergesinde belirtilen yol şablonundan yol parametreleri alabilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-163">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="28e46-164">Yönlendirici, karşılık gelen bileşen parametrelerini doldurmak için yol parametrelerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="28e46-164">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="28e46-165">*Pages/RouteParameter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="28e46-165">*Pages/RouteParameter.razor*:</span></span>

[!code-razor[](components/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

<span data-ttu-id="28e46-166">İsteğe bağlı parametreler desteklenmez, bu nedenle yukarıdaki örnekte iki `@page` yönergesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="28e46-166">Optional parameters aren't supported, so two `@page` directives are applied in the preceding example.</span></span> <span data-ttu-id="28e46-167">İlki, bir parametre olmadan bileşene gezinmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="28e46-167">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="28e46-168">İkinci `@page` yönergesi `{text}` Route parametresini alır ve değeri `Text` özelliğine atar.</span><span class="sxs-lookup"><span data-stu-id="28e46-168">The second `@page` directive receives the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="28e46-169">*Catch-all* parametre sözdizimi (`*`/`**`), birden çok klasör sınırları genelinde yolu yakalayan Razor bileşenlerinde ( *. Razor* **) desteklenmez.**</span><span class="sxs-lookup"><span data-stu-id="28e46-169">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

### <a name="component-parameters"></a><span data-ttu-id="28e46-170">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="28e46-170">Component parameters</span></span>

<span data-ttu-id="28e46-171">Bileşenler, bileşen sınıfında `[Parameter]` özniteliğiyle ortak özellikler kullanılarak tanımlanan *bileşen parametrelerine*sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-171">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="28e46-172">Biçimlendirme içindeki bir bileşenin bağımsız değişkenlerini belirtmek için öznitelikleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="28e46-172">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="28e46-173">*Bileşenler/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="28e46-173">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

<span data-ttu-id="28e46-174">Örnek uygulamadan aşağıdaki örnekte `ParentComponent`, `ChildComponent``Title` özelliğinin değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="28e46-174">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="28e46-175">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="28e46-175">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="28e46-176">Alt içerik</span><span class="sxs-lookup"><span data-stu-id="28e46-176">Child content</span></span>

<span data-ttu-id="28e46-177">Bileşenler, başka bir bileşenin içeriğini ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-177">Components can set the content of another component.</span></span> <span data-ttu-id="28e46-178">Atama bileşeni, alıcı bileşeni belirten Etiketler arasında içerik sağlar.</span><span class="sxs-lookup"><span data-stu-id="28e46-178">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="28e46-179">Aşağıdaki örnekte `ChildComponent`, işlemek için bir kullanıcı arabirimi segmentini temsil eden bir `RenderFragment`temsil eden bir `ChildContent` özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="28e46-179">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="28e46-180">`ChildContent` değeri bileşenin, içeriğin işlenmesi gereken biçimlendirmesinde konumlandırılır.</span><span class="sxs-lookup"><span data-stu-id="28e46-180">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="28e46-181">`ChildContent` değeri üst bileşenden alınır ve önyükleme bölmesinin `panel-body`içinde işlenir.</span><span class="sxs-lookup"><span data-stu-id="28e46-181">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="28e46-182">*Bileşenler/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="28e46-182">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="28e46-183">`RenderFragment` içeriği alan özelliğin kurala göre `ChildContent` olarak adlandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="28e46-183">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="28e46-184">Örnek uygulamadaki `ParentComponent`, içeriği `<ChildComponent>` etiketlerin içine yerleştirerek `ChildComponent` işlemek için içerik sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-184">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="28e46-185">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="28e46-185">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="28e46-186">Öznitelik döndürme ve rastgele parametreler</span><span class="sxs-lookup"><span data-stu-id="28e46-186">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="28e46-187">Bileşenler, bileşen tarafından tanımlanan parametrelere ek olarak ek öznitelikler yakalayabilir ve işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-187">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="28e46-188">Ek öznitelikler bir sözlükte yakalanabilir ve sonra bileşen [`@attributes`](xref:mvc/views/razor#attributes) Razor yönergesi kullanılarak işlendiğinde bir *öğe üzerine bırakılabilir* .</span><span class="sxs-lookup"><span data-stu-id="28e46-188">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="28e46-189">Bu senaryo, çeşitli özelleştirmeleri destekleyen bir işaretleme öğesi üreten bir bileşen tanımlarken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="28e46-189">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="28e46-190">Örneğin, çok sayıda parametreyi destekleyen bir `<input>` öznitelikleri ayrı olarak tanımlamak sıkıcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-190">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="28e46-191">Aşağıdaki örnekte, ilk `<input>` öğesi (`id="useIndividualParams"`) bağımsız bileşen parametrelerini kullanır, ancak ikinci `<input>` öğesi (`id="useAttributesDict"`) öznitelik sponu kullanır:</span><span class="sxs-lookup"><span data-stu-id="28e46-191">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```razor
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="28e46-192">Parametrenin türü dize anahtarlarıyla `IEnumerable<KeyValuePair<string, object>>` uygulamalıdır.</span><span class="sxs-lookup"><span data-stu-id="28e46-192">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="28e46-193">`IReadOnlyDictionary<string, object>` kullanmak Bu senaryoda da bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="28e46-193">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="28e46-194">Her iki yaklaşımın de kullanıldığı işlenen `<input>` öğeleri aynıdır:</span><span class="sxs-lookup"><span data-stu-id="28e46-194">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

<span data-ttu-id="28e46-195">Rastgele öznitelikleri kabul etmek için, `CaptureUnmatchedValues` özelliği `true`olarak ayarlanan `[Parameter]` özniteliğini kullanarak bir bileşen parametresi tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="28e46-195">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="28e46-196">`[Parameter]` `CaptureUnmatchedValues` özelliği, parametrenin diğer bir parametreyle eşleşmeyen tüm özniteliklerle eşleşmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="28e46-196">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="28e46-197">Bir bileşen yalnızca `CaptureUnmatchedValues`olan tek bir parametre tanımlayabilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-197">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="28e46-198">`CaptureUnmatchedValues` ile kullanılan özellik türü, `Dictionary<string, object>` dize anahtarlarıyla atanabilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="28e46-198">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="28e46-199">`IEnumerable<KeyValuePair<string, object>>` veya `IReadOnlyDictionary<string, object>` Ayrıca bu senaryodaki seçeneklerdir.</span><span class="sxs-lookup"><span data-stu-id="28e46-199">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="28e46-200">Öğe özniteliklerinin konumuna göre `@attributes` konumu önemlidir.</span><span class="sxs-lookup"><span data-stu-id="28e46-200">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="28e46-201">Öğe üzerinde `@attributes`, öznitelikler sağdan sola (son olarak) işlenir.</span><span class="sxs-lookup"><span data-stu-id="28e46-201">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="28e46-202">`Child` bileşeni tüketen bir bileşen için aşağıdaki örneği göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="28e46-202">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="28e46-203">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="28e46-203">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="28e46-204">*Childcomponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="28e46-204">*ChildComponent.razor*:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="28e46-205">`Child` bileşenin `extra` özniteliği `@attributes`sağına ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="28e46-205">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="28e46-206">`Parent` bileşenin işlenmiş `<div>`, öznitelikler sağdan sola (en son) işlendiği için ek özniteliğiyle geçirildiğinde `extra="5"` içerir:</span><span class="sxs-lookup"><span data-stu-id="28e46-206">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="28e46-207">Aşağıdaki örnekte, `extra` ve `@attributes` sırası `Child` bileşeninin `<div>`tersine çevrilir:</span><span class="sxs-lookup"><span data-stu-id="28e46-207">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="28e46-208">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="28e46-208">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="28e46-209">*Childcomponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="28e46-209">*ChildComponent.razor*:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="28e46-210">`Parent` bileşenindeki işlenen `<div>`, ek öznitelik üzerinden geçirildiğinde `extra="10"` içerir:</span><span class="sxs-lookup"><span data-stu-id="28e46-210">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a><span data-ttu-id="28e46-211">Bileşenlere başvuruları yakala</span><span class="sxs-lookup"><span data-stu-id="28e46-211">Capture references to components</span></span>

<span data-ttu-id="28e46-212">Bileşen başvuruları, bir bileşen örneğine başvurmak için bir yol sağlar, böylece bu örneğe `Show` veya `Reset`gibi komutlar verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="28e46-212">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="28e46-213">Bir bileşen başvurusunu yakalamak için:</span><span class="sxs-lookup"><span data-stu-id="28e46-213">To capture a component reference:</span></span>

* <span data-ttu-id="28e46-214">Alt bileşene bir [`@ref`](xref:mvc/views/razor#ref) özniteliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="28e46-214">Add an [`@ref`](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="28e46-215">Alt bileşenle aynı türde bir alan tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="28e46-215">Define a field with the same type as the child component.</span></span>

```razor
<MyLoginDialog @ref="_loginDialog" ... />

@code {
    private MyLoginDialog _loginDialog;

    private void OnSomething()
    {
        _loginDialog.Show();
    }
}
```

<span data-ttu-id="28e46-216">Bileşen işlendiğinde `_loginDialog` alanı `MyLoginDialog` alt bileşen örneğiyle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="28e46-216">When the component is rendered, the `_loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="28e46-217">Daha sonra bileşen örneğinde .NET yöntemlerini çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="28e46-217">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="28e46-218">`_loginDialog` değişkeni yalnızca bileşen işlendikten sonra ve çıktısı `MyLoginDialog` öğesini içerdiğinde doldurulur.</span><span class="sxs-lookup"><span data-stu-id="28e46-218">The `_loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="28e46-219">Bu noktaya kadar başvurulmasına hiçbir şey yok.</span><span class="sxs-lookup"><span data-stu-id="28e46-219">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="28e46-220">Bileşen işlemesini tamamladıktan sonra bileşen başvurularını işlemek için [Onafterrenderasync veya OnAfterRender yöntemlerini](xref:blazor/lifecycle#after-component-render)kullanın.</span><span class="sxs-lookup"><span data-stu-id="28e46-220">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="28e46-221">Bir döngüdeki bileşenlere başvurmak için bkz. [birden çok benzer alt bileşene başvuruları yakalama (DotNet/aspnetcore #13358)](https://github.com/dotnet/aspnetcore/issues/13358).</span><span class="sxs-lookup"><span data-stu-id="28e46-221">To reference components in a loop, see [Capture references to multiple similar child-components (dotnet/aspnetcore #13358)](https://github.com/dotnet/aspnetcore/issues/13358).</span></span>

<span data-ttu-id="28e46-222">Bileşen başvurularını yakalama, [öğe başvurularını yakalamak](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements)için benzer bir sözdizimi kullanın, bir JavaScript birlikte çalışma özelliği değildir.</span><span class="sxs-lookup"><span data-stu-id="28e46-222">While capturing component references use a similar syntax to [capturing element references](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements), it isn't a JavaScript interop feature.</span></span> <span data-ttu-id="28e46-223">Bileşen başvuruları yalnızca .NET kodunda kullanıldıkları&mdash;JavaScript koduna aktarılmaz.</span><span class="sxs-lookup"><span data-stu-id="28e46-223">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="28e46-224">Alt bileşenlerin durumunu bulunmamalıdır için bileşen **başvurularını kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="28e46-224">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="28e46-225">Bunun yerine, alt bileşenlere veri geçirmek için normal bildirime dayalı parametreleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="28e46-225">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="28e46-226">Normal bildirime dayalı parametrelerin kullanımı, otomatik olarak doğru zamanların yeniden yönlendirmesi için alt bileşenlerde oluşur.</span><span class="sxs-lookup"><span data-stu-id="28e46-226">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="28e46-227">Durumu güncelleştirmek için bileşen yöntemlerini dışarıdan çağır</span><span class="sxs-lookup"><span data-stu-id="28e46-227">Invoke component methods externally to update state</span></span>

Blazor<span data-ttu-id="28e46-228">, yürütmenin tek bir mantıksal iş parçacığını zorlamak için bir `SynchronizationContext` kullanır.</span><span class="sxs-lookup"><span data-stu-id="28e46-228"> uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="28e46-229">Bir bileşenin [yaşam döngüsü yöntemleri](xref:blazor/lifecycle) ve Blazor tarafından oluşturulan tüm olay geri çağırmaları bu `SynchronizationContext`yürütülür.</span><span class="sxs-lookup"><span data-stu-id="28e46-229">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="28e46-230">Bir bileşenin Zamanlayıcı veya diğer bildirimler gibi dış bir olaya göre güncellenmesi gerekir, Blazor`SynchronizationContext`dağıtım yapılacak `InvokeAsync` yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="28e46-230">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="28e46-231">Örneğin, güncelleştirilmiş durumdaki herhangi bir dinleme bileşenine bildirimde bulunan bir *bildirim hizmeti* düşünün:</span><span class="sxs-lookup"><span data-stu-id="28e46-231">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

<span data-ttu-id="28e46-232">`NotifierService` bir tekalma olarak kaydedin:</span><span class="sxs-lookup"><span data-stu-id="28e46-232">Register the `NotifierService` as a singletion:</span></span>

* <span data-ttu-id="28e46-233">Blazor WebAssembly ' de hizmeti `Program.Main`kaydedin:</span><span class="sxs-lookup"><span data-stu-id="28e46-233">In Blazor WebAssembly, register the service in `Program.Main`:</span></span>

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* <span data-ttu-id="28e46-234">Blazor sunucusu 'nda hizmeti `Startup.ConfigureServices`kaydedin:</span><span class="sxs-lookup"><span data-stu-id="28e46-234">In Blazor Server, register the service in `Startup.ConfigureServices`:</span></span>

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

<span data-ttu-id="28e46-235">Bir bileşeni güncelleştirmek için `NotifierService` kullanın:</span><span class="sxs-lookup"><span data-stu-id="28e46-235">Use the `NotifierService` to update a component:</span></span>

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @_lastNotification.key = @_lastNotification.value</p>

@code {
    private (string key, int value) _lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            _lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="28e46-236">Yukarıdaki örnekte `NotifierService` bileşenin `OnNotify` yöntemini Blazor`SynchronizationContext`dışında çağırır.</span><span class="sxs-lookup"><span data-stu-id="28e46-236">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="28e46-237">`InvokeAsync`, doğru bağlama geçmek ve bir işlemeyi kuyruğa almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="28e46-237">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="28e46-238">Öğelerin ve bileşenlerin korunmasını denetlemek için \@anahtarını kullanın</span><span class="sxs-lookup"><span data-stu-id="28e46-238">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="28e46-239">Bir öğe veya bileşen listesi işlendiğinde ve öğeler ya da bileşenler daha sonra değiştiğinde, Blazoryayılma algoritması, önceki öğelerin veya bileşenlerin ne zaman tutulacağına ve model nesnelerinin bunlara nasıl eşleneceğine karar vermelidir.</span><span class="sxs-lookup"><span data-stu-id="28e46-239">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="28e46-240">Normalde, bu işlem otomatiktir ve yoksayılabilir, ancak işlemi denetlemek isteyebileceğiniz durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="28e46-240">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="28e46-241">Aşağıdaki örnek göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="28e46-241">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="28e46-242">`People` koleksiyonun içerikleri, ekli, silinmiş veya yeniden sıralanmış girdilerle değişebilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-242">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="28e46-243">Bileşen yeniden oluşturulduğunda `<DetailsEditor>` bileşeni farklı `Details` parametre değerleri almak için değişebilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-243">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="28e46-244">Bu, beklenenden daha karmaşık rerendering oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-244">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="28e46-245">Bazı durumlarda rerendering, kayıp öğe odağı gibi görünür davranış farklılıklarına yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-245">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="28e46-246">Eşleme işlemi `@key` Directive özniteliğiyle denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-246">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="28e46-247">`@key`, anahtar değerine göre öğelerin veya bileşenlerin korunmasını güvence altına almak için dağıtılmış algoritmaya neden olur:</span><span class="sxs-lookup"><span data-stu-id="28e46-247">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="28e46-248">`People` koleksiyonu değiştiğinde, yayılma algoritması `<DetailsEditor>` örnekleri ve `person` örnekleri arasındaki ilişkilendirmeyi korur:</span><span class="sxs-lookup"><span data-stu-id="28e46-248">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="28e46-249">Bir `Person` `People` listesinden silinirse, yalnızca ilgili `<DetailsEditor>` örneği kullanıcı arabiriminden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="28e46-249">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="28e46-250">Diğer örnekler değişmeden bırakılır.</span><span class="sxs-lookup"><span data-stu-id="28e46-250">Other instances are left unchanged.</span></span>
* <span data-ttu-id="28e46-251">Listedeki bir konuma `Person` eklenirse, ilgili konuma bir yeni `<DetailsEditor>` örneği eklenir.</span><span class="sxs-lookup"><span data-stu-id="28e46-251">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="28e46-252">Diğer örnekler değişmeden bırakılır.</span><span class="sxs-lookup"><span data-stu-id="28e46-252">Other instances are left unchanged.</span></span>
* <span data-ttu-id="28e46-253">`Person` girdileri yeniden sıralandıysanız, karşılık gelen `<DetailsEditor>` örnekleri korunur ve Kullanıcı arabiriminde yeniden sıralanır.</span><span class="sxs-lookup"><span data-stu-id="28e46-253">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="28e46-254">Bazı senaryolarda `@key` kullanımı, rerendering karmaşıklığını en aza indirir ve odak konumu gibi DOM 'ın durum bilgisi olan kısımlarıyla ilgili olası sorunları önler.</span><span class="sxs-lookup"><span data-stu-id="28e46-254">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="28e46-255">Anahtarlar her kapsayıcı öğesi veya bileşeni için yereldir.</span><span class="sxs-lookup"><span data-stu-id="28e46-255">Keys are local to each container element or component.</span></span> <span data-ttu-id="28e46-256">Anahtarlar belge genelinde küresel olarak karşılaştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="28e46-256">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="28e46-257">\@anahtarı ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="28e46-257">When to use \@key</span></span>

<span data-ttu-id="28e46-258">Genellikle, bir liste işlendiğinde (örneğin, bir `@foreach` bloğunda) ve `@key`tanımlamak için uygun bir değer olduğunda `@key` kullanmak mantıklı olur.</span><span class="sxs-lookup"><span data-stu-id="28e46-258">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="28e46-259">Ayrıca, bir nesne değiştiğinde Blazor bir öğeyi veya bileşen alt ağacını `@key` engellemek için de kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="28e46-259">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="28e46-260">`@currentPerson` değişirse `@key` Attribute yönergesi Blazor `<div>` ve alt öğelerini atmayı ve yeni öğeler ve bileşenlerle Kullanıcı arabiriminde alt ağacı yeniden oluşturmayı zorlar.</span><span class="sxs-lookup"><span data-stu-id="28e46-260">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="28e46-261">`@currentPerson` değiştiğinde hiçbir Kullanıcı arabirimi durumunun korunmayacağını garanti etmeniz gerekirse bu yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-261">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="28e46-262">\@anahtar ne zaman kullanılmaz</span><span class="sxs-lookup"><span data-stu-id="28e46-262">When not to use \@key</span></span>

<span data-ttu-id="28e46-263">`@key`bir performans maliyeti vardır.</span><span class="sxs-lookup"><span data-stu-id="28e46-263">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="28e46-264">Performans maliyeti büyük değildir, ancak öğe veya bileşen koruma kurallarının uygulamanın avantajına göre denetlenmesi durumunda yalnızca `@key` belirtin.</span><span class="sxs-lookup"><span data-stu-id="28e46-264">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="28e46-265">`@key` kullanılmasa bile, Blazor alt öğe ve bileşen örneklerini mümkün olduğunca korur.</span><span class="sxs-lookup"><span data-stu-id="28e46-265">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="28e46-266">`@key` kullanmanın avantajı, model örneklerinin eşlemeyi seçme algoritması yerine, korunan bileşen örneklerine *nasıl* eşlendiğine ilişkin denetimdir.</span><span class="sxs-lookup"><span data-stu-id="28e46-266">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="28e46-267">\@anahtarı için kullanılacak değerler</span><span class="sxs-lookup"><span data-stu-id="28e46-267">What values to use for \@key</span></span>

<span data-ttu-id="28e46-268">Genellikle, `@key`için aşağıdaki değer türlerinden birini sağlamak mantıklı olur:</span><span class="sxs-lookup"><span data-stu-id="28e46-268">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="28e46-269">Model nesne örnekleri (örneğin, önceki örnekte olduğu gibi `Person` örneği).</span><span class="sxs-lookup"><span data-stu-id="28e46-269">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="28e46-270">Bu, nesne başvurusu eşitliğine göre koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="28e46-270">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="28e46-271">Benzersiz tanımlayıcılar (örneğin, `int`, `string`veya `Guid`için birincil anahtar değerleri).</span><span class="sxs-lookup"><span data-stu-id="28e46-271">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="28e46-272">`@key` için kullanılan değerlerin çakışmayın olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="28e46-272">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="28e46-273">Aynı üst öğe içinde çakışan değerler algılanırsa, eski öğeleri veya bileşenleri yeni öğe veya bileşenlere kesin bir şekilde eşlemediğinden Blazor bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="28e46-273">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="28e46-274">Yalnızca nesne örnekleri veya birincil anahtar değerleri gibi farklı değerleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="28e46-274">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="partial-class-support"></a><span data-ttu-id="28e46-275">Kısmi sınıf desteği</span><span class="sxs-lookup"><span data-stu-id="28e46-275">Partial class support</span></span>

<span data-ttu-id="28e46-276">Razor bileşenleri kısmi sınıflar olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="28e46-276">Razor components are generated as partial classes.</span></span> <span data-ttu-id="28e46-277">Razor bileşenleri aşağıdaki yaklaşımlardan birini kullanarak yazılır:</span><span class="sxs-lookup"><span data-stu-id="28e46-277">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="28e46-278">C#kod, tek bir dosyada HTML işaretlemesi ve Razor kodu ile bir [`@code`](xref:mvc/views/razor#code) bloğunda tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="28e46-278">C# code is defined in an [`@code`](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> Blazor<span data-ttu-id="28e46-279"> şablonlar, bu yaklaşımı kullanarak Razor bileşenlerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="28e46-279"> templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="28e46-280">C#kod, kısmi sınıf olarak tanımlanan bir arka plan kod dosyasına yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-280">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="28e46-281">Aşağıdaki örnek, bir Blazor şablonundan oluşturulan bir uygulamada `@code` bloğu olan varsayılan `Counter` bileşenini gösterir.</span><span class="sxs-lookup"><span data-stu-id="28e46-281">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="28e46-282">HTML işaretleme, Razor kodu ve C# kod aynı dosyada:</span><span class="sxs-lookup"><span data-stu-id="28e46-282">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="28e46-283">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="28e46-283">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int _currentCount = 0;

    void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="28e46-284">`Counter` bileşeni, kısmi bir sınıf içeren bir arka plan kod dosyası kullanılarak da oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="28e46-284">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="28e46-285">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="28e46-285">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="28e46-286">*Counter.Razor.cs*:</span><span class="sxs-lookup"><span data-stu-id="28e46-286">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int _currentCount = 0;

        void IncrementCount()
        {
            _currentCount++;
        }
    }
}
```

<span data-ttu-id="28e46-287">Gerekli olan ad alanlarını kısmi sınıf dosyasına gereken şekilde ekleyin.</span><span class="sxs-lookup"><span data-stu-id="28e46-287">Add any required namespaces to the partial class file as needed.</span></span> <span data-ttu-id="28e46-288">Razor bileşenleri tarafından kullanılan tipik ad alanları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="28e46-288">Typical namespaces used by Razor components include:</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a><span data-ttu-id="28e46-289">Temel sınıf belirtin</span><span class="sxs-lookup"><span data-stu-id="28e46-289">Specify a base class</span></span>

<span data-ttu-id="28e46-290">[`@inherits`](xref:mvc/views/razor#inherits) yönergesi, bir bileşen için temel sınıf belirtmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-290">The [`@inherits`](xref:mvc/views/razor#inherits) directive can be used to specify a base class for a component.</span></span> <span data-ttu-id="28e46-291">Aşağıdaki örnek, bileşenin özelliklerini ve yöntemlerini sağlamak için bir bileşenin `BlazorRocksBase`bir temel sınıfı nasıl devralmasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="28e46-291">The following example shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span> <span data-ttu-id="28e46-292">Temel sınıf `ComponentBase`türetmelidir.</span><span class="sxs-lookup"><span data-stu-id="28e46-292">The base class should derive from `ComponentBase`.</span></span>

<span data-ttu-id="28e46-293">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="28e46-293">*Pages/BlazorRocks.razor*:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="28e46-294">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="28e46-294">*BlazorRocksBase.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components;

namespace BlazorSample
{
    public class BlazorRocksBase : ComponentBase
    {
        public string BlazorRocksText { get; set; } = 
            "Blazor rocks the browser!";
    }
}
```

## <a name="specify-an-attribute"></a><span data-ttu-id="28e46-295">Bir öznitelik belirtin</span><span class="sxs-lookup"><span data-stu-id="28e46-295">Specify an attribute</span></span>

<span data-ttu-id="28e46-296">Özellikler Razor bileşenlerinde [`@attribute`](xref:mvc/views/razor#attribute) yönergesi ile belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-296">Attributes can be specified in Razor components with the [`@attribute`](xref:mvc/views/razor#attribute) directive.</span></span> <span data-ttu-id="28e46-297">Aşağıdaki örnek bileşen sınıfına `[Authorize]` özniteliğini uygular:</span><span class="sxs-lookup"><span data-stu-id="28e46-297">The following example applies the `[Authorize]` attribute to the component class:</span></span>

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a><span data-ttu-id="28e46-298">Bileşenleri içeri aktar</span><span class="sxs-lookup"><span data-stu-id="28e46-298">Import components</span></span>

<span data-ttu-id="28e46-299">Razor ile yazılan bir bileşenin ad alanı temel alınarak belirlenir (öncelik sırasına göre):</span><span class="sxs-lookup"><span data-stu-id="28e46-299">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="28e46-300">Razor dosyası ( *. Razor*) biçimlendirmesinde [`@namespace`](xref:mvc/views/razor#namespace) ataması (`@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="28e46-300">[`@namespace`](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="28e46-301">Projenin proje dosyasında `RootNamespace` (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="28e46-301">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="28e46-302">Proje dosyasının dosya adından ( *. csproj*) ve proje kökünden bileşen yolundan alınan proje adı.</span><span class="sxs-lookup"><span data-stu-id="28e46-302">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="28e46-303">Örneğin Framework, *{Project root}/Pages/Index.Razor* (*BlazorSample. csproj*) ad alanına `BlazorSample.Pages`çözümleniyor.</span><span class="sxs-lookup"><span data-stu-id="28e46-303">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="28e46-304">Bileşenler ad C# bağlama kurallarını izler.</span><span class="sxs-lookup"><span data-stu-id="28e46-304">Components follow C# name binding rules.</span></span> <span data-ttu-id="28e46-305">Bu örnekteki `Index` bileşeni için, kapsamdaki bileşenler tüm bileşenlerdir:</span><span class="sxs-lookup"><span data-stu-id="28e46-305">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="28e46-306">Aynı klasörde, *sayfalarda*.</span><span class="sxs-lookup"><span data-stu-id="28e46-306">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="28e46-307">Proje kökündeki, açıkça farklı bir ad alanı belirtmeyen bileşenler.</span><span class="sxs-lookup"><span data-stu-id="28e46-307">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="28e46-308">Farklı bir ad alanında tanımlanan bileşenler, Razor 'nin [`@using`](xref:mvc/views/razor#using) yönergesi kullanılarak kapsam içine getirilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-308">Components defined in a different namespace are brought into scope using Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="28e46-309">*BlazorSample/Shared/* klasöründe `NavMenu.razor`başka bir bileşen varsa, bileşen aşağıdaki `@using` ifadesiyle `Index.razor` kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="28e46-309">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="28e46-310">Bileşenlere Ayrıca, [`@using`](xref:mvc/views/razor#using) yönergesini gerektirmeyen tam nitelikli adları kullanılarak başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="28e46-310">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`](xref:mvc/views/razor#using) directive:</span></span>

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="28e46-311">`global::` niteleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="28e46-311">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="28e46-312">Diğer ad `using` deyimleriyle bileşenleri içeri aktarma (örneğin, `@using Foo = Bar`) desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="28e46-312">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="28e46-313">Kısmen nitelenmiş adlar desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="28e46-313">Partially qualified names aren't supported.</span></span> <span data-ttu-id="28e46-314">Örneğin, `@using BlazorSample` eklemek ve `NavMenu.razor` başvurmak `<Shared.NavMenu></Shared.NavMenu>` desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="28e46-314">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="28e46-315">Koşullu HTML öğesi öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="28e46-315">Conditional HTML element attributes</span></span>

<span data-ttu-id="28e46-316">HTML öğesi öznitelikleri, .NET değerine göre koşullu olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="28e46-316">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="28e46-317">Değer `false` veya `null`ise, öznitelik işlenmez.</span><span class="sxs-lookup"><span data-stu-id="28e46-317">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="28e46-318">Değer `true`ise, öznitelik küçültülmüş olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="28e46-318">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="28e46-319">Aşağıdaki örnekte, `checked` öğenin biçimlendirmesinde işlenip işlenmeyeceğini `IsCompleted` belirler:</span><span class="sxs-lookup"><span data-stu-id="28e46-319">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="28e46-320">`IsCompleted` `true`, onay kutusu şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="28e46-320">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="28e46-321">`IsCompleted` `false`, onay kutusu şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="28e46-321">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="28e46-322">Daha fazla bilgi için bkz. <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="28e46-322">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="28e46-323">.NET türü `bool`olduğunda, [Aria-basılan](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons)gıbı bazı HTML öznitelikleri düzgün şekilde çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="28e46-323">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="28e46-324">Bu durumlarda, `bool`yerine `string` türü kullanın.</span><span class="sxs-lookup"><span data-stu-id="28e46-324">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="28e46-325">Ham HTML</span><span class="sxs-lookup"><span data-stu-id="28e46-325">Raw HTML</span></span>

<span data-ttu-id="28e46-326">Dizeler normalde DOM metin düğümleri kullanılarak işlenir. Bu, içerdikleri tüm biçimlendirmenin yok sayıldığı ve değişmez değer olarak kabul edildiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="28e46-326">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="28e46-327">Ham HTML işlemek için, HTML içeriğini bir `MarkupString` değerde sarın.</span><span class="sxs-lookup"><span data-stu-id="28e46-327">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="28e46-328">Değer HTML veya SVG olarak ayrıştırılır ve DOM 'a eklenir.</span><span class="sxs-lookup"><span data-stu-id="28e46-328">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="28e46-329">Güvenilmeyen bir kaynaktan oluşturulan ham HTML işleme bir **güvenlik riskidir** ve kaçınılması gerekir!</span><span class="sxs-lookup"><span data-stu-id="28e46-329">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="28e46-330">Aşağıdaki örnek, bir bileşenin işlenmiş çıktısına statik HTML içeriği bloğunu eklemek için `MarkupString` türünü kullanmayı gösterir:</span><span class="sxs-lookup"><span data-stu-id="28e46-330">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="28e46-331">Değerleri ve parametreleri basamaklama</span><span class="sxs-lookup"><span data-stu-id="28e46-331">Cascading values and parameters</span></span>

<span data-ttu-id="28e46-332">Bazı senaryolarda, özellikle birden çok bileşen katmanı olduğunda, [bileşen parametreleri](#component-parameters)kullanarak bir üst bileşenden bir alt bileşene veri akışı yapmak uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="28e46-332">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="28e46-333">Değerleri ve parametreleri basamaklama, bir üst bileşenin tüm alt bileşenlerine değer sağlaması için kullanışlı bir yol sağlayarak bu sorunu çözebilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-333">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="28e46-334">Basamaklı değerler ve parametreler, bileşenlerin koordinasyonu için bir yaklaşım sağlar.</span><span class="sxs-lookup"><span data-stu-id="28e46-334">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="28e46-335">Tema örneği</span><span class="sxs-lookup"><span data-stu-id="28e46-335">Theme example</span></span>

<span data-ttu-id="28e46-336">Örnek uygulamadan aşağıdaki örnekte, `ThemeInfo` sınıfı, uygulamanın belirli bir bölümündeki tüm düğmelerin aynı stili paylaştığı şekilde bileşen hiyerarşisinin akışını yapmak için tema bilgilerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="28e46-336">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="28e46-337">*Uıthemeclasses/Themeınfo. cs*:</span><span class="sxs-lookup"><span data-stu-id="28e46-337">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="28e46-338">Bir üst bileşen basamaklı değer bileşeni kullanılarak basamaklı bir değer sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-338">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="28e46-339">`CascadingValue` bileşeni, bileşen hiyerarşisinin bir alt ağacını sarmalanmış ve bu alt ağaçta bulunan tüm bileşenlere tek bir değer sağlar.</span><span class="sxs-lookup"><span data-stu-id="28e46-339">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="28e46-340">Örneğin, örnek uygulama, `@Body` özelliğinin düzen gövdesini oluşturan tüm bileşenler için bir geçişli parametre olarak uygulamanın düzenlerindeki tema bilgilerini (`ThemeInfo`) belirtir.</span><span class="sxs-lookup"><span data-stu-id="28e46-340">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="28e46-341">`ButtonClass`, düzen bileşeninde `btn-success` bir değeri atanır.</span><span class="sxs-lookup"><span data-stu-id="28e46-341">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="28e46-342">Tüm alt bileşenler, `ThemeInfo` basamaklı nesne aracılığıyla bu özelliği kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-342">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="28e46-343">`CascadingValuesParametersLayout` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="28e46-343">`CascadingValuesParametersLayout` component:</span></span>

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="_theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo _theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="28e46-344">Basamaklı değerleri kullanmak için, bileşenler `[CascadingParameter]` özniteliği kullanarak Geçişli Parametreler bildirir.</span><span class="sxs-lookup"><span data-stu-id="28e46-344">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="28e46-345">Basamaklı değerler, türe göre basamaklı parametrelere bağlanır.</span><span class="sxs-lookup"><span data-stu-id="28e46-345">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="28e46-346">Örnek uygulamada `CascadingValuesParametersTheme` bileşeni, `ThemeInfo` geçişli değeri basamaklı bir parametreye bağlar.</span><span class="sxs-lookup"><span data-stu-id="28e46-346">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="28e46-347">Parametresi, bileşen tarafından görünen düğmelerden birine ait CSS sınıfını ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="28e46-347">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="28e46-348">`CascadingValuesParametersTheme` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="28e46-348">`CascadingValuesParametersTheme` component:</span></span>

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @_currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int _currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="28e46-349">Aynı alt ağaç içindeki aynı türdeki birden çok değeri basamakla, her bir `CascadingValue` bileşenine ve karşılık gelen `CascadingParameter`benzersiz bir `Name` dizesi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="28e46-349">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="28e46-350">Aşağıdaki örnekte, iki `CascadingValue` bileşeni, `MyCascadingType` farklı örneklerini ada göre basamakla:</span><span class="sxs-lookup"><span data-stu-id="28e46-350">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```razor
<CascadingValue Value=@_parentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType _parentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="28e46-351">Alt bileşende, basamaklı parametreler değerlerini, üst bileşendeki ilgili basamaklı değerleri ada göre alır:</span><span class="sxs-lookup"><span data-stu-id="28e46-351">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="28e46-352">TabSet örneği</span><span class="sxs-lookup"><span data-stu-id="28e46-352">TabSet example</span></span>

<span data-ttu-id="28e46-353">Basamaklı parametreler, bileşenlerin bileşen hiyerarşisinde işbirliği yapmasına de olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="28e46-353">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="28e46-354">Örneğin, örnek uygulamada aşağıdaki *Tabset* örneğini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="28e46-354">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="28e46-355">Örnek uygulama, sekmelerin uygulandığı bir `ITab` arabirimine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="28e46-355">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="28e46-356">`CascadingValuesParametersTabSet` bileşeni, çeşitli `Tab` bileşenleri içeren `TabSet` bileşenini kullanır:</span><span class="sxs-lookup"><span data-stu-id="28e46-356">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

```razor
<TabSet>
    <Tab Title="First tab">
        <h4>Greetings from the first tab!</h4>

        <label>
            <input type="checkbox" @bind="showThirdTab" />
            Toggle third tab
        </label>
    </Tab>
    <Tab Title="Second tab">
        <h4>The second tab says Hello World!</h4>
    </Tab>

    @if (showThirdTab)
    {
        <Tab Title="Third tab">
            <h4>Welcome to the disappearing third tab!</h4>
            <p>Toggle this tab from the first tab.</p>
        </Tab>
    }
</TabSet>
```

<span data-ttu-id="28e46-357">Alt `Tab` bileşenleri, `TabSet`açıkça parametre olarak geçirilmemektedir.</span><span class="sxs-lookup"><span data-stu-id="28e46-357">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="28e46-358">Bunun yerine, alt `Tab` bileşenleri `TabSet`alt içeriğinin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="28e46-358">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="28e46-359">Ancak, `TabSet` üst bilgileri ve etkin sekmeyi işleyebilmesi için, her bir `Tab` bileşeni hakkında yine de bilmesi gerekir. Ek kod gerektirmeden bu koordinasyonu etkinleştirmek için, `TabSet` bileşen kendisini, alt `Tab` bileşenleri tarafından çekilen *basamaklı bir değer olarak sağlayabilir* .</span><span class="sxs-lookup"><span data-stu-id="28e46-359">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="28e46-360">`TabSet` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="28e46-360">`TabSet` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="28e46-361">Alt `Tab` bileşenleri, kapsayan `TabSet` kendisini basamaklı bir parametre olarak yakalar, bu nedenle `Tab` bileşenleri kendilerini `TabSet` ve bu sekmenin etkin olduğu koordine eder.</span><span class="sxs-lookup"><span data-stu-id="28e46-361">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="28e46-362">`Tab` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="28e46-362">`Tab` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="28e46-363">Razor şablonları</span><span class="sxs-lookup"><span data-stu-id="28e46-363">Razor templates</span></span>

<span data-ttu-id="28e46-364">Oluşturma parçaları Razor şablonu sözdizimi kullanılarak tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-364">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="28e46-365">Razor şablonları, bir UI parçacığı tanımlamak ve aşağıdaki biçimi varsaymak için bir yoldur:</span><span class="sxs-lookup"><span data-stu-id="28e46-365">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="28e46-366">Aşağıdaki örnek, `RenderFragment` ve `RenderFragment<T>` değerlerinin nasıl belirtildiğini ve şablonlarının doğrudan bir bileşende nasıl işleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="28e46-366">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="28e46-367">Oluşturma parçaları, [şablonlu bileşenlere](xref:blazor/templated-components)bağımsız değişken olarak da geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="28e46-367">Render fragments can also be passed as arguments to [templated components](xref:blazor/templated-components).</span></span>

```razor
@_timeTemplate

@_petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment _timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> _petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="28e46-368">Önceki kodun işlenmiş çıktısı:</span><span class="sxs-lookup"><span data-stu-id="28e46-368">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="28e46-369">Ölçeklenebilir vektör grafik (SVG) görüntüleri</span><span class="sxs-lookup"><span data-stu-id="28e46-369">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="28e46-370">Blazor HTML oluşturduğundan, ölçeklenebilir vektör grafik (SVG) görüntüleri ( *. SVG*) dahil olmak üzere tarayıcıda desteklenen görüntüler `<img>` etiketi aracılığıyla desteklenir:</span><span class="sxs-lookup"><span data-stu-id="28e46-370">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="28e46-371">Benzer şekilde, SVG görüntüleri bir stil sayfası dosyasının ( *. css*) CSS kurallarında desteklenir:</span><span class="sxs-lookup"><span data-stu-id="28e46-371">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="28e46-372">Ancak, satır içi SVG işaretlemesi tüm senaryolarda desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="28e46-372">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="28e46-373">Bir `<svg>` etiketini doğrudan bir bileşen dosyasına ( *. Razor*) yerleştirirseniz, temel görüntü işleme desteklenir, ancak birçok gelişmiş senaryo desteklenmemiştir.</span><span class="sxs-lookup"><span data-stu-id="28e46-373">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="28e46-374">Örneğin, `<use>` Etiketler Şu anda dikkate alınamaz ve `@bind` bazı SVG etiketleriyle kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="28e46-374">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="28e46-375">Gelecekteki bir sürümde bu sınırlamaları ele almayı bekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="28e46-375">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="28e46-376">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="28e46-376">Additional resources</span></span>

* <span data-ttu-id="28e46-377"><xref:security/blazor/server> &ndash;, kaynak tükenmesi ile Çekişmek zorunda olan Blazor sunucu uygulamaları oluşturmaya yönelik yönergeler Içerir.</span><span class="sxs-lookup"><span data-stu-id="28e46-377"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
