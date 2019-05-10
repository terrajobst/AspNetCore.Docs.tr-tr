Blazor sunucu tarafı uygulama prerendering karşın, bir tarayıcı bağlantı kuran taşınmadığından çünkü çağırma JavaScript gibi belirli eylemleri mümkün değildir. Bileşenleri prerendered, farklı şekilde işlemek gerekebilir.

Birlikte çalışma JavaScript gecikme, tarayıcı bağlantısı kurulduktan sonra kullanabileceğiniz kadar çağıran `OnAfterRenderAsync` bileşen yaşam döngüsü olayı. Bu olay, yalnızca uygulama tam olarak işlenir ve istemci bağlantı kurulduktan sonra çağrılır.

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<input ref="myInput" value="Value set during render" />

@functions {
    ElementRef myInput;

    protected override void OnAfterRender()
    {
        JSRuntime.InvokeAsync<object>(
            "setElementValue", myInput, "Value set after render");
    }
}
```

Bu aşağıdaki bileşen prerendering ile uyumlu bir şekilde bir bileşenin başlatma mantığının parçası olarak JavaScript birlikte çalışma nasıl yapılacağı açıklanır.

Bileşen içinde bir işleme güncelleştirmesi tetiklemek mümkün olduğunu gösterir. `OnAfterRenderAsync`. Bu senaryo için. Geliştirici, sonsuz bir döngüye oluşturmaktan kaçının gerekir.

Burada `JSRuntime.InvokeAsync` çağrıldığında `ElementRef` yalnızca kullanılan `OnAfterRenderAsync` ve önceki hiçbir yaşam döngüsü yöntemi olduğundan herhangi bir JavaScript öğe kadar bileşen oluşturulduğunda.

`StateHasChanged` JavaScript birlikte çalışma çağrısından alınan yeni durumu ile birlikte bileşen rerender için çağrılır. Kodun sonsuz bir döngüye oluşturmaz, çünkü `StateHasChanged` yalnızca aldığında çağrılan `infoFromJs` olduğu `null`.

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
    <input id="val-set-by-interop" ref="@myElem" />
</p>

@functions {
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
