---
title: ASP.NET Core Razor bileşenleri oluşturma ve kullanma
author: guardrex
description: Veri bağlama, olayları işleme ve bileşen yaşam döngülerini yönetme dahil Razor bileşenleri oluşturmayı ve kullanmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/19/2019
uid: blazor/components
ms.openlocfilehash: 55b40bc640715bf4052fa99ed68f63250b67e8d1
ms.sourcegitcommit: e5a74f882c14eaa0e5639ff082355e130559ba83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168215"
---
# <a name="create-and-use-aspnet-core-razor-components"></a>ASP.NET Core Razor bileşenleri oluşturma ve kullanma

, [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27) tarafından

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Blazor uygulamaları, *bileşenleri*kullanılarak oluşturulmuştur. Bir bileşen, bir sayfa, iletişim veya form gibi bir kullanıcı arabirimi (UI) öbekidir. Bir bileşen, veri eklemek veya UI olaylarına yanıt vermek için gereken HTML işaretlemesini ve işleme mantığını içerir. Bileşenler esnek ve hafif. Bunlar, iç içe geçmiş, yeniden kullanılabilir ve projeler arasında paylaşılabilir.

## <a name="component-classes"></a>Bileşen sınıfları

Bileşenler, C# ve HTML Işaretlemesi kullanılarak [Razor](xref:mvc/views/razor) bileşen dosyalarında ( *. Razor*) uygulanır. Blazor içindeki bir bileşen, bir *Razor bileşeni*olarak adlandırılır.

Bir bileşenin adı, büyük harfle başlamalıdır. Örneğin, *mycoolcomponent. Razor* geçerlidir ve *mycoolcomponent. Razor* geçersizdir.

Dosyalar, `_RazorComponentInclude` MSBuild özelliği kullanılarak Razor bileşen dosyaları olarak tanımlandığı sürece *. cshtml* dosya uzantısı kullanılarak yazılabilir. Örneğin, *Sayfalar* klasörü altındaki tüm *. cshtml* dosyalarının Razor bileşenleri dosyası olarak değerlendirilip değerlendirilmeyeceğini belirten bir uygulama:

```xml
<PropertyGroup>
  <_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
</PropertyGroup>
```

Bir bileşen için Kullanıcı arabirimi HTML kullanılarak tanımlanır. Dinamik işleme mantığı (örneğin, döngüler, koşullar, ifadeler) C# [Razor](xref:mvc/views/razor)adlı gömülü bir sözdizimi kullanılarak eklenir. Bir uygulama derlendiğinde, HTML biçimlendirme ve C# işleme mantığı bir bileşen sınıfına dönüştürülür. Oluşturulan sınıfın adı, dosyanın adıyla eşleşir.

Bileşen sınıfının üyeleri bir `@code` blokta tanımlanır. `@code` Bloğunda, bileşen durumu (özellikler, alanlar) olay işleme yöntemleriyle veya diğer bileşen mantığını tanımlamaya yönelik yöntemlerle belirtilir. Birden çok blok izin verilir. `@code`

> [!NOTE]
> ASP.NET Core 3,0 ' nin önceki önizlemelerinde `@functions` , bloklar Razor bileşenlerinde bulunan `@code` bloklarla aynı amaçla kullanılmıştı. `@functions`bloklar Razor bileşenlerinde çalışmaya devam eder, ancak `@code` blok ASP.NET Core 3,0 Preview 6 veya sonraki bir sürümünde kullanmanızı öneririz.

Bileşen üyeleri, ile C# `@`başlayan ifadeleri kullanarak bileşenin işleme mantığının bir parçası olarak kullanılabilir. Örneğin, bir C# alan, alan adının önüne eklenerek `@` işlenir. Aşağıdaki örnek değerlendirilir ve işler:

* `_headingFontStyle`için CSS özellik değerine `font-style`.
* `_headingText``<h1>` öğenin içeriğine.

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

Bileşen ilk olarak işlendikten sonra, bileşen işleme ağacını olaylara yanıt olarak yeniden oluşturur. Blazor ardından yeni işleme ağacını önceki bir ile karşılaştırır ve tarayıcının Belge Nesne Modeli (DOM) üzerinde herhangi bir değişiklik uygular.

Bileşenler sıradan C# sınıflardır ve bir proje içinde herhangi bir yere yerleştirilebilir. Web sayfalarını üreten bileşenler genellikle *Sayfalar* klasöründe bulunur. Sayfa olmayan bileşenler sıklıkla *paylaşılan* klasöre veya projeye eklenen özel bir klasöre yerleştirilir. Özel bir klasör kullanmak için, özel klasörün ad alanını üst bileşene ya da uygulamanın *_ımports. Razor* dosyasına ekleyin. Örneğin, aşağıdaki ad alanı, uygulamanın kök ad alanı `WebApplication`olduğunda bir bileşenler klasöründeki bileşenleri kullanılabilir yapar:

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a>Bileşenleri Razor Pages ve MVC uygulamalarıyla tümleştirme

Mevcut Razor Pages ve MVC uygulamalarıyla bileşenleri kullanın. Razor bileşenleri kullanmak için mevcut sayfaları veya görünümleri yeniden yazmanız gerekmez. Sayfa veya görünüm işlendiğinde, bileşenler aynı anda önceden işlenir.

Bir sayfadan veya görünümden bir bileşeni işlemek için `RenderComponentAsync<TComponent>` HTML yardımcı yöntemini kullanın:

```cshtml
<div id="MyComponent">
    @(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
</div>
```

Sayfalar ve görünümler bileşenleri kullanırken, listesiyse doğru değildir. Bileşenler, kısmi görünümler ve bölümler gibi görüntüleme ve sayfaya özgü senaryolar kullanamaz. Bir bileşende kısmi görünümden mantığı kullanmak için kısmi görünüm mantığını bir bileşene ayırın.

Bileşenlerin nasıl işlendiği ve bileşen durumunun Blazor sunucu uygulamalarında nasıl yönetildiği hakkında daha fazla bilgi için <xref:blazor/hosting-models> makalesine bakın.

## <a name="use-components"></a>Bileşenleri kullanma

Bileşenler, HTML öğesi söz dizimini kullanarak bildirerek diğer bileşenleri içerebilir. Bir bileşeni kullanmak için biçimlendirme, etiket adının bileşen türü olduğu bir HTML etiketi gibi görünür.

Öznitelik bağlama büyük/küçük harfe duyarlıdır. Örneğin, `@bind` geçerlidir ve `@Bind` geçersizdir.

*Index. Razor* dosyasında aşağıdaki biçimlendirme bir `HeadingComponent` örneği işler:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

*Bileşenler/HeadingComponent. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

Bir bileşen bir bileşen adıyla eşleşmeyen büyük harfle yazılmış bir HTML öğesi içeriyorsa, öğenin beklenmeyen bir adı olduğunu gösteren bir uyarı yayınlanır. Bileşenin ad `@using` alanı için bir deyimin eklenmesi, bileşenin kullanılabilir olmasını sağlar ve bu da uyarıyı kaldırır.

## <a name="component-parameters"></a>Bileşen parametreleri

Bileşenler, bileşen sınıfında `[Parameter]` özniteliği ile ortak özellikler kullanılarak tanımlanan *bileşen parametrelerine*sahip olabilir. Biçimlendirme içindeki bir bileşenin bağımsız değişkenlerini belirtmek için öznitelikleri kullanın.

*Bileşenler/ChildComponent. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

Aşağıdaki örnekte `ParentComponent` , öğesinin `Title` özelliğinin değerini ayarlar. `ChildComponent`

*Pages/ParentComponent. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a>Alt içerik

Bileşenler, başka bir bileşenin içeriğini ayarlayabilir. Atama bileşeni, alıcı bileşeni belirten Etiketler arasında içerik sağlar.

Aşağıdaki örnekte `ChildComponent` , öğesinin işlemek için bir kullanıcı arabirimi segmentini temsil `RenderFragment`eden öğesini temsil eden bir `ChildContent` özelliği vardır. Değeri `ChildContent` , bileşenin, içeriğin işlenmesi gereken biçimlendirmesinde konumlandırılır. Değeri `ChildContent` , ana bileşenden alınır ve önyükleme `panel-body`paneli içinde işlenir.

*Bileşenler/ChildComponent. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> `RenderFragment` İçeriği alan özelliğin kural tarafından adlandırılması `ChildContent` gerekir.

