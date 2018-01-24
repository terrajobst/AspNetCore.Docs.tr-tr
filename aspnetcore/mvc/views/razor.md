---
title: "ASP.NET Core için Razor söz dizimi başvurusu"
author: rick-anderson
description: "Web sayfalarının sunucu tabanlı kod katıştırma Razor biçimlendirme söz dizimi hakkında bilgi edinin."
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: d932e28246998c60e2b3f9c77a2521fe55991e85
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="cc000-103">ASP.NET Core için Razor sözdizimi</span><span class="sxs-lookup"><span data-stu-id="cc000-103">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="cc000-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), ve [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="cc000-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="cc000-105">Razor kod sunucu tabanlı Web sayfalarının katıştırma için işaretleme sözdizimi aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="cc000-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="cc000-106">Razor sözdizimi Razor biçimlendirme, C# ve HTML oluşur.</span><span class="sxs-lookup"><span data-stu-id="cc000-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="cc000-107">Razor genellikle içeren dosyalarınız bir *.cshtml* dosya uzantısı.</span><span class="sxs-lookup"><span data-stu-id="cc000-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="cc000-108">HTML işleme</span><span class="sxs-lookup"><span data-stu-id="cc000-108">Rendering HTML</span></span>

<span data-ttu-id="cc000-109">Varsayılan Razor HTML dilidir.</span><span class="sxs-lookup"><span data-stu-id="cc000-109">The default Razor language is HTML.</span></span> <span data-ttu-id="cc000-110">Razor biçimlendirme işleme HTML bir HTML dosyası HTML işleme daha farklı değildir.</span><span class="sxs-lookup"><span data-stu-id="cc000-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="cc000-111">HTML biçimlendirmede *.cshtml* Razor dosyaları değiştirmeden sunucu tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="cc000-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="cc000-112">Razor sözdizimi</span><span class="sxs-lookup"><span data-stu-id="cc000-112">Razor syntax</span></span>

<span data-ttu-id="cc000-113">Razor C# destekler ve kullandığı `@` C# HTML geçiş simgesini.</span><span class="sxs-lookup"><span data-stu-id="cc000-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="cc000-114">Razor C# ifadeleri değerlendirir ve HTML çıkışında işler.</span><span class="sxs-lookup"><span data-stu-id="cc000-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="cc000-115">Zaman bir `@` simgesi tarafından izlenen bir [Razor ayrılmış anahtar sözcüğü](#razor-reserved-keywords), Razor özgü biçimlendirme geçiş.</span><span class="sxs-lookup"><span data-stu-id="cc000-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="cc000-116">Aksi takdirde, düz C# diline geçiş.</span><span class="sxs-lookup"><span data-stu-id="cc000-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="cc000-117">Çıkış için bir `@` sembol Razor biçimlendirme içinde ikinci bir `@` simgesi:</span><span class="sxs-lookup"><span data-stu-id="cc000-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="cc000-118">Kod tek bir HTML olarak işlenen `@` simgesi:</span><span class="sxs-lookup"><span data-stu-id="cc000-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="cc000-119">HTML öznitelikleri ve e-posta adreslerini içeren içerik yok işler `@` geçiş karakter olarak simge.</span><span class="sxs-lookup"><span data-stu-id="cc000-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="cc000-120">Aşağıdaki örnekte e-posta adresleri tarafından Razor ayrıştırma aynı şekilde:</span><span class="sxs-lookup"><span data-stu-id="cc000-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="cc000-121">Örtük Razor ifadeleri</span><span class="sxs-lookup"><span data-stu-id="cc000-121">Implicit Razor expressions</span></span>

