---
title: Blazor JavaScript birlikte çalışma
author: guardrex
description: .NET ve JavaScript işlevleri çağırmak nasıl öğrenin Blazor uygulamalarındaki JavaScript yöntemleri.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/13/2019
uid: blazor/javascript-interop
ms.openlocfilehash: 2cd0ae66c8d0ee26badbf640a00267acc774feb8
ms.sourcegitcommit: e67356f5e643a5d43f6d567c5c998ce6002bdeb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66004913"
---
# <a name="blazor-javascript-interop"></a>Blazor JavaScript birlikte çalışma

Tarafından [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), ve [Luke Latham](https://github.com/guardrex)

.NET ve JavaScript işlevleri Blazor uygulama çağırabilirsiniz JavaScript kodundan yöntemleri.

## <a name="invoke-javascript-functions-from-net-methods"></a>.NET yöntemlerinden JavaScript işlevleri çağırma

.NET kodu bir JavaScript işlevi çağırmak için gerekli olduğunda zamanlar vardır. Örneğin, JavaScript çağrısı, tarayıcının yeteneklerini veya uygulamaya bir JavaScript kitaplığı işlevi kullanıma sunabilirsiniz.

.NET içinde JavaScript çağırmak için kullanın `IJSRuntime` Özet. `InvokeAsync<T>` Yöntemi, JSON seri hale getirilebilir bağımsız değişkenler herhangi bir sayıda birlikte çağrılacak istediğiniz JavaScript işlevi için bir tanımlayıcı alır. Genel kapsam göre işlevi tanımlayıcıdır (`window`). Çağrılacak istiyorsanız `window.someScope.someFunction`, tanımlayıcı `someScope.someFunction`. İşlevin çağrıldığı önce kaydetmeniz gerek yoktur. Dönüş türü `T` ayrıca JSON seri hale getirilebilir olması gerekir.

Sunucu tarafı uygulamalar için:

* Birden çok kullanıcı isteklerini, sunucu tarafı uygulama tarafından işlenir. Remove() çağırmayın `JSRuntime.Current` JavaScript işlevleri çağırmak için bir bileşende.
* Ekleme `IJSRuntime` soyutlama ve eklenen nesnenin JavaScript birlikte çalışma çağrıları kullanın.
* Blazor uygulama prerendering karşın, bir tarayıcı bağlantı kuran taşınmadığından olası JavaScript çağırma değildir. Daha fazla bilgi için [algılamak Blazor uygulama prerendering olduğunda](#detect-when-a-blazor-app-is-prerendering) bölümü.

Aşağıdaki örnek dayanır [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), Deneysel bir JavaScript tabanlı kod çözücü. Örnek bir JavaScript işlevini çağırmak nasıl gösterir bir C# yöntemi. JavaScript işlevi, bir bayt dizisinden kabul eden bir C# yöntemi, dizinin kodunu çözer ve bileşenine görüntülenmesi için metni döndürür.

İçinde `<head>` öğesinin *wwwroot/index.html* (Blazor istemci-tarafı) veya *sayfaları /\_Host.cshtml* (Blazor sunucu-tarafı), kullanan bir işlev sağlayan `TextDecoder` için geçirilen dizi kod çözme:

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

Kod önceki örnekte gösterildiği gibi JavaScript kodunu da bir JavaScript dosyasından yüklenemiyor (*.js*) betik dosyasına bir başvuru ile:

```html
<script src="exampleJsInterop.js"></script>
```

Aşağıdaki bileşen:

* Çağırır `ConvertArray` JavaScript işlevini kullanarak `JsRuntime` bir bileşen düğme (**dönüştürme dizi**) seçilir.
* JavaScript işlev çağrıldıktan sonra geçirilen dizinin bir dizeye dönüştürülür. Dize, görüntü bileşenine döndürülür.

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

Kullanılacak `IJSRuntime` soyutlama, aşağıdaki yaklaşımlardan birini benimseme:

* Ekleme `IJSRuntime` soyutlama Razor bileşen içine (*.razor*):

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* Ekleme `IJSRuntime` Özet bir sınıf içinde (*.cs*):

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* Dinamik içerik oluşturma ile [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), kullanın `[Inject]` özniteliği:

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

Bu konuda eşlik eden istemci-tarafı örnek uygulamada, kullanıcı girişini alır ve bir karşılama iletisi görüntüleyecek şekilde DOM ile etkileşim kuran istemci-tarafı uygulaması için iki JavaScript işlevleri kullanılabilir:

* `showPrompt` &ndash; Kullanıcı girişi (kullanıcı adı) kabul etmek için bir istem oluşturur ve ad çağırana döner.
* `displayWelcome` &ndash; Bir karşılama iletisi ile bir DOM nesnesi çağrıyı yapandan atar bir `id` , `welcome`.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Bir yerde `<script>` JavaScript dosyasında başvuruda etiketi *wwwroot/index.html* dosyası (Blazor istemci-tarafı) veya *sayfaları /\_Host.cshtml* dosyası (Blazor sunucu-tarafı).

*wwwroot/index.HTML* (Blazor istemci-tarafı):

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

*Sayfa /\_Host.cshtml* (Blazor sunucu-tarafı):

[!code-cshtml[](javascript-interop/samples_snapshot/_Host.cshtml?highlight=29)]

Yerleştirmeyin bir `<script>` çünkü bir bileşen dosyasında etiketi `<script>` etiketi dinamik olarak güncelleştirilemiyor.

JavaScript ile .NET yöntemleri birlikte çalışır *exampleJsInterop.js* çağırarak dosya `IJSRuntime.InvokeAsync<T>`.

`IJSRuntime` Soyutlamadır için sunucu tarafı senaryoları için zaman uyumsuz. İstemci tarafı uygulama çalışır ve eşzamanlı olarak, bir JavaScript işlevi çağırmak istiyorsanız alta `IJSInProcessRuntime` ve çağrı `Invoke<T>` yerine. Çoğu JavaScript birlikte çalışma kitaplıkları tüm senaryolarda, istemci tarafı ve sunucu tarafı kitaplıklar emin olmak için zaman uyumsuz API'leri kullanmanızı öneririz.

Örnek uygulamayı JavaScript birlikte çalışma göstermek için bir bileşeni içerir. Bileşeni:

* JavaScript komut istemi aracılığıyla kullanıcı girişini alır.
* İşleme bileşenine metni döndürür.
* Bir karşılama iletisi görüntüleyecek şekilde DOM ile etkileşime giren ikinci bir JavaScript işlevi çağırır.

*Pages/JSInterop.razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. Zaman `TriggerJsPrompt` bileşenin seçerek yürütülür **tetikleyici JavaScript istemi** button, JavaScript `showPrompt` sağlanan işlev *wwwroot/exampleJsInterop.js* dosyasıdır çağrılır.
1. `showPrompt` İşlevi olan HTML olarak kodlanan ve döndürülen bileşenine (kullanıcı adı), kullanıcı girişi kabul eder. Bileşen kullanıcı adının bir yerel değişkende depolar `name`.
1. Dize depolanan `name` bir JavaScript işleve geçirilir, bir karşılama iletisi dahil `displayWelcome`, bir başlık etiketine Hoş Geldiniz iletisi oluşturur.

## <a name="detect-when-a-blazor-app-is-prerendering"></a>Blazor uygulama prerendering Algıla
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>Öğelere başvurular yakalama

Bazı [JavaScript birlikte çalışma](xref:blazor/javascript-interop) senaryoları HTML öğeleri için başvuru gerektirir. Örneğin, bir kullanıcı Arabirimi kitaplığı başlatma için öğe başvurusu gerektirebilir veya bir öğe üzerinde komut benzeri API'leri çağırmak ihtiyacınız olabilecek `focus` veya `play`.

HTML öğeleri aşağıdaki yaklaşımı kullanarak bir bileşende başvuruları yakalayabilirsiniz:

* Ekleme bir `ref` özniteliği için HTML öğesi.
* Türünde bir alan tanımlayın `ElementRef` adıyla eşleşen `ref` özniteliği.

Aşağıdaki örnek, bir başvuru yakalama gösterir `username` `<input>` öğesi:

```cshtml
<input ref="username" ...>

@functions {
    ElementRef username;
}
```

> [!NOTE]
> Yapmak **değil** yakalanan öğesi başvuruları doldurma veya Blazor başvurulan öğeler ile etkileşim kurduğunda DOM düzenleme bir yolu olarak kullanın. Bunun yapılması bildirim temelli işleme modeliyle etkileyebilir.

.NET kodu ilgili olduğu kadar bir `ElementRef` donuk bir işleyicisidir. *Yalnızca* ile yapabileceğiniz bir şey `ElementRef` olan JavaScript kodu aracılığıyla JavaScript birlikte çalışma geçirin. Bunu yaptığınızda, JavaScript tarafı kodunu alır bir `HTMLElement` normal DOM API'leri ile kullanabileceğiniz örnek.

Örneğin, aşağıdaki kod, bir öğede odak ayarlama sağlayan bir .NET genişletme yöntemi tanımlar:

*exampleJsInterop.js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Kullanım `IJSRuntime.InvokeAsync<T>` ve çağrı `exampleJsFunctions.focusElement` ile bir `ElementRef` öğenin odaklanmak için:

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,7,11-12)]

