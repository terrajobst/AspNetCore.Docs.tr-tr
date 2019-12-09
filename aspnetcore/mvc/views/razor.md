---
title: ASP.NET Core Razor söz dizimi başvurusu
author: rick-anderson
description: Kodu sunucu tabanlı Web sayfalarını eklemek için Razor söz dizimi biçimlendirme hakkında bilgi edinin.
ms.author: riande
ms.date: 12/05/2019
uid: mvc/views/razor
ms.openlocfilehash: baac0ac38a0781cb9c16689cf3e29526b602d8da
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944258"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a><span data-ttu-id="bb5d2-103">ASP.NET Core Razor söz dizimi başvurusu</span><span class="sxs-lookup"><span data-stu-id="bb5d2-103">Razor syntax reference for ASP.NET Core</span></span>

<span data-ttu-id="bb5d2-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), ve [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="bb5d2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="bb5d2-105">Razor kod sunucu tabanlı Web sayfalarını eklemek için bir biçimlendirme sözdizimi aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="bb5d2-106">Razor işaretlemesi Razor sözdizimini oluşur C#ve HTML.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="bb5d2-107">Razor genellikle içeren dosyalarınız bir *.cshtml* dosya uzantısı.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-107">Files containing Razor generally have a *.cshtml* file extension.</span></span> <span data-ttu-id="bb5d2-108">Razor, [Razor bileşenleri](xref:blazor/components) dosyalarında ( *. Razor*) de bulunur.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-108">Razor is also found in [Razor components](xref:blazor/components) files (*.razor*).</span></span>

## <a name="rendering-html"></a><span data-ttu-id="bb5d2-109">HTML işleme</span><span class="sxs-lookup"><span data-stu-id="bb5d2-109">Rendering HTML</span></span>

<span data-ttu-id="bb5d2-110">HTML varsayılan Razor dilidir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-110">The default Razor language is HTML.</span></span> <span data-ttu-id="bb5d2-111">HTML Razor işaretlemesi işleme bir HTML dosyasından HTML'yi işlemeye değerinden farklı değildir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="bb5d2-112">HTML biçimlendirmede *.cshtml* Razor dosyaları değiştirilmemiş sunucu tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-112">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="bb5d2-113">Razor söz dizimi</span><span class="sxs-lookup"><span data-stu-id="bb5d2-113">Razor syntax</span></span>

<span data-ttu-id="bb5d2-114">Razor destekler C# ve kullandığı `@` HTML geçiş sembolünden C#.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="bb5d2-115">Razor değerlendirir C# ifadeleri ve bunları HTML çıktısında oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="bb5d2-116">Olduğunda bir `@` sembol tarafından izlenen bir [Razor ayrılmış anahtar sözcüğü](#razor-reserved-keywords), Razor özgü biçimlendirme geçer.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="bb5d2-117">Aksi takdirde düz geçiş C#.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="bb5d2-118">Kaçış için bir `@` sembol Razor işaretlemede, ikinci bir kullanın `@` sembol:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="bb5d2-119">Kodunu HTML ile tek bir işlenen `@` sembol:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="bb5d2-120">Olmayan HTML öznitelikleri ve e-posta adreslerini içeren bir içerik işleme `@` geçiş karakter olarak sembol.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="bb5d2-121">Aşağıdaki örnekte e-posta adresleri tarafından Razor ayrıştırma olduğu:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="bb5d2-122">Örtük Razor ifadeleri</span><span class="sxs-lookup"><span data-stu-id="bb5d2-122">Implicit Razor expressions</span></span>

<span data-ttu-id="bb5d2-123">Örtük Razor ifadeleri ile başlayıp `@` ardından C# kod:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="bb5d2-124">Dışında C# `await` anahtar sözcüğü, örtük ifadeleri boşluk içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="bb5d2-125">Varsa C# deyimi açık bir bitiş sahipse, boşluk intermingled:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-125">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="bb5d2-126">Örtük ifade **olamaz** içeren C# köşeli ayraçlar içindeki karakterler olarak genel türler (`<>`) bir HTML etiketi yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-126">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="bb5d2-127">Aşağıdaki kod **değil** geçerli:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-127">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="bb5d2-128">Yukarıdaki kod, aşağıdakilerden birini benzer bir derleyici hatası oluşturur:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-128">The preceding code generates a compiler error similar to one of the following:</span></span>

* <span data-ttu-id="bb5d2-129">"İnt" öğesi kapalı değildi.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-129">The "int" element wasn't closed.</span></span> <span data-ttu-id="bb5d2-130">Tüm öğeleri olmalıdır kendi kendine kapanan veya eşleşen bir bitiş etiketi sahip.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-130">All elements must be either self-closing or have a matching end tag.</span></span>
* <span data-ttu-id="bb5d2-131">Yöntem grubu 'object' türü temsilci GenericMethod' dönüştürülemiyor.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-131">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="bb5d2-132">Bir yöntemi çağırmak mı istiyordunuz?'</span><span class="sxs-lookup"><span data-stu-id="bb5d2-132">Did you intend to invoke the method?\`</span></span>

<span data-ttu-id="bb5d2-133">Genel yöntem çağrılarını sarmalanmış, içinde bir [Razor açık ifadesi](#explicit-razor-expressions) veya [Razor kodu bloğu](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="bb5d2-133">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="bb5d2-134">Açık Razor ifadeleri</span><span class="sxs-lookup"><span data-stu-id="bb5d2-134">Explicit Razor expressions</span></span>

<span data-ttu-id="bb5d2-135">Açık Razor ifadeleri oluşur bir `@` dengeli parantez ile simge.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-135">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="bb5d2-136">Geçen haftaki zaman işlemek için aşağıdaki Razor işaretlemesi kullanılır:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-136">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="bb5d2-137">İçinde herhangi bir içerik `@()` parantez değerlendirilir ve işlenen çıkışı.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-137">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="bb5d2-138">Önceki bölümde açıklanan, örtük ifadeleri genellikle boşluk içeremez.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-138">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="bb5d2-139">Aşağıdaki kodda, bir hafta, geçerli saatten çıkarılabilir değil:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-139">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="bb5d2-140">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-140">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="bb5d2-141">Açık ifadeler, metin bir ifadenin sonucu ile birleştirmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-141">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="bb5d2-142">Açık bir ifade olmadan `<p>Age@joe.Age</p>` bir e-posta adresi kabul edilir ve `<p>Age@joe.Age</p>` işlenir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-142">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="bb5d2-143">Açık bir ifade olarak yazılırken `<p>Age33</p>` işlenir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-143">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<span data-ttu-id="bb5d2-144">Açık ifadeleri, genel yöntemleri gelen çıkış işleme için kullanılabilir *.cshtml* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-144">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="bb5d2-145">Aşağıdaki biçimlendirmede gösterilen hatayı düzeltmek gösterilmektedir köşeli parantez önceki neden bir C# genel.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-145">The following markup shows how to correct the error shown earlier caused by the brackets of a C# generic.</span></span> <span data-ttu-id="bb5d2-146">Kodu açık bir ifade olarak yazılır:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-146">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="bb5d2-147">İfade kodlama</span><span class="sxs-lookup"><span data-stu-id="bb5d2-147">Expression encoding</span></span>

<span data-ttu-id="bb5d2-148">C#HTML kodlu bir dizeye değerlendirilen ifadeleri var.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-148">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="bb5d2-149">C#değerlendirmek için ifadeleri `IHtmlContent` doğrudan aracılığıyla işlenir `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-149">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="bb5d2-150">C#Değerlendirme yok ifadeleri `IHtmlContent` tarafından bir dizeye dönüştürülür `ToString` ve bunlar işlenen önce kodlanır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-150">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="bb5d2-151">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-151">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="bb5d2-152">HTML tarayıcı gösterilir:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-152">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="bb5d2-153">`HtmlHelper.Raw` Çıktı kodlanmış değildir, ancak geri HTML biçimlendirmesi olarak çizilir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-153">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="bb5d2-154">Kullanarak `HtmlHelper.Raw` unsanitized kullanıcı girişi bir güvenlik riski oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-154">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="bb5d2-155">Kullanıcı girişi, kötü amaçlı JavaScript veya diğer saldırılara içerebilir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-155">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="bb5d2-156">Kullanıcı girişi temizlenirken zordur.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-156">Sanitizing user input is difficult.</span></span> <span data-ttu-id="bb5d2-157">Kullanmaktan kaçının `HtmlHelper.Raw` kullanıcı girişi ile.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-157">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="bb5d2-158">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-158">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="bb5d2-159">Razor kodu bloğu</span><span class="sxs-lookup"><span data-stu-id="bb5d2-159">Razor code blocks</span></span>

