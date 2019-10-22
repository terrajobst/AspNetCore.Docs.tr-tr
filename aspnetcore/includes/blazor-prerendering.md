Blazor sunucu uygulaması prerendering olduğunda, tarayıcıyla bir bağlantı kurulmadığından, JavaScript 'e çağırma gibi bazı eylemler mümkün değildir. Bileşenler, ön işlenmiş olduğunda farklı şekilde işlenmesi gerekebilir.

Tarayıcı bağlantısı kurulana kadar JavaScript birlikte çalışma çağrılarını geciktirmek için `OnAfterRenderAsync` bileşen yaşam döngüsü olayını kullanabilirsiniz. Bu olay yalnızca uygulama tam olarak işlendikten ve istemci bağlantısı kurulduktan sonra çağırılır.

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

Yukarıdaki örnek kod için, *Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_host. cshtml* (Blazor Server) öğesinin `<head>` öğesi içinde bir `setElementText` JavaScript işlevi sağlayın. İşlevi `IJSRuntime.InvokeVoidAsync` ile çağrılır ve bir değer döndürmez:

```html
<!--  -->
<script>
  window.setElementText = (element, text) => element.innerText = text;
</script>
```

> [!WARNING]
> Yukarıdaki örnek yalnızca tanıtım amacıyla Belge Nesne Modeli (DOM) değiştirir. JavaScript, Blazor 'in değişiklik izlemesini kesintiye uğradığı için çoğu senaryoda, JavaScript ile DOM 'ı doğrudan değiştirme önerilmez.

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

Yukarıdaki örnek kod için, *Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_host. cshtml* (Blazor Server) öğesinin `<head>` öğesi içinde bir `setElementText` JavaScript işlevi sağlayın. İşlevi `IJSRuntime.InvokeAsync` ile çağrılır ve bir değer döndürür:

```html
<script>
  window.setElementText = (element, text) => {
    element.innerText = text;
    return text;
  };
</script>
```

> [!WARNING]
> Yukarıdaki örnek yalnızca tanıtım amacıyla Belge Nesne Modeli (DOM) değiştirir. JavaScript, Blazor 'in değişiklik izlemesini kesintiye uğradığı için çoğu senaryoda, JavaScript ile DOM 'ı doğrudan değiştirme önerilmez.
