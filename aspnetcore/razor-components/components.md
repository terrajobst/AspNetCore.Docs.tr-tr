---
title: Oluşturma ve Razor bileşenleri kullanma
author: guardrex
description: Oluşturma ve Razor bileşenler, bileşen ömürleri yönetme verilere bağlayın ve olayları işlemek nasıl dahil olmak üzere kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/components
ms.openlocfilehash: 8f0a2c7ab7650894010e39ba3cbdcc6a97e04069
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159535"
---
# <a name="create-and-use-razor-components"></a>Oluşturma ve Razor bileşenleri kullanma

Tarafından [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), ve [Morné Zaayman](https://github.com/MorneZaayman)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)). Bkz: [başlama](xref:razor-components/get-started) Önkoşullar için konu.

Razor bileşenleri uygulamaları kullanılarak oluşturulur *bileşenleri*. Bir bileşen, kullanıcı arabirimi (UI), sayfa, iletişim veya form gibi kendi içinde bir öbektir. Her iki HTML biçimlendirmesi veri ekleme veya UI olaylarına yanıt vermek için gereken işleme mantığı ile birlikte işlemek için bir bileşeni içerir. Esnek ve basit bileşenlerdir ve bunlar iç içe geçmiş, yeniden kullanılabilir ve projeler arasında paylaşılan.

## <a name="component-classes"></a>Bileşen sınıfları

Bileşenleri genellikle uygulanır  *\*.cshtml* bir birleşimini kullanarak dosyaları C# ve HTML biçimlendirmesi. Bir bileşen için kullanıcı Arabirimi, HTML kullanılarak tanımlanır. (Örneğin, döngü, koşullular, ifadeleri) dinamik işleme mantığı, katıştırılmış kullanarak eklenir C# adlı söz dizimi [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor). Ne zaman bir Razor bileşenleri uygulama derlendiğinde, HTML biçimlendirmesi ve C# işleme mantığı, bir bileşen sınıfı dönüştürülür. Oluşturulan sınıfın adı dosya adıyla aynıdır.

Bileşen sınıfı üyeleri tanımlanmış bir `@functions` blok (birden fazla `@functions` bloğu izin verilen). İçinde `@functions` blok, bileşen durumu (Özellikler, alanlar), olay işleme için veya başka bir bileşen mantığı tanımlamak için yöntemlerle birlikte belirtilir.

Bileşen üyeleri, ardından bileşenin parçası mantığı kullanarak işleme olarak kullanılabilir C# ile başlayan ifadeleri `@`. Örneğin, bir C# alan ekleyerek işlenen `@` alan adı. Aşağıdaki örnek, değerlendirir ve işler:

* `_headingFontStyle` CSS özellik değerini `font-style`.
* `_headingText` içeriği için `<h1>` öğesi.

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

Bileşen, bileşen başlangıçta işlenen sonra olaylara yanıt olarak, işleme ağacında yeniden oluşturur. Razor bileşenleri yeni bir işleme ağacı Öncekine karşı karşılaştırır ve herhangi bir değişiklik tarayıcının belge nesne modeli (DOM) için geçerlidir.

## <a name="using-components"></a>Bileşenleri kullanma

Bileşenleri, diğer bileşenlerin bunları bildirerek içerebilir HTML öğesi söz dizimini kullanarak. Bir bileşen kullanma için işaretleme, etiketin adını bileşen türü olduğu gibi HTML etiketleri arar.

Aşağıdaki biçimlendirmede işleyen bir `HeadingComponent` (*HeadingComponent.cshtml*) örneği:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?start=11&end=11)]

## <a name="component-parameters"></a>Bileşen parametreleri

Bileşenleri olabilir *bileşeni parametreleri*, hangi kullanılarak tanımlanır *genel olmayan* bileşen sınıfı özellikleri düzenlenmiş ile `[Parameter]`. Öznitelikleri bir bileşen için bağımsız değişken biçimlendirme içinde belirtmek için kullanın.

Aşağıdaki örnekte, `ParentComponent` değerini ayarlar `Title` özelliği `ChildComponent`:

*ParentComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?start=1&end=7&highlight=5)]

*ChildComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a>Alt içeriğin

Bileşenleri içeriği başka bir bileşen ayarlayabilirsiniz. Atama bileşen alıcı bileşeni belirtin etiketleri arasında içerik sağlar. Örneğin, bir `ParentComponent` tarafından işlenmek üzere içerik sağlayabilir bir `ChildComponent` içeriğini yerleştirerek  **\<ChildComponent >** etiketler.

*ParentComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?start=1&end=7&highlight=6)]

Alt bileşen bir `ChildContent` temsil eden özellik bir `RenderFragment`. Değerini `ChildContent` alt bileşen işaretlemede içerik nerede oluşturulması gerekip konumlandırıldı. Aşağıdaki örnekte, değerini `ChildContent` ana bileşenden alınan ve önyükleme bölmenin içinde işlenen `panel-body`.

*ChildComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> Özellik Alma `RenderFragment` içeriği adlı `ChildContent` kural tarafından.

## <a name="data-binding"></a>Veri bağlama

Veri bağlama bileşenleri hem DOM öğeleri ile gerçekleştirilir `bind` özniteliği. Aşağıdaki örnek bağlar `ItalicsCheck` özelliğini onay kutusunun işaretli durumu:

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" bind="@_italicsCheck" />
```

Onay kutusunu işaretli ve seçildiğinde özelliğin değerini şekilde güncelleştirilir `true` ve `false`sırasıyla.

Yalnızca bileşen, özelliğin değerinin değiştirilmesi için değil yanıt oluşturulduğunda onay kutusunu kullanıcı Arabiriminde güncelleştirilir. Olay işleyici kodu yürütüldükten sonra bileşenleri kendilerini işleme olduğundan, özellik güncelleştirmeleri genellikle kullanıcı Arabiriminde hemen yansıtılır.

Kullanarak `bind` ile bir `CurrentValue` özelliği (`<input bind="@CurrentValue" />`) aslında aşağıdakine eşdeğerdir:

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

Bileşen işlendiğinde `value` giriş öğesinin geldiği `CurrentValue` özelliği. Kullanıcı, metin kutusuna yazdığında `onchange` tetiklenir ve `CurrentValue` özelliği değiştirilmiş değerine ayarlanır. Gerçekte, kod oluşturma biraz daha karmaşık olduğundan `bind` tür dönüştürmeleri gerçekleştirildiği birkaç durum ile ilgilidir. Giren İlkesi `bind` geçerli değerini bir ifade ile ilişkilendirir bir `value` kayıtlı işleyici kullanarak öznitelik ve işleyicilerini değişiklikler.

**Biçim dizeleri**

Veri bağlama ile birlikte çalışır [DateTime](https://docs.microsoft.com/dotnet/api/system.datetime) biçimlendirme dizeleri (ancak diğer biçim ifadelerini para birimi veya sayı biçimleri gibi şu anda değil):

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd" />

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

`format-value` Özniteliği uygulamak için tarih biçimini belirtir `value` , `input` öğesi. Biçimi de değer ayrıştırmak için kullanılan zaman bir `onchange` olayı oluşur.

**Bileşen parametreleri**

Bağlama bileşeni parametreleri de tanır burada `bind-{property}` bir özellik değeri bileşenlerinde bağlayabilirsiniz.

Aşağıdaki bileşen `ChildComponent` ve bağlar `ParentYear` üst parametresinden `Year` alt bileşen parametresi:

Ana bileşenin:

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">Change Year to 1986</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

Alt bileşeni:

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private Action<int> YearChanged { get; set; }
}
```

`Year` Bir yardımcı olduğundan parametre bağlanabilir `YearChanged` türüyle eşleşen olay `Year` parametresi.

Yükleme `ParentComponent` aşağıdaki biçimlendirme oluşturur:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

Varsa değerini `ParentYear` düğmesini seçerek özelliği değiştirildiğinde `ParentComponent`, `Year` özelliği `ChildComponent` güncelleştirilir. Öğesinin yeni değeri `Year` Arabiriminde işlenen olduğunda `ParentComponent` rerendered:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

