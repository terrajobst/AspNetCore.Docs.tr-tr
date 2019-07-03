---
title: Oluşturma ve ASP.NET Core Razor bileşenleri kullanma
author: guardrex
description: Oluşturma ve bileşen ömürleri yönetme verilere bağlayın ve olayları işlemek nasıl dahil olmak üzere, Razor bileşenlerini kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/components
ms.openlocfilehash: c52f23ea319d30d871ecdfc9648a4e30aa877324
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538507"
---
# <a name="create-and-use-aspnet-core-razor-components"></a>Oluşturma ve ASP.NET Core Razor bileşenleri kullanma

Tarafından [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Blazor uygulamaları kullanılarak oluşturulur *bileşenleri*. Bir bileşen, kullanıcı arabirimi (UI), sayfa, iletişim veya form gibi kendi içinde bir öbektir. Bir bileşeni, HTML biçimlendirmesi ve veri ekleme veya UI olaylarına yanıt vermek için gereken işleme mantığı içerir. Esnek ve basit bileşenlerdir. Bunlar iç içe geçmiş, yeniden kullanılabilir ve projeler arasında paylaşılan.

## <a name="component-classes"></a>Bileşen sınıfları

Bileşenleri içinde uygulanan [Razor](xref:mvc/views/razor) bileşen dosyaları ( *.razor*) bir birleşimi kullanılarak C# ve HTML biçimlendirmesi. Bir bileşenin Blazor, resmi olarak adlandırılır bir *Razor bileşen*.

Bileşenlerini kullanarak yazarı olduğu *.cshtml* dosyaları kullanarak Razor bileşen dosyaları tanımlanmış olduğu sürece dosya uzantısı `_RazorComponentInclude` MSBuild özelliği. Örneğin, belirten bir uygulamayı tüm *.cshtml* altında dosyaları *sayfaları* klasör Razor bileşenleri dosyaları kabul:

```xml
<_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
```

Bir bileşen için kullanıcı Arabirimi, HTML kullanılarak tanımlanır. (Örneğin, döngü, koşullular, ifadeleri) dinamik işleme mantığı, katıştırılmış kullanarak eklenir C# adlı söz dizimi [Razor](xref:mvc/views/razor). Bir uygulamanın ne zaman derlenir, HTML biçimlendirmesi ve C# işleme mantığı, bir bileşen sınıfı dönüştürülür. Oluşturulan sınıfın adı dosya adıyla aynıdır.

Bileşen sınıfı üyeleri tanımlanmış bir `@code` blok. İçinde `@code` blok, bileşen durumu (Özellikler, alanlar) olay işleme için veya başka bir bileşen mantığı tanımlamak için yöntemleri ile belirtilir. Birden fazla `@code` bloğu izin verilebilir.

> [!NOTE]
> ASP.NET Core, önceki sürümlerinde `@functions` blokları için aynı amaca kullanılmış `@code` engeller. `@functions` çalışmaya devam blokları, ancak kullanmanızı öneririz `@code` yönergesi.

Bileşen üyeleri, ardından bileşenin parçası mantığı kullanarak işleme olarak kullanılabilir C# ile başlayan ifadeleri `@`. Örneğin, bir C# alan ekleyerek işlenen `@` alan adı. Aşağıdaki örnek, değerlendirir ve işler:

* `_headingFontStyle` CSS özellik değerini `font-style`.
* `_headingText` içeriği için `<h1>` öğesi.

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

Bileşen, bileşen başlangıçta işlenen sonra olaylara yanıt olarak, işleme ağacında yeniden oluşturur. Blazor yeni bir işleme ağacı Öncekine karşı karşılaştırır ve herhangi bir değişiklik tarayıcının belge nesne modeli (DOM) için geçerlidir.

Bileşenleri sıradan C# sınıfları ve bir projesi içinde her yerden yerleştirilebilir. Web sayfaları genellikle üreten bileşenler bulunan *sayfaları* klasör. Sayfası olmayan bileşenleri yerleştirildiğinde sık *paylaşılan* klasör veya özel bir klasör projeye eklendi. Özel bir klasör kullanmayı ekleyin ya da özel klasör ad alanı ana bileşen veya uygulamanın *_Imports.razor* dosya. Örneğin, aşağıdaki ad alanı bileşenlerde yapar bir *bileşenleri* klasör uygulamanın kök ad alanı olduğunda kullanılabilen `WebApplication`:

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a>Bileşenleri Razor sayfaları ve MVC uygulamalarla tümleştirin

Bileşenleri ile mevcut Razor sayfaları ve MVC uygulamaları kullanın. Var olan sayfaları veya Razor bileşenler kullanmaya görünümleri yeniden gerek yoktur. Sayfa veya Görünüm işlendiğinde bileşenleri aynı anda prerendered.

Bir bileşenden bir sayfa ya da Görünüm işlemek için `RenderComponentAsync<TComponent>` HTML yardımcı yöntemi:

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

Sayfalar ve görünümler bileşenleri kullanabilirsiniz, ancak listesiyse true değil. Bileşenler, kısmi görünümleri ve bölümler gibi görünümü ve sayfa belirli senaryoları kullanamazsınız. Bir bileşene dönüştürerek bir bileşende kısmi görünümü, kısmi görünümü mantıksal çarpanını mantığı kullanılacak.

Blazor sunucu tarafı uygulamalar yönetilen bileşenleri işlenmiş ve bileşen durumunu şeklini hakkında daha fazla bilgi için bkz: <xref:blazor/hosting-models> makalesi.

## <a name="using-components"></a>Bileşenleri kullanma

Bileşenleri, diğer bileşenlerin bunları bildirerek içerebilir HTML öğesi söz dizimini kullanarak. Bir bileşen kullanma için işaretleme, etiketin adını bileşen türü olduğu gibi HTML etiketleri arar.

Aşağıdaki biçimlendirmede *Index.razor* işleyen bir `HeadingComponent` örneği:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

*Components/HeadingComponent.razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

## <a name="component-parameters"></a>Bileşen parametreleri

Bileşenleri olabilir *bileşeni parametreleri*, hangi özellikleri kullanılarak tanımlanır (genellikle *genel olmayan*) ile bileşen sınıfı üzerinde `[Parameter]` özniteliği. Öznitelikleri bir bileşen için bağımsız değişken biçimlendirme içinde belirtmek için kullanın.

*Components/ChildComponent.razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

Aşağıdaki örnekte, `ParentComponent` değerini ayarlar `Title` özelliği `ChildComponent`.

*Pages/ParentComponent.razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a>Alt içeriğin

Bileşenleri başka bir bileşen içeriğini ayarlayabilirsiniz. Atama bileşen alıcı bileşeni belirtin etiketleri arasında içerik sağlar.

Aşağıdaki örnekte, `ChildComponent` sahip bir `ChildContent` temsil eden özellik bir `RenderFragment`. Değerini `ChildContent` bileşenin işaretlemede içeriği burada işleneceğini konumlandırıldı. Değerini `ChildContent` ana bileşenden alınan ve önyükleme bölmenin içinde işlenen `panel-body`.

*Components/ChildComponent.razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> Özellik Alma `RenderFragment` içeriği adlı `ChildContent` kural tarafından.

Aşağıdaki `ParentComponent` içerik işleme için sağlayabilir `ChildComponent` içeriğini yerleştirerek `<ChildComponent>` etiketler.

*Pages/ParentComponent.razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="data-binding"></a>Veri bağlama

Veri bağlama bileşenleri hem DOM öğeleri ile gerçekleştirilir `@bind` özniteliği. Aşağıdaki örnek bağlar `_italicsCheck` alan için onay kutusunun işaretli durumu:

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

Onay kutusunu işaretli ve seçildiğinde özelliğin değerini şekilde güncelleştirilir `true` ve `false`sırasıyla.

Yalnızca bileşen, özelliğin değerinin değiştirilmesi için değil yanıt oluşturulduğunda onay kutusunu kullanıcı Arabiriminde güncelleştirilir. Olay işleyici kodu yürütüldükten sonra bileşenleri kendilerini işleme olduğundan, özellik güncelleştirmeleri genellikle kullanıcı Arabiriminde hemen yansıtılır.

Kullanarak `@bind` ile bir `CurrentValue` özelliği (`<input @bind="CurrentValue" />`) aslında aşağıdakine eşdeğerdir:

```cshtml
<input value="@CurrentValue" 
    @onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

Bileşen işlendiğinde `value` giriş öğesinin geldiği `CurrentValue` özelliği. Kullanıcı, metin kutusuna yazdığında `onchange` olay tetiklenir ve `CurrentValue` özelliği değiştirilmiş değerine ayarlanır. Gerçekte, kod oluşturma biraz daha karmaşık olduğundan `@bind` tür dönüştürmeleri gerçekleştirildiği birkaç durum işler. Giren İlkesi `@bind` geçerli değerini bir ifade ile ilişkilendirir bir `value` kayıtlı işleyici kullanarak öznitelik ve işleyicilerini değişiklikler.

İşleme yanı sıra `onchange` olaylarıyla `@bind` söz dizimi, özelliği veya alanı belirterek diğer olayları kullanarak bağlanabilir bir `@bind-value` özniteliğini bir `event` parametresi. Aşağıdaki örnek bağlar `CurrentValue` özelliği `oninput` olay:

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />
```

Farklı `onchange`, öğe odağından çıktığında tetiklenen `oninput` değeri metin kutusunda değiştiğinde harekete geçirilir.

**Biçim dizeleri**

Veri bağlama ile birlikte çalışır <xref:System.DateTime> biçim dizeleri. Para birimi veya sayı biçimleri gibi diğer biçim ifadeleri şu anda kullanılamıyor.

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

`@bind:format` Özniteliği uygulamak için tarih biçimini belirtir `value` , `<input>` öğesi. Biçimi de değer ayrıştırmak için kullanılan zaman bir `onchange` olayı oluşur.

**Bileşen parametreleri**

Bağlama bileşeni parametreleri de tanır burada `@bind-{property}` bir özellik değeri bileşenlerinde bağlayabilirsiniz.

Aşağıdaki alt bileşen (`ChildComponent`) sahip bir `Year` bileşen parametresi ve `YearChanged` geri çağırma:

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private EventCallback<int> YearChanged { get; set; }
}
```

`EventCallback<T>` açıklanan [EventCallback](#eventcallback) bölümü.

Aşağıdaki ana bileşen kullanan `ChildComponent` ve bağlar `ParentYear` üst parametresinden `Year` alt bileşen parametresi:

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="@ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

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

`Year` Bir yardımcı olduğundan parametre bağlanabilir `YearChanged` türüyle eşleşen olay `Year` parametresi.

Kural olarak, `<ChildComponent @bind-Year="ParentYear" />` yazma temelde eşdeğerdir:

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

Genel olarak, bir özelliği karşılık gelen olay işleyicisi kullanarak bir bağlanabilir `@bind-property:event` özniteliği. Örneğin, özellik `MyProp` bağlanabilir `MyEventHandler` aşağıdaki iki öznitelikleri kullanarak:

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a>Olay işleme

Razor bileşenleri olay işleme özellikleri sağlar. İçin bir HTML öğesi öznitelik adlı `on<event>` (örneğin, `onclick` ve `onsubmit`) temsilci türü belirtilmiş bir değer ile Razor bileşenlerini değerlendirir özniteliğin değeri bir olay işleyicisi. Özniteliğin adı her zaman ile başlayan `@on`.

Aşağıdaki kod çağrıları `UpdateHeading` Arabiriminde düğme seçildiğinde yöntemi:

```cshtml
<button class="btn btn-primary" @onclick="@UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

