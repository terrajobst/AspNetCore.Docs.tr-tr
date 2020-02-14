---
title: ASP.NET Core Blazor yaşam döngüsü
author: guardrex
description: ASP.NET Core Blazor uygulamalarında Razor bileşeni yaşam döngüsü yöntemlerini nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/lifecycle
ms.openlocfilehash: ecacd0a9728cbefd716e9dc7cd8a8c62f3df6e0d
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213395"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a>ASP.NET Core Blazor yaşam döngüsü

, [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27) tarafından

Blazor Framework zaman uyumlu ve zaman uyumsuz yaşam döngüsü yöntemlerini içerir. Bileşen başlatma ve işleme sırasında bileşenlerde ek işlemler gerçekleştirmek için yaşam döngüsü yöntemlerini geçersiz kılın.

## <a name="lifecycle-methods"></a>Yaşam döngüsü yöntemleri

### <a name="component-initialization-methods"></a>Bileşen başlatma yöntemleri

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> ve <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*>, bileşen ilk parametrelerini ana bileşeninden aldıktan sonra başlatıldığında çağrılır. Bileşen zaman uyumsuz bir işlem gerçekleştirdiğinde ve işlem tamamlandığında yenilenmesi gerektiğinde `OnInitializedAsync` kullanın.

Zaman uyumlu bir işlem için `OnInitialized`geçersiz kılın:

```csharp
protected override void OnInitialized()
{
    ...
}
```

Zaman uyumsuz bir işlem gerçekleştirmek için `OnInitializedAsync` geçersiz kılın ve işlem üzerinde `await` anahtar sözcüğünü kullanın:

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

