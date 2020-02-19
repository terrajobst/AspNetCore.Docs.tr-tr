---
title: ASP.NET Core Blazor JavaScript birlikte çalışabilirliği
author: guardrex
description: Blazor uygulamalarında JavaScript 'ten .NET ve .NET yöntemlerinden JavaScript işlevlerini çağırmayı öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/javascript-interop
ms.openlocfilehash: d681eea5a5e876912bd614fba8ea45a464844496
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447171"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a><span data-ttu-id="26f65-103">ASP.NET Core Blazor JavaScript birlikte çalışabilirliği</span><span class="sxs-lookup"><span data-stu-id="26f65-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="26f65-104">Sağlayan [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="26f65-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="26f65-105">Blazor bir uygulama, JavaScript kodundan .NET ve .NET yöntemlerinden JavaScript işlevlerini çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="26f65-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="26f65-106">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="26f65-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="26f65-107">.NET metotlarından JavaScript işlevlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="26f65-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="26f65-108">Bir JavaScript işlevini çağırmak için .NET kodunun gerekli olduğu durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="26f65-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="26f65-109">Örneğin, JavaScript çağrısı, tarayıcı özelliklerini veya bir JavaScript kitaplığından uygulamaya yönelik işlevselliği sunabilir.</span><span class="sxs-lookup"><span data-stu-id="26f65-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span> <span data-ttu-id="26f65-110">Bu senaryoya *JavaScript birlikte çalışabilirliği* (*js birlikte çalışma*) denir.</span><span class="sxs-lookup"><span data-stu-id="26f65-110">This scenario is called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="26f65-111">.NET 'ten JavaScript 'i çağırmak için `IJSRuntime` soyutlamasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="26f65-111">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="26f65-112">JS birlikte çalışma çağrıları vermek için `IJSRuntime` soyutlamasını bileşeninizin içine ekler.</span><span class="sxs-lookup"><span data-stu-id="26f65-112">To issue JS interop calls, inject the `IJSRuntime` abstraction in your component.</span></span> <span data-ttu-id="26f65-113">`InvokeAsync<T>` yöntemi, herhangi bir sayıda JSON seri hale getirilebilir bağımsız değişkenle birlikte çağırmak istediğiniz JavaScript işlevi için bir tanımlayıcı alır.</span><span class="sxs-lookup"><span data-stu-id="26f65-113">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="26f65-114">İşlev tanımlayıcısı, genel kapsama (`window`) göredir.</span><span class="sxs-lookup"><span data-stu-id="26f65-114">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="26f65-115">`window.someScope.someFunction`çağırmak isterseniz, tanımlayıcı `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="26f65-115">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="26f65-116">Çağrılmadan önce işlevi kaydetmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="26f65-116">There's no need to register the function before it's called.</span></span> <span data-ttu-id="26f65-117">Dönüş türü `T` ayrıca seri hale getirilebilir JSON olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="26f65-117">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="26f65-118">`T`, döndürülen JSON türüyle en iyi eşleşen .NET türüyle eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="26f65-118">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="26f65-119">Prerendering özellikli Blazor Server uygulamaları için, ilk prerendering sırasında JavaScript 'e çağırma mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="26f65-119">For Blazor Server apps with prerendering enabled, calling into JavaScript isn't possible during the initial prerendering.</span></span> <span data-ttu-id="26f65-120">JavaScript birlikte çalışma çağrılarının, tarayıcıyla bağlantı kurulana kadar ertelenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="26f65-120">JavaScript interop calls must be deferred until after the connection with the browser is established.</span></span> <span data-ttu-id="26f65-121">Daha fazla bilgi için, [Blazor bir uygulamanın ne zaman prerendering olduğunu Algıla](#detect-when-a-blazor-app-is-prerendering) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="26f65-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="26f65-122">Aşağıdaki örnek, deneysel bir JavaScript tabanlı kod çözücüsü olan [Textdecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)tabanlıdır.</span><span class="sxs-lookup"><span data-stu-id="26f65-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="26f65-123">Örnek, bir C# yöntemden JavaScript işlevinin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="26f65-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="26f65-124">JavaScript işlevi bir C# yöntemden bir bayt dizisi kabul eder, dizinin kodunu çözer ve görüntülenecek metni bileşene döndürür.</span><span class="sxs-lookup"><span data-stu-id="26f65-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="26f65-125">*Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_Host. cshtml* (Blazor Server) `<head>` öğesi içinde, geçirilen bir dizinin kodunu çözmek Için `TextDecoder` kullanan bir JavaScript işlevi sağlayın ve kodu çözülen değeri döndürün:</span><span class="sxs-lookup"><span data-stu-id="26f65-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="26f65-126">Önceki örnekte gösterilen kod gibi JavaScript kodu, betik dosyasına yönelik bir başvuruya sahip bir JavaScript dosyasından ( *. js*) de yüklenebilir:</span><span class="sxs-lookup"><span data-stu-id="26f65-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="26f65-127">Aşağıdaki bileşen:</span><span class="sxs-lookup"><span data-stu-id="26f65-127">The following component:</span></span>

* <span data-ttu-id="26f65-128">Bir bileşen düğmesi (**diziyi Dönüştür**) seçildiğinde `JSRuntime` kullanarak `convertArray` JavaScript işlevini çağırır.</span><span class="sxs-lookup"><span data-stu-id="26f65-128">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="26f65-129">JavaScript işlevi çağrıldıktan sonra, geçirilen dizi bir dizeye dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="26f65-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="26f65-130">Dize, görüntüleme için bileşene döndürülür.</span><span class="sxs-lookup"><span data-stu-id="26f65-130">The string is returned to the component for display.</span></span>

[!code-razor[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a><span data-ttu-id="26f65-131">IJSRuntime kullanımı</span><span class="sxs-lookup"><span data-stu-id="26f65-131">Use of IJSRuntime</span></span>

<span data-ttu-id="26f65-132">`IJSRuntime` soyutlamasını kullanmak için aşağıdaki yaklaşımlardan birini benimseyin:</span><span class="sxs-lookup"><span data-stu-id="26f65-132">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="26f65-133">`IJSRuntime` soyutlama Razor bileşenine ( *. Razor*) ekleme:</span><span class="sxs-lookup"><span data-stu-id="26f65-133">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-razor[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="26f65-134">*Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_Host. cshtml* (Blazor Server) `<head>` öğesinin Içinde, bir `handleTickerChanged` JavaScript işlevi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="26f65-134">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="26f65-135">İşlevi `IJSRuntime.InvokeVoidAsync` ile çağrılır ve bir değer döndürmez:</span><span class="sxs-lookup"><span data-stu-id="26f65-135">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="26f65-136">`IJSRuntime` soyutlamasını bir sınıfa ( *. cs*) Ekle:</span><span class="sxs-lookup"><span data-stu-id="26f65-136">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="26f65-137">*Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_Host. cshtml* (Blazor Server) `<head>` öğesinin Içinde, bir `handleTickerChanged` JavaScript işlevi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="26f65-137">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="26f65-138">İşlevi `JSRuntime.InvokeAsync` ile çağrılır ve bir değer döndürür:</span><span class="sxs-lookup"><span data-stu-id="26f65-138">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="26f65-139">[Buildrendertree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic)ile dinamik içerik oluşturma için `[Inject]` özniteliğini kullanın:</span><span class="sxs-lookup"><span data-stu-id="26f65-139">For dynamic content generation with [BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="26f65-140">Bu konuya eşlik eden istemci tarafı örnek uygulamada, Kullanıcı girişi almak ve bir hoş geldiniz iletisi göstermek üzere DOM ile etkileşime geçen uygulama için iki JavaScript işlevi mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="26f65-140">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="26f65-141">`showPrompt` &ndash; Kullanıcı girişini kabul etmek için bir istem üretir (kullanıcının adı) ve çağıranın adını döndürür.</span><span class="sxs-lookup"><span data-stu-id="26f65-141">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="26f65-142">`displayWelcome` &ndash;, çağıran bir `id` `welcome`bir DOM nesnesine bir hoş geldiniz iletisi atar.</span><span class="sxs-lookup"><span data-stu-id="26f65-142">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="26f65-143">*Wwwroot/Examplejsınterop. js*:</span><span class="sxs-lookup"><span data-stu-id="26f65-143">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="26f65-144">JavaScript dosyasına başvuran `<script>` etiketini *Wwwroot/index.html* dosyasında (Blazor WebAssembly) veya *Pages/_Host. cshtml* dosyasında (Blazor Server) yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="26f65-144">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="26f65-145">*Wwwroot/index.html* (Blazor WebAssembly):</span><span class="sxs-lookup"><span data-stu-id="26f65-145">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

<span data-ttu-id="26f65-146">*Pages/_Host. cshtml* (Blazor sunucusu):</span><span class="sxs-lookup"><span data-stu-id="26f65-146">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

<span data-ttu-id="26f65-147">`<script>` etiketi dinamik olarak güncelleştirilemediğinden bir `<script>` etiketini bileşen dosyasına yerleştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="26f65-147">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="26f65-148">.NET yöntemleri, `IJSRuntime.InvokeAsync<T>`çağırarak *Examplejsınterop. js* dosyasında JavaScript işlevleriyle birlikte çalışır.</span><span class="sxs-lookup"><span data-stu-id="26f65-148">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="26f65-149">`IJSRuntime` soyutlama, Blazor sunucu senaryolarına izin vermek için zaman uyumsuzdur.</span><span class="sxs-lookup"><span data-stu-id="26f65-149">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="26f65-150">Uygulama bir Blazor Weelsembly uygulaması ise ve bir JavaScript işlevini zaman uyumlu olarak çağırmak istiyorsanız, `IJSInProcessRuntime` ve bunun yerine `Invoke<T>` çağırın.</span><span class="sxs-lookup"><span data-stu-id="26f65-150">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="26f65-151">Çoğu JS birlikte çalışma kitaplıklarının, kitaplıkların tüm senaryolarda kullanılabilir olmasını sağlamak için zaman uyumsuz API 'Leri kullanmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="26f65-151">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="26f65-152">Örnek uygulama, JS birlikte çalışabilirliği göstermek için bir bileşeni içerir.</span><span class="sxs-lookup"><span data-stu-id="26f65-152">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="26f65-153">Bileşen:</span><span class="sxs-lookup"><span data-stu-id="26f65-153">The component:</span></span>

* <span data-ttu-id="26f65-154">Bir JavaScript istemi aracılığıyla Kullanıcı girişini alır.</span><span class="sxs-lookup"><span data-stu-id="26f65-154">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="26f65-155">İşlemek için bileşene metni döndürür.</span><span class="sxs-lookup"><span data-stu-id="26f65-155">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="26f65-156">Bir hoş geldiniz iletisini göstermek için DOM ile etkileşime sahip ikinci bir JavaScript işlevini çağırır.</span><span class="sxs-lookup"><span data-stu-id="26f65-156">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="26f65-157">*Pages/Jsınterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="26f65-157">*Pages/JSInterop.razor*:</span></span>

```razor
@page "/JSInterop"
@using BlazorSample.JsInteropClasses
@inject IJSRuntime JSRuntime

<h1>JavaScript Interop</h1>

<h2>Invoke JavaScript functions from .NET methods</h2>

<button type="button" class="btn btn-primary" @onclick="TriggerJsPrompt">
    Trigger JavaScript Prompt
</button>

<h3 id="welcome" style="color:green;font-style:italic"></h3>

@code {
    public async Task TriggerJsPrompt()
    {
        // showPrompt is implemented in wwwroot/exampleJsInterop.js
        var name = await JSRuntime.InvokeAsync<string>(
                "exampleJsFunctions.showPrompt",
                "What's your name?");
        // displayWelcome is implemented in wwwroot/exampleJsInterop.js
        await JSRuntime.InvokeVoidAsync(
                "exampleJsFunctions.displayWelcome",
                $"Hello {name}! Welcome to Blazor!");
    }
}
```

1. <span data-ttu-id="26f65-158">`TriggerJsPrompt`, bileşenin **tetikleyicisi JavaScript istem** düğmesi seçilerek yürütüldüğünde, *Wwwroot/Examplejsınterop. js* dosyasında verilen JavaScript `showPrompt` işlevi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="26f65-158">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="26f65-159">`showPrompt` işlevi, HTML kodlu ve bileşene döndürülen kullanıcı girişini (kullanıcının adı) kabul eder.</span><span class="sxs-lookup"><span data-stu-id="26f65-159">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="26f65-160">Bileşen, kullanıcının adını `name`yerel bir değişkende depolar.</span><span class="sxs-lookup"><span data-stu-id="26f65-160">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="26f65-161">`name` depolanan dize, hoş geldiniz iletisini bir başlık etiketine işleyen `displayWelcome`bir JavaScript işlevine iletilen bir hoş geldiniz iletisine dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="26f65-161">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="26f65-162">Void JavaScript işlevini çağırın</span><span class="sxs-lookup"><span data-stu-id="26f65-162">Call a void JavaScript function</span></span>

<span data-ttu-id="26f65-163">[Void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) veya [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) döndüren JavaScript işlevleri `IJSRuntime.InvokeVoidAsync`ile çağırılır.</span><span class="sxs-lookup"><span data-stu-id="26f65-163">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a><span data-ttu-id="26f65-164">Blazor bir uygulamanın ne zaman prerendering olduğunu Algıla</span><span class="sxs-lookup"><span data-stu-id="26f65-164">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="26f65-165">Öğelere başvuruları yakala</span><span class="sxs-lookup"><span data-stu-id="26f65-165">Capture references to elements</span></span>

<span data-ttu-id="26f65-166">Bazı JS birlikte çalışma senaryoları HTML öğelerine başvurular gerektirir.</span><span class="sxs-lookup"><span data-stu-id="26f65-166">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="26f65-167">Örneğin, bir kullanıcı arabirimi kitaplığı başlatma için bir öğe başvurusu gerektirebilir veya `focus` veya `play`gibi bir öğe üzerinde komut benzeri API 'Ler çağırmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="26f65-167">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="26f65-168">Aşağıdaki yaklaşımı kullanarak bir bileşen içindeki HTML öğelerine başvuruları yakalayın:</span><span class="sxs-lookup"><span data-stu-id="26f65-168">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="26f65-169">HTML öğesine bir `@ref` özniteliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="26f65-169">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="26f65-170">Adı `@ref` özniteliği değeri ile eşleşen `ElementReference` türünde bir alan tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="26f65-170">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="26f65-171">Aşağıdaki örnek, `username` `<input>` öğesine bir başvuru yakalama göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="26f65-171">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="26f65-172">Yalnızca Blazoretkileşimde bulunmayan boş bir öğenin içeriğini bulunmamalıdır için bir öğe başvurusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="26f65-172">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="26f65-173">Bu senaryo, bir 3. taraf API 'SI öğeye içerik sağladığı zaman yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="26f65-173">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="26f65-174">Blazor öğesiyle etkileşmediği için, Blazoröğesi ve DOM gösterimi arasında bir çakışma olabilir.</span><span class="sxs-lookup"><span data-stu-id="26f65-174">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="26f65-175">Aşağıdaki örnekte, Blazor, bu öğenin liste öğelerini (`<li>`) doldurmak üzere DOM ile etkileşimde bulunduğundan, sıralanmamış listenin (`ul`) içeriğini () zaman *zaman aşmaktır* .</span><span class="sxs-lookup"><span data-stu-id="26f65-175">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
>
> ```razor
> <ul ref="MyList">
>     @foreach (var item in Todos)
>     {
>         <li>@item.Text</li>
>     }
> </ul>
> ```
>
> <span data-ttu-id="26f65-176">JS birlikte çalışma öğesi, öğe `MyList` içeriğini değiştiriyorsa ve Blazor SLA 'ya uygulamaya çalışırsa, diffler DOM ile eşleşmez.</span><span class="sxs-lookup"><span data-stu-id="26f65-176">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="26f65-177">.NET kodu açısından düşünüldüğünde, `ElementReference` donuk bir tanıtıcıdır.</span><span class="sxs-lookup"><span data-stu-id="26f65-177">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="26f65-178">`ElementReference` ile yapabileceğiniz *tek* şey, JS birlikte çalışma yoluyla JavaScript koduna geçiş yapar.</span><span class="sxs-lookup"><span data-stu-id="26f65-178">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="26f65-179">Bunu yaptığınızda, JavaScript tarafı kodu normal DOM API 'Leri ile kullanılabilecek bir `HTMLElement` örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="26f65-179">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="26f65-180">Örneğin, aşağıdaki kod bir öğe üzerinde odağı ayarlamaya izin veren bir .NET genişletme yöntemi tanımlar:</span><span class="sxs-lookup"><span data-stu-id="26f65-180">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="26f65-181">*Examplejsınterop. js*:</span><span class="sxs-lookup"><span data-stu-id="26f65-181">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="26f65-182">Değer döndürmeyen bir JavaScript işlevini çağırmak için `IJSRuntime.InvokeVoidAsync`kullanın.</span><span class="sxs-lookup"><span data-stu-id="26f65-182">To call a JavaScript function that doesn't return a value, use `IJSRuntime.InvokeVoidAsync`.</span></span> <span data-ttu-id="26f65-183">Aşağıdaki kod, yakalanan `ElementReference`önceki JavaScript işlevini çağırarak Kullanıcı adı girişi üzerinde odağı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="26f65-183">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured `ElementReference`:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="26f65-184">Bir genişletme yöntemi kullanmak için `IJSRuntime` örneğini alan bir statik genişletme yöntemi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="26f65-184">To use an extension method, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="26f65-185">`Focus` yöntemi doğrudan nesne üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="26f65-185">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="26f65-186">Aşağıdaki örnek, `Focus` yönteminin `JsInteropClasses` ad alanından kullanılabildiğini varsayar:</span><span class="sxs-lookup"><span data-stu-id="26f65-186">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="26f65-187">`username` değişkeni yalnızca bileşen işlendikten sonra doldurulur.</span><span class="sxs-lookup"><span data-stu-id="26f65-187">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="26f65-188">Doldurulmamış bir `ElementReference` JavaScript koduna geçirilirse, JavaScript kodu bir `null`değeri alır.</span><span class="sxs-lookup"><span data-stu-id="26f65-188">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="26f65-189">Bileşen işlemeyi tamamladıktan sonra öğe başvurularını değiştirmek için (bir öğe üzerinde ilk odağı ayarlamak için) [Onafterrenderasync veya OnAfterRender bileşen yaşam döngüsü yöntemlerini](xref:blazor/lifecycle#after-component-render)kullanın.</span><span class="sxs-lookup"><span data-stu-id="26f65-189">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="26f65-190">Genel türlerle çalışırken ve bir değer döndürürken, [Valuetask\<t >](xref:System.Threading.Tasks.ValueTask`1)kullanın:</span><span class="sxs-lookup"><span data-stu-id="26f65-190">When working with generic types and returning a value, use [ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="26f65-191">`GenericMethod` doğrudan nesne üzerinde bir tür ile çağırılır.</span><span class="sxs-lookup"><span data-stu-id="26f65-191">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="26f65-192">Aşağıdaki örnek, `GenericMethod` `JsInteropClasses` ad alanından kullanılabilir olduğunu varsayar:</span><span class="sxs-lookup"><span data-stu-id="26f65-192">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a><span data-ttu-id="26f65-193">Bileşenler arasında başvuru öğeleri</span><span class="sxs-lookup"><span data-stu-id="26f65-193">Reference elements across components</span></span>

<span data-ttu-id="26f65-194">`ElementReference`, bir bileşenin `OnAfterRender` yönteminde (ve bir öğe başvurusu bir `struct`), bu nedenle bileşenler arasında bir öğe başvurusu geçirilememelidir.</span><span class="sxs-lookup"><span data-stu-id="26f65-194">An `ElementReference` is only guaranteed valid in a component's `OnAfterRender` method (and an element reference is a `struct`), so an element reference can't be passed between components.</span></span>

<span data-ttu-id="26f65-195">Bir üst bileşen için bir öğe başvurusunu diğer bileşenlere kullanılabilir hale getirmek için, üst bileşen şunları yapabilir:</span><span class="sxs-lookup"><span data-stu-id="26f65-195">For a parent component to make an element reference available to other components, the parent component can:</span></span>

* <span data-ttu-id="26f65-196">Alt bileşenlerin geri çağırmaları kaydetmesine izin ver.</span><span class="sxs-lookup"><span data-stu-id="26f65-196">Allow child components to register callbacks.</span></span>
* <span data-ttu-id="26f65-197">Geçirilen öğe başvurusuyla `OnAfterRender` olayı sırasında kayıtlı geri çağırmaları çağırın.</span><span class="sxs-lookup"><span data-stu-id="26f65-197">Invoke the registered callbacks during the `OnAfterRender` event with the passed element reference.</span></span> <span data-ttu-id="26f65-198">Bu yaklaşım dolaylı olarak, alt bileşenlerin üst öğenin öğe başvurusuyla etkileşime geçmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="26f65-198">Indirectly, this approach allows child components to interact with the parent's element reference.</span></span>

<span data-ttu-id="26f65-199">Aşağıdaki Blazor WebAssembly örneğinde yaklaşım gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="26f65-199">The following Blazor WebAssembly example illustrates the approach.</span></span>

<span data-ttu-id="26f65-200">*Wwwroot/index.html*`<head>`:</span><span class="sxs-lookup"><span data-stu-id="26f65-200">In the `<head>` of *wwwroot/index.html*:</span></span>

```html
<style>
    .red { color: red }
</style>
```

<span data-ttu-id="26f65-201">*Wwwroot/index.html*`<body>`:</span><span class="sxs-lookup"><span data-stu-id="26f65-201">In the `<body>` of *wwwroot/index.html*:</span></span>

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

<span data-ttu-id="26f65-202">*Pages/Index. Razor* (üst bileşen):</span><span class="sxs-lookup"><span data-stu-id="26f65-202">*Pages/Index.razor* (parent component):</span></span>

```razor
@page "/"

<h1 @ref="_title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

<span data-ttu-id="26f65-203">*Sayfa/dizin. Razor. cs*:</span><span class="sxs-lookup"><span data-stu-id="26f65-203">*Pages/Index.razor.cs*:</span></span>

```csharp
using System;
using System.Collections.Generic;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Pages
{
    public partial class Index : 
        ComponentBase, IObservable<ElementReference>, IDisposable
    {
        private bool _disposing;
        private IList<IObserver<ElementReference>> _subscriptions = 
            new List<IObserver<ElementReference>>();
        private ElementReference _title;

        protected override void OnAfterRender(bool firstRender)
        {
            base.OnAfterRender(firstRender);

            foreach (var subscription in _subscriptions)
            {
                try
                {
                    subscription.OnNext(_title);
                }
                catch (Exception)
                {
                    throw;
                }
            }
        }

        public void Dispose()
        {
            _disposing = true;

            foreach (var subscription in _subscriptions)
            {
                try
                {
                    subscription.OnCompleted();
                }
                catch (Exception)
                {
                }
            }

            _subscriptions.Clear();
        }

        public IDisposable Subscribe(IObserver<ElementReference> observer)
        {
            if (_disposing)
            {
                throw new InvalidOperationException("Parent being disposed");
            }

            _subscriptions.Add(observer);

            return new Subscription(observer, this);
        }

        private class Subscription : IDisposable
        {
            public Subscription(IObserver<ElementReference> observer, Index self)
            {
                Observer = observer;
                Self = self;
            }

            public IObserver<ElementReference> Observer { get; }
            public Index Self { get; }

            public void Dispose()
            {
                Self._subscriptions.Remove(Observer);
            }
        }
    }
}
```

<span data-ttu-id="26f65-204">*Shared/gözetimi ve Prompt. Razor* (alt bileşen):</span><span class="sxs-lookup"><span data-stu-id="26f65-204">*Shared/SurveyPrompt.razor* (child component):</span></span>

```razor
@inject IJSRuntime JS

<div class="alert alert-secondary mt-4" role="alert">
    <span class="oi oi-pencil mr-2" aria-hidden="true"></span>
    <strong>@Title</strong>

    <span class="text-nowrap">
        Please take our
        <a target="_blank" class="font-weight-bold" 
            href="https://go.microsoft.com/fwlink/?linkid=2109206">brief survey</a>
    </span>
    and tell us what you think.
</div>

@code {
    [Parameter]
    public string Title { get; set; }
}
```

<span data-ttu-id="26f65-205">*Shared/gözetimi ve Prompt. Razor. cs*:</span><span class="sxs-lookup"><span data-stu-id="26f65-205">*Shared/SurveyPrompt.razor.cs*:</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Shared
{
    public partial class SurveyPrompt : 
        ComponentBase, IObserver<ElementReference>, IDisposable
    {
        private IDisposable _subscription = null;

        [Parameter]
        public IObservable<ElementReference> Parent { get; set; }

        protected override void OnParametersSet()
        {
            base.OnParametersSet();

            if (_subscription != null)
            {
                _subscription.Dispose();
            }

            _subscription = Parent.Subscribe(this);
        }

        public void OnCompleted()
        {
            _subscription = null;
        }

        public void OnError(Exception error)
        {
            _subscription = null;
        }

        public void OnNext(ElementReference value)
        {
            JS.InvokeAsync<object>(
                "setElementClass", new object[] { value, "red" });
        }

        public void Dispose()
        {
            _subscription?.Dispose();
        }
    }
}
```

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="26f65-206">JavaScript işlevlerinden .NET yöntemlerini çağır</span><span class="sxs-lookup"><span data-stu-id="26f65-206">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="26f65-207">Statik .NET yöntemi çağrısı</span><span class="sxs-lookup"><span data-stu-id="26f65-207">Static .NET method call</span></span>

<span data-ttu-id="26f65-208">JavaScript 'ten statik bir .NET yöntemi çağırmak için `DotNet.invokeMethod` veya `DotNet.invokeMethodAsync` işlevlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="26f65-208">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="26f65-209">Çağırmak istediğiniz statik metodun tanımlayıcısını, işlevi içeren derlemenin adını ve tüm bağımsız değişkenleri geçirin.</span><span class="sxs-lookup"><span data-stu-id="26f65-209">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="26f65-210">Blazor sunucu senaryolarını desteklemek için zaman uyumsuz sürüm tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="26f65-210">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="26f65-211">.NET yöntemi genel, statik ve `[JSInvokable]` özniteliğine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="26f65-211">The .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="26f65-212">Açık genel yöntemlerin çağrılması Şu anda desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="26f65-212">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="26f65-213">Örnek uygulama, `int`s C# dizisini döndürmek için bir yöntem içerir.</span><span class="sxs-lookup"><span data-stu-id="26f65-213">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="26f65-214">`JSInvokable` özniteliği yöntemine uygulanır.</span><span class="sxs-lookup"><span data-stu-id="26f65-214">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="26f65-215">*Pages/Jsınterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="26f65-215">*Pages/JsInterop.razor*:</span></span>

```razor
<button type="button" class="btn btn-primary"
        onclick="exampleJsFunctions.returnArrayAsyncJs()">
    Trigger .NET static method ReturnArrayAsync
</button>

@code {
    [JSInvokable]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="26f65-216">İstemciye sunulan JavaScript, C# .net yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="26f65-216">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="26f65-217">*Wwwroot/Examplejsınterop. js*:</span><span class="sxs-lookup"><span data-stu-id="26f65-217">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="26f65-218">**Tetikleyici .net static yöntemi ReturnArrayAsync** düğmesi seçildiğinde, tarayıcının Web geliştirici araçlarında konsol çıkışını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="26f65-218">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="26f65-219">Konsol çıktısı:</span><span class="sxs-lookup"><span data-stu-id="26f65-219">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="26f65-220">Dördüncü dizi değeri, `ReturnArrayAsync`tarafından döndürülen diziye (`data.push(4);`) gönderilir.</span><span class="sxs-lookup"><span data-stu-id="26f65-220">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

<span data-ttu-id="26f65-221">Varsayılan olarak, yöntem tanımlayıcısı yöntem adıdır, ancak `JSInvokableAttribute` oluşturucusunu kullanarak farklı bir tanımlayıcı belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="26f65-221">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor:</span></span>

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="26f65-222">İstemci tarafı JavaScript dosyasında:</span><span class="sxs-lookup"><span data-stu-id="26f65-222">In the client-side JavaScript file:</span></span>

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

### <a name="instance-method-call"></a><span data-ttu-id="26f65-223">Örnek yöntem çağrısı</span><span class="sxs-lookup"><span data-stu-id="26f65-223">Instance method call</span></span>

<span data-ttu-id="26f65-224">JavaScript 'ten de .NET örnek yöntemlerini çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="26f65-224">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="26f65-225">JavaScript 'ten bir .NET örnek yöntemi çağırmak için:</span><span class="sxs-lookup"><span data-stu-id="26f65-225">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="26f65-226">.NET örneğini JavaScript 'e başvuruya göre geçirin:</span><span class="sxs-lookup"><span data-stu-id="26f65-226">Pass the .NET instance by reference to JavaScript:</span></span>
  * <span data-ttu-id="26f65-227">`DotNetObjectReference.Create`için statik bir çağrı yapın.</span><span class="sxs-lookup"><span data-stu-id="26f65-227">Make a static call to `DotNetObjectReference.Create`.</span></span>
  * <span data-ttu-id="26f65-228">Örneği bir `DotNetObjectReference` örneğine sarın ve `DotNetObjectReference` örneğindeki `Create` çağırın.</span><span class="sxs-lookup"><span data-stu-id="26f65-228">Wrap the instance in a `DotNetObjectReference` instance and call `Create` on the `DotNetObjectReference` instance.</span></span> <span data-ttu-id="26f65-229">`DotNetObjectReference` nesneleri atılamadı (Bu bölümün ilerleyen kısımlarında bir örnek görünür).</span><span class="sxs-lookup"><span data-stu-id="26f65-229">Dispose of `DotNetObjectReference` objects (an example appears later in this section).</span></span>
* <span data-ttu-id="26f65-230">`invokeMethod` veya `invokeMethodAsync` işlevlerini kullanarak örnekte .NET örnek yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="26f65-230">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="26f65-231">.NET örneği, JavaScript 'ten başka .NET yöntemleri çağrılırken bir bağımsız değişken olarak da geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="26f65-231">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="26f65-232">Örnek uygulama, iletileri istemci tarafı konsoluna kaydeder.</span><span class="sxs-lookup"><span data-stu-id="26f65-232">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="26f65-233">Örnek uygulama tarafından gösterilen aşağıdaki örnekler için tarayıcının geliştirici araçlarında tarayıcının konsol çıkışını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="26f65-233">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="26f65-234">**Tetikleyici .NET örnek yöntemi HelloHelper. SayHello** düğmesi seçildiğinde, `ExampleJsInterop.CallHelloHelperSayHello` çağrılır ve `Blazor`bir adı yöntemine geçirir.</span><span class="sxs-lookup"><span data-stu-id="26f65-234">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="26f65-235">*Pages/Jsınterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="26f65-235">*Pages/JsInterop.razor*:</span></span>

```razor
<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    public async Task TriggerNetInstanceMethod()
    {
        var exampleJsInterop = new ExampleJsInterop(JSRuntime);
        await exampleJsInterop.CallHelloHelperSayHello("Blazor");
    }
}
```

<span data-ttu-id="26f65-236">`CallHelloHelperSayHello`, JavaScript işlevini `sayHello` yeni bir `HelloHelper`örneğiyle çağırır.</span><span class="sxs-lookup"><span data-stu-id="26f65-236">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="26f65-237">*JsInteropClasses/Examplejsınterop. cs*:</span><span class="sxs-lookup"><span data-stu-id="26f65-237">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="26f65-238">*Wwwroot/Examplejsınterop. js*:</span><span class="sxs-lookup"><span data-stu-id="26f65-238">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="26f65-239">Ad, `HelloHelper.Name` özelliğini ayarlayan `HelloHelper`oluşturucusuna geçirilir.</span><span class="sxs-lookup"><span data-stu-id="26f65-239">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="26f65-240">JavaScript işlevi `sayHello` yürütüldüğünde, `HelloHelper.SayHello` JavaScript işlevi tarafından konsola yazılan `Hello, {Name}!` iletisi döndürür.</span><span class="sxs-lookup"><span data-stu-id="26f65-240">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="26f65-241">*JsInteropClasses/HelloHelper. cs*:</span><span class="sxs-lookup"><span data-stu-id="26f65-241">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="26f65-242">Tarayıcının Web geliştirici araçlarında konsol çıkışı:</span><span class="sxs-lookup"><span data-stu-id="26f65-242">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

<span data-ttu-id="26f65-243">Bir bellek sızıntısını önlemek ve bir `DotNetObjectReference`oluşturan bir bileşende çöp toplamaya izin vermek için, `DotNetObjectReference` örneğini oluşturan sınıfta nesnesini atın:</span><span class="sxs-lookup"><span data-stu-id="26f65-243">To avoid a memory leak and allow garbage collection on a component that creates a `DotNetObjectReference`, dispose of the object in the class that created the `DotNetObjectReference` instance:</span></span>

```csharp
public class ExampleJsInterop : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private DotNetObjectReference<HelloHelper> _objRef;

    public ExampleJsInterop(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public ValueTask<string> CallHelloHelperSayHello(string name)
    {
        _objRef = DotNetObjectReference.Create(new HelloHelper(name));

        return _jsRuntime.InvokeAsync<string>(
            "exampleJsFunctions.sayHello",
            _objRef);
    }

    public void Dispose()
    {
        _objRef?.Dispose();
    }
}
```
  
<span data-ttu-id="26f65-244">`ExampleJsInterop` sınıfında gösterilen önceki model de bir bileşende uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="26f65-244">The preceding pattern shown in the `ExampleJsInterop` class can also be implemented in a component:</span></span>
  
```razor
@page "/JSInteropComponent"
@using BlazorSample.JsInteropClasses
@implements IDisposable
@inject IJSRuntime JSRuntime

