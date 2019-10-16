---
title: ASP.NET Core Blazor JavaScript birlikte çalışması
author: guardrex
description: Blazor Apps 'teki JavaScript 'ten .NET ve .NET yöntemlerinden JavaScript işlevlerini çağırmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/javascript-interop
ms.openlocfilehash: b4776a20c6da6c722d2c057d19863c570f530a21
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391061"
---
# <a name="aspnet-core-blazor-javascript-interop"></a><span data-ttu-id="3d3b5-103">ASP.NET Core Blazor JavaScript birlikte çalışması</span><span class="sxs-lookup"><span data-stu-id="3d3b5-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="3d3b5-104">Sağlayan [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3d3b5-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="3d3b5-105">Bir Blazor uygulaması, JavaScript kodundan .NET ve .NET yöntemlerinden JavaScript işlevlerini çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="3d3b5-106">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3d3b5-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="3d3b5-107">.NET metotlarından JavaScript işlevlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="3d3b5-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="3d3b5-108">Bir JavaScript işlevini çağırmak için .NET kodunun gerekli olduğu durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="3d3b5-109">Örneğin, JavaScript çağrısı, tarayıcı özelliklerini veya bir JavaScript kitaplığından uygulamaya yönelik işlevselliği sunabilir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="3d3b5-110">.NET 'ten JavaScript 'i çağırmak için `IJSRuntime` soyutlama kullanın.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-110">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="3d3b5-111">@No__t-0 yöntemi, herhangi bir sayıda JSON seri hale getirilebilir bağımsız değişkenle birlikte çağırmak istediğiniz JavaScript işlevi için bir tanımlayıcı alır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-111">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="3d3b5-112">İşlev tanımlayıcısı genel kapsama göredir (`window`).</span><span class="sxs-lookup"><span data-stu-id="3d3b5-112">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="3d3b5-113">@No__t-0 ' ı çağırmak isterseniz, tanımlayıcı `someScope.someFunction` ' dir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-113">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="3d3b5-114">Çağrılmadan önce işlevi kaydetmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-114">There's no need to register the function before it's called.</span></span> <span data-ttu-id="3d3b5-115">@No__t-0 dönüş türü de JSON serileştirilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-115">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="3d3b5-116">Blazor Server uygulamaları için:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-116">For Blazor Server apps:</span></span>

* <span data-ttu-id="3d3b5-117">Birden çok kullanıcı isteği, Blazor Server uygulaması tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-117">Multiple user requests are processed by the Blazor Server app.</span></span> <span data-ttu-id="3d3b5-118">JavaScript işlevlerini çağırmak için bir bileşende `JSRuntime.Current` çağrısı yapmayın.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-118">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="3d3b5-119">@No__t-0 soyutlamasını ekler ve JavaScript birlikte çalışma çağrılarını vermek için eklenen nesneyi kullanın.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-119">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>
* <span data-ttu-id="3d3b5-120">Blazor uygulaması prerendering olduğunda, tarayıcıyla bir bağlantı kurulmadığı için JavaScript 'e çağrı yapılamaz.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-120">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="3d3b5-121">Daha fazla bilgi için bkz. [bir Blazor uygulaması ne zaman prerendering](#detect-when-a-blazor-app-is-prerendering) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="3d3b5-122">Aşağıdaki örnek, deneysel bir JavaScript tabanlı kod çözücüsü olan [Textdecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)tabanlıdır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="3d3b5-123">Örnek, bir C# yöntemden JavaScript işlevinin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="3d3b5-124">JavaScript işlevi bir C# yöntemden bir bayt dizisi kabul eder, dizinin kodunu çözer ve görüntülenecek metni bileşene döndürür.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="3d3b5-125">*Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_host. cshtml* (Blazor Server) `<head>` öğesi içinde, geçirilen bir dizinin kodunu çözmek için `TextDecoder` kullanan bir işlev sağlayın:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a function that uses `TextDecoder` to decode a passed array:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

<span data-ttu-id="3d3b5-126">Önceki örnekte gösterilen kod gibi JavaScript kodu, betik dosyasına yönelik bir başvuruya sahip bir JavaScript dosyasından ( *. js*) de yüklenebilir:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="3d3b5-127">Aşağıdaki bileşen:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-127">The following component:</span></span>

