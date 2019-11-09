---
title: ASP.NET Core için Razor söz dizimi başvurusu
author: rick-anderson
description: Sunucu tabanlı kodu Web sayfalarına eklemek için Razor biçimlendirme sözdizimi hakkında bilgi edinin.
ms.author: riande
ms.date: 09/28/2019
uid: mvc/views/razor
ms.openlocfilehash: d8d686c23ea61950947798f213c9846058f1812e
ms.sourcegitcommit: 4818385c3cfe0805e15138a2c1785b62deeaab90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2019
ms.locfileid: "73896907"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a><span data-ttu-id="cda28-103">ASP.NET Core için Razor söz dizimi başvurusu</span><span class="sxs-lookup"><span data-stu-id="cda28-103">Razor syntax reference for ASP.NET Core</span></span>

<span data-ttu-id="cda28-104">[Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen)ve [dan vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="cda28-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="cda28-105">Razor, Web sayfalarına sunucu tabanlı kod eklemeye yönelik bir biçimlendirme sözdizimidir.</span><span class="sxs-lookup"><span data-stu-id="cda28-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="cda28-106">Razor söz dizimi Razor işaretlemesi, C#ve HTML 'den oluşur.</span><span class="sxs-lookup"><span data-stu-id="cda28-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="cda28-107">Razor içeren dosyalar genellikle *. cshtml* dosya uzantısına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="cda28-107">Files containing Razor generally have a *.cshtml* file extension.</span></span> <span data-ttu-id="cda28-108">Razor, [Razor bileşenleri](xref:blazor/components) dosyalarında ( *. Razor*) de bulunur.</span><span class="sxs-lookup"><span data-stu-id="cda28-108">Razor is also found in [Razor components](xref:blazor/components) files (*.razor*).</span></span>

## <a name="rendering-html"></a><span data-ttu-id="cda28-109">HTML işleniyor</span><span class="sxs-lookup"><span data-stu-id="cda28-109">Rendering HTML</span></span>

<span data-ttu-id="cda28-110">Varsayılan Razor dili HTML 'dir.</span><span class="sxs-lookup"><span data-stu-id="cda28-110">The default Razor language is HTML.</span></span> <span data-ttu-id="cda28-111">Razor işaretlemesinin HTML işleme HTML dosyasından HTML işlemeden farklı değildir.</span><span class="sxs-lookup"><span data-stu-id="cda28-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="cda28-112">*. Cshtml* Razor dosyalarındaki HTML işaretlemesi sunucu tarafından değiştirilmeden işlenir.</span><span class="sxs-lookup"><span data-stu-id="cda28-112">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="cda28-113">Razor söz dizimi</span><span class="sxs-lookup"><span data-stu-id="cda28-113">Razor syntax</span></span>

<span data-ttu-id="cda28-114">Razor, C# HTML 'den ' ye C#geçiş yapmak için `@` sembolünü destekler ve kullanır.</span><span class="sxs-lookup"><span data-stu-id="cda28-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="cda28-115">Razor ifadeleri C# değerlendirir ve bunları HTML çıktısında işler.</span><span class="sxs-lookup"><span data-stu-id="cda28-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="cda28-116">Bir `@` sembol sonrasında [Razor ayrılmış bir anahtar sözcük](#razor-reserved-keywords)olduğunda, bu, Razor 'e özgü işaretlere geçer.</span><span class="sxs-lookup"><span data-stu-id="cda28-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="cda28-117">Aksi halde, düz C#olarak geçiş yapar.</span><span class="sxs-lookup"><span data-stu-id="cda28-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="cda28-118">Razor biçimlendirmesinde bir `@` sembolünden çıkmak için ikinci bir `@` sembol kullanın:</span><span class="sxs-lookup"><span data-stu-id="cda28-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="cda28-119">Kod, HTML 'de tek bir `@` simgesiyle işlenir:</span><span class="sxs-lookup"><span data-stu-id="cda28-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="cda28-120">HTML öznitelikleri ve e-posta adreslerini içeren içerikler `@` sembolünü geçiş karakteri olarak değerlendirmez.</span><span class="sxs-lookup"><span data-stu-id="cda28-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="cda28-121">Aşağıdaki örnekteki e-posta adresleri Razor ayrıştırma tarafından dokunmaz:</span><span class="sxs-lookup"><span data-stu-id="cda28-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="cda28-122">Örtük Razor ifadeleri</span><span class="sxs-lookup"><span data-stu-id="cda28-122">Implicit Razor expressions</span></span>

<span data-ttu-id="cda28-123">Örtük Razor ifadeleri `@` sonrasında C# kod ile başlar:</span><span class="sxs-lookup"><span data-stu-id="cda28-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="cda28-124">C# `await` anahtar sözcüğünün dışında, örtük ifadeler boşluk içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="cda28-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="cda28-125">C# Deyimde Temizleme işlemi varsa, boşluklar ıntermingled olabilir:</span><span class="sxs-lookup"><span data-stu-id="cda28-125">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="cda28-126">Parantez içinde (`<>`C# ) karakterler bir HTML etiketi olarak yorumlandığından **örtük ifadeler genel türler içeremez.**</span><span class="sxs-lookup"><span data-stu-id="cda28-126">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="cda28-127">Aşağıdaki kod geçerli **değil** :</span><span class="sxs-lookup"><span data-stu-id="cda28-127">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="cda28-128">Yukarıdaki kod, aşağıdakilerden birine benzer bir derleyici hatası oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cda28-128">The preceding code generates a compiler error similar to one of the following:</span></span>

* <span data-ttu-id="cda28-129">"İnt" öğesi kapatılmamış.</span><span class="sxs-lookup"><span data-stu-id="cda28-129">The "int" element wasn't closed.</span></span> <span data-ttu-id="cda28-130">Tüm öğeler kendi kendini kapatıyor ya da eşleşen bir bitiş etiketine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="cda28-130">All elements must be either self-closing or have a matching end tag.</span></span>
* <span data-ttu-id="cda28-131">' GenericMethod ' Yöntem grubu temsilci olmayan ' Object ' türüne dönüştürülemiyor.</span><span class="sxs-lookup"><span data-stu-id="cda28-131">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="cda28-132">Yöntemi çağırmak mı istiyordunuz? "</span><span class="sxs-lookup"><span data-stu-id="cda28-132">Did you intend to invoke the method?\`</span></span>

<span data-ttu-id="cda28-133">Genel metot çağrılarının [açık bir Razor ifadesinde](#explicit-razor-expressions) veya [Razor kodu bloğunda](#razor-code-blocks)sarmalanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="cda28-133">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="cda28-134">Açık Razor ifadeleri</span><span class="sxs-lookup"><span data-stu-id="cda28-134">Explicit Razor expressions</span></span>

<span data-ttu-id="cda28-135">Açık Razor ifadeleri, dengeli parantezle bir `@` sembolünden oluşur.</span><span class="sxs-lookup"><span data-stu-id="cda28-135">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="cda28-136">Son haftanın saatini işlemek için aşağıdaki Razor işaretlemesi kullanılır:</span><span class="sxs-lookup"><span data-stu-id="cda28-136">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="cda28-137">`@()` parantez içindeki içerikler değerlendirilir ve çıkışa işlenir.</span><span class="sxs-lookup"><span data-stu-id="cda28-137">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="cda28-138">Önceki bölümde açıklanan örtük ifadeler, genellikle boşluk içeremez.</span><span class="sxs-lookup"><span data-stu-id="cda28-138">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="cda28-139">Aşağıdaki kodda, bir hafta geçerli saatten çıkarılır:</span><span class="sxs-lookup"><span data-stu-id="cda28-139">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="cda28-140">Kod, aşağıdaki HTML 'yi işler:</span><span class="sxs-lookup"><span data-stu-id="cda28-140">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="cda28-141">Açık ifadeler, metni bir ifade sonucuyla birleştirmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="cda28-141">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="cda28-142">Açık ifade olmadan, `<p>Age@joe.Age</p>` bir e-posta adresi olarak kabul edilir ve `<p>Age@joe.Age</p>` işlenir.</span><span class="sxs-lookup"><span data-stu-id="cda28-142">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="cda28-143">Açık bir ifade olarak yazıldığında `<p>Age33</p>` işlenir.</span><span class="sxs-lookup"><span data-stu-id="cda28-143">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<span data-ttu-id="cda28-144">Açık ifadeler, *. cshtml* dosyalarındaki genel metotlardan çıkış oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cda28-144">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="cda28-145">Aşağıdaki biçimlendirme, C# genel parantezinin neden olduğu daha önce gösterilen hatayı nasıl düzeltebileceğiniz gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="cda28-145">The following markup shows how to correct the error shown earlier caused by the brackets of a C# generic.</span></span> <span data-ttu-id="cda28-146">Kod açık bir ifade olarak yazılır:</span><span class="sxs-lookup"><span data-stu-id="cda28-146">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="cda28-147">İfade kodlaması</span><span class="sxs-lookup"><span data-stu-id="cda28-147">Expression encoding</span></span>

<span data-ttu-id="cda28-148">C#dizeyi değerlendiren ifadeler HTML olarak kodlanır.</span><span class="sxs-lookup"><span data-stu-id="cda28-148">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="cda28-149">C#`IHtmlContent` değerlendiren ifadeler doğrudan `IHtmlContent.WriteTo`üzerinden işlenir.</span><span class="sxs-lookup"><span data-stu-id="cda28-149">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="cda28-150">C#`IHtmlContent` olarak değerlendirilmeyen ifadeler, `ToString` tarafından bir dizeye dönüştürülür ve işlenmeden önce kodlanır.</span><span class="sxs-lookup"><span data-stu-id="cda28-150">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="cda28-151">Kod, aşağıdaki HTML 'yi işler:</span><span class="sxs-lookup"><span data-stu-id="cda28-151">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="cda28-152">HTML tarayıcıda şu şekilde gösterilir:</span><span class="sxs-lookup"><span data-stu-id="cda28-152">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="cda28-153">`HtmlHelper.Raw` çıktısı kodlanmamış, ancak HTML işaretlemesi olarak işlendi.</span><span class="sxs-lookup"><span data-stu-id="cda28-153">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="cda28-154">Ayıklanmış Kullanıcı girişinde `HtmlHelper.Raw` kullanmak bir güvenlik riskidir.</span><span class="sxs-lookup"><span data-stu-id="cda28-154">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="cda28-155">Kullanıcı girişi kötü amaçlı JavaScript veya diğer kötüye kullanım içerebilir.</span><span class="sxs-lookup"><span data-stu-id="cda28-155">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="cda28-156">Kullanıcı girişinin Temizleme işlemi zordur.</span><span class="sxs-lookup"><span data-stu-id="cda28-156">Sanitizing user input is difficult.</span></span> <span data-ttu-id="cda28-157">Kullanıcı girişiyle `HtmlHelper.Raw` kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="cda28-157">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="cda28-158">Kod, aşağıdaki HTML 'yi işler:</span><span class="sxs-lookup"><span data-stu-id="cda28-158">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="cda28-159">Razor kod blokları</span><span class="sxs-lookup"><span data-stu-id="cda28-159">Razor code blocks</span></span>

<span data-ttu-id="cda28-160">Razor kodu blokları `@` başlar ve `{}`alınır.</span><span class="sxs-lookup"><span data-stu-id="cda28-160">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="cda28-161">İfadelerin aksine, C# kod blokları içindeki kod işlenmez.</span><span class="sxs-lookup"><span data-stu-id="cda28-161">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="cda28-162">Bir görünümdeki kod blokları ve ifadeler aynı kapsamı paylaşır ve sırasıyla tanımlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="cda28-162">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="cda28-163">Kod, aşağıdaki HTML 'yi işler:</span><span class="sxs-lookup"><span data-stu-id="cda28-163">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cda28-164">Kod blokları ' nda, şablon oluşturma yöntemleri olarak kullanılacak biçimlendirme ile [yerel işlevler](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) bildirin:</span><span class="sxs-lookup"><span data-stu-id="cda28-164">In code blocks, declare [local functions](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) with markup to serve as templating methods:</span></span>

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

<span data-ttu-id="cda28-165">Kod, aşağıdaki HTML 'yi işler:</span><span class="sxs-lookup"><span data-stu-id="cda28-165">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="implicit-transitions"></a><span data-ttu-id="cda28-166">Örtük geçişler</span><span class="sxs-lookup"><span data-stu-id="cda28-166">Implicit transitions</span></span>

<span data-ttu-id="cda28-167">Bir kod bloğundaki varsayılan dil C#, ancak Razor sayfası HTML 'ye geri dönebilir:</span><span class="sxs-lookup"><span data-stu-id="cda28-167">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="cda28-168">Açık sınırlı geçiş</span><span class="sxs-lookup"><span data-stu-id="cda28-168">Explicit delimited transition</span></span>

<span data-ttu-id="cda28-169">HTML oluşturması gereken bir kod bloğunun alt bölümünü tanımlamak için, karakterleri Razor `<text>` etiketiyle çevreleyin:</span><span class="sxs-lookup"><span data-stu-id="cda28-169">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor `<text>` tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="cda28-170">HTML etiketiyle çevrelenmiş HTML 'yi işlemek için bu yaklaşımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="cda28-170">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="cda28-171">HTML veya Razor etiketi olmadan Razor çalışma zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="cda28-171">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="cda28-172">`<text>` etiketi, içerik işlerken boşluğu denetlemek için yararlıdır:</span><span class="sxs-lookup"><span data-stu-id="cda28-172">The `<text>` tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="cda28-173">Yalnızca `<text>` etiketi arasındaki içerik işlenir.</span><span class="sxs-lookup"><span data-stu-id="cda28-173">Only the content between the `<text>` tag is rendered.</span></span>
* <span data-ttu-id="cda28-174">HTML çıktısında `<text>` etiketi görüntülenmeden önce veya sonra boşluk yok.</span><span class="sxs-lookup"><span data-stu-id="cda28-174">No whitespace before or after the `<text>` tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition"></a><span data-ttu-id="cda28-175">Açık satır geçişi</span><span class="sxs-lookup"><span data-stu-id="cda28-175">Explicit line transition</span></span>

<span data-ttu-id="cda28-176">Tüm satırın geri kalanını bir kod bloğu içinde HTML olarak işlemek için `@:` söz dizimini kullanın:</span><span class="sxs-lookup"><span data-stu-id="cda28-176">To render the rest of an entire line as HTML inside a code block, use `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="cda28-177">Kodda `@:` olmadan bir Razor çalışma zamanı hatası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cda28-177">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="cda28-178">Razor dosyasındaki ek `@` karakterler, bloktaki daha sonra bulunan deyimlerde derleyici hatalarına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="cda28-178">Extra `@` characters in a Razor file can cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="cda28-179">Bu derleyici hatalarının anlaşılması zor olabilir çünkü asıl hata bildirilen hatadan önce oluşur.</span><span class="sxs-lookup"><span data-stu-id="cda28-179">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="cda28-180">Bu hata, birden çok örtük/açık ifade tek bir kod bloğu içinde birleştirildikten sonra ortaktır.</span><span class="sxs-lookup"><span data-stu-id="cda28-180">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="cda28-181">Denetim yapıları</span><span class="sxs-lookup"><span data-stu-id="cda28-181">Control structures</span></span>

<span data-ttu-id="cda28-182">Denetim yapıları, kod bloklarının bir uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="cda28-182">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="cda28-183">Kod bloklarının (biçimlendirme, satır içi C#) tüm yönleri de aşağıdaki yapılar için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="cda28-183">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="cda28-184">Conditionals, else if, Else ve \@Switch \@</span><span class="sxs-lookup"><span data-stu-id="cda28-184">Conditionals \@if, else if, else, and \@switch</span></span>

<span data-ttu-id="cda28-185">Kod çalıştığında denetimleri `@if`:</span><span class="sxs-lookup"><span data-stu-id="cda28-185">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="cda28-186">`else` ve `else if` `@` sembolünü gerektirmez:</span><span class="sxs-lookup"><span data-stu-id="cda28-186">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="cda28-187">Aşağıdaki biçimlendirme bir switch ifadesinin nasıl kullanılacağını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="cda28-187">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="cda28-188">İçin döngü \@, \@foreach, \@sırasında ve \@do</span><span class="sxs-lookup"><span data-stu-id="cda28-188">Looping \@for, \@foreach, \@while, and \@do while</span></span>

<span data-ttu-id="cda28-189">Şablonlu HTML, döngü denetim deyimleri ile oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="cda28-189">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="cda28-190">Bir kişi listesini işlemek için:</span><span class="sxs-lookup"><span data-stu-id="cda28-190">To render a list of people:</span></span>

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

<span data-ttu-id="cda28-191">Aşağıdaki döngü deyimleri desteklenir:</span><span class="sxs-lookup"><span data-stu-id="cda28-191">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="cda28-192">Kullanarak bileşik \@</span><span class="sxs-lookup"><span data-stu-id="cda28-192">Compound \@using</span></span>

<span data-ttu-id="cda28-193">' C#De, bir nesnenin atıldığından emin olmak için `using` bir ifade kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cda28-193">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="cda28-194">Razor 'de, ek içerik içeren HTML Yardımcıları oluşturmak için aynı mekanizma kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cda28-194">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="cda28-195">Aşağıdaki kodda, HTML Yardımcıları `@using` ifadesiyle bir `<form>` etiketi işlerler:</span><span class="sxs-lookup"><span data-stu-id="cda28-195">In the following code, HTML Helpers render a `<form>` tag with the `@using` statement:</span></span>

```cshtml
@using (Html.BeginForm())
{
    <div>
        Email: <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

### <a name="try-catch-finally"></a><span data-ttu-id="cda28-196">\@TRY, catch, finally</span><span class="sxs-lookup"><span data-stu-id="cda28-196">\@try, catch, finally</span></span>

<span data-ttu-id="cda28-197">Özel durum işleme şuna benzerdir C#:</span><span class="sxs-lookup"><span data-stu-id="cda28-197">Exception handling is similar to C#:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a><span data-ttu-id="cda28-198">\@kilidi</span><span class="sxs-lookup"><span data-stu-id="cda28-198">\@lock</span></span>

<span data-ttu-id="cda28-199">Razor, kilit deyimleriyle önemli bölümleri koruma özelliğine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="cda28-199">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="cda28-200">Açıklamalar</span><span class="sxs-lookup"><span data-stu-id="cda28-200">Comments</span></span>

<span data-ttu-id="cda28-201">Razor desteği C# ve HTML açıklamaları:</span><span class="sxs-lookup"><span data-stu-id="cda28-201">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="cda28-202">Kod, aşağıdaki HTML 'yi işler:</span><span class="sxs-lookup"><span data-stu-id="cda28-202">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="cda28-203">Razor açıklamaları, Web sayfası işlenmeden önce sunucu tarafından kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="cda28-203">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="cda28-204">Razor açıklamaları sınırlandırmak için `@*  *@` kullanır.</span><span class="sxs-lookup"><span data-stu-id="cda28-204">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="cda28-205">Aşağıdaki kod açıklama olarak belirlenir, bu nedenle sunucu herhangi bir biçimlendirme oluşturmaz:</span><span class="sxs-lookup"><span data-stu-id="cda28-205">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="cda28-206">Yönergeler</span><span class="sxs-lookup"><span data-stu-id="cda28-206">Directives</span></span>

<span data-ttu-id="cda28-207">Razor yönergeleri `@` sembolünü takip eden ayrılmış anahtar sözcüklerle örtük ifadelerle temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="cda28-207">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="cda28-208">Yönerge genellikle görünümün ayrıştırılma şeklini değiştirir veya farklı işlevleri sunar.</span><span class="sxs-lookup"><span data-stu-id="cda28-208">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="cda28-209">Razor 'nin bir görünüm için nasıl kod üretdiğini anlamak, yönergelerin nasıl çalıştığını anlamayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="cda28-209">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="cda28-210">Kod, aşağıdakine benzer bir sınıf oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cda28-210">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="cda28-211">Bu makalenin ilerleyen kısımlarında, [bir görünüm için oluşturulan Razor C# sınıfını İnceleme](#inspect-the-razor-c-class-generated-for-a-view) bölümünde bu oluşturulan sınıfın nasıl görüntüleneceği açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="cda28-211">Later in this article, the section [Inspect the Razor C# class generated for a view](#inspect-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="attribute"></a><span data-ttu-id="cda28-212">\@özniteliği</span><span class="sxs-lookup"><span data-stu-id="cda28-212">\@attribute</span></span>

<span data-ttu-id="cda28-213">`@attribute` yönergesi, verilen özniteliği oluşturulan sayfanın veya görünümün sınıfına ekler.</span><span class="sxs-lookup"><span data-stu-id="cda28-213">The `@attribute` directive adds the given attribute to the class of the generated page or view.</span></span> <span data-ttu-id="cda28-214">Aşağıdaki örnek `[Authorize]` özniteliğini ekler:</span><span class="sxs-lookup"><span data-stu-id="cda28-214">The following example adds the `[Authorize]` attribute:</span></span>

```cshtml
@attribute [Authorize]
```

::: moniker range=">= aspnetcore-3.0"

### <a name="code"></a><span data-ttu-id="cda28-215">\@kodu</span><span class="sxs-lookup"><span data-stu-id="cda28-215">\@code</span></span>

<span data-ttu-id="cda28-216">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="cda28-216">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="cda28-217">`@code` bloğu bir [Razor bileşeninin](xref:blazor/components) bir bileşene üye ( C# alanlar, Özellikler ve Yöntemler) eklemesini sağlar:</span><span class="sxs-lookup"><span data-stu-id="cda28-217">The `@code` block enables a [Razor component](xref:blazor/components) to add C# members (fields, properties, and methods) to a component:</span></span>

```cshtml
@code {
    // C# members (fields, properties, and methods)
}
```

<span data-ttu-id="cda28-218">Razor bileşenleri için, `@code` [@functions](#functions) diğer adı `@functions`üzerinde önerilir.</span><span class="sxs-lookup"><span data-stu-id="cda28-218">For Razor components, `@code` is an alias of [@functions](#functions) and recommended over `@functions`.</span></span> <span data-ttu-id="cda28-219">Birden fazla `@code` bloğu izin verilir.</span><span class="sxs-lookup"><span data-stu-id="cda28-219">More than one `@code` block is permissible.</span></span>

::: moniker-end

### <a name="functions"></a><span data-ttu-id="cda28-220">\@işlevleri</span><span class="sxs-lookup"><span data-stu-id="cda28-220">\@functions</span></span>

<span data-ttu-id="cda28-221">`@functions` yönergesi, oluşturulan sınıfa C# üye (alanlar, Özellikler ve Yöntemler) eklemeyi sağlar:</span><span class="sxs-lookup"><span data-stu-id="cda28-221">The `@functions` directive enables adding C# members (fields, properties, and methods) to the generated class:</span></span>

```cshtml
@functions {
    // C# members (fields, properties, and methods)
}
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cda28-222">[Razor bileşenlerinde](xref:blazor/components), üye eklemek C# için `@functions` üzerinde `@code` kullanın.</span><span class="sxs-lookup"><span data-stu-id="cda28-222">In [Razor components](xref:blazor/components), use `@code` over `@functions` to add C# members.</span></span>

::: moniker-end

<span data-ttu-id="cda28-223">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cda28-223">For example:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="cda28-224">Kod, aşağıdaki HTML işaretlemesini oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cda28-224">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="cda28-225">Aşağıdaki kod, üretilen Razor C# sınıfıdır:</span><span class="sxs-lookup"><span data-stu-id="cda28-225">The following code is the generated Razor C# class:</span></span>

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cda28-226">`@functions` Yöntemler, biçimlendirme olduğunda şablon oluşturma yöntemleri olarak görev yapar:</span><span class="sxs-lookup"><span data-stu-id="cda28-226">`@functions` methods serve as templating methods when they have markup:</span></span>

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

<span data-ttu-id="cda28-227">Kod, aşağıdaki HTML 'yi işler:</span><span class="sxs-lookup"><span data-stu-id="cda28-227">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

### <a name="implements"></a><span data-ttu-id="cda28-228">\@uygular</span><span class="sxs-lookup"><span data-stu-id="cda28-228">\@implements</span></span>

<span data-ttu-id="cda28-229">`@implements` yönergesi, oluşturulan sınıf için bir arabirim uygular.</span><span class="sxs-lookup"><span data-stu-id="cda28-229">The `@implements` directive implements an interface for the generated class.</span></span>

<span data-ttu-id="cda28-230">Aşağıdaki örnek, <xref:System.IDisposable.Dispose*> yönteminin çağrılabilmesi için <xref:System.IDisposable?displayProperty=fullName> uygular:</span><span class="sxs-lookup"><span data-stu-id="cda28-230">The following example implements <xref:System.IDisposable?displayProperty=fullName> so that the <xref:System.IDisposable.Dispose*> method can be called:</span></span>

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

### <a name="inherits"></a><span data-ttu-id="cda28-231">\@devralır</span><span class="sxs-lookup"><span data-stu-id="cda28-231">\@inherits</span></span>

<span data-ttu-id="cda28-232">`@inherits` yönergesi, görünümün devraldığı sınıfın tam denetimini sağlar:</span><span class="sxs-lookup"><span data-stu-id="cda28-232">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="cda28-233">Aşağıdaki kod özel bir Razor sayfası türüdür:</span><span class="sxs-lookup"><span data-stu-id="cda28-233">The following code is a custom Razor page type:</span></span>

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="cda28-234">`CustomText` bir görünümde görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="cda28-234">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="cda28-235">Kod, aşağıdaki HTML 'yi işler:</span><span class="sxs-lookup"><span data-stu-id="cda28-235">The code renders the following HTML:</span></span>

```html
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

 <span data-ttu-id="cda28-236">`@model` ve `@inherits` aynı görünümde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cda28-236">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="cda28-237">`@inherits`, görünümün içeri aktardığı bir *_ViewImports. cshtml* dosyasında olabilir:</span><span class="sxs-lookup"><span data-stu-id="cda28-237">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="cda28-238">Aşağıdaki kod, türü kesin belirlenmiş bir görünümün örneğidir:</span><span class="sxs-lookup"><span data-stu-id="cda28-238">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="cda28-239">Modelde "rick@contoso.com" geçirilirse, Görünüm aşağıdaki HTML işaretlemesini oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cda28-239">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

### <a name="inject"></a><span data-ttu-id="cda28-240">\@ekleme</span><span class="sxs-lookup"><span data-stu-id="cda28-240">\@inject</span></span>

<span data-ttu-id="cda28-241">`@inject` yönergesi, Razor sayfasının hizmet [kapsayıcısından](xref:fundamentals/dependency-injection) bir hizmeti bir görünüme eklemesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="cda28-241">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="cda28-242">Daha fazla bilgi için bkz. [görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cda28-242">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="layout"></a><span data-ttu-id="cda28-243">\@düzeni</span><span class="sxs-lookup"><span data-stu-id="cda28-243">\@layout</span></span>

<span data-ttu-id="cda28-244">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="cda28-244">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="cda28-245">`@layout` yönergesi Razor bileşeninin bir yerleşimini belirtir.</span><span class="sxs-lookup"><span data-stu-id="cda28-245">The `@layout` directive specifies a layout for a Razor component.</span></span> <span data-ttu-id="cda28-246">Düzen bileşenleri kod yinelemeyi ve tutarsızlığın önüne geçmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cda28-246">Layout components are used to avoid code duplication and inconsistency.</span></span> <span data-ttu-id="cda28-247">Daha fazla bilgi için bkz. <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="cda28-247">For more information, see <xref:blazor/layouts>.</span></span>

::: moniker-end

### <a name="model"></a><span data-ttu-id="cda28-248">\@modeli</span><span class="sxs-lookup"><span data-stu-id="cda28-248">\@model</span></span>

<span data-ttu-id="cda28-249">*Bu senaryo yalnızca MVC görünümleri ve Razor Pages (. cshtml) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="cda28-249">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="cda28-250">`@model` yönergesi, bir görünüme veya sayfaya geçirilen modelin türünü belirtir:</span><span class="sxs-lookup"><span data-stu-id="cda28-250">The `@model` directive specifies the type of the model passed to a view or page:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="cda28-251">Bir ASP.NET Core MVC veya bireysel kullanıcı hesaplarıyla oluşturulan Razor Pages uygulama, *Görünümler/Account/Login. cshtml* aşağıdaki model bildirimini içerir:</span><span class="sxs-lookup"><span data-stu-id="cda28-251">In an ASP.NET Core MVC or Razor Pages app created with individual user accounts, *Views/Account/Login.cshtml* contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="cda28-252">Oluşturulan sınıf `RazorPage<dynamic>`devralır:</span><span class="sxs-lookup"><span data-stu-id="cda28-252">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="cda28-253">Razor, görünüme geçirilen modele erişim için bir `Model` özelliği kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="cda28-253">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="cda28-254">`@model` yönergesi `Model` özelliğinin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="cda28-254">The `@model` directive specifies the type of the `Model` property.</span></span> <span data-ttu-id="cda28-255">Yönergesi, görünümün türettiği oluşturulan sınıfın `RazorPage<T>` `T` belirtir.</span><span class="sxs-lookup"><span data-stu-id="cda28-255">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="cda28-256">`@model` yönergesi belirtilmemişse, `Model` özelliği `dynamic`türündedir.</span><span class="sxs-lookup"><span data-stu-id="cda28-256">If the `@model` directive isn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="cda28-257">Daha fazla bilgi için bkz. [türü kesin belirlenmiş modeller ve @model anahtar sözcüğü](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span><span class="sxs-lookup"><span data-stu-id="cda28-257">For more information, see [Strongly typed models and the @model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span></span>

### <a name="namespace"></a><span data-ttu-id="cda28-258">\@ad alanı</span><span class="sxs-lookup"><span data-stu-id="cda28-258">\@namespace</span></span>

<span data-ttu-id="cda28-259">`@namespace` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="cda28-259">The `@namespace` directive:</span></span>

* <span data-ttu-id="cda28-260">Oluşturulan Razor sayfası, MVC görünümü veya Razor bileşeni sınıfının ad alanını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="cda28-260">Sets the namespace of the class of the generated Razor page, MVC view, or Razor component.</span></span>
* <span data-ttu-id="cda28-261">Bir sayfa, görünüm veya bileşen sınıfının kök türetilmiş ad alanlarını dizin ağacındaki en yakın içeri aktarmalar dosyasından, *_ViewImports. cshtml* (görünümler veya sayfalar) veya *_Imports. Razor* (Razor bileşenleri) olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="cda28-261">Sets the root derived namespaces of a pages, views, or components classes from the closest imports file in the directory tree, *_ViewImports.cshtml* (views or pages) or *_Imports.razor* (Razor components).</span></span>

```cshtml
@namespace Your.Namespace.Here
```

<span data-ttu-id="cda28-262">Aşağıdaki tabloda gösterilen Razor Pages örneği için:</span><span class="sxs-lookup"><span data-stu-id="cda28-262">For the Razor Pages example shown in the following table:</span></span>

* <span data-ttu-id="cda28-263">Her sayfa *sayfaları/_ViewImports. cshtml*'yi içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="cda28-263">Each page imports *Pages/_ViewImports.cshtml*.</span></span>
* <span data-ttu-id="cda28-264">*Pages/_ViewImports. cshtml* `@namespace Hello.World`içerir.</span><span class="sxs-lookup"><span data-stu-id="cda28-264">*Pages/_ViewImports.cshtml* contains `@namespace Hello.World`.</span></span>
* <span data-ttu-id="cda28-265">Her sayfa, ad alanının kökü olarak `Hello.World` sahiptir.</span><span class="sxs-lookup"><span data-stu-id="cda28-265">Each page has `Hello.World` as the root of it's namespace.</span></span>

| <span data-ttu-id="cda28-266">Sayfasında</span><span class="sxs-lookup"><span data-stu-id="cda28-266">Page</span></span>                                        | <span data-ttu-id="cda28-267">Ad Alanı</span><span class="sxs-lookup"><span data-stu-id="cda28-267">Namespace</span></span>                             |
| ------------------------------------------- | ------------------------------------- |
| <span data-ttu-id="cda28-268">*Pages/Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="cda28-268">*Pages/Index.cshtml*</span></span>                        | `Hello.World`                         |
| <span data-ttu-id="cda28-269">*Sayfa/fazla sayfa/sayfa. cshtml*</span><span class="sxs-lookup"><span data-stu-id="cda28-269">*Pages/MorePages/Page.cshtml*</span></span>               | `Hello.World.MorePages`               |
| <span data-ttu-id="cda28-270">*Pages/te Pages/Evente Pages/Page. cshtml*</span><span class="sxs-lookup"><span data-stu-id="cda28-270">*Pages/MorePages/EvenMorePages/Page.cshtml*</span></span> | `Hello.World.MorePages.EvenMorePages` |

<span data-ttu-id="cda28-271">Yukarıdaki ilişkiler, MVC görünümleri ve Razor bileşenleriyle kullanılan dosyaları içeri aktarmak için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="cda28-271">The preceding relationships apply to import files used with MVC views and Razor components.</span></span>

<span data-ttu-id="cda28-272">Birden çok içeri aktarma dosyası `@namespace` yönergesine sahip olduğunda, kök ad alanını ayarlamak için dizin ağacındaki sayfaya, görünüme veya bileşene en yakın dosya kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cda28-272">When multiple import files have a `@namespace` directive, the file closest to the page, view, or component in the directory tree is used to set the root namespace.</span></span>

<span data-ttu-id="cda28-273">Yukarıdaki örnekteki *evente Pages* klasörünün `@namespace Another.Planet` bir içeri aktarmalar dosyası varsa (veya *sayfa/değer sayfaları/Evente Pages/Page. cshtml* dosyası `@namespace Another.Planet`içeriyorsa), sonuç aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="cda28-273">If the *EvenMorePages* folder in the preceding example has an imports file with `@namespace Another.Planet` (or the *Pages/MorePages/EvenMorePages/Page.cshtml* file contains `@namespace Another.Planet`), the result is shown in the following table.</span></span>

| <span data-ttu-id="cda28-274">Sayfasında</span><span class="sxs-lookup"><span data-stu-id="cda28-274">Page</span></span>                                        | <span data-ttu-id="cda28-275">Ad Alanı</span><span class="sxs-lookup"><span data-stu-id="cda28-275">Namespace</span></span>               |
| ------------------------------------------- | ----------------------- |
| <span data-ttu-id="cda28-276">*Pages/Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="cda28-276">*Pages/Index.cshtml*</span></span>                        | `Hello.World`           |
| <span data-ttu-id="cda28-277">*Sayfa/fazla sayfa/sayfa. cshtml*</span><span class="sxs-lookup"><span data-stu-id="cda28-277">*Pages/MorePages/Page.cshtml*</span></span>               | `Hello.World.MorePages` |
| <span data-ttu-id="cda28-278">*Pages/te Pages/Evente Pages/Page. cshtml*</span><span class="sxs-lookup"><span data-stu-id="cda28-278">*Pages/MorePages/EvenMorePages/Page.cshtml*</span></span> | `Another.Planet`        |

### <a name="page"></a><span data-ttu-id="cda28-279">\@sayfası</span><span class="sxs-lookup"><span data-stu-id="cda28-279">\@page</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cda28-280">`@page` yönergesinin göründüğü dosyanın türüne bağlı olarak farklı etkileri vardır.</span><span class="sxs-lookup"><span data-stu-id="cda28-280">The `@page` directive has different effects depending on the type of the file where it appears.</span></span> <span data-ttu-id="cda28-281">Yönergesi:</span><span class="sxs-lookup"><span data-stu-id="cda28-281">The directive:</span></span>

* <span data-ttu-id="cda28-282">İçindeki bir *. cshtml* dosyasında, dosyanın bir Razor sayfası olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="cda28-282">In in a *.cshtml* file indicates that the file is a Razor Page.</span></span> <span data-ttu-id="cda28-283">Daha fazla bilgi için bkz. [özel rotalar](xref:razor-pages/index#custom-routes) ve <xref:razor-pages/index>.</span><span class="sxs-lookup"><span data-stu-id="cda28-283">For more information, see [Custom routes](xref:razor-pages/index#custom-routes) and <xref:razor-pages/index>.</span></span>
* <span data-ttu-id="cda28-284">Bir Razor bileşeninin istekleri doğrudan işlemesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="cda28-284">Specifies that a Razor component should handle requests directly.</span></span> <span data-ttu-id="cda28-285">Daha fazla bilgi için bkz. <xref:blazor/routing>.</span><span class="sxs-lookup"><span data-stu-id="cda28-285">For more information, see <xref:blazor/routing>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="cda28-286">Bir *. cshtml* dosyasının ilk satırındaki `@page` yönergesi, dosyanın bir Razor sayfası olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="cda28-286">The `@page` directive on the first line of a *.cshtml* file indicates that the file is a Razor Page.</span></span> <span data-ttu-id="cda28-287">Daha fazla bilgi için bkz. <xref:razor-pages/index>.</span><span class="sxs-lookup"><span data-stu-id="cda28-287">For more information, see <xref:razor-pages/index>.</span></span>

::: moniker-end

### <a name="section"></a><span data-ttu-id="cda28-288">\@bölümü</span><span class="sxs-lookup"><span data-stu-id="cda28-288">\@section</span></span>

<span data-ttu-id="cda28-289">*Bu senaryo yalnızca MVC görünümleri ve Razor Pages (. cshtml) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="cda28-289">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="cda28-290">`@section` yönergesi, görünümler veya sayfaların HTML sayfasının farklı bölümlerinde içerik işlemesini sağlamak için [MVC ve Razor Pages düzenleriyle](xref:mvc/views/layout) birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cda28-290">The `@section` directive is used in conjunction with [MVC and Razor Pages layouts](xref:mvc/views/layout) to enable views or pages to render content in different parts of the HTML page.</span></span> <span data-ttu-id="cda28-291">Daha fazla bilgi için bkz. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="cda28-291">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="using"></a><span data-ttu-id="cda28-292">kullanarak \@</span><span class="sxs-lookup"><span data-stu-id="cda28-292">\@using</span></span>

<span data-ttu-id="cda28-293">`@using` yönergesi, oluşturulan görünüme C# `using` yönergesini ekler:</span><span class="sxs-lookup"><span data-stu-id="cda28-293">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cda28-294">[Razor bileşenlerinde](xref:blazor/components)`@using` Ayrıca hangi bileşenlerin kapsamda olduğunu denetler.</span><span class="sxs-lookup"><span data-stu-id="cda28-294">In [Razor components](xref:blazor/components), `@using` also controls which components are in scope.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="directive-attributes"></a><span data-ttu-id="cda28-295">Yönerge öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="cda28-295">Directive attributes</span></span>

### <a name="attributes"></a><span data-ttu-id="cda28-296">\@öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="cda28-296">\@attributes</span></span>

<span data-ttu-id="cda28-297">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="cda28-297">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="cda28-298">`@attributes`, bir bileşenin bildirilmeyen öznitelikleri işlemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="cda28-298">`@attributes` allows a component to render non-declared attributes.</span></span> <span data-ttu-id="cda28-299">Daha fazla bilgi için bkz. <xref:blazor/components#attribute-splatting-and-arbitrary-parameters>.</span><span class="sxs-lookup"><span data-stu-id="cda28-299">For more information, see <xref:blazor/components#attribute-splatting-and-arbitrary-parameters>.</span></span>

### <a name="bind"></a><span data-ttu-id="cda28-300">\@bağlama</span><span class="sxs-lookup"><span data-stu-id="cda28-300">\@bind</span></span>

<span data-ttu-id="cda28-301">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="cda28-301">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="cda28-302">Bileşenlerdeki veri bağlama `@bind` özniteliğiyle gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="cda28-302">Data binding in components is accomplished with the `@bind` attribute.</span></span> <span data-ttu-id="cda28-303">Daha fazla bilgi için bkz. <xref:blazor/components#data-binding>.</span><span class="sxs-lookup"><span data-stu-id="cda28-303">For more information, see <xref:blazor/components#data-binding>.</span></span>

### <a name="onevent"></a><span data-ttu-id="cda28-304">{Event} üzerinde \@</span><span class="sxs-lookup"><span data-stu-id="cda28-304">\@on{event}</span></span>

<span data-ttu-id="cda28-305">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="cda28-305">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="cda28-306">Razor, bileşenler için olay işleme özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="cda28-306">Razor provides event handling features for components.</span></span> <span data-ttu-id="cda28-307">Daha fazla bilgi için bkz. <xref:blazor/components#event-handling>.</span><span class="sxs-lookup"><span data-stu-id="cda28-307">For more information, see <xref:blazor/components#event-handling>.</span></span>

### <a name="key"></a><span data-ttu-id="cda28-308">\@anahtarı</span><span class="sxs-lookup"><span data-stu-id="cda28-308">\@key</span></span>

<span data-ttu-id="cda28-309">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="cda28-309">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="cda28-310">`@key` Directive özniteliği, bileşenlerin, anahtar değerine göre öğelerin veya bileşenlerin korunmasını güvence altına almasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="cda28-310">The `@key` directive attribute causes the components diffing algorithm to guarantee preservation of elements or components based on the key's value.</span></span> <span data-ttu-id="cda28-311">Daha fazla bilgi için bkz. <xref:blazor/components#use-key-to-control-the-preservation-of-elements-and-components>.</span><span class="sxs-lookup"><span data-stu-id="cda28-311">For more information, see <xref:blazor/components#use-key-to-control-the-preservation-of-elements-and-components>.</span></span>

### <a name="ref"></a><span data-ttu-id="cda28-312">\@ref</span><span class="sxs-lookup"><span data-stu-id="cda28-312">\@ref</span></span>

<span data-ttu-id="cda28-313">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="cda28-313">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="cda28-314">Bileşen başvuruları (`@ref`) bir bileşen örneğine başvurmak için bir yol sağlar, böylece bu örneğe komut verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cda28-314">Component references (`@ref`) provide a way to reference a component instance so that you can issue commands to that instance.</span></span> <span data-ttu-id="cda28-315">Daha fazla bilgi için bkz. <xref:blazor/components#capture-references-to-components>.</span><span class="sxs-lookup"><span data-stu-id="cda28-315">For more information, see <xref:blazor/components#capture-references-to-components>.</span></span>

### <a name="typeparam"></a><span data-ttu-id="cda28-316">\@typeparam</span><span class="sxs-lookup"><span data-stu-id="cda28-316">\@typeparam</span></span>

<span data-ttu-id="cda28-317">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="cda28-317">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="cda28-318">`@typeparam` yönergesi, oluşturulan bileşen sınıfı için genel bir tür parametresi bildirir.</span><span class="sxs-lookup"><span data-stu-id="cda28-318">The `@typeparam` directive declares a generic type parameter for the generated component class.</span></span> <span data-ttu-id="cda28-319">Daha fazla bilgi için bkz. <xref:blazor/components#generic-typed-components>.</span><span class="sxs-lookup"><span data-stu-id="cda28-319">For more information, see <xref:blazor/components#generic-typed-components>.</span></span>

::: moniker-end

## <a name="templated-razor-delegates"></a><span data-ttu-id="cda28-320">Şablonlu Razor temsilcileri</span><span class="sxs-lookup"><span data-stu-id="cda28-320">Templated Razor delegates</span></span>

<span data-ttu-id="cda28-321">Razor şablonları aşağıdaki biçimde bir UI parçacığı tanımlamanızı sağlar:</span><span class="sxs-lookup"><span data-stu-id="cda28-321">Razor templates allow you to define a UI snippet with the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="cda28-322">Aşağıdaki örnekte, <xref:System.Func%602>olarak şablonlu bir Razor temsilcisinin nasıl belirtildiği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="cda28-322">The following example illustrates how to specify a templated Razor delegate as a <xref:System.Func%602>.</span></span> <span data-ttu-id="cda28-323">[Dinamik tür](/dotnet/csharp/programming-guide/types/using-type-dynamic) , temsilcinin sarmalayan yönteminin parametresi için belirtilir.</span><span class="sxs-lookup"><span data-stu-id="cda28-323">The [dynamic type](/dotnet/csharp/programming-guide/types/using-type-dynamic) is specified for the parameter of the method that the delegate encapsulates.</span></span> <span data-ttu-id="cda28-324">Bir [nesne türü](/dotnet/csharp/language-reference/keywords/object) , temsilcinin dönüş değeri olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="cda28-324">An [object type](/dotnet/csharp/language-reference/keywords/object) is specified as the return value of the delegate.</span></span> <span data-ttu-id="cda28-325">Şablon, bir `Name` özelliğine sahip `Pet` <xref:System.Collections.Generic.List%601> birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cda28-325">The template is used with a <xref:System.Collections.Generic.List%601> of `Pet` that has a `Name` property.</span></span>

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

<span data-ttu-id="cda28-326">Şablon, bir `foreach` ifadesiyle sağlanan `pets` ile işlenir:</span><span class="sxs-lookup"><span data-stu-id="cda28-326">The template is rendered with `pets` supplied by a `foreach` statement:</span></span>

```cshtml
@foreach (var pet in pets)
{
    @petTemplate(pet)
}
```

<span data-ttu-id="cda28-327">İşlenmiş çıkış:</span><span class="sxs-lookup"><span data-stu-id="cda28-327">Rendered output:</span></span>

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

<span data-ttu-id="cda28-328">Ayrıca bir yönteme bağımsız değişken olarak bir satır içi Razor şablonu da sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cda28-328">You can also supply an inline Razor template as an argument to a method.</span></span> <span data-ttu-id="cda28-329">Aşağıdaki örnekte `Repeat` yöntemi bir Razor şablonu alır.</span><span class="sxs-lookup"><span data-stu-id="cda28-329">In the following example, the `Repeat` method receives a Razor template.</span></span> <span data-ttu-id="cda28-330">Yöntemi, bir listeden sağlanan öğelerin tekrarlandığı HTML içeriğini oluşturmak için şablonunu kullanır:</span><span class="sxs-lookup"><span data-stu-id="cda28-330">The method uses the template to produce HTML content with repeats of items supplied from a list:</span></span>

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

<span data-ttu-id="cda28-331">Önceki örnekteki Evcil hayvanlar 'ların listesini kullanarak `Repeat` yöntemi ile çağrılır:</span><span class="sxs-lookup"><span data-stu-id="cda28-331">Using the list of pets from the prior example, the `Repeat` method is called with:</span></span>

* <span data-ttu-id="cda28-332">`Pet`<xref:System.Collections.Generic.List%601>.</span><span class="sxs-lookup"><span data-stu-id="cda28-332"><xref:System.Collections.Generic.List%601> of `Pet`.</span></span>
* <span data-ttu-id="cda28-333">Her evcil hayvan kaç kez tekrarlanacak.</span><span class="sxs-lookup"><span data-stu-id="cda28-333">Number of times to repeat each pet.</span></span>
* <span data-ttu-id="cda28-334">Sıralanmamış bir listenin liste öğeleri için kullanılacak satır içi şablon.</span><span class="sxs-lookup"><span data-stu-id="cda28-334">Inline template to use for the list items of an unordered list.</span></span>

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

<span data-ttu-id="cda28-335">İşlenmiş çıkış:</span><span class="sxs-lookup"><span data-stu-id="cda28-335">Rendered output:</span></span>

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

## <a name="tag-helpers"></a><span data-ttu-id="cda28-336">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="cda28-336">Tag Helpers</span></span>

<span data-ttu-id="cda28-337">*Bu senaryo yalnızca MVC görünümleri ve Razor Pages (. cshtml) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="cda28-337">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="cda28-338">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)ile ilgili üç yönergeler vardır.</span><span class="sxs-lookup"><span data-stu-id="cda28-338">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="cda28-339">Deki</span><span class="sxs-lookup"><span data-stu-id="cda28-339">Directive</span></span> | <span data-ttu-id="cda28-340">İşlev</span><span class="sxs-lookup"><span data-stu-id="cda28-340">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="cda28-341">Etiket yardımcılarını bir görünüm için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="cda28-341">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="cda28-342">Daha önce bir görünümden eklenen etiket yardımcıları kaldırır.</span><span class="sxs-lookup"><span data-stu-id="cda28-342">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="cda28-343">Etiket Yardımcısı desteğini etkinleştirmek ve etiket Yardımcısı kullanımını açık hale getirmek için bir etiket ön eki belirtir.</span><span class="sxs-lookup"><span data-stu-id="cda28-343">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="cda28-344">Razor ayrılmış anahtar sözcükler</span><span class="sxs-lookup"><span data-stu-id="cda28-344">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="cda28-345">Razor anahtar sözcükleri</span><span class="sxs-lookup"><span data-stu-id="cda28-345">Razor keywords</span></span>

* <span data-ttu-id="cda28-346">sayfa (ASP.NET Core 2,1 veya üzeri bir sürüm gerektirir)</span><span class="sxs-lookup"><span data-stu-id="cda28-346">page (Requires ASP.NET Core 2.1 or later)</span></span>
* <span data-ttu-id="cda28-347">ad alanı</span><span class="sxs-lookup"><span data-stu-id="cda28-347">namespace</span></span>
* <span data-ttu-id="cda28-348">işlevleri</span><span class="sxs-lookup"><span data-stu-id="cda28-348">functions</span></span>
* <span data-ttu-id="cda28-349">alıp</span><span class="sxs-lookup"><span data-stu-id="cda28-349">inherits</span></span>
* <span data-ttu-id="cda28-350">model</span><span class="sxs-lookup"><span data-stu-id="cda28-350">model</span></span>
* <span data-ttu-id="cda28-351">section</span><span class="sxs-lookup"><span data-stu-id="cda28-351">section</span></span>
* <span data-ttu-id="cda28-352">yardımcı (Şu anda ASP.NET Core tarafından desteklenmiyor)</span><span class="sxs-lookup"><span data-stu-id="cda28-352">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="cda28-353">Razor anahtar kelimelerinde `@(Razor Keyword)` (örneğin, `@(functions)`) yok edilir.</span><span class="sxs-lookup"><span data-stu-id="cda28-353">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="cda28-354">C#Razor anahtar sözcükleri</span><span class="sxs-lookup"><span data-stu-id="cda28-354">C# Razor keywords</span></span>

* <span data-ttu-id="cda28-355">büyük/küçük harf</span><span class="sxs-lookup"><span data-stu-id="cda28-355">case</span></span>
* <span data-ttu-id="cda28-356">do</span><span class="sxs-lookup"><span data-stu-id="cda28-356">do</span></span>
* <span data-ttu-id="cda28-357">default</span><span class="sxs-lookup"><span data-stu-id="cda28-357">default</span></span>
* <span data-ttu-id="cda28-358">for</span><span class="sxs-lookup"><span data-stu-id="cda28-358">for</span></span>
* <span data-ttu-id="cda28-359">foreach</span><span class="sxs-lookup"><span data-stu-id="cda28-359">foreach</span></span>
* <span data-ttu-id="cda28-360">if</span><span class="sxs-lookup"><span data-stu-id="cda28-360">if</span></span>
* <span data-ttu-id="cda28-361">else</span><span class="sxs-lookup"><span data-stu-id="cda28-361">else</span></span>
* <span data-ttu-id="cda28-362">lock</span><span class="sxs-lookup"><span data-stu-id="cda28-362">lock</span></span>
* <span data-ttu-id="cda28-363">anahtarı</span><span class="sxs-lookup"><span data-stu-id="cda28-363">switch</span></span>
* <span data-ttu-id="cda28-364">almaya</span><span class="sxs-lookup"><span data-stu-id="cda28-364">try</span></span>
* <span data-ttu-id="cda28-365">yakalaya</span><span class="sxs-lookup"><span data-stu-id="cda28-365">catch</span></span>
* <span data-ttu-id="cda28-366">finally</span><span class="sxs-lookup"><span data-stu-id="cda28-366">finally</span></span>
* <span data-ttu-id="cda28-367">kullanma</span><span class="sxs-lookup"><span data-stu-id="cda28-367">using</span></span>
* <span data-ttu-id="cda28-368">while</span><span class="sxs-lookup"><span data-stu-id="cda28-368">while</span></span>

<span data-ttu-id="cda28-369">C#Razor anahtar kelimelerinde `@(@C# Razor Keyword)` (örneğin, `@(@case)`) çift kaçış olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="cda28-369">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="cda28-370">İlk `@` Razor ayrıştırıcısının çıkar.</span><span class="sxs-lookup"><span data-stu-id="cda28-370">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="cda28-371">İkinci `@` C# Ayrıştırıcıdan çıkar.</span><span class="sxs-lookup"><span data-stu-id="cda28-371">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="cda28-372">Razor tarafından kullanılmayan ayrılmış anahtar sözcükler</span><span class="sxs-lookup"><span data-stu-id="cda28-372">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="cda28-373">sınıf</span><span class="sxs-lookup"><span data-stu-id="cda28-373">class</span></span>

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="cda28-374">Bir görünüm için C# oluşturulan Razor sınıfını inceleyin</span><span class="sxs-lookup"><span data-stu-id="cda28-374">Inspect the Razor C# class generated for a view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="cda28-375">.NET Core SDK 2,1 veya üzeri bir sürümde, [Razor SDK](xref:razor-pages/sdk) Razor dosyalarının derlemesini işler.</span><span class="sxs-lookup"><span data-stu-id="cda28-375">With .NET Core SDK 2.1 or later, the [Razor SDK](xref:razor-pages/sdk) handles compilation of Razor files.</span></span> <span data-ttu-id="cda28-376">Bir proje oluştururken, Razor SDK proje kökünde bir *obj/< build_configuration >/< target_framework_moniker >/Razor* dizini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cda28-376">When building a project, the Razor SDK generates an *obj/<build_configuration>/<target_framework_moniker>/Razor* directory in the project root.</span></span> <span data-ttu-id="cda28-377">*Razor* dizini içindeki dizin yapısı, projenin dizin yapısını yansıtır.</span><span class="sxs-lookup"><span data-stu-id="cda28-377">The directory structure within the *Razor* directory mirrors the project's directory structure.</span></span>

<span data-ttu-id="cda28-378">.NET Core 2,1 ' i hedefleyen bir ASP.NET Core 2,1 Razor Pages proje için aşağıdaki dizin yapısını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="cda28-378">Consider the following directory structure in an ASP.NET Core 2.1 Razor Pages project targeting .NET Core 2.1:</span></span>

* <span data-ttu-id="cda28-379">**Alanları**</span><span class="sxs-lookup"><span data-stu-id="cda28-379">**Areas/**</span></span>
  * <span data-ttu-id="cda28-380">**Yöneticileri**</span><span class="sxs-lookup"><span data-stu-id="cda28-380">**Admin/**</span></span>
    * <span data-ttu-id="cda28-381">**Sayfaları**</span><span class="sxs-lookup"><span data-stu-id="cda28-381">**Pages/**</span></span>
      * <span data-ttu-id="cda28-382">*Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="cda28-382">*Index.cshtml*</span></span>
      * <span data-ttu-id="cda28-383">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="cda28-383">*Index.cshtml.cs*</span></span>
* <span data-ttu-id="cda28-384">**Sayfaları**</span><span class="sxs-lookup"><span data-stu-id="cda28-384">**Pages/**</span></span>
  * <span data-ttu-id="cda28-385">**Paylaşılan**</span><span class="sxs-lookup"><span data-stu-id="cda28-385">**Shared/**</span></span>
    * <span data-ttu-id="cda28-386">*_Layout. cshtml*</span><span class="sxs-lookup"><span data-stu-id="cda28-386">*_Layout.cshtml*</span></span>
  * <span data-ttu-id="cda28-387">*_ViewImports. cshtml*</span><span class="sxs-lookup"><span data-stu-id="cda28-387">*_ViewImports.cshtml*</span></span>
  * <span data-ttu-id="cda28-388">*_ViewStart. cshtml*</span><span class="sxs-lookup"><span data-stu-id="cda28-388">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="cda28-389">*Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="cda28-389">*Index.cshtml*</span></span>
  * <span data-ttu-id="cda28-390">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="cda28-390">*Index.cshtml.cs*</span></span>

<span data-ttu-id="cda28-391">Projenin *hata ayıklama* yapılandırmasında oluşturulması aşağıdaki *obj* dizinini verir:</span><span class="sxs-lookup"><span data-stu-id="cda28-391">Building the project in *Debug* configuration yields the following *obj* directory:</span></span>

* <span data-ttu-id="cda28-392">**nesnesi**</span><span class="sxs-lookup"><span data-stu-id="cda28-392">**obj/**</span></span>
  * <span data-ttu-id="cda28-393">**H**</span><span class="sxs-lookup"><span data-stu-id="cda28-393">**Debug/**</span></span>
    * <span data-ttu-id="cda28-394">**netcoreapp 2.1/**</span><span class="sxs-lookup"><span data-stu-id="cda28-394">**netcoreapp2.1/**</span></span>
      * <span data-ttu-id="cda28-395">**Razor**</span><span class="sxs-lookup"><span data-stu-id="cda28-395">**Razor/**</span></span>
        * <span data-ttu-id="cda28-396">**Alanları**</span><span class="sxs-lookup"><span data-stu-id="cda28-396">**Areas/**</span></span>
          * <span data-ttu-id="cda28-397">**Yöneticileri**</span><span class="sxs-lookup"><span data-stu-id="cda28-397">**Admin/**</span></span>
            * <span data-ttu-id="cda28-398">**Sayfaları**</span><span class="sxs-lookup"><span data-stu-id="cda28-398">**Pages/**</span></span>
              * <span data-ttu-id="cda28-399">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="cda28-399">*Index.g.cshtml.cs*</span></span>
        * <span data-ttu-id="cda28-400">**Sayfaları**</span><span class="sxs-lookup"><span data-stu-id="cda28-400">**Pages/**</span></span>
          * <span data-ttu-id="cda28-401">**Paylaşılan**</span><span class="sxs-lookup"><span data-stu-id="cda28-401">**Shared/**</span></span>
            * <span data-ttu-id="cda28-402">*_Layout. g. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="cda28-402">*_Layout.g.cshtml.cs*</span></span>
          * <span data-ttu-id="cda28-403">*_ViewImports. g. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="cda28-403">*_ViewImports.g.cshtml.cs*</span></span>
          * <span data-ttu-id="cda28-404">*_ViewStart. g. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="cda28-404">*_ViewStart.g.cshtml.cs*</span></span>
          * <span data-ttu-id="cda28-405">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="cda28-405">*Index.g.cshtml.cs*</span></span>

<span data-ttu-id="cda28-406">*Pages/Index. cshtml*için oluşturulan sınıfı görüntülemek için, *obj/Debug/netcoreapp 2.1/Razor/Pages/Index. g. cshtml. cs*dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="cda28-406">To view the generated class for *Pages/Index.cshtml*, open *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="cda28-407">Aşağıdaki Sınıfı ASP.NET Core MVC projesine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="cda28-407">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="cda28-408">`Startup.ConfigureServices`, MVC tarafından `CustomTemplateEngine` sınıfıyla eklenen `RazorTemplateEngine` geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="cda28-408">In `Startup.ConfigureServices`, override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="cda28-409">`CustomTemplateEngine``return csharpDocument;` bildiriminde bir kesme noktası ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="cda28-409">Set a breakpoint on the `return csharpDocument;` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="cda28-410">Kesme noktasında program yürütmesi durdurulduğunda, `generatedCode`değerini görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="cda28-410">When program execution stops at the breakpoint, view the value of `generatedCode`.</span></span>

![GeneratedCode 'ın metin görselleştiricisi görünümü](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="cda28-412">Aramaları ve büyük/küçük harf duyarlılığını görüntüle</span><span class="sxs-lookup"><span data-stu-id="cda28-412">View lookups and case sensitivity</span></span>

<span data-ttu-id="cda28-413">Razor Görünüm altyapısı, görünümler için büyük/küçük harfe duyarlı aramalar gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="cda28-413">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="cda28-414">Ancak gerçek arama, temel alınan dosya sistemine göre belirlenir:</span><span class="sxs-lookup"><span data-stu-id="cda28-414">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="cda28-415">Dosya tabanlı kaynak:</span><span class="sxs-lookup"><span data-stu-id="cda28-415">File based source:</span></span>
  * <span data-ttu-id="cda28-416">Büyük/küçük harf duyarsız dosya sistemlerine (örneğin, Windows) sahip işletim sistemlerinde, fiziksel dosya sağlayıcısı aramaları büyük/küçük harfe duyarsızdır.</span><span class="sxs-lookup"><span data-stu-id="cda28-416">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="cda28-417">Örneğin, `return View("Test")` */views/Home/test.exe*, */views/Home/test.exe*ve diğer tüm büyük/küçük harf çeşitlerüyle sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="cda28-417">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="cda28-418">Büyük/küçük harfe duyarlı dosya sistemlerinde (örneğin, Linux, OSX ve `EmbeddedFileProvider`), aramalar büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="cda28-418">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="cda28-419">Örneğin, `return View("Test")` özellikle */views/Home/test.exe. cshtml*ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="cda28-419">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="cda28-420">Önceden derlenmiş görünümler: ASP.NET Core 2,0 ve üzeri sürümlerde, önceden derlenmiş görünümleri aramak tüm işletim sistemlerinde büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="cda28-420">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="cda28-421">Davranış, fiziksel dosya sağlayıcısının Windows 'daki davranışlarıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="cda28-421">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="cda28-422">Ön derlenmiş iki görünüm yalnızca bir durumda farklıysa, arama sonucu belirleyici değildir.</span><span class="sxs-lookup"><span data-stu-id="cda28-422">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="cda28-423">Geliştiricilerin dosya ve dizin adlarını büyük küçük harf olarak eşleşmesi önerilir:</span><span class="sxs-lookup"><span data-stu-id="cda28-423">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

* <span data-ttu-id="cda28-424">Alan, denetleyici ve eylem adları.</span><span class="sxs-lookup"><span data-stu-id="cda28-424">Area, controller, and action names.</span></span>
* <span data-ttu-id="cda28-425">Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="cda28-425">Razor Pages.</span></span>

<span data-ttu-id="cda28-426">Eşleşen durum, dağıtımların görünümlerini temel alınan dosya sisteminden bağımsız olarak bulmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="cda28-426">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
