<span data-ttu-id="0bc24-101">Blazor sunucu uygulaması prerendering olduğunda, tarayıcıyla bir bağlantı kurulmadığından, JavaScript 'e çağırma gibi bazı eylemler mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="0bc24-101">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="0bc24-102">Bileşenler, ön işlenmiş olduğunda farklı şekilde işlenmesi gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="0bc24-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="0bc24-103">Tarayıcı bağlantısı kurulana kadar JavaScript birlikte çalışma çağrılarını geciktirmek için `OnAfterRenderAsync` bileşen yaşam döngüsü olayını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0bc24-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the `OnAfterRenderAsync` component lifecycle event.</span></span> <span data-ttu-id="0bc24-104">Bu olay yalnızca uygulama tam olarak işlendikten ve istemci bağlantısı kurulduktan sonra çağırılır.</span><span class="sxs-lookup"><span data-stu-id="0bc24-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<input @ref="myInput" value="Value set during render" />

@code {
    private ElementReference myInput;

    protected override void OnAfterRender(bool firstRender)
    {
        if (firstRender)
        {
            JSRuntime.InvokeAsync<object>(
                "setElementValue", myInput, "Value set after render");
        }
    }
}
```

<span data-ttu-id="0bc24-105">Aşağıdaki bileşen, prerendering ile uyumlu bir şekilde bileşenin başlatma mantığının bir parçası olarak JavaScript birlikte çalışabilirinin nasıl kullanılacağını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="0bc24-105">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="0bc24-106">Bileşen içinden bir işleme güncelleştirmesi tetiklemenin `OnAfterRenderAsync`mümkün olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="0bc24-106">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="0bc24-107">Geliştirici Bu senaryoda sonsuz bir döngü oluşturmaktan kaçınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0bc24-107">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="0bc24-108">Burada `JSRuntime.InvokeAsync` çağrılır, bileşen `ElementRef` işlenene kadar JavaScript `OnAfterRenderAsync` öğesi olmadığından, yalnızca ' de ' de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0bc24-108">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="0bc24-109">`StateHasChanged`bileşeni JavaScript birlikte çalışma çağrısından alınan yeni durumla yeniden eklemek için çağırılır.</span><span class="sxs-lookup"><span data-stu-id="0bc24-109">`StateHasChanged` is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="0bc24-110">Kod sonsuz döngü oluşturmaz çünkü `StateHasChanged` yalnızca olduğu `null`zaman `infoFromJs` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="0bc24-110">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<p>
    Get value via JS interop call:
    <strong id="val-get-by-interop">@(infoFromJs ?? "No value yet")</strong>
</p>

<p>
    Set value via JS interop call:
    <input id="val-set-by-interop" @ref="myElem" />
</p>

@code {
    private string infoFromJs;
    private ElementReference myElem;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender && infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementValue", myElem, "Hello from interop call");

            StateHasChanged();
        }
    }
}
```