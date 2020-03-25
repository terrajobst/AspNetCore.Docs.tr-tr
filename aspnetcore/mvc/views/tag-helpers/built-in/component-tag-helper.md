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
# <a name="component-tag-helper-in-aspnet-core"></a>ASP.NET Core bileşen etiketi Yardımcısı

[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından

Bir sayfadan veya görünümden bir bileşeni işlemek için [bileşen etiketi yardımcısını](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper)kullanın.

Aşağıdaki bileşen etiketi Yardımcısı, `Counter` bileşenini bir sayfada veya görünümde işler:

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

Bileşen etiketi Yardımcısı, parametreleri bileşenlere de geçirebilir. Onay kutusu etiketinin rengini ve boyutunu ayarlayan aşağıdaki `ColorfulCheckbox` bileşenini göz önünde bulundurun:

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

`Size` (`int`) ve `Color` (`string`) [bileşen parametreleri](xref:blazor/components#component-parameters) bileşen etiketi Yardımcısı tarafından ayarlanabilir:

```cshtml
<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

Aşağıdaki HTML sayfada veya görünümde işlenir:

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

Tırnak içine alınan bir dizeyi geçirmek, önceki örnekteki `param-Color` gösterildiği gibi [açık bir Razor ifadesi](xref:mvc/views/razor#explicit-razor-expressions)gerektirir. Öznitelik bir `object` türü olduğundan, bir `string` tür değeri için Razor ayrıştırma davranışı `param-*` bir özniteliğe uygulanmaz.

Parametre türünün JSON serileştirilebilir olması gerekir, bu, genellikle türün bir varsayılan oluşturucuya ve ayarlanabilir özelliklere sahip olması anlamına gelir. Örneğin, `Size` ve `Color` türleri, JSON seri hale getirici tarafından desteklenen basit türler (`int` ve `string`) olduğundan, önceki örnekte `Size` ve `Color` için bir değer belirtebilirsiniz.

<xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode>, bileşenin şunları yapıp kullanmadığını yapılandırır:

* , Sayfaya ön gönderilir.
* , Sayfada statik HTML olarak veya Kullanıcı aracısından bir Blazor uygulamasını önyüklemek için gerekli bilgileri içeriyorsa.

| Oluşturma modu | Açıklama |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | Bileşeni statik HTML olarak işler ve Blazor sunucusu uygulaması için bir işaret içerir. Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır. |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | Blazor sunucusu uygulaması için bir işaret oluşturur. Bileşen çıkışı dahil değildir. Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır. |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | Bileşeni statik HTML olarak işler. |

Sayfalar ve görünümler bileşenleri kullanırken, listesiyse doğru değildir. Bileşenler, kısmi görünümler ve bölümler gibi görünüm ve sayfaya özgü özellikleri kullanamaz. Bileşen içindeki kısmi görünümden mantığı kullanmak için kısmi görünüm mantığını bir bileşene ayırın.

Statik HTML sayfasından sunucu bileşenleri işleme desteklenmiyor.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components>