<span data-ttu-id="cc000-122">Örtük Razor ifadeleri başlayarak `@` C# kodu tarafından izlenen:</span><span class="sxs-lookup"><span data-stu-id="cc000-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="cc000-123">C# dışında `await` anahtar sözcüğü, örtük ifadeleri boşluk içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="cc000-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="cc000-124">C# deyimi Temizle bitiş varsa, alanları birbirine:</span><span class="sxs-lookup"><span data-stu-id="cc000-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="cc000-125">Örtük ifadeleri **olamaz** köşeli ayraçlar içindeki karakterlerden olarak C# genel türleri, içeren (`<>`) bir HTML etiketi olarak yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="cc000-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="cc000-126">Aşağıdaki kodu **değil** geçerli:</span><span class="sxs-lookup"><span data-stu-id="cc000-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="cc000-127">Önceki kod, aşağıdakilerden birini benzer bir derleyici hatası oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cc000-127">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="cc000-128">"İnt" öğesi kapatılmadı.</span><span class="sxs-lookup"><span data-stu-id="cc000-128">The "int" element was not closed.</span></span> <span data-ttu-id="cc000-129">Tüm öğeleri ya da olmalıdır otomatik olarak kapatma veya eşleşen bir bitiş etiketi vardır.</span><span class="sxs-lookup"><span data-stu-id="cc000-129">All elements must be either self-closing or have a matching end tag.</span></span>
 * <span data-ttu-id="cc000-130">Yöntem Grup 'object' türü temsilci için GenericMethod' dönüştürülemiyor.</span><span class="sxs-lookup"><span data-stu-id="cc000-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="cc000-131">Yöntemini çağırmak istiyordunuz?'</span><span class="sxs-lookup"><span data-stu-id="cc000-131">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="cc000-132">Genel yöntem çağrılarını Sarmalanan, içinde bir [açık Razor ifade](#explicit-razor-expressions) veya [Razor kod bloğunu](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="cc000-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="cc000-133">Açık Razor ifadeleri</span><span class="sxs-lookup"><span data-stu-id="cc000-133">Explicit Razor expressions</span></span>

<span data-ttu-id="cc000-134">Açık Razor ifadeleri oluşur bir `@` dengeli parantez simgesiyle.</span><span class="sxs-lookup"><span data-stu-id="cc000-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="cc000-135">Geçen haftaki zaman işlemek için aşağıdaki Razor biçimlendirme kullanılır:</span><span class="sxs-lookup"><span data-stu-id="cc000-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="cc000-136">İçinde herhangi bir içerik `@()` parantez değerlendirilir ve çıktıyı çizilir.</span><span class="sxs-lookup"><span data-stu-id="cc000-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="cc000-137">Örtük ifadeleri, önceki bölümde açıklanan genellikle boşluk içeremez.</span><span class="sxs-lookup"><span data-stu-id="cc000-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="cc000-138">Aşağıdaki kodda bir hafta geçerli saatten çıkarılır değil:</span><span class="sxs-lookup"><span data-stu-id="cc000-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="cc000-139">Aşağıdaki HTML kod işler:</span><span class="sxs-lookup"><span data-stu-id="cc000-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="cc000-140">Açık ifadeler bir ifade sonucu metinle birleştirmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="cc000-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="cc000-141">Açık ifade olmadan `<p>Age@joe.Age</p>` bir e-posta adresi olarak kabul edilir ve `<p>Age@joe.Age</p>` işlenir.</span><span class="sxs-lookup"><span data-stu-id="cc000-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="cc000-142">Açık bir ifade olarak yazılırken `<p>Age33</p>` işlenir.</span><span class="sxs-lookup"><span data-stu-id="cc000-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="cc000-143">Açık ifadeleri, genel yöntemleri çıktısını işlemek için kullanılabilir *.cshtml* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="cc000-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="cc000-144">Köşeli ayraçlar içindeki karakterlerden örtük bir ifadede (`<>`) bir HTML etiketi olarak yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="cc000-144">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="cc000-145">Aşağıdaki biçimlendirme **değil** geçerli Razor:</span><span class="sxs-lookup"><span data-stu-id="cc000-145">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="cc000-146">Önceki kod, aşağıdakilerden birini benzer bir derleyici hatası oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cc000-146">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="cc000-147">"İnt" öğesi kapatılmadı.</span><span class="sxs-lookup"><span data-stu-id="cc000-147">The "int" element was not closed.</span></span> <span data-ttu-id="cc000-148">Tüm öğeleri ya da olmalıdır otomatik olarak kapatma veya eşleşen bir bitiş etiketi vardır.</span><span class="sxs-lookup"><span data-stu-id="cc000-148">All elements must be either self-closing or have a matching end tag.</span></span>
 * <span data-ttu-id="cc000-149">Yöntem Grup 'object' türü temsilci için GenericMethod' dönüştürülemiyor.</span><span class="sxs-lookup"><span data-stu-id="cc000-149">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="cc000-150">Yöntemini çağırmak istiyordunuz?'</span><span class="sxs-lookup"><span data-stu-id="cc000-150">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="cc000-151">Aşağıdaki biçimlendirmede doğru bir şekilde yazma bu kodu gösterir.</span><span class="sxs-lookup"><span data-stu-id="cc000-151">The following markup shows the correct way write this code.</span></span> <span data-ttu-id="cc000-152">Kod açık bir ifade olarak yazılır:</span><span class="sxs-lookup"><span data-stu-id="cc000-152">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="cc000-153">Expression encoding</span><span class="sxs-lookup"><span data-stu-id="cc000-153">Expression encoding</span></span>

<span data-ttu-id="cc000-154">Bir dizeyi değerlendirmek C# HTML kodlu ifadelerini.</span><span class="sxs-lookup"><span data-stu-id="cc000-154">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="cc000-155">Değerlendirmek için C# ifadeleri `IHtmlContent` doğrudan ile işlenir `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="cc000-155">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="cc000-156">Değerlendirme yok C# ifadeleri `IHtmlContent` tarafından bir dizeye dönüştürülür `ToString` ve işlenen önce kodlanır.</span><span class="sxs-lookup"><span data-stu-id="cc000-156">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="cc000-157">Aşağıdaki HTML kod işler:</span><span class="sxs-lookup"><span data-stu-id="cc000-157">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="cc000-158">HTML tarayıcıda gösterilir:</span><span class="sxs-lookup"><span data-stu-id="cc000-158">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="cc000-159">`HtmlHelper.Raw`Çıktı kodlanmış değildir, ancak geri HTML biçimlendirmesi olarak çizilir.</span><span class="sxs-lookup"><span data-stu-id="cc000-159">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="cc000-160">Kullanarak `HtmlHelper.Raw` unsanitized kullanıcı girişi bir güvenlik riskidir.</span><span class="sxs-lookup"><span data-stu-id="cc000-160">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="cc000-161">Kullanıcı girişi kötü amaçlı JavaScript veya diğer açıkları içerebilir.</span><span class="sxs-lookup"><span data-stu-id="cc000-161">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="cc000-162">Kullanıcı girişi temizleme zordur.</span><span class="sxs-lookup"><span data-stu-id="cc000-162">Sanitizing user input is difficult.</span></span> <span data-ttu-id="cc000-163">Kullanmaktan kaçının `HtmlHelper.Raw` kullanıcı girişi ile.</span><span class="sxs-lookup"><span data-stu-id="cc000-163">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="cc000-164">Aşağıdaki HTML kod işler:</span><span class="sxs-lookup"><span data-stu-id="cc000-164">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="cc000-165">Razor kod blokları</span><span class="sxs-lookup"><span data-stu-id="cc000-165">Razor code blocks</span></span>

<span data-ttu-id="cc000-166">Razor kod blokları başlayarak `@` ve tarafından içine `{}`.</span><span class="sxs-lookup"><span data-stu-id="cc000-166">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="cc000-167">İfadeler C# kodu kod blokları içinde çizilir değil.</span><span class="sxs-lookup"><span data-stu-id="cc000-167">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="cc000-168">Kod blokları bir görünüm ifadelerinde aynı kapsamı paylaşabilir ve sıraya göre tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="cc000-168">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="cc000-169">Aşağıdaki HTML kod işler:</span><span class="sxs-lookup"><span data-stu-id="cc000-169">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="cc000-170">Örtük geçişleri</span><span class="sxs-lookup"><span data-stu-id="cc000-170">Implicit transitions</span></span>

<span data-ttu-id="cc000-171">Kod bloğu varsayılan dil C# olduğu, ancak Razor sayfasını HTML olarak geçiş yapabilir:</span><span class="sxs-lookup"><span data-stu-id="cc000-171">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="cc000-172">Açık sınırlandırılmış geçiş</span><span class="sxs-lookup"><span data-stu-id="cc000-172">Explicit delimited transition</span></span>

<span data-ttu-id="cc000-173">HTML oluşturması gerektiğini bir kod bloğunun alt tanımlamak için Razor ile işleme için karakterleri arasında  **\<metin >** etiketi:</span><span class="sxs-lookup"><span data-stu-id="cc000-173">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="cc000-174">Bir HTML etiketi tarafından çevrelenen olmayan HTML oluşturmak için bu yaklaşımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="cc000-174">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="cc000-175">Bir HTML veya Razor etiketi bir Razor çalışma zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="cc000-175">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="cc000-176">**\<Metin >** etiketi, içeriği işlenirken boşluk denetlemek yararlıdır:</span><span class="sxs-lookup"><span data-stu-id="cc000-176">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="cc000-177">Yalnızca arasında içerik  **\<metin >** etiketi işlenir.</span><span class="sxs-lookup"><span data-stu-id="cc000-177">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="cc000-178">Hiçbir boşluk önce veya sonra  **\<metin >** etiketi HTML çıkışında görünür.</span><span class="sxs-lookup"><span data-stu-id="cc000-178">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="cc000-179">Açık satır geçişle @:</span><span class="sxs-lookup"><span data-stu-id="cc000-179">Explicit Line Transition with @:</span></span>

<span data-ttu-id="cc000-180">Tam bir satırın geri kalanı bir kod bloğunun içine HTML olarak işlemek için kullanmak `@:` sözdizimi:</span><span class="sxs-lookup"><span data-stu-id="cc000-180">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="cc000-181">Olmadan `@:` kodda bir Razor çalışma zamanı hatası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cc000-181">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="cc000-182">Uyarı: Ek `@` Razor dosyasının karakter bloğu içinde deyimleri neden derleyici hataları neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="cc000-182">Warning: Extra `@` characters in a Razor file can cause cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="cc000-183">Bu derleyici hataları gerçek hata önce bildirilen hata oluştuğundan anlaşılması zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="cc000-183">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="cc000-184">Bu hata, bir tek bir kod bloğu birden çok dolaylı/açık ifadelere birleştirme sonra yaygındır.</span><span class="sxs-lookup"><span data-stu-id="cc000-184">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="cc000-185">Denetim yapıları</span><span class="sxs-lookup"><span data-stu-id="cc000-185">Control Structures</span></span>

<span data-ttu-id="cc000-186">Denetim yapıları kod blokları bir uzantı var.</span><span class="sxs-lookup"><span data-stu-id="cc000-186">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="cc000-187">Ayrıca kod blokları (geçiş biçimlendirmesi, satır içi C#) tüm yönlerini aşağıdaki yapıları için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="cc000-187">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="cc000-188">Koşulları @if, başka ise, başka ve@switch</span><span class="sxs-lookup"><span data-stu-id="cc000-188">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="cc000-189">`@if`kod çalıştığında denetimleri:</span><span class="sxs-lookup"><span data-stu-id="cc000-189">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="cc000-190">`else`ve `else if` gerektirmeyen `@` simgesi:</span><span class="sxs-lookup"><span data-stu-id="cc000-190">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="cc000-191">Aşağıdaki biçimlendirmede switch deyimi kullanmayı gösterir:</span><span class="sxs-lookup"><span data-stu-id="cc000-191">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="cc000-192">Döngü @for, @foreach, @while, ve @do sırada</span><span class="sxs-lookup"><span data-stu-id="cc000-192">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="cc000-193">Şablonlu HTML denetim ifadeleri döngü ile oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="cc000-193">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="cc000-194">Kişilerin listesini oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="cc000-194">To render a list of people:</span></span>

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

<span data-ttu-id="cc000-195">Aşağıdaki döngü ifadeleri desteklenir:</span><span class="sxs-lookup"><span data-stu-id="cc000-195">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="cc000-196">Bileşik@using</span><span class="sxs-lookup"><span data-stu-id="cc000-196">Compound @using</span></span>

<span data-ttu-id="cc000-197">C# ' ta, bir `using` deyimi, bir nesne kapatılır emin olmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cc000-197">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="cc000-198">Razor aynı mekanizmayı ek içeriklere sahip bir HTML Yardımcıları oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cc000-198">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="cc000-199">Aşağıdaki kodda bir form etiketi HTML Yardımcıları işlemek `@using` deyimi:</span><span class="sxs-lookup"><span data-stu-id="cc000-199">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


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

<span data-ttu-id="cc000-200">Kapsam düzeyinde Eylemler ile yapılabilir [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="cc000-200">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="cc000-201">@try, son olarak, yakalama</span><span class="sxs-lookup"><span data-stu-id="cc000-201">@try, catch, finally</span></span>

<span data-ttu-id="cc000-202">Özel durum işleme, C# benzer:</span><span class="sxs-lookup"><span data-stu-id="cc000-202">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="cc000-203">Razor kritik bölümler kilit deyimleri ile koruma özelliği vardır:</span><span class="sxs-lookup"><span data-stu-id="cc000-203">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="cc000-204">Açıklamalar</span><span class="sxs-lookup"><span data-stu-id="cc000-204">Comments</span></span>

<span data-ttu-id="cc000-205">Razor C# ve HTML açıklamaları destekler:</span><span class="sxs-lookup"><span data-stu-id="cc000-205">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="cc000-206">Aşağıdaki HTML kod işler:</span><span class="sxs-lookup"><span data-stu-id="cc000-206">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="cc000-207">Web sayfası işlenmeden önce razor açıklama sunucu tarafından kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="cc000-207">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="cc000-208">Razor kullanan `@*  *@` açıklamaları sınırlandırmak için.</span><span class="sxs-lookup"><span data-stu-id="cc000-208">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="cc000-209">Sunucu tüm biçimlendirmesi oluşturmak olmayan şekilde aşağıdaki kod, kılınmıştır:</span><span class="sxs-lookup"><span data-stu-id="cc000-209">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="cc000-210">Yönergeler</span><span class="sxs-lookup"><span data-stu-id="cc000-210">Directives</span></span>

<span data-ttu-id="cc000-211">Ayrılmış anahtar sözcükler aşağıdaki örtük ifadelerle ile temsil Razor yönergeleri `@` simgesi.</span><span class="sxs-lookup"><span data-stu-id="cc000-211">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="cc000-212">Bir yönerge genellikle bir görünüm ayrıştırılır veya farklı işlevler sağlayan şeklini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="cc000-212">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="cc000-213">Razor kod bir görünümün nasıl oluşturur anlama yönergeleri nasıl çalıştığını anlamak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="cc000-213">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="cc000-214">Kod aşağıdakine benzer bir sınıf oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cc000-214">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="cc000-215">Bu makalede, bölümün sonraki [bir görünümü için oluşturulan Razor C# sınıfı görüntüleme](#viewing-the-razor-c-class-generated-for-a-view) bu oluşturulan sınıf görüntülemek açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="cc000-215">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="cc000-216">`@using` Yönergesi ekler C# `using` yönerge oluşturulan görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="cc000-216">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="cc000-217">`@model` Yönergesi bir görünüme iletilen model türünü belirtir:</span><span class="sxs-lookup"><span data-stu-id="cc000-217">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="cc000-218">Bireysel kullanıcı hesapları ile oluşturulmuş bir ASP.NET Core MVC uygulamasında *Views/Account/Login.cshtml* görünümü aşağıdaki model bildirimi içerir:</span><span class="sxs-lookup"><span data-stu-id="cc000-218">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="cc000-219">Oluşturulan sınıf devraldığı `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="cc000-219">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="cc000-220">Razor kullanıma sunan bir `Model` model erişmek için özelliği geçirilen görünümü:</span><span class="sxs-lookup"><span data-stu-id="cc000-220">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="cc000-221">`@model` Yönergesi bu özelliğin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="cc000-221">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="cc000-222">Yönergeyi belirtir `T` içinde `RazorPage<T>` türeyen görünümü oluşturulan, sınıfın.</span><span class="sxs-lookup"><span data-stu-id="cc000-222">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="cc000-223">Varsa `@model` belirtilen yönerge iisn't `Model` özelliği türüdür `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="cc000-223">If the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="cc000-224">Model değeri denetleyicisinden görünümüne geçirilir.</span><span class="sxs-lookup"><span data-stu-id="cc000-224">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="cc000-225">Daha fazla bilgi için [modelleri'kesin türü belirtilmiş ve @model anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="cc000-225">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="cc000-226">`@inherits` Yönergesi görünümü devralır sınıfının tam denetim sağlar:</span><span class="sxs-lookup"><span data-stu-id="cc000-226">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="cc000-227">Aşağıdaki kod bir özel Razor sayfa türüdür:</span><span class="sxs-lookup"><span data-stu-id="cc000-227">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="cc000-228">`CustomText` Bir görünümde görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="cc000-228">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="cc000-229">Aşağıdaki HTML kod işler:</span><span class="sxs-lookup"><span data-stu-id="cc000-229">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="cc000-230">`@model`ve `@inherits` aynı görünümünde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cc000-230">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="cc000-231">`@inherits`kullanılabilir bir *_viewımports.cshtml* görünümü alır dosyası:</span><span class="sxs-lookup"><span data-stu-id="cc000-231">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="cc000-232">Aşağıdaki kod, kesin türü belirtilmiş görünüm örneğidir:</span><span class="sxs-lookup"><span data-stu-id="cc000-232">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="cc000-233">Varsa "rick@contoso.com" geçirilen modeldeki görünümünü aşağıdaki HTML biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cc000-233">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="cc000-234">`@inject` Yönergesi sağlayan bir hizmetinden eklemesine Razor sayfasını [hizmet kapsayıcısı](xref:fundamentals/dependency-injection) görünüm.</span><span class="sxs-lookup"><span data-stu-id="cc000-234">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="cc000-235">Daha fazla bilgi için bkz: [bağımlılık ekleme görünümleri içine](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cc000-235">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="cc000-236">`@functions` Yönergesi işlev düzeyi içerik için bir görünüm eklemek bir Razor sayfasını sağlar:</span><span class="sxs-lookup"><span data-stu-id="cc000-236">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="cc000-237">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cc000-237">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="cc000-238">Kod aşağıdaki HTML biçimlendirmeleri oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cc000-238">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="cc000-239">Aşağıdaki kodu oluşturulan Razor C# sınıf verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="cc000-239">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="cc000-240">`@section` Yönergesi ile birlikte kullanılan [düzeni](xref:mvc/views/layout) içeriği HTML sayfası farklı bölümlerini işlemek görünümleri etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="cc000-240">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="cc000-241">Daha fazla bilgi için bkz: [bölümleri](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="cc000-241">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="cc000-242">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="cc000-242">Tag Helpers</span></span>

<span data-ttu-id="cc000-243">İlgilidir üç yönergeleri vardır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="cc000-243">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="cc000-244">Yönergesi</span><span class="sxs-lookup"><span data-stu-id="cc000-244">Directive</span></span> | <span data-ttu-id="cc000-245">İşlev</span><span class="sxs-lookup"><span data-stu-id="cc000-245">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="cc000-246">Etiket Yardımcıları bir görünüm için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="cc000-246">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="cc000-247">Etiket Yardımcıları daha önce eklediğiniz bir görünümden kaldırır.</span><span class="sxs-lookup"><span data-stu-id="cc000-247">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="cc000-248">Etiket Yardımcısı desteğinin etkinleştirmek ve etiket Yardımcısı kullanım açık yapmak için etiket öneki belirtir.</span><span class="sxs-lookup"><span data-stu-id="cc000-248">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="cc000-249">Razor ayrılmış anahtar sözcükler</span><span class="sxs-lookup"><span data-stu-id="cc000-249">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="cc000-250">Razor anahtar sözcükler</span><span class="sxs-lookup"><span data-stu-id="cc000-250">Razor keywords</span></span>

* <span data-ttu-id="cc000-251">Sayfa (ASP.NET Core 2.0 ve üzeri gerektirir)</span><span class="sxs-lookup"><span data-stu-id="cc000-251">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="cc000-252">işlevleri</span><span class="sxs-lookup"><span data-stu-id="cc000-252">functions</span></span>
* <span data-ttu-id="cc000-253">devralır</span><span class="sxs-lookup"><span data-stu-id="cc000-253">inherits</span></span>
* <span data-ttu-id="cc000-254">model</span><span class="sxs-lookup"><span data-stu-id="cc000-254">model</span></span>
* <span data-ttu-id="cc000-255">section</span><span class="sxs-lookup"><span data-stu-id="cc000-255">section</span></span>
* <span data-ttu-id="cc000-256">yardımcı (ASP.NET Core tarafından şu anda desteklenmiyor)</span><span class="sxs-lookup"><span data-stu-id="cc000-256">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="cc000-257">Razor anahtar sözcükleri ile kaçışlı `@(Razor Keyword)` (örneğin, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="cc000-257">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="cc000-258">C# Razor anahtar sözcükler</span><span class="sxs-lookup"><span data-stu-id="cc000-258">C# Razor keywords</span></span>

* <span data-ttu-id="cc000-259">büyük/küçük harf</span><span class="sxs-lookup"><span data-stu-id="cc000-259">case</span></span>
* <span data-ttu-id="cc000-260">do</span><span class="sxs-lookup"><span data-stu-id="cc000-260">do</span></span>
* <span data-ttu-id="cc000-261">default</span><span class="sxs-lookup"><span data-stu-id="cc000-261">default</span></span>
* <span data-ttu-id="cc000-262">for</span><span class="sxs-lookup"><span data-stu-id="cc000-262">for</span></span>
* <span data-ttu-id="cc000-263">foreach</span><span class="sxs-lookup"><span data-stu-id="cc000-263">foreach</span></span>
* <span data-ttu-id="cc000-264">if</span><span class="sxs-lookup"><span data-stu-id="cc000-264">if</span></span>
* <span data-ttu-id="cc000-265">else</span><span class="sxs-lookup"><span data-stu-id="cc000-265">else</span></span>
* <span data-ttu-id="cc000-266">lock</span><span class="sxs-lookup"><span data-stu-id="cc000-266">lock</span></span>
* <span data-ttu-id="cc000-267">anahtarı</span><span class="sxs-lookup"><span data-stu-id="cc000-267">switch</span></span>
* <span data-ttu-id="cc000-268">Deneyin</span><span class="sxs-lookup"><span data-stu-id="cc000-268">try</span></span>
* <span data-ttu-id="cc000-269">Yakalama</span><span class="sxs-lookup"><span data-stu-id="cc000-269">catch</span></span>
* <span data-ttu-id="cc000-270">finally</span><span class="sxs-lookup"><span data-stu-id="cc000-270">finally</span></span>
* <span data-ttu-id="cc000-271">kullanma</span><span class="sxs-lookup"><span data-stu-id="cc000-271">using</span></span>
* <span data-ttu-id="cc000-272">while</span><span class="sxs-lookup"><span data-stu-id="cc000-272">while</span></span>

<span data-ttu-id="cc000-273">C# Razor anahtar sözcükler, çift kaçışlı ile olmalıdır `@(@C# Razor Keyword)` (örneğin, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="cc000-273">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="cc000-274">İlk `@` Razor ayrıştırıcısı çıkışları.</span><span class="sxs-lookup"><span data-stu-id="cc000-274">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="cc000-275">İkinci `@` C# ayrıştırıcı çıkışları.</span><span class="sxs-lookup"><span data-stu-id="cc000-275">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="cc000-276">Razor tarafından kullanılmayan ayrılmış anahtar sözcükler</span><span class="sxs-lookup"><span data-stu-id="cc000-276">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="cc000-277">ad alanı</span><span class="sxs-lookup"><span data-stu-id="cc000-277">namespace</span></span>
* <span data-ttu-id="cc000-278">sınıf</span><span class="sxs-lookup"><span data-stu-id="cc000-278">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="cc000-279">Bir görünümü için oluşturulan Razor C# sınıfı görüntüleme</span><span class="sxs-lookup"><span data-stu-id="cc000-279">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="cc000-280">Aşağıdaki sınıf ASP.NET Core MVC projenize ekleyin:</span><span class="sxs-lookup"><span data-stu-id="cc000-280">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="cc000-281">Geçersiz kılma `RazorTemplateEngine` MVC tarafından eklenen `CustomTemplateEngine` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="cc000-281">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="cc000-282">Bir kesme noktası ayarlayın `return csharpDocument` deyiminin `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="cc000-282">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="cc000-283">Program yürütme kesme noktasına durduğunda değerini görüntülemek `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="cc000-283">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![GeneratedCode metin Görselleştirici görünümü](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="cc000-285">Görünüm aramaları ve büyük/küçük harfe duyarlılık</span><span class="sxs-lookup"><span data-stu-id="cc000-285">View lookups and case sensitivity</span></span>

<span data-ttu-id="cc000-286">Razor görüntüleme altyapısı için görünümleri büyük küçük harfe duyarlı arama gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="cc000-286">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="cc000-287">Ancak, gerçek arama temel alınan dosya sistemi tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="cc000-287">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="cc000-288">Dosya tabanlı kaynağı:</span><span class="sxs-lookup"><span data-stu-id="cc000-288">File based source:</span></span> 
  * <span data-ttu-id="cc000-289">Büyük küçük harfe duyarlı dosya sistemleri (örneğin, Windows) ile işletim sistemlerinde, fiziksel dosya sağlayıcısı aramaları büyük küçük harfe duyarlı.</span><span class="sxs-lookup"><span data-stu-id="cc000-289">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="cc000-290">Örneğin, `return View("Test")` için eşleşen sonuçlanıyor */Views/Home/Test.cshtml*, */Views/home/test.cshtml*ve diğer büyük/küçük harf değişken.</span><span class="sxs-lookup"><span data-stu-id="cc000-290">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="cc000-291">Büyük küçük harfe duyarlı dosya sistemlerindeki (örneğin, Linux, OSX ile `EmbeddedFileProvider`), arama duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="cc000-291">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="cc000-292">Örneğin, `return View("Test")` özellikle eşleşmeleri */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cc000-292">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="cc000-293">Görünümleri önceden derlenmiş: önceden derlenmiş görünümleri arayan ASP.NET Core ile 2.0 ve üzeri, servis talebi tüm işletim sistemlerinde harflere duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="cc000-293">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="cc000-294">Davranış Windows fiziksel dosya sağlayıcının davranışı aynıdır.</span><span class="sxs-lookup"><span data-stu-id="cc000-294">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="cc000-295">İki önceden derlenmiş görünüm yalnızca durumda farklıysa, arama sonucu belirleyici değildir.</span><span class="sxs-lookup"><span data-stu-id="cc000-295">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="cc000-296">Geliştiriciler, büyük/küçük harf, büyük küçük harf dosya ve dizin adlarını eşleştirmek için önerilir:</span><span class="sxs-lookup"><span data-stu-id="cc000-296">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="cc000-297">Alan, denetleyici ve eylem adları.</span><span class="sxs-lookup"><span data-stu-id="cc000-297">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="cc000-298">Razor sayfalarının.</span><span class="sxs-lookup"><span data-stu-id="cc000-298">Razor Pages.</span></span>
    
<span data-ttu-id="cc000-299">Servis talebi eşleşen dağıtımları temeldeki dosya sistemi bağımsız olarak kendi görünümler Bul sağlar.</span><span class="sxs-lookup"><span data-stu-id="cc000-299">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>