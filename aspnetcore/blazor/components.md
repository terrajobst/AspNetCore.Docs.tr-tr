---
title: ASP.NET Core Razor bileşenleri oluşturma ve kullanma
author: guardrex
description: Veri bağlama, olayları işleme ve bileşen yaşam döngülerini yönetme dahil Razor bileşenleri oluşturmayı ve kullanmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/20/2019
uid: blazor/components
ms.openlocfilehash: 065a3a078c56f813ed38f85d7414f22061217dff
ms.sourcegitcommit: eb4fcdeb2f9e8413117624de42841a4997d1d82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697958"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="66faa-103">ASP.NET Core Razor bileşenleri oluşturma ve kullanma</span><span class="sxs-lookup"><span data-stu-id="66faa-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="66faa-104">, [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="66faa-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="66faa-105">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="66faa-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="66faa-106">Blazor uygulamaları, *bileşenleri*kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="66faa-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="66faa-107">Bir bileşen, bir sayfa, iletişim veya form gibi bir kullanıcı arabirimi (UI) öbekidir.</span><span class="sxs-lookup"><span data-stu-id="66faa-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="66faa-108">Bir bileşen, veri eklemek veya UI olaylarına yanıt vermek için gereken HTML işaretlemesini ve işleme mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="66faa-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="66faa-109">Bileşenler esnek ve hafif.</span><span class="sxs-lookup"><span data-stu-id="66faa-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="66faa-110">Bunlar, iç içe geçmiş, yeniden kullanılabilir ve projeler arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="66faa-111">Bileşen sınıfları</span><span class="sxs-lookup"><span data-stu-id="66faa-111">Component classes</span></span>

<span data-ttu-id="66faa-112">Bileşenler, C# ve HTML Işaretlemesi kullanılarak [Razor](xref:mvc/views/razor) bileşen dosyalarında ( *. Razor*) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="66faa-113">Blazor içindeki bir bileşen, bir *Razor bileşeni*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="66faa-114">Bir bileşenin adı, büyük harfle başlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="66faa-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="66faa-115">Örneğin, *mycoolcomponent. Razor* geçerlidir ve *mycoolcomponent. Razor* geçersizdir.</span><span class="sxs-lookup"><span data-stu-id="66faa-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="66faa-116">Bir bileşen için Kullanıcı arabirimi HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="66faa-117">Dinamik işleme mantığı (örneğin, döngüler, koşullar, ifadeler) C# [Razor](xref:mvc/views/razor)adlı gömülü bir sözdizimi kullanılarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="66faa-118">Bir uygulama derlendiğinde, HTML biçimlendirme ve C# işleme mantığı bir bileşen sınıfına dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="66faa-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="66faa-119">Oluşturulan sınıfın adı, dosyanın adıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="66faa-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="66faa-120">Bileşen sınıfının üyeleri `@code` bloğunda tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="66faa-121">@No__t_0 bloğunda, bileşen durumu (özellikler, alanlar) olay işleme yöntemleriyle veya diğer bileşen mantığını tanımlamaya yönelik yöntemlerle belirtilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="66faa-122">Birden fazla `@code` bloğu izin verilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-122">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="66faa-123">ASP.NET Core 3,0 ' nin önceki önizlemelerinde, `@functions` blokları Razor bileşenlerinde `@code` bloklarında aynı amaçla kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="66faa-123">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="66faa-124">`@functions` blokları Razor bileşenlerinde çalışmaya devam eder, ancak ASP.NET Core 3,0 Preview 6 veya sonraki bir sürümünde `@code` bloğunu kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="66faa-124">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="66faa-125">Bileşen üyeleri, `@` ile başlayan ifadeler kullanılarak C# bileşen işleme mantığının bir parçası olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-125">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="66faa-126">Örneğin, bir C# alan `@` ' i alan adı ' na önek olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-126">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="66faa-127">Aşağıdaki örnek değerlendirilir ve işler:</span><span class="sxs-lookup"><span data-stu-id="66faa-127">The following example evaluates and renders:</span></span>

* <span data-ttu-id="66faa-128">`font-style` için CSS özellik değerine `_headingFontStyle`.</span><span class="sxs-lookup"><span data-stu-id="66faa-128">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="66faa-129">`<h1>` öğesinin içeriğine `_headingText`.</span><span class="sxs-lookup"><span data-stu-id="66faa-129">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="66faa-130">Bileşen ilk olarak işlendikten sonra, bileşen işleme ağacını olaylara yanıt olarak yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="66faa-130">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="66faa-131">Blazor ardından yeni işleme ağacını önceki bir ile karşılaştırır ve tarayıcının Belge Nesne Modeli (DOM) üzerinde herhangi bir değişiklik uygular.</span><span class="sxs-lookup"><span data-stu-id="66faa-131">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="66faa-132">Bileşenler sıradan C# sınıflardır ve bir proje içinde herhangi bir yere yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-132">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="66faa-133">Web sayfalarını üreten bileşenler genellikle *Sayfalar* klasöründe bulunur.</span><span class="sxs-lookup"><span data-stu-id="66faa-133">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="66faa-134">Sayfa olmayan bileşenler sıklıkla *paylaşılan* klasöre veya projeye eklenen özel bir klasöre yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-134">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="66faa-135">Özel bir klasör kullanmak için, özel klasörün ad alanını üst bileşene ya da uygulamanın *_ımports. Razor* dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="66faa-135">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="66faa-136">Örneğin, aşağıdaki ad alanı, uygulamanın kök ad alanı `WebApplication` olduğunda bir *Bileşenler* klasöründeki bileşenleri kullanılabilir yapar:</span><span class="sxs-lookup"><span data-stu-id="66faa-136">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="66faa-137">Bileşenleri Razor Pages ve MVC uygulamalarıyla tümleştirme</span><span class="sxs-lookup"><span data-stu-id="66faa-137">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="66faa-138">Mevcut Razor Pages ve MVC uygulamalarıyla bileşenleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="66faa-138">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="66faa-139">Razor bileşenleri kullanmak için mevcut sayfaları veya görünümleri yeniden yazmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="66faa-139">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="66faa-140">Sayfa veya görünüm işlendiğinde, bileşenler aynı anda önceden işlenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-140">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="66faa-141">Bir sayfadan veya görünümden bir bileşeni işlemek için `RenderComponentAsync<TComponent>` HTML yardımcı yöntemini kullanın:</span><span class="sxs-lookup"><span data-stu-id="66faa-141">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="MyComponent">
    @(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
</div>
```

<span data-ttu-id="66faa-142">Sayfalar ve görünümler bileşenleri kullanırken, listesiyse doğru değildir.</span><span class="sxs-lookup"><span data-stu-id="66faa-142">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="66faa-143">Bileşenler, kısmi görünümler ve bölümler gibi görüntüleme ve sayfaya özgü senaryolar kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="66faa-143">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="66faa-144">Bir bileşende kısmi görünümden mantığı kullanmak için kısmi görünüm mantığını bir bileşene ayırın.</span><span class="sxs-lookup"><span data-stu-id="66faa-144">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="66faa-145">Bileşenlerin nasıl işlendiği ve bileşen durumunun Blazor sunucu uygulamalarında nasıl yönetildiği hakkında daha fazla bilgi için <xref:blazor/hosting-models> makalesine bakın.</span><span class="sxs-lookup"><span data-stu-id="66faa-145">For more information on how components are rendered and component state is managed in Blazor Server apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="use-components"></a><span data-ttu-id="66faa-146">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="66faa-146">Use components</span></span>

<span data-ttu-id="66faa-147">Bileşenler, HTML öğesi söz dizimini kullanarak bildirerek diğer bileşenleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-147">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="66faa-148">Bir bileşeni kullanmak için biçimlendirme, etiket adının bileşen türü olduğu bir HTML etiketi gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="66faa-148">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="66faa-149">Öznitelik bağlama büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="66faa-149">Attribute binding is case sensitive.</span></span> <span data-ttu-id="66faa-150">Örneğin, `@bind` geçerli ve `@Bind` geçersiz.</span><span class="sxs-lookup"><span data-stu-id="66faa-150">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="66faa-151">*Index. Razor* dosyasında aşağıdaki biçimlendirme `HeadingComponent` örneğini işler:</span><span class="sxs-lookup"><span data-stu-id="66faa-151">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="66faa-152">*Bileşenler/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="66faa-152">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="66faa-153">Bir bileşen bir bileşen adıyla eşleşmeyen büyük harfle yazılmış bir HTML öğesi içeriyorsa, öğenin beklenmeyen bir adı olduğunu gösteren bir uyarı yayınlanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-153">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="66faa-154">Bileşenin ad alanı için `@using` ifadesinin eklenmesi, bileşenin kullanılabilir olmasını sağlar ve bu da uyarıyı kaldırır.</span><span class="sxs-lookup"><span data-stu-id="66faa-154">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="66faa-155">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="66faa-155">Component parameters</span></span>

<span data-ttu-id="66faa-156">Bileşenler, bileşen sınıfında `[Parameter]` özniteliğiyle ortak özellikler kullanılarak tanımlanan *bileşen parametrelerine*sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-156">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="66faa-157">Biçimlendirme içindeki bir bileşenin bağımsız değişkenlerini belirtmek için öznitelikleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="66faa-157">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="66faa-158">*Bileşenler/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="66faa-158">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="66faa-159">Aşağıdaki örnekte, `ParentComponent` `ChildComponent` ' nin `Title` özelliğinin değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-159">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="66faa-160">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="66faa-160">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="66faa-161">Alt içerik</span><span class="sxs-lookup"><span data-stu-id="66faa-161">Child content</span></span>

<span data-ttu-id="66faa-162">Bileşenler, başka bir bileşenin içeriğini ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-162">Components can set the content of another component.</span></span> <span data-ttu-id="66faa-163">Atama bileşeni, alıcı bileşeni belirten Etiketler arasında içerik sağlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-163">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="66faa-164">Aşağıdaki örnekte `ChildComponent`, işlemek için bir kullanıcı arabirimi segmentini temsil eden bir `RenderFragment` temsil eden bir `ChildContent` özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="66faa-164">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="66faa-165">@No__t_0 değeri bileşenin, içeriğin işlenmesi gereken biçimlendirmesinde konumlandırılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-165">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="66faa-166">@No__t_0 değeri üst bileşenden alınır ve önyükleme bölmesinin `panel-body` içinde işlenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-166">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="66faa-167">*Bileşenler/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="66faa-167">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="66faa-168">@No__t_0 içeriği alan özelliğin kurala göre `ChildContent` olarak adlandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="66faa-168">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="66faa-169">Aşağıdaki `ParentComponent`, içeriği `<ChildComponent>` etiketlerinin içine yerleştirerek `ChildComponent` ' i işlemek için içerik sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-169">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="66faa-170">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="66faa-170">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="66faa-171">Öznitelik döndürme ve rastgele parametreler</span><span class="sxs-lookup"><span data-stu-id="66faa-171">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="66faa-172">Bileşenler, bileşen tarafından tanımlanan parametrelere ek olarak ek öznitelikler yakalayabilir ve işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-172">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="66faa-173">Ek öznitelikler bir sözlükte yakalanabilir *ve sonra bileşen* [@attributes](xref:mvc/views/razor#attributes) Razor yönergesi kullanılarak işlendiğinde bir öğe üzerine bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-173">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="66faa-174">Bu senaryo, çeşitli özelleştirmeleri destekleyen bir işaretleme öğesi üreten bir bileşen tanımlarken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="66faa-174">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="66faa-175">Örneğin, çok sayıda parametreyi destekleyen bir `<input>` için öznitelikleri ayrı olarak tanımlamak sıkıcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-175">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="66faa-176">Aşağıdaki örnekte, ilk `<input>` öğesi (`id="useIndividualParams"`) tek tek bileşen parametrelerini kullanır, ancak ikinci `<input>` öğesi (`id="useAttributesDict"`) öznitelik splatesini kullanır:</span><span class="sxs-lookup"><span data-stu-id="66faa-176">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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

<span data-ttu-id="66faa-177">Parametrenin türü dize anahtarlarıyla `IEnumerable<KeyValuePair<string, object>>` uygulamalıdır.</span><span class="sxs-lookup"><span data-stu-id="66faa-177">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="66faa-178">@No__t_0 kullanmak Bu senaryoda da bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="66faa-178">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="66faa-179">Her iki yaklaşımın de kullanıldığı işlenen `<input>` öğeleri aynıdır:</span><span class="sxs-lookup"><span data-stu-id="66faa-179">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="66faa-180">Rastgele öznitelikleri kabul etmek için, `CaptureUnmatchedValues` özelliği `true` olarak ayarlanan `[Parameter]` özniteliğini kullanarak bir bileşen parametresi tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="66faa-180">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="66faa-181">@No__t_1 `CaptureUnmatchedValues` özelliği, parametrenin diğer bir parametreyle eşleşmeyen tüm özniteliklerle eşleşmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-181">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="66faa-182">Bir bileşen yalnızca `CaptureUnmatchedValues` olan tek bir parametre tanımlayabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-182">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="66faa-183">@No__t_0 ile kullanılan özellik türü, `Dictionary<string, object>` dize anahtarlarıyla atanabilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="66faa-183">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="66faa-184">`IEnumerable<KeyValuePair<string, object>>` veya `IReadOnlyDictionary<string, object>` Ayrıca bu senaryodaki seçeneklerdir.</span><span class="sxs-lookup"><span data-stu-id="66faa-184">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="66faa-185">Veri bağlama</span><span class="sxs-lookup"><span data-stu-id="66faa-185">Data binding</span></span>

<span data-ttu-id="66faa-186">Hem bileşenlere hem de DOM öğelerine veri bağlama [@bind](xref:mvc/views/razor#bind) özniteliğiyle gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-186">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="66faa-187">Aşağıdaki örnek bir `CurrentValue` özelliğini metin kutusu değerine bağlar:</span><span class="sxs-lookup"><span data-stu-id="66faa-187">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```cshtml
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="66faa-188">Metin kutusu odağı kaybettiğinde, özelliğin değeri güncellenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-188">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="66faa-189">Metin kutusu kullanıcı arabiriminde, özelliğin değerini değiştirme yanıt olarak değil, yalnızca bileşen işlendiğinde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-189">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="66faa-190">Bileşenler olay işleyicisi kodu yürütüldükten sonra kendilerini oluşturduğundan, özellik güncelleştirmeleri *genellikle* olay işleyicisi tetiklendikten hemen sonra Kullanıcı arabirimine yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-190">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="66faa-191">@No__t_1 özelliği (`<input @bind="CurrentValue" />`) ile `@bind` kullanmak, temelde aşağıdakilere eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="66faa-191">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="66faa-192">Bileşen işlendiğinde, giriş öğesinin `value` ' ı `CurrentValue` özelliğinden gelir.</span><span class="sxs-lookup"><span data-stu-id="66faa-192">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="66faa-193">Kullanıcı metin kutusuna yazdığında ve öğe odağını değiştirdiğinde, `onchange` olayı tetiklenir ve `CurrentValue` özelliği değiştirilen değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-193">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="66faa-194">@No__t_0, tür dönüştürmelerin gerçekleştirildiği durumları işlediği için kod oluşturma daha karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="66faa-194">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="66faa-195">İlke ' de, `@bind` bir ifadenin geçerli değerini bir `value` özniteliğiyle ilişkilendirir ve kayıtlı işleyiciyi kullanarak değişiklikleri işler.</span><span class="sxs-lookup"><span data-stu-id="66faa-195">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="66faa-196">@No__t_1 sözdizimiyle `onchange` olaylarının işlenmesine ek olarak, bir özellik veya alan, `event` parametreli bir [@bind-value](xref:mvc/views/razor#bind) özniteliği belirterek diğer olaylar kullanılarak da bağlanabilir ([ @bind-value:event](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="66faa-196">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="66faa-197">Aşağıdaki örnek, `CurrentValue` özelliğini `oninput` olayı için bağlar:</span><span class="sxs-lookup"><span data-stu-id="66faa-197">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="66faa-198">@No__t_0 aksine, öğe odağı kaybettiğinde harekete geçirilir `oninput` metin kutusunun değeri değiştiğinde harekete geçirilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-198">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="66faa-199">**Ayrıştırılamayan değerler**</span><span class="sxs-lookup"><span data-stu-id="66faa-199">**Unparsable values**</span></span>

<span data-ttu-id="66faa-200">Bir Kullanıcı, bir veri sınırlama öğesine ayrıştırılamayan bir değer sağlıyorsa, bağlama olayı tetiklendiğinde, çözümlenemeyen değer otomatik olarak önceki değerine döndürülür.</span><span class="sxs-lookup"><span data-stu-id="66faa-200">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="66faa-201">Aşağıdaki senaryoyu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="66faa-201">Consider the following scenario:</span></span>

* <span data-ttu-id="66faa-202">Bir `<input>` öğesi, `123` başlangıçtaki değeri olan bir `int` türüne bağlanır:</span><span class="sxs-lookup"><span data-stu-id="66faa-202">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```cshtml
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="66faa-203">Kullanıcı, öğe değerini sayfada `123.45` olarak güncelleştirir ve öğe odağını değiştirir.</span><span class="sxs-lookup"><span data-stu-id="66faa-203">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="66faa-204">Önceki senaryoda, öğenin değeri `123` ' a geri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="66faa-204">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="66faa-205">Değer `123.45` özgün `123` değerinin yararına reddedildiğinde, Kullanıcı değerinin kabul edilmediğini anlamıştır.</span><span class="sxs-lookup"><span data-stu-id="66faa-205">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="66faa-206">Varsayılan olarak, bağlama öğenin `onchange` olayına uygulanır (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="66faa-206">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="66faa-207">Farklı bir olay ayarlamak için `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` kullanın.</span><span class="sxs-lookup"><span data-stu-id="66faa-207">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="66faa-208">@No__t_0 olayı (`@bind-value:event="oninput"`) için yeniden sürüm, ayrıştırılamayan bir değer sunan herhangi bir tuş vuruşu sonrasında oluşur.</span><span class="sxs-lookup"><span data-stu-id="66faa-208">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="66faa-209">@No__t_0 olayı `int` bağlantılı bir türle hedeflenirken, kullanıcının bir `.` karakteri yazmasının engellenmiş olması engellenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-209">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="66faa-210">@No__t_0 bir karakter hemen kaldırılır, bu nedenle Kullanıcı yalnızca tam sayılara izin verilen anında geri bildirim alır.</span><span class="sxs-lookup"><span data-stu-id="66faa-210">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="66faa-211">@No__t_0 olaylarındaki değerin geri döndürülmesi ideal olmayan, örneğin kullanıcının ayrıştırılamayan `<input>` bir değeri temizlemeye izin verilmesi gereken senaryolar vardır.</span><span class="sxs-lookup"><span data-stu-id="66faa-211">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="66faa-212">Alternatifler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="66faa-212">Alternatives include:</span></span>

* <span data-ttu-id="66faa-213">@No__t_0 olayını kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="66faa-213">Don't use the `oninput` event.</span></span> <span data-ttu-id="66faa-214">Öğe odağı kaybederene kadar geçersiz bir değer geri döndürülmediğinde, varsayılan `onchange` olayını (`@bind="{PROPERTY OR FIELD}"`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="66faa-214">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="66faa-215">@No__t_0 veya `string` gibi null yapılabilir bir türe bağlayın ve geçersiz girdileri işlemek için özel mantık sağlayın.</span><span class="sxs-lookup"><span data-stu-id="66faa-215">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="66faa-216">@No__t_1 veya `InputDate` gibi bir [form doğrulama bileşeni](xref:blazor/forms-validation)kullanın.</span><span class="sxs-lookup"><span data-stu-id="66faa-216">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="66faa-217">Form doğrulama bileşenlerinde geçersiz girişleri yönetmek için yerleşik destek vardır.</span><span class="sxs-lookup"><span data-stu-id="66faa-217">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="66faa-218">Form doğrulama bileşenleri:</span><span class="sxs-lookup"><span data-stu-id="66faa-218">Form validation components:</span></span>
  * <span data-ttu-id="66faa-219">Kullanıcının, ilişkili `EditContext` ' da geçersiz giriş sağlamasına ve doğrulama hataları almasına izin verin.</span><span class="sxs-lookup"><span data-stu-id="66faa-219">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="66faa-220">Kullanıcı ek WebForm verisi girmeye uğramadan doğrulama hatalarını Kullanıcı ARABIRIMINDE görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="66faa-220">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="66faa-221">**Genelleştirme**</span><span class="sxs-lookup"><span data-stu-id="66faa-221">**Globalization**</span></span>

<span data-ttu-id="66faa-222">`@bind` değerleri, geçerli kültürün kuralları kullanılarak görüntülenmek üzere biçimlendirilir ve ayrıştırılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-222">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="66faa-223">Geçerli kültüre <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> özelliğinden erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-223">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="66faa-224">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) aşağıdaki alan türleri için kullanılır (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="66faa-224">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="66faa-225">Yukarıdaki alan türleri:</span><span class="sxs-lookup"><span data-stu-id="66faa-225">The preceding field types:</span></span>

* <span data-ttu-id="66faa-226">, Uygun tarayıcı tabanlı biçimlendirme kuralları kullanılarak görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-226">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="66faa-227">Serbest biçimli metin içeremez.</span><span class="sxs-lookup"><span data-stu-id="66faa-227">Can't contain free-form text.</span></span>
* <span data-ttu-id="66faa-228">Tarayıcının uygulamasına göre Kullanıcı etkileşimi özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-228">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="66faa-229">Aşağıdaki alan türleri özel biçimlendirme gereksinimlerine sahiptir ve şu anda tüm büyük tarayıcılarda desteklenmediği için Blazor tarafından desteklenmemektedir:</span><span class="sxs-lookup"><span data-stu-id="66faa-229">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="66faa-230">`@bind`, bir değeri ayrıştırmak ve biçimlendirmek için <xref:System.Globalization.CultureInfo?displayProperty=fullName> sağlamak üzere `@bind:culture` parametresini destekler.</span><span class="sxs-lookup"><span data-stu-id="66faa-230">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="66faa-231">@No__t_0 ve `number` alan türleri kullanılırken bir kültür belirtilmesi önerilmez.</span><span class="sxs-lookup"><span data-stu-id="66faa-231">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="66faa-232">`date` ve `number`, gerekli kültürü sağlayan yerleşik Blazor desteğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="66faa-232">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="66faa-233">Kullanıcının kültürünü ayarlama hakkında daha fazla bilgi için [Yerelleştirme](#localization) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="66faa-233">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="66faa-234">**Biçim dizeleri**</span><span class="sxs-lookup"><span data-stu-id="66faa-234">**Format strings**</span></span>

<span data-ttu-id="66faa-235">Veri bağlama, [@bind:format](xref:mvc/views/razor#bind)kullanılarak <xref:System.DateTime> biçim dizeleriyle birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-235">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="66faa-236">Para birimi veya sayı biçimleri gibi diğer biçim ifadeleri şu anda kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="66faa-236">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="66faa-237">Yukarıdaki kodda `<input>` öğesinin alan türü (`type`) varsayılan olarak `text` ' dir.</span><span class="sxs-lookup"><span data-stu-id="66faa-237">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="66faa-238">`@bind:format`, aşağıdaki .NET türlerini bağlamak için desteklenir:</span><span class="sxs-lookup"><span data-stu-id="66faa-238">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="66faa-239"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="66faa-239"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="66faa-240"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="66faa-240"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="66faa-241">@No__t_0 özniteliği, `<input>` öğesinin `value` uygulanacak tarih biçimini belirtir.</span><span class="sxs-lookup"><span data-stu-id="66faa-241">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="66faa-242">Biçim, bir `onchange` olayı gerçekleştiğinde değeri ayrıştırmak için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-242">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="66faa-243">Blazor 'in tarihleri biçimlendirmek için yerleşik desteği olduğundan `date` alan türü için biçim belirtme önerilmez.</span><span class="sxs-lookup"><span data-stu-id="66faa-243">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="66faa-244">**Bileşen parametreleri**</span><span class="sxs-lookup"><span data-stu-id="66faa-244">**Component parameters**</span></span>

<span data-ttu-id="66faa-245">Bağlama, `@bind-{property}` ' ın bileşenler arasında bir özellik değeri bağlayabileceği bileşen parametrelerini tanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-245">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="66faa-246">Aşağıdaki alt bileşende (`ChildComponent`) bir `Year` bileşen parametresi ve `YearChanged` geri çağırması vardır:</span><span class="sxs-lookup"><span data-stu-id="66faa-246">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="66faa-247">`EventCallback<T>`, [Eventcallback](#eventcallback) bölümünde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="66faa-247">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="66faa-248">Aşağıdaki üst bileşen `ChildComponent` kullanır ve `ParentYear` parametresini alt bileşendeki `Year` parametresine bağlar:</span><span class="sxs-lookup"><span data-stu-id="66faa-248">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

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

<span data-ttu-id="66faa-249">@No__t_0 yüklemek aşağıdaki biçimlendirmeyi üretir:</span><span class="sxs-lookup"><span data-stu-id="66faa-249">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="66faa-250">@No__t_0 özelliğinin değeri `ParentComponent` düğme seçilerek değiştirilirse, `ChildComponent` `Year` özelliği güncellenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-250">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="66faa-251">@No__t_0 yeni değeri, `ParentComponent` yeniden eklendiğinde Kullanıcı arabiriminde işlenir:</span><span class="sxs-lookup"><span data-stu-id="66faa-251">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="66faa-252">@No__t_0 parametresi, `Year` parametresinin türüyle eşleşen bir yardımcı `YearChanged` olayına sahip olduğundan bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-252">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="66faa-253">Kurala göre `<ChildComponent @bind-Year="ParentYear" />` temelde yazmaya eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="66faa-253">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="66faa-254">Genel olarak, bir özellik `@bind-property:event` özniteliği kullanılarak karşılık gelen bir olay işleyicisine bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-254">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="66faa-255">Örneğin, `MyProp` özelliği aşağıdaki iki öznitelik kullanılarak `MyEventHandler` ' e bağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="66faa-255">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="66faa-256">Olay işleme</span><span class="sxs-lookup"><span data-stu-id="66faa-256">Event handling</span></span>

<span data-ttu-id="66faa-257">Razor bileşenleri olay işleme özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-257">Razor components provide event handling features.</span></span> <span data-ttu-id="66faa-258">Temsilci türü belirtilmiş bir değerle `on{event}` (örneğin, `onclick` ve `onsubmit`) adlı bir HTML öğesi özniteliği için, Razor bileşenleri özniteliğin değerini bir olay işleyicisi olarak değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="66faa-258">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="66faa-259">Özniteliğin adı her zaman [@on {Event}](xref:mvc/views/razor#onevent)olarak biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-259">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="66faa-260">Aşağıdaki kod, Kullanıcı arabiriminde düğme seçildiğinde `UpdateHeading` yöntemini çağırır:</span><span class="sxs-lookup"><span data-stu-id="66faa-260">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

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

<span data-ttu-id="66faa-261">Aşağıdaki kod, Kullanıcı arabiriminde onay kutusu değiştirildiğinde `CheckChanged` yöntemini çağırır:</span><span class="sxs-lookup"><span data-stu-id="66faa-261">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="66faa-262">Olay işleyicileri Ayrıca zaman uyumsuz olabilir ve bir <xref:System.Threading.Tasks.Task> döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-262">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="66faa-263">@No__t_0 el ile çağırmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="66faa-263">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="66faa-264">Özel durumlar oluştuğunda günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-264">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="66faa-265">Aşağıdaki örnekte, düğme seçildiğinde `UpdateHeading` zaman uyumsuz olarak çağrılır:</span><span class="sxs-lookup"><span data-stu-id="66faa-265">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

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

### <a name="event-argument-types"></a><span data-ttu-id="66faa-266">Olay bağımsız değişken türleri</span><span class="sxs-lookup"><span data-stu-id="66faa-266">Event argument types</span></span>

<span data-ttu-id="66faa-267">Bazı olaylar için olay bağımsız değişkeni türlerine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-267">For some events, event argument types are permitted.</span></span> <span data-ttu-id="66faa-268">Bu olay türlerinden birine erişim gerekmiyorsa, yöntem çağrısında gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="66faa-268">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="66faa-269">Desteklenen `EventArgs` aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="66faa-269">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="66faa-270">Olay</span><span class="sxs-lookup"><span data-stu-id="66faa-270">Event</span></span> | <span data-ttu-id="66faa-271">örneği</span><span class="sxs-lookup"><span data-stu-id="66faa-271">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="66faa-272">Pano</span><span class="sxs-lookup"><span data-stu-id="66faa-272">Clipboard</span></span>        | `ClipboardEventArgs` |
| <span data-ttu-id="66faa-273">Sürükleyin</span><span class="sxs-lookup"><span data-stu-id="66faa-273">Drag</span></span>             | <span data-ttu-id="66faa-274">`DragEventArgs` &ndash; `DataTransfer` ve `DataTransferItem` basılı tutup öğe verileri sürüklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="66faa-274">`DragEventArgs` &ndash; `DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="66faa-275">Hata</span><span class="sxs-lookup"><span data-stu-id="66faa-275">Error</span></span>            | `ErrorEventArgs` |
| <span data-ttu-id="66faa-276">Çı</span><span class="sxs-lookup"><span data-stu-id="66faa-276">Focus</span></span>            | <span data-ttu-id="66faa-277">`FocusEventArgs` &ndash; `relatedTarget` desteğini içermez.</span><span class="sxs-lookup"><span data-stu-id="66faa-277">`FocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="66faa-278">`<input>` değişiklik</span><span class="sxs-lookup"><span data-stu-id="66faa-278">`<input>` change</span></span> | `ChangeEventArgs` |
| <span data-ttu-id="66faa-279">Klavye</span><span class="sxs-lookup"><span data-stu-id="66faa-279">Keyboard</span></span>         | `KeyboardEventArgs` |
| <span data-ttu-id="66faa-280">Tığında</span><span class="sxs-lookup"><span data-stu-id="66faa-280">Mouse</span></span>            | `MouseEventArgs` |
| <span data-ttu-id="66faa-281">Fare işaretçisi</span><span class="sxs-lookup"><span data-stu-id="66faa-281">Mouse pointer</span></span>    | `PointerEventArgs` |
| <span data-ttu-id="66faa-282">Fare tekerleği</span><span class="sxs-lookup"><span data-stu-id="66faa-282">Mouse wheel</span></span>      | `WheelEventArgs` |
| <span data-ttu-id="66faa-283">İlerleme durumu</span><span class="sxs-lookup"><span data-stu-id="66faa-283">Progress</span></span>         | `ProgressEventArgs` |
| <span data-ttu-id="66faa-284">Dokunma</span><span class="sxs-lookup"><span data-stu-id="66faa-284">Touch</span></span>            | <span data-ttu-id="66faa-285">`TouchEventArgs` &ndash; `TouchPoint`, dokunmaya duyarlı bir cihazdaki tek bir iletişim noktasını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="66faa-285">`TouchEventArgs` &ndash; `TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="66faa-286">Önceki tablodaki olayların özellikleri ve olay işleme davranışı hakkında bilgi için bkz. [başvuru kaynağında EventArgs sınıfları (ASPNET/AspNetCore Release/3.0 dalı)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="66faa-286">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="66faa-287">Lambda ifadeleri</span><span class="sxs-lookup"><span data-stu-id="66faa-287">Lambda expressions</span></span>

<span data-ttu-id="66faa-288">Lambda ifadeleri de kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="66faa-288">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="66faa-289">Genellikle, bir dizi öğe üzerinde yineleme yaparken olduğu gibi ek değerlerin üzerinde kapatılabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-289">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="66faa-290">Aşağıdaki örnek, her biri Kullanıcı arabiriminde seçildiğinde `UpdateHeading` ' ı bir olay bağımsız değişkenini (`MouseEventArgs`) ve düğme numarasını (`buttonNumber`) geçen üç düğme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="66faa-290">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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
> <span data-ttu-id="66faa-291">Döngü değişkenini (`i`) doğrudan bir lambda ifadesinde bir `for` **döngüsünde kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="66faa-291">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="66faa-292">Aksi halde aynı değişken tüm lambda ifadeleri tarafından, `i` ' ın değerinin tüm Lambdalar ile aynı olmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="66faa-292">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="66faa-293">Her zaman değerini yerel bir değişkende yakala (önceki örnekte `buttonNumber`) ve ardından kullanın.</span><span class="sxs-lookup"><span data-stu-id="66faa-293">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="66faa-294">EventCallback</span><span class="sxs-lookup"><span data-stu-id="66faa-294">EventCallback</span></span>

<span data-ttu-id="66faa-295">İç içe bileşenler içeren yaygın bir senaryo, alt bileşen olayı gerçekleştiğinde bir üst bileşenin yöntemini çalıştırmak, örneğin `onclick` bir olay oluştuğunda &mdash;for.</span><span class="sxs-lookup"><span data-stu-id="66faa-295">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="66faa-296">Olayları bileşenler arasında ortaya çıkarmak için `EventCallback` kullanın.</span><span class="sxs-lookup"><span data-stu-id="66faa-296">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="66faa-297">Bir üst bileşen, bir alt bileşenin `EventCallback` ' a bir geri çağırma yöntemi atayabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-297">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="66faa-298">Örnek uygulamadaki `ChildComponent` ' ın, bir düğmenin `onclick` işleyicisinin, örneğin `ParentComponent` ' ten `EventCallback` temsilcisini almak üzere nasıl ayarlandığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="66faa-298">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="66faa-299">@No__t_0, bir çevresel cihazdan `onclick` olayına uygun `MouseEventArgs` ile yazılır:</span><span class="sxs-lookup"><span data-stu-id="66faa-299">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="66faa-300">@No__t_0, alt öğenin `EventCallback<T>` `ShowMessage` yöntemine ayarlar:</span><span class="sxs-lookup"><span data-stu-id="66faa-300">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="66faa-301">@No__t_0 düğme seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="66faa-301">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="66faa-302">@No__t_0 `ShowMessage` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-302">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="66faa-303">`messageText`, `ParentComponent` ' de güncellenir ve görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-303">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="66faa-304">Geri çağırma yönteminde `StateHasChanged` çağrısı gerekli değildir (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="66faa-304">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="66faa-305">`StateHasChanged` `ParentComponent` ' i yeniden çalıştırmak için otomatik olarak çağrılır. Örneğin, alt olaylar, alt öğe içinde yürütülen olay işleyicilerinde bileşen rerendering tetikler.</span><span class="sxs-lookup"><span data-stu-id="66faa-305">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="66faa-306">`EventCallback` ve `EventCallback<T>` zaman uyumsuz temsilcilere izin verir.</span><span class="sxs-lookup"><span data-stu-id="66faa-306">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="66faa-307">`EventCallback<T>` kesin bir şekilde türdedir ve belirli bir bağımsız değişken türü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="66faa-307">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="66faa-308">`EventCallback`, kesin olarak yazılmış ve herhangi bir bağımsız değişken türüne izin veriyor.</span><span class="sxs-lookup"><span data-stu-id="66faa-308">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="66faa-309">@No__t_2 ile bir `EventCallback` veya `EventCallback<T>` çağırın ve <xref:System.Threading.Tasks.Task> await:</span><span class="sxs-lookup"><span data-stu-id="66faa-309">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="66faa-310">Olay işleme ve bağlama bileşeni parametrelerini `EventCallback` ve `EventCallback<T>` kullanın.</span><span class="sxs-lookup"><span data-stu-id="66faa-310">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="66faa-311">@No__t_1 üzerinde türü kesin belirlenmiş `EventCallback<T>` tercih edin.</span><span class="sxs-lookup"><span data-stu-id="66faa-311">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="66faa-312">`EventCallback<T>`, bileşenin kullanıcılarına daha iyi hata geri bildirimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-312">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="66faa-313">Diğer UI olay işleyicileriyle benzer şekilde, olay parametresini belirtmek isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="66faa-313">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="66faa-314">Geri çağırmaya hiçbir değer geçirilmemişse `EventCallback` kullanın.</span><span class="sxs-lookup"><span data-stu-id="66faa-314">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="chained-bind"></a><span data-ttu-id="66faa-315">Zincirleme bağlama</span><span class="sxs-lookup"><span data-stu-id="66faa-315">Chained bind</span></span>

<span data-ttu-id="66faa-316">Yaygın bir senaryo, bir veri bağlama parametresini bileşen çıkışında bir sayfa öğesine zincirlemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-316">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="66faa-317">Birden çok bağlama düzeyi aynı anda gerçekleştiğinden, bu senaryoya *zincirleme bağlama* denir.</span><span class="sxs-lookup"><span data-stu-id="66faa-317">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="66faa-318">Bir zincir bağlama, sayfanın öğesinde `@bind` sözdizimi ile uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="66faa-318">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="66faa-319">Olay işleyicisi ve değeri ayrı olarak belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="66faa-319">The event handler and value must be specified separately.</span></span> <span data-ttu-id="66faa-320">Ancak bir üst bileşen, bileşen parametresiyle `@bind` sözdizimini kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-320">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="66faa-321">Aşağıdaki `PasswordField` bileşeni (*Passwordfield. Razor*):</span><span class="sxs-lookup"><span data-stu-id="66faa-321">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="66faa-322">Bir `<input>` öğesinin değerini bir `Password` özelliğine ayarlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-322">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="66faa-323">@No__t_0 özelliğindeki değişiklikleri, bir [Eventcallback](#eventcallback)ile bir üst bileşene gösterir.</span><span class="sxs-lookup"><span data-stu-id="66faa-323">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

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

<span data-ttu-id="66faa-324">@No__t_0 bileşeni başka bir bileşende kullanılır:</span><span class="sxs-lookup"><span data-stu-id="66faa-324">The `PasswordField` component is used in another component:</span></span>

```cshtml
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

<span data-ttu-id="66faa-325">Önceki örnekteki parolada denetim veya tuzak hataları gerçekleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="66faa-325">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="66faa-326">@No__t_0 için bir yedekleme alanı oluşturun (Aşağıdaki örnek kodda `password`).</span><span class="sxs-lookup"><span data-stu-id="66faa-326">Create a backing field for `Password` (`password` in the following example code).</span></span>
* <span data-ttu-id="66faa-327">@No__t_0 ayarlayıcısı 'nda denetimleri veya yakalama hatalarını gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="66faa-327">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="66faa-328">Aşağıdaki örnek, parolanın değerinde bir boşluk kullanılmışsa kullanıcıya anında geri bildirim sağlar:</span><span class="sxs-lookup"><span data-stu-id="66faa-328">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

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

## <a name="capture-references-to-components"></a><span data-ttu-id="66faa-329">Bileşenlere başvuruları yakala</span><span class="sxs-lookup"><span data-stu-id="66faa-329">Capture references to components</span></span>

<span data-ttu-id="66faa-330">Bileşen başvuruları bir bileşen örneğine başvurmak için bir yol sağlar, böylece bu örneğe `Show` veya `Reset` gibi komutlar verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66faa-330">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="66faa-331">Bir bileşen başvurusunu yakalamak için:</span><span class="sxs-lookup"><span data-stu-id="66faa-331">To capture a component reference:</span></span>

* <span data-ttu-id="66faa-332">Alt bileşene bir [@ref](xref:mvc/views/razor#ref) özniteliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="66faa-332">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="66faa-333">Alt bileşenle aynı türde bir alan tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="66faa-333">Define a field with the same type as the child component.</span></span>

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

<span data-ttu-id="66faa-334">Bileşen işlendiğinde `loginDialog` alanı `MyLoginDialog` alt bileşen örneğiyle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="66faa-334">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="66faa-335">Daha sonra bileşen örneğinde .NET yöntemlerini çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66faa-335">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="66faa-336">@No__t_0 değişkeni yalnızca bileşen işlendikten sonra ve çıktısı `MyLoginDialog` öğesini içerdiğinde doldurulur.</span><span class="sxs-lookup"><span data-stu-id="66faa-336">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="66faa-337">Bu noktaya kadar başvurulmasına hiçbir şey yok.</span><span class="sxs-lookup"><span data-stu-id="66faa-337">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="66faa-338">Bileşen işlemesini tamamladıktan sonra bileşen başvurularını işlemek için [Onafterrenderasync veya OnAfterRender yöntemlerini](#lifecycle-methods)kullanın.</span><span class="sxs-lookup"><span data-stu-id="66faa-338">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](#lifecycle-methods).</span></span>

<span data-ttu-id="66faa-339">Bileşen başvurularını yakalama, [öğe başvurularını yakalamak](xref:blazor/javascript-interop#capture-references-to-elements)için benzer bir sözdizimi kullanın, bir [JavaScript birlikte çalışma](xref:blazor/javascript-interop) özelliği değildir.</span><span class="sxs-lookup"><span data-stu-id="66faa-339">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="66faa-340">Bileşen başvuruları JavaScript koduna aktarılmaz &mdash;they yalnızca .NET kodunda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-340">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="66faa-341">Alt bileşenlerin durumunu bulunmamalıdır için bileşen **başvurularını kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="66faa-341">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="66faa-342">Bunun yerine, alt bileşenlere veri geçirmek için normal bildirime dayalı parametreleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="66faa-342">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="66faa-343">Normal bildirime dayalı parametrelerin kullanımı, otomatik olarak doğru zamanların yeniden yönlendirmesi için alt bileşenlerde oluşur.</span><span class="sxs-lookup"><span data-stu-id="66faa-343">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="66faa-344">Durumu güncelleştirmek için bileşen yöntemlerini dışarıdan çağır</span><span class="sxs-lookup"><span data-stu-id="66faa-344">Invoke component methods externally to update state</span></span>

<span data-ttu-id="66faa-345">Blazor, yürütmenin tek bir mantıksal iş parçacığını zorlamak için `SynchronizationContext` kullanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-345">Blazor uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="66faa-346">Bir bileşenin yaşam döngüsü yöntemleri ve Blazor tarafından oluşturulan tüm olay geri çağırmaları bu `SynchronizationContext` yürütülür.</span><span class="sxs-lookup"><span data-stu-id="66faa-346">A component's lifecycle methods and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="66faa-347">Bir bileşenin bir zamanlayıcı veya diğer bildirimler gibi dış bir olaya göre güncellenmesi gerekir, `SynchronizationContext` Blazor ' ye gönderilecek `InvokeAsync` yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="66faa-347">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="66faa-348">Örneğin, güncelleştirilmiş durumdaki herhangi bir dinleme bileşenine bildirimde bulunan bir *bildirim hizmeti* düşünün:</span><span class="sxs-lookup"><span data-stu-id="66faa-348">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="66faa-349">Bir bileşeni güncelleştirmek için `NotifierService` kullanımı:</span><span class="sxs-lookup"><span data-stu-id="66faa-349">Usage of the `NotifierService` to update a component:</span></span>

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

<span data-ttu-id="66faa-350">Yukarıdaki örnekte, `NotifierService` bileşenin `OnNotify` yöntemini Blazor `SynchronizationContext` dışında çağırır.</span><span class="sxs-lookup"><span data-stu-id="66faa-350">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="66faa-351">`InvokeAsync`, doğru bağlama geçmek ve bir işlemeyi kuyruğa almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-351">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="66faa-352">Öğelerin ve bileşenlerin korunmasını denetlemek için \@key kullanın</span><span class="sxs-lookup"><span data-stu-id="66faa-352">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="66faa-353">Bir öğe veya bileşen listesi işlenirken ve öğeler ya da bileşenler daha sonra değiştiğinde, Blazor 'in yayılma algoritması, önceki öğelerin veya bileşenlerin ne zaman tutulacağına ve model nesnelerinin bunlara nasıl eşleneceğine karar vermelidir.</span><span class="sxs-lookup"><span data-stu-id="66faa-353">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="66faa-354">Normalde, bu işlem otomatiktir ve yoksayılabilir, ancak işlemi denetlemek isteyebileceğiniz durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="66faa-354">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="66faa-355">Aşağıdaki örneği göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="66faa-355">Consider the following example:</span></span>

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

<span data-ttu-id="66faa-356">@No__t_0 koleksiyonun içerikleri, ekli, silinmiş veya yeniden sıralanmış girdilerle değişebilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-356">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="66faa-357">Bileşen yeniden oluşturulduğunda, `<DetailsEditor>` bileşeni farklı `Details` parametre değerleri almak için değişebilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-357">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="66faa-358">Bu, beklenenden daha karmaşık rerendering oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-358">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="66faa-359">Bazı durumlarda rerendering, kayıp öğe odağı gibi görünür davranış farklılıklarına yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-359">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="66faa-360">Eşleme işlemi `@key` yönergesi özniteliğiyle denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-360">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="66faa-361">`@key`, anahtar değerine göre öğelerin veya bileşenlerin korunmasını güvence altına almak için dağıtılmış algoritmaya neden olur:</span><span class="sxs-lookup"><span data-stu-id="66faa-361">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="66faa-362">@No__t_0 koleksiyonu değiştiğinde, yayılma algoritması `<DetailsEditor>` örnekleri ve `person` örnekleri arasındaki ilişkilendirmeyi korur:</span><span class="sxs-lookup"><span data-stu-id="66faa-362">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="66faa-363">Bir `Person` `People` listesinden silinirse, yalnızca ilgili `<DetailsEditor>` örneği kullanıcı arabiriminden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-363">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="66faa-364">Diğer örnekler değişmeden bırakılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-364">Other instances are left unchanged.</span></span>
* <span data-ttu-id="66faa-365">Listedeki bir konuma `Person` eklenirse, ilgili konuma bir yeni `<DetailsEditor>` örneği eklenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-365">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="66faa-366">Diğer örnekler değişmeden bırakılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-366">Other instances are left unchanged.</span></span>
* <span data-ttu-id="66faa-367">@No__t_0 girdileri yeniden sıralandıysanız, karşılık gelen `<DetailsEditor>` örnekleri korunur ve Kullanıcı arabiriminde yeniden sıralanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-367">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="66faa-368">Bazı senaryolarda `@key` kullanımı, rerendering karmaşıklığını en aza indirir ve odak konumu gibi DOM 'ın durum bilgisi olan kısımlarıyla ilgili olası sorunları önler.</span><span class="sxs-lookup"><span data-stu-id="66faa-368">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="66faa-369">Anahtarlar her kapsayıcı öğesi veya bileşeni için yereldir.</span><span class="sxs-lookup"><span data-stu-id="66faa-369">Keys are local to each container element or component.</span></span> <span data-ttu-id="66faa-370">Anahtarlar belge genelinde küresel olarak karşılaştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="66faa-370">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="66faa-371">Ne zaman kullanılacağı \@key</span><span class="sxs-lookup"><span data-stu-id="66faa-371">When to use \@key</span></span>

<span data-ttu-id="66faa-372">Genellikle, bir liste işlendiğinde (örneğin, bir `@foreach` bloğunda) ve `@key` tanımlamak için uygun bir değer olduğunda `@key` ' ı kullanmak mantıklı olur.</span><span class="sxs-lookup"><span data-stu-id="66faa-372">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="66faa-373">Bir nesne değiştiğinde Blazor 'in bir öğeyi veya bileşen alt ağacını engellemesini engellemek için `@key` ' yı da kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="66faa-373">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="66faa-374">@No__t_0 değişirse `@key` Attribute yönergesi, tüm `<div>` ve alt öğelerini atmayı ve yeni öğeler ve bileşenlerle Kullanıcı arabiriminde alt ağacı yeniden oluşturmayı zorlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-374">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="66faa-375">Bu, `@currentPerson` değiştiğinde hiçbir Kullanıcı arabirimi durumunun korunmayacağını garanti etmeniz gerektiğinde yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-375">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="66faa-376">Ne zaman kullanılmaz \@key</span><span class="sxs-lookup"><span data-stu-id="66faa-376">When not to use \@key</span></span>

<span data-ttu-id="66faa-377">@No__t_0 bir performans maliyeti vardır.</span><span class="sxs-lookup"><span data-stu-id="66faa-377">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="66faa-378">Performans maliyeti büyük değildir, ancak öğe veya bileşen koruma kuralları denetlenmediğinde yalnızca `@key` belirtin.</span><span class="sxs-lookup"><span data-stu-id="66faa-378">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="66faa-379">@No__t_0 kullanılmasa bile, Blazor alt öğe ve bileşen örneklerini mümkün olduğunca korur.</span><span class="sxs-lookup"><span data-stu-id="66faa-379">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="66faa-380">@No__t_0 kullanmanın avantajı, model örneklerinin eşlemeyi seçme algoritması yerine, korunan bileşen örneklerine *nasıl* eşlendiğine ilişkin denetimdir.</span><span class="sxs-lookup"><span data-stu-id="66faa-380">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="66faa-381">@No__t_0key için kullanılacak değerler</span><span class="sxs-lookup"><span data-stu-id="66faa-381">What values to use for \@key</span></span>

<span data-ttu-id="66faa-382">Genellikle, `@key` için aşağıdaki değer türlerinden birini sağlamak mantıklı olur:</span><span class="sxs-lookup"><span data-stu-id="66faa-382">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="66faa-383">Model nesne örnekleri (örneğin, önceki örnekte olduğu gibi `Person` örneği).</span><span class="sxs-lookup"><span data-stu-id="66faa-383">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="66faa-384">Bu, nesne başvurusu eşitliğine göre koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-384">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="66faa-385">Benzersiz tanımlayıcılar (örneğin, `int`, `string` veya `Guid`) türündeki birincil anahtar değerleri.</span><span class="sxs-lookup"><span data-stu-id="66faa-385">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="66faa-386">@No__t_0 için kullanılan değerlerin çakışmayın olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="66faa-386">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="66faa-387">Aynı üst öğe içinde çakışan değerler algılanırsa, eski öğeleri veya bileşenleri yeni öğe veya bileşenlere kesin bir şekilde eşlemediğinden Blazor bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="66faa-387">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="66faa-388">Yalnızca nesne örnekleri veya birincil anahtar değerleri gibi farklı değerleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="66faa-388">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="66faa-389">Yaşam döngüsü yöntemleri</span><span class="sxs-lookup"><span data-stu-id="66faa-389">Lifecycle methods</span></span>

<span data-ttu-id="66faa-390">`OnInitializedAsync` ve `OnInitialized` bileşeni başlatmak için kodu yürütün.</span><span class="sxs-lookup"><span data-stu-id="66faa-390">`OnInitializedAsync` and `OnInitialized` execute code to initialize the component.</span></span> <span data-ttu-id="66faa-391">Zaman uyumsuz bir işlem gerçekleştirmek için işlem üzerinde `OnInitializedAsync` ve `await` anahtar sözcüğünü kullanın:</span><span class="sxs-lookup"><span data-stu-id="66faa-391">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="66faa-392">Bileşen başlatma sırasında zaman uyumsuz çalışma `OnInitializedAsync` yaşam döngüsü olayı sırasında gerçekleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="66faa-392">Asynchronous work during component initialization must occur during the `OnInitializedAsync` lifecycle event.</span></span>

<span data-ttu-id="66faa-393">Zaman uyumlu bir işlem için `OnInitialized` kullanın:</span><span class="sxs-lookup"><span data-stu-id="66faa-393">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="66faa-394">`OnParametersSetAsync` ve `OnParametersSet`, bir bileşen üst öğeden parametreleri aldığında ve değerler özelliklere atandığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-394">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="66faa-395">Bu yöntemler bileşen başlatıldıktan sonra ve bileşen her işlendiğinde yürütülür:</span><span class="sxs-lookup"><span data-stu-id="66faa-395">These methods are executed after component initialization and each time the component is rendered:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="66faa-396">Parametre ve özellik değerleri uygulanırken zaman uyumsuz iş `OnParametersSetAsync` yaşam döngüsü olayı sırasında gerçekleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="66faa-396">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="66faa-397">`OnAfterRenderAsync` ve `OnAfterRender` bir bileşen işlemeyi tamamladıktan sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-397">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="66faa-398">Öğe ve bileşen başvuruları bu noktada doldurulur.</span><span class="sxs-lookup"><span data-stu-id="66faa-398">Element and component references are populated at this point.</span></span> <span data-ttu-id="66faa-399">İşlenmiş DOM öğelerinde çalışan üçüncü taraf JavaScript kitaplıklarını etkinleştirme gibi, işlenmiş içeriği kullanarak ek başlatma adımları gerçekleştirmek için bu aşamayı kullanın.</span><span class="sxs-lookup"><span data-stu-id="66faa-399">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="66faa-400">`OnAfterRender` *sunucuda prerendering çağrıldığında çağrılmaz.*</span><span class="sxs-lookup"><span data-stu-id="66faa-400">`OnAfterRender` *isn't called when prerendering on the server.*</span></span>

<span data-ttu-id="66faa-401">@No__t_1 ve `OnAfterRender` için `firstRender` parametresi:</span><span class="sxs-lookup"><span data-stu-id="66faa-401">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender` is:</span></span>

* <span data-ttu-id="66faa-402">Bileşen örneği ilk kez çağrıldığında `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="66faa-402">Set to `true` the first time that the component instance is invoked.</span></span>
* <span data-ttu-id="66faa-403">Başlatma işinin yalnızca bir kez gerçekleştirildiğinden emin olur.</span><span class="sxs-lookup"><span data-stu-id="66faa-403">Ensures that initialization work is only performed once.</span></span>

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
> <span data-ttu-id="66faa-404">@No__t_0 yaşam döngüsü olayında, işleme hemen sonra zaman uyumsuz çalışma gerçekleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="66faa-404">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="66faa-405">İşleme sırasında tamamlanmamış zaman uyumsuz eylemleri işle</span><span class="sxs-lookup"><span data-stu-id="66faa-405">Handle incomplete async actions at render</span></span>

<span data-ttu-id="66faa-406">Yaşam döngüsü olaylarında gerçekleştirilen zaman uyumsuz eylemler, bileşen işlenmeden önce tamamlanmamış olabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-406">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="66faa-407">Yaşam döngüsü yöntemi yürütülürken nesneler `null` veya verilerle tamamen doldurulmuş olabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-407">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="66faa-408">Nesnelerin başlatıldığını onaylamak için işleme mantığı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="66faa-408">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="66faa-409">Nesneler `null` olduğunda yer tutucu Kullanıcı arabirimi öğelerini (örneğin, bir yükleme iletisi) işleme.</span><span class="sxs-lookup"><span data-stu-id="66faa-409">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="66faa-410">Blazor şablonlarının `FetchData` bileşeninde, `OnInitializedAsync`, Asychronously tahmin verileri al (`forecasts`) için geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="66faa-410">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="66faa-411">@No__t_0 `null` olduğunda, kullanıcıya bir yükleme iletisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-411">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="66faa-412">@No__t_1 tamamlandığında döndürülen `Task`, bileşen güncelleştirilmiş durumla yeniden yapılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-412">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="66faa-413">*Pages/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="66faa-413">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="66faa-414">Parametreler ayarlanmadan önce kodu yürütün</span><span class="sxs-lookup"><span data-stu-id="66faa-414">Execute code before parameters are set</span></span>

<span data-ttu-id="66faa-415">Parametreler ayarlanmadan önce kodu yürütmek için `SetParameters` geçersiz kılınabilir:</span><span class="sxs-lookup"><span data-stu-id="66faa-415">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="66faa-416">@No__t_0 çağrılmazsa, özel kod gelen parametreler değerini gerekli herhangi bir şekilde yorumlayabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-416">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="66faa-417">Örneğin, gelen parametrelerin, sınıftaki özelliklere atanması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="66faa-417">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="66faa-418">Kullanıcı arabiriminin yenilenmesini gösterme</span><span class="sxs-lookup"><span data-stu-id="66faa-418">Suppress refreshing of the UI</span></span>

<span data-ttu-id="66faa-419">`ShouldRender`, Kullanıcı arabiriminin yenilenmesini gizlemek için geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-419">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="66faa-420">Uygulama `true` döndürürse, Kullanıcı arabirimi yenilenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-420">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="66faa-421">@No__t_0 geçersiz kılınsa bile, bileşen her zaman ilk olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-421">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="66faa-422">IDisposable ile bileşen atma</span><span class="sxs-lookup"><span data-stu-id="66faa-422">Component disposal with IDisposable</span></span>

<span data-ttu-id="66faa-423">Bir bileşen <xref:System.IDisposable> ' ı uygularsa, bileşen kullanıcı arabiriminden kaldırıldığında [Dispose yöntemi](/dotnet/standard/garbage-collection/implementing-dispose) çağrılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-423">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="66faa-424">Aşağıdaki bileşen `@implements IDisposable` ve `Dispose` yöntemini kullanır:</span><span class="sxs-lookup"><span data-stu-id="66faa-424">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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

> [!NOTE]
> <span data-ttu-id="66faa-425">@No__t_1 `StateHasChanged` çağrısı desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="66faa-425">Calling `StateHasChanged` in `Dispose` isn't supported.</span></span> <span data-ttu-id="66faa-426">`StateHasChanged`, yırtılmış oluşturucunun parçası olarak çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-426">`StateHasChanged` might be invoked as part of the renderer being torn down.</span></span> <span data-ttu-id="66faa-427">Bu noktada UI güncelleştirmelerini isteme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="66faa-427">Requesting UI updates at that point isn't supported.</span></span>

## <a name="routing"></a><span data-ttu-id="66faa-428">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="66faa-428">Routing</span></span>

<span data-ttu-id="66faa-429">Blazor ' de yönlendirme, uygulamadaki her erişilebilir bileşene bir rota şablonu sunarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-429">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="66faa-430">@No__t_0 yönergesine sahip bir Razor dosyası derlendiğinde, oluşturulan sınıfa yol şablonunu belirten bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> verilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-430">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="66faa-431">Çalışma zamanında, yönlendirici `RouteAttribute` ile bileşen sınıflarına bakar ve hangi bileşenin istenen URL ile eşleşen bir rota şablonuna sahip olduğunu işler.</span><span class="sxs-lookup"><span data-stu-id="66faa-431">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="66faa-432">Birden çok yol şablonu, bir bileşene uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-432">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="66faa-433">Aşağıdaki bileşen `/BlazorRoute` ve `/DifferentBlazorRoute` isteklerine yanıt verir:</span><span class="sxs-lookup"><span data-stu-id="66faa-433">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="66faa-434">Rota parametreleri</span><span class="sxs-lookup"><span data-stu-id="66faa-434">Route parameters</span></span>

<span data-ttu-id="66faa-435">Bileşenler, `@page` yönergesinde belirtilen yol şablonundan rota parametreleri alabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-435">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="66faa-436">Yönlendirici, karşılık gelen bileşen parametrelerini doldurmak için yol parametrelerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-436">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="66faa-437">*Rota parametresi bileşeni*:</span><span class="sxs-lookup"><span data-stu-id="66faa-437">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="66faa-438">İsteğe bağlı parametreler desteklenmez, bu nedenle yukarıdaki örnekte iki `@page` yönergesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-438">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="66faa-439">İlki, bir parametre olmadan bileşene gezinmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="66faa-439">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="66faa-440">İkinci `@page` yönergesi `{text}` yol parametresini alır ve değeri `Text` özelliğine atar.</span><span class="sxs-lookup"><span data-stu-id="66faa-440">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

::: moniker range=">= aspnetcore-3.1"

## <a name="partial-class-support"></a><span data-ttu-id="66faa-441">Kısmi sınıf desteği</span><span class="sxs-lookup"><span data-stu-id="66faa-441">Partial class support</span></span>

<span data-ttu-id="66faa-442">Razor bileşenleri kısmi sınıflar olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="66faa-442">Razor components are generated as partial classes.</span></span> <span data-ttu-id="66faa-443">Razor bileşenleri aşağıdaki yaklaşımlardan birini kullanarak yazılır:</span><span class="sxs-lookup"><span data-stu-id="66faa-443">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="66faa-444">C#kod, tek bir dosyada HTML işaretlemesi ve Razor kodu ile bir [@code](xref:mvc/views/razor#code) bloğunda tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="66faa-444">C# code is defined in an [@code](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> <span data-ttu-id="66faa-445">Blazor şablonları bu yaklaşımı kullanarak Razor bileşenlerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-445">Blazor templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="66faa-446">C#kod, kısmi sınıf olarak tanımlanan bir arka plan kod dosyasına yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-446">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="66faa-447">Aşağıdaki örnek, Blazor şablonundan oluşturulan bir uygulamada bir `@code` bloğu olan varsayılan `Counter` bileşenini gösterir.</span><span class="sxs-lookup"><span data-stu-id="66faa-447">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="66faa-448">HTML işaretleme, Razor kodu ve C# kod aynı dosyada:</span><span class="sxs-lookup"><span data-stu-id="66faa-448">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="66faa-449">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="66faa-449">*Counter.razor*:</span></span>

```cshtml
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    int currentCount = 0;

    void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="66faa-450">@No__t_0 bileşeni, kısmi bir sınıf içeren bir arka plan kod dosyası kullanılarak da oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="66faa-450">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="66faa-451">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="66faa-451">*Counter.razor*:</span></span>

```cshtml
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="66faa-452">*Counter.Razor.cs*:</span><span class="sxs-lookup"><span data-stu-id="66faa-452">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

## <a name="specify-a-component-base-class"></a><span data-ttu-id="66faa-453">Bir bileşen taban sınıfı belirtin</span><span class="sxs-lookup"><span data-stu-id="66faa-453">Specify a component base class</span></span>

<span data-ttu-id="66faa-454">@No__t_0 yönergesi, bir bileşen için temel sınıf belirtmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-454">The `@inherits` directive can be used to specify a base class for a component.</span></span>

<span data-ttu-id="66faa-455">[Örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) , bileşenin özelliklerini ve yöntemlerini sağlamak için bir bileşenin `BlazorRocksBase` temel sınıfını nasıl devralmasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="66faa-455">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="66faa-456">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="66faa-456">*Pages/BlazorRocks.razor*:</span></span>

```cshtml
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="66faa-457">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="66faa-457">*BlazorRocksBase.cs*:</span></span>

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

<span data-ttu-id="66faa-458">Temel sınıf `ComponentBase` ' dan türetilmelidir.</span><span class="sxs-lookup"><span data-stu-id="66faa-458">The base class should derive from `ComponentBase`.</span></span>

::: moniker-end

## <a name="import-components"></a><span data-ttu-id="66faa-459">Bileşenleri içeri aktar</span><span class="sxs-lookup"><span data-stu-id="66faa-459">Import components</span></span>

<span data-ttu-id="66faa-460">Razor ile yazılan bir bileşenin ad alanı temel alınarak belirlenir (öncelik sırasına göre):</span><span class="sxs-lookup"><span data-stu-id="66faa-460">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="66faa-461">Razor dosyası ( *. Razor*) biçimlendirmesinde [@namespace](xref:mvc/views/razor#namespace) atama (`@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="66faa-461">[@namespace](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="66faa-462">Projenin proje dosyasında `RootNamespace` (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="66faa-462">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="66faa-463">Proje dosyasının dosya adından ( *. csproj*) ve proje kökünden bileşen yolundan alınan proje adı.</span><span class="sxs-lookup"><span data-stu-id="66faa-463">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="66faa-464">Örneğin Framework, *{Project root}/Pages/Index.Razor* (*BlazorSample. csproj*) ad alanına `BlazorSample.Pages` ' yi çözer.</span><span class="sxs-lookup"><span data-stu-id="66faa-464">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="66faa-465">Bileşenler ad C# bağlama kurallarını izler.</span><span class="sxs-lookup"><span data-stu-id="66faa-465">Components follow C# name binding rules.</span></span> <span data-ttu-id="66faa-466">Bu örnekteki `Index` bileşeni için, kapsamdaki bileşenler tüm bileşenlerdir:</span><span class="sxs-lookup"><span data-stu-id="66faa-466">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="66faa-467">Aynı klasörde, *sayfalarda*.</span><span class="sxs-lookup"><span data-stu-id="66faa-467">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="66faa-468">Proje kökündeki, açıkça farklı bir ad alanı belirtmeyen bileşenler.</span><span class="sxs-lookup"><span data-stu-id="66faa-468">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="66faa-469">Farklı bir ad alanında tanımlanan bileşenler, Razor 'nin [@using](xref:mvc/views/razor#using) yönergesi kullanılarak kapsam içine getirilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-469">Components defined in a different namespace are brought into scope using Razor's [@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="66faa-470">*BlazorSample/Shared/* klasöründe `NavMenu.razor` başka bir bileşen varsa, bileşen aşağıdaki `@using` ifadesiyle `Index.razor` kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="66faa-470">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="66faa-471">Bileşenlere, [@using](xref:mvc/views/razor#using) yönergesini gerektirmeyen kendi tam adları kullanılarak da başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="66faa-471">Components can also be referenced using their fully qualified names, which doesn't require the [@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="66faa-472">@No__t_0 niteleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="66faa-472">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="66faa-473">Diğer ad `using` deyimleriyle bileşenleri içeri aktarma (örneğin, `@using Foo = Bar`) desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="66faa-473">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="66faa-474">Kısmen nitelenmiş adlar desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="66faa-474">Partially qualified names aren't supported.</span></span> <span data-ttu-id="66faa-475">Örneğin, `@using BlazorSample` ' ı ekleme ve `<Shared.NavMenu></Shared.NavMenu>` ile `NavMenu.razor` ' i eklemek desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="66faa-475">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="66faa-476">Koşullu HTML öğesi öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="66faa-476">Conditional HTML element attributes</span></span>

<span data-ttu-id="66faa-477">HTML öğesi öznitelikleri, .NET değerine göre koşullu olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-477">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="66faa-478">Değer `false` veya `null` ise, öznitelik işlenmez.</span><span class="sxs-lookup"><span data-stu-id="66faa-478">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="66faa-479">Değer `true` ise, öznitelik küçültülmüş olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-479">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="66faa-480">Aşağıdaki örnekte `IsCompleted` öğesi öğenin biçimlendirmesinde `checked` ' in işlenip işlenmeyeceğini belirler:</span><span class="sxs-lookup"><span data-stu-id="66faa-480">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="66faa-481">@No__t_0 `true`, onay kutusu şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="66faa-481">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="66faa-482">@No__t_0 `false`, onay kutusu şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="66faa-482">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="66faa-483">Daha fazla bilgi için bkz. <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="66faa-483">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="66faa-484">.NET türü `bool` olduğunda, [Aria-basılan](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons)gıbı bazı HTML öznitelikleri düzgün şekilde çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="66faa-484">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="66faa-485">Bu durumlarda, `bool` yerine `string` türü kullanın.</span><span class="sxs-lookup"><span data-stu-id="66faa-485">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="66faa-486">Ham HTML</span><span class="sxs-lookup"><span data-stu-id="66faa-486">Raw HTML</span></span>

<span data-ttu-id="66faa-487">Dizeler normalde DOM metin düğümleri kullanılarak işlenir. Bu, içerdikleri tüm biçimlendirmenin yok sayıldığı ve değişmez değer olarak kabul edildiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="66faa-487">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="66faa-488">Ham HTML işlemek için, HTML içeriğini `MarkupString` değerinde sarın.</span><span class="sxs-lookup"><span data-stu-id="66faa-488">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="66faa-489">Değer HTML veya SVG olarak ayrıştırılır ve DOM 'a eklenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-489">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="66faa-490">Güvenilmeyen bir kaynaktan oluşturulan ham HTML işleme bir **güvenlik riskidir** ve kaçınılması gerekir!</span><span class="sxs-lookup"><span data-stu-id="66faa-490">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="66faa-491">Aşağıdaki örnek, bir bileşenin işlenmiş çıktısına statik HTML içeriği bloğunu eklemek için `MarkupString` türünü kullanmayı gösterir:</span><span class="sxs-lookup"><span data-stu-id="66faa-491">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="66faa-492">Şablonlu bileşenler</span><span class="sxs-lookup"><span data-stu-id="66faa-492">Templated components</span></span>

<span data-ttu-id="66faa-493">Şablonlu bileşenler, bir veya daha fazla UI şablonunu parametre olarak kabul eden bileşenlerdir, daha sonra bileşen işleme mantığının bir parçası olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-493">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="66faa-494">Şablonlu bileşenler, normal bileşenlerden daha yeniden kullanılabilir olan üst düzey bileşenleri yazmanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="66faa-494">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="66faa-495">Birkaç örnek şunlardır:</span><span class="sxs-lookup"><span data-stu-id="66faa-495">A couple of examples include:</span></span>

* <span data-ttu-id="66faa-496">Kullanıcının tablo üst bilgisi, satırları ve altbilgisi için şablon belirtmesini sağlayan tablo bileşeni.</span><span class="sxs-lookup"><span data-stu-id="66faa-496">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="66faa-497">Bir kullanıcının bir listedeki öğeleri işlemek için şablon belirlemesine izin veren bir liste bileşenidir.</span><span class="sxs-lookup"><span data-stu-id="66faa-497">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="66faa-498">Şablon parametreleri</span><span class="sxs-lookup"><span data-stu-id="66faa-498">Template parameters</span></span>

<span data-ttu-id="66faa-499">Şablonlu bir bileşen, `RenderFragment` veya `RenderFragment<T>` türünde bir veya daha fazla bileşen parametresi belirtilerek tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-499">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="66faa-500">Bir işleme parçası, işlenecek Kullanıcı arabiriminin bir kesimini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="66faa-500">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="66faa-501">`RenderFragment<T>`, işleme parçası çağrıldığında belirtilebildiği bir tür parametresi alır.</span><span class="sxs-lookup"><span data-stu-id="66faa-501">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="66faa-502">`TableTemplate` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="66faa-502">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="66faa-503">Şablonlu bir bileşen kullanırken, şablon parametreleri parametre adlarıyla eşleşen alt öğeler kullanılarak belirtilebilir (`TableHeader` ve `RowTemplate` aşağıdaki örnekte):</span><span class="sxs-lookup"><span data-stu-id="66faa-503">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="66faa-504">Şablon bağlam parametreleri</span><span class="sxs-lookup"><span data-stu-id="66faa-504">Template context parameters</span></span>

<span data-ttu-id="66faa-505">Öğe olarak geçirilen `RenderFragment<T>` türündeki bileşen bağımsız değişkenleri, `context` adlı bir örtük parametreye sahiptir (örneğin, yukarıdaki kod örneğinden, `@context.PetId`), ancak parametre adını alt öğedeki `Context` özniteliğini kullanarak değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66faa-505">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="66faa-506">Aşağıdaki örnekte, `RowTemplate` öğesinin `Context` özniteliği `pet` parametresini belirtir:</span><span class="sxs-lookup"><span data-stu-id="66faa-506">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="66faa-507">Alternatif olarak, bileşen öğesi üzerinde `Context` özniteliğini de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66faa-507">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="66faa-508">Belirtilen `Context` özniteliği belirtilen tüm şablon parametreleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="66faa-508">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="66faa-509">Bu, örtük alt içerik (herhangi bir sarmalama alt öğesi olmadan) için içerik parametre adını belirtmek istediğinizde yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-509">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="66faa-510">Aşağıdaki örnekte, `Context` özniteliği `TableTemplate` öğesinde görünür ve tüm şablon parametreleri için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="66faa-510">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="66faa-511">Genel olarak yazılmış bileşenler</span><span class="sxs-lookup"><span data-stu-id="66faa-511">Generic-typed components</span></span>

<span data-ttu-id="66faa-512">Şablonlu bileşenler çoğunlukla genel olarak türdedir.</span><span class="sxs-lookup"><span data-stu-id="66faa-512">Templated components are often generically typed.</span></span> <span data-ttu-id="66faa-513">Örneğin, `IEnumerable<T>` değerlerini işlemek için genel `ListViewTemplate` bir bileşen kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-513">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="66faa-514">Genel bir bileşen tanımlamak için [@typeparam](xref:mvc/views/razor#typeparam) yönergesini kullanarak tür parametrelerini belirtin:</span><span class="sxs-lookup"><span data-stu-id="66faa-514">To define a generic component, use the [@typeparam](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="66faa-515">Genel türsüz bileşenleri kullanırken tür parametresi mümkünse algılanır:</span><span class="sxs-lookup"><span data-stu-id="66faa-515">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="66faa-516">Aksi halde tür parametresi, tür parametresinin adıyla eşleşen bir öznitelik kullanılarak açıkça belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="66faa-516">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="66faa-517">Aşağıdaki örnekte, `TItem="Pet"` türü belirtir:</span><span class="sxs-lookup"><span data-stu-id="66faa-517">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="66faa-518">Değerleri ve parametreleri basamaklama</span><span class="sxs-lookup"><span data-stu-id="66faa-518">Cascading values and parameters</span></span>

<span data-ttu-id="66faa-519">Bazı senaryolarda, özellikle birden çok bileşen katmanı olduğunda, [bileşen parametreleri](#component-parameters)kullanarak bir üst bileşenden bir alt bileşene veri akışı yapmak uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="66faa-519">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="66faa-520">Değerleri ve parametreleri basamaklama, bir üst bileşenin tüm alt bileşenlerine değer sağlaması için kullanışlı bir yol sağlayarak bu sorunu çözebilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-520">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="66faa-521">Basamaklı değerler ve parametreler, bileşenlerin koordinasyonu için bir yaklaşım sağlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-521">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="66faa-522">Tema örneği</span><span class="sxs-lookup"><span data-stu-id="66faa-522">Theme example</span></span>

<span data-ttu-id="66faa-523">Örnek uygulamadan aşağıdaki örnekte, `ThemeInfo` sınıfı, uygulamanın belirli bir bölümü içindeki tüm düğmelerin aynı stili paylaşması için bileşen hiyerarşisinin akmasını sağlayacak tema bilgilerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="66faa-523">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="66faa-524">*Uıthemeclasses/Themeınfo. cs*:</span><span class="sxs-lookup"><span data-stu-id="66faa-524">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="66faa-525">Bir üst bileşen basamaklı değer bileşeni kullanılarak basamaklı bir değer sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-525">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="66faa-526">@No__t_0 bileşeni, bileşen hiyerarşisinin bir alt ağacını sarmalanmış ve bu alt ağaçta bulunan tüm bileşenlere tek bir değer sağlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-526">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="66faa-527">Örneğin, örnek uygulama, `@Body` özelliğinin düzen gövdesini oluşturan tüm bileşenler için bir geçişli parametre olarak uygulamanın düzenlerindeki tema bilgilerini (`ThemeInfo`) belirtir.</span><span class="sxs-lookup"><span data-stu-id="66faa-527">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="66faa-528">`ButtonClass` ' a, düzen bileşeninde `btn-success` değeri atanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-528">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="66faa-529">Tüm alt bileşenler, `ThemeInfo` basamaklı nesne aracılığıyla bu özelliği kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-529">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="66faa-530">`CascadingValuesParametersLayout` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="66faa-530">`CascadingValuesParametersLayout` component:</span></span>

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

<span data-ttu-id="66faa-531">Basamaklı değerleri kullanmak için, bileşenler `[CascadingParameter]` özniteliği kullanarak Geçişli Parametreler bildirir.</span><span class="sxs-lookup"><span data-stu-id="66faa-531">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="66faa-532">Basamaklı değerler, türe göre basamaklı parametrelere bağlanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-532">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="66faa-533">Örnek uygulamada `CascadingValuesParametersTheme` bileşeni, `ThemeInfo` geçişli değeri basamaklı bir parametreye bağlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-533">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="66faa-534">Parametresi, bileşen tarafından görünen düğmelerden birine ait CSS sınıfını ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-534">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="66faa-535">`CascadingValuesParametersTheme` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="66faa-535">`CascadingValuesParametersTheme` component:</span></span>

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

<span data-ttu-id="66faa-536">Aynı alt ağaçta aynı türdeki birden çok değeri basamakla, her bir `CascadingValue` bileşenine ve karşılık gelen `CascadingParameter` ' ye benzersiz bir `Name` dizesi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="66faa-536">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="66faa-537">Aşağıdaki örnekte, iki `CascadingValue` bileşeni, ada göre farklı `MyCascadingType` örneklerini basamakla:</span><span class="sxs-lookup"><span data-stu-id="66faa-537">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

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

<span data-ttu-id="66faa-538">Alt bileşende, basamaklı parametreler değerlerini, üst bileşendeki ilgili basamaklı değerleri ada göre alır:</span><span class="sxs-lookup"><span data-stu-id="66faa-538">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="66faa-539">TabSet örneği</span><span class="sxs-lookup"><span data-stu-id="66faa-539">TabSet example</span></span>

<span data-ttu-id="66faa-540">Basamaklı parametreler, bileşenlerin bileşen hiyerarşisinde işbirliği yapmasına de olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-540">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="66faa-541">Örneğin, örnek uygulamada aşağıdaki *Tabset* örneğini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="66faa-541">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="66faa-542">Örnek uygulama, sekmelerin uygulandığı `ITab` arabirimine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="66faa-542">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="66faa-543">@No__t_0 bileşeni, çeşitli `Tab` bileşenleri içeren `TabSet` bileşenini kullanır:</span><span class="sxs-lookup"><span data-stu-id="66faa-543">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="66faa-544">Alt `Tab` bileşenleri, `TabSet` ' e açıkça parametre olarak geçirilmemektedir.</span><span class="sxs-lookup"><span data-stu-id="66faa-544">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="66faa-545">Bunun yerine, alt `Tab` bileşenleri, `TabSet` ' in alt içeriğinin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="66faa-545">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="66faa-546">Ancak, `TabSet` ' ın, üst bilgileri ve etkin sekmeyi işleyebilmesi için her bir `Tab` bileşeni hakkında hala bilmeleri gerekir. Ek kod gerektirmeden bu koordinasyonu etkinleştirmek için, `TabSet` bileşeni, kendisini alt `Tab` bileşenleri tarafından çekilen *basamaklı bir değer olarak sağlayabilir* .</span><span class="sxs-lookup"><span data-stu-id="66faa-546">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="66faa-547">`TabSet` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="66faa-547">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="66faa-548">Alt `Tab` bileşenleri, kapsayan `TabSet` kendisini basamaklı bir parametre olarak yakalar, bu nedenle `Tab` bileşenleri kendilerini `TabSet` ve bu sekmenin etkin olduğu koordine eder.</span><span class="sxs-lookup"><span data-stu-id="66faa-548">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="66faa-549">`Tab` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="66faa-549">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="66faa-550">Razor şablonları</span><span class="sxs-lookup"><span data-stu-id="66faa-550">Razor templates</span></span>

<span data-ttu-id="66faa-551">Oluşturma parçaları Razor şablonu sözdizimi kullanılarak tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-551">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="66faa-552">Razor şablonları, bir UI parçacığı tanımlamak ve aşağıdaki biçimi varsaymak için bir yoldur:</span><span class="sxs-lookup"><span data-stu-id="66faa-552">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="66faa-553">Aşağıdaki örnek, `RenderFragment` ve `RenderFragment<T>` değerlerinin nasıl belirtildiğini ve şablonlarının doğrudan bir bileşende nasıl işleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="66faa-553">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="66faa-554">Oluşturma parçaları, [şablonlu bileşenlere](#templated-components)bağımsız değişken olarak da geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-554">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

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

<span data-ttu-id="66faa-555">Önceki kodun işlenmiş çıktısı:</span><span class="sxs-lookup"><span data-stu-id="66faa-555">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="66faa-556">El ile RenderTreeBuilder mantığı</span><span class="sxs-lookup"><span data-stu-id="66faa-556">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="66faa-557">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder`, bileşenleri ve öğeleri işlemek için, C# kodda el ile bileşen oluşturma gibi yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-557">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="66faa-558">Bileşen oluşturmak için `RenderTreeBuilder` kullanımı gelişmiş bir senaryodur.</span><span class="sxs-lookup"><span data-stu-id="66faa-558">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="66faa-559">Hatalı biçimlendirilmiş bir bileşen (örneğin, kapatılmamış bir biçimlendirme etiketi) tanımsız davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-559">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="66faa-560">Başka bir bileşende el ile kullanılabilecek aşağıdaki `PetDetails` bileşenini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="66faa-560">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="66faa-561">Aşağıdaki örnekte, `CreateComponent` yöntemindeki döngü üç `PetDetails` bileşeni oluşturur.</span><span class="sxs-lookup"><span data-stu-id="66faa-561">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="66faa-562">Bileşenleri oluşturmak için `RenderTreeBuilder` yöntemleri çağrılırken (`OpenComponent` ve `AddAttribute`), dizi numaraları kaynak kodu satır sayılarıdır.</span><span class="sxs-lookup"><span data-stu-id="66faa-562">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="66faa-563">Blazor fark algoritması, ayrı çağrı etkinleştirmeleri değil ayrı kod satırlarına karşılık gelen sıra numaralarına dayanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-563">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="66faa-564">@No__t_0 yöntemlerle bir bileşen oluştururken, dizi numaraları için bağımsız değişkenleri sabit olarak kodlayın.</span><span class="sxs-lookup"><span data-stu-id="66faa-564">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="66faa-565">**Sıra numarasını oluşturmak için bir hesaplama veya sayaç kullanmak kötü performansa neden olabilir.**</span><span class="sxs-lookup"><span data-stu-id="66faa-565">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="66faa-566">Daha fazla bilgi için bkz. [kod satırı numaralarıyla Ilgili sıra numaraları ve yürütme sırası çalışmıyor](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) bölümü.</span><span class="sxs-lookup"><span data-stu-id="66faa-566">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="66faa-567">`BuiltContent` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="66faa-567">`BuiltContent` component:</span></span>

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

> <span data-ttu-id="66faa-568">! WARNING @No__t_0 türler, işleme işlemlerinin *sonuçlarının* işlenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="66faa-568">![WARNING] The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="66faa-569">Bunlar, Blazor Framework uygulamasının iç ayrıntılardır.</span><span class="sxs-lookup"><span data-stu-id="66faa-569">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="66faa-570">Bu türlerin *dengesizleşilmesi* ve gelecekteki sürümlerde değişikliğe tabi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="66faa-570">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="66faa-571">Sıra numaraları, kod satırı numaralarıyla ilgilidir ve yürütme sırası değildir</span><span class="sxs-lookup"><span data-stu-id="66faa-571">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="66faa-572">Blazor `.razor` dosyaları her zaman derlenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-572">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="66faa-573">Derleme adımı, çalışma zamanında uygulama performansını geliştiren bilgileri eklemek için kullanılabilir olduğundan, bu büyük olasılıkla `.razor` için harika bir avantajdır.</span><span class="sxs-lookup"><span data-stu-id="66faa-573">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="66faa-574">Bu geliştirmelerin önemli bir örneği *sıra numarası*içerir.</span><span class="sxs-lookup"><span data-stu-id="66faa-574">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="66faa-575">Sıra numaraları, hangi çıkışların ayrı ve sıralı kod satırlarından geldiğini çalışma zamanına işaret ediyor.</span><span class="sxs-lookup"><span data-stu-id="66faa-575">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="66faa-576">Çalışma zamanı, doğrusal bir zamanda, genel ağaç farkı algoritması için genellikle mümkün olandan çok daha hızlı olan etkili ağaç SLA 'ları oluşturmak için bu bilgileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-576">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="66faa-577">Aşağıdaki basit `.razor` dosyasını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="66faa-577">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="66faa-578">Yukarıdaki kod, aşağıdakine benzer şekilde derlenir:</span><span class="sxs-lookup"><span data-stu-id="66faa-578">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="66faa-579">Kod ilk kez yürütüldüğünde, `someFlag` `true` ise, Oluşturucu şunları alır:</span><span class="sxs-lookup"><span data-stu-id="66faa-579">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="66faa-580">Sequence</span><span class="sxs-lookup"><span data-stu-id="66faa-580">Sequence</span></span> | <span data-ttu-id="66faa-581">Tür</span><span class="sxs-lookup"><span data-stu-id="66faa-581">Type</span></span>      | <span data-ttu-id="66faa-582">Veri</span><span class="sxs-lookup"><span data-stu-id="66faa-582">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="66faa-583">0</span><span class="sxs-lookup"><span data-stu-id="66faa-583">0</span></span>        | <span data-ttu-id="66faa-584">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="66faa-584">Text node</span></span> | <span data-ttu-id="66faa-585">Adı</span><span class="sxs-lookup"><span data-stu-id="66faa-585">First</span></span>  |
| <span data-ttu-id="66faa-586">1\.</span><span class="sxs-lookup"><span data-stu-id="66faa-586">1</span></span>        | <span data-ttu-id="66faa-587">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="66faa-587">Text node</span></span> | <span data-ttu-id="66faa-588">Saniye</span><span class="sxs-lookup"><span data-stu-id="66faa-588">Second</span></span> |

<span data-ttu-id="66faa-589">@No__t_0 `false` hale geldiğini ve biçimlendirmenin yeniden işleneceğini varsayın.</span><span class="sxs-lookup"><span data-stu-id="66faa-589">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="66faa-590">Bu kez, Oluşturucu şunları alır:</span><span class="sxs-lookup"><span data-stu-id="66faa-590">This time, the builder receives:</span></span>

| <span data-ttu-id="66faa-591">Sequence</span><span class="sxs-lookup"><span data-stu-id="66faa-591">Sequence</span></span> | <span data-ttu-id="66faa-592">Tür</span><span class="sxs-lookup"><span data-stu-id="66faa-592">Type</span></span>       | <span data-ttu-id="66faa-593">Veri</span><span class="sxs-lookup"><span data-stu-id="66faa-593">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="66faa-594">1\.</span><span class="sxs-lookup"><span data-stu-id="66faa-594">1</span></span>        | <span data-ttu-id="66faa-595">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="66faa-595">Text node</span></span>  | <span data-ttu-id="66faa-596">Saniye</span><span class="sxs-lookup"><span data-stu-id="66faa-596">Second</span></span> |

<span data-ttu-id="66faa-597">Çalışma zamanı bir fark gerçekleştirdiğinde, sırasıyla `0` olan öğenin kaldırıldığını görür, bu nedenle aşağıdaki önemsiz *düzenleme betiğini*oluşturur:</span><span class="sxs-lookup"><span data-stu-id="66faa-597">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="66faa-598">İlk metin düğümünü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="66faa-598">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="66faa-599">Program aracılığıyla sıra numaraları oluşturursanız ne yanlış gider</span><span class="sxs-lookup"><span data-stu-id="66faa-599">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="66faa-600">Aşağıdaki işleme Ağacı Oluşturucusu mantığını yazmanız yerine düşünün:</span><span class="sxs-lookup"><span data-stu-id="66faa-600">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="66faa-601">Şimdi ilk çıktı:</span><span class="sxs-lookup"><span data-stu-id="66faa-601">Now, the first output is:</span></span>

| <span data-ttu-id="66faa-602">Sequence</span><span class="sxs-lookup"><span data-stu-id="66faa-602">Sequence</span></span> | <span data-ttu-id="66faa-603">Tür</span><span class="sxs-lookup"><span data-stu-id="66faa-603">Type</span></span>      | <span data-ttu-id="66faa-604">Veri</span><span class="sxs-lookup"><span data-stu-id="66faa-604">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="66faa-605">0</span><span class="sxs-lookup"><span data-stu-id="66faa-605">0</span></span>        | <span data-ttu-id="66faa-606">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="66faa-606">Text node</span></span> | <span data-ttu-id="66faa-607">Adı</span><span class="sxs-lookup"><span data-stu-id="66faa-607">First</span></span>  |
| <span data-ttu-id="66faa-608">1\.</span><span class="sxs-lookup"><span data-stu-id="66faa-608">1</span></span>        | <span data-ttu-id="66faa-609">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="66faa-609">Text node</span></span> | <span data-ttu-id="66faa-610">Saniye</span><span class="sxs-lookup"><span data-stu-id="66faa-610">Second</span></span> |

<span data-ttu-id="66faa-611">Bu sonuç önceki bir durum ile aynıdır, bu nedenle olumsuz bir sorun yoktur.</span><span class="sxs-lookup"><span data-stu-id="66faa-611">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="66faa-612">`someFlag`, ikinci işlemede `false` ' dir ve çıktı:</span><span class="sxs-lookup"><span data-stu-id="66faa-612">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="66faa-613">Sequence</span><span class="sxs-lookup"><span data-stu-id="66faa-613">Sequence</span></span> | <span data-ttu-id="66faa-614">Tür</span><span class="sxs-lookup"><span data-stu-id="66faa-614">Type</span></span>      | <span data-ttu-id="66faa-615">Veri</span><span class="sxs-lookup"><span data-stu-id="66faa-615">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="66faa-616">0</span><span class="sxs-lookup"><span data-stu-id="66faa-616">0</span></span>        | <span data-ttu-id="66faa-617">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="66faa-617">Text node</span></span> | <span data-ttu-id="66faa-618">Saniye</span><span class="sxs-lookup"><span data-stu-id="66faa-618">Second</span></span> |

<span data-ttu-id="66faa-619">Bu kez, fark algoritması *iki* değişikliğin oluştuğunu görür ve algoritma aşağıdaki düzenleme betiğini üretir:</span><span class="sxs-lookup"><span data-stu-id="66faa-619">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="66faa-620">İlk metin düğümünün değerini `Second` olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="66faa-620">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="66faa-621">İkinci metin düğümünü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="66faa-621">Remove the second text node.</span></span>

<span data-ttu-id="66faa-622">Sıra numaralarının oluşturulması, `if/else` dallarının ve döngülerinin özgün kodda bulunduğu yer hakkındaki tüm yararlı bilgileri kaybetti.</span><span class="sxs-lookup"><span data-stu-id="66faa-622">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="66faa-623">Bu, daha önce olduğu gibi bir fark **ile iki kez** sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-623">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="66faa-624">Bu, önemsiz bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="66faa-624">This is a trivial example.</span></span> <span data-ttu-id="66faa-625">Karmaşık ve derin iç içe yapıları ve özellikle döngülerle daha gerçekçi durumlarda performans maliyeti daha önemlidir.</span><span class="sxs-lookup"><span data-stu-id="66faa-625">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="66faa-626">Hangi döngü bloklarının veya dallarının eklendiğini veya kaldırıldığını hemen belirlemek yerine, fark algoritmasının işleme ağaçlarına katmaları ve genellikle eski ve yeni yapıların nasıl olduğu hakkında yanlış bir biçimde düzenleme betikleri oluşturması birbirleriyle ilişkilendir.</span><span class="sxs-lookup"><span data-stu-id="66faa-626">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="66faa-627">Kılavuz ve ekibinizle</span><span class="sxs-lookup"><span data-stu-id="66faa-627">Guidance and conclusions</span></span>

* <span data-ttu-id="66faa-628">Sıra numaraları dinamik olarak oluşturulursa uygulama performansı de vardır.</span><span class="sxs-lookup"><span data-stu-id="66faa-628">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="66faa-629">Altyapı, derleme zamanında yakalanmadığı takdirde gerekli bilgiler bulunmadığından, çalışma zamanında kendi sıra numaralarını otomatik olarak oluşturamaz.</span><span class="sxs-lookup"><span data-stu-id="66faa-629">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="66faa-630">El ile uygulanan `RenderTreeBuilder` Logic uzun bloklar yazmayın.</span><span class="sxs-lookup"><span data-stu-id="66faa-630">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="66faa-631">@No__t_0 dosyalarını tercih edin ve derleyicinin sıra numaralarıyla uğraşmak için izin verin.</span><span class="sxs-lookup"><span data-stu-id="66faa-631">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="66faa-632">El ile `RenderTreeBuilder` mantığın olmaması durumunda, uzun kod bloklarını `OpenRegion` / `CloseRegion` çağrılarına kaydırılmış daha küçük parçalara ayırın.</span><span class="sxs-lookup"><span data-stu-id="66faa-632">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="66faa-633">Her bölge kendi ayrı dizi numaralarına sahiptir, bu nedenle her bölge içinde sıfırdan (veya herhangi bir rastgele sayıdan) yeniden başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66faa-633">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="66faa-634">Dizi numaraları sabit kodluysa, fark algoritması yalnızca değer değerinde sıra numaralarının artırılmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="66faa-634">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="66faa-635">İlk değer ve boşluklar ilgisiz.</span><span class="sxs-lookup"><span data-stu-id="66faa-635">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="66faa-636">Tek bir seçenek, kod satırı numarasını sıra numarası olarak kullanmak veya sıfırdan başlayıp bir ya da yüzlerce (ya da tercih edilen aralığa) artırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="66faa-636">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="66faa-637">Blazor, sıra numaralarını kullanır, diğer ağaç dağıtma Kullanıcı arabirimi çerçeveleri bunları kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="66faa-637">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="66faa-638">Dizi numaraları kullanıldığında ve Blazor, `.razor` dosyalarını yazan geliştiriciler için otomatik olarak sıra numaralarıyla ilgilenen bir derleme adımının avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="66faa-638">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>

## <a name="localization"></a><span data-ttu-id="66faa-639">Yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="66faa-639">Localization</span></span>

<span data-ttu-id="66faa-640">Blazor Server uygulamaları, [Yerelleştirme ara yazılımı](xref:fundamentals/localization#localization-middleware)kullanılarak yerelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-640">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="66faa-641">Ara yazılım, uygulamadan kaynak isteyen kullanıcılar için uygun kültürü seçer.</span><span class="sxs-lookup"><span data-stu-id="66faa-641">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="66faa-642">Kültür aşağıdaki yaklaşımlardan biri kullanılarak ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="66faa-642">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="66faa-643">Çerezler</span><span class="sxs-lookup"><span data-stu-id="66faa-643">Cookies</span></span>](#cookies)
* [<span data-ttu-id="66faa-644">Kültürü seçmek için Kullanıcı arabirimi sağlama</span><span class="sxs-lookup"><span data-stu-id="66faa-644">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="66faa-645">Daha fazla bilgi ve örnek için bkz. <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="66faa-645">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="66faa-646">Özgü</span><span class="sxs-lookup"><span data-stu-id="66faa-646">Cookies</span></span>

<span data-ttu-id="66faa-647">Yerelleştirme kültürü tanımlama bilgisi kullanıcının kültürünü kalıcı hale getirebilirler.</span><span class="sxs-lookup"><span data-stu-id="66faa-647">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="66faa-648">Tanımlama bilgisi, uygulamanın ana bilgisayar sayfasının `OnGet` yöntemiyle oluşturulur (*Pages/Host. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="66faa-648">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="66faa-649">Yerelleştirme ara yazılımı, sonraki isteklerde Kullanıcı kültürünü ayarlamak için tanımlama bilgilerini okur.</span><span class="sxs-lookup"><span data-stu-id="66faa-649">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="66faa-650">Tanımlama bilgisinin kullanımı, WebSocket bağlantısının kültürü doğru şekilde yaymasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-650">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="66faa-651">Yerelleştirme şemaları URL yolunu veya sorgu dizesini temel alıyorsa, düzen WebSockets ile çalışmayabilir, bu nedenle kültürü kalıcı hale getiremeyebilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-651">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="66faa-652">Bu nedenle, yerelleştirme kültürü tanımlama bilgisinin kullanılması önerilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="66faa-652">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="66faa-653">Kültür bir yerelleştirme tanımlama bilgisinde kalıcı hale getirilir kültür atamak için herhangi bir teknik kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-653">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="66faa-654">Uygulamanın zaten sunucu tarafı ASP.NET Core için bir yerelleştirme şeması varsa, uygulamanın var olan yerelleştirme altyapısını kullanmaya devam edin ve uygulamanın şeması içinde yerelleştirme kültür tanımlama bilgisini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="66faa-654">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="66faa-655">Aşağıdaki örnekte, yerelleştirme ara yazılımı tarafından okunabilen bir tanımlama bilgisinde geçerli kültürün nasıl ayarlanacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="66faa-655">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="66faa-656">Blazor Server uygulamasında aşağıdaki içeriklerle bir *Pages/Host. cshtml. cs* dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="66faa-656">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

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

<span data-ttu-id="66faa-657">Yerelleştirme uygulamada işlenir:</span><span class="sxs-lookup"><span data-stu-id="66faa-657">Localization is handled in the app:</span></span>

1. <span data-ttu-id="66faa-658">Tarayıcı, uygulamaya bir ilk HTTP isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="66faa-658">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="66faa-659">Kültür, yerelleştirme ara yazılımı tarafından atanır.</span><span class="sxs-lookup"><span data-stu-id="66faa-659">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="66faa-660">*_Host. cshtml. cs* içindeki `OnGet` yöntemi, yanıtın bir parçası olarak bir tanımlama bilgisinde kültürü devam ettirir.</span><span class="sxs-lookup"><span data-stu-id="66faa-660">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="66faa-661">Tarayıcı, etkileşimli bir Blazor Server oturumu oluşturmak için bir WebSocket bağlantısı açar.</span><span class="sxs-lookup"><span data-stu-id="66faa-661">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="66faa-662">Yerelleştirme ara yazılımı tanımlama bilgisini okur ve kültürü atar.</span><span class="sxs-lookup"><span data-stu-id="66faa-662">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="66faa-663">Blazor sunucusu oturumu doğru kültür ile başlar.</span><span class="sxs-lookup"><span data-stu-id="66faa-663">The Blazor Server session begins with the correct culture.</span></span>

## <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="66faa-664">Kültürü seçmek için Kullanıcı arabirimi sağlama</span><span class="sxs-lookup"><span data-stu-id="66faa-664">Provide UI to choose the culture</span></span>

<span data-ttu-id="66faa-665">Bir kullanıcının bir kültür seçmesine izin vermek için kullanıcı ARABIRIMI sağlamak üzere bir *yeniden yönlendirme tabanlı yaklaşım* önerilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-665">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="66faa-666">Bu işlem, bir Kullanıcı güvenli bir kaynağa erişmeyi denediğinde bir Web uygulamasında gerçekleşmelere benzer &mdash;the Kullanıcı, oturum açma sayfasına yönlendirilir ve ardından özgün kaynağa yeniden yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="66faa-666">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="66faa-667">Uygulama, bir denetleyiciye yeniden yönlendirme yoluyla kullanıcının seçili kültürünü devam ettirir.</span><span class="sxs-lookup"><span data-stu-id="66faa-667">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="66faa-668">Denetleyici kullanıcının seçili kültürünü bir tanımlama bilgisine ayarlar ve kullanıcıyı özgün URI 'ye yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="66faa-668">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="66faa-669">Bir tanımlama bilgisinde kullanıcının seçili kültürünü ayarlamak ve özgün URI 'ye yeniden yönlendirmeyi gerçekleştirmek için sunucuda bir HTTP uç noktası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="66faa-669">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="66faa-670">Açık yeniden yönlendirme saldırılarını engellemek için `LocalRedirect` eylem sonucunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="66faa-670">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="66faa-671">Daha fazla bilgi için bkz. <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="66faa-671">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="66faa-672">Aşağıdaki bileşen, Kullanıcı bir kültür seçtiğinde ilk yeniden yönlendirmenin nasıl gerçekleştirileceği hakkında bir örnek göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="66faa-672">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

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

### <a name="use-net-localization-scenarios-in-blazor-apps"></a><span data-ttu-id="66faa-673">Blazor uygulamalarında .NET yerelleştirme senaryolarını kullanma</span><span class="sxs-lookup"><span data-stu-id="66faa-673">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="66faa-674">Blazor uygulamaları içinde, aşağıdaki .NET yerelleştirme ve genelleştirme senaryoları kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="66faa-674">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="66faa-675">. NET ' in kaynak sistemi</span><span class="sxs-lookup"><span data-stu-id="66faa-675">.NET's resources system</span></span>
* <span data-ttu-id="66faa-676">Kültüre özgü sayı ve Tarih biçimlendirmesi</span><span class="sxs-lookup"><span data-stu-id="66faa-676">Culture-specific number and date formatting</span></span>

<span data-ttu-id="66faa-677">Blazor 'ın `@bind` işlevselliği, kullanıcının geçerli kültürüne göre Genelleştirme gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="66faa-677">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="66faa-678">Daha fazla bilgi için [veri bağlama](#data-binding) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="66faa-678">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="66faa-679">Sınırlı bir ASP.NET Core yerelleştirme senaryosu kümesi şu anda desteklenmektedir:</span><span class="sxs-lookup"><span data-stu-id="66faa-679">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="66faa-680">`IStringLocalizer<>` *,* Blazor uygulamalarında desteklenir.</span><span class="sxs-lookup"><span data-stu-id="66faa-680">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="66faa-681">`IHtmlLocalizer<>`, `IViewLocalizer<>` ve veri ek açıklamaları yerelleştirme ASP.NET Core MVC senaryolarından ve Blazor uygulamalarında **desteklenmez** .</span><span class="sxs-lookup"><span data-stu-id="66faa-681">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="66faa-682">Daha fazla bilgi için bkz. <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="66faa-682">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="66faa-683">Ölçeklenebilir vektör grafik (SVG) görüntüleri</span><span class="sxs-lookup"><span data-stu-id="66faa-683">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="66faa-684">Blazor tarafından oluşturulduğundan, ölçeklenebilir vektör grafik (SVG) görüntüleri ( *. SVG*) dahil olmak üzere tarayıcıda desteklenen görüntüler `<img>` etiketi aracılığıyla desteklenir:</span><span class="sxs-lookup"><span data-stu-id="66faa-684">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="66faa-685">Benzer şekilde, SVG görüntüleri bir stil sayfası dosyasının ( *. css*) CSS kurallarında desteklenir:</span><span class="sxs-lookup"><span data-stu-id="66faa-685">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="66faa-686">Ancak, satır içi SVG işaretlemesi tüm senaryolarda desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="66faa-686">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="66faa-687">Bir `<svg>` etiketini doğrudan bir bileşen dosyasına ( *. Razor*) yerleştirirseniz, temel görüntü işleme desteklenir, ancak birçok gelişmiş senaryo henüz desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="66faa-687">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="66faa-688">Örneğin, `<use>` etiketleri şu anda dikkate alınamaz ve `@bind` bazı SVG etiketleriyle kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="66faa-688">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="66faa-689">Gelecekteki bir sürümde bu sınırlamaları ele almayı bekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="66faa-689">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="66faa-690">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="66faa-690">Additional resources</span></span>

* <span data-ttu-id="66faa-691"><xref:security/blazor/server> &ndash; kaynak tükenmesi ile Çekişmek zorunda olması gereken Blazor sunucu uygulamaları oluşturmaya yönelik yönergeler Içerir.</span><span class="sxs-lookup"><span data-stu-id="66faa-691"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