Aşağıdaki kod çağrıları `CheckChanged` onay kutusunu kullanıcı Arabiriminde değiştirildiğinde yöntemi:

```cshtml
<input type="checkbox" class="form-check-input" @onchange="@CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

Olay işleyicileri zaman uyumsuz ve dönüş ayrıca olabilir bir <xref:System.Threading.Tasks.Task>. El ile çağırmaya gerek yoktur `StateHasChanged()`. Ortaya çıkan özel durumlar günlüğe kaydedilir.

Aşağıdaki örnekte, `UpdateHeading` düğmesi seçili olduğunda zaman uyumsuz olarak adlandırılır:

```cshtml
<button class="btn btn-primary" @onclick="@UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

Bazı olaylar için olaya özgü olay bağımsız değişken türleri de izin verilir. Bu olay türleri birine erişimi gerekli değilse, yöntem çağrısında gerekli değildir.

Desteklenen [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/UIEventArgs.cs) aşağıdaki tabloda gösterilmiştir.

| Olay | örneği |
| ----- | ----- |
| Pano | `UIClipboardEventArgs` |
| Sürükle  | `UIDragEventArgs` &ndash; `DataTransfer` bir Sürükle ve bırak işlemi sırasında sürüklenen verileri tutmak için kullanılır ve bir veya daha fazla tutabilir `UIDataTransferItem`. `UIDataTransferItem` temsil eder bir veri öğesi sürükleyin. |
| Hata | `UIErrorEventArgs` |
| Odağı | `UIFocusEventArgs` &ndash; İçin destek içermez `relatedTarget`. |
| `<input>` Değişiklik | `UIChangeEventArgs` |
| Klavye | `UIKeyboardEventArgs` |
| Fare | `UIMouseEventArgs` |
| Fare işaretçisi | `UIPointerEventArgs` |
| Fare tekerleği | `UIWheelEventArgs` |
| İlerleme durumu | `UIProgressEventArgs` |
| Dokunma | `UITouchEventArgs` &ndash; `UITouchPoint` dokunmaya duyarlı cihazda tek bir iletişim noktası temsil eder. |

