---
title: ASP.NET Core Razor söz dizimi başvurusu
author: rick-anderson
description: Kodu sunucu tabanlı Web sayfalarını eklemek için Razor söz dizimi biçimlendirme hakkında bilgi edinin.
ms.author: riande
ms.date: 08/05/2019
uid: mvc/views/razor
ms.openlocfilehash: 75bf0e792ff7975f03e0f7c2fa6a71ed74d813e1
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819801"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a>ASP.NET Core Razor söz dizimi başvurusu

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), ve [Dan Vicarel](https://github.com/Rabadash8820)

Razor kod sunucu tabanlı Web sayfalarını eklemek için bir biçimlendirme sözdizimi aşağıdaki gibidir. Razor işaretlemesi Razor sözdizimini oluşur C#ve HTML. Razor genellikle içeren dosyalarınız bir *.cshtml* dosya uzantısı. Razor, [Razor bileşenleri](xref:blazor/components) dosyalarında ( *. Razor*) de bulunur.

## <a name="rendering-html"></a>HTML işleme

HTML varsayılan Razor dilidir. HTML Razor işaretlemesi işleme bir HTML dosyasından HTML'yi işlemeye değerinden farklı değildir. HTML biçimlendirmede *.cshtml* Razor dosyaları değiştirilmemiş sunucu tarafından işlenir.

## <a name="razor-syntax"></a>Razor sözdizimi

Razor destekler C# ve kullandığı `@` HTML geçiş sembolünden C#. Razor değerlendirir C# ifadeleri ve bunları HTML çıktısında oluşturur.

Olduğunda bir `@` sembol tarafından izlenen bir [Razor ayrılmış anahtar sözcüğü](#razor-reserved-keywords), Razor özgü biçimlendirme geçer. Aksi takdirde düz geçiş C#.

Kaçış için bir `@` sembol Razor işaretlemede, ikinci bir kullanın `@` sembol:

```cshtml
<p>@@Username</p>
```

Kodunu HTML ile tek bir işlenen `@` sembol:

```html
<p>@Username</p>
```

Olmayan HTML öznitelikleri ve e-posta adreslerini içeren bir içerik işleme `@` geçiş karakter olarak sembol. Aşağıdaki örnekte e-posta adresleri tarafından Razor ayrıştırma olduğu:

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Örtük Razor ifadeleri

Örtük Razor ifadeleri ile başlayıp `@` ardından C# kod:

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

Dışında C# `await` anahtar sözcüğü, örtük ifadeleri boşluk içermemelidir. Varsa C# deyimi açık bir bitiş sahipse, boşluk intermingled:

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

Örtük ifade **olamaz** içeren C# köşeli ayraçlar içindeki karakterler olarak genel türler (`<>`) bir HTML etiketi yorumlanır. Aşağıdaki kod **değil** geçerli:

```cshtml
<p>@GenericMethod<int>()</p>
```

Yukarıdaki kod, aşağıdakilerden birini benzer bir derleyici hatası oluşturur:

* "İnt" öğesi kapalı değildi. Tüm öğeleri olmalıdır kendi kendine kapanan veya eşleşen bir bitiş etiketi sahip.
* Yöntem grubu 'object' türü temsilci GenericMethod' dönüştürülemiyor. Bir yöntemi çağırmak mı istiyordunuz?'

Genel yöntem çağrılarını sarmalanmış, içinde bir [Razor açık ifadesi](#explicit-razor-expressions) veya [Razor kodu bloğu](#razor-code-blocks).

## <a name="explicit-razor-expressions"></a>Açık Razor ifadeleri

Açık Razor ifadeleri oluşur bir `@` dengeli parantez ile simge. Geçen haftaki zaman işlemek için aşağıdaki Razor işaretlemesi kullanılır:

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

İçinde herhangi bir içerik `@()` parantez değerlendirilir ve işlenen çıkışı.

Önceki bölümde açıklanan, örtük ifadeleri genellikle boşluk içeremez. Aşağıdaki kodda, bir hafta, geçerli saatten çıkarılabilir değil:

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

Kod aşağıdaki HTML'yi oluşturur:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

Açık ifadeler, metin bir ifadenin sonucu ile birleştirmek için kullanılabilir:

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

Açık bir ifade olmadan `<p>Age@joe.Age</p>` bir e-posta adresi kabul edilir ve `<p>Age@joe.Age</p>` işlenir. Açık bir ifade olarak yazılırken `<p>Age33</p>` işlenir.

Açık ifadeleri, genel yöntemleri gelen çıkış işleme için kullanılabilir *.cshtml* dosyaları. Aşağıdaki biçimlendirmede gösterilen hatayı düzeltmek gösterilmektedir köşeli parantez önceki neden bir C# genel. Kodu açık bir ifade olarak yazılır:

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a>İfade kodlama

C#HTML kodlu bir dizeye değerlendirilen ifadeleri var. C#değerlendirmek için ifadeleri `IHtmlContent` doğrudan aracılığıyla işlenir `IHtmlContent.WriteTo`. C#Değerlendirme yok ifadeleri `IHtmlContent` tarafından bir dizeye dönüştürülür `ToString` ve bunlar işlenen önce kodlanır.

```cshtml
@("<span>Hello World</span>")
```

Kod aşağıdaki HTML'yi oluşturur:

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

HTML tarayıcı gösterilir:

```
<span>Hello World</span>
```

`HtmlHelper.Raw` Çıktı kodlanmış değildir, ancak geri HTML biçimlendirmesi olarak çizilir.

> [!WARNING]
> Kullanarak `HtmlHelper.Raw` unsanitized kullanıcı girişi bir güvenlik riski oluşturur. Kullanıcı girişi, kötü amaçlı JavaScript veya diğer saldırılara içerebilir. Kullanıcı girişi temizlenirken zordur. Kullanmaktan kaçının `HtmlHelper.Raw` kullanıcı girişi ile.

```cshtml
@Html.Raw("<span>Hello World</span>")
```

Kod aşağıdaki HTML'yi oluşturur:

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Razor kodu bloğu

Razor kod blokları kullanmaya başlama `@` ve tarafından alınmış `{}`. İfadeler, aksine C# olmayan kod blokları içinde kod çizilir. Kod blokları ve ifadeler bir görünümdeki aynı kapsamı paylaşan ve sırayla tanımlanır:

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

Kod aşağıdaki HTML'yi oluşturur:

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

::: moniker range=">= aspnetcore-3.0"

Kod blokları ' nda, şablon oluşturma yöntemleri olarak kullanılacak biçimlendirme ile [yerel işlevler](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) bildirin:

```cshtml
@{
    void RenderName(string name)
    {
        <p>Name: <strong>@name</strong></p>
    }

    RenderName("Mahatma Gandhi");
    RenderName("Martin Luther King, Jr.");
}
```

Kod aşağıdaki HTML'yi oluşturur:

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="implicit-transitions"></a>Örtük geçişleri

Bir kod bloğu varsayılan dilde C#, ancak Razor sayfası HTML olarak geçiş yapabilir:

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>Açık ayrılmış geçiş

HTML oluşturması gereken bir kod bloğunun alt bölümünü tanımlamak için, göstermek için karakterleri Razor `<text>` etiketiyle çevreleyin:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Tarafından HTML etiketleri arasına olmayan HTML oluşturmak için bu yaklaşımı kullanın. Bir HTML veya Razor etiket olmadan, bir Razor çalışma zamanı hatası oluşur.

Etiket `<text>` , içerik işlerken boşluğu denetlemek için yararlıdır:

* Yalnızca `<text>` etiketi arasındaki içerik işlenir.
* HTML çıktısında `<text>` etiket görüntülenmeden önce veya sonra boşluk yok.

### <a name="explicit-line-transition-with-colon"></a>İle açık satır geçişi\@&colon;

Tüm bir satırı geri kalanı bir kod bloğunun içine HTML olarak işlemek için kullanmak `@:` söz dizimi:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Olmadan `@:` kodda bir Razor çalışma zamanı hatası oluşturulur.

Razor `@` dosyasındaki fazla karakter, bloktaki daha sonra bulunan deyimlerde derleyici hatalarına neden olabilir. Bu derleyici hataları önce bildirilen hatayı gerçek bir hata oluştuğu için anlamak zor olabilir. Bu hata, tek bir kod bloğunun birden çok örtük/açık ifadelere birleştirdikten sonra yaygındır.

## <a name="control-structures"></a>Denetim yapıları

Denetim yapıları kod bloğu bir uzantı var. Kod blokları tüm yönlerini (biçimlendirme, satır içi geçiş C#) aşağıdaki yapılar için de geçerlidir:

### <a name="conditionals-if-else-if-else-and-switch"></a>Koşul varsa, Else, Else ve \@anahtar \@

`@if` kod çalıştığında denetimleri:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else` ve `else if` gerektirmeyen `@` sembol:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

Aşağıdaki biçimlendirme switch deyimi kullanma işlemini gösterir:

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a>For \@döngüsü, \@foreach, \@while ve \@do

Şablonlu HTML denetim ifadeleri döngü ile oluşturulabilir. Kişi listesini oluşturmak için:

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

Aşağıdaki döngü deyimi desteklenir:

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a>Kullanarak \@bileşik

İçinde C#, `using` deyimi, bir nesne kullanıldığında emin olmak için kullanılır. Razor aynı mekanizmayı ek içeriklere sahip bir HTML Yardımcıları oluşturmak için kullanılır. Aşağıdaki kodda, HTML Yardımcıları `<form>` `@using` ifadesiyle bir etiketi işlerler:

```cshtml
@using (Html.BeginForm())
{
    <div>
        Email: <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

### <a name="try-catch-finally"></a>\@TRY, catch, finally

Özel durum işleme benzer C#:

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>\@ine

Razor kilit deyimleri kritik bölümlerle korumanın yeteneğine sahiptir:

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Açıklamalar

Razor destekler C# ve HTML yorumlarında:

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

Kod aşağıdaki HTML'yi oluşturur:

```html
<!-- HTML comment -->
```

Web sayfası işlenmeden önce razor açıklama sunucu tarafından kaldırılır. Razor kullanan `@*  *@` açıklamaları sınırlandırmak için. Sunucu tüm biçimlendirme işlemesi yapmıyor için aşağıdaki kodu, geçersiz kılınan:

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a>Yönergeler

Razor yönergesi örtük ayrılmış anahtar sözcükler aşağıdaki deyimlerle tarafından temsil edilir `@` simgesi. Bir yönergesi, genellikle bir görünüm ayrıştırılır veya farklı bir işlevsellik sağlar şeklini değiştirir.

Razor kod görünümü nasıl oluşturur? anlama yönergeleri nasıl çalıştığını anlamak kolaylaştırır.

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

Kod aşağıdaki gibi bir sınıf oluşturur:

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

Bölümü bu makalenin ilerleyen bölümlerinde [Razor İnceleme C# bir görünümü için oluşturulan sınıf](#inspect-the-razor-c-class-generated-for-a-view) bu oluşturulan sınıf görüntülemek açıklanmaktadır.

### <a name="attribute"></a>\@özniteliğe

`@attribute` Yönergesi verilen özniteliği oluşturulan sayfanın veya görünümün sınıfına ekler. Aşağıdaki örnek `[Authorize]` özniteliğini ekler:

```cshtml
@attribute [Authorize]
```

::: moniker range=">= aspnetcore-3.0"

### <a name="code"></a>\@kodudur

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

Blok `@code` , bir [Razor bileşeninin](xref:blazor/components) bir bileşene üye C# (alanlar, Özellikler ve Yöntemler) eklemesini sağlar:

```cshtml
@code {
    // C# members (fields, properties, and methods)
}
```

Razor bileşenleri için, `@code` öğesinin [@functions](#functions) diğer adı ve önerilir `@functions`. Birden çok blok izin verilir. `@code`

::: moniker-end

### <a name="functions"></a>\@lerdir

Yönergesi, oluşturulan sınıfa C# üye (alanlar, Özellikler ve Yöntemler) eklemeyi sağlar: `@functions`

```cshtml
@functions {
    // C# members (fields, properties, and methods)
}
```

::: moniker range=">= aspnetcore-3.0"

[Razor bileşenlerinde](xref:blazor/components), üye eklemek `@code` `@functions` C# için ' i kullanın.

::: moniker-end

Örneğin:

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

Kod aşağıdaki HTML biçimlendirmeyi oluşturur:

```html
<div>From method: Hello</div>
```

Aşağıdaki kodu oluşturulmuş Razor olan C# sınıfı:

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

::: moniker range=">= aspnetcore-3.0"

`@functions`Yöntemler, işaretlemelerdeki şablon oluşturma yöntemleri olarak görev yapar:

```cshtml
@{
    RenderName("Mahatma Gandhi");
    RenderName("Martin Luther King, Jr.");
}

@functions {
    private void RenderName(string name)
    {
        <p>Name: <strong>@name</strong></p>
    }
}
```

Kod aşağıdaki HTML'yi oluşturur:

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

### <a name="implements"></a>\@uygular

`@implements` Yönergesi, oluşturulan sınıf için bir arabirim uygular.

Aşağıdaki örnek, yönteminin <xref:System.IDisposable?displayProperty=fullName> çağrılabilmesi için <xref:System.IDisposable.Dispose*> uygular:

```cshtml
@implements IDisposable

<h1>Example</h1>

@functions {
    private bool _isDisposed;

    ...

    public void Dispose() => _isDisposed = true;
}
```

::: moniker-end

### <a name="inherits"></a>\@alıp

`@inherits` Yönergesi görünümü devralan sınıf tam denetim sağlar:

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

Aşağıdaki kod bir özel bir Razor sayfası türüdür:

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

`CustomText` Bir görünümde görüntülenir:

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

Kod aşağıdaki HTML'yi oluşturur:

```html
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

 `@model` ve `@inherits` aynı görünümde kullanılabilir. `@inherits` kullanılabilir bir *_viewımports.cshtml* görünümü alır dosyası:

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

Aşağıdaki kod, kesin türü belirtilmiş görünüm örneğidir:

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

Varsa "rick@contoso.com" geçirilen modeldeki görünümünü aşağıdaki HTML biçimlendirmeyi oluşturur:

```html
<div>The Login Email: rick@contoso.com</div>
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

### <a name="inject"></a>\@eklenecek

`@inject` Yönergesi sağlayan bir hizmet eklemesine Razor sayfası [hizmet kapsayıcı](xref:fundamentals/dependency-injection) bir görünümde. Daha fazla bilgi için [görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection).

::: moniker range=">= aspnetcore-3.0"

### <a name="layout"></a>\@Düzenle

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

Yönerge `@layout` , Razor bileşeni için bir düzen belirtir. Düzen bileşenleri kod yinelemeyi ve tutarsızlığın önüne geçmek için kullanılır. Daha fazla bilgi için bkz. <xref:blazor/layouts>.

::: moniker-end

### <a name="model"></a>\@modelinizi

*Bu senaryo yalnızca MVC görünümleri ve Razor Pages (. cshtml) için geçerlidir.*

`@model` Yönergesi bir görünüm veya sayfaya geçirilen modelin türünü belirtir:

```cshtml
@model TypeNameOfModel
```

Bir ASP.NET Core MVC veya bireysel kullanıcı hesaplarıyla oluşturulan Razor Pages uygulama, *Görünümler/Account/Login. cshtml* aşağıdaki model bildirimini içerir:

```cshtml
@model LoginViewModel
```

Oluşturulan sınıf devraldığı `RazorPage<dynamic>`:

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Razor sunan bir `Model` modeli erişmek için özelliği geçirilen görünümü:

```cshtml
<div>The Login Email: @Model.Email</div>
```

Yönerge, `Model` özelliğin türünü belirtir. `@model` Yönergesi belirtir `T` içinde `RazorPage<T>` türetildiği görünümü oluşturulan, sınıfın. Varsa `@model` yönergesi belirtilmediyse, `Model` özelliği türüdür `dynamic`. Daha fazla bilgi için bkz. [türü kesin belirlenmiş modeller @model ve anahtar sözcüğü](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).

### <a name="namespace"></a>\@uzayına

`@namespace` Yönergesi:

* Oluşturulan Razor sayfası, MVC görünümü veya Razor bileşeni sınıfının ad alanını ayarlar.
* Bir sayfa, görünüm veya bileşen sınıfının kök türetilmiş ad alanlarını dizin ağacındaki en yakın içeri aktarmalar dosyasından (görünümler veya sayfalar) veya *_ımports. Razor* (Razor bileşenleri) ayarlar.

```cshtml
@namespace Your.Namespace.Here
```

Aşağıdaki tabloda gösterilen Razor Pages örneği için:

* Her sayfa sayfa */_Viewwimports. cshtml*içeri aktarır.
* *Pages/_Viewwimports. cshtml* Contains `@namespace Hello.World`.
* Her sayfa, `Hello.World` ad alanının kökü olarak bulunur.

| Sayfasında                                        | Ad Alanı                             |
| ------------------------------------------- | ------------------------------------- |
| *Pages/Index. cshtml*                        | `Hello.World`                         |
| *Sayfa/fazla sayfa/sayfa. cshtml*               | `Hello.World.MorePages`               |
| *Pages/te Pages/Evente Pages/Page. cshtml* | `Hello.World.MorePages.EvenMorePages` |

Yukarıdaki ilişkiler, MVC görünümleri ve Razor bileşenleriyle kullanılan dosyaları içeri aktarmak için geçerlidir.

Birden çok içeri aktarma dosyasında bir `@namespace` yönerge olduğunda, kök ad alanını ayarlamak için dizin ağacındaki sayfaya, görünüme veya bileşene en yakın dosya kullanılır.

Yukarıdaki örnekteki *evente Pages* klasöründe bir içeri aktarmalar dosyası `@namespace Another.Planet` varsa (veya sayfa/değer sayfaları */evente Pages/Page. cshtml* dosyası içeriyorsa `@namespace Another.Planet`), sonuç aşağıdaki tabloda gösterilir.

| Sayfasında                                        | Ad Alanı               |
| ------------------------------------------- | ----------------------- |
| *Pages/Index. cshtml*                        | `Hello.World`           |
| *Sayfa/fazla sayfa/sayfa. cshtml*               | `Hello.World.MorePages` |
| *Pages/te Pages/Evente Pages/Page. cshtml* | `Another.Planet`        |

### <a name="page"></a>\@sayfasında

::: moniker range=">= aspnetcore-3.0"

`@page` Yönergesinin, göründüğü dosyanın türüne bağlı olarak farklı etkileri vardır. Yönergesi:

* İçindeki bir *. cshtml* dosyasında, dosyanın bir Razor sayfası olduğunu gösterir. Daha fazla bilgi için bkz. <xref:razor-pages/index>.
* Bir Razor bileşeninin istekleri doğrudan işlemesini belirtir. Daha fazla bilgi için bkz. <xref:blazor/routing>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bir *. cshtml* dosyasının ilk satırındaki yönergesi,dosyanınbirRazorsayfasıolduğunugösterir.`@page` Daha fazla bilgi için bkz. <xref:razor-pages/index>.

::: moniker-end

### <a name="section"></a>\@kısmı

*Bu senaryo yalnızca MVC görünümleri ve Razor Pages (. cshtml) için geçerlidir.*

Yönerge, HTML sayfasının farklı kısımlarında görünüm veya sayfaların içerik işlemesini sağlamak için [MVC ve Razor Pages düzenleriyle](xref:mvc/views/layout) birlikte kullanılır. `@section` Daha fazla bilgi için bkz. <xref:mvc/views/layout>.

### <a name="using"></a>\@kullanarak

`@using` Yönergesi ekler C# `using` yönerge oluşturulmuş görünümü için:

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

::: moniker range=">= aspnetcore-3.0"

[Razor bileşenlerinde](xref:blazor/components) `@using` Ayrıca hangi bileşenlerin kapsamda olduğunu da kontrol eder.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="directive-attributes"></a>Yönerge öznitelikleri

### <a name="attributes"></a>\@özelliklerine

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

`@attributes`bir bileşenin bildirilmeyen öznitelikleri işlemesini sağlar. Daha fazla bilgi için bkz. <xref:blazor/components#attribute-splatting-and-arbitrary-parameters>.

### <a name="bind"></a>\@bağladığınızda

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

Bileşenlerdeki veri bağlama, `@bind` özniteliğiyle gerçekleştirilir. Daha fazla bilgi için bkz. <xref:blazor/components#data-binding>.

### <a name="onevent"></a>\@{Event} üzerinde

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

Razor, bileşenler için olay işleme özellikleri sağlar. Daha fazla bilgi için bkz. <xref:blazor/components#event-handling>.

### <a name="key"></a>\@anahtar

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

`@key` Directive özniteliği, anahtar değerine göre öğelerin veya bileşenlerin korunmasını güvence altına almak için bileşenlerin algoritma almasına neden olur. Daha fazla bilgi için bkz. <xref:blazor/components#use-key-to-control-the-preservation-of-elements-and-components>.

### <a name="ref"></a>\@ref

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

Bileşen başvuruları (`@ref`) bir bileşen örneğine başvurmak için bir yol sağlar, böylece bu örneğe komut verebilirsiniz. Daha fazla bilgi için bkz. <xref:blazor/components#capture-references-to-components>.

::: moniker-end

## <a name="templated-razor-delegates"></a>Şablonlu Razor temsilciler

Razor şablonları aşağıdaki biçimde bir kullanıcı Arabirimi parçacığı tanımlamanıza izin ver:

```cshtml
@<tag>...</tag>
```

Aşağıdaki örnekte, şablonlu Razor temsilci olarak belirtmek verilmektedir bir <xref:System.Func%602>. [Dinamik tür](/dotnet/csharp/programming-guide/types/using-type-dynamic) temsilci kapsülleyen yönteminin parametresi için belirtilir. Bir [nesne türü](/dotnet/csharp/language-reference/keywords/object) temsilcinin dönüş değeri olarak belirtilir. Şablon ile kullanılan bir <xref:System.Collections.Generic.List%601> , `Pet` olan bir `Name` özelliği.

```csharp
public class Pet
{
    public string Name { get; set; }
}
```

```cshtml
@{
    Func<dynamic, object> petTemplate = @<p>You have a pet named <strong>@item.Name</strong>.</p>;

    var pets = new List<Pet>
    {
        new Pet { Name = "Rin Tin Tin" },
        new Pet { Name = "Mr. Bigglesworth" },
        new Pet { Name = "K-9" }
    };
}
```

Şablon ile işlenen `pets` tarafından sağlanan bir `foreach` deyimi:

```cshtml
@foreach (var pet in pets)
{
    @petTemplate(pet)
}
```

İşlenmiş çıkışı:

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

Bir yöntem bağımsız değişkeni olarak bir satır içi Razor şablonu da sağlayabilirsiniz. Aşağıdaki örnekte, `Repeat` yöntem Razor şablonu alır. Yöntemi, HTML içerik ile sağlanan bir listeden öğeleri yineler üretmek için şablonu kullanır:

```cshtml
@using Microsoft.AspNetCore.Html

@functions {
    public static IHtmlContent Repeat(IEnumerable<dynamic> items, int times,
        Func<dynamic, IHtmlContent> template)
    {
        var html = new HtmlContentBuilder();

        foreach (var item in items)
        {
            for (var i = 0; i < times; i++)
            {
                html.AppendHtml(template(item));
            }
        }

        return html;
    }
}
```

Önceki örnekte, Evcil Hayvanlar listesi kullanarak `Repeat` yöntemi çağrıldığında:

* <xref:System.Collections.Generic.List%601> ' ın `Pet`.
* Her evcil hayvan yineleme sayısı.
* Satır içi şablon sırasız bir listesini liste öğeleri için kullanın.

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

İşlenmiş çıkışı:

```html
<ul>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>K-9</li>
    <li>K-9</li>
    <li>K-9</li>
</ul>
```

## <a name="tag-helpers"></a>Etiket Yardımcıları

*Bu senaryo yalnızca MVC görünümleri ve Razor Pages (. cshtml) için geçerlidir.*

İlgili üç yönergeleri vardır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).

| Yönergesi | İşlev |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | Etiket Yardımcıları bir görünüm için kullanılabilir hale getirir. |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | Daha önce bir görünümden eklenen etiket Yardımcıları kaldırır. |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | Etiket Yardımcısı desteğinin etkinleştirmek ve etiket Yardımcısı kullanım açık hale getirmek için bir etiket öneki belirtir. |

## <a name="razor-reserved-keywords"></a>Razor ayrılmış anahtar sözcükler

### <a name="razor-keywords"></a>Razor anahtar sözcükleri

* sayfa (ASP.NET Core 2,1 veya üzeri bir sürüm gerektirir)
* ad alanı
* işlevleri
* Devralan
* model
* section
* yardımcı (şu anda ASP.NET Core tarafından desteklenmez)

Razor anahtar sözcükleri kaçış ile `@(Razor Keyword)` (örneğin, `@(functions)`).

### <a name="c-razor-keywords"></a>C#Razor anahtar sözcükleri

* büyük/küçük harf
* do
* default
* for
* foreach
* if
* else
* lock
* anahtarı
* deneyin
* Yakalama
* finally
* kullanma
* while

C#Razor anahtar sözcükleri çift kaçış ile olmalıdır `@(@C# Razor Keyword)` (örneğin, `@(@case)`). İlk `@` Razor ayrıştırıcısı atlar. İkinci `@` çıkışları C# ayrıştırıcı.

### <a name="reserved-keywords-not-used-by-razor"></a>Razor tarafından kullanılmayan ayrılmış anahtar sözcükler

* sınıf

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a>Razor İnceleme C# bir görünümü için oluşturulan sınıfı

::: moniker range=">= aspnetcore-2.1"

.NET Core SDK'sını 2.1 veya daha sonra [Razor SDK](xref:razor-pages/sdk) derleme Razor dosyaları işler. Bir proje derlenirken Razor SDK'sı oluşturur bir *obj / < build_configuration > / < target_framework_moniker > / Razor* proje kök dizini. Dizin yapısında *Razor* dizini proje dizin yapısına yansıtır.

.NET Core 2.1 hedefleyen ASP.NET Core 2.1 Razor sayfaları projesinde aşağıdaki dizin yapısını göz önünde bulundurun:

* **Alanlar /**
  * **Yönetim /**
    * **Sayfa /**
      * *Index.cshtml*
      * *Index.cshtml.cs*
* **Sayfa /**
  * **Paylaşılan /**
    * *_Layout.cshtml*
  * *_Viewımports.cshtml*
  * *_ViewStart.cshtml*
  * *Index.cshtml*
  * *Index.cshtml.cs*

Projenin oluşturulmasında *hata ayıklama* yapılandırma aşağıdaki verir *obj* dizini:

* **obj /**
  * **Hata ayıklama /**
    * **netcoreapp2.1 /**
      * **Razor /**
        * **Alanlar /**
          * **Yönetim /**
            * **Sayfa /**
              * *Index.g.cshtml.cs*
        * **Sayfa /**
          * **Paylaşılan /**
            * *_Layout.g.cshtml.cs*
          * *_ViewImports.g.cshtml.cs*
          * *_ViewStart.g.cshtml.cs*
          * *Index.g.cshtml.cs*

Oluşturulan sınıf için görüntülenecek *Pages/Index.cshtml*açın *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Aşağıdaki sınıf, ASP.NET Core MVC projeye ekleyin:

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

İçinde `Startup.ConfigureServices`, geçersiz kılma `RazorTemplateEngine` ile MVC tarafından eklenen `CustomTemplateEngine` sınıfı:

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

Bir kesme noktası ayarlamak `return csharpDocument;` deyiminin `CustomTemplateEngine`. Program yürütme kesme noktasında durduğunda değerini görüntülemek `generatedCode`.

![Metin Görselleştirici generatedCode görünümünü](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a>Görünüm aramaları ve büyük/küçük harfe duyarlılık

Razor görüntüleme motorunu büyük küçük harfe duyarlı aramalar, görünümler için gerçekleştirir. Ancak, gerçek arama, temel alınan dosya sistemi tarafından belirlenir:

* Dosya tabanlı kaynağı:
  * Büyük küçük harfe duyarlı dosya sistemleri (örneğin, Windows) ile işletim sistemlerinde, fiziksel dosya sağlayıcısı aramaları büyük küçük harfe duyarlı. Örneğin, `return View("Test")` eşleşmelerini sonuçlanıyor */Views/Home/Test.cshtml*, */Views/home/test.cshtml*ve diğer büyük/küçük harf değişken.
  * Büyük küçük harfe duyarlı dosya sistemlerindeki (örneğin, Linux, OSX ile `EmbeddedFileProvider`), büyük küçük harfe duyarlı aramalar. Örneğin, `return View("Test")` özellikle eşleşen */Views/Home/Test.cshtml*.
* Önceden derlenmiş görünümler: ASP.NET Core 2,0 ve üzeri sürümlerde, önceden derlenmiş görünümleri aramak tüm işletim sistemlerinde büyük/küçük harfe duyarlı değildir. Davranış Windows fiziksel dosya Sağlayıcısı'nın davranış aynıdır. Önceden derlenmiş iki görünüm yalnızca durumda farklıysa, arama sonucu belirleyici değildir.

Geliştiriciler, dosya ve dizin adlarını büyük küçük harfleri büyük/küçük harf eşleşmesi için önerilir:

* Alan, denetleyici ve eylem adları.
* Razor sayfaları.

Eşleşen servis talebi, temel alınan dosya sisteminden bağımsız olarak kendi görünümler dağıtımları Bul sağlar.
