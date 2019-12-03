---
title: ASP.NET Core Blazor JavaScript birlikte çalışabilirliği
author: guardrex
description: Blazor uygulamalarında JavaScript 'ten .NET ve .NET yöntemlerinden JavaScript işlevlerini çağırmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/02/2019
no-loc:
- Blazor
uid: blazor/javascript-interop
ms.openlocfilehash: 108fdac8667f407adba3470de4eb8e35883cefbf
ms.sourcegitcommit: 169ea5116de729c803685725d96450a270bc55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74733836"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a>ASP.NET Core Blazor JavaScript birlikte çalışabilirliği

Sağlayan [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)ve [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor bir uygulama, JavaScript kodundan .NET ve .NET yöntemlerinden JavaScript işlevlerini çağırabilir.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>.NET metotlarından JavaScript işlevlerini çağırma

Bir JavaScript işlevini çağırmak için .NET kodunun gerekli olduğu durumlar vardır. Örneğin, JavaScript çağrısı, tarayıcı özelliklerini veya bir JavaScript kitaplığından uygulamaya yönelik işlevselliği sunabilir. Bu senaryoya *JavaScript birlikte çalışabilirliği* (*js birlikte çalışma*) denir.

.NET 'ten JavaScript 'i çağırmak için `IJSRuntime` soyutlamasını kullanın. `InvokeAsync<T>` yöntemi, herhangi bir sayıda JSON seri hale getirilebilir bağımsız değişkenle birlikte çağırmak istediğiniz JavaScript işlevi için bir tanımlayıcı alır. İşlev tanımlayıcısı, genel kapsama (`window`) göredir. `window.someScope.someFunction`çağırmak isterseniz, tanımlayıcı `someScope.someFunction`. Çağrılmadan önce işlevi kaydetmeniz gerekmez. Dönüş türü `T` ayrıca seri hale getirilebilir JSON olmalıdır. `T`, döndürülen JSON türüyle en iyi eşleşen .NET türüyle eşleşmelidir.

Blazor Server uygulamaları için:

* Blazor sunucusu uygulaması tarafından birden çok kullanıcı isteği işlenir. JavaScript işlevlerini çağırmak için bir bileşende `JSRuntime.Current` çağırmayın.
* `IJSRuntime` soyutlamasını ekler ve bu nesneyi kullanarak JS birlikte çalışma çağrıları verme.
* Blazor bir uygulama prerendering olduğunda, tarayıcıyla bir bağlantı kurulmadığından JavaScript 'e çağrı yapılamaz. Daha fazla bilgi için, [Blazor bir uygulamanın ne zaman prerendering olduğunu Algıla](#detect-when-a-blazor-app-is-prerendering) bölümüne bakın.

Aşağıdaki örnek, deneysel bir JavaScript tabanlı kod çözücüsü olan [Textdecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)tabanlıdır. Örnek, bir C# yöntemden JavaScript işlevinin nasıl çağrılacağını gösterir. JavaScript işlevi bir C# yöntemden bir bayt dizisi kabul eder, dizinin kodunu çözer ve görüntülenecek metni bileşene döndürür.

*Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_Host. cshtml* (Blazor Server) `<head>` öğesi içinde, geçirilen bir dizinin kodunu çözmek Için `TextDecoder` kullanan bir JavaScript işlevi sağlayın ve kodu çözülen değeri döndürün:

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

Önceki örnekte gösterilen kod gibi JavaScript kodu, betik dosyasına yönelik bir başvuruya sahip bir JavaScript dosyasından ( *. js*) de yüklenebilir:

```html
<script src="exampleJsInterop.js"></script>
```

Aşağıdaki bileşen:

* Bir bileşen düğmesi (**diziyi Dönüştür**) seçildiğinde `JSRuntime` kullanarak `convertArray` JavaScript işlevini çağırır.
* JavaScript işlevi çağrıldıktan sonra, geçirilen dizi bir dizeye dönüştürülür. Dize, görüntüleme için bileşene döndürülür.

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a>IJSRuntime kullanımı

`IJSRuntime` soyutlamasını kullanmak için aşağıdaki yaklaşımlardan birini benimseyin:

* `IJSRuntime` soyutlama Razor bileşenine ( *. Razor*) ekleme:

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  *Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_Host. cshtml* (Blazor Server) `<head>` öğesinin Içinde, bir `handleTickerChanged` JavaScript işlevi sağlayın. İşlevi `IJSRuntime.InvokeVoidAsync` ile çağrılır ve bir değer döndürmez:

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* `IJSRuntime` soyutlamasını bir sınıfa ( *. cs*) Ekle:

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  *Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_Host. cshtml* (Blazor Server) `<head>` öğesinin Içinde, bir `handleTickerChanged` JavaScript işlevi sağlayın. İşlevi `JSRuntime.InvokeAsync` ile çağrılır ve bir değer döndürür:

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* [Buildrendertree](xref:blazor/components#manual-rendertreebuilder-logic)ile dinamik içerik oluşturma için `[Inject]` özniteliğini kullanın:

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

Bu konuya eşlik eden istemci tarafı örnek uygulamada, Kullanıcı girişi almak ve bir hoş geldiniz iletisi göstermek üzere DOM ile etkileşime geçen uygulama için iki JavaScript işlevi mevcuttur:

* `showPrompt` &ndash; Kullanıcı girişini kabul etmek için bir istem üretir (kullanıcının adı) ve çağıranın adını döndürür.
* `displayWelcome` &ndash;, çağıran bir `id` `welcome`bir DOM nesnesine bir hoş geldiniz iletisi atar.

*Wwwroot/Examplejsınterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

JavaScript dosyasına başvuran `<script>` etiketini *Wwwroot/index.html* dosyasında (Blazor WebAssembly) veya *Pages/_Host. cshtml* dosyasında (Blazor Server) yerleştirin.

*Wwwroot/index.html* (Blazor WebAssembly):

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

*Pages/_Host. cshtml* (Blazor sunucusu):

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

`<script>` etiketi dinamik olarak güncelleştirilemediğinden bir `<script>` etiketini bileşen dosyasına yerleştirmeyin.

.NET yöntemleri, `IJSRuntime.InvokeAsync<T>`çağırarak *Examplejsınterop. js* dosyasında JavaScript işlevleriyle birlikte çalışır.

`IJSRuntime` soyutlama, Blazor sunucu senaryolarına izin vermek için zaman uyumsuzdur. Uygulama bir Blazor Weelsembly uygulaması ise ve bir JavaScript işlevini zaman uyumlu olarak çağırmak istiyorsanız, `IJSInProcessRuntime` ve bunun yerine `Invoke<T>` çağırın. Çoğu JS birlikte çalışma kitaplıklarının, kitaplıkların tüm senaryolarda kullanılabilir olmasını sağlamak için zaman uyumsuz API 'Leri kullanmasını öneririz.

Örnek uygulama, JS birlikte çalışabilirliği göstermek için bir bileşeni içerir. Bileşen:

* Bir JavaScript istemi aracılığıyla Kullanıcı girişini alır.
* İşlemek için bileşene metni döndürür.
* Bir hoş geldiniz iletisini göstermek için DOM ile etkileşime sahip ikinci bir JavaScript işlevini çağırır.

*Pages/Jsınterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. `TriggerJsPrompt`, bileşenin **tetikleyicisi JavaScript istem** düğmesi seçilerek yürütüldüğünde, *Wwwroot/Examplejsınterop. js* dosyasında verilen JavaScript `showPrompt` işlevi çağırılır.
1. `showPrompt` işlevi, HTML kodlu ve bileşene döndürülen kullanıcı girişini (kullanıcının adı) kabul eder. Bileşen, kullanıcının adını `name`yerel bir değişkende depolar.
1. `name` depolanan dize, hoş geldiniz iletisini bir başlık etiketine işleyen `displayWelcome`bir JavaScript işlevine iletilen bir hoş geldiniz iletisine dahil edilir.

## <a name="call-a-void-javascript-function"></a>Void JavaScript işlevini çağırın

[Void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) veya [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) döndüren JavaScript işlevleri `IJSRuntime.InvokeVoidAsync`ile çağırılır.

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a>Blazor bir uygulamanın ne zaman prerendering olduğunu Algıla
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>Öğelere başvuruları yakala

Bazı JS birlikte çalışma senaryoları HTML öğelerine başvurular gerektirir. Örneğin, bir kullanıcı arabirimi kitaplığı başlatma için bir öğe başvurusu gerektirebilir veya `focus` veya `play`gibi bir öğe üzerinde komut benzeri API 'Ler çağırmanız gerekebilir.

Aşağıdaki yaklaşımı kullanarak bir bileşen içindeki HTML öğelerine başvuruları yakalayın:

* HTML öğesine bir `@ref` özniteliği ekleyin.
* Adı `@ref` özniteliği değeri ile eşleşen `ElementReference` türünde bir alan tanımlayın.

Aşağıdaki örnek, `username` `<input>` öğesine bir başvuru yakalama göstermektedir:

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> Yalnızca Blazoretkileşimde bulunmayan boş bir öğenin içeriğini bulunmamalıdır için bir öğe başvurusu kullanın. Bu senaryo, bir 3. taraf API 'SI öğeye içerik sağladığı zaman yararlıdır. Blazor öğesiyle etkileşmediği için, Blazoröğesi ve DOM gösterimi arasında bir çakışma olabilir.
>
> Aşağıdaki örnekte, Blazor, bu öğenin liste öğelerini (`<li>`) doldurmak üzere DOM ile etkileşimde bulunduğundan, sıralanmamış listenin (`ul`) içeriğini () zaman *zaman aşmaktır* .
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
> JS birlikte çalışma öğesi, öğe `MyList` içeriğini değiştiriyorsa ve Blazor SLA 'ya uygulamaya çalışırsa, diffler DOM ile eşleşmez.

.NET kodu açısından düşünüldüğünde, `ElementReference` donuk bir tanıtıcıdır. `ElementReference` ile yapabileceğiniz *tek* şey, JS birlikte çalışma yoluyla JavaScript koduna geçiş yapar. Bunu yaptığınızda, JavaScript tarafı kodu normal DOM API 'Leri ile kullanılabilecek bir `HTMLElement` örneğini alır.

Örneğin, aşağıdaki kod bir öğe üzerinde odağı ayarlamaya izin veren bir .NET genişletme yöntemi tanımlar:

*Examplejsınterop. js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Değer döndürmeyen bir JavaScript işlevini çağırmak için `IJSRuntime.InvokeVoidAsync`kullanın. Aşağıdaki kod, yakalanan `ElementReference`önceki JavaScript işlevini çağırarak Kullanıcı adı girişi üzerinde odağı ayarlar:

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

Bir genişletme yöntemi kullanmak için `IJSRuntime` örneğini alan bir statik genişletme yöntemi oluşturun:

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

`Focus` yöntemi doğrudan nesne üzerinde çağrılır. Aşağıdaki örnek, `Focus` yönteminin `JsInteropClasses` ad alanından kullanılabildiğini varsayar:

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> `username` değişkeni yalnızca bileşen işlendikten sonra doldurulur. Doldurulmamış bir `ElementReference` JavaScript koduna geçirilirse, JavaScript kodu bir `null`değeri alır. Bileşen işlemeyi tamamladıktan sonra öğe başvurularını değiştirmek için (bir öğe üzerinde ilk odağı ayarlamak için) [Onafterrenderasync veya OnAfterRender bileşen yaşam döngüsü yöntemlerini](xref:blazor/lifecycle#after-component-render)kullanın.

Genel türlerle çalışırken ve bir değer döndürürken, [Valuetask\<t >](xref:System.Threading.Tasks.ValueTask`1)kullanın:

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

`GenericMethod` doğrudan nesne üzerinde bir tür ile çağırılır. Aşağıdaki örnek, `GenericMethod` `JsInteropClasses` ad alanından kullanılabilir olduğunu varsayar:

[!code-cshtml[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="invoke-net-methods-from-javascript-functions"></a>JavaScript işlevlerinden .NET yöntemlerini çağır

### <a name="static-net-method-call"></a>Statik .NET yöntemi çağrısı

JavaScript 'ten statik bir .NET yöntemi çağırmak için `DotNet.invokeMethod` veya `DotNet.invokeMethodAsync` işlevlerini kullanın. Çağırmak istediğiniz statik metodun tanımlayıcısını, işlevi içeren derlemenin adını ve tüm bağımsız değişkenleri geçirin. Blazor sunucu senaryolarını desteklemek için zaman uyumsuz sürüm tercih edilir. JavaScript 'ten bir .NET yöntemi çağırmak için, .NET yönteminin public, static ve `[JSInvokable]` özniteliğine sahip olması gerekir. Varsayılan olarak, yöntem tanımlayıcısı yöntem adıdır, ancak `JSInvokableAttribute` oluşturucusunu kullanarak farklı bir tanımlayıcı belirtebilirsiniz. Açık genel yöntemlerin çağrılması Şu anda desteklenmiyor.

Örnek uygulama, `int`s C# dizisini döndürmek için bir yöntem içerir. `JSInvokable` özniteliği yöntemine uygulanır.

*Pages/Jsınterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

İstemciye sunulan JavaScript, C# .net yöntemini çağırır.

*Wwwroot/Examplejsınterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

**Tetikleyici .net static yöntemi ReturnArrayAsync** düğmesi seçildiğinde, tarayıcının Web geliştirici araçlarında konsol çıkışını inceleyin.

Konsol çıktısı:

```console
Array(4) [ 1, 2, 3, 4 ]
```

Dördüncü dizi değeri, `ReturnArrayAsync`tarafından döndürülen diziye (`data.push(4);`) gönderilir.

### <a name="instance-method-call"></a>Örnek yöntem çağrısı

JavaScript 'ten de .NET örnek yöntemlerini çağırabilirsiniz. JavaScript 'ten bir .NET örnek yöntemi çağırmak için:

* .NET örneğini bir `DotNetObjectReference` örneğine sarmalayarak JavaScript 'e geçirin. .NET örneği, JavaScript 'e başvuruya göre geçirilir.
* `invokeMethod` veya `invokeMethodAsync` işlevlerini kullanarak örnekte .NET örnek yöntemlerini çağırın. .NET örneği, JavaScript 'ten başka .NET yöntemleri çağrılırken bir bağımsız değişken olarak da geçirilebilir.

> [!NOTE]
> Örnek uygulama, iletileri istemci tarafı konsoluna kaydeder. Örnek uygulama tarafından gösterilen aşağıdaki örnekler için tarayıcının geliştirici araçlarında tarayıcının konsol çıkışını inceleyin.

**Tetikleyici .NET örnek yöntemi HelloHelper. SayHello** düğmesi seçildiğinde, `ExampleJsInterop.CallHelloHelperSayHello` çağrılır ve `Blazor`bir adı yöntemine geçirir.

*Pages/Jsınterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello`, JavaScript işlevini `sayHello` yeni bir `HelloHelper`örneğiyle çağırır.

*JsInteropClasses/Examplejsınterop. cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*Wwwroot/Examplejsınterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Ad, `HelloHelper.Name` özelliğini ayarlayan `HelloHelper`oluşturucusuna geçirilir. JavaScript işlevi `sayHello` yürütüldüğünde, `HelloHelper.SayHello` JavaScript işlevi tarafından konsola yazılan `Hello, {Name}!` iletisi döndürür.

*JsInteropClasses/HelloHelper. cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Tarayıcının Web geliştirici araçlarında konsol çıkışı:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a>Birlikte çalışma kodunu bir sınıf kitaplığında paylaşma

JS birlikte çalışma kodu bir NuGet paketindeki kodu paylaşmanıza olanak sağlayan bir sınıf kitaplığına dahil edilebilir.

Sınıf kitaplığı, yerleşik derlemede JavaScript kaynaklarını katıştırmayı işler. JavaScript dosyaları *Wwwroot* klasörüne yerleştirilir. Araç, kitaplık oluşturulduğunda kaynakları katıştırmaya önem kazanır.

Oluşturulan NuGet paketine, uygulamanın proje dosyasında herhangi bir NuGet paketiyle aynı şekilde başvurulur. Paket geri yüklendikten sonra, uygulama kodu JavaScript 'e, gibi çağrı yapabilir C#.

Daha fazla bilgi için bkz. <xref:blazor/class-libraries>.

## <a name="harden-js-interop-calls"></a>Harden JS birlikte çalışma çağrıları

JS birlikte çalışması, ağ hataları nedeniyle başarısız olabilir ve güvenilmez olarak değerlendirilmelidir. Varsayılan olarak, bir Blazor sunucusu uygulaması, bir dakika sonra sunucu üzerinde JS birlikte çalışabilirlik çağrılarını zaman aşımına uğrar. Bir uygulama, 10 saniye gibi daha agresif zaman aşımına uğrayedebilmesine, aşağıdaki yaklaşımlardan birini kullanarak zaman aşımını ayarlayın:

* `Startup.ConfigureServices`genel olarak, zaman aşımını belirtin:

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* Bileşen kodunda çağrı başına, tek bir çağrı zaman aşımını belirtebilir:

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

Kaynak tükenmesi hakkında daha fazla bilgi için bkz. <xref:security/blazor/server>.

## <a name="additional-resources"></a>Ek kaynaklar

* [InteropComponent. Razor örneği (ASPNET/AspNetCore GitHub deposu, 3,0 yayın dalı)](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