Özellikleri ve olay davranışını önceki tabloda olayları işleme hakkında daha fazla bilgi için bkz: [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/UIEventArgs.cs) başvuru kaynak.
  
Lambda ifadeleri de kullanılabilir:

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

Genellikle gibi ek değerler kapatmak uygun olan öğeleri kümesi yineleme olduğunda. Aşağıdaki örnek, üç oluşturur düğmeler, her biri çağıran `UpdateHeading` olay bağımsız değişken geçirme (`UIMouseEventArgs`) ve düğme sayısı (`buttonNumber`) kullanıcı Arabiriminde seçili olduğunda:

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> Yapmak **değil** Döngü değişkeninin kullanın (`i`) içinde bir `for` bir lambda ifadesinde doğrudan döngü. Aksi takdirde aynı değişkene neden tüm lambda ifadeleri tarafından kullanılan `i`değerini tüm lambda içinde ile aynı. Yerel bir değişkende değeri her zaman yakalama (`buttonNumber` önceki örnekte) ve ardından kullanın.

### <a name="eventcallback"></a>EventCallback

İç içe geçmiş bileşen sık karşılaşılan bir senaryodur arzusu bir alt bileşen olay gerçekleştiğinde bir ana bileşenin yöntemini çalıştırılacak olan&mdash;Örneğin, bir `onclick` olay alt gerçekleşir. Bileşenlerinde olaylar oluşturmak için kullanmak bir `EventCallback`. Ana bileşenin bir alt bileşen için bir geri çağırma yöntemi atayabilirsiniz `EventCallback`.

`ChildComponent` Örnekte bir düğme nasıl ait uygulama gösterilmektedir `onclick` işleyici ayarlandığından almak için bir `EventCallback` temsilci örnekten 's `ParentComponent`. `EventCallback` İle yazılan `UIMouseEventArgs`, uygun olduğu bir `onclick` çevre cihazı olay:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

`ParentComponent` Çocuğun ayarlar `EventCallback<T>` için kendi `ShowMessage` yöntemi:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

İçinde düğme seçildiğinde `ChildComponent`:

* `ParentComponent`'S `ShowMessage` yöntemi çağrılır. `messageText` Güncelleştirilen ve görüntülenen `ParentComponent`.
* Bir çağrı `StateHasChanged` geri çağırma'nın yöntemi gerekli değildir (`ShowMessage`). `StateHasChanged` rerender için otomatik olarak çağrılan `ParentComponent`, bileşen içinde alt yürütme olay işleyicilerinde rerendering alt olaylarını tetiklemek gibi.

`EventCallback` ve `EventCallback<T>` zaman uyumsuz temsilciler izin verir. `EventCallback<T>` türü kesin olarak belirtilmiş ve belirli bir bağımsız değişken türü gerektirir. `EventCallback` zayıf yazılmış ve herhangi bir bağımsız değişken türü sağlar.

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

Çağırma bir `EventCallback` veya `EventCallback<T>` ile `InvokeAsync` ve await <xref:System.Threading.Tasks.Task>:

```csharp
await callback.InvokeAsync(arg);
```

Kullanım `EventCallback` ve `EventCallback<T>` olay işleme ve bileşen parametre bağlama için.

Kesin olarak belirlenmiş tercih `EventCallback<T>` üzerinden `EventCallback`. `EventCallback<T>` Bileşen kullanıcıları için daha iyi hata geri bildirim sağlar. Diğer UI olay işleyicilerine benzer, olay parametresi belirten isteğe bağlıdır. Kullanım `EventCallback` çağırma işlemine geçirilen değer olduğunda.

