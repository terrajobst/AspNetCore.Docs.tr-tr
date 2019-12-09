---
title: ASP.NET Core Blazor yaşam döngüsü
author: guardrex
description: ASP.NET Core Blazor uygulamalarında Razor bileşeni yaşam döngüsü yöntemlerini nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/lifecycle
ms.openlocfilehash: e600e7c7a6a8c646a655520bd5c127f2cd662753
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944037"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a>ASP.NET Core Blazor yaşam döngüsü

, [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27) tarafından

Blazor Framework zaman uyumlu ve zaman uyumsuz yaşam döngüsü yöntemlerini içerir. Bileşen başlatma ve işleme sırasında bileşenlerde ek işlemler gerçekleştirmek için yaşam döngüsü yöntemlerini geçersiz kılın.

## <a name="lifecycle-methods"></a>Yaşam döngüsü yöntemleri

### <a name="component-initialization-methods"></a>Bileşen başlatma yöntemleri

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> ve <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> bir bileşeni başlatan kodu yürütün. Bu yöntemlere yalnızca bileşen ilk örneği oluşturulduğunda bir kez çağırılır.

Zaman uyumsuz bir işlem gerçekleştirmek için işlem üzerinde `OnInitializedAsync` ve `await` anahtar sözcüğünü kullanın:

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

> [!NOTE]
> Bileşen başlatma sırasında zaman uyumsuz çalışma `OnInitializedAsync` yaşam döngüsü olayında gerçekleşmelidir.

Zaman uyumlu bir işlem için `OnInitialized`kullanın:

```csharp
protected override void OnInitialized()
{
    ...
}
```

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

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> ve <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> bir bileşen üst öğeden parametreleri aldığında ve değerler özelliklerine atandığında çağrılır. Bu yöntemler bileşen başlatıldıktan sonra ve her yeni parametre değeri belirtildiğinde yürütülür:

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

```csharp
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