<span data-ttu-id="bb5d2-160">Razor kod blokları kullanmaya başlama `@` ve tarafından alınmış `{}`.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-160">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="bb5d2-161">İfadeler, aksine C# olmayan kod blokları içinde kod çizilir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-161">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="bb5d2-162">Kod blokları ve ifadeler bir görünümdeki aynı kapsamı paylaşan ve sırayla tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-162">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="bb5d2-163">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-163">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bb5d2-164">Kod blokları ' nda, şablon oluşturma yöntemleri olarak kullanılacak biçimlendirme ile [yerel işlevler](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) bildirin:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-164">In code blocks, declare [local functions](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) with markup to serve as templating methods:</span></span>

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

<span data-ttu-id="bb5d2-165">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-165">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="implicit-transitions"></a><span data-ttu-id="bb5d2-166">Örtük geçişleri</span><span class="sxs-lookup"><span data-stu-id="bb5d2-166">Implicit transitions</span></span>

<span data-ttu-id="bb5d2-167">Bir kod bloğu varsayılan dilde C#, ancak Razor sayfası HTML olarak geçiş yapabilir:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-167">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="bb5d2-168">Açık ayrılmış geçiş</span><span class="sxs-lookup"><span data-stu-id="bb5d2-168">Explicit delimited transition</span></span>

<span data-ttu-id="bb5d2-169">HTML oluşturması gereken bir kod bloğunun alt bölümünü tanımlamak için, karakterleri Razor `<text>` etiketiyle çevreleyin:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-169">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor `<text>` tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="bb5d2-170">Tarafından HTML etiketleri arasına olmayan HTML oluşturmak için bu yaklaşımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-170">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="bb5d2-171">Bir HTML veya Razor etiket olmadan, bir Razor çalışma zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-171">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="bb5d2-172">`<text>` etiketi, içerik işlerken boşluğu denetlemek için yararlıdır:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-172">The `<text>` tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="bb5d2-173">Yalnızca `<text>` etiketi arasındaki içerik işlenir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-173">Only the content between the `<text>` tag is rendered.</span></span>
* <span data-ttu-id="bb5d2-174">HTML çıktısında `<text>` etiketi görüntülenmeden önce veya sonra boşluk yok.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-174">No whitespace before or after the `<text>` tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition"></a><span data-ttu-id="bb5d2-175">Açık satır geçişi</span><span class="sxs-lookup"><span data-stu-id="bb5d2-175">Explicit line transition</span></span>

<span data-ttu-id="bb5d2-176">Tüm satırın geri kalanını bir kod bloğu içinde HTML olarak işlemek için `@:` söz dizimini kullanın:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-176">To render the rest of an entire line as HTML inside a code block, use `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="bb5d2-177">Olmadan `@:` kodda bir Razor çalışma zamanı hatası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-177">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="bb5d2-178">Razor dosyasındaki ek `@` karakterler, bloktaki daha sonra bulunan deyimlerde derleyici hatalarına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-178">Extra `@` characters in a Razor file can cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="bb5d2-179">Bu derleyici hataları önce bildirilen hatayı gerçek bir hata oluştuğu için anlamak zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-179">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="bb5d2-180">Bu hata, tek bir kod bloğunun birden çok örtük/açık ifadelere birleştirdikten sonra yaygındır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-180">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="bb5d2-181">Denetim yapıları</span><span class="sxs-lookup"><span data-stu-id="bb5d2-181">Control structures</span></span>

<span data-ttu-id="bb5d2-182">Denetim yapıları kod bloğu bir uzantı var.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-182">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="bb5d2-183">Kod blokları tüm yönlerini (biçimlendirme, satır içi geçiş C#) aşağıdaki yapılar için de geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-183">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="bb5d2-184">Conditionals, else if, Else ve \@Switch \@</span><span class="sxs-lookup"><span data-stu-id="bb5d2-184">Conditionals \@if, else if, else, and \@switch</span></span>

<span data-ttu-id="bb5d2-185">`@if` kod çalıştığında denetimleri:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-185">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="bb5d2-186">`else` ve `else if` gerektirmeyen `@` sembol:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-186">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="bb5d2-187">Aşağıdaki biçimlendirme switch deyimi kullanma işlemini gösterir:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-187">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="bb5d2-188">İçin döngü \@, \@foreach, \@sırasında ve \@do</span><span class="sxs-lookup"><span data-stu-id="bb5d2-188">Looping \@for, \@foreach, \@while, and \@do while</span></span>

<span data-ttu-id="bb5d2-189">Şablonlu HTML denetim ifadeleri döngü ile oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-189">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="bb5d2-190">Kişi listesini oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-190">To render a list of people:</span></span>

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

<span data-ttu-id="bb5d2-191">Aşağıdaki döngü deyimi desteklenir:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-191">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="bb5d2-192">Kullanarak bileşik \@</span><span class="sxs-lookup"><span data-stu-id="bb5d2-192">Compound \@using</span></span>

<span data-ttu-id="bb5d2-193">İçinde C#, `using` deyimi, bir nesne kullanıldığında emin olmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-193">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="bb5d2-194">Razor aynı mekanizmayı ek içeriklere sahip bir HTML Yardımcıları oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-194">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="bb5d2-195">Aşağıdaki kodda, HTML Yardımcıları `@using` ifadesiyle bir `<form>` etiketi işlerler:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-195">In the following code, HTML Helpers render a `<form>` tag with the `@using` statement:</span></span>

