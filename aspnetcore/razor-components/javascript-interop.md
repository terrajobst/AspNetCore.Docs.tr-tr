---
title: Razor bileşenleri JavaScript birlikte çalışma
author: guardrex
description: .NET ve JavaScript işlevleri çağırmak nasıl öğrenin Blazor ve Razor bileşenleri uygulamalarındaki JavaScript yöntemleri.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/javascript-interop
ms.openlocfilehash: ac772b052a8f61937350b0d999013b7ba06dfd74
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265006"
---
# <a name="razor-components-javascript-interop"></a><span data-ttu-id="d1b6f-103">Razor bileşenleri JavaScript birlikte çalışma</span><span class="sxs-lookup"><span data-stu-id="d1b6f-103">Razor Components JavaScript interop</span></span>

<span data-ttu-id="d1b6f-104">Tarafından [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d1b6f-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d1b6f-105">Bir Razor bileşenleri uygulaması JavaScript işlevleri .NET ve .NET çağırabilirsiniz JavaScript kodundan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-105">A Razor Components app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="d1b6f-106">.NET yöntemlerinden JavaScript işlevleri çağırma</span><span class="sxs-lookup"><span data-stu-id="d1b6f-106">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="d1b6f-107">.NET kodu bir JavaScript işlevi çağırmak için gerekli olduğunda zamanlar vardır.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-107">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="d1b6f-108">Örneğin, JavaScript çağrısı, tarayıcının yeteneklerini veya uygulamaya bir JavaScript kitaplığı işlevi kullanıma sunabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-108">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="d1b6f-109">.NET içinde JavaScript çağırmak için kullanın `IJSRuntime` Özet.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-109">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="d1b6f-110">`InvokeAsync<T>` Yöntemi, JSON seri hale getirilebilir bağımsız değişkenler herhangi bir sayıda birlikte çağrılacak istediğiniz JavaScript işlevi için bir tanımlayıcı alır.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-110">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="d1b6f-111">Genel kapsam göre işlevi tanımlayıcıdır (`window`).</span><span class="sxs-lookup"><span data-stu-id="d1b6f-111">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="d1b6f-112">Çağrılacak istiyorsanız `window.someScope.someFunction`, tanımlayıcı `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-112">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="d1b6f-113">İşlevin çağrıldığı önce kaydetmeniz gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-113">There's no need to register the function before it's called.</span></span> <span data-ttu-id="d1b6f-114">Dönüş türü `T` ayrıca JSON seri hale getirilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-114">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="d1b6f-115">Sunucu tarafı ASP.NET Core Razor bileşenleri uygulamalar için:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-115">For server-side ASP.NET Core Razor Components apps:</span></span>

* <span data-ttu-id="d1b6f-116">Birden çok kullanıcı isteklerini, sunucu tarafı uygulama tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-116">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="d1b6f-117">Remove() çağırmayın `JSRuntime.Current` JavaScript işlevleri çağırmak için bir bileşende.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-117">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="d1b6f-118">Ekleme `IJSRuntime` soyutlama ve eklenen nesnenin JavaScript birlikte çalışma çağrıları kullanın.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-118">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>

<span data-ttu-id="d1b6f-119">Aşağıdaki örnek dayanır [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), Deneysel bir JavaScript tabanlı kod çözücü.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-119">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="d1b6f-120">Örnek bir JavaScript işlevini çağırmak nasıl gösterir bir C# yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-120">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="d1b6f-121">JavaScript işlevi, bir bayt dizisinden kabul eden bir C# yöntemi, dizinin kodunu çözer ve bileşenine görüntülenmesi için metni döndürür.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-121">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="d1b6f-122">İçinde `<head>` öğesinin *wwwroot/index.html*, kullanan bir işlev sağlayan `TextDecoder` geçirilen dizisinin kodunu çözmek için:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-122">Inside the `<head>` element of *wwwroot/index.html*, provide a function that uses `TextDecoder` to decode a passed array:</span></span>

```html
<script>
  window.ConvertArray = (win1251Array) => {
    var win1251decoder = new TextDecoder('windows-1251');
    var bytes = new Uint8Array(win1251Array);
    var decodedArray = win1251decoder.decode(bytes);
    console.log(decodedArray);
    return decodedArray;
  };
</script>
```

<span data-ttu-id="d1b6f-123">Kod önceki örnekte gösterildiği gibi JavaScript kodunu da bir JavaScript dosyasından yüklenemiyor (*.js*) içindeki betik dosyasının başvurusuyla *wwwroot/index.html* dosyası:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-123">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file in the *wwwroot/index.html* file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="d1b6f-124">Aşağıdaki bileşen:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-124">The following component:</span></span>

* <span data-ttu-id="d1b6f-125">Çağırır `ConvertArray` JavaScript işlevini kullanarak `JsRuntime` bir bileşen düğme (**dönüştürme dizi**) seçilir.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-125">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="d1b6f-126">JavaScript işlev çağrıldıktan sonra geçirilen dizinin bir dizeye dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-126">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="d1b6f-127">Dize, görüntü bileşenine döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-127">The string is returned to the component for display.</span></span>

```cshtml
@page "/"
@inject IJSRuntime JsRuntime;

<h1>Call JavaScript Function Example</h1>

<button type="button" class="btn btn-primary" onclick="@ConvertArray">
    Convert Array
</button>

<p class="mt-2" style="font-size:1.6em">
    <span class="badge badge-success">
        @ConvertedText
    </span>
</p>

@functions {
    // Quote (c)2005 Universal Pictures: Serenity
    // https://www.uphe.com/movies/serenity
    // David Krumholtz on IMDB: https://www.imdb.com/name/nm0472710/

    private MarkupString ConvertedText =
        new MarkupString("Select the <b>Convert Array</b> button.");

    private uint[] QuoteArray = new uint[]
        {
            60, 101, 109, 62, 67, 97, 110, 39, 116, 32, 115, 116, 111, 112, 32,
            116, 104, 101, 32, 115, 105, 103, 110, 97, 108, 44, 32, 77, 97,
            108, 46, 60, 47, 101, 109, 62, 32, 45, 32, 77, 114, 46, 32, 85, 110,
            105, 118, 101, 114, 115, 101, 10, 10,
        };

    async void ConvertArray()
    {
        var text =
            await JsRuntime.InvokeAsync<string>("ConvertArray", QuoteArray);

        ConvertedText = new MarkupString(text);

        StateHasChanged();
    }
}
```

<span data-ttu-id="d1b6f-128">Kullanılacak `IJSRuntime` soyutlama, aşağıdaki yaklaşımlardan birini benimseme:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-128">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="d1b6f-129">Ekleme `IJSRuntime` soyutlama Razor dosyasına (*.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="d1b6f-129">Inject the `IJSRuntime` abstraction into the Razor file (*.cshtml*):</span></span>

  ```cshtml
  @inject IJSRuntime JSRuntime

  @functions {
      public override void OnInit()
      {
          StocksService.OnStockTickerUpdated += stockUpdate =>
          {
              JSRuntime.InvokeAsync<object>(
                  "handleTickerChanged",
                  stockUpdate.symbol,
                  stockUpdate.price);
          };
      }
  }
  ```

* <span data-ttu-id="d1b6f-130">Ekleme `IJSRuntime` Özet bir sınıf içinde (*.cs*):</span><span class="sxs-lookup"><span data-stu-id="d1b6f-130">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  ```csharp
  public class MyJsInterop
  {
      private readonly IJSRuntime _jsRuntime;

      public MyJsInterop(IJSRuntime jsRuntime)
      {
          _jsRuntime = jsRuntime;
      }

      public Task<string> DoSomething(string data)
      {
          // The doSomething JavaScript method is implemented
          // in a JavaScript file, such as 'wwwroot/MyJsInterop.js'.
          return _jsRuntime.InvokeAsync<string>(
              "myJsFunctions.doSomething",
              data);
      }
  }
  ```

* <span data-ttu-id="d1b6f-131">Dinamik içerik oluşturma ile `BuildRenderTree`, kullanın `[Inject]` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-131">For dynamic content generation with `BuildRenderTree`, use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject] IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="d1b6f-132">Bu konuda eşlik eden istemci-tarafı örnek uygulamada, kullanıcı girişini alır ve bir karşılama iletisi görüntüleyecek şekilde DOM ile etkileşim kuran istemci-tarafı uygulaması için iki JavaScript işlevleri kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-132">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="d1b6f-133">`showPrompt` &ndash; Kullanıcı girişi (kullanıcı adı) kabul etmek için bir istem oluşturur ve ad çağırana döner.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-133">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="d1b6f-134">`displayWelcome` &ndash; Bir karşılama iletisi ile bir DOM nesnesi çağrıyı yapandan atar bir `id` , `welcome`.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-134">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="d1b6f-135">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-135">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="d1b6f-136">Bir yerde `<script>` JavaScript dosyasında başvuruda etiketi *wwwroot/index.html* dosyası:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-136">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file:</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="d1b6f-137">Yerleştirmeyin bir `<script>` çünkü bir bileşen dosyasında etiketi `<script>` etiketi dinamik olarak güncelleştirilemiyor.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-137">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="d1b6f-138">JavaScript ile .NET yöntemleri birlikte çalışır *exampleJsInterop.js* çağırarak dosya `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-138">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="d1b6f-139">Örnek uygulamayı kullanan bir çift C# yöntemleri `Prompt` ve `Display`, çağrılacak `showPrompt` ve `displayWelcome` JavaScript işlevleri:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-139">The sample app uses a pair of C# methods, `Prompt` and `Display`, to invoke the `showPrompt` and `displayWelcome` JavaScript functions:</span></span>

<span data-ttu-id="d1b6f-140">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-140">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=13-15,21-23)]

<span data-ttu-id="d1b6f-141">`IJSRuntime` Soyutlamadır için sunucu tarafı senaryoları için zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-141">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="d1b6f-142">İstemci tarafı uygulama çalışır ve eşzamanlı olarak, bir JavaScript işlevi çağırmak istiyorsanız alta `IJSInProcessRuntime` ve çağrı `Invoke<T>` yerine.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-142">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="d1b6f-143">Çoğu JavaScript birlikte çalışma kitaplıkları tüm senaryolarda, istemci tarafı ve sunucu tarafı kitaplıklar emin olmak için zaman uyumsuz API'leri kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-143">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="d1b6f-144">Örnek uygulamayı JavaScript birlikte çalışma göstermek için bir bileşeni içerir.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-144">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="d1b6f-145">Bileşeni:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-145">The component:</span></span>

* <span data-ttu-id="d1b6f-146">JavaScript komut istemi aracılığıyla kullanıcı girişini alır.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-146">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="d1b6f-147">İşleme bileşenine metni döndürür.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-147">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="d1b6f-148">Bir karşılama iletisi görüntüleyecek şekilde DOM ile etkileşime giren ikinci bir JavaScript işlevi çağırır.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-148">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="d1b6f-149">*Pages/JSInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-149">*Pages/JSInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=1&end=21)]

