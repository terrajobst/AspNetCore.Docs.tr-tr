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
# <a name="razor-syntax-reference-for-aspnet-core"></a><span data-ttu-id="67287-103">ASP.NET Core Razor söz dizimi başvurusu</span><span class="sxs-lookup"><span data-stu-id="67287-103">Razor syntax reference for ASP.NET Core</span></span>

<span data-ttu-id="67287-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), ve [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="67287-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="67287-105">Razor kod sunucu tabanlı Web sayfalarını eklemek için bir biçimlendirme sözdizimi aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="67287-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="67287-106">Razor işaretlemesi Razor sözdizimini oluşur C#ve HTML.</span><span class="sxs-lookup"><span data-stu-id="67287-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="67287-107">Razor genellikle içeren dosyalarınız bir *.cshtml* dosya uzantısı.</span><span class="sxs-lookup"><span data-stu-id="67287-107">Files containing Razor generally have a *.cshtml* file extension.</span></span> <span data-ttu-id="67287-108">Razor, [Razor bileşenleri](xref:blazor/components) dosyalarında ( *. Razor*) de bulunur.</span><span class="sxs-lookup"><span data-stu-id="67287-108">Razor is also found in [Razor components](xref:blazor/components) files (*.razor*).</span></span>

## <a name="rendering-html"></a><span data-ttu-id="67287-109">HTML işleme</span><span class="sxs-lookup"><span data-stu-id="67287-109">Rendering HTML</span></span>

<span data-ttu-id="67287-110">HTML varsayılan Razor dilidir.</span><span class="sxs-lookup"><span data-stu-id="67287-110">The default Razor language is HTML.</span></span> <span data-ttu-id="67287-111">HTML Razor işaretlemesi işleme bir HTML dosyasından HTML'yi işlemeye değerinden farklı değildir.</span><span class="sxs-lookup"><span data-stu-id="67287-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="67287-112">HTML biçimlendirmede *.cshtml* Razor dosyaları değiştirilmemiş sunucu tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="67287-112">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="67287-113">Razor sözdizimi</span><span class="sxs-lookup"><span data-stu-id="67287-113">Razor syntax</span></span>

<span data-ttu-id="67287-114">Razor destekler C# ve kullandığı `@` HTML geçiş sembolünden C#.</span><span class="sxs-lookup"><span data-stu-id="67287-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="67287-115">Razor değerlendirir C# ifadeleri ve bunları HTML çıktısında oluşturur.</span><span class="sxs-lookup"><span data-stu-id="67287-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="67287-116">Olduğunda bir `@` sembol tarafından izlenen bir [Razor ayrılmış anahtar sözcüğü](#razor-reserved-keywords), Razor özgü biçimlendirme geçer.</span><span class="sxs-lookup"><span data-stu-id="67287-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="67287-117">Aksi takdirde düz geçiş C#.</span><span class="sxs-lookup"><span data-stu-id="67287-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="67287-118">Kaçış için bir `@` sembol Razor işaretlemede, ikinci bir kullanın `@` sembol:</span><span class="sxs-lookup"><span data-stu-id="67287-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="67287-119">Kodunu HTML ile tek bir işlenen `@` sembol:</span><span class="sxs-lookup"><span data-stu-id="67287-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="67287-120">Olmayan HTML öznitelikleri ve e-posta adreslerini içeren bir içerik işleme `@` geçiş karakter olarak sembol.</span><span class="sxs-lookup"><span data-stu-id="67287-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="67287-121">Aşağıdaki örnekte e-posta adresleri tarafından Razor ayrıştırma olduğu:</span><span class="sxs-lookup"><span data-stu-id="67287-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="67287-122">Örtük Razor ifadeleri</span><span class="sxs-lookup"><span data-stu-id="67287-122">Implicit Razor expressions</span></span>

<span data-ttu-id="67287-123">Örtük Razor ifadeleri ile başlayıp `@` ardından C# kod:</span><span class="sxs-lookup"><span data-stu-id="67287-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="67287-124">Dışında C# `await` anahtar sözcüğü, örtük ifadeleri boşluk içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="67287-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="67287-125">Varsa C# deyimi açık bir bitiş sahipse, boşluk intermingled:</span><span class="sxs-lookup"><span data-stu-id="67287-125">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="67287-126">Örtük ifade **olamaz** içeren C# köşeli ayraçlar içindeki karakterler olarak genel türler (`<>`) bir HTML etiketi yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="67287-126">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="67287-127">Aşağıdaki kod **değil** geçerli:</span><span class="sxs-lookup"><span data-stu-id="67287-127">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="67287-128">Yukarıdaki kod, aşağıdakilerden birini benzer bir derleyici hatası oluşturur:</span><span class="sxs-lookup"><span data-stu-id="67287-128">The preceding code generates a compiler error similar to one of the following:</span></span>

* <span data-ttu-id="67287-129">"İnt" öğesi kapalı değildi.</span><span class="sxs-lookup"><span data-stu-id="67287-129">The "int" element wasn't closed.</span></span> <span data-ttu-id="67287-130">Tüm öğeleri olmalıdır kendi kendine kapanan veya eşleşen bir bitiş etiketi sahip.</span><span class="sxs-lookup"><span data-stu-id="67287-130">All elements must be either self-closing or have a matching end tag.</span></span>
* <span data-ttu-id="67287-131">Yöntem grubu 'object' türü temsilci GenericMethod' dönüştürülemiyor.</span><span class="sxs-lookup"><span data-stu-id="67287-131">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="67287-132">Bir yöntemi çağırmak mı istiyordunuz?'</span><span class="sxs-lookup"><span data-stu-id="67287-132">Did you intend to invoke the method?\`</span></span>

<span data-ttu-id="67287-133">Genel yöntem çağrılarını sarmalanmış, içinde bir [Razor açık ifadesi](#explicit-razor-expressions) veya [Razor kodu bloğu](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="67287-133">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="67287-134">Açık Razor ifadeleri</span><span class="sxs-lookup"><span data-stu-id="67287-134">Explicit Razor expressions</span></span>

<span data-ttu-id="67287-135">Açık Razor ifadeleri oluşur bir `@` dengeli parantez ile simge.</span><span class="sxs-lookup"><span data-stu-id="67287-135">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="67287-136">Geçen haftaki zaman işlemek için aşağıdaki Razor işaretlemesi kullanılır:</span><span class="sxs-lookup"><span data-stu-id="67287-136">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="67287-137">İçinde herhangi bir içerik `@()` parantez değerlendirilir ve işlenen çıkışı.</span><span class="sxs-lookup"><span data-stu-id="67287-137">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="67287-138">Önceki bölümde açıklanan, örtük ifadeleri genellikle boşluk içeremez.</span><span class="sxs-lookup"><span data-stu-id="67287-138">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="67287-139">Aşağıdaki kodda, bir hafta, geçerli saatten çıkarılabilir değil:</span><span class="sxs-lookup"><span data-stu-id="67287-139">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="67287-140">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="67287-140">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="67287-141">Açık ifadeler, metin bir ifadenin sonucu ile birleştirmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="67287-141">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="67287-142">Açık bir ifade olmadan `<p>Age@joe.Age</p>` bir e-posta adresi kabul edilir ve `<p>Age@joe.Age</p>` işlenir.</span><span class="sxs-lookup"><span data-stu-id="67287-142">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="67287-143">Açık bir ifade olarak yazılırken `<p>Age33</p>` işlenir.</span><span class="sxs-lookup"><span data-stu-id="67287-143">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<span data-ttu-id="67287-144">Açık ifadeleri, genel yöntemleri gelen çıkış işleme için kullanılabilir *.cshtml* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="67287-144">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="67287-145">Aşağıdaki biçimlendirmede gösterilen hatayı düzeltmek gösterilmektedir köşeli parantez önceki neden bir C# genel.</span><span class="sxs-lookup"><span data-stu-id="67287-145">The following markup shows how to correct the error shown earlier caused by the brackets of a C# generic.</span></span> <span data-ttu-id="67287-146">Kodu açık bir ifade olarak yazılır:</span><span class="sxs-lookup"><span data-stu-id="67287-146">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="67287-147">İfade kodlama</span><span class="sxs-lookup"><span data-stu-id="67287-147">Expression encoding</span></span>

<span data-ttu-id="67287-148">C#HTML kodlu bir dizeye değerlendirilen ifadeleri var.</span><span class="sxs-lookup"><span data-stu-id="67287-148">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="67287-149">C#değerlendirmek için ifadeleri `IHtmlContent` doğrudan aracılığıyla işlenir `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="67287-149">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="67287-150">C#Değerlendirme yok ifadeleri `IHtmlContent` tarafından bir dizeye dönüştürülür `ToString` ve bunlar işlenen önce kodlanır.</span><span class="sxs-lookup"><span data-stu-id="67287-150">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="67287-151">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="67287-151">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="67287-152">HTML tarayıcı gösterilir:</span><span class="sxs-lookup"><span data-stu-id="67287-152">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="67287-153">`HtmlHelper.Raw` Çıktı kodlanmış değildir, ancak geri HTML biçimlendirmesi olarak çizilir.</span><span class="sxs-lookup"><span data-stu-id="67287-153">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="67287-154">Kullanarak `HtmlHelper.Raw` unsanitized kullanıcı girişi bir güvenlik riski oluşturur.</span><span class="sxs-lookup"><span data-stu-id="67287-154">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="67287-155">Kullanıcı girişi, kötü amaçlı JavaScript veya diğer saldırılara içerebilir.</span><span class="sxs-lookup"><span data-stu-id="67287-155">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="67287-156">Kullanıcı girişi temizlenirken zordur.</span><span class="sxs-lookup"><span data-stu-id="67287-156">Sanitizing user input is difficult.</span></span> <span data-ttu-id="67287-157">Kullanmaktan kaçının `HtmlHelper.Raw` kullanıcı girişi ile.</span><span class="sxs-lookup"><span data-stu-id="67287-157">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="67287-158">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="67287-158">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="67287-159">Razor kodu bloğu</span><span class="sxs-lookup"><span data-stu-id="67287-159">Razor code blocks</span></span>

