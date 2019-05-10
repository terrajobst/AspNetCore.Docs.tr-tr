---
title: Blazor JavaScript birlikte çalışma
author: guardrex
description: .NET ve JavaScript işlevleri çağırmak nasıl öğrenin Blazor uygulamalarındaki JavaScript yöntemleri.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/25/2019
uid: blazor/javascript-interop
ms.openlocfilehash: f249d96d310c3d28b56e4920adda145ba07b34ee
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898655"
---
# <a name="blazor-javascript-interop"></a><span data-ttu-id="22dd3-103">Blazor JavaScript birlikte çalışma</span><span class="sxs-lookup"><span data-stu-id="22dd3-103">Blazor JavaScript interop</span></span>

<span data-ttu-id="22dd3-104">Tarafından [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="22dd3-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="22dd3-105">.NET ve JavaScript işlevleri Blazor uygulama çağırabilirsiniz JavaScript kodundan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="22dd3-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="22dd3-106">.NET yöntemlerinden JavaScript işlevleri çağırma</span><span class="sxs-lookup"><span data-stu-id="22dd3-106">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="22dd3-107">.NET kodu bir JavaScript işlevi çağırmak için gerekli olduğunda zamanlar vardır.</span><span class="sxs-lookup"><span data-stu-id="22dd3-107">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="22dd3-108">Örneğin, JavaScript çağrısı, tarayıcının yeteneklerini veya uygulamaya bir JavaScript kitaplığı işlevi kullanıma sunabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22dd3-108">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="22dd3-109">.NET içinde JavaScript çağırmak için kullanın `IJSRuntime` Özet.</span><span class="sxs-lookup"><span data-stu-id="22dd3-109">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="22dd3-110">`InvokeAsync<T>` Yöntemi, JSON seri hale getirilebilir bağımsız değişkenler herhangi bir sayıda birlikte çağrılacak istediğiniz JavaScript işlevi için bir tanımlayıcı alır.</span><span class="sxs-lookup"><span data-stu-id="22dd3-110">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="22dd3-111">Genel kapsam göre işlevi tanımlayıcıdır (`window`).</span><span class="sxs-lookup"><span data-stu-id="22dd3-111">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="22dd3-112">Çağrılacak istiyorsanız `window.someScope.someFunction`, tanımlayıcı `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="22dd3-112">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="22dd3-113">İşlevin çağrıldığı önce kaydetmeniz gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="22dd3-113">There's no need to register the function before it's called.</span></span> <span data-ttu-id="22dd3-114">Dönüş türü `T` ayrıca JSON seri hale getirilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="22dd3-114">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="22dd3-115">Sunucu tarafı uygulamalar için:</span><span class="sxs-lookup"><span data-stu-id="22dd3-115">For server-side apps:</span></span>

* <span data-ttu-id="22dd3-116">Birden çok kullanıcı isteklerini, sunucu tarafı uygulama tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="22dd3-116">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="22dd3-117">Remove() çağırmayın `JSRuntime.Current` JavaScript işlevleri çağırmak için bir bileşende.</span><span class="sxs-lookup"><span data-stu-id="22dd3-117">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="22dd3-118">Ekleme `IJSRuntime` soyutlama ve eklenen nesnenin JavaScript birlikte çalışma çağrıları kullanın.</span><span class="sxs-lookup"><span data-stu-id="22dd3-118">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>
* <span data-ttu-id="22dd3-119">Blazor uygulama prerendering karşın, bir tarayıcı bağlantı kuran taşınmadığından olası JavaScript çağırma değildir.</span><span class="sxs-lookup"><span data-stu-id="22dd3-119">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="22dd3-120">Daha fazla bilgi için [algılamak Blazor uygulama prerendering olduğunda](#detect-when-a-blazor-app-is-prerendering) bölümü.</span><span class="sxs-lookup"><span data-stu-id="22dd3-120">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="22dd3-121">Aşağıdaki örnek dayanır [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), Deneysel bir JavaScript tabanlı kod çözücü.</span><span class="sxs-lookup"><span data-stu-id="22dd3-121">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="22dd3-122">Örnek bir JavaScript işlevini çağırmak nasıl gösterir bir C# yöntemi.</span><span class="sxs-lookup"><span data-stu-id="22dd3-122">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="22dd3-123">JavaScript işlevi, bir bayt dizisinden kabul eden bir C# yöntemi, dizinin kodunu çözer ve bileşenine görüntülenmesi için metni döndürür.</span><span class="sxs-lookup"><span data-stu-id="22dd3-123">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="22dd3-124">İçinde `<head>` öğesinin *wwwroot/index.html*, kullanan bir işlev sağlayan `TextDecoder` geçirilen dizisinin kodunu çözmek için:</span><span class="sxs-lookup"><span data-stu-id="22dd3-124">Inside the `<head>` element of *wwwroot/index.html*, provide a function that uses `TextDecoder` to decode a passed array:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

<span data-ttu-id="22dd3-125">Kod önceki örnekte gösterildiği gibi JavaScript kodunu da bir JavaScript dosyasından yüklenemiyor (*.js*) içindeki betik dosyasının başvurusuyla *wwwroot/index.html* dosyası:</span><span class="sxs-lookup"><span data-stu-id="22dd3-125">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file in the *wwwroot/index.html* file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="22dd3-126">Aşağıdaki bileşen:</span><span class="sxs-lookup"><span data-stu-id="22dd3-126">The following component:</span></span>

* <span data-ttu-id="22dd3-127">Çağırır `ConvertArray` JavaScript işlevini kullanarak `JsRuntime` bir bileşen düğme (**dönüştürme dizi**) seçilir.</span><span class="sxs-lookup"><span data-stu-id="22dd3-127">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="22dd3-128">JavaScript işlev çağrıldıktan sonra geçirilen dizinin bir dizeye dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="22dd3-128">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="22dd3-129">Dize, görüntü bileşenine döndürülür.</span><span class="sxs-lookup"><span data-stu-id="22dd3-129">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

<span data-ttu-id="22dd3-130">Kullanılacak `IJSRuntime` soyutlama, aşağıdaki yaklaşımlardan birini benimseme:</span><span class="sxs-lookup"><span data-stu-id="22dd3-130">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="22dd3-131">Ekleme `IJSRuntime` soyutlama Razor bileşen içine (*.razor*):</span><span class="sxs-lookup"><span data-stu-id="22dd3-131">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* <span data-ttu-id="22dd3-132">Ekleme `IJSRuntime` Özet bir sınıf içinde (*.cs*):</span><span class="sxs-lookup"><span data-stu-id="22dd3-132">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* <span data-ttu-id="22dd3-133">Dinamik içerik oluşturma ile [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), kullanın `[Inject]` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="22dd3-133">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="22dd3-134">Bu konuda eşlik eden istemci-tarafı örnek uygulamada, kullanıcı girişini alır ve bir karşılama iletisi görüntüleyecek şekilde DOM ile etkileşim kuran istemci-tarafı uygulaması için iki JavaScript işlevleri kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="22dd3-134">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="22dd3-135">`showPrompt` &ndash; Kullanıcı girişi (kullanıcı adı) kabul etmek için bir istem oluşturur ve ad çağırana döner.</span><span class="sxs-lookup"><span data-stu-id="22dd3-135">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="22dd3-136">`displayWelcome` &ndash; Bir karşılama iletisi ile bir DOM nesnesi çağrıyı yapandan atar bir `id` , `welcome`.</span><span class="sxs-lookup"><span data-stu-id="22dd3-136">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="22dd3-137">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="22dd3-137">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="22dd3-138">Bir yerde `<script>` JavaScript dosyasında başvuruda etiketi *wwwroot/index.html* dosyası:</span><span class="sxs-lookup"><span data-stu-id="22dd3-138">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file:</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="22dd3-139">Yerleştirmeyin bir `<script>` çünkü bir bileşen dosyasında etiketi `<script>` etiketi dinamik olarak güncelleştirilemiyor.</span><span class="sxs-lookup"><span data-stu-id="22dd3-139">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="22dd3-140">JavaScript ile .NET yöntemleri birlikte çalışır *exampleJsInterop.js* çağırarak dosya `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="22dd3-140">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="22dd3-141">`IJSRuntime` Soyutlamadır için sunucu tarafı senaryoları için zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="22dd3-141">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="22dd3-142">İstemci tarafı uygulama çalışır ve eşzamanlı olarak, bir JavaScript işlevi çağırmak istiyorsanız alta `IJSInProcessRuntime` ve çağrı `Invoke<T>` yerine.</span><span class="sxs-lookup"><span data-stu-id="22dd3-142">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="22dd3-143">Çoğu JavaScript birlikte çalışma kitaplıkları tüm senaryolarda, istemci tarafı ve sunucu tarafı kitaplıklar emin olmak için zaman uyumsuz API'leri kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="22dd3-143">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="22dd3-144">Örnek uygulamayı JavaScript birlikte çalışma göstermek için bir bileşeni içerir.</span><span class="sxs-lookup"><span data-stu-id="22dd3-144">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="22dd3-145">Bileşeni:</span><span class="sxs-lookup"><span data-stu-id="22dd3-145">The component:</span></span>

* <span data-ttu-id="22dd3-146">JavaScript komut istemi aracılığıyla kullanıcı girişini alır.</span><span class="sxs-lookup"><span data-stu-id="22dd3-146">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="22dd3-147">İşleme bileşenine metni döndürür.</span><span class="sxs-lookup"><span data-stu-id="22dd3-147">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="22dd3-148">Bir karşılama iletisi görüntüleyecek şekilde DOM ile etkileşime giren ikinci bir JavaScript işlevi çağırır.</span><span class="sxs-lookup"><span data-stu-id="22dd3-148">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="22dd3-149">*Pages/JSInterop.razor*:</span><span class="sxs-lookup"><span data-stu-id="22dd3-149">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="22dd3-150">Zaman `TriggerJsPrompt` bileşenin seçerek yürütülür **tetikleyici JavaScript istemi** button, JavaScript `showPrompt` sağlanan işlev *wwwroot/exampleJsInterop.js* dosyasıdır çağrılır.</span><span class="sxs-lookup"><span data-stu-id="22dd3-150">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="22dd3-151">`showPrompt` İşlevi olan HTML olarak kodlanan ve döndürülen bileşenine (kullanıcı adı), kullanıcı girişi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="22dd3-151">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="22dd3-152">Bileşen kullanıcı adının bir yerel değişkende depolar `name`.</span><span class="sxs-lookup"><span data-stu-id="22dd3-152">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="22dd3-153">Dize depolanan `name` bir JavaScript işleve geçirilir, bir karşılama iletisi dahil `displayWelcome`, bir başlık etiketine Hoş Geldiniz iletisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="22dd3-153">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="detect-when-a-blazor-app-is-prerendering"></a><span data-ttu-id="22dd3-154">Blazor uygulama prerendering Algıla</span><span class="sxs-lookup"><span data-stu-id="22dd3-154">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="22dd3-155">Öğelere başvurular yakalama</span><span class="sxs-lookup"><span data-stu-id="22dd3-155">Capture references to elements</span></span>

<span data-ttu-id="22dd3-156">Bazı [JavaScript birlikte çalışma](xref:blazor/javascript-interop) senaryoları HTML öğeleri için başvuru gerektirir.</span><span class="sxs-lookup"><span data-stu-id="22dd3-156">Some [JavaScript interop](xref:blazor/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="22dd3-157">Örneğin, bir kullanıcı Arabirimi kitaplığı başlatma için öğe başvurusu gerektirebilir veya bir öğe üzerinde komut benzeri API'leri çağırmak ihtiyacınız olabilecek `focus` veya `play`.</span><span class="sxs-lookup"><span data-stu-id="22dd3-157">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="22dd3-158">HTML öğeleri aşağıdaki yaklaşımı kullanarak bir bileşende başvuruları yakalayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="22dd3-158">You can capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="22dd3-159">Ekleme bir `ref` özniteliği için HTML öğesi.</span><span class="sxs-lookup"><span data-stu-id="22dd3-159">Add a `ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="22dd3-160">Türünde bir alan tanımlayın `ElementRef` adıyla eşleşen `ref` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="22dd3-160">Define a field of type `ElementRef` whose name matches the value of the `ref` attribute.</span></span>

<span data-ttu-id="22dd3-161">Aşağıdaki örnek, bir başvuru yakalama gösterir `username` `<input>` öğesi:</span><span class="sxs-lookup"><span data-stu-id="22dd3-161">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input ref="username" ...>

@functions {
    ElementRef username;
}
```

> [!NOTE]
> <span data-ttu-id="22dd3-162">Yapmak **değil** yakalanan öğesi başvuruları yerli doldurma bir yol kullan</span><span class="sxs-lookup"><span data-stu-id="22dd3-162">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="22dd3-163">Bunun yapılması bildirim temelli işleme modeliyle etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="22dd3-163">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="22dd3-164">.NET kodu ilgili olduğu kadar bir `ElementRef` donuk bir işleyicisidir.</span><span class="sxs-lookup"><span data-stu-id="22dd3-164">As far as .NET code is concerned, an `ElementRef` is an opaque handle.</span></span> <span data-ttu-id="22dd3-165">*Yalnızca* ile yapabileceğiniz bir şey `ElementRef` olan JavaScript kodu aracılığıyla JavaScript birlikte çalışma geçirin.</span><span class="sxs-lookup"><span data-stu-id="22dd3-165">The *only* thing you can do with `ElementRef` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="22dd3-166">Bunu yaptığınızda, JavaScript tarafı kodunu alır bir `HTMLElement` normal DOM API'leri ile kullanabileceğiniz örnek.</span><span class="sxs-lookup"><span data-stu-id="22dd3-166">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="22dd3-167">Örneğin, aşağıdaki kod, bir öğede odak ayarlama sağlayan bir .NET genişletme yöntemi tanımlar:</span><span class="sxs-lookup"><span data-stu-id="22dd3-167">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="22dd3-168">*exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="22dd3-168">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="22dd3-169">Kullanım `IJSRuntime.InvokeAsync<T>` ve çağrı `exampleJsFunctions.focusElement` ile bir `ElementRef` öğenin odaklanmak için:</span><span class="sxs-lookup"><span data-stu-id="22dd3-169">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementRef` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,7,11-12)]

<span data-ttu-id="22dd3-170">Bir öğe odaklanmak için bir genişletme yöntemi kullanmak için alan bir statik genişletme yöntemi oluşturma `IJSRuntime` örneği:</span><span class="sxs-lookup"><span data-stu-id="22dd3-170">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementRef elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="22dd3-171">Yöntem doğrudan nesne üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="22dd3-171">The method is called directly on the object.</span></span> <span data-ttu-id="22dd3-172">Aşağıdaki örnek olduğunu varsayar statik `Focus` yöntemi kullanılabilir `JsInteropClasses` ad alanı:</span><span class="sxs-lookup"><span data-stu-id="22dd3-172">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,8,12)]

> [!IMPORTANT]
> <span data-ttu-id="22dd3-173">`username` Değişkeni bileşeni işler ve çıktısını içeren sonra yalnızca doldurulmuş `>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="22dd3-173">The `username` variable is only populated after the component renders and its output includes the `>` element.</span></span> <span data-ttu-id="22dd3-174">Bir doldurulmamış geçirmeye çalışırsanız `ElementRef` JavaScript kodu için JavaScript kodunu alır `null`.</span><span class="sxs-lookup"><span data-stu-id="22dd3-174">If you try to pass an unpopulated `ElementRef` to JavaScript code, the JavaScript code receives `null`.</span></span> <span data-ttu-id="22dd3-175">Bileşen kullanın (bir öğede ilk odağı ayarlamak için) işleme tamamlandıktan sonra öğesi başvuruları işlemek için `OnAfterRenderAsync` veya `OnAfterRender` [bileşen yaşam döngüsü yöntemleri](xref:blazor/components#lifecycle-methods).</span><span class="sxs-lookup"><span data-stu-id="22dd3-175">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="22dd3-176">JavaScript işlevleri .NET yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="22dd3-176">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="22dd3-177">Statik .NET yöntem çağrısı</span><span class="sxs-lookup"><span data-stu-id="22dd3-177">Static .NET method call</span></span>

<span data-ttu-id="22dd3-178">JavaScript .NET statik bir yöntemi çağırmak için `DotNet.invokeMethod` veya `DotNet.invokeMethodAsync` işlevleri.</span><span class="sxs-lookup"><span data-stu-id="22dd3-178">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="22dd3-179">Statik yöntem çağırmak istediğiniz işlevin yanı sıra, herhangi bir bağımsız değişken içeren derlemenin adını tanımlayıcıda geçirin.</span><span class="sxs-lookup"><span data-stu-id="22dd3-179">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="22dd3-180">Zaman uyumsuz sürümü, sunucu tarafı senaryoları desteklemek için tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="22dd3-180">The asynchronous version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="22dd3-181">JavaScript'ten bir .NET yöntemini çağırmak için .NET yöntemi ortak olmalıdır, statik ve `[JSInvokable]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="22dd3-181">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="22dd3-182">Varsayılan olarak, yöntem tanımlayıcısı yöntemi adıdır, ancak farklı bir kimlik kullanarak bir belirtebilirsiniz `JSInvokableAttribute` Oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="22dd3-182">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="22dd3-183">Açık genel yöntemleri çağırma şu anda desteklenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="22dd3-183">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="22dd3-184">Örnek uygulamayı içeren bir C# bir dizi döndürmek için yöntemin `int`s.</span><span class="sxs-lookup"><span data-stu-id="22dd3-184">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="22dd3-185">`JSInvokable` Özniteliği yönteme uygulanır.</span><span class="sxs-lookup"><span data-stu-id="22dd3-185">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="22dd3-186">*Pages/JsInterop.razor*:</span><span class="sxs-lookup"><span data-stu-id="22dd3-186">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="22dd3-187">Çağıran istemciye sunulan JavaScript C# .NET yöntemi.</span><span class="sxs-lookup"><span data-stu-id="22dd3-187">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="22dd3-188">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="22dd3-188">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="22dd3-189">Zaman **tetikleyici .NET statik yöntem ReturnArrayAsync** düğmesi seçildiğinde, tarayıcının web geliştirici araçları konsol çıkışını dikkatle inceleyin.</span><span class="sxs-lookup"><span data-stu-id="22dd3-189">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="22dd3-190">Konsol çıktısı şöyledir:</span><span class="sxs-lookup"><span data-stu-id="22dd3-190">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="22dd3-191">Dördüncü dizi değeri diziye gönderilir (`data.push(4);`) tarafından döndürülen `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="22dd3-191">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="22dd3-192">Örnek yöntem çağrısı</span><span class="sxs-lookup"><span data-stu-id="22dd3-192">Instance method call</span></span>