## <a name="event-handling"></a>Olay işleme

Razor bileşenleri olay işleme özellikleri sağlar. İçin bir HTML öğesi öznitelik adlı `on<event>` (örneğin, `onclick`, `onsubmit`) temsilci türü belirtilmiş bir değer ile Razor bileşenleri özniteliğinin değeri bir olay işleyicisi değerlendirir. Özniteliğin adı her zaman ile başlayan `on`.

Aşağıdaki kod çağrıları `UpdateHeading` Arabiriminde düğme seçildiğinde yöntemi:

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

Aşağıdaki kod çağrıları `CheckboxChanged` onay kutusunu kullanıcı Arabiriminde değiştirildiğinde yöntemi:

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged" />

@functions {
    void CheckboxChanged()
    {
        ...
    }
}
```

Olay işleyicileri zaman uyumsuz ve dönüş ayrıca olabilir bir `Task`. El ile çağırmaya gerek yoktur `StateHasChanged()`. Ortaya çıkan özel durumlar günlüğe kaydedilir.

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

Bazı olaylar için olaya özgü olay bağımsız değişken türleri de izin verilir. Bu olay türleri birine erişimi gerekli değilse, yöntem çağrısında gerekli değildir.

Desteklenen olay bağımsız değişkenleri listesi verilmiştir:

* UIEventArgs
* UIChangeEventArgs
* UIKeyboardEventArgs
* UIMouseEventArgs

Lambda ifadeleri de kullanılabilir:

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

Genellikle gibi ek değerler kapatmak uygun olan öğeleri kümesi yineleme olduğunda. Aşağıdaki örnek, üç oluşturur düğmeler, her biri çağıran `UpdateHeading` olay bağımsız değişken geçirme (`UIMouseEventArgs`) ve düğme sayısı (`buttonNumber`) kullanıcı Arabiriminde seçili olduğunda:

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary" 
            onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@functions {
    string message = "Select a button to learn its position.";

    void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            "mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

## <a name="capture-references-to-components"></a>Bileşenleri başvurular yakalama

Bileşen başvuruları komutları gibi bu örneğe verebilir böylece bu şekilde get bileşen örneğe bir başvuru sağlar `Show` veya `Reset`. Bir bileşen başvurusunu yakalamak için ekleme bir `ref` özniteliği alt bileşen ve aynı ada ve aynı türe sahip bir alan alt bileşeni olarak tanımlayabilirsiniz.

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    MyLoginDialog loginDialog;

    void OnSomething()
    {
        loginDialog.Show();
    }
}
```

Bileşen işlendiğinde `loginDialog` alanı ile doldurulur `MyLoginDialog` alt bileşen örneği. Ardından bileşen örneği üzerinde .NET yöntemlerini çağırabilirsiniz.

> [!IMPORTANT]
> `loginDialog` Değişkeni bileşeni işlenir ve çıktısını içeren sonra yalnızca doldurulmuş `MyLoginDialog` öğesi o zamana kadar olduğundan başvuru bir şey yok. Bileşen oluşturma işlemini tamamladıktan sonra bileşenleri başvurularını değiştirmek üzere `OnAfterRenderAsync` veya `OnAfterRender` yaşam döngüsü yöntemleri.