<h1>JavaScript Interop</h1>

<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    private DotNetObjectReference<HelloHelper> _objRef;

    public async Task TriggerNetInstanceMethod()
    {
        _objRef = DotNetObjectReference.Create(new HelloHelper("Blazor"));

        await JSRuntime.InvokeAsync<string>(
            "exampleJsFunctions.sayHello",
            _objRef);
    }

    public void Dispose()
    {
        _objRef?.Dispose();
    }
}
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="26f65-245">Birlikte çalışma kodunu bir sınıf kitaplığında paylaşma</span><span class="sxs-lookup"><span data-stu-id="26f65-245">Share interop code in a class library</span></span>

<span data-ttu-id="26f65-246">JS birlikte çalışma kodu bir NuGet paketindeki kodu paylaşmanıza olanak sağlayan bir sınıf kitaplığına dahil edilebilir.</span><span class="sxs-lookup"><span data-stu-id="26f65-246">JS interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="26f65-247">Sınıf kitaplığı, yerleşik derlemede JavaScript kaynaklarını katıştırmayı işler.</span><span class="sxs-lookup"><span data-stu-id="26f65-247">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="26f65-248">JavaScript dosyaları *Wwwroot* klasörüne yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="26f65-248">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="26f65-249">Araç, kitaplık oluşturulduğunda kaynakları katıştırmaya önem kazanır.</span><span class="sxs-lookup"><span data-stu-id="26f65-249">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="26f65-250">Oluşturulan NuGet paketine, uygulamanın proje dosyasında herhangi bir NuGet paketiyle aynı şekilde başvurulur.</span><span class="sxs-lookup"><span data-stu-id="26f65-250">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="26f65-251">Paket geri yüklendikten sonra, uygulama kodu JavaScript 'e, gibi çağrı yapabilir C#.</span><span class="sxs-lookup"><span data-stu-id="26f65-251">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="26f65-252">Daha fazla bilgi için bkz. <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="26f65-252">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="26f65-253">Harden JS birlikte çalışma çağrıları</span><span class="sxs-lookup"><span data-stu-id="26f65-253">Harden JS interop calls</span></span>