[içerik](xref:blazor/hosting-model-configuration#render-mode) çağrısına yönelik Blazor sunucu uygulamaları **_iki kez_** `OnInitializedAsync`:

* Bir kez, bileşen sayfanın bir parçası olarak başlangıçta statik olarak işlendiğinde.
* Tarayıcı sunucuya geri bir bağlantı kurduğunda ikinci bir zaman.

`OnInitializedAsync` geliştirici kodunun iki kez çalışmasını engellemek için, [prerendering sonrasında durum bilgisi olan yeniden bağlanma](#stateful-reconnection-after-prerendering) bölümüne bakın.

Bir Blazor sunucusu uygulaması prerendering olsa da, tarayıcıyla bir bağlantı kurulmadığından, JavaScript 'e çağırma gibi bazı eylemler mümkün değildir. Bileşenler, ön işlenmiş olduğunda farklı şekilde işlenmesi gerekebilir. Daha fazla bilgi için bkz. [uygulamanın ne zaman prerendering](#detect-when-the-app-is-prerendering) bölümüne bakın.

### <a name="before-parameters-are-set"></a>Parametreler ayarlanmadan önce

<xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*>, işleme ağacındaki bileşenin üst öğesi tarafından sağlanan parametreleri ayarlar:

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<xref:Microsoft.AspNetCore.Components.ParameterView>, `SetParametersAsync` her çağrıldığında parametre değerleri kümesinin tamamını içerir.

`SetParametersAsync` varsayılan uygulanması her bir özelliğin değerini, `ParameterView`karşılık gelen bir değere sahip `[Parameter]` veya `[CascadingParameter]` özniteliğiyle ayarlar. `ParameterView` karşılık gelen bir değere sahip olmayan parametreler değiştirilmeden bırakılır.

`base.SetParametersAync` çağrılmazsa, özel kod gelen parametreler değerini gerekli herhangi bir şekilde yorumlayabilir. Örneğin, sınıftaki özelliklere gelen parametreleri atama gereksinimi yoktur.

### <a name="after-parameters-are-set"></a>Parametreler ayarlandıktan sonra

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> ve <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> şu şekilde adlandırılır:

* Bileşen başlatıldığında ve üst bileşeninden ilk parametre kümesini aldığında.
* Üst bileşen yeniden oluşturup şunları sağlar:
  * Yalnızca en az bir parametresinin değiştiği, bilinen temel sabit türler.
  * Tüm karmaşık türsüz parametreler. Çerçeve, karmaşık yazılmış bir parametrenin değerlerinin dahili olarak değişip değişmediğini bilmez, bu yüzden parametre kümesini değiştirilmiş olarak değerlendirir.

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> `OnParametersSetAsync` yaşam döngüsü olayında parametre ve özellik değerleri uygulanırken zaman uyumsuz iş olması gerekir.

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a>Bileşen oluşturulduktan sonra

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> ve <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> bir bileşen işlemeyi tamamladıktan sonra çağrılır. Öğe ve bileşen başvuruları bu noktada doldurulur. İşlenmiş DOM öğelerinde çalışan üçüncü taraf JavaScript kitaplıklarını etkinleştirme gibi, işlenmiş içeriği kullanarak ek başlatma adımları gerçekleştirmek için bu aşamayı kullanın.

`OnAfterRenderAsync` ve `OnAfterRender`için `firstRender` parametresi:

* , Bileşen örneği ilk kez işlendiğinde `true` olarak ayarlanır.
* Başlatma işinin yalnızca bir kez gerçekleştirildiğinden emin olmak için kullanılabilir.

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

> [!NOTE]
> `OnAfterRenderAsync` yaşam döngüsü olayında, işleme hemen sonra zaman uyumsuz çalışma gerçekleşmelidir.
>
> `OnAfterRenderAsync`bir <xref:System.Threading.Tasks.Task> döndürseniz bile, çerçeve bu görev tamamlandıktan sonra bileşen için başka bir işleme çevrimi zamanlayamaz. Bu, sonsuz bir işleme döngüsünden kaçınmaktır. Diğer yaşam döngüsü yöntemlerinden farklı olduğundan, döndürülen görev tamamlandığında daha fazla işleme döngüsü zamanlayabilirsiniz.

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

`OnAfterRender` ve `OnAfterRenderAsync` *sunucuda prerendering çağrılmaz.*

### <a name="suppress-ui-refreshing"></a>UI yenilemeyi bastır

UI yenilemeyi gizlemek için <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> geçersiz kılın. Uygulama `true`döndürürse, Kullanıcı arabirimi yenilenir:

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

`ShouldRender` bileşen her işlendiğinde çağrılır.

`ShouldRender` geçersiz kılınsa bile, bileşen her zaman ilk olarak işlenir.

## <a name="state-changes"></a>Durum değişiklikleri

<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> bileşene durumunu değiştiğini bildirir. Uygun olduğunda, `StateHasChanged` çağırmak bileşenin yeniden yönlendirilmesine neden olur.

## <a name="handle-incomplete-async-actions-at-render"></a>İşleme sırasında tamamlanmamış zaman uyumsuz eylemleri işle

Yaşam döngüsü olaylarında gerçekleştirilen zaman uyumsuz eylemler, bileşen işlenmeden önce tamamlanmamış olabilir. Yaşam döngüsü yöntemi yürütülürken nesneler `null` veya verilerle tamamen doldurulmuş olabilir. Nesnelerin başlatıldığını onaylamak için işleme mantığı sağlayın. Nesneler `null`yer tutucu Kullanıcı arabirimi öğelerini (örneğin, bir yükleme iletisi) işleme.

Blazor şablonlarının `FetchData` bileşeninde, `OnInitializedAsync` tahmin verileri alma (`forecasts`) için geçersiz kılınır. `forecasts` `null`olduğunda, kullanıcıya bir yükleme iletisi görüntülenir. `OnInitializedAsync` tamamlandığında döndürülen `Task`, bileşen güncelleştirilmiş durumla yeniden yapılır.

Blazor Server şablonundaki *Pages/FetchData. Razor* :

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a>IDisposable ile bileşen atma

Bir bileşen <xref:System.IDisposable>uygularsa, bileşen kullanıcı arabiriminden kaldırıldığında [Dispose yöntemi](/dotnet/standard/garbage-collection/implementing-dispose) çağırılır. Aşağıdaki bileşen `@implements IDisposable` ve `Dispose` yöntemini kullanır:

```razor
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

> [!NOTE]
> `Dispose` <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> çağrısı desteklenmiyor. `StateHasChanged` oluşturucunun bir parçası olarak çağrılabilir, bu nedenle bu noktada UI güncelleştirmelerinin kullanılması desteklenmez.

## <a name="handle-errors"></a>Hataları işleme

Yaşam döngüsü yöntemi yürütme sırasında hataları işleme hakkında bilgi için bkz. <xref:blazor/handle-errors#lifecycle-methods>.

## <a name="stateful-reconnection-after-prerendering"></a>Prerendering sonrasında durum bilgisi olan yeniden bağlanma

`RenderMode` `ServerPrerendered`Blazor sunucu uygulamasında, bileşen başlangıçta sayfanın bir parçası olarak statik olarak işlenir. Tarayıcı sunucuya geri bir bağlantı kurduğunda, bileşen *yeniden*işlenir ve bileşen artık etkileşimli olur. Bileşeni başlatmak için [Onbaşlatılmış {Async}](xref:blazor/lifecycle#component-initialization-methods) yaşam döngüsü yöntemi varsa, yöntem *iki kez*yürütülür:

* Bileşen statik olarak önceden kullanılırken.
* Sunucu bağlantısı kurulduktan sonra.

Bu, bileşen son işlendiğinde Kullanıcı arabiriminde görünen verilerde fark edilebilir bir değişikliğe neden olabilir.

Blazor sunucu uygulamasında çift işleme senaryosunu önlemek için:

* Prerendering sırasında durumu önbelleğe almak için kullanılabilecek bir tanımlayıcı geçirin ve uygulamayı yeniden başlattıktan sonra durumu alma.
* Bileşen durumunu kaydetmek için prerendering sırasında tanımlayıcıyı kullanın.
* Önbelleğe alınan durumu almak için prerendering öğesinden sonra tanımlayıcıyı kullanın.

Aşağıdaki kod, Çift işlemeyi engelleyen şablon tabanlı Blazor sunucu uygulamasındaki güncelleştirilmiş bir `WeatherForecastService` gösterir:

```csharp
public class WeatherForecastService
{
    private static readonly string[] _summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = _summaries[rng.Next(_summaries.Length)]
            }).ToArray();
        });
    }
}
```

`RenderMode`hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-model-configuration#render-mode>.

## <a name="detect-when-the-app-is-prerendering"></a>Uygulamanın ne zaman prerendering olduğunu Algıla

[!INCLUDE[](~/includes/blazor-prerendering.md)]
