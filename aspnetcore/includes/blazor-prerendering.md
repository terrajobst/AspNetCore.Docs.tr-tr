<span data-ttu-id="8a1de-101">Blazor sunucu tarafı uygulama prerendering karşın, bir tarayıcı bağlantı kuran taşınmadığından çünkü çağırma JavaScript gibi belirli eylemleri mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="8a1de-101">While a Blazor server-side app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="8a1de-102">Bileşenleri prerendered, farklı şekilde işlemek gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="8a1de-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="8a1de-103">Birlikte çalışma JavaScript gecikme, tarayıcı bağlantısı kurulduktan sonra kullanabileceğiniz kadar çağıran `OnAfterRenderAsync` bileşen yaşam döngüsü olayı.</span><span class="sxs-lookup"><span data-stu-id="8a1de-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the `OnAfterRenderAsync` component lifecycle event.</span></span> <span data-ttu-id="8a1de-104">Bu olay, yalnızca uygulama tam olarak işlenir ve istemci bağlantı kurulduktan sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="8a1de-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<input @ref="myInput" value="Value set during render" />

@code {
    ElementRef myInput;

    protected override void OnAfterRender()
    {
        JSRuntime.InvokeAsync<object>(
            "setElementValue", myInput, "Value set after render");
    }
}
```

<span data-ttu-id="8a1de-105">Bu aşağıdaki bileşen prerendering ile uyumlu bir şekilde bir bileşenin başlatma mantığının parçası olarak JavaScript birlikte çalışma nasıl yapılacağı açıklanır.</span><span class="sxs-lookup"><span data-stu-id="8a1de-105">This following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span>

<span data-ttu-id="8a1de-106">Bileşen içinde bir işleme güncelleştirmesi tetiklemek mümkün olduğunu gösterir. `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="8a1de-106">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="8a1de-107">Bu senaryo için.</span><span class="sxs-lookup"><span data-stu-id="8a1de-107">For this scenario.</span></span> <span data-ttu-id="8a1de-108">Geliştirici, sonsuz bir döngüye oluşturmaktan kaçının gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a1de-108">the developer must avoid creating an infinite loop.</span></span>

<span data-ttu-id="8a1de-109">Burada `JSRuntime.InvokeAsync` çağrıldığında `ElementRef` yalnızca kullanılan `OnAfterRenderAsync` ve önceki hiçbir yaşam döngüsü yöntemi olduğundan herhangi bir JavaScript öğe kadar bileşen oluşturulduğunda.</span><span class="sxs-lookup"><span data-stu-id="8a1de-109">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="8a1de-110">`StateHasChanged` JavaScript birlikte çalışma çağrısından alınan yeni durumu ile birlikte bileşen rerender için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="8a1de-110">`StateHasChanged` is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="8a1de-111">Kodun sonsuz bir döngüye oluşturmaz, çünkü `StateHasChanged` yalnızca aldığında çağrılan `infoFromJs` olduğu `null`.</span><span class="sxs-lookup"><span data-stu-id="8a1de-111">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
@inject IComponentContext ComponentContext
@inject IJSRuntime JSRuntime

<p>
    Get value via JS interop call:
    <strong id="val-get-by-interop">@(infoFromJs ?? "No value yet")</strong>
</p>

<p>
    Set value via JS interop call:
    <input id="val-set-by-interop" @ref="@myElem" />
</p>

@code {
    string infoFromJs;
    ElementRef myElem;

    protected override async Task OnAfterRenderAsync()
    {
        // TEMPORARY: Currently we need this guard to avoid making the interop
        // call during prerendering. Soon this will be unnecessary because we
        // will change OnAfterRenderAsync so that it won't run during the
        // prerendering phase.
        if (!ComponentContext.IsConnected)
        {
            return;
        }

        if (infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementValue", myElem, "Hello from interop call");

            StateHasChanged();
        }
    }
}
```

<span data-ttu-id="8a1de-112">Farklı içerik olup olmadığını uygulama şu anda içerik prerendering olduğundan üzerinde temel koşullu olarak işlemek için `IsConnected` özelliği `IComponentContext` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="8a1de-112">To conditionally render different content based on whether the app is currently prerendering content, use the `IsConnected` property on the `IComponentContext` service.</span></span> <span data-ttu-id="8a1de-113">Sunucu tarafı çalıştırırken `IsConnected` yalnızca döndürür `true` etkin bir istemcinin bağlantısı varsa.</span><span class="sxs-lookup"><span data-stu-id="8a1de-113">When running server-side, `IsConnected` only returns `true` if there's an active connection to the client.</span></span> <span data-ttu-id="8a1de-114">Her zaman döndürür `true` istemci-tarafı çalışırken.</span><span class="sxs-lookup"><span data-stu-id="8a1de-114">It always returns `true` when running client-side.</span></span>

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
    <button id="increment-count" @onclick="@(() => count++)">Click me</button>
</p>

@code {
    private int count;
}
```
