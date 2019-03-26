---
title: Oluşturma ve Razor bileşenleri kullanma
author: guardrex
description: Oluşturma ve Razor bileşenler, bileşen ömürleri yönetme verilere bağlayın ve olayları işlemek nasıl dahil olmak üzere kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2019
uid: razor-components/components
ms.openlocfilehash: 59c8540ea297f8396d6aac9b3246639667ad0cd7
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488682"
---
# <a name="create-and-use-razor-components"></a><span data-ttu-id="5222a-103">Oluşturma ve Razor bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="5222a-103">Create and use Razor Components</span></span>

<span data-ttu-id="5222a-104">Tarafından [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), ve [Morné Zaayman](https://github.com/MorneZaayman)</span><span class="sxs-lookup"><span data-stu-id="5222a-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Morné Zaayman](https://github.com/MorneZaayman)</span></span>

<span data-ttu-id="5222a-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="5222a-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="5222a-106">Bkz: [başlama](xref:razor-components/get-started) Önkoşullar için konu.</span><span class="sxs-lookup"><span data-stu-id="5222a-106">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="5222a-107">Razor bileşenleri uygulamaları kullanılarak oluşturulur *bileşenleri*.</span><span class="sxs-lookup"><span data-stu-id="5222a-107">Razor Components apps are built using *components*.</span></span> <span data-ttu-id="5222a-108">Bir bileşen, kullanıcı arabirimi (UI), sayfa, iletişim veya form gibi kendi içinde bir öbektir.</span><span class="sxs-lookup"><span data-stu-id="5222a-108">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="5222a-109">Bir bileşeni, HTML biçimlendirmesi ve veri ekleme veya UI olaylarına yanıt vermek için gereken işleme mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="5222a-109">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="5222a-110">Esnek ve basit bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="5222a-110">Components are flexible and lightweight.</span></span> <span data-ttu-id="5222a-111">Bunlar iç içe geçmiş, yeniden kullanılabilir ve projeler arasında paylaşılan.</span><span class="sxs-lookup"><span data-stu-id="5222a-111">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="5222a-112">Bileşen sınıfları</span><span class="sxs-lookup"><span data-stu-id="5222a-112">Component classes</span></span>

<span data-ttu-id="5222a-113">Bileşenleri Razor bileşen dosyaları genellikle uygulanır (*.razor*) bir birleşimi kullanılarak C# ve HTML biçimlendirmeyi (*.cshtml* dosyaları Blazor uygulamalarda kullanılır).</span><span class="sxs-lookup"><span data-stu-id="5222a-113">Components are typically implemented in Razor Component files (*.razor*) using a combination of C# and HTML markup (*.cshtml* files are used in Blazor apps).</span></span>

<span data-ttu-id="5222a-114">Bileşenlerini kullanarak Razor bileşenleri uygulamalarında yazılmış *.cshtml* dosyaları kullanarak Razor bileşen dosyaları tanımlanmış olduğu sürece dosya uzantısı `_RazorComponentInclude` MSBuild özelliği.</span><span class="sxs-lookup"><span data-stu-id="5222a-114">Components can be authored in Razor Components apps using the *.cshtml* file extension as long as the files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="5222a-115">Örneğin, Razor bileşen şablonu kullanılarak oluşturulan bir uygulamayı belirtir tüm *.cshtml* altında dosyaları *bileşenleri* klasör Razor bileşenleri dosyaları olarak kabul:</span><span class="sxs-lookup"><span data-stu-id="5222a-115">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components files:</span></span>

```xml
<_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
```

<span data-ttu-id="5222a-116">Bir bileşen için kullanıcı Arabirimi, HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="5222a-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="5222a-117">(Örneğin, döngü, koşullular, ifadeleri) dinamik işleme mantığı, katıştırılmış kullanarak eklenir C# adlı söz dizimi [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="5222a-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="5222a-118">Ne zaman bir Razor bileşenleri uygulama derlendiğinde, HTML biçimlendirmesi ve C# işleme mantığı, bir bileşen sınıfı dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="5222a-118">When a Razor Components app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="5222a-119">Oluşturulan sınıfın adı dosya adıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="5222a-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="5222a-120">Bileşen sınıfı üyeleri tanımlanmış bir `@functions` blok (birden fazla `@functions` bloğu izin verilen).</span><span class="sxs-lookup"><span data-stu-id="5222a-120">Members of the component class are defined in a `@functions` block (more than one `@functions` block is permissible).</span></span> <span data-ttu-id="5222a-121">İçinde `@functions` blok, bileşen durumu (Özellikler, alanlar), olay işleme için veya başka bir bileşen mantığı tanımlamak için yöntemlerle birlikte belirtilir.</span><span class="sxs-lookup"><span data-stu-id="5222a-121">In the `@functions` block, component state (properties, fields) is specified along with methods for event handling or for defining other component logic.</span></span>

<span data-ttu-id="5222a-122">Bileşen üyeleri, ardından bileşenin parçası mantığı kullanarak işleme olarak kullanılabilir C# ile başlayan ifadeleri `@`.</span><span class="sxs-lookup"><span data-stu-id="5222a-122">Component members can then be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="5222a-123">Örneğin, bir C# alan ekleyerek işlenen `@` alan adı.</span><span class="sxs-lookup"><span data-stu-id="5222a-123">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="5222a-124">Aşağıdaki örnek, değerlendirir ve işler:</span><span class="sxs-lookup"><span data-stu-id="5222a-124">The following example evaluates and renders:</span></span>

* <span data-ttu-id="5222a-125">`_headingFontStyle` CSS özellik değerini `font-style`.</span><span class="sxs-lookup"><span data-stu-id="5222a-125">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="5222a-126">`_headingText` içeriği için `<h1>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="5222a-126">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="5222a-127">Bileşen, bileşen başlangıçta işlenen sonra olaylara yanıt olarak, işleme ağacında yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5222a-127">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="5222a-128">Razor bileşenleri yeni bir işleme ağacı Öncekine karşı karşılaştırır ve herhangi bir değişiklik tarayıcının belge nesne modeli (DOM) için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="5222a-128">Razor Components then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

## <a name="using-components"></a><span data-ttu-id="5222a-129">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="5222a-129">Using components</span></span>

<span data-ttu-id="5222a-130">Bileşenleri, diğer bileşenlerin bunları bildirerek içerebilir HTML öğesi söz dizimini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="5222a-130">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="5222a-131">Bir bileşen kullanma için işaretleme, etiketin adını bileşen türü olduğu gibi HTML etiketleri arar.</span><span class="sxs-lookup"><span data-stu-id="5222a-131">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="5222a-132">Aşağıdaki biçimlendirmede işleyen bir `HeadingComponent` (*HeadingComponent.cshtml*) örneği:</span><span class="sxs-lookup"><span data-stu-id="5222a-132">The following markup renders a `HeadingComponent` (*HeadingComponent.cshtml*) instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?name=snippet_HeadingComponent)]

## <a name="component-parameters"></a><span data-ttu-id="5222a-133">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="5222a-133">Component parameters</span></span>

<span data-ttu-id="5222a-134">Bileşenleri olabilir *bileşeni parametreleri*, hangi kullanılarak tanımlanır *genel olmayan* bileşen sınıfı özellikleri düzenlenmiş ile `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="5222a-134">Components can have *component parameters*, which are defined using *non-public* properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="5222a-135">Öznitelikleri bir bileşen için bağımsız değişken biçimlendirme içinde belirtmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="5222a-135">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="5222a-136">Aşağıdaki örnekte, `ParentComponent` değerini ayarlar `Title` özelliği `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="5222a-136">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`:</span></span>

<span data-ttu-id="5222a-137">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5222a-137">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=5)]

<span data-ttu-id="5222a-138">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5222a-138">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a><span data-ttu-id="5222a-139">Alt içeriğin</span><span class="sxs-lookup"><span data-stu-id="5222a-139">Child content</span></span>

<span data-ttu-id="5222a-140">Bileşenleri başka bir bileşen içeriğini ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5222a-140">Components can set the content of another component.</span></span> <span data-ttu-id="5222a-141">Atama bileşen alıcı bileşeni belirtin etiketleri arasında içerik sağlar.</span><span class="sxs-lookup"><span data-stu-id="5222a-141">The assigning component provides the content between the tags that specify the receiving component.</span></span> <span data-ttu-id="5222a-142">Örneğin, bir `ParentComponent` içerik işleme için bir alt bileşen tarafından içerik içine yerleştirerek sağlayabilir `<ChildComponent>` etiketler.</span><span class="sxs-lookup"><span data-stu-id="5222a-142">For example, a `ParentComponent` can provide content for rendering by a Child component by placing the content inside `<ChildComponent>` tags.</span></span>

<span data-ttu-id="5222a-143">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5222a-143">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=6-7)]

<span data-ttu-id="5222a-144">Alt bileşen bir `ChildContent` temsil eden özellik bir `RenderFragment`.</span><span class="sxs-lookup"><span data-stu-id="5222a-144">The Child component has a `ChildContent` property that represents a `RenderFragment`.</span></span> <span data-ttu-id="5222a-145">Değerini `ChildContent` alt bileşen işaretlemede içerik nerede oluşturulması gerekip konumlandırıldı.</span><span class="sxs-lookup"><span data-stu-id="5222a-145">The value of `ChildContent` is positioned in the child component's markup where the content should be rendered.</span></span> <span data-ttu-id="5222a-146">Aşağıdaki örnekte, değerini `ChildContent` ana bileşenden alınan ve önyükleme bölmenin içinde işlenen `panel-body`.</span><span class="sxs-lookup"><span data-stu-id="5222a-146">In the following example, the value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="5222a-147">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5222a-147">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> <span data-ttu-id="5222a-148">Özellik Alma `RenderFragment` içeriği adlı `ChildContent` kural tarafından.</span><span class="sxs-lookup"><span data-stu-id="5222a-148">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

## <a name="data-binding"></a><span data-ttu-id="5222a-149">Veri bağlama</span><span class="sxs-lookup"><span data-stu-id="5222a-149">Data binding</span></span>

<span data-ttu-id="5222a-150">Veri bağlama bileşenleri hem DOM öğeleri ile gerçekleştirilir `bind` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5222a-150">Data binding to both components and DOM elements is accomplished with the `bind` attribute.</span></span> <span data-ttu-id="5222a-151">Aşağıdaki örnek bağlar `ItalicsCheck` özelliğini onay kutusunun işaretli durumu:</span><span class="sxs-lookup"><span data-stu-id="5222a-151">The following example binds the `ItalicsCheck` property to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    bind="@_italicsCheck" />
```

<span data-ttu-id="5222a-152">Onay kutusunu işaretli ve seçildiğinde özelliğin değerini şekilde güncelleştirilir `true` ve `false`sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="5222a-152">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="5222a-153">Yalnızca bileşen, özelliğin değerinin değiştirilmesi için değil yanıt oluşturulduğunda onay kutusunu kullanıcı Arabiriminde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5222a-153">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="5222a-154">Olay işleyici kodu yürütüldükten sonra bileşenleri kendilerini işleme olduğundan, özellik güncelleştirmeleri genellikle kullanıcı Arabiriminde hemen yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="5222a-154">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="5222a-155">Kullanarak `bind` ile bir `CurrentValue` özelliği (`<input bind="@CurrentValue" />`) aslında aşağıdakine eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="5222a-155">Using `bind` with a `CurrentValue` property (`<input bind="@CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="5222a-156">Bileşen işlendiğinde `value` giriş öğesinin geldiği `CurrentValue` özelliği.</span><span class="sxs-lookup"><span data-stu-id="5222a-156">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="5222a-157">Kullanıcı, metin kutusuna yazdığında `onchange` olay tetiklenir ve `CurrentValue` özelliği değiştirilmiş değerine ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="5222a-157">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="5222a-158">Gerçekte, kod oluşturma biraz daha karmaşık olduğundan `bind` tür dönüştürmeleri gerçekleştirildiği birkaç durum işler.</span><span class="sxs-lookup"><span data-stu-id="5222a-158">In reality, the code generation is a little more complex because `bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="5222a-159">Giren İlkesi `bind` geçerli değerini bir ifade ile ilişkilendirir bir `value` kayıtlı işleyici kullanarak öznitelik ve işleyicilerini değişiklikler.</span><span class="sxs-lookup"><span data-stu-id="5222a-159">In principle, `bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="5222a-160">Ek olarak `onchange`, özelliği gibi diğer olayları kullanarak bağlanabilir `oninput` bağlamak gerekenler hakkında daha fazla açık olan tarafından:</span><span class="sxs-lookup"><span data-stu-id="5222a-160">In addition to `onchange`, the property can be bound using other events like `oninput` by being more explicit about what to bind to:</span></span>

```cshtml
<input type="text" bind-value-oninput="@CurrentValue" />
```

<span data-ttu-id="5222a-161">Farklı `onchange`, `oninput` metin kutusuna giriş her karakter için ateşlenir.</span><span class="sxs-lookup"><span data-stu-id="5222a-161">Unlike `onchange`, `oninput` fires for every character that is input into the text box.</span></span>

<span data-ttu-id="5222a-162">**Biçim dizeleri**</span><span class="sxs-lookup"><span data-stu-id="5222a-162">**Format strings**</span></span>

<span data-ttu-id="5222a-163">Veri bağlama ile birlikte çalışır <xref:System.DateTime> biçim dizeleri.</span><span class="sxs-lookup"><span data-stu-id="5222a-163">Data binding works with <xref:System.DateTime> format strings.</span></span> <span data-ttu-id="5222a-164">Para birimi veya sayı biçimleri gibi diğer biçim ifadeleri şu anda kullanılamıyor.</span><span class="sxs-lookup"><span data-stu-id="5222a-164">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd" />

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="5222a-165">`format-value` Özniteliği uygulamak için tarih biçimini belirtir `value` , `input` öğesi.</span><span class="sxs-lookup"><span data-stu-id="5222a-165">The `format-value` attribute specifies the date format to apply to the `value` of the `input` element.</span></span> <span data-ttu-id="5222a-166">Biçimi de değer ayrıştırmak için kullanılan zaman bir `onchange` olayı oluşur.</span><span class="sxs-lookup"><span data-stu-id="5222a-166">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="5222a-167">**Bileşen parametreleri**</span><span class="sxs-lookup"><span data-stu-id="5222a-167">**Component parameters**</span></span>

<span data-ttu-id="5222a-168">Bağlama bileşeni parametreleri de tanır burada `bind-{property}` bir özellik değeri bileşenlerinde bağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5222a-168">Binding also recognizes component parameters, where `bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="5222a-169">Aşağıdaki bileşen `ChildComponent` ve bağlar `ParentYear` üst parametresinden `Year` alt bileşen parametresi:</span><span class="sxs-lookup"><span data-stu-id="5222a-169">The following component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

<span data-ttu-id="5222a-170">Ana bileşenin:</span><span class="sxs-lookup"><span data-stu-id="5222a-170">Parent component:</span></span>

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

    void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="5222a-171">Alt bileşeni:</span><span class="sxs-lookup"><span data-stu-id="5222a-171">Child component:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private Action<int> YearChanged { get; set; }
}
```

<span data-ttu-id="5222a-172">Yükleme `ParentComponent` aşağıdaki biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="5222a-172">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="5222a-173">Varsa değerini `ParentYear` düğmesini seçerek özelliği değiştirildiğinde `ParentComponent`, `Year` özelliği `ChildComponent` güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5222a-173">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="5222a-174">Öğesinin yeni değeri `Year` Arabiriminde işlenen olduğunda `ParentComponent` rerendered:</span><span class="sxs-lookup"><span data-stu-id="5222a-174">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="5222a-175">`Year` Bir yardımcı olduğundan parametre bağlanabilir `YearChanged` türüyle eşleşen olay `Year` parametresi.</span><span class="sxs-lookup"><span data-stu-id="5222a-175">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="5222a-176">Kural olarak, `<ChildComponent bind-Year="@ParentYear" />` yazmak, temelde eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="5222a-176">By convention, `<ChildComponent bind-Year="@ParentYear" />` is essentially equivalent to writing,</span></span>

```cshtml
    <ChildComponent bind-Year-YearChanged="@ParentYear" />