## <a name="capture-references-to-components"></a>Bileşenleri başvurular yakalama

Bileşen başvurularını komutları gibi bu örneğe verebilir böylece bileşen örneğinin başvurmak için bir yol sağlar `Show` veya `Reset`. Bir bileşen başvurusunu yakalamak için ekleme bir `@ref` özniteliği alt bileşen ve aynı ada ve aynı türe sahip bir alan alt bileşeni olarak tanımlayabilirsiniz.

```cshtml
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

Bileşen işlendiğinde `loginDialog` alanı ile doldurulur `MyLoginDialog` alt bileşen örneği. Ardından bileşen örneği üzerinde .NET yöntemlerini çağırabilirsiniz.

> [!IMPORTANT]
> `loginDialog` Değişkeni bileşeni işlenir ve çıktısını içeren sonra yalnızca doldurulmuş `MyLoginDialog` öğesi. O noktaya kadar hiçbir şey yoktur başvurmak için. Bileşen oluşturma işlemini tamamladıktan sonra bileşenleri başvurularını değiştirmek üzere `OnAfterRenderAsync` veya `OnAfterRender` yöntemleri.

Bileşen başvurularını yakalarken kullanmak için benzer bir sözdizimi [öğesi başvuruları yakalama](xref:blazor/javascript-interop#capture-references-to-elements), öyle bir [JavaScript birlikte çalışma](xref:blazor/javascript-interop) özelliği. Bileşen başvurularını JavaScript kodunu geçirilen olmayan&mdash;yalnızca .NET kodda kullanılırlar.

> [!NOTE]
> Yapmak **değil** alt bileşenlerin durumunu kesilecek bileşen başvuruları kullanın. Bunun yerine, normal bildirim temelli parametreler alt bileşenler için veri aktarmak için kullanın. Normal bildirim temelli parametreler sonucunda otomatik olarak doğru zamanlarda rerender alt bileşenlerle kullanma.

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a>Kullanım @key öğeleri ve bileşenleri korunması denetlemek için

Öğeleri veya bileşenleri ve öğeleri veya bileşenleri listesini sonradan işleme değiştirdiğinizde Blazor'ın ayırırken algoritması, önceki öğeleri veya bileşenleri tutulabilir ve model nesneleri için bunları nasıl eşlemelisiniz karar vermeniz gerekir. Genellikle bu işlemi otomatik olarak yüklenir ve göz ardı edilebilir, ancak işlemini denetlemek için burada isteyebileceğiniz durumlar vardır.

Aşağıdaki örnek göz önünde bulundurun:

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="@person.Details" />
}

@code {
    [Parameter]
    private IEnumerable<Person> People { get; set; }
}
```

İçeriğini `People` koleksiyon değişebilir ile eklenen, silinen veya Girişleri'yeniden sıralanabilir. Bileşen rerenders, `<DetailsEditor>` bileşeni farklı almak için değişebilir `Details` parametre değerleri. Bu işlem, beklenenden daha karmaşık rerendering neden olabilir. Bazı durumlarda, rerendering kayıp öğesi odak gibi görünür davranış farklılıklarının neden olabilir.

Eşleme işlemi ile denetlenebilir `@key` yönerge özniteliği. `@key` öğeleri veya bileşenleri anahtarın değerine göre korunmasını sağlamak fark alma işlemini algoritması neden olur:

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="@person" Details="@person.Details" />
}

@code {
    [Parameter]
    private IEnumerable<Person> People { get; set; }
}
```

Zaman `People` koleksiyonu değişiklikleri ayırırken algoritması korur arasındaki ilişkiyi `<DetailsEditor>` örnekleri ve `person` örnekleri:

* Varsa bir `Person` hizmetinden silinir `People` listesi, yalnızca ilgili `<DetailsEditor>` örneği, kullanıcı Arabiriminden kaldırılır. Diğer örnekleri sol değişmez.
* Varsa bir `Person` listesinde yeni bir tane bazı konumunda eklenen `<DetailsEditor>` örneğine karşılık gelen o konumdan eklenir. Diğer örnekleri sol değişmez.
* Varsa `Person` girişleri yeniden sıralanır, karşılık gelen `<DetailsEditor>` örnekleri korunur ve kullanıcı Arabiriminde yeniden sıralanabilir.

Bazı senaryolarda kullanım `@key` rerendering karmaşıklığını en aza indirir ve durum bilgisi olan bölümleri DOM, odak konumu gibi değiştirme ile ilgili olası sorunları önler.

> [!IMPORTANT]
> Anahtarlar, her bir kapsayıcı öğe veya bileşeni için yereldir. Anahtarları genel belge boyunca karşılaştırıldığında değildir.

### <a name="when-to-use-key"></a>Ne zaman kullanılır? @key

Genellikle, kullanılacak mantıklıdır `@key` listesini işlenen her (örneğin, bir `@foreach` bloğu) ve tanımlamak için uygun bir değer var. `@key`.

Ayrıca `@key` bir nesne değiştirildiğinde bir öğe veya bileşen alt koruma gelen Blazor önlemek için:

```cshtml
<div @key="@currentPerson">
    ... content that depends on @currentPerson ...