<span data-ttu-id="26f65-254">JS birlikte çalışması, ağ hataları nedeniyle başarısız olabilir ve güvenilmez olarak değerlendirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="26f65-254">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="26f65-255">Varsayılan olarak, bir Blazor sunucusu uygulaması, bir dakika sonra sunucu üzerinde JS birlikte çalışabilirlik çağrılarını zaman aşımına uğrar.</span><span class="sxs-lookup"><span data-stu-id="26f65-255">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="26f65-256">Bir uygulama, 10 saniye gibi daha agresif zaman aşımına uğrayedebilmesine, aşağıdaki yaklaşımlardan birini kullanarak zaman aşımını ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="26f65-256">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="26f65-257">`Startup.ConfigureServices`genel olarak, zaman aşımını belirtin:</span><span class="sxs-lookup"><span data-stu-id="26f65-257">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="26f65-258">Bileşen kodunda çağrı başına, tek bir çağrı zaman aşımını belirtebilir:</span><span class="sxs-lookup"><span data-stu-id="26f65-258">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="26f65-259">Kaynak tükenmesi hakkında daha fazla bilgi için bkz. <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="26f65-259">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>

## <a name="perform-large-data-transfers-in-opno-locblazor-server-apps"></a><span data-ttu-id="26f65-260">Blazor Server uygulamalarında büyük veri aktarımları gerçekleştirin</span><span class="sxs-lookup"><span data-stu-id="26f65-260">Perform large data transfers in Blazor Server apps</span></span>