```

<span data-ttu-id="5222a-177">Genel olarak, bir özelliği karşılık gelen olay işleyicisi kullanarak bir bağlanabilir `bind-property-event` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5222a-177">In general, a property can be bound to a corresponding event handler using `bind-property-event` attribute.</span></span>

## <a name="event-handling"></a><span data-ttu-id="5222a-178">Olay işleme</span><span class="sxs-lookup"><span data-stu-id="5222a-178">Event handling</span></span>

<span data-ttu-id="5222a-179">Razor bileşenleri olay işleme özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="5222a-179">Razor Components provide event handling features.</span></span> <span data-ttu-id="5222a-180">İçin bir HTML öğesi öznitelik adlı `on<event>` (örneğin, `onclick`, `onsubmit`) temsilci türü belirtilmiş bir değer ile Razor bileşenleri özniteliğinin değeri bir olay işleyicisi değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="5222a-180">For an HTML element attribute named `on<event>` (for example, `onclick`, `onsubmit`) with a delegate-typed value, Razor Components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="5222a-181">Özniteliğin adı her zaman ile başlayan `on`.</span><span class="sxs-lookup"><span data-stu-id="5222a-181">The attribute's name always starts with `on`.</span></span>

<span data-ttu-id="5222a-182">Aşağıdaki kod çağrıları `UpdateHeading` Arabiriminde düğme seçildiğinde yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5222a-182">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="5222a-183">Aşağıdaki kod çağrıları `CheckboxChanged` onay kutusunu kullanıcı Arabiriminde değiştirildiğinde yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5222a-183">The following code calls the `CheckboxChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged" />

