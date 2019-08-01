---
title: ASP.NET Core Blazor JavaScript birlikte çalışması
author: guardrex
description: Blazor Apps 'teki JavaScript 'ten .NET ve .NET yöntemlerinden JavaScript işlevlerini çağırmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/31/2019
uid: blazor/javascript-interop
ms.openlocfilehash: 09fbf12da5dae6fbada58e263b6a90e5d7d4a932
ms.sourcegitcommit: 979dbfc5e9ce09b9470789989cddfcfb57079d94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68681876"
---
# <a name="aspnet-core-blazor-javascript-interop"></a><span data-ttu-id="ad4f9-103">ASP.NET Core Blazor JavaScript birlikte çalışması</span><span class="sxs-lookup"><span data-stu-id="ad4f9-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="ad4f9-104">Sağlayan [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ad4f9-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ad4f9-105">Bir Blazor uygulaması, JavaScript kodundan .NET ve .NET yöntemlerinden JavaScript işlevlerini çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="ad4f9-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ad4f9-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="ad4f9-107">.NET metotlarından JavaScript işlevlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="ad4f9-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="ad4f9-108">Bir JavaScript işlevini çağırmak için .NET kodunun gerekli olduğu durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="ad4f9-109">Örneğin, JavaScript çağrısı, tarayıcı özelliklerini veya bir JavaScript kitaplığından uygulamaya yönelik işlevselliği sunabilir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="ad4f9-110">.Net 'ten JavaScript 'e çağrı yapmak için `IJSRuntime` soyutlamayı kullanın.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-110">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="ad4f9-111">Yöntemi `InvokeAsync<T>` , herhangi bir sayıda JSON seri hale getirilebilir bağımsız değişkenle birlikte çağırmak istediğiniz JavaScript işlevi için bir tanımlayıcı alır.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-111">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="ad4f9-112">İşlev tanımlayıcısı genel kapsama (`window`) göredir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-112">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="ad4f9-113">Çağırmak `window.someScope.someFunction`isterseniz, tanımlayıcı olur `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-113">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="ad4f9-114">Çağrılmadan önce işlevi kaydetmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-114">There's no need to register the function before it's called.</span></span> <span data-ttu-id="ad4f9-115">Dönüş türünün `T` de JSON seri hale getirilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-115">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="ad4f9-116">Sunucu tarafı uygulamalar için:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-116">For server-side apps:</span></span>

* <span data-ttu-id="ad4f9-117">Sunucu tarafı uygulama tarafından birden çok kullanıcı isteği işlenir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-117">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="ad4f9-118">JavaScript işlevlerini `JSRuntime.Current` çağırmak için bir bileşeni çağırmayın.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-118">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="ad4f9-119">`IJSRuntime` Soyutlamayı ekler ve JavaScript birlikte çalışma çağrılarını vermek için eklenen nesneyi kullanın.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-119">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>
* <span data-ttu-id="ad4f9-120">Blazor uygulaması prerendering olduğunda, tarayıcıyla bir bağlantı kurulmadığı için JavaScript 'e çağrı yapılamaz.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-120">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="ad4f9-121">Daha fazla bilgi için bkz. [bir Blazor uygulaması ne zaman prerendering](#detect-when-a-blazor-app-is-prerendering) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="ad4f9-122">Aşağıdaki örnek, deneysel bir JavaScript tabanlı kod çözücüsü olan [Textdecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)tabanlıdır.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="ad4f9-123">Örnek, bir C# yöntemden JavaScript işlevinin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="ad4f9-124">JavaScript işlevi bir C# yöntemden bir bayt dizisi kabul eder, dizinin kodunu çözer ve görüntülenecek metni bileşene döndürür.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="ad4f9-125">Wwwroot/index.html (Blazor Client -Side) veya *Pages/_host. cshtml* (Blazor sunucu-tarafı) `TextDecoder` öğesininiçinde,geçirilenbirdizininkodunuçözmekiçinkullananbirişlevsağlayın:`<head>`</span><span class="sxs-lookup"><span data-stu-id="ad4f9-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor client-side) or *Pages/_Host.cshtml* (Blazor server-side), provide a function that uses `TextDecoder` to decode a passed array:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