<span data-ttu-id="22dd3-193">JavaScript'ten .NET örnek yöntemler de çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="22dd3-193">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="22dd3-194">JavaScript bir .NET örneği yöntemi çağırmak için:</span><span class="sxs-lookup"><span data-stu-id="22dd3-194">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="22dd3-195">.NET örnek içinde sarmalama için JavaScript geçirin. bir `DotNetObjectRef` örneği.</span><span class="sxs-lookup"><span data-stu-id="22dd3-195">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectRef` instance.</span></span> <span data-ttu-id="22dd3-196">.NET örnek JavaScript başvurusu tarafından geçirilir.</span><span class="sxs-lookup"><span data-stu-id="22dd3-196">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="22dd3-197">.NET örnek yöntemlerini kullanma örneği `invokeMethod` veya `invokeMethodAsync` işlevleri.</span><span class="sxs-lookup"><span data-stu-id="22dd3-197">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="22dd3-198">.NET örnek JavaScript diğer .NET yöntemleri çağrılırken bağımsız değişken olarak de geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="22dd3-198">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="22dd3-199">Örnek uygulama, istemci tarafı konsola iletileri günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="22dd3-199">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="22dd3-200">Örnek uygulama tarafından gösterilen aşağıdaki örneklerde, tarayıcının geliştirici araçları konsol çıkışında tarayıcının inceleyin.</span><span class="sxs-lookup"><span data-stu-id="22dd3-200">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="22dd3-201">Zaman **tetikleyici .NET örnek yöntemi HelloHelper.SayHello** düğmesi seçili `ExampleJsInterop.CallHelloHelperSayHello` olarak adlandırılır ve bir ad geçirmeden `Blazor`, yönteme.</span><span class="sxs-lookup"><span data-stu-id="22dd3-201">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="22dd3-202">*Pages/JsInterop.razor*:</span><span class="sxs-lookup"><span data-stu-id="22dd3-202">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="22dd3-203">`CallHelloHelperSayHello` JavaScript işlevini çağıran `sayHello` ile yeni bir örneğini `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="22dd3-203">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="22dd3-204">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="22dd3-204">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="22dd3-205">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="22dd3-205">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="22dd3-206">Adı geçirilir `HelloHelper`ait ayarlar Oluşturucu `HelloHelper.Name` özelliği.</span><span class="sxs-lookup"><span data-stu-id="22dd3-206">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="22dd3-207">Zaman JavaScript işlevinin `sayHello` yürütüldüğünde, `HelloHelper.SayHello` döndürür `Hello, {Name}!` ileti konsola JavaScript işlevi yazılır.</span><span class="sxs-lookup"><span data-stu-id="22dd3-207">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="22dd3-208">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="22dd3-208">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="22dd3-209">Konsol, web geliştirici araçları tarayıcının çıktısı:</span><span class="sxs-lookup"><span data-stu-id="22dd3-209">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="22dd3-210">Bir sınıf kitaplığı'nda birlikte çalışma kod paylaşın</span><span class="sxs-lookup"><span data-stu-id="22dd3-210">Share interop code in a class library</span></span>