@functions {
    void CheckboxChanged()
    {
        ...
    }
}
```

<span data-ttu-id="5222a-184">Olay işleyicileri zaman uyumsuz ve dönüş ayrıca olabilir bir <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="5222a-184">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="5222a-185">El ile çağırmaya gerek yoktur `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="5222a-185">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="5222a-186">Ortaya çıkan özel durumlar günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5222a-186">Exceptions are logged when they occur.</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="5222a-187">Bazı olaylar için olaya özgü olay bağımsız değişken türleri de izin verilir.</span><span class="sxs-lookup"><span data-stu-id="5222a-187">For some events, event-specific event argument types are permitted.</span></span> <span data-ttu-id="5222a-188">Bu olay türleri birine erişimi gerekli değilse, yöntem çağrısında gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="5222a-188">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="5222a-189">Desteklenen olay bağımsız değişkenleri listesi verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="5222a-189">The list of supported event arguments is:</span></span>

* <span data-ttu-id="5222a-190">UIEventArgs</span><span class="sxs-lookup"><span data-stu-id="5222a-190">UIEventArgs</span></span>
* <span data-ttu-id="5222a-191">UIChangeEventArgs</span><span class="sxs-lookup"><span data-stu-id="5222a-191">UIChangeEventArgs</span></span>
* <span data-ttu-id="5222a-192">UIKeyboardEventArgs</span><span class="sxs-lookup"><span data-stu-id="5222a-192">UIKeyboardEventArgs</span></span>
* <span data-ttu-id="5222a-193">UIMouseEventArgs</span><span class="sxs-lookup"><span data-stu-id="5222a-193">UIMouseEventArgs</span></span>

<span data-ttu-id="5222a-194">Lambda ifadeleri de kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="5222a-194">Lambda expressions can also be used:</span></span>

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="5222a-195">Genellikle gibi ek değerler kapatmak uygun olan öğeleri kümesi yineleme olduğunda.</span><span class="sxs-lookup"><span data-stu-id="5222a-195">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="5222a-196">Aşağıdaki örnek, üç oluşturur düğmeler, her biri çağıran `UpdateHeading` olay bağımsız değişken geçirme (`UIMouseEventArgs`) ve düğme sayısı (`buttonNumber`) kullanıcı Arabiriminde seçili olduğunda:</span><span class="sxs-lookup"><span data-stu-id="5222a-196">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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
    string message = "Select a button to learn its position.";

    void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            "mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="5222a-197">Yapmak **değil** Döngü değişkeninin kullanın (`i`) içinde bir `for` bir lambda ifadesinde doğrudan döngü.</span><span class="sxs-lookup"><span data-stu-id="5222a-197">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="5222a-198">Aksi takdirde aynı değişkene neden tüm lambda ifadeleri tarafından kullanılan `i`değerini tüm lambda içinde ile aynı.</span><span class="sxs-lookup"><span data-stu-id="5222a-198">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="5222a-199">Yerel bir değişkende değeri her zaman yakalama (`buttonNumber` önceki örnekte) ve ardından kullanın.</span><span class="sxs-lookup"><span data-stu-id="5222a-199">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="5222a-200">Bileşenleri başvurular yakalama</span><span class="sxs-lookup"><span data-stu-id="5222a-200">Capture references to components</span></span>