```cshtml
@using (Html.BeginForm())
{
    <div>
        Email: <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

### <a name="try-catch-finally"></a><span data-ttu-id="bb5d2-196">\@TRY, catch, finally</span><span class="sxs-lookup"><span data-stu-id="bb5d2-196">\@try, catch, finally</span></span>

<span data-ttu-id="bb5d2-197">Özel durum işleme benzer C#:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-197">Exception handling is similar to C#:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a><span data-ttu-id="bb5d2-198">\@kilidi</span><span class="sxs-lookup"><span data-stu-id="bb5d2-198">\@lock</span></span>

<span data-ttu-id="bb5d2-199">Razor kilit deyimleri kritik bölümlerle korumanın yeteneğine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-199">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="bb5d2-200">Açıklamalar</span><span class="sxs-lookup"><span data-stu-id="bb5d2-200">Comments</span></span>

<span data-ttu-id="bb5d2-201">Razor destekler C# ve HTML yorumlarında:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-201">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="bb5d2-202">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-202">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="bb5d2-203">Web sayfası işlenmeden önce razor açıklama sunucu tarafından kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-203">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="bb5d2-204">Razor kullanan `@*  *@` açıklamaları sınırlandırmak için.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-204">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="bb5d2-205">Sunucu tüm biçimlendirme işlemesi yapmıyor için aşağıdaki kodu, geçersiz kılınan:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-205">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="bb5d2-206">Yönergeler</span><span class="sxs-lookup"><span data-stu-id="bb5d2-206">Directives</span></span>

<span data-ttu-id="bb5d2-207">Razor yönergesi örtük ayrılmış anahtar sözcükler aşağıdaki deyimlerle tarafından temsil edilir `@` simgesi.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-207">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="bb5d2-208">Bir yönergesi, genellikle bir görünüm ayrıştırılır veya farklı bir işlevsellik sağlar şeklini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-208">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="bb5d2-209">Razor kod görünümü nasıl oluşturur? anlama yönergeleri nasıl çalıştığını anlamak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-209">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="bb5d2-210">Kod aşağıdaki gibi bir sınıf oluşturur:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-210">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="bb5d2-211">Bölümü bu makalenin ilerleyen bölümlerinde [Razor İnceleme C# bir görünümü için oluşturulan sınıf](#inspect-the-razor-c-class-generated-for-a-view) bu oluşturulan sınıf görüntülemek açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-211">Later in this article, the section [Inspect the Razor C# class generated for a view](#inspect-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="attribute"></a><span data-ttu-id="bb5d2-212">\@özniteliği</span><span class="sxs-lookup"><span data-stu-id="bb5d2-212">\@attribute</span></span>

<span data-ttu-id="bb5d2-213">`@attribute` yönergesi, verilen özniteliği oluşturulan sayfanın veya görünümün sınıfına ekler.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-213">The `@attribute` directive adds the given attribute to the class of the generated page or view.</span></span> <span data-ttu-id="bb5d2-214">Aşağıdaki örnek `[Authorize]` özniteliğini ekler:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-214">The following example adds the `[Authorize]` attribute:</span></span>

```cshtml
@attribute [Authorize]
```

::: moniker range=">= aspnetcore-3.0"

### <a name="code"></a><span data-ttu-id="bb5d2-215">\@kodu</span><span class="sxs-lookup"><span data-stu-id="bb5d2-215">\@code</span></span>

<span data-ttu-id="bb5d2-216">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-216">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="bb5d2-217">`@code` bloğu bir [Razor bileşeninin](xref:blazor/components) bir bileşene üye ( C# alanlar, Özellikler ve Yöntemler) eklemesini sağlar:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-217">The `@code` block enables a [Razor component](xref:blazor/components) to add C# members (fields, properties, and methods) to a component:</span></span>

```razor
@code {
    // C# members (fields, properties, and methods)
}
```

<span data-ttu-id="bb5d2-218">Razor bileşenleri için, `@code` [`@functions`](#functions) diğer adı `@functions`üzerinde önerilir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-218">For Razor components, `@code` is an alias of [`@functions`](#functions) and recommended over `@functions`.</span></span> <span data-ttu-id="bb5d2-219">Birden fazla `@code` bloğu izin verilir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-219">More than one `@code` block is permissible.</span></span>

::: moniker-end

### <a name="functions"></a><span data-ttu-id="bb5d2-220">\@işlevleri</span><span class="sxs-lookup"><span data-stu-id="bb5d2-220">\@functions</span></span>

<span data-ttu-id="bb5d2-221">`@functions` yönergesi, oluşturulan sınıfa C# üye (alanlar, Özellikler ve Yöntemler) eklemeyi sağlar:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-221">The `@functions` directive enables adding C# members (fields, properties, and methods) to the generated class:</span></span>

```cshtml
@functions {
    // C# members (fields, properties, and methods)
}
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bb5d2-222">[Razor bileşenlerinde](xref:blazor/components), üye eklemek C# için `@functions` üzerinde `@code` kullanın.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-222">In [Razor components](xref:blazor/components), use `@code` over `@functions` to add C# members.</span></span>

::: moniker-end

<span data-ttu-id="bb5d2-223">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-223">For example:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="bb5d2-224">Kod aşağıdaki HTML biçimlendirmeyi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-224">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="bb5d2-225">Aşağıdaki kodu oluşturulmuş Razor olan C# sınıfı:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-225">The following code is the generated Razor C# class:</span></span>

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bb5d2-226">`@functions` Yöntemler, biçimlendirme olduğunda şablon oluşturma yöntemleri olarak görev yapar:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-226">`@functions` methods serve as templating methods when they have markup:</span></span>

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

<span data-ttu-id="bb5d2-227">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-227">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

### <a name="implements"></a><span data-ttu-id="bb5d2-228">\@uygular</span><span class="sxs-lookup"><span data-stu-id="bb5d2-228">\@implements</span></span>

<span data-ttu-id="bb5d2-229">`@implements` yönergesi, oluşturulan sınıf için bir arabirim uygular.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-229">The `@implements` directive implements an interface for the generated class.</span></span>

<span data-ttu-id="bb5d2-230">Aşağıdaki örnek, <xref:System.IDisposable.Dispose*> yönteminin çağrılabilmesi için <xref:System.IDisposable?displayProperty=fullName> uygular:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-230">The following example implements <xref:System.IDisposable?displayProperty=fullName> so that the <xref:System.IDisposable.Dispose*> method can be called:</span></span>

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

### <a name="inherits"></a><span data-ttu-id="bb5d2-231">\@devralır</span><span class="sxs-lookup"><span data-stu-id="bb5d2-231">\@inherits</span></span>

<span data-ttu-id="bb5d2-232">`@inherits` Yönergesi görünümü devralan sınıf tam denetim sağlar:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-232">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="bb5d2-233">Aşağıdaki kod bir özel bir Razor sayfası türüdür:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-233">The following code is a custom Razor page type:</span></span>

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="bb5d2-234">`CustomText` Bir görünümde görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-234">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="bb5d2-235">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-235">The code renders the following HTML:</span></span>