<span data-ttu-id="26f65-261">Bazı senaryolarda, JavaScript ve Blazorarasında büyük miktarlarda veri aktarılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="26f65-261">In some scenarios, large amounts of data must be transferred between JavaScript and Blazor.</span></span> <span data-ttu-id="26f65-262">Genellikle, büyük veri aktarımları şu durumlarda oluşur:</span><span class="sxs-lookup"><span data-stu-id="26f65-262">Typically, large data transfers occur when:</span></span>

* <span data-ttu-id="26f65-263">Tarayıcı dosya sistemi API 'Leri bir dosyayı karşıya yüklemek veya indirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="26f65-263">Browser file system APIs are used to upload or download a file.</span></span>
* <span data-ttu-id="26f65-264">Üçüncü taraf kitaplığı ile birlikte çalışma gerekir.</span><span class="sxs-lookup"><span data-stu-id="26f65-264">Interop with a third party library is required.</span></span>

<span data-ttu-id="26f65-265">Blazor sunucuda, performans sorunlarına neden olabilecek tek büyük mesajların geçirilmesini engellemek için bir sınırlama vardır.</span><span class="sxs-lookup"><span data-stu-id="26f65-265">In Blazor Server, a limitation is in place to prevent passing single large messages that may result in performance issues.</span></span>

