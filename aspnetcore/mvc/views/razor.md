---
title: ASP.NET Core Razor söz dizimi başvurusu
author: rick-anderson
description: Kodu sunucu tabanlı Web sayfalarını eklemek için Razor söz dizimi biçimlendirme hakkında bilgi edinin.
ms.author: riande
ms.date: 10/26/2018
uid: mvc/views/razor
ms.openlocfilehash: ab9fb3f55399764c5fe985811d92c504ed210767
ms.sourcegitcommit: ad28d1bc6657a743d5c2fa8902f82740689733bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52256586"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a><span data-ttu-id="c0c99-103">ASP.NET Core Razor söz dizimi başvurusu</span><span class="sxs-lookup"><span data-stu-id="c0c99-103">Razor syntax reference for ASP.NET Core</span></span>

<span data-ttu-id="c0c99-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), ve [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="c0c99-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="c0c99-105">Razor kod sunucu tabanlı Web sayfalarını eklemek için bir biçimlendirme sözdizimi aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="c0c99-106">Razor işaretlemesi Razor sözdizimini oluşur C#ve HTML.</span><span class="sxs-lookup"><span data-stu-id="c0c99-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="c0c99-107">Razor genellikle içeren dosyalarınız bir *.cshtml* dosya uzantısı.</span><span class="sxs-lookup"><span data-stu-id="c0c99-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="c0c99-108">HTML işleme</span><span class="sxs-lookup"><span data-stu-id="c0c99-108">Rendering HTML</span></span>

<span data-ttu-id="c0c99-109">HTML varsayılan Razor dilidir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-109">The default Razor language is HTML.</span></span> <span data-ttu-id="c0c99-110">HTML Razor işaretlemesi işleme bir HTML dosyasından HTML'yi işlemeye değerinden farklı değildir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="c0c99-111">HTML biçimlendirmede *.cshtml* Razor dosyaları değiştirilmemiş sunucu tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="c0c99-112">Razor sözdizimi</span><span class="sxs-lookup"><span data-stu-id="c0c99-112">Razor syntax</span></span>

<span data-ttu-id="c0c99-113">Razor destekler C# ve kullandığı `@` HTML geçiş sembolünden C#.</span><span class="sxs-lookup"><span data-stu-id="c0c99-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="c0c99-114">Razor değerlendirir C# ifadeleri ve bunları HTML çıktısında oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c0c99-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="c0c99-115">Olduğunda bir `@` sembol tarafından izlenen bir [Razor ayrılmış anahtar sözcüğü](#razor-reserved-keywords), Razor özgü biçimlendirme geçer.</span><span class="sxs-lookup"><span data-stu-id="c0c99-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="c0c99-116">Aksi takdirde düz geçiş C#.</span><span class="sxs-lookup"><span data-stu-id="c0c99-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="c0c99-117">Kaçış için bir `@` sembol Razor işaretlemede, ikinci bir kullanın `@` sembol:</span><span class="sxs-lookup"><span data-stu-id="c0c99-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="c0c99-118">Kodunu HTML ile tek bir işlenen `@` sembol:</span><span class="sxs-lookup"><span data-stu-id="c0c99-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="c0c99-119">Olmayan HTML öznitelikleri ve e-posta adreslerini içeren bir içerik işleme `@` geçiş karakter olarak sembol.</span><span class="sxs-lookup"><span data-stu-id="c0c99-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="c0c99-120">Aşağıdaki örnekte e-posta adresleri tarafından Razor ayrıştırma olduğu:</span><span class="sxs-lookup"><span data-stu-id="c0c99-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="c0c99-121">Örtük Razor ifadeleri</span><span class="sxs-lookup"><span data-stu-id="c0c99-121">Implicit Razor expressions</span></span>

<span data-ttu-id="c0c99-122">Örtük Razor ifadeleri ile başlayıp `@` ardından C# kod:</span><span class="sxs-lookup"><span data-stu-id="c0c99-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="c0c99-123">Dışında C# `await` anahtar sözcüğü, örtük ifadeleri boşluk içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="c0c99-124">Varsa C# deyimi açık bir bitiş sahipse, boşluk intermingled:</span><span class="sxs-lookup"><span data-stu-id="c0c99-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="c0c99-125">Örtük ifade **olamaz** içeren C# köşeli ayraçlar içindeki karakterler olarak genel türler (`<>`) bir HTML etiketi yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="c0c99-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="c0c99-126">Aşağıdaki kod **değil** geçerli:</span><span class="sxs-lookup"><span data-stu-id="c0c99-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="c0c99-127">Yukarıdaki kod, aşağıdakilerden birini benzer bir derleyici hatası oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c0c99-127">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="c0c99-128">"İnt" öğesi kapalı değildi.</span><span class="sxs-lookup"><span data-stu-id="c0c99-128">The "int" element wasn't closed.</span></span> <span data-ttu-id="c0c99-129">Tüm öğeleri olmalıdır kendi kendine kapanan veya eşleşen bir bitiş etiketi sahip.</span><span class="sxs-lookup"><span data-stu-id="c0c99-129">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="c0c99-130">Yöntem grubu 'object' türü temsilci GenericMethod' dönüştürülemiyor.</span><span class="sxs-lookup"><span data-stu-id="c0c99-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="c0c99-131">Bir yöntemi çağırmak mı istiyordunuz?'</span><span class="sxs-lookup"><span data-stu-id="c0c99-131">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="c0c99-132">Genel yöntem çağrılarını sarmalanmış, içinde bir [Razor açık ifadesi](#explicit-razor-expressions) veya [Razor kodu bloğu](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="c0c99-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="c0c99-133">Açık Razor ifadeleri</span><span class="sxs-lookup"><span data-stu-id="c0c99-133">Explicit Razor expressions</span></span>