* <span data-ttu-id="3d3b5-128">Bir bileşen düğmesi (**diziyi Dönüştür**) seçildiğinde `JsRuntime` kullanarak `ConvertArray` JavaScript işlevini çağırır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-128">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="3d3b5-129">JavaScript işlevi çağrıldıktan sonra, geçirilen dizi bir dizeye dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="3d3b5-130">Dize, görüntüleme için bileşene döndürülür.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-130">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

<span data-ttu-id="3d3b5-131">@No__t-0 soyutlamasını kullanmak için aşağıdaki yaklaşımlardan birini benimseyin:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-131">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="3d3b5-132">@No__t-0 soyutlama Razor bileşenine ( *. Razor*) ekleme:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-132">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* <span data-ttu-id="3d3b5-133">@No__t-0 soyutlamasını bir sınıfa ( *. cs*) Ekle:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-133">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* <span data-ttu-id="3d3b5-134">[Buildrendertree](xref:blazor/components#manual-rendertreebuilder-logic)ile dinamik içerik oluşturma için `[Inject]` özniteliğini kullanın:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-134">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="3d3b5-135">Bu konuya eşlik eden istemci tarafı örnek uygulamada, Kullanıcı girişi almak ve bir hoş geldiniz iletisi göstermek üzere DOM ile etkileşime geçen uygulama için iki JavaScript işlevi mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-135">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="3d3b5-136">`showPrompt` &ndash; Kullanıcı girişini kabul etmek için bir istem üretir (kullanıcının adı) ve çağıranın adını döndürür.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-136">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="3d3b5-137">`displayWelcome` &ndash;, çağırandan bir DOM nesnesine `id` `welcome` ile bir hoş geldiniz iletisi atar.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-137">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="3d3b5-138">*Wwwroot/Examplejsınterop. js*:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-138">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="3d3b5-139">*Wwwroot/index.html* dosyasındaki (Blazor WebAssembly) veya *Pages/_host. cshtml* dosyasındaki (Blazor Server) JavaScript dosyasına başvuran `<script>` etiketini yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-139">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="3d3b5-140">*Wwwroot/index.html* (Blazor WebAssembly):</span><span class="sxs-lookup"><span data-stu-id="3d3b5-140">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="3d3b5-141">*Pages/_Host. cshtml* (Blazor Server):</span><span class="sxs-lookup"><span data-stu-id="3d3b5-141">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/_Host.cshtml?highlight=29)]

<span data-ttu-id="3d3b5-142">@No__t-0 etiketini bir bileşen dosyasına yerleştirmeyin çünkü `<script>` etiketi dinamik olarak güncelleştirilemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-142">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="3d3b5-143">.NET yöntemleri, `IJSRuntime.InvokeAsync<T>` çağırarak *Examplejsınterop. js* dosyasında JavaScript işlevleriyle birlikte çalışır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-143">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="3d3b5-144">@No__t-0 soyutlama, Blazor sunucu senaryolarına izin vermek için zaman uyumsuzdur.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-144">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="3d3b5-145">Uygulama bir Blazor WebAssembly uygulaması ise ve bir JavaScript işlevini zaman uyumlu olarak çağırmak istiyorsanız, `IJSInProcessRuntime` ' a alt türe çevirme yapın ve bunun yerine `Invoke<T>` ' i çağırın.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-145">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="3d3b5-146">Çoğu JavaScript birlikte çalışma kitaplıklarının, kitaplıkların tüm senaryolarda kullanılabilir olduğundan emin olmak için zaman uyumsuz API 'Leri kullanmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-146">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="3d3b5-147">Örnek uygulama, JavaScript birlikte çalışabildiğini gösteren bir bileşen içerir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-147">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="3d3b5-148">Bileşen:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-148">The component:</span></span>

