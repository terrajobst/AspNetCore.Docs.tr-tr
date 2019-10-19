Blazor sunucu uygulaması prerendering olduğunda, tarayıcıyla bir bağlantı kurulmadığından, JavaScript 'e çağırma gibi bazı eylemler mümkün değildir. Bileşenler, ön işlenmiş olduğunda farklı şekilde işlenmesi gerekebilir.

Tarayıcı bağlantısı kurulana kadar JavaScript birlikte çalışma çağrılarını geciktirmek için `OnAfterRenderAsync` bileşen yaşam döngüsü olayını kullanabilirsiniz. Bu olay yalnızca uygulama tam olarak işlendikten ve istemci bağlantısı kurulduktan sonra çağırılır.

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
            JSRuntime.InvokeVoidAsync(
                "setElementValue", myInput, "Value set after render");
        }
    }
}
```

Aşağıdaki bileşen, prerendering ile uyumlu bir şekilde bileşenin başlatma mantığının bir parçası olarak JavaScript birlikte çalışabilirinin nasıl kullanılacağını göstermektedir. Bileşeni, `OnAfterRenderAsync` içinden bir işleme güncelleştirmesi tetiklemenin mümkün olduğunu gösterir. Geliştirici Bu senaryoda sonsuz bir döngü oluşturmaktan kaçınmalıdır.

@No__t_0 çağrıldığında, bileşen işlenene kadar hiçbir JavaScript öğesi olmadığından, `ElementRef` yalnızca `OnAfterRenderAsync` için kullanılır ve daha önceki bir yaşam döngüsü yönteminde değil.

`StateHasChanged`, bileşeni JavaScript birlikte çalışma çağrısından alınan yeni durumla yeniden eklemek için çağrılır. @No__t_0 yalnızca `infoFromJs` `null` olduğunda çağrıldığı için, kod sonsuz bir döngü oluşturmaz.

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