<span data-ttu-id="c0c99-134">Açık Razor ifadeleri oluşur bir `@` dengeli parantez ile simge.</span><span class="sxs-lookup"><span data-stu-id="c0c99-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="c0c99-135">Geçen haftaki zaman işlemek için aşağıdaki Razor işaretlemesi kullanılır:</span><span class="sxs-lookup"><span data-stu-id="c0c99-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="c0c99-136">İçinde herhangi bir içerik `@()` parantez değerlendirilir ve işlenen çıkışı.</span><span class="sxs-lookup"><span data-stu-id="c0c99-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="c0c99-137">Önceki bölümde açıklanan, örtük ifadeleri genellikle boşluk içeremez.</span><span class="sxs-lookup"><span data-stu-id="c0c99-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="c0c99-138">Aşağıdaki kodda, bir hafta, geçerli saatten çıkarılabilir değil:</span><span class="sxs-lookup"><span data-stu-id="c0c99-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="c0c99-139">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c0c99-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="c0c99-140">Açık ifadeler, metin bir ifadenin sonucu ile birleştirmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="c0c99-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="c0c99-141">Açık bir ifade olmadan `<p>Age@joe.Age</p>` bir e-posta adresi kabul edilir ve `<p>Age@joe.Age</p>` işlenir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="c0c99-142">Açık bir ifade olarak yazılırken `<p>Age33</p>` işlenir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<span data-ttu-id="c0c99-143">Açık ifadeleri, genel yöntemleri gelen çıkış işleme için kullanılabilir *.cshtml* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="c0c99-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="c0c99-144">Aşağıdaki biçimlendirmede gösterilen hatayı düzeltmek gösterilmektedir köşeli parantez önceki neden bir C# genel.</span><span class="sxs-lookup"><span data-stu-id="c0c99-144">The following markup shows how to correct the error shown earlier caused by the brackets of a C# generic.</span></span> <span data-ttu-id="c0c99-145">Kodu açık bir ifade olarak yazılır:</span><span class="sxs-lookup"><span data-stu-id="c0c99-145">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="c0c99-146">İfade kodlama</span><span class="sxs-lookup"><span data-stu-id="c0c99-146">Expression encoding</span></span>

<span data-ttu-id="c0c99-147">C#HTML kodlu bir dizeye değerlendirilen ifadeleri var.</span><span class="sxs-lookup"><span data-stu-id="c0c99-147">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="c0c99-148">C#değerlendirmek için ifadeleri `IHtmlContent` doğrudan aracılığıyla işlenir `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="c0c99-148">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="c0c99-149">C#Değerlendirme yok ifadeleri `IHtmlContent` tarafından bir dizeye dönüştürülür `ToString` ve bunlar işlenen önce kodlanır.</span><span class="sxs-lookup"><span data-stu-id="c0c99-149">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="c0c99-150">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c0c99-150">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="c0c99-151">HTML tarayıcı gösterilir:</span><span class="sxs-lookup"><span data-stu-id="c0c99-151">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="c0c99-152">`HtmlHelper.Raw` Çıktı kodlanmış değildir, ancak geri HTML biçimlendirmesi olarak çizilir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-152">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="c0c99-153">Kullanarak `HtmlHelper.Raw` unsanitized kullanıcı girişi bir güvenlik riski oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c0c99-153">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="c0c99-154">Kullanıcı girişi, kötü amaçlı JavaScript veya diğer saldırılara içerebilir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-154">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="c0c99-155">Kullanıcı girişi temizlenirken zordur.</span><span class="sxs-lookup"><span data-stu-id="c0c99-155">Sanitizing user input is difficult.</span></span> <span data-ttu-id="c0c99-156">Kullanmaktan kaçının `HtmlHelper.Raw` kullanıcı girişi ile.</span><span class="sxs-lookup"><span data-stu-id="c0c99-156">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="c0c99-157">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c0c99-157">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="c0c99-158">Razor kodu bloğu</span><span class="sxs-lookup"><span data-stu-id="c0c99-158">Razor code blocks</span></span>

<span data-ttu-id="c0c99-159">Razor kod blokları kullanmaya başlama `@` ve tarafından alınmış `{}`.</span><span class="sxs-lookup"><span data-stu-id="c0c99-159">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="c0c99-160">İfadeler, aksine C# olmayan kod blokları içinde kod çizilir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-160">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="c0c99-161">Kod blokları ve ifadeler bir görünümdeki aynı kapsamı paylaşan ve sırayla tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="c0c99-161">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="c0c99-162">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c0c99-162">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="c0c99-163">Örtük geçişleri</span><span class="sxs-lookup"><span data-stu-id="c0c99-163">Implicit transitions</span></span>

<span data-ttu-id="c0c99-164">Bir kod bloğu varsayılan dilde C#, ancak Razor sayfası HTML olarak geçiş yapabilir:</span><span class="sxs-lookup"><span data-stu-id="c0c99-164">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="c0c99-165">Açık ayrılmış geçiş</span><span class="sxs-lookup"><span data-stu-id="c0c99-165">Explicit delimited transition</span></span>