Bir öğe odaklanmak için bir genişletme yöntemi kullanmak için alan bir statik genişletme yöntemi oluşturma `IJSRuntime` örneği:

```csharp
public static Task Focus(this ElementRef elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Yöntem doğrudan nesne üzerinde çağrılır. Aşağıdaki örnek olduğunu varsayar statik `Focus` yöntemi kullanılabilir `JsInteropClasses` ad alanı:

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,8,12)]

> [!IMPORTANT]
> `username` Değişkeni bileşeni işler ve çıktısını içeren sonra yalnızca doldurulmuş `>` öğesi. Bir doldurulmamış geçirmeye çalışırsanız `ElementRef` JavaScript kodu için JavaScript kodunu alır `null`. Bileşen kullanın (bir öğede ilk odağı ayarlamak için) işleme tamamlandıktan sonra öğesi başvuruları işlemek için `OnAfterRenderAsync` veya `OnAfterRender` [bileşen yaşam döngüsü yöntemleri](xref:blazor/components#lifecycle-methods).

## <a name="invoke-net-methods-from-javascript-functions"></a>JavaScript işlevleri .NET yöntemleri çağırma

### <a name="static-net-method-call"></a>Statik .NET yöntem çağrısı

JavaScript .NET statik bir yöntemi çağırmak için `DotNet.invokeMethod` veya `DotNet.invokeMethodAsync` işlevleri. Statik yöntem çağırmak istediğiniz işlevin yanı sıra, herhangi bir bağımsız değişken içeren derlemenin adını tanımlayıcıda geçirin. Zaman uyumsuz sürümü, sunucu tarafı senaryoları desteklemek için tercih edilir. JavaScript'ten bir .NET yöntemini çağırmak için .NET yöntemi ortak olmalıdır, statik ve `[JSInvokable]` özniteliği. Varsayılan olarak, yöntem tanımlayıcısı yöntemi adıdır, ancak farklı bir kimlik kullanarak bir belirtebilirsiniz `JSInvokableAttribute` Oluşturucusu. Açık genel yöntemleri çağırma şu anda desteklenmemektedir.

Örnek uygulamayı içeren bir C# bir dizi döndürmek için yöntemin `int`s. `JSInvokable` Özniteliği yönteme uygulanır.

*Pages/JsInterop.razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

Çağıran istemciye sunulan JavaScript C# .NET yöntemi.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

Zaman **tetikleyici .NET statik yöntem ReturnArrayAsync** düğmesi seçildiğinde, tarayıcının web geliştirici araçları konsol çıkışını dikkatle inceleyin.

Konsol çıktısı şöyledir:

```console
Array(4) [ 1, 2, 3, 4 ]
```

Dördüncü dizi değeri diziye gönderilir (`data.push(4);`) tarafından döndürülen `ReturnArrayAsync`.

### <a name="instance-method-call"></a>Örnek yöntem çağrısı

JavaScript'ten .NET örnek yöntemler de çağırabilir. JavaScript bir .NET örneği yöntemi çağırmak için:

* .NET örnek içinde sarmalama için JavaScript geçirin. bir `DotNetObjectRef` örneği. .NET örnek JavaScript başvurusu tarafından geçirilir.
* .NET örnek yöntemlerini kullanma örneği `invokeMethod` veya `invokeMethodAsync` işlevleri. .NET örnek JavaScript diğer .NET yöntemleri çağrılırken bağımsız değişken olarak de geçirilebilir.

> [!NOTE]
> Örnek uygulama, istemci tarafı konsola iletileri günlüğe kaydeder. Örnek uygulama tarafından gösterilen aşağıdaki örneklerde, tarayıcının geliştirici araçları konsol çıkışında tarayıcının inceleyin.

Zaman **tetikleyici .NET örnek yöntemi HelloHelper.SayHello** düğmesi seçili `ExampleJsInterop.CallHelloHelperSayHello` olarak adlandırılır ve bir ad geçirmeden `Blazor`, yönteme.

*Pages/JsInterop.razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello` JavaScript işlevini çağıran `sayHello` ile yeni bir örneğini `HelloHelper`.

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Adı geçirilir `HelloHelper`ait ayarlar Oluşturucu `HelloHelper.Name` özelliği. Zaman JavaScript işlevinin `sayHello` yürütüldüğünde, `HelloHelper.SayHello` döndürür `Hello, {Name}!` ileti konsola JavaScript işlevi yazılır.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Konsol, web geliştirici araçları tarayıcının çıktısı:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a>Bir sınıf kitaplığı'nda birlikte çalışma kod paylaşın

JavaScript birlikte çalışma kodu, bir NuGet paketi kod paylaşmanıza olanak sağlayan bir sınıf kitaplığı'nda bulunabilir.

Sınıf kitaplığı ekleme JavaScript kaynakları derlemesi işler. JavaScript dosyaları yerleştirilir *wwwroot* klasör. Alet kullanımı dışındaki kaynaklar ekleme kitaplık oluşturulduğunda üstlenir.

Yalnızca normal bir NuGet paketi başvurulduğu yerleşik NuGet paketini uygulamanın proje dosyasında başvurulan. Uygulama geri yüklendikten sonra olarak bulunuyorlarmış uygulama kodu içinde JavaScript çağırabilirsiniz C#.

Daha fazla bilgi için bkz. <xref:blazor/class-libraries>.
