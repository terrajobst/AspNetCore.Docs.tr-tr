---
title: ASP.NET Core bileşen etiketi Yardımcısı
author: guardrex
ms.author: riande
description: Sayfalardaki ve görünümlerde Razor bileşenlerini işlemek için ASP.NET Core bileşen etiketi Yardımcısı 'nı nasıl kullanacağınızı öğrenin.
ms.custom: mvc
ms.date: 03/18/2020
no-loc:
- Blazor
- SignalR
uid: mvc/views/tag-helpers/builtin-th/component-tag-helper
ms.openlocfilehash: 801ceb73de5bb4ef7500624e1fbddbf96d1ab89c
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226398"
---
# <a name="component-tag-helper-in-aspnet-core"></a><span data-ttu-id="c11ff-103">ASP.NET Core bileşen etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="c11ff-103">Component Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="c11ff-104">[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="c11ff-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c11ff-105">Bir sayfadan veya görünümden bir bileşeni işlemek için [bileşen etiketi yardımcısını](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper)kullanın.</span><span class="sxs-lookup"><span data-stu-id="c11ff-105">To render a component from a page or view, use the [Component Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).</span></span>

<span data-ttu-id="c11ff-106">Aşağıdaki bileşen etiketi Yardımcısı, `Counter` bileşenini bir sayfada veya görünümde işler:</span><span class="sxs-lookup"><span data-stu-id="c11ff-106">The following Component Tag Helper renders the `Counter` component in a page or view:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

<span data-ttu-id="c11ff-107">Bileşen etiketi Yardımcısı, parametreleri bileşenlere de geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="c11ff-107">The Component Tag Helper can also pass parameters to components.</span></span> <span data-ttu-id="c11ff-108">Onay kutusu etiketinin rengini ve boyutunu ayarlayan aşağıdaki `ColorfulCheckbox` bileşenini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="c11ff-108">Consider the following `ColorfulCheckbox` component that sets the check box label's color and size:</span></span>

```razor
<label style="font-size:@(Size)px;color:@Color">
    <input @bind="Value"
           id="survey" 
           name="blazor" 
           type="checkbox" />
    Enjoying Blazor?
</label>

@code {
    [Parameter]
    public bool Value { get; set; }

    [Parameter]
    public int Size { get; set; } = 8;

    [Parameter]
    public string Color { get; set; }

    protected override void OnInitialized()
    {
        Size += 10;
    }
}
```

<span data-ttu-id="c11ff-109">`Size` (`int`) ve `Color` (`string`) [bileşen parametreleri](xref:blazor/components#component-parameters) bileşen etiketi Yardımcısı tarafından ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="c11ff-109">The `Size` (`int`) and `Color` (`string`) [component parameters](xref:blazor/components#component-parameters) can be set by the Component Tag Helper:</span></span>

```cshtml
<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

<span data-ttu-id="c11ff-110">Aşağıdaki HTML sayfada veya görünümde işlenir:</span><span class="sxs-lookup"><span data-stu-id="c11ff-110">The following HTML is rendered in the page or view:</span></span>

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

<span data-ttu-id="c11ff-111">Tırnak içine alınan bir dizeyi geçirmek, önceki örnekteki `param-Color` gösterildiği gibi [açık bir Razor ifadesi](xref:mvc/views/razor#explicit-razor-expressions)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="c11ff-111">Passing a quoted string requires an [explicit Razor expression](xref:mvc/views/razor#explicit-razor-expressions), as shown for `param-Color` in the preceding example.</span></span> <span data-ttu-id="c11ff-112">Öznitelik bir `object` türü olduğundan, bir `string` tür değeri için Razor ayrıştırma davranışı `param-*` bir özniteliğe uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="c11ff-112">The Razor parsing behavior for a `string` type value doesn't apply to a `param-*` attribute because the attribute is an `object` type.</span></span>

<span data-ttu-id="c11ff-113">Parametre türünün JSON serileştirilebilir olması gerekir, bu, genellikle türün bir varsayılan oluşturucuya ve ayarlanabilir özelliklere sahip olması anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="c11ff-113">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="c11ff-114">Örneğin, `Size` ve `Color` türleri, JSON seri hale getirici tarafından desteklenen basit türler (`int` ve `string`) olduğundan, önceki örnekte `Size` ve `Color` için bir değer belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c11ff-114">For example, you can specify a value for `Size` and `Color` in the preceding example because the types of `Size` and `Color` are primitive types (`int` and `string`), which are supported by the JSON serializer.</span></span>

<span data-ttu-id="c11ff-115"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode>, bileşenin şunları yapıp kullanmadığını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="c11ff-115"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the component:</span></span>

* <span data-ttu-id="c11ff-116">, Sayfaya ön gönderilir.</span><span class="sxs-lookup"><span data-stu-id="c11ff-116">Is prerendered into the page.</span></span>
* <span data-ttu-id="c11ff-117">, Sayfada statik HTML olarak veya Kullanıcı aracısından bir Blazor uygulamasını önyüklemek için gerekli bilgileri içeriyorsa.</span><span class="sxs-lookup"><span data-stu-id="c11ff-117">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| <span data-ttu-id="c11ff-118">Oluşturma modu</span><span class="sxs-lookup"><span data-stu-id="c11ff-118">Render Mode</span></span> | <span data-ttu-id="c11ff-119">Açıklama</span><span class="sxs-lookup"><span data-stu-id="c11ff-119">Description</span></span> |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | <span data-ttu-id="c11ff-120">Bileşeni statik HTML olarak işler ve Blazor sunucusu uygulaması için bir işaret içerir.</span><span class="sxs-lookup"><span data-stu-id="c11ff-120">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="c11ff-121">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c11ff-121">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | <span data-ttu-id="c11ff-122">Blazor sunucusu uygulaması için bir işaret oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c11ff-122">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="c11ff-123">Bileşen çıkışı dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="c11ff-123">Output from the component isn't included.</span></span> <span data-ttu-id="c11ff-124">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c11ff-124">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | <span data-ttu-id="c11ff-125">Bileşeni statik HTML olarak işler.</span><span class="sxs-lookup"><span data-stu-id="c11ff-125">Renders the component into static HTML.</span></span> |

<span data-ttu-id="c11ff-126">Sayfalar ve görünümler bileşenleri kullanırken, listesiyse doğru değildir.</span><span class="sxs-lookup"><span data-stu-id="c11ff-126">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="c11ff-127">Bileşenler, kısmi görünümler ve bölümler gibi görünüm ve sayfaya özgü özellikleri kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="c11ff-127">Components can't use view- and page-specific features, such as partial views and sections.</span></span> <span data-ttu-id="c11ff-128">Bileşen içindeki kısmi görünümden mantığı kullanmak için kısmi görünüm mantığını bir bileşene ayırın.</span><span class="sxs-lookup"><span data-stu-id="c11ff-128">To use logic from a partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="c11ff-129">Statik HTML sayfasından sunucu bileşenleri işleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="c11ff-129">Rendering server components from a static HTML page isn't supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c11ff-130">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c11ff-130">Additional resources</span></span>

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components>