<span data-ttu-id="26f65-266">JavaScript ve Blazorarasında veri aktaran kodu geliştirirken aşağıdaki kılavuzu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="26f65-266">Consider the following guidance when developing code that transfers data between JavaScript and Blazor:</span></span>

* <span data-ttu-id="26f65-267">Verileri daha küçük parçalara dilimleyin ve tüm veriler sunucu tarafından alınana kadar veri segmentlerini sırayla gönderin.</span><span class="sxs-lookup"><span data-stu-id="26f65-267">Slice the data into smaller pieces, and send the data segments sequentially until all of the data is received by the server.</span></span>
* <span data-ttu-id="26f65-268">JavaScript ve C# kodda büyük nesneler ayırmayın.</span><span class="sxs-lookup"><span data-stu-id="26f65-268">Don't allocate large objects in JavaScript and C# code.</span></span>
* <span data-ttu-id="26f65-269">Veri gönderirken veya alırken uzun süreler için ana UI iş parçacığını engellemez.</span><span class="sxs-lookup"><span data-stu-id="26f65-269">Don't block the main UI thread for long periods when sending or receiving data.</span></span>
* <span data-ttu-id="26f65-270">İşlem tamamlandığında veya iptal edildiğinde tüketilen tüm belleği boşaltın.</span><span class="sxs-lookup"><span data-stu-id="26f65-270">Free any memory consumed when the process is completed or cancelled.</span></span>
* <span data-ttu-id="26f65-271">Güvenlik amaçları için aşağıdaki ek gereksinimleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="26f65-271">Enforce the following additional requirements for security purposes:</span></span>
  * <span data-ttu-id="26f65-272">Geçirilebilen en büyük dosya veya veri boyutunu bildirin.</span><span class="sxs-lookup"><span data-stu-id="26f65-272">Declare the maximum file or data size that can be passed.</span></span>
  * <span data-ttu-id="26f65-273">İstemciden sunucuya en düşük karşıya yükleme hızını bildirin.</span><span class="sxs-lookup"><span data-stu-id="26f65-273">Declare the minimum upload rate from the client to the server.</span></span>