1. <span data-ttu-id="d1b6f-150">Zaman `TriggerJsPrompt` bileşenin seçerek yürütülür **tetikleyici JavaScript istemi** düğmesini `ExampleJsInterop.Prompt` yönteminde C# kod çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-150">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the `ExampleJsInterop.Prompt` method in C# code is called.</span></span>
1. <span data-ttu-id="d1b6f-151">`Prompt` Yöntemini yürütür JavaScript `showPrompt` sağlanan işlev *wwwroot/exampleJsInterop.js* dosya.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-151">The `Prompt` method executes the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file.</span></span>
1. <span data-ttu-id="d1b6f-152">`showPrompt` İşlevi olan HTML olarak kodlanan ve iade (kullanıcı adı), kullanıcı girişi kabul eden `Prompt` yöntemi ve sonuçta bileşenine yedekleyin.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-152">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the `Prompt` method and ultimately back to the component.</span></span> <span data-ttu-id="d1b6f-153">Bileşen kullanıcı adının bir yerel değişkende depolar `name`.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-153">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="d1b6f-154">Dize depolanan `name` ikinci bir geçirilen bir karşılama iletisi eklenmiştir C# yöntemi `ExampleJsInterop.Display`.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-154">The string stored in `name` is incorporated into a welcome message, which is passed to a second C# method, `ExampleJsInterop.Display`.</span></span>
1. <span data-ttu-id="d1b6f-155">`Display` bir JavaScript işlevini çağıran `displayWelcome`, bir başlık etiketine Hoş Geldiniz iletisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-155">`Display` calls a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="capture-references-to-elements"></a><span data-ttu-id="d1b6f-156">Öğelere başvurular yakalama</span><span class="sxs-lookup"><span data-stu-id="d1b6f-156">Capture references to elements</span></span>

