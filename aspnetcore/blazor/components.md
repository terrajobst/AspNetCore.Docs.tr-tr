---
title: ASP.NET Core Razor bileşenleri oluşturma ve kullanma
author: guardrex
description: Veri bağlama, olayları işleme ve bileşen yaşam döngülerini yönetme dahil Razor bileşenleri oluşturmayı ve kullanmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/02/2019
uid: blazor/components
ms.openlocfilehash: 43457bffd748ebba68cc86d33fdeb98dc419704b
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68913887"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="daa10-103">ASP.NET Core Razor bileşenleri oluşturma ve kullanma</span><span class="sxs-lookup"><span data-stu-id="daa10-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="daa10-104">, [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="daa10-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="daa10-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="daa10-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="daa10-106">Blazor uygulamaları, *bileşenleri*kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="daa10-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="daa10-107">Bir bileşen, bir sayfa, iletişim veya form gibi bir kullanıcı arabirimi (UI) öbekidir.</span><span class="sxs-lookup"><span data-stu-id="daa10-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="daa10-108">Bir bileşen, veri eklemek veya UI olaylarına yanıt vermek için gereken HTML işaretlemesini ve işleme mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="daa10-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="daa10-109">Bileşenler esnek ve hafif.</span><span class="sxs-lookup"><span data-stu-id="daa10-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="daa10-110">Bunlar, iç içe geçmiş, yeniden kullanılabilir ve projeler arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="daa10-111">Bileşen sınıfları</span><span class="sxs-lookup"><span data-stu-id="daa10-111">Component classes</span></span>

<span data-ttu-id="daa10-112">Bileşenler, C# ve HTML Işaretlemesi kullanılarak [Razor](xref:mvc/views/razor) bileşen dosyalarında ( *. Razor*) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="daa10-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="daa10-113">Blazor içindeki bir bileşen, bir *Razor bileşeni*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="daa10-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="daa10-114">Bileşenler *. cshtml* dosya uzantısı kullanılarak yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-114">Components can be authored using the *.cshtml* file extension.</span></span> <span data-ttu-id="daa10-115">Component *. cshtml* dosyalarını tanımlamak için proje dosyasındaki MSBuildözelliğinikullanın.`_RazorComponentInclude`</span><span class="sxs-lookup"><span data-stu-id="daa10-115">Use the `_RazorComponentInclude` MSBuild property in the project file to identify the component *.cshtml* files.</span></span> <span data-ttu-id="daa10-116">Örneğin, *Sayfalar* klasörü altındaki tüm *. cshtml* dosyalarının Razor bileşenleri dosyası olarak değerlendirilip değerlendirilmeyeceğini belirten bir uygulama:</span><span class="sxs-lookup"><span data-stu-id="daa10-116">For example, an app that specifies that all *.cshtml* files under the *Pages* folder should be treated as Razor components files:</span></span>

```xml
<PropertyGroup>
  <_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
</PropertyGroup>
```

<span data-ttu-id="daa10-117">Bir bileşen için Kullanıcı arabirimi HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="daa10-117">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="daa10-118">Dinamik işleme mantığı (örneğin, döngüler, koşullar, ifadeler) C# [Razor](xref:mvc/views/razor)adlı gömülü bir sözdizimi kullanılarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="daa10-118">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="daa10-119">Bir uygulama derlendiğinde, HTML biçimlendirme ve C# işleme mantığı bir bileşen sınıfına dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="daa10-119">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="daa10-120">Oluşturulan sınıfın adı, dosyanın adıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="daa10-120">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="daa10-121">Bileşen sınıfının üyeleri bir `@code` blokta tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="daa10-121">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="daa10-122">`@code` Bloğunda, bileşen durumu (özellikler, alanlar) olay işleme yöntemleriyle veya diğer bileşen mantığını tanımlamaya yönelik yöntemlerle belirtilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-122">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="daa10-123">Birden çok blok izin verilir. `@code`</span><span class="sxs-lookup"><span data-stu-id="daa10-123">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="daa10-124">ASP.NET Core 3,0 ' nin önceki önizlemelerinde `@functions` , bloklar Razor bileşenlerinde bulunan `@code` bloklarla aynı amaçla kullanılmıştı.</span><span class="sxs-lookup"><span data-stu-id="daa10-124">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="daa10-125">`@functions`bloklar Razor bileşenlerinde çalışmaya devam eder, ancak `@code` blok ASP.NET Core 3,0 Preview 6 veya sonraki bir sürümünde kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="daa10-125">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="daa10-126">Bileşen üyeleri, ile C# `@`başlayan ifadeleri kullanarak bileşenin işleme mantığının bir parçası olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-126">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="daa10-127">Örneğin, bir C# alan, alan adının önüne eklenerek `@` işlenir.</span><span class="sxs-lookup"><span data-stu-id="daa10-127">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="daa10-128">Aşağıdaki örnek değerlendirilir ve işler:</span><span class="sxs-lookup"><span data-stu-id="daa10-128">The following example evaluates and renders:</span></span>

* <span data-ttu-id="daa10-129">`_headingFontStyle`için CSS özellik değerine `font-style`.</span><span class="sxs-lookup"><span data-stu-id="daa10-129">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="daa10-130">`_headingText``<h1>` öğenin içeriğine.</span><span class="sxs-lookup"><span data-stu-id="daa10-130">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="daa10-131">Bileşen ilk olarak işlendikten sonra, bileşen işleme ağacını olaylara yanıt olarak yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="daa10-131">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="daa10-132">Blazor ardından yeni işleme ağacını önceki bir ile karşılaştırır ve tarayıcının Belge Nesne Modeli (DOM) üzerinde herhangi bir değişiklik uygular.</span><span class="sxs-lookup"><span data-stu-id="daa10-132">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="daa10-133">Bileşenler sıradan C# sınıflardır ve bir proje içinde herhangi bir yere yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-133">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="daa10-134">Web sayfalarını üreten bileşenler genellikle *Sayfalar* klasöründe bulunur.</span><span class="sxs-lookup"><span data-stu-id="daa10-134">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="daa10-135">Sayfa olmayan bileşenler sıklıkla *paylaşılan* klasöre veya projeye eklenen özel bir klasöre yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-135">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="daa10-136">Özel bir klasör kullanmak için, özel klasörün ad alanını üst bileşene ya da uygulamanın *_ımports. Razor* dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="daa10-136">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="daa10-137">Örneğin, aşağıdaki ad alanı, uygulamanın kök ad alanı `WebApplication`olduğunda bir bileşenler klasöründeki bileşenleri kullanılabilir yapar:</span><span class="sxs-lookup"><span data-stu-id="daa10-137">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="daa10-138">Bileşenleri Razor Pages ve MVC uygulamalarıyla tümleştirme</span><span class="sxs-lookup"><span data-stu-id="daa10-138">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="daa10-139">Mevcut Razor Pages ve MVC uygulamalarıyla bileşenleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="daa10-139">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="daa10-140">Razor bileşenleri kullanmak için mevcut sayfaları veya görünümleri yeniden yazmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="daa10-140">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="daa10-141">Sayfa veya görünüm işlendiğinde, bileşenler aynı anda önceden işlenir.</span><span class="sxs-lookup"><span data-stu-id="daa10-141">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="daa10-142">Bir sayfadan veya görünümden bir bileşeni işlemek için `RenderComponentAsync<TComponent>` HTML yardımcı yöntemini kullanın:</span><span class="sxs-lookup"><span data-stu-id="daa10-142">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

<span data-ttu-id="daa10-143">Sayfalar ve görünümler bileşenleri kullanırken, listesiyse doğru değildir.</span><span class="sxs-lookup"><span data-stu-id="daa10-143">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="daa10-144">Bileşenler, kısmi görünümler ve bölümler gibi görüntüleme ve sayfaya özgü senaryolar kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="daa10-144">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="daa10-145">Bir bileşende kısmi görünümden mantığı kullanmak için kısmi görünüm mantığını bir bileşene ayırın.</span><span class="sxs-lookup"><span data-stu-id="daa10-145">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="daa10-146">Bileşenlerin nasıl işlendiği ve bileşen durumunun Blazor sunucu tarafı uygulamalarda nasıl yönetildiği hakkında daha fazla bilgi için <xref:blazor/hosting-models> makalesine bakın.</span><span class="sxs-lookup"><span data-stu-id="daa10-146">For more information on how components are rendered and component state is managed in Blazor server-side apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="using-components"></a><span data-ttu-id="daa10-147">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="daa10-147">Using components</span></span>

<span data-ttu-id="daa10-148">Bileşenler, HTML öğesi söz dizimini kullanarak bildirerek diğer bileşenleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-148">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="daa10-149">Bir bileşeni kullanmak için biçimlendirme, etiket adının bileşen türü olduğu bir HTML etiketi gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="daa10-149">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="daa10-150">*Index. Razor* dosyasında aşağıdaki biçimlendirme bir `HeadingComponent` örneği işler:</span><span class="sxs-lookup"><span data-stu-id="daa10-150">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="daa10-151">*Bileşenler/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="daa10-151">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

## <a name="component-parameters"></a><span data-ttu-id="daa10-152">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="daa10-152">Component parameters</span></span>

<span data-ttu-id="daa10-153">Bileşenler bileşen`[Parameter]` sınıfında özniteliği kullanılarak tanımlanan *bileşen parametrelerine*sahip olabilir (genellikle *genel olmayan*).</span><span class="sxs-lookup"><span data-stu-id="daa10-153">Components can have *component parameters*, which are defined using properties (usually *non-public*) on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="daa10-154">Biçimlendirme içindeki bir bileşenin bağımsız değişkenlerini belirtmek için öznitelikleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="daa10-154">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="daa10-155">*Bileşenler/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="daa10-155">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="daa10-156">Aşağıdaki örnekte `ParentComponent` , öğesinin `Title` özelliğinin değerini ayarlar. `ChildComponent`</span><span class="sxs-lookup"><span data-stu-id="daa10-156">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="daa10-157">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="daa10-157">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="daa10-158">Alt içerik</span><span class="sxs-lookup"><span data-stu-id="daa10-158">Child content</span></span>

<span data-ttu-id="daa10-159">Bileşenler, başka bir bileşenin içeriğini ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-159">Components can set the content of another component.</span></span> <span data-ttu-id="daa10-160">Atama bileşeni, alıcı bileşeni belirten Etiketler arasında içerik sağlar.</span><span class="sxs-lookup"><span data-stu-id="daa10-160">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="daa10-161">Aşağıdaki örnekte `ChildComponent` , öğesinin işlemek için bir kullanıcı arabirimi segmentini temsil `RenderFragment`eden öğesini temsil eden bir `ChildContent` özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="daa10-161">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="daa10-162">Değeri `ChildContent` , bileşenin, içeriğin işlenmesi gereken biçimlendirmesinde konumlandırılır.</span><span class="sxs-lookup"><span data-stu-id="daa10-162">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="daa10-163">Değeri `ChildContent` , ana bileşenden alınır ve önyükleme `panel-body`paneli içinde işlenir.</span><span class="sxs-lookup"><span data-stu-id="daa10-163">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="daa10-164">*Bileşenler/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="daa10-164">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="daa10-165">`RenderFragment` İçeriği alan özelliğin kural tarafından adlandırılması `ChildContent` gerekir.</span><span class="sxs-lookup"><span data-stu-id="daa10-165">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="daa10-166">Aşağıdakiler `ParentComponent` `<ChildComponent>` , `ChildComponent` içeriği etiketlerin içine yerleştirerek işleme için içerik sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-166">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="daa10-167">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="daa10-167">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="daa10-168">Öznitelik döndürme ve rastgele parametreler</span><span class="sxs-lookup"><span data-stu-id="daa10-168">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="daa10-169">Bileşenler, bileşen tarafından tanımlanan parametrelere ek olarak ek öznitelikler yakalayabilir ve işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-169">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="daa10-170">Ek öznitelikler bir sözlükte yakalanıp, daha sonra bileşen [@attributes](xref:mvc/views/razor#attributes) Razor yönergesi kullanılarak işlendiğinde bir öğe üzerine bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-170">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="daa10-171">Bu senaryo, çeşitli özelleştirmeleri destekleyen bir işaretleme öğesi üreten bir bileşen tanımlarken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="daa10-171">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="daa10-172">Örneğin, çok sayıda parametreyi destekleyen bir `<input>` için öznitelikleri ayrı olarak tanımlamak sıkıcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-172">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="daa10-173">Aşağıdaki `<input>` örnekte, ilk öğesi (`id="useIndividualParams"`) bağımsız bileşen parametrelerini kullanır, ancak ikinci `<input>` öğe (`id="useAttributesDict"`) öznitelik splatesini kullanır:</span><span class="sxs-lookup"><span data-stu-id="daa10-173">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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
    private string Maxlength { get; set; } = "10";

    [Parameter]
    private string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    private string Required { get; set; } = "required";

    [Parameter]
    private string Size { get; set; } = "50";

    [Parameter]
    private Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "true" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="daa10-174">Parametrenin türü dize anahtarlarıyla gerçekleştirmelidir `IEnumerable<KeyValuePair<string, object>>` .</span><span class="sxs-lookup"><span data-stu-id="daa10-174">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="daa10-175">Bu senaryoda ayrıca bir seçenek de vardır.`IReadOnlyDictionary<string, object>`</span><span class="sxs-lookup"><span data-stu-id="daa10-175">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="daa10-176">Her iki `<input>` yaklaşımın de kullanıldığı işlenen öğeler aynıdır:</span><span class="sxs-lookup"><span data-stu-id="daa10-176">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="true"
       size="50">
```

<span data-ttu-id="daa10-177">Rastgele öznitelikleri kabul etmek için `[Parameter]` `CaptureUnmatchedValues` özelliği olarak `true`ayarlanmış özniteliği kullanarak bir bileşen parametresi tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="daa10-177">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    private Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="daa10-178">`CaptureUnmatchedValues` Üzerindeki`[Parameter]` özelliği, parametresinin diğer bir parametreyle eşleşmeyen tüm özniteliklerle eşleşmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="daa10-178">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="daa10-179">Bir bileşen yalnızca ile `CaptureUnmatchedValues`tek bir parametre tanımlayabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-179">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="daa10-180">İle `CaptureUnmatchedValues` kullanılan özellik türü dize anahtarlarıyla `Dictionary<string, object>` atanabilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="daa10-180">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="daa10-181">`IEnumerable<KeyValuePair<string, object>>`Ayrıca `IReadOnlyDictionary<string, object>` , Bu senaryodaki seçenekler de vardır.</span><span class="sxs-lookup"><span data-stu-id="daa10-181">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="daa10-182">Veri bağlama</span><span class="sxs-lookup"><span data-stu-id="daa10-182">Data binding</span></span>

<span data-ttu-id="daa10-183">Hem bileşenlere hem de Dom öğelerine veri bağlama, [@bind](xref:mvc/views/razor#bind) özniteliğiyle birlikte gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-183">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="daa10-184">Aşağıdaki örnek, `_italicsCheck` alanı onay kutusunun işaretli durumuna bağlar:</span><span class="sxs-lookup"><span data-stu-id="daa10-184">The following example binds the `_italicsCheck` field to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

<span data-ttu-id="daa10-185">Onay kutusu seçildiğinde ve kaldırıldığında, özelliğin değeri sırasıyla `true` ve `false`olarak güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-185">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="daa10-186">Onay kutusu kullanıcı arabiriminde, özelliğin değerini değiştirme yanıt olarak değil, yalnızca bileşen işlendiğinde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-186">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="daa10-187">Bileşenler olay işleyicisi kodu yürütüldükten sonra kendilerini oluşturduğundan, özellik güncelleştirmeleri genellikle kullanıcı arabirimine hemen yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="daa10-187">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="daa10-188">`CurrentValue` Özelliği `@bind` (`<input @bind="CurrentValue" />`) ile kullanmak, temelde aşağıdakilere eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="daa10-188">Using `@bind` with a `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="daa10-189">Bileşen işlendiğinde, `value` giriş öğesi `CurrentValue` özelliğinden gelir.</span><span class="sxs-lookup"><span data-stu-id="daa10-189">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="daa10-190">Kullanıcı metin kutusunda yazdığında, `onchange` olay tetiklenir `CurrentValue` ve özellik değiştirilen değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="daa10-190">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="daa10-191">Tür dönüştürmelerinde birkaç durum olduğu için, gerçekte kod oluşturma biraz `@bind` daha karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="daa10-191">In reality, the code generation is a little more complex because `@bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="daa10-192">İlke ' de `@bind` , bir ifadenin geçerli değerini bir `value` özniteliğiyle ilişkilendirir ve kayıtlı işleyiciyi kullanarak değişiklikleri işler.</span><span class="sxs-lookup"><span data-stu-id="daa10-192">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="daa10-193">`onchange` Sözdizimi ile [@bind-value](xref:mvc/views/razor#bind) `event` [@bind-value:event](xref:mvc/views/razor#bind)olayları işlemenin yanı sıra, bir özellik veya alan, parametresi () olan bir öznitelik belirtilerek diğer olaylar kullanılarak da bağlanabilir. `@bind`</span><span class="sxs-lookup"><span data-stu-id="daa10-193">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="daa10-194">Aşağıdaki örnek, `oninput` olay için `CurrentValue` özelliği bağlar:</span><span class="sxs-lookup"><span data-stu-id="daa10-194">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />
```

<span data-ttu-id="daa10-195">' `onchange`In aksine, öğe odağı kaybettiğinde harekete geçirilir, `oninput` metin kutusunun değeri değiştiğinde harekete geçirilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-195">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="daa10-196">**Biçim dizeleri**</span><span class="sxs-lookup"><span data-stu-id="daa10-196">**Format strings**</span></span>

<span data-ttu-id="daa10-197">Veri bağlama, kullanılarak <xref:System.DateTime> [@bind:format](xref:mvc/views/razor#bind)biçim dizeleriyle birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-197">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="daa10-198">Para birimi veya sayı biçimleri gibi diğer biçim ifadeleri şu anda kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="daa10-198">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="daa10-199">`@bind:format` Özniteliği öğesi`<input>` için uygulanacak `value` tarih biçimini belirtir.</span><span class="sxs-lookup"><span data-stu-id="daa10-199">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="daa10-200">Biçim Ayrıca bir `onchange` olay gerçekleştiğinde değeri ayrıştırmak için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="daa10-200">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="daa10-201">**Bileşen parametreleri**</span><span class="sxs-lookup"><span data-stu-id="daa10-201">**Component parameters**</span></span>

<span data-ttu-id="daa10-202">Bağlama bileşen parametrelerini tanır, burada `@bind-{property}` bir özellik değeri bileşenler arasında bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-202">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="daa10-203">Aşağıdaki alt bileşende (`ChildComponent`) bir `Year` bileşen parametresi ve `YearChanged` geri çağırması vardır:</span><span class="sxs-lookup"><span data-stu-id="daa10-203">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="daa10-204">`EventCallback<T>`, [Eventcallback](#eventcallback) bölümünde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="daa10-204">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="daa10-205">Aşağıdaki üst bileşen öğesini kullanır `ChildComponent` ve üst `Year` öğeden `ParentYear` parametreyi alt bileşen üzerindeki parametresine bağlar:</span><span class="sxs-lookup"><span data-stu-id="daa10-205">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

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
    private int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="daa10-206">Yüklemesi aşağıdaki biçimlendirmeyi üretir: `ParentComponent`</span><span class="sxs-lookup"><span data-stu-id="daa10-206">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="daa10-207">`ParentYear` Özelliğin değeri, `ParentComponent` içindekidüğme`ChildComponent` seçilerek değiştirilirse, öğesinin özelliğigüncellenir.`Year`</span><span class="sxs-lookup"><span data-stu-id="daa10-207">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="daa10-208">Yeni değeri `Year` , `ParentComponent` yeniden kullanıldığında kullanıcı arabiriminde işlenir:</span><span class="sxs-lookup"><span data-stu-id="daa10-208">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="daa10-209">Parametrenin türüyle `YearChanged` `Year` eşleşenbiryardımcıolayı`Year` olduğundan parametre bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-209">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="daa10-210">Kurala göre, `<ChildComponent @bind-Year="ParentYear" />` temelde yazmaya eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="daa10-210">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="daa10-211">Genel olarak, bir özellik öznitelik kullanılarak `@bind-property:event` karşılık gelen bir olay işleyicisine bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-211">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="daa10-212">Örneğin, özelliği `MyProp` aşağıdaki iki öznitelik `MyEventHandler` kullanılarak bağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="daa10-212">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="daa10-213">Olay işleme</span><span class="sxs-lookup"><span data-stu-id="daa10-213">Event handling</span></span>

<span data-ttu-id="daa10-214">Razor bileşenleri olay işleme özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="daa10-214">Razor components provide event handling features.</span></span> <span data-ttu-id="daa10-215">Temsilci türü belirtilmiş bir değer ile `on{event}` adlı bir HTML öğesi `onclick` özniteliği `onsubmit`için (örneğin, ve), Razor bileşenleri özniteliğin değerini bir olay işleyicisi olarak değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="daa10-215">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="daa10-216">Özniteliğin adı her zaman [ @on{Event}](xref:mvc/views/razor#onevent)olarak biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-216">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="daa10-217">Aşağıdaki kod, Kullanıcı arabiriminde `UpdateHeading` düğme seçildiğinde yöntemini çağırır:</span><span class="sxs-lookup"><span data-stu-id="daa10-217">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="daa10-218">Aşağıdaki kod, Kullanıcı arabiriminde `CheckChanged` onay kutusu değiştirildiğinde yöntemini çağırır:</span><span class="sxs-lookup"><span data-stu-id="daa10-218">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="daa10-219">Olay işleyicileri Ayrıca zaman uyumsuz olabilir ve döndürebilir <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="daa10-219">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="daa10-220">El ile çağırmanız `StateHasChanged()`gerekmez.</span><span class="sxs-lookup"><span data-stu-id="daa10-220">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="daa10-221">Özel durumlar oluştuğunda günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-221">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="daa10-222">Aşağıdaki örnekte, `UpdateHeading` düğme seçildiğinde zaman uyumsuz olarak çağrılır:</span><span class="sxs-lookup"><span data-stu-id="daa10-222">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a><span data-ttu-id="daa10-223">Olay bağımsız değişken türleri</span><span class="sxs-lookup"><span data-stu-id="daa10-223">Event argument types</span></span>

<span data-ttu-id="daa10-224">Bazı olaylar için olay bağımsız değişkeni türlerine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-224">For some events, event argument types are permitted.</span></span> <span data-ttu-id="daa10-225">Bu olay türlerinden birine erişim gerekmiyorsa, yöntem çağrısında gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="daa10-225">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="daa10-226">Desteklenen [Uıeventargs](https://github.com/aspnet/AspNetCore/blob/release/3.0-preview8/src/Components/Components/src/UIEventArgs.cs) aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="daa10-226">Supported [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/release/3.0-preview8/src/Components/Components/src/UIEventArgs.cs) are shown in the following table.</span></span>

| <span data-ttu-id="daa10-227">Olay</span><span class="sxs-lookup"><span data-stu-id="daa10-227">Event</span></span> | <span data-ttu-id="daa10-228">Sınıf</span><span class="sxs-lookup"><span data-stu-id="daa10-228">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="daa10-229">Pano</span><span class="sxs-lookup"><span data-stu-id="daa10-229">Clipboard</span></span> | `UIClipboardEventArgs` |
| <span data-ttu-id="daa10-230">Sürükle</span><span class="sxs-lookup"><span data-stu-id="daa10-230">Drag</span></span>  | <span data-ttu-id="daa10-231">`UIDragEventArgs`sürükle ve bırak işlemi sırasında sürüklenen verileri tutmak için kullanılır ve bir veya daha fazla `UIDataTransferItem`tutabilir. &ndash; `DataTransfer`</span><span class="sxs-lookup"><span data-stu-id="daa10-231">`UIDragEventArgs` &ndash; `DataTransfer` is used to hold the dragged data during a drag and drop operation and may hold one or more `UIDataTransferItem`.</span></span> <span data-ttu-id="daa10-232">`UIDataTransferItem`bir sürükle veri öğesini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="daa10-232">`UIDataTransferItem` represents one drag data item.</span></span> |
| <span data-ttu-id="daa10-233">Hata</span><span class="sxs-lookup"><span data-stu-id="daa10-233">Error</span></span> | `UIErrorEventArgs` |
| <span data-ttu-id="daa10-234">Çı</span><span class="sxs-lookup"><span data-stu-id="daa10-234">Focus</span></span> | <span data-ttu-id="daa10-235">`UIFocusEventArgs`&ndash; İçin`relatedTarget`destek içermez.</span><span class="sxs-lookup"><span data-stu-id="daa10-235">`UIFocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="daa10-236">`<input>`değişebilir</span><span class="sxs-lookup"><span data-stu-id="daa10-236">`<input>` change</span></span> | `UIChangeEventArgs` |
| <span data-ttu-id="daa10-237">Klavye</span><span class="sxs-lookup"><span data-stu-id="daa10-237">Keyboard</span></span> | `UIKeyboardEventArgs` |
| <span data-ttu-id="daa10-238">Tığında</span><span class="sxs-lookup"><span data-stu-id="daa10-238">Mouse</span></span> | `UIMouseEventArgs` |
| <span data-ttu-id="daa10-239">Fare işaretçisi</span><span class="sxs-lookup"><span data-stu-id="daa10-239">Mouse pointer</span></span> | `UIPointerEventArgs` |
| <span data-ttu-id="daa10-240">Fare tekerleği</span><span class="sxs-lookup"><span data-stu-id="daa10-240">Mouse wheel</span></span> | `UIWheelEventArgs` |
| <span data-ttu-id="daa10-241">İlerleme durumu</span><span class="sxs-lookup"><span data-stu-id="daa10-241">Progress</span></span> | `UIProgressEventArgs` |
| <span data-ttu-id="daa10-242">Dokunma</span><span class="sxs-lookup"><span data-stu-id="daa10-242">Touch</span></span> | <span data-ttu-id="daa10-243">`UITouchEventArgs`&ndash; dokunarakduyarlıbircihazdakitekbir`UITouchPoint` iletişim noktasını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="daa10-243">`UITouchEventArgs` &ndash; `UITouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="daa10-244">Önceki tablodaki olayların özellikleri ve olay işleme davranışı hakkında daha fazla bilgi için bkz. [başvuru kaynağındaki EventArgs sınıfları](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview8/src/Components/Web/src).</span><span class="sxs-lookup"><span data-stu-id="daa10-244">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview8/src/Components/Web/src).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="daa10-245">Lambda ifadeleri</span><span class="sxs-lookup"><span data-stu-id="daa10-245">Lambda expressions</span></span>

<span data-ttu-id="daa10-246">Lambda ifadeleri de kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="daa10-246">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="daa10-247">Genellikle, bir dizi öğe üzerinde yineleme yaparken olduğu gibi ek değerlerin üzerinde kapatılabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-247">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="daa10-248">Aşağıdaki örnek, her biri `UpdateHeading` Kullanıcı arabiriminde seçildiğinde bir olay bağımsız değişkeni (`UIMouseEventArgs`) ve düğme numarası (`buttonNumber`) geçiren üç düğme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="daa10-248">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="daa10-249">Döngü değişkenini (`i`) bir `for` döngüde doğrudan bir lambda ifadesinde kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="daa10-249">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="daa10-250">Aksi halde, tüm lambda ifadeleri tarafından değeri tüm Lambdalar aynı olmasına neden olan `i`değişken aynı değişken kullanılır.</span><span class="sxs-lookup"><span data-stu-id="daa10-250">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="daa10-251">Her zaman değerini yerel bir değişkende (`buttonNumber` önceki örnekte) yakalayın ve sonra kullanın.</span><span class="sxs-lookup"><span data-stu-id="daa10-251">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="daa10-252">EventCallback</span><span class="sxs-lookup"><span data-stu-id="daa10-252">EventCallback</span></span>

<span data-ttu-id="daa10-253">İç içe bileşenler içeren yaygın bir senaryo, alt bileşen olayı&mdash;olduğunda bir üst bileşenin yöntemini, `onclick` örneğin bir olay gerçekleştiğinde bir olay oluştuğunda çalıştırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="daa10-253">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="daa10-254">Olayları bileşenler genelinde göstermek için bir `EventCallback`kullanın.</span><span class="sxs-lookup"><span data-stu-id="daa10-254">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="daa10-255">Bir üst bileşen bir alt bileşene `EventCallback`geri çağırma yöntemi atayabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-255">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="daa10-256">Örnek `ChildComponent` uygulamada, bir `onclick` düğmenin işleyicisinin örnek `ParentComponent`tarafından bir `EventCallback` temsilci almak üzere nasıl ayarlandığı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="daa10-256">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="daa10-257">,, Bir çevre `UIMouseEventArgs`cihazından bir `onclick` olay için uygun olan ile öğesine yazılır: `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="daa10-257">The `EventCallback` is typed with `UIMouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="daa10-258">, `ParentComponent` Alt`ShowMessage` öğenin yöntemine göre ayarlar: `EventCallback<T>`</span><span class="sxs-lookup"><span data-stu-id="daa10-258">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="daa10-259">Düğme ' de `ChildComponent`seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="daa10-259">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="daa10-260">`ParentComponent` Öğesinin`ShowMessage` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="daa10-260">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="daa10-261">`messageText`güncelleştirilir ve içinde `ParentComponent`görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="daa10-261">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="daa10-262">Geri çağırma yönteminde `StateHasChanged` (`ShowMessage`) bir çağrısı gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="daa10-262">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="daa10-263">`StateHasChanged`alt olaylar, alt öğe içinde yürütülen `ParentComponent`olay işleyicilerinde bileşen rerendering tetiklenmesi için otomatik olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="daa10-263">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="daa10-264">`EventCallback`ve `EventCallback<T>` zaman uyumsuz temsilcilere izin verir.</span><span class="sxs-lookup"><span data-stu-id="daa10-264">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="daa10-265">`EventCallback<T>`kesin bir şekilde türdedir ve belirli bir bağımsız değişken türü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="daa10-265">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="daa10-266">`EventCallback`zayıf ve bağımsız değişken türüne izin veriyor.</span><span class="sxs-lookup"><span data-stu-id="daa10-266">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="daa10-267">Bir `EventCallback` veya `EventCallback<T>` ile <xref:System.Threading.Tasks.Task>çağırın ve şunu bekler: `InvokeAsync`</span><span class="sxs-lookup"><span data-stu-id="daa10-267">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="daa10-268">Olay `EventCallback` işleme `EventCallback<T>` ve bağlama bileşeni parametreleri için ve kullanın.</span><span class="sxs-lookup"><span data-stu-id="daa10-268">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="daa10-269">Kesin olarak belirlenmiş `EventCallback<T>` `EventCallback`türü tercih edin.</span><span class="sxs-lookup"><span data-stu-id="daa10-269">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="daa10-270">`EventCallback<T>`bileşenin kullanıcılarına daha iyi hata geri bildirimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="daa10-270">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="daa10-271">Diğer UI olay işleyicileriyle benzer şekilde, olay parametresini belirtmek isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="daa10-271">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="daa10-272">Geri `EventCallback` çağırmaya hiçbir değer geçirilmemişse kullanın.</span><span class="sxs-lookup"><span data-stu-id="daa10-272">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="daa10-273">Bileşenlere başvuruları yakala</span><span class="sxs-lookup"><span data-stu-id="daa10-273">Capture references to components</span></span>

<span data-ttu-id="daa10-274">Bileşen başvuruları, bir bileşen örneğine başvurmak için bir yol sağlar; böylece, veya `Show` `Reset`gibi komutları bu örneğe verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="daa10-274">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="daa10-275">Bir bileşen başvurusunu yakalamak için, alt bileşene [@ref](xref:mvc/views/razor#ref) bir öznitelik ekleyin ve ardından aynı ada ve alt bileşenle aynı türe sahip bir alan tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="daa10-275">To capture a component reference, add a [@ref](xref:mvc/views/razor#ref) attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

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

<span data-ttu-id="daa10-276">Bileşen işlendiğinde, `loginDialog` alan `MyLoginDialog` alt bileşen örneğiyle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="daa10-276">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="daa10-277">Daha sonra bileşen örneğinde .NET yöntemlerini çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="daa10-277">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="daa10-278">Değişken yalnızca bileşen işlendikten sonra ve çıktısı `MyLoginDialog` öğesi içerdiğinde doldurulur. `loginDialog`</span><span class="sxs-lookup"><span data-stu-id="daa10-278">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="daa10-279">Bu noktaya kadar başvurulmasına hiçbir şey yok.</span><span class="sxs-lookup"><span data-stu-id="daa10-279">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="daa10-280">Bileşen işlemesini tamamladıktan sonra bileşen başvurularını işlemek için, `OnAfterRenderAsync` veya `OnAfterRender` yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="daa10-280">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="daa10-281">Bileşen başvurularını yakalama, [öğe başvurularını yakalamak](xref:blazor/javascript-interop#capture-references-to-elements)için benzer bir sözdizimi kullanın, bir [JavaScript birlikte çalışma](xref:blazor/javascript-interop) özelliği değildir.</span><span class="sxs-lookup"><span data-stu-id="daa10-281">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="daa10-282">Bileşen başvuruları yalnızca .net kodunda kullanıldıkları JavaScript&mdash;koduna aktarılmaz.</span><span class="sxs-lookup"><span data-stu-id="daa10-282">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="daa10-283">Alt bileşenlerin durumunu bulunmamalıdır için bileşen başvurularını kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="daa10-283">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="daa10-284">Bunun yerine, alt bileşenlere veri geçirmek için normal bildirime dayalı parametreleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="daa10-284">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="daa10-285">Normal bildirime dayalı parametrelerin kullanımı, otomatik olarak doğru zamanların yeniden yönlendirmesi için alt bileşenlerde oluşur.</span><span class="sxs-lookup"><span data-stu-id="daa10-285">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="daa10-286">Öğe \@ve bileşenlerin korunmasını denetlemek için anahtar kullanın</span><span class="sxs-lookup"><span data-stu-id="daa10-286">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="daa10-287">Bir öğe veya bileşen listesi işlenirken ve öğeler ya da bileşenler daha sonra değiştiğinde, Blazor 'in yayılma algoritması, önceki öğelerin veya bileşenlerin ne zaman tutulacağına ve model nesnelerinin bunlara nasıl eşleneceğine karar vermelidir.</span><span class="sxs-lookup"><span data-stu-id="daa10-287">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="daa10-288">Normalde, bu işlem otomatiktir ve yoksayılabilir, ancak işlemi denetlemek isteyebileceğiniz durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="daa10-288">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="daa10-289">Aşağıdaki örnek göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="daa10-289">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="@person.Details" />
}

@code {
    [Parameter]
    private IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="daa10-290">`People` Koleksiyonun içeriği, ekli, silinmiş veya yeniden sıralanmış girdilerle değişebilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-290">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="daa10-291">Bileşen yeniden oluşturulduğunda, `<DetailsEditor>` bileşen farklı `Details` parametre değerleri almak için değişebilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-291">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="daa10-292">Bu, beklenenden daha karmaşık rerendering oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-292">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="daa10-293">Bazı durumlarda rerendering, kayıp öğe odağı gibi görünür davranış farklılıklarına yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-293">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="daa10-294">Eşleme işlemi, `@key` Directive özniteliğiyle denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-294">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="daa10-295">`@key`, anahtar değerine göre öğelerin veya bileşenlerin korunmasını güvence altına almak için dağıtılmış algoritmaya neden olur:</span><span class="sxs-lookup"><span data-stu-id="daa10-295">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="@person" Details="@person.Details" />
}

@code {
    [Parameter]
    private IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="daa10-296">Koleksiyon değiştiğinde, yayılma algoritması örnekler ve `person` örnekler arasındaki `<DetailsEditor>` ilişkilendirmeyi korur: `People`</span><span class="sxs-lookup"><span data-stu-id="daa10-296">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="daa10-297">Bir `Person` , `People` listeden silinirse, yalnızca ilgili `<DetailsEditor>` örnek kullanıcı arabiriminden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="daa10-297">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="daa10-298">Diğer örnekler değişmeden bırakılır.</span><span class="sxs-lookup"><span data-stu-id="daa10-298">Other instances are left unchanged.</span></span>
* <span data-ttu-id="daa10-299">Listedeki bir `Person` konuma eklenirse, ilgili konuma bir yeni `<DetailsEditor>` örnek eklenir.</span><span class="sxs-lookup"><span data-stu-id="daa10-299">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="daa10-300">Diğer örnekler değişmeden bırakılır.</span><span class="sxs-lookup"><span data-stu-id="daa10-300">Other instances are left unchanged.</span></span>
* <span data-ttu-id="daa10-301">Girişler `Person` yeniden Sıralansa, ilgili `<DetailsEditor>` örnekler Kullanıcı arabiriminde korunur ve yeniden sıralanır.</span><span class="sxs-lookup"><span data-stu-id="daa10-301">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="daa10-302">Bazı senaryolarda, kullanımı `@key` rerendering karmaşıklığını en aza indirir ve odak konumu gibi Dom 'ın durum bilgisi olan kısımlarıyla ilgili olası sorunları önler.</span><span class="sxs-lookup"><span data-stu-id="daa10-302">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="daa10-303">Anahtarlar her kapsayıcı öğesi veya bileşeni için yereldir.</span><span class="sxs-lookup"><span data-stu-id="daa10-303">Keys are local to each container element or component.</span></span> <span data-ttu-id="daa10-304">Anahtarlar belge genelinde küresel olarak karşılaştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="daa10-304">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="daa10-305">Anahtar ne zaman \@kullanılır?</span><span class="sxs-lookup"><span data-stu-id="daa10-305">When to use \@key</span></span>

<span data-ttu-id="daa10-306">Genellikle, bir liste işlendiğinde (örneğin `@key` , bir `@foreach` blokta) ve tanımlamak için uygun bir değer varsa, `@key`bu işlem kullanım açısından mantıklı olur.</span><span class="sxs-lookup"><span data-stu-id="daa10-306">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="daa10-307">Bir nesne değiştiğinde Blazor `@key` 'in bir öğeyi veya bileşen alt ağacını koruma altına almasını engellemek için de kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="daa10-307">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="@currentPerson">
    ... content that depends on @currentPerson ...
</div>
```

<span data-ttu-id="daa10-308">Değişiklik `@currentPerson` olursa `@key` , öznitelik yönergesi Blazor tüm `<div>` alt öğelerini atmayı ve yeni öğeler ve bileşenlerle Kullanıcı arabiriminde alt ağacı yeniden oluşturmayı zorlar.</span><span class="sxs-lookup"><span data-stu-id="daa10-308">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="daa10-309">Değişiklik sırasında `@currentPerson` hiçbir Kullanıcı arabirimi durumunun korunmayacağını garanti etmeniz gerekirse bu yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-309">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="daa10-310">\@Anahtar ne zaman kullanılmaz?</span><span class="sxs-lookup"><span data-stu-id="daa10-310">When not to use \@key</span></span>

<span data-ttu-id="daa10-311">İle `@key`yayılma yaparken bir performans maliyeti vardır.</span><span class="sxs-lookup"><span data-stu-id="daa10-311">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="daa10-312">Performans maliyeti büyük değildir, ancak yalnızca öğenin veya `@key` bileşen koruma kurallarının denetlenmesi uygulamanın avantajına göre belirleyin.</span><span class="sxs-lookup"><span data-stu-id="daa10-312">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="daa10-313">`@key` Kullanılmasa bile, Blazor alt öğe ve bileşen örneklerini mümkün olduğunca korur.</span><span class="sxs-lookup"><span data-stu-id="daa10-313">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="daa10-314">Kullanmanın `@key` tek avantajı model örneklerinin, eşlemeyi seçme algoritması yerine, korunan bileşen örneklerine *nasıl* eşlendiğine ilişkin denetimdir.</span><span class="sxs-lookup"><span data-stu-id="daa10-314">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="daa10-315">Anahtar için \@kullanılacak değerler</span><span class="sxs-lookup"><span data-stu-id="daa10-315">What values to use for \@key</span></span>

<span data-ttu-id="daa10-316">Genellikle, için `@key`aşağıdaki değer türlerinden birini sağlamak mantıklı olur:</span><span class="sxs-lookup"><span data-stu-id="daa10-316">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="daa10-317">Model nesne örnekleri (örneğin, `Person` önceki örnekte olduğu gibi).</span><span class="sxs-lookup"><span data-stu-id="daa10-317">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="daa10-318">Bu, nesne başvurusu eşitliğine göre koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="daa10-318">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="daa10-319">Benzersiz tanımlayıcılar (örneğin, veya `int` `Guid`türündeki `string`birincil anahtar değerleri).</span><span class="sxs-lookup"><span data-stu-id="daa10-319">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="daa10-320">Beklenmedik bir şekilde çakışacak bir değer vermekten kaçının.</span><span class="sxs-lookup"><span data-stu-id="daa10-320">Avoid supplying a value that can clash unexpectedly.</span></span> <span data-ttu-id="daa10-321">`@key="@someObject.GetHashCode()"` Sağlanırsa, ilişkisiz nesnelerin karma kodları aynı olabileceğinden beklenmeyen çakışıyor oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-321">If `@key="@someObject.GetHashCode()"` is supplied, unexpected clashes may occur because the hash codes of unrelated objects can be the same.</span></span> <span data-ttu-id="daa10-322">Aynı üst öğe içinde çakışan `@key` değerleristeniyorsa,değerleronaylanmaz.`@key`</span><span class="sxs-lookup"><span data-stu-id="daa10-322">If clashing `@key` values are requested within the same parent, the `@key` values won't be honored.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="daa10-323">Yaşam döngüsü yöntemleri</span><span class="sxs-lookup"><span data-stu-id="daa10-323">Lifecycle methods</span></span>

<span data-ttu-id="daa10-324">`OnInitAsync`ve `OnInit` bileşeni başlatmak için kodu yürütün.</span><span class="sxs-lookup"><span data-stu-id="daa10-324">`OnInitAsync` and `OnInit` execute code to initialize the component.</span></span> <span data-ttu-id="daa10-325">Zaman uyumsuz bir işlem gerçekleştirmek için, `OnInitAsync` `await` ve işlem üzerinde anahtar sözcüğünü kullanın:</span><span class="sxs-lookup"><span data-stu-id="daa10-325">To perform an asynchronous operation, use `OnInitAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="daa10-326">Zaman uyumlu bir işlem için şunu `OnInit`kullanın:</span><span class="sxs-lookup"><span data-stu-id="daa10-326">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

<span data-ttu-id="daa10-327">`OnParametersSetAsync`ve `OnParametersSet` bir bileşen üst öğeden parametreler aldığında ve değerler özelliklerine atandığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="daa10-327">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="daa10-328">Bu yöntemler bileşen başlatıldıktan sonra ve bileşen her işlendiğinde yürütülür:</span><span class="sxs-lookup"><span data-stu-id="daa10-328">These methods are executed after component initialization and each time the component is rendered:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="daa10-329">`OnAfterRenderAsync`ve `OnAfterRender` bir bileşen işlemeyi tamamladıktan sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="daa10-329">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="daa10-330">Öğe ve bileşen başvuruları bu noktada doldurulur.</span><span class="sxs-lookup"><span data-stu-id="daa10-330">Element and component references are populated at this point.</span></span> <span data-ttu-id="daa10-331">İşlenmiş DOM öğelerinde çalışan üçüncü taraf JavaScript kitaplıklarını etkinleştirme gibi, işlenmiş içeriği kullanarak ek başlatma adımları gerçekleştirmek için bu aşamayı kullanın.</span><span class="sxs-lookup"><span data-stu-id="daa10-331">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="daa10-332">İşleme sırasında tamamlanmamış zaman uyumsuz eylemleri işle</span><span class="sxs-lookup"><span data-stu-id="daa10-332">Handle incomplete async actions at render</span></span>

<span data-ttu-id="daa10-333">Yaşam döngüsü olaylarında gerçekleştirilen zaman uyumsuz eylemler, bileşen işlenmeden önce tamamlanmamış olabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-333">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="daa10-334">Yaşam döngüsü yöntemi `null` yürütülürken nesneler, verilerle tamamen doldurulmuş olabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-334">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="daa10-335">Nesnelerin başlatıldığını onaylamak için işleme mantığı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="daa10-335">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="daa10-336">Nesneler olduğunda `null`yer tutucu Kullanıcı arabirimi öğelerini (örneğin, bir yükleme iletisi) işleme.</span><span class="sxs-lookup"><span data-stu-id="daa10-336">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="daa10-337">Blazor şablonlarının `OnInitAsync`bileşeninde, Asychronously tahmin verileri al (`forecasts`) için geçersiz kılınır. `FetchData`</span><span class="sxs-lookup"><span data-stu-id="daa10-337">In the `FetchData` component of the Blazor templates, `OnInitAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="daa10-338">Ne zaman olduğunda `forecasts` ,kullanıcıyabiryüklemeiletisigörüntülenir.`null`</span><span class="sxs-lookup"><span data-stu-id="daa10-338">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="daa10-339">İşlem tamamlandıktan sonra, bileşen güncelleştirilmiş duruma `Task` `OnInitAsync` geri döner.</span><span class="sxs-lookup"><span data-stu-id="daa10-339">After the `Task` returned by `OnInitAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="daa10-340">*Pages/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="daa10-340">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="daa10-341">Parametreler ayarlanmadan önce kodu yürütün</span><span class="sxs-lookup"><span data-stu-id="daa10-341">Execute code before parameters are set</span></span>

<span data-ttu-id="daa10-342">`SetParameters`Parametreler ayarlanmadan önce kodu yürütmek için geçersiz kılınabilir:</span><span class="sxs-lookup"><span data-stu-id="daa10-342">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="daa10-343">Çağrılmadıysa `base.SetParameters` , özel kod gelen parametreler değerini gerekli herhangi bir şekilde yorumlayabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-343">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="daa10-344">Örneğin, gelen parametrelerin, sınıftaki özelliklere atanması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="daa10-344">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="daa10-345">Kullanıcı arabiriminin yenilenmesini gösterme</span><span class="sxs-lookup"><span data-stu-id="daa10-345">Suppress refreshing of the UI</span></span>

<span data-ttu-id="daa10-346">`ShouldRender`Kullanıcı arabiriminin yenilenmesini gizlemek için geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-346">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="daa10-347">Uygulama döndürürse `true`, Kullanıcı arabirimi yenilenir.</span><span class="sxs-lookup"><span data-stu-id="daa10-347">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="daa10-348">`ShouldRender` Geçersiz kılınsa bile, bileşen her zaman ilk olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="daa10-348">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="daa10-349">IDisposable ile bileşen atma</span><span class="sxs-lookup"><span data-stu-id="daa10-349">Component disposal with IDisposable</span></span>

<span data-ttu-id="daa10-350">Bir bileşen uygularsa <xref:System.IDisposable>, bileşen kullanıcı arabiriminden kaldırıldığında [Dispose yöntemi](/dotnet/standard/garbage-collection/implementing-dispose) çağırılır.</span><span class="sxs-lookup"><span data-stu-id="daa10-350">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="daa10-351">Aşağıdaki bileşen `@implements IDisposable` `Dispose` ve yöntemini kullanır:</span><span class="sxs-lookup"><span data-stu-id="daa10-351">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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

## <a name="routing"></a><span data-ttu-id="daa10-352">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="daa10-352">Routing</span></span>

<span data-ttu-id="daa10-353">Blazor ' de yönlendirme, uygulamadaki her erişilebilir bileşene bir rota şablonu sunarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-353">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="daa10-354">Bir `@page` yönergeyle bir Razor dosyası derlendiğinde, oluşturulan sınıfa yol şablonu <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> belirtilerek verilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-354">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="daa10-355">Çalışma zamanında, yönlendirici bileşen sınıflarını bir `RouteAttribute` ile arar ve hangi bileşenin istenen URL ile eşleşen bir rota şablonuna sahip olduğunu işler.</span><span class="sxs-lookup"><span data-stu-id="daa10-355">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="daa10-356">Birden çok yol şablonu, bir bileşene uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-356">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="daa10-357">Aşağıdaki bileşen ve `/BlazorRoute` `/DifferentBlazorRoute`için isteklere yanıt verir:</span><span class="sxs-lookup"><span data-stu-id="daa10-357">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="daa10-358">Rota parametreleri</span><span class="sxs-lookup"><span data-stu-id="daa10-358">Route parameters</span></span>

<span data-ttu-id="daa10-359">Bileşenler, `@page` yönergede belirtilen yol şablonundan rota parametreleri alabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-359">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="daa10-360">Yönlendirici, karşılık gelen bileşen parametrelerini doldurmak için yol parametrelerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="daa10-360">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="daa10-361">*Rota parametresi bileşeni*:</span><span class="sxs-lookup"><span data-stu-id="daa10-361">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="daa10-362">İsteğe bağlı parametreler desteklenmez, bu nedenle `@page` Yukarıdaki örnekte iki yönergeler uygulanır.</span><span class="sxs-lookup"><span data-stu-id="daa10-362">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="daa10-363">İlki, bir parametre olmadan bileşene gezinmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="daa10-363">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="daa10-364">İkinci `@page` yönerge, `{text}` yol parametresini alır ve değeri `Text` özelliğine atar.</span><span class="sxs-lookup"><span data-stu-id="daa10-364">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="daa10-365">"Arka plan kod" deneyimi için temel sınıf devralma</span><span class="sxs-lookup"><span data-stu-id="daa10-365">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="daa10-366">Bileşen dosyaları aynı dosyada HTML biçimlendirme C# ve işleme kodu karışımını.</span><span class="sxs-lookup"><span data-stu-id="daa10-366">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="daa10-367">Yönergesi `@inherits` , bileşen işaretlemesini işleme kodundan ayıran "arka plan kod" deneyimiyle Blazor uygulamalar sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-367">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="daa10-368">[Örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) , bileşenin özelliklerini ve yöntemlerini sağlamak için bir bileşenin temel sınıfı `BlazorRocksBase`nasıl devralmasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="daa10-368">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="daa10-369">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="daa10-369">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="daa10-370">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="daa10-370">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="daa10-371">Taban sınıfın türevi `ComponentBase`olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="daa10-371">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="daa10-372">Bileşenleri içeri aktar</span><span class="sxs-lookup"><span data-stu-id="daa10-372">Import components</span></span>

<span data-ttu-id="daa10-373">Razor ile yazılmış bir bileşenin ad alanı temel alınarak belirlenir:</span><span class="sxs-lookup"><span data-stu-id="daa10-373">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="daa10-374">Projenin `RootNamespace`.</span><span class="sxs-lookup"><span data-stu-id="daa10-374">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="daa10-375">Proje kökünden bileşene olan yol.</span><span class="sxs-lookup"><span data-stu-id="daa10-375">The path from the project root to the component.</span></span> <span data-ttu-id="daa10-376">Örneğin, `ComponentsSample/Pages/Index.razor` ad alanı `ComponentsSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="daa10-376">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="daa10-377">Bileşenler ad C# bağlama kurallarını izler.</span><span class="sxs-lookup"><span data-stu-id="daa10-377">Components follow C# name binding rules.</span></span> <span data-ttu-id="daa10-378">*Index. Razor*söz konusu olduğunda, aynı klasördeki, *sayfalardaki*ve üst klasördeki tüm bileşenler ( *componentssample*) kapsamdadır.</span><span class="sxs-lookup"><span data-stu-id="daa10-378">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="daa10-379">Farklı bir ad alanında tanımlanan bileşenler, Razor 'in [ \@using](xref:mvc/views/razor#using) yönergesi kullanılarak kapsama getirilebilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-379">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="daa10-380">Klasöründe `NavMenu.razor` `Index.razor` `@using` başka bir bileşen varsa, bileşeni aşağıdaki ifadesiyle birlikte kullanılabilir: `ComponentsSample/Shared/`</span><span class="sxs-lookup"><span data-stu-id="daa10-380">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="daa10-381">Bileşenlere Ayrıca kendi tam adları kullanılarak başvurulabilir, bu da [ \@using](xref:mvc/views/razor#using) yönergesinin gereksinimini ortadan kaldırır:</span><span class="sxs-lookup"><span data-stu-id="daa10-381">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="daa10-382">`global::` Nitelendirme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="daa10-382">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="daa10-383">Diğer ad `using` deyimleri (örneğin, `@using Foo = Bar`) ile bileşenleri içeri aktarma desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="daa10-383">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="daa10-384">Kısmen nitelenmiş adlar desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="daa10-384">Partially qualified names aren't supported.</span></span> <span data-ttu-id="daa10-385">Örneğin, ile `@using ComponentsSample` `NavMenu.razor` eklemevebaşvurudesteklenmez`<Shared.NavMenu></Shared.NavMenu>` .</span><span class="sxs-lookup"><span data-stu-id="daa10-385">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="daa10-386">Koşullu HTML öğesi öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="daa10-386">Conditional HTML element attributes</span></span>

<span data-ttu-id="daa10-387">HTML öğesi öznitelikleri, .NET değerine göre koşullu olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="daa10-387">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="daa10-388">Değer veya `false` `null`ise, öznitelik işlenmez.</span><span class="sxs-lookup"><span data-stu-id="daa10-388">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="daa10-389">Değer ise `true`, öznitelik küçültülmüş olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="daa10-389">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="daa10-390">Aşağıdaki örnekte, `IsCompleted` öğesinin biçimlendirmesinde işlenip işlenmeyeceğini `checked` belirler:</span><span class="sxs-lookup"><span data-stu-id="daa10-390">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

<span data-ttu-id="daa10-391">`IsCompleted` İse`true`, onay kutusu şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="daa10-391">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="daa10-392">`IsCompleted` İse`false`, onay kutusu şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="daa10-392">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="daa10-393">Daha fazla bilgi için bkz. <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="daa10-393">For more information, see <xref:mvc/views/razor>.</span></span>

## <a name="raw-html"></a><span data-ttu-id="daa10-394">Ham HTML</span><span class="sxs-lookup"><span data-stu-id="daa10-394">Raw HTML</span></span>

<span data-ttu-id="daa10-395">Dizeler normalde DOM metin düğümleri kullanılarak işlenir. Bu, içerdikleri tüm biçimlendirmenin yok sayıldığı ve değişmez değer olarak kabul edildiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="daa10-395">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="daa10-396">Ham HTML işlemek için, HTML içeriğini bir `MarkupString` değerde sarın.</span><span class="sxs-lookup"><span data-stu-id="daa10-396">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="daa10-397">Değer HTML veya SVG olarak ayrıştırılır ve DOM 'a eklenir.</span><span class="sxs-lookup"><span data-stu-id="daa10-397">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="daa10-398">Güvenilmeyen bir kaynaktan oluşturulan ham HTML işleme bir **güvenlik riskidir** ve kaçınılması gerekir!</span><span class="sxs-lookup"><span data-stu-id="daa10-398">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="daa10-399">Aşağıdaki örnek, bir bileşenin işlenmiş `MarkupString` çıktısına statik HTML içeriği bloğunu eklemek için türünün kullanımını gösterir:</span><span class="sxs-lookup"><span data-stu-id="daa10-399">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="daa10-400">Şablonlu bileşenler</span><span class="sxs-lookup"><span data-stu-id="daa10-400">Templated components</span></span>

<span data-ttu-id="daa10-401">Şablonlu bileşenler, bir veya daha fazla UI şablonunu parametre olarak kabul eden bileşenlerdir, daha sonra bileşen işleme mantığının bir parçası olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-401">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="daa10-402">Şablonlu bileşenler, normal bileşenlerden daha yeniden kullanılabilir olan üst düzey bileşenleri yazmanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="daa10-402">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="daa10-403">Birkaç örnek şunlardır:</span><span class="sxs-lookup"><span data-stu-id="daa10-403">A couple of examples include:</span></span>

* <span data-ttu-id="daa10-404">Kullanıcının tablo üst bilgisi, satırları ve altbilgisi için şablon belirtmesini sağlayan tablo bileşeni.</span><span class="sxs-lookup"><span data-stu-id="daa10-404">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="daa10-405">Bir kullanıcının bir listedeki öğeleri işlemek için şablon belirlemesine izin veren bir liste bileşenidir.</span><span class="sxs-lookup"><span data-stu-id="daa10-405">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="daa10-406">Şablon parametreleri</span><span class="sxs-lookup"><span data-stu-id="daa10-406">Template parameters</span></span>

<span data-ttu-id="daa10-407">Şablonlu bir bileşen, veya `RenderFragment` `RenderFragment<T>`türünde bir veya daha fazla bileşen parametresi belirtilerek tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="daa10-407">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="daa10-408">Bir işleme parçası, işlenecek Kullanıcı arabiriminin bir kesimini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="daa10-408">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="daa10-409">`RenderFragment<T>`işleme parçası çağrıldığında belirtilebildiği bir tür parametresi alır.</span><span class="sxs-lookup"><span data-stu-id="daa10-409">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="daa10-410">`TableTemplate`bileşeninde</span><span class="sxs-lookup"><span data-stu-id="daa10-410">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="daa10-411">Şablonlu bir bileşen kullanırken, şablon parametreleri parametrelerin adlarıyla (`TableHeader` ve `RowTemplate` aşağıdaki örnekte) eşleşen alt öğeler kullanılarak belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="daa10-411">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
<TableTemplate Items="@pets">
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

### <a name="template-context-parameters"></a><span data-ttu-id="daa10-412">Şablon bağlam parametreleri</span><span class="sxs-lookup"><span data-stu-id="daa10-412">Template context parameters</span></span>

<span data-ttu-id="daa10-413">Öğe olarak geçirilmiş türdeki `RenderFragment<T>` bileşen bağımsız değişkenleri adlı `context` örtük bir parametreye sahiptir (örneğin, `@context.PetId`Yukarıdaki kod örneğinden), ancak alt öğe `Context` özniteliğini kullanarak parametre adını değiştirebilirsiniz dosyalarında.</span><span class="sxs-lookup"><span data-stu-id="daa10-413">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="daa10-414">Aşağıdaki örnekte, `RowTemplate` `Context` öğesinin özniteliği `pet` parametresini belirtir:</span><span class="sxs-lookup"><span data-stu-id="daa10-414">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
<TableTemplate Items="@pets">
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

<span data-ttu-id="daa10-415">Alternatif olarak, bileşen öğesi üzerinde `Context` özniteliğini de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="daa10-415">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="daa10-416">Belirtilen `Context` öznitelik, belirtilen tüm şablon parametreleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="daa10-416">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="daa10-417">Bu, örtük alt içerik (herhangi bir sarmalama alt öğesi olmadan) için içerik parametre adını belirtmek istediğinizde yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-417">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="daa10-418">Aşağıdaki örnekte, `Context` özniteliği `TableTemplate` öğesinde görünür ve tüm şablon parametreleri için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="daa10-418">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
<TableTemplate Items="@pets" Context="pet">
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

### <a name="generic-typed-components"></a><span data-ttu-id="daa10-419">Genel olarak yazılmış bileşenler</span><span class="sxs-lookup"><span data-stu-id="daa10-419">Generic-typed components</span></span>

<span data-ttu-id="daa10-420">Şablonlu bileşenler çoğunlukla genel olarak türdedir.</span><span class="sxs-lookup"><span data-stu-id="daa10-420">Templated components are often generically typed.</span></span> <span data-ttu-id="daa10-421">Örneğin, değerleri işlemek `ListViewTemplate` `IEnumerable<T>` için genel bir bileşen kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-421">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="daa10-422">Genel bir bileşen tanımlamak için, tür parametrelerini `@typeparam` belirtmek için yönergesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="daa10-422">To define a generic component, use the `@typeparam` directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="daa10-423">Genel türsüz bileşenleri kullanırken tür parametresi mümkünse algılanır:</span><span class="sxs-lookup"><span data-stu-id="daa10-423">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="daa10-424">Aksi halde tür parametresi, tür parametresinin adıyla eşleşen bir öznitelik kullanılarak açıkça belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="daa10-424">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="daa10-425">Aşağıdaki örnekte, `TItem="Pet"` türü belirtir:</span><span class="sxs-lookup"><span data-stu-id="daa10-425">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="daa10-426">Değerleri ve parametreleri basamaklama</span><span class="sxs-lookup"><span data-stu-id="daa10-426">Cascading values and parameters</span></span>

<span data-ttu-id="daa10-427">Bazı senaryolarda, özellikle birden çok bileşen katmanı olduğunda, [bileşen parametreleri](#component-parameters)kullanarak bir üst bileşenden bir alt bileşene veri akışı yapmak uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="daa10-427">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="daa10-428">Değerleri ve parametreleri basamaklama, bir üst bileşenin tüm alt bileşenlerine değer sağlaması için kullanışlı bir yol sağlayarak bu sorunu çözebilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-428">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="daa10-429">Basamaklı değerler ve parametreler, bileşenlerin koordinasyonu için bir yaklaşım sağlar.</span><span class="sxs-lookup"><span data-stu-id="daa10-429">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="daa10-430">Tema örneği</span><span class="sxs-lookup"><span data-stu-id="daa10-430">Theme example</span></span>

<span data-ttu-id="daa10-431">Örnek uygulamadan aşağıdaki örnekte, `ThemeInfo` sınıfı, uygulamanın belirli bir bölümündeki tüm düğmelerin aynı stili paylaştığı şekilde bileşen hiyerarşisinin akışını yapmak için tema bilgilerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="daa10-431">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="daa10-432">*Uıthemeclasses/Themeınfo. cs*:</span><span class="sxs-lookup"><span data-stu-id="daa10-432">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="daa10-433">Bir üst bileşen basamaklı değer bileşeni kullanılarak basamaklı bir değer sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-433">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="daa10-434">`CascadingValue` Bileşen, bileşen hiyerarşisinin bir alt ağacını sarmalanmış ve bu alt ağaçta bulunan tüm bileşenlere tek bir değer sağlar.</span><span class="sxs-lookup"><span data-stu-id="daa10-434">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="daa10-435">Örneğin, örnek uygulama, uygulamanın düzenleriyle,`ThemeInfo` `@Body` özelliğin düzen gövdesini oluşturan tüm bileşenler için bir geçişli parametre olarak tema bilgilerini () belirtir.</span><span class="sxs-lookup"><span data-stu-id="daa10-435">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="daa10-436">`ButtonClass`öğesinin `btn-success` bir değeri, düzen bileşeninde atanır.</span><span class="sxs-lookup"><span data-stu-id="daa10-436">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="daa10-437">Tüm alt bileşenler bu özelliği `ThemeInfo` basamaklı nesne aracılığıyla kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-437">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="daa10-438">`CascadingValuesParametersLayout`bileşeninde</span><span class="sxs-lookup"><span data-stu-id="daa10-438">`CascadingValuesParametersLayout` component:</span></span>

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
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

<span data-ttu-id="daa10-439">Basamaklı değerleri kullanmak için, bileşenler `[CascadingParameter]` özniteliği kullanarak veya dize adı değerini temel alarak basamaklı parametreleri bildirir:</span><span class="sxs-lookup"><span data-stu-id="daa10-439">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

<span data-ttu-id="daa10-440">Aynı türde birden fazla basamaklı değer varsa ve bunları aynı alt ağaçta ayırt etmeniz gerekiyorsa, bir dize adı değeri ile bağlama geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="daa10-440">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="daa10-441">Basamaklı değerler, türe göre basamaklı parametrelere bağlanır.</span><span class="sxs-lookup"><span data-stu-id="daa10-441">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="daa10-442">Örnek uygulamada, `CascadingValuesParametersTheme` bileşen `ThemeInfo` basamaklı değeri basamaklı bir parametreye bağlar.</span><span class="sxs-lookup"><span data-stu-id="daa10-442">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="daa10-443">Parametresi, bileşen tarafından görünen düğmelerden birine ait CSS sınıfını ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="daa10-443">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="daa10-444">`CascadingValuesParametersTheme`bileşeninde</span><span class="sxs-lookup"><span data-stu-id="daa10-444">`CascadingValuesParametersTheme` component:</span></span>

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

### <a name="tabset-example"></a><span data-ttu-id="daa10-445">TabSet örneği</span><span class="sxs-lookup"><span data-stu-id="daa10-445">TabSet example</span></span>

<span data-ttu-id="daa10-446">Basamaklı parametreler, bileşenlerin bileşen hiyerarşisinde işbirliği yapmasına de olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="daa10-446">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="daa10-447">Örneğin, örnek uygulamada aşağıdaki *Tabset* örneğini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="daa10-447">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="daa10-448">Örnek uygulamada, sekmelerin uygulandığı `ITab` bir arabirim vardır:</span><span class="sxs-lookup"><span data-stu-id="daa10-448">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="daa10-449">Bileşen, `TabSet` çeşitli`Tab` bileşenleri içeren bileşenini kullanır: `CascadingValuesParametersTabSet`</span><span class="sxs-lookup"><span data-stu-id="daa10-449">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="daa10-450">Alt `Tab` bileşenler, `TabSet`öğesine açıkça parametre olarak aktarılmaz.</span><span class="sxs-lookup"><span data-stu-id="daa10-450">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="daa10-451">Bunun yerine, alt `Tab` bileşenleri `TabSet`öğesinin alt içeriğinin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="daa10-451">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="daa10-452">Bununla birlikte, `TabSet` üst bilgileri ve etkin sekmeyi işleyebilmesi için her `Tab` bileşen hakkında hala bilmeniz gerekir. Ek kod `TabSet` gerektirmeden bu koordinasyonu etkinleştirmek için bileşen, kendisini alt `Tab` bileşenler tarafından çekilen *basamaklı bir değer olarak sağlayabilir* .</span><span class="sxs-lookup"><span data-stu-id="daa10-452">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="daa10-453">`TabSet`bileşeninde</span><span class="sxs-lookup"><span data-stu-id="daa10-453">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="daa10-454">Alt bileşenler, kapsayan `TabSet` ' i `Tab` basamaklı bir parametre olarak yakalar, bu nedenle bileşenler, bu sekmenin `TabSet` etkin olduğu ve koordinasyonu üzerine eklenir. `Tab`</span><span class="sxs-lookup"><span data-stu-id="daa10-454">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="daa10-455">`Tab`bileşeninde</span><span class="sxs-lookup"><span data-stu-id="daa10-455">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="daa10-456">Razor şablonları</span><span class="sxs-lookup"><span data-stu-id="daa10-456">Razor templates</span></span>

<span data-ttu-id="daa10-457">Oluşturma parçaları Razor şablonu sözdizimi kullanılarak tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-457">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="daa10-458">Razor şablonları, bir UI parçacığı tanımlamak ve aşağıdaki biçimi varsaymak için bir yoldur:</span><span class="sxs-lookup"><span data-stu-id="daa10-458">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="daa10-459">Aşağıdaki örnek, bir bileşeni doğrudan bir `RenderFragment` bileşende `RenderFragment<T>` nasıl belirtdiğini ve işleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="daa10-459">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="daa10-460">Oluşturma parçaları, [şablonlu bileşenlere](#templated-components)bağımsız değişken olarak da geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-460">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

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

<span data-ttu-id="daa10-461">Önceki kodun işlenmiş çıktısı:</span><span class="sxs-lookup"><span data-stu-id="daa10-461">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="daa10-462">El ile RenderTreeBuilder mantığı</span><span class="sxs-lookup"><span data-stu-id="daa10-462">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="daa10-463">`Microsoft.AspNetCore.Components.RenderTree`C# kodu kodda el ile oluşturma dahil olmak üzere bileşenleri ve öğeleri düzenleme yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="daa10-463">`Microsoft.AspNetCore.Components.RenderTree` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="daa10-464">Bileşenlerini oluşturmak `RenderTreeBuilder` için kullanımı gelişmiş bir senaryodur.</span><span class="sxs-lookup"><span data-stu-id="daa10-464">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="daa10-465">Hatalı biçimlendirilmiş bir bileşen (örneğin, kapatılmamış bir biçimlendirme etiketi) tanımsız davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="daa10-465">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="daa10-466">Aşağıdaki `PetDetails` bileşeni, başka bir bileşende el ile yerleşik olarak kullanılabilecek şekilde göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="daa10-466">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    private string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="daa10-467">Aşağıdaki örnekte, `CreateComponent` yöntemindeki döngü üç `PetDetails` bileşen oluşturur.</span><span class="sxs-lookup"><span data-stu-id="daa10-467">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="daa10-468">Bileşenleri ( `RenderTreeBuilder` `OpenComponent` ve`AddAttribute`) oluşturmak için yöntemler çağrılırken, dizi numaraları kaynak kodu satır numaralarıdır.</span><span class="sxs-lookup"><span data-stu-id="daa10-468">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="daa10-469">Blazor fark algoritması, ayrı çağrı etkinleştirmeleri değil ayrı kod satırlarına karşılık gelen sıra numaralarına dayanır.</span><span class="sxs-lookup"><span data-stu-id="daa10-469">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="daa10-470">Yöntemler içeren `RenderTreeBuilder` bir bileşen oluştururken, dizi numaralarına yönelik bağımsız değişkenleri kod olarak kodlayın.</span><span class="sxs-lookup"><span data-stu-id="daa10-470">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="daa10-471">**Sıra numarasını oluşturmak için bir hesaplama veya sayaç kullanmak kötü performansa neden olabilir.**</span><span class="sxs-lookup"><span data-stu-id="daa10-471">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="daa10-472">Daha fazla bilgi için bkz. [kod satırı numaralarıyla Ilgili sıra numaraları ve yürütme sırası çalışmıyor](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) bölümü.</span><span class="sxs-lookup"><span data-stu-id="daa10-472">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="daa10-473">`BuiltContent`bileşeninde</span><span class="sxs-lookup"><span data-stu-id="daa10-473">`BuiltContent` component:</span></span>

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

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="daa10-474">Sıra numaraları, kod satırı numaralarıyla ilgilidir ve yürütme sırası değildir</span><span class="sxs-lookup"><span data-stu-id="daa10-474">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="daa10-475">Blazor `.razor` dosyaları her zaman derlenir.</span><span class="sxs-lookup"><span data-stu-id="daa10-475">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="daa10-476">Derleme adımının çalışma zamanında uygulama performansını geliştiren `.razor` bilgileri eklemek için kullanılabilmesi için bu büyük olasılıkla harika bir avantajdır.</span><span class="sxs-lookup"><span data-stu-id="daa10-476">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="daa10-477">Bu geliştirmelerin önemli bir örneği *sıra numarası*içerir.</span><span class="sxs-lookup"><span data-stu-id="daa10-477">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="daa10-478">Sıra numaraları, hangi çıkışların ayrı ve sıralı kod satırlarından geldiğini çalışma zamanına işaret ediyor.</span><span class="sxs-lookup"><span data-stu-id="daa10-478">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="daa10-479">Çalışma zamanı, doğrusal bir zamanda, genel ağaç farkı algoritması için genellikle mümkün olandan çok daha hızlı olan etkili ağaç SLA 'ları oluşturmak için bu bilgileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="daa10-479">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="daa10-480">Aşağıdaki basit `.razor` dosyayı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="daa10-480">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="daa10-481">Yukarıdaki kod, aşağıdakine benzer şekilde derlenir:</span><span class="sxs-lookup"><span data-stu-id="daa10-481">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="daa10-482">Kod ilk kez `someFlag` `true`çalıştırıldığında, Oluşturucu şunları alır:</span><span class="sxs-lookup"><span data-stu-id="daa10-482">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="daa10-483">Dizisi</span><span class="sxs-lookup"><span data-stu-id="daa10-483">Sequence</span></span> | <span data-ttu-id="daa10-484">Type</span><span class="sxs-lookup"><span data-stu-id="daa10-484">Type</span></span>      | <span data-ttu-id="daa10-485">Data</span><span class="sxs-lookup"><span data-stu-id="daa10-485">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="daa10-486">0</span><span class="sxs-lookup"><span data-stu-id="daa10-486">0</span></span>        | <span data-ttu-id="daa10-487">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="daa10-487">Text node</span></span> | <span data-ttu-id="daa10-488">Adı</span><span class="sxs-lookup"><span data-stu-id="daa10-488">First</span></span>  |
| <span data-ttu-id="daa10-489">1\.</span><span class="sxs-lookup"><span data-stu-id="daa10-489">1</span></span>        | <span data-ttu-id="daa10-490">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="daa10-490">Text node</span></span> | <span data-ttu-id="daa10-491">Saniye</span><span class="sxs-lookup"><span data-stu-id="daa10-491">Second</span></span> |

<span data-ttu-id="daa10-492">`someFlag` Olduğunu`false`düşünün ve biçimlendirme yeniden işlenir.</span><span class="sxs-lookup"><span data-stu-id="daa10-492">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="daa10-493">Bu kez, Oluşturucu şunları alır:</span><span class="sxs-lookup"><span data-stu-id="daa10-493">This time, the builder receives:</span></span>

| <span data-ttu-id="daa10-494">Dizisi</span><span class="sxs-lookup"><span data-stu-id="daa10-494">Sequence</span></span> | <span data-ttu-id="daa10-495">Type</span><span class="sxs-lookup"><span data-stu-id="daa10-495">Type</span></span>       | <span data-ttu-id="daa10-496">Data</span><span class="sxs-lookup"><span data-stu-id="daa10-496">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="daa10-497">1\.</span><span class="sxs-lookup"><span data-stu-id="daa10-497">1</span></span>        | <span data-ttu-id="daa10-498">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="daa10-498">Text node</span></span>  | <span data-ttu-id="daa10-499">Saniye</span><span class="sxs-lookup"><span data-stu-id="daa10-499">Second</span></span> |

<span data-ttu-id="daa10-500">Çalışma zamanı bir fark gerçekleştirdiğinde, sıradaki `0` öğenin kaldırıldığını görür, bu nedenle aşağıdaki önemsiz *düzenleme betiğini*oluşturur:</span><span class="sxs-lookup"><span data-stu-id="daa10-500">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="daa10-501">İlk metin düğümünü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="daa10-501">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="daa10-502">Program aracılığıyla sıra numaraları oluşturursanız ne yanlış gider</span><span class="sxs-lookup"><span data-stu-id="daa10-502">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="daa10-503">Aşağıdaki işleme Ağacı Oluşturucusu mantığını yazmanız yerine düşünün:</span><span class="sxs-lookup"><span data-stu-id="daa10-503">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="daa10-504">Şimdi ilk çıktı:</span><span class="sxs-lookup"><span data-stu-id="daa10-504">Now, the first output is:</span></span>

| <span data-ttu-id="daa10-505">Dizisi</span><span class="sxs-lookup"><span data-stu-id="daa10-505">Sequence</span></span> | <span data-ttu-id="daa10-506">Type</span><span class="sxs-lookup"><span data-stu-id="daa10-506">Type</span></span>      | <span data-ttu-id="daa10-507">Data</span><span class="sxs-lookup"><span data-stu-id="daa10-507">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="daa10-508">0</span><span class="sxs-lookup"><span data-stu-id="daa10-508">0</span></span>        | <span data-ttu-id="daa10-509">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="daa10-509">Text node</span></span> | <span data-ttu-id="daa10-510">Adı</span><span class="sxs-lookup"><span data-stu-id="daa10-510">First</span></span>  |
| <span data-ttu-id="daa10-511">1\.</span><span class="sxs-lookup"><span data-stu-id="daa10-511">1</span></span>        | <span data-ttu-id="daa10-512">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="daa10-512">Text node</span></span> | <span data-ttu-id="daa10-513">Saniye</span><span class="sxs-lookup"><span data-stu-id="daa10-513">Second</span></span> |

<span data-ttu-id="daa10-514">Bu sonuç önceki bir durum ile aynıdır, bu nedenle olumsuz bir sorun yoktur.</span><span class="sxs-lookup"><span data-stu-id="daa10-514">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="daa10-515">`someFlag``false` ikinci işleme ve çıktı:</span><span class="sxs-lookup"><span data-stu-id="daa10-515">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="daa10-516">Dizisi</span><span class="sxs-lookup"><span data-stu-id="daa10-516">Sequence</span></span> | <span data-ttu-id="daa10-517">Type</span><span class="sxs-lookup"><span data-stu-id="daa10-517">Type</span></span>      | <span data-ttu-id="daa10-518">Data</span><span class="sxs-lookup"><span data-stu-id="daa10-518">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="daa10-519">0</span><span class="sxs-lookup"><span data-stu-id="daa10-519">0</span></span>        | <span data-ttu-id="daa10-520">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="daa10-520">Text node</span></span> | <span data-ttu-id="daa10-521">Saniye</span><span class="sxs-lookup"><span data-stu-id="daa10-521">Second</span></span> |

<span data-ttu-id="daa10-522">Bu kez, fark algoritması *iki* değişikliğin oluştuğunu görür ve algoritma aşağıdaki düzenleme betiğini üretir:</span><span class="sxs-lookup"><span data-stu-id="daa10-522">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="daa10-523">İlk metin düğümünün değerini olarak `Second`değiştirin.</span><span class="sxs-lookup"><span data-stu-id="daa10-523">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="daa10-524">İkinci metin düğümünü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="daa10-524">Remove the second text node.</span></span>

<span data-ttu-id="daa10-525">Sıra numaralarının oluşturulması, `if/else` dal ve döngülerin orijinal kodda bulunduğu yer hakkındaki tüm yararlı bilgileri kaybetti.</span><span class="sxs-lookup"><span data-stu-id="daa10-525">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="daa10-526">Bu, daha önce olduğu gibi bir fark **ile iki kez** sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="daa10-526">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="daa10-527">Bu, önemsiz bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="daa10-527">This is a trivial example.</span></span> <span data-ttu-id="daa10-528">Karmaşık ve derin iç içe yapıları ve özellikle döngülerle daha gerçekçi durumlarda performans maliyeti daha önemlidir.</span><span class="sxs-lookup"><span data-stu-id="daa10-528">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="daa10-529">Hangi döngü bloklarının veya dallarının eklendiğini veya kaldırıldığını hemen belirlemek yerine, fark algoritmasının işleme ağaçlarına katmaları ve genellikle eski ve yeni yapıların nasıl olduğu hakkında yanlış bir biçimde düzenleme betikleri oluşturması birbirleriyle ilişkilendir.</span><span class="sxs-lookup"><span data-stu-id="daa10-529">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="daa10-530">Kılavuz ve ekibinizle</span><span class="sxs-lookup"><span data-stu-id="daa10-530">Guidance and conclusions</span></span>

* <span data-ttu-id="daa10-531">Sıra numaraları dinamik olarak oluşturulursa uygulama performansı de vardır.</span><span class="sxs-lookup"><span data-stu-id="daa10-531">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="daa10-532">Altyapı, derleme zamanında yakalanmadığı takdirde gerekli bilgiler bulunmadığından, çalışma zamanında kendi sıra numaralarını otomatik olarak oluşturamaz.</span><span class="sxs-lookup"><span data-stu-id="daa10-532">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="daa10-533">El ile uygulanan `RenderTreeBuilder` mantık uzun blokları yazmayın.</span><span class="sxs-lookup"><span data-stu-id="daa10-533">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="daa10-534">Dosyaları `.razor` tercih edin ve derleyicinin sıra numaralarıyla uğraşmak için izin verin.</span><span class="sxs-lookup"><span data-stu-id="daa10-534">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span>
* <span data-ttu-id="daa10-535">Dizi numaraları sabit kodluysa, fark algoritması yalnızca değer değerinde sıra numaralarının artırılmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="daa10-535">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="daa10-536">İlk değer ve boşluklar ilgisiz.</span><span class="sxs-lookup"><span data-stu-id="daa10-536">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="daa10-537">Tek bir seçenek, kod satırı numarasını sıra numarası olarak kullanmak veya sıfırdan başlayıp bir ya da yüzlerce (ya da tercih edilen aralığa) artırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="daa10-537">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="daa10-538">Blazor, sıra numaralarını kullanır, diğer ağaç dağıtma Kullanıcı arabirimi çerçeveleri bunları kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="daa10-538">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="daa10-539">Dizi numaraları kullanıldığında ve Blazor, geliştiricilerin yazma `.razor` dosyaları için otomatik olarak sıra numaralarıyla ilgilenen bir derleme adımının avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="daa10-539">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>