* <span data-ttu-id="26f65-274">Veriler, sunucu tarafından alındıktan sonra şu olabilir:</span><span class="sxs-lookup"><span data-stu-id="26f65-274">After the data is received by the server, the data can be:</span></span>
  * <span data-ttu-id="26f65-275">Tüm segmentler toplanana kadar bir bellek arabelleğinde geçici olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="26f65-275">Temporarily stored in a memory buffer until all of the segments are collected.</span></span>
  * <span data-ttu-id="26f65-276">Hemen tüketildi.</span><span class="sxs-lookup"><span data-stu-id="26f65-276">Consumed immediately.</span></span> <span data-ttu-id="26f65-277">Örneğin, veriler bir veritabanında hemen depolanabilir veya her bir segment alındığında diske yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="26f65-277">For example, the data can be stored immediately in a database or written to disk as each segment is received.</span></span>

<span data-ttu-id="26f65-278">Aşağıdaki dosya Yükleyici sınıfı istemcisiyle JS birlikte çalışabilirliği yönetir.</span><span class="sxs-lookup"><span data-stu-id="26f65-278">The following file uploader class handles JS interop with the client.</span></span> <span data-ttu-id="26f65-279">Uploader sınıfı, JS birlikte çalışması için şunu kullanır:</span><span class="sxs-lookup"><span data-stu-id="26f65-279">The uploader class uses JS interop to:</span></span>

