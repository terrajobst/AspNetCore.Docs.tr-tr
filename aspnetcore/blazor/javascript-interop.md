---
title: ASP.NET Core Blazor JavaScript birlikte çalışması
author: guardrex
description: Blazor Apps 'teki JavaScript 'ten .NET ve .NET yöntemlerinden JavaScript işlevlerini çağırmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/javascript-interop
ms.openlocfilehash: a8c3a0951761faab1c11507834aeef2507388d71
ms.sourcegitcommit: ce2bfb01f2cc7dd83f8a97da0689d232c71bcdc4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72531135"
---
# <a name="aspnet-core-blazor-javascript-interop"></a>ASP.NET Core Blazor JavaScript birlikte çalışması

Sağlayan [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)ve [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Bir Blazor uygulaması, JavaScript kodundan .NET ve .NET yöntemlerinden JavaScript işlevlerini çağırabilir.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>.NET metotlarından JavaScript işlevlerini çağırma

Bir JavaScript işlevini çağırmak için .NET kodunun gerekli olduğu durumlar vardır. Örneğin, JavaScript çağrısı, tarayıcı özelliklerini veya bir JavaScript kitaplığından uygulamaya yönelik işlevselliği sunabilir.

.NET 'ten JavaScript 'i çağırmak için `IJSRuntime` soyutlama kullanın. @No__t-0 yöntemi, herhangi bir sayıda JSON seri hale getirilebilir bağımsız değişkenle birlikte çağırmak istediğiniz JavaScript işlevi için bir tanımlayıcı alır. İşlev tanımlayıcısı genel kapsama göredir (`window`). @No__t-0 ' ı çağırmak isterseniz, tanımlayıcı `someScope.someFunction` ' dir. Çağrılmadan önce işlevi kaydetmeniz gerekmez. @No__t-0 dönüş türü de JSON serileştirilebilir olmalıdır.

Blazor Server uygulamaları için:

* Birden çok kullanıcı isteği, Blazor Server uygulaması tarafından işlenir. JavaScript işlevlerini çağırmak için bir bileşende `JSRuntime.Current` çağrısı yapmayın.
* @No__t-0 soyutlamasını ekler ve JavaScript birlikte çalışma çağrılarını vermek için eklenen nesneyi kullanın.
* Blazor uygulaması prerendering olduğunda, tarayıcıyla bir bağlantı kurulmadığı için JavaScript 'e çağrı yapılamaz. Daha fazla bilgi için bkz. [bir Blazor uygulaması ne zaman prerendering](#detect-when-a-blazor-app-is-prerendering) bölümüne bakın.

Aşağıdaki örnek, deneysel bir JavaScript tabanlı kod çözücüsü olan [Textdecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)tabanlıdır. Örnek, bir C# yöntemden JavaScript işlevinin nasıl çağrılacağını gösterir. JavaScript işlevi bir C# yöntemden bir bayt dizisi kabul eder, dizinin kodunu çözer ve görüntülenecek metni bileşene döndürür.

*Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_host. cshtml* (Blazor Server) `<head>` öğesinin içinde, geçirilen bir dizinin kodunu çözmek için `TextDecoder` kullanan bir işlev sağlayın:

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

Önceki örnekte gösterilen kod gibi JavaScript kodu, betik dosyasına yönelik bir başvuruya sahip bir JavaScript dosyasından ( *. js*) de yüklenebilir:

```html
<script src="exampleJsInterop.js"></script>
```

Aşağıdaki bileşen:

* Bir bileşen düğmesi (**diziyi Dönüştür**) seçildiğinde `JsRuntime` kullanarak `ConvertArray` JavaScript işlevini çağırır.
* JavaScript işlevi çağrıldıktan sonra, geçirilen dizi bir dizeye dönüştürülür. Dize, görüntüleme için bileşene döndürülür.

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

@No__t-0 soyutlamasını kullanmak için aşağıdaki yaklaşımlardan birini benimseyin:

* @No__t-0 soyutlama Razor bileşenine ( *. Razor*) ekleme:

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* @No__t-0 soyutlamasını bir sınıfa ( *. cs*) Ekle:

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* [Buildrendertree](xref:blazor/components#manual-rendertreebuilder-logic)ile dinamik içerik oluşturma için `[Inject]` özniteliğini kullanın:

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

Bu konuya eşlik eden istemci tarafı örnek uygulamada, Kullanıcı girişi almak ve bir hoş geldiniz iletisi göstermek üzere DOM ile etkileşime geçen uygulama için iki JavaScript işlevi mevcuttur:

* `showPrompt` &ndash; Kullanıcı girişini kabul etmek için bir istem üretir (kullanıcının adı) ve çağıranın adını döndürür.
* `displayWelcome` &ndash;, çağırandan bir DOM nesnesine `id` `welcome` ile bir hoş geldiniz iletisi atar.

*Wwwroot/Examplejsınterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

JavaScript dosyasına başvuran `<script>` etiketini *Wwwroot/index.html* dosyasında (Blazor WebAssembly) veya *Pages/_host. cshtml* dosyasında (Blazor Server) yerleştirin.

*Wwwroot/index.html* (Blazor WebAssembly):

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

*Pages/_Host. cshtml* (Blazor Server):

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

@No__t-0 etiketini bir bileşen dosyasına yerleştirmeyin çünkü `<script>` etiketi dinamik olarak güncelleştirilemeyebilir.

.NET yöntemleri, `IJSRuntime.InvokeAsync<T>` çağırarak *Examplejsınterop. js* dosyasında JavaScript işlevleriyle birlikte çalışır.

@No__t-0 soyutlama, Blazor sunucu senaryolarına izin vermek için zaman uyumsuzdur. Uygulama bir Blazor WebAssembly uygulaması ise ve bir JavaScript işlevini zaman uyumlu olarak çağırmak istiyorsanız, `IJSInProcessRuntime` ' a alt türe çevirme yapın ve bunun yerine `Invoke<T>` ' i çağırın. Çoğu JavaScript birlikte çalışma kitaplıklarının, kitaplıkların tüm senaryolarda kullanılabilir olduğundan emin olmak için zaman uyumsuz API 'Leri kullanmasını öneririz.

Örnek uygulama, JavaScript birlikte çalışabildiğini gösteren bir bileşen içerir. Bileşen:

* Bir JavaScript istemi aracılığıyla Kullanıcı girişini alır.
* İşlemek için bileşene metni döndürür.
* Bir hoş geldiniz iletisini göstermek için DOM ile etkileşime sahip ikinci bir JavaScript işlevini çağırır.

*Pages/Jsınterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. @No__t-0 ' ın bileşen **tetikleyicisi JavaScript istem** düğmesi seçilerek yürütülmesi durumunda, *Wwwroot/Examplejsınterop. js* dosyasında bulunan JavaScript `showPrompt` işlevi çağırılır.
1. @No__t-0 işlevi, HTML kodlu ve bileşene döndürülen kullanıcı girişini (kullanıcının adı) kabul eder. Bileşen kullanıcının adını yerel bir değişkende depolar, `name`.
1. @No__t-0 ' da depolanan dize, hoş geldiniz iletisini bir başlık etiketine işleyen bir JavaScript işlevine (`displayWelcome`) geçirilen bir hoş geldiniz iletisine dahil edilir.

## <a name="call-a-void-javascript-function"></a>Void JavaScript işlevini çağırın

[Void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) veya [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) döndüren JavaScript işlevleri `IJSRuntime.InvokeVoidAsync` ile çağırılır.

## <a name="detect-when-a-blazor-app-is-prerendering"></a>Bir Blazor uygulamasının ne zaman prerendering olduğunu Algıla
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>Öğelere başvuruları yakala

Bazı [JavaScript birlikte çalışma](xref:blazor/javascript-interop) senaryoları HTML öğelerine başvuru gerektirir. Örneğin, bir kullanıcı arabirimi kitaplığı başlatma için bir öğe başvurusu gerektirebilir veya `focus` veya `play` gibi bir öğe üzerinde komut benzeri API 'Ler çağırmanız gerekebilir.

Aşağıdaki yaklaşımı kullanarak bir bileşen içindeki HTML öğelerine başvuruları yakalayın:

* HTML öğesine `@ref` özniteliği ekleyin.
* Adı `@ref` özniteliğinin değeriyle eşleşen `ElementReference` türünde bir alan tanımlayın.

Aşağıdaki örnek `username` `<input>` öğesine bir başvuru yakalama göstermektedir:

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> Yakalanan öğe başvurularını DOM doldurma yöntemi **olarak kullanmayın.** Bunun yapılması, bildirim temelli işleme modeliyle karışabilir.

.NET kodu düşünüldüğünde, `ElementReference` donuk bir tanıtıcıdır. @No__t-1 ile yapabileceğiniz *tek* şey, JavaScript birlikte çalışabilirliği aracılığıyla JavaScript koduna geçiş yapar. Bunu yaptığınızda, JavaScript tarafı kodu, normal DOM API 'Leri ile kullanılabilecek bir `HTMLElement` örneği alır.

Örneğin, aşağıdaki kod bir öğe üzerinde odağı ayarlamaya izin veren bir .NET genişletme yöntemi tanımlar:

*Examplejsınterop. js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Bir öğeye odaklanmak için `IJSRuntime.InvokeAsync<T>` kullanın ve `ElementReference` ile `exampleJsFunctions.focusElement` çağırın:

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

Bir öğeyi odaklamak üzere bir genişletme yöntemi kullanmak için, `IJSRuntime` örneğini alan bir statik genişletme yöntemi oluşturun:

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Yöntemi doğrudan nesnesi üzerinde çağrılır. Aşağıdaki örnek, statik `Focus` yönteminin `JsInteropClasses` ad alanından kullanılabildiğini varsayar:

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> @No__t-0 değişkeni yalnızca bileşen işlendikten sonra doldurulur. Bir undoldurulmamış `ElementReference` JavaScript koduna geçirilirse, JavaScript kodu bir `null` değeri alır. Bileşen işlemeyi tamamladıktan sonra öğe başvurularını işlemek için (bir öğe üzerinde ilk odağı ayarlamak için) `OnAfterRenderAsync` veya `OnAfterRender` [bileşen yaşam döngüsü yöntemlerini](xref:blazor/components#lifecycle-methods)kullanın.

## <a name="invoke-net-methods-from-javascript-functions"></a>JavaScript işlevlerinden .NET yöntemlerini çağır

### <a name="static-net-method-call"></a>Statik .NET yöntemi çağrısı

JavaScript 'ten statik bir .NET yöntemi çağırmak için `DotNet.invokeMethod` veya `DotNet.invokeMethodAsync` işlevlerini kullanın. Çağırmak istediğiniz statik metodun tanımlayıcısını, işlevi içeren derlemenin adını ve tüm bağımsız değişkenleri geçirin. Blazor sunucu senaryolarını desteklemek için zaman uyumsuz sürüm tercih edilir. JavaScript 'ten bir .NET yöntemi çağırmak için, .NET yönteminin public, static ve `[JSInvokable]` özniteliğine sahip olması gerekir. Varsayılan olarak, yöntem tanımlayıcısı yöntem adıdır, ancak `JSInvokableAttribute` oluşturucusunu kullanarak farklı bir tanımlayıcı belirtebilirsiniz. Açık genel yöntemlerin çağrılması Şu anda desteklenmiyor.

Örnek uygulama, @no__t- C# 1s dizisini döndürmek için bir yöntem içerir. @No__t-0 özniteliği yöntemine uygulanır.

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

Dördüncü dizi değeri `ReturnArrayAsync` tarafından döndürülen diziye (`data.push(4);`) gönderilir.

### <a name="instance-method-call"></a>Örnek yöntem çağrısı

JavaScript 'ten de .NET örnek yöntemlerini çağırabilirsiniz. JavaScript 'ten bir .NET örnek yöntemi çağırmak için:

* .NET örneğini `DotNetObjectReference` örneğine sarmalayarak JavaScript 'e geçirin. .NET örneği, JavaScript 'e başvuruya göre geçirilir.
* @No__t-0 veya `invokeMethodAsync` işlevlerini kullanarak örnekte .NET örnek yöntemlerini çağırın. .NET örneği, JavaScript 'ten başka .NET yöntemleri çağrılırken bir bağımsız değişken olarak da geçirilebilir.

> [!NOTE]
> Örnek uygulama, iletileri istemci tarafı konsoluna kaydeder. Örnek uygulama tarafından gösterilen aşağıdaki örnekler için tarayıcının geliştirici araçlarında tarayıcının konsol çıkışını inceleyin.

**Tetikleyici .NET örnek yöntemi HelloHelper. SayHello** düğmesi seçildiğinde `ExampleJsInterop.CallHelloHelperSayHello` çağrılır ve bir ad, `Blazor` ' yi yöntemine geçirir.

*Pages/Jsınterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello` JavaScript işlevini, yeni bir `HelloHelper` örneğiyle birlikte @no__t çağırır.

*JsInteropClasses/Examplejsınterop. cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*Wwwroot/Examplejsınterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Ad, `HelloHelper.Name` özelliğini ayarlayan `HelloHelper` ' a geçirilir. @No__t-0 JavaScript işlevi yürütüldüğünde, `HelloHelper.SayHello`, JavaScript işlevi tarafından konsola yazılan `Hello, {Name}!` iletisini döndürür.

*JsInteropClasses/HelloHelper. cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

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

* @No__t-0 ' da genel olarak, zaman aşımını belirtin:

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