Bileşen başvurularını yakalama için benzer bir sözdizimi kullanırken [öğesi başvuruları yakalama](xref:razor-components/javascript-interop#capture-references-to-elements), öyle bir [JavaScript birlikte çalışma](xref:razor-components/javascript-interop) özelliği. Bileşen başvurularını JavaScript koduna geçen değildir; Bunlar, yalnızca .NET kodda kullanılırlar.

> [!NOTE]
> Yapmak **değil** alt bileşenlerin durumunu kesilecek bileşen başvuruları kullanın. Bunun yerine, alt bileşenler için veri iletmek için her zaman normal bildirim temelli parametreler kullanın. Bu alt bileşenleri otomatik olarak doğru zamanlarda rerender neden olur.

## <a name="lifecycle-methods"></a>Yaşam döngüsü yöntemleri

`OnInitAsync` ve `OnInit` bileşeni başlatıldıktan sonra kod yürütebilir. Zaman uyumsuz bir işlemi gerçekleştirmek için `OnInitAsync` ve `await` anahtar sözcüğü:

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

Eşzamanlı bir işlem için kullanmak `OnInit`:

```csharp
protected override void OnInit()
{
    ...
}
```

`OnParametersSetAsync` ve `OnParametersSet` bir bileşen üst öğesinden parametreleri aldı ve özelliklerine değerler atanır çağırılır. Bu yöntemler, sonra yürütülür `OnInit` bileşen başlatma sırasında.

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

`OnAfterRenderAsync` ve `OnAfterRender` bir bileşen oluşturma işlemini tamamladıktan sonra her zaman çağrılır. Öğe ve bileşen başvuruları bu noktada doldurulur. Bu aşama, işlenmiş DOM öğeleri üzerinde çalışan üçüncü taraf JavaScript kitaplıklarını etkinleştirme gibi işlenmiş içeriği kullanarak ek başlatma adımları gerçekleştirmek için kullanın.

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

`SetParameters` parametreleri ayarlamadan önce kodu çalıştırmak için geçersiz kılınabilir:

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

Varsa `base.SetParameters` değilse çağrılır, özel kod yolu gerekli gelen parametre değeri yorumlayabilir. Örneğin, gelen parametreleri sınıfındaki özellikleri atanması için gerekli değildir.

`ShouldRender` Kullanıcı Arabiriminde yenilemeyi engellemek için geçersiz kılınabilir. Uygulama döndürürse `true`, UI yenilenir. Bile `ShouldRender` olan geçersiz kılınan, bileşen her zaman başlangıçta işlenir.

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a>IDisposable ile bileşen elden çıkarma

Bir bileşen uyguluyorsa [IDisposable](https://docs.microsoft.com/dotnet/api/system.idisposable), [Dispose yöntemini](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose) bileşeni Arabiriminden kaldırıldığında çağrılır. Aşağıdaki bileşen `@implements IDisposable` ve `Dispose` yöntemi:

```csharp
@using System
@implements IDisposable

...

@functions {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a>Yönlendirme

Razor bileşenlerde yönlendirme uygulamasında erişilebilir her bileşeni için bir rota şablonu sağlayarak gerçekleştirilir.

Olduğunda bir  *\*.cshtml* ile dosya bir `@page` yönergesi derlendiğinde, oluşturulan sınıfın belirli bir [RouteAttribute](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) belirten rota şablonu. Çalışma zamanında bileşen sınıfları ile yönlendirici arar bir `RouteAttribute` ve hangi bileşen istenen URL ile eşleşen bir rota şablonuna sahip işler.

Bir bileşenin birden çok yol şablonu uygulanabilir. Aşağıdaki bileşen isteklerine yanıt veren `/BlazorRoute` ve `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

## <a name="route-parameters"></a>Yol parametreleri

Sağlanan yol şablondan, rota parametrelerinin bileşenleri alabilir `@page` yönergesi. Yönlendirici, karşılık gelen bileşen parametreleri doldurmak için rota parametrelerini kullanır.

*RouteParameter.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=9)]

İsteğe bağlı parametreler desteklenmez, bu nedenle iki `@page` yönergeleri, yukarıdaki örnekte uygulanır. İlk Gezinti parametresi olmadan bileşenine izin verir. İkinci `@page` yönergesi gereken `{text}` rota parametresi ve değeri atar `Text` özelliği.

## <a name="base-class-inheritance-for-a-code-behind-experience"></a>Bir "code-behind" deneyimi için temel sınıf devralma

Bileşen dosyaları (*\*.cshtml*) HTML biçimlendirmeyi karışımı ve C# aynı dosyada kod işleme. `@inherits` Yönergesi, Razor bileşenleri uygulamalar Bileşen biçimlendirme işleme koddan ayıran bir "code-behind" deneyimi sağlamak için kullanılabilir.

