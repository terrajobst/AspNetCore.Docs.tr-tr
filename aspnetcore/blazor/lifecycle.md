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
ms.openlocfilehash: 280ea832f492852e425e3e15c61cac54fd1e39d6
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74879678"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a><span data-ttu-id="20f58-103">ASP.NET Core Blazor yaşam döngüsü</span><span class="sxs-lookup"><span data-stu-id="20f58-103">ASP.NET Core Blazor lifecycle</span></span>

<span data-ttu-id="20f58-104">, [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="20f58-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="20f58-105">Blazor Framework zaman uyumlu ve zaman uyumsuz yaşam döngüsü yöntemlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="20f58-105">The Blazor framework includes synchronous and asynchronous lifecycle methods.</span></span> <span data-ttu-id="20f58-106">Bileşen başlatma ve işleme sırasında bileşenlerde ek işlemler gerçekleştirmek için yaşam döngüsü yöntemlerini geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="20f58-106">Override lifecycle methods to perform additional operations on components during component initialization and rendering.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="20f58-107">Yaşam döngüsü yöntemleri</span><span class="sxs-lookup"><span data-stu-id="20f58-107">Lifecycle methods</span></span>

### <a name="component-initialization-methods"></a><span data-ttu-id="20f58-108">Bileşen başlatma yöntemleri</span><span class="sxs-lookup"><span data-stu-id="20f58-108">Component initialization methods</span></span>

<span data-ttu-id="20f58-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> ve <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> bir bileşeni başlatan kodu yürütün.</span><span class="sxs-lookup"><span data-stu-id="20f58-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> execute code that initializes a component.</span></span> <span data-ttu-id="20f58-110">Bu yöntemlere yalnızca bileşen ilk örneği oluşturulduğunda bir kez çağırılır.</span><span class="sxs-lookup"><span data-stu-id="20f58-110">These methods are only called one time when the component is first instantiated.</span></span>

<span data-ttu-id="20f58-111">Zaman uyumsuz bir işlem gerçekleştirmek için işlem üzerinde `OnInitializedAsync` ve `await` anahtar sözcüğünü kullanın:</span><span class="sxs-lookup"><span data-stu-id="20f58-111">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="20f58-112">Bileşen başlatma sırasında zaman uyumsuz çalışma `OnInitializedAsync` yaşam döngüsü olayında gerçekleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="20f58-112">Asynchronous work during component initialization must occur during the `OnInitializedAsync` lifecycle event.</span></span>

<span data-ttu-id="20f58-113">Zaman uyumlu bir işlem için `OnInitialized`kullanın:</span><span class="sxs-lookup"><span data-stu-id="20f58-113">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

### <a name="before-parameters-are-set"></a><span data-ttu-id="20f58-114">Parametreler ayarlanmadan önce</span><span class="sxs-lookup"><span data-stu-id="20f58-114">Before parameters are set</span></span>

<span data-ttu-id="20f58-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*>, işleme ağacındaki bileşenin üst öğesi tarafından sağlanan parametreleri ayarlar:</span><span class="sxs-lookup"><span data-stu-id="20f58-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> sets parameters supplied by the component's parent in the render tree:</span></span>

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<span data-ttu-id="20f58-116"><xref:Microsoft.AspNetCore.Components.ParameterView>, `SetParametersAsync` her çağrıldığında parametre değerleri kümesinin tamamını içerir.</span><span class="sxs-lookup"><span data-stu-id="20f58-116"><xref:Microsoft.AspNetCore.Components.ParameterView> contains the entire set of parameter values each time `SetParametersAsync` is called.</span></span>

<span data-ttu-id="20f58-117">`SetParametersAsync` varsayılan uygulanması her bir özelliğin değerini, `ParameterView`karşılık gelen bir değere sahip `[Parameter]` veya `[CascadingParameter]` özniteliğiyle ayarlar.</span><span class="sxs-lookup"><span data-stu-id="20f58-117">The default implementation of `SetParametersAsync` sets the value of each property with the `[Parameter]` or `[CascadingParameter]` attribute that has a corresponding value in the `ParameterView`.</span></span> <span data-ttu-id="20f58-118">`ParameterView` karşılık gelen bir değere sahip olmayan parametreler değiştirilmeden bırakılır.</span><span class="sxs-lookup"><span data-stu-id="20f58-118">Parameters that don't have a corresponding value in `ParameterView` are left unchanged.</span></span>

<span data-ttu-id="20f58-119">`base.SetParametersAync` çağrılmazsa, özel kod gelen parametreler değerini gerekli herhangi bir şekilde yorumlayabilir.</span><span class="sxs-lookup"><span data-stu-id="20f58-119">If `base.SetParametersAync` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="20f58-120">Örneğin, sınıftaki özelliklere gelen parametreleri atama gereksinimi yoktur.</span><span class="sxs-lookup"><span data-stu-id="20f58-120">For example, there's no requirement to assign the incoming parameters to the properties on the class.</span></span>

### <a name="after-parameters-are-set"></a><span data-ttu-id="20f58-121">Parametreler ayarlandıktan sonra</span><span class="sxs-lookup"><span data-stu-id="20f58-121">After parameters are set</span></span>

<span data-ttu-id="20f58-122"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> ve <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> bir bileşen üst öğeden parametreleri aldığında ve değerler özelliklerine atandığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="20f58-122"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="20f58-123">Bu yöntemler bileşen başlatıldıktan sonra ve her yeni parametre değeri belirtildiğinde yürütülür:</span><span class="sxs-lookup"><span data-stu-id="20f58-123">These methods are executed after component initialization and each time new parameter values are specified:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="20f58-124">`OnParametersSetAsync` yaşam döngüsü olayında parametre ve özellik değerleri uygulanırken zaman uyumsuz iş olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="20f58-124">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a><span data-ttu-id="20f58-125">Bileşen oluşturulduktan sonra</span><span class="sxs-lookup"><span data-stu-id="20f58-125">After component render</span></span>

<span data-ttu-id="20f58-126"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> ve <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> bir bileşen işlemeyi tamamladıktan sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="20f58-126"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> are called after a component has finished rendering.</span></span> <span data-ttu-id="20f58-127">Öğe ve bileşen başvuruları bu noktada doldurulur.</span><span class="sxs-lookup"><span data-stu-id="20f58-127">Element and component references are populated at this point.</span></span> <span data-ttu-id="20f58-128">İşlenmiş DOM öğelerinde çalışan üçüncü taraf JavaScript kitaplıklarını etkinleştirme gibi, işlenmiş içeriği kullanarak ek başlatma adımları gerçekleştirmek için bu aşamayı kullanın.</span><span class="sxs-lookup"><span data-stu-id="20f58-128">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="20f58-129">`OnAfterRenderAsync` ve `OnAfterRender`için `firstRender` parametresi:</span><span class="sxs-lookup"><span data-stu-id="20f58-129">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender`:</span></span>

* <span data-ttu-id="20f58-130">, Bileşen örneği ilk kez işlendiğinde `true` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="20f58-130">Is set to `true` the first time that the component instance is rendered.</span></span>
* <span data-ttu-id="20f58-131">Başlatma işinin yalnızca bir kez gerçekleştirildiğinden emin olmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="20f58-131">Can be used to ensure that initialization work is only performed once.</span></span>

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
> <span data-ttu-id="20f58-132">`OnAfterRenderAsync` yaşam döngüsü olayında, işleme hemen sonra zaman uyumsuz çalışma gerçekleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="20f58-132">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>
>
> <span data-ttu-id="20f58-133">`OnAfterRenderAsync`bir <xref:System.Threading.Tasks.Task> döndürseniz bile, çerçeve bu görev tamamlandıktan sonra bileşen için başka bir işleme çevrimi zamanlayamaz.</span><span class="sxs-lookup"><span data-stu-id="20f58-133">Even if you return a <xref:System.Threading.Tasks.Task> from `OnAfterRenderAsync`, the framework doesn't schedule a further render cycle for your component once that task completes.</span></span> <span data-ttu-id="20f58-134">Bu, sonsuz bir işleme döngüsünden kaçınmaktır.</span><span class="sxs-lookup"><span data-stu-id="20f58-134">This is to avoid an infinite render loop.</span></span> <span data-ttu-id="20f58-135">Diğer yaşam döngüsü yöntemlerinden farklı olduğundan, döndürülen görev tamamlandığında daha fazla işleme döngüsü zamanlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="20f58-135">It's different from the other lifecycle methods, which schedule a further render cycle once the returned task completes.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<span data-ttu-id="20f58-136">`OnAfterRender` ve `OnAfterRenderAsync` *sunucuda prerendering çağrılmaz.*</span><span class="sxs-lookup"><span data-stu-id="20f58-136">`OnAfterRender` and `OnAfterRenderAsync` *aren't called when prerendering on the server.*</span></span>

### <a name="suppress-ui-refreshing"></a><span data-ttu-id="20f58-137">UI yenilemeyi bastır</span><span class="sxs-lookup"><span data-stu-id="20f58-137">Suppress UI refreshing</span></span>

<span data-ttu-id="20f58-138">UI yenilemeyi gizlemek için <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="20f58-138">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> to suppress UI refreshing.</span></span> <span data-ttu-id="20f58-139">Uygulama `true`döndürürse, Kullanıcı arabirimi yenilenir:</span><span class="sxs-lookup"><span data-stu-id="20f58-139">If the implementation returns `true`, the UI is refreshed:</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<span data-ttu-id="20f58-140">`ShouldRender` bileşen her işlendiğinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="20f58-140">`ShouldRender` is called each time the component is rendered.</span></span>

<span data-ttu-id="20f58-141">`ShouldRender` geçersiz kılınsa bile, bileşen her zaman ilk olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="20f58-141">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

## <a name="state-changes"></a><span data-ttu-id="20f58-142">Durum değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="20f58-142">State changes</span></span>

<span data-ttu-id="20f58-143"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> bileşene durumunu değiştiğini bildirir.</span><span class="sxs-lookup"><span data-stu-id="20f58-143"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifies the component that its state has changed.</span></span> <span data-ttu-id="20f58-144">Uygun olduğunda, `StateHasChanged` çağırmak bileşenin yeniden yönlendirilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="20f58-144">When applicable, calling `StateHasChanged` causes the component to be rerendered.</span></span>

## <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="20f58-145">İşleme sırasında tamamlanmamış zaman uyumsuz eylemleri işle</span><span class="sxs-lookup"><span data-stu-id="20f58-145">Handle incomplete async actions at render</span></span>

<span data-ttu-id="20f58-146">Yaşam döngüsü olaylarında gerçekleştirilen zaman uyumsuz eylemler, bileşen işlenmeden önce tamamlanmamış olabilir.</span><span class="sxs-lookup"><span data-stu-id="20f58-146">Asynchronous actions performed in lifecycle events might not have completed before the component is rendered.</span></span> <span data-ttu-id="20f58-147">Yaşam döngüsü yöntemi yürütülürken nesneler `null` veya verilerle tamamen doldurulmuş olabilir.</span><span class="sxs-lookup"><span data-stu-id="20f58-147">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="20f58-148">Nesnelerin başlatıldığını onaylamak için işleme mantığı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="20f58-148">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="20f58-149">Nesneler `null`yer tutucu Kullanıcı arabirimi öğelerini (örneğin, bir yükleme iletisi) işleme.</span><span class="sxs-lookup"><span data-stu-id="20f58-149">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="20f58-150">Blazor şablonlarının `FetchData` bileşeninde, `OnInitializedAsync` tahmin verileri alma (`forecasts`) için geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="20f58-150">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="20f58-151">`forecasts` `null`olduğunda, kullanıcıya bir yükleme iletisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="20f58-151">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="20f58-152">`OnInitializedAsync` tamamlandığında döndürülen `Task`, bileşen güncelleştirilmiş durumla yeniden yapılır.</span><span class="sxs-lookup"><span data-stu-id="20f58-152">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="20f58-153">Blazor Server şablonundaki *Pages/FetchData. Razor* :</span><span class="sxs-lookup"><span data-stu-id="20f58-153">*Pages/FetchData.razor* in the Blazor Server template:</span></span>

[!code-cshtml[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="20f58-154">IDisposable ile bileşen atma</span><span class="sxs-lookup"><span data-stu-id="20f58-154">Component disposal with IDisposable</span></span>

<span data-ttu-id="20f58-155">Bir bileşen <xref:System.IDisposable>uygularsa, bileşen kullanıcı arabiriminden kaldırıldığında [Dispose yöntemi](/dotnet/standard/garbage-collection/implementing-dispose) çağırılır.</span><span class="sxs-lookup"><span data-stu-id="20f58-155">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="20f58-156">Aşağıdaki bileşen `@implements IDisposable` ve `Dispose` yöntemini kullanır:</span><span class="sxs-lookup"><span data-stu-id="20f58-156">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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
> <span data-ttu-id="20f58-157">`Dispose` <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> çağrısı desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="20f58-157">Calling <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` isn't supported.</span></span> <span data-ttu-id="20f58-158">`StateHasChanged` oluşturucunun bir parçası olarak çağrılabilir, bu nedenle bu noktada UI güncelleştirmelerinin kullanılması desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="20f58-158">`StateHasChanged` might be invoked as part of tearing down the renderer, so requesting UI updates at that point isn't supported.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="20f58-159">Hataları işleme</span><span class="sxs-lookup"><span data-stu-id="20f58-159">Handle errors</span></span>

<span data-ttu-id="20f58-160">Yaşam döngüsü yöntemi yürütme sırasında hataları işleme hakkında bilgi için bkz. <xref:blazor/handle-errors#lifecycle-methods>.</span><span class="sxs-lookup"><span data-stu-id="20f58-160">For information on handling errors during lifecycle method execution, see <xref:blazor/handle-errors#lifecycle-methods>.</span></span>