</div>
```

Varsa `@currentPerson` değişiklikleri `@key` özniteliği yönergesi, tüm atmak Blazor zorlar `<div>` ve alt öğeler ve alt ağacı içinde yeni öğeler ve bileşenleri ile kullanıcı arabirimini yeniden oluşturma. Bu, hiçbir kullanıcı Arabirimi durumu ne zaman korunduğunu garantilemeye ihtiyacınız varsa yararlı olabilir `@currentPerson` değişiklikler.

### <a name="when-not-to-use-key"></a>Kullanılmayacağı zaman @key

Performans maliyetine ne zaman yoktur ile ayırırken `@key`. Performans maliyetini büyük değildir, ancak yalnızca belirtin `@key` öğe veya bileşen korunması denetleme kuralları yararlı uygulama ise.

Bile `@key` değil kullanılan Blazor alt öğe ve bileşen örnekleri mümkün olduğunca korur. Yalnızca önyüklenebildiği `@key` üzerinden denetimdir *nasıl* modeli örnek eşleme seçme ayırırken algoritması yerine korunan bileşen örneklerine eşleştirilir.

### <a name="what-values-to-use-for-key"></a>Hangi için kullanılacak değerleri @key

Aşağıdaki tür için değer birini sağlamak mantıklı genellikle `@key`:

* Model nesne örnekleri (örneğin, bir `Person` örneği önceki örnekte olduğu gibi). Bu, üzerinde nesne başvuru eşitliğine dayalı korunmasını sağlar.
* Benzersiz tanımlayıcıları (örneğin, birincil anahtar değerlerini türü `int`, `string`, veya `Guid`).

Beklenmedik bir şekilde bırakmayan bir değeri sağlama kaçının. Varsa `@key="@someObject.GetHashCode()"` karma kodları ilgisiz nesnelerin aynı olabileceğinden, beklenmeyen çakışmaları oluşabilir sağlanır. Çakışan varsa `@key` değerleri aynı üst öğe içinde istenen `@key` değerleri olmaz dikkate alınır.

## <a name="lifecycle-methods"></a>Yaşam döngüsü yöntemleri

`OnInitAsync` ve `OnInit` bileşeni başlatmak için kod yürütebilir. Zaman uyumsuz bir işlemi gerçekleştirmek için `OnInitAsync` ve `await` anahtar sözcüğü, işlemi:

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

`OnParametersSetAsync` ve `OnParametersSet` bir bileşen üst öğesinden parametreleri aldı ve özelliklerine değerler atanır çağırılır. Bu yöntemler bileşen başlatmadan sonra yürütülür ve her bileşenin oluşturulur:

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

`OnAfterRenderAsync` ve `OnAfterRender` bir bileşen oluşturma tamamlandıktan sonra çağrılır. Öğe ve bileşen başvuruları bu noktada doldurulur. Bu aşama, işlenmiş DOM öğeleri üzerinde çalışan üçüncü taraf JavaScript kitaplıklarını etkinleştirme gibi işlenmiş içeriği kullanarak ek başlatma adımları gerçekleştirmek için kullanın.

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

### <a name="handle-incomplete-async-actions-at-render"></a>Tamamlanmamış zaman uyumsuz işleme eylemlerini işlemek

Yaşam döngüsü olayları zaman uyumsuz eylemlerin bileşeni işlenmeden önce tamamlanmamış olabilir. Nesneleri olabilir `null` veya yaşam döngüsü metodu yürütülürken verilerle doldurulmuş önişlemcinin. Nesneleri başlatıldığını onaylamak için işleme mantığı sağlar. Yer tutucu nesneleri sırasında kullanıcı Arabirimi öğeleri (örneğin, bir yükleme iletisi) işleme olan `null`.

İçinde `FetchData` Blazor şablonlarının bileşen `OnInitAsync` kopyalanmamasına için geçersiz kılındı tahmin veri alma (`forecasts`). Zaman `forecasts` olduğu `null`, kullanıcıya yüklenirken bir ileti görüntülenir. Sonra `Task` tarafından döndürülen `OnInitAsync` tamamlandıktan, bileşen, güncelleştirilmiş durumuyla rerendered.

*Pages/FetchData.razor*:

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a>Parametreleri ayarlamadan önce kod yürütme

`SetParameters` parametreleri ayarlamadan önce kodu çalıştırmak için geçersiz kılınabilir:

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

Varsa `base.SetParameters` değilse çağrılır, özel kod yolu gerekli gelen parametre değeri yorumlayabilir. Örneğin, gelen parametreleri sınıfındaki özellikleri atanması için gerekli değildir.

### <a name="suppress-refreshing-of-the-ui"></a>Kullanıcı Arabiriminde yenileme Gizle

`ShouldRender` Kullanıcı Arabiriminde yenilemeyi engellemek için geçersiz kılınabilir. Uygulama döndürürse `true`, UI yenilenir. Bile `ShouldRender` olan geçersiz kılınan, bileşen her zaman başlangıçta işlenir.

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a>IDisposable ile bileşen elden çıkarma

Bir bileşen uyguluyorsa <xref:System.IDisposable>, [Dispose yöntemini](/dotnet/standard/garbage-collection/implementing-dispose) bileşeni Arabiriminden kaldırıldığında çağrılır. Aşağıdaki bileşen `@implements IDisposable` ve `Dispose` yöntemi:

```csharp
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a>Yönlendirme

İçinde Blazor yönlendirme uygulamasında erişilebilir her bileşeni için bir rota şablonu sağlayarak gerçekleştirilir.

Bir Razor dosya ile bir `@page` yönergesi derlendiğinde, oluşturulan sınıfın belirli bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> belirten rota şablonu. Çalışma zamanında bileşen sınıfları ile yönlendirici arar bir `RouteAttribute` ve hangi bileşen istenen URL ile eşleşen bir rota şablonuna sahip işler.