<span data-ttu-id="5222a-201">Bileşen başvuruları komutları gibi bu örneğe verebilir böylece bu şekilde get bileşen örneğe bir başvuru sağlar `Show` veya `Reset`.</span><span class="sxs-lookup"><span data-stu-id="5222a-201">Component references provide a way get a reference to a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="5222a-202">Bir bileşen başvurusunu yakalamak için ekleme bir `ref` özniteliği alt bileşen ve aynı ada ve aynı türe sahip bir alan alt bileşeni olarak tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5222a-202">To capture a component reference, add a `ref` attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    MyLoginDialog loginDialog;

    void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="5222a-203">Bileşen işlendiğinde `loginDialog` alanı ile doldurulur `MyLoginDialog` alt bileşen örneği.</span><span class="sxs-lookup"><span data-stu-id="5222a-203">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="5222a-204">Ardından bileşen örneği üzerinde .NET yöntemlerini çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5222a-204">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5222a-205">`loginDialog` Değişkeni bileşeni işlenir ve çıktısını içeren sonra yalnızca doldurulmuş `MyLoginDialog` öğesi.</span><span class="sxs-lookup"><span data-stu-id="5222a-205">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="5222a-206">O noktaya kadar hiçbir şey yoktur başvurmak için.</span><span class="sxs-lookup"><span data-stu-id="5222a-206">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="5222a-207">Bileşen oluşturma işlemini tamamladıktan sonra bileşenleri başvurularını değiştirmek üzere `OnAfterRenderAsync` veya `OnAfterRender` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="5222a-207">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="5222a-208">Bileşen başvurularını yakalama için benzer bir sözdizimi kullanırken [öğesi başvuruları yakalama](xref:razor-components/javascript-interop#capture-references-to-elements), öyle bir [JavaScript birlikte çalışma](xref:razor-components/javascript-interop) özelliği.</span><span class="sxs-lookup"><span data-stu-id="5222a-208">While capturing component references uses a similar syntax to [capturing element references](xref:razor-components/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:razor-components/javascript-interop) feature.</span></span> <span data-ttu-id="5222a-209">Bileşen başvurularını JavaScript koduna geçen değildir; Bunlar, yalnızca .NET kodda kullanılırlar.</span><span class="sxs-lookup"><span data-stu-id="5222a-209">Component references aren't passed to JavaScript code; they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="5222a-210">Yapmak **değil** alt bileşenlerin durumunu kesilecek bileşen başvuruları kullanın.</span><span class="sxs-lookup"><span data-stu-id="5222a-210">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="5222a-211">Bunun yerine, normal bildirim temelli parametreler alt bileşenler için veri aktarmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="5222a-211">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="5222a-212">Bu alt bileşenleri otomatik olarak doğru zamanlarda rerender neden olur.</span><span class="sxs-lookup"><span data-stu-id="5222a-212">This causes child components to rerender at the correct times automatically.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="5222a-213">Yaşam döngüsü yöntemleri</span><span class="sxs-lookup"><span data-stu-id="5222a-213">Lifecycle methods</span></span>

<span data-ttu-id="5222a-214">`OnInitAsync` ve `OnInit` bileşeni başlatmak için kod yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="5222a-214">`OnInitAsync` and `OnInit` execute code to initialize the component.</span></span> <span data-ttu-id="5222a-215">Zaman uyumsuz bir işlemi gerçekleştirmek için `OnInitAsync` ve `await` anahtar sözcüğü, işlemi:</span><span class="sxs-lookup"><span data-stu-id="5222a-215">To perform an asynchronous operation, use `OnInitAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="5222a-216">Eşzamanlı bir işlem için kullanmak `OnInit`:</span><span class="sxs-lookup"><span data-stu-id="5222a-216">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

<span data-ttu-id="5222a-217">`OnParametersSetAsync` ve `OnParametersSet` bir bileşen üst öğesinden parametreleri aldı ve özelliklerine değerler atanır çağırılır.</span><span class="sxs-lookup"><span data-stu-id="5222a-217">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="5222a-218">Bu yöntemler bileşen başlatmadan sonra yürütülür ve her bileşenin sonra işlenir:</span><span class="sxs-lookup"><span data-stu-id="5222a-218">These methods are executed after component initialization and then each time the component is rendered:</span></span>

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

<span data-ttu-id="5222a-219">`OnAfterRenderAsync` ve `OnAfterRender` bir bileşen oluşturma tamamlandıktan sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5222a-219">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="5222a-220">Öğe ve bileşen başvuruları bu noktada doldurulur.</span><span class="sxs-lookup"><span data-stu-id="5222a-220">Element and component references are populated at this point.</span></span> <span data-ttu-id="5222a-221">Bu aşama, işlenmiş DOM öğeleri üzerinde çalışan üçüncü taraf JavaScript kitaplıklarını etkinleştirme gibi işlenmiş içeriği kullanarak ek başlatma adımları gerçekleştirmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="5222a-221">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

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

<span data-ttu-id="5222a-222">`SetParameters` parametreleri ayarlamadan önce kodu çalıştırmak için geçersiz kılınabilir:</span><span class="sxs-lookup"><span data-stu-id="5222a-222">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="5222a-223">Varsa `base.SetParameters` değilse çağrılır, özel kod yolu gerekli gelen parametre değeri yorumlayabilir.</span><span class="sxs-lookup"><span data-stu-id="5222a-223">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="5222a-224">Örneğin, gelen parametreleri sınıfındaki özellikleri atanması için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="5222a-224">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

<span data-ttu-id="5222a-225">`ShouldRender` Kullanıcı Arabiriminde yenilemeyi engellemek için geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="5222a-225">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="5222a-226">Uygulama döndürürse `true`, UI yenilenir.</span><span class="sxs-lookup"><span data-stu-id="5222a-226">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="5222a-227">Bile `ShouldRender` olan geçersiz kılınan, bileşen her zaman başlangıçta işlenir.</span><span class="sxs-lookup"><span data-stu-id="5222a-227">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="5222a-228">IDisposable ile bileşen elden çıkarma</span><span class="sxs-lookup"><span data-stu-id="5222a-228">Component disposal with IDisposable</span></span>

<span data-ttu-id="5222a-229">Bir bileşen uyguluyorsa <xref:System.IDisposable>, [Dispose yöntemini](/dotnet/standard/garbage-collection/implementing-dispose) bileşeni Arabiriminden kaldırıldığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5222a-229">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="5222a-230">Aşağıdaki bileşen `@implements IDisposable` ve `Dispose` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5222a-230">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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

## <a name="routing"></a><span data-ttu-id="5222a-231">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="5222a-231">Routing</span></span>

<span data-ttu-id="5222a-232">Razor bileşenlerde yönlendirme uygulamasında erişilebilir her bileşeni için bir rota şablonu sağlayarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5222a-232">Routing in Razor Components is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="5222a-233">Olduğunda bir *.cshtml* ile dosya bir `@page` yönergesi derlendiğinde, oluşturulan sınıfın belirli bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> belirten rota şablonu.</span><span class="sxs-lookup"><span data-stu-id="5222a-233">When a *.cshtml* file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="5222a-234">Çalışma zamanında bileşen sınıfları ile yönlendirici arar bir `RouteAttribute` ve hangi bileşen istenen URL ile eşleşen bir rota şablonuna sahip işler.</span><span class="sxs-lookup"><span data-stu-id="5222a-234">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="5222a-235">Bir bileşenin birden çok yol şablonu uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="5222a-235">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="5222a-236">Aşağıdaki bileşen isteklerine yanıt veren `/BlazorRoute` ve `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="5222a-236">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="5222a-237">Yol parametreleri</span><span class="sxs-lookup"><span data-stu-id="5222a-237">Route parameters</span></span>

<span data-ttu-id="5222a-238">Sağlanan yol şablondan, rota parametrelerinin bileşenleri alabilir `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="5222a-238">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="5222a-239">Yönlendirici, karşılık gelen bileşen parametreleri doldurmak için rota parametrelerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="5222a-239">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="5222a-240">*RouteParameter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5222a-240">*RouteParameter.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter)]

<span data-ttu-id="5222a-241">İsteğe bağlı parametreler desteklenmez, bu nedenle iki `@page` yönergeleri, yukarıdaki örnekte uygulanır.</span><span class="sxs-lookup"><span data-stu-id="5222a-241">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="5222a-242">İlk Gezinti parametresi olmadan bileşenine izin verir.</span><span class="sxs-lookup"><span data-stu-id="5222a-242">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="5222a-243">İkinci `@page` yönergesi gereken `{text}` rota parametresi ve değeri atar `Text` özelliği.</span><span class="sxs-lookup"><span data-stu-id="5222a-243">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="5222a-244">Bir "code-behind" deneyimi için temel sınıf devralma</span><span class="sxs-lookup"><span data-stu-id="5222a-244">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="5222a-245">Bileşen dosyaları (*.cshtml*) HTML biçimlendirmeyi karışımı ve C# aynı dosyada kod işleme.</span><span class="sxs-lookup"><span data-stu-id="5222a-245">Component files (*.cshtml*) mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="5222a-246">`@inherits` Yönergesi, Razor bileşenleri uygulamalar Bileşen biçimlendirme işleme koddan ayıran bir "code-behind" deneyimi sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5222a-246">The `@inherits` directive can be used to provide Razor Components apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="5222a-247">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) nasıl bir bileşen bir temel sınıf devralma işlemi yapabileceğini gösterir `BlazorRocksBase`, bileşenin özellikleri ve yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="5222a-247">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="5222a-248">*BlazorRocks.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5222a-248">*BlazorRocks.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?name=snippet_BlazorRocks)]

<span data-ttu-id="5222a-249">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="5222a-249">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="5222a-250">Temel sınıfın türetilmesi `BlazorComponent`.</span><span class="sxs-lookup"><span data-stu-id="5222a-250">The base class should derive from `BlazorComponent`.</span></span>

## <a name="razor-support"></a><span data-ttu-id="5222a-251">Razor desteği</span><span class="sxs-lookup"><span data-stu-id="5222a-251">Razor support</span></span>

<span data-ttu-id="5222a-252">**Razor yönergesi**</span><span class="sxs-lookup"><span data-stu-id="5222a-252">**Razor directives**</span></span>

<span data-ttu-id="5222a-253">Razor yönergeleri, aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="5222a-253">Razor directives are shown in the following table.</span></span>

| <span data-ttu-id="5222a-254">Yönergesi</span><span class="sxs-lookup"><span data-stu-id="5222a-254">Directive</span></span> | <span data-ttu-id="5222a-255">Açıklama</span><span class="sxs-lookup"><span data-stu-id="5222a-255">Description</span></span> |
| --------- | ----------- |
| [<span data-ttu-id="5222a-256">\@İşlevleri</span><span class="sxs-lookup"><span data-stu-id="5222a-256">\@functions</span></span>](xref:mvc/views/razor#section-5) | <span data-ttu-id="5222a-257">Ekler bir C# bileşenine kod bloğu.</span><span class="sxs-lookup"><span data-stu-id="5222a-257">Adds a C# code block to a component.</span></span> |
| `@implements` | <span data-ttu-id="5222a-258">Oluşturulan bileşen sınıfı için bir arabirim uygular.</span><span class="sxs-lookup"><span data-stu-id="5222a-258">Implements an interface for the generated component class.</span></span> |
| [<span data-ttu-id="5222a-259">\@Devralan</span><span class="sxs-lookup"><span data-stu-id="5222a-259">\@inherits</span></span>](xref:mvc/views/razor#section-3) | <span data-ttu-id="5222a-260">Bileşen devralan sınıf tam denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="5222a-260">Provides full control of the class that the component inherits.</span></span> |
| [<span data-ttu-id="5222a-261">\@ekleme</span><span class="sxs-lookup"><span data-stu-id="5222a-261">\@inject</span></span>](xref:mvc/views/razor#section-4) | <span data-ttu-id="5222a-262">Hizmet ekleme gelen etkinleştirir [hizmet kapsayıcı](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5222a-262">Enables service injection from the [service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="5222a-263">Daha fazla bilgi için [görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5222a-263">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span> |
| `@layout` | <span data-ttu-id="5222a-264">Bir düzen bileşeni belirtir.</span><span class="sxs-lookup"><span data-stu-id="5222a-264">Specifies a layout component.</span></span> <span data-ttu-id="5222a-265">Düzen bileşenleri, kod yinelemesi ve tutarsızlık önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5222a-265">Layout components are used to avoid code duplication and inconsistency.</span></span> |
| [<span data-ttu-id="5222a-266">\@Sayfa</span><span class="sxs-lookup"><span data-stu-id="5222a-266">\@page</span></span>](xref:razor-pages/index#razor-pages) | <span data-ttu-id="5222a-267">Bileşen doğrudan istekleri işleyeceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="5222a-267">Specifies that the component should handle requests directly.</span></span> <span data-ttu-id="5222a-268">`@page` Yönergesi, bir rota ve isteğe bağlı parametreler ile belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="5222a-268">The `@page` directive can be specified with a route and optional parameters.</span></span> <span data-ttu-id="5222a-269">Razor sayfaları aksine `@page` yönergesi üst dosyanın ilk yönerge olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="5222a-269">Unlike Razor Pages, the `@page` directive doesn't need to be the first directive at the top of the file.</span></span> <span data-ttu-id="5222a-270">Daha fazla bilgi için [yönlendirme](xref:razor-components/routing).</span><span class="sxs-lookup"><span data-stu-id="5222a-270">For more information, see [Routing](xref:razor-components/routing).</span></span> |
| [<span data-ttu-id="5222a-271">\@kullanma</span><span class="sxs-lookup"><span data-stu-id="5222a-271">\@using</span></span>](xref:mvc/views/razor#using) | <span data-ttu-id="5222a-272">Ekler C# `using` yönergesini oluşturulan bileşen sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5222a-272">Adds the C# `using` directive to the generated component class.</span></span> |
| [<span data-ttu-id="5222a-273">\@addTagHelper</span><span class="sxs-lookup"><span data-stu-id="5222a-273">\@addTagHelper</span></span>](xref:mvc/views/razor#tag-helpers) | <span data-ttu-id="5222a-274">Kullanma `@addTagHelper` uygulamanın derleme farklı bir derleme bir bileşeni kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="5222a-274">Use `@addTagHelper` to use a component in a different assembly than the app's assembly.</span></span> |

<span data-ttu-id="5222a-275">**Koşullu öznitelikleri**</span><span class="sxs-lookup"><span data-stu-id="5222a-275">**Conditional attributes**</span></span>

<span data-ttu-id="5222a-276">Öznitelikleri .NET değerine göre koşullu olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="5222a-276">Attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="5222a-277">Değer ise `false` veya `null`, öznitelik işlenen değil.</span><span class="sxs-lookup"><span data-stu-id="5222a-277">If the value is `false` or `null`,  the attribute isn't rendered.</span></span> <span data-ttu-id="5222a-278">Değer ise `true`, öznitelik işlenen simge.</span><span class="sxs-lookup"><span data-stu-id="5222a-278">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="5222a-279">Aşağıdaki örnekte, `IsCompleted` belirler `checked` denetimin biçimlendirme içinde işlenir:</span><span class="sxs-lookup"><span data-stu-id="5222a-279">In the following example, `IsCompleted` determines if `checked` is rendered in the control's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

<span data-ttu-id="5222a-280">Varsa `IsCompleted` olduğu `true`, onay kutusunu olarak işlenir:</span><span class="sxs-lookup"><span data-stu-id="5222a-280">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="5222a-281">Varsa `IsCompleted` olduğu `false`, onay kutusunu olarak işlenir:</span><span class="sxs-lookup"><span data-stu-id="5222a-281">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="5222a-282">**Razor hakkında daha fazla bilgi**</span><span class="sxs-lookup"><span data-stu-id="5222a-282">**Additional information on Razor**</span></span>

<span data-ttu-id="5222a-283">Razor hakkında daha fazla bilgi için bkz. [Razor söz dizimi başvurusu](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="5222a-283">For more information on Razor, see the [Razor syntax reference](xref:mvc/views/razor).</span></span>

## <a name="raw-html"></a><span data-ttu-id="5222a-284">Ham HTML</span><span class="sxs-lookup"><span data-stu-id="5222a-284">Raw HTML</span></span>

<span data-ttu-id="5222a-285">Dizeler genellikle DOM metin düğümleri kullanarak içeriyor olabilir herhangi bir biçimlendirme yok sayıldı ve düz metin kabul yani işlenir.</span><span class="sxs-lookup"><span data-stu-id="5222a-285">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="5222a-286">Ham HTML oluşturmak için bir HTML içeriği kaydırma bir `MarkupString` değeri.</span><span class="sxs-lookup"><span data-stu-id="5222a-286">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="5222a-287">Değer HTML veya SVG ayrıştırılması ve DOM'a eklenmesi</span><span class="sxs-lookup"><span data-stu-id="5222a-287">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="5222a-288">Herhangi bir yapıda ham HTML'yi işlemeye güvenilmeyen kaynak bir **güvenlik riski** ve kaçınılmalıdır!</span><span class="sxs-lookup"><span data-stu-id="5222a-288">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="5222a-289">Aşağıdaki örnek, gösterir kullanarak `MarkupString` bir bileşenin işlenen çıkışı için bir statik HTML içeriği bloğunu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5222a-289">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@functions {
    string myMarkup = "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="5222a-290">Şablonlu bileşenleri</span><span class="sxs-lookup"><span data-stu-id="5222a-290">Templated components</span></span>

<span data-ttu-id="5222a-291">Şablonlu bileşenleri bileşenin işleme mantığı bir parçası olarak kullanılabilir bir parametre olarak bir veya daha fazla kullanıcı Arabirimi şablonları kabul bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="5222a-291">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="5222a-292">Şablonlu bileşenleri normal bileşenleri daha fazla yeniden kullanılabilir olan üst düzey bileşeni yazmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="5222a-292">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="5222a-293">Birkaç örnek şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5222a-293">A couple of examples include:</span></span>

* <span data-ttu-id="5222a-294">Bir tablonun üst bilgi, satırları ve alt bilgisi için şablonları belirtmesine imkan tanıyan bir tablo bileşeni.</span><span class="sxs-lookup"><span data-stu-id="5222a-294">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="5222a-295">Bir listedeki öğeleri işleme için bir şablon belirtmesine imkan tanıyan bir liste bileşeni.</span><span class="sxs-lookup"><span data-stu-id="5222a-295">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="5222a-296">Şablon parametreleri</span><span class="sxs-lookup"><span data-stu-id="5222a-296">Template parameters</span></span>

<span data-ttu-id="5222a-297">Şablonlu bir bileşen türünde bir veya daha fazla bileşen parametreleri belirterek tanımlanmış `RenderFragment` veya `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="5222a-297">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="5222a-298">Bir işleme parça bileşen tarafından işlenen kullanıcı arabiriminin bir kesimi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="5222a-298">A render fragment represents a segment of UI that is rendered by the component.</span></span> <span data-ttu-id="5222a-299">Bir işleme parça, isteğe bağlı olarak işleme parça çağrıldığında belirtilebilecek bir parametre alır.</span><span class="sxs-lookup"><span data-stu-id="5222a-299">A render fragment optionally takes a parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="5222a-300">*Components/TableTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5222a-300">*Components/TableTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

<span data-ttu-id="5222a-301">Şablonlu bir bileşen kullanırken, şablon parametrelerinin parametrelerinin adları eşleşen alt öğeleri kullanılarak belirtilebilir (`TableHeader` ve `RowTemplate` aşağıdaki örnekte):</span><span class="sxs-lookup"><span data-stu-id="5222a-301">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="5222a-302">Şablon bağlam parametreleri</span><span class="sxs-lookup"><span data-stu-id="5222a-302">Template context parameters</span></span>

<span data-ttu-id="5222a-303">Bileşen Türü bağımsız değişkenleri `RenderFragment<T>` öğeleri adlı örtük bir parametre olarak geçirilen `context` (örneğin önceki kod örneğinde,'den `@context.PetId`), ancak parametre adını kullanarak değiştirebilirsiniz `Context` alt özniteliği öğe.</span><span class="sxs-lookup"><span data-stu-id="5222a-303">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="5222a-304">Aşağıdaki örnekte, `RowTemplate` öğenin `Context` özniteliği belirtir `pet` parametresi:</span><span class="sxs-lookup"><span data-stu-id="5222a-304">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="5222a-305">Alternatif olarak, belirtebileceğiniz `Context` bileşen öğesindeki özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5222a-305">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="5222a-306">Belirtilen `Context` özniteliği için tüm belirtilen şablon parametreleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="5222a-306">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="5222a-307">Örtük alt içeriğin içerik parametresi adını belirlemek istediğinizde bu kullanışlı olabilir (herhangi bir sarmalama olmadan alt öğesi).</span><span class="sxs-lookup"><span data-stu-id="5222a-307">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="5222a-308">Aşağıdaki örnekte, `Context` özniteliği görünür `TableTemplate` öğenin ve tüm şablon parametreleri için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="5222a-308">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="5222a-309">Genel yazılmış bileşenler</span><span class="sxs-lookup"><span data-stu-id="5222a-309">Generic-typed components</span></span>

<span data-ttu-id="5222a-310">Şablonlu bileşenleri çoğunlukla genel olarak yazılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5222a-310">Templated components are often generically typed.</span></span> <span data-ttu-id="5222a-311">Örneğin, bir genel liste görünümü şablonu bileşeni işlemek için kullanılabilir `IEnumerable<T>` değerleri.</span><span class="sxs-lookup"><span data-stu-id="5222a-311">For example, a generic List View Template component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="5222a-312">Genel bileşen tanımlamak için `@typeparam` tür parametreleri belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="5222a-312">To define a generic component, use the `@typeparam` directive to specify type parameters.</span></span>

<span data-ttu-id="5222a-313">*Components/ListViewTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5222a-313">*Components/ListViewTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml?highlight=1)]

<span data-ttu-id="5222a-314">Tür parametresi, genel yazılmış bileşenler kullanırken, mümkünse algılanır:</span><span class="sxs-lookup"><span data-stu-id="5222a-314">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="5222a-315">Aksi takdirde, tür parametresi açık tür parametresinin adıyla eşleşen bir özniteliği kullanılarak belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="5222a-315">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="5222a-316">Aşağıdaki örnekte, `TItem="Pet"` türünü belirtir:</span><span class="sxs-lookup"><span data-stu-id="5222a-316">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="5222a-317">Geçişli değerleri ve parametreler</span><span class="sxs-lookup"><span data-stu-id="5222a-317">Cascading values and parameters</span></span>

<span data-ttu-id="5222a-318">Bazı senaryolarda, akış verileri kullanarak bir alt bileşen bir üst bileşeninden kullanışsız [bileşeni parametreleri](#component-parameters), özellikle birçok bileşen katmanları olduğunda.</span><span class="sxs-lookup"><span data-stu-id="5222a-318">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="5222a-319">Geçişli değerleri ve parametre bir üst bileşeni tüm alt öğe bileşenleri için bir değer sağlamak için kullanışlı bir yol sağlayarak bu sorunu çözer.</span><span class="sxs-lookup"><span data-stu-id="5222a-319">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="5222a-320">Ayrıca basamaklı değerleri ve parametreler koordine etmek, bileşenler için bir yaklaşım sağlar.</span><span class="sxs-lookup"><span data-stu-id="5222a-320">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="5222a-321">Tema örnek</span><span class="sxs-lookup"><span data-stu-id="5222a-321">Theme example</span></span>

<span data-ttu-id="5222a-322">Aşağıdaki *tema* örnek uygulama örneğinden `ThemeInfo` sınıfı, böylece tüm düğmelerin belirli bir uygulamanın parçası içinde aynı stili paylaşır bileşen hiyerarşisi akış için tema bilgileri belirtir.</span><span class="sxs-lookup"><span data-stu-id="5222a-322">In the following *Theme* example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="5222a-323">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="5222a-323">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="5222a-324">Üst bileşeni, geçişli değeri bileşenini kullanarak basamaklı bir değer sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5222a-324">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="5222a-325">Geçişli değer bileşeni bileşen hiyerarşisi bir alt ağacı sarmalar ve o alt ağacı içinde tüm bileşenleri tek bir değer sağlar.</span><span class="sxs-lookup"><span data-stu-id="5222a-325">The Cascading Value component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="5222a-326">Örneğin, örnek uygulamayı tema bilgileri belirtir (`ThemeInfo`) bir düzen gövdesini oluşturan tüm bileşenlerin basamaklı bir parametre olarak uygulamanın düzenleri `@Body` özelliği.</span><span class="sxs-lookup"><span data-stu-id="5222a-326">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="5222a-327">`ButtonClass` bir değeri atanır `btn-success` Düzen bileşende.</span><span class="sxs-lookup"><span data-stu-id="5222a-327">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="5222a-328">Bu özelliği üzerinden herhangi bir alt bileşen tüketebileceği `ThemeInfo` basamaklı nesnesi.</span><span class="sxs-lookup"><span data-stu-id="5222a-328">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="5222a-329">*Shared/CascadingValuesParametersLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5222a-329">*Shared/CascadingValuesParametersLayout.cshtml*:</span></span>

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
    ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="5222a-330">Yapmak için geçişli değerlerini kullanmak, bileşenlerini kullanarak basamaklı parametreleri bildirmek `[CascadingParameter]` özniteliği veya bir dize adı değerine göre:</span><span class="sxs-lookup"><span data-stu-id="5222a-330">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")] PermInfo Permissions { get; set; }
```

<span data-ttu-id="5222a-331">Bir dize adı değeri bağlamayla aynı türde birden fazla basamaklı değerler varsa ve aynı alt ağacı içinde ayrılmaları gerekiyorsa geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="5222a-331">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="5222a-332">Basamaklı değerler türüne göre geçişli parametrelerine bağlı.</span><span class="sxs-lookup"><span data-stu-id="5222a-332">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="5222a-333">Örnek uygulamada, geçişli değerleri parametreleri tema bileşen bağlar `ThemeInfo` basamaklı basamaklı bir parametre değeri.</span><span class="sxs-lookup"><span data-stu-id="5222a-333">In the sample app, the Cascading Values Parameters Theme component binds to the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="5222a-334">Parametre için bir bileşen tarafından görüntülenen düğmeleri CSS sınıfının ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5222a-334">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="5222a-335">*Pages/CascadingValuesParametersTheme.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5222a-335">*Pages/CascadingValuesParametersTheme.cshtml*:</span></span>

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
    int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a><span data-ttu-id="5222a-336">TabSet örneği</span><span class="sxs-lookup"><span data-stu-id="5222a-336">TabSet example</span></span>

<span data-ttu-id="5222a-337">Geçişli parametreleri, bileşen hiyerarşi işbirliği yapmak bileşenler de olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="5222a-337">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="5222a-338">Örneğin, aşağıdakileri dikkate alın *TabSet* örnek uygulamada örnek.</span><span class="sxs-lookup"><span data-stu-id="5222a-338">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="5222a-339">Örnek uygulamanın bir `ITab` uygulama sekme arabirimi:</span><span class="sxs-lookup"><span data-stu-id="5222a-339">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="5222a-340">Basamaklı değerler parametreleri TabSet bileşen birden fazla sekme bileşenleri içeren sekmesinin bileşeni kullanır:</span><span class="sxs-lookup"><span data-stu-id="5222a-340">The Cascading Values Parameters TabSet component uses the Tab Set component, which contains several Tab components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

<span data-ttu-id="5222a-341">Alt sekmesinde bileşenleri sekmesini ayarlamak için parametre olarak açıkça geçirilen değildir.</span><span class="sxs-lookup"><span data-stu-id="5222a-341">The child Tab components aren't explicitly passed as parameters to the Tab Set.</span></span> <span data-ttu-id="5222a-342">Bunun yerine, alt sekmesinde bileşenleri alt içeriğin sekme kümesi'nin parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="5222a-342">Instead, the child Tab components are part of the child content of the Tab Set.</span></span> <span data-ttu-id="5222a-343">Ancak sekme kümesi yine de üst bilgiler ve etkin sekmede işleyebilir, böylece her sekme bileşeni hakkında bilmesi gerekir. Ek kod, sekme kümesini bileşen gerek kalmadan bu koordinasyon sağlamak *kendisini basamaklı bir değer sağlayabilirsiniz* , ardından alındığından bloğun sekmesini bileşenleri tarafından.</span><span class="sxs-lookup"><span data-stu-id="5222a-343">However, the Tab Set still needs to know about each Tab component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the Tab Set component *can provide itself as a cascading value* that is then picked up by the descendent Tab components.</span></span>

<span data-ttu-id="5222a-344">*Components/TabSet.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5222a-344">*Components/TabSet.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

<span data-ttu-id="5222a-345">Descendent sekmesini bileşenleri yakalama sekmesini içeren ayarlayın basamaklı bir parametre olarak sekmesini ayarlamak ve koordinat sekmesini bileşenleri kendilerini ekleme hangi sekmesinde olduğundan etkin.</span><span class="sxs-lookup"><span data-stu-id="5222a-345">The descendent Tab components capture the containing Tab Set as a cascading parameter, so the Tab components add themselves to the Tab Set and coordinate on which tab is active.</span></span>

<span data-ttu-id="5222a-346">*Components/Tab.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5222a-346">*Components/Tab.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a><span data-ttu-id="5222a-347">Razor şablonları</span><span class="sxs-lookup"><span data-stu-id="5222a-347">Razor templates</span></span>

<span data-ttu-id="5222a-348">İşleme parçaları, Razor şablon söz dizimi kullanılarak tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="5222a-348">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="5222a-349">Razor şablonları UI parçacık tanımlayın ve aşağıdaki biçimi varsayar için bir yol sağlar:</span><span class="sxs-lookup"><span data-stu-id="5222a-349">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="5222a-350">Aşağıdaki örnek nasıl belirtileceğini göstermektedir `RenderFragment` ve `RenderFragment<T>` değerleri.</span><span class="sxs-lookup"><span data-stu-id="5222a-350">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values.</span></span>

<span data-ttu-id="5222a-351">*RazorTemplates.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5222a-351">*RazorTemplates.cshtml*:</span></span>

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

<span data-ttu-id="5222a-352">Şablonları şablonlu bileşenleri için bağımsız değişken olarak geçirilen veya doğrudan işlenmiş Razor kullanılarak tanımlanan parçalarının işleyin.</span><span class="sxs-lookup"><span data-stu-id="5222a-352">Render fragments defined using Razor templates can be passed as arguments to templated components or rendered directly.</span></span> <span data-ttu-id="5222a-353">Örneğin, önceki şablonları aşağıdaki Razor işaretlemesi ile doğrudan işlenir:</span><span class="sxs-lookup"><span data-stu-id="5222a-353">For example, the previous templates are directly rendered with the following Razor markup:</span></span>

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

<span data-ttu-id="5222a-354">İşlenmiş çıkışı:</span><span class="sxs-lookup"><span data-stu-id="5222a-354">Rendered output:</span></span>

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="5222a-355">El ile RenderTreeBuilder mantığı</span><span class="sxs-lookup"><span data-stu-id="5222a-355">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="5222a-356">`Microsoft.AspNetCore.Components.RenderTree` bileşenleri ve bileşenleri el ile oluşturma da dahil olmak üzere öğeleri işlemek için yöntemler sağlar C# kod.</span><span class="sxs-lookup"><span data-stu-id="5222a-356">`Microsoft.AspNetCore.Components.RenderTree` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="5222a-357">Kullanım `RenderTreeBuilder` bileşenler oluşturmak için Gelişmiş bir senaryodur.</span><span class="sxs-lookup"><span data-stu-id="5222a-357">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="5222a-358">Hatalı biçimlendirilmiş bir bileşen (örneğin, bir kapatılmamış biçimlendirme etiketi) tanımsız davranışlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="5222a-358">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="5222a-359">Aşağıdaki evcil hayvan ayrıntıları bileşeni göz önünde bulundurun (*PetDetails.razor* Razor bileşenlerinde; *PetDetails.cshtml* Blazor içinde), hangi el ile oluşturulabilen başka bir bileşene dönüştürerek:</span><span class="sxs-lookup"><span data-stu-id="5222a-359">Consider the following Pet Details component (*PetDetails.razor* in Razor Components; *PetDetails.cshtml* in Blazor), which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@functions
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="5222a-360">Aşağıdaki örnekte, bir döngüde `CreateComponent` yöntem üç evcil hayvan ayrıntıları bileşeni oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5222a-360">In the following example, the loop in the `CreateComponent` method generates three Pet Details components.</span></span> <span data-ttu-id="5222a-361">Çağrılırken `RenderTreeBuilder` bileşenler oluşturmak için yöntemleri (`OpenComponent` ve `AddAttribute`), sıra numaraları olan kaynak kodu satır numaraları.</span><span class="sxs-lookup"><span data-stu-id="5222a-361">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="5222a-362">Razor kod değil ayrı ayrı satırlara karşılık gelen sıra numaraları fark algoritması kullanır bileşenleri çağrılarını çağırın.</span><span class="sxs-lookup"><span data-stu-id="5222a-362">The Razor Components difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="5222a-363">Bir bileşen ile oluştururken `RenderTreeBuilder` yöntemleri, sabit kodlamayın seri numaraları için bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="5222a-363">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="5222a-364">**Sıra numarası oluşturmak için bir hesaplama veya sayaç kullanarak düşük performansa neden olabilir.**</span><span class="sxs-lookup"><span data-stu-id="5222a-364">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span>

<span data-ttu-id="5222a-365">Yerleşik bileşeni (*BuiltContent.razor* Razor bileşenlerinde; *BuiltContent.cshtml* Blazor içinde):</span><span class="sxs-lookup"><span data-stu-id="5222a-365">Built component (*BuiltContent.razor* in Razor Components; *BuiltContent.cshtml* in Blazor):</span></span>

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" onclick="@RenderComponent">
    Create three Pet Details components
</button>

@functions {
    RenderFragment CustomRender { get; set; }
    
    RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```
