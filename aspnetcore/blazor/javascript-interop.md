---
title: ASP.NET Core Blazor JavaScript birlikte çalışması
author: guardrex
description: Blazor Apps 'teki JavaScript 'ten .NET ve .NET yöntemlerinden JavaScript işlevlerini çağırmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/javascript-interop
ms.openlocfilehash: 2b5d1433fce6e09adf3caa58e55e678b00ad98ee
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71211649"
---
# <a name="aspnet-core-blazor-javascript-interop"></a>ASP.NET Core Blazor JavaScript birlikte çalışması

Sağlayan [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)ve [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Bir Blazor uygulaması, JavaScript kodundan .NET ve .NET yöntemlerinden JavaScript işlevlerini çağırabilir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>.NET metotlarından JavaScript işlevlerini çağırma

Bir JavaScript işlevini çağırmak için .NET kodunun gerekli olduğu durumlar vardır. Örneğin, JavaScript çağrısı, tarayıcı özelliklerini veya bir JavaScript kitaplığından uygulamaya yönelik işlevselliği sunabilir.

.Net 'ten JavaScript 'e çağrı yapmak için `IJSRuntime` soyutlamayı kullanın. Yöntemi `InvokeAsync<T>` , herhangi bir sayıda JSON seri hale getirilebilir bağımsız değişkenle birlikte çağırmak istediğiniz JavaScript işlevi için bir tanımlayıcı alır. İşlev tanımlayıcısı genel kapsama (`window`) göredir. Çağırmak `window.someScope.someFunction`isterseniz, tanımlayıcı olur `someScope.someFunction`. Çağrılmadan önce işlevi kaydetmeniz gerekmez. Dönüş türünün `T` de JSON seri hale getirilebilir olması gerekir.

Blazor Server uygulamaları için:

* Birden çok kullanıcı isteği, Blazor Server uygulaması tarafından işlenir. JavaScript işlevlerini `JSRuntime.Current` çağırmak için bir bileşeni çağırmayın.
* `IJSRuntime` Soyutlamayı ekler ve JavaScript birlikte çalışma çağrılarını vermek için eklenen nesneyi kullanın.
* Blazor uygulaması prerendering olduğunda, tarayıcıyla bir bağlantı kurulmadığı için JavaScript 'e çağrı yapılamaz. Daha fazla bilgi için bkz. [bir Blazor uygulaması ne zaman prerendering](#detect-when-a-blazor-app-is-prerendering) bölümüne bakın.

Aşağıdaki örnek, deneysel bir JavaScript tabanlı kod çözücüsü olan [Textdecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)tabanlıdır. Örnek, bir C# yöntemden JavaScript işlevinin nasıl çağrılacağını gösterir. JavaScript işlevi bir C# yöntemden bir bayt dizisi kabul eder, dizinin kodunu çözer ve görüntülenecek metni bileşene döndürür.

Wwwroot/index.html (Blazor webassembly) veya *Pages/_host. cshtml* (Blazor Server) `TextDecoder` öğesininiçinde,geçirilenbirdizininkodunuçözmekiçinkullananbirişlev`<head>` sağlayın:

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

Önceki örnekte gösterilen kod gibi JavaScript kodu, betik dosyasına yönelik bir başvuruya sahip bir JavaScript dosyasından ( *. js*) de yüklenebilir:

```html
<script src="exampleJsInterop.js"></script>
```

Aşağıdaki bileşen:

* Bir bileşen düğmesi (**diziyi Dönüştür**) seçildiğinde `JsRuntime` JavaScriptişleviniçağırır.`ConvertArray`
* JavaScript işlevi çağrıldıktan sonra, geçirilen dizi bir dizeye dönüştürülür. Dize, görüntüleme için bileşene döndürülür.

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

`IJSRuntime` Soyutlamayı kullanmak için aşağıdaki yaklaşımlardan birini benimseyin:

* Soyutlama Razor bileşenine ( *. Razor*) ekleme: `IJSRuntime`

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* Soyutlamayı bir sınıfa ( *. cs*) Ekle: `IJSRuntime`

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* [Buildrendertree](xref:blazor/components#manual-rendertreebuilder-logic)ile dinamik içerik oluşturma için `[Inject]` özniteliğini kullanın:

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

Bu konuya eşlik eden istemci tarafı örnek uygulamada, Kullanıcı girişi almak ve bir hoş geldiniz iletisi göstermek üzere DOM ile etkileşime geçen uygulama için iki JavaScript işlevi mevcuttur:

* `showPrompt`&ndash; Kullanıcı girişini kabul etmek için bir istem üretir (kullanıcının adı) ve arayan adına adı döndürür.
* `displayWelcome`' A `id` sahip bir DOM nesnesine çağırandan bir hoş geldiniz iletisi atar. `welcome` &ndash;

*Wwwroot/Examplejsınterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

JavaScript dosyasına başvuran etiketiWwwroot/index.htmldosyasına(Blazorwebassembly)veyaPages/_host.cshtmldosyasına(BlazorServer)`<script>` yerleştirin.

*Wwwroot/index.html* (Blazor WebAssembly):

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

*Pages/_Host. cshtml* (Blazor sunucusu):

[!code-cshtml[](javascript-interop/samples_snapshot/_Host.cshtml?highlight=29)]

`<script>` Etiket dinamik olarak `<script>` güncelleştirilemediğinden bir bileşen dosyasına etiket yerleştirmeyin.

.NET yöntemleri, çağırarak `IJSRuntime.InvokeAsync<T>`, *examplejsınterop. js* dosyasındaki JavaScript işlevleriyle birlikte çalışır.

Soyutlama `IJSRuntime` , Blazor sunucu senaryolarına izin vermek için zaman uyumsuzdur. Uygulama bir Blazor webassembly uygulaması ise ve bir JavaScript işlevini zaman uyumlu olarak çağırmak istiyorsanız bunun yerine alt türe çevirme `IJSInProcessRuntime` yapın ve `Invoke<T>` çağırın. Çoğu JavaScript birlikte çalışma kitaplıklarının, kitaplıkların tüm senaryolarda kullanılabilir olduğundan emin olmak için zaman uyumsuz API 'Leri kullanmasını öneririz.

Örnek uygulama, JavaScript birlikte çalışabildiğini gösteren bir bileşen içerir. Bileşen:

* Bir JavaScript istemi aracılığıyla Kullanıcı girişini alır.
* İşlemek için bileşene metni döndürür.
* Bir hoş geldiniz iletisini göstermek için DOM ile etkileşime sahip ikinci bir JavaScript işlevini çağırır.

*Pages/Jsınterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. Bileşenin tetikleme **JavaScript istem** düğmesi seçilerek çalıştırıldığında, `showPrompt` *Wwwroot/examplejsınterop. js* dosyasında verilen JavaScript işlevi çağırılır. `TriggerJsPrompt`
1. `showPrompt` İşlevi, HTML kodlu ve bileşene döndürülen kullanıcı girişini (kullanıcının adı) kabul eder. Bileşen, kullanıcının adını yerel bir değişkende `name`depolar.
1. İçinde `name` depolanan dize, hoş geldiniz iletisini bir başlık etiketine işleyen bir JavaScript `displayWelcome`işlevine iletilen bir hoş geldiniz iletisine eklenmiştir.

## <a name="call-a-void-javascript-function"></a>Void JavaScript işlevini çağırın

[Void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) veya [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) döndüren JavaScript işlevleri ile `IJSRuntime.InvokeVoidAsync`çağırılır.

## <a name="detect-when-a-blazor-app-is-prerendering"></a>Bir Blazor uygulamasının ne zaman prerendering olduğunu Algıla
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>Öğelere başvuruları yakala

Bazı [JavaScript birlikte çalışma](xref:blazor/javascript-interop) senaryoları HTML öğelerine başvuru gerektirir. Örneğin, bir kullanıcı arabirimi kitaplığı başlatma için bir öğe başvurusu gerektirebilir veya `focus` ya `play`da gibi bir öğe üzerinde komut benzeri API 'ler çağırmanız gerekebilir.

Aşağıdaki yaklaşımı kullanarak bir bileşen içindeki HTML öğelerine başvuruları yakalayın:

* HTML öğesine `@ref` bir öznitelik ekleyin.
* Adı `@ref` özniteliğin değeriyle eşleşen bir `ElementReference` tür alanı tanımlayın.

Aşağıdaki örnek, `username` `<input>` öğesine bir başvuru yakalama göstermektedir:

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> Blazor başvurulan öğelerle etkileşime geçtiğinde DOM 'ı doldurma veya işleme gibi yakalanan öğe **başvurularını kullanmayın.** Bunun yapılması, bildirim temelli işleme modeliyle karışabilir.

.NET kodu açısından düşünüldüğünde, donuk bir `ElementReference` tanıtıcıdır. İle`ElementReference` yapabileceğiniz *tek* şey, JavaScript birlikte çalışması aracılığıyla JavaScript koduna geçer. Bunu yaptığınızda, JavaScript tarafı kodu normal Dom API 'leri ile kullanılabilecek `HTMLElement` bir örnek alır.

Örneğin, aşağıdaki kod bir öğe üzerinde odağı ayarlamaya izin veren bir .NET genişletme yöntemi tanımlar:

*Examplejsınterop. js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Öğesini `IJSRuntime.InvokeAsync<T>` kullanarak bir `exampleJsFunctions.focusElement` öğesi odaklamak `ElementReference` için kullanın ve ile çağırın:

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

Bir öğeyi odaklamak için bir genişletme yöntemi kullanmak için, `IJSRuntime` örneği alan bir statik genişletme yöntemi oluşturun:

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Yöntemi doğrudan nesnesi üzerinde çağrılır. Aşağıdaki örnek, statik `Focus` metodun `JsInteropClasses` ad alanından kullanılabildiğini varsayar:

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> `username` Değişken yalnızca bileşen işlendikten sonra doldurulur. Doldurulmamış bir `ElementReference` JavaScript koduna geçirilirse JavaScript kodu bir `null`değeri alır. Bileşen işlemeyi tamamladıktan sonra öğe başvurularını değiştirmek için (bir öğe üzerinde ilk odağı ayarlamak için) `OnAfterRenderAsync` veya [](xref:blazor/components#lifecycle-methods) `OnAfterRender` bileşen yaşam döngüsü yöntemlerini kullanın.

## <a name="invoke-net-methods-from-javascript-functions"></a>JavaScript işlevlerinden .NET yöntemlerini çağır

### <a name="static-net-method-call"></a>Statik .NET yöntemi çağrısı

JavaScript 'ten statik bir .NET yöntemi çağırmak için `DotNet.invokeMethod` veya `DotNet.invokeMethodAsync` işlevlerini kullanın. Çağırmak istediğiniz statik metodun tanımlayıcısını, işlevi içeren derlemenin adını ve tüm bağımsız değişkenleri geçirin. Blazor sunucu senaryolarını desteklemek için zaman uyumsuz sürüm tercih edilir. JavaScript 'ten bir .NET yöntemi çağırmak için, .net yönteminin public, static ve `[JSInvokable]` özniteliğe sahip olması gerekir. Varsayılan olarak, yöntem tanımlayıcısı yöntem adıdır, ancak `JSInvokableAttribute` oluşturucuyu kullanarak farklı bir tanımlayıcı belirtebilirsiniz. Açık genel yöntemlerin çağrılması Şu anda desteklenmiyor.

Örnek uygulama, bir dizi C# `int`öğeleri döndürmek için bir yöntem içerir. `JSInvokable` Özniteliği yöntemine uygulanır.

*Pages/Jsınterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

İstemciye sunulan JavaScript, C# .net yöntemini çağırır.

*Wwwroot/Examplejsınterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

**Tetikleyici .net static yöntemi ReturnArrayAsync** düğmesi seçildiğinde, tarayıcının Web geliştirici araçlarında konsol çıkışını inceleyin.

Konsol çıktısı:

```console
Array(4) [ 1, 2, 3, 4 ]
```

Dördüncü dizi değeri tarafından`data.push(4);` `ReturnArrayAsync`döndürülen diziye () gönderilir.

### <a name="instance-method-call"></a>Örnek yöntem çağrısı

JavaScript 'ten de .NET örnek yöntemlerini çağırabilirsiniz. JavaScript 'ten bir .NET örnek yöntemi çağırmak için:

* .Net örneğini bir `DotNetObjectReference` örneğe sarmalayarak JavaScript 'e geçirin. .NET örneği, JavaScript 'e başvuruya göre geçirilir.
* `invokeMethod` Or`invokeMethodAsync` işlevlerini kullanarak örnekte .NET örnek yöntemlerini çağırın. .NET örneği, JavaScript 'ten başka .NET yöntemleri çağrılırken bir bağımsız değişken olarak da geçirilebilir.

> [!NOTE]
> Örnek uygulama, iletileri istemci tarafı konsoluna kaydeder. Örnek uygulama tarafından gösterilen aşağıdaki örnekler için tarayıcının geliştirici araçlarında tarayıcının konsol çıkışını inceleyin.

**Tetikleyici .NET örnek yöntemi hellohelper. sayHello** düğmesi seçildiğinde, `ExampleJsInterop.CallHelloHelperSayHello` çağrılır ve yöntemine bir ad `Blazor`geçirir.

*Pages/Jsınterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello`JavaScript işlevini `sayHello` yeni bir `HelloHelper`örneğiyle çağırır.

*JsInteropClasses/Examplejsınterop. cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*Wwwroot/Examplejsınterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Ad, `HelloHelper.Name` özelliğini ayarlayan oluşturucuya `HelloHelper`geçirilir. JavaScript işlevi `sayHello` yürütüldüğünde, `HelloHelper.SayHello` JavaScript işlevi tarafından konsola yazılan `Hello, {Name}!` iletiyi döndürür.

*JsInteropClasses/HelloHelper. cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Tarayıcının Web geliştirici araçlarında konsol çıkışı:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a>Birlikte çalışma kodunu bir sınıf kitaplığında paylaşma

JavaScript birlikte çalışma kodu bir NuGet paketindeki kodu paylaşmanıza olanak sağlayan bir sınıf kitaplığına dahil edilebilir.

Sınıf kitaplığı, yerleşik derlemede JavaScript kaynaklarını katıştırmayı işler. JavaScript dosyaları *Wwwroot* klasörüne yerleştirilir. Araç, kitaplık oluşturulduğunda kaynakları katıştırmaya önem kazanır.

Oluşturulan NuGet paketine, uygulamanın proje dosyasında herhangi bir NuGet paketiyle aynı şekilde başvurulur. Paket geri yüklendikten sonra, uygulama kodu JavaScript 'e, gibi çağrı yapabilir C#.

Daha fazla bilgi için bkz. <xref:blazor/class-libraries>.

## <a name="harden-js-interop-calls"></a>Harden JS birlikte çalışma çağrıları

JS birlikte çalışması, ağ hataları nedeniyle başarısız olabilir ve güvenilmez olarak değerlendirilmelidir. Varsayılan olarak, Blazor sunucu uygulaması, bir dakika sonra sunucu üzerinde JS birlikte çalışabilirlik çağrılarını zaman aşımına uğrar. Bir uygulama, 10 saniye gibi daha agresif zaman aşımına uğrayedebilmesine, aşağıdaki yaklaşımlardan birini kullanarak zaman aşımını ayarlayın:

* İçinde `Startup.ConfigureServices`genel olarak, zaman aşımını belirtin:

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* Bileşen kodunda çağrı başına, tek bir çağrı zaman aşımını belirtebilir:

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

Kaynak tükenmesi hakkında daha fazla bilgi için bkz <xref:security/blazor/server>.