Bir bileşenin birden çok yol şablonu uygulanabilir. Aşağıdaki bileşen isteklerine yanıt veren `/BlazorRoute` ve `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a>Yol parametreleri

Sağlanan yol şablondan, rota parametrelerinin bileşenleri alabilir `@page` yönergesi. Yönlendirici, karşılık gelen bileşen parametreleri doldurmak için rota parametrelerini kullanır.

*Rota parametresinin bileşen*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

İsteğe bağlı parametreler desteklenmez, bu nedenle iki `@page` yönergeleri, yukarıdaki örnekte uygulanır. İlk Gezinti parametresi olmadan bileşenine izin verir. İkinci `@page` yönergesi gereken `{text}` rota parametresi ve değeri atar `Text` özelliği.

## <a name="base-class-inheritance-for-a-code-behind-experience"></a>Bir "code-behind" deneyimi için temel sınıf devralma

Bileşen dosyaları karıştırmak HTML biçimlendirmesi ve C# aynı dosyada kod işleme. `@inherits` Yönergesi, bileşen biçimlendirme işleme koddan ayıran bir "code-behind" deneyimiyle Blazor uygulamaları sağlamak için kullanılabilir.

[Örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) nasıl bir bileşen bir temel sınıf devralma işlemi yapabileceğini gösterir `BlazorRocksBase`, bileşenin özellikleri ve yöntemleri sağlar.

*Pages/BlazorRocks.razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

*BlazorRocksBase.cs*:

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

Temel sınıfın türetilmesi `ComponentBase`.

## <a name="import-components"></a>İçeri aktarma bileşenleri

Ad alanı, Razor ile yazılmış bir bileşenin dayanır:

* Projenin `RootNamespace`.
* Bileşen yolu proje kök. Örneğin, `ComponentsSample/Pages/Index.razor` ad alanındaki `ComponentsSample.Pages`. Bileşenleri izleyin C# bağlama kurallarını olarak adlandırın. Durumunda, *Index.razor*, tüm bileşenleri aynı klasörde *sayfaları*ve üst klasör *ComponentsSample*, kapsam içindedir.

Farklı bir ad alanında tanımlanan bileşenleri Razor'ın kullanarak kapsamına alınabilir [ \@kullanarak](xref:mvc/views/razor#using) yönergesi.

Başka bir bileşen `NavMenu.razor`, klasörde mevcut `ComponentsSample/Shared/`, bileşen kullanılabilir `Index.razor` aşağıdaki `@using` deyimi:

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

Bileşenleri de başvurabilir bunların tam nitelikli adlarını kullanarak gereksinimini kaldıran [ \@kullanarak](xref:mvc/views/razor#using) yönergesi:

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> `global::` Nitelik desteklenmez.
>
> Diğer adlı bileşenleriyle alma `using` deyimleri (örneğin, `@using Foo = Bar`) desteklenmiyor.
>
> Kısmen nitelenmiş adlar desteklenmez. Örneğin, ekleme `@using ComponentsSample` ve bunlara başvurma `NavMenu.razor` ile `<Shared.NavMenu></Shared.NavMenu>` desteklenmiyor.

## <a name="razor-support"></a>Razor desteği

**Razor yönergesi**

Razor yönergeleri, aşağıdaki tabloda gösterilmiştir.

| Yönergesi | Açıklama |
| --------- | ----------- |
| [\@Kod](xref:mvc/views/razor#section-5) | Ekler bir C# bileşenine kod bloğu. `@code` bir diğer adıdır `@functions`. `@code` üzerinden önerilen `@functions`. Birden fazla `@code` bloğu izin verilebilir. |
| [\@İşlevleri](xref:mvc/views/razor#section-5) | Ekler bir C# bileşenine kod bloğu. Seçin `@code` üzerinden `@functions` için C# kod blokları. |
| `@implements` | Oluşturulan bileşen sınıfı için bir arabirim uygular. |
| [\@Devralan](xref:mvc/views/razor#section-3) | Bileşen devralan sınıf tam denetim sağlar. |
| [\@ekleme](xref:mvc/views/razor#section-4) | Hizmet ekleme gelen etkinleştirir [hizmet kapsayıcı](xref:fundamentals/dependency-injection). Daha fazla bilgi için [görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection). |
| `@layout` | Bir düzen bileşeni belirtir. Düzen bileşenleri, kod yinelemesi ve tutarsızlık önlemek için kullanılır. |
| [\@Sayfa](xref:razor-pages/index#razor-pages) | Bileşen doğrudan istekleri işleyeceğini belirtir. `@page` Yönergesi, bir rota ve isteğe bağlı parametreler ile belirtilebilir. Razor sayfaları aksine `@page` yönergesi üst dosyanın ilk yönerge olması gerekmez. Daha fazla bilgi için [yönlendirme](xref:blazor/routing). |
| [\@kullanma](xref:mvc/views/razor#using) | Ekler C# `using` yönergesini oluşturulan bileşen sınıfı. Bu kapsamın içine o ad alanında tanımlanan tüm bileşenleri de getirir. |
| [\@Namespace](xref:mvc/views/razor#section-6) | Ad alanı oluşturulan bileşen sınıfının ayarlar. |
| [\@Özniteliği](xref:mvc/views/razor#section-7) | Bir öznitelik için oluşturulan bileşen sınıfı ekler. |

**Koşullu HTML öğesi özniteliklerini**

HTML öğesi özniteliklerini .NET değerine göre koşullu olarak işlenir. Değer ise `false` veya `null`, öznitelik işlenen değil. Değer ise `true`, öznitelik işlenen simge.

Aşağıdaki örnekte, `IsCompleted` belirler `checked` öğenin biçimlendirmesi içinde işlenir:

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
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

Razor hakkında daha fazla bilgi için bkz. [Razor söz dizimi başvurusu](xref:mvc/views/razor).

## <a name="raw-html"></a>Ham HTML

Dizeler genellikle DOM metin düğümleri kullanarak içeriyor olabilir herhangi bir biçimlendirme yok sayıldı ve düz metin kabul yani işlenir. Ham HTML oluşturmak için bir HTML içeriği kaydırma bir `MarkupString` değeri. Değer HTML veya SVG ayrıştırılması ve DOM'a eklenmesi

> [!WARNING]
> Herhangi bir yapıda ham HTML'yi işlemeye güvenilmeyen kaynak bir **güvenlik riski** ve kaçınılmalıdır!

Aşağıdaki örnek, gösterir kullanarak `MarkupString` bir bileşenin işlenen çıkışı için bir statik HTML içeriği bloğunu ekleyin:

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a>Şablonlu bileşenleri

Şablonlu bileşenleri bileşenin işleme mantığı bir parçası olarak kullanılabilir bir parametre olarak bir veya daha fazla kullanıcı Arabirimi şablonları kabul bileşenlerdir. Şablonlu bileşenleri normal bileşenleri daha fazla yeniden kullanılabilir olan üst düzey bileşeni yazmanızı sağlar. Birkaç örnek şunlardır:

* Bir tablonun üst bilgi, satırları ve alt bilgisi için şablonları belirtmesine imkan tanıyan bir tablo bileşeni.
* Bir listedeki öğeleri işleme için bir şablon belirtmesine imkan tanıyan bir liste bileşeni.

### <a name="template-parameters"></a>Şablon parametreleri

Şablonlu bir bileşen türünde bir veya daha fazla bileşen parametreleri belirterek tanımlanmış `RenderFragment` veya `RenderFragment<T>`. Bir işleme parça bileşen tarafından işlenen kullanıcı arabiriminin bir kesimi temsil eder. Bir işleme parça, isteğe bağlı olarak işleme parça çağrıldığında belirtilebilecek bir parametre alır.

`TableTemplate` Bileşen:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

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

Şablonlu bileşenleri çoğunlukla genel olarak yazılmalıdır. Örneğin, genel `ListViewTemplate` bileşeni oluşturmak için kullanılabilir `IEnumerable<T>` değerleri. Genel bileşen tanımlamak için `@typeparam` tür parametreleri belirtmek için:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

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

Aşağıdaki örnekte, örnek uygulamadan `ThemeInfo` sınıfı, böylece tüm düğmelerin belirli bir uygulamanın parçası içinde aynı stili paylaşır bileşen hiyerarşisi akış için tema bilgileri belirtir.

*UIThemeClasses/ThemeInfo.cs*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

Üst bileşeni, geçişli değeri bileşenini kullanarak basamaklı bir değer sağlayabilirsiniz. `CascadingValue` Bileşeni bileşen hiyerarşisi bir alt ağacı sarmalar ve o alt ağacı içinde tüm bileşenleri tek bir değer sağlar.

Örneğin, örnek uygulamayı tema bilgileri belirtir (`ThemeInfo`) bir düzen gövdesini oluşturan tüm bileşenlerin basamaklı bir parametre olarak uygulamanın düzenleri `@Body` özelliği. `ButtonClass` bir değeri atanır `btn-success` Düzen bileşende. Bu özelliği üzerinden herhangi bir alt bileşen tüketebileceği `ThemeInfo` basamaklı nesnesi.

`CascadingValuesParametersLayout` Bileşen:

```cshtml
@inherits LayoutComponentBase
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