<span data-ttu-id="67287-160">Razor kod blokları kullanmaya başlama `@` ve tarafından alınmış `{}`.</span><span class="sxs-lookup"><span data-stu-id="67287-160">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="67287-161">İfadeler, aksine C# olmayan kod blokları içinde kod çizilir.</span><span class="sxs-lookup"><span data-stu-id="67287-161">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="67287-162">Kod blokları ve ifadeler bir görünümdeki aynı kapsamı paylaşan ve sırayla tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="67287-162">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="67287-163">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="67287-163">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="67287-164">Kod blokları ' nda, şablon oluşturma yöntemleri olarak kullanılacak biçimlendirme ile [yerel işlevler](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) bildirin:</span><span class="sxs-lookup"><span data-stu-id="67287-164">In code blocks, declare [local functions](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) with markup to serve as templating methods:</span></span>

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

<span data-ttu-id="67287-165">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="67287-165">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="implicit-transitions"></a><span data-ttu-id="67287-166">Örtük geçişleri</span><span class="sxs-lookup"><span data-stu-id="67287-166">Implicit transitions</span></span>

<span data-ttu-id="67287-167">Bir kod bloğu varsayılan dilde C#, ancak Razor sayfası HTML olarak geçiş yapabilir:</span><span class="sxs-lookup"><span data-stu-id="67287-167">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="67287-168">Açık ayrılmış geçiş</span><span class="sxs-lookup"><span data-stu-id="67287-168">Explicit delimited transition</span></span>

<span data-ttu-id="67287-169">HTML oluşturması gereken bir kod bloğunun alt bölümünü tanımlamak için, göstermek için karakterleri Razor `<text>` etiketiyle çevreleyin:</span><span class="sxs-lookup"><span data-stu-id="67287-169">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor `<text>` tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="67287-170">Tarafından HTML etiketleri arasına olmayan HTML oluşturmak için bu yaklaşımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="67287-170">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="67287-171">Bir HTML veya Razor etiket olmadan, bir Razor çalışma zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="67287-171">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="67287-172">Etiket `<text>` , içerik işlerken boşluğu denetlemek için yararlıdır:</span><span class="sxs-lookup"><span data-stu-id="67287-172">The `<text>` tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="67287-173">Yalnızca `<text>` etiketi arasındaki içerik işlenir.</span><span class="sxs-lookup"><span data-stu-id="67287-173">Only the content between the `<text>` tag is rendered.</span></span>
* <span data-ttu-id="67287-174">HTML çıktısında `<text>` etiket görüntülenmeden önce veya sonra boşluk yok.</span><span class="sxs-lookup"><span data-stu-id="67287-174">No whitespace before or after the `<text>` tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-colon"></a><span data-ttu-id="67287-175">İle açık satır geçişi\@&colon;</span><span class="sxs-lookup"><span data-stu-id="67287-175">Explicit line transition with \@&colon;</span></span>

<span data-ttu-id="67287-176">Tüm bir satırı geri kalanı bir kod bloğunun içine HTML olarak işlemek için kullanmak `@:` söz dizimi:</span><span class="sxs-lookup"><span data-stu-id="67287-176">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="67287-177">Olmadan `@:` kodda bir Razor çalışma zamanı hatası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="67287-177">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="67287-178">Razor `@` dosyasındaki fazla karakter, bloktaki daha sonra bulunan deyimlerde derleyici hatalarına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="67287-178">Extra `@` characters in a Razor file can cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="67287-179">Bu derleyici hataları önce bildirilen hatayı gerçek bir hata oluştuğu için anlamak zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="67287-179">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="67287-180">Bu hata, tek bir kod bloğunun birden çok örtük/açık ifadelere birleştirdikten sonra yaygındır.</span><span class="sxs-lookup"><span data-stu-id="67287-180">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="67287-181">Denetim yapıları</span><span class="sxs-lookup"><span data-stu-id="67287-181">Control structures</span></span>