<span data-ttu-id="d1b6f-157">Bazı [JavaScript birlikte çalışma](xref:razor-components/javascript-interop) senaryoları HTML öğeleri için başvuru gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-157">Some [JavaScript interop](xref:razor-components/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="d1b6f-158">Örneğin, bir kullanıcı Arabirimi kitaplığı başlatma için öğe başvurusu gerektirebilir veya bir öğe üzerinde komut benzeri API'leri çağırmak ihtiyacınız olabilecek `focus` veya `play`.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-158">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="d1b6f-159">HTML öğesi sayısında ciddi bir bileşen başvuruları ekleyerek yakalamak için bir `ref` öznitelik HTML öğesine ve ardından türünde bir alan tanımlama `ElementRef` adıyla eşleşen `ref` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-159">You can capture references to HTML elements in a component by adding a `ref` attribute to the HTML element and then defining a field of type `ElementRef` whose name matches the value of the `ref` attribute.</span></span>

<span data-ttu-id="d1b6f-160">Aşağıdaki örnek, bir kullanıcı adı giriş öğeye başvuru yakalama gösterir:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-160">The following example shows capturing a reference to the username input element:</span></span>

```cshtml
<input ref="username" ... />

@functions {
    ElementRef username;
}
```

> [!NOTE]
> <span data-ttu-id="d1b6f-161">Yapmak **değil** yakalanan öğesi başvuruları yerli doldurma bir yol kullan</span><span class="sxs-lookup"><span data-stu-id="d1b6f-161">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="d1b6f-162">Bunun yapılması bildirim temelli işleme modeliyle etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-162">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="d1b6f-163">.NET kodu ilgili olduğu kadar bir `ElementRef` donuk bir işleyicisidir.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-163">As far as .NET code is concerned, an `ElementRef` is an opaque handle.</span></span> <span data-ttu-id="d1b6f-164">*Yalnızca* ile yapabileceğiniz bir şey `ElementRef` olan JavaScript kodu aracılığıyla JavaScript birlikte çalışma geçirin.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-164">The *only* thing you can do with `ElementRef` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="d1b6f-165">Bunu yaptığınızda, JavaScript tarafı kodunu alır bir `HTMLElement` normal DOM API'leri ile kullanabileceğiniz örnek.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-165">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="d1b6f-166">Örneğin, aşağıdaki kod, bir öğede odak ayarlama sağlayan bir .NET genişletme yöntemi tanımlar:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-166">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="d1b6f-167">*mylib.js*:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-167">*mylib.js*:</span></span>

