---
title: ASP.NET Core Razor bileşenleri oluşturma ve kullanma
author: guardrex
description: Veri bağlama, olayları işleme ve bileşen yaşam döngülerini yönetme dahil Razor bileşenleri oluşturmayı ve kullanmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: blazor/components
ms.openlocfilehash: a71bbf3921417cbd23aeb14d0d78ad8354d6e93a
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378684"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="25053-103">ASP.NET Core Razor bileşenleri oluşturma ve kullanma</span><span class="sxs-lookup"><span data-stu-id="25053-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="25053-104">, [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="25053-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="25053-105">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="25053-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="25053-106">Blazor uygulamaları, *bileşenleri*kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="25053-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="25053-107">Bir bileşen, bir sayfa, iletişim veya form gibi bir kullanıcı arabirimi (UI) öbekidir.</span><span class="sxs-lookup"><span data-stu-id="25053-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="25053-108">Bir bileşen, veri eklemek veya UI olaylarına yanıt vermek için gereken HTML işaretlemesini ve işleme mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="25053-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="25053-109">Bileşenler esnek ve hafif.</span><span class="sxs-lookup"><span data-stu-id="25053-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="25053-110">Bunlar, iç içe geçmiş, yeniden kullanılabilir ve projeler arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="25053-111">Bileşen sınıfları</span><span class="sxs-lookup"><span data-stu-id="25053-111">Component classes</span></span>

<span data-ttu-id="25053-112">Bileşenler, C# ve HTML Işaretlemesi kullanılarak [Razor](xref:mvc/views/razor) bileşen dosyalarında ( *. Razor*) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="25053-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="25053-113">Blazor içindeki bir bileşen, bir *Razor bileşeni*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="25053-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="25053-114">Bir bileşenin adı, büyük harfle başlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="25053-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="25053-115">Örneğin, *mycoolcomponent. Razor* geçerlidir ve *mycoolcomponent. Razor* geçersizdir.</span><span class="sxs-lookup"><span data-stu-id="25053-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="25053-116">Bir bileşen için Kullanıcı arabirimi HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="25053-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="25053-117">Dinamik işleme mantığı (örneğin, döngüler, koşullar, ifadeler) C# [Razor](xref:mvc/views/razor)adlı gömülü bir sözdizimi kullanılarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="25053-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="25053-118">Bir uygulama derlendiğinde, HTML biçimlendirme ve C# işleme mantığı bir bileşen sınıfına dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="25053-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="25053-119">Oluşturulan sınıfın adı, dosyanın adıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="25053-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="25053-120">Bileşen sınıfının üyeleri `@code` bloğunda tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="25053-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="25053-121">@No__t-0 bloğunda, bileşen durumu (özellikler, alanlar) olay işleme yöntemleriyle veya diğer bileşen mantığını tanımlamaya yönelik yöntemlerle belirtilir.</span><span class="sxs-lookup"><span data-stu-id="25053-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="25053-122">Birden fazla `@code` bloğu izin verilir.</span><span class="sxs-lookup"><span data-stu-id="25053-122">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="25053-123">ASP.NET Core 3,0 ' nin önceki önizlemelerinde, `@functions` blokları Razor bileşenlerinde `@code` bloklarında aynı amaçla kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="25053-123">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="25053-124">`@functions` blokları Razor bileşenlerinde çalışmaya devam eder, ancak ASP.NET Core 3,0 Preview 6 veya sonraki bir sürümünde `@code` bloğunu kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="25053-124">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="25053-125">Bileşen üyeleri, `@` ile başlayan ifadeler kullanılarak C# bileşen işleme mantığının bir parçası olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-125">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="25053-126">Örneğin, bir C# alan `@` ' i alan adı ' na önek olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="25053-126">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="25053-127">Aşağıdaki örnek değerlendirilir ve işler:</span><span class="sxs-lookup"><span data-stu-id="25053-127">The following example evaluates and renders:</span></span>

* <span data-ttu-id="25053-128">`font-style` için CSS özellik değerine `_headingFontStyle`.</span><span class="sxs-lookup"><span data-stu-id="25053-128">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="25053-129">`<h1>` öğesinin içeriğine `_headingText`.</span><span class="sxs-lookup"><span data-stu-id="25053-129">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="25053-130">Bileşen ilk olarak işlendikten sonra, bileşen işleme ağacını olaylara yanıt olarak yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="25053-130">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="25053-131">Blazor ardından yeni işleme ağacını önceki bir ile karşılaştırır ve tarayıcının Belge Nesne Modeli (DOM) üzerinde herhangi bir değişiklik uygular.</span><span class="sxs-lookup"><span data-stu-id="25053-131">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="25053-132">Bileşenler sıradan C# sınıflardır ve bir proje içinde herhangi bir yere yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="25053-132">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="25053-133">Web sayfalarını üreten bileşenler genellikle *Sayfalar* klasöründe bulunur.</span><span class="sxs-lookup"><span data-stu-id="25053-133">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="25053-134">Sayfa olmayan bileşenler sıklıkla *paylaşılan* klasöre veya projeye eklenen özel bir klasöre yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="25053-134">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="25053-135">Özel bir klasör kullanmak için, özel klasörün ad alanını üst bileşene ya da uygulamanın *_ımports. Razor* dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="25053-135">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="25053-136">Örneğin, aşağıdaki ad alanı, uygulamanın kök ad alanı `WebApplication` olduğunda bir *Bileşenler* klasöründeki bileşenleri kullanılabilir yapar:</span><span class="sxs-lookup"><span data-stu-id="25053-136">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="25053-137">Bileşenleri Razor Pages ve MVC uygulamalarıyla tümleştirme</span><span class="sxs-lookup"><span data-stu-id="25053-137">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="25053-138">Mevcut Razor Pages ve MVC uygulamalarıyla bileşenleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="25053-138">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="25053-139">Razor bileşenleri kullanmak için mevcut sayfaları veya görünümleri yeniden yazmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="25053-139">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="25053-140">Sayfa veya görünüm işlendiğinde, bileşenler aynı anda önceden işlenir.</span><span class="sxs-lookup"><span data-stu-id="25053-140">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="25053-141">Bir sayfadan veya görünümden bir bileşeni işlemek için `RenderComponentAsync<TComponent>` HTML yardımcı yöntemini kullanın:</span><span class="sxs-lookup"><span data-stu-id="25053-141">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="MyComponent">
    @(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
</div>
```

<span data-ttu-id="25053-142">Sayfalar ve görünümler bileşenleri kullanırken, listesiyse doğru değildir.</span><span class="sxs-lookup"><span data-stu-id="25053-142">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="25053-143">Bileşenler, kısmi görünümler ve bölümler gibi görüntüleme ve sayfaya özgü senaryolar kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="25053-143">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="25053-144">Bir bileşende kısmi görünümden mantığı kullanmak için kısmi görünüm mantığını bir bileşene ayırın.</span><span class="sxs-lookup"><span data-stu-id="25053-144">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="25053-145">Bileşenlerin nasıl işlendiği ve bileşen durumunun Blazor sunucu uygulamalarında nasıl yönetildiği hakkında daha fazla bilgi için <xref:blazor/hosting-models> makalesine bakın.</span><span class="sxs-lookup"><span data-stu-id="25053-145">For more information on how components are rendered and component state is managed in Blazor Server apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="use-components"></a><span data-ttu-id="25053-146">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="25053-146">Use components</span></span>

<span data-ttu-id="25053-147">Bileşenler, HTML öğesi söz dizimini kullanarak bildirerek diğer bileşenleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="25053-147">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="25053-148">Bir bileşeni kullanmak için biçimlendirme, etiket adının bileşen türü olduğu bir HTML etiketi gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="25053-148">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="25053-149">Öznitelik bağlama büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="25053-149">Attribute binding is case sensitive.</span></span> <span data-ttu-id="25053-150">Örneğin, `@bind` geçerli ve `@Bind` geçersiz.</span><span class="sxs-lookup"><span data-stu-id="25053-150">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="25053-151">*Index. Razor* dosyasında aşağıdaki biçimlendirme `HeadingComponent` örneğini işler:</span><span class="sxs-lookup"><span data-stu-id="25053-151">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="25053-152">*Bileşenler/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="25053-152">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

<span data-ttu-id="25053-153">Bir bileşen bir bileşen adıyla eşleşmeyen büyük harfle yazılmış bir HTML öğesi içeriyorsa, öğenin beklenmeyen bir adı olduğunu gösteren bir uyarı yayınlanır.</span><span class="sxs-lookup"><span data-stu-id="25053-153">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="25053-154">Bileşenin ad alanı için `@using` ifadesinin eklenmesi, bileşenin kullanılabilir olmasını sağlar ve bu da uyarıyı kaldırır.</span><span class="sxs-lookup"><span data-stu-id="25053-154">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="25053-155">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="25053-155">Component parameters</span></span>

<span data-ttu-id="25053-156">Bileşenler, bileşen sınıfında `[Parameter]` özniteliğiyle ortak özellikler kullanılarak tanımlanan *bileşen parametrelerine*sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-156">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="25053-157">Biçimlendirme içindeki bir bileşenin bağımsız değişkenlerini belirtmek için öznitelikleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="25053-157">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="25053-158">*Bileşenler/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="25053-158">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="25053-159">Aşağıdaki örnekte, `ParentComponent` `ChildComponent` ' nin `Title` özelliğinin değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="25053-159">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="25053-160">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="25053-160">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="25053-161">Alt içerik</span><span class="sxs-lookup"><span data-stu-id="25053-161">Child content</span></span>

<span data-ttu-id="25053-162">Bileşenler, başka bir bileşenin içeriğini ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-162">Components can set the content of another component.</span></span> <span data-ttu-id="25053-163">Atama bileşeni, alıcı bileşeni belirten Etiketler arasında içerik sağlar.</span><span class="sxs-lookup"><span data-stu-id="25053-163">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="25053-164">Aşağıdaki örnekte, `ChildComponent` ' ı @no__t temsil eden bir `ChildContent` özelliği vardır. Bu bir kullanıcı arabirimi, işlemek için bir segmenti temsil eder.</span><span class="sxs-lookup"><span data-stu-id="25053-164">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="25053-165">@No__t-0 değeri, içeriğin işlenmesi gereken bileşenin biçimlendirmesinde konumlandırılır.</span><span class="sxs-lookup"><span data-stu-id="25053-165">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="25053-166">@No__t-0 değeri üst bileşenden alınır ve önyükleme panelinin `panel-body` ' in içinde işlenir.</span><span class="sxs-lookup"><span data-stu-id="25053-166">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="25053-167">*Bileşenler/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="25053-167">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="25053-168">@No__t-0 içeriğini alan özelliğin kurala göre `ChildContent` olarak adlandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="25053-168">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="25053-169">Aşağıdaki `ParentComponent`, içeriği `<ChildComponent>` etiketlerinin içine yerleştirerek `ChildComponent` ' i işlemek için içerik sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-169">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="25053-170">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="25053-170">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="25053-171">Öznitelik döndürme ve rastgele parametreler</span><span class="sxs-lookup"><span data-stu-id="25053-171">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="25053-172">Bileşenler, bileşen tarafından tanımlanan parametrelere ek olarak ek öznitelikler yakalayabilir ve işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="25053-172">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="25053-173">Ek öznitelikler bir sözlükte yakalanabilir *ve sonra bileşen* [@attributes](xref:mvc/views/razor#attributes) Razor yönergesi kullanılarak işlendiğinde bir öğe üzerine bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-173">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="25053-174">Bu senaryo, çeşitli özelleştirmeleri destekleyen bir işaretleme öğesi üreten bir bileşen tanımlarken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="25053-174">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="25053-175">Örneğin, çok sayıda parametreyi destekleyen bir `<input>` için öznitelikleri ayrı olarak tanımlamak sıkıcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-175">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="25053-176">Aşağıdaki örnekte, ilk `<input>` öğesi (`id="useIndividualParams"`) tek tek bileşen parametrelerini kullanır, ancak ikinci `<input>` öğesi (`id="useAttributesDict"`) öznitelik splatesini kullanır:</span><span class="sxs-lookup"><span data-stu-id="25053-176">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```cshtml
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

<span data-ttu-id="25053-177">Parametrenin türü dize anahtarlarıyla `IEnumerable<KeyValuePair<string, object>>` uygulamalıdır.</span><span class="sxs-lookup"><span data-stu-id="25053-177">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="25053-178">@No__t-0 kullanmak Bu senaryoda da bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="25053-178">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="25053-179">Her iki yaklaşımın de kullanıldığı işlenen @no__t 0 öğeleri aynıdır:</span><span class="sxs-lookup"><span data-stu-id="25053-179">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="25053-180">Rastgele öznitelikleri kabul etmek için, `CaptureUnmatchedValues` özelliği `true` ' ye ayarlanmış `[Parameter]` özniteliğini kullanarak bir bileşen parametresi tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="25053-180">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="25053-181">@No__t-1 ' deki `CaptureUnmatchedValues` özelliği, parametrenin diğer bir parametreyle eşleşmeyen tüm özniteliklerle eşleşmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="25053-181">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="25053-182">Bir bileşen yalnızca `CaptureUnmatchedValues` olan tek bir parametre tanımlayabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-182">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="25053-183">@No__t-0 ile kullanılan özellik türü, `Dictionary<string, object>` ' den dize anahtarlarıyla atanabilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="25053-183">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="25053-184">`IEnumerable<KeyValuePair<string, object>>` veya `IReadOnlyDictionary<string, object>` Ayrıca bu senaryodaki seçeneklerdir.</span><span class="sxs-lookup"><span data-stu-id="25053-184">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="25053-185">Veri bağlama</span><span class="sxs-lookup"><span data-stu-id="25053-185">Data binding</span></span>

<span data-ttu-id="25053-186">Hem bileşenlere hem de DOM öğelerine veri bağlama [@bind](xref:mvc/views/razor#bind) özniteliğiyle gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="25053-186">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="25053-187">Aşağıdaki örnek bir `CurrentValue` özelliğini metin kutusu değerine bağlar:</span><span class="sxs-lookup"><span data-stu-id="25053-187">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```cshtml
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="25053-188">Metin kutusu odağı kaybettiğinde, özelliğin değeri güncellenir.</span><span class="sxs-lookup"><span data-stu-id="25053-188">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="25053-189">Metin kutusu kullanıcı arabiriminde, özelliğin değerini değiştirme yanıt olarak değil, yalnızca bileşen işlendiğinde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="25053-189">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="25053-190">Bileşenler olay işleyicisi kodu yürütüldükten sonra kendilerini oluşturduğundan, özellik güncelleştirmeleri *genellikle* olay işleyicisi tetiklendikten hemen sonra Kullanıcı arabirimine yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="25053-190">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="25053-191">@No__t-0 ' i `CurrentValue` özelliği (`<input @bind="CurrentValue" />`) ile kullanmak, temelde aşağıdakilere eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="25053-191">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="25053-192">Bileşen işlendiğinde, giriş öğesinin `value` ' ı `CurrentValue` özelliğinden gelir.</span><span class="sxs-lookup"><span data-stu-id="25053-192">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="25053-193">Kullanıcı metin kutusuna yazdığında ve öğe odağını değiştirdiğinde, `onchange` olayı tetiklenir ve `CurrentValue` özelliği değiştirilen değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="25053-193">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="25053-194">@No__t-0 tür dönüştürmelerin gerçekleştirildiği durumları işlediği için, gerçekte kod oluşturma daha karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="25053-194">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="25053-195">İlke ' de, `@bind` bir ifadenin geçerli değerini bir `value` özniteliğiyle ilişkilendirir ve kayıtlı işleyiciyi kullanarak değişiklikleri işler.</span><span class="sxs-lookup"><span data-stu-id="25053-195">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="25053-196">@No__t-1 sözdizimiyle `onchange` olaylarını işlemenin yanı sıra, bir özellik veya alan, `event` parametresiyle bir [@bind-value](xref:mvc/views/razor#bind) özniteliği belirterek diğer olaylar kullanılarak da bağlanabilir ([@bind-value:event](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="25053-196">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="25053-197">Aşağıdaki örnek, `CurrentValue` özelliğini `oninput` olayı için bağlar:</span><span class="sxs-lookup"><span data-stu-id="25053-197">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="25053-198">@No__t-0 ' dan farklı olarak, öğe odağı kaybettiğinde harekete geçirilir `oninput`, metin kutusunun değeri değiştiğinde harekete geçirilir.</span><span class="sxs-lookup"><span data-stu-id="25053-198">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="25053-199">**Ayrıştırılamayan değerler**</span><span class="sxs-lookup"><span data-stu-id="25053-199">**Unparsable values**</span></span>

<span data-ttu-id="25053-200">Bir Kullanıcı, bir veri sınırlama öğesine ayrıştırılamayan bir değer sağlıyorsa, bağlama olayı tetiklendiğinde, çözümlenemeyen değer otomatik olarak önceki değerine döndürülür.</span><span class="sxs-lookup"><span data-stu-id="25053-200">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="25053-201">Aşağıdaki senaryoyu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="25053-201">Consider the following scenario:</span></span>

* <span data-ttu-id="25053-202">@No__t-0 öğesi, ilk değeri `123` olan `int` türüne bağlanır:</span><span class="sxs-lookup"><span data-stu-id="25053-202">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```cshtml
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="25053-203">Kullanıcı, öğe değerini sayfada `123.45` olarak güncelleştirir ve öğe odağını değiştirir.</span><span class="sxs-lookup"><span data-stu-id="25053-203">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="25053-204">Önceki senaryoda, öğenin değeri `123` ' a geri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="25053-204">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="25053-205">@No__t-0 değeri, `123` ' in özgün değeri yararına reddedildiğinde, Kullanıcı değerinin kabul edilmediğini anlamıştır.</span><span class="sxs-lookup"><span data-stu-id="25053-205">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="25053-206">Varsayılan olarak, bağlama öğenin `onchange` olayına uygulanır (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="25053-206">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="25053-207">Farklı bir olay ayarlamak için `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` kullanın.</span><span class="sxs-lookup"><span data-stu-id="25053-207">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="25053-208">@No__t-0 olayı (`@bind-value:event="oninput"`) için, yeniden sürüm, ayrıştırılamayan bir değer sunan herhangi bir tuş vuruşu sonrasında oluşur.</span><span class="sxs-lookup"><span data-stu-id="25053-208">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="25053-209">@No__t-0 olayını @no__t -1 ile bağlantılı bir türle hedeflerken, kullanıcının `.` karakteri yazmasının engellenmiş olması engellenir.</span><span class="sxs-lookup"><span data-stu-id="25053-209">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="25053-210">@No__t-0 karakteri hemen kaldırılır, bu nedenle Kullanıcı yalnızca tam sayılara izin verilen anında geri bildirim alır.</span><span class="sxs-lookup"><span data-stu-id="25053-210">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="25053-211">@No__t-0 olaylarındaki değerin geri döndürülebileceği senaryolar vardır; örneğin, kullanıcının çözümlenemeyen bir `<input>` değerini temizme izni verilmesi gerektiği durumlar gibi.</span><span class="sxs-lookup"><span data-stu-id="25053-211">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="25053-212">Alternatifler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="25053-212">Alternatives include:</span></span>

* <span data-ttu-id="25053-213">@No__t-0 olayını kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="25053-213">Don't use the `oninput` event.</span></span> <span data-ttu-id="25053-214">Öğe odağı kaybederene kadar geçersiz bir değer geri döndürülmediğinde, varsayılan `onchange` olayını (`@bind="{PROPERTY OR FIELD}"`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="25053-214">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="25053-215">@No__t-0 veya `string` gibi null yapılabilir bir türe bağlayın ve geçersiz girdileri işlemek için özel mantık sağlayın.</span><span class="sxs-lookup"><span data-stu-id="25053-215">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="25053-216">@No__t-1 veya `InputDate` gibi bir [form doğrulama bileşeni](xref:blazor/forms-validation)kullanın.</span><span class="sxs-lookup"><span data-stu-id="25053-216">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="25053-217">Form doğrulama bileşenlerinde geçersiz girişleri yönetmek için yerleşik destek vardır.</span><span class="sxs-lookup"><span data-stu-id="25053-217">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="25053-218">Form doğrulama bileşenleri:</span><span class="sxs-lookup"><span data-stu-id="25053-218">Form validation components:</span></span>
  * <span data-ttu-id="25053-219">Kullanıcının, ilişkili `EditContext` ' da geçersiz giriş sağlamasına ve doğrulama hataları almasına izin verin.</span><span class="sxs-lookup"><span data-stu-id="25053-219">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="25053-220">Kullanıcı ek WebForm verisi girmeye uğramadan doğrulama hatalarını Kullanıcı ARABIRIMINDE görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="25053-220">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="25053-221">**Genelleştirme**</span><span class="sxs-lookup"><span data-stu-id="25053-221">**Globalization**</span></span>

<span data-ttu-id="25053-222">`@bind` değerleri, geçerli kültürün kuralları kullanılarak görüntülenmek üzere biçimlendirilir ve ayrıştırılır.</span><span class="sxs-lookup"><span data-stu-id="25053-222">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="25053-223">Geçerli kültüre <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> özelliğinden erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="25053-223">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="25053-224">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) aşağıdaki alan türleri için kullanılır (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="25053-224">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="25053-225">Yukarıdaki alan türleri:</span><span class="sxs-lookup"><span data-stu-id="25053-225">The preceding field types:</span></span>

* <span data-ttu-id="25053-226">, Uygun tarayıcı tabanlı biçimlendirme kuralları kullanılarak görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="25053-226">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="25053-227">Serbest biçimli metin içeremez.</span><span class="sxs-lookup"><span data-stu-id="25053-227">Can't contain free-form text.</span></span>
* <span data-ttu-id="25053-228">Tarayıcının uygulamasına göre Kullanıcı etkileşimi özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="25053-228">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="25053-229">Aşağıdaki alan türleri özel biçimlendirme gereksinimlerine sahiptir ve şu anda tüm büyük tarayıcılarda desteklenmediği için Blazor tarafından desteklenmemektedir:</span><span class="sxs-lookup"><span data-stu-id="25053-229">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="25053-230">`@bind`, bir değeri ayrıştırmak ve biçimlendirmek için <xref:System.Globalization.CultureInfo?displayProperty=fullName> sağlamak üzere `@bind:culture` parametresini destekler.</span><span class="sxs-lookup"><span data-stu-id="25053-230">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="25053-231">@No__t-0 ve `number` alan türleri kullanılırken bir kültür belirtilmesi önerilmez.</span><span class="sxs-lookup"><span data-stu-id="25053-231">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="25053-232">`date` ve `number`, gerekli kültürü sağlayan yerleşik Blazor desteğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="25053-232">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="25053-233">Kullanıcının kültürünü ayarlama hakkında daha fazla bilgi için [Yerelleştirme](#localization) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="25053-233">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="25053-234">**Biçim dizeleri**</span><span class="sxs-lookup"><span data-stu-id="25053-234">**Format strings**</span></span>

<span data-ttu-id="25053-235">Veri bağlama, [@bind:format](xref:mvc/views/razor#bind)kullanılarak <xref:System.DateTime> biçim dizeleriyle birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-235">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="25053-236">Para birimi veya sayı biçimleri gibi diğer biçim ifadeleri şu anda kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="25053-236">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="25053-237">Yukarıdaki kodda `<input>` öğesinin alan türü (`type`) varsayılan olarak `text` ' dir.</span><span class="sxs-lookup"><span data-stu-id="25053-237">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="25053-238">`@bind:format`, aşağıdaki .NET türlerini bağlamak için desteklenir:</span><span class="sxs-lookup"><span data-stu-id="25053-238">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="25053-239"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="25053-239"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="25053-240"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="25053-240"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="25053-241">@No__t-0 özniteliği, `<input>` öğesinin `value` ' e uygulanacak tarih biçimini belirtir.</span><span class="sxs-lookup"><span data-stu-id="25053-241">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="25053-242">Biçim, bir `onchange` olayı gerçekleştiğinde değeri ayrıştırmak için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="25053-242">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="25053-243">Blazor 'in tarihleri biçimlendirmek için yerleşik desteği olduğundan `date` alan türü için biçim belirtme önerilmez.</span><span class="sxs-lookup"><span data-stu-id="25053-243">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="25053-244">**Bileşen parametreleri**</span><span class="sxs-lookup"><span data-stu-id="25053-244">**Component parameters**</span></span>

<span data-ttu-id="25053-245">Bağlama, `@bind-{property}` ' ın bileşenler arasında bir özellik değeri bağlayabileceği bileşen parametrelerini tanır.</span><span class="sxs-lookup"><span data-stu-id="25053-245">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="25053-246">Aşağıdaki alt bileşende (`ChildComponent`) bir `Year` bileşen parametresi ve `YearChanged` geri çağırması vardır:</span><span class="sxs-lookup"><span data-stu-id="25053-246">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="25053-247">`EventCallback<T>`, [Eventcallback](#eventcallback) bölümünde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="25053-247">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="25053-248">Aşağıdaki üst bileşen `ChildComponent` kullanır ve `ParentYear` parametresini alt bileşendeki `Year` parametresine bağlar:</span><span class="sxs-lookup"><span data-stu-id="25053-248">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="25053-249">@No__t yüklemek-0 aşağıdaki biçimlendirmeyi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="25053-249">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="25053-250">@No__t-0 özelliğinin değeri `ParentComponent` ' deki düğme seçilerek değiştirilirse, `ChildComponent` ' ün `Year` özelliği güncellenir.</span><span class="sxs-lookup"><span data-stu-id="25053-250">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="25053-251">@No__t-0 ' ın yeni değeri, `ParentComponent` tekrar eklendiğinde Kullanıcı arabiriminde işlenir:</span><span class="sxs-lookup"><span data-stu-id="25053-251">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="25053-252">@No__t-0 parametresi, `Year` parametresinin türüyle eşleşen bir yardımcı `YearChanged` olayına sahip olduğundan bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-252">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="25053-253">Kurala göre `<ChildComponent @bind-Year="ParentYear" />` temelde yazmaya eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="25053-253">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="25053-254">Genel olarak, bir özellik `@bind-property:event` özniteliği kullanılarak karşılık gelen bir olay işleyicisine bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-254">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="25053-255">Örneğin, `MyProp` özelliği aşağıdaki iki öznitelik kullanılarak `MyEventHandler` ' e bağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="25053-255">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="25053-256">Olay işleme</span><span class="sxs-lookup"><span data-stu-id="25053-256">Event handling</span></span>

<span data-ttu-id="25053-257">Razor bileşenleri olay işleme özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="25053-257">Razor components provide event handling features.</span></span> <span data-ttu-id="25053-258">Temsilci türü belirtilmiş bir değerle `on{event}` (örneğin, `onclick` ve `onsubmit`) adlı bir HTML öğesi özniteliği için, Razor bileşenleri özniteliğin değerini bir olay işleyicisi olarak değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="25053-258">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="25053-259">Özniteliğin adı her zaman [@on {Event}](xref:mvc/views/razor#onevent)olarak biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="25053-259">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="25053-260">Aşağıdaki kod, Kullanıcı arabiriminde düğme seçildiğinde `UpdateHeading` yöntemini çağırır:</span><span class="sxs-lookup"><span data-stu-id="25053-260">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="25053-261">Aşağıdaki kod, Kullanıcı arabiriminde onay kutusu değiştirildiğinde `CheckChanged` yöntemini çağırır:</span><span class="sxs-lookup"><span data-stu-id="25053-261">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="25053-262">Olay işleyicileri Ayrıca zaman uyumsuz olabilir ve bir @no__t döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="25053-262">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="25053-263">@No__t-0 ' yı el ile çağırmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="25053-263">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="25053-264">Özel durumlar oluştuğunda günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="25053-264">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="25053-265">Aşağıdaki örnekte, düğme seçildiğinde `UpdateHeading` zaman uyumsuz olarak çağrılır:</span><span class="sxs-lookup"><span data-stu-id="25053-265">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a><span data-ttu-id="25053-266">Olay bağımsız değişken türleri</span><span class="sxs-lookup"><span data-stu-id="25053-266">Event argument types</span></span>

<span data-ttu-id="25053-267">Bazı olaylar için olay bağımsız değişkeni türlerine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="25053-267">For some events, event argument types are permitted.</span></span> <span data-ttu-id="25053-268">Bu olay türlerinden birine erişim gerekmiyorsa, yöntem çağrısında gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="25053-268">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="25053-269">Desteklenen `EventArgs` aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="25053-269">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="25053-270">Olay</span><span class="sxs-lookup"><span data-stu-id="25053-270">Event</span></span> | <span data-ttu-id="25053-271">örneği</span><span class="sxs-lookup"><span data-stu-id="25053-271">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="25053-272">Pano</span><span class="sxs-lookup"><span data-stu-id="25053-272">Clipboard</span></span>        | `ClipboardEventArgs` |
| <span data-ttu-id="25053-273">Sürükleyin</span><span class="sxs-lookup"><span data-stu-id="25053-273">Drag</span></span>             | <span data-ttu-id="25053-274">`DragEventArgs` &ndash; `DataTransfer` ve @no__t 3 saklama öğe verilerini sürüklemiş.</span><span class="sxs-lookup"><span data-stu-id="25053-274">`DragEventArgs` &ndash; `DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="25053-275">Hata</span><span class="sxs-lookup"><span data-stu-id="25053-275">Error</span></span>            | `ErrorEventArgs` |
| <span data-ttu-id="25053-276">Çı</span><span class="sxs-lookup"><span data-stu-id="25053-276">Focus</span></span>            | <span data-ttu-id="25053-277">`FocusEventArgs` &ndash; `relatedTarget` desteğini içermez.</span><span class="sxs-lookup"><span data-stu-id="25053-277">`FocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="25053-278">`<input>` değişiklik</span><span class="sxs-lookup"><span data-stu-id="25053-278">`<input>` change</span></span> | `ChangeEventArgs` |
| <span data-ttu-id="25053-279">Klavye</span><span class="sxs-lookup"><span data-stu-id="25053-279">Keyboard</span></span>         | `KeyboardEventArgs` |
| <span data-ttu-id="25053-280">Tığında</span><span class="sxs-lookup"><span data-stu-id="25053-280">Mouse</span></span>            | `MouseEventArgs` |
| <span data-ttu-id="25053-281">Fare işaretçisi</span><span class="sxs-lookup"><span data-stu-id="25053-281">Mouse pointer</span></span>    | `PointerEventArgs` |
| <span data-ttu-id="25053-282">Fare tekerleği</span><span class="sxs-lookup"><span data-stu-id="25053-282">Mouse wheel</span></span>      | `WheelEventArgs` |
| <span data-ttu-id="25053-283">İlerleme durumu</span><span class="sxs-lookup"><span data-stu-id="25053-283">Progress</span></span>         | `ProgressEventArgs` |
| <span data-ttu-id="25053-284">Dokunma</span><span class="sxs-lookup"><span data-stu-id="25053-284">Touch</span></span>            | <span data-ttu-id="25053-285">`TouchEventArgs` &ndash; `TouchPoint`, dokunmaya duyarlı bir cihazdaki tek bir iletişim noktasını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="25053-285">`TouchEventArgs` &ndash; `TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="25053-286">Önceki tablodaki olayların özellikleri ve olay işleme davranışı hakkında bilgi için bkz. [başvuru kaynağında EventArgs sınıfları (ASPNET/AspNetCore Release/3.0 dalı)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="25053-286">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="25053-287">Lambda ifadeleri</span><span class="sxs-lookup"><span data-stu-id="25053-287">Lambda expressions</span></span>

<span data-ttu-id="25053-288">Lambda ifadeleri de kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="25053-288">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="25053-289">Genellikle, bir dizi öğe üzerinde yineleme yaparken olduğu gibi ek değerlerin üzerinde kapatılabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-289">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="25053-290">Aşağıdaki örnek, her biri Kullanıcı arabiriminde seçildiğinde `UpdateHeading` ' ı bir olay bağımsız değişkenini (`MouseEventArgs`) ve düğme numarasını (`buttonNumber`) geçen üç düğme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="25053-290">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="25053-291">Döngü değişkenini (`i`) doğrudan bir lambda ifadesinde bir `for` **döngüsünde kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="25053-291">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="25053-292">Aksi halde aynı değişken tüm lambda ifadeleri tarafından, `i` ' ın değerinin tüm Lambdalar ile aynı olmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="25053-292">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="25053-293">Her zaman değerini yerel bir değişkende yakala (önceki örnekte `buttonNumber`) ve ardından kullanın.</span><span class="sxs-lookup"><span data-stu-id="25053-293">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="25053-294">EventCallback</span><span class="sxs-lookup"><span data-stu-id="25053-294">EventCallback</span></span>

<span data-ttu-id="25053-295">İç içe bileşenler içeren yaygın bir senaryo, alt bileşen olayı olduğunda bir üst bileşen yönteminin (örneğin, bir `onclick`) bir ' no__t-0' ı bir olay gerçekleştiği zaman çalıştırılması yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="25053-295">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="25053-296">Olayları bileşenler arasında ortaya çıkarmak için `EventCallback` kullanın.</span><span class="sxs-lookup"><span data-stu-id="25053-296">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="25053-297">Bir üst bileşen, bir alt bileşenin `EventCallback` ' a bir geri çağırma yöntemi atayabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-297">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="25053-298">Örnek uygulamadaki `ChildComponent` ' ın, bir düğmenin `onclick` işleyicisinin, örneğin `ParentComponent` ' ten `EventCallback` temsilcisini almak üzere nasıl ayarlandığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="25053-298">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="25053-299">@No__t-0 `MouseEventArgs` ile yazılır ve bu, çevrebirim cihazından gelen @no__t 2 olayına uygundur:</span><span class="sxs-lookup"><span data-stu-id="25053-299">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="25053-300">@No__t-0, çocuğun `EventCallback<T>` ' i `ShowMessage` yöntemine ayarlar:</span><span class="sxs-lookup"><span data-stu-id="25053-300">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="25053-301">Düğme @no__t seçildiğinde-0:</span><span class="sxs-lookup"><span data-stu-id="25053-301">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="25053-302">@No__t-0 ' ın `ShowMessage` yöntemi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="25053-302">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="25053-303">`messageText`, `ParentComponent` ' de güncellenir ve görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="25053-303">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="25053-304">Geri çağırma yönteminde `StateHasChanged` çağrısı gerekli değildir (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="25053-304">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="25053-305">`StateHasChanged` `ParentComponent` ' i yeniden çalıştırmak için otomatik olarak çağrılır. Örneğin, alt olaylar, alt öğe içinde yürütülen olay işleyicilerinde bileşen rerendering tetikler.</span><span class="sxs-lookup"><span data-stu-id="25053-305">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="25053-306">`EventCallback` ve `EventCallback<T>` zaman uyumsuz temsilcilere izin verir.</span><span class="sxs-lookup"><span data-stu-id="25053-306">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="25053-307">`EventCallback<T>` kesin bir şekilde türdedir ve belirli bir bağımsız değişken türü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="25053-307">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="25053-308">`EventCallback`, kesin olarak yazılmış ve herhangi bir bağımsız değişken türüne izin veriyor.</span><span class="sxs-lookup"><span data-stu-id="25053-308">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="25053-309">@No__t-0 veya `EventCallback<T>` ' i `InvokeAsync` ile çağırın ve <xref:System.Threading.Tasks.Task> ' i await:</span><span class="sxs-lookup"><span data-stu-id="25053-309">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="25053-310">Olay işleme ve bağlama bileşeni parametrelerini `EventCallback` ve `EventCallback<T>` kullanın.</span><span class="sxs-lookup"><span data-stu-id="25053-310">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="25053-311">@No__t-1 üzerinde türü kesin belirlenmiş `EventCallback<T>` tercih edin.</span><span class="sxs-lookup"><span data-stu-id="25053-311">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="25053-312">`EventCallback<T>`, bileşenin kullanıcılarına daha iyi hata geri bildirimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="25053-312">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="25053-313">Diğer UI olay işleyicileriyle benzer şekilde, olay parametresini belirtmek isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="25053-313">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="25053-314">Geri çağırmaya hiçbir değer geçirilmemişse `EventCallback` kullanın.</span><span class="sxs-lookup"><span data-stu-id="25053-314">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="chained-bind"></a><span data-ttu-id="25053-315">Zincirleme bağlama</span><span class="sxs-lookup"><span data-stu-id="25053-315">Chained bind</span></span>

<span data-ttu-id="25053-316">Yaygın bir senaryo, bir veri bağlama parametresini bileşen çıkışında bir sayfa öğesine zincirlemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="25053-316">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="25053-317">Birden çok bağlama düzeyi aynı anda gerçekleştiğinden, bu senaryoya *zincirleme bağlama* denir.</span><span class="sxs-lookup"><span data-stu-id="25053-317">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="25053-318">Bir zincir bağlama, sayfanın öğesinde `@bind` sözdizimi ile uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="25053-318">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="25053-319">Olay işleyicisi ve değeri ayrı olarak belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="25053-319">The event handler and value must be specified separately.</span></span> <span data-ttu-id="25053-320">Ancak bir üst bileşen, bileşen parametresiyle `@bind` sözdizimini kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-320">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="25053-321">Aşağıdaki `PasswordField` bileşeni (*Passwordfield. Razor*):</span><span class="sxs-lookup"><span data-stu-id="25053-321">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="25053-322">@No__t-0 öğesinin değerini bir `Password` özelliğine ayarlar.</span><span class="sxs-lookup"><span data-stu-id="25053-322">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="25053-323">@No__t-0 özelliğindeki değişiklikleri [Eventcallback](#eventcallback)ile bir üst bileşene gösterir.</span><span class="sxs-lookup"><span data-stu-id="25053-323">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

<span data-ttu-id="25053-324">@No__t-0 bileşeni başka bir bileşende kullanılır:</span><span class="sxs-lookup"><span data-stu-id="25053-324">The `PasswordField` component is used in another component:</span></span>

```cshtml
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

<span data-ttu-id="25053-325">Önceki örnekteki parolada denetim veya tuzak hataları gerçekleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="25053-325">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="25053-326">@No__t-0 (Aşağıdaki örnek kodda `password`) için bir yedekleme alanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="25053-326">Create a backing field for `Password` (`password` in the following example code).</span></span>
* <span data-ttu-id="25053-327">@No__t-0 ayarlayıcısından denetimleri veya yakalama hatalarını gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="25053-327">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="25053-328">Aşağıdaki örnek, parolanın değerinde bir boşluk kullanılmışsa kullanıcıya anında geri bildirim sağlar:</span><span class="sxs-lookup"><span data-stu-id="25053-328">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@validationMessage</span>

@code {
    private bool showPassword;
    private string password;
    private string validationMessage;

    [Parameter]
    public string Password
    {
        get { return password ?? string.Empty; }
        set
        {
            if (password != value)
            {
                if (value.Contains(' '))
                {
                    validationMessage = "Spaces not allowed!";
                }
                else
                {
                    password = value;
                    validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

## <a name="capture-references-to-components"></a><span data-ttu-id="25053-329">Bileşenlere başvuruları yakala</span><span class="sxs-lookup"><span data-stu-id="25053-329">Capture references to components</span></span>

<span data-ttu-id="25053-330">Bileşen başvuruları bir bileşen örneğine başvurmak için bir yol sağlar, böylece bu örneğe `Show` veya `Reset` gibi komutlar verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="25053-330">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="25053-331">Bir bileşen başvurusunu yakalamak için:</span><span class="sxs-lookup"><span data-stu-id="25053-331">To capture a component reference:</span></span>

* <span data-ttu-id="25053-332">Alt bileşene bir [@ref](xref:mvc/views/razor#ref) özniteliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="25053-332">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="25053-333">Alt bileşenle aynı türde bir alan tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="25053-333">Define a field with the same type as the child component.</span></span>

```cshtml
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="25053-334">Bileşen işlendiğinde `loginDialog` alanı `MyLoginDialog` alt bileşen örneğiyle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="25053-334">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="25053-335">Daha sonra bileşen örneğinde .NET yöntemlerini çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="25053-335">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="25053-336">@No__t-0 değişkeni yalnızca bileşen işlendikten sonra ve çıktısı `MyLoginDialog` öğesini içerdiğinde doldurulur.</span><span class="sxs-lookup"><span data-stu-id="25053-336">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="25053-337">Bu noktaya kadar başvurulmasına hiçbir şey yok.</span><span class="sxs-lookup"><span data-stu-id="25053-337">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="25053-338">Bileşen işlemesini tamamladıktan sonra bileşen başvurularını işlemek için [Onafterrenderasync veya OnAfterRender yöntemlerini](#lifecycle-methods)kullanın.</span><span class="sxs-lookup"><span data-stu-id="25053-338">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](#lifecycle-methods).</span></span>

<span data-ttu-id="25053-339">Bileşen başvurularını yakalama, [öğe başvurularını yakalamak](xref:blazor/javascript-interop#capture-references-to-elements)için benzer bir sözdizimi kullanın, bir [JavaScript birlikte çalışma](xref:blazor/javascript-interop) özelliği değildir.</span><span class="sxs-lookup"><span data-stu-id="25053-339">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="25053-340">Bileşen başvuruları, yalnızca .NET kodunda kullanılan @ no__t-0thei JavaScript koduna aktarılmaz.</span><span class="sxs-lookup"><span data-stu-id="25053-340">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="25053-341">Alt bileşenlerin durumunu bulunmamalıdır için bileşen **başvurularını kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="25053-341">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="25053-342">Bunun yerine, alt bileşenlere veri geçirmek için normal bildirime dayalı parametreleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="25053-342">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="25053-343">Normal bildirime dayalı parametrelerin kullanımı, otomatik olarak doğru zamanların yeniden yönlendirmesi için alt bileşenlerde oluşur.</span><span class="sxs-lookup"><span data-stu-id="25053-343">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="25053-344">Durumu güncelleştirmek için bileşen yöntemlerini dışarıdan çağır</span><span class="sxs-lookup"><span data-stu-id="25053-344">Invoke component methods externally to update state</span></span>

<span data-ttu-id="25053-345">Blazor, yürütmenin tek bir mantıksal iş parçacığını zorlamak için `SynchronizationContext` kullanır.</span><span class="sxs-lookup"><span data-stu-id="25053-345">Blazor uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="25053-346">Bir bileşenin yaşam döngüsü yöntemleri ve Blazor tarafından oluşturulan tüm olay geri çağırmaları bu @no__t yürütülür-0.</span><span class="sxs-lookup"><span data-stu-id="25053-346">A component's lifecycle methods and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="25053-347">Bir bileşenin, Zamanlayıcı veya diğer bildirimler gibi dış bir olaya göre güncellenmesi gerekir, bu, @no__t Blazor ' a gönderilen `InvokeAsync` yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="25053-347">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="25053-348">Örneğin, güncelleştirilmiş durumdaki herhangi bir dinleme bileşenine bildirimde bulunan bir *bildirim hizmeti* düşünün:</span><span class="sxs-lookup"><span data-stu-id="25053-348">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="25053-349">Bir bileşeni güncelleştirmek için `NotifierService` kullanımı:</span><span class="sxs-lookup"><span data-stu-id="25053-349">Usage of the `NotifierService` to update a component:</span></span>

```cshtml
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="25053-350">Yukarıdaki örnekte, `NotifierService` bileşenin `OnNotify` yöntemini Blazor `SynchronizationContext` dışında çağırır.</span><span class="sxs-lookup"><span data-stu-id="25053-350">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="25053-351">`InvokeAsync`, doğru bağlama geçmek ve bir işlemeyi kuyruğa almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="25053-351">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="25053-352">Öğelerin ve bileşenlerin korunmasını denetlemek için \@key kullanın</span><span class="sxs-lookup"><span data-stu-id="25053-352">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="25053-353">Bir öğe veya bileşen listesi işlenirken ve öğeler ya da bileşenler daha sonra değiştiğinde, Blazor 'in yayılma algoritması, önceki öğelerin veya bileşenlerin ne zaman tutulacağına ve model nesnelerinin bunlara nasıl eşleneceğine karar vermelidir.</span><span class="sxs-lookup"><span data-stu-id="25053-353">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="25053-354">Normalde, bu işlem otomatiktir ve yoksayılabilir, ancak işlemi denetlemek isteyebileceğiniz durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="25053-354">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="25053-355">Aşağıdaki örneği göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="25053-355">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="25053-356">@No__t-0 koleksiyonunun içeriği, ekli, silinmiş veya yeniden sıralanmış girdilerle değişebilir.</span><span class="sxs-lookup"><span data-stu-id="25053-356">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="25053-357">Bileşen yeniden oluşturulduğunda, `<DetailsEditor>` bileşeni farklı `Details` parametre değerleri almak için değişebilir.</span><span class="sxs-lookup"><span data-stu-id="25053-357">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="25053-358">Bu, beklenenden daha karmaşık rerendering oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-358">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="25053-359">Bazı durumlarda rerendering, kayıp öğe odağı gibi görünür davranış farklılıklarına yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-359">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="25053-360">Eşleme işlemi `@key` yönergesi özniteliğiyle denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="25053-360">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="25053-361">`@key`, anahtar değerine göre öğelerin veya bileşenlerin korunmasını güvence altına almak için dağıtılmış algoritmaya neden olur:</span><span class="sxs-lookup"><span data-stu-id="25053-361">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="25053-362">@No__t-0 koleksiyonu değiştiğinde, yayılma algoritması `<DetailsEditor>` örnekleri ile `person` örnekleri arasındaki ilişkilendirmeyi korur:</span><span class="sxs-lookup"><span data-stu-id="25053-362">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="25053-363">@No__t-0 `People` listesinden silinirse, yalnızca ilgili `<DetailsEditor>` örneği kullanıcı arabiriminden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="25053-363">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="25053-364">Diğer örnekler değişmeden bırakılır.</span><span class="sxs-lookup"><span data-stu-id="25053-364">Other instances are left unchanged.</span></span>
* <span data-ttu-id="25053-365">@No__t-0 listedeki bir konuma eklenirse, ilgili konuma yeni bir `<DetailsEditor>` örnek eklenir.</span><span class="sxs-lookup"><span data-stu-id="25053-365">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="25053-366">Diğer örnekler değişmeden bırakılır.</span><span class="sxs-lookup"><span data-stu-id="25053-366">Other instances are left unchanged.</span></span>
* <span data-ttu-id="25053-367">@No__t-0 girdileri yeniden sıralandıysanız, karşılık gelen `<DetailsEditor>` örnekleri korunur ve Kullanıcı arabiriminde yeniden sıralanır.</span><span class="sxs-lookup"><span data-stu-id="25053-367">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="25053-368">Bazı senaryolarda `@key` kullanımı, rerendering karmaşıklığını en aza indirir ve odak konumu gibi DOM 'ın durum bilgisi olan kısımlarıyla ilgili olası sorunları önler.</span><span class="sxs-lookup"><span data-stu-id="25053-368">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="25053-369">Anahtarlar her kapsayıcı öğesi veya bileşeni için yereldir.</span><span class="sxs-lookup"><span data-stu-id="25053-369">Keys are local to each container element or component.</span></span> <span data-ttu-id="25053-370">Anahtarlar belge genelinde küresel olarak karşılaştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="25053-370">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="25053-371">@No__t-0key ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="25053-371">When to use \@key</span></span>

<span data-ttu-id="25053-372">Genellikle, bir liste işlendiğinde (örneğin, bir `@foreach` bloğunda) ve `@key` tanımlamak için uygun bir değer olduğunda `@key` ' ı kullanmak mantıklı olur.</span><span class="sxs-lookup"><span data-stu-id="25053-372">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="25053-373">Bir nesne değiştiğinde Blazor 'in bir öğeyi veya bileşen alt ağacını engellemesini engellemek için `@key` ' yı da kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="25053-373">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="25053-374">@No__t-0 değişirse `@key` öznitelik yönergesi, Blazor tüm `<div>` ve alt öğelerini atmayı ve yeni öğeler ve bileşenlerle Kullanıcı arabiriminde alt ağacı yeniden oluşturmayı zorlar.</span><span class="sxs-lookup"><span data-stu-id="25053-374">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="25053-375">Bu, `@currentPerson` değiştiğinde hiçbir Kullanıcı arabirimi durumunun korunmayacağını garanti etmeniz gerektiğinde yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-375">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="25053-376">@No__t-0key ne zaman kullanılmaz</span><span class="sxs-lookup"><span data-stu-id="25053-376">When not to use \@key</span></span>

<span data-ttu-id="25053-377">@No__t-0 ile dağıtılmış bir performans maliyeti vardır.</span><span class="sxs-lookup"><span data-stu-id="25053-377">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="25053-378">Performans maliyeti büyük değildir, ancak öğe veya bileşen koruma kuralları denetlenmediğinde yalnızca `@key` belirtin.</span><span class="sxs-lookup"><span data-stu-id="25053-378">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="25053-379">@No__t-0 kullanılmasa bile, Blazor alt öğe ve bileşen örneklerini mümkün olduğunca korur.</span><span class="sxs-lookup"><span data-stu-id="25053-379">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="25053-380">@No__t-0 ' ı kullanmanın tek avantajı, model örneklerinin eşlemeyi seçme algoritması yerine, korunan bileşen örneklerine *nasıl* eşlendiğine ilişkin bir denetimdir.</span><span class="sxs-lookup"><span data-stu-id="25053-380">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="25053-381">@No__t-0key için kullanılacak değerler</span><span class="sxs-lookup"><span data-stu-id="25053-381">What values to use for \@key</span></span>

<span data-ttu-id="25053-382">Genellikle, `@key` için aşağıdaki değer türlerinden birini sağlamak mantıklı olur:</span><span class="sxs-lookup"><span data-stu-id="25053-382">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="25053-383">Model nesne örnekleri (örneğin, önceki örnekte olduğu gibi `Person` örneği).</span><span class="sxs-lookup"><span data-stu-id="25053-383">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="25053-384">Bu, nesne başvurusu eşitliğine göre koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="25053-384">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="25053-385">Benzersiz tanımlayıcılar (örneğin, `int`, `string` veya `Guid`) türündeki birincil anahtar değerleri.</span><span class="sxs-lookup"><span data-stu-id="25053-385">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="25053-386">@No__t-0 için kullanılan değerlerin çakışmayın olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="25053-386">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="25053-387">Aynı üst öğe içinde çakışan değerler algılanırsa, eski öğeleri veya bileşenleri yeni öğe veya bileşenlere kesin bir şekilde eşlemediğinden Blazor bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="25053-387">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="25053-388">Yalnızca nesne örnekleri veya birincil anahtar değerleri gibi farklı değerleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="25053-388">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="25053-389">Yaşam döngüsü yöntemleri</span><span class="sxs-lookup"><span data-stu-id="25053-389">Lifecycle methods</span></span>

<span data-ttu-id="25053-390">`OnInitializedAsync` ve `OnInitialized` bileşeni başlatmak için kodu yürütün.</span><span class="sxs-lookup"><span data-stu-id="25053-390">`OnInitializedAsync` and `OnInitialized` execute code to initialize the component.</span></span> <span data-ttu-id="25053-391">Zaman uyumsuz bir işlem gerçekleştirmek için işlem üzerinde `OnInitializedAsync` ve `await` anahtar sözcüğünü kullanın:</span><span class="sxs-lookup"><span data-stu-id="25053-391">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="25053-392">Bileşen başlatma sırasında zaman uyumsuz çalışma `OnInitializedAsync` yaşam döngüsü olayı sırasında gerçekleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="25053-392">Asynchronous work during component initialization must occur during the `OnInitializedAsync` lifecycle event.</span></span>

<span data-ttu-id="25053-393">Zaman uyumlu bir işlem için `OnInitialized` kullanın:</span><span class="sxs-lookup"><span data-stu-id="25053-393">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="25053-394">`OnParametersSetAsync` ve `OnParametersSet`, bir bileşen üst öğeden parametreleri aldığında ve değerler özelliklere atandığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="25053-394">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="25053-395">Bu yöntemler bileşen başlatıldıktan sonra ve bileşen her işlendiğinde yürütülür:</span><span class="sxs-lookup"><span data-stu-id="25053-395">These methods are executed after component initialization and each time the component is rendered:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="25053-396">Parametre ve özellik değerleri uygulanırken zaman uyumsuz iş `OnParametersSetAsync` yaşam döngüsü olayı sırasında gerçekleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="25053-396">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="25053-397">`OnAfterRenderAsync` ve `OnAfterRender` bir bileşen işlemeyi tamamladıktan sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="25053-397">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="25053-398">Öğe ve bileşen başvuruları bu noktada doldurulur.</span><span class="sxs-lookup"><span data-stu-id="25053-398">Element and component references are populated at this point.</span></span> <span data-ttu-id="25053-399">İşlenmiş DOM öğelerinde çalışan üçüncü taraf JavaScript kitaplıklarını etkinleştirme gibi, işlenmiş içeriği kullanarak ek başlatma adımları gerçekleştirmek için bu aşamayı kullanın.</span><span class="sxs-lookup"><span data-stu-id="25053-399">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="25053-400">`OnAfterRender` *sunucuda prerendering çağrıldığında çağrılmaz.*</span><span class="sxs-lookup"><span data-stu-id="25053-400">`OnAfterRender` *isn't called when prerendering on the server.*</span></span>

<span data-ttu-id="25053-401">@No__t-1 ve `OnAfterRender` için `firstRender` parametresi:</span><span class="sxs-lookup"><span data-stu-id="25053-401">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender` is:</span></span>

* <span data-ttu-id="25053-402">Bileşen örneği ilk kez çağrıldığında `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="25053-402">Set to `true` the first time that the component instance is invoked.</span></span>
* <span data-ttu-id="25053-403">Başlatma işinin yalnızca bir kez gerçekleştirildiğinden emin olur.</span><span class="sxs-lookup"><span data-stu-id="25053-403">Ensures that initialization work is only performed once.</span></span>

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

> [!NOTE]
> <span data-ttu-id="25053-404">@No__t-0 yaşam döngüsü olayı sırasında, işleme hemen sonra zaman uyumsuz çalışma gerçekleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="25053-404">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="25053-405">İşleme sırasında tamamlanmamış zaman uyumsuz eylemleri işle</span><span class="sxs-lookup"><span data-stu-id="25053-405">Handle incomplete async actions at render</span></span>

<span data-ttu-id="25053-406">Yaşam döngüsü olaylarında gerçekleştirilen zaman uyumsuz eylemler, bileşen işlenmeden önce tamamlanmamış olabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-406">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="25053-407">Yaşam döngüsü yöntemi yürütülürken nesneler `null` veya verilerle tamamen doldurulmuş olabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-407">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="25053-408">Nesnelerin başlatıldığını onaylamak için işleme mantığı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="25053-408">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="25053-409">Nesneler `null` olduğunda yer tutucu Kullanıcı arabirimi öğelerini (örneğin, bir yükleme iletisi) işleme.</span><span class="sxs-lookup"><span data-stu-id="25053-409">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="25053-410">Blazor şablonlarının `FetchData` bileşeninde, `OnInitializedAsync`, Asychronously tahmin verileri al (`forecasts`) için geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="25053-410">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="25053-411">@No__t-0 `null` olduğunda, kullanıcıya bir yükleme iletisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="25053-411">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="25053-412">@No__t-1 tarafından döndürülen `Task` ' ı tamamladıktan sonra, bileşen güncelleştirilmiş durumla yeniden yapılır.</span><span class="sxs-lookup"><span data-stu-id="25053-412">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="25053-413">*Pages/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="25053-413">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="25053-414">Parametreler ayarlanmadan önce kodu yürütün</span><span class="sxs-lookup"><span data-stu-id="25053-414">Execute code before parameters are set</span></span>

<span data-ttu-id="25053-415">Parametreler ayarlanmadan önce kodu yürütmek için `SetParameters` geçersiz kılınabilir:</span><span class="sxs-lookup"><span data-stu-id="25053-415">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="25053-416">@No__t-0 çağrılmazsa, özel kod gelen parametreler değerini gerekli herhangi bir şekilde yorumlayabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-416">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="25053-417">Örneğin, gelen parametrelerin, sınıftaki özelliklere atanması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="25053-417">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="25053-418">Kullanıcı arabiriminin yenilenmesini gösterme</span><span class="sxs-lookup"><span data-stu-id="25053-418">Suppress refreshing of the UI</span></span>

<span data-ttu-id="25053-419">`ShouldRender`, Kullanıcı arabiriminin yenilenmesini gizlemek için geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-419">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="25053-420">Uygulama `true` döndürürse, Kullanıcı arabirimi yenilenir.</span><span class="sxs-lookup"><span data-stu-id="25053-420">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="25053-421">@No__t-0 geçersiz kılınsa bile, bileşen her zaman ilk olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="25053-421">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="25053-422">IDisposable ile bileşen atma</span><span class="sxs-lookup"><span data-stu-id="25053-422">Component disposal with IDisposable</span></span>

<span data-ttu-id="25053-423">Bir bileşen <xref:System.IDisposable> ' ı uygularsa, bileşen kullanıcı arabiriminden kaldırıldığında [Dispose yöntemi](/dotnet/standard/garbage-collection/implementing-dispose) çağrılır.</span><span class="sxs-lookup"><span data-stu-id="25053-423">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="25053-424">Aşağıdaki bileşen `@implements IDisposable` ve `Dispose` yöntemini kullanır:</span><span class="sxs-lookup"><span data-stu-id="25053-424">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a><span data-ttu-id="25053-425">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="25053-425">Routing</span></span>

<span data-ttu-id="25053-426">Blazor ' de yönlendirme, uygulamadaki her erişilebilir bileşene bir rota şablonu sunarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="25053-426">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="25053-427">@No__t-0 yönergesi içeren bir Razor dosyası derlendiğinde, oluşturulan sınıfa yol şablonunu belirten bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> verilir.</span><span class="sxs-lookup"><span data-stu-id="25053-427">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="25053-428">Çalışma zamanında, yönlendirici `RouteAttribute` ile bileşen sınıflarına bakar ve hangi bileşenin istenen URL ile eşleşen bir rota şablonuna sahip olduğunu işler.</span><span class="sxs-lookup"><span data-stu-id="25053-428">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="25053-429">Birden çok yol şablonu, bir bileşene uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-429">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="25053-430">Aşağıdaki bileşen `/BlazorRoute` ve `/DifferentBlazorRoute` isteklerine yanıt verir:</span><span class="sxs-lookup"><span data-stu-id="25053-430">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="25053-431">Rota parametreleri</span><span class="sxs-lookup"><span data-stu-id="25053-431">Route parameters</span></span>

<span data-ttu-id="25053-432">Bileşenler, `@page` yönergesinde belirtilen yol şablonundan rota parametreleri alabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-432">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="25053-433">Yönlendirici, karşılık gelen bileşen parametrelerini doldurmak için yol parametrelerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="25053-433">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="25053-434">*Rota parametresi bileşeni*:</span><span class="sxs-lookup"><span data-stu-id="25053-434">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="25053-435">İsteğe bağlı parametreler desteklenmez, bu nedenle yukarıdaki örnekte iki `@page` yönergesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="25053-435">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="25053-436">İlki, bir parametre olmadan bileşene gezinmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="25053-436">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="25053-437">İkinci `@page` yönergesi `{text}` yol parametresini alır ve değeri `Text` özelliğine atar.</span><span class="sxs-lookup"><span data-stu-id="25053-437">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="25053-438">"Arka plan kod" deneyimi için temel sınıf devralma</span><span class="sxs-lookup"><span data-stu-id="25053-438">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="25053-439">Bileşen dosyaları aynı dosyada HTML biçimlendirme C# ve işleme kodu karışımını.</span><span class="sxs-lookup"><span data-stu-id="25053-439">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="25053-440">@No__t-0 yönergesi, bileşen işaretlemesini işleme kodundan ayıran "arka plan kod" deneyimiyle Blazor uygulamalar sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-440">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="25053-441">[Örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) , bileşenin özelliklerini ve yöntemlerini sağlamak için bir bileşenin `BlazorRocksBase` temel sınıfını nasıl devralmasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="25053-441">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="25053-442">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="25053-442">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="25053-443">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="25053-443">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="25053-444">Temel sınıf `ComponentBase` ' dan türetilmelidir.</span><span class="sxs-lookup"><span data-stu-id="25053-444">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="25053-445">Bileşenleri içeri aktar</span><span class="sxs-lookup"><span data-stu-id="25053-445">Import components</span></span>

<span data-ttu-id="25053-446">Razor ile yazılan bir bileşenin ad alanı temel alınarak belirlenir (öncelik sırasına göre):</span><span class="sxs-lookup"><span data-stu-id="25053-446">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="25053-447">Razor dosyası ( *. Razor*) biçimlendirmesinde [@namespace](xref:mvc/views/razor#namespace) atama (`@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="25053-447">[@namespace](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="25053-448">Projenin proje dosyasında `RootNamespace` (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="25053-448">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="25053-449">Proje dosyasının dosya adından ( *. csproj*) ve proje kökünden bileşen yolundan alınan proje adı.</span><span class="sxs-lookup"><span data-stu-id="25053-449">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="25053-450">Örneğin Framework, *{Project root}/Pages/Index.Razor* (*BlazorSample. csproj*) ad alanına `BlazorSample.Pages` ' yi çözer.</span><span class="sxs-lookup"><span data-stu-id="25053-450">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="25053-451">Bileşenler ad C# bağlama kurallarını izler.</span><span class="sxs-lookup"><span data-stu-id="25053-451">Components follow C# name binding rules.</span></span> <span data-ttu-id="25053-452">Bu örnekteki `Index` bileşeni için, kapsamdaki bileşenler tüm bileşenlerdir:</span><span class="sxs-lookup"><span data-stu-id="25053-452">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="25053-453">Aynı klasörde, *sayfalarda*.</span><span class="sxs-lookup"><span data-stu-id="25053-453">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="25053-454">Proje kökündeki, açıkça farklı bir ad alanı belirtmeyen bileşenler.</span><span class="sxs-lookup"><span data-stu-id="25053-454">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="25053-455">Farklı bir ad alanında tanımlanan bileşenler, Razor 'nin [@using](xref:mvc/views/razor#using) yönergesi kullanılarak kapsam içine getirilir.</span><span class="sxs-lookup"><span data-stu-id="25053-455">Components defined in a different namespace are brought into scope using Razor's [@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="25053-456">@No__t-0 adlı başka bir bileşen *BlazorSample/Shared/* klasöründe varsa, bileşen aşağıdaki `@using` ifadesiyle `Index.razor` ' de kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="25053-456">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="25053-457">Bileşenlere, [@using](xref:mvc/views/razor#using) yönergesini gerektirmeyen kendi tam adları kullanılarak da başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="25053-457">Components can also be referenced using their fully qualified names, which doesn't require the [@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="25053-458">@No__t-0 niteliği desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="25053-458">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="25053-459">Diğer ad `using` deyimleriyle bileşenleri içeri aktarma (örneğin, `@using Foo = Bar`) desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="25053-459">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="25053-460">Kısmen nitelenmiş adlar desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="25053-460">Partially qualified names aren't supported.</span></span> <span data-ttu-id="25053-461">Örneğin, `@using BlazorSample` ' ı ekleme ve `<Shared.NavMenu></Shared.NavMenu>` ile `NavMenu.razor` ' i eklemek desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="25053-461">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="25053-462">Koşullu HTML öğesi öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="25053-462">Conditional HTML element attributes</span></span>

<span data-ttu-id="25053-463">HTML öğesi öznitelikleri, .NET değerine göre koşullu olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="25053-463">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="25053-464">Değer `false` veya `null` ise, öznitelik işlenmez.</span><span class="sxs-lookup"><span data-stu-id="25053-464">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="25053-465">Değer `true` ise, öznitelik küçültülmüş olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="25053-465">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="25053-466">Aşağıdaki örnekte `IsCompleted` öğesi öğenin biçimlendirmesinde `checked` ' in işlenip işlenmeyeceğini belirler:</span><span class="sxs-lookup"><span data-stu-id="25053-466">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="25053-467">@No__t-0 `true` ise, onay kutusu şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="25053-467">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="25053-468">@No__t-0 `false` ise, onay kutusu şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="25053-468">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="25053-469">Daha fazla bilgi için bkz. <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="25053-469">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="25053-470">.NET türü `bool` olduğunda, [Aria-basılan](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons)gıbı bazı HTML öznitelikleri düzgün şekilde çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="25053-470">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="25053-471">Bu durumlarda, `bool` yerine `string` türü kullanın.</span><span class="sxs-lookup"><span data-stu-id="25053-471">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="25053-472">Ham HTML</span><span class="sxs-lookup"><span data-stu-id="25053-472">Raw HTML</span></span>

<span data-ttu-id="25053-473">Dizeler normalde DOM metin düğümleri kullanılarak işlenir. Bu, içerdikleri tüm biçimlendirmenin yok sayıldığı ve değişmez değer olarak kabul edildiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="25053-473">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="25053-474">Ham HTML işlemek için, HTML içeriğini `MarkupString` değerinde sarın.</span><span class="sxs-lookup"><span data-stu-id="25053-474">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="25053-475">Değer HTML veya SVG olarak ayrıştırılır ve DOM 'a eklenir.</span><span class="sxs-lookup"><span data-stu-id="25053-475">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="25053-476">Güvenilmeyen bir kaynaktan oluşturulan ham HTML işleme bir **güvenlik riskidir** ve kaçınılması gerekir!</span><span class="sxs-lookup"><span data-stu-id="25053-476">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="25053-477">Aşağıdaki örnek, bir bileşenin işlenmiş çıktısına statik HTML içeriği bloğunu eklemek için `MarkupString` türünü kullanmayı gösterir:</span><span class="sxs-lookup"><span data-stu-id="25053-477">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="25053-478">Şablonlu bileşenler</span><span class="sxs-lookup"><span data-stu-id="25053-478">Templated components</span></span>

<span data-ttu-id="25053-479">Şablonlu bileşenler, bir veya daha fazla UI şablonunu parametre olarak kabul eden bileşenlerdir, daha sonra bileşen işleme mantığının bir parçası olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-479">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="25053-480">Şablonlu bileşenler, normal bileşenlerden daha yeniden kullanılabilir olan üst düzey bileşenleri yazmanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="25053-480">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="25053-481">Birkaç örnek şunlardır:</span><span class="sxs-lookup"><span data-stu-id="25053-481">A couple of examples include:</span></span>

* <span data-ttu-id="25053-482">Kullanıcının tablo üst bilgisi, satırları ve altbilgisi için şablon belirtmesini sağlayan tablo bileşeni.</span><span class="sxs-lookup"><span data-stu-id="25053-482">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="25053-483">Bir kullanıcının bir listedeki öğeleri işlemek için şablon belirlemesine izin veren bir liste bileşenidir.</span><span class="sxs-lookup"><span data-stu-id="25053-483">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="25053-484">Şablon parametreleri</span><span class="sxs-lookup"><span data-stu-id="25053-484">Template parameters</span></span>

<span data-ttu-id="25053-485">Şablonlu bir bileşen, `RenderFragment` veya `RenderFragment<T>` türünde bir veya daha fazla bileşen parametresi belirtilerek tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="25053-485">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="25053-486">Bir işleme parçası, işlenecek Kullanıcı arabiriminin bir kesimini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="25053-486">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="25053-487">`RenderFragment<T>`, işleme parçası çağrıldığında belirtilebildiği bir tür parametresi alır.</span><span class="sxs-lookup"><span data-stu-id="25053-487">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="25053-488">`TableTemplate` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="25053-488">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="25053-489">Şablonlu bir bileşen kullanırken, şablon parametreleri parametre adlarıyla eşleşen alt öğeler kullanılarak belirtilebilir (`TableHeader` ve `RowTemplate` aşağıdaki örnekte):</span><span class="sxs-lookup"><span data-stu-id="25053-489">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a><span data-ttu-id="25053-490">Şablon bağlam parametreleri</span><span class="sxs-lookup"><span data-stu-id="25053-490">Template context parameters</span></span>

<span data-ttu-id="25053-491">Öğe olarak geçirilen `RenderFragment<T>` türündeki bileşen bağımsız değişkenleri, `context` adlı bir örtük parametreye sahiptir (örneğin, yukarıdaki kod örneğinden, `@context.PetId`), ancak parametre adını alt öğedeki `Context` özniteliğini kullanarak değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="25053-491">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="25053-492">Aşağıdaki örnekte, `RowTemplate` öğesinin `Context` özniteliği `pet` parametresini belirtir:</span><span class="sxs-lookup"><span data-stu-id="25053-492">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

<span data-ttu-id="25053-493">Alternatif olarak, bileşen öğesi üzerinde `Context` özniteliğini de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="25053-493">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="25053-494">Belirtilen `Context` özniteliği belirtilen tüm şablon parametreleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="25053-494">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="25053-495">Bu, örtük alt içerik (herhangi bir sarmalama alt öğesi olmadan) için içerik parametre adını belirtmek istediğinizde yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-495">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="25053-496">Aşağıdaki örnekte, `Context` özniteliği `TableTemplate` öğesinde görünür ve tüm şablon parametreleri için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="25053-496">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
<TableTemplate Items="pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a><span data-ttu-id="25053-497">Genel olarak yazılmış bileşenler</span><span class="sxs-lookup"><span data-stu-id="25053-497">Generic-typed components</span></span>

<span data-ttu-id="25053-498">Şablonlu bileşenler çoğunlukla genel olarak türdedir.</span><span class="sxs-lookup"><span data-stu-id="25053-498">Templated components are often generically typed.</span></span> <span data-ttu-id="25053-499">Örneğin, `IEnumerable<T>` değerlerini işlemek için genel `ListViewTemplate` bir bileşen kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-499">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="25053-500">Genel bir bileşen tanımlamak için [@typeparam](xref:mvc/views/razor#typeparam) yönergesini kullanarak tür parametrelerini belirtin:</span><span class="sxs-lookup"><span data-stu-id="25053-500">To define a generic component, use the [@typeparam](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="25053-501">Genel türsüz bileşenleri kullanırken tür parametresi mümkünse algılanır:</span><span class="sxs-lookup"><span data-stu-id="25053-501">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="25053-502">Aksi halde tür parametresi, tür parametresinin adıyla eşleşen bir öznitelik kullanılarak açıkça belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="25053-502">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="25053-503">Aşağıdaki örnekte, `TItem="Pet"` türü belirtir:</span><span class="sxs-lookup"><span data-stu-id="25053-503">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="25053-504">Değerleri ve parametreleri basamaklama</span><span class="sxs-lookup"><span data-stu-id="25053-504">Cascading values and parameters</span></span>

<span data-ttu-id="25053-505">Bazı senaryolarda, özellikle birden çok bileşen katmanı olduğunda, [bileşen parametreleri](#component-parameters)kullanarak bir üst bileşenden bir alt bileşene veri akışı yapmak uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="25053-505">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="25053-506">Değerleri ve parametreleri basamaklama, bir üst bileşenin tüm alt bileşenlerine değer sağlaması için kullanışlı bir yol sağlayarak bu sorunu çözebilir.</span><span class="sxs-lookup"><span data-stu-id="25053-506">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="25053-507">Basamaklı değerler ve parametreler, bileşenlerin koordinasyonu için bir yaklaşım sağlar.</span><span class="sxs-lookup"><span data-stu-id="25053-507">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="25053-508">Tema örneği</span><span class="sxs-lookup"><span data-stu-id="25053-508">Theme example</span></span>

<span data-ttu-id="25053-509">Örnek uygulamadan aşağıdaki örnekte, `ThemeInfo` sınıfı, uygulamanın belirli bir bölümü içindeki tüm düğmelerin aynı stili paylaşması için bileşen hiyerarşisinin akmasını sağlayacak tema bilgilerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="25053-509">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="25053-510">*Uıthemeclasses/Themeınfo. cs*:</span><span class="sxs-lookup"><span data-stu-id="25053-510">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="25053-511">Bir üst bileşen basamaklı değer bileşeni kullanılarak basamaklı bir değer sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-511">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="25053-512">@No__t-0 bileşeni, bileşen hiyerarşisinin bir alt ağacını sarmalanmış ve bu alt ağaçta bulunan tüm bileşenlere tek bir değer sağlar.</span><span class="sxs-lookup"><span data-stu-id="25053-512">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="25053-513">Örneğin, örnek uygulama, `@Body` özelliğinin düzen gövdesini oluşturan tüm bileşenler için bir geçişli parametre olarak uygulamanın düzenlerindeki tema bilgilerini (`ThemeInfo`) belirtir.</span><span class="sxs-lookup"><span data-stu-id="25053-513">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="25053-514">`ButtonClass` ' a, düzen bileşeninde `btn-success` değeri atanır.</span><span class="sxs-lookup"><span data-stu-id="25053-514">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="25053-515">Tüm alt bileşenler, `ThemeInfo` basamaklı nesne aracılığıyla bu özelliği kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-515">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="25053-516">`CascadingValuesParametersLayout` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="25053-516">`CascadingValuesParametersLayout` component:</span></span>

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="25053-517">Basamaklı değerleri kullanmak için, bileşenler `[CascadingParameter]` özniteliğini kullanarak Geçişli Parametreler bildirir.</span><span class="sxs-lookup"><span data-stu-id="25053-517">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="25053-518">Basamaklı değerler, türe göre basamaklı parametrelere bağlanır.</span><span class="sxs-lookup"><span data-stu-id="25053-518">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="25053-519">Örnek uygulamada `CascadingValuesParametersTheme` bileşeni, `ThemeInfo` geçişli değeri basamaklı bir parametreye bağlar.</span><span class="sxs-lookup"><span data-stu-id="25053-519">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="25053-520">Parametresi, bileşen tarafından görünen düğmelerden birine ait CSS sınıfını ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="25053-520">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="25053-521">`CascadingValuesParametersTheme` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="25053-521">`CascadingValuesParametersTheme` component:</span></span>

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

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
    private int currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="25053-522">Aynı alt ağaçta aynı türdeki birden çok değeri basamakla, her bir `CascadingValue` bileşenine ve karşılık gelen `CascadingParameter` ' ye benzersiz bir `Name` dizesi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="25053-522">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="25053-523">Aşağıdaki örnekte, iki `CascadingValue` bileşeni, ada göre farklı `MyCascadingType` örneklerini basamakla:</span><span class="sxs-lookup"><span data-stu-id="25053-523">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```cshtml
<CascadingValue Value=@ParentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType ParentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="25053-524">Alt bileşende, basamaklı parametreler değerlerini, üst bileşendeki ilgili basamaklı değerleri ada göre alır:</span><span class="sxs-lookup"><span data-stu-id="25053-524">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="25053-525">TabSet örneği</span><span class="sxs-lookup"><span data-stu-id="25053-525">TabSet example</span></span>

<span data-ttu-id="25053-526">Basamaklı parametreler, bileşenlerin bileşen hiyerarşisinde işbirliği yapmasına de olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="25053-526">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="25053-527">Örneğin, örnek uygulamada aşağıdaki *Tabset* örneğini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="25053-527">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="25053-528">Örnek uygulama, sekmelerin uygulandığı `ITab` arabirimine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="25053-528">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="25053-529">@No__t-0 bileşeni, çeşitli `Tab` bileşenleri içeren `TabSet` bileşenini kullanır:</span><span class="sxs-lookup"><span data-stu-id="25053-529">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="25053-530">Alt `Tab` bileşenleri, `TabSet` ' e açıkça parametre olarak geçirilmemektedir.</span><span class="sxs-lookup"><span data-stu-id="25053-530">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="25053-531">Bunun yerine, alt `Tab` bileşenleri, `TabSet` ' in alt içeriğinin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="25053-531">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="25053-532">Ancak, `TabSet` ' ın, üst bilgileri ve etkin sekmeyi işleyebilmesi için her bir `Tab` bileşeni hakkında hala bilmeleri gerekir. Ek kod gerektirmeden bu koordinasyonu etkinleştirmek için, `TabSet` bileşeni, kendisini alt `Tab` bileşenleri tarafından çekilen *basamaklı bir değer olarak sağlayabilir* .</span><span class="sxs-lookup"><span data-stu-id="25053-532">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="25053-533">`TabSet` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="25053-533">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="25053-534">@No__t-0 bileşenleri, kapsayan `TabSet` ' i basamaklı bir parametre olarak yakalar, bu nedenle `Tab` bileşenleri kendilerini `TabSet` ' e ekler ve sekmenin etkin olduğu koordine edilir.</span><span class="sxs-lookup"><span data-stu-id="25053-534">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="25053-535">`Tab` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="25053-535">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="25053-536">Razor şablonları</span><span class="sxs-lookup"><span data-stu-id="25053-536">Razor templates</span></span>

<span data-ttu-id="25053-537">Oluşturma parçaları Razor şablonu sözdizimi kullanılarak tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-537">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="25053-538">Razor şablonları, bir UI parçacığı tanımlamak ve aşağıdaki biçimi varsaymak için bir yoldur:</span><span class="sxs-lookup"><span data-stu-id="25053-538">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="25053-539">Aşağıdaki örnek, `RenderFragment` ve `RenderFragment<T>` değerlerinin nasıl belirtildiğini ve şablonlarının doğrudan bir bileşende nasıl işleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="25053-539">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="25053-540">Oluşturma parçaları, [şablonlu bileşenlere](#templated-components)bağımsız değişken olarak da geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="25053-540">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

```cshtml
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = 
        (pet) => @<p>Your pet's name is @pet.Name.</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="25053-541">Önceki kodun işlenmiş çıktısı:</span><span class="sxs-lookup"><span data-stu-id="25053-541">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="25053-542">El ile RenderTreeBuilder mantığı</span><span class="sxs-lookup"><span data-stu-id="25053-542">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="25053-543">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder`, bileşenleri ve öğeleri işlemek için, C# kodda el ile bileşen oluşturma gibi yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="25053-543">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="25053-544">Bileşen oluşturmak için `RenderTreeBuilder` kullanımı gelişmiş bir senaryodur.</span><span class="sxs-lookup"><span data-stu-id="25053-544">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="25053-545">Hatalı biçimlendirilmiş bir bileşen (örneğin, kapatılmamış bir biçimlendirme etiketi) tanımsız davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-545">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="25053-546">Başka bir bileşende el ile kullanılabilecek aşağıdaki `PetDetails` bileşenini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="25053-546">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="25053-547">Aşağıdaki örnekte, `CreateComponent` yöntemindeki döngü üç `PetDetails` bileşeni oluşturur.</span><span class="sxs-lookup"><span data-stu-id="25053-547">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="25053-548">Bileşenleri oluşturmak için `RenderTreeBuilder` yöntemleri çağrılırken (`OpenComponent` ve `AddAttribute`), dizi numaraları kaynak kodu satır sayılarıdır.</span><span class="sxs-lookup"><span data-stu-id="25053-548">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="25053-549">Blazor fark algoritması, ayrı çağrı etkinleştirmeleri değil ayrı kod satırlarına karşılık gelen sıra numaralarına dayanır.</span><span class="sxs-lookup"><span data-stu-id="25053-549">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="25053-550">@No__t-0 yöntemleriyle bir bileşen oluştururken, dizi numaralarına yönelik bağımsız değişkenleri sabit olarak kodlayın.</span><span class="sxs-lookup"><span data-stu-id="25053-550">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="25053-551">**Sıra numarasını oluşturmak için bir hesaplama veya sayaç kullanmak kötü performansa neden olabilir.**</span><span class="sxs-lookup"><span data-stu-id="25053-551">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="25053-552">Daha fazla bilgi için bkz. [kod satırı numaralarıyla Ilgili sıra numaraları ve yürütme sırası çalışmıyor](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) bölümü.</span><span class="sxs-lookup"><span data-stu-id="25053-552">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="25053-553">`BuiltContent` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="25053-553">`BuiltContent` component:</span></span>

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> <span data-ttu-id="25053-554">! WARNING @No__t-0 ' daki türler, işleme işlemlerinin *sonuçlarının* işlenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="25053-554">![WARNING] The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="25053-555">Bunlar, Blazor Framework uygulamasının iç ayrıntılardır.</span><span class="sxs-lookup"><span data-stu-id="25053-555">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="25053-556">Bu türlerin *dengesizleşilmesi* ve gelecekteki sürümlerde değişikliğe tabi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="25053-556">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="25053-557">Sıra numaraları, kod satırı numaralarıyla ilgilidir ve yürütme sırası değildir</span><span class="sxs-lookup"><span data-stu-id="25053-557">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="25053-558">Blazor `.razor` dosyaları her zaman derlenir.</span><span class="sxs-lookup"><span data-stu-id="25053-558">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="25053-559">Derleme adımı, çalışma zamanında uygulama performansını geliştiren bilgileri eklemek için kullanılabilir olduğundan, bu büyük olasılıkla `.razor` için harika bir avantajdır.</span><span class="sxs-lookup"><span data-stu-id="25053-559">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="25053-560">Bu geliştirmelerin önemli bir örneği *sıra numarası*içerir.</span><span class="sxs-lookup"><span data-stu-id="25053-560">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="25053-561">Sıra numaraları, hangi çıkışların ayrı ve sıralı kod satırlarından geldiğini çalışma zamanına işaret ediyor.</span><span class="sxs-lookup"><span data-stu-id="25053-561">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="25053-562">Çalışma zamanı, doğrusal bir zamanda, genel ağaç farkı algoritması için genellikle mümkün olandan çok daha hızlı olan etkili ağaç SLA 'ları oluşturmak için bu bilgileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="25053-562">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="25053-563">Aşağıdaki basit `.razor` dosyasını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="25053-563">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="25053-564">Yukarıdaki kod, aşağıdakine benzer şekilde derlenir:</span><span class="sxs-lookup"><span data-stu-id="25053-564">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="25053-565">Kod ilk kez yürütüldüğünde, `someFlag` `true` ise, Oluşturucu şunları alır:</span><span class="sxs-lookup"><span data-stu-id="25053-565">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="25053-566">Sequence</span><span class="sxs-lookup"><span data-stu-id="25053-566">Sequence</span></span> | <span data-ttu-id="25053-567">Tür</span><span class="sxs-lookup"><span data-stu-id="25053-567">Type</span></span>      | <span data-ttu-id="25053-568">Veri</span><span class="sxs-lookup"><span data-stu-id="25053-568">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="25053-569">0</span><span class="sxs-lookup"><span data-stu-id="25053-569">0</span></span>        | <span data-ttu-id="25053-570">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="25053-570">Text node</span></span> | <span data-ttu-id="25053-571">Adı</span><span class="sxs-lookup"><span data-stu-id="25053-571">First</span></span>  |
| <span data-ttu-id="25053-572">1\.</span><span class="sxs-lookup"><span data-stu-id="25053-572">1</span></span>        | <span data-ttu-id="25053-573">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="25053-573">Text node</span></span> | <span data-ttu-id="25053-574">Saniye</span><span class="sxs-lookup"><span data-stu-id="25053-574">Second</span></span> |

<span data-ttu-id="25053-575">@No__t-0 `false` ' in olduğunu düşünün ve biçimlendirme yeniden işlenir.</span><span class="sxs-lookup"><span data-stu-id="25053-575">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="25053-576">Bu kez, Oluşturucu şunları alır:</span><span class="sxs-lookup"><span data-stu-id="25053-576">This time, the builder receives:</span></span>

| <span data-ttu-id="25053-577">Sequence</span><span class="sxs-lookup"><span data-stu-id="25053-577">Sequence</span></span> | <span data-ttu-id="25053-578">Tür</span><span class="sxs-lookup"><span data-stu-id="25053-578">Type</span></span>       | <span data-ttu-id="25053-579">Veri</span><span class="sxs-lookup"><span data-stu-id="25053-579">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="25053-580">1\.</span><span class="sxs-lookup"><span data-stu-id="25053-580">1</span></span>        | <span data-ttu-id="25053-581">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="25053-581">Text node</span></span>  | <span data-ttu-id="25053-582">Saniye</span><span class="sxs-lookup"><span data-stu-id="25053-582">Second</span></span> |

<span data-ttu-id="25053-583">Çalışma zamanı bir fark gerçekleştirdiğinde, sırasıyla `0` olan öğenin kaldırıldığını görür, bu nedenle aşağıdaki önemsiz *düzenleme betiğini*oluşturur:</span><span class="sxs-lookup"><span data-stu-id="25053-583">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="25053-584">İlk metin düğümünü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="25053-584">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="25053-585">Program aracılığıyla sıra numaraları oluşturursanız ne yanlış gider</span><span class="sxs-lookup"><span data-stu-id="25053-585">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="25053-586">Aşağıdaki işleme Ağacı Oluşturucusu mantığını yazmanız yerine düşünün:</span><span class="sxs-lookup"><span data-stu-id="25053-586">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="25053-587">Şimdi ilk çıktı:</span><span class="sxs-lookup"><span data-stu-id="25053-587">Now, the first output is:</span></span>

| <span data-ttu-id="25053-588">Sequence</span><span class="sxs-lookup"><span data-stu-id="25053-588">Sequence</span></span> | <span data-ttu-id="25053-589">Tür</span><span class="sxs-lookup"><span data-stu-id="25053-589">Type</span></span>      | <span data-ttu-id="25053-590">Veri</span><span class="sxs-lookup"><span data-stu-id="25053-590">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="25053-591">0</span><span class="sxs-lookup"><span data-stu-id="25053-591">0</span></span>        | <span data-ttu-id="25053-592">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="25053-592">Text node</span></span> | <span data-ttu-id="25053-593">Adı</span><span class="sxs-lookup"><span data-stu-id="25053-593">First</span></span>  |
| <span data-ttu-id="25053-594">1\.</span><span class="sxs-lookup"><span data-stu-id="25053-594">1</span></span>        | <span data-ttu-id="25053-595">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="25053-595">Text node</span></span> | <span data-ttu-id="25053-596">Saniye</span><span class="sxs-lookup"><span data-stu-id="25053-596">Second</span></span> |

<span data-ttu-id="25053-597">Bu sonuç önceki bir durum ile aynıdır, bu nedenle olumsuz bir sorun yoktur.</span><span class="sxs-lookup"><span data-stu-id="25053-597">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="25053-598">`someFlag`, ikinci işlemede `false` ' dir ve çıktı:</span><span class="sxs-lookup"><span data-stu-id="25053-598">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="25053-599">Sequence</span><span class="sxs-lookup"><span data-stu-id="25053-599">Sequence</span></span> | <span data-ttu-id="25053-600">Tür</span><span class="sxs-lookup"><span data-stu-id="25053-600">Type</span></span>      | <span data-ttu-id="25053-601">Veri</span><span class="sxs-lookup"><span data-stu-id="25053-601">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="25053-602">0</span><span class="sxs-lookup"><span data-stu-id="25053-602">0</span></span>        | <span data-ttu-id="25053-603">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="25053-603">Text node</span></span> | <span data-ttu-id="25053-604">Saniye</span><span class="sxs-lookup"><span data-stu-id="25053-604">Second</span></span> |

<span data-ttu-id="25053-605">Bu kez, fark algoritması *iki* değişikliğin oluştuğunu görür ve algoritma aşağıdaki düzenleme betiğini üretir:</span><span class="sxs-lookup"><span data-stu-id="25053-605">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="25053-606">İlk metin düğümünün değerini `Second` olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="25053-606">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="25053-607">İkinci metin düğümünü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="25053-607">Remove the second text node.</span></span>

<span data-ttu-id="25053-608">Sıra numaralarının oluşturulması, `if/else` dallarının ve döngülerinin özgün kodda bulunduğu yer hakkındaki tüm yararlı bilgileri kaybetti.</span><span class="sxs-lookup"><span data-stu-id="25053-608">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="25053-609">Bu, daha önce olduğu gibi bir fark **ile iki kez** sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="25053-609">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="25053-610">Bu, önemsiz bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="25053-610">This is a trivial example.</span></span> <span data-ttu-id="25053-611">Karmaşık ve derin iç içe yapıları ve özellikle döngülerle daha gerçekçi durumlarda performans maliyeti daha önemlidir.</span><span class="sxs-lookup"><span data-stu-id="25053-611">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="25053-612">Hangi döngü bloklarının veya dallarının eklendiğini veya kaldırıldığını hemen belirlemek yerine, fark algoritmasının işleme ağaçlarına katmaları ve genellikle eski ve yeni yapıların nasıl olduğu hakkında yanlış bir biçimde düzenleme betikleri oluşturması birbirleriyle ilişkilendir.</span><span class="sxs-lookup"><span data-stu-id="25053-612">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="25053-613">Kılavuz ve ekibinizle</span><span class="sxs-lookup"><span data-stu-id="25053-613">Guidance and conclusions</span></span>

* <span data-ttu-id="25053-614">Sıra numaraları dinamik olarak oluşturulursa uygulama performansı de vardır.</span><span class="sxs-lookup"><span data-stu-id="25053-614">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="25053-615">Altyapı, derleme zamanında yakalanmadığı takdirde gerekli bilgiler bulunmadığından, çalışma zamanında kendi sıra numaralarını otomatik olarak oluşturamaz.</span><span class="sxs-lookup"><span data-stu-id="25053-615">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="25053-616">El ile uygulanan `RenderTreeBuilder` Logic uzun bloklar yazmayın.</span><span class="sxs-lookup"><span data-stu-id="25053-616">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="25053-617">@No__t-0 dosyalarını tercih edin ve derleyicinin sıra numaralarıyla uğraşmak için izin verin.</span><span class="sxs-lookup"><span data-stu-id="25053-617">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="25053-618">@No__t-0 mantığının el ile kurtulamadıysanız, uzun kod bloklarını `OpenRegion` @ no__t-2 @ no__t-3 çağrılarında kaydırılmış daha küçük parçalara ayırın.</span><span class="sxs-lookup"><span data-stu-id="25053-618">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="25053-619">Her bölge kendi ayrı dizi numaralarına sahiptir, bu nedenle her bölge içinde sıfırdan (veya herhangi bir rastgele sayıdan) yeniden başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="25053-619">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="25053-620">Dizi numaraları sabit kodluysa, fark algoritması yalnızca değer değerinde sıra numaralarının artırılmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="25053-620">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="25053-621">İlk değer ve boşluklar ilgisiz.</span><span class="sxs-lookup"><span data-stu-id="25053-621">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="25053-622">Tek bir seçenek, kod satırı numarasını sıra numarası olarak kullanmak veya sıfırdan başlayıp bir ya da yüzlerce (ya da tercih edilen aralığa) artırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="25053-622">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="25053-623">Blazor, sıra numaralarını kullanır, diğer ağaç dağıtma Kullanıcı arabirimi çerçeveleri bunları kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="25053-623">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="25053-624">Dizi numaraları kullanıldığında ve Blazor, `.razor` dosyalarını yazan geliştiriciler için otomatik olarak sıra numaralarıyla ilgilenen bir derleme adımının avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="25053-624">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>

## <a name="localization"></a><span data-ttu-id="25053-625">Yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="25053-625">Localization</span></span>

<span data-ttu-id="25053-626">Blazor Server uygulamaları, [Yerelleştirme ara yazılımı](xref:fundamentals/localization#localization-middleware)kullanılarak yerelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="25053-626">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="25053-627">Ara yazılım, uygulamadan kaynak isteyen kullanıcılar için uygun kültürü seçer.</span><span class="sxs-lookup"><span data-stu-id="25053-627">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="25053-628">Kültür aşağıdaki yaklaşımlardan biri kullanılarak ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="25053-628">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="25053-629">Çerezler</span><span class="sxs-lookup"><span data-stu-id="25053-629">Cookies</span></span>](#cookies)
* [<span data-ttu-id="25053-630">Kültürü seçmek için Kullanıcı arabirimi sağlama</span><span class="sxs-lookup"><span data-stu-id="25053-630">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="25053-631">Daha fazla bilgi ve örnek için bkz. <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="25053-631">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="25053-632">Özgü</span><span class="sxs-lookup"><span data-stu-id="25053-632">Cookies</span></span>

<span data-ttu-id="25053-633">Yerelleştirme kültürü tanımlama bilgisi kullanıcının kültürünü kalıcı hale getirebilirler.</span><span class="sxs-lookup"><span data-stu-id="25053-633">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="25053-634">Tanımlama bilgisi, uygulamanın ana bilgisayar sayfasının `OnGet` yöntemiyle oluşturulur (*Pages/Host. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="25053-634">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="25053-635">Yerelleştirme ara yazılımı, sonraki isteklerde Kullanıcı kültürünü ayarlamak için tanımlama bilgilerini okur.</span><span class="sxs-lookup"><span data-stu-id="25053-635">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="25053-636">Tanımlama bilgisinin kullanımı, WebSocket bağlantısının kültürü doğru şekilde yaymasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="25053-636">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="25053-637">Yerelleştirme şemaları URL yolunu veya sorgu dizesini temel alıyorsa, düzen WebSockets ile çalışmayabilir, bu nedenle kültürü kalıcı hale getiremeyebilir.</span><span class="sxs-lookup"><span data-stu-id="25053-637">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="25053-638">Bu nedenle, yerelleştirme kültürü tanımlama bilgisinin kullanılması önerilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="25053-638">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="25053-639">Kültür bir yerelleştirme tanımlama bilgisinde kalıcı hale getirilir kültür atamak için herhangi bir teknik kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="25053-639">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="25053-640">Uygulamanın zaten sunucu tarafı ASP.NET Core için bir yerelleştirme şeması varsa, uygulamanın var olan yerelleştirme altyapısını kullanmaya devam edin ve uygulamanın şeması içinde yerelleştirme kültür tanımlama bilgisini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="25053-640">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="25053-641">Aşağıdaki örnekte, yerelleştirme ara yazılımı tarafından okunabilen bir tanımlama bilgisinde geçerli kültürün nasıl ayarlanacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="25053-641">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="25053-642">Blazor Server uygulamasında aşağıdaki içeriklerle bir *Pages/Host. cshtml. cs* dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="25053-642">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

<span data-ttu-id="25053-643">Yerelleştirme uygulamada işlenir:</span><span class="sxs-lookup"><span data-stu-id="25053-643">Localization is handled in the app:</span></span>

1. <span data-ttu-id="25053-644">Tarayıcı, uygulamaya bir ilk HTTP isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="25053-644">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="25053-645">Kültür, yerelleştirme ara yazılımı tarafından atanır.</span><span class="sxs-lookup"><span data-stu-id="25053-645">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="25053-646">*_Host. cshtml. cs* içindeki `OnGet` yöntemi, yanıtın bir parçası olarak bir tanımlama bilgisinde kültürü devam ettirir.</span><span class="sxs-lookup"><span data-stu-id="25053-646">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="25053-647">Tarayıcı, etkileşimli bir Blazor Server oturumu oluşturmak için bir WebSocket bağlantısı açar.</span><span class="sxs-lookup"><span data-stu-id="25053-647">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="25053-648">Yerelleştirme ara yazılımı tanımlama bilgisini okur ve kültürü atar.</span><span class="sxs-lookup"><span data-stu-id="25053-648">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="25053-649">Blazor sunucusu oturumu doğru kültür ile başlar.</span><span class="sxs-lookup"><span data-stu-id="25053-649">The Blazor Server session begins with the correct culture.</span></span>

## <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="25053-650">Kültürü seçmek için Kullanıcı arabirimi sağlama</span><span class="sxs-lookup"><span data-stu-id="25053-650">Provide UI to choose the culture</span></span>

<span data-ttu-id="25053-651">Bir kullanıcının bir kültür seçmesine izin vermek için kullanıcı ARABIRIMI sağlamak üzere bir *yeniden yönlendirme tabanlı yaklaşım* önerilir.</span><span class="sxs-lookup"><span data-stu-id="25053-651">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="25053-652">Bu işlem, bir Kullanıcı güvenli bir kaynağa erişmeye çalıştığında bir Web uygulamasında gerçekleşmelere benzer @ no__t-0kullanıcı oturum açma sayfasına yönlendirilir ve ardından özgün kaynağa yeniden yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="25053-652">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="25053-653">Uygulama, bir denetleyiciye yeniden yönlendirme yoluyla kullanıcının seçili kültürünü devam ettirir.</span><span class="sxs-lookup"><span data-stu-id="25053-653">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="25053-654">Denetleyici kullanıcının seçili kültürünü bir tanımlama bilgisine ayarlar ve kullanıcıyı özgün URI 'ye yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="25053-654">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="25053-655">Bir tanımlama bilgisinde kullanıcının seçili kültürünü ayarlamak ve özgün URI 'ye yeniden yönlendirmeyi gerçekleştirmek için sunucuda bir HTTP uç noktası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="25053-655">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> <span data-ttu-id="25053-656">Açık yeniden yönlendirme saldırılarını engellemek için `LocalRedirect` eylem sonucunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="25053-656">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="25053-657">Daha fazla bilgi için bkz. <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="25053-657">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="25053-658">Aşağıdaki bileşen, Kullanıcı bir kültür seçtiğinde ilk yeniden yönlendirmenin nasıl gerçekleştirileceği hakkında bir örnek göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="25053-658">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```cshtml
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-blazor-apps"></a><span data-ttu-id="25053-659">Blazor uygulamalarında .NET yerelleştirme senaryolarını kullanma</span><span class="sxs-lookup"><span data-stu-id="25053-659">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="25053-660">Blazor uygulamaları içinde, aşağıdaki .NET yerelleştirme ve genelleştirme senaryoları kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="25053-660">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="25053-661">. NET ' in kaynak sistemi</span><span class="sxs-lookup"><span data-stu-id="25053-661">.NET's resources system</span></span>
* <span data-ttu-id="25053-662">Kültüre özgü sayı ve Tarih biçimlendirmesi</span><span class="sxs-lookup"><span data-stu-id="25053-662">Culture-specific number and date formatting</span></span>

<span data-ttu-id="25053-663">Blazor 'ın `@bind` işlevselliği, kullanıcının geçerli kültürüne göre Genelleştirme gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="25053-663">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="25053-664">Daha fazla bilgi için [veri bağlama](#data-binding) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="25053-664">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="25053-665">Sınırlı bir ASP.NET Core yerelleştirme senaryosu kümesi şu anda desteklenmektedir:</span><span class="sxs-lookup"><span data-stu-id="25053-665">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="25053-666">`IStringLocalizer<>` *,* Blazor uygulamalarında desteklenir.</span><span class="sxs-lookup"><span data-stu-id="25053-666">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="25053-667">`IHtmlLocalizer<>`, `IViewLocalizer<>` ve veri ek açıklamaları yerelleştirme ASP.NET Core MVC senaryolarından ve Blazor uygulamalarında **desteklenmez** .</span><span class="sxs-lookup"><span data-stu-id="25053-667">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="25053-668">Daha fazla bilgi için bkz. <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="25053-668">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="25053-669">Ölçeklenebilir vektör grafik (SVG) görüntüleri</span><span class="sxs-lookup"><span data-stu-id="25053-669">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="25053-670">Blazor tarafından oluşturulduğundan, ölçeklenebilir vektör grafik (SVG) görüntüleri ( *. SVG*) dahil olmak üzere tarayıcıda desteklenen görüntüler `<img>` etiketi aracılığıyla desteklenir:</span><span class="sxs-lookup"><span data-stu-id="25053-670">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="25053-671">Benzer şekilde, SVG görüntüleri bir stil sayfası dosyasının ( *. css*) CSS kurallarında desteklenir:</span><span class="sxs-lookup"><span data-stu-id="25053-671">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="25053-672">Ancak, satır içi SVG işaretlemesi tüm senaryolarda desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="25053-672">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="25053-673">Bir `<svg>` etiketini doğrudan bir bileşen dosyasına ( *. Razor*) yerleştirirseniz, temel görüntü işleme desteklenir, ancak birçok gelişmiş senaryo henüz desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="25053-673">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="25053-674">Örneğin, `<use>` etiketleri şu anda dikkate alınamaz ve `@bind` bazı SVG etiketleriyle kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="25053-674">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="25053-675">Gelecekteki bir sürümde bu sınırlamaları ele almayı bekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="25053-675">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25053-676">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="25053-676">Additional resources</span></span>

* <span data-ttu-id="25053-677"><xref:security/blazor/server> &ndash; kaynak tükenmesi ile Çekişmek zorunda olması gereken Blazor sunucu uygulamaları oluşturmaya yönelik yönergeler Içerir.</span><span class="sxs-lookup"><span data-stu-id="25053-677"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