@code {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

Yapmak için geçişli değerlerini kullanmak, bileşenlerini kullanarak basamaklı parametreleri bildirmek `[CascadingParameter]` özniteliği veya bir dize adı değerine göre:

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

Bir dize adı değeri bağlamayla aynı türde birden fazla basamaklı değerler varsa ve aynı alt ağacı içinde ayrılmaları gerekiyorsa geçerlidir.

Basamaklı değerler türüne göre geçişli parametrelerine bağlı.

Örnek uygulamada `CascadingValuesParametersTheme` bileşeni bağlar `ThemeInfo` basamaklı basamaklı bir parametre değeri. Parametre için bir bileşen tarafından görüntülenen düğmeleri CSS sınıfının ayarlamak için kullanılır.

`CascadingValuesParametersTheme` Bileşen:

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" @onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
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

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

Alt `Tab` bileşenleri olmayan açıkça için parametre olarak geçirilen `TabSet`. Bunun yerine, alt `Tab` bileşenlerdir içeriği alt parçası `TabSet`. Ancak, `TabSet` yine her biri hakkında bilmesi gereken `Tab` bileşen böylece üstbilgileri ve etkin sekmede işleyebilirsiniz. Ek kod gerek kalmadan bu koordinasyon sağlamak `TabSet` bileşen *kendisini basamaklı bir değer sağlayabilirsiniz* , ardından alındığından descendent `Tab` bileşenleri.

`TabSet` Bileşen:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

Bloğun `Tab` bileşenleri yakalama içeren `TabSet` basamaklı bir parametre olarak bu nedenle `Tab` bileşenleri eklemek için kendilerini `TabSet` ve hangi sekmesi etkin olduğu koordinat.

`Tab` Bileşen:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a>Razor şablonları

İşleme parçaları, Razor şablon söz dizimi kullanılarak tanımlanabilir. Razor şablonları UI parçacık tanımlayın ve aşağıdaki biçimi varsayar için bir yol sağlar:

```cshtml
@<tag>...</tag>
```

Aşağıdaki örnek nasıl belirtileceğini göstermektedir `RenderFragment` ve `RenderFragment<T>` değerleri.

`RazorTemplates` Bileşen:

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

## <a name="manual-rendertreebuilder-logic"></a>El ile RenderTreeBuilder mantığı

`Microsoft.AspNetCore.Components.RenderTree` bileşenleri ve bileşenleri el ile oluşturma da dahil olmak üzere öğeleri işlemek için yöntemler sağlar C# kod.

> [!NOTE]
> Kullanım `RenderTreeBuilder` bileşenler oluşturmak için Gelişmiş bir senaryodur. Hatalı biçimlendirilmiş bir bileşen (örneğin, bir kapatılmamış biçimlendirme etiketi) tanımsız davranışlara neden olabilir.

Aşağıdakileri göz önünde bulundurun `PetDetails` el ile başka bir bileşene dönüştürerek yerleşik bileşeni:

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@code
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

Aşağıdaki örnekte, bir döngüde `CreateComponent` yöntemi oluşturur üç `PetDetails` bileşenleri. Çağrılırken `RenderTreeBuilder` bileşenler oluşturmak için yöntemleri (`OpenComponent` ve `AddAttribute`), sıra numaraları olan kaynak kodu satır numaraları. Kod, değil ayrı çağrı çağrılarını ayrı satırlara karşılık gelen sıra numaraları Blazor fark algoritması kullanır. Bir bileşen ile oluştururken `RenderTreeBuilder` yöntemleri, sabit kodlamayın seri numaraları için bağımsız değişkenler. **Sıra numarası oluşturmak için bir hesaplama veya sayaç kullanarak düşük performansa neden olabilir.** Daha fazla bilgi için [sıra numaraları ilişkilendirmek için kod satır numaraları ve değil yürütme sırası](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) bölümü.

`BuiltContent` Bileşen:

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="@RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a>Seri numaraları için kod satır numaraları ve değil yürütme sırası arasında bir ilişki

Blazor `.razor` dosyaları her zaman derlenir. Bu büyük olasılıkla için harika bir avantajı, `.razor` çünkü derleme adımı çalışma zamanında uygulama performansını bilgi eklenmek üzere kullanılabilir.

Bu geliştirmeler anahtar bir örnek içeren *sıra numaraları*. Sıra numaraları, hangi kod ayrı ve sıralı satırlarından çıkışları gelen çalışma zamanı gösterir. Çalışma zamanı, genel ağaç fark algoritması için normalde mümkün olandan çok daha hızlı doğrusal zamanda verimli ağaç farkları oluşturmak için bu bilgileri kullanır.

Aşağıdakileri göz önünde bulundurun basit `.razor` dosyası:

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

Yukarıdaki kod, aşağıdaki gibi bir şey derler:

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

Ne zaman kodu yürütür, ilk kez, `someFlag` olduğu `true`, oluşturucu alır:

| Sequence | Tür      | Veri   |
| :------: | --------- | :----: |
| 0        | Metin düğümü | ilk  |
| 1\.        | Metin düğümü | Saniye |

Bu Imagine `someFlag` olur `false`, ve biçimlendirme yeniden oluşturulur. Bu kez, oluşturucu alır:

| Sequence | Tür       | Veri   |
| :------: | ---------- | :----: |
| 1\.        | Metin düğümü  | Saniye |

Çalışma zamanı bir fark gerçekleştirdiğinde, görür öğe dizisi `0` kaldırıldı, aşağıdaki Önemsiz oluşturmasını sağlayacak şekilde *betiğini Düzenle*:

* İlk metin düğümü kaldırın.

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a>Sıra numaraları programlı olarak oluşturursanız yanlış unsurları

Bunun yerine aşağıdaki ağaç Oluşturucu mantığı işlemek yazdığınız varsayalım:

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

Şimdi ilk çıktı.

| Dizisi | Tür | Veri || :------: | --------- | :--- : | | 0 | Metin düğümü | İlk || 1 | Metin düğümü | İkinci |

Bu sonuç için önceki durum, aynı olduğundan negatif hiçbir sorun yoktur. `someFlag` olan `false` işleme ve çıktısını ikinci olan:

| Sequence | Tür      | Veri   |
| :------: | --------- | ------ |
| 0        | Metin düğümü | Saniye |

Fark algoritması, bu kez görür *iki* değişiklikleri oluşmuş ve algoritma aşağıdaki düzenleme komut dosyası oluşturur:

* İlk metin düğümü değiştirin `Second`.
* İkinci metin düğümü kaldırın.

Sıra numaraları oluşturma tüm hakkında yararlı bilgiler nerede kaybetti `if/else` dallar ve döngüler özgün koda mevcut. Bu bir fark sonuçlarını **iki kez sürece** önceki gibi.

Bu basit bir örnektir. Karmaşık ve iç içe yapılar ve özellikle döngüler daha gerçekçi durumda da, performans maliyeti daha ciddi. Hemen hangi döngü blokları veya dallar eklenen veya kaldırılan tanımlamak yerine, derin bir şekilde işleme ağaçlara recurse ve onu hakkında misinformed çünkü genellikle derleme çok uzun düzenleme betiklerini fark algoritmasına sahip eski ve yeni yapılar birbiriyle ilgilidir.

#### <a name="guidance-and-conclusions"></a>Yönergeler ve sonuçları

* Sıra numaraları dinamik olarak üretilen uygulama performans düşebilir.
* Framework'te derleme zamanında yakalanır sürece gerekli bilgileri mevcut olmadığından kendi sıra numaraları çalışma zamanında otomatik olarak oluşturulamıyor.
* El ile uygulanan uzun bloklarını yazmayın `RenderTreeBuilder` mantığı. Tercih ettiğiniz `.razor` dosyaları ve sıra numaraları ile dağıtılacak derleyici sağlar.
* Sıra numaraları sabittir, fark algoritması yalnızca bir sıra numaraları değeri artırmak gerektirir. İlk değer ve boşluklar önemli değildir. Yasal bir seçenek olan sıra numarası kod satır numarasını kullanın veya sıfırdan başlayın ve olanlara ya da yüz artırmak için (veya herhangi bir tercih edilen aralığı). 
* Ağaç ayırırken diğer UI çerçeveleri bunları kullanmayın Blazor seri numaraları kullanır. Sıra numaraları kullanılır ve Blazor sıra numaraları ile otomatik olarak yazma geliştiriciler için ilgilenen bir derleme adımı avantajlarından fark alma işlemini çok daha hızlı `.razor` dosyaları.