<span data-ttu-id="ad4f9-126">Önceki örnekte gösterilen kod gibi JavaScript kodu, betik dosyasına yönelik bir başvuruya sahip bir JavaScript dosyasından ( *. js*) de yüklenebilir:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="ad4f9-127">Aşağıdaki bileşen:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-127">The following component:</span></span>

* <span data-ttu-id="ad4f9-128">Bir bileşen düğmesi (**diziyi Dönüştür**) seçildiğinde `JsRuntime` JavaScriptişleviniçağırır.`ConvertArray`</span><span class="sxs-lookup"><span data-stu-id="ad4f9-128">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="ad4f9-129">JavaScript işlevi çağrıldıktan sonra, geçirilen dizi bir dizeye dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="ad4f9-130">Dize, görüntüleme için bileşene döndürülür.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-130">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

<span data-ttu-id="ad4f9-131">`IJSRuntime` Soyutlamayı kullanmak için aşağıdaki yaklaşımlardan birini benimseyin:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-131">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="ad4f9-132">Soyutlama Razor bileşenine ( *. Razor*) ekleme: `IJSRuntime`</span><span class="sxs-lookup"><span data-stu-id="ad4f9-132">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* <span data-ttu-id="ad4f9-133">Soyutlamayı bir sınıfa ( *. cs*) Ekle: `IJSRuntime`</span><span class="sxs-lookup"><span data-stu-id="ad4f9-133">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* <span data-ttu-id="ad4f9-134">[Buildrendertree](xref:blazor/components#manual-rendertreebuilder-logic)ile dinamik içerik oluşturma için `[Inject]` özniteliğini kullanın:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-134">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="ad4f9-135">Bu konuya eşlik eden istemci tarafı örnek uygulamada, Kullanıcı girişi almak ve bir hoş geldiniz iletisi göstermek üzere DOM ile etkileşime geçen istemci tarafı uygulama için iki JavaScript işlevi mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-135">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="ad4f9-136">`showPrompt`&ndash; Kullanıcı girişini kabul etmek için bir istem üretir (kullanıcının adı) ve arayan adına adı döndürür.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-136">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="ad4f9-137">`displayWelcome`' A `id` sahip bir DOM nesnesine çağırandan bir hoş geldiniz iletisi atar. `welcome` &ndash;</span><span class="sxs-lookup"><span data-stu-id="ad4f9-137">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="ad4f9-138">*Wwwroot/Examplejsınterop. js*:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-138">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="ad4f9-139">JavaScript dosyasına başvuran etiketiWwwroot/index.HTMLFile(BlazorClient-Side)veyaPages/_host.cshtmldosyasında(BlazorServer-Side)`<script>` yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-139">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor client-side) or *Pages/_Host.cshtml* file (Blazor server-side).</span></span>

<span data-ttu-id="ad4f9-140">*Wwwroot/index.html* (Blazor istemci tarafı):</span><span class="sxs-lookup"><span data-stu-id="ad4f9-140">*wwwroot/index.html* (Blazor client-side):</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="ad4f9-141">*Pages/_Host. cshtml* (Blazor sunucu tarafı):</span><span class="sxs-lookup"><span data-stu-id="ad4f9-141">*Pages/_Host.cshtml* (Blazor server-side):</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/_Host.cshtml?highlight=29)]

<span data-ttu-id="ad4f9-142">`<script>` Etiket dinamik olarak `<script>` güncelleştirilemediğinden bir bileşen dosyasına etiket yerleştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-142">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="ad4f9-143">.NET yöntemleri, çağırarak `IJSRuntime.InvokeAsync<T>`, *examplejsınterop. js* dosyasındaki JavaScript işlevleriyle birlikte çalışır.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-143">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="ad4f9-144">Soyutlama `IJSRuntime` , sunucu tarafı senaryolara izin vermek için zaman uyumsuzdur.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-144">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="ad4f9-145">Uygulama istemci tarafı çalıştırıyorsa ve bir JavaScript işlevini zaman uyumlu olarak çağırmak istiyorsanız bunun yerine alt türe çevirme `IJSInProcessRuntime` yapın ve çağırın. `Invoke<T>`</span><span class="sxs-lookup"><span data-stu-id="ad4f9-145">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="ad4f9-146">Çoğu JavaScript birlikte çalışma kitaplıklarının, kitaplıkların tüm senaryolarda, istemci tarafında veya sunucu tarafında kullanılabilir olmasını sağlamak için zaman uyumsuz API 'Leri kullanmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-146">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="ad4f9-147">Örnek uygulama, JavaScript birlikte çalışabildiğini gösteren bir bileşen içerir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-147">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="ad4f9-148">Bileşen:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-148">The component:</span></span>