* <span data-ttu-id="3d3b5-149">Bir JavaScript istemi aracılığıyla Kullanıcı girişini alır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-149">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="3d3b5-150">İşlemek için bileşene metni döndürür.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-150">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="3d3b5-151">Bir hoş geldiniz iletisini göstermek için DOM ile etkileşime sahip ikinci bir JavaScript işlevini çağırır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-151">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="3d3b5-152">*Pages/Jsınterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-152">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="3d3b5-153">@No__t-0 ' ın bileşen **tetikleyicisi JavaScript istem** düğmesi seçilerek yürütülmesi durumunda, *Wwwroot/Examplejsınterop. js* dosyasında bulunan JavaScript `showPrompt` işlevi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-153">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="3d3b5-154">@No__t-0 işlevi, HTML kodlu ve bileşene döndürülen kullanıcı girişini (kullanıcının adı) kabul eder.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-154">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="3d3b5-155">Bileşen kullanıcının adını yerel bir değişkende depolar, `name`.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-155">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="3d3b5-156">@No__t-0 ' da depolanan dize, hoş geldiniz iletisini bir başlık etiketine işleyen bir JavaScript işlevine (`displayWelcome`) geçirilen bir hoş geldiniz iletisine dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-156">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="3d3b5-157">Void JavaScript işlevini çağırın</span><span class="sxs-lookup"><span data-stu-id="3d3b5-157">Call a void JavaScript function</span></span>

<span data-ttu-id="3d3b5-158">[Void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) veya [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) döndüren JavaScript işlevleri `IJSRuntime.InvokeVoidAsync` ile çağırılır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-158">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-blazor-app-is-prerendering"></a><span data-ttu-id="3d3b5-159">Bir Blazor uygulamasının ne zaman prerendering olduğunu Algıla</span><span class="sxs-lookup"><span data-stu-id="3d3b5-159">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="3d3b5-160">Öğelere başvuruları yakala</span><span class="sxs-lookup"><span data-stu-id="3d3b5-160">Capture references to elements</span></span>