```html
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

 <span data-ttu-id="bb5d2-236">`@model` ve `@inherits` aynı görünümde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-236">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="bb5d2-237">`@inherits` kullanılabilir bir *_viewımports.cshtml* görünümü alır dosyası:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-237">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="bb5d2-238">Aşağıdaki kod, kesin türü belirtilmiş görünüm örneğidir:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-238">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="bb5d2-239">Varsa "rick@contoso.com" geçirilen modeldeki görünümünü aşağıdaki HTML biçimlendirmeyi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-239">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

### <a name="inject"></a><span data-ttu-id="bb5d2-240">\@ekleme</span><span class="sxs-lookup"><span data-stu-id="bb5d2-240">\@inject</span></span>

<span data-ttu-id="bb5d2-241">`@inject` Yönergesi sağlayan bir hizmet eklemesine Razor sayfası [hizmet kapsayıcı](xref:fundamentals/dependency-injection) bir görünümde.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-241">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="bb5d2-242">Daha fazla bilgi için [görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="bb5d2-242">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="layout"></a><span data-ttu-id="bb5d2-243">\@düzeni</span><span class="sxs-lookup"><span data-stu-id="bb5d2-243">\@layout</span></span>

<span data-ttu-id="bb5d2-244">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-244">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="bb5d2-245">`@layout` yönergesi Razor bileşeninin bir yerleşimini belirtir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-245">The `@layout` directive specifies a layout for a Razor component.</span></span> <span data-ttu-id="bb5d2-246">Düzen bileşenleri kod yinelemeyi ve tutarsızlığın önüne geçmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-246">Layout components are used to avoid code duplication and inconsistency.</span></span> <span data-ttu-id="bb5d2-247">Daha fazla bilgi için bkz. <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-247">For more information, see <xref:blazor/layouts>.</span></span>

::: moniker-end

### <a name="model"></a><span data-ttu-id="bb5d2-248">\@modeli</span><span class="sxs-lookup"><span data-stu-id="bb5d2-248">\@model</span></span>

<span data-ttu-id="bb5d2-249">*Bu senaryo yalnızca MVC görünümleri ve Razor Pages (. cshtml) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-249">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="bb5d2-250">`@model` yönergesi, bir görünüme veya sayfaya geçirilen modelin türünü belirtir:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-250">The `@model` directive specifies the type of the model passed to a view or page:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="bb5d2-251">Bir ASP.NET Core MVC veya bireysel kullanıcı hesaplarıyla oluşturulan Razor Pages uygulama, *Görünümler/Account/Login. cshtml* aşağıdaki model bildirimini içerir:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-251">In an ASP.NET Core MVC or Razor Pages app created with individual user accounts, *Views/Account/Login.cshtml* contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="bb5d2-252">Oluşturulan sınıf devraldığı `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-252">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="bb5d2-253">Razor sunan bir `Model` modeli erişmek için özelliği geçirilen görünümü:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-253">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="bb5d2-254">`@model` yönergesi `Model` özelliğinin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-254">The `@model` directive specifies the type of the `Model` property.</span></span> <span data-ttu-id="bb5d2-255">Yönergesi belirtir `T` içinde `RazorPage<T>` türetildiği görünümü oluşturulan, sınıfın.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-255">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="bb5d2-256">Varsa `@model` yönergesi belirtilmediyse, `Model` özelliği türüdür `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-256">If the `@model` directive isn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="bb5d2-257">Daha fazla bilgi için bkz. [türü kesin belirlenmiş modeller ve @model anahtar sözcüğü](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span><span class="sxs-lookup"><span data-stu-id="bb5d2-257">For more information, see [Strongly typed models and the @model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span></span>

### <a name="namespace"></a><span data-ttu-id="bb5d2-258">\@ad alanı</span><span class="sxs-lookup"><span data-stu-id="bb5d2-258">\@namespace</span></span>

<span data-ttu-id="bb5d2-259">`@namespace` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-259">The `@namespace` directive:</span></span>

* <span data-ttu-id="bb5d2-260">Oluşturulan Razor sayfası, MVC görünümü veya Razor bileşeni sınıfının ad alanını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-260">Sets the namespace of the class of the generated Razor page, MVC view, or Razor component.</span></span>
* <span data-ttu-id="bb5d2-261">Bir sayfa, görünüm veya bileşen sınıfının kök türetilmiş ad alanlarını dizin ağacındaki en yakın içeri aktarmalar dosyasından, *_ViewImports. cshtml* (görünümler veya sayfalar) veya *_Imports. Razor* (Razor bileşenleri) olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-261">Sets the root derived namespaces of a pages, views, or components classes from the closest imports file in the directory tree, *_ViewImports.cshtml* (views or pages) or *_Imports.razor* (Razor components).</span></span>

```cshtml
@namespace Your.Namespace.Here
```

<span data-ttu-id="bb5d2-262">Aşağıdaki tabloda gösterilen Razor Pages örneği için:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-262">For the Razor Pages example shown in the following table:</span></span>

* <span data-ttu-id="bb5d2-263">Her sayfa *sayfaları/_ViewImports. cshtml*'yi içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-263">Each page imports *Pages/_ViewImports.cshtml*.</span></span>
* <span data-ttu-id="bb5d2-264">*Pages/_ViewImports. cshtml* `@namespace Hello.World`içerir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-264">*Pages/_ViewImports.cshtml* contains `@namespace Hello.World`.</span></span>
* <span data-ttu-id="bb5d2-265">Her sayfa, ad alanının kökü olarak `Hello.World` sahiptir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-265">Each page has `Hello.World` as the root of it's namespace.</span></span>

| <span data-ttu-id="bb5d2-266">Sayfa</span><span class="sxs-lookup"><span data-stu-id="bb5d2-266">Page</span></span>                                        | <span data-ttu-id="bb5d2-267">Ad alanı</span><span class="sxs-lookup"><span data-stu-id="bb5d2-267">Namespace</span></span>                             |
| ------------------------------------------- | ------------------------------------- |
| <span data-ttu-id="bb5d2-268">*Pages/Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-268">*Pages/Index.cshtml*</span></span>                        | `Hello.World`                         |
| <span data-ttu-id="bb5d2-269">*Sayfa/fazla sayfa/sayfa. cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-269">*Pages/MorePages/Page.cshtml*</span></span>               | `Hello.World.MorePages`               |
| <span data-ttu-id="bb5d2-270">*Pages/te Pages/Evente Pages/Page. cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-270">*Pages/MorePages/EvenMorePages/Page.cshtml*</span></span> | `Hello.World.MorePages.EvenMorePages` |

<span data-ttu-id="bb5d2-271">Yukarıdaki ilişkiler, MVC görünümleri ve Razor bileşenleriyle kullanılan dosyaları içeri aktarmak için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-271">The preceding relationships apply to import files used with MVC views and Razor components.</span></span>

<span data-ttu-id="bb5d2-272">Birden çok içeri aktarma dosyası `@namespace` yönergesine sahip olduğunda, kök ad alanını ayarlamak için dizin ağacındaki sayfaya, görünüme veya bileşene en yakın dosya kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-272">When multiple import files have a `@namespace` directive, the file closest to the page, view, or component in the directory tree is used to set the root namespace.</span></span>

<span data-ttu-id="bb5d2-273">Yukarıdaki örnekteki *evente Pages* klasörünün `@namespace Another.Planet` bir içeri aktarmalar dosyası varsa (veya *sayfa/değer sayfaları/Evente Pages/Page. cshtml* dosyası `@namespace Another.Planet`içeriyorsa), sonuç aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-273">If the *EvenMorePages* folder in the preceding example has an imports file with `@namespace Another.Planet` (or the *Pages/MorePages/EvenMorePages/Page.cshtml* file contains `@namespace Another.Planet`), the result is shown in the following table.</span></span>

| <span data-ttu-id="bb5d2-274">Sayfa</span><span class="sxs-lookup"><span data-stu-id="bb5d2-274">Page</span></span>                                        | <span data-ttu-id="bb5d2-275">Ad alanı</span><span class="sxs-lookup"><span data-stu-id="bb5d2-275">Namespace</span></span>               |
| ------------------------------------------- | ----------------------- |
| <span data-ttu-id="bb5d2-276">*Pages/Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-276">*Pages/Index.cshtml*</span></span>                        | `Hello.World`           |
| <span data-ttu-id="bb5d2-277">*Sayfa/fazla sayfa/sayfa. cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-277">*Pages/MorePages/Page.cshtml*</span></span>               | `Hello.World.MorePages` |
| <span data-ttu-id="bb5d2-278">*Pages/te Pages/Evente Pages/Page. cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-278">*Pages/MorePages/EvenMorePages/Page.cshtml*</span></span> | `Another.Planet`        |

### <a name="page"></a><span data-ttu-id="bb5d2-279">\@sayfası</span><span class="sxs-lookup"><span data-stu-id="bb5d2-279">\@page</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bb5d2-280">`@page` yönergesinin göründüğü dosyanın türüne bağlı olarak farklı etkileri vardır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-280">The `@page` directive has different effects depending on the type of the file where it appears.</span></span> <span data-ttu-id="bb5d2-281">Yönergesi:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-281">The directive:</span></span>

* <span data-ttu-id="bb5d2-282">İçindeki bir *. cshtml* dosyasında, dosyanın bir Razor sayfası olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-282">In in a *.cshtml* file indicates that the file is a Razor Page.</span></span> <span data-ttu-id="bb5d2-283">Daha fazla bilgi için bkz. [özel rotalar](xref:razor-pages/index#custom-routes) ve <xref:razor-pages/index>.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-283">For more information, see [Custom routes](xref:razor-pages/index#custom-routes) and <xref:razor-pages/index>.</span></span>
* <span data-ttu-id="bb5d2-284">Bir Razor bileşeninin istekleri doğrudan işlemesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-284">Specifies that a Razor component should handle requests directly.</span></span> <span data-ttu-id="bb5d2-285">Daha fazla bilgi için bkz. <xref:blazor/routing>.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-285">For more information, see <xref:blazor/routing>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="bb5d2-286">Bir *. cshtml* dosyasının ilk satırındaki `@page` yönergesi, dosyanın bir Razor sayfası olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-286">The `@page` directive on the first line of a *.cshtml* file indicates that the file is a Razor Page.</span></span> <span data-ttu-id="bb5d2-287">Daha fazla bilgi için bkz. <xref:razor-pages/index>.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-287">For more information, see <xref:razor-pages/index>.</span></span>

::: moniker-end

### <a name="section"></a><span data-ttu-id="bb5d2-288">\@bölümü</span><span class="sxs-lookup"><span data-stu-id="bb5d2-288">\@section</span></span>

<span data-ttu-id="bb5d2-289">*Bu senaryo yalnızca MVC görünümleri ve Razor Pages (. cshtml) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-289">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="bb5d2-290">`@section` yönergesi, görünümler veya sayfaların HTML sayfasının farklı bölümlerinde içerik işlemesini sağlamak için [MVC ve Razor Pages düzenleriyle](xref:mvc/views/layout) birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-290">The `@section` directive is used in conjunction with [MVC and Razor Pages layouts](xref:mvc/views/layout) to enable views or pages to render content in different parts of the HTML page.</span></span> <span data-ttu-id="bb5d2-291">Daha fazla bilgi için bkz. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-291">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="using"></a><span data-ttu-id="bb5d2-292">kullanarak \@</span><span class="sxs-lookup"><span data-stu-id="bb5d2-292">\@using</span></span>

<span data-ttu-id="bb5d2-293">`@using` Yönergesi ekler C# `using` yönerge oluşturulmuş görünümü için:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-293">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bb5d2-294">[Razor bileşenlerinde](xref:blazor/components)`@using` Ayrıca hangi bileşenlerin kapsamda olduğunu denetler.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-294">In [Razor components](xref:blazor/components), `@using` also controls which components are in scope.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="directive-attributes"></a><span data-ttu-id="bb5d2-295">Yönerge öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="bb5d2-295">Directive attributes</span></span>

### <a name="attributes"></a><span data-ttu-id="bb5d2-296">\@öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="bb5d2-296">\@attributes</span></span>

<span data-ttu-id="bb5d2-297">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-297">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="bb5d2-298">`@attributes`, bir bileşenin bildirilmeyen öznitelikleri işlemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-298">`@attributes` allows a component to render non-declared attributes.</span></span> <span data-ttu-id="bb5d2-299">Daha fazla bilgi için bkz. <xref:blazor/components#attribute-splatting-and-arbitrary-parameters>.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-299">For more information, see <xref:blazor/components#attribute-splatting-and-arbitrary-parameters>.</span></span>

### <a name="bind"></a><span data-ttu-id="bb5d2-300">\@bağlama</span><span class="sxs-lookup"><span data-stu-id="bb5d2-300">\@bind</span></span>

<span data-ttu-id="bb5d2-301">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-301">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="bb5d2-302">Bileşenlerdeki veri bağlama `@bind` özniteliğiyle gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-302">Data binding in components is accomplished with the `@bind` attribute.</span></span> <span data-ttu-id="bb5d2-303">Daha fazla bilgi için bkz. <xref:blazor/components#data-binding>.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-303">For more information, see <xref:blazor/components#data-binding>.</span></span>

### <a name="onevent"></a><span data-ttu-id="bb5d2-304">{EVENT} üzerinde \@</span><span class="sxs-lookup"><span data-stu-id="bb5d2-304">\@on{EVENT}</span></span>

<span data-ttu-id="bb5d2-305">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-305">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="bb5d2-306">Razor, bileşenler için olay işleme özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-306">Razor provides event handling features for components.</span></span> <span data-ttu-id="bb5d2-307">Daha fazla bilgi için bkz. <xref:blazor/components#event-handling>.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-307">For more information, see <xref:blazor/components#event-handling>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.1"

### <a name="oneventpreventdefault"></a><span data-ttu-id="bb5d2-308">{EVENT}:p reventDefault \@</span><span class="sxs-lookup"><span data-stu-id="bb5d2-308">\@on{EVENT}:preventDefault</span></span>

<span data-ttu-id="bb5d2-309">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-309">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="bb5d2-310">Olay için varsayılan eylemi engeller.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-310">Prevents the default action for the event.</span></span>

### <a name="oneventstoppropagation"></a><span data-ttu-id="bb5d2-311">{EVENT} üzerinde \@: Stopyayma</span><span class="sxs-lookup"><span data-stu-id="bb5d2-311">\@on{EVENT}:stopPropagation</span></span>

<span data-ttu-id="bb5d2-312">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-312">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="bb5d2-313">Olay için olay yaymayı sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-313">Stops event propagation for the event.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="key"></a><span data-ttu-id="bb5d2-314">\@anahtarı</span><span class="sxs-lookup"><span data-stu-id="bb5d2-314">\@key</span></span>

<span data-ttu-id="bb5d2-315">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-315">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="bb5d2-316">`@key` Directive özniteliği, bileşenlerin, anahtar değerine göre öğelerin veya bileşenlerin korunmasını güvence altına almasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-316">The `@key` directive attribute causes the components diffing algorithm to guarantee preservation of elements or components based on the key's value.</span></span> <span data-ttu-id="bb5d2-317">Daha fazla bilgi için bkz. <xref:blazor/components#use-key-to-control-the-preservation-of-elements-and-components>.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-317">For more information, see <xref:blazor/components#use-key-to-control-the-preservation-of-elements-and-components>.</span></span>

### <a name="ref"></a><span data-ttu-id="bb5d2-318">\@ref</span><span class="sxs-lookup"><span data-stu-id="bb5d2-318">\@ref</span></span>

<span data-ttu-id="bb5d2-319">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-319">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="bb5d2-320">Bileşen başvuruları (`@ref`) bir bileşen örneğine başvurmak için bir yol sağlar, böylece bu örneğe komut verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-320">Component references (`@ref`) provide a way to reference a component instance so that you can issue commands to that instance.</span></span> <span data-ttu-id="bb5d2-321">Daha fazla bilgi için bkz. <xref:blazor/components#capture-references-to-components>.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-321">For more information, see <xref:blazor/components#capture-references-to-components>.</span></span>

### <a name="typeparam"></a><span data-ttu-id="bb5d2-322">\@typeparam</span><span class="sxs-lookup"><span data-stu-id="bb5d2-322">\@typeparam</span></span>

<span data-ttu-id="bb5d2-323">*Bu senaryo yalnızca Razor bileşenleri (. Razor) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-323">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="bb5d2-324">`@typeparam` yönergesi, oluşturulan bileşen sınıfı için genel bir tür parametresi bildirir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-324">The `@typeparam` directive declares a generic type parameter for the generated component class.</span></span> <span data-ttu-id="bb5d2-325">Daha fazla bilgi için bkz. <xref:blazor/components#generic-typed-components>.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-325">For more information, see <xref:blazor/components#generic-typed-components>.</span></span>

::: moniker-end

## <a name="templated-razor-delegates"></a><span data-ttu-id="bb5d2-326">Şablonlu Razor temsilciler</span><span class="sxs-lookup"><span data-stu-id="bb5d2-326">Templated Razor delegates</span></span>

<span data-ttu-id="bb5d2-327">Razor şablonları aşağıdaki biçimde bir kullanıcı Arabirimi parçacığı tanımlamanıza izin ver:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-327">Razor templates allow you to define a UI snippet with the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="bb5d2-328">Aşağıdaki örnekte, şablonlu Razor temsilci olarak belirtmek verilmektedir bir <xref:System.Func%602>.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-328">The following example illustrates how to specify a templated Razor delegate as a <xref:System.Func%602>.</span></span> <span data-ttu-id="bb5d2-329">[Dinamik tür](/dotnet/csharp/programming-guide/types/using-type-dynamic) temsilci kapsülleyen yönteminin parametresi için belirtilir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-329">The [dynamic type](/dotnet/csharp/programming-guide/types/using-type-dynamic) is specified for the parameter of the method that the delegate encapsulates.</span></span> <span data-ttu-id="bb5d2-330">Bir [nesne türü](/dotnet/csharp/language-reference/keywords/object) temsilcinin dönüş değeri olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-330">An [object type](/dotnet/csharp/language-reference/keywords/object) is specified as the return value of the delegate.</span></span> <span data-ttu-id="bb5d2-331">Şablon ile kullanılan bir <xref:System.Collections.Generic.List%601> , `Pet` olan bir `Name` özelliği.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-331">The template is used with a <xref:System.Collections.Generic.List%601> of `Pet` that has a `Name` property.</span></span>

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

<span data-ttu-id="bb5d2-332">Şablon ile işlenen `pets` tarafından sağlanan bir `foreach` deyimi:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-332">The template is rendered with `pets` supplied by a `foreach` statement:</span></span>

```cshtml
@foreach (var pet in pets)
{
    @petTemplate(pet)
}
```

<span data-ttu-id="bb5d2-333">İşlenmiş çıkışı:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-333">Rendered output:</span></span>

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

<span data-ttu-id="bb5d2-334">Bir yöntem bağımsız değişkeni olarak bir satır içi Razor şablonu da sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-334">You can also supply an inline Razor template as an argument to a method.</span></span> <span data-ttu-id="bb5d2-335">Aşağıdaki örnekte, `Repeat` yöntem Razor şablonu alır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-335">In the following example, the `Repeat` method receives a Razor template.</span></span> <span data-ttu-id="bb5d2-336">Yöntemi, HTML içerik ile sağlanan bir listeden öğeleri yineler üretmek için şablonu kullanır:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-336">The method uses the template to produce HTML content with repeats of items supplied from a list:</span></span>

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

<span data-ttu-id="bb5d2-337">Önceki örnekte, Evcil Hayvanlar listesi kullanarak `Repeat` yöntemi çağrıldığında:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-337">Using the list of pets from the prior example, the `Repeat` method is called with:</span></span>

* <span data-ttu-id="bb5d2-338"><xref:System.Collections.Generic.List%601> ' ın `Pet`.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-338"><xref:System.Collections.Generic.List%601> of `Pet`.</span></span>
* <span data-ttu-id="bb5d2-339">Her evcil hayvan yineleme sayısı.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-339">Number of times to repeat each pet.</span></span>
* <span data-ttu-id="bb5d2-340">Satır içi şablon sırasız bir listesini liste öğeleri için kullanın.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-340">Inline template to use for the list items of an unordered list.</span></span>

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

<span data-ttu-id="bb5d2-341">İşlenmiş çıkışı:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-341">Rendered output:</span></span>

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

## <a name="tag-helpers"></a><span data-ttu-id="bb5d2-342">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="bb5d2-342">Tag Helpers</span></span>

<span data-ttu-id="bb5d2-343">*Bu senaryo yalnızca MVC görünümleri ve Razor Pages (. cshtml) için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-343">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="bb5d2-344">İlgili üç yönergeleri vardır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="bb5d2-344">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="bb5d2-345">Yönergesi</span><span class="sxs-lookup"><span data-stu-id="bb5d2-345">Directive</span></span> | <span data-ttu-id="bb5d2-346">İşlev</span><span class="sxs-lookup"><span data-stu-id="bb5d2-346">Function</span></span> |
| --------- | -------- |
| [`@addTagHelper`](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="bb5d2-347">Etiket Yardımcıları bir görünüm için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-347">Makes Tag Helpers available to a view.</span></span> |
| [`@removeTagHelper`](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="bb5d2-348">Daha önce bir görünümden eklenen etiket Yardımcıları kaldırır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-348">Removes Tag Helpers previously added from a view.</span></span> |
| [`@tagHelperPrefix`](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="bb5d2-349">Etiket Yardımcısı desteğinin etkinleştirmek ve etiket Yardımcısı kullanım açık hale getirmek için bir etiket öneki belirtir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-349">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="bb5d2-350">Razor ayrılmış anahtar sözcükler</span><span class="sxs-lookup"><span data-stu-id="bb5d2-350">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="bb5d2-351">Razor anahtar sözcükleri</span><span class="sxs-lookup"><span data-stu-id="bb5d2-351">Razor keywords</span></span>

* <span data-ttu-id="bb5d2-352">sayfa (ASP.NET Core 2,1 veya üzeri bir sürüm gerektirir)</span><span class="sxs-lookup"><span data-stu-id="bb5d2-352">page (Requires ASP.NET Core 2.1 or later)</span></span>
* <span data-ttu-id="bb5d2-353">ad alanı</span><span class="sxs-lookup"><span data-stu-id="bb5d2-353">namespace</span></span>
* <span data-ttu-id="bb5d2-354">işlevleri</span><span class="sxs-lookup"><span data-stu-id="bb5d2-354">functions</span></span>
* <span data-ttu-id="bb5d2-355">Devralan</span><span class="sxs-lookup"><span data-stu-id="bb5d2-355">inherits</span></span>
* <span data-ttu-id="bb5d2-356">{1&gt;model&lt;1}</span><span class="sxs-lookup"><span data-stu-id="bb5d2-356">model</span></span>
* <span data-ttu-id="bb5d2-357">section</span><span class="sxs-lookup"><span data-stu-id="bb5d2-357">section</span></span>
* <span data-ttu-id="bb5d2-358">yardımcı (şu anda ASP.NET Core tarafından desteklenmez)</span><span class="sxs-lookup"><span data-stu-id="bb5d2-358">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="bb5d2-359">Razor anahtar sözcükleri kaçış ile `@(Razor Keyword)` (örneğin, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="bb5d2-359">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="bb5d2-360">C#Razor anahtar sözcükleri</span><span class="sxs-lookup"><span data-stu-id="bb5d2-360">C# Razor keywords</span></span>

* <span data-ttu-id="bb5d2-361">büyük/küçük harf</span><span class="sxs-lookup"><span data-stu-id="bb5d2-361">case</span></span>
* <span data-ttu-id="bb5d2-362">do</span><span class="sxs-lookup"><span data-stu-id="bb5d2-362">do</span></span>
* <span data-ttu-id="bb5d2-363">{1&gt;default&lt;1}</span><span class="sxs-lookup"><span data-stu-id="bb5d2-363">default</span></span>
* <span data-ttu-id="bb5d2-364">for</span><span class="sxs-lookup"><span data-stu-id="bb5d2-364">for</span></span>
* <span data-ttu-id="bb5d2-365">foreach</span><span class="sxs-lookup"><span data-stu-id="bb5d2-365">foreach</span></span>
* <span data-ttu-id="bb5d2-366">if</span><span class="sxs-lookup"><span data-stu-id="bb5d2-366">if</span></span>
* <span data-ttu-id="bb5d2-367">else</span><span class="sxs-lookup"><span data-stu-id="bb5d2-367">else</span></span>
* <span data-ttu-id="bb5d2-368">lock</span><span class="sxs-lookup"><span data-stu-id="bb5d2-368">lock</span></span>
* <span data-ttu-id="bb5d2-369">anahtarı</span><span class="sxs-lookup"><span data-stu-id="bb5d2-369">switch</span></span>
* <span data-ttu-id="bb5d2-370">deneyin</span><span class="sxs-lookup"><span data-stu-id="bb5d2-370">try</span></span>
* <span data-ttu-id="bb5d2-371">Yakalama</span><span class="sxs-lookup"><span data-stu-id="bb5d2-371">catch</span></span>
* <span data-ttu-id="bb5d2-372">finally</span><span class="sxs-lookup"><span data-stu-id="bb5d2-372">finally</span></span>
* <span data-ttu-id="bb5d2-373">kullanma</span><span class="sxs-lookup"><span data-stu-id="bb5d2-373">using</span></span>
* <span data-ttu-id="bb5d2-374">while</span><span class="sxs-lookup"><span data-stu-id="bb5d2-374">while</span></span>

<span data-ttu-id="bb5d2-375">C#Razor anahtar sözcükleri çift kaçış ile olmalıdır `@(@C# Razor Keyword)` (örneğin, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="bb5d2-375">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="bb5d2-376">İlk `@` Razor ayrıştırıcısı atlar.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-376">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="bb5d2-377">İkinci `@` çıkışları C# ayrıştırıcı.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-377">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="bb5d2-378">Razor tarafından kullanılmayan ayrılmış anahtar sözcükler</span><span class="sxs-lookup"><span data-stu-id="bb5d2-378">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="bb5d2-379">sınıf</span><span class="sxs-lookup"><span data-stu-id="bb5d2-379">class</span></span>

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="bb5d2-380">Razor İnceleme C# bir görünümü için oluşturulan sınıfı</span><span class="sxs-lookup"><span data-stu-id="bb5d2-380">Inspect the Razor C# class generated for a view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bb5d2-381">.NET Core SDK'sını 2.1 veya daha sonra [Razor SDK](xref:razor-pages/sdk) derleme Razor dosyaları işler.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-381">With .NET Core SDK 2.1 or later, the [Razor SDK](xref:razor-pages/sdk) handles compilation of Razor files.</span></span> <span data-ttu-id="bb5d2-382">Bir proje derlenirken Razor SDK'sı oluşturur bir *obj / < build_configuration > / < target_framework_moniker > / Razor* proje kök dizini.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-382">When building a project, the Razor SDK generates an *obj/<build_configuration>/<target_framework_moniker>/Razor* directory in the project root.</span></span> <span data-ttu-id="bb5d2-383">Dizin yapısında *Razor* dizini proje dizin yapısına yansıtır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-383">The directory structure within the *Razor* directory mirrors the project's directory structure.</span></span>

<span data-ttu-id="bb5d2-384">.NET Core 2.1 hedefleyen ASP.NET Core 2.1 Razor sayfaları projesinde aşağıdaki dizin yapısını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-384">Consider the following directory structure in an ASP.NET Core 2.1 Razor Pages project targeting .NET Core 2.1:</span></span>

* <span data-ttu-id="bb5d2-385">**Alanlar /**</span><span class="sxs-lookup"><span data-stu-id="bb5d2-385">**Areas/**</span></span>
  * <span data-ttu-id="bb5d2-386">**Yönetim /**</span><span class="sxs-lookup"><span data-stu-id="bb5d2-386">**Admin/**</span></span>
    * <span data-ttu-id="bb5d2-387">**Sayfa /**</span><span class="sxs-lookup"><span data-stu-id="bb5d2-387">**Pages/**</span></span>
      * <span data-ttu-id="bb5d2-388">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-388">*Index.cshtml*</span></span>
      * <span data-ttu-id="bb5d2-389">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-389">*Index.cshtml.cs*</span></span>
* <span data-ttu-id="bb5d2-390">**Sayfa /**</span><span class="sxs-lookup"><span data-stu-id="bb5d2-390">**Pages/**</span></span>
  * <span data-ttu-id="bb5d2-391">**Paylaşılan /**</span><span class="sxs-lookup"><span data-stu-id="bb5d2-391">**Shared/**</span></span>
    * <span data-ttu-id="bb5d2-392">*_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-392">*_Layout.cshtml*</span></span>
  * <span data-ttu-id="bb5d2-393">*_Viewımports.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-393">*_ViewImports.cshtml*</span></span>
  * <span data-ttu-id="bb5d2-394">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-394">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="bb5d2-395">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-395">*Index.cshtml*</span></span>
  * <span data-ttu-id="bb5d2-396">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-396">*Index.cshtml.cs*</span></span>

<span data-ttu-id="bb5d2-397">Projenin oluşturulmasında *hata ayıklama* yapılandırma aşağıdaki verir *obj* dizini:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-397">Building the project in *Debug* configuration yields the following *obj* directory:</span></span>

* <span data-ttu-id="bb5d2-398">**obj /**</span><span class="sxs-lookup"><span data-stu-id="bb5d2-398">**obj/**</span></span>
  * <span data-ttu-id="bb5d2-399">**Hata ayıklama /**</span><span class="sxs-lookup"><span data-stu-id="bb5d2-399">**Debug/**</span></span>
    * <span data-ttu-id="bb5d2-400">**netcoreapp2.1 /**</span><span class="sxs-lookup"><span data-stu-id="bb5d2-400">**netcoreapp2.1/**</span></span>
      * <span data-ttu-id="bb5d2-401">**Razor /**</span><span class="sxs-lookup"><span data-stu-id="bb5d2-401">**Razor/**</span></span>
        * <span data-ttu-id="bb5d2-402">**Alanlar /**</span><span class="sxs-lookup"><span data-stu-id="bb5d2-402">**Areas/**</span></span>
          * <span data-ttu-id="bb5d2-403">**Yönetim /**</span><span class="sxs-lookup"><span data-stu-id="bb5d2-403">**Admin/**</span></span>
            * <span data-ttu-id="bb5d2-404">**Sayfa /**</span><span class="sxs-lookup"><span data-stu-id="bb5d2-404">**Pages/**</span></span>
              * <span data-ttu-id="bb5d2-405">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-405">*Index.g.cshtml.cs*</span></span>
        * <span data-ttu-id="bb5d2-406">**Sayfa /**</span><span class="sxs-lookup"><span data-stu-id="bb5d2-406">**Pages/**</span></span>
          * <span data-ttu-id="bb5d2-407">**Paylaşılan /**</span><span class="sxs-lookup"><span data-stu-id="bb5d2-407">**Shared/**</span></span>
            * <span data-ttu-id="bb5d2-408">*_Layout.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-408">*_Layout.g.cshtml.cs*</span></span>
          * <span data-ttu-id="bb5d2-409">*_ViewImports.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-409">*_ViewImports.g.cshtml.cs*</span></span>
          * <span data-ttu-id="bb5d2-410">*_ViewStart.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-410">*_ViewStart.g.cshtml.cs*</span></span>
          * <span data-ttu-id="bb5d2-411">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="bb5d2-411">*Index.g.cshtml.cs*</span></span>

<span data-ttu-id="bb5d2-412">Oluşturulan sınıf için görüntülenecek *Pages/Index.cshtml*açın *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-412">To view the generated class for *Pages/Index.cshtml*, open *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="bb5d2-413">Aşağıdaki sınıf, ASP.NET Core MVC projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-413">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="bb5d2-414">İçinde `Startup.ConfigureServices`, geçersiz kılma `RazorTemplateEngine` ile MVC tarafından eklenen `CustomTemplateEngine` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-414">In `Startup.ConfigureServices`, override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="bb5d2-415">Bir kesme noktası ayarlamak `return csharpDocument;` deyiminin `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-415">Set a breakpoint on the `return csharpDocument;` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="bb5d2-416">Program yürütme kesme noktasında durduğunda değerini görüntülemek `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-416">When program execution stops at the breakpoint, view the value of `generatedCode`.</span></span>

![Metin Görselleştirici generatedCode görünümünü](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="bb5d2-418">Görünüm aramaları ve büyük/küçük harfe duyarlılık</span><span class="sxs-lookup"><span data-stu-id="bb5d2-418">View lookups and case sensitivity</span></span>

<span data-ttu-id="bb5d2-419">Razor görüntüleme motorunu büyük küçük harfe duyarlı aramalar, görünümler için gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-419">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="bb5d2-420">Ancak, gerçek arama, temel alınan dosya sistemi tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-420">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="bb5d2-421">Dosya tabanlı kaynağı:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-421">File based source:</span></span>
  * <span data-ttu-id="bb5d2-422">Büyük küçük harfe duyarlı dosya sistemleri (örneğin, Windows) ile işletim sistemlerinde, fiziksel dosya sağlayıcısı aramaları büyük küçük harfe duyarlı.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-422">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="bb5d2-423">Örneğin, `return View("Test")` eşleşmelerini sonuçlanıyor */Views/Home/Test.cshtml*, */Views/home/test.cshtml*ve diğer büyük/küçük harf değişken.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-423">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="bb5d2-424">Büyük küçük harfe duyarlı dosya sistemlerindeki (örneğin, Linux, OSX ile `EmbeddedFileProvider`), büyük küçük harfe duyarlı aramalar.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-424">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="bb5d2-425">Örneğin, `return View("Test")` özellikle eşleşen */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-425">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="bb5d2-426">Görünümleri önceden derlenmiş: ASP.NET Core 2.0 ve sonraki sürümleri, önceden derlenmiş görünümleri arama büyük/küçük harf tüm işletim sistemlerinde büyük harflere duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-426">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="bb5d2-427">Davranış Windows fiziksel dosya Sağlayıcısı'nın davranış aynıdır.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-427">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="bb5d2-428">Önceden derlenmiş iki görünüm yalnızca durumda farklıysa, arama sonucu belirleyici değildir.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-428">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="bb5d2-429">Geliştiriciler, dosya ve dizin adlarını büyük küçük harfleri büyük/küçük harf eşleşmesi için önerilir:</span><span class="sxs-lookup"><span data-stu-id="bb5d2-429">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

* <span data-ttu-id="bb5d2-430">Alan, denetleyici ve eylem adları.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-430">Area, controller, and action names.</span></span>
* <span data-ttu-id="bb5d2-431">Razor sayfaları.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-431">Razor Pages.</span></span>

<span data-ttu-id="bb5d2-432">Eşleşen servis talebi, temel alınan dosya sisteminden bağımsız olarak kendi görünümler dağıtımları Bul sağlar.</span><span class="sxs-lookup"><span data-stu-id="bb5d2-432">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