* <span data-ttu-id="ad4f9-149">Bir JavaScript istemi aracılığıyla Kullanıcı girişini alır.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-149">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="ad4f9-150">İşlemek için bileşene metni döndürür.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-150">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="ad4f9-151">Bir hoş geldiniz iletisini göstermek için DOM ile etkileşime sahip ikinci bir JavaScript işlevini çağırır.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-151">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="ad4f9-152">*Pages/Jsınterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-152">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="ad4f9-153">Bileşenin tetikleme **JavaScript istem** düğmesi seçilerek çalıştırıldığında, `showPrompt` *Wwwroot/examplejsınterop. js* dosyasında verilen JavaScript işlevi çağırılır. `TriggerJsPrompt`</span><span class="sxs-lookup"><span data-stu-id="ad4f9-153">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="ad4f9-154">`showPrompt` İşlevi, HTML kodlu ve bileşene döndürülen kullanıcı girişini (kullanıcının adı) kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-154">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="ad4f9-155">Bileşen, kullanıcının adını yerel bir değişkende `name`depolar.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-155">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="ad4f9-156">İçinde `name` depolanan dize, hoş geldiniz iletisini bir başlık etiketine işleyen bir JavaScript `displayWelcome`işlevine iletilen bir hoş geldiniz iletisine eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-156">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="ad4f9-157">Void JavaScript işlevini çağırın</span><span class="sxs-lookup"><span data-stu-id="ad4f9-157">Call a void JavaScript function</span></span>

<span data-ttu-id="ad4f9-158">[Void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) veya [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) döndüren JavaScript işlevleri ile `IJSRuntime.InvokeAsync<object>`çağırılır ve döndürür. `null`</span><span class="sxs-lookup"><span data-stu-id="ad4f9-158">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeAsync<object>`, which returns `null`.</span></span>

## <a name="detect-when-a-blazor-app-is-prerendering"></a><span data-ttu-id="ad4f9-159">Bir Blazor uygulamasının ne zaman prerendering olduğunu Algıla</span><span class="sxs-lookup"><span data-stu-id="ad4f9-159">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="ad4f9-160">Öğelere başvuruları yakala</span><span class="sxs-lookup"><span data-stu-id="ad4f9-160">Capture references to elements</span></span>

<span data-ttu-id="ad4f9-161">Bazı [JavaScript birlikte çalışma](xref:blazor/javascript-interop) senaryoları HTML öğelerine başvuru gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-161">Some [JavaScript interop](xref:blazor/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="ad4f9-162">Örneğin, bir kullanıcı arabirimi kitaplığı başlatma için bir öğe başvurusu gerektirebilir veya `focus` ya `play`da gibi bir öğe üzerinde komut benzeri API 'ler çağırmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-162">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="ad4f9-163">Aşağıdaki yaklaşımı kullanarak bir bileşen içindeki HTML öğelerine başvuruları yakalayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-163">You can capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="ad4f9-164">HTML öğesine `@ref` bir öznitelik ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-164">Add a `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="ad4f9-165">Adı `@ref` özniteliğin değeriyle eşleşen bir `ElementRef` tür alanı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-165">Define a field of type `ElementRef` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="ad4f9-166">Aşağıdaki örnek, `username` `<input>` öğesine bir başvuru yakalama göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-166">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input @ref="username" ... />