<span data-ttu-id="3d3b5-161">Bazı [JavaScript birlikte çalışma](xref:blazor/javascript-interop) senaryoları HTML öğelerine başvuru gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-161">Some [JavaScript interop](xref:blazor/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="3d3b5-162">Örneğin, bir kullanıcı arabirimi kitaplığı başlatma için bir öğe başvurusu gerektirebilir veya `focus` veya `play` gibi bir öğe üzerinde komut benzeri API 'Ler çağırmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-162">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="3d3b5-163">Aşağıdaki yaklaşımı kullanarak bir bileşen içindeki HTML öğelerine başvuruları yakalayın:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-163">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="3d3b5-164">HTML öğesine `@ref` özniteliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-164">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="3d3b5-165">Adı `@ref` özniteliğinin değeriyle eşleşen `ElementReference` türünde bir alan tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-165">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="3d3b5-166">Aşağıdaki örnek `username` `<input>` öğesine bir başvuru yakalama göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-166">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> <span data-ttu-id="3d3b5-167">Yakalanan öğe başvurularını DOM doldurma yöntemi **olarak kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="3d3b5-167">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="3d3b5-168">Bunun yapılması, bildirim temelli işleme modeliyle karışabilir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-168">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="3d3b5-169">.NET kodu düşünüldüğünde, `ElementReference` donuk bir tanıtıcıdır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-169">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="3d3b5-170">@No__t-1 ile yapabileceğiniz *tek* şey, JavaScript birlikte çalışabilirliği aracılığıyla JavaScript koduna geçiş yapar.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-170">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="3d3b5-171">Bunu yaptığınızda, JavaScript tarafı kodu, normal DOM API 'Leri ile kullanılabilecek bir `HTMLElement` örneği alır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-171">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="3d3b5-172">Örneğin, aşağıdaki kod bir öğe üzerinde odağı ayarlamaya izin veren bir .NET genişletme yöntemi tanımlar:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-172">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="3d3b5-173">*Examplejsınterop. js*:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-173">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="3d3b5-174">Bir öğeye odaklanmak için `IJSRuntime.InvokeAsync<T>` kullanın ve `ElementReference` ile `exampleJsFunctions.focusElement` çağırın:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-174">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementReference` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="3d3b5-175">Bir öğeyi odaklamak üzere bir genişletme yöntemi kullanmak için, `IJSRuntime` örneğini alan bir statik genişletme yöntemi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-175">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="3d3b5-176">Yöntemi doğrudan nesnesi üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-176">The method is called directly on the object.</span></span> <span data-ttu-id="3d3b5-177">Aşağıdaki örnek, statik `Focus` yönteminin `JsInteropClasses` ad alanından kullanılabildiğini varsayar:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-177">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> <span data-ttu-id="3d3b5-178">@No__t-0 değişkeni yalnızca bileşen işlendikten sonra doldurulur.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-178">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="3d3b5-179">Bir undoldurulmamış `ElementReference` JavaScript koduna geçirilirse, JavaScript kodu bir `null` değeri alır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-179">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="3d3b5-180">Bileşen işlemeyi tamamladıktan sonra öğe başvurularını işlemek için (bir öğe üzerinde ilk odağı ayarlamak için) `OnAfterRenderAsync` veya `OnAfterRender` [bileşen yaşam döngüsü yöntemlerini](xref:blazor/components#lifecycle-methods)kullanın.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-180">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="3d3b5-181">JavaScript işlevlerinden .NET yöntemlerini çağır</span><span class="sxs-lookup"><span data-stu-id="3d3b5-181">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="3d3b5-182">Statik .NET yöntemi çağrısı</span><span class="sxs-lookup"><span data-stu-id="3d3b5-182">Static .NET method call</span></span>

<span data-ttu-id="3d3b5-183">JavaScript 'ten statik bir .NET yöntemi çağırmak için `DotNet.invokeMethod` veya `DotNet.invokeMethodAsync` işlevlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-183">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="3d3b5-184">Çağırmak istediğiniz statik metodun tanımlayıcısını, işlevi içeren derlemenin adını ve tüm bağımsız değişkenleri geçirin.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-184">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="3d3b5-185">Blazor sunucu senaryolarını desteklemek için zaman uyumsuz sürüm tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-185">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="3d3b5-186">JavaScript 'ten bir .NET yöntemi çağırmak için, .NET yönteminin ortak, statik ve `[JSInvokable]` özniteliğine sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-186">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="3d3b5-187">Varsayılan olarak, yöntem tanımlayıcısı yöntem adıdır, ancak `JSInvokableAttribute` oluşturucusunu kullanarak farklı bir tanımlayıcı belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-187">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="3d3b5-188">Açık genel yöntemlerin çağrılması Şu anda desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-188">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="3d3b5-189">Örnek uygulama, @no__t- C# 1s dizisini döndürmek için bir yöntem içerir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-189">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="3d3b5-190">@No__t-0 özniteliği yöntemine uygulanır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-190">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="3d3b5-191">*Pages/Jsınterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-191">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="3d3b5-192">İstemciye sunulan JavaScript, C# .net yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-192">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="3d3b5-193">*Wwwroot/Examplejsınterop. js*:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-193">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="3d3b5-194">**Tetikleyici .net static yöntemi ReturnArrayAsync** düğmesi seçildiğinde, tarayıcının Web geliştirici araçlarında konsol çıkışını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-194">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="3d3b5-195">Konsol çıktısı:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-195">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="3d3b5-196">Dördüncü dizi değeri `ReturnArrayAsync` tarafından döndürülen diziye (`data.push(4);`) gönderilir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-196">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="3d3b5-197">Örnek yöntem çağrısı</span><span class="sxs-lookup"><span data-stu-id="3d3b5-197">Instance method call</span></span>

<span data-ttu-id="3d3b5-198">JavaScript 'ten de .NET örnek yöntemlerini çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-198">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="3d3b5-199">JavaScript 'ten bir .NET örnek yöntemi çağırmak için:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-199">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="3d3b5-200">.NET örneğini `DotNetObjectReference` örneğine sarmalayarak JavaScript 'e geçirin.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-200">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="3d3b5-201">.NET örneği, JavaScript 'e başvuruya göre geçirilir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-201">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="3d3b5-202">@No__t-0 veya `invokeMethodAsync` işlevlerini kullanarak örnekte .NET örnek yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-202">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="3d3b5-203">.NET örneği, JavaScript 'ten başka .NET yöntemleri çağrılırken bir bağımsız değişken olarak da geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-203">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="3d3b5-204">Örnek uygulama, iletileri istemci tarafı konsoluna kaydeder.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-204">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="3d3b5-205">Örnek uygulama tarafından gösterilen aşağıdaki örnekler için tarayıcının geliştirici araçlarında tarayıcının konsol çıkışını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-205">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="3d3b5-206">**Tetikleyici .NET örnek yöntemi HelloHelper. SayHello** düğmesi seçildiğinde `ExampleJsInterop.CallHelloHelperSayHello` çağrılır ve bir ad, `Blazor` ' yi yöntemine geçirir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-206">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="3d3b5-207">*Pages/Jsınterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-207">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="3d3b5-208">`CallHelloHelperSayHello` JavaScript işlevini, yeni bir `HelloHelper` örneğiyle birlikte @no__t çağırır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-208">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="3d3b5-209">*JsInteropClasses/Examplejsınterop. cs*:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-209">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="3d3b5-210">*Wwwroot/Examplejsınterop. js*:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-210">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="3d3b5-211">Ad, `HelloHelper.Name` özelliğini ayarlayan `HelloHelper` ' a geçirilir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-211">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="3d3b5-212">@No__t-0 JavaScript işlevi yürütüldüğünde, `HelloHelper.SayHello`, JavaScript işlevi tarafından konsola yazılan `Hello, {Name}!` iletisini döndürür.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-212">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="3d3b5-213">*JsInteropClasses/HelloHelper. cs*:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-213">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="3d3b5-214">Tarayıcının Web geliştirici araçlarında konsol çıkışı:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-214">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="3d3b5-215">Birlikte çalışma kodunu bir sınıf kitaplığında paylaşma</span><span class="sxs-lookup"><span data-stu-id="3d3b5-215">Share interop code in a class library</span></span>

<span data-ttu-id="3d3b5-216">JavaScript birlikte çalışma kodu bir NuGet paketindeki kodu paylaşmanıza olanak sağlayan bir sınıf kitaplığına dahil edilebilir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-216">JavaScript interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="3d3b5-217">Sınıf kitaplığı, yerleşik derlemede JavaScript kaynaklarını katıştırmayı işler.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-217">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="3d3b5-218">JavaScript dosyaları *Wwwroot* klasörüne yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-218">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="3d3b5-219">Araç, kitaplık oluşturulduğunda kaynakları katıştırmaya önem kazanır.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-219">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="3d3b5-220">Oluşturulan NuGet paketine, uygulamanın proje dosyasında herhangi bir NuGet paketiyle aynı şekilde başvurulur.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-220">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="3d3b5-221">Paket geri yüklendikten sonra, uygulama kodu JavaScript 'e, gibi çağrı yapabilir C#.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-221">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="3d3b5-222">Daha fazla bilgi için bkz. <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-222">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="3d3b5-223">Harden JS birlikte çalışma çağrıları</span><span class="sxs-lookup"><span data-stu-id="3d3b5-223">Harden JS interop calls</span></span>

<span data-ttu-id="3d3b5-224">JS birlikte çalışması, ağ hataları nedeniyle başarısız olabilir ve güvenilmez olarak değerlendirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-224">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="3d3b5-225">Varsayılan olarak, Blazor sunucu uygulaması, bir dakika sonra sunucu üzerinde JS birlikte çalışabilirlik çağrılarını zaman aşımına uğrar.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-225">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="3d3b5-226">Bir uygulama, 10 saniye gibi daha agresif zaman aşımına uğrayedebilmesine, aşağıdaki yaklaşımlardan birini kullanarak zaman aşımını ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-226">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="3d3b5-227">@No__t-0 ' da genel olarak, zaman aşımını belirtin:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-227">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="3d3b5-228">Bileşen kodunda çağrı başına, tek bir çağrı zaman aşımını belirtebilir:</span><span class="sxs-lookup"><span data-stu-id="3d3b5-228">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="3d3b5-229">Kaynak tükenmesi hakkında daha fazla bilgi için bkz. <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="3d3b5-229">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>