```javascript
window.myLib = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="d1b6f-168">*ElementRefExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-168">*ElementRefExtensions.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components;
using Microsoft.JSInterop;
using System.Threading.Tasks;

namespace MyLib
{
    public static class MyLibElementRefExtensions
    {
        private readonly IJSRuntime _jsRuntime;

        public MyJsInterop(IJSRuntime jsRuntime)
        {
            _jsRuntime = jsRuntime;
        }

        public static Task Focus(this ElementRef elementRef)
        {
            return _jsRuntime.InvokeAsync<object>(
                "myLib.focusElement", elementRef);
        }
    }
}
```

<span data-ttu-id="d1b6f-169">Kullanım `MyLib` ve çağrı `Focus` üzerinde bir `ElementRef` odak girişlerinde herhangi bir bileşeni için:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-169">Use `MyLib` and call `Focus` on an `ElementRef` to focus inputs in any component:</span></span>

```cshtml
@using MyLib

<input ref="username" />
<button onclick="@SetFocus">Set focus</button>

@functions {
    ElementRef username;

    void SetFocus()
    {
        username.Focus();
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="d1b6f-170">`username` Değişkeni bileşeni işler ve çıktısını içeren sonra yalnızca doldurulmuş `<input>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-170">The `username` variable is only populated after the component renders and its output includes the `<input>` element.</span></span> <span data-ttu-id="d1b6f-171">Bir doldurulmamış geçirmeye çalışırsanız `ElementRef` JavaScript kodu için JavaScript kodunu alır `null`.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-171">If you try to pass an unpopulated `ElementRef` to JavaScript code, the JavaScript code receives `null`.</span></span> <span data-ttu-id="d1b6f-172">Bileşen kullanın (bir öğede ilk odağı ayarlamak için) işleme tamamlandıktan sonra öğesi başvuruları işlemek için `OnAfterRenderAsync` veya `OnAfterRender` [bileşen yaşam döngüsü yöntemleri](xref:razor-components/components#lifecycle-methods).</span><span class="sxs-lookup"><span data-stu-id="d1b6f-172">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:razor-components/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="d1b6f-173">JavaScript işlevleri .NET yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="d1b6f-173">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="d1b6f-174">Statik .NET yöntem çağrısı</span><span class="sxs-lookup"><span data-stu-id="d1b6f-174">Static .NET method call</span></span>

<span data-ttu-id="d1b6f-175">JavaScript .NET statik bir yöntemi çağırmak için `DotNet.invokeMethod` veya `DotNet.invokeMethodAsync` işlevleri.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-175">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="d1b6f-176">Statik yöntem çağırmak istediğiniz işlevin yanı sıra, herhangi bir bağımsız değişken içeren derlemenin adını tanımlayıcıda geçirin.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-176">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="d1b6f-177">Zaman uyumsuz sürümü, sunucu tarafı senaryoları desteklemek için tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-177">The asynchronous version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="d1b6f-178">JavaScript'ten çağırma olması için .NET yöntemi genel, statik ve ile donatılmış olmalıdır `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-178">To be invokable from JavaScript, the .NET method must be public, static, and decorated with `[JSInvokable]`.</span></span> <span data-ttu-id="d1b6f-179">Varsayılan olarak, yöntem tanımlayıcısı yöntemi adıdır, ancak farklı bir kimlik kullanarak bir belirtebilirsiniz `JSInvokableAttribute` Oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-179">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="d1b6f-180">Açık genel yöntemleri çağırma şu anda desteklenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-180">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="d1b6f-181">Örnek uygulamayı içeren bir C# bir dizi döndürmek için yöntemin `int`s.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-181">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="d1b6f-182">Yöntem ile donatılmış `JSInvokable` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-182">The method is decorated with the `JSInvokable` attribute.</span></span>

<span data-ttu-id="d1b6f-183">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-183">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=48&end=59&highlight=7-11)]

<span data-ttu-id="d1b6f-184">Çağıran istemciye sunulan JavaScript C# .NET yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-184">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="d1b6f-185">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-185">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-12)]

<span data-ttu-id="d1b6f-186">Zaman **tetikleyici .NET statik yöntem ReturnArrayAsync** düğmesi seçildiğinde, tarayıcının web geliştirici araçları konsol çıkışını dikkatle inceleyin.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-186">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="d1b6f-187">Konsol çıktısı şöyledir:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-187">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="d1b6f-188">Dördüncü dizi değeri diziye gönderilir (`data.push(4);`) tarafından döndürülen `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-188">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="d1b6f-189">Örnek yöntem çağrısı</span><span class="sxs-lookup"><span data-stu-id="d1b6f-189">Instance method call</span></span>

<span data-ttu-id="d1b6f-190">JavaScript'ten .NET örnek yöntemler de çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-190">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="d1b6f-191">JavaScript bir .NET örneği yöntemi çağırmak için ilk .NET örnek JavaScript sarmalama tarafından başarılı bir `DotNetObjectRef` örneği.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-191">To invoke a .NET instance method from JavaScript, first pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectRef` instance.</span></span> <span data-ttu-id="d1b6f-192">.NET örnek JavaScript başvurusu tarafından geçirilir ve örneği kullanarak .NET örnek yöntemler çağırabilirsiniz `invokeMethod` veya `invokeMethodAsync` işlevleri.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-192">The .NET instance is passed by reference to JavaScript, and you can invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="d1b6f-193">.NET örnek JavaScript diğer .NET yöntemleri çağrılırken bağımsız değişken olarak de geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-193">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="d1b6f-194">Örnek uygulama, istemci tarafı konsola iletileri günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-194">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="d1b6f-195">Örnek uygulama tarafından gösterilen aşağıdaki örneklerde, tarayıcının geliştirici araçları konsol çıkışında tarayıcının inceleyin.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-195">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="d1b6f-196">Zaman **tetikleyici .NET örnek yöntemi HelloHelper.SayHello** düğmesi seçili `ExampleJsInterop.CallHelloHelperSayHello` olarak adlandırılır ve bir ad geçirmeden `Blazor`, yönteme.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-196">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="d1b6f-197">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-197">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=61&end=71&highlight=8-9)]

