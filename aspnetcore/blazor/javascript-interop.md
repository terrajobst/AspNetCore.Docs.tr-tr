---
title: ASP.NET Core Blazor JavaScript birlikte çalışabilirliği
author: guardrex
description: Blazor uygulamalarında JavaScript 'ten .NET ve .NET yöntemlerinden JavaScript işlevlerini çağırmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/23/2019
no-loc:
- Blazor
uid: blazor/javascript-interop
ms.openlocfilehash: 79555ca6c987e2ca57e0cfab9779024498fdd58b
ms.sourcegitcommit: 0dd224b2b7efca1fda0041b5c3f45080327033f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74681037"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a><span data-ttu-id="d38bc-103">ASP.NET Core Blazor JavaScript birlikte çalışabilirliği</span><span class="sxs-lookup"><span data-stu-id="d38bc-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="d38bc-104">Sağlayan [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d38bc-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="d38bc-105">Blazor bir uygulama, JavaScript kodundan .NET ve .NET yöntemlerinden JavaScript işlevlerini çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="d38bc-106">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d38bc-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="d38bc-107">.NET metotlarından JavaScript işlevlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="d38bc-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="d38bc-108">Bir JavaScript işlevini çağırmak için .NET kodunun gerekli olduğu durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="d38bc-109">Örneğin, JavaScript çağrısı, tarayıcı özelliklerini veya bir JavaScript kitaplığından uygulamaya yönelik işlevselliği sunabilir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span> <span data-ttu-id="d38bc-110">Bu senaryoya *JavaScript birlikte çalışabilirliği* (*js birlikte çalışma*) denir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-110">This scenario is called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="d38bc-111">.NET 'ten JavaScript 'i çağırmak için `IJSRuntime` soyutlamasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="d38bc-111">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="d38bc-112">`InvokeAsync<T>` yöntemi, herhangi bir sayıda JSON seri hale getirilebilir bağımsız değişkenle birlikte çağırmak istediğiniz JavaScript işlevi için bir tanımlayıcı alır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-112">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="d38bc-113">İşlev tanımlayıcısı, genel kapsama (`window`) göredir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-113">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="d38bc-114">`window.someScope.someFunction`çağırmak isterseniz, tanımlayıcı `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="d38bc-114">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="d38bc-115">Çağrılmadan önce işlevi kaydetmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="d38bc-115">There's no need to register the function before it's called.</span></span> <span data-ttu-id="d38bc-116">Dönüş türü `T` ayrıca seri hale getirilebilir JSON olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-116">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="d38bc-117">Blazor Server uygulamaları için:</span><span class="sxs-lookup"><span data-stu-id="d38bc-117">For Blazor Server apps:</span></span>

* <span data-ttu-id="d38bc-118">Blazor sunucusu uygulaması tarafından birden çok kullanıcı isteği işlenir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-118">Multiple user requests are processed by the Blazor Server app.</span></span> <span data-ttu-id="d38bc-119">JavaScript işlevlerini çağırmak için bir bileşende `JSRuntime.Current` çağırmayın.</span><span class="sxs-lookup"><span data-stu-id="d38bc-119">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="d38bc-120">`IJSRuntime` soyutlamasını ekler ve bu nesneyi kullanarak JS birlikte çalışma çağrıları verme.</span><span class="sxs-lookup"><span data-stu-id="d38bc-120">Inject the `IJSRuntime` abstraction and use the injected object to issue JS interop calls.</span></span>
* <span data-ttu-id="d38bc-121">Blazor bir uygulama prerendering olduğunda, tarayıcıyla bir bağlantı kurulmadığından JavaScript 'e çağrı yapılamaz.</span><span class="sxs-lookup"><span data-stu-id="d38bc-121">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="d38bc-122">Daha fazla bilgi için, [Blazor bir uygulamanın ne zaman prerendering olduğunu Algıla](#detect-when-a-blazor-app-is-prerendering) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="d38bc-122">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="d38bc-123">Aşağıdaki örnek, deneysel bir JavaScript tabanlı kod çözücüsü olan [Textdecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)tabanlıdır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-123">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="d38bc-124">Örnek, bir C# yöntemden JavaScript işlevinin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-124">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="d38bc-125">JavaScript işlevi bir C# yöntemden bir bayt dizisi kabul eder, dizinin kodunu çözer ve görüntülenecek metni bileşene döndürür.</span><span class="sxs-lookup"><span data-stu-id="d38bc-125">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="d38bc-126">*Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_Host. cshtml* (Blazor Server) `<head>` öğesi içinde, geçirilen bir dizinin kodunu çözmek Için `TextDecoder` kullanan bir JavaScript işlevi sağlayın ve kodu çözülen değeri döndürün:</span><span class="sxs-lookup"><span data-stu-id="d38bc-126">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="d38bc-127">Önceki örnekte gösterilen kod gibi JavaScript kodu, betik dosyasına yönelik bir başvuruya sahip bir JavaScript dosyasından ( *. js*) de yüklenebilir:</span><span class="sxs-lookup"><span data-stu-id="d38bc-127">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="d38bc-128">Aşağıdaki bileşen:</span><span class="sxs-lookup"><span data-stu-id="d38bc-128">The following component:</span></span>

* <span data-ttu-id="d38bc-129">Bir bileşen düğmesi (**diziyi Dönüştür**) seçildiğinde `JSRuntime` kullanarak `convertArray` JavaScript işlevini çağırır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-129">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="d38bc-130">JavaScript işlevi çağrıldıktan sonra, geçirilen dizi bir dizeye dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="d38bc-130">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="d38bc-131">Dize, görüntüleme için bileşene döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d38bc-131">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a><span data-ttu-id="d38bc-132">IJSRuntime kullanımı</span><span class="sxs-lookup"><span data-stu-id="d38bc-132">Use of IJSRuntime</span></span>

<span data-ttu-id="d38bc-133">`IJSRuntime` soyutlamasını kullanmak için aşağıdaki yaklaşımlardan birini benimseyin:</span><span class="sxs-lookup"><span data-stu-id="d38bc-133">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="d38bc-134">`IJSRuntime` soyutlama Razor bileşenine ( *. Razor*) ekleme:</span><span class="sxs-lookup"><span data-stu-id="d38bc-134">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="d38bc-135">*Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_Host. cshtml* (Blazor Server) `<head>` öğesinin Içinde, bir `handleTickerChanged` JavaScript işlevi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="d38bc-135">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="d38bc-136">İşlevi `IJSRuntime.InvokeVoidAsync` ile çağrılır ve bir değer döndürmez:</span><span class="sxs-lookup"><span data-stu-id="d38bc-136">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="d38bc-137">`IJSRuntime` soyutlamasını bir sınıfa ( *. cs*) Ekle:</span><span class="sxs-lookup"><span data-stu-id="d38bc-137">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="d38bc-138">*Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_Host. cshtml* (Blazor Server) `<head>` öğesinin Içinde, bir `handleTickerChanged` JavaScript işlevi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="d38bc-138">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="d38bc-139">İşlevi `JSRuntime.InvokeAsync` ile çağrılır ve bir değer döndürür:</span><span class="sxs-lookup"><span data-stu-id="d38bc-139">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="d38bc-140">[Buildrendertree](xref:blazor/components#manual-rendertreebuilder-logic)ile dinamik içerik oluşturma için `[Inject]` özniteliğini kullanın:</span><span class="sxs-lookup"><span data-stu-id="d38bc-140">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="d38bc-141">Bu konuya eşlik eden istemci tarafı örnek uygulamada, Kullanıcı girişi almak ve bir hoş geldiniz iletisi göstermek üzere DOM ile etkileşime geçen uygulama için iki JavaScript işlevi mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="d38bc-141">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="d38bc-142">`showPrompt` &ndash; Kullanıcı girişini kabul etmek için bir istem üretir (kullanıcının adı) ve çağıranın adını döndürür.</span><span class="sxs-lookup"><span data-stu-id="d38bc-142">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="d38bc-143">`displayWelcome` &ndash;, çağıran bir `id` `welcome`bir DOM nesnesine bir hoş geldiniz iletisi atar.</span><span class="sxs-lookup"><span data-stu-id="d38bc-143">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="d38bc-144">*Wwwroot/Examplejsınterop. js*:</span><span class="sxs-lookup"><span data-stu-id="d38bc-144">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="d38bc-145">JavaScript dosyasına başvuran `<script>` etiketini *Wwwroot/index.html* dosyasında (Blazor WebAssembly) veya *Pages/_Host. cshtml* dosyasında (Blazor Server) yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="d38bc-145">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="d38bc-146">*Wwwroot/index.html* (Blazor WebAssembly):</span><span class="sxs-lookup"><span data-stu-id="d38bc-146">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="d38bc-147">*Pages/_Host. cshtml* (Blazor sunucusu):</span><span class="sxs-lookup"><span data-stu-id="d38bc-147">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

<span data-ttu-id="d38bc-148">`<script>` etiketi dinamik olarak güncelleştirilemediğinden bir `<script>` etiketini bileşen dosyasına yerleştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="d38bc-148">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="d38bc-149">.NET yöntemleri, `IJSRuntime.InvokeAsync<T>`çağırarak *Examplejsınterop. js* dosyasında JavaScript işlevleriyle birlikte çalışır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-149">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="d38bc-150">`IJSRuntime` soyutlama, Blazor sunucu senaryolarına izin vermek için zaman uyumsuzdur.</span><span class="sxs-lookup"><span data-stu-id="d38bc-150">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="d38bc-151">Uygulama bir Blazor Weelsembly uygulaması ise ve bir JavaScript işlevini zaman uyumlu olarak çağırmak istiyorsanız, `IJSInProcessRuntime` ve bunun yerine `Invoke<T>` çağırın.</span><span class="sxs-lookup"><span data-stu-id="d38bc-151">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="d38bc-152">Çoğu JS birlikte çalışma kitaplıklarının, kitaplıkların tüm senaryolarda kullanılabilir olmasını sağlamak için zaman uyumsuz API 'Leri kullanmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="d38bc-152">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="d38bc-153">Örnek uygulama, JS birlikte çalışabilirliği göstermek için bir bileşeni içerir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-153">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="d38bc-154">Bileşen:</span><span class="sxs-lookup"><span data-stu-id="d38bc-154">The component:</span></span>

* <span data-ttu-id="d38bc-155">Bir JavaScript istemi aracılığıyla Kullanıcı girişini alır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-155">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="d38bc-156">İşlemek için bileşene metni döndürür.</span><span class="sxs-lookup"><span data-stu-id="d38bc-156">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="d38bc-157">Bir hoş geldiniz iletisini göstermek için DOM ile etkileşime sahip ikinci bir JavaScript işlevini çağırır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-157">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="d38bc-158">*Pages/Jsınterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d38bc-158">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="d38bc-159">`TriggerJsPrompt`, bileşenin **tetikleyicisi JavaScript istem** düğmesi seçilerek yürütüldüğünde, *Wwwroot/Examplejsınterop. js* dosyasında verilen JavaScript `showPrompt` işlevi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-159">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="d38bc-160">`showPrompt` işlevi, HTML kodlu ve bileşene döndürülen kullanıcı girişini (kullanıcının adı) kabul eder.</span><span class="sxs-lookup"><span data-stu-id="d38bc-160">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="d38bc-161">Bileşen, kullanıcının adını `name`yerel bir değişkende depolar.</span><span class="sxs-lookup"><span data-stu-id="d38bc-161">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="d38bc-162">`name` depolanan dize, hoş geldiniz iletisini bir başlık etiketine işleyen `displayWelcome`bir JavaScript işlevine iletilen bir hoş geldiniz iletisine dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-162">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="d38bc-163">Void JavaScript işlevini çağırın</span><span class="sxs-lookup"><span data-stu-id="d38bc-163">Call a void JavaScript function</span></span>

<span data-ttu-id="d38bc-164">[Void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) veya [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) döndüren JavaScript işlevleri `IJSRuntime.InvokeVoidAsync`ile çağırılır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-164">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a><span data-ttu-id="d38bc-165">Blazor bir uygulamanın ne zaman prerendering olduğunu Algıla</span><span class="sxs-lookup"><span data-stu-id="d38bc-165">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="d38bc-166">Öğelere başvuruları yakala</span><span class="sxs-lookup"><span data-stu-id="d38bc-166">Capture references to elements</span></span>

<span data-ttu-id="d38bc-167">Bazı JS birlikte çalışma senaryoları HTML öğelerine başvurular gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-167">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="d38bc-168">Örneğin, bir kullanıcı arabirimi kitaplığı başlatma için bir öğe başvurusu gerektirebilir veya `focus` veya `play`gibi bir öğe üzerinde komut benzeri API 'Ler çağırmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-168">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="d38bc-169">Aşağıdaki yaklaşımı kullanarak bir bileşen içindeki HTML öğelerine başvuruları yakalayın:</span><span class="sxs-lookup"><span data-stu-id="d38bc-169">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="d38bc-170">HTML öğesine bir `@ref` özniteliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d38bc-170">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="d38bc-171">Adı `@ref` özniteliği değeri ile eşleşen `ElementReference` türünde bir alan tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="d38bc-171">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="d38bc-172">Aşağıdaki örnek, `username` `<input>` öğesine bir başvuru yakalama göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="d38bc-172">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="d38bc-173">Yalnızca Blazoretkileşimde bulunmayan boş bir öğenin içeriğini bulunmamalıdır için bir öğe başvurusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="d38bc-173">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="d38bc-174">Bu senaryo, bir 3. taraf API 'SI öğeye içerik sağladığı zaman yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-174">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="d38bc-175">Blazor öğesiyle etkileşmediği için, Blazoröğesi ve DOM gösterimi arasında bir çakışma olabilir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-175">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="d38bc-176">Aşağıdaki örnekte, Blazor, bu öğenin liste öğelerini (`<li>`) doldurmak üzere DOM ile etkileşimde bulunduğundan, sıralanmamış listenin (`ul`) içeriğini () zaman *zaman aşmaktır* .</span><span class="sxs-lookup"><span data-stu-id="d38bc-176">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
>
> ```cshtml
> <ul ref="MyList">
>     @foreach (var item in Todos)
>     {
>         <li>@item.Text</li>
>     }
> </ul>
> ```
>
> <span data-ttu-id="d38bc-177">JS birlikte çalışma öğesi, öğe `MyList` içeriğini değiştiriyorsa ve Blazor SLA 'ya uygulamaya çalışırsa, diffler DOM ile eşleşmez.</span><span class="sxs-lookup"><span data-stu-id="d38bc-177">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="d38bc-178">.NET kodu açısından düşünüldüğünde, `ElementReference` donuk bir tanıtıcıdır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-178">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="d38bc-179">`ElementReference` ile yapabileceğiniz *tek* şey, JS birlikte çalışma yoluyla JavaScript koduna geçiş yapar.</span><span class="sxs-lookup"><span data-stu-id="d38bc-179">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="d38bc-180">Bunu yaptığınızda, JavaScript tarafı kodu normal DOM API 'Leri ile kullanılabilecek bir `HTMLElement` örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-180">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="d38bc-181">Örneğin, aşağıdaki kod bir öğe üzerinde odağı ayarlamaya izin veren bir .NET genişletme yöntemi tanımlar:</span><span class="sxs-lookup"><span data-stu-id="d38bc-181">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="d38bc-182">*Examplejsınterop. js*:</span><span class="sxs-lookup"><span data-stu-id="d38bc-182">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="d38bc-183">`IJSRuntime.InvokeAsync<T>` kullanın ve bir öğeyi odaklanmak için bir `ElementReference` `exampleJsFunctions.focusElement` çağırın:</span><span class="sxs-lookup"><span data-stu-id="d38bc-183">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementReference` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="d38bc-184">Bir öğeyi odaklamak için bir genişletme yöntemi kullanmak için, `IJSRuntime` örneğini alan bir statik genişletme yöntemi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d38bc-184">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="d38bc-185">Yöntemi doğrudan nesnesi üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-185">The method is called directly on the object.</span></span> <span data-ttu-id="d38bc-186">Aşağıdaki örnek, statik `Focus` yönteminin `JsInteropClasses` ad alanından kullanılabildiğini varsayar:</span><span class="sxs-lookup"><span data-stu-id="d38bc-186">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> <span data-ttu-id="d38bc-187">`username` değişkeni yalnızca bileşen işlendikten sonra doldurulur.</span><span class="sxs-lookup"><span data-stu-id="d38bc-187">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="d38bc-188">Doldurulmamış bir `ElementReference` JavaScript koduna geçirilirse, JavaScript kodu bir `null`değeri alır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-188">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="d38bc-189">Bileşen işlemeyi tamamladıktan sonra öğe başvurularını değiştirmek için (bir öğe üzerinde ilk odağı ayarlamak için) [Onafterrenderasync veya OnAfterRender bileşen yaşam döngüsü yöntemlerini](xref:blazor/lifecycle#after-component-render)kullanın.</span><span class="sxs-lookup"><span data-stu-id="d38bc-189">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="d38bc-190">JavaScript işlevlerinden .NET yöntemlerini çağır</span><span class="sxs-lookup"><span data-stu-id="d38bc-190">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="d38bc-191">Statik .NET yöntemi çağrısı</span><span class="sxs-lookup"><span data-stu-id="d38bc-191">Static .NET method call</span></span>

<span data-ttu-id="d38bc-192">JavaScript 'ten statik bir .NET yöntemi çağırmak için `DotNet.invokeMethod` veya `DotNet.invokeMethodAsync` işlevlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="d38bc-192">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="d38bc-193">Çağırmak istediğiniz statik metodun tanımlayıcısını, işlevi içeren derlemenin adını ve tüm bağımsız değişkenleri geçirin.</span><span class="sxs-lookup"><span data-stu-id="d38bc-193">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="d38bc-194">Blazor sunucu senaryolarını desteklemek için zaman uyumsuz sürüm tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-194">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="d38bc-195">JavaScript 'ten bir .NET yöntemi çağırmak için, .NET yönteminin public, static ve `[JSInvokable]` özniteliğine sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-195">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="d38bc-196">Varsayılan olarak, yöntem tanımlayıcısı yöntem adıdır, ancak `JSInvokableAttribute` oluşturucusunu kullanarak farklı bir tanımlayıcı belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d38bc-196">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="d38bc-197">Açık genel yöntemlerin çağrılması Şu anda desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="d38bc-197">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="d38bc-198">Örnek uygulama, `int`s C# dizisini döndürmek için bir yöntem içerir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-198">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="d38bc-199">`JSInvokable` özniteliği yöntemine uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-199">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="d38bc-200">*Pages/Jsınterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d38bc-200">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="d38bc-201">İstemciye sunulan JavaScript, C# .net yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-201">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="d38bc-202">*Wwwroot/Examplejsınterop. js*:</span><span class="sxs-lookup"><span data-stu-id="d38bc-202">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="d38bc-203">**Tetikleyici .net static yöntemi ReturnArrayAsync** düğmesi seçildiğinde, tarayıcının Web geliştirici araçlarında konsol çıkışını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="d38bc-203">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="d38bc-204">Konsol çıktısı:</span><span class="sxs-lookup"><span data-stu-id="d38bc-204">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="d38bc-205">Dördüncü dizi değeri, `ReturnArrayAsync`tarafından döndürülen diziye (`data.push(4);`) gönderilir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-205">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="d38bc-206">Örnek yöntem çağrısı</span><span class="sxs-lookup"><span data-stu-id="d38bc-206">Instance method call</span></span>

<span data-ttu-id="d38bc-207">JavaScript 'ten de .NET örnek yöntemlerini çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d38bc-207">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="d38bc-208">JavaScript 'ten bir .NET örnek yöntemi çağırmak için:</span><span class="sxs-lookup"><span data-stu-id="d38bc-208">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="d38bc-209">.NET örneğini bir `DotNetObjectReference` örneğine sarmalayarak JavaScript 'e geçirin.</span><span class="sxs-lookup"><span data-stu-id="d38bc-209">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="d38bc-210">.NET örneği, JavaScript 'e başvuruya göre geçirilir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-210">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="d38bc-211">`invokeMethod` veya `invokeMethodAsync` işlevlerini kullanarak örnekte .NET örnek yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="d38bc-211">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="d38bc-212">.NET örneği, JavaScript 'ten başka .NET yöntemleri çağrılırken bir bağımsız değişken olarak da geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-212">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="d38bc-213">Örnek uygulama, iletileri istemci tarafı konsoluna kaydeder.</span><span class="sxs-lookup"><span data-stu-id="d38bc-213">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="d38bc-214">Örnek uygulama tarafından gösterilen aşağıdaki örnekler için tarayıcının geliştirici araçlarında tarayıcının konsol çıkışını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="d38bc-214">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="d38bc-215">**Tetikleyici .NET örnek yöntemi HelloHelper. SayHello** düğmesi seçildiğinde, `ExampleJsInterop.CallHelloHelperSayHello` çağrılır ve `Blazor`bir adı yöntemine geçirir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-215">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="d38bc-216">*Pages/Jsınterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d38bc-216">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="d38bc-217">`CallHelloHelperSayHello`, JavaScript işlevini `sayHello` yeni bir `HelloHelper`örneğiyle çağırır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-217">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="d38bc-218">*JsInteropClasses/Examplejsınterop. cs*:</span><span class="sxs-lookup"><span data-stu-id="d38bc-218">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="d38bc-219">*Wwwroot/Examplejsınterop. js*:</span><span class="sxs-lookup"><span data-stu-id="d38bc-219">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="d38bc-220">Ad, `HelloHelper.Name` özelliğini ayarlayan `HelloHelper`oluşturucusuna geçirilir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-220">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="d38bc-221">JavaScript işlevi `sayHello` yürütüldüğünde, `HelloHelper.SayHello` JavaScript işlevi tarafından konsola yazılan `Hello, {Name}!` iletisi döndürür.</span><span class="sxs-lookup"><span data-stu-id="d38bc-221">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="d38bc-222">*JsInteropClasses/HelloHelper. cs*:</span><span class="sxs-lookup"><span data-stu-id="d38bc-222">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="d38bc-223">Tarayıcının Web geliştirici araçlarında konsol çıkışı:</span><span class="sxs-lookup"><span data-stu-id="d38bc-223">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="d38bc-224">Birlikte çalışma kodunu bir sınıf kitaplığında paylaşma</span><span class="sxs-lookup"><span data-stu-id="d38bc-224">Share interop code in a class library</span></span>

<span data-ttu-id="d38bc-225">JS birlikte çalışma kodu bir NuGet paketindeki kodu paylaşmanıza olanak sağlayan bir sınıf kitaplığına dahil edilebilir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-225">JS interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="d38bc-226">Sınıf kitaplığı, yerleşik derlemede JavaScript kaynaklarını katıştırmayı işler.</span><span class="sxs-lookup"><span data-stu-id="d38bc-226">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="d38bc-227">JavaScript dosyaları *Wwwroot* klasörüne yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-227">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="d38bc-228">Araç, kitaplık oluşturulduğunda kaynakları katıştırmaya önem kazanır.</span><span class="sxs-lookup"><span data-stu-id="d38bc-228">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="d38bc-229">Oluşturulan NuGet paketine, uygulamanın proje dosyasında herhangi bir NuGet paketiyle aynı şekilde başvurulur.</span><span class="sxs-lookup"><span data-stu-id="d38bc-229">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="d38bc-230">Paket geri yüklendikten sonra, uygulama kodu JavaScript 'e, gibi çağrı yapabilir C#.</span><span class="sxs-lookup"><span data-stu-id="d38bc-230">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="d38bc-231">Daha fazla bilgi için bkz. <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="d38bc-231">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="d38bc-232">Harden JS birlikte çalışma çağrıları</span><span class="sxs-lookup"><span data-stu-id="d38bc-232">Harden JS interop calls</span></span>

<span data-ttu-id="d38bc-233">JS birlikte çalışması, ağ hataları nedeniyle başarısız olabilir ve güvenilmez olarak değerlendirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="d38bc-233">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="d38bc-234">Varsayılan olarak, bir Blazor sunucusu uygulaması, bir dakika sonra sunucu üzerinde JS birlikte çalışabilirlik çağrılarını zaman aşımına uğrar.</span><span class="sxs-lookup"><span data-stu-id="d38bc-234">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="d38bc-235">Bir uygulama, 10 saniye gibi daha agresif zaman aşımına uğrayedebilmesine, aşağıdaki yaklaşımlardan birini kullanarak zaman aşımını ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="d38bc-235">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="d38bc-236">`Startup.ConfigureServices`genel olarak, zaman aşımını belirtin:</span><span class="sxs-lookup"><span data-stu-id="d38bc-236">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="d38bc-237">Bileşen kodunda çağrı başına, tek bir çağrı zaman aşımını belirtebilir:</span><span class="sxs-lookup"><span data-stu-id="d38bc-237">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="d38bc-238">Kaynak tükenmesi hakkında daha fazla bilgi için bkz. <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="d38bc-238">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>
