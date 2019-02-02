---
title: Oluşturma ve Razor bileşenleri kullanma
author: guardrex
description: Oluşturma ve Razor bileşenler, bileşen ömürleri yönetme verilere bağlayın ve olayları işlemek nasıl dahil olmak üzere kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/components
ms.openlocfilehash: 550755d4cc2bd309846e1cc332c3bb1f0b812c25
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668199"
---
# <a name="create-and-use-razor-components"></a><span data-ttu-id="b9a91-103">Oluşturma ve Razor bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="b9a91-103">Create and use Razor Components</span></span>

<span data-ttu-id="b9a91-104">Tarafından [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), ve [Morné Zaayman](https://github.com/MorneZaayman)</span><span class="sxs-lookup"><span data-stu-id="b9a91-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Morné Zaayman](https://github.com/MorneZaayman)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="b9a91-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b9a91-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="b9a91-106">Bkz: [başlama](xref:razor-components/get-started) Önkoşullar için konu.</span><span class="sxs-lookup"><span data-stu-id="b9a91-106">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="b9a91-107">Razor bileşenleri uygulamaları kullanılarak oluşturulur *bileşenleri*.</span><span class="sxs-lookup"><span data-stu-id="b9a91-107">Razor Components apps are built using *components*.</span></span> <span data-ttu-id="b9a91-108">Bir bileşen, kullanıcı arabirimi (UI), sayfa, iletişim veya form gibi kendi içinde bir öbektir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-108">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="b9a91-109">Her iki HTML biçimlendirmesi veri ekleme veya UI olaylarına yanıt vermek için gereken işleme mantığı ile birlikte işlemek için bir bileşeni içerir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-109">A component includes both the HTML markup to render along with the processing logic needed to inject data or respond to UI events.</span></span> <span data-ttu-id="b9a91-110">Esnek ve basit bileşenlerdir ve bunlar iç içe geçmiş, yeniden kullanılabilir ve projeler arasında paylaşılan.</span><span class="sxs-lookup"><span data-stu-id="b9a91-110">Components are flexible and lightweight, and they can be nested, reused, and shared between projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="b9a91-111">Bileşen sınıfları</span><span class="sxs-lookup"><span data-stu-id="b9a91-111">Component classes</span></span>

<span data-ttu-id="b9a91-112">Bileşenleri genellikle uygulanır  *\*.cshtml* bir birleşimini kullanarak dosyaları C# ve HTML biçimlendirmesi.</span><span class="sxs-lookup"><span data-stu-id="b9a91-112">Components are typically implemented in *\*.cshtml* files using a combination of C# and HTML markup.</span></span> <span data-ttu-id="b9a91-113">Bir bileşen için kullanıcı Arabirimi, HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="b9a91-113">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="b9a91-114">(Örneğin, döngü, koşullular, ifadeleri) dinamik işleme mantığı, katıştırılmış kullanarak eklenir C# adlı söz dizimi [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="b9a91-114">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span></span> <span data-ttu-id="b9a91-115">Ne zaman bir Razor bileşenleri uygulama derlendiğinde, HTML biçimlendirmesi ve C# işleme mantığı, bir bileşen sınıfı dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="b9a91-115">When a Razor Components app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="b9a91-116">Oluşturulan sınıfın adı dosya adıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="b9a91-116">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="b9a91-117">Bileşen sınıfı üyeleri tanımlanmış bir `@functions` blok (birden fazla `@functions` bloğu izin verilen).</span><span class="sxs-lookup"><span data-stu-id="b9a91-117">Members of the component class are defined in a `@functions` block (more than one `@functions` block is permissible).</span></span> <span data-ttu-id="b9a91-118">İçinde `@functions` blok, bileşen durumu (Özellikler, alanlar), olay işleme için veya başka bir bileşen mantığı tanımlamak için yöntemlerle birlikte belirtilir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-118">In the `@functions` block, component state (properties, fields) is specified along with methods for event handling or for defining other component logic.</span></span>

<span data-ttu-id="b9a91-119">Bileşen üyeleri, ardından bileşenin parçası mantığı kullanarak işleme olarak kullanılabilir C# ile başlayan ifadeleri `@`.</span><span class="sxs-lookup"><span data-stu-id="b9a91-119">Component members can then be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="b9a91-120">Örneğin, bir C# alan ekleyerek işlenen `@` alan adı.</span><span class="sxs-lookup"><span data-stu-id="b9a91-120">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="b9a91-121">Aşağıdaki örnek, değerlendirir ve işler:</span><span class="sxs-lookup"><span data-stu-id="b9a91-121">The following example evaluates and renders:</span></span>

* <span data-ttu-id="b9a91-122">`_headingFontStyle` CSS özellik değerini `font-style`.</span><span class="sxs-lookup"><span data-stu-id="b9a91-122">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="b9a91-123">`_headingText` içeriği için `<h1>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="b9a91-123">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="b9a91-124">Bileşen, bileşen başlangıçta işlenen sonra olaylara yanıt olarak, işleme ağacında yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b9a91-124">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="b9a91-125">Razor bileşenleri yeni bir işleme ağacı Öncekine karşı karşılaştırır ve herhangi bir değişiklik tarayıcının belge nesne modeli (DOM) için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-125">Razor Components then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

## <a name="using-components"></a><span data-ttu-id="b9a91-126">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="b9a91-126">Using components</span></span>

<span data-ttu-id="b9a91-127">Bileşenleri, diğer bileşenlerin bunları bildirerek içerebilir HTML öğesi söz dizimini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="b9a91-127">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="b9a91-128">Bir bileşen kullanma için işaretleme, etiketin adını bileşen türü olduğu gibi HTML etiketleri arar.</span><span class="sxs-lookup"><span data-stu-id="b9a91-128">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="b9a91-129">Aşağıdaki biçimlendirmede işleyen bir `HeadingComponent` (*HeadingComponent.cshtml*) örneği:</span><span class="sxs-lookup"><span data-stu-id="b9a91-129">The following markup renders a `HeadingComponent` (*HeadingComponent.cshtml*) instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?start=11&end=11)]

## <a name="component-parameters"></a><span data-ttu-id="b9a91-130">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="b9a91-130">Component parameters</span></span>

<span data-ttu-id="b9a91-131">Bileşenleri olabilir *bileşeni parametreleri*, hangi kullanılarak tanımlanır *genel olmayan* bileşen sınıfı özellikleri düzenlenmiş ile `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="b9a91-131">Components can have *component parameters*, which are defined using *non-public* properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="b9a91-132">Öznitelikleri bir bileşen için bağımsız değişken biçimlendirme içinde belirtmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="b9a91-132">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="b9a91-133">Aşağıdaki örnekte, `ParentComponent` değerini ayarlar `Title` özelliği `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="b9a91-133">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`:</span></span>

<span data-ttu-id="b9a91-134">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b9a91-134">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?start=1&end=7&highlight=5)]

<span data-ttu-id="b9a91-135">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b9a91-135">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a><span data-ttu-id="b9a91-136">Alt içeriğin</span><span class="sxs-lookup"><span data-stu-id="b9a91-136">Child content</span></span>

<span data-ttu-id="b9a91-137">Bileşenleri içeriği başka bir bileşen ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b9a91-137">Components can set the content in another component.</span></span> <span data-ttu-id="b9a91-138">Atama bileşen alıcı bileşeni belirtin etiketleri arasında içerik sağlar.</span><span class="sxs-lookup"><span data-stu-id="b9a91-138">The assigning component provides the content between the tags that specify the receiving component.</span></span> <span data-ttu-id="b9a91-139">Örneğin, bir `ParentComponent` tarafından işlenmek üzere içerik sağlayabilir bir `ChildComponent` içeriğini yerleştirerek  **\<ChildComponent >** etiketler.</span><span class="sxs-lookup"><span data-stu-id="b9a91-139">For example, a `ParentComponent` can provide content that is to be rendered by a `ChildComponent` by placing the content inside **\<ChildComponent>** tags.</span></span>

<span data-ttu-id="b9a91-140">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b9a91-140">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?start=1&end=7&highlight=6)]

<span data-ttu-id="b9a91-141">Alt bileşen bir `ChildContent` temsil eden özellik bir `RenderFragment`.</span><span class="sxs-lookup"><span data-stu-id="b9a91-141">The child component has a `ChildContent` property that represents a `RenderFragment`.</span></span> <span data-ttu-id="b9a91-142">Değerini `ChildContent` alt bileşen işaretlemede içerik nerede oluşturulması gerekip konumlandırıldı.</span><span class="sxs-lookup"><span data-stu-id="b9a91-142">The value of `ChildContent` is positioned in the child component's markup where the content should be rendered.</span></span> <span data-ttu-id="b9a91-143">Aşağıdaki örnekte, değerini `ChildContent` ana bileşenden alınan ve önyükleme bölmenin içinde işlenen `panel-body`.</span><span class="sxs-lookup"><span data-stu-id="b9a91-143">In the following example, the value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="b9a91-144">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b9a91-144">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> <span data-ttu-id="b9a91-145">Özellik Alma `RenderFragment` içeriği adlı `ChildContent` kural tarafından.</span><span class="sxs-lookup"><span data-stu-id="b9a91-145">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

## <a name="data-binding"></a><span data-ttu-id="b9a91-146">Veri bağlama</span><span class="sxs-lookup"><span data-stu-id="b9a91-146">Data binding</span></span>

<span data-ttu-id="b9a91-147">Veri bağlama bileşenleri hem DOM öğeleri ile gerçekleştirilir `bind` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="b9a91-147">Data binding to both components and DOM elements is accomplished with the `bind` attribute.</span></span> <span data-ttu-id="b9a91-148">Aşağıdaki örnek bağlar `ItalicsCheck` özelliğini onay kutusunun işaretli durumu:</span><span class="sxs-lookup"><span data-stu-id="b9a91-148">The following example binds the `ItalicsCheck` property to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" bind="@_italicsCheck" />
```

<span data-ttu-id="b9a91-149">Onay kutusunu işaretli ve seçildiğinde özelliğin değerini şekilde güncelleştirilir `true` ve `false`sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="b9a91-149">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="b9a91-150">Yalnızca bileşen, özelliğin değerinin değiştirilmesi için değil yanıt oluşturulduğunda onay kutusunu kullanıcı Arabiriminde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-150">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="b9a91-151">Olay işleyici kodu yürütüldükten sonra bileşenleri kendilerini işleme olduğundan, özellik güncelleştirmeleri genellikle kullanıcı Arabiriminde hemen yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="b9a91-151">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="b9a91-152">Kullanarak `bind` ile bir `CurrentValue` özelliği (`<input bind="@CurrentValue" />`) aslında aşağıdakine eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="b9a91-152">Using `bind` with a `CurrentValue` property (`<input bind="@CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="b9a91-153">Bileşen işlendiğinde `value` giriş öğesinin geldiği `CurrentValue` özelliği.</span><span class="sxs-lookup"><span data-stu-id="b9a91-153">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="b9a91-154">Kullanıcı, metin kutusuna yazdığında `onchange` tetiklenir ve `CurrentValue` özelliği değiştirilmiş değerine ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="b9a91-154">When the user types in the text box, the `onchange` is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="b9a91-155">Gerçekte, kod oluşturma biraz daha karmaşık olduğundan `bind` tür dönüştürmeleri gerçekleştirildiği birkaç durum ile ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-155">In reality, the code generation is a little more complex because `bind` deals with a few cases where type conversions are performed.</span></span> <span data-ttu-id="b9a91-156">Giren İlkesi `bind` geçerli değerini bir ifade ile ilişkilendirir bir `value` kayıtlı işleyici kullanarak öznitelik ve işleyicilerini değişiklikler.</span><span class="sxs-lookup"><span data-stu-id="b9a91-156">In principle, `bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="b9a91-157">**Biçim dizeleri**</span><span class="sxs-lookup"><span data-stu-id="b9a91-157">**Format strings**</span></span>

<span data-ttu-id="b9a91-158">Veri bağlama ile birlikte çalışır [DateTime](https://docs.microsoft.com/dotnet/api/system.datetime) biçimlendirme dizeleri (ancak diğer biçim ifadelerini para birimi veya sayı biçimleri gibi şu anda değil):</span><span class="sxs-lookup"><span data-stu-id="b9a91-158">Data binding works with [DateTime](https://docs.microsoft.com/dotnet/api/system.datetime) format strings (but not other format expressions at this time, such as currency or number formats):</span></span>

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd" />

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="b9a91-159">`format-value` Özniteliği uygulamak için tarih biçimini belirtir `value` , `input` öğesi.</span><span class="sxs-lookup"><span data-stu-id="b9a91-159">The `format-value` attribute specifies the date format to apply to the `value` of the `input` element.</span></span> <span data-ttu-id="b9a91-160">Biçimi de değer ayrıştırmak için kullanılan zaman bir `onchange` olayı oluşur.</span><span class="sxs-lookup"><span data-stu-id="b9a91-160">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="b9a91-161">**Bileşen parametreleri**</span><span class="sxs-lookup"><span data-stu-id="b9a91-161">**Component parameters**</span></span>

<span data-ttu-id="b9a91-162">Bağlama bileşeni parametreleri de tanır burada `bind-{property}` bir özellik değeri bileşenlerinde bağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b9a91-162">Binding also recognizes component parameters, where `bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="b9a91-163">Aşağıdaki bileşen `ChildComponent` ve bağlar `ParentYear` üst parametresinden `Year` alt bileşen parametresi:</span><span class="sxs-lookup"><span data-stu-id="b9a91-163">The following component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

<span data-ttu-id="b9a91-164">Ana bileşenin:</span><span class="sxs-lookup"><span data-stu-id="b9a91-164">Parent component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">Change Year to 1986</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="b9a91-165">Alt bileşeni:</span><span class="sxs-lookup"><span data-stu-id="b9a91-165">Child component:</span></span>

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

<span data-ttu-id="b9a91-166">`Year` Bir yardımcı olduğundan parametre bağlanabilir `YearChanged` türüyle eşleşen olay `Year` parametresi.</span><span class="sxs-lookup"><span data-stu-id="b9a91-166">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="b9a91-167">Yükleme `ParentComponent` aşağıdaki biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="b9a91-167">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="b9a91-168">Varsa değerini `ParentYear` düğmesini seçerek özelliği değiştirildiğinde `ParentComponent`, `Year` özelliği `ChildComponent` güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-168">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="b9a91-169">Öğesinin yeni değeri `Year` Arabiriminde işlenen olduğunda `ParentComponent` rerendered:</span><span class="sxs-lookup"><span data-stu-id="b9a91-169">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

## <a name="event-handling"></a><span data-ttu-id="b9a91-170">Olay işleme</span><span class="sxs-lookup"><span data-stu-id="b9a91-170">Event handling</span></span>

<span data-ttu-id="b9a91-171">Razor bileşenleri olay işleme özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="b9a91-171">Razor Components provide event handling features.</span></span> <span data-ttu-id="b9a91-172">İçin bir HTML öğesi öznitelik adlı `on<event>` (örneğin, `onclick`, `onsubmit`) temsilci türü belirtilmiş bir değer ile Razor bileşenleri özniteliğinin değeri bir olay işleyicisi değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-172">For an HTML element attribute named `on<event>` (for example, `onclick`, `onsubmit`) with a delegate-typed value, Razor Components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="b9a91-173">Özniteliğin adı her zaman ile başlayan `on`.</span><span class="sxs-lookup"><span data-stu-id="b9a91-173">The attribute's name always starts with `on`.</span></span>

<span data-ttu-id="b9a91-174">Aşağıdaki kod çağrıları `UpdateHeading` Arabiriminde düğme seçildiğinde yöntemi:</span><span class="sxs-lookup"><span data-stu-id="b9a91-174">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

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

<span data-ttu-id="b9a91-175">Aşağıdaki kod çağrıları `CheckboxChanged` onay kutusunu kullanıcı Arabiriminde değiştirildiğinde yöntemi:</span><span class="sxs-lookup"><span data-stu-id="b9a91-175">The following code calls the `CheckboxChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged" />

@functions {
    void CheckboxChanged()
    {
        ...
    }
}
```

<span data-ttu-id="b9a91-176">Olay işleyicileri zaman uyumsuz ve dönüş ayrıca olabilir bir `Task`.</span><span class="sxs-lookup"><span data-stu-id="b9a91-176">Event handlers can also be asynchronous and return a `Task`.</span></span> <span data-ttu-id="b9a91-177">El ile çağırmaya gerek yoktur `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="b9a91-177">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="b9a91-178">Ortaya çıkan özel durumlar günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-178">Exceptions are logged when they occur.</span></span>

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

<span data-ttu-id="b9a91-179">Bazı olaylar için olaya özgü olay bağımsız değişken türleri de izin verilir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-179">For some events, event-specific event argument types are permitted.</span></span> <span data-ttu-id="b9a91-180">Bu olay türleri birine erişimi gerekli değilse, yöntem çağrısında gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-180">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="b9a91-181">Desteklenen olay bağımsız değişkenleri listesi verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="b9a91-181">The list of supported event arguments is:</span></span>

* <span data-ttu-id="b9a91-182">UIEventArgs</span><span class="sxs-lookup"><span data-stu-id="b9a91-182">UIEventArgs</span></span>
* <span data-ttu-id="b9a91-183">UIChangeEventArgs</span><span class="sxs-lookup"><span data-stu-id="b9a91-183">UIChangeEventArgs</span></span>
* <span data-ttu-id="b9a91-184">UIKeyboardEventArgs</span><span class="sxs-lookup"><span data-stu-id="b9a91-184">UIKeyboardEventArgs</span></span>
* <span data-ttu-id="b9a91-185">UIMouseEventArgs</span><span class="sxs-lookup"><span data-stu-id="b9a91-185">UIMouseEventArgs</span></span>

<span data-ttu-id="b9a91-186">Lambda ifadeleri de kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="b9a91-186">Lambda expressions can also be used:</span></span>

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="b9a91-187">Genellikle gibi ek değerler kapatmak uygun olan öğeleri kümesi yineleme olduğunda.</span><span class="sxs-lookup"><span data-stu-id="b9a91-187">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="b9a91-188">Aşağıdaki örnek, üç oluşturur düğmeler, her biri çağıran `UpdateHeading` olay bağımsız değişken geçirme (`UIMouseEventArgs`) ve düğme sayısı (`buttonNumber`) kullanıcı Arabiriminde seçili olduğunda:</span><span class="sxs-lookup"><span data-stu-id="b9a91-188">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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

## <a name="capture-references-to-components"></a><span data-ttu-id="b9a91-189">Bileşenleri başvurular yakalama</span><span class="sxs-lookup"><span data-stu-id="b9a91-189">Capture references to components</span></span>

<span data-ttu-id="b9a91-190">Bileşen başvuruları komutları gibi bu örneğe verebilir böylece bu şekilde get bileşen örneğe bir başvuru sağlar `Show` veya `Reset`.</span><span class="sxs-lookup"><span data-stu-id="b9a91-190">Component references provide a way get a reference to a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="b9a91-191">Bir bileşen başvurusunu yakalamak için ekleme bir `ref` özniteliği alt bileşen ve aynı ada ve aynı türe sahip bir alan alt bileşeni olarak tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b9a91-191">To capture a component reference, add a `ref` attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

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

<span data-ttu-id="b9a91-192">Bileşen işlendiğinde `loginDialog` alanı ile doldurulur `MyLoginDialog` alt bileşen örneği.</span><span class="sxs-lookup"><span data-stu-id="b9a91-192">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="b9a91-193">Ardından bileşen örneği üzerinde .NET yöntemlerini çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b9a91-193">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b9a91-194">`loginDialog` Değişkeni bileşeni işlenir ve çıktısını içeren sonra yalnızca doldurulmuş `MyLoginDialog` öğesi o zamana kadar olduğundan başvuru bir şey yok.</span><span class="sxs-lookup"><span data-stu-id="b9a91-194">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element because until then there is nothing to reference.</span></span> <span data-ttu-id="b9a91-195">Bileşen oluşturma işlemini tamamladıktan sonra bileşenleri başvurularını değiştirmek üzere `OnAfterRenderAsync` veya `OnAfterRender` yaşam döngüsü yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="b9a91-195">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` lifecycle methods.</span></span>

<span data-ttu-id="b9a91-196">Bileşen başvurularını yakalama için benzer bir sözdizimi kullanırken [öğesi başvuruları yakalama](xref:razor-components/javascript-interop#capture-references-to-elements), öyle bir [JavaScript birlikte çalışma](xref:razor-components/javascript-interop) özelliği.</span><span class="sxs-lookup"><span data-stu-id="b9a91-196">While capturing component references uses a similar syntax to [capturing element references](xref:razor-components/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:razor-components/javascript-interop) feature.</span></span> <span data-ttu-id="b9a91-197">Bileşen başvurularını JavaScript koduna geçen değildir; Bunlar, yalnızca .NET kodda kullanılırlar.</span><span class="sxs-lookup"><span data-stu-id="b9a91-197">Component references aren't passed to JavaScript code; they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="b9a91-198">Yapmak **değil** alt bileşenlerin durumunu kesilecek bileşen başvuruları kullanın.</span><span class="sxs-lookup"><span data-stu-id="b9a91-198">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="b9a91-199">Bunun yerine, alt bileşenler için veri iletmek için her zaman normal bildirim temelli parametreler kullanın.</span><span class="sxs-lookup"><span data-stu-id="b9a91-199">Instead, always use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="b9a91-200">Bu alt bileşenleri otomatik olarak doğru zamanlarda rerender neden olur.</span><span class="sxs-lookup"><span data-stu-id="b9a91-200">This causes child components to rerender at the correct times automatically.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="b9a91-201">Yaşam döngüsü yöntemleri</span><span class="sxs-lookup"><span data-stu-id="b9a91-201">Lifecycle methods</span></span>

<span data-ttu-id="b9a91-202">`OnInitAsync` ve `OnInit` bileşeni başlatıldıktan sonra kod yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-202">`OnInitAsync` and `OnInit` execute code after the component has been initialized.</span></span> <span data-ttu-id="b9a91-203">Zaman uyumsuz bir işlemi gerçekleştirmek için `OnInitAsync` ve `await` anahtar sözcüğü:</span><span class="sxs-lookup"><span data-stu-id="b9a91-203">To perform an asynchronous operation, use `OnInitAsync` and use the `await` keyword:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="b9a91-204">Eşzamanlı bir işlem için kullanmak `OnInit`:</span><span class="sxs-lookup"><span data-stu-id="b9a91-204">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

<span data-ttu-id="b9a91-205">`OnParametersSetAsync` ve `OnParametersSet` bir bileşen üst öğesinden parametreleri aldı ve özelliklerine değerler atanır çağırılır.</span><span class="sxs-lookup"><span data-stu-id="b9a91-205">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="b9a91-206">Bu yöntemler, sonra yürütülür `OnInit` bileşen başlatma sırasında.</span><span class="sxs-lookup"><span data-stu-id="b9a91-206">These methods are executed after `OnInit` during component initialization.</span></span>

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

<span data-ttu-id="b9a91-207">`OnAfterRenderAsync` ve `OnAfterRender` bir bileşen oluşturma işlemini tamamladıktan sonra her zaman çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b9a91-207">`OnAfterRenderAsync` and `OnAfterRender` are called each time after a component has finished rendering.</span></span> <span data-ttu-id="b9a91-208">Öğe ve bileşen başvuruları bu noktada doldurulur.</span><span class="sxs-lookup"><span data-stu-id="b9a91-208">Element and component references are populated at this point.</span></span> <span data-ttu-id="b9a91-209">Bu aşama, işlenmiş DOM öğeleri üzerinde çalışan üçüncü taraf JavaScript kitaplıklarını etkinleştirme gibi işlenmiş içeriği kullanarak ek başlatma adımları gerçekleştirmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="b9a91-209">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

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

<span data-ttu-id="b9a91-210">`SetParameters` parametreleri ayarlamadan önce kodu çalıştırmak için geçersiz kılınabilir:</span><span class="sxs-lookup"><span data-stu-id="b9a91-210">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="b9a91-211">Varsa `base.SetParameters` değilse çağrılır, özel kod yolu gerekli gelen parametre değeri yorumlayabilir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-211">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="b9a91-212">Örneğin, gelen parametreleri sınıfındaki özellikleri atanması için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-212">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

<span data-ttu-id="b9a91-213">`ShouldRender` Kullanıcı Arabiriminde yenilemeyi engellemek için geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-213">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="b9a91-214">Uygulama döndürürse `true`, UI yenilenir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-214">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="b9a91-215">Bile `ShouldRender` olan geçersiz kılınan, bileşen her zaman başlangıçta işlenir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-215">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="b9a91-216">IDisposable ile bileşen elden çıkarma</span><span class="sxs-lookup"><span data-stu-id="b9a91-216">Component disposal with IDisposable</span></span>

<span data-ttu-id="b9a91-217">Bir bileşen uyguluyorsa [IDisposable](https://docs.microsoft.com/dotnet/api/system.idisposable), [Dispose yöntemini](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose) bileşeni Arabiriminden kaldırıldığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b9a91-217">If a component implements [IDisposable](https://docs.microsoft.com/dotnet/api/system.idisposable), the [Dispose method](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="b9a91-218">Aşağıdaki bileşen `@implements IDisposable` ve `Dispose` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="b9a91-218">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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

## <a name="routing"></a><span data-ttu-id="b9a91-219">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="b9a91-219">Routing</span></span>

<span data-ttu-id="b9a91-220">Razor bileşenlerde yönlendirme uygulamasında erişilebilir her bileşeni için bir rota şablonu sağlayarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-220">Routing in Razor Components is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="b9a91-221">Olduğunda bir  *\*.cshtml* ile dosya bir `@page` yönergesi derlendiğinde, oluşturulan sınıfın belirli bir [RouteAttribute](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) belirten rota şablonu.</span><span class="sxs-lookup"><span data-stu-id="b9a91-221">When a *\*.cshtml* file with an `@page` directive is compiled, the generated class is given a [RouteAttribute](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) specifying the route template.</span></span> <span data-ttu-id="b9a91-222">Çalışma zamanında bileşen sınıfları ile yönlendirici arar bir `RouteAttribute` ve hangi bileşen istenen URL ile eşleşen bir rota şablonuna sahip işler.</span><span class="sxs-lookup"><span data-stu-id="b9a91-222">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="b9a91-223">Bir bileşenin birden çok yol şablonu uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-223">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="b9a91-224">Aşağıdaki bileşen isteklerine yanıt veren `/BlazorRoute` ve `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="b9a91-224">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

## <a name="route-parameters"></a><span data-ttu-id="b9a91-225">Yol parametreleri</span><span class="sxs-lookup"><span data-stu-id="b9a91-225">Route parameters</span></span>

<span data-ttu-id="b9a91-226">Sağlanan yol şablondan, rota parametrelerinin bileşenleri alabilir `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="b9a91-226">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="b9a91-227">Yönlendirici, karşılık gelen bileşen parametreleri doldurmak için rota parametrelerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="b9a91-227">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="b9a91-228">*RouteParameter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b9a91-228">*RouteParameter.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=9)]

<span data-ttu-id="b9a91-229">İsteğe bağlı parametreler desteklenmez, bu nedenle iki `@page` yönergeleri, yukarıdaki örnekte uygulanır.</span><span class="sxs-lookup"><span data-stu-id="b9a91-229">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="b9a91-230">İlk Gezinti parametresi olmadan bileşenine izin verir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-230">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="b9a91-231">İkinci `@page` yönergesi gereken `{text}` rota parametresi ve değeri atar `Text` özelliği.</span><span class="sxs-lookup"><span data-stu-id="b9a91-231">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="b9a91-232">Bir "code-behind" deneyimi için temel sınıf devralma</span><span class="sxs-lookup"><span data-stu-id="b9a91-232">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="b9a91-233">Bileşen dosyaları (*\*.cshtml*) HTML biçimlendirmeyi karışımı ve C# aynı dosyada kod işleme.</span><span class="sxs-lookup"><span data-stu-id="b9a91-233">Component files (*\*.cshtml*) mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="b9a91-234">`@inherits` Yönergesi, Razor bileşenleri uygulamalar Bileşen biçimlendirme işleme koddan ayıran bir "code-behind" deneyimi sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-234">The `@inherits` directive can be used to provide Razor Components apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="b9a91-235">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) nasıl bir bileşen bir temel sınıf devralma işlemi yapabileceğini gösterir `BlazorRocksBase`, bileşenin özellikleri ve yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="b9a91-235">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="b9a91-236">*BlazorRocks.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b9a91-236">*BlazorRocks.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?start=1&end=8)]

<span data-ttu-id="b9a91-237">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="b9a91-237">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="b9a91-238">Temel sınıfın türetilmesi `BlazorComponent`.</span><span class="sxs-lookup"><span data-stu-id="b9a91-238">The base class should derive from `BlazorComponent`.</span></span>

## <a name="razor-support"></a><span data-ttu-id="b9a91-239">Razor desteği</span><span class="sxs-lookup"><span data-stu-id="b9a91-239">Razor support</span></span>

<span data-ttu-id="b9a91-240">**Razor yönergesi**</span><span class="sxs-lookup"><span data-stu-id="b9a91-240">**Razor directives**</span></span>

<span data-ttu-id="b9a91-241">Razor yönergeleri, aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-241">Razor directives are shown in the following table.</span></span>

| <span data-ttu-id="b9a91-242">Yönergesi</span><span class="sxs-lookup"><span data-stu-id="b9a91-242">Directive</span></span> | <span data-ttu-id="b9a91-243">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b9a91-243">Description</span></span> |
| --------- | ----------- |
| [@functions](https://docs.microsoft.com/aspnet/core/mvc/views/razor#functions) | <span data-ttu-id="b9a91-244">Ekler bir C# bileşenine kod bloğu.</span><span class="sxs-lookup"><span data-stu-id="b9a91-244">Adds a C# code block to a component.</span></span> |
| `@implements` | <span data-ttu-id="b9a91-245">Oluşturulan bileşen sınıfı için bir arabirim uygular.</span><span class="sxs-lookup"><span data-stu-id="b9a91-245">Implements an interface for the generated component class.</span></span> |
| [@inherits](https://docs.microsoft.com/aspnet/core/mvc/views/razor#inherits) | <span data-ttu-id="b9a91-246">Bileşen devralan sınıf tam denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="b9a91-246">Provides full control of the class that the component inherits.</span></span> |
| [@inject](https://docs.microsoft.com/aspnet/core/mvc/views/razor#inject) | <span data-ttu-id="b9a91-247">Hizmet ekleme gelen etkinleştirir [hizmet kapsayıcı](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b9a91-247">Enables service injection from the [service container](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span></span> <span data-ttu-id="b9a91-248">Daha fazla bilgi için [görünümlere bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b9a91-248">For more information, see [Dependency injection into views](https://docs.microsoft.com/aspnet/core/mvc/views/dependency-injection).</span></span> |
| `@layout` | <span data-ttu-id="b9a91-249">Bir düzen bileşeni belirtir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-249">Specifies a layout component.</span></span> <span data-ttu-id="b9a91-250">Düzen bileşenleri, kod yinelemesi ve tutarsızlık önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b9a91-250">Layout components are used to avoid code duplication and inconsistency.</span></span> |
| [@page](https://docs.microsoft.com/aspnet/core/mvc/razor-pages#razor-pages) | <span data-ttu-id="b9a91-251">Bileşen doğrudan istekleri işleyeceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-251">Specifies that the component should handle requests directly.</span></span> <span data-ttu-id="b9a91-252">`@page` Yönergesi, bir rota ve isteğe bağlı parametreler ile belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-252">The `@page` directive can be specified with a route and optional parameters.</span></span> <span data-ttu-id="b9a91-253">Razor sayfaları aksine `@page` yönergesi üst dosyanın ilk yönerge olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="b9a91-253">Unlike Razor Pages, the `@page` directive doesn't need to be the first directive at the top of the file.</span></span> <span data-ttu-id="b9a91-254">Daha fazla bilgi için [yönlendirme](xref:razor-components/routing).</span><span class="sxs-lookup"><span data-stu-id="b9a91-254">For more information, see [Routing](xref:razor-components/routing).</span></span> |
| [@using](https://docs.microsoft.com/aspnet/core/mvc/views/razor#using) | <span data-ttu-id="b9a91-255">Ekler C# `using` yönergesini oluşturulan bileşen sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b9a91-255">Adds the C# `using` directive to the generated component class.</span></span> |
| [@addTagHelper](https://docs.microsoft.com/aspnet/core/mvc/views/razor#tag-helpers) | <span data-ttu-id="b9a91-256">Kullanma `@addTagHelper` uygulamanın derleme farklı bir derleme bir bileşeni kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="b9a91-256">Use `@addTagHelper` to use a component in a different assembly than the app's assembly.</span></span> |

<span data-ttu-id="b9a91-257">**Koşullu öznitelikleri**</span><span class="sxs-lookup"><span data-stu-id="b9a91-257">**Conditional attributes**</span></span>

<span data-ttu-id="b9a91-258">Öznitelikleri .NET değerine göre koşullu olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-258">Attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="b9a91-259">Değer ise `false` veya `null`, öznitelik işlenen değil.</span><span class="sxs-lookup"><span data-stu-id="b9a91-259">If the value is `false` or `null`,  the attribute isn't rendered.</span></span> <span data-ttu-id="b9a91-260">Değer ise `true`, öznitelik işlenen simge.</span><span class="sxs-lookup"><span data-stu-id="b9a91-260">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="b9a91-261">Aşağıdaki örnekte, `IsCompleted` belirler `checked` denetimin biçimlendirme içinde işlenir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-261">In the following example, `IsCompleted` determines if `checked` is rendered in the control's markup.</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

<span data-ttu-id="b9a91-262">Varsa `IsCompleted` olduğu `true`, onay kutusunu olarak işlenir:</span><span class="sxs-lookup"><span data-stu-id="b9a91-262">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="b9a91-263">Varsa `IsCompleted` olduğu `false`, onay kutusunu olarak işlenir:</span><span class="sxs-lookup"><span data-stu-id="b9a91-263">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="b9a91-264">**Razor hakkında daha fazla bilgi**</span><span class="sxs-lookup"><span data-stu-id="b9a91-264">**Additional information on Razor**</span></span>

<span data-ttu-id="b9a91-265">Razor hakkında daha fazla bilgi için bkz. [Razor söz dizimi başvurusu](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="b9a91-265">For more information on Razor, see the [Razor syntax reference](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span></span> <span data-ttu-id="b9a91-266">Tüm Razor özellikleri şu anda Razor bileşenlerinde kullanılabilir olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="b9a91-266">Note that not all of the features of Razor are available in Razor Components at this time.</span></span>

## <a name="raw-html"></a><span data-ttu-id="b9a91-267">Ham HTML</span><span class="sxs-lookup"><span data-stu-id="b9a91-267">Raw HTML</span></span>

<span data-ttu-id="b9a91-268">Dizeler genellikle DOM metin düğümleri kullanarak içeriyor olabilir herhangi bir biçimlendirme yok sayıldı ve düz metin kabul yani işlenir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-268">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="b9a91-269">Ham HTML oluşturmak için bir HTML içeriği kaydırma bir `MarkupString` ardından HTML veya SVG olarak ayrıştırılması ve DOM'a eklenmesi, değer</span><span class="sxs-lookup"><span data-stu-id="b9a91-269">To render raw HTML, wrap the HTML content in a `MarkupString` value, which is then parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="b9a91-270">Herhangi bir yapıda ham HTML'yi işlemeye güvenilmeyen kaynak bir **güvenlik riski** ve kaçınılmalıdır!</span><span class="sxs-lookup"><span data-stu-id="b9a91-270">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="b9a91-271">Aşağıdaki örnek, gösterir kullanarak `MarkupString` bir bileşenin işlenen çıkışı için bir statik HTML içeriği bloğunu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="b9a91-271">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@functions {
    string myMarkup = "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="b9a91-272">Şablonlu bileşenleri</span><span class="sxs-lookup"><span data-stu-id="b9a91-272">Templated components</span></span>

<span data-ttu-id="b9a91-273">Şablonlu bileşenleri bileşenin işleme mantığı bir parçası olarak kullanılabilir bir parametre olarak bir veya daha fazla kullanıcı Arabirimi şablonları kabul bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-273">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="b9a91-274">Şablonlu bileşenleri normal bileşenleri daha fazla yeniden kullanılabilir olan üst düzey bileşeni yazmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="b9a91-274">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="b9a91-275">Birkaç örnek şunlardır:</span><span class="sxs-lookup"><span data-stu-id="b9a91-275">A couple of examples include:</span></span>

* <span data-ttu-id="b9a91-276">Bir tablonun üst bilgi, satırları ve alt bilgisi için şablonları belirtmesine imkan tanıyan bir tablo bileşeni.</span><span class="sxs-lookup"><span data-stu-id="b9a91-276">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="b9a91-277">Bir listedeki öğeleri işleme için bir şablon belirtmesine imkan tanıyan bir liste bileşeni.</span><span class="sxs-lookup"><span data-stu-id="b9a91-277">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="b9a91-278">Şablon parametreleri</span><span class="sxs-lookup"><span data-stu-id="b9a91-278">Template parameters</span></span>

<span data-ttu-id="b9a91-279">Şablonlu bir bileşen türünde bir veya daha fazla bileşen parametreleri belirterek tanımlanmış `RenderFragment` veya `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="b9a91-279">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="b9a91-280">Bir işleme parça bileşen tarafından işlenen kullanıcı arabiriminin bir kesimi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="b9a91-280">A render fragment represents a segment of UI that is rendered by the component.</span></span> <span data-ttu-id="b9a91-281">Bir işleme parça, isteğe bağlı olarak işleme parça çağrıldığında belirtilebilecek bir parametre alır.</span><span class="sxs-lookup"><span data-stu-id="b9a91-281">A render fragment optionally takes a parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="b9a91-282">*Components/TableTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b9a91-282">*Components/TableTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

<span data-ttu-id="b9a91-283">Şablonlu bir bileşen kullanırken, şablon parametrelerinin parametrelerinin adları eşleşen alt öğeleri kullanılarak belirtilebilir (`TableHeader` ve `RowTemplate` aşağıdaki örnekte):</span><span class="sxs-lookup"><span data-stu-id="b9a91-283">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="b9a91-284">Şablon bağlam parametreleri</span><span class="sxs-lookup"><span data-stu-id="b9a91-284">Template context parameters</span></span>

<span data-ttu-id="b9a91-285">Bileşen Türü bağımsız değişkenleri `RenderFragment<T>` öğeleri adlı örtük bir parametre olarak geçirilen `context` (örneğin önceki kod örneğinde,'den `@context.PetId`), ancak parametre adını kullanarak değiştirebilirsiniz `Context` alt özniteliği öğe.</span><span class="sxs-lookup"><span data-stu-id="b9a91-285">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="b9a91-286">Aşağıdaki örnekte, `RowTemplate` öğenin `Context` özniteliği belirtir `pet` parametresi:</span><span class="sxs-lookup"><span data-stu-id="b9a91-286">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="b9a91-287">Alternatif olarak, belirtebileceğiniz `Context` bileşen öğesindeki özniteliği.</span><span class="sxs-lookup"><span data-stu-id="b9a91-287">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="b9a91-288">Belirtilen `Context` özniteliği için tüm belirtilen şablon parametreleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-288">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="b9a91-289">Örtük alt içeriğin içerik parametresi adını belirlemek istediğinizde bu kullanışlı olabilir (herhangi bir sarmalama olmadan alt öğesi).</span><span class="sxs-lookup"><span data-stu-id="b9a91-289">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="b9a91-290">Aşağıdaki örnekte, `Context` özniteliği görünür `TableTemplate` öğenin ve tüm şablon parametreleri için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="b9a91-290">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="b9a91-291">Genel yazılmış bileşenler</span><span class="sxs-lookup"><span data-stu-id="b9a91-291">Generic-typed components</span></span>

<span data-ttu-id="b9a91-292">Şablonlu bileşenleri çoğunlukla genel olarak yazılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b9a91-292">Templated components are often generically typed.</span></span> <span data-ttu-id="b9a91-293">Örneğin, bir genel ListView bileşeni oluşturmak için kullanılabilir `IEnumerable<T>` değerleri.</span><span class="sxs-lookup"><span data-stu-id="b9a91-293">For example, a generic ListView component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="b9a91-294">Genel bileşen tanımlamak için `@typeparam` tür parametreleri belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="b9a91-294">To define a generic component, use the `@typeparam` directive to specify type parameters.</span></span>

<span data-ttu-id="b9a91-295">*Components/ListViewTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b9a91-295">*Components/ListViewTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml?highlight=1)]

<span data-ttu-id="b9a91-296">Tür parametresi, genel yazılmış bileşenler kullanırken, mümkünse algılanır:</span><span class="sxs-lookup"><span data-stu-id="b9a91-296">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="b9a91-297">Aksi takdirde, tür parametresi açık tür parametresinin adıyla eşleşen bir özniteliği kullanılarak belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-297">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="b9a91-298">Aşağıdaki örnekte, `TItem="Pet"` türünü belirtir:</span><span class="sxs-lookup"><span data-stu-id="b9a91-298">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="b9a91-299">Geçişli değerleri ve parametreler</span><span class="sxs-lookup"><span data-stu-id="b9a91-299">Cascading values and parameters</span></span>

<span data-ttu-id="b9a91-300">Bazı senaryolarda, akış verileri kullanarak bir alt bileşen bir üst bileşeninden kullanışsız [bileşeni parametreleri](#component-parameters), özellikle birçok bileşen katmanları olduğunda.</span><span class="sxs-lookup"><span data-stu-id="b9a91-300">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="b9a91-301">Geçişli değerleri ve parametre bir üst bileşeni tüm alt öğe bileşenleri için bir değer sağlamak için kullanışlı bir yol sağlayarak bu sorunu çözer.</span><span class="sxs-lookup"><span data-stu-id="b9a91-301">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="b9a91-302">Ayrıca basamaklı değerleri ve parametreler koordine etmek, bileşenler için bir yaklaşım sağlar.</span><span class="sxs-lookup"><span data-stu-id="b9a91-302">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="b9a91-303">Tema örnek</span><span class="sxs-lookup"><span data-stu-id="b9a91-303">Theme example</span></span>

<span data-ttu-id="b9a91-304">Aşağıdaki *tema* örnek uygulama örneğinden `ThemeInfo` sınıfı, böylece tüm düğmelerin belirli bir uygulamanın parçası içinde aynı stili paylaşır bileşen hiyerarşisi akış için tema bilgileri belirtir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-304">In the following *Theme* example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="b9a91-305">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="b9a91-305">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="b9a91-306">Üst bileşeni kullanarak basamaklı bir değer sağlayabilirsiniz `CascadingValue` bileşeni.</span><span class="sxs-lookup"><span data-stu-id="b9a91-306">An ancestor component can provide a cascading value using the `CascadingValue` component.</span></span> <span data-ttu-id="b9a91-307">`CascadingValue` Bileşeni bileşen hiyerarşisi bir alt ağacı sarmalar ve o alt ağacı içinde tüm bileşenleri tek bir değer sağlar.</span><span class="sxs-lookup"><span data-stu-id="b9a91-307">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="b9a91-308">Örneğin, örnek uygulamayı tema bilgileri belirtir (`ThemeInfo`) bir düzen gövdesini oluşturan tüm bileşenlerin basamaklı bir parametre olarak uygulamanın düzenleri `@Body` özelliği.</span><span class="sxs-lookup"><span data-stu-id="b9a91-308">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="b9a91-309">`ButtonClass` bir değeri atanır `btn-success` Düzen bileşende.</span><span class="sxs-lookup"><span data-stu-id="b9a91-309">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="b9a91-310">Bu özelliği üzerinden herhangi bir alt bileşen tüketebileceği `ThemeInfo` basamaklı nesnesi.</span><span class="sxs-lookup"><span data-stu-id="b9a91-310">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="b9a91-311">*Shared/CascadingValuesParametersLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b9a91-311">*Shared/CascadingValuesParametersLayout.cshtml*:</span></span>

```cshtml
@inherits BlazorLayoutComponent
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

<span data-ttu-id="b9a91-312">Yapmak için geçişli değerlerini kullanmak, bileşenlerini kullanarak basamaklı parametreleri bildirmek `[CascadingParameter]` özniteliği veya bir dize adı değerine göre:</span><span class="sxs-lookup"><span data-stu-id="b9a91-312">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")] PermInfo Permissions { get; set; }
```

<span data-ttu-id="b9a91-313">Bir dize adı değeri bağlamayla aynı türde birden fazla basamaklı değerler varsa ve aynı alt ağacı içinde ayrılmaları gerekiyorsa geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-313">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="b9a91-314">Basamaklı değerler türüne göre geçişli parametrelerine bağlı.</span><span class="sxs-lookup"><span data-stu-id="b9a91-314">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="b9a91-315">Örnek uygulamada `CascadingValuesParametersTheme` bileşeni bağlanır `ThemeInfo` basamaklı basamaklı bir parametre değeri.</span><span class="sxs-lookup"><span data-stu-id="b9a91-315">In the sample app, the `CascadingValuesParametersTheme` component binds to the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="b9a91-316">Parametre için bir bileşen tarafından görüntülenen düğmeleri CSS sınıfının ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b9a91-316">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="b9a91-317">*Pages/CascadingValuesParametersTheme.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b9a91-317">*Pages/CascadingValuesParametersTheme.cshtml*:</span></span>

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

### <a name="tabset-example"></a><span data-ttu-id="b9a91-318">TabSet örneği</span><span class="sxs-lookup"><span data-stu-id="b9a91-318">TabSet example</span></span>

<span data-ttu-id="b9a91-319">Geçişli parametreleri, bileşen hiyerarşi işbirliği yapmak bileşenler de olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="b9a91-319">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="b9a91-320">Örneğin, aşağıdakileri dikkate alın *TabSet* örnek uygulamada örnek.</span><span class="sxs-lookup"><span data-stu-id="b9a91-320">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="b9a91-321">Örnek uygulamanın bir `ITab` uygulama sekme arabirimi:</span><span class="sxs-lookup"><span data-stu-id="b9a91-321">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="b9a91-322">`CascadingValuesParametersTabSet` Bileşen `TabSet` birkaç içeren bileşen `Tab` bileşenler:</span><span class="sxs-lookup"><span data-stu-id="b9a91-322">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

<span data-ttu-id="b9a91-323">Alt `Tab` bileşenleri olmayan açıkça için parametre olarak geçirilen `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="b9a91-323">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="b9a91-324">Bunun yerine, alt `Tab` bileşenlerdir içeriği alt parçası `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="b9a91-324">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="b9a91-325">Ancak, `TabSet` yine her biri hakkında bilmesi gereken `Tab` böylece üstbilgileri ve etkin sekmede işleyebilirsiniz. Ek kod gerek kalmadan bu koordinasyon sağlamak `TabSet` bileşen *kendisini basamaklı bir değer sağlayabilirsiniz* , ardından alındığından descendent `Tab` bileşenleri.</span><span class="sxs-lookup"><span data-stu-id="b9a91-325">However, the `TabSet` still needs to know about each `Tab` so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="b9a91-326">*Components/TabSet.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b9a91-326">*Components/TabSet.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

<span data-ttu-id="b9a91-327">Bloğun `Tab` bileşenleri yakalama içeren `TabSet` basamaklı bir parametre olarak böylece `Tab` bileşenleri eklemek için kendilerini `TabSet` ve üretileceği koordinat `Tab` etkindir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-327">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which `Tab` is active.</span></span>

<span data-ttu-id="b9a91-328">*Components/Tab.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b9a91-328">*Components/Tab.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a><span data-ttu-id="b9a91-329">Razor şablonları</span><span class="sxs-lookup"><span data-stu-id="b9a91-329">Razor templates</span></span>

<span data-ttu-id="b9a91-330">İşleme parçaları, Razor şablon söz dizimi kullanılarak tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="b9a91-330">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="b9a91-331">Razor şablonları UI parçacık tanımlayın ve aşağıdaki biçimi varsayar için bir yol sağlar:</span><span class="sxs-lookup"><span data-stu-id="b9a91-331">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="b9a91-332">Aşağıdaki örnek nasıl belirtileceğini göstermektedir `RenderFragment` ve `RenderFragment<T>` değerleri.</span><span class="sxs-lookup"><span data-stu-id="b9a91-332">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values.</span></span>

<span data-ttu-id="b9a91-333">*RazorTemplates.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b9a91-333">*RazorTemplates.cshtml*:</span></span>

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

<span data-ttu-id="b9a91-334">Şablonları şablonlu bileşenleri için bağımsız değişken olarak geçirilen veya doğrudan işlenmiş Razor kullanılarak tanımlanan parçalarının işleyin.</span><span class="sxs-lookup"><span data-stu-id="b9a91-334">Render fragments defined using Razor templates can be passed as arguments to templated components or rendered directly.</span></span> <span data-ttu-id="b9a91-335">Örneğin, önceki şablonları aşağıdaki Razor işaretlemesi ile doğrudan işlenir:</span><span class="sxs-lookup"><span data-stu-id="b9a91-335">For example, the previous templates are directly rendered with the following Razor markup:</span></span>

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

<span data-ttu-id="b9a91-336">İşlenmiş çıkışı:</span><span class="sxs-lookup"><span data-stu-id="b9a91-336">Rendered output:</span></span>

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```