<span data-ttu-id="d1b6f-198">`CallHelloHelperSayHello` JavaScript işlevini çağıran `sayHello` ile yeni bir örneğini `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-198">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="d1b6f-199">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-199">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=26-32)]

<span data-ttu-id="d1b6f-200">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-200">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-17)]

<span data-ttu-id="d1b6f-201">Adı geçirilir `HelloHelper`ait ayarlar Oluşturucu `HelloHelper.Name` özelliği.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-201">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="d1b6f-202">Zaman JavaScript işlevinin `sayHello` yürütüldüğünde, `HelloHelper.SayHello` döndürür `Hello, {Name}!` ileti konsola JavaScript işlevi yazılır.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-202">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="d1b6f-203">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-203">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="d1b6f-204">Konsol, web geliştirici araçları tarayıcının çıktısı:</span><span class="sxs-lookup"><span data-stu-id="d1b6f-204">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a><span data-ttu-id="d1b6f-205">Bir Razor bileşen Sınıf Kitaplığı'nda birlikte çalışma kod paylaşın</span><span class="sxs-lookup"><span data-stu-id="d1b6f-205">Share interop code in a Razor Component class library</span></span>

<span data-ttu-id="d1b6f-206">JavaScript birlikte çalışma kodu Razor bileşen sınıf kitaplığında dahil edilebilir (`dotnet new razorclasslib`), bir NuGet paketi kod paylaşmanıza imkan tanıyan.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-206">JavaScript interop code can be included in a Razor Component class library (`dotnet new razorclasslib`), which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="d1b6f-207">Razor bileşen sınıf kitaplığı ekleme JavaScript kaynakları derlemesi işler.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-207">The Razor Component class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="d1b6f-208">JavaScript dosyaları yerleştirilir *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-208">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="d1b6f-209">Alet kullanımı dışındaki kaynaklar ekleme kitaplık oluşturulduğunda üstlenir.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-209">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="d1b6f-210">Yalnızca normal bir NuGet paketi başvurulduğu yerleşik NuGet paketini uygulamanın proje dosyasında başvurulan.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-210">The built NuGet package is referenced in the project file of the app just as any normal NuGet package is referenced.</span></span> <span data-ttu-id="d1b6f-211">Uygulama geri yüklendikten sonra olarak bulunuyorlarmış uygulama kodu içinde JavaScript çağırabilirsiniz C#.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-211">After the app is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="d1b6f-212">Daha fazla bilgi için bkz. <xref:razor-components/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="d1b6f-212">For more information, see <xref:razor-components/class-libraries>.</span></span>
