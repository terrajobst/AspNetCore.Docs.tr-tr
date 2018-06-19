---
title: ASP.NET Core için Razor söz dizimi başvurusu
author: rick-anderson
description: Web sayfalarının sunucu tabanlı kod katıştırma Razor biçimlendirme söz dizimi hakkında bilgi edinin.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/razor
ms.openlocfilehash: 224c855b355b8ecde36377bba6966edec251af6a
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962498"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a>ASP.NET Core için Razor söz dizimi başvurusu

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), ve [Dan Vicarel](https://github.com/Rabadash8820)

Razor kod sunucu tabanlı Web sayfalarının katıştırma için işaretleme sözdizimi aşağıdaki gibidir. Razor sözdizimi Razor biçimlendirme, C# ve HTML oluşur. Razor genellikle içeren dosyalarınız bir *.cshtml* dosya uzantısı.

## <a name="rendering-html"></a>HTML işleme

Varsayılan Razor HTML dilidir. Razor biçimlendirme işleme HTML bir HTML dosyası HTML işleme daha farklı değildir. HTML biçimlendirmede *.cshtml* Razor dosyaları değiştirmeden sunucu tarafından işlenir.

## <a name="razor-syntax"></a>Razor sözdizimi

Razor C# destekler ve kullandığı `@` C# HTML geçiş simgesini. Razor C# ifadeleri değerlendirir ve HTML çıkışında işler.

Zaman bir `@` simgesi tarafından izlenen bir [Razor ayrılmış anahtar sözcüğü](#razor-reserved-keywords), Razor özgü biçimlendirme geçiş. Aksi takdirde, düz C# diline geçiş.

Çıkış için bir `@` sembol Razor biçimlendirme içinde ikinci bir `@` simgesi:

```cshtml
<p>@@Username</p>
```

Kod tek bir HTML olarak işlenen `@` simgesi:

```html
<p>@Username</p>
```

HTML öznitelikleri ve e-posta adreslerini içeren içerik yok işler `@` geçiş karakter olarak simge. Aşağıdaki örnekte e-posta adresleri tarafından Razor ayrıştırma aynı şekilde:

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Örtük Razor ifadeleri

Örtük Razor ifadeleri başlayarak `@` C# kodu tarafından izlenen:

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

C# dışında `await` anahtar sözcüğü, örtük ifadeleri boşluk içermemelidir. C# deyimi Temizle bitiş varsa, alanları birbirine:

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

Örtük ifadeleri **olamaz** köşeli ayraçlar içindeki karakterlerden olarak C# genel türleri, içeren (`<>`) bir HTML etiketi olarak yorumlanır. Aşağıdaki kodu **değil** geçerli:

```cshtml
<p>@GenericMethod<int>()</p>
```

Önceki kod, aşağıdakilerden birini benzer bir derleyici hatası oluşturur:

 * "İnt" öğesi kapalı değildi. Tüm öğeleri ya da olmalıdır otomatik olarak kapatma veya eşleşen bir bitiş etiketi vardır.
 *  Yöntem Grup 'object' türü temsilci için GenericMethod' dönüştürülemiyor. Yöntemini çağırmak istiyordunuz?' 
 
Genel yöntem çağrılarını Sarmalanan, içinde bir [açık Razor ifade](#explicit-razor-expressions) veya [Razor kod bloğunu](#razor-code-blocks).

## <a name="explicit-razor-expressions"></a>Açık Razor ifadeleri

Açık Razor ifadeleri oluşur bir `@` dengeli parantez simgesiyle. Geçen haftaki zaman işlemek için aşağıdaki Razor biçimlendirme kullanılır:

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

İçinde herhangi bir içerik `@()` parantez değerlendirilir ve çıktıyı çizilir.

Örtük ifadeleri, önceki bölümde açıklanan genellikle boşluk içeremez. Aşağıdaki kodda bir hafta geçerli saatten çıkarılır değil:

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

Aşağıdaki HTML kod işler:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

Açık ifadeler bir ifade sonucu metinle birleştirmek için kullanılabilir:

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

Açık ifade olmadan `<p>Age@joe.Age</p>` bir e-posta adresi olarak kabul edilir ve `<p>Age@joe.Age</p>` işlenir. Açık bir ifade olarak yazılırken `<p>Age33</p>` işlenir.

Açık ifadeleri, genel yöntemleri çıktısını işlemek için kullanılabilir *.cshtml* dosyaları. Aşağıdaki biçimlendirmede gösterilen hatayı düzeltmek gösterilmiştir daha önce bir C# genel köşeli parantez neden oldu. Kod açık bir ifade olarak yazılır:

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a>İfade kodlama

Bir dizeyi değerlendirmek C# HTML kodlu ifadelerini. Değerlendirmek için C# ifadeleri `IHtmlContent` doğrudan ile işlenir `IHtmlContent.WriteTo`. Değerlendirme yok C# ifadeleri `IHtmlContent` tarafından bir dizeye dönüştürülür `ToString` ve işlenen önce kodlanır.

```cshtml
@("<span>Hello World</span>")
```

Aşağıdaki HTML kod işler:

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

HTML tarayıcıda gösterilir:

```
<span>Hello World</span>
```

`HtmlHelper.Raw` Çıktı kodlanmış değildir, ancak geri HTML biçimlendirmesi olarak çizilir.

> [!WARNING]
> Kullanarak `HtmlHelper.Raw` unsanitized kullanıcı girişi bir güvenlik riskidir. Kullanıcı girişi kötü amaçlı JavaScript veya diğer açıkları içerebilir. Kullanıcı girişi temizleme zordur. Kullanmaktan kaçının `HtmlHelper.Raw` kullanıcı girişi ile.

```cshtml
@Html.Raw("<span>Hello World</span>")
```

Aşağıdaki HTML kod işler:

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Razor kod blokları

Razor kod blokları başlayarak `@` ve tarafından içine `{}`. İfadeler C# kodu kod blokları içinde çizilir değil. Kod blokları bir görünüm ifadelerinde aynı kapsamı paylaşabilir ve sıraya göre tanımlanır:

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

Aşağıdaki HTML kod işler:

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a>Örtük geçişleri

Kod bloğu varsayılan dil C# olduğu, ancak Razor sayfasını HTML olarak geçiş yapabilir:

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>Açık sınırlandırılmış geçiş

HTML oluşturması gerektiğini bir kod bloğunun alt tanımlamak için Razor ile işleme için karakterleri arasında  **\<metin >** etiketi:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Bir HTML etiketi tarafından çevrelenen olmayan HTML oluşturmak için bu yaklaşımı kullanın. Bir HTML veya Razor etiketi bir Razor çalışma zamanı hatası oluşur.

**\<Metin >** etiketi, içeriği işlenirken boşluk denetlemek yararlıdır:

* Yalnızca arasında içerik  **\<metin >** etiketi işlenir. 
* Hiçbir boşluk önce veya sonra  **\<metin >** etiketi HTML çıkışında görünür.

### <a name="explicit-line-transition-with-"></a>Açık satır geçişle @:

Tam bir satırın geri kalanı bir kod bloğunun içine HTML olarak işlemek için kullanmak `@:` sözdizimi:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Olmadan `@:` kodda bir Razor çalışma zamanı hatası oluşturulur.

Uyarı: Ek `@` Razor dosyasının karakter bloğu içinde deyimleri derleyici hataları neden olabilir. Bu derleyici hataları gerçek hata önce bildirilen hata oluştuğundan anlaşılması zor olabilir. Bu hata, bir tek bir kod bloğu birden çok dolaylı/açık ifadelere birleştirme sonra yaygındır.

## <a name="control-structures"></a>Denetim yapıları

Denetim yapıları kod blokları bir uzantı var. Ayrıca kod blokları (geçiş biçimlendirmesi, satır içi C#) tüm yönlerini aşağıdaki yapıları için geçerlidir:

### <a name="conditionals-if-else-if-else-and-switch"></a>Koşulları @if, başka ise, başka ve @switch

`@if` kod çalıştığında denetimleri:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else` ve `else if` gerektirmeyen `@` simgesi:

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

Aşağıdaki biçimlendirmede switch deyimi kullanmayı gösterir:

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

### <a name="looping-for-foreach-while-and-do-while"></a>Döngü @for, @foreach, @while, ve @do sırada

Şablonlu HTML denetim ifadeleri döngü ile oluşturulabilir. Kişilerin listesini oluşturmak için:

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

Aşağıdaki döngü ifadeleri desteklenir:

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

### <a name="compound-using"></a>Bileşik @using

C# ' ta, bir `using` deyimi, bir nesne kapatılır emin olmak için kullanılır. Razor aynı mekanizmayı ek içeriklere sahip bir HTML Yardımcıları oluşturmak için kullanılır. Aşağıdaki kodda bir form etiketi HTML Yardımcıları işlemek `@using` deyimi:


```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

Kapsam düzeyinde Eylemler ile yapılabilir [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).

### <a name="try-catch-finally"></a>@try, son olarak, yakalama

Özel durum işleme, C# benzer:

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

Razor kritik bölümler kilit deyimleri ile koruma özelliği vardır:

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Açıklamalar

Razor C# ve HTML açıklamaları destekler:

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

Aşağıdaki HTML kod işler:

```html
<!-- HTML comment -->
```

Web sayfası işlenmeden önce razor açıklama sunucu tarafından kaldırıldı. Razor kullanan `@*  *@` açıklamaları sınırlandırmak için. Sunucu tüm biçimlendirmesi oluşturmak olmayan şekilde aşağıdaki kod, kılınmıştır:

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

Ayrılmış anahtar sözcükler aşağıdaki örtük ifadelerle ile temsil Razor yönergeleri `@` simgesi. Bir yönerge genellikle bir görünüm ayrıştırılır veya farklı işlevler sağlayan şeklini değiştirir.

Razor kod bir görünümün nasıl oluşturur anlama yönergeleri nasıl çalıştığını anlamak kolaylaştırır.

[!code-html[](razor/sample/Views/Home/Contact8.cshtml)]

Kod aşağıdakine benzer bir sınıf oluşturur:

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

Bu makalede, bölümün sonraki [bir görünümü için oluşturulan Razor C# sınıfı görüntüleme](#viewing-the-razor-c-class-generated-for-a-view) bu oluşturulan sınıf görüntülemek açıklanmaktadır.

<a name="using"></a>
### <a name="using"></a>@using

`@using` Yönergesi ekler C# `using` yönerge oluşturulan görüntülemek için:

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

`@model` Yönergesi bir görünüme iletilen model türünü belirtir:

```cshtml
@model TypeNameOfModel
```

Bireysel kullanıcı hesapları ile oluşturulmuş bir ASP.NET Core MVC uygulamasında *Views/Account/Login.cshtml* görünümü aşağıdaki model bildirimi içerir:

```cshtml
@model LoginViewModel
```

Oluşturulan sınıf devraldığı `RazorPage<dynamic>`:

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Razor kullanıma sunan bir `Model` model erişmek için özelliği geçirilen görünümü:

```cshtml
<div>The Login Email: @Model.Email</div>
```

`@model` Yönergesi bu özelliğin türünü belirtir. Yönergeyi belirtir `T` içinde `RazorPage<T>` türeyen görünümü oluşturulan, sınıfın. Varsa `@model` yönergesi değil belirtilen, `Model` özelliği türüdür `dynamic`. Model değeri denetleyicisinden görünümüne geçirilir. Daha fazla bilgi için bkz: [modelleri'kesin türü belirtilmiş ve &commat;model anahtar sözcük](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).

### <a name="inherits"></a>@inherits

`@inherits` Yönergesi görünümü devralır sınıfının tam denetim sağlar:

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

Aşağıdaki kod bir özel Razor sayfa türüdür:

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

`CustomText` Bir görünümde görüntülenir:

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

Aşağıdaki HTML kod işler:

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 `@model` ve `@inherits` aynı görünümünde kullanılabilir. `@inherits` kullanılabilir bir *_viewımports.cshtml* görünümü alır dosyası:

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

Aşağıdaki kod, kesin türü belirtilmiş görünüm örneğidir:

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

Varsa "rick@contoso.com" geçirilen modeldeki görünümünü aşağıdaki HTML biçimlendirme oluşturur:

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


`@inject` Yönergesi sağlayan bir hizmetinden eklemesine Razor sayfasını [hizmet kapsayıcısı](xref:fundamentals/dependency-injection) görünüm. Daha fazla bilgi için bkz: [bağımlılık ekleme görünümleri içine](xref:mvc/views/dependency-injection).

### <a name="functions"></a>@functions

`@functions` Yönergesi C# kod bloğu bir görünüme eklemek bir Razor sayfasını sağlar:

```cshtml
@functions { // C# Code }
```

Örneğin:

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

Kod aşağıdaki HTML biçimlendirmeleri oluşturur:

```html
<div>From method: Hello</div>
```

Aşağıdaki kodu oluşturulan Razor C# sınıf verilmiştir:

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

`@section` Yönergesi ile birlikte kullanılan [düzeni](xref:mvc/views/layout) içeriği HTML sayfası farklı bölümlerini işlemek görünümleri etkinleştirmek için. Daha fazla bilgi için bkz: [bölümleri](xref:mvc/views/layout#layout-sections-label).

## <a name="tag-helpers"></a>Etiket Yardımcıları

İlgilidir üç yönergeleri vardır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).

| Yönergesi | İşlev |
| --------- | -------- |
| [&commat;addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | Etiket Yardımcıları bir görünüm için kullanılabilir hale getirir. |
| [&commat;removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | Etiket Yardımcıları daha önce eklediğiniz bir görünümden kaldırır. |
| [&commat;tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | Etiket Yardımcısı desteğinin etkinleştirmek ve etiket Yardımcısı kullanım açık yapmak için etiket öneki belirtir. |

## <a name="razor-reserved-keywords"></a>Razor ayrılmış anahtar sözcükler

### <a name="razor-keywords"></a>Razor anahtar sözcükler

* Sayfa (ASP.NET Core 2.0 ve üzeri gerektirir)
* ad alanı
* işlevleri
* devralır
* model
* section
* yardımcı (ASP.NET Core tarafından şu anda desteklenmiyor)

Razor anahtar sözcükleri ile kaçışlı `@(Razor Keyword)` (örneğin, `@(functions)`).

### <a name="c-razor-keywords"></a>C# Razor anahtar sözcükler

* büyük/küçük harf
* do
* default
* for
* foreach
* if
* else
* lock
* anahtarı
* Deneyin
* Yakalama
* finally
* kullanma
* while

C# Razor anahtar sözcükler, çift kaçışlı ile olmalıdır `@(@C# Razor Keyword)` (örneğin, `@(@case)`). İlk `@` Razor ayrıştırıcısı çıkışları. İkinci `@` C# ayrıştırıcı çıkışları.

### <a name="reserved-keywords-not-used-by-razor"></a>Razor tarafından kullanılmayan ayrılmış anahtar sözcükler

* sınıf

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>Bir görünümü için oluşturulan Razor C# sınıfı görüntüleme

Aşağıdaki sınıf ASP.NET Core MVC projenize ekleyin:

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

Geçersiz kılma `RazorTemplateEngine` MVC tarafından eklenen `CustomTemplateEngine` sınıfı:

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

Bir kesme noktası ayarlayın `return csharpDocument` deyiminin `CustomTemplateEngine`. Program yürütme kesme noktasına durduğunda değerini görüntülemek `generatedCode`.

![GeneratedCode metin Görselleştirici görünümü](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a>Görünüm aramaları ve büyük/küçük harfe duyarlılık

Razor görüntüleme altyapısı için görünümleri büyük küçük harfe duyarlı arama gerçekleştirir. Ancak, gerçek arama temel alınan dosya sistemi tarafından belirlenir:

* Dosya tabanlı kaynağı: 
  * Büyük küçük harfe duyarlı dosya sistemleri (örneğin, Windows) ile işletim sistemlerinde, fiziksel dosya sağlayıcısı aramaları büyük küçük harfe duyarlı. Örneğin, `return View("Test")` için eşleşen sonuçlanıyor */Views/Home/Test.cshtml*, */Views/home/test.cshtml*ve diğer büyük/küçük harf değişken.
  * Büyük küçük harfe duyarlı dosya sistemlerindeki (örneğin, Linux, OSX ile `EmbeddedFileProvider`), arama duyarlıdır. Örneğin, `return View("Test")` özellikle eşleşmeleri */Views/Home/Test.cshtml*.
* Görünümleri önceden derlenmiş: önceden derlenmiş görünümleri arayan ASP.NET Core ile 2.0 ve üzeri, servis talebi tüm işletim sistemlerinde harflere duyarlı değildir. Davranış Windows fiziksel dosya sağlayıcının davranışı aynıdır. İki önceden derlenmiş görünüm yalnızca durumda farklıysa, arama sonucu belirleyici değildir.

Geliştiriciler, büyük/küçük harf, büyük küçük harf dosya ve dizin adlarını eşleştirmek için önerilir:

    * Alan, denetleyici ve eylem adları. 
    * Razor sayfalarının.
    
Servis talebi eşleşen dağıtımları temeldeki dosya sistemi bağımsız olarak kendi görünümler Bul sağlar.
