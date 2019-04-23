---
ms.openlocfilehash: 03ed846171097e30e4954cf632875c0249697665
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983064"
---
Blazor sunucu tarafı uygulama prerendering karşın, bir tarayıcı bağlantı kuran taşınmadığından çünkü çağırma JavaScript gibi belirli eylemleri mümkün değildir. Bileşenleri prerendered, farklı şekilde işlemek gerekebilir.

Birlikte çalışma JavaScript gecikme, tarayıcı bağlantısı kurulduktan sonra kullanabileceğiniz kadar çağıran `OnAfterRenderAsync` bileşen yaşam döngüsü olayı. Bu olay, yalnızca uygulama tam olarak işlenir ve istemci bağlantı kurulduktan sonra çağrılır.

Farklı içerik olup olmadığını uygulama şu anda içerik prerendering olduğundan üzerinde temel koşullu olarak işlemek için `IsConnected` özelliği `IComponentContext` hizmeti. Sunucu tarafı çalıştırırken `IsConnected` yalnızca döndürür `true` etkin bir istemcinin bağlantısı varsa. Her zaman döndürür `true` istemci-tarafı çalışırken.

```cshtml
@page "/isconnected-example"
@using Microsoft.AspNetCore.Components.Services
@inject IComponentContext ComponentContext

<h1>IsConnected Example</h1>

<p>
    Current state:
    <strong id="connected-state">
        @(ComponentContext.IsConnected ? "connected" : "not connected")
    </strong>
</p>

<p>
    Clicks:
    <strong id="count">@count</strong>
    <button id="increment-count" onclick="@(() => count++)">Click me</button>
</p>

@functions {
    private int count;
}
```
