---
title: Oluşturma ve Razor bileşenleri kullanma
author: guardrex
description: Oluşturma ve bileşen ömürleri yönetme verilere bağlayın ve olayları işlemek nasıl dahil olmak üzere, Razor bileşenlerini kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/05/2019
uid: blazor/components
ms.openlocfilehash: fdd755a245b0ef9697b500c734a44fac8942f068
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750142"
---
# <a name="create-and-use-razor-components"></a><span data-ttu-id="8c629-103">Oluşturma ve Razor bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="8c629-103">Create and use Razor components</span></span>

<span data-ttu-id="8c629-104">Tarafından [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), ve [Morné Zaayman](https://github.com/MorneZaayman)</span><span class="sxs-lookup"><span data-stu-id="8c629-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Morné Zaayman](https://github.com/MorneZaayman)</span></span>

<span data-ttu-id="8c629-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8c629-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8c629-106">Blazor uygulamaları kullanılarak oluşturulur *bileşenleri*.</span><span class="sxs-lookup"><span data-stu-id="8c629-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="8c629-107">Bir bileşen, kullanıcı arabirimi (UI), sayfa, iletişim veya form gibi kendi içinde bir öbektir.</span><span class="sxs-lookup"><span data-stu-id="8c629-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="8c629-108">Bir bileşeni, HTML biçimlendirmesi ve veri ekleme veya UI olaylarına yanıt vermek için gereken işleme mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="8c629-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="8c629-109">Esnek ve basit bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="8c629-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="8c629-110">Bunlar iç içe geçmiş, yeniden kullanılabilir ve projeler arasında paylaşılan.</span><span class="sxs-lookup"><span data-stu-id="8c629-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="8c629-111">Bileşen sınıfları</span><span class="sxs-lookup"><span data-stu-id="8c629-111">Component classes</span></span>

<span data-ttu-id="8c629-112">Bileşenleri içinde uygulanan [Razor](xref:mvc/views/razor) bileşen dosyaları ( *.razor*) bir birleşimi kullanılarak C# ve HTML biçimlendirmesi.</span><span class="sxs-lookup"><span data-stu-id="8c629-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span>

<span data-ttu-id="8c629-113">Bileşenlerini kullanarak yazarı olduğu *.cshtml* dosyaları kullanarak Razor bileşen dosyaları tanımlanmış olduğu sürece dosya uzantısı `_RazorComponentInclude` MSBuild özelliği.</span><span class="sxs-lookup"><span data-stu-id="8c629-113">Components can be authored using the *.cshtml* file extension as long as the files are identified as Razor component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="8c629-114">Örneğin, belirten bir uygulamayı tüm *.cshtml* altında dosyaları *sayfaları* klasör Razor bileşenleri dosyaları kabul:</span><span class="sxs-lookup"><span data-stu-id="8c629-114">For example, an app that specifies that all *.cshtml* files under the *Pages* folder should be treated as Razor components files:</span></span>

```xml
<_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
```

<span data-ttu-id="8c629-115">Bir bileşen için kullanıcı Arabirimi, HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="8c629-115">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="8c629-116">(Örneğin, döngü, koşullular, ifadeleri) dinamik işleme mantığı, katıştırılmış kullanarak eklenir C# adlı söz dizimi [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="8c629-116">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="8c629-117">Bir uygulamanın ne zaman derlenir, HTML biçimlendirmesi ve C# işleme mantığı, bir bileşen sınıfı dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="8c629-117">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="8c629-118">Oluşturulan sınıfın adı dosya adıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="8c629-118">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="8c629-119">Bileşen sınıfı üyeleri tanımlanmış bir `@functions` blok (birden fazla `@functions` bloğu izin verilen).</span><span class="sxs-lookup"><span data-stu-id="8c629-119">Members of the component class are defined in an `@functions` block (more than one `@functions` block is permissible).</span></span> <span data-ttu-id="8c629-120">İçinde `@functions` blok, bileşen durumu (Özellikler, alanlar) olay işleme için veya başka bir bileşen mantığı tanımlamak için yöntemleri ile belirtilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-120">In the `@functions` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span>

<span data-ttu-id="8c629-121">Bileşen üyeleri, ardından bileşenin parçası mantığı kullanarak işleme olarak kullanılabilir C# ile başlayan ifadeleri `@`.</span><span class="sxs-lookup"><span data-stu-id="8c629-121">Component members can then be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="8c629-122">Örneğin, bir C# alan ekleyerek işlenen `@` alan adı.</span><span class="sxs-lookup"><span data-stu-id="8c629-122">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="8c629-123">Aşağıdaki örnek, değerlendirir ve işler:</span><span class="sxs-lookup"><span data-stu-id="8c629-123">The following example evaluates and renders:</span></span>

* <span data-ttu-id="8c629-124">`_headingFontStyle` CSS özellik değerini `font-style`.</span><span class="sxs-lookup"><span data-stu-id="8c629-124">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="8c629-125">`_headingText` içeriği için `<h1>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="8c629-125">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="8c629-126">Bileşen, bileşen başlangıçta işlenen sonra olaylara yanıt olarak, işleme ağacında yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8c629-126">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="8c629-127">Blazor yeni bir işleme ağacı Öncekine karşı karşılaştırır ve herhangi bir değişiklik tarayıcının belge nesne modeli (DOM) için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8c629-127">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="8c629-128">Bileşenleri sıradan C# sınıfları ve bir projesi içinde her yerden yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-128">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="8c629-129">Web sayfaları genellikle üreten bileşenler bulunan *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="8c629-129">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="8c629-130">Sayfası olmayan bileşenleri içine yerleştirildiğinde sık *paylaşılan* klasör veya özel bir klasör projeye eklendi.</span><span class="sxs-lookup"><span data-stu-id="8c629-130">Non-page components are frequently placed into the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="8c629-131">Özel bir klasör kullanmayı ekleyin ya da özel klasör ad alanı ana bileşen veya uygulamanın  *\_Imports.razor* dosya.</span><span class="sxs-lookup"><span data-stu-id="8c629-131">To use a custom folder, either add the custom folder's namespace to the parent component or to the app's *\_Imports.razor* file.</span></span> <span data-ttu-id="8c629-132">Örneğin, aşağıdaki ad alanı bileşenlerde yapar bir *bileşenleri* klasör uygulamanın kök ad alanı olduğunda kullanılabilen `WebApplication`:</span><span class="sxs-lookup"><span data-stu-id="8c629-132">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="8c629-133">Bileşenleri Razor sayfaları ve MVC uygulamalarla tümleştirin</span><span class="sxs-lookup"><span data-stu-id="8c629-133">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="8c629-134">Bileşenleri ile mevcut Razor sayfaları ve MVC uygulamaları kullanın.</span><span class="sxs-lookup"><span data-stu-id="8c629-134">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="8c629-135">Var olan sayfaları veya Razor bileşenler kullanmaya görünümleri yeniden gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="8c629-135">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="8c629-136">Sayfa veya Görünüm işlendiğinde bileşenleri prerendered&dagger; aynı anda.</span><span class="sxs-lookup"><span data-stu-id="8c629-136">When the page or view is rendered, components are prerendered&dagger; at the same time.</span></span> 

> [!NOTE]
> <span data-ttu-id="8c629-137">&dagger;Sunucu tarafı prerendering Blazor sunucu tarafı uygulamalar için varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="8c629-137">&dagger;Server-side prerendering is enabled for Blazor server-side apps by default.</span></span> <span data-ttu-id="8c629-138">İstemci tarafı Blazor uygulamaları prerendering yaklaşan Preview 5 sürümüne destekler.</span><span class="sxs-lookup"><span data-stu-id="8c629-138">Client-side Blazor apps will support prerendering in the upcoming Preview 5 release.</span></span> <span data-ttu-id="8c629-139">Daha fazla bilgi için [MapFallbackToPage/dosyasını kullanmak için şablonlar/ara yazılım güncelleştirmesi](https://github.com/aspnet/AspNetCore/issues/8852).</span><span class="sxs-lookup"><span data-stu-id="8c629-139">For more information, see [Update templates/middleware to use MapFallbackToPage/File](https://github.com/aspnet/AspNetCore/issues/8852).</span></span>

<span data-ttu-id="8c629-140">Bir bileşenden bir sayfa ya da Görünüm işlemek için `RenderComponentAsync<TComponent>` HTML yardımcı yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8c629-140">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

<span data-ttu-id="8c629-141">Sayfalar ve görünümler bileşenleri kullanabilirsiniz, ancak listesiyse true değil.</span><span class="sxs-lookup"><span data-stu-id="8c629-141">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="8c629-142">Bileşenler, kısmi görünümleri ve bölümler gibi görünümü ve sayfa belirli senaryoları kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="8c629-142">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="8c629-143">Bir bileşene dönüştürerek bir bileşende kısmi görünümü, kısmi görünümü mantıksal çarpanını mantığı kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="8c629-143">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="8c629-144">Blazor sunucu tarafı uygulamalar yönetilen bileşenleri işlenmiş ve bileşen durumunu şeklini hakkında daha fazla bilgi için bkz: <xref:blazor/hosting-models> makalesi.</span><span class="sxs-lookup"><span data-stu-id="8c629-144">For more information on how components are rendered and component state is managed in Blazor server-side apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="using-components"></a><span data-ttu-id="8c629-145">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="8c629-145">Using components</span></span>

<span data-ttu-id="8c629-146">Bileşenleri, diğer bileşenlerin bunları bildirerek içerebilir HTML öğesi söz dizimini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="8c629-146">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="8c629-147">Bir bileşen kullanma için işaretleme, etiketin adını bileşen türü olduğu gibi HTML etiketleri arar.</span><span class="sxs-lookup"><span data-stu-id="8c629-147">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="8c629-148">Aşağıdaki biçimlendirmede *Index.razor* işleyen bir `HeadingComponent` örneği:</span><span class="sxs-lookup"><span data-stu-id="8c629-148">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="8c629-149">*Components/HeadingComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="8c629-149">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

## <a name="component-parameters"></a><span data-ttu-id="8c629-150">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="8c629-150">Component parameters</span></span>

<span data-ttu-id="8c629-151">Bileşenleri olabilir *bileşeni parametreleri*, hangi kullanılarak tanımlanır *genel olmayan* ile bileşen sınıfı özellikleri `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8c629-151">Components can have *component parameters*, which are defined using *non-public* properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="8c629-152">Öznitelikleri bir bileşen için bağımsız değişken biçimlendirme içinde belirtmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="8c629-152">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="8c629-153">Aşağıdaki örnekte, `ParentComponent` değerini ayarlar `Title` özelliği `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="8c629-153">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="8c629-154">*Pages/ParentComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="8c629-154">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

<span data-ttu-id="8c629-155">*Components/ChildComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="8c629-155">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

## <a name="child-content"></a><span data-ttu-id="8c629-156">Alt içeriğin</span><span class="sxs-lookup"><span data-stu-id="8c629-156">Child content</span></span>

<span data-ttu-id="8c629-157">Bileşenleri başka bir bileşen içeriğini ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c629-157">Components can set the content of another component.</span></span> <span data-ttu-id="8c629-158">Atama bileşen alıcı bileşeni belirtin etiketleri arasında içerik sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c629-158">The assigning component provides the content between the tags that specify the receiving component.</span></span> <span data-ttu-id="8c629-159">Örneğin, bir `ParentComponent` içerik işleme için bir alt bileşen tarafından içerik içine yerleştirerek sağlayabilir `<ChildComponent>` etiketler.</span><span class="sxs-lookup"><span data-stu-id="8c629-159">For example, a `ParentComponent` can provide content for rendering by a Child component by placing the content inside `<ChildComponent>` tags.</span></span>

<span data-ttu-id="8c629-160">*Pages/ParentComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="8c629-160">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

<span data-ttu-id="8c629-161">Alt bileşen bir `ChildContent` temsil eden özellik bir `RenderFragment`.</span><span class="sxs-lookup"><span data-stu-id="8c629-161">The Child component has a `ChildContent` property that represents a `RenderFragment`.</span></span> <span data-ttu-id="8c629-162">Değerini `ChildContent` alt bileşen işaretlemede içerik nerede oluşturulması gerekip konumlandırıldı.</span><span class="sxs-lookup"><span data-stu-id="8c629-162">The value of `ChildContent` is positioned in the child component's markup where the content should be rendered.</span></span> <span data-ttu-id="8c629-163">Aşağıdaki örnekte, değerini `ChildContent` ana bileşenden alınan ve önyükleme bölmenin içinde işlenen `panel-body`.</span><span class="sxs-lookup"><span data-stu-id="8c629-163">In the following example, the value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="8c629-164">*Components/ChildComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="8c629-164">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="8c629-165">Özellik Alma `RenderFragment` içeriği adlı `ChildContent` kural tarafından.</span><span class="sxs-lookup"><span data-stu-id="8c629-165">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

## <a name="data-binding"></a><span data-ttu-id="8c629-166">Veri bağlama</span><span class="sxs-lookup"><span data-stu-id="8c629-166">Data binding</span></span>

<span data-ttu-id="8c629-167">Veri bağlama bileşenleri hem DOM öğeleri ile gerçekleştirilir `bind` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8c629-167">Data binding to both components and DOM elements is accomplished with the `bind` attribute.</span></span> <span data-ttu-id="8c629-168">Aşağıdaki örnek bağlar `_italicsCheck` alan için onay kutusunun işaretli durumu:</span><span class="sxs-lookup"><span data-stu-id="8c629-168">The following example binds the `_italicsCheck` field to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    bind="@_italicsCheck" />
```

<span data-ttu-id="8c629-169">Onay kutusunu işaretli ve seçildiğinde özelliğin değerini şekilde güncelleştirilir `true` ve `false`sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="8c629-169">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="8c629-170">Yalnızca bileşen, özelliğin değerinin değiştirilmesi için değil yanıt oluşturulduğunda onay kutusunu kullanıcı Arabiriminde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-170">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="8c629-171">Olay işleyici kodu yürütüldükten sonra bileşenleri kendilerini işleme olduğundan, özellik güncelleştirmeleri genellikle kullanıcı Arabiriminde hemen yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="8c629-171">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="8c629-172">Kullanarak `bind` ile bir `CurrentValue` özelliği (`<input bind="@CurrentValue" />`) aslında aşağıdakine eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="8c629-172">Using `bind` with a `CurrentValue` property (`<input bind="@CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="8c629-173">Bileşen işlendiğinde `value` giriş öğesinin geldiği `CurrentValue` özelliği.</span><span class="sxs-lookup"><span data-stu-id="8c629-173">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="8c629-174">Kullanıcı, metin kutusuna yazdığında `onchange` olay tetiklenir ve `CurrentValue` özelliği değiştirilmiş değerine ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8c629-174">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="8c629-175">Gerçekte, kod oluşturma biraz daha karmaşık olduğundan `bind` tür dönüştürmeleri gerçekleştirildiği birkaç durum işler.</span><span class="sxs-lookup"><span data-stu-id="8c629-175">In reality, the code generation is a little more complex because `bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="8c629-176">Giren İlkesi `bind` geçerli değerini bir ifade ile ilişkilendirir bir `value` kayıtlı işleyici kullanarak öznitelik ve işleyicilerini değişiklikler.</span><span class="sxs-lookup"><span data-stu-id="8c629-176">In principle, `bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="8c629-177">Ek olarak `onchange`, özelliği gibi diğer olayları kullanarak bağlanabilir `oninput` bağlamak gerekenler hakkında daha fazla açık olan tarafından:</span><span class="sxs-lookup"><span data-stu-id="8c629-177">In addition to `onchange`, the property can be bound using other events like `oninput` by being more explicit about what to bind to:</span></span>

```cshtml
<input type="text" bind-value-oninput="@CurrentValue" />
```

<span data-ttu-id="8c629-178">Farklı `onchange`, `oninput` metin kutusuna giriş her karakter için ateşlenir.</span><span class="sxs-lookup"><span data-stu-id="8c629-178">Unlike `onchange`, `oninput` fires for every character that is input into the text box.</span></span>

<span data-ttu-id="8c629-179">**Biçim dizeleri**</span><span class="sxs-lookup"><span data-stu-id="8c629-179">**Format strings**</span></span>

<span data-ttu-id="8c629-180">Veri bağlama ile birlikte çalışır <xref:System.DateTime> biçim dizeleri.</span><span class="sxs-lookup"><span data-stu-id="8c629-180">Data binding works with <xref:System.DateTime> format strings.</span></span> <span data-ttu-id="8c629-181">Para birimi veya sayı biçimleri gibi diğer biçim ifadeleri şu anda kullanılamıyor.</span><span class="sxs-lookup"><span data-stu-id="8c629-181">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd" />

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="8c629-182">`format-value` Özniteliği uygulamak için tarih biçimini belirtir `value` , `input` öğesi.</span><span class="sxs-lookup"><span data-stu-id="8c629-182">The `format-value` attribute specifies the date format to apply to the `value` of the `input` element.</span></span> <span data-ttu-id="8c629-183">Biçimi de değer ayrıştırmak için kullanılan zaman bir `onchange` olayı oluşur.</span><span class="sxs-lookup"><span data-stu-id="8c629-183">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="8c629-184">**Bileşen parametreleri**</span><span class="sxs-lookup"><span data-stu-id="8c629-184">**Component parameters**</span></span>

<span data-ttu-id="8c629-185">Bağlama bileşeni parametreleri de tanır burada `bind-{property}` bir özellik değeri bileşenlerinde bağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c629-185">Binding also recognizes component parameters, where `bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="8c629-186">Aşağıdaki bileşen `ChildComponent` ve bağlar `ParentYear` üst parametresinden `Year` alt bileşen parametresi:</span><span class="sxs-lookup"><span data-stu-id="8c629-186">The following component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

<span data-ttu-id="8c629-187">Ana bileşenin:</span><span class="sxs-lookup"><span data-stu-id="8c629-187">Parent component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">
    Change Year to 1986
</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="8c629-188">Alt bileşeni:</span><span class="sxs-lookup"><span data-stu-id="8c629-188">Child component:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="8c629-189">`EventCallback<T>` açıklanan [EventCallback](#eventcallback) bölümü.</span><span class="sxs-lookup"><span data-stu-id="8c629-189">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="8c629-190">Yükleme `ParentComponent` aşağıdaki biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="8c629-190">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="8c629-191">Varsa değerini `ParentYear` düğmesini seçerek özelliği değiştirildiğinde `ParentComponent`, `Year` özelliği `ChildComponent` güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-191">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="8c629-192">Öğesinin yeni değeri `Year` Arabiriminde işlenen olduğunda `ParentComponent` rerendered:</span><span class="sxs-lookup"><span data-stu-id="8c629-192">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="8c629-193">`Year` Bir yardımcı olduğundan parametre bağlanabilir `YearChanged` türüyle eşleşen olay `Year` parametresi.</span><span class="sxs-lookup"><span data-stu-id="8c629-193">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="8c629-194">Kural olarak, `<ChildComponent bind-Year="@ParentYear" />` yazmak, temelde eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="8c629-194">By convention, `<ChildComponent bind-Year="@ParentYear" />` is essentially equivalent to writing,</span></span>

```cshtml
<ChildComponent bind-Year-YearChanged="@ParentYear" />
```

<span data-ttu-id="8c629-195">Genel olarak, bir özelliği karşılık gelen olay işleyicisi kullanarak bir bağlanabilir `bind-property-event` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8c629-195">In general, a property can be bound to a corresponding event handler using `bind-property-event` attribute.</span></span>

## <a name="event-handling"></a><span data-ttu-id="8c629-196">Olay işleme</span><span class="sxs-lookup"><span data-stu-id="8c629-196">Event handling</span></span>

<span data-ttu-id="8c629-197">Razor bileşenleri olay işleme özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c629-197">Razor components provide event handling features.</span></span> <span data-ttu-id="8c629-198">İçin bir HTML öğesi öznitelik adlı `on<event>` (örneğin, `onclick`, `onsubmit`) temsilci türü belirtilmiş bir değer ile Razor bileşenlerini değerlendirir özniteliğin değeri bir olay işleyicisi.</span><span class="sxs-lookup"><span data-stu-id="8c629-198">For an HTML element attribute named `on<event>` (for example, `onclick`, `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="8c629-199">Özniteliğin adı her zaman ile başlayan `on`.</span><span class="sxs-lookup"><span data-stu-id="8c629-199">The attribute's name always starts with `on`.</span></span>

<span data-ttu-id="8c629-200">Aşağıdaki kod çağrıları `UpdateHeading` Arabiriminde düğme seçildiğinde yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8c629-200">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="8c629-201">Aşağıdaki kod çağrıları `CheckboxChanged` onay kutusunu kullanıcı Arabiriminde değiştirildiğinde yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8c629-201">The following code calls the `CheckboxChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged" />

@functions {
    private void CheckboxChanged()
    {
        ...
    }
}
```

<span data-ttu-id="8c629-202">Olay işleyicileri zaman uyumsuz ve dönüş ayrıca olabilir bir <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="8c629-202">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="8c629-203">El ile çağırmaya gerek yoktur `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="8c629-203">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="8c629-204">Ortaya çıkan özel durumlar günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-204">Exceptions are logged when they occur.</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="8c629-205">Bazı olaylar için olaya özgü olay bağımsız değişken türleri de izin verilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-205">For some events, event-specific event argument types are permitted.</span></span> <span data-ttu-id="8c629-206">Bu olay türleri birine erişimi gerekli değilse, yöntem çağrısında gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="8c629-206">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="8c629-207">Desteklenen olay bağımsız değişkenleri listesi verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="8c629-207">The list of supported event arguments is:</span></span>

* <span data-ttu-id="8c629-208">UIEventArgs</span><span class="sxs-lookup"><span data-stu-id="8c629-208">UIEventArgs</span></span>
* <span data-ttu-id="8c629-209">UIChangeEventArgs</span><span class="sxs-lookup"><span data-stu-id="8c629-209">UIChangeEventArgs</span></span>
* <span data-ttu-id="8c629-210">UIKeyboardEventArgs</span><span class="sxs-lookup"><span data-stu-id="8c629-210">UIKeyboardEventArgs</span></span>
* <span data-ttu-id="8c629-211">UIMouseEventArgs</span><span class="sxs-lookup"><span data-stu-id="8c629-211">UIMouseEventArgs</span></span>

<span data-ttu-id="8c629-212">Lambda ifadeleri de kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="8c629-212">Lambda expressions can also be used:</span></span>

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="8c629-213">Genellikle gibi ek değerler kapatmak uygun olan öğeleri kümesi yineleme olduğunda.</span><span class="sxs-lookup"><span data-stu-id="8c629-213">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="8c629-214">Aşağıdaki örnek, üç oluşturur düğmeler, her biri çağıran `UpdateHeading` olay bağımsız değişken geçirme (`UIMouseEventArgs`) ve düğme sayısı (`buttonNumber`) kullanıcı Arabiriminde seçili olduğunda:</span><span class="sxs-lookup"><span data-stu-id="8c629-214">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@functions {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="8c629-215">Yapmak **değil** Döngü değişkeninin kullanın (`i`) içinde bir `for` bir lambda ifadesinde doğrudan döngü.</span><span class="sxs-lookup"><span data-stu-id="8c629-215">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="8c629-216">Aksi takdirde aynı değişkene neden tüm lambda ifadeleri tarafından kullanılan `i`değerini tüm lambda içinde ile aynı.</span><span class="sxs-lookup"><span data-stu-id="8c629-216">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="8c629-217">Yerel bir değişkende değeri her zaman yakalama (`buttonNumber` önceki örnekte) ve ardından kullanın.</span><span class="sxs-lookup"><span data-stu-id="8c629-217">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="8c629-218">EventCallback</span><span class="sxs-lookup"><span data-stu-id="8c629-218">EventCallback</span></span>

<span data-ttu-id="8c629-219">İç içe geçmiş bileşen sık karşılaşılan bir senaryodur arzusu bir alt bileşen olay gerçekleştiğinde bir ana bileşenin yöntemini çalıştırılacak olan&mdash;Örneğin, bir `onclick` olay alt gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="8c629-219">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="8c629-220">Bileşenlerinde olaylar oluşturmak için kullanmak bir `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="8c629-220">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="8c629-221">Ana bileşenin bir alt bileşen için bir geri çağırma yöntemi atayabilirsiniz `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="8c629-221">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="8c629-222">Örnek uygulamada alt bileşen bir düğme nasıl 's gösterir `onclick` işleyici ayarlandığından almak için bir `EventCallback` temsilci örneği'nın ana bileşeni.</span><span class="sxs-lookup"><span data-stu-id="8c629-222">The Child component in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's Parent component.</span></span> <span data-ttu-id="8c629-223">`EventCallback` İle yazılan `UIMouseEventArgs`, uygun olduğu bir `onclick` çevre cihazı olay:</span><span class="sxs-lookup"><span data-stu-id="8c629-223">The `EventCallback` is typed with `UIMouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="8c629-224">Ana bileşenin çocuğun ayarlar `EventCallback<T>` için kendi `ShowMessage` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8c629-224">The Parent component sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="8c629-225">Düğme alt bileşeni seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="8c629-225">When the button is selected in the Child component:</span></span>

* <span data-ttu-id="8c629-226">Ana bileşenin `ShowMessage` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="8c629-226">The Parent component's `ShowMessage` method is called.</span></span> <span data-ttu-id="8c629-227">`messageText` Güncelleştirilen ve ana bileşenin içinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8c629-227">`messageText` is updated and displayed in the Parent component.</span></span>
* <span data-ttu-id="8c629-228">Bir çağrı `StateHasChanged` geri çağırma'nın yöntemi gerekli değildir (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="8c629-228">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="8c629-229">`StateHasChanged` yalnızca alt içinde yürütülen olay işleyicileri bileşen rerendering alt olaylarını tetiklemek gibi ana bileşen rerender için otomatik olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="8c629-229">`StateHasChanged` is called automatically to rerender the Parent component, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="8c629-230">`EventCallback` ve `EventCallback<T>` zaman uyumsuz temsilciler izin verir.</span><span class="sxs-lookup"><span data-stu-id="8c629-230">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="8c629-231">`EventCallback<T>` türü kesin olarak belirtilmiş ve belirli bir bağımsız değişken türü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="8c629-231">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="8c629-232">`EventCallback` zayıf yazılmış ve herhangi bir bağımsız değişken türü sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c629-232">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@functions {
    private string messageText;
}
```

<span data-ttu-id="8c629-233">Çağırma bir `EventCallback` veya `EventCallback<T>` ile `InvokeAsync` ve await <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="8c629-233">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="8c629-234">Kullanım `EventCallback` ve `EventCallback<T>` olay işleme ve bileşen parametre bağlama için.</span><span class="sxs-lookup"><span data-stu-id="8c629-234">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span> <span data-ttu-id="8c629-235">Kullanmayın `EventCallback` ve `EventCallback<T>` alt içerik&mdash;kullanmaya devam `RenderFragment` ve `RenderFragment<T>` alt içeriği.</span><span class="sxs-lookup"><span data-stu-id="8c629-235">Don't use `EventCallback` and `EventCallback<T>` for child content&mdash;continue to use `RenderFragment` and `RenderFragment<T>` for child content.</span></span>

<span data-ttu-id="8c629-236">Kesin olarak belirlenmiş tercih `EventCallback<T>`, bileşen kullanıcıları için daha iyi hata geri bildirim sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c629-236">Prefer the strongly typed `EventCallback<T>`, which provides better error feedback to users of the component.</span></span> <span data-ttu-id="8c629-237">Diğer UI olay işleyicilerine benzer, olay parametresi belirten isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="8c629-237">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="8c629-238">Kullanım `EventCallback` çağırma işlemine geçirilen değer olduğunda.</span><span class="sxs-lookup"><span data-stu-id="8c629-238">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="8c629-239">Bileşenleri başvurular yakalama</span><span class="sxs-lookup"><span data-stu-id="8c629-239">Capture references to components</span></span>

<span data-ttu-id="8c629-240">Bileşen başvurularını komutları gibi bu örneğe verebilir böylece bileşen örneğinin başvurmak için bir yol sağlar `Show` veya `Reset`.</span><span class="sxs-lookup"><span data-stu-id="8c629-240">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="8c629-241">Bir bileşen başvurusunu yakalamak için ekleme bir `ref` özniteliği alt bileşen ve aynı ada ve aynı türe sahip bir alan alt bileşeni olarak tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c629-241">To capture a component reference, add a `ref` attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="8c629-242">Bileşen işlendiğinde `loginDialog` alanı ile doldurulur `MyLoginDialog` alt bileşen örneği.</span><span class="sxs-lookup"><span data-stu-id="8c629-242">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="8c629-243">Ardından bileşen örneği üzerinde .NET yöntemlerini çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c629-243">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8c629-244">`loginDialog` Değişkeni bileşeni işlenir ve çıktısını içeren sonra yalnızca doldurulmuş `MyLoginDialog` öğesi.</span><span class="sxs-lookup"><span data-stu-id="8c629-244">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="8c629-245">O noktaya kadar hiçbir şey yoktur başvurmak için.</span><span class="sxs-lookup"><span data-stu-id="8c629-245">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="8c629-246">Bileşen oluşturma işlemini tamamladıktan sonra bileşenleri başvurularını değiştirmek üzere `OnAfterRenderAsync` veya `OnAfterRender` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="8c629-246">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="8c629-247">Bileşen başvurularını yakalarken kullanmak için benzer bir sözdizimi [öğesi başvuruları yakalama](xref:blazor/javascript-interop#capture-references-to-elements), öyle bir [JavaScript birlikte çalışma](xref:blazor/javascript-interop) özelliği.</span><span class="sxs-lookup"><span data-stu-id="8c629-247">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="8c629-248">Bileşen başvurularını JavaScript kodunu geçirilen olmayan&mdash;yalnızca .NET kodda kullanılırlar.</span><span class="sxs-lookup"><span data-stu-id="8c629-248">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="8c629-249">Yapmak **değil** alt bileşenlerin durumunu kesilecek bileşen başvuruları kullanın.</span><span class="sxs-lookup"><span data-stu-id="8c629-249">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="8c629-250">Bunun yerine, normal bildirim temelli parametreler alt bileşenler için veri aktarmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="8c629-250">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="8c629-251">Bu alt bileşenleri otomatik olarak doğru zamanlarda rerender neden olur.</span><span class="sxs-lookup"><span data-stu-id="8c629-251">This causes child components to rerender at the correct times automatically.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="8c629-252">Yaşam döngüsü yöntemleri</span><span class="sxs-lookup"><span data-stu-id="8c629-252">Lifecycle methods</span></span>

<span data-ttu-id="8c629-253">`OnInitAsync` ve `OnInit` bileşeni başlatmak için kod yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-253">`OnInitAsync` and `OnInit` execute code to initialize the component.</span></span> <span data-ttu-id="8c629-254">Zaman uyumsuz bir işlemi gerçekleştirmek için `OnInitAsync` ve `await` anahtar sözcüğü, işlemi:</span><span class="sxs-lookup"><span data-stu-id="8c629-254">To perform an asynchronous operation, use `OnInitAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="8c629-255">Eşzamanlı bir işlem için kullanmak `OnInit`:</span><span class="sxs-lookup"><span data-stu-id="8c629-255">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

<span data-ttu-id="8c629-256">`OnParametersSetAsync` ve `OnParametersSet` bir bileşen üst öğesinden parametreleri aldı ve özelliklerine değerler atanır çağırılır.</span><span class="sxs-lookup"><span data-stu-id="8c629-256">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="8c629-257">Bu yöntemler bileşen başlatmadan sonra yürütülür ve her bileşenin oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="8c629-257">These methods are executed after component initialization and each time the component is rendered:</span></span>

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

<span data-ttu-id="8c629-258">`OnAfterRenderAsync` ve `OnAfterRender` bir bileşen oluşturma tamamlandıktan sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="8c629-258">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="8c629-259">Öğe ve bileşen başvuruları bu noktada doldurulur.</span><span class="sxs-lookup"><span data-stu-id="8c629-259">Element and component references are populated at this point.</span></span> <span data-ttu-id="8c629-260">Bu aşama, işlenmiş DOM öğeleri üzerinde çalışan üçüncü taraf JavaScript kitaplıklarını etkinleştirme gibi işlenmiş içeriği kullanarak ek başlatma adımları gerçekleştirmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="8c629-260">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

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

<span data-ttu-id="8c629-261">`SetParameters` parametreleri ayarlamadan önce kodu çalıştırmak için geçersiz kılınabilir:</span><span class="sxs-lookup"><span data-stu-id="8c629-261">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="8c629-262">Varsa `base.SetParameters` değilse çağrılır, özel kod yolu gerekli gelen parametre değeri yorumlayabilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-262">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="8c629-263">Örneğin, gelen parametreleri sınıfındaki özellikleri atanması için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="8c629-263">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

<span data-ttu-id="8c629-264">`ShouldRender` Kullanıcı Arabiriminde yenilemeyi engellemek için geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-264">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="8c629-265">Uygulama döndürürse `true`, UI yenilenir.</span><span class="sxs-lookup"><span data-stu-id="8c629-265">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="8c629-266">Bile `ShouldRender` olan geçersiz kılınan, bileşen her zaman başlangıçta işlenir.</span><span class="sxs-lookup"><span data-stu-id="8c629-266">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="8c629-267">IDisposable ile bileşen elden çıkarma</span><span class="sxs-lookup"><span data-stu-id="8c629-267">Component disposal with IDisposable</span></span>

<span data-ttu-id="8c629-268">Bir bileşen uyguluyorsa <xref:System.IDisposable>, [Dispose yöntemini](/dotnet/standard/garbage-collection/implementing-dispose) bileşeni Arabiriminden kaldırıldığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="8c629-268">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="8c629-269">Aşağıdaki bileşen `@implements IDisposable` ve `Dispose` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8c629-269">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@functions {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a><span data-ttu-id="8c629-270">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="8c629-270">Routing</span></span>

<span data-ttu-id="8c629-271">İçinde Blazor yönlendirme uygulamasında erişilebilir her bileşeni için bir rota şablonu sağlayarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-271">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="8c629-272">Bir Razor dosya ile bir `@page` yönergesi derlendiğinde, oluşturulan sınıfın belirli bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> belirten rota şablonu.</span><span class="sxs-lookup"><span data-stu-id="8c629-272">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="8c629-273">Çalışma zamanında bileşen sınıfları ile yönlendirici arar bir `RouteAttribute` ve hangi bileşen istenen URL ile eşleşen bir rota şablonuna sahip işler.</span><span class="sxs-lookup"><span data-stu-id="8c629-273">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="8c629-274">Bir bileşenin birden çok yol şablonu uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-274">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="8c629-275">Aşağıdaki bileşen isteklerine yanıt veren `/BlazorRoute` ve `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="8c629-275">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="8c629-276">Yol parametreleri</span><span class="sxs-lookup"><span data-stu-id="8c629-276">Route parameters</span></span>

<span data-ttu-id="8c629-277">Sağlanan yol şablondan, rota parametrelerinin bileşenleri alabilir `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="8c629-277">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="8c629-278">Yönlendirici, karşılık gelen bileşen parametreleri doldurmak için rota parametrelerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="8c629-278">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="8c629-279">*Rota parametresinin bileşen*:</span><span class="sxs-lookup"><span data-stu-id="8c629-279">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="8c629-280">İsteğe bağlı parametreler desteklenmez, bu nedenle iki `@page` yönergeleri, yukarıdaki örnekte uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8c629-280">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="8c629-281">İlk Gezinti parametresi olmadan bileşenine izin verir.</span><span class="sxs-lookup"><span data-stu-id="8c629-281">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="8c629-282">İkinci `@page` yönergesi gereken `{text}` rota parametresi ve değeri atar `Text` özelliği.</span><span class="sxs-lookup"><span data-stu-id="8c629-282">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="8c629-283">Bir "code-behind" deneyimi için temel sınıf devralma</span><span class="sxs-lookup"><span data-stu-id="8c629-283">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="8c629-284">Bileşen dosyaları karıştırmak HTML biçimlendirmesi ve C# aynı dosyada kod işleme.</span><span class="sxs-lookup"><span data-stu-id="8c629-284">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="8c629-285">`@inherits` Yönergesi, bileşen biçimlendirme işleme koddan ayıran bir "code-behind" deneyimiyle Blazor uygulamaları sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-285">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="8c629-286">[Örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) nasıl bir bileşen bir temel sınıf devralma işlemi yapabileceğini gösterir `BlazorRocksBase`, bileşenin özellikleri ve yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c629-286">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="8c629-287">*Blazor Rocks bileşen*:</span><span class="sxs-lookup"><span data-stu-id="8c629-287">*Blazor Rocks component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="8c629-288">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="8c629-288">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="8c629-289">Temel sınıfın türetilmesi `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="8c629-289">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="8c629-290">İçeri aktarma bileşenleri</span><span class="sxs-lookup"><span data-stu-id="8c629-290">Import components</span></span>

<span data-ttu-id="8c629-291">Ad alanı, Razor ile yazılmış bir bileşenin dayanır:</span><span class="sxs-lookup"><span data-stu-id="8c629-291">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="8c629-292">Projenin `RootNamespace`.</span><span class="sxs-lookup"><span data-stu-id="8c629-292">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="8c629-293">Bileşen yolu proje kök.</span><span class="sxs-lookup"><span data-stu-id="8c629-293">The path from the project root to the component.</span></span> <span data-ttu-id="8c629-294">Örneğin, `ComponentsSample/Pages/Index.razor` ad alanındaki `ComponentsSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="8c629-294">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="8c629-295">Bileşenleri izleyin C# bağlama kurallarını olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="8c629-295">Components follow C# name binding rules.</span></span> <span data-ttu-id="8c629-296">Durumunda, *Index.razor*, tüm bileşenleri aynı klasörde *sayfaları*ve üst klasör *ComponentsSample*, kapsam içindedir.</span><span class="sxs-lookup"><span data-stu-id="8c629-296">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="8c629-297">Farklı bir ad alanında tanımlanan bileşenleri Razor'ın kullanarak kapsamına alınabilir [ \@kullanarak](xref:mvc/views/razor#using) yönergesi.</span><span class="sxs-lookup"><span data-stu-id="8c629-297">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="8c629-298">Başka bir bileşen `NavMenu.razor`, klasörde mevcut `ComponentsSample/Shared/`, bileşen kullanılabilir `Index.razor` aşağıdaki `@using` deyimi:</span><span class="sxs-lookup"><span data-stu-id="8c629-298">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="8c629-299">Bileşenleri de başvurabilir bunların tam nitelikli adlarını kullanarak gereksinimini kaldıran [ \@kullanarak](xref:mvc/views/razor#using) yönergesi:</span><span class="sxs-lookup"><span data-stu-id="8c629-299">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="8c629-300">`global::` Nitelik desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="8c629-300">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="8c629-301">Diğer adlı bileşenleriyle alma `using` deyimleri (örneğin, `@using Foo = Bar`) desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="8c629-301">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="8c629-302">Kısmen nitelenmiş adlar desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="8c629-302">Partially qualified names aren't supported.</span></span> <span data-ttu-id="8c629-303">Örneğin, ekleme `@using ComponentsSample` ve bunlara başvurma `NavMenu.razor` ile `<Shared.NavMenu></Shared.NavMenu>` desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="8c629-303">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="razor-support"></a><span data-ttu-id="8c629-304">Razor desteği</span><span class="sxs-lookup"><span data-stu-id="8c629-304">Razor support</span></span>

<span data-ttu-id="8c629-305">**Razor yönergesi**</span><span class="sxs-lookup"><span data-stu-id="8c629-305">**Razor directives**</span></span>

<span data-ttu-id="8c629-306">Razor yönergeleri, aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8c629-306">Razor directives are shown in the following table.</span></span>

| <span data-ttu-id="8c629-307">Yönergesi</span><span class="sxs-lookup"><span data-stu-id="8c629-307">Directive</span></span> | <span data-ttu-id="8c629-308">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8c629-308">Description</span></span> |
| --------- | ----------- |
| [<span data-ttu-id="8c629-309">\@İşlevleri</span><span class="sxs-lookup"><span data-stu-id="8c629-309">\@functions</span></span>](xref:mvc/views/razor#section-5) | <span data-ttu-id="8c629-310">Ekler bir C# bileşenine kod bloğu.</span><span class="sxs-lookup"><span data-stu-id="8c629-310">Adds a C# code block to a component.</span></span> |
| `@implements` | <span data-ttu-id="8c629-311">Oluşturulan bileşen sınıfı için bir arabirim uygular.</span><span class="sxs-lookup"><span data-stu-id="8c629-311">Implements an interface for the generated component class.</span></span> |
| [<span data-ttu-id="8c629-312">\@Devralan</span><span class="sxs-lookup"><span data-stu-id="8c629-312">\@inherits</span></span>](xref:mvc/views/razor#section-3) | <span data-ttu-id="8c629-313">Bileşen devralan sınıf tam denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c629-313">Provides full control of the class that the component inherits.</span></span> |
| [<span data-ttu-id="8c629-314">\@ekleme</span><span class="sxs-lookup"><span data-stu-id="8c629-314">\@inject</span></span>](xref:mvc/views/razor#section-4) | <span data-ttu-id="8c629-315">Hizmet ekleme gelen etkinleştirir [hizmet kapsayıcı](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8c629-315">Enables service injection from the [service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="8c629-316">Daha fazla bilgi için [görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8c629-316">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span> |
| `@layout` | <span data-ttu-id="8c629-317">Bir düzen bileşeni belirtir.</span><span class="sxs-lookup"><span data-stu-id="8c629-317">Specifies a layout component.</span></span> <span data-ttu-id="8c629-318">Düzen bileşenleri, kod yinelemesi ve tutarsızlık önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8c629-318">Layout components are used to avoid code duplication and inconsistency.</span></span> |
| [<span data-ttu-id="8c629-319">\@Sayfa</span><span class="sxs-lookup"><span data-stu-id="8c629-319">\@page</span></span>](xref:razor-pages/index#razor-pages) | <span data-ttu-id="8c629-320">Bileşen doğrudan istekleri işleyeceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="8c629-320">Specifies that the component should handle requests directly.</span></span> <span data-ttu-id="8c629-321">`@page` Yönergesi, bir rota ve isteğe bağlı parametreler ile belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-321">The `@page` directive can be specified with a route and optional parameters.</span></span> <span data-ttu-id="8c629-322">Razor sayfaları aksine `@page` yönergesi üst dosyanın ilk yönerge olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="8c629-322">Unlike Razor Pages, the `@page` directive doesn't need to be the first directive at the top of the file.</span></span> <span data-ttu-id="8c629-323">Daha fazla bilgi için [yönlendirme](xref:blazor/routing).</span><span class="sxs-lookup"><span data-stu-id="8c629-323">For more information, see [Routing](xref:blazor/routing).</span></span> |
| [<span data-ttu-id="8c629-324">\@kullanma</span><span class="sxs-lookup"><span data-stu-id="8c629-324">\@using</span></span>](xref:mvc/views/razor#using) | <span data-ttu-id="8c629-325">Ekler C# `using` yönergesini oluşturulan bileşen sınıfı.</span><span class="sxs-lookup"><span data-stu-id="8c629-325">Adds the C# `using` directive to the generated component class.</span></span> <span data-ttu-id="8c629-326">Bu kapsamın içine o ad alanında tanımlanan tüm bileşenleri de getirir.</span><span class="sxs-lookup"><span data-stu-id="8c629-326">This also brings all the components defined in that namespace into scope.</span></span> |

<span data-ttu-id="8c629-327">**Koşullu öznitelikleri**</span><span class="sxs-lookup"><span data-stu-id="8c629-327">**Conditional attributes**</span></span>

<span data-ttu-id="8c629-328">Öznitelikleri .NET değerine göre koşullu olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="8c629-328">Attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="8c629-329">Değer ise `false` veya `null`, öznitelik işlenen değil.</span><span class="sxs-lookup"><span data-stu-id="8c629-329">If the value is `false` or `null`,  the attribute isn't rendered.</span></span> <span data-ttu-id="8c629-330">Değer ise `true`, öznitelik işlenen simge.</span><span class="sxs-lookup"><span data-stu-id="8c629-330">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="8c629-331">Aşağıdaki örnekte, `IsCompleted` belirler `checked` denetimin biçimlendirme içinde işlenir:</span><span class="sxs-lookup"><span data-stu-id="8c629-331">In the following example, `IsCompleted` determines if `checked` is rendered in the control's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

<span data-ttu-id="8c629-332">Varsa `IsCompleted` olduğu `true`, onay kutusunu olarak işlenir:</span><span class="sxs-lookup"><span data-stu-id="8c629-332">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="8c629-333">Varsa `IsCompleted` olduğu `false`, onay kutusunu olarak işlenir:</span><span class="sxs-lookup"><span data-stu-id="8c629-333">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="8c629-334">**Razor hakkında daha fazla bilgi**</span><span class="sxs-lookup"><span data-stu-id="8c629-334">**Additional information on Razor**</span></span>

<span data-ttu-id="8c629-335">Razor hakkında daha fazla bilgi için bkz. [Razor söz dizimi başvurusu](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="8c629-335">For more information on Razor, see the [Razor syntax reference](xref:mvc/views/razor).</span></span>

## <a name="raw-html"></a><span data-ttu-id="8c629-336">Ham HTML</span><span class="sxs-lookup"><span data-stu-id="8c629-336">Raw HTML</span></span>

<span data-ttu-id="8c629-337">Dizeler genellikle DOM metin düğümleri kullanarak içeriyor olabilir herhangi bir biçimlendirme yok sayıldı ve düz metin kabul yani işlenir.</span><span class="sxs-lookup"><span data-stu-id="8c629-337">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="8c629-338">Ham HTML oluşturmak için bir HTML içeriği kaydırma bir `MarkupString` değeri.</span><span class="sxs-lookup"><span data-stu-id="8c629-338">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="8c629-339">Değer HTML veya SVG ayrıştırılması ve DOM'a eklenmesi</span><span class="sxs-lookup"><span data-stu-id="8c629-339">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="8c629-340">Herhangi bir yapıda ham HTML'yi işlemeye güvenilmeyen kaynak bir **güvenlik riski** ve kaçınılmalıdır!</span><span class="sxs-lookup"><span data-stu-id="8c629-340">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="8c629-341">Aşağıdaki örnek, gösterir kullanarak `MarkupString` bir bileşenin işlenen çıkışı için bir statik HTML içeriği bloğunu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8c629-341">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@functions {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="8c629-342">Şablonlu bileşenleri</span><span class="sxs-lookup"><span data-stu-id="8c629-342">Templated components</span></span>

<span data-ttu-id="8c629-343">Şablonlu bileşenleri bileşenin işleme mantığı bir parçası olarak kullanılabilir bir parametre olarak bir veya daha fazla kullanıcı Arabirimi şablonları kabul bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="8c629-343">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="8c629-344">Şablonlu bileşenleri normal bileşenleri daha fazla yeniden kullanılabilir olan üst düzey bileşeni yazmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c629-344">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="8c629-345">Birkaç örnek şunlardır:</span><span class="sxs-lookup"><span data-stu-id="8c629-345">A couple of examples include:</span></span>

* <span data-ttu-id="8c629-346">Bir tablonun üst bilgi, satırları ve alt bilgisi için şablonları belirtmesine imkan tanıyan bir tablo bileşeni.</span><span class="sxs-lookup"><span data-stu-id="8c629-346">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="8c629-347">Bir listedeki öğeleri işleme için bir şablon belirtmesine imkan tanıyan bir liste bileşeni.</span><span class="sxs-lookup"><span data-stu-id="8c629-347">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="8c629-348">Şablon parametreleri</span><span class="sxs-lookup"><span data-stu-id="8c629-348">Template parameters</span></span>

<span data-ttu-id="8c629-349">Şablonlu bir bileşen türünde bir veya daha fazla bileşen parametreleri belirterek tanımlanmış `RenderFragment` veya `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="8c629-349">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="8c629-350">Bir işleme parça bileşen tarafından işlenen kullanıcı arabiriminin bir kesimi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="8c629-350">A render fragment represents a segment of UI that is rendered by the component.</span></span> <span data-ttu-id="8c629-351">Bir işleme parça, isteğe bağlı olarak işleme parça çağrıldığında belirtilebilecek bir parametre alır.</span><span class="sxs-lookup"><span data-stu-id="8c629-351">A render fragment optionally takes a parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="8c629-352">*Tablo şablon bileşeni*:</span><span class="sxs-lookup"><span data-stu-id="8c629-352">*Table Template component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="8c629-353">Şablonlu bir bileşen kullanırken, şablon parametrelerinin parametrelerinin adları eşleşen alt öğeleri kullanılarak belirtilebilir (`TableHeader` ve `RowTemplate` aşağıdaki örnekte):</span><span class="sxs-lookup"><span data-stu-id="8c629-353">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="8c629-354">Şablon bağlam parametreleri</span><span class="sxs-lookup"><span data-stu-id="8c629-354">Template context parameters</span></span>

<span data-ttu-id="8c629-355">Bileşen Türü bağımsız değişkenleri `RenderFragment<T>` öğeleri adlı örtük bir parametre olarak geçirilen `context` (örneğin önceki kod örneğinde,'den `@context.PetId`), ancak parametre adını kullanarak değiştirebilirsiniz `Context` alt özniteliği öğe.</span><span class="sxs-lookup"><span data-stu-id="8c629-355">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="8c629-356">Aşağıdaki örnekte, `RowTemplate` öğenin `Context` özniteliği belirtir `pet` parametresi:</span><span class="sxs-lookup"><span data-stu-id="8c629-356">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="8c629-357">Alternatif olarak, belirtebileceğiniz `Context` bileşen öğesindeki özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8c629-357">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="8c629-358">Belirtilen `Context` özniteliği için tüm belirtilen şablon parametreleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8c629-358">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="8c629-359">Örtük alt içeriğin içerik parametresi adını belirlemek istediğinizde bu kullanışlı olabilir (herhangi bir sarmalama olmadan alt öğesi).</span><span class="sxs-lookup"><span data-stu-id="8c629-359">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="8c629-360">Aşağıdaki örnekte, `Context` özniteliği görünür `TableTemplate` öğenin ve tüm şablon parametreleri için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="8c629-360">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="8c629-361">Genel yazılmış bileşenler</span><span class="sxs-lookup"><span data-stu-id="8c629-361">Generic-typed components</span></span>

<span data-ttu-id="8c629-362">Şablonlu bileşenleri çoğunlukla genel olarak yazılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8c629-362">Templated components are often generically typed.</span></span> <span data-ttu-id="8c629-363">Örneğin, bir genel liste görünümü şablonu bileşeni işlemek için kullanılabilir `IEnumerable<T>` değerleri.</span><span class="sxs-lookup"><span data-stu-id="8c629-363">For example, a generic List View Template component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="8c629-364">Genel bileşen tanımlamak için `@typeparam` tür parametreleri belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="8c629-364">To define a generic component, use the `@typeparam` directive to specify type parameters.</span></span>

<span data-ttu-id="8c629-365">*ListView şablonu bileşen*:</span><span class="sxs-lookup"><span data-stu-id="8c629-365">*ListView Template component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="8c629-366">Tür parametresi, genel yazılmış bileşenler kullanırken, mümkünse algılanır:</span><span class="sxs-lookup"><span data-stu-id="8c629-366">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="8c629-367">Aksi takdirde, tür parametresi açık tür parametresinin adıyla eşleşen bir özniteliği kullanılarak belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="8c629-367">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="8c629-368">Aşağıdaki örnekte, `TItem="Pet"` türünü belirtir:</span><span class="sxs-lookup"><span data-stu-id="8c629-368">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="8c629-369">Geçişli değerleri ve parametreler</span><span class="sxs-lookup"><span data-stu-id="8c629-369">Cascading values and parameters</span></span>

<span data-ttu-id="8c629-370">Bazı senaryolarda, akış verileri kullanarak bir alt bileşen bir üst bileşeninden kullanışsız [bileşeni parametreleri](#component-parameters), özellikle birçok bileşen katmanları olduğunda.</span><span class="sxs-lookup"><span data-stu-id="8c629-370">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="8c629-371">Geçişli değerleri ve parametre bir üst bileşeni tüm alt öğe bileşenleri için bir değer sağlamak için kullanışlı bir yol sağlayarak bu sorunu çözer.</span><span class="sxs-lookup"><span data-stu-id="8c629-371">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="8c629-372">Ayrıca basamaklı değerleri ve parametreler koordine etmek, bileşenler için bir yaklaşım sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c629-372">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="8c629-373">Tema örnek</span><span class="sxs-lookup"><span data-stu-id="8c629-373">Theme example</span></span>

<span data-ttu-id="8c629-374">Aşağıdaki *tema* örnek uygulama örneğinden `ThemeInfo` sınıfı, böylece tüm düğmelerin belirli bir uygulamanın parçası içinde aynı stili paylaşır bileşen hiyerarşisi akış için tema bilgileri belirtir.</span><span class="sxs-lookup"><span data-stu-id="8c629-374">In the following *Theme* example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="8c629-375">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="8c629-375">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="8c629-376">Üst bileşeni, geçişli değeri bileşenini kullanarak basamaklı bir değer sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c629-376">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="8c629-377">Geçişli değer bileşeni bileşen hiyerarşisi bir alt ağacı sarmalar ve o alt ağacı içinde tüm bileşenleri tek bir değer sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c629-377">The Cascading Value component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="8c629-378">Örneğin, örnek uygulamayı tema bilgileri belirtir (`ThemeInfo`) bir düzen gövdesini oluşturan tüm bileşenlerin basamaklı bir parametre olarak uygulamanın düzenleri `@Body` özelliği.</span><span class="sxs-lookup"><span data-stu-id="8c629-378">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="8c629-379">`ButtonClass` bir değeri atanır `btn-success` Düzen bileşende.</span><span class="sxs-lookup"><span data-stu-id="8c629-379">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="8c629-380">Bu özelliği üzerinden herhangi bir alt bileşen tüketebileceği `ThemeInfo` basamaklı nesnesi.</span><span class="sxs-lookup"><span data-stu-id="8c629-380">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="8c629-381">*Basamaklı değerler parametreleri Düzen bileşen*:</span><span class="sxs-lookup"><span data-stu-id="8c629-381">*Cascading Values Parameters Layout component*:</span></span>

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

@functions {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="8c629-382">Yapmak için geçişli değerlerini kullanmak, bileşenlerini kullanarak basamaklı parametreleri bildirmek `[CascadingParameter]` özniteliği veya bir dize adı değerine göre:</span><span class="sxs-lookup"><span data-stu-id="8c629-382">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

<span data-ttu-id="8c629-383">Bir dize adı değeri bağlamayla aynı türde birden fazla basamaklı değerler varsa ve aynı alt ağacı içinde ayrılmaları gerekiyorsa geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8c629-383">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="8c629-384">Basamaklı değerler türüne göre geçişli parametrelerine bağlı.</span><span class="sxs-lookup"><span data-stu-id="8c629-384">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="8c629-385">Örnek uygulamada, geçişli değerleri parametreleri tema bileşen bağlar `ThemeInfo` basamaklı basamaklı bir parametre değeri.</span><span class="sxs-lookup"><span data-stu-id="8c629-385">In the sample app, the Cascading Values Parameters Theme component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="8c629-386">Parametre için bir bileşen tarafından görüntülenen düğmeleri CSS sınıfının ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8c629-386">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="8c629-387">*Basamaklı değerler parametreleri tema bileşen*:</span><span class="sxs-lookup"><span data-stu-id="8c629-387">*Cascading Values Parameters Theme component*:</span></span>

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@functions {
    private int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a><span data-ttu-id="8c629-388">TabSet örneği</span><span class="sxs-lookup"><span data-stu-id="8c629-388">TabSet example</span></span>

<span data-ttu-id="8c629-389">Geçişli parametreleri, bileşen hiyerarşi işbirliği yapmak bileşenler de olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="8c629-389">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="8c629-390">Örneğin, aşağıdakileri dikkate alın *TabSet* örnek uygulamada örnek.</span><span class="sxs-lookup"><span data-stu-id="8c629-390">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="8c629-391">Örnek uygulamanın bir `ITab` uygulama sekme arabirimi:</span><span class="sxs-lookup"><span data-stu-id="8c629-391">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="8c629-392">Basamaklı değerler parametreleri TabSet bileşen birden fazla sekme bileşenleri içeren sekmesinin bileşeni kullanır:</span><span class="sxs-lookup"><span data-stu-id="8c629-392">The Cascading Values Parameters TabSet component uses the Tab Set component, which contains several Tab components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="8c629-393">Alt sekmesinde bileşenleri sekmesini ayarlamak için parametre olarak açıkça geçirilen değildir.</span><span class="sxs-lookup"><span data-stu-id="8c629-393">The child Tab components aren't explicitly passed as parameters to the Tab Set.</span></span> <span data-ttu-id="8c629-394">Bunun yerine, alt sekmesinde bileşenleri alt içeriğin sekme kümesi'nin parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="8c629-394">Instead, the child Tab components are part of the child content of the Tab Set.</span></span> <span data-ttu-id="8c629-395">Ancak sekme kümesi yine de üst bilgiler ve etkin sekmede işleyebilir, böylece her sekme bileşeni hakkında bilmesi gerekir. Ek kod, sekme kümesini bileşen gerek kalmadan bu koordinasyon sağlamak *kendisini basamaklı bir değer sağlayabilirsiniz* , ardından alındığından bloğun sekmesini bileşenleri tarafından.</span><span class="sxs-lookup"><span data-stu-id="8c629-395">However, the Tab Set still needs to know about each Tab component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the Tab Set component *can provide itself as a cascading value* that is then picked up by the descendent Tab components.</span></span>

<span data-ttu-id="8c629-396">*TabSet bileşen*:</span><span class="sxs-lookup"><span data-stu-id="8c629-396">*TabSet component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="8c629-397">Descendent sekmesini bileşenleri yakalama sekmesini içeren ayarlayın basamaklı bir parametre olarak sekmesini ayarlamak ve koordinat sekmesini bileşenleri kendilerini ekleme hangi sekmesinde olduğundan etkin.</span><span class="sxs-lookup"><span data-stu-id="8c629-397">The descendent Tab components capture the containing Tab Set as a cascading parameter, so the Tab components add themselves to the Tab Set and coordinate on which tab is active.</span></span>

<span data-ttu-id="8c629-398">*Sekme bileşen*:</span><span class="sxs-lookup"><span data-stu-id="8c629-398">*Tab component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="8c629-399">Razor şablonları</span><span class="sxs-lookup"><span data-stu-id="8c629-399">Razor templates</span></span>

<span data-ttu-id="8c629-400">İşleme parçaları, Razor şablon söz dizimi kullanılarak tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-400">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="8c629-401">Razor şablonları UI parçacık tanımlayın ve aşağıdaki biçimi varsayar için bir yol sağlar:</span><span class="sxs-lookup"><span data-stu-id="8c629-401">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="8c629-402">Aşağıdaki örnek nasıl belirtileceğini göstermektedir `RenderFragment` ve `RenderFragment<T>` değerleri.</span><span class="sxs-lookup"><span data-stu-id="8c629-402">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values.</span></span>

<span data-ttu-id="8c629-403">*Razor şablonları bileşen*:</span><span class="sxs-lookup"><span data-stu-id="8c629-403">*Razor Templates component*:</span></span>

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

<span data-ttu-id="8c629-404">Şablonları şablonlu bileşenleri için bağımsız değişken olarak geçirilen veya doğrudan işlenmiş Razor kullanılarak tanımlanan parçalarının işleyin.</span><span class="sxs-lookup"><span data-stu-id="8c629-404">Render fragments defined using Razor templates can be passed as arguments to templated components or rendered directly.</span></span> <span data-ttu-id="8c629-405">Örneğin, önceki şablonları aşağıdaki Razor işaretlemesi ile doğrudan işlenir:</span><span class="sxs-lookup"><span data-stu-id="8c629-405">For example, the previous templates are directly rendered with the following Razor markup:</span></span>

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

<span data-ttu-id="8c629-406">İşlenmiş çıkışı:</span><span class="sxs-lookup"><span data-stu-id="8c629-406">Rendered output:</span></span>

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="8c629-407">El ile RenderTreeBuilder mantığı</span><span class="sxs-lookup"><span data-stu-id="8c629-407">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="8c629-408">`Microsoft.AspNetCore.Components.RenderTree` bileşenleri ve bileşenleri el ile oluşturma da dahil olmak üzere öğeleri işlemek için yöntemler sağlar C# kod.</span><span class="sxs-lookup"><span data-stu-id="8c629-408">`Microsoft.AspNetCore.Components.RenderTree` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="8c629-409">Kullanım `RenderTreeBuilder` bileşenler oluşturmak için Gelişmiş bir senaryodur.</span><span class="sxs-lookup"><span data-stu-id="8c629-409">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="8c629-410">Hatalı biçimlendirilmiş bir bileşen (örneğin, bir kapatılmamış biçimlendirme etiketi) tanımsız davranışlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-410">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="8c629-411">Başka bir bileşene dönüştürerek el ile oluşturulabilir aşağıdaki evcil hayvan ayrıntıları bileşeni göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8c629-411">Consider the following Pet Details component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@functions
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="8c629-412">Aşağıdaki örnekte, bir döngüde `CreateComponent` yöntem üç evcil hayvan ayrıntıları bileşeni oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8c629-412">In the following example, the loop in the `CreateComponent` method generates three Pet Details components.</span></span> <span data-ttu-id="8c629-413">Çağrılırken `RenderTreeBuilder` bileşenler oluşturmak için yöntemleri (`OpenComponent` ve `AddAttribute`), sıra numaraları olan kaynak kodu satır numaraları.</span><span class="sxs-lookup"><span data-stu-id="8c629-413">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="8c629-414">Kod, değil ayrı çağrı çağrılarını ayrı satırlara karşılık gelen sıra numaraları Blazor fark algoritması kullanır.</span><span class="sxs-lookup"><span data-stu-id="8c629-414">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="8c629-415">Bir bileşen ile oluştururken `RenderTreeBuilder` yöntemleri, sabit kodlamayın seri numaraları için bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="8c629-415">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="8c629-416">**Sıra numarası oluşturmak için bir hesaplama veya sayaç kullanarak düşük performansa neden olabilir.**</span><span class="sxs-lookup"><span data-stu-id="8c629-416">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="8c629-417">Daha fazla bilgi için [sıra numaraları ilişkilendirmek için kod satır numaraları ve değil yürütme sırası](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) bölümü.</span><span class="sxs-lookup"><span data-stu-id="8c629-417">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="8c629-418">*İçerik bileşen yerleşik*:</span><span class="sxs-lookup"><span data-stu-id="8c629-418">*Built Content component*:</span></span>

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" onclick="@RenderComponent">
    Create three Pet Details components
</button>

@functions {
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

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="8c629-419">Seri numaraları için kod satır numaraları ve değil yürütme sırası arasında bir ilişki</span><span class="sxs-lookup"><span data-stu-id="8c629-419">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="8c629-420">Blazor `.razor` dosyaları her zaman derlenir.</span><span class="sxs-lookup"><span data-stu-id="8c629-420">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="8c629-421">Bu büyük olasılıkla için harika bir avantajı, `.razor` çünkü derleme adımı çalışma zamanında uygulama performansını bilgi eklenmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-421">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="8c629-422">Bu geliştirmeler anahtar bir örnek içeren *sıra numaraları*.</span><span class="sxs-lookup"><span data-stu-id="8c629-422">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="8c629-423">Sıra numaraları, hangi kod ayrı ve sıralı satırlarından çıkışları gelen çalışma zamanı gösterir.</span><span class="sxs-lookup"><span data-stu-id="8c629-423">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="8c629-424">Çalışma zamanı, genel ağaç fark algoritması için normalde mümkün olandan çok daha hızlı doğrusal zamanda verimli ağaç farkları oluşturmak için bu bilgileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="8c629-424">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="8c629-425">Aşağıdakileri göz önünde bulundurun basit `.razor` dosyası:</span><span class="sxs-lookup"><span data-stu-id="8c629-425">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="8c629-426">Bu, aşağıdakine benzer bir şey derler:</span><span class="sxs-lookup"><span data-stu-id="8c629-426">This compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="8c629-427">Ne zaman bu kodu yürütür, ilk kez, `someFlag` olduğu `true`, oluşturucu alır:</span><span class="sxs-lookup"><span data-stu-id="8c629-427">When this code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="8c629-428">Sequence</span><span class="sxs-lookup"><span data-stu-id="8c629-428">Sequence</span></span> | <span data-ttu-id="8c629-429">Tür</span><span class="sxs-lookup"><span data-stu-id="8c629-429">Type</span></span>      | <span data-ttu-id="8c629-430">Veri</span><span class="sxs-lookup"><span data-stu-id="8c629-430">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="8c629-431">0</span><span class="sxs-lookup"><span data-stu-id="8c629-431">0</span></span>        | <span data-ttu-id="8c629-432">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="8c629-432">Text node</span></span> | <span data-ttu-id="8c629-433">ilk</span><span class="sxs-lookup"><span data-stu-id="8c629-433">First</span></span>  |
| <span data-ttu-id="8c629-434">1.</span><span class="sxs-lookup"><span data-stu-id="8c629-434">1</span></span>        | <span data-ttu-id="8c629-435">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="8c629-435">Text node</span></span> | <span data-ttu-id="8c629-436">Saniye</span><span class="sxs-lookup"><span data-stu-id="8c629-436">Second</span></span> |

<span data-ttu-id="8c629-437">Şimdi, Imagine `someFlag` olur `false`, ve yeniden oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8c629-437">Now imagine that `someFlag` becomes `false`, and we render again.</span></span> <span data-ttu-id="8c629-438">Bu kez, oluşturucu alır:</span><span class="sxs-lookup"><span data-stu-id="8c629-438">This time, the builder receives:</span></span>

| <span data-ttu-id="8c629-439">Sequence</span><span class="sxs-lookup"><span data-stu-id="8c629-439">Sequence</span></span> | <span data-ttu-id="8c629-440">Tür</span><span class="sxs-lookup"><span data-stu-id="8c629-440">Type</span></span>       | <span data-ttu-id="8c629-441">Veri</span><span class="sxs-lookup"><span data-stu-id="8c629-441">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="8c629-442">1.</span><span class="sxs-lookup"><span data-stu-id="8c629-442">1</span></span>        | <span data-ttu-id="8c629-443">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="8c629-443">Text node</span></span>  | <span data-ttu-id="8c629-444">Saniye</span><span class="sxs-lookup"><span data-stu-id="8c629-444">Second</span></span> |

<span data-ttu-id="8c629-445">Çalışma zamanı bir fark gerçekleştirdiğinde, görür öğe dizisi `0` kaldırıldı, aşağıdaki Önemsiz oluşturmasını sağlayacak şekilde *betiğini Düzenle*:</span><span class="sxs-lookup"><span data-stu-id="8c629-445">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="8c629-446">İlk metin düğümü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="8c629-446">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="8c629-447">Sıra numaraları programlı olarak oluşturursanız yanlış unsurları</span><span class="sxs-lookup"><span data-stu-id="8c629-447">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="8c629-448">Bunun yerine aşağıdaki rendertree Oluşturucu mantığı yazdığınız varsayalım:</span><span class="sxs-lookup"><span data-stu-id="8c629-448">Imagine instead that you wrote the following rendertree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="8c629-449">Artık ilk çıktı aşağıdaki gibi olur:</span><span class="sxs-lookup"><span data-stu-id="8c629-449">Now the first output would be:</span></span>

<span data-ttu-id="8c629-450">| Dizisi | Tür | Veri || :------: | --------- | :--- : | | 0 | Metin düğümü | İlk || 1 | Metin düğümü | İkinci |</span><span class="sxs-lookup"><span data-stu-id="8c629-450">| Sequence | Type      | Data   | | :------: | --------- | :--- : | | 0        | Text node | First  | | 1        | Text node | Second |</span></span>

<span data-ttu-id="8c629-451">Bu sonuç için önceki durum, aynı olduğundan negatif hiçbir sorun yoktur.</span><span class="sxs-lookup"><span data-stu-id="8c629-451">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="8c629-452">İkinci işleme `someFlag` olduğu `false`, çıktı şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="8c629-452">On the second render when `someFlag` is `false`, the output is:</span></span>

| <span data-ttu-id="8c629-453">Sequence</span><span class="sxs-lookup"><span data-stu-id="8c629-453">Sequence</span></span> | <span data-ttu-id="8c629-454">Tür</span><span class="sxs-lookup"><span data-stu-id="8c629-454">Type</span></span>      | <span data-ttu-id="8c629-455">Veri</span><span class="sxs-lookup"><span data-stu-id="8c629-455">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="8c629-456">0</span><span class="sxs-lookup"><span data-stu-id="8c629-456">0</span></span>        | <span data-ttu-id="8c629-457">Metin düğümü</span><span class="sxs-lookup"><span data-stu-id="8c629-457">Text node</span></span> | <span data-ttu-id="8c629-458">Saniye</span><span class="sxs-lookup"><span data-stu-id="8c629-458">Second</span></span> |

<span data-ttu-id="8c629-459">Fark algoritması, bu kez görür *iki* değişiklikleri oluşmuş ve algoritma aşağıdaki düzenleme komut dosyası oluşturur:</span><span class="sxs-lookup"><span data-stu-id="8c629-459">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="8c629-460">İlk metin düğümü değiştirin `Second`.</span><span class="sxs-lookup"><span data-stu-id="8c629-460">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="8c629-461">İkinci metin düğümü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="8c629-461">Remove the second text node.</span></span>

<span data-ttu-id="8c629-462">Sıra numaraları oluşturma tüm hakkında yararlı bilgiler nerede kaybetti `if/else` dallar ve döngüler özgün koda mevcut.</span><span class="sxs-lookup"><span data-stu-id="8c629-462">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="8c629-463">Bu bir fark sonuçlarını **iki kez sürece** önceki gibi.</span><span class="sxs-lookup"><span data-stu-id="8c629-463">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="8c629-464">Bu basit bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="8c629-464">This is a trivial example.</span></span> <span data-ttu-id="8c629-465">Karmaşık ve iç içe yapılar ve özellikle döngüler daha gerçekçi durumda da, performans maliyeti daha ciddi.</span><span class="sxs-lookup"><span data-stu-id="8c629-465">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="8c629-466">Hemen hangi döngü blokları veya dallar eklenen veya kaldırılan tanımlamak yerine, derin bir şekilde işleme ağaçlara recurse ve onu hakkında misinformed çünkü genellikle derleme çok uzun düzenleme betiklerini fark algoritmasına sahip eski ve yeni yapılar birbiriyle ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="8c629-466">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="8c629-467">Yönergeler ve sonuçları</span><span class="sxs-lookup"><span data-stu-id="8c629-467">Guidance and conclusions</span></span>

* <span data-ttu-id="8c629-468">Sıra numaraları dinamik olarak üretilen uygulama performans düşebilir.</span><span class="sxs-lookup"><span data-stu-id="8c629-468">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="8c629-469">Framework'te derleme zamanında yakalanır sürece gerekli bilgileri mevcut olmadığından kendi sıra numaraları çalışma zamanında otomatik olarak oluşturulamıyor.</span><span class="sxs-lookup"><span data-stu-id="8c629-469">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="8c629-470">El ile uygulanan uzun bloklarını yazmayın `RenderTreeBuilder` mantığı.</span><span class="sxs-lookup"><span data-stu-id="8c629-470">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="8c629-471">Tercih ettiğiniz `.razor` dosyaları ve sıra numaraları ile dağıtılacak derleyici sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c629-471">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span>
* <span data-ttu-id="8c629-472">Sıra numaraları sabittir, fark algoritması yalnızca bir sıra numaraları değeri artırmak gerektirir.</span><span class="sxs-lookup"><span data-stu-id="8c629-472">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="8c629-473">İlk değer ve boşluklar önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="8c629-473">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="8c629-474">Yasal bir seçenek olan sıra numarası kod satır numarasını kullanın veya sıfırdan başlayın ve olanlara ya da yüz artırmak için (veya herhangi bir tercih edilen aralığı).</span><span class="sxs-lookup"><span data-stu-id="8c629-474">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="8c629-475">Ağaç ayırırken diğer UI çerçeveleri bunları kullanmayın Blazor seri numaraları kullanır.</span><span class="sxs-lookup"><span data-stu-id="8c629-475">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="8c629-476">Sıra numaraları kullanılır ve Blazor sıra numaraları ile otomatik olarak yazma geliştiriciler için ilgilenen bir derleme adımı avantajlarından fark alma işlemini çok daha hızlı `.razor` dosyaları.</span><span class="sxs-lookup"><span data-stu-id="8c629-476">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>