@code {
    ElementRef username;
}
```

> [!NOTE]
> <span data-ttu-id="ad4f9-167">Blazor başvurulan öğelerle etkileşime geçtiğinde DOM 'ı doldurma veya işleme gibi yakalanan öğe **başvurularını kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="ad4f9-167">Do **not** use captured element references as a way of populating or manipulating the DOM when Blazor interacts with the elements referenced.</span></span> <span data-ttu-id="ad4f9-168">Bunun yapılması, bildirim temelli işleme modeliyle karışabilir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-168">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="ad4f9-169">.NET kodu açısından düşünüldüğünde, donuk bir `ElementRef` tanıtıcıdır.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-169">As far as .NET code is concerned, an `ElementRef` is an opaque handle.</span></span> <span data-ttu-id="ad4f9-170">İle`ElementRef` yapabileceğiniz *tek* şey, JavaScript birlikte çalışması aracılığıyla JavaScript koduna geçer.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-170">The *only* thing you can do with `ElementRef` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="ad4f9-171">Bunu yaptığınızda, JavaScript tarafı kodu normal Dom API 'leri ile kullanılabilecek `HTMLElement` bir örnek alır.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-171">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="ad4f9-172">Örneğin, aşağıdaki kod bir öğe üzerinde odağı ayarlamaya izin veren bir .NET genişletme yöntemi tanımlar:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-172">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="ad4f9-173">*Examplejsınterop. js*:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-173">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="ad4f9-174">Öğesini `IJSRuntime.InvokeAsync<T>` kullanarak bir `exampleJsFunctions.focusElement` öğesi odaklamak `ElementRef` için kullanın ve ile çağırın:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-174">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementRef` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,7,11-12)]

<span data-ttu-id="ad4f9-175">Bir öğeyi odaklamak için bir genişletme yöntemi kullanmak için, `IJSRuntime` örneği alan bir statik genişletme yöntemi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-175">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementRef elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="ad4f9-176">Yöntemi doğrudan nesnesi üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-176">The method is called directly on the object.</span></span> <span data-ttu-id="ad4f9-177">Aşağıdaki örnek, statik `Focus` metodun `JsInteropClasses` ad alanından kullanılabildiğini varsayar:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-177">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,8,12)]