<span data-ttu-id="c0c99-166">HTML oluşturması gerektiğini bir kod bloğu alt tanımlamak için karakter işleme için Razor ile çevreleyen  **\<metin >** etiketi:</span><span class="sxs-lookup"><span data-stu-id="c0c99-166">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="c0c99-167">Tarafından HTML etiketleri arasına olmayan HTML oluşturmak için bu yaklaşımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="c0c99-167">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="c0c99-168">Bir HTML veya Razor etiket olmadan, bir Razor çalışma zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="c0c99-168">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="c0c99-169"> *\*\<Metin >** etiketi, boşluk içeriği işlenirken denetlemek kullanışlıdır:</span><span class="sxs-lookup"><span data-stu-id="c0c99-169">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="c0c99-170">Yalnızca arasında içerik  **\<metin >** etiketi işlenir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-170">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="c0c99-171">Hiçbir boşluk önce veya sonra  **\<metin >** etiketi HTML çıkışında görünür.</span><span class="sxs-lookup"><span data-stu-id="c0c99-171">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="c0c99-172">Açık satır geçişle @:</span><span class="sxs-lookup"><span data-stu-id="c0c99-172">Explicit Line Transition with @:</span></span>

<span data-ttu-id="c0c99-173">Tüm bir satırı geri kalanı bir kod bloğunun içine HTML olarak işlemek için kullanmak `@:` söz dizimi:</span><span class="sxs-lookup"><span data-stu-id="c0c99-173">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="c0c99-174">Olmadan `@:` kodda bir Razor çalışma zamanı hatası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c0c99-174">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="c0c99-175">Uyarı: Ek `@` Razor dosyadaki karakterler deyimleri derleyici hatalarını blok içindeki neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-175">Warning: Extra `@` characters in a Razor file can cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="c0c99-176">Bu derleyici hataları önce bildirilen hatayı gerçek bir hata oluştuğu için anlamak zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-176">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="c0c99-177">Bu hata, tek bir kod bloğunun birden çok örtük/açık ifadelere birleştirdikten sonra yaygındır.</span><span class="sxs-lookup"><span data-stu-id="c0c99-177">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="c0c99-178">Denetim yapıları</span><span class="sxs-lookup"><span data-stu-id="c0c99-178">Control structures</span></span>