<span data-ttu-id="67287-182">Denetim yapıları kod bloğu bir uzantı var.</span><span class="sxs-lookup"><span data-stu-id="67287-182">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="67287-183">Kod blokları tüm yönlerini (biçimlendirme, satır içi geçiş C#) aşağıdaki yapılar için de geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="67287-183">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="67287-184">Koşul varsa, Else, Else ve \@anahtar \@</span><span class="sxs-lookup"><span data-stu-id="67287-184">Conditionals \@if, else if, else, and \@switch</span></span>

<span data-ttu-id="67287-185">`@if` kod çalıştığında denetimleri:</span><span class="sxs-lookup"><span data-stu-id="67287-185">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="67287-186">`else` ve `else if` gerektirmeyen `@` sembol:</span><span class="sxs-lookup"><span data-stu-id="67287-186">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="67287-187">Aşağıdaki biçimlendirme switch deyimi kullanma işlemini gösterir:</span><span class="sxs-lookup"><span data-stu-id="67287-187">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="67287-188">For \@döngüsü, \@foreach, \@while ve \@do</span><span class="sxs-lookup"><span data-stu-id="67287-188">Looping \@for, \@foreach, \@while, and \@do while</span></span>

<span data-ttu-id="67287-189">Şablonlu HTML denetim ifadeleri döngü ile oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="67287-189">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="67287-190">Kişi listesini oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="67287-190">To render a list of people:</span></span>

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

<span data-ttu-id="67287-191">Aşağıdaki döngü deyimi desteklenir:</span><span class="sxs-lookup"><span data-stu-id="67287-191">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="67287-192">Kullanarak \@bileşik</span><span class="sxs-lookup"><span data-stu-id="67287-192">Compound \@using</span></span>

<span data-ttu-id="67287-193">İçinde C#, `using` deyimi, bir nesne kullanıldığında emin olmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="67287-193">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="67287-194">Razor aynı mekanizmayı ek içeriklere sahip bir HTML Yardımcıları oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="67287-194">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="67287-195">Aşağıdaki kodda, HTML Yardımcıları `<form>` `@using` ifadesiyle bir etiketi işlerler:</span><span class="sxs-lookup"><span data-stu-id="67287-195">In the following code, HTML Helpers render a `<form>` tag with the `@using` statement:</span></span>

```cshtml
@using (Html.BeginForm())
{
    <div>
        Email: <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

### <a name="try-catch-finally"></a><span data-ttu-id="67287-196">\@TRY, catch, finally</span><span class="sxs-lookup"><span data-stu-id="67287-196">\@try, catch, finally</span></span>

<span data-ttu-id="67287-197">Özel durum işleme benzer C#:</span><span class="sxs-lookup"><span data-stu-id="67287-197">Exception handling is similar to C#:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a><span data-ttu-id="67287-198">\@ine</span><span class="sxs-lookup"><span data-stu-id="67287-198">\@lock</span></span>

<span data-ttu-id="67287-199">Razor kilit deyimleri kritik bölümlerle korumanın yeteneğine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="67287-199">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="67287-200">Açıklamalar</span><span class="sxs-lookup"><span data-stu-id="67287-200">Comments</span></span>

<span data-ttu-id="67287-201">Razor destekler C# ve HTML yorumlarında:</span><span class="sxs-lookup"><span data-stu-id="67287-201">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="67287-202">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="67287-202">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="67287-203">Web sayfası işlenmeden önce razor açıklama sunucu tarafından kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="67287-203">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="67287-204">Razor kullanan `@*  *@` açıklamaları sınırlandırmak için.</span><span class="sxs-lookup"><span data-stu-id="67287-204">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="67287-205">Sunucu tüm biçimlendirme işlemesi yapmıyor için aşağıdaki kodu, geçersiz kılınan:</span><span class="sxs-lookup"><span data-stu-id="67287-205">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="67287-206">Yönergeler</span><span class="sxs-lookup"><span data-stu-id="67287-206">Directives</span></span>

<span data-ttu-id="67287-207">Razor yönergesi örtük ayrılmış anahtar sözcükler aşağıdaki deyimlerle tarafından temsil edilir `@` simgesi.</span><span class="sxs-lookup"><span data-stu-id="67287-207">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="67287-208">Bir yönergesi, genellikle bir görünüm ayrıştırılır veya farklı bir işlevsellik sağlar şeklini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="67287-208">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="67287-209">Razor kod görünümü nasıl oluşturur? anlama yönergeleri nasıl çalıştığını anlamak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="67287-209">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="67287-210">Kod aşağıdaki gibi bir sınıf oluşturur:</span><span class="sxs-lookup"><span data-stu-id="67287-210">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="67287-211">Bölümü bu makalenin ilerleyen bölümlerinde [Razor İnceleme C# bir görünümü için oluşturulan sınıf](#inspect-the-razor-c-class-generated-for-a-view) bu oluşturulan sınıf görüntülemek açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="67287-211">Later in this article, the section [Inspect the Razor C# class generated for a view](#inspect-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="attribute"></a><span data-ttu-id="67287-212">\@özniteliğe</span><span class="sxs-lookup"><span data-stu-id="67287-212">\@attribute</span></span>

<span data-ttu-id="67287-213">`@attribute` Yönergesi verilen özniteliği oluşturulan sayfanın veya görünümün sınıfına ekler.</span><span class="sxs-lookup"><span data-stu-id="67287-213">The `@attribute` directive adds the given attribute to the class of the generated page or view.</span></span> <span data-ttu-id="67287-214">Aşağıdaki örnek `[Authorize]` özniteliğini ekler:</span><span class="sxs-lookup"><span data-stu-id="67287-214">The following example adds the `[Authorize]` attribute:</span></span>

```cshtml
@attribute [Authorize]
```

::: moniker range=">= aspnetcore-3.0"

### <a name="code"></a><span data-ttu-id="67287-215">\@kodudur</span><span class="sxs-lookup"><span data-stu-id="67287-215">\@code</span></span>

<span data-ttu-id="67287-216">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="67287-216">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="67287-217">Blok `@code` , bir [Razor bileşeninin](xref:blazor/components) bir bileşene üye C# (alanlar, Özellikler ve Yöntemler) eklemesini sağlar:</span><span class="sxs-lookup"><span data-stu-id="67287-217">The `@code` block enables a [Razor component](xref:blazor/components) to add C# members (fields, properties, and methods) to a component:</span></span>

```cshtml
@code {
    // C# members (fields, properties, and methods)
}
```

<span data-ttu-id="67287-218">Razor bileşenleri için, `@code` öğesinin [@functions](#functions) diğer adı ve önerilir `@functions`.</span><span class="sxs-lookup"><span data-stu-id="67287-218">For Razor components, `@code` is an alias of [@functions](#functions) and recommended over `@functions`.</span></span> <span data-ttu-id="67287-219">Birden çok blok izin verilir. `@code`</span><span class="sxs-lookup"><span data-stu-id="67287-219">More than one `@code` block is permissible.</span></span>

::: moniker-end

### <a name="functions"></a><span data-ttu-id="67287-220">\@lerdir</span><span class="sxs-lookup"><span data-stu-id="67287-220">\@functions</span></span>

<span data-ttu-id="67287-221">Yönergesi, oluşturulan sınıfa C# üye (alanlar, Özellikler ve Yöntemler) eklemeyi sağlar: `@functions`</span><span class="sxs-lookup"><span data-stu-id="67287-221">The `@functions` directive enables adding C# members (fields, properties, and methods) to the generated class:</span></span>

```cshtml
@functions {
    // C# members (fields, properties, and methods)
}
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="67287-222">[Razor bileşenlerinde](xref:blazor/components), üye eklemek `@code` `@functions` C# için ' i kullanın.</span><span class="sxs-lookup"><span data-stu-id="67287-222">In [Razor components](xref:blazor/components), use `@code` over `@functions` to add C# members.</span></span>

::: moniker-end

<span data-ttu-id="67287-223">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="67287-223">For example:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="67287-224">Kod aşağıdaki HTML biçimlendirmeyi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="67287-224">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="67287-225">Aşağıdaki kodu oluşturulmuş Razor olan C# sınıfı:</span><span class="sxs-lookup"><span data-stu-id="67287-225">The following code is the generated Razor C# class:</span></span>

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="67287-226">`@functions`Yöntemler, işaretlemelerdeki şablon oluşturma yöntemleri olarak görev yapar:</span><span class="sxs-lookup"><span data-stu-id="67287-226">`@functions` methods serve as templating methods when they have markup:</span></span>

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

<span data-ttu-id="67287-227">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="67287-227">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

### <a name="implements"></a><span data-ttu-id="67287-228">\@uygular</span><span class="sxs-lookup"><span data-stu-id="67287-228">\@implements</span></span>

<span data-ttu-id="67287-229">`@implements` Yönergesi, oluşturulan sınıf için bir arabirim uygular.</span><span class="sxs-lookup"><span data-stu-id="67287-229">The `@implements` directive implements an interface for the generated class.</span></span>

<span data-ttu-id="67287-230">Aşağıdaki örnek, yönteminin <xref:System.IDisposable?displayProperty=fullName> çağrılabilmesi için <xref:System.IDisposable.Dispose*> uygular:</span><span class="sxs-lookup"><span data-stu-id="67287-230">The following example implements <xref:System.IDisposable?displayProperty=fullName> so that the <xref:System.IDisposable.Dispose*> method can be called:</span></span>

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

### <a name="inherits"></a><span data-ttu-id="67287-231">\@alıp</span><span class="sxs-lookup"><span data-stu-id="67287-231">\@inherits</span></span>

<span data-ttu-id="67287-232">`@inherits` Yönergesi görünümü devralan sınıf tam denetim sağlar:</span><span class="sxs-lookup"><span data-stu-id="67287-232">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="67287-233">Aşağıdaki kod bir özel bir Razor sayfası türüdür:</span><span class="sxs-lookup"><span data-stu-id="67287-233">The following code is a custom Razor page type:</span></span>

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="67287-234">`CustomText` Bir görünümde görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="67287-234">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="67287-235">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="67287-235">The code renders the following HTML:</span></span>

```html
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

 <span data-ttu-id="67287-236">`@model` ve `@inherits` aynı görünümde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="67287-236">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="67287-237">`@inherits` kullanılabilir bir *_viewımports.cshtml* görünümü alır dosyası:</span><span class="sxs-lookup"><span data-stu-id="67287-237">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="67287-238">Aşağıdaki kod, kesin türü belirtilmiş görünüm örneğidir:</span><span class="sxs-lookup"><span data-stu-id="67287-238">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="67287-239">Varsa "rick@contoso.com" geçirilen modeldeki görünümünü aşağıdaki HTML biçimlendirmeyi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="67287-239">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

### <a name="inject"></a><span data-ttu-id="67287-240">\@eklenecek</span><span class="sxs-lookup"><span data-stu-id="67287-240">\@inject</span></span>

<span data-ttu-id="67287-241">`@inject` Yönergesi sağlayan bir hizmet eklemesine Razor sayfası [hizmet kapsayıcı](xref:fundamentals/dependency-injection) bir görünümde.</span><span class="sxs-lookup"><span data-stu-id="67287-241">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="67287-242">Daha fazla bilgi için [görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="67287-242">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="layout"></a><span data-ttu-id="67287-243">\@Düzenle</span><span class="sxs-lookup"><span data-stu-id="67287-243">\@layout</span></span>

<span data-ttu-id="67287-244">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="67287-244">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="67287-245">Yönerge `@layout` , Razor bileşeni için bir düzen belirtir.</span><span class="sxs-lookup"><span data-stu-id="67287-245">The `@layout` directive specifies a layout for a Razor component.</span></span> <span data-ttu-id="67287-246">Düzen bileşenleri kod yinelemeyi ve tutarsızlığın önüne geçmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="67287-246">Layout components are used to avoid code duplication and inconsistency.</span></span> <span data-ttu-id="67287-247">Daha fazla bilgi için bkz. <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="67287-247">For more information, see <xref:blazor/layouts>.</span></span>

::: moniker-end

### <a name="model"></a><span data-ttu-id="67287-248">\@modelinizi</span><span class="sxs-lookup"><span data-stu-id="67287-248">\@model</span></span>

<span data-ttu-id="67287-249">*Bu senaryo yalnızca MVC görünümleri ve Razor Pages (. cshtml) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="67287-249">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="67287-250">`@model` Yönergesi bir görünüm veya sayfaya geçirilen modelin türünü belirtir:</span><span class="sxs-lookup"><span data-stu-id="67287-250">The `@model` directive specifies the type of the model passed to a view or page:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="67287-251">Bir ASP.NET Core MVC veya bireysel kullanıcı hesaplarıyla oluşturulan Razor Pages uygulama, *Görünümler/Account/Login. cshtml* aşağıdaki model bildirimini içerir:</span><span class="sxs-lookup"><span data-stu-id="67287-251">In an ASP.NET Core MVC or Razor Pages app created with individual user accounts, *Views/Account/Login.cshtml* contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="67287-252">Oluşturulan sınıf devraldığı `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="67287-252">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="67287-253">Razor sunan bir `Model` modeli erişmek için özelliği geçirilen görünümü:</span><span class="sxs-lookup"><span data-stu-id="67287-253">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="67287-254">Yönerge, `Model` özelliğin türünü belirtir. `@model`</span><span class="sxs-lookup"><span data-stu-id="67287-254">The `@model` directive specifies the type of the `Model` property.</span></span> <span data-ttu-id="67287-255">Yönergesi belirtir `T` içinde `RazorPage<T>` türetildiği görünümü oluşturulan, sınıfın.</span><span class="sxs-lookup"><span data-stu-id="67287-255">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="67287-256">Varsa `@model` yönergesi belirtilmediyse, `Model` özelliği türüdür `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="67287-256">If the `@model` directive isn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="67287-257">Daha fazla bilgi için bkz. [türü kesin belirlenmiş modeller @model ve anahtar sözcüğü](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span><span class="sxs-lookup"><span data-stu-id="67287-257">For more information, see [Strongly typed models and the @model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span></span>

### <a name="namespace"></a><span data-ttu-id="67287-258">\@uzayına</span><span class="sxs-lookup"><span data-stu-id="67287-258">\@namespace</span></span>

<span data-ttu-id="67287-259">`@namespace` Yönergesi:</span><span class="sxs-lookup"><span data-stu-id="67287-259">The `@namespace` directive:</span></span>

* <span data-ttu-id="67287-260">Oluşturulan Razor sayfası, MVC görünümü veya Razor bileşeni sınıfının ad alanını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="67287-260">Sets the namespace of the class of the generated Razor page, MVC view, or Razor component.</span></span>
* <span data-ttu-id="67287-261">Bir sayfa, görünüm veya bileşen sınıfının kök türetilmiş ad alanlarını dizin ağacındaki en yakın içeri aktarmalar dosyasından (görünümler veya sayfalar) veya *_ımports. Razor* (Razor bileşenleri) ayarlar.</span><span class="sxs-lookup"><span data-stu-id="67287-261">Sets the root derived namespaces of a pages, views, or components classes from the closest imports file in the directory tree, *_ViewImports.cshtml* (views or pages) or *_Imports.razor* (Razor components).</span></span>

```cshtml
@namespace Your.Namespace.Here
```

<span data-ttu-id="67287-262">Aşağıdaki tabloda gösterilen Razor Pages örneği için:</span><span class="sxs-lookup"><span data-stu-id="67287-262">For the Razor Pages example shown in the following table:</span></span>

* <span data-ttu-id="67287-263">Her sayfa sayfa */_Viewwimports. cshtml*içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="67287-263">Each page imports *Pages/_ViewImports.cshtml*.</span></span>
* <span data-ttu-id="67287-264">*Pages/_Viewwimports. cshtml* Contains `@namespace Hello.World`.</span><span class="sxs-lookup"><span data-stu-id="67287-264">*Pages/_ViewImports.cshtml* contains `@namespace Hello.World`.</span></span>
* <span data-ttu-id="67287-265">Her sayfa, `Hello.World` ad alanının kökü olarak bulunur.</span><span class="sxs-lookup"><span data-stu-id="67287-265">Each page has `Hello.World` as the root of it's namespace.</span></span>

| <span data-ttu-id="67287-266">Sayfasında</span><span class="sxs-lookup"><span data-stu-id="67287-266">Page</span></span>                                        | <span data-ttu-id="67287-267">Ad Alanı</span><span class="sxs-lookup"><span data-stu-id="67287-267">Namespace</span></span>                             |
| ------------------------------------------- | ------------------------------------- |
| <span data-ttu-id="67287-268">*Pages/Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="67287-268">*Pages/Index.cshtml*</span></span>                        | `Hello.World`                         |
| <span data-ttu-id="67287-269">*Sayfa/fazla sayfa/sayfa. cshtml*</span><span class="sxs-lookup"><span data-stu-id="67287-269">*Pages/MorePages/Page.cshtml*</span></span>               | `Hello.World.MorePages`               |
| <span data-ttu-id="67287-270">*Pages/te Pages/Evente Pages/Page. cshtml*</span><span class="sxs-lookup"><span data-stu-id="67287-270">*Pages/MorePages/EvenMorePages/Page.cshtml*</span></span> | `Hello.World.MorePages.EvenMorePages` |

<span data-ttu-id="67287-271">Yukarıdaki ilişkiler, MVC görünümleri ve Razor bileşenleriyle kullanılan dosyaları içeri aktarmak için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="67287-271">The preceding relationships apply to import files used with MVC views and Razor components.</span></span>

<span data-ttu-id="67287-272">Birden çok içeri aktarma dosyasında bir `@namespace` yönerge olduğunda, kök ad alanını ayarlamak için dizin ağacındaki sayfaya, görünüme veya bileşene en yakın dosya kullanılır.</span><span class="sxs-lookup"><span data-stu-id="67287-272">When multiple import files have a `@namespace` directive, the file closest to the page, view, or component in the directory tree is used to set the root namespace.</span></span>

<span data-ttu-id="67287-273">Yukarıdaki örnekteki *evente Pages* klasöründe bir içeri aktarmalar dosyası `@namespace Another.Planet` varsa (veya sayfa/değer sayfaları */evente Pages/Page. cshtml* dosyası içeriyorsa `@namespace Another.Planet`), sonuç aşağıdaki tabloda gösterilir.</span><span class="sxs-lookup"><span data-stu-id="67287-273">If the *EvenMorePages* folder in the preceding example has an imports file with `@namespace Another.Planet` (or the *Pages/MorePages/EvenMorePages/Page.cshtml* file contains `@namespace Another.Planet`), the result is shown in the following table.</span></span>

| <span data-ttu-id="67287-274">Sayfasında</span><span class="sxs-lookup"><span data-stu-id="67287-274">Page</span></span>                                        | <span data-ttu-id="67287-275">Ad Alanı</span><span class="sxs-lookup"><span data-stu-id="67287-275">Namespace</span></span>               |
| ------------------------------------------- | ----------------------- |
| <span data-ttu-id="67287-276">*Pages/Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="67287-276">*Pages/Index.cshtml*</span></span>                        | `Hello.World`           |
| <span data-ttu-id="67287-277">*Sayfa/fazla sayfa/sayfa. cshtml*</span><span class="sxs-lookup"><span data-stu-id="67287-277">*Pages/MorePages/Page.cshtml*</span></span>               | `Hello.World.MorePages` |
| <span data-ttu-id="67287-278">*Pages/te Pages/Evente Pages/Page. cshtml*</span><span class="sxs-lookup"><span data-stu-id="67287-278">*Pages/MorePages/EvenMorePages/Page.cshtml*</span></span> | `Another.Planet`        |

### <a name="page"></a><span data-ttu-id="67287-279">\@sayfasında</span><span class="sxs-lookup"><span data-stu-id="67287-279">\@page</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="67287-280">`@page` Yönergesinin, göründüğü dosyanın türüne bağlı olarak farklı etkileri vardır.</span><span class="sxs-lookup"><span data-stu-id="67287-280">The `@page` directive has different effects depending on the type of the file where it appears.</span></span> <span data-ttu-id="67287-281">Yönergesi:</span><span class="sxs-lookup"><span data-stu-id="67287-281">The directive:</span></span>

* <span data-ttu-id="67287-282">İçindeki bir *. cshtml* dosyasında, dosyanın bir Razor sayfası olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="67287-282">In in a *.cshtml* file indicates that the file is a Razor Page.</span></span> <span data-ttu-id="67287-283">Daha fazla bilgi için bkz. <xref:razor-pages/index>.</span><span class="sxs-lookup"><span data-stu-id="67287-283">For more information, see <xref:razor-pages/index>.</span></span>
* <span data-ttu-id="67287-284">Bir Razor bileşeninin istekleri doğrudan işlemesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="67287-284">Specifies that a Razor component should handle requests directly.</span></span> <span data-ttu-id="67287-285">Daha fazla bilgi için bkz. <xref:blazor/routing>.</span><span class="sxs-lookup"><span data-stu-id="67287-285">For more information, see <xref:blazor/routing>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="67287-286">Bir *. cshtml* dosyasının ilk satırındaki yönergesi,dosyanınbirRazorsayfasıolduğunugösterir.`@page`</span><span class="sxs-lookup"><span data-stu-id="67287-286">The `@page` directive on the first line of a *.cshtml* file indicates that the file is a Razor Page.</span></span> <span data-ttu-id="67287-287">Daha fazla bilgi için bkz. <xref:razor-pages/index>.</span><span class="sxs-lookup"><span data-stu-id="67287-287">For more information, see <xref:razor-pages/index>.</span></span>

::: moniker-end

### <a name="section"></a><span data-ttu-id="67287-288">\@kısmı</span><span class="sxs-lookup"><span data-stu-id="67287-288">\@section</span></span>

<span data-ttu-id="67287-289">*Bu senaryo yalnızca MVC görünümleri ve Razor Pages (. cshtml) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="67287-289">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="67287-290">Yönerge, HTML sayfasının farklı kısımlarında görünüm veya sayfaların içerik işlemesini sağlamak için [MVC ve Razor Pages düzenleriyle](xref:mvc/views/layout) birlikte kullanılır. `@section`</span><span class="sxs-lookup"><span data-stu-id="67287-290">The `@section` directive is used in conjunction with [MVC and Razor Pages layouts](xref:mvc/views/layout) to enable views or pages to render content in different parts of the HTML page.</span></span> <span data-ttu-id="67287-291">Daha fazla bilgi için bkz. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="67287-291">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="using"></a><span data-ttu-id="67287-292">\@kullanarak</span><span class="sxs-lookup"><span data-stu-id="67287-292">\@using</span></span>

<span data-ttu-id="67287-293">`@using` Yönergesi ekler C# `using` yönerge oluşturulmuş görünümü için:</span><span class="sxs-lookup"><span data-stu-id="67287-293">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="67287-294">[Razor bileşenlerinde](xref:blazor/components) `@using` Ayrıca hangi bileşenlerin kapsamda olduğunu da kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="67287-294">In [Razor components](xref:blazor/components), `@using` also controls which components are in scope.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="directive-attributes"></a><span data-ttu-id="67287-295">Yönerge öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="67287-295">Directive attributes</span></span>

### <a name="attributes"></a><span data-ttu-id="67287-296">\@özelliklerine</span><span class="sxs-lookup"><span data-stu-id="67287-296">\@attributes</span></span>

<span data-ttu-id="67287-297">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="67287-297">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="67287-298">`@attributes`bir bileşenin bildirilmeyen öznitelikleri işlemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="67287-298">`@attributes` allows a component to render non-declared attributes.</span></span> <span data-ttu-id="67287-299">Daha fazla bilgi için bkz. <xref:blazor/components#attribute-splatting-and-arbitrary-parameters>.</span><span class="sxs-lookup"><span data-stu-id="67287-299">For more information, see <xref:blazor/components#attribute-splatting-and-arbitrary-parameters>.</span></span>

### <a name="bind"></a><span data-ttu-id="67287-300">\@bağladığınızda</span><span class="sxs-lookup"><span data-stu-id="67287-300">\@bind</span></span>

<span data-ttu-id="67287-301">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="67287-301">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="67287-302">Bileşenlerdeki veri bağlama, `@bind` özniteliğiyle gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="67287-302">Data binding in components is accomplished with the `@bind` attribute.</span></span> <span data-ttu-id="67287-303">Daha fazla bilgi için bkz. <xref:blazor/components#data-binding>.</span><span class="sxs-lookup"><span data-stu-id="67287-303">For more information, see <xref:blazor/components#data-binding>.</span></span>

### <a name="onevent"></a><span data-ttu-id="67287-304">\@{Event} üzerinde</span><span class="sxs-lookup"><span data-stu-id="67287-304">\@on{event}</span></span>

<span data-ttu-id="67287-305">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="67287-305">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="67287-306">Razor, bileşenler için olay işleme özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="67287-306">Razor provides event handling features for components.</span></span> <span data-ttu-id="67287-307">Daha fazla bilgi için bkz. <xref:blazor/components#event-handling>.</span><span class="sxs-lookup"><span data-stu-id="67287-307">For more information, see <xref:blazor/components#event-handling>.</span></span>

### <a name="key"></a><span data-ttu-id="67287-308">\@anahtar</span><span class="sxs-lookup"><span data-stu-id="67287-308">\@key</span></span>

<span data-ttu-id="67287-309">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="67287-309">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="67287-310">`@key` Directive özniteliği, anahtar değerine göre öğelerin veya bileşenlerin korunmasını güvence altına almak için bileşenlerin algoritma almasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="67287-310">The `@key` directive attribute causes the components diffing algorithm to guarantee preservation of elements or components based on the key's value.</span></span> <span data-ttu-id="67287-311">Daha fazla bilgi için bkz. <xref:blazor/components#use-key-to-control-the-preservation-of-elements-and-components>.</span><span class="sxs-lookup"><span data-stu-id="67287-311">For more information, see <xref:blazor/components#use-key-to-control-the-preservation-of-elements-and-components>.</span></span>

### <a name="ref"></a><span data-ttu-id="67287-312">\@ref</span><span class="sxs-lookup"><span data-stu-id="67287-312">\@ref</span></span>

<span data-ttu-id="67287-313">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="67287-313">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="67287-314">Bileşen başvuruları (`@ref`) bir bileşen örneğine başvurmak için bir yol sağlar, böylece bu örneğe komut verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="67287-314">Component references (`@ref`) provide a way to reference a component instance so that you can issue commands to that instance.</span></span> <span data-ttu-id="67287-315">Daha fazla bilgi için bkz. <xref:blazor/components#capture-references-to-components>.</span><span class="sxs-lookup"><span data-stu-id="67287-315">For more information, see <xref:blazor/components#capture-references-to-components>.</span></span>

::: moniker-end

## <a name="templated-razor-delegates"></a><span data-ttu-id="67287-316">Şablonlu Razor temsilciler</span><span class="sxs-lookup"><span data-stu-id="67287-316">Templated Razor delegates</span></span>

<span data-ttu-id="67287-317">Razor şablonları aşağıdaki biçimde bir kullanıcı Arabirimi parçacığı tanımlamanıza izin ver:</span><span class="sxs-lookup"><span data-stu-id="67287-317">Razor templates allow you to define a UI snippet with the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="67287-318">Aşağıdaki örnekte, şablonlu Razor temsilci olarak belirtmek verilmektedir bir <xref:System.Func%602>.</span><span class="sxs-lookup"><span data-stu-id="67287-318">The following example illustrates how to specify a templated Razor delegate as a <xref:System.Func%602>.</span></span> <span data-ttu-id="67287-319">[Dinamik tür](/dotnet/csharp/programming-guide/types/using-type-dynamic) temsilci kapsülleyen yönteminin parametresi için belirtilir.</span><span class="sxs-lookup"><span data-stu-id="67287-319">The [dynamic type](/dotnet/csharp/programming-guide/types/using-type-dynamic) is specified for the parameter of the method that the delegate encapsulates.</span></span> <span data-ttu-id="67287-320">Bir [nesne türü](/dotnet/csharp/language-reference/keywords/object) temsilcinin dönüş değeri olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="67287-320">An [object type](/dotnet/csharp/language-reference/keywords/object) is specified as the return value of the delegate.</span></span> <span data-ttu-id="67287-321">Şablon ile kullanılan bir <xref:System.Collections.Generic.List%601> , `Pet` olan bir `Name` özelliği.</span><span class="sxs-lookup"><span data-stu-id="67287-321">The template is used with a <xref:System.Collections.Generic.List%601> of `Pet` that has a `Name` property.</span></span>

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

<span data-ttu-id="67287-322">Şablon ile işlenen `pets` tarafından sağlanan bir `foreach` deyimi:</span><span class="sxs-lookup"><span data-stu-id="67287-322">The template is rendered with `pets` supplied by a `foreach` statement:</span></span>

```cshtml
@foreach (var pet in pets)
{
    @petTemplate(pet)
}
```

<span data-ttu-id="67287-323">İşlenmiş çıkışı:</span><span class="sxs-lookup"><span data-stu-id="67287-323">Rendered output:</span></span>

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

<span data-ttu-id="67287-324">Bir yöntem bağımsız değişkeni olarak bir satır içi Razor şablonu da sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="67287-324">You can also supply an inline Razor template as an argument to a method.</span></span> <span data-ttu-id="67287-325">Aşağıdaki örnekte, `Repeat` yöntem Razor şablonu alır.</span><span class="sxs-lookup"><span data-stu-id="67287-325">In the following example, the `Repeat` method receives a Razor template.</span></span> <span data-ttu-id="67287-326">Yöntemi, HTML içerik ile sağlanan bir listeden öğeleri yineler üretmek için şablonu kullanır:</span><span class="sxs-lookup"><span data-stu-id="67287-326">The method uses the template to produce HTML content with repeats of items supplied from a list:</span></span>

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

<span data-ttu-id="67287-327">Önceki örnekte, Evcil Hayvanlar listesi kullanarak `Repeat` yöntemi çağrıldığında:</span><span class="sxs-lookup"><span data-stu-id="67287-327">Using the list of pets from the prior example, the `Repeat` method is called with:</span></span>

* <span data-ttu-id="67287-328"><xref:System.Collections.Generic.List%601> ' ın `Pet`.</span><span class="sxs-lookup"><span data-stu-id="67287-328"><xref:System.Collections.Generic.List%601> of `Pet`.</span></span>
* <span data-ttu-id="67287-329">Her evcil hayvan yineleme sayısı.</span><span class="sxs-lookup"><span data-stu-id="67287-329">Number of times to repeat each pet.</span></span>
* <span data-ttu-id="67287-330">Satır içi şablon sırasız bir listesini liste öğeleri için kullanın.</span><span class="sxs-lookup"><span data-stu-id="67287-330">Inline template to use for the list items of an unordered list.</span></span>

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

<span data-ttu-id="67287-331">İşlenmiş çıkışı:</span><span class="sxs-lookup"><span data-stu-id="67287-331">Rendered output:</span></span>

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

## <a name="tag-helpers"></a><span data-ttu-id="67287-332">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="67287-332">Tag Helpers</span></span>

<span data-ttu-id="67287-333">*Bu senaryo yalnızca MVC görünümleri ve Razor Pages (. cshtml) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="67287-333">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="67287-334">İlgili üç yönergeleri vardır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="67287-334">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="67287-335">Yönergesi</span><span class="sxs-lookup"><span data-stu-id="67287-335">Directive</span></span> | <span data-ttu-id="67287-336">İşlev</span><span class="sxs-lookup"><span data-stu-id="67287-336">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="67287-337">Etiket Yardımcıları bir görünüm için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="67287-337">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="67287-338">Daha önce bir görünümden eklenen etiket Yardımcıları kaldırır.</span><span class="sxs-lookup"><span data-stu-id="67287-338">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="67287-339">Etiket Yardımcısı desteğinin etkinleştirmek ve etiket Yardımcısı kullanım açık hale getirmek için bir etiket öneki belirtir.</span><span class="sxs-lookup"><span data-stu-id="67287-339">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="67287-340">Razor ayrılmış anahtar sözcükler</span><span class="sxs-lookup"><span data-stu-id="67287-340">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="67287-341">Razor anahtar sözcükleri</span><span class="sxs-lookup"><span data-stu-id="67287-341">Razor keywords</span></span>

* <span data-ttu-id="67287-342">sayfa (ASP.NET Core 2,1 veya üzeri bir sürüm gerektirir)</span><span class="sxs-lookup"><span data-stu-id="67287-342">page (Requires ASP.NET Core 2.1 or later)</span></span>
* <span data-ttu-id="67287-343">ad alanı</span><span class="sxs-lookup"><span data-stu-id="67287-343">namespace</span></span>
* <span data-ttu-id="67287-344">işlevleri</span><span class="sxs-lookup"><span data-stu-id="67287-344">functions</span></span>
* <span data-ttu-id="67287-345">Devralan</span><span class="sxs-lookup"><span data-stu-id="67287-345">inherits</span></span>
* <span data-ttu-id="67287-346">model</span><span class="sxs-lookup"><span data-stu-id="67287-346">model</span></span>
* <span data-ttu-id="67287-347">section</span><span class="sxs-lookup"><span data-stu-id="67287-347">section</span></span>
* <span data-ttu-id="67287-348">yardımcı (şu anda ASP.NET Core tarafından desteklenmez)</span><span class="sxs-lookup"><span data-stu-id="67287-348">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="67287-349">Razor anahtar sözcükleri kaçış ile `@(Razor Keyword)` (örneğin, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="67287-349">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="67287-350">C#Razor anahtar sözcükleri</span><span class="sxs-lookup"><span data-stu-id="67287-350">C# Razor keywords</span></span>

* <span data-ttu-id="67287-351">büyük/küçük harf</span><span class="sxs-lookup"><span data-stu-id="67287-351">case</span></span>
* <span data-ttu-id="67287-352">do</span><span class="sxs-lookup"><span data-stu-id="67287-352">do</span></span>
* <span data-ttu-id="67287-353">default</span><span class="sxs-lookup"><span data-stu-id="67287-353">default</span></span>
* <span data-ttu-id="67287-354">for</span><span class="sxs-lookup"><span data-stu-id="67287-354">for</span></span>
* <span data-ttu-id="67287-355">foreach</span><span class="sxs-lookup"><span data-stu-id="67287-355">foreach</span></span>
* <span data-ttu-id="67287-356">if</span><span class="sxs-lookup"><span data-stu-id="67287-356">if</span></span>
* <span data-ttu-id="67287-357">else</span><span class="sxs-lookup"><span data-stu-id="67287-357">else</span></span>
* <span data-ttu-id="67287-358">lock</span><span class="sxs-lookup"><span data-stu-id="67287-358">lock</span></span>
* <span data-ttu-id="67287-359">anahtarı</span><span class="sxs-lookup"><span data-stu-id="67287-359">switch</span></span>
* <span data-ttu-id="67287-360">deneyin</span><span class="sxs-lookup"><span data-stu-id="67287-360">try</span></span>
* <span data-ttu-id="67287-361">Yakalama</span><span class="sxs-lookup"><span data-stu-id="67287-361">catch</span></span>
* <span data-ttu-id="67287-362">finally</span><span class="sxs-lookup"><span data-stu-id="67287-362">finally</span></span>
* <span data-ttu-id="67287-363">kullanma</span><span class="sxs-lookup"><span data-stu-id="67287-363">using</span></span>
* <span data-ttu-id="67287-364">while</span><span class="sxs-lookup"><span data-stu-id="67287-364">while</span></span>

<span data-ttu-id="67287-365">C#Razor anahtar sözcükleri çift kaçış ile olmalıdır `@(@C# Razor Keyword)` (örneğin, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="67287-365">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="67287-366">İlk `@` Razor ayrıştırıcısı atlar.</span><span class="sxs-lookup"><span data-stu-id="67287-366">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="67287-367">İkinci `@` çıkışları C# ayrıştırıcı.</span><span class="sxs-lookup"><span data-stu-id="67287-367">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="67287-368">Razor tarafından kullanılmayan ayrılmış anahtar sözcükler</span><span class="sxs-lookup"><span data-stu-id="67287-368">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="67287-369">sınıf</span><span class="sxs-lookup"><span data-stu-id="67287-369">class</span></span>

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="67287-370">Razor İnceleme C# bir görünümü için oluşturulan sınıfı</span><span class="sxs-lookup"><span data-stu-id="67287-370">Inspect the Razor C# class generated for a view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="67287-371">.NET Core SDK'sını 2.1 veya daha sonra [Razor SDK](xref:razor-pages/sdk) derleme Razor dosyaları işler.</span><span class="sxs-lookup"><span data-stu-id="67287-371">With .NET Core SDK 2.1 or later, the [Razor SDK](xref:razor-pages/sdk) handles compilation of Razor files.</span></span> <span data-ttu-id="67287-372">Bir proje derlenirken Razor SDK'sı oluşturur bir *obj / < build_configuration > / < target_framework_moniker > / Razor* proje kök dizini.</span><span class="sxs-lookup"><span data-stu-id="67287-372">When building a project, the Razor SDK generates an *obj/<build_configuration>/<target_framework_moniker>/Razor* directory in the project root.</span></span> <span data-ttu-id="67287-373">Dizin yapısında *Razor* dizini proje dizin yapısına yansıtır.</span><span class="sxs-lookup"><span data-stu-id="67287-373">The directory structure within the *Razor* directory mirrors the project's directory structure.</span></span>

<span data-ttu-id="67287-374">.NET Core 2.1 hedefleyen ASP.NET Core 2.1 Razor sayfaları projesinde aşağıdaki dizin yapısını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="67287-374">Consider the following directory structure in an ASP.NET Core 2.1 Razor Pages project targeting .NET Core 2.1:</span></span>

* <span data-ttu-id="67287-375">**Alanlar /**</span><span class="sxs-lookup"><span data-stu-id="67287-375">**Areas/**</span></span>
  * <span data-ttu-id="67287-376">**Yönetim /**</span><span class="sxs-lookup"><span data-stu-id="67287-376">**Admin/**</span></span>
    * <span data-ttu-id="67287-377">**Sayfa /**</span><span class="sxs-lookup"><span data-stu-id="67287-377">**Pages/**</span></span>
      * <span data-ttu-id="67287-378">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="67287-378">*Index.cshtml*</span></span>
      * <span data-ttu-id="67287-379">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="67287-379">*Index.cshtml.cs*</span></span>
* <span data-ttu-id="67287-380">**Sayfa /**</span><span class="sxs-lookup"><span data-stu-id="67287-380">**Pages/**</span></span>
  * <span data-ttu-id="67287-381">**Paylaşılan /**</span><span class="sxs-lookup"><span data-stu-id="67287-381">**Shared/**</span></span>
    * <span data-ttu-id="67287-382">*_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="67287-382">*_Layout.cshtml*</span></span>
  * <span data-ttu-id="67287-383">*_Viewımports.cshtml*</span><span class="sxs-lookup"><span data-stu-id="67287-383">*_ViewImports.cshtml*</span></span>
  * <span data-ttu-id="67287-384">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="67287-384">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="67287-385">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="67287-385">*Index.cshtml*</span></span>
  * <span data-ttu-id="67287-386">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="67287-386">*Index.cshtml.cs*</span></span>

<span data-ttu-id="67287-387">Projenin oluşturulmasında *hata ayıklama* yapılandırma aşağıdaki verir *obj* dizini:</span><span class="sxs-lookup"><span data-stu-id="67287-387">Building the project in *Debug* configuration yields the following *obj* directory:</span></span>

* <span data-ttu-id="67287-388">**obj /**</span><span class="sxs-lookup"><span data-stu-id="67287-388">**obj/**</span></span>
  * <span data-ttu-id="67287-389">**Hata ayıklama /**</span><span class="sxs-lookup"><span data-stu-id="67287-389">**Debug/**</span></span>
    * <span data-ttu-id="67287-390">**netcoreapp2.1 /**</span><span class="sxs-lookup"><span data-stu-id="67287-390">**netcoreapp2.1/**</span></span>
      * <span data-ttu-id="67287-391">**Razor /**</span><span class="sxs-lookup"><span data-stu-id="67287-391">**Razor/**</span></span>
        * <span data-ttu-id="67287-392">**Alanlar /**</span><span class="sxs-lookup"><span data-stu-id="67287-392">**Areas/**</span></span>
          * <span data-ttu-id="67287-393">**Yönetim /**</span><span class="sxs-lookup"><span data-stu-id="67287-393">**Admin/**</span></span>
            * <span data-ttu-id="67287-394">**Sayfa /**</span><span class="sxs-lookup"><span data-stu-id="67287-394">**Pages/**</span></span>
              * <span data-ttu-id="67287-395">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="67287-395">*Index.g.cshtml.cs*</span></span>
        * <span data-ttu-id="67287-396">**Sayfa /**</span><span class="sxs-lookup"><span data-stu-id="67287-396">**Pages/**</span></span>
          * <span data-ttu-id="67287-397">**Paylaşılan /**</span><span class="sxs-lookup"><span data-stu-id="67287-397">**Shared/**</span></span>
            * <span data-ttu-id="67287-398">*_Layout.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="67287-398">*_Layout.g.cshtml.cs*</span></span>
          * <span data-ttu-id="67287-399">*_ViewImports.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="67287-399">*_ViewImports.g.cshtml.cs*</span></span>
          * <span data-ttu-id="67287-400">*_ViewStart.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="67287-400">*_ViewStart.g.cshtml.cs*</span></span>
          * <span data-ttu-id="67287-401">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="67287-401">*Index.g.cshtml.cs*</span></span>

<span data-ttu-id="67287-402">Oluşturulan sınıf için görüntülenecek *Pages/Index.cshtml*açın *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="67287-402">To view the generated class for *Pages/Index.cshtml*, open *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="67287-403">Aşağıdaki sınıf, ASP.NET Core MVC projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="67287-403">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="67287-404">İçinde `Startup.ConfigureServices`, geçersiz kılma `RazorTemplateEngine` ile MVC tarafından eklenen `CustomTemplateEngine` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="67287-404">In `Startup.ConfigureServices`, override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="67287-405">Bir kesme noktası ayarlamak `return csharpDocument;` deyiminin `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="67287-405">Set a breakpoint on the `return csharpDocument;` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="67287-406">Program yürütme kesme noktasında durduğunda değerini görüntülemek `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="67287-406">When program execution stops at the breakpoint, view the value of `generatedCode`.</span></span>

![Metin Görselleştirici generatedCode görünümünü](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="67287-408">Görünüm aramaları ve büyük/küçük harfe duyarlılık</span><span class="sxs-lookup"><span data-stu-id="67287-408">View lookups and case sensitivity</span></span>

<span data-ttu-id="67287-409">Razor görüntüleme motorunu büyük küçük harfe duyarlı aramalar, görünümler için gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="67287-409">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="67287-410">Ancak, gerçek arama, temel alınan dosya sistemi tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="67287-410">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="67287-411">Dosya tabanlı kaynağı:</span><span class="sxs-lookup"><span data-stu-id="67287-411">File based source:</span></span>
  * <span data-ttu-id="67287-412">Büyük küçük harfe duyarlı dosya sistemleri (örneğin, Windows) ile işletim sistemlerinde, fiziksel dosya sağlayıcısı aramaları büyük küçük harfe duyarlı.</span><span class="sxs-lookup"><span data-stu-id="67287-412">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="67287-413">Örneğin, `return View("Test")` eşleşmelerini sonuçlanıyor */Views/Home/Test.cshtml*, */Views/home/test.cshtml*ve diğer büyük/küçük harf değişken.</span><span class="sxs-lookup"><span data-stu-id="67287-413">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="67287-414">Büyük küçük harfe duyarlı dosya sistemlerindeki (örneğin, Linux, OSX ile `EmbeddedFileProvider`), büyük küçük harfe duyarlı aramalar.</span><span class="sxs-lookup"><span data-stu-id="67287-414">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="67287-415">Örneğin, `return View("Test")` özellikle eşleşen */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="67287-415">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="67287-416">Önceden derlenmiş görünümler: ASP.NET Core 2,0 ve üzeri sürümlerde, önceden derlenmiş görünümleri aramak tüm işletim sistemlerinde büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="67287-416">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="67287-417">Davranış Windows fiziksel dosya Sağlayıcısı'nın davranış aynıdır.</span><span class="sxs-lookup"><span data-stu-id="67287-417">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="67287-418">Önceden derlenmiş iki görünüm yalnızca durumda farklıysa, arama sonucu belirleyici değildir.</span><span class="sxs-lookup"><span data-stu-id="67287-418">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="67287-419">Geliştiriciler, dosya ve dizin adlarını büyük küçük harfleri büyük/küçük harf eşleşmesi için önerilir:</span><span class="sxs-lookup"><span data-stu-id="67287-419">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

* <span data-ttu-id="67287-420">Alan, denetleyici ve eylem adları.</span><span class="sxs-lookup"><span data-stu-id="67287-420">Area, controller, and action names.</span></span>
* <span data-ttu-id="67287-421">Razor sayfaları.</span><span class="sxs-lookup"><span data-stu-id="67287-421">Razor Pages.</span></span>

<span data-ttu-id="67287-422">Eşleşen servis talebi, temel alınan dosya sisteminden bağımsız olarak kendi görünümler dağıtımları Bul sağlar.</span><span class="sxs-lookup"><span data-stu-id="67287-422">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