> [!IMPORTANT]
> <span data-ttu-id="ad4f9-178">`username` Değişken yalnızca bileşen işlendikten sonra doldurulur.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-178">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="ad4f9-179">Doldurulmamış bir `ElementRef` JavaScript koduna geçirilirse JavaScript kodu bir `null`değeri alır.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-179">If an unpopulated `ElementRef` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="ad4f9-180">Bileşen işlemeyi tamamladıktan sonra öğe başvurularını değiştirmek için (bir öğe üzerinde ilk odağı ayarlamak için) `OnAfterRenderAsync` veya `OnAfterRender` [bileşen yaşam döngüsü yöntemlerini](xref:blazor/components#lifecycle-methods) kullanın.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-180">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="ad4f9-181">JavaScript işlevlerinden .NET yöntemlerini çağır</span><span class="sxs-lookup"><span data-stu-id="ad4f9-181">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="ad4f9-182">Statik .NET yöntemi çağrısı</span><span class="sxs-lookup"><span data-stu-id="ad4f9-182">Static .NET method call</span></span>

<span data-ttu-id="ad4f9-183">JavaScript 'ten statik bir .NET yöntemi çağırmak için `DotNet.invokeMethod` veya `DotNet.invokeMethodAsync` işlevlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-183">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="ad4f9-184">Çağırmak istediğiniz statik metodun tanımlayıcısını, işlevi içeren derlemenin adını ve tüm bağımsız değişkenleri geçirin.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-184">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="ad4f9-185">Zaman uyumsuz sürüm, sunucu tarafı senaryolarını desteklemek için tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-185">The asynchronous version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="ad4f9-186">JavaScript 'ten bir .NET yöntemi çağırmak için, .net yönteminin public, static ve `[JSInvokable]` özniteliğe sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-186">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="ad4f9-187">Varsayılan olarak, yöntem tanımlayıcısı yöntem adıdır, ancak `JSInvokableAttribute` oluşturucuyu kullanarak farklı bir tanımlayıcı belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-187">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="ad4f9-188">Açık genel yöntemlerin çağrılması Şu anda desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-188">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="ad4f9-189">Örnek uygulama, bir dizi C# `int`öğeleri döndürmek için bir yöntem içerir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-189">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="ad4f9-190">`JSInvokable` Özniteliği yöntemine uygulanır.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-190">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="ad4f9-191">*Pages/Jsınterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-191">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="ad4f9-192">İstemciye sunulan JavaScript, C# .net yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-192">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="ad4f9-193">*Wwwroot/Examplejsınterop. js*:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-193">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="ad4f9-194">**Tetikleyici .net static yöntemi ReturnArrayAsync** düğmesi seçildiğinde, tarayıcının Web geliştirici araçlarında konsol çıkışını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-194">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="ad4f9-195">Konsol çıktısı:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-195">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="ad4f9-196">Dördüncü dizi değeri tarafından`data.push(4);` `ReturnArrayAsync`döndürülen diziye () gönderilir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-196">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="ad4f9-197">Örnek yöntem çağrısı</span><span class="sxs-lookup"><span data-stu-id="ad4f9-197">Instance method call</span></span>

<span data-ttu-id="ad4f9-198">JavaScript 'ten de .NET örnek yöntemlerini çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-198">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="ad4f9-199">JavaScript 'ten bir .NET örnek yöntemi çağırmak için:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-199">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="ad4f9-200">.Net örneğini bir `DotNetObjectRef` örneğe sarmalayarak JavaScript 'e geçirin.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-200">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectRef` instance.</span></span> <span data-ttu-id="ad4f9-201">.NET örneği, JavaScript 'e başvuruya göre geçirilir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-201">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="ad4f9-202">`invokeMethod` Or`invokeMethodAsync` işlevlerini kullanarak örnekte .NET örnek yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-202">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="ad4f9-203">.NET örneği, JavaScript 'ten başka .NET yöntemleri çağrılırken bir bağımsız değişken olarak da geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-203">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="ad4f9-204">Örnek uygulama, iletileri istemci tarafı konsoluna kaydeder.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-204">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="ad4f9-205">Örnek uygulama tarafından gösterilen aşağıdaki örnekler için tarayıcının geliştirici araçlarında tarayıcının konsol çıkışını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-205">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="ad4f9-206">**Tetikleyici .NET örnek yöntemi hellohelper. sayHello** düğmesi seçildiğinde, `ExampleJsInterop.CallHelloHelperSayHello` çağrılır ve yöntemine bir ad `Blazor`geçirir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-206">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="ad4f9-207">*Pages/Jsınterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-207">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="ad4f9-208">`CallHelloHelperSayHello`JavaScript işlevini `sayHello` yeni bir `HelloHelper`örneğiyle çağırır.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-208">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="ad4f9-209">*JsInteropClasses/Examplejsınterop. cs*:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-209">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="ad4f9-210">*Wwwroot/Examplejsınterop. js*:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-210">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="ad4f9-211">Ad, `HelloHelper.Name` özelliğini ayarlayan oluşturucuya `HelloHelper`geçirilir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-211">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="ad4f9-212">JavaScript işlevi `sayHello` yürütüldüğünde, `HelloHelper.SayHello` JavaScript işlevi tarafından konsola yazılan `Hello, {Name}!` iletiyi döndürür.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-212">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="ad4f9-213">*JsInteropClasses/HelloHelper. cs*:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-213">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="ad4f9-214">Tarayıcının Web geliştirici araçlarında konsol çıkışı:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-214">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="ad4f9-215">Birlikte çalışma kodunu bir sınıf kitaplığında paylaşma</span><span class="sxs-lookup"><span data-stu-id="ad4f9-215">Share interop code in a class library</span></span>

<span data-ttu-id="ad4f9-216">JavaScript birlikte çalışma kodu bir NuGet paketindeki kodu paylaşmanıza olanak sağlayan bir sınıf kitaplığına dahil edilebilir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-216">JavaScript interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="ad4f9-217">Sınıf kitaplığı, yerleşik derlemede JavaScript kaynaklarını katıştırmayı işler.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-217">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="ad4f9-218">JavaScript dosyaları *Wwwroot* klasörüne yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-218">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="ad4f9-219">Araç, kitaplık oluşturulduğunda kaynakları katıştırmaya önem kazanır.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-219">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="ad4f9-220">Oluşturulan NuGet paketine, uygulamanın proje dosyasında herhangi bir NuGet paketiyle aynı şekilde başvurulur.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-220">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="ad4f9-221">Paket geri yüklendikten sonra, uygulama kodu JavaScript 'e, gibi çağrı yapabilir C#.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-221">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="ad4f9-222">Daha fazla bilgi için bkz. <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-222">For more information, see <xref:blazor/class-libraries>.</span></span>