Aşağıdakiler `ParentComponent` `<ChildComponent>` , `ChildComponent` içeriği etiketlerin içine yerleştirerek işleme için içerik sağlayabilir.

*Pages/ParentComponent. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a>Öznitelik döndürme ve rastgele parametreler

Bileşenler, bileşen tarafından tanımlanan parametrelere ek olarak ek öznitelikler yakalayabilir ve işleyebilir. Ek öznitelikler bir sözlükte yakalanıp *, daha sonra* bileşen [@attributes](xref:mvc/views/razor#attributes) Razor yönergesi kullanılarak işlendiğinde bir öğe üzerine bırakılabilir. Bu senaryo, çeşitli özelleştirmeleri destekleyen bir işaretleme öğesi üreten bir bileşen tanımlarken yararlıdır. Örneğin, çok sayıda parametreyi destekleyen bir `<input>` için öznitelikleri ayrı olarak tanımlamak sıkıcı olabilir.

Aşağıdaki `<input>` örnekte, ilk öğesi (`id="useIndividualParams"`) bağımsız bileşen parametrelerini kullanır, ancak ikinci `<input>` öğe (`id="useAttributesDict"`) öznitelik splatesini kullanır:

```cshtml
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

Parametrenin türü dize anahtarlarıyla gerçekleştirmelidir `IEnumerable<KeyValuePair<string, object>>` . Bu senaryoda ayrıca bir seçenek de vardır.`IReadOnlyDictionary<string, object>`

Her iki `<input>` yaklaşımın de kullanıldığı işlenen öğeler aynıdır:

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

Rastgele öznitelikleri kabul etmek için `[Parameter]` `CaptureUnmatchedValues` özelliği olarak `true`ayarlanmış özniteliği kullanarak bir bileşen parametresi tanımlayın:

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

`CaptureUnmatchedValues` Üzerindeki`[Parameter]` özelliği, parametresinin diğer bir parametreyle eşleşmeyen tüm özniteliklerle eşleşmesini sağlar. Bir bileşen yalnızca ile `CaptureUnmatchedValues`tek bir parametre tanımlayabilir. İle `CaptureUnmatchedValues` kullanılan özellik türü dize anahtarlarıyla `Dictionary<string, object>` atanabilir olmalıdır. `IEnumerable<KeyValuePair<string, object>>`Ayrıca `IReadOnlyDictionary<string, object>` , Bu senaryodaki seçenekler de vardır.

## <a name="data-binding"></a>Veri bağlama

Hem bileşenlere hem de Dom öğelerine veri bağlama, [@bind](xref:mvc/views/razor#bind) özniteliğiyle birlikte gerçekleştirilir. Aşağıdaki örnek, `_italicsCheck` alanı onay kutusunun işaretli durumuna bağlar:

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

Onay kutusu seçildiğinde ve kaldırıldığında, özelliğin değeri sırasıyla `true` ve `false`olarak güncelleştirilir.

Onay kutusu kullanıcı arabiriminde, özelliğin değerini değiştirme yanıt olarak değil, yalnızca bileşen işlendiğinde güncelleştirilir. Bileşenler olay işleyicisi kodu yürütüldükten sonra kendilerini oluşturduğundan, özellik güncelleştirmeleri genellikle kullanıcı arabirimine hemen yansıtılır.

`CurrentValue` Özelliği `@bind` (`<input @bind="CurrentValue" />`) ile kullanmak, temelde aşağıdakilere eşdeğerdir:

```cshtml
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

Bileşen işlendiğinde, `value` giriş öğesi `CurrentValue` özelliğinden gelir. Kullanıcı metin kutusunda yazdığında, `onchange` olay tetiklenir `CurrentValue` ve özellik değiştirilen değere ayarlanır. Tür dönüştürmelerinde birkaç durum olduğu için, gerçekte kod oluşturma biraz `@bind` daha karmaşıktır. İlke ' de `@bind` , bir ifadenin geçerli değerini bir `value` özniteliğiyle ilişkilendirir ve kayıtlı işleyiciyi kullanarak değişiklikleri işler.

`onchange` Sözdizimi ile [@bind-value](xref:mvc/views/razor#bind) `event` [@bind-value:event](xref:mvc/views/razor#bind)olayları işlemenin yanı sıra, bir özellik veya alan, parametresi () olan bir öznitelik belirtilerek diğer olaylar kullanılarak da bağlanabilir. `@bind` Aşağıdaki örnek, `oninput` olay için `CurrentValue` özelliği bağlar:

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />
```

' `onchange`In aksine, öğe odağı kaybettiğinde harekete geçirilir, `oninput` metin kutusunun değeri değiştiğinde harekete geçirilir.

**Ayrıştırılamayan değerler**

Bir Kullanıcı, bir veri sınırlama öğesine ayrıştırılamayan bir değer sağlıyorsa, bağlama olayı tetiklendiğinde, çözümlenemeyen değer otomatik olarak önceki değerine döndürülür.

Aşağıdaki senaryoyu göz önünde bulundurun:

* Bir `<input>` öğesi, başlangıç değeri `123`olan `int` bir türe bağlanır:

  ```cshtml
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* Kullanıcı, öğesinin değerini sayfada olarak `123.45` güncelleştirir ve öğe odağını değiştirir.

Önceki senaryoda, öğenin değeri öğesine `123`geri döndürülür. Değeri `123.45` özgün`123`değeri yararına reddedildiğinde, Kullanıcı değerinin kabul edilmediğini anlamıştır.

Varsayılan olarak, bağlama öğenin `onchange` olayına (`@bind="{PROPERTY OR FIELD}"`) uygulanır. Farklı `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` bir olay ayarlamak için kullanın. `oninput` Event(`@bind-value:event="oninput"`) için yeniden sürüm, ayrıştırılamayan bir değer sunan herhangi bir tuş vuruşu sonrasında oluşur. `oninput` Olayı, ilişkili bir `int`tür ile hedeflerken, bir kullanıcının bir `.` karakter yazmasının engellenmiş olması engellenir. Bir `.` karakter hemen kaldırılır, bu nedenle Kullanıcı yalnızca tam sayılara izin verilen anında geri bildirim alır. `oninput` Olay üzerindeki değerin geri döndürülmesi ideal değil (örneğin, kullanıcının ayrıştırılamayan `<input>` bir değeri temizlemeye izin verilmesi gerektiği durumlarda). Alternatifler şunlardır:

* `oninput` Olayı kullanmayın. Öğe odağı kaybederene kadar`@bind="{PROPERTY OR FIELD}"`geçersiz bir değer geri döndürülmediğinde, varsayılan `onchange` olayı () kullanın.
* `int?` Veya`string`gibi null yapılabilir bir türe bağlayın ve geçersiz girdileri işlemek için özel mantık sağlayın.
* `InputNumber` [](xref:blazor/forms-validation) Veya`InputDate`gibi bir form doğrulama bileşeni kullanın. Form doğrulama bileşenlerinde geçersiz girişleri yönetmek için yerleşik destek vardır. Form doğrulama bileşenleri:
  * Kullanıcının geçersiz giriş sağlamasına ve ilişkili `EditContext`doğrulama hatalarını almasına izin verin.
  * Kullanıcı ek WebForm verisi girmeye uğramadan doğrulama hatalarını Kullanıcı ARABIRIMINDE görüntüleyin.

**Genelleştirme**

`@bind`değerler görüntülenmek üzere biçimlendirilir ve geçerli kültürün kuralları kullanılarak ayrıştırılır.

Geçerli kültüre <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> özelliğinden erişilebilir.

Aşağıdaki alan türleri için [CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) kullanılır (`<input type="{TYPE}" />`):

* `date`
* `number`

Yukarıdaki alan türleri:

* , Uygun tarayıcı tabanlı biçimlendirme kuralları kullanılarak görüntülenir.
* Serbest biçimli metin içeremez.
* Tarayıcının uygulamasına göre Kullanıcı etkileşimi özellikleri sağlar.

Aşağıdaki alan türleri özel biçimlendirme gereksinimlerine sahiptir ve şu anda tüm büyük tarayıcılarda desteklenmediği için Blazor tarafından desteklenmemektedir:

* `datetime-local`
* `month`
* `week`

`@bind`, `@bind:culture` bir değeri ayrıştırmak ve biçimlendirmek <xref:System.Globalization.CultureInfo?displayProperty=fullName> için bir parametresini destekler. `date` Ve`number` alan türleri kullanılırken bir kültürün belirtilmesi önerilmez. `date`ve `number` gerekli kültürü sağlayan yerleşik Blazor desteğine sahiptir.

Kullanıcının kültürünü ayarlama hakkında daha fazla bilgi için [Yerelleştirme](#localization) bölümüne bakın.

**Biçim dizeleri**

Veri bağlama, kullanılarak <xref:System.DateTime> [@bind:format](xref:mvc/views/razor#bind)biçim dizeleriyle birlikte kullanılabilir. Para birimi veya sayı biçimleri gibi diğer biçim ifadeleri şu anda kullanılamaz.

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

Yukarıdaki kodda, `<input>` öğesinin alan türü (`type`) varsayılan olarak `text`olur. `@bind:format`, aşağıdaki .NET türlerini bağlamak için desteklenir:

* <xref:System.DateTime?displayProperty=fullName>
* <xref:System.DateTime?displayProperty=fullName>?
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <xref:System.DateTimeOffset?displayProperty=fullName>?

`@bind:format` Özniteliği öğesi`<input>` için uygulanacak `value` tarih biçimini belirtir. Biçim Ayrıca bir `onchange` olay gerçekleştiğinde değeri ayrıştırmak için de kullanılır.

Blazor 'in tarihleri biçimlendirmek için `date` yerleşik desteği olduğundan, alan türü için bir biçim belirtilmesi önerilmez.

**Bileşen parametreleri**

Bağlama bileşen parametrelerini tanır, burada `@bind-{property}` bir özellik değeri bileşenler arasında bağlanabilir.

Aşağıdaki alt bileşende (`ChildComponent`) bir `Year` bileşen parametresi ve `YearChanged` geri çağırması vardır:

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

`EventCallback<T>`, [Eventcallback](#eventcallback) bölümünde açıklanmaktadır.

Aşağıdaki üst bileşen öğesini kullanır `ChildComponent` ve üst `Year` öğeden `ParentYear` parametreyi alt bileşen üzerindeki parametresine bağlar:

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

Yüklemesi aşağıdaki biçimlendirmeyi üretir: `ParentComponent`

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

`ParentYear` Özelliğin değeri, `ParentComponent` içindekidüğme`ChildComponent` seçilerek değiştirilirse, öğesinin özelliğigüncellenir.`Year` Yeni değeri `Year` , `ParentComponent` yeniden kullanıldığında kullanıcı arabiriminde işlenir:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

Parametrenin türüyle `YearChanged` `Year` eşleşenbiryardımcıolayı`Year` olduğundan parametre bağlanabilir.

Kurala göre, `<ChildComponent @bind-Year="ParentYear" />` temelde yazmaya eşdeğerdir:

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

Genel olarak, bir özellik öznitelik kullanılarak `@bind-property:event` karşılık gelen bir olay işleyicisine bağlanabilir. Örneğin, özelliği `MyProp` aşağıdaki iki öznitelik `MyEventHandler` kullanılarak bağlanabilir:

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a>Olay işleme

Razor bileşenleri olay işleme özellikleri sağlar. Temsilci türü belirtilmiş bir değer ile `on{event}` adlı bir HTML öğesi `onclick` özniteliği `onsubmit`için (örneğin, ve), Razor bileşenleri özniteliğin değerini bir olay işleyicisi olarak değerlendirir. Özniteliğin adı her zaman [ @on{Event}](xref:mvc/views/razor#onevent)olarak biçimlendirilir.

Aşağıdaki kod, Kullanıcı arabiriminde `UpdateHeading` düğme seçildiğinde yöntemini çağırır:

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

Aşağıdaki kod, Kullanıcı arabiriminde `CheckChanged` onay kutusu değiştirildiğinde yöntemini çağırır:

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

Olay işleyicileri Ayrıca zaman uyumsuz olabilir ve döndürebilir <xref:System.Threading.Tasks.Task>. El ile çağırmanız `StateHasChanged()`gerekmez. Özel durumlar oluştuğunda günlüğe kaydedilir.

Aşağıdaki örnekte, `UpdateHeading` düğme seçildiğinde zaman uyumsuz olarak çağrılır:

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a>Olay bağımsız değişken türleri

Bazı olaylar için olay bağımsız değişkeni türlerine izin verilir. Bu olay türlerinden birine erişim gerekmiyorsa, yöntem çağrısında gerekli değildir.

Desteklenen [EventArgs](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web) aşağıdaki tabloda gösterilmiştir.

| Olay | örneği |
| ----- | ----- |
| Pano        | `ClipboardEventArgs` |
| Sürükle             | `DragEventArgs`&ndash; veöğe`DataTransferItem` verilerini sürüklemiş tutun. `DataTransfer` |
| Hata            | `ErrorEventArgs` |
| Çı            | `FocusEventArgs`&ndash; İçin`relatedTarget`destek içermez. |
| `<input>`değişebilir | `ChangeEventArgs` |
| Klavye         | `KeyboardEventArgs` |
| Tığında            | `MouseEventArgs` |
| Fare işaretçisi    | `PointerEventArgs` |
| Fare tekerleği      | `WheelEventArgs` |
| İlerleme durumu         | `ProgressEventArgs` |
| Dokunma            | `TouchEventArgs`&ndash; dokunarakduyarlıbircihazdakitekbir`TouchPoint` iletişim noktasını temsil eder. |

Önceki tablodaki olayların özellikleri ve olay işleme davranışı hakkında bilgi için bkz. [başvuru kaynağında EventArgs sınıfları (ASPNET/AspNetCore Release/3.0-preview9 Branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web).

### <a name="lambda-expressions"></a>Lambda ifadeleri

Lambda ifadeleri de kullanılabilir:

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

Genellikle, bir dizi öğe üzerinde yineleme yaparken olduğu gibi ek değerlerin üzerinde kapatılabilir. Aşağıdaki örnek, her biri `UpdateHeading` Kullanıcı arabiriminde seçildiğinde bir olay bağımsız değişkeni (`MouseEventArgs`) ve düğme numarası (`buttonNumber`) geçiren üç düğme oluşturur:

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

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> Döngü değişkenini (`i`) bir `for` döngüde doğrudan bir lambda ifadesinde kullanmayın. Aksi halde, tüm lambda ifadeleri tarafından değeri tüm Lambdalar aynı olmasına neden olan `i`değişken aynı değişken kullanılır. Her zaman değerini yerel bir değişkende (`buttonNumber` önceki örnekte) yakalayın ve sonra kullanın.

### <a name="eventcallback"></a>EventCallback

İç içe bileşenler içeren yaygın bir senaryo, alt bileşen olayı&mdash;olduğunda bir üst bileşenin yöntemini, `onclick` örneğin bir olay gerçekleştiğinde bir olay oluştuğunda çalıştırmak için gereklidir. Olayları bileşenler genelinde göstermek için bir `EventCallback`kullanın. Bir üst bileşen bir alt bileşene `EventCallback`geri çağırma yöntemi atayabilir.

Örnek `ChildComponent` uygulamada, bir `onclick` düğmenin işleyicisinin örnek `ParentComponent`tarafından bir `EventCallback` temsilci almak üzere nasıl ayarlandığı gösterilmektedir. ,, Bir çevre `MouseEventArgs`cihazından bir `onclick` olay için uygun olan ile öğesine yazılır: `EventCallback`

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

, `ParentComponent` Alt`ShowMessage` öğenin yöntemine göre ayarlar: `EventCallback<T>`

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

Düğme ' de `ChildComponent`seçildiğinde:

* `ParentComponent` Öğesinin`ShowMessage` yöntemi çağrılır. `messageText`güncelleştirilir ve içinde `ParentComponent`görüntülenir.
* Geri çağırma yönteminde `StateHasChanged` (`ShowMessage`) bir çağrısı gerekli değildir. `StateHasChanged`alt olaylar, alt öğe içinde yürütülen `ParentComponent`olay işleyicilerinde bileşen rerendering tetiklenmesi için otomatik olarak çağrılır.

`EventCallback`ve `EventCallback<T>` zaman uyumsuz temsilcilere izin verir. `EventCallback<T>`kesin bir şekilde türdedir ve belirli bir bağımsız değişken türü gerektirir. `EventCallback`zayıf ve bağımsız değişken türüne izin veriyor.

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

Bir `EventCallback` veya `EventCallback<T>` ile <xref:System.Threading.Tasks.Task>çağırın ve şunu bekler: `InvokeAsync`

```csharp
await callback.InvokeAsync(arg);
```

Olay `EventCallback` işleme `EventCallback<T>` ve bağlama bileşeni parametreleri için ve kullanın.

Kesin olarak belirlenmiş `EventCallback<T>` `EventCallback`türü tercih edin. `EventCallback<T>`bileşenin kullanıcılarına daha iyi hata geri bildirimi sağlar. Diğer UI olay işleyicileriyle benzer şekilde, olay parametresini belirtmek isteğe bağlıdır. Geri `EventCallback` çağırmaya hiçbir değer geçirilmemişse kullanın.

## <a name="chained-bind"></a>Zincirleme bağlama

Yaygın bir senaryo, bir veri bağlama parametresini bileşen çıkışında bir sayfa öğesine zincirlemesini sağlar. Birden çok bağlama düzeyi aynı anda gerçekleştiğinden, bu senaryoya *zincirleme bağlama* denir.

Bir zincir bağlama, sayfanın öğesinde sözdizimi `@bind` ile uygulanamaz. Olay işleyicisi ve değeri ayrı olarak belirtilmelidir. Ancak, bir üst bileşen, bir sözdizimi `@bind` bileşenin parametresiyle birlikte kullanabilir.

Aşağıdaki `PasswordField` bileşen (*passwordfield. Razor*):

* Bir öğenin değerini bir `Password` özelliğe ayarlar. `<input>`
* `Password` Özellik değişikliklerini bir [eventcallback](#eventcallback)ile üst bileşene gösterir.

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

`PasswordField` Bileşen başka bir bileşende kullanılır:

```cshtml
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

Önceki örnekteki parolada denetim veya tuzak hataları gerçekleştirmek için:

* İçin `Password` bir yedekleme alanı oluşturun (`password` aşağıdaki örnek kodda).
* `Password` Ayarlayıcıdaki denetimleri veya yakalama hatalarını gerçekleştirin.

Aşağıdaki örnek, parolanın değerinde bir boşluk kullanılmışsa kullanıcıya anında geri bildirim sağlar:

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@validationMessage</span>

@code {
    private bool showPassword;
    private string password;
    private string validationMessage;

    [Parameter]
    public string Password
    {
        get { return password ?? string.Empty; }
        set
        {
            if (password != value)
            {
                if (value.Contains(' '))
                {
                    validationMessage = "Spaces not allowed!";
                }
                else
                {
                    password = value;
                    validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

## <a name="capture-references-to-components"></a>Bileşenlere başvuruları yakala

Bileşen başvuruları, bir bileşen örneğine başvurmak için bir yol sağlar; böylece, veya `Show` `Reset`gibi komutları bu örneğe verebilirsiniz. Bir bileşen başvurusunu yakalamak için:

* Alt bileşene [@ref](xref:mvc/views/razor#ref) bir öznitelik ekleyin.
* Alt bileşenle aynı türde bir alan tanımlayın.

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

Bileşen işlendiğinde, `loginDialog` alan `MyLoginDialog` alt bileşen örneğiyle doldurulur. Daha sonra bileşen örneğinde .NET yöntemlerini çağırabilirsiniz.

> [!IMPORTANT]
> Değişken yalnızca bileşen işlendikten sonra ve çıktısı `MyLoginDialog` öğesi içerdiğinde doldurulur. `loginDialog` Bu noktaya kadar başvurulmasına hiçbir şey yok. Bileşen işlemesini tamamladıktan sonra bileşen başvurularını işlemek için, `OnAfterRenderAsync` veya `OnAfterRender` yöntemlerini kullanın.

Bileşen başvurularını yakalama, [öğe başvurularını yakalamak](xref:blazor/javascript-interop#capture-references-to-elements)için benzer bir sözdizimi kullanın, bir [JavaScript birlikte çalışma](xref:blazor/javascript-interop) özelliği değildir. Bileşen başvuruları yalnızca .net kodunda kullanıldıkları JavaScript&mdash;koduna aktarılmaz.

> [!NOTE]
> Alt bileşenlerin durumunu bulunmamalıdır için bileşen **başvurularını kullanmayın.** Bunun yerine, alt bileşenlere veri geçirmek için normal bildirime dayalı parametreleri kullanın. Normal bildirime dayalı parametrelerin kullanımı, otomatik olarak doğru zamanların yeniden yönlendirmesi için alt bileşenlerde oluşur.

## <a name="invoke-component-methods-externally-to-update-state"></a>Durumu güncelleştirmek için bileşen yöntemlerini dışarıdan çağır

Blazor, yürütmenin `SynchronizationContext` tek bir mantıksal iş parçacığını zorlamak için bir kullanır. Bir bileşenin yaşam döngüsü yöntemleri ve Blazor tarafından oluşturulan tüm olay geri çağırmaları bunun `SynchronizationContext`üzerinde yürütülür. Bir bileşenin bir zamanlayıcı veya diğer bildirimler gibi dış bir olaya göre güncellenmesi gerekir, `InvokeAsync` `SynchronizationContext`Blazor ' a gönderilecek yöntemi kullanın.

Örneğin, güncelleştirilmiş durumdaki herhangi bir dinleme bileşenine bildirimde bulunan bir *bildirim hizmeti* düşünün:

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

Bir bileşeni güncelleştirmek `NotifierService` için öğesinin kullanımı:

```cshtml
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

Önceki örnekte, `NotifierService` `OnNotify` bileşen yöntemini Blazor 'in `SynchronizationContext`dışında çağırır. `InvokeAsync`doğru bağlama geçmek ve bir işlemeyi kuyruğa almak için kullanılır.

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a>Öğe \@ve bileşenlerin korunmasını denetlemek için anahtar kullanın

Bir öğe veya bileşen listesi işlenirken ve öğeler ya da bileşenler daha sonra değiştiğinde, Blazor 'in yayılma algoritması, önceki öğelerin veya bileşenlerin ne zaman tutulacağına ve model nesnelerinin bunlara nasıl eşleneceğine karar vermelidir. Normalde, bu işlem otomatiktir ve yoksayılabilir, ancak işlemi denetlemek isteyebileceğiniz durumlar vardır.

Aşağıdaki örnek göz önünde bulundurun:

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

`People` Koleksiyonun içeriği, ekli, silinmiş veya yeniden sıralanmış girdilerle değişebilir. Bileşen yeniden oluşturulduğunda, `<DetailsEditor>` bileşen farklı `Details` parametre değerleri almak için değişebilir. Bu, beklenenden daha karmaşık rerendering oluşmasına neden olabilir. Bazı durumlarda rerendering, kayıp öğe odağı gibi görünür davranış farklılıklarına yol açabilir.

Eşleme işlemi, `@key` Directive özniteliğiyle denetlenebilir. `@key`, anahtar değerine göre öğelerin veya bileşenlerin korunmasını güvence altına almak için dağıtılmış algoritmaya neden olur:

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

Koleksiyon değiştiğinde, yayılma algoritması örnekler ve `person` örnekler arasındaki `<DetailsEditor>` ilişkilendirmeyi korur: `People`

* Bir `Person` , `People` listeden silinirse, yalnızca ilgili `<DetailsEditor>` örnek kullanıcı arabiriminden kaldırılır. Diğer örnekler değişmeden bırakılır.
* Listedeki bir `Person` konuma eklenirse, ilgili konuma bir yeni `<DetailsEditor>` örnek eklenir. Diğer örnekler değişmeden bırakılır.
* Girişler `Person` yeniden Sıralansa, ilgili `<DetailsEditor>` örnekler Kullanıcı arabiriminde korunur ve yeniden sıralanır.

Bazı senaryolarda, kullanımı `@key` rerendering karmaşıklığını en aza indirir ve odak konumu gibi Dom 'ın durum bilgisi olan kısımlarıyla ilgili olası sorunları önler.

> [!IMPORTANT]
> Anahtarlar her kapsayıcı öğesi veya bileşeni için yereldir. Anahtarlar belge genelinde küresel olarak karşılaştırılmaz.

### <a name="when-to-use-key"></a>Anahtar ne zaman \@kullanılır?

Genellikle, bir liste işlendiğinde (örneğin `@key` , bir `@foreach` blokta) ve tanımlamak için uygun bir değer varsa, `@key`bu işlem kullanım açısından mantıklı olur.

Bir nesne değiştiğinde Blazor `@key` 'in bir öğeyi veya bileşen alt ağacını koruma altına almasını engellemek için de kullanabilirsiniz:

```cshtml
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

Değişiklik `@currentPerson` olursa `@key` , öznitelik yönergesi Blazor tüm `<div>` alt öğelerini atmayı ve yeni öğeler ve bileşenlerle Kullanıcı arabiriminde alt ağacı yeniden oluşturmayı zorlar. Değişiklik sırasında `@currentPerson` hiçbir Kullanıcı arabirimi durumunun korunmayacağını garanti etmeniz gerekirse bu yararlı olabilir.

### <a name="when-not-to-use-key"></a>\@Anahtar ne zaman kullanılmaz?

İle `@key`yayılma yaparken bir performans maliyeti vardır. Performans maliyeti büyük değildir, ancak yalnızca öğenin veya `@key` bileşen koruma kurallarının denetlenmesi uygulamanın avantajına göre belirleyin.

`@key` Kullanılmasa bile, Blazor alt öğe ve bileşen örneklerini mümkün olduğunca korur. Kullanmanın `@key` tek avantajı model örneklerinin, eşlemeyi seçme algoritması yerine, korunan bileşen örneklerine *nasıl* eşlendiğine ilişkin denetimdir.

### <a name="what-values-to-use-for-key"></a>Anahtar için \@kullanılacak değerler

Genellikle, için `@key`aşağıdaki değer türlerinden birini sağlamak mantıklı olur:

* Model nesne örnekleri (örneğin, `Person` önceki örnekte olduğu gibi). Bu, nesne başvurusu eşitliğine göre koruma sağlar.
* Benzersiz tanımlayıcılar (örneğin, veya `int` `Guid`türündeki `string`birincil anahtar değerleri).

Bu değerlerin çakışmayın için `@key` kullanıldığından emin olun. Aynı üst öğe içinde çakışan değerler algılanırsa, eski öğeleri veya bileşenleri yeni öğe veya bileşenlere kesin bir şekilde eşlemediğinden Blazor bir özel durum oluşturur. Yalnızca nesne örnekleri veya birincil anahtar değerleri gibi farklı değerleri kullanın.

## <a name="lifecycle-methods"></a>Yaşam döngüsü yöntemleri

`OnInitializedAsync`ve `OnInitialized` bileşeni başlatmak için kodu yürütün. Zaman uyumsuz bir işlem gerçekleştirmek için, `OnInitializedAsync` `await` ve işlem üzerinde anahtar sözcüğünü kullanın:

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

Zaman uyumlu bir işlem için şunu `OnInitialized`kullanın:

```csharp
protected override void OnInitialized()
{
    ...
}
```

`OnParametersSetAsync`ve `OnParametersSet` bir bileşen üst öğeden parametreler aldığında ve değerler özelliklerine atandığında çağrılır. Bu yöntemler bileşen başlatıldıktan sonra ve bileşen her işlendiğinde yürütülür:

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

`OnAfterRenderAsync`ve `OnAfterRender` bir bileşen işlemeyi tamamladıktan sonra çağrılır. Öğe ve bileşen başvuruları bu noktada doldurulur. İşlenmiş DOM öğelerinde çalışan üçüncü taraf JavaScript kitaplıklarını etkinleştirme gibi, işlenmiş içeriği kullanarak ek başlatma adımları gerçekleştirmek için bu aşamayı kullanın.

`OnAfterRender`*sunucuda prerendering çağrıldığında çağrılmaz.*

Ve `firstRender` için`OnAfterRender` parametresi: `OnAfterRenderAsync`

* `true` Bileşen örneği ilk kez çağrıldığında ayarlanır.
* Başlatma işinin yalnızca bir kez gerçekleştirildiğinden emin olur.

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

### <a name="handle-incomplete-async-actions-at-render"></a>İşleme sırasında tamamlanmamış zaman uyumsuz eylemleri işle

Yaşam döngüsü olaylarında gerçekleştirilen zaman uyumsuz eylemler, bileşen işlenmeden önce tamamlanmamış olabilir. Yaşam döngüsü yöntemi `null` yürütülürken nesneler, verilerle tamamen doldurulmuş olabilir. Nesnelerin başlatıldığını onaylamak için işleme mantığı sağlayın. Nesneler olduğunda `null`yer tutucu Kullanıcı arabirimi öğelerini (örneğin, bir yükleme iletisi) işleme.

Blazor şablonlarının `OnInitializedAsync`bileşeninde, Asychronously tahmin verileri al (`forecasts`) için geçersiz kılınır. `FetchData` Ne zaman olduğunda `forecasts` ,kullanıcıyabiryüklemeiletisigörüntülenir.`null` İşlem tamamlandıktan sonra, bileşen güncelleştirilmiş duruma `Task` `OnInitializedAsync` geri döner.

*Pages/FetchData. Razor*:

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a>Parametreler ayarlanmadan önce kodu yürütün

`SetParameters`Parametreler ayarlanmadan önce kodu yürütmek için geçersiz kılınabilir:

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

Çağrılmadıysa `base.SetParameters` , özel kod gelen parametreler değerini gerekli herhangi bir şekilde yorumlayabilir. Örneğin, gelen parametrelerin, sınıftaki özelliklere atanması gerekmez.

### <a name="suppress-refreshing-of-the-ui"></a>Kullanıcı arabiriminin yenilenmesini gösterme

`ShouldRender`Kullanıcı arabiriminin yenilenmesini gizlemek için geçersiz kılınabilir. Uygulama döndürürse `true`, Kullanıcı arabirimi yenilenir. `ShouldRender` Geçersiz kılınsa bile, bileşen her zaman ilk olarak işlenir.

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a>IDisposable ile bileşen atma

Bir bileşen uygularsa <xref:System.IDisposable>, bileşen kullanıcı arabiriminden kaldırıldığında [Dispose yöntemi](/dotnet/standard/garbage-collection/implementing-dispose) çağırılır. Aşağıdaki bileşen `@implements IDisposable` `Dispose` ve yöntemini kullanır:

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

Blazor ' de yönlendirme, uygulamadaki her erişilebilir bileşene bir rota şablonu sunarak elde edilir.

Bir `@page` yönergeyle bir Razor dosyası derlendiğinde, oluşturulan sınıfa yol şablonu <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> belirtilerek verilir. Çalışma zamanında, yönlendirici bileşen sınıflarını bir `RouteAttribute` ile arar ve hangi bileşenin istenen URL ile eşleşen bir rota şablonuna sahip olduğunu işler.

Birden çok yol şablonu, bir bileşene uygulanabilir. Aşağıdaki bileşen ve `/BlazorRoute` `/DifferentBlazorRoute`için isteklere yanıt verir:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a>Rota parametreleri

Bileşenler, `@page` yönergede belirtilen yol şablonundan rota parametreleri alabilir. Yönlendirici, karşılık gelen bileşen parametrelerini doldurmak için yol parametrelerini kullanır.

*Rota parametresi bileşeni*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

İsteğe bağlı parametreler desteklenmez, bu nedenle `@page` Yukarıdaki örnekte iki yönergeler uygulanır. İlki, bir parametre olmadan bileşene gezinmesine izin verir. İkinci `@page` yönerge, `{text}` yol parametresini alır ve değeri `Text` özelliğine atar.

## <a name="base-class-inheritance-for-a-code-behind-experience"></a>"Arka plan kod" deneyimi için temel sınıf devralma

Bileşen dosyaları aynı dosyada HTML biçimlendirme C# ve işleme kodu karışımını. Yönergesi `@inherits` , bileşen işaretlemesini işleme kodundan ayıran "arka plan kod" deneyimiyle Blazor uygulamalar sağlamak için kullanılabilir.

[Örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) , bileşenin özelliklerini ve yöntemlerini sağlamak için bir bileşenin temel sınıfı `BlazorRocksBase`nasıl devralmasını gösterir.

*Pages/BlazorRocks. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

*BlazorRocksBase.cs*:

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

Taban sınıfın türevi `ComponentBase`olması gerekir.

## <a name="import-components"></a>Bileşenleri içeri aktar

Razor ile yazılmış bir bileşenin ad alanı temel alınarak belirlenir:

* Projenin `RootNamespace`.
* Proje kökünden bileşene olan yol. Örneğin, `ComponentsSample/Pages/Index.razor` ad alanı `ComponentsSample.Pages`. Bileşenler ad C# bağlama kurallarını izler. *Index. Razor*söz konusu olduğunda, aynı klasördeki, *sayfalardaki*ve üst klasördeki tüm bileşenler ( *componentssample*) kapsamdadır.

Farklı bir ad alanında tanımlanan bileşenler, Razor 'in [ \@using](xref:mvc/views/razor#using) yönergesi kullanılarak kapsama getirilebilir.

Klasöründe `NavMenu.razor` `Index.razor` `@using` başka bir bileşen varsa, bileşeni aşağıdaki ifadesiyle birlikte kullanılabilir: `ComponentsSample/Shared/`

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

Bileşenlere Ayrıca kendi tam adları kullanılarak başvurulabilir, bu da [ \@using](xref:mvc/views/razor#using) yönergesinin gereksinimini ortadan kaldırır:

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> `global::` Nitelendirme desteklenmiyor.
>
> Diğer ad `using` deyimleri (örneğin, `@using Foo = Bar`) ile bileşenleri içeri aktarma desteklenmiyor.
>
> Kısmen nitelenmiş adlar desteklenmez. Örneğin, ile `@using ComponentsSample` `NavMenu.razor` eklemevebaşvurudesteklenmez`<Shared.NavMenu></Shared.NavMenu>` .

## <a name="conditional-html-element-attributes"></a>Koşullu HTML öğesi öznitelikleri

HTML öğesi öznitelikleri, .NET değerine göre koşullu olarak işlenir. Değer veya `false` `null`ise, öznitelik işlenmez. Değer ise `true`, öznitelik küçültülmüş olarak işlenir.

Aşağıdaki örnekte, `IsCompleted` öğesinin biçimlendirmesinde işlenip işlenmeyeceğini `checked` belirler:

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

`IsCompleted` İse`true`, onay kutusu şu şekilde işlenir:

```html
<input type="checkbox" checked />
```

`IsCompleted` İse`false`, onay kutusu şu şekilde işlenir:

```html
<input type="checkbox" />
```

Daha fazla bilgi için bkz. <xref:mvc/views/razor>.

## <a name="raw-html"></a>Ham HTML

Dizeler normalde DOM metin düğümleri kullanılarak işlenir. Bu, içerdikleri tüm biçimlendirmenin yok sayıldığı ve değişmez değer olarak kabul edildiği anlamına gelir. Ham HTML işlemek için, HTML içeriğini bir `MarkupString` değerde sarın. Değer HTML veya SVG olarak ayrıştırılır ve DOM 'a eklenir.

> [!WARNING]
> Güvenilmeyen bir kaynaktan oluşturulan ham HTML işleme bir **güvenlik riskidir** ve kaçınılması gerekir!

Aşağıdaki örnek, bir bileşenin işlenmiş `MarkupString` çıktısına statik HTML içeriği bloğunu eklemek için türünün kullanımını gösterir:

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a>Şablonlu bileşenler

Şablonlu bileşenler, bir veya daha fazla UI şablonunu parametre olarak kabul eden bileşenlerdir, daha sonra bileşen işleme mantığının bir parçası olarak kullanılabilir. Şablonlu bileşenler, normal bileşenlerden daha yeniden kullanılabilir olan üst düzey bileşenleri yazmanıza izin verir. Birkaç örnek şunlardır:

* Kullanıcının tablo üst bilgisi, satırları ve altbilgisi için şablon belirtmesini sağlayan tablo bileşeni.
* Bir kullanıcının bir listedeki öğeleri işlemek için şablon belirlemesine izin veren bir liste bileşenidir.

### <a name="template-parameters"></a>Şablon parametreleri

Şablonlu bir bileşen, veya `RenderFragment` `RenderFragment<T>`türünde bir veya daha fazla bileşen parametresi belirtilerek tanımlanır. Bir işleme parçası, işlenecek Kullanıcı arabiriminin bir kesimini temsil eder. `RenderFragment<T>`işleme parçası çağrıldığında belirtilebildiği bir tür parametresi alır.

`TableTemplate`bileşeninde

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

Şablonlu bir bileşen kullanırken, şablon parametreleri parametrelerin adlarıyla (`TableHeader` ve `RowTemplate` aşağıdaki örnekte) eşleşen alt öğeler kullanılarak belirtilebilir:

```cshtml
<TableTemplate Items="pets">
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

Öğe olarak geçirilmiş türdeki `RenderFragment<T>` bileşen bağımsız değişkenleri adlı `context` örtük bir parametreye sahiptir (örneğin, `@context.PetId`Yukarıdaki kod örneğinden), ancak alt öğe `Context` özniteliğini kullanarak parametre adını değiştirebilirsiniz dosyalarında. Aşağıdaki örnekte, `RowTemplate` `Context` öğesinin özniteliği `pet` parametresini belirtir:

```cshtml
<TableTemplate Items="pets">
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

Alternatif olarak, bileşen öğesi üzerinde `Context` özniteliğini de belirtebilirsiniz. Belirtilen `Context` öznitelik, belirtilen tüm şablon parametreleri için geçerlidir. Bu, örtük alt içerik (herhangi bir sarmalama alt öğesi olmadan) için içerik parametre adını belirtmek istediğinizde yararlı olabilir. Aşağıdaki örnekte, `Context` özniteliği `TableTemplate` öğesinde görünür ve tüm şablon parametreleri için geçerlidir:

```cshtml
<TableTemplate Items="pets" Context="pet">
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

### <a name="generic-typed-components"></a>Genel olarak yazılmış bileşenler

Şablonlu bileşenler çoğunlukla genel olarak türdedir. Örneğin, değerleri işlemek `ListViewTemplate` `IEnumerable<T>` için genel bir bileşen kullanılabilir. Genel bir bileşen tanımlamak için, tür parametrelerini [@typeparam](xref:mvc/views/razor#typeparam) belirtmek için yönergesini kullanın:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

Genel türsüz bileşenleri kullanırken tür parametresi mümkünse algılanır:

```cshtml
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

Aksi halde tür parametresi, tür parametresinin adıyla eşleşen bir öznitelik kullanılarak açıkça belirtilmelidir. Aşağıdaki örnekte, `TItem="Pet"` türü belirtir:

```cshtml
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a>Değerleri ve parametreleri basamaklama

Bazı senaryolarda, özellikle birden çok bileşen katmanı olduğunda, [bileşen parametreleri](#component-parameters)kullanarak bir üst bileşenden bir alt bileşene veri akışı yapmak uygun değildir. Değerleri ve parametreleri basamaklama, bir üst bileşenin tüm alt bileşenlerine değer sağlaması için kullanışlı bir yol sağlayarak bu sorunu çözebilir. Basamaklı değerler ve parametreler, bileşenlerin koordinasyonu için bir yaklaşım sağlar.

### <a name="theme-example"></a>Tema örneği

Örnek uygulamadan aşağıdaki örnekte, `ThemeInfo` sınıfı, uygulamanın belirli bir bölümündeki tüm düğmelerin aynı stili paylaştığı şekilde bileşen hiyerarşisinin akışını yapmak için tema bilgilerini belirtir.

*Uıthemeclasses/Themeınfo. cs*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

Bir üst bileşen basamaklı değer bileşeni kullanılarak basamaklı bir değer sağlayabilir. `CascadingValue` Bileşen, bileşen hiyerarşisinin bir alt ağacını sarmalanmış ve bu alt ağaçta bulunan tüm bileşenlere tek bir değer sağlar.

Örneğin, örnek uygulama, uygulamanın düzenleriyle,`ThemeInfo` `@Body` özelliğin düzen gövdesini oluşturan tüm bileşenler için bir geçişli parametre olarak tema bilgilerini () belirtir. `ButtonClass`öğesinin `btn-success` bir değeri, düzen bileşeninde atanır. Tüm alt bileşenler bu özelliği `ThemeInfo` basamaklı nesne aracılığıyla kullanabilir.

`CascadingValuesParametersLayout`bileşeninde

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="theme">
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

Basamaklı değerlerin kullanılması için, bileşenler `[CascadingParameter]` özniteliği kullanarak Geçişli Parametreler bildirir. Basamaklı değerler, türe göre basamaklı parametrelere bağlanır.

Örnek uygulamada, `CascadingValuesParametersTheme` bileşen `ThemeInfo` basamaklı değeri basamaklı bir parametreye bağlar. Parametresi, bileşen tarafından görünen düğmelerden birine ait CSS sınıfını ayarlamak için kullanılır.

`CascadingValuesParametersTheme`bileşeninde

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
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

Aynı alt ağaç içindeki aynı türdeki birden çok değeri basamakla, her `Name` `CascadingValue` bileşene ve karşılık gelen `CascadingParameter`öğesine benzersiz bir dize sağlayın. Aşağıdaki örnekte, iki `CascadingValue` bileşen adına `MyCascadingType` göre farklı örneklerini basamakla:

```cshtml
<CascadingValue Value=@ParentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType ParentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

Alt bileşende, basamaklı parametreler değerlerini, üst bileşendeki ilgili basamaklı değerleri ada göre alır:

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a>TabSet örneği

Basamaklı parametreler, bileşenlerin bileşen hiyerarşisinde işbirliği yapmasına de olanak tanır. Örneğin, örnek uygulamada aşağıdaki *Tabset* örneğini göz önünde bulundurun.

Örnek uygulamada, sekmelerin uygulandığı `ITab` bir arabirim vardır:

[!code-csharp[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

Bileşen, `TabSet` çeşitli`Tab` bileşenleri içeren bileşenini kullanır: `CascadingValuesParametersTabSet`

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

Alt `Tab` bileşenler, `TabSet`öğesine açıkça parametre olarak aktarılmaz. Bunun yerine, alt `Tab` bileşenleri `TabSet`öğesinin alt içeriğinin bir parçasıdır. Bununla birlikte, `TabSet` üst bilgileri ve etkin sekmeyi işleyebilmesi için her `Tab` bileşen hakkında hala bilmeniz gerekir. Ek kod `TabSet` gerektirmeden bu koordinasyonu etkinleştirmek için bileşen, kendisini alt `Tab` bileşenler tarafından çekilen *basamaklı bir değer olarak sağlayabilir* .

`TabSet`bileşeninde

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

Alt bileşenler, kapsayan `TabSet` ' i `Tab` basamaklı bir parametre olarak yakalar, bu nedenle bileşenler, bu sekmenin `TabSet` etkin olduğu ve koordinasyonu üzerine eklenir. `Tab`

`Tab`bileşeninde

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a>Razor şablonları

Oluşturma parçaları Razor şablonu sözdizimi kullanılarak tanımlanabilir. Razor şablonları, bir UI parçacığı tanımlamak ve aşağıdaki biçimi varsaymak için bir yoldur:

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

Aşağıdaki örnek, bir bileşeni doğrudan bir `RenderFragment` bileşende `RenderFragment<T>` nasıl belirtdiğini ve işleneceğini gösterir. Oluşturma parçaları, [şablonlu bileşenlere](#templated-components)bağımsız değişken olarak da geçirilebilir.

```cshtml
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = 
        (pet) => @<p>Your pet's name is @pet.Name.</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

Önceki kodun işlenmiş çıktısı:

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a>El ile RenderTreeBuilder mantığı

`Microsoft.AspNetCore.Components.RenderTree`C# kodu kodda el ile oluşturma dahil olmak üzere bileşenleri ve öğeleri düzenleme yöntemleri sağlar.

> [!NOTE]
> Bileşenlerini oluşturmak `RenderTreeBuilder` için kullanımı gelişmiş bir senaryodur. Hatalı biçimlendirilmiş bir bileşen (örneğin, kapatılmamış bir biçimlendirme etiketi) tanımsız davranışa neden olabilir.

Aşağıdaki `PetDetails` bileşeni, başka bir bileşende el ile yerleşik olarak kullanılabilecek şekilde göz önünde bulundurun:

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

Aşağıdaki örnekte, `CreateComponent` yöntemindeki döngü üç `PetDetails` bileşen oluşturur. Bileşenleri ( `RenderTreeBuilder` `OpenComponent` ve`AddAttribute`) oluşturmak için yöntemler çağrılırken, dizi numaraları kaynak kodu satır numaralarıdır. Blazor fark algoritması, ayrı çağrı etkinleştirmeleri değil ayrı kod satırlarına karşılık gelen sıra numaralarına dayanır. Yöntemler içeren `RenderTreeBuilder` bir bileşen oluştururken, dizi numaralarına yönelik bağımsız değişkenleri kod olarak kodlayın. **Sıra numarasını oluşturmak için bir hesaplama veya sayaç kullanmak kötü performansa neden olabilir.** Daha fazla bilgi için bkz. [kod satırı numaralarıyla Ilgili sıra numaraları ve yürütme sırası çalışmıyor](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) bölümü.

`BuiltContent`bileşeninde

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
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

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a>Sıra numaraları, kod satırı numaralarıyla ilgilidir ve yürütme sırası değildir

Blazor `.razor` dosyaları her zaman derlenir. Derleme adımının çalışma zamanında uygulama performansını geliştiren `.razor` bilgileri eklemek için kullanılabilmesi için bu büyük olasılıkla harika bir avantajdır.

Bu geliştirmelerin önemli bir örneği *sıra numarası*içerir. Sıra numaraları, hangi çıkışların ayrı ve sıralı kod satırlarından geldiğini çalışma zamanına işaret ediyor. Çalışma zamanı, doğrusal bir zamanda, genel ağaç farkı algoritması için genellikle mümkün olandan çok daha hızlı olan etkili ağaç SLA 'ları oluşturmak için bu bilgileri kullanır.

Aşağıdaki basit `.razor` dosyayı göz önünde bulundurun:

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

Yukarıdaki kod, aşağıdakine benzer şekilde derlenir:

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

Kod ilk kez `someFlag` `true`çalıştırıldığında, Oluşturucu şunları alır:

| Sequence | Tür      | Veri   |
| :------: | --------- | :----: |
| 0        | Metin düğümü | adı  |
| 1\.        | Metin düğümü | Saniye |

`someFlag` Olduğunu`false`düşünün ve biçimlendirme yeniden işlenir. Bu kez, Oluşturucu şunları alır:

| Sequence | Tür       | Veri   |
| :------: | ---------- | :----: |
| 1\.        | Metin düğümü  | Saniye |

Çalışma zamanı bir fark gerçekleştirdiğinde, sıradaki `0` öğenin kaldırıldığını görür, bu nedenle aşağıdaki önemsiz *düzenleme betiğini*oluşturur:

* İlk metin düğümünü kaldırın.

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a>Program aracılığıyla sıra numaraları oluşturursanız ne yanlış gider

Aşağıdaki işleme Ağacı Oluşturucusu mantığını yazmanız yerine düşünün:

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

Şimdi ilk çıktı:

| Sequence | Tür      | Veri   |
| :------: | --------- | :----: |
| 0        | Metin düğümü | adı  |
| 1\.        | Metin düğümü | Saniye |

Bu sonuç önceki bir durum ile aynıdır, bu nedenle olumsuz bir sorun yoktur. `someFlag``false` ikinci işleme ve çıktı:

| Sequence | Tür      | Veri   |
| :------: | --------- | ------ |
| 0        | Metin düğümü | Saniye |

Bu kez, fark algoritması *iki* değişikliğin oluştuğunu görür ve algoritma aşağıdaki düzenleme betiğini üretir:

* İlk metin düğümünün değerini olarak `Second`değiştirin.
* İkinci metin düğümünü kaldırın.

Sıra numaralarının oluşturulması, `if/else` dal ve döngülerin orijinal kodda bulunduğu yer hakkındaki tüm yararlı bilgileri kaybetti. Bu, daha önce olduğu gibi bir fark **ile iki kez** sonuçlanır.

Bu, önemsiz bir örnektir. Karmaşık ve derin iç içe yapıları ve özellikle döngülerle daha gerçekçi durumlarda performans maliyeti daha önemlidir. Hangi döngü bloklarının veya dallarının eklendiğini veya kaldırıldığını hemen belirlemek yerine, fark algoritmasının işleme ağaçlarına katmaları ve genellikle eski ve yeni yapıların nasıl olduğu hakkında yanlış bir biçimde düzenleme betikleri oluşturması birbirleriyle ilişkilendir.

#### <a name="guidance-and-conclusions"></a>Kılavuz ve ekibinizle

* Sıra numaraları dinamik olarak oluşturulursa uygulama performansı de vardır.
* Altyapı, derleme zamanında yakalanmadığı takdirde gerekli bilgiler bulunmadığından, çalışma zamanında kendi sıra numaralarını otomatik olarak oluşturamaz.
* El ile uygulanan `RenderTreeBuilder` mantık uzun blokları yazmayın. Dosyaları `.razor` tercih edin ve derleyicinin sıra numaralarıyla uğraşmak için izin verin. El ile `RenderTreeBuilder` mantığın olmaması durumunda, uzun kod bloklarını `CloseRegion` çağrılarında `OpenRegion` / kaydırılmış küçük parçalara ayırın. Her bölge kendi ayrı dizi numaralarına sahiptir, bu nedenle her bölge içinde sıfırdan (veya herhangi bir rastgele sayıdan) yeniden başlatabilirsiniz.
* Dizi numaraları sabit kodluysa, fark algoritması yalnızca değer değerinde sıra numaralarının artırılmasını gerektirir. İlk değer ve boşluklar ilgisiz. Tek bir seçenek, kod satırı numarasını sıra numarası olarak kullanmak veya sıfırdan başlayıp bir ya da yüzlerce (ya da tercih edilen aralığa) artırmak için kullanılır. 
* Blazor, sıra numaralarını kullanır, diğer ağaç dağıtma Kullanıcı arabirimi çerçeveleri bunları kullanmaz. Dizi numaraları kullanıldığında ve Blazor, geliştiricilerin yazma `.razor` dosyaları için otomatik olarak sıra numaralarıyla ilgilenen bir derleme adımının avantajına sahiptir.

## <a name="localization"></a>Yerelleştirme

Blazor Server uygulamaları, [Yerelleştirme ara yazılımı](xref:fundamentals/localization#localization-middleware)kullanılarak yerelleştirilir. Ara yazılım, uygulamadan kaynak isteyen kullanıcılar için uygun kültürü seçer.

Kültür aşağıdaki yaklaşımlardan biri kullanılarak ayarlanabilir:

* [Çerezler](#cookies)
* [Kültürü seçmek için Kullanıcı arabirimi sağlama](#provide-ui-to-choose-the-culture)

Daha fazla bilgi ve örnek için bkz <xref:fundamentals/localization>.

### <a name="cookies"></a>Özgü

Yerelleştirme kültürü tanımlama bilgisi kullanıcının kültürünü kalıcı hale getirebilirler. Tanımlama bilgisi, uygulamanın ana bilgisayar `OnGet` sayfası (*Pages/Host. cshtml. cs*) yöntemi tarafından oluşturulur. Yerelleştirme ara yazılımı, sonraki isteklerde Kullanıcı kültürünü ayarlamak için tanımlama bilgilerini okur. 

Tanımlama bilgisinin kullanımı, WebSocket bağlantısının kültürü doğru şekilde yaymasını sağlar. Yerelleştirme şemaları URL yolunu veya sorgu dizesini temel alıyorsa, düzen WebSockets ile çalışmayabilir, bu nedenle kültürü kalıcı hale getiremeyebilir. Bu nedenle, yerelleştirme kültürü tanımlama bilgisinin kullanılması önerilen yaklaşımdır.

Kültür bir yerelleştirme tanımlama bilgisinde kalıcı hale getirilir kültür atamak için herhangi bir teknik kullanılabilir. Uygulamanın zaten sunucu tarafı ASP.NET Core için bir yerelleştirme şeması varsa, uygulamanın var olan yerelleştirme altyapısını kullanmaya devam edin ve uygulamanın şeması içinde yerelleştirme kültür tanımlama bilgisini ayarlayın.

Aşağıdaki örnekte, yerelleştirme ara yazılımı tarafından okunabilen bir tanımlama bilgisinde geçerli kültürün nasıl ayarlanacağı gösterilmektedir. Blazor Server uygulamasında aşağıdaki içeriklerle bir *Pages/Host. cshtml. cs* dosyası oluşturun:

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

Yerelleştirme uygulamada işlenir:

1. Tarayıcı, uygulamaya bir ilk HTTP isteği gönderir.
1. Kültür, yerelleştirme ara yazılımı tarafından atanır.
1. *_Host. cshtml. cs* dosyasındaki yöntemi,yanıtınbirparçasıolarakbirtanımlamabilgisindekültürüdevamettirir.`OnGet`
1. Tarayıcı, etkileşimli bir Blazor Server oturumu oluşturmak için bir WebSocket bağlantısı açar.
1. Yerelleştirme ara yazılımı tanımlama bilgisini okur ve kültürü atar.
1. Blazor sunucusu oturumu doğru kültür ile başlar.

## <a name="provide-ui-to-choose-the-culture"></a>Kültürü seçmek için Kullanıcı arabirimi sağlama

Bir kullanıcının bir kültür seçmesine izin vermek için kullanıcı ARABIRIMI sağlamak üzere bir *yeniden yönlendirme tabanlı yaklaşım* önerilir. Kullanıcı, kullanıcının oturum açma sayfasına yeniden yönlendirildiği ve sonra özgün kaynağa yeniden yönlendirildiği güvenli bir kaynağa&mdash;erişmeyi denediğinde, Web uygulamasında gerçekleşmelere benzer. 

Uygulama, bir denetleyiciye yeniden yönlendirme yoluyla kullanıcının seçili kültürünü devam ettirir. Denetleyici kullanıcının seçili kültürünü bir tanımlama bilgisine ayarlar ve kullanıcıyı özgün URI 'ye yeniden yönlendirir.

Bir tanımlama bilgisinde kullanıcının seçili kültürünü ayarlamak ve özgün URI 'ye yeniden yönlendirmeyi gerçekleştirmek için sunucuda bir HTTP uç noktası oluşturun:

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> Açık yeniden yönlendirme saldırılarını engellemek için eylemsonucunukullanın.`LocalRedirect` Daha fazla bilgi için bkz. <xref:security/preventing-open-redirects>.

Aşağıdaki bileşen, Kullanıcı bir kültür seçtiğinde ilk yeniden yönlendirmenin nasıl gerçekleştirileceği hakkında bir örnek göstermektedir:

```cshtml
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-blazor-apps"></a>Blazor uygulamalarında .NET yerelleştirme senaryolarını kullanma

Blazor uygulamaları içinde, aşağıdaki .NET yerelleştirme ve genelleştirme senaryoları kullanılabilir:

* . NET ' in kaynak sistemi
* Kültüre özgü sayı ve Tarih biçimlendirmesi

Blazor 'ın `@bind` işlevselliği, kullanıcının geçerli kültürüne göre Genelleştirme gerçekleştirir. Daha fazla bilgi için [veri bağlama](#data-binding) bölümüne bakın.

Sınırlı bir ASP.NET Core yerelleştirme senaryosu kümesi şu anda desteklenmektedir:

* `IStringLocalizer<>`Blazor uygulamalarında *desteklenir* .
* `IHtmlLocalizer<>`, `IViewLocalizer<>`ve veri ek açıklamaları yerelleştirme ASP.NET Core MVC senaryolardır ve Blazor uygulamalarında **desteklenmez** .

Daha fazla bilgi için bkz. <xref:fundamentals/localization>.

## <a name="scalable-vector-graphics-svg-images"></a>Ölçeklenebilir vektör grafik (SVG) görüntüleri

Blazor tarafından oluşturulduğundan, ölçeklenebilir vektör grafik (SVG) görüntüleri ( *. SVG*) dahil olmak üzere tarayıcıda desteklenen görüntüler, `<img>` etiketi aracılığıyla desteklenir:

```html
<img alt="Example image" src="some-image.svg" />
```

Benzer şekilde, SVG görüntüleri bir stil sayfası dosyasının ( *. css*) CSS kurallarında desteklenir:

```css
.my-element {
    background-image: url("some-image.svg");
}
```

Ancak, satır içi SVG işaretlemesi tüm senaryolarda desteklenmez. Bir `<svg>` etiketi doğrudan bir bileşen dosyasına ( *. Razor*) yerleştirirseniz, temel görüntü işleme desteklenir, ancak birçok gelişmiş senaryo desteklenmemiştir. Örneğin, `<use>` Etiketler Şu anda dikkate alınamaz ve `@bind` bazı SVG etiketleriyle kullanılamaz. Gelecekteki bir sürümde bu sınırlamaları ele almayı bekliyoruz.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/blazor/server>&ndash; Kaynak tükenmesi ile Çekişmek zorunda olması gereken Blazor sunucu uygulamaları oluşturmaya yönelik yönergeler içerir.
