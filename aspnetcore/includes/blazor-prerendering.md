---
ms.openlocfilehash: 03ed846171097e30e4954cf632875c0249697665
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983064"
---
<span data-ttu-id="1e6f7-101">Blazor sunucu tarafı uygulama prerendering karşın, bir tarayıcı bağlantı kuran taşınmadığından çünkü çağırma JavaScript gibi belirli eylemleri mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="1e6f7-101">While a Blazor server-side app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="1e6f7-102">Bileşenleri prerendered, farklı şekilde işlemek gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="1e6f7-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="1e6f7-103">Birlikte çalışma JavaScript gecikme, tarayıcı bağlantısı kurulduktan sonra kullanabileceğiniz kadar çağıran `OnAfterRenderAsync` bileşen yaşam döngüsü olayı.</span><span class="sxs-lookup"><span data-stu-id="1e6f7-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the `OnAfterRenderAsync` component lifecycle event.</span></span> <span data-ttu-id="1e6f7-104">Bu olay, yalnızca uygulama tam olarak işlenir ve istemci bağlantı kurulduktan sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1e6f7-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

<span data-ttu-id="1e6f7-105">Farklı içerik olup olmadığını uygulama şu anda içerik prerendering olduğundan üzerinde temel koşullu olarak işlemek için `IsConnected` özelliği `IComponentContext` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="1e6f7-105">To conditionally render different content based on whether the app is currently prerendering content, use the `IsConnected` property on the `IComponentContext` service.</span></span> <span data-ttu-id="1e6f7-106">Sunucu tarafı çalıştırırken `IsConnected` yalnızca döndürür `true` etkin bir istemcinin bağlantısı varsa.</span><span class="sxs-lookup"><span data-stu-id="1e6f7-106">When running server-side, `IsConnected` only returns `true` if there's an active connection to the client.</span></span> <span data-ttu-id="1e6f7-107">Her zaman döndürür `true` istemci-tarafı çalışırken.</span><span class="sxs-lookup"><span data-stu-id="1e6f7-107">It always returns `true` when running client-side.</span></span>

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