<span data-ttu-id="c0c99-179">Denetim yapıları kod bloğu bir uzantı var.</span><span class="sxs-lookup"><span data-stu-id="c0c99-179">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="c0c99-180">Kod blokları tüm yönlerini (biçimlendirme, satır içi geçiş C#) aşağıdaki yapılar için de geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="c0c99-180">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="c0c99-181">Koşullular @if, else if, #else ve @switch</span><span class="sxs-lookup"><span data-stu-id="c0c99-181">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="c0c99-182">`@if` kod çalıştığında denetimleri:</span><span class="sxs-lookup"><span data-stu-id="c0c99-182">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="c0c99-183">`else` ve `else if` gerektirmeyen `@` sembol:</span><span class="sxs-lookup"><span data-stu-id="c0c99-183">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="c0c99-184">Aşağıdaki biçimlendirme switch deyimi kullanma işlemini gösterir:</span><span class="sxs-lookup"><span data-stu-id="c0c99-184">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="c0c99-185">Döngü @for, @foreach, @while, ve @do sırada</span><span class="sxs-lookup"><span data-stu-id="c0c99-185">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="c0c99-186">Şablonlu HTML denetim ifadeleri döngü ile oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-186">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="c0c99-187">Kişi listesini oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="c0c99-187">To render a list of people:</span></span>

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

<span data-ttu-id="c0c99-188">Aşağıdaki döngü deyimi desteklenir:</span><span class="sxs-lookup"><span data-stu-id="c0c99-188">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="c0c99-189">Bileşik @using</span><span class="sxs-lookup"><span data-stu-id="c0c99-189">Compound @using</span></span>

<span data-ttu-id="c0c99-190">İçinde C#, `using` deyimi, bir nesne kullanıldığında emin olmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c0c99-190">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="c0c99-191">Razor aynı mekanizmayı ek içeriklere sahip bir HTML Yardımcıları oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c0c99-191">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="c0c99-192">Aşağıdaki kodda, bir form etiketi ile HTML Yardımcıları oluşturma `@using` deyimi:</span><span class="sxs-lookup"><span data-stu-id="c0c99-192">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


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

<span data-ttu-id="c0c99-193">Kapsam düzeyinde Eylemler ile yapılabilir [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c0c99-193">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="c0c99-194">@try, son olarak, catch</span><span class="sxs-lookup"><span data-stu-id="c0c99-194">@try, catch, finally</span></span>

<span data-ttu-id="c0c99-195">Özel durum işleme benzer C#:</span><span class="sxs-lookup"><span data-stu-id="c0c99-195">Exception handling is similar to C#:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="c0c99-196">Razor kilit deyimleri kritik bölümlerle korumanın yeteneğine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="c0c99-196">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="c0c99-197">Açıklamalar</span><span class="sxs-lookup"><span data-stu-id="c0c99-197">Comments</span></span>

<span data-ttu-id="c0c99-198">Razor destekler C# ve HTML yorumlarında:</span><span class="sxs-lookup"><span data-stu-id="c0c99-198">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="c0c99-199">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c0c99-199">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="c0c99-200">Web sayfası işlenmeden önce razor açıklama sunucu tarafından kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="c0c99-200">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="c0c99-201">Razor kullanan `@*  *@` açıklamaları sınırlandırmak için.</span><span class="sxs-lookup"><span data-stu-id="c0c99-201">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="c0c99-202">Sunucu tüm biçimlendirme işlemesi yapmıyor için aşağıdaki kodu, geçersiz kılınan:</span><span class="sxs-lookup"><span data-stu-id="c0c99-202">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="c0c99-203">Yönergeler</span><span class="sxs-lookup"><span data-stu-id="c0c99-203">Directives</span></span>

<span data-ttu-id="c0c99-204">Razor yönergesi örtük ayrılmış anahtar sözcükler aşağıdaki deyimlerle tarafından temsil edilir `@` simgesi.</span><span class="sxs-lookup"><span data-stu-id="c0c99-204">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="c0c99-205">Bir yönergesi, genellikle bir görünüm ayrıştırılır veya farklı bir işlevsellik sağlar şeklini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-205">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="c0c99-206">Razor kod görünümü nasıl oluşturur? anlama yönergeleri nasıl çalıştığını anlamak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="c0c99-206">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="c0c99-207">Kod aşağıdaki gibi bir sınıf oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c0c99-207">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="c0c99-208">Bölümü bu makalenin ilerleyen bölümlerinde [Razor İnceleme C# bir görünümü için oluşturulan sınıf](#inspect-the-razor-c-class-generated-for-a-view) bu oluşturulan sınıf görüntülemek açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c0c99-208">Later in this article, the section [Inspect the Razor C# class generated for a view](#inspect-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

<a name="using"></a>
### <a name="using"></a>@using

<span data-ttu-id="c0c99-209">`@using` Yönergesi ekler C# `using` yönerge oluşturulmuş görünümü için:</span><span class="sxs-lookup"><span data-stu-id="c0c99-209">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="c0c99-210">`@model` Yönergesi bir görünüme iletilen model türünü belirtir:</span><span class="sxs-lookup"><span data-stu-id="c0c99-210">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="c0c99-211">Bireysel kullanıcı hesapları ile oluşturulmuş bir ASP.NET Core MVC uygulamasında *Views/Account/Login.cshtml* görünümü aşağıdaki model bildirimi içerir:</span><span class="sxs-lookup"><span data-stu-id="c0c99-211">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="c0c99-212">Oluşturulan sınıf devraldığı `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="c0c99-212">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="c0c99-213">Razor sunan bir `Model` modeli erişmek için özelliği geçirilen görünümü:</span><span class="sxs-lookup"><span data-stu-id="c0c99-213">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="c0c99-214">`@model` Yönergesi, bu özelliğin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-214">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="c0c99-215">Yönergesi belirtir `T` içinde `RazorPage<T>` türetildiği görünümü oluşturulan, sınıfın.</span><span class="sxs-lookup"><span data-stu-id="c0c99-215">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="c0c99-216">Varsa `@model` yönergesi belirtilmediyse, `Model` özelliği türüdür `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="c0c99-216">If the `@model` directive isn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="c0c99-217">Model değeri denetleyicisinden görünüme iletilir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-217">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="c0c99-218">Daha fazla bilgi için [modelleri türü kesin ve &commat;model anahtar sözcük](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span><span class="sxs-lookup"><span data-stu-id="c0c99-218">For more information, see [Strongly typed models and the &commat;model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="c0c99-219">`@inherits` Yönergesi görünümü devralan sınıf tam denetim sağlar:</span><span class="sxs-lookup"><span data-stu-id="c0c99-219">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="c0c99-220">Aşağıdaki kod bir özel bir Razor sayfası türüdür:</span><span class="sxs-lookup"><span data-stu-id="c0c99-220">The following code is a custom Razor page type:</span></span>

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="c0c99-221">`CustomText` Bir görünümde görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="c0c99-221">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="c0c99-222">Kod aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c0c99-222">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="c0c99-223">`@model` ve `@inherits` aynı görünümde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-223">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="c0c99-224">`@inherits` kullanılabilir bir *_viewımports.cshtml* görünümü alır dosyası:</span><span class="sxs-lookup"><span data-stu-id="c0c99-224">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="c0c99-225">Aşağıdaki kod, kesin türü belirtilmiş görünüm örneğidir:</span><span class="sxs-lookup"><span data-stu-id="c0c99-225">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="c0c99-226">Varsa "rick@contoso.com" geçirilen modeldeki görünümünü aşağıdaki HTML biçimlendirmeyi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c0c99-226">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

<span data-ttu-id="c0c99-227">`@inject` Yönergesi sağlayan bir hizmet eklemesine Razor sayfası [hizmet kapsayıcı](xref:fundamentals/dependency-injection) bir görünümde.</span><span class="sxs-lookup"><span data-stu-id="c0c99-227">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="c0c99-228">Daha fazla bilgi için [görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c0c99-228">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="c0c99-229">`@functions` Yönergesi eklemek bir Razor sayfası sağlayan bir C# kod bloğu bir görünüm için:</span><span class="sxs-lookup"><span data-stu-id="c0c99-229">The `@functions` directive enables a Razor Page to add a C# code block to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="c0c99-230">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="c0c99-230">For example:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="c0c99-231">Kod aşağıdaki HTML biçimlendirmeyi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c0c99-231">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="c0c99-232">Aşağıdaki kodu oluşturulmuş Razor olan C# sınıfı:</span><span class="sxs-lookup"><span data-stu-id="c0c99-232">The following code is the generated Razor C# class:</span></span>

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="c0c99-233">`@section` Yönergesi ile birlikte kullanılan [Düzen](xref:mvc/views/layout) içeriği HTML sayfasının farklı bölümlerini işlemek görünümlerini etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="c0c99-233">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="c0c99-234">Daha fazla bilgi için [bölümleri](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="c0c99-234">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="templated-razor-delegates"></a><span data-ttu-id="c0c99-235">Şablonlu Razor temsilciler</span><span class="sxs-lookup"><span data-stu-id="c0c99-235">Templated Razor delegates</span></span>

<span data-ttu-id="c0c99-236">Razor şablonları aşağıdaki biçimde bir kullanıcı Arabirimi parçacığı tanımlamanıza izin ver:</span><span class="sxs-lookup"><span data-stu-id="c0c99-236">Razor templates allow you to define a UI snippet with the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="c0c99-237">Aşağıdaki örnekte, şablonlu Razor temsilci olarak belirtmek verilmektedir bir <xref:System.Func`2>.</span><span class="sxs-lookup"><span data-stu-id="c0c99-237">The following example illustrates how to specify a templated Razor delegate as a <xref:System.Func`2>.</span></span> <span data-ttu-id="c0c99-238">[Dinamik tür](/dotnet/csharp/programming-guide/types/using-type-dynamic) temsilci kapsülleyen yönteminin parametresi için belirtilir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-238">The [dynamic type](/dotnet/csharp/programming-guide/types/using-type-dynamic) is specified for the parameter of the method that the delegate encapsulates.</span></span> <span data-ttu-id="c0c99-239">Bir [nesne türü](/dotnet/csharp/language-reference/keywords/object) temsilcinin dönüş değeri olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-239">An [object type](/dotnet/csharp/language-reference/keywords/object) is specified as the return value of the delegate.</span></span> <span data-ttu-id="c0c99-240">Şablon ile kullanılan bir <xref:System.Collections.Generic.List`1> , `Pet` olan bir `Name` özelliği.</span><span class="sxs-lookup"><span data-stu-id="c0c99-240">The template is used with a <xref:System.Collections.Generic.List`1> of `Pet` that has a `Name` property.</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }
}
```

```cshtml
@{
    Func<dynamic, object> petTemplate = @<p>You have a pet named @item.Name.</p>;

    var pets = new List<Pet>
    {
        new Pet { Name = "Rin Tin Tin" },
        new Pet { Name = "Mr. Bigglesworth" },
        new Pet { Name = "K-9" }
    };
}
```

<span data-ttu-id="c0c99-241">Şablon ile işlenen `pets` tarafından sağlanan bir `foreach` deyimi:</span><span class="sxs-lookup"><span data-stu-id="c0c99-241">The template is rendered with `pets` supplied by a `foreach` statement:</span></span>

```cshtml
@foreach (var pet in pets)
{
    @petTemplate2(pet)
}
```

<span data-ttu-id="c0c99-242">İşlenmiş çıkışı:</span><span class="sxs-lookup"><span data-stu-id="c0c99-242">Rendered output:</span></span>

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

<span data-ttu-id="c0c99-243">Bir yöntem bağımsız değişkeni olarak bir satır içi Razor şablonu da sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c0c99-243">You can also supply an inline Razor template as an argument to a method.</span></span> <span data-ttu-id="c0c99-244">Aşağıdaki örnekte, `Repeat` yöntem Razor şablonu alır.</span><span class="sxs-lookup"><span data-stu-id="c0c99-244">In the following example, the `Repeat` method receives a Razor template.</span></span> <span data-ttu-id="c0c99-245">Yöntemi, HTML içerik ile sağlanan bir listeden öğeleri yineler üretmek için şablonu kullanır:</span><span class="sxs-lookup"><span data-stu-id="c0c99-245">The method uses the template to produce HTML content with repeats of items supplied from a list:</span></span>

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

<span data-ttu-id="c0c99-246">Önceki örnekte, Evcil Hayvanlar listesi kullanarak `Repeat` yöntemi çağrıldığında:</span><span class="sxs-lookup"><span data-stu-id="c0c99-246">Using the list of pets from the prior example, the `Repeat` method is called with:</span></span>

* <span data-ttu-id="c0c99-247"><xref:System.Collections.Generic.List`1> ' ın `Pet`.</span><span class="sxs-lookup"><span data-stu-id="c0c99-247"><xref:System.Collections.Generic.List`1> of `Pet`.</span></span>
* <span data-ttu-id="c0c99-248">Her evcil hayvan yineleme sayısı.</span><span class="sxs-lookup"><span data-stu-id="c0c99-248">Number of times to repeat each pet.</span></span>
* <span data-ttu-id="c0c99-249">Satır içi şablon sırasız bir listesini liste öğeleri için kullanın.</span><span class="sxs-lookup"><span data-stu-id="c0c99-249">Inline template to use for the list items of an unordered list.</span></span>

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

<span data-ttu-id="c0c99-250">İşlenmiş çıkışı:</span><span class="sxs-lookup"><span data-stu-id="c0c99-250">Rendered output:</span></span>

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

## <a name="tag-helpers"></a><span data-ttu-id="c0c99-251">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="c0c99-251">Tag Helpers</span></span>

<span data-ttu-id="c0c99-252">İlgili üç yönergeleri vardır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c0c99-252">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="c0c99-253">Yönergesi</span><span class="sxs-lookup"><span data-stu-id="c0c99-253">Directive</span></span> | <span data-ttu-id="c0c99-254">İşlev</span><span class="sxs-lookup"><span data-stu-id="c0c99-254">Function</span></span> |
| --------- | -------- |
| [<span data-ttu-id="c0c99-255">&commat;addTagHelper</span><span class="sxs-lookup"><span data-stu-id="c0c99-255">&commat;addTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="c0c99-256">Etiket Yardımcıları bir görünüm için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-256">Makes Tag Helpers available to a view.</span></span> |
| [<span data-ttu-id="c0c99-257">&commat;removeTagHelper</span><span class="sxs-lookup"><span data-stu-id="c0c99-257">&commat;removeTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="c0c99-258">Daha önce bir görünümden eklenen etiket Yardımcıları kaldırır.</span><span class="sxs-lookup"><span data-stu-id="c0c99-258">Removes Tag Helpers previously added from a view.</span></span> |
| [<span data-ttu-id="c0c99-259">&commat;tagHelperPrefix</span><span class="sxs-lookup"><span data-stu-id="c0c99-259">&commat;tagHelperPrefix</span></span>](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="c0c99-260">Etiket Yardımcısı desteğinin etkinleştirmek ve etiket Yardımcısı kullanım açık hale getirmek için bir etiket öneki belirtir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-260">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="c0c99-261">Razor ayrılmış anahtar sözcükler</span><span class="sxs-lookup"><span data-stu-id="c0c99-261">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="c0c99-262">Razor anahtar sözcükleri</span><span class="sxs-lookup"><span data-stu-id="c0c99-262">Razor keywords</span></span>

* <span data-ttu-id="c0c99-263">Sayfa (ASP.NET Core 2.0 ve sonraki sürümleri gerektirir)</span><span class="sxs-lookup"><span data-stu-id="c0c99-263">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="c0c99-264"> ad alanı</span><span class="sxs-lookup"><span data-stu-id="c0c99-264">namespace</span></span>
* <span data-ttu-id="c0c99-265"> işlevleri</span><span class="sxs-lookup"><span data-stu-id="c0c99-265">functions</span></span>
* <span data-ttu-id="c0c99-266">Devralan</span><span class="sxs-lookup"><span data-stu-id="c0c99-266">inherits</span></span>
* <span data-ttu-id="c0c99-267">model</span><span class="sxs-lookup"><span data-stu-id="c0c99-267">model</span></span>
* <span data-ttu-id="c0c99-268">section</span><span class="sxs-lookup"><span data-stu-id="c0c99-268">section</span></span>
* <span data-ttu-id="c0c99-269">yardımcı (şu anda ASP.NET Core tarafından desteklenmez)</span><span class="sxs-lookup"><span data-stu-id="c0c99-269">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="c0c99-270">Razor anahtar sözcükleri kaçış ile `@(Razor Keyword)` (örneğin, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="c0c99-270">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="c0c99-271">C#Razor anahtar sözcükleri</span><span class="sxs-lookup"><span data-stu-id="c0c99-271">C# Razor keywords</span></span>

* <span data-ttu-id="c0c99-272">büyük/küçük harf</span><span class="sxs-lookup"><span data-stu-id="c0c99-272">case</span></span>
* <span data-ttu-id="c0c99-273">do</span><span class="sxs-lookup"><span data-stu-id="c0c99-273">do</span></span>
* <span data-ttu-id="c0c99-274">default</span><span class="sxs-lookup"><span data-stu-id="c0c99-274">default</span></span>
* <span data-ttu-id="c0c99-275">for</span><span class="sxs-lookup"><span data-stu-id="c0c99-275">for</span></span>
* <span data-ttu-id="c0c99-276">foreach</span><span class="sxs-lookup"><span data-stu-id="c0c99-276">foreach</span></span>
* <span data-ttu-id="c0c99-277">if</span><span class="sxs-lookup"><span data-stu-id="c0c99-277">if</span></span>
* <span data-ttu-id="c0c99-278">else</span><span class="sxs-lookup"><span data-stu-id="c0c99-278">else</span></span>
* <span data-ttu-id="c0c99-279">lock</span><span class="sxs-lookup"><span data-stu-id="c0c99-279">lock</span></span>
* <span data-ttu-id="c0c99-280"> anahtarı</span><span class="sxs-lookup"><span data-stu-id="c0c99-280">switch</span></span>
* <span data-ttu-id="c0c99-281">deneyin</span><span class="sxs-lookup"><span data-stu-id="c0c99-281">try</span></span>
* <span data-ttu-id="c0c99-282">Yakalama</span><span class="sxs-lookup"><span data-stu-id="c0c99-282">catch</span></span>
* <span data-ttu-id="c0c99-283">finally</span><span class="sxs-lookup"><span data-stu-id="c0c99-283">finally</span></span>
* <span data-ttu-id="c0c99-284">kullanma</span><span class="sxs-lookup"><span data-stu-id="c0c99-284">using</span></span>
* <span data-ttu-id="c0c99-285">while</span><span class="sxs-lookup"><span data-stu-id="c0c99-285">while</span></span>

<span data-ttu-id="c0c99-286">C#Razor anahtar sözcükleri çift kaçış ile olmalıdır `@(@C# Razor Keyword)` (örneğin, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="c0c99-286">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="c0c99-287">İlk `@` Razor ayrıştırıcısı atlar.</span><span class="sxs-lookup"><span data-stu-id="c0c99-287">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="c0c99-288">İkinci `@` çıkışları C# ayrıştırıcı.</span><span class="sxs-lookup"><span data-stu-id="c0c99-288">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="c0c99-289">Razor tarafından kullanılmayan ayrılmış anahtar sözcükler</span><span class="sxs-lookup"><span data-stu-id="c0c99-289">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="c0c99-290">sınıf</span><span class="sxs-lookup"><span data-stu-id="c0c99-290">class</span></span>

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="c0c99-291">Razor İnceleme C# bir görünümü için oluşturulan sınıfı</span><span class="sxs-lookup"><span data-stu-id="c0c99-291">Inspect the Razor C# class generated for a view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c0c99-292">.NET Core SDK'sını 2.1 veya daha sonra [Razor SDK](xref:razor-pages/sdk) derleme Razor dosyaları işler.</span><span class="sxs-lookup"><span data-stu-id="c0c99-292">With .NET Core SDK 2.1 or later, the [Razor SDK](xref:razor-pages/sdk) handles compilation of Razor files.</span></span> <span data-ttu-id="c0c99-293">Bir proje derlenirken Razor SDK'sı oluşturur bir *obj / < build_configuration > / < target_framework_moniker > / Razor* proje kök dizini.</span><span class="sxs-lookup"><span data-stu-id="c0c99-293">When building a project, the Razor SDK generates an *obj/<build_configuration>/<target_framework_moniker>/Razor* directory in the project root.</span></span> <span data-ttu-id="c0c99-294">Dizin yapısında *Razor* dizini proje dizin yapısına yansıtır.</span><span class="sxs-lookup"><span data-stu-id="c0c99-294">The directory structure within the *Razor* directory mirrors the project's directory structure.</span></span>

<span data-ttu-id="c0c99-295">.NET Core 2.1 hedefleyen ASP.NET Core 2.1 Razor sayfaları projesinde aşağıdaki dizin yapısını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="c0c99-295">Consider the following directory structure in an ASP.NET Core 2.1 Razor Pages project targeting .NET Core 2.1:</span></span>

* <span data-ttu-id="c0c99-296">**Alanlar /**</span><span class="sxs-lookup"><span data-stu-id="c0c99-296">**Areas/**</span></span>
  * <span data-ttu-id="c0c99-297">**Yönetim /**</span><span class="sxs-lookup"><span data-stu-id="c0c99-297">**Admin/**</span></span>
    * <span data-ttu-id="c0c99-298">**Sayfa /**</span><span class="sxs-lookup"><span data-stu-id="c0c99-298">**Pages/**</span></span>
      * <span data-ttu-id="c0c99-299">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c0c99-299">*Index.cshtml*</span></span>
      * <span data-ttu-id="c0c99-300">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="c0c99-300">*Index.cshtml.cs*</span></span>
* <span data-ttu-id="c0c99-301">**Sayfa /**</span><span class="sxs-lookup"><span data-stu-id="c0c99-301">**Pages/**</span></span>
  * <span data-ttu-id="c0c99-302">**Paylaşılan /**</span><span class="sxs-lookup"><span data-stu-id="c0c99-302">**Shared/**</span></span>
    * <span data-ttu-id="c0c99-303">*_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c0c99-303">*_Layout.cshtml*</span></span>
  * <span data-ttu-id="c0c99-304">*_Viewımports.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c0c99-304">*_ViewImports.cshtml*</span></span>
  * <span data-ttu-id="c0c99-305">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c0c99-305">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="c0c99-306">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c0c99-306">*Index.cshtml*</span></span>
  * <span data-ttu-id="c0c99-307">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="c0c99-307">*Index.cshtml.cs*</span></span>

<span data-ttu-id="c0c99-308">Projenin oluşturulmasında *hata ayıklama* yapılandırma aşağıdaki verir *obj* dizini:</span><span class="sxs-lookup"><span data-stu-id="c0c99-308">Building the project in *Debug* configuration yields the following *obj* directory:</span></span>

* <span data-ttu-id="c0c99-309">**obj /**</span><span class="sxs-lookup"><span data-stu-id="c0c99-309">**obj/**</span></span>
  * <span data-ttu-id="c0c99-310">**Hata ayıklama /**</span><span class="sxs-lookup"><span data-stu-id="c0c99-310">**Debug/**</span></span>
    * <span data-ttu-id="c0c99-311">**netcoreapp2.1 /**</span><span class="sxs-lookup"><span data-stu-id="c0c99-311">**netcoreapp2.1/**</span></span>
      * <span data-ttu-id="c0c99-312">**Razor /**</span><span class="sxs-lookup"><span data-stu-id="c0c99-312">**Razor/**</span></span>
        * <span data-ttu-id="c0c99-313">**Alanlar /**</span><span class="sxs-lookup"><span data-stu-id="c0c99-313">**Areas/**</span></span>
          * <span data-ttu-id="c0c99-314">**Yönetim /**</span><span class="sxs-lookup"><span data-stu-id="c0c99-314">**Admin/**</span></span>
            * <span data-ttu-id="c0c99-315">**Sayfa /**</span><span class="sxs-lookup"><span data-stu-id="c0c99-315">**Pages/**</span></span>
              * <span data-ttu-id="c0c99-316">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="c0c99-316">*Index.g.cshtml.cs*</span></span>
        * <span data-ttu-id="c0c99-317">**Sayfa /**</span><span class="sxs-lookup"><span data-stu-id="c0c99-317">**Pages/**</span></span>
          * <span data-ttu-id="c0c99-318">**Paylaşılan /**</span><span class="sxs-lookup"><span data-stu-id="c0c99-318">**Shared/**</span></span>
            * <span data-ttu-id="c0c99-319">*_Layout.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="c0c99-319">*_Layout.g.cshtml.cs*</span></span>
          * <span data-ttu-id="c0c99-320">*_ViewImports.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="c0c99-320">*_ViewImports.g.cshtml.cs*</span></span>
          * <span data-ttu-id="c0c99-321">*_ViewStart.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="c0c99-321">*_ViewStart.g.cshtml.cs*</span></span>
          * <span data-ttu-id="c0c99-322">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="c0c99-322">*Index.g.cshtml.cs*</span></span>

<span data-ttu-id="c0c99-323">Oluşturulan sınıf için görüntülenecek *Pages/Index.cshtml*açın *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="c0c99-323">To view the generated class for *Pages/Index.cshtml*, open *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="c0c99-324">Aşağıdaki sınıf, ASP.NET Core MVC projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="c0c99-324">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="c0c99-325">İçinde `Startup.ConfigureServices`, geçersiz kılma `RazorTemplateEngine` ile MVC tarafından eklenen `CustomTemplateEngine` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="c0c99-325">In `Startup.ConfigureServices`, override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="c0c99-326">Bir kesme noktası ayarlamak `return csharpDocument;` deyiminin `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="c0c99-326">Set a breakpoint on the `return csharpDocument;` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="c0c99-327">Program yürütme kesme noktasında durduğunda değerini görüntülemek `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="c0c99-327">When program execution stops at the breakpoint, view the value of `generatedCode`.</span></span>

![Metin Görselleştirici generatedCode görünümünü](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="c0c99-329">Görünüm aramaları ve büyük/küçük harfe duyarlılık</span><span class="sxs-lookup"><span data-stu-id="c0c99-329">View lookups and case sensitivity</span></span>

<span data-ttu-id="c0c99-330">Razor görüntüleme motorunu büyük küçük harfe duyarlı aramalar, görünümler için gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-330">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="c0c99-331">Ancak, gerçek arama, temel alınan dosya sistemi tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="c0c99-331">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="c0c99-332">Dosya tabanlı kaynağı:</span><span class="sxs-lookup"><span data-stu-id="c0c99-332">File based source:</span></span>
  * <span data-ttu-id="c0c99-333">Büyük küçük harfe duyarlı dosya sistemleri (örneğin, Windows) ile işletim sistemlerinde, fiziksel dosya sağlayıcısı aramaları büyük küçük harfe duyarlı.</span><span class="sxs-lookup"><span data-stu-id="c0c99-333">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="c0c99-334">Örneğin, `return View("Test")` eşleşmelerini sonuçlanıyor */Views/Home/Test.cshtml*, */Views/home/test.cshtml*ve diğer büyük/küçük harf değişken.</span><span class="sxs-lookup"><span data-stu-id="c0c99-334">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="c0c99-335">Büyük küçük harfe duyarlı dosya sistemlerindeki (örneğin, Linux, OSX ile `EmbeddedFileProvider`), büyük küçük harfe duyarlı aramalar.</span><span class="sxs-lookup"><span data-stu-id="c0c99-335">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="c0c99-336">Örneğin, `return View("Test")` özellikle eşleşen */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c0c99-336">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="c0c99-337">Görünümleri önceden derlenmiş: ASP.NET Core 2.0 ve sonraki sürümleri, önceden derlenmiş görünümleri arama büyük/küçük harf tüm işletim sistemlerinde büyük harflere duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-337">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="c0c99-338">Davranış Windows fiziksel dosya Sağlayıcısı'nın davranış aynıdır.</span><span class="sxs-lookup"><span data-stu-id="c0c99-338">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="c0c99-339">Önceden derlenmiş iki görünüm yalnızca durumda farklıysa, arama sonucu belirleyici değildir.</span><span class="sxs-lookup"><span data-stu-id="c0c99-339">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="c0c99-340">Geliştiriciler, dosya ve dizin adlarını büyük küçük harfleri büyük/küçük harf eşleşmesi için önerilir:</span><span class="sxs-lookup"><span data-stu-id="c0c99-340">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="c0c99-341">Alan, denetleyici ve eylem adları.</span><span class="sxs-lookup"><span data-stu-id="c0c99-341">Area, controller, and action names.</span></span>
    * <span data-ttu-id="c0c99-342">Razor sayfaları.</span><span class="sxs-lookup"><span data-stu-id="c0c99-342">Razor Pages.</span></span>

<span data-ttu-id="c0c99-343">Eşleşen servis talebi, temel alınan dosya sisteminden bağımsız olarak kendi görünümler dağıtımları Bul sağlar.</span><span class="sxs-lookup"><span data-stu-id="c0c99-343">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
