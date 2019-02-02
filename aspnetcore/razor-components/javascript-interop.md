---
title: Razor bileşenleri JavaScript birlikte çalışma
author: guardrex
description: .NET ve JavaScript işlevleri çağırmak nasıl öğrenin Blazor ve Razor bileşenleri uygulamalarındaki JavaScript yöntemleri.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/javascript-interop
ms.openlocfilehash: 99aa2b7844879f3fa9b577116115ba1bfcf9defe
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668191"
---
# <a name="razor-components-javascript-interop"></a>Razor bileşenleri JavaScript birlikte çalışma

Tarafından [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), ve [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Bir Razor bileşenleri uygulaması JavaScript işlevleri .NET ve .NET çağırabilirsiniz JavaScript kodundan yöntemleri.

## <a name="invoke-javascript-functions-from-net-methods"></a>.NET yöntemlerinden JavaScript işlevleri çağırma

.NET kodu bir JavaScript işlevi çağırmak için gerekli olduğunda zamanlar vardır. Örneğin, JavaScript çağrısı, tarayıcının yeteneklerini veya uygulamaya bir JavaScript kitaplığı işlevi kullanıma sunabilirsiniz.

.NET içinde JavaScript çağırmak için kullanın `IJSRuntime` Özet. `InvokeAsync<T>` Metodunda `IJSRuntime` istediğiniz bağımsız JSON seri hale getirilebilir herhangi bir sayıda birlikte çağrılacak JavaScript işlevi için bir tanımlayıcı alır. Genel kapsam göre işlevi tanımlayıcıdır (`window`). Çağrılacak istiyorsanız `window.someScope.someFunction`, tanımlayıcı `someScope.someFunction`. İşlevin çağrıldığı önce kaydetmeniz gerek yoktur. Dönüş türü `T` ayrıca JSON seri hale getirilebilir olması gerekir.

Sunucu tarafı ASP.NET Core Razor bileşenleri uygulamalar için:

* Birden çok kullanıcı isteklerini, sunucu tarafı uygulama tarafından işlenir. Remove() çağırmayın `JSRuntime.Current` JavaScript işlevleri çağırmak için bir bileşende.
* Ekleme `IJSRuntime` soyutlama ve eklenen nesnenin JavaScript birlikte çalışma çağrıları kullanın.

Aşağıdaki örnek dayanır [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), Deneysel bir JavaScript tabanlı kod çözücü. Örnek bir JavaScript işlevini çağırmak nasıl gösterir bir C# yöntemi. JavaScript işlevi, bir bayt dizisinden kabul eden bir C# yöntemi, dizinin kodunu çözer ve bileşenine görüntülenmesi için metni döndürür.

İçinde `<head>` öğesinin *wwwroot/index.html*, kullanan bir işlev sağlayan `TextDecoder` geçirilen dizisinin kodunu çözmek için:

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

Kod önceki örnekte gösterildiği gibi bir JavaScript kod da yüklenebilir bir komut dosyası başvurusu olan bir JavaScript dosyası tarafından *wwwroot/index.html* dosya.

Aşağıdaki bileşen:

* Çağırır `ConvertArray` JavaScript işlevini kullanarak `JsRuntime` bir bileşen düğme (**dönüştürme dizi**) seçilir.
* JavaScript işlev çağrıldıktan sonra geçirilen dizinin bir dizeye dönüştürülür. Dize, görüntü bileşenine döndürülür.

```cshtml
@page "/"
@using Microsoft.JSInterop;
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

İstemci tarafı Blazor uygulamalar için `IJSRuntime` soyutlama aracılığıyla erişilebilir `JSRuntime.Current`, geçerli kullanıcının isteğine ifade eder. Bir istemci-tarafı Blazor uygulamasının yalnızca tek bir kullanıcı olduğundan kullanarak `JSRuntime.Current` JavaScript çağrılacak işlev normal olarak çalışır. Yalnızca `JSRuntime.Current` istemci-tarafı Blazor uygulamalarında.

Bu konuda eşlik eden istemci-tarafı örnek uygulamada, kullanıcı girişini alır ve bir karşılama iletisi görüntüleyecek şekilde DOM ile etkileşim kuran istemci-tarafı uygulaması için iki JavaScript işlevleri kullanılabilir:

* `showPrompt` &ndash; Kullanıcı girişi (kullanıcı adı) kabul etmek için bir istem oluşturur ve ad çağırana döner.
* `displayWelcome` &ndash; Bir karşılama iletisi ile bir DOM nesnesi çağrıyı yapandan atar bir `id` , `welcome`.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Bir yerde `<script>` JavaScript dosyasında başvuruda etiketi *wwwroot/index.html* dosyası:

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=16)]

Komut dosyası etiketini dinamik olarak güncelleştirilemediğinden bir komut dosyası etiketini bir bileşen dosyasında yerleştirmeyin.

JavaScript işlevleri çağırarak birlikte .NET yöntemleri çalışma `InvokeAsync<T>` metodunda `IJSRuntime`.

Örnek uygulamayı kullanan bir çift C# yöntemleri `Prompt` ve `Display`, çağrılacak `showPrompt` ve `displayWelcome` JavaScript işlevleri:

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=6-8,14-16)]

`IJSRuntime` Soyutlamadır için sunucu tarafı senaryoları için zaman uyumsuz. İstemci tarafı uygulama çalışır ve eşzamanlı olarak, bir JavaScript işlevi çağırmak istiyorsanız alta `IJSInProcessRuntime` ve çağrı `Invoke<T>` yerine. Çoğu JavaScript birlikte çalışma kitaplığı API'leri, kitaplıklar emin olmak için zaman uyumsuz kullanan tüm senaryolarda, istemci tarafı ve sunucu tarafı öneririz.

Örnek uygulamayı JS Interop göstermek için bir bileşeni içerir. Bileşeni:

* JS istemi aracılığıyla kullanıcı girişini alır.
* İşleme bileşenine metni döndürür.
* Bir karşılama iletisi görüntüleyecek şekilde DOM ile etkileşime giren ikinci bir JS işlevi çağırır.

*Pages/JSInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=1&end=21&highlight=2-3,9-11,13,16-20)]

1. Zaman `TriggerJsPrompt` bileşenin seçerek yürütülür **tetikleyici JavaScript istemi** düğmesini `ExampleJsInterop.Prompt` yönteminde C# kod çağrılır.
1. `Prompt` Yöntemini yürütür JavaScript `showPrompt` sağlanan işlev *wwwroot/exampleJsInterop.js* dosya.
1. `showPrompt` İşlevi olan HTML olarak kodlanan ve iade (kullanıcı adı), kullanıcı girişi kabul eden `Prompt` yöntemi ve sonuçta bileşenine yedekleyin. Bileşen kullanıcı adının bir yerel değişkende depolar `name`.
1. Dize depolanan `name` ikinci bir geçirilen bir karşılama iletisi eklenmiştir C# yöntemi `ExampleJsInterop.Display`.
1. `Display` bir JavaScript işlevini çağıran `displayWelcome`, bir başlık etiketine Hoş Geldiniz iletisi oluşturur.

## <a name="capture-references-to-elements"></a>Öğelere başvurular yakalama

Bazı [JavaScript birlikte çalışma](xref:razor-components/javascript-interop) senaryoları HTML öğeleri için başvuru gerektirir. Örneğin, bir kullanıcı Arabirimi kitaplığı başlatma için öğe başvurusu gerektirebilir veya bir öğe üzerinde komut benzeri API'leri çağırmak ihtiyacınız olabilecek `focus` veya `play`.

HTML öğesi sayısında ciddi bir bileşen başvuruları ekleyerek yakalamak için bir `ref` öznitelik HTML öğesine ve ardından türünde bir alan tanımlama `ElementRef` adıyla eşleşen `ref` özniteliği.

Aşağıdaki örnek, bir kullanıcı adı giriş öğeye başvuru yakalama gösterir:

```csharp
<input ref="username" ... />

@functions {
    ElementRef username;
}
```

> [!NOTE]
> Yapmak **değil** yakalanan öğesi başvuruları yerli doldurma bir yol kullan Bunun yapılması bildirim temelli işleme modeliyle etkileyebilir.

.NET kodu ilgili olduğu kadar bir `ElementRef` donuk bir işleyicisidir. *Yalnızca* ile yapabilir şeydir JavaScript kodunu aracılığıyla JavaScript birlikte çalışma geçirin. Bunu yaptığınızda, JavaScript tarafı kodunu alır bir `HTMLElement` normal DOM API'leri ile kullanabileceğiniz örnek.

Örneğin, aşağıdaki kod, bir öğede odak ayarlama sağlayan bir .NET genişletme yöntemi tanımlar:

*mylib.js*:

```javascript
window.myLib = {
  focusElement : function (element) {
    element.focus();
  }
}
```

*ElementRefExtensions.cs*:

```csharp
using Microsoft.AspNetCore.Blazor;
using Microsoft.JSInterop;
using System.Threading.Tasks;

namespace MyLib
{
    public static class MyLibElementRefExtensions
    {
        public static Task Focus(this ElementRef elementRef)
        {
            return JSRuntime.Current.InvokeAsync<object>("myLib.focusElement", elementRef);
        }
    }
}
```

Artık girişleri bileşenlerinizi hiçbirinde odaklanabilirsiniz:

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
> `username` Değişkeni bileşeni işler ve çıktısını içeren sonra yalnızca doldurulmuş `<input>` öğesi. Bir doldurulmamış geçirmeye çalışırsanız `ElementRef` JavaScript kodu için JavaScript kodunu alır `null`. Bileşen kullanın (bir öğede ilk odağı ayarlamak için) işleme tamamlandıktan sonra öğesi başvuruları işlemek için `OnAfterRenderAsync` veya `OnAfterRender` [bileşen yaşam döngüsü yöntemleri](xref:razor-components/components#lifecycle-methods).

## <a name="invoke-net-methods-from-javascript-functions"></a>JavaScript işlevleri .NET yöntemleri çağırma

### <a name="static-net-method-call"></a>Statik .NET yöntem çağrısı

JavaScript .NET statik bir yöntemi çağırmak için `DotNet.invokeMethod` veya `DotNet.invokeMethodAsync` işlevleri. Statik yöntem çağırmak istediğiniz işlevin yanı sıra, herhangi bir bağımsız değişken içeren derlemenin adını tanımlayıcıda geçirin. Yine, zaman uyumsuz sürümü, sunucu tarafı senaryoları desteklemek için tercih edilir. JavaScript'ten çağırma olması için .NET yöntemi genel, statik ve ile donatılmış olmalıdır `[JSInvokable]`. Varsayılan olarak, yöntem tanımlayıcısı yöntemi adıdır, ancak farklı bir kimlik kullanarak bir belirtebilirsiniz `JSInvokableAttribute` Oluşturucusu. Açık genel yöntemleri çağırma şu anda desteklenmemektedir.

Örnek uygulamayı içeren bir C# bir dizi döndürmek için yöntemin `int`s. Yöntem ile donatılmış `JSInvokable` özniteliği.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=47&end=58&highlight=7-11)]

Çağıran istemciye sunulan JavaScript C# .NET yöntemi.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-12)]

Zaman **tetikleyici .NET statik yöntem ReturnArrayAsync** düğmesi seçildiğinde, tarayıcının, web geliştirici araçları konsol çıkışında inceleyin:

```console
Array(4) [ 1, 2, 3, 4 ]
```

Dördüncü dizi değeri diziye gönderilir (`data.push(4);`) tarafından döndürülen `ReturnArrayAsync`.

### <a name="instance-method-call"></a>Örnek yöntem çağrısı

JavaScript'ten .NET örnek yöntemler de çağırabilir. JavaScript bir .NET örneği yöntemi çağırmak için ilk .NET örnek JavaScript sarmalama tarafından başarılı bir `DotNetObjectRef` örneği. .NET örnek JavaScript başvurusu tarafından geçirilir ve örneği kullanarak .NET örnek yöntemler çağırabilirsiniz `invokeMethod` veya `invokeMethodAsync` işlevleri. .NET örnek JavaScript diğer .NET yöntemleri çağrılırken bağımsız değişken olarak de geçirilebilir.

> [!NOTE]
> Örnek uygulama, istemci tarafı konsola iletileri günlüğe kaydeder. Örnek uygulama tarafından gösterilen aşağıdaki örneklerde, tarayıcının geliştirici araçları konsol çıkışında tarayıcının inceleyin.

Zaman **tetikleyici .NET örnek yöntemi HelloHelper.SayHello** düğmesi seçili `ExampleJsInterop.CallHelloHelperSayHello` olarak adlandırılır ve bir ad geçirmeden `Blazor`, yönteme.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=60&end=69&highlight=8)]

`CallHelloHelperSayHello` JavaScript işlevini çağıran `sayHello` ile yeni bir örneğini `HelloHelper`.

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=19-25)]

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-17)]

Adı geçirilir `HelloHelper`ait ayarlar Oluşturucu `HelloHelper.Name` özelliği. Zaman JavaScript işlevinin `sayHello` yürütüldüğünde, `HelloHelper.SayHello` döndürür `Hello, {Name}!` ileti konsola JavaScript işlevi yazılır.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Konsol, web geliştirici araçları tarayıcının çıktısı:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a>Bir Razor bileşen Sınıf Kitaplığı'nda birlikte çalışma kod paylaşın

JavaScript birlikte çalışma kodu Razor bileşen sınıf kitaplığında dahil edilebilir (`dotnet new blazorlib`), bir NuGet paketi kod paylaşmanıza imkan tanıyan.

Razor bileşen sınıf kitaplığı ekleme JavaScript kaynakları derlemesi işler. JavaScript dosyaları yerleştirilir *wwwroot* klasörünü açın ve araç üstlenir kaynaklar ekleme kitaplığı yapılandırıldığında.

Yalnızca normal bir NuGet paketi başvurulduğu yerleşik NuGet paketini uygulamanın proje dosyasında başvurulan. Uygulama geri yüklendikten sonra olarak bulunuyorlarmış uygulama kodu içinde JavaScript çağırabilirsiniz C#.