* <span data-ttu-id="26f65-280">Veri segmenti göndermek için istemciyi yoklayın.</span><span class="sxs-lookup"><span data-stu-id="26f65-280">Poll the client to send a data segment.</span></span>
* <span data-ttu-id="26f65-281">Yoklama zaman aşımına uğrarsa işlemi iptal edin.</span><span class="sxs-lookup"><span data-stu-id="26f65-281">Abort the transaction if polling times out.</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Microsoft.JSInterop;

public class FileUploader : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private readonly int _segmentSize = 6144;
    private readonly int _maxBase64SegmentSize = 8192;
    private readonly DotNetObjectReference<FileUploader> _thisReference;
    private List<IMemoryOwner<byte>> _uploadedSegments = 
        new List<IMemoryOwner<byte>>();

    public FileUploader(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public async Task<Stream> ReceiveFile(string selector, int maxSize)
    {
        var fileSize = 
            await _jsRuntime.InvokeAsync<int>("getFileSize", selector);

        if (fileSize > maxSize)
        {
            return null;
        }

        var numberOfSegments = Math.Floor(fileSize / (double)_segmentSize) + 1;
        var lastSegmentBytes = 0;
        string base64EncodedSegment;

        for (var i = 0; i < numberOfSegments; i++)
        {
            try
            {
                base64EncodedSegment = 
                    await _jsRuntime.InvokeAsync<string>(
                        "receiveSegment", i, selector);

                if (base64EncodedSegment.Length < _maxBase64SegmentSize && 
                    i < numberOfSegments - 1)
                {
                    return null;
                }
            }
            catch
            {
                return null;
            }

          var current = MemoryPool<byte>.Shared.Rent(_segmentSize);

          if (!Convert.TryFromBase64String(base64EncodedSegment, 
              current.Memory.Slice(0, _segmentSize).Span, out lastSegmentBytes))
          {
              return null;
          }

          _uploadedSegments.Add(current);
        }

        var segments = _uploadedSegments;
        _uploadedSegments = null;

        return new SegmentedStream(segments, _segmentSize, lastSegmentBytes);
    }

    public void Dispose()
    {
        if (_uploadedSegments != null)
        {
            foreach (var segment in _uploadedSegments)
            {
                segment.Dispose();
            }
        }
    }
}
```

<span data-ttu-id="26f65-282">Yukarıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="26f65-282">In the preceding example:</span></span>

* <span data-ttu-id="26f65-283">`_maxBase64SegmentSize`, `_maxBase64SegmentSize = _segmentSize * 4 / 3`hesaplanan `8192`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="26f65-283">The `_maxBase64SegmentSize` is set to `8192`, which is calculated from `_maxBase64SegmentSize = _segmentSize * 4 / 3`.</span></span>
* <span data-ttu-id="26f65-284">Düşük düzey .NET Core bellek yönetimi API 'Leri, bellek kesimlerini `_uploadedSegments`sunucuda depolamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="26f65-284">Low level .NET Core memory management APIs are used to store the memory segments on the server in `_uploadedSegments`.</span></span>
* <span data-ttu-id="26f65-285">`ReceiveFile` yöntemi, JS birlikte çalışabilirliği aracılığıyla karşıya yüklemeyi işlemek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="26f65-285">A `ReceiveFile` method is used to handle the upload through JS interop:</span></span>
  * <span data-ttu-id="26f65-286">Dosya boyutu, `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`ile JS birlikte çalışma aracılığıyla bayt olarak belirlenir.</span><span class="sxs-lookup"><span data-stu-id="26f65-286">The file size is determined in bytes through JS interop with `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.</span></span>
  * <span data-ttu-id="26f65-287">Alacak segmentlerin sayısı `numberOfSegments`' de hesaplanıp depolanır.</span><span class="sxs-lookup"><span data-stu-id="26f65-287">The number of segments to receive are calculated and stored in `numberOfSegments`.</span></span>
  * <span data-ttu-id="26f65-288">Kesimleri, `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`ile JS birlikte çalışabilirliği aracılığıyla `for` döngüsünde istenir.</span><span class="sxs-lookup"><span data-stu-id="26f65-288">The segments are requested in a `for` loop through JS interop with `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`.</span></span> <span data-ttu-id="26f65-289">Tüm kesimler ancak son, kod çözmede önce 8.192 bayt olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="26f65-289">All segments but the last must be 8,192 bytes before decoding.</span></span> <span data-ttu-id="26f65-290">İstemci verileri verimli bir şekilde gönderilmeye zorlanır.</span><span class="sxs-lookup"><span data-stu-id="26f65-290">The client is forced to send the data in an efficient manner.</span></span>
  * <span data-ttu-id="26f65-291">Alınan her segment için, <xref:System.Convert.TryFromBase64String*>ile kod çözme işleminden önce denetimler gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="26f65-291">For each segment received, checks are performed before decoding with <xref:System.Convert.TryFromBase64String*>.</span></span>
  * <span data-ttu-id="26f65-292">Karşıya yükleme tamamlandıktan sonra verileri içeren bir akış yeni bir <xref:System.IO.Stream> (`SegmentedStream`) olarak döndürülür.</span><span class="sxs-lookup"><span data-stu-id="26f65-292">A stream with the data is returned as a new <xref:System.IO.Stream> (`SegmentedStream`) after the upload is complete.</span></span>

