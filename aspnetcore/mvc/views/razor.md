---
title: ASP.NET Core Razor söz dizimi başvurusu
author: rick-anderson
description: Kodu sunucu tabanlı Web sayfalarını eklemek için Razor söz dizimi biçimlendirme hakkında bilgi edinin.
ms.author: riande
ms.date: 02/12/2020
uid: mvc/views/razor
ms.openlocfilehash: e9d2e42ba3c36bc1661739f3b105ec8efe03de48
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658720"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a>ASP.NET Core Razor söz dizimi başvurusu

By [Rick Anderson](https://twitter.com/RickAndMSFT), [Taylor Mullen](https://twitter.com/ntaylormullen)ve [dan vicarel](https://github.com/Rabadash8820)

Razor kod sunucu tabanlı Web sayfalarını eklemek için bir biçimlendirme sözdizimi aşağıdaki gibidir. Razor işaretlemesi Razor sözdizimini oluşur C#ve HTML. Razor içeren dosyalar genellikle *. cshtml* dosya uzantısına sahiptir. Razor, [Razor bileşenleri](xref:blazor/components) dosyalarında ( *. Razor*) de bulunur.

## <a name="rendering-html"></a>HTML işleme

HTML varsayılan Razor dilidir. HTML Razor işaretlemesi işleme bir HTML dosyasından HTML'yi işlemeye değerinden farklı değildir. *. Cshtml* Razor dosyalarındaki HTML işaretlemesi sunucu tarafından değiştirilmeden işlenir.

## <a name="razor-syntax"></a>Razor sözdizimi

Razor, C# HTML 'den ' ye C#geçiş yapmak için `@` sembolünü destekler ve kullanır. Razor değerlendirir C# ifadeleri ve bunları HTML çıktısında oluşturur.

Bir `@` sembol sonrasında [Razor ayrılmış bir anahtar sözcük](#razor-reserved-keywords)olduğunda, bu, Razor 'e özgü işaretlere geçer. Aksi takdirde düz geçiş C#.

Razor biçimlendirmesinde bir `@` sembolünden çıkmak için ikinci bir `@` sembol kullanın:

```cshtml
<p>@@Username</p>
```

Kod, HTML 'de tek bir `@` simgesiyle işlenir:

```html
<p>@Username</p>
```

HTML öznitelikleri ve e-posta adreslerini içeren içerikler `@` sembolünü geçiş karakteri olarak değerlendirmez. Aşağıdaki örnekte e-posta adresleri tarafından Razor ayrıştırma olduğu:

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Örtük Razor ifadeleri

Örtük Razor ifadeleri `@` sonrasında C# kod ile başlar:

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

C# `await` anahtar sözcüğünün dışında, örtük ifadeler boşluk içermemelidir. Varsa C# deyimi açık bir bitiş sahipse, boşluk intermingled:

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

Parantez içinde (`<>`C# ) karakterler bir HTML etiketi olarak yorumlandığından **örtük ifadeler genel türler içeremez.** Aşağıdaki kod geçerli **değil** :

```cshtml
<p>@GenericMethod<int>()</p>
```

Yukarıdaki kod, aşağıdakilerden birini benzer bir derleyici hatası oluşturur:

* "İnt" öğesi kapalı değildi. Tüm öğeleri olmalıdır kendi kendine kapanan veya eşleşen bir bitiş etiketi sahip.
* Yöntem grubu 'object' türü temsilci GenericMethod' dönüştürülemiyor. Bir yöntemi çağırmak mı istiyordunuz?'

Genel metot çağrılarının [açık bir Razor ifadesinde](#explicit-razor-expressions) veya [Razor kodu bloğunda](#razor-code-blocks)sarmalanması gerekir.

## <a name="explicit-razor-expressions"></a>Açık Razor ifadeleri

Açık Razor ifadeleri, dengeli parantezle bir `@` sembolünden oluşur. Geçen haftaki zaman işlemek için aşağıdaki Razor işaretlemesi kullanılır:

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

`@()` parantez içindeki içerikler değerlendirilir ve çıkışa işlenir.

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

Açık ifade olmadan, `<p>Age@joe.Age</p>` bir e-posta adresi olarak kabul edilir ve `<p>Age@joe.Age</p>` işlenir. Açık bir ifade olarak yazıldığında `<p>Age33</p>` işlenir.

Açık ifadeler, *. cshtml* dosyalarındaki genel metotlardan çıkış oluşturmak için kullanılabilir. Aşağıdaki biçimlendirmede gösterilen hatayı düzeltmek gösterilmektedir köşeli parantez önceki neden bir C# genel. Kodu açık bir ifade olarak yazılır:

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a>İfade kodlama

C#HTML kodlu bir dizeye değerlendirilen ifadeleri var. C#`IHtmlContent` değerlendiren ifadeler doğrudan `IHtmlContent.WriteTo`üzerinden işlenir. C#`IHtmlContent` olarak değerlendirilmeyen ifadeler, `ToString` tarafından bir dizeye dönüştürülür ve işlenmeden önce kodlanır.

```cshtml
@("<span>Hello World</span>")
```

Kod aşağıdaki HTML'yi oluşturur:

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

HTML tarayıcı gösterilir:

```html
<span>Hello World</span>
```

`HtmlHelper.Raw` çıktısı kodlanmamış, ancak HTML işaretlemesi olarak işlendi.

> [!WARNING]
> Ayıklanmış Kullanıcı girişinde `HtmlHelper.Raw` kullanmak bir güvenlik riskidir. Kullanıcı girişi, kötü amaçlı JavaScript veya diğer saldırılara içerebilir. Kullanıcı girişi temizlenirken zordur. Kullanıcı girişiyle `HtmlHelper.Raw` kullanmaktan kaçının.

```cshtml
@Html.Raw("<span>Hello World</span>")
```

Kod aşağıdaki HTML'yi oluşturur:

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Razor kodu bloğu

Razor kodu blokları `@` başlar ve `{}`alınır. İfadeler, aksine C# olmayan kod blokları içinde kod çizilir. Kod blokları ve ifadeler bir görünümdeki aynı kapsamı paylaşan ve sırayla tanımlanır:

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

HTML oluşturması gereken bir kod bloğunun alt bölümünü tanımlamak için, karakterleri Razor `<text>` etiketiyle çevreleyin:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Tarafından HTML etiketleri arasına olmayan HTML oluşturmak için bu yaklaşımı kullanın. Bir HTML veya Razor etiket olmadan, bir Razor çalışma zamanı hatası oluşur.

`<text>` etiketi, içerik işlerken boşluğu denetlemek için yararlıdır:

* Yalnızca `<text>` etiketi arasındaki içerik işlenir.
* HTML çıktısında `<text>` etiketi görüntülenmeden önce veya sonra boşluk yok.

### <a name="explicit-line-transition"></a>Açık satır geçişi

Tüm satırın geri kalanını bir kod bloğu içinde HTML olarak işlemek için `@:` söz dizimini kullanın:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Kodda `@:` olmadan bir Razor çalışma zamanı hatası oluşturulur.

Razor dosyasındaki ek `@` karakterler, bloktaki daha sonra bulunan deyimlerde derleyici hatalarına neden olabilir. Bu derleyici hataları önce bildirilen hatayı gerçek bir hata oluştuğu için anlamak zor olabilir. Bu hata, tek bir kod bloğunun birden çok örtük/açık ifadelere birleştirdikten sonra yaygındır.

## <a name="control-structures"></a>Denetim yapıları

Denetim yapıları kod bloğu bir uzantı var. Kod blokları tüm yönlerini (biçimlendirme, satır içi geçiş C#) aşağıdaki yapılar için de geçerlidir:

### <a name="conditionals-if-else-if-else-and-switch"></a>Conditionals, else if, Else ve \@Switch \@

Kod çalıştığında denetimleri `@if`:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else` ve `else if` `@` sembolünü gerektirmez:

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

### <a name="looping-for-foreach-while-and-do-while"></a>İçin döngü \@, \@foreach, \@sırasında ve \@do

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

### <a name="compound-using"></a>Kullanarak bileşik \@

' C#De, bir nesnenin atıldığından emin olmak için `using` bir ifade kullanılır. Razor aynı mekanizmayı ek içeriklere sahip bir HTML Yardımcıları oluşturmak için kullanılır. Aşağıdaki kodda, HTML Yardımcıları `@using` ifadesiyle bir `<form>` etiketi işlerler:

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

### <a name="lock"></a>\@kilidi

Razor kilit deyimleri kritik bölümlerle korumanın yeteneğine sahiptir:

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Yorumlar

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

Web sayfası işlenmeden önce razor açıklama sunucu tarafından kaldırılır. Razor açıklamaları sınırlandırmak için `@*  *@` kullanır. Sunucu tüm biçimlendirme işlemesi yapmıyor için aşağıdaki kodu, geçersiz kılınan:

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

Razor yönergeleri `@` sembolünü takip eden ayrılmış anahtar sözcüklerle örtük ifadelerle temsil edilir. Bir yönergesi, genellikle bir görünüm ayrıştırılır veya farklı bir işlevsellik sağlar şeklini değiştirir.

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

Bu makalenin ilerleyen kısımlarında, [bir görünüm için oluşturulan Razor C# sınıfını İnceleme](#inspect-the-razor-c-class-generated-for-a-view) bölümünde bu oluşturulan sınıfın nasıl görüntüleneceği açıklanmaktadır.

### <a name="attribute"></a>\@özniteliği

`@attribute` yönergesi, verilen özniteliği oluşturulan sayfanın veya görünümün sınıfına ekler. Aşağıdaki örnek `[Authorize]` özniteliğini ekler:

```cshtml
@attribute [Authorize]
```

::: moniker range=">= aspnetcore-3.0"

### <a name="code"></a>\@kodu

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

`@code` bloğu bir [Razor bileşeninin](xref:blazor/components) bir bileşene üye ( C# alanlar, Özellikler ve Yöntemler) eklemesini sağlar:

```razor
@code {
    // C# members (fields, properties, and methods)
}
```

Razor bileşenleri için, `@code` [`@functions`](#functions) diğer adı `@functions`üzerinde önerilir. Birden fazla `@code` bloğu izin verilir.

::: moniker-end

### <a name="functions"></a>\@işlevleri

`@functions` yönergesi, oluşturulan sınıfa C# üye (alanlar, Özellikler ve Yöntemler) eklemeyi sağlar:

```cshtml
@functions {
    // C# members (fields, properties, and methods)
}
```

::: moniker range=">= aspnetcore-3.0"

[Razor bileşenlerinde](xref:blazor/components), üye eklemek C# için `@functions` üzerinde `@code` kullanın.

::: moniker-end

Örnek:

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

Kod aşağıdaki HTML biçimlendirmeyi oluşturur:

```html
<div>From method: Hello</div>
```

Aşağıdaki kodu oluşturulmuş Razor olan C# sınıfı:

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

::: moniker range=">= aspnetcore-3.0"

`@functions` Yöntemler, biçimlendirme olduğunda şablon oluşturma yöntemleri olarak görev yapar:

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

`@implements` yönergesi, oluşturulan sınıf için bir arabirim uygular.

Aşağıdaki örnek, <xref:System.IDisposable.Dispose*> yönteminin çağrılabilmesi için <xref:System.IDisposable?displayProperty=fullName> uygular:

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

### <a name="inherits"></a>\@devralır

`@inherits` yönergesi, görünümün devraldığı sınıfın tam denetimini sağlar:

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

Aşağıdaki kod bir özel bir Razor sayfası türüdür:

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

`CustomText` bir görünümde görüntülenir:

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

Kod aşağıdaki HTML'yi oluşturur:

```html
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

 `@model` ve `@inherits` aynı görünümde kullanılabilir. `@inherits`, görünümün içeri aktardığı bir *_ViewImports. cshtml* dosyasında olabilir:

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

Aşağıdaki kod, kesin türü belirtilmiş görünüm örneğidir:

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

Modelde "rick@contoso.com" geçirilirse, Görünüm aşağıdaki HTML işaretlemesini oluşturur:

```html
<div>The Login Email: rick@contoso.com</div>
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

### <a name="inject"></a>\@ekleme

`@inject` yönergesi, Razor sayfasının hizmet [kapsayıcısından](xref:fundamentals/dependency-injection) bir hizmeti bir görünüme eklemesine olanak sağlar. Daha fazla bilgi için bkz. [görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection).

::: moniker range=">= aspnetcore-3.0"

### <a name="layout"></a>\@düzeni

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

`@layout` yönergesi Razor bileşeninin bir yerleşimini belirtir. Düzen bileşenleri kod yinelemeyi ve tutarsızlığın önüne geçmek için kullanılır. Daha fazla bilgi için bkz. <xref:blazor/layouts>.

::: moniker-end

### <a name="model"></a>\@modeli

*Bu senaryo yalnızca MVC görünümleri ve Razor Pages (. cshtml) için geçerlidir.*

`@model` yönergesi, bir görünüme veya sayfaya geçirilen modelin türünü belirtir:

```cshtml
@model TypeNameOfModel
```

Bir ASP.NET Core MVC veya bireysel kullanıcı hesaplarıyla oluşturulan Razor Pages uygulama, *Görünümler/Account/Login. cshtml* aşağıdaki model bildirimini içerir:

```cshtml
@model LoginViewModel
```

Oluşturulan sınıf `RazorPage<dynamic>`devralır:

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Razor, görünüme geçirilen modele erişim için bir `Model` özelliği kullanıma sunar:

```cshtml
<div>The Login Email: @Model.Email</div>
```

`@model` yönergesi `Model` özelliğinin türünü belirtir. Yönergesi, görünümün türettiği oluşturulan sınıfın `RazorPage<T>` `T` belirtir. `@model` yönergesi belirtilmemişse, `Model` özelliği `dynamic`türündedir. Daha fazla bilgi için bkz. [türü kesin belirlenmiş modeller ve @model anahtar sözcüğü](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).

### <a name="namespace"></a>\@ad alanı

`@namespace` yönergesi:

* Oluşturulan Razor sayfası, MVC görünümü veya Razor bileşeni sınıfının ad alanını ayarlar.
* Bir sayfa, görünüm veya bileşen sınıfının kök türetilmiş ad alanlarını dizin ağacındaki en yakın içeri aktarmalar dosyasından, *_ViewImports. cshtml* (görünümler veya sayfalar) veya *_Imports. Razor* (Razor bileşenleri) olarak ayarlar.

```cshtml
@namespace Your.Namespace.Here
```

Aşağıdaki tabloda gösterilen Razor Pages örneği için:

* Her sayfa *sayfaları/_ViewImports. cshtml*'yi içeri aktarır.
* *Pages/_ViewImports. cshtml* `@namespace Hello.World`içerir.
* Her sayfa, ad alanının kökü olarak `Hello.World` sahiptir.

| Sayfasında                                        | Ad Alanı                             |
| ------------------------------------------- | ------------------------------------- |
| *Pages/Index. cshtml*                        | `Hello.World`                         |
| *Sayfa/fazla sayfa/sayfa. cshtml*               | `Hello.World.MorePages`               |
| *Pages/te Pages/Evente Pages/Page. cshtml* | `Hello.World.MorePages.EvenMorePages` |

Yukarıdaki ilişkiler, MVC görünümleri ve Razor bileşenleriyle kullanılan dosyaları içeri aktarmak için geçerlidir.

Birden çok içeri aktarma dosyası `@namespace` yönergesine sahip olduğunda, kök ad alanını ayarlamak için dizin ağacındaki sayfaya, görünüme veya bileşene en yakın dosya kullanılır.

Yukarıdaki örnekteki *evente Pages* klasörünün `@namespace Another.Planet` bir içeri aktarmalar dosyası varsa (veya *sayfa/değer sayfaları/Evente Pages/Page. cshtml* dosyası `@namespace Another.Planet`içeriyorsa), sonuç aşağıdaki tabloda gösterilmiştir.

| Sayfasında                                        | Ad Alanı               |
| ------------------------------------------- | ----------------------- |
| *Pages/Index. cshtml*                        | `Hello.World`           |
| *Sayfa/fazla sayfa/sayfa. cshtml*               | `Hello.World.MorePages` |
| *Pages/te Pages/Evente Pages/Page. cshtml* | `Another.Planet`        |

### <a name="page"></a>\@sayfası

::: moniker range=">= aspnetcore-3.0"

`@page` yönergesinin göründüğü dosyanın türüne bağlı olarak farklı etkileri vardır. Yönergesi:

* İçindeki bir *. cshtml* dosyasında, dosyanın bir Razor sayfası olduğunu gösterir. Daha fazla bilgi için bkz. [özel rotalar](xref:razor-pages/index#custom-routes) ve <xref:razor-pages/index>.
* Bir Razor bileşeninin istekleri doğrudan işlemesini belirtir. Daha fazla bilgi için bkz. <xref:blazor/routing>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bir *. cshtml* dosyasının ilk satırındaki `@page` yönergesi, dosyanın bir Razor sayfası olduğunu gösterir. Daha fazla bilgi için bkz. <xref:razor-pages/index>.

::: moniker-end

### <a name="section"></a>\@bölümü

*Bu senaryo yalnızca MVC görünümleri ve Razor Pages (. cshtml) için geçerlidir.*

`@section` yönergesi, görünümler veya sayfaların HTML sayfasının farklı bölümlerinde içerik işlemesini sağlamak için [MVC ve Razor Pages düzenleriyle](xref:mvc/views/layout) birlikte kullanılır. Daha fazla bilgi için bkz. <xref:mvc/views/layout>.

### <a name="using"></a>kullanarak \@

`@using` yönergesi, oluşturulan görünüme C# `using` yönergesini ekler:

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

::: moniker range=">= aspnetcore-3.0"

[Razor bileşenlerinde](xref:blazor/components)`@using` Ayrıca hangi bileşenlerin kapsamda olduğunu denetler.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="directive-attributes"></a>Yönerge öznitelikleri

### <a name="attributes"></a>\@öznitelikleri

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

`@attributes`, bir bileşenin bildirilmeyen öznitelikleri işlemesini sağlar. Daha fazla bilgi için bkz. <xref:blazor/components#attribute-splatting-and-arbitrary-parameters>.

### <a name="bind"></a>\@bağlama

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

Bileşenlerdeki veri bağlama `@bind` özniteliğiyle gerçekleştirilir. Daha fazla bilgi için bkz. <xref:blazor/data-binding>.

### <a name="onevent"></a>{EVENT} üzerinde \@

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

Razor, bileşenler için olay işleme özellikleri sağlar. Daha fazla bilgi için bkz. <xref:blazor/event-handling>.

::: moniker-end

::: moniker range=">= aspnetcore-3.1"

### <a name="oneventpreventdefault"></a>{EVENT}:p reventDefault \@

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

Olay için varsayılan eylemi engeller.

### <a name="oneventstoppropagation"></a>{EVENT} üzerinde \@: Stopyayma

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

Olay için olay yaymayı sonlandırır.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="key"></a>\@anahtarı

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

`@key` Directive özniteliği, bileşenlerin, anahtar değerine göre öğelerin veya bileşenlerin korunmasını güvence altına almasına neden olur. Daha fazla bilgi için bkz. <xref:blazor/components#use-key-to-control-the-preservation-of-elements-and-components>.

### <a name="ref"></a>\@ref

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

Bileşen başvuruları (`@ref`) bir bileşen örneğine başvurmak için bir yol sağlar, böylece bu örneğe komut verebilirsiniz. Daha fazla bilgi için bkz. <xref:blazor/components#capture-references-to-components>.

### <a name="typeparam"></a>\@typeparam

*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*

`@typeparam` yönergesi, oluşturulan bileşen sınıfı için genel bir tür parametresi bildirir. Daha fazla bilgi için bkz. <xref:blazor/templated-components#generic-typed-components>.

::: moniker-end

## <a name="templated-razor-delegates"></a>Şablonlu Razor temsilciler

Razor şablonları aşağıdaki biçimde bir kullanıcı Arabirimi parçacığı tanımlamanıza izin ver:

```cshtml
@<tag>...</tag>
```

Aşağıdaki örnekte, <xref:System.Func%602>olarak şablonlu bir Razor temsilcisinin nasıl belirtildiği gösterilmektedir. [Dinamik tür](/dotnet/csharp/programming-guide/types/using-type-dynamic) , temsilcinin sarmalayan yönteminin parametresi için belirtilir. Bir [nesne türü](/dotnet/csharp/language-reference/keywords/object) , temsilcinin dönüş değeri olarak belirtilir. Şablon, bir `Name` özelliğine sahip `Pet` <xref:System.Collections.Generic.List%601> birlikte kullanılır.

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

Şablon, bir `foreach` ifadesiyle sağlanan `pets` ile işlenir:

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

Bir yöntem bağımsız değişkeni olarak bir satır içi Razor şablonu da sağlayabilirsiniz. Aşağıdaki örnekte `Repeat` yöntemi bir Razor şablonu alır. Yöntemi, HTML içerik ile sağlanan bir listeden öğeleri yineler üretmek için şablonu kullanır:

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

Önceki örnekteki Evcil hayvanlar 'ların listesini kullanarak `Repeat` yöntemi ile çağrılır:

* `Pet`<xref:System.Collections.Generic.List%601>.
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

[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)ile ilgili üç yönergeler vardır.

| Yönergesi | İşlev |
| --------- | -------- |
| [`@addTagHelper`](xref:mvc/views/tag-helpers/intro#add-helper-label) | Etiket Yardımcıları bir görünüm için kullanılabilir hale getirir. |
| [`@removeTagHelper`](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | Daha önce bir görünümden eklenen etiket Yardımcıları kaldırır. |
| [`@tagHelperPrefix`](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | Etiket Yardımcısı desteğinin etkinleştirmek ve etiket Yardımcısı kullanım açık hale getirmek için bir etiket öneki belirtir. |

## <a name="razor-reserved-keywords"></a>Razor ayrılmış anahtar sözcükler

### <a name="razor-keywords"></a>Razor anahtar sözcükleri

* sayfa (ASP.NET Core 2,1 veya üzeri bir sürüm gerektirir)
* ad alanı
* işlevleri
* Devralan
* model
* section
* yardımcı (şu anda ASP.NET Core tarafından desteklenmez)

Razor anahtar kelimelerinde `@(Razor Keyword)` (örneğin, `@(functions)`) yok edilir.

### <a name="c-razor-keywords"></a>C#Razor anahtar sözcükleri

* büyük/küçük harf
* do
* default
* for
* foreach
* if
* else
* Kilit
* anahtarı
* deneyin
* Yakalama
* finally
* kullanma
* while

C#Razor anahtar kelimelerinde `@(@C# Razor Keyword)` (örneğin, `@(@case)`) çift kaçış olmalıdır. İlk `@` Razor ayrıştırıcısının çıkar. İkinci `@` C# Ayrıştırıcıdan çıkar.

### <a name="reserved-keywords-not-used-by-razor"></a>Razor tarafından kullanılmayan ayrılmış anahtar sözcükler

* sınıf

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a>Razor İnceleme C# bir görünümü için oluşturulan sınıfı

::: moniker range=">= aspnetcore-2.1"

.NET Core SDK 2,1 veya üzeri bir sürümde, [Razor SDK](xref:razor-pages/sdk) Razor dosyalarının derlemesini işler. Bir proje oluştururken, Razor SDK proje kökünde bir *obj/< build_configuration >/< target_framework_moniker >/Razor* dizini oluşturur. *Razor* dizini içindeki dizin yapısı, projenin dizin yapısını yansıtır.

.NET Core 2.1 hedefleyen ASP.NET Core 2.1 Razor sayfaları projesinde aşağıdaki dizin yapısını göz önünde bulundurun:

* **Alanları**
  * **Yöneticileri**
    * **Sayfaları**
      * *Index. cshtml*
      * *Index.cshtml.cs*
* **Sayfaları**
  * **Paylaşılan**
    * *_Layout. cshtml*
  * *_ViewImports. cshtml*
  * *_ViewStart. cshtml*
  * *Index. cshtml*
  * *Index.cshtml.cs*

Projenin *hata ayıklama* yapılandırmasında oluşturulması aşağıdaki *obj* dizinini verir:

* **nesnesi**
  * **H**
    * **netcoreapp 2.1/**
      * **Razor**
        * **Alanları**
          * **Yöneticileri**
            * **Sayfaları**
              * *Index.g.cshtml.cs*
        * **Sayfaları**
          * **Paylaşılan**
            * *_Layout. g. cshtml. cs*
          * *_ViewImports. g. cshtml. cs*
          * *_ViewStart. g. cshtml. cs*
          * *Index.g.cshtml.cs*

*Pages/Index. cshtml*için oluşturulan sınıfı görüntülemek için, *obj/Debug/netcoreapp 2.1/Razor/Pages/Index. g. cshtml. cs*dosyasını açın.

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Aşağıdaki sınıf, ASP.NET Core MVC projeye ekleyin:

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

`Startup.ConfigureServices`, MVC tarafından `CustomTemplateEngine` sınıfıyla eklenen `RazorTemplateEngine` geçersiz kılın:

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

`CustomTemplateEngine``return csharpDocument;` bildiriminde bir kesme noktası ayarlayın. Kesme noktasında program yürütmesi durdurulduğunda, `generatedCode`değerini görüntüleyin.

![Metin Görselleştirici generatedCode görünümünü](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a>Görünüm aramaları ve büyük/küçük harfe duyarlılık

Razor görüntüleme motorunu büyük küçük harfe duyarlı aramalar, görünümler için gerçekleştirir. Ancak, gerçek arama, temel alınan dosya sistemi tarafından belirlenir:

* Dosya tabanlı kaynağı:
  * Büyük küçük harfe duyarlı dosya sistemleri (örneğin, Windows) ile işletim sistemlerinde, fiziksel dosya sağlayıcısı aramaları büyük küçük harfe duyarlı. Örneğin, `return View("Test")` */views/Home/test.exe*, */views/Home/test.exe*ve diğer tüm büyük/küçük harf çeşitlerüyle sonuçlanır.
  * Büyük/küçük harfe duyarlı dosya sistemlerinde (örneğin, Linux, OSX ve `EmbeddedFileProvider`), aramalar büyük/küçük harfe duyarlıdır. Örneğin, `return View("Test")` özellikle */views/Home/test.exe. cshtml*ile eşleşir.
* Görünümleri önceden derlenmiş: ASP.NET Core 2.0 ve sonraki sürümleri, önceden derlenmiş görünümleri arama büyük/küçük harf tüm işletim sistemlerinde büyük harflere duyarlı değildir. Davranış Windows fiziksel dosya Sağlayıcısı'nın davranış aynıdır. Önceden derlenmiş iki görünüm yalnızca durumda farklıysa, arama sonucu belirleyici değildir.

Geliştiriciler, dosya ve dizin adlarını büyük küçük harfleri büyük/küçük harf eşleşmesi için önerilir:

* Alan, denetleyici ve eylem adları.
* Razor sayfaları.

Eşleşen servis talebi, temel alınan dosya sisteminden bağımsız olarak kendi görünümler dağıtımları Bul sağlar.
