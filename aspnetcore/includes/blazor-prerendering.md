---
no-loc:
- Blazor
- SignalR
ms.openlocfilehash: 5f3e22e04fe18149ec5a8acb42f42a8ef83a7664
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159916"
---
<span data-ttu-id="27550-101">Bir Blazor sunucusu uygulaması prerendering olsa da, tarayıcıyla bir bağlantı kurulmadığından, JavaScript 'e çağırma gibi bazı eylemler mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="27550-101">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="27550-102">Bileşenler, ön işlenmiş olduğunda farklı şekilde işlenmesi gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="27550-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="27550-103">Tarayıcı bağlantısı kurulana kadar JavaScript birlikte çalışma çağrılarını geciktirmek için [Onafterrenderasync bileşen yaşam döngüsü olayını](xref:blazor/lifecycle#after-component-render)kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27550-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the [OnAfterRenderAsync component lifecycle event](xref:blazor/lifecycle#after-component-render).</span></span> <span data-ttu-id="27550-104">Bu olay yalnızca uygulama tam olarak işlendikten ve istemci bağlantısı kurulduktan sonra çağırılır.</span><span class="sxs-lookup"><span data-stu-id="27550-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<div @ref="divElement">Text during render</div>

@code {
    private ElementReference divElement;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            await JSRuntime.InvokeVoidAsync(
                "setElementText", divElement, "Text after render");
        }
    }
}
```

<span data-ttu-id="27550-105">Yukarıdaki örnek kod için, *Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_Host. cshtml* (Blazor Server) `<head>` öğesi içinde bir `setElementText` JavaScript işlevi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="27550-105">For the preceding example code, provide a `setElementText` JavaScript function inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> <span data-ttu-id="27550-106">İşlevi `IJSRuntime.InvokeVoidAsync` ile çağrılır ve bir değer döndürmez:</span><span class="sxs-lookup"><span data-stu-id="27550-106">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

```html
<script>
  window.setElementText = (element, text) => element.innerText = text;
</script>
```

> [!WARNING]
> <span data-ttu-id="27550-107">Yukarıdaki örnek yalnızca tanıtım amacıyla Belge Nesne Modeli (DOM) değiştirir.</span><span class="sxs-lookup"><span data-stu-id="27550-107">The preceding example modifies the Document Object Model (DOM) directly for demonstration purposes only.</span></span> <span data-ttu-id="27550-108">JavaScript 'in Blazordeğişiklik izlemesini kesintiye uğratacağından, Çoğu senaryoda DOM 'ın JavaScript ile doğrudan değiştirilmesi önerilmez.</span><span class="sxs-lookup"><span data-stu-id="27550-108">Directly modifying the DOM with JavaScript isn't recommended in most scenarios because JavaScript can interfere with Blazor's change tracking.</span></span>

<span data-ttu-id="27550-109">Aşağıdaki bileşen, prerendering ile uyumlu bir şekilde bileşenin başlatma mantığının bir parçası olarak JavaScript birlikte çalışabilirinin nasıl kullanılacağını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="27550-109">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="27550-110">Bileşeni, `OnAfterRenderAsync`içinden bir işleme güncelleştirmesi tetiklemenin mümkün olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="27550-110">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="27550-111">Geliştirici Bu senaryoda sonsuz bir döngü oluşturmaktan kaçınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="27550-111">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="27550-112">`JSRuntime.InvokeAsync` çağrıldığında, bileşen işlenene kadar hiçbir JavaScript öğesi olmadığından, `ElementRef` yalnızca `OnAfterRenderAsync` için kullanılır ve daha önceki bir yaşam döngüsü yönteminde değil.</span><span class="sxs-lookup"><span data-stu-id="27550-112">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="27550-113">JavaScript birlikte çalışma çağrısından alınan yeni durumla birlikte bileşeni yeniden sağlamak için [Statehaschanged](xref:blazor/lifecycle#state-changes) çağrılır.</span><span class="sxs-lookup"><span data-stu-id="27550-113">[StateHasChanged](xref:blazor/lifecycle#state-changes) is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="27550-114">`StateHasChanged` yalnızca `infoFromJs` `null`olduğunda çağrıldığı için, kod sonsuz bir döngü oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="27550-114">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<p>
    Get value via JS interop call:
    <strong id="val-get-by-interop">@(infoFromJs ?? "No value yet")</strong>
</p>

Set value via JS interop call:
<div id="val-set-by-interop" @ref="divElement"></div>

@code {
    private string infoFromJs;
    private ElementReference divElement;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender && infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementText", divElement, "Hello from interop call!");

            StateHasChanged();
        }
    }
}
```

<span data-ttu-id="27550-115">Yukarıdaki örnek kod için, *Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_Host. cshtml* (Blazor Server) `<head>` öğesi içinde bir `setElementText` JavaScript işlevi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="27550-115">For the preceding example code, provide a `setElementText` JavaScript function inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> <span data-ttu-id="27550-116">İşlevi `IJSRuntime.InvokeAsync` ile çağrılır ve bir değer döndürür:</span><span class="sxs-lookup"><span data-stu-id="27550-116">The function is called with `IJSRuntime.InvokeAsync` and returns a value:</span></span>

```html
<script>
  window.setElementText = (element, text) => {
    element.innerText = text;
    return text;
  };
</script>
```

> [!WARNING]
> <span data-ttu-id="27550-117">Yukarıdaki örnek yalnızca tanıtım amacıyla Belge Nesne Modeli (DOM) değiştirir.</span><span class="sxs-lookup"><span data-stu-id="27550-117">The preceding example modifies the Document Object Model (DOM) directly for demonstration purposes only.</span></span> <span data-ttu-id="27550-118">JavaScript 'in Blazordeğişiklik izlemesini kesintiye uğratacağından, Çoğu senaryoda DOM 'ın JavaScript ile doğrudan değiştirilmesi önerilmez.</span><span class="sxs-lookup"><span data-stu-id="27550-118">Directly modifying the DOM with JavaScript isn't recommended in most scenarios because JavaScript can interfere with Blazor's change tracking.</span></span>