<span data-ttu-id="26f65-293">Kesimli akış sınıfı, kesim listesini ReadOnly aranabilir olmayan <xref:System.IO.Stream>olarak kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="26f65-293">The segmented stream class exposes the list of segments as a readonly non-seekable <xref:System.IO.Stream>:</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;

public class SegmentedStream : Stream
{
    private readonly ReadOnlySequence<byte> _sequence;
    private long _currentPosition = 0;

    public SegmentedStream(IList<IMemoryOwner<byte>> segments, int segmentSize, 
        int lastSegmentSize)
    {
        if (segments.Count == 1)
        {
            _sequence = new ReadOnlySequence<byte>(
                segments[0].Memory.Slice(0, lastSegmentSize));
            return;
        }

        var sequenceSegment = new BufferSegment<byte>(
            segments[0].Memory.Slice(0, segmentSize));
        var lastSegment = sequenceSegment;

        for (int i = 1; i < segments.Count; i++)
        {
            var isLastSegment = i + 1 == segments.Count;
            lastSegment = lastSegment.Append(segments[i].Memory.Slice(
                0, isLastSegment ? lastSegmentSize : segmentSize));
        }

        _sequence = new ReadOnlySequence<byte>(
            sequenceSegment, 0, lastSegment, lastSegmentSize);
    }

    public override long Position
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }

    public override int Read(byte[] buffer, int offset, int count)
    {
        var bytesToWrite = (int)(_currentPosition + count < _sequence.Length ? 
            count : _sequence.Length - _currentPosition);
        var data = _sequence.Slice(_currentPosition, bytesToWrite);
        data.CopyTo(buffer.AsSpan(offset, bytesToWrite));
        _currentPosition += bytesToWrite;

        return bytesToWrite;
    }

    private class BufferSegment<T> : ReadOnlySequenceSegment<T>
    {
        public BufferSegment(ReadOnlyMemory<T> memory)
        {
            Memory = memory;
        }

        public BufferSegment<T> Append(ReadOnlyMemory<T> memory)
        {
            var segment = new BufferSegment<T>(memory)
            {
                RunningIndex = RunningIndex + Memory.Length
            };

            Next = segment;

            return segment;
        }
    }

    public override bool CanRead => true;

    public override bool CanSeek => false;

    public override bool CanWrite => false;

    public override long Length => throw new NotImplementedException();

    public override void Flush() => throw new NotImplementedException();

    public override long Seek(long offset, SeekOrigin origin) => 
        throw new NotImplementedException();

    public override void SetLength(long value) => 
        throw new NotImplementedException();

    public override void Write(byte[] buffer, int offset, int count) => 
        throw new NotImplementedException();
}
```

<span data-ttu-id="26f65-294">Aşağıdaki kod, verileri almak için JavaScript işlevlerini uygular:</span><span class="sxs-lookup"><span data-stu-id="26f65-294">The following code implements JavaScript functions to receive the data:</span></span>

```javascript
function getFileSize(selector) {
  const file = getFile(selector);
  return file.size;
}

async function receiveSegment(segmentNumber, selector) {
  const file = getFile(selector);
  var segments = getFileSegments(file);
  var index = segmentNumber * 6144;
  return await getNextChunk(file, index);
}

function getFile(selector) {
  const element = document.querySelector(selector);
  if (!element) {
    throw new Error('Invalid selector');
  }
  const files = element.files;
  if (!files || files.length === 0) {
    throw new Error(`Element ${elementId} doesn't contain any files.`);
  }
  const file = files[0];
  return file;
}

function getFileSegments(file) {
  const segments = Math.floor(size % 6144 === 0 ? size / 6144 : 1 + size / 6144);
  return segments;
}

async function getNextChunk(file, index) {
  const length = file.size - index <= 6144 ? file.size - index : 6144;
  const chunk = file.slice(index, index + length);
  index += length;
  const base64Chunk = await this.base64EncodeAsync(chunk);
  return { base64Chunk, index };
}

async function base64EncodeAsync(chunk) {
  const reader = new FileReader();
  const result = new Promise((resolve, reject) => {
    reader.addEventListener('load',
      () => {
        const base64Chunk = reader.result;
        const cleanChunk = 
          base64Chunk.replace('data:application/octet-stream;base64,', '');
        resolve(cleanChunk);
      },
      false);
    reader.addEventListener('error', reject);
  });
  reader.readAsDataURL(chunk);
  return result;
}
```

## <a name="additional-resources"></a><span data-ttu-id="26f65-295">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="26f65-295">Additional resources</span></span>

* [<span data-ttu-id="26f65-296">InteropComponent. Razor örneği (DotNet/AspNetCore GitHub deposu, 3,1 yayın dalı)</span><span class="sxs-lookup"><span data-stu-id="26f65-296">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