[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) nasıl bir bileşen bir temel sınıf devralma işlemi yapabileceğini gösterir `BlazorRocksBase`, bileşenin özellikleri ve yöntemleri sağlar.

*BlazorRocks.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?start=1&end=8)]

*BlazorRocksBase.cs*:

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

Temel sınıfın türetilmesi `BlazorComponent`.

## <a name="razor-support"></a>Razor desteği

**Razor yönergesi**

Razor yönergeleri, aşağıdaki tabloda gösterilmiştir.

| Yönergesi | Açıklama |
| --------- | ----------- |
| [@functions](https://docs.microsoft.com/aspnet/core/mvc/views/razor#functions) | Ekler bir C# bileşenine kod bloğu. |
| `@implements` | Oluşturulan bileşen sınıfı için bir arabirim uygular. |
| [@inherits](https://docs.microsoft.com/aspnet/core/mvc/views/razor#inherits) | Bileşen devralan sınıf tam denetim sağlar. |
| [@inject](https://docs.microsoft.com/aspnet/core/mvc/views/razor#inject) | Hizmet ekleme gelen etkinleştirir [hizmet kapsayıcı](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). Daha fazla bilgi için [görünümlere bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/mvc/views/dependency-injection). |
| `@layout` | Bir düzen bileşeni belirtir. Düzen bileşenleri, kod yinelemesi ve tutarsızlık önlemek için kullanılır. |
| [@page](https://docs.microsoft.com/aspnet/core/mvc/razor-pages#razor-pages) | Bileşen doğrudan istekleri işleyeceğini belirtir. `@page` Yönergesi, bir rota ve isteğe bağlı parametreler ile belirtilebilir. Razor sayfaları aksine `@page` yönergesi üst dosyanın ilk yönerge olması gerekmez. Daha fazla bilgi için [yönlendirme](xref:razor-components/routing). |
| [@using](https://docs.microsoft.com/aspnet/core/mvc/views/razor#using) | Ekler C# `using` yönergesini oluşturulan bileşen sınıfı. |
| [@addTagHelper](https://docs.microsoft.com/aspnet/core/mvc/views/razor#tag-helpers) | Kullanma `@addTagHelper` uygulamanın derleme farklı bir derleme bir bileşeni kullanmak için. |

**Koşullu öznitelikleri**

Öznitelikleri .NET değerine göre koşullu olarak işlenir. Değer ise `false` veya `null`, öznitelik işlenen değil. Değer ise `true`, öznitelik işlenen simge.

Aşağıdaki örnekte, `IsCompleted` belirler `checked` denetimin biçimlendirme içinde işlenir.

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

Varsa `IsCompleted` olduğu `true`, onay kutusunu olarak işlenir:

```html
<input type="checkbox" checked />
```

Varsa `IsCompleted` olduğu `false`, onay kutusunu olarak işlenir:

```html
<input type="checkbox" />
```

**Razor hakkında daha fazla bilgi**

Razor hakkında daha fazla bilgi için bkz. [Razor söz dizimi başvurusu](https://docs.microsoft.com/aspnet/core/mvc/views/razor). Tüm Razor özellikleri şu anda Razor bileşenlerinde kullanılabilir olduğunu unutmayın.

## <a name="raw-html"></a>Ham HTML

Dizeler genellikle DOM metin düğümleri kullanarak içeriyor olabilir herhangi bir biçimlendirme yok sayıldı ve düz metin kabul yani işlenir. Ham HTML oluşturmak için bir HTML içeriği kaydırma bir `MarkupString` ardından HTML veya SVG olarak ayrıştırılması ve DOM'a eklenmesi, değer

> [!WARNING]
> Herhangi bir yapıda ham HTML'yi işlemeye güvenilmeyen kaynak bir **güvenlik riski** ve kaçınılmalıdır!

Aşağıdaki örnek, gösterir kullanarak `MarkupString` bir bileşenin işlenen çıkışı için bir statik HTML içeriği bloğunu ekleyin:

```html
@((MarkupString)myMarkup)

@functions {
    string myMarkup = "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a>Şablonlu bileşenleri

Şablonlu bileşenleri bileşenin işleme mantığı bir parçası olarak kullanılabilir bir parametre olarak bir veya daha fazla kullanıcı Arabirimi şablonları kabul bileşenlerdir. Şablonlu bileşenleri normal bileşenleri daha fazla yeniden kullanılabilir olan üst düzey bileşeni yazmanızı sağlar. Birkaç örnek şunlardır:

* Bir tablonun üst bilgi, satırları ve alt bilgisi için şablonları belirtmesine imkan tanıyan bir tablo bileşeni.
* Bir listedeki öğeleri işleme için bir şablon belirtmesine imkan tanıyan bir liste bileşeni.

### <a name="template-parameters"></a>Şablon parametreleri

Şablonlu bir bileşen türünde bir veya daha fazla bileşen parametreleri belirterek tanımlanmış `RenderFragment` veya `RenderFragment<T>`. Bir işleme parça bileşen tarafından işlenen kullanıcı arabiriminin bir kesimi temsil eder. Bir işleme parça, isteğe bağlı olarak işleme parça çağrıldığında belirtilebilecek bir parametre alır.

*Components/TableTemplate.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

Şablonlu bir bileşen kullanırken, şablon parametrelerinin parametrelerinin adları eşleşen alt öğeleri kullanılarak belirtilebilir (`TableHeader` ve `RowTemplate` aşağıdaki örnekte):

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a>Şablon bağlam parametreleri

Bileşen Türü bağımsız değişkenleri `RenderFragment<T>` öğeleri adlı örtük bir parametre olarak geçirilen `context` (örneğin önceki kod örneğinde,'den `@context.PetId`), ancak parametre adını kullanarak değiştirebilirsiniz `Context` alt özniteliği öğe. Aşağıdaki örnekte, `RowTemplate` öğenin `Context` özniteliği belirtir `pet` parametresi:

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

Alternatif olarak, belirtebileceğiniz `Context` bileşen öğesindeki özniteliği. Belirtilen `Context` özniteliği için tüm belirtilen şablon parametreleri geçerlidir. Örtük alt içeriğin içerik parametresi adını belirlemek istediğinizde bu kullanışlı olabilir (herhangi bir sarmalama olmadan alt öğesi). Aşağıdaki örnekte, `Context` özniteliği görünür `TableTemplate` öğenin ve tüm şablon parametreleri için geçerlidir:

```cshtml
<TableTemplate Items="@pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a>Genel yazılmış bileşenler

Şablonlu bileşenleri çoğunlukla genel olarak yazılmalıdır. Örneğin, bir genel ListView bileşeni oluşturmak için kullanılabilir `IEnumerable<T>` değerleri. Genel bileşen tanımlamak için `@typeparam` tür parametreleri belirtmek için.

*Components/ListViewTemplate.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml?highlight=1)]

Tür parametresi, genel yazılmış bileşenler kullanırken, mümkünse algılanır:

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

Aksi takdirde, tür parametresi açık tür parametresinin adıyla eşleşen bir özniteliği kullanılarak belirtilmelidir. Aşağıdaki örnekte, `TItem="Pet"` türünü belirtir:

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a>Geçişli değerleri ve parametreler

Bazı senaryolarda, akış verileri kullanarak bir alt bileşen bir üst bileşeninden kullanışsız [bileşeni parametreleri](#component-parameters), özellikle birçok bileşen katmanları olduğunda. Geçişli değerleri ve parametre bir üst bileşeni tüm alt öğe bileşenleri için bir değer sağlamak için kullanışlı bir yol sağlayarak bu sorunu çözer. Ayrıca basamaklı değerleri ve parametreler koordine etmek, bileşenler için bir yaklaşım sağlar.

### <a name="theme-example"></a>Tema örnek

Aşağıdaki *tema* örnek uygulama örneğinden `ThemeInfo` sınıfı, böylece tüm düğmelerin belirli bir uygulamanın parçası içinde aynı stili paylaşır bileşen hiyerarşisi akış için tema bilgileri belirtir.

*UIThemeClasses/ThemeInfo.cs*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

Üst bileşeni kullanarak basamaklı bir değer sağlayabilirsiniz `CascadingValue` bileşeni. `CascadingValue` Bileşeni bileşen hiyerarşisi bir alt ağacı sarmalar ve o alt ağacı içinde tüm bileşenleri tek bir değer sağlar.

Örneğin, örnek uygulamayı tema bilgileri belirtir (`ThemeInfo`) bir düzen gövdesini oluşturan tüm bileşenlerin basamaklı bir parametre olarak uygulamanın düzenleri `@Body` özelliği. `ButtonClass` bir değeri atanır `btn-success` Düzen bileşende. Bu özelliği üzerinden herhangi bir alt bileşen tüketebileceği `ThemeInfo` basamaklı nesnesi.

*Shared/CascadingValuesParametersLayout.cshtml*:

```cshtml
@inherits BlazorLayoutComponent
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@functions {
    ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

Yapmak için geçişli değerlerini kullanmak, bileşenlerini kullanarak basamaklı parametreleri bildirmek `[CascadingParameter]` özniteliği veya bir dize adı değerine göre:

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")] PermInfo Permissions { get; set; }
```

Bir dize adı değeri bağlamayla aynı türde birden fazla basamaklı değerler varsa ve aynı alt ağacı içinde ayrılmaları gerekiyorsa geçerlidir.

Basamaklı değerler türüne göre geçişli parametrelerine bağlı.

Örnek uygulamada `CascadingValuesParametersTheme` bileşeni bağlanır `ThemeInfo` basamaklı basamaklı bir parametre değeri. Parametre için bir bileşen tarafından görüntülenen düğmeleri CSS sınıfının ayarlamak için kullanılır.

*Pages/CascadingValuesParametersTheme.cshtml*:

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@functions {
    int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a>TabSet örneği

Geçişli parametreleri, bileşen hiyerarşi işbirliği yapmak bileşenler de olanak tanır. Örneğin, aşağıdakileri dikkate alın *TabSet* örnek uygulamada örnek.

Örnek uygulamanın bir `ITab` uygulama sekme arabirimi:

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

`CascadingValuesParametersTabSet` Bileşen `TabSet` birkaç içeren bileşen `Tab` bileşenler:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

Alt `Tab` bileşenleri olmayan açıkça için parametre olarak geçirilen `TabSet`. Bunun yerine, alt `Tab` bileşenlerdir içeriği alt parçası `TabSet`. Ancak, `TabSet` yine her biri hakkında bilmesi gereken `Tab` böylece üstbilgileri ve etkin sekmede işleyebilirsiniz. Ek kod gerek kalmadan bu koordinasyon sağlamak `TabSet` bileşen *kendisini basamaklı bir değer sağlayabilirsiniz* , ardından alındığından descendent `Tab` bileşenleri.

*Components/TabSet.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

Bloğun `Tab` bileşenleri yakalama içeren `TabSet` basamaklı bir parametre olarak böylece `Tab` bileşenleri eklemek için kendilerini `TabSet` ve üretileceği koordinat `Tab` etkindir.

*Components/Tab.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a>Razor şablonları

İşleme parçaları, Razor şablon söz dizimi kullanılarak tanımlanabilir. Razor şablonları UI parçacık tanımlayın ve aşağıdaki biçimi varsayar için bir yol sağlar:

```cshtml
@<tag>...</tag>
```

Aşağıdaki örnek nasıl belirtileceğini göstermektedir `RenderFragment` ve `RenderFragment<T>` değerleri.

*RazorTemplates.cshtml*:

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

Şablonları şablonlu bileşenleri için bağımsız değişken olarak geçirilen veya doğrudan işlenmiş Razor kullanılarak tanımlanan parçalarının işleyin. Örneğin, önceki şablonları aşağıdaki Razor işaretlemesi ile doğrudan işlenir:

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

İşlenmiş çıkışı:

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```