<span data-ttu-id="22dd3-211">JavaScript birlikte çalışma kodu, bir NuGet paketi kod paylaşmanıza olanak sağlayan bir sınıf kitaplığı'nda bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="22dd3-211">JavaScript interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="22dd3-212">Sınıf kitaplığı ekleme JavaScript kaynakları derlemesi işler.</span><span class="sxs-lookup"><span data-stu-id="22dd3-212">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="22dd3-213">JavaScript dosyaları yerleştirilir *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="22dd3-213">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="22dd3-214">Alet kullanımı dışındaki kaynaklar ekleme kitaplık oluşturulduğunda üstlenir.</span><span class="sxs-lookup"><span data-stu-id="22dd3-214">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="22dd3-215">Yalnızca normal bir NuGet paketi başvurulduğu yerleşik NuGet paketini uygulamanın proje dosyasında başvurulan.</span><span class="sxs-lookup"><span data-stu-id="22dd3-215">The built NuGet package is referenced in the project file of the app just as any normal NuGet package is referenced.</span></span> <span data-ttu-id="22dd3-216">Uygulama geri yüklendikten sonra olarak bulunuyorlarmış uygulama kodu içinde JavaScript çağırabilirsiniz C#.</span><span class="sxs-lookup"><span data-stu-id="22dd3-216">After the app is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="22dd3-217">Daha fazla bilgi için bkz. <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="22dd3-217">For more information, see <xref:blazor/class-libraries>.</span></span>
