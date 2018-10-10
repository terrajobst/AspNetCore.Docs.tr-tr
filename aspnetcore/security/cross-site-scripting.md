---
title: Siteler arası betik kullanmayı (XSS) ASP.NET core'da engelle
author: rick-anderson
description: Siteler arası betik (XSS) ve ASP.NET Core uygulaması, bu güvenlik açığını ele alan teknikleri öğrenin.
ms.author: riande
ms.date: 10/02/2018
uid: security/cross-site-scripting
ms.openlocfilehash: 50f0211a2c64708d9b788dd10ce9064e66014d55
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910531"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a><span data-ttu-id="51d02-103">Siteler arası betik kullanmayı (XSS) ASP.NET core'da engelle</span><span class="sxs-lookup"><span data-stu-id="51d02-103">Prevent Cross-Site Scripting (XSS) in ASP.NET Core</span></span>

<span data-ttu-id="51d02-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="51d02-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="51d02-105">Siteler arası betik (XSS), bir saldırganın istemci tarafı komut dosyalarını (genellikle JavaScript) web sayfalarına yerleştirmek sağlayan bir güvenlik açığı var.</span><span class="sxs-lookup"><span data-stu-id="51d02-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="51d02-106">Diğer kullanıcıların saldırganın komut dosyası çalışma etkilenen sayfaları yüklediğinizde, tanımlama bilgileri ve oturum belirteçleri çalmaya saldırgan etkinleştirme DOM işlemesi aracılığıyla web sayfasının içeriğini değiştirebilir veya tarayıcıyı yeniden yönlendirmek için başka bir sayfa.</span><span class="sxs-lookup"><span data-stu-id="51d02-106">When other users load affected pages the attacker's scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="51d02-107">Bir uygulamanın kullanıcı girişini alır ve doğrulama, kodlama veya, kaçış olmadan bir sayfaya çıkarır XSS Güvenlik Açıkları genellikle ortaya çıkar.</span><span class="sxs-lookup"><span data-stu-id="51d02-107">XSS vulnerabilities generally occur when an application takes user input and outputs it to a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="51d02-108">Uygulamanızı XSS karşı koruma</span><span class="sxs-lookup"><span data-stu-id="51d02-108">Protecting your application against XSS</span></span>

<span data-ttu-id="51d02-109">En temel bir düzey XSS çalışır içine ekleyerek uygulamanızı şekilde kandırma tarafından bir `<script>` etiketi ekleyerek veya işlenen sayfanız bir `On*` olay içine bir öğe.</span><span class="sxs-lookup"><span data-stu-id="51d02-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="51d02-110">Geliştiriciler kendi uygulamasına XSS önlemek için aşağıdaki önleme adımları kullanmalısınız.</span><span class="sxs-lookup"><span data-stu-id="51d02-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="51d02-111">Hiçbir zaman aşağıdaki adımları izlemeden sürece güvenilir olmayan verileri, HTML giriş yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="51d02-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="51d02-112">Güvenilir olmayan verileri kontrol edilebilir bir saldırgan, HTML form girişleri, sorgu dizeleri, HTTP üstbilgileri, bir saldırganın uygulamanızı ihlal edemiyor olsanız bile, veritabanınızı güvenlik ihlali çözebileceğiniz gibi bir veritabanından kaynaklanan bile veri herhangi bir veridir.</span><span class="sxs-lookup"><span data-stu-id="51d02-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="51d02-113">HTML öğesi içinde güvenilir olmayan verileri geçirmeden önce kodlanmış HTML olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="51d02-113">Before putting untrusted data inside an HTML element ensure it's HTML encoded.</span></span> <span data-ttu-id="51d02-114">HTML kodlaması gereken karakter gibi &lt; ve bunları gibi güvenli bir forma değişiklikleri &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="51d02-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="51d02-115">HTML özniteliğin güvenilir olmayan verileri geçirmeden önce kodlanmış HTML olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="51d02-115">Before putting untrusted data into an HTML attribute ensure it's HTML encoded.</span></span> <span data-ttu-id="51d02-116">HTML öznitelik kodlaması HTML kodlaması bir üst kümesidir ve ek karakterler gibi kodlar "ve '.</span><span class="sxs-lookup"><span data-stu-id="51d02-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="51d02-117">JavaScript ile güvenilir olmayan verileri geçirmeden önce veri içerikleri çalışma zamanında almak bir HTML öğesi koyun.</span><span class="sxs-lookup"><span data-stu-id="51d02-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="51d02-118">Bu mümkün değilse, ardından JavaScript kodlanmış verileri olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="51d02-118">If this isn't possible, then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="51d02-119">JavaScript kodlama için JavaScript tehlikeli karakterleri alır ve örneğin kendi onaltılık ile değiştirir &lt; olarak kodlanması `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="51d02-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="51d02-120">URL sorgu dizesi içinde güvenilir olmayan verileri geçirmeden önce URL kodlanmış olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="51d02-120">Before putting untrusted data into a URL query string ensure it's URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="51d02-121">Razor kullanarak HTML kodlama</span><span class="sxs-lookup"><span data-stu-id="51d02-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="51d02-122">Razor altyapı MVC'de otomatik olarak kullanılan tüm kodlar gerçekten sabit, bunu önlemek için çalışmıyorsanız değişkenlerinden, çıkış kaynağı.</span><span class="sxs-lookup"><span data-stu-id="51d02-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="51d02-123">Her kullandığınızda HTML öznitelik kodlama kurallarını kullanır. *@* yönergesi.</span><span class="sxs-lookup"><span data-stu-id="51d02-123">It uses HTML attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="51d02-124">HTML öznitelik kodlaması, kendiniz, HTML kodlaması veya HTML öznitelik kodlaması kullanmanız gerekir ile uğraşmak zorunda olmadığınız anlamına gelir HTML kodlaması bir üst kümesidir.</span><span class="sxs-lookup"><span data-stu-id="51d02-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="51d02-125">Yalnızca @ HTML bağlamında, güvenilmeyen girişler doğrudan JavaScript eklemek değil çalışırken kullanmanızı emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="51d02-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="51d02-126">Etiket Yardımcıları de giriş etiketi parametrelerinde kullandığınız kodlar.</span><span class="sxs-lookup"><span data-stu-id="51d02-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="51d02-127">Aşağıdaki Razor görünüm uygulayın:</span><span class="sxs-lookup"><span data-stu-id="51d02-127">Take the following Razor view:</span></span>

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="51d02-128">Bu görünüm içeriğini çıkarır *untrustedInput* değişkeni.</span><span class="sxs-lookup"><span data-stu-id="51d02-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="51d02-129">Bu değişken XSS saldırılarında, yani kullanılan bazı karakterler içeren &lt;, "ve &gt;.</span><span class="sxs-lookup"><span data-stu-id="51d02-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="51d02-130">Kaynak İnceleme olarak kodlanmış işlenmiş çıktı gösterir:</span><span class="sxs-lookup"><span data-stu-id="51d02-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="51d02-131">ASP.NET Core MVC sağlayan bir `HtmlString` sınıfını otomatik olarak çıktı kodlanmış değil.</span><span class="sxs-lookup"><span data-stu-id="51d02-131">ASP.NET Core MVC provides an `HtmlString` class which isn't automatically encoded upon output.</span></span> <span data-ttu-id="51d02-132">Bu, XSS bir güvenlik açığı harekete geçirecek şekilde bu hiçbir zaman güvenilmeyen giriş ile birlikte kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="51d02-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="51d02-133">Razor kullanarak JavaScript kodlama</span><span class="sxs-lookup"><span data-stu-id="51d02-133">JavaScript Encoding using Razor</span></span>

<span data-ttu-id="51d02-134">JavaScript görünümünüzde işlemek için bir değer eklemek istediğiniz zamanlar olabilir.</span><span class="sxs-lookup"><span data-stu-id="51d02-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="51d02-135">Bunu yapmanın iki yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="51d02-135">There are two ways to do this.</span></span> <span data-ttu-id="51d02-136">Bir veri özniteliği bir etiketin değeri koyun ve, JavaScript dilinde almak için değerleri eklemek için en güvenli yolu var.</span><span class="sxs-lookup"><span data-stu-id="51d02-136">The safest way to insert values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="51d02-137">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="51d02-137">For example:</span></span>

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="51d02-138">Bu aşağıdaki HTML'yi oluşturur</span><span class="sxs-lookup"><span data-stu-id="51d02-138">This will produce the following HTML</span></span>

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="51d02-139">Çalıştığında, aşağıdaki şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="51d02-139">Which, when it runs, will render the following:</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="51d02-140">JavaScript Kodlayıcı doğrudan de çağırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="51d02-140">You can also call the JavaScript encoder directly:</span></span>

```cshtml
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

<span data-ttu-id="51d02-141">Bu tarayıcıda şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="51d02-141">This will render in the browser as follows:</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="51d02-142">DOM öğeleri oluşturmak için JavaScript güvenilmeyen girişinde birleştirme yok.</span><span class="sxs-lookup"><span data-stu-id="51d02-142">Don't concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="51d02-143">Kullanmanız gereken `createElement()` ve özellik değerlerini uygun şekilde aşağıdaki gibi atayabilirsiniz `node.TextContent=`, veya `element.SetAttribute()` / `element[attribute]=` Aksi takdirde, kendiniz için DOM tabanlı XSS kullanıma.</span><span class="sxs-lookup"><span data-stu-id="51d02-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="51d02-144">Kod kodlayıcılara erişme</span><span class="sxs-lookup"><span data-stu-id="51d02-144">Accessing encoders in code</span></span>

<span data-ttu-id="51d02-145">Aracılığıyla ekleyebilir, HTML, JavaScript ve URL kodlayıcılarda kodunuzu iki şekilde kullanılabilir [bağımlılık ekleme](xref:fundamentals/dependency-injection) veya içerdiği varsayılan Kodlayıcıları kullanabilirsiniz `System.Text.Encodings.Web` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="51d02-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](xref:fundamentals/dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="51d02-146">İçin uygulanan tüm sonra varsayılan kodlayıcılarda kullanırsanız güvenli olarak kabul edilmesi için karakter aralıkları uygulanmayacak - varsayılan kodlayıcılarda olası güvenli kodlama kurallarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="51d02-146">If you use the default encoders then any  you applied to character ranges to be treated as safe won't take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="51d02-147">DI, oluşturucu kısa sürecektir aracılığıyla yapılandırılabilir kodlayıcılarda kullanmak için bir *HtmlEncoder*, *JavaScriptEncoder* ve *UrlEncoder* uygun şekilde parametresi.</span><span class="sxs-lookup"><span data-stu-id="51d02-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="51d02-148">Örneğin;</span><span class="sxs-lookup"><span data-stu-id="51d02-148">For example;</span></span>

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a><span data-ttu-id="51d02-149">URL parametrelerini kodlama</span><span class="sxs-lookup"><span data-stu-id="51d02-149">Encoding URL Parameters</span></span>

<span data-ttu-id="51d02-150">URL sorgu dizesi güvenilmeyen giriş olarak bir değer kullanımı ile derlemek istiyorsanız `UrlEncoder` değeri kodlamak için.</span><span class="sxs-lookup"><span data-stu-id="51d02-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="51d02-151">Örneğin,</span><span class="sxs-lookup"><span data-stu-id="51d02-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="51d02-152">EncodedValue kodladıktan sonra değişken içerecektir `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="51d02-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="51d02-153">Alanları, tırnak işaretleri, noktalama işaretleri ve diğer güvenli olmayan karakterleri kendi onaltılı değerine kodlanmış yüzde olacaktır, örneğin bir boşluk karakteri % 20 olur.</span><span class="sxs-lookup"><span data-stu-id="51d02-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="51d02-154">Güvenilmeyen girişler, bir URL yolu bir parçası olarak kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="51d02-154">Don't use untrusted input as part of a URL path.</span></span> <span data-ttu-id="51d02-155">Her zaman güvenilmeyen Giriş bir sorgu dizesi değerini geçirin.</span><span class="sxs-lookup"><span data-stu-id="51d02-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="51d02-156">Kodlayıcıları özelleştirme</span><span class="sxs-lookup"><span data-stu-id="51d02-156">Customizing the Encoders</span></span>

<span data-ttu-id="51d02-157">Varsayılan olarak kodlayıcılar temel Latin Unicode aralığın sınırlı güvenli bir listesini kullanın ve bu aralığın dışında tüm karakterleri Karakter kodu eşdeğerlerine olarak kodlayın.</span><span class="sxs-lookup"><span data-stu-id="51d02-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="51d02-158">Dizelerinizi çıktısını almak için Kodlayıcıları kullanır gibi bu davranış, Razor TagHelper ve HtmlHelper işleme de etkiler.</span><span class="sxs-lookup"><span data-stu-id="51d02-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="51d02-159">Bunun ardındaki mantık (önceki tarayıcı hataları İngilizce olmayan karakterleri işleme dayalı ayrıştırma yukarı dönüş) bilinmeyen ya da gelecekte tarayıcı hataları karşı korunmasını sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="51d02-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="51d02-160">Web sitenizi Çince gibi Latin olmayan karakterler ağır olarak kullanan yaparsa Kiril veya başkalarının istediğiniz davranış budur olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="51d02-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="51d02-161">Kodlayıcı güvenli listeleri de aralık uygun uygulamanıza başlatma sırasında Unicode içerecek şekilde özelleştirebilirsiniz `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="51d02-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="51d02-162">Örneğin, varsayılan yapılandırmayla bir Razor HtmlHelper kullanıyor olabileceğiniz gibi bunu;</span><span class="sxs-lookup"><span data-stu-id="51d02-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="51d02-163">Kaynak web sayfası görüntülediğinizde gibi kodlanan metnin Çince ile işlendikten görürsünüz;</span><span class="sxs-lookup"><span data-stu-id="51d02-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="51d02-164">Karakter olarak kabul genişletmek için Kodlayıcı tarafından güvenli, aşağıdaki satır içine ekler `ConfigureServices()` yönteminde `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="51d02-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="51d02-165">Bu örnekte, Unicode aralığı CjkUnifiedIdeographs dahil etmek için güvenli listeye widens.</span><span class="sxs-lookup"><span data-stu-id="51d02-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="51d02-166">İşlenen çıkışı artık hale gelir</span><span class="sxs-lookup"><span data-stu-id="51d02-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="51d02-167">Güvenli listeye aralıkları Unicode kod grafikleri, değil diller belirtilir.</span><span class="sxs-lookup"><span data-stu-id="51d02-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="51d02-168">[Unicode standart](http://unicode.org/) listesine sahip [kod grafikleri](http://www.unicode.org/charts/index.html) , karakterler içeren grafik bulmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="51d02-168">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="51d02-169">Her Kodlayıcı, Html, JavaScript ve Url, ayrı olarak yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="51d02-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="51d02-170">Güvenli listeye özelleştirmesini yalnızca DI kaynaklanan kodlayıcılar etkiler.</span><span class="sxs-lookup"><span data-stu-id="51d02-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="51d02-171">Bir kodlayıcı aracılığıyla doğrudan erişirseniz `System.Text.Encodings.Web.*Encoder.Default` sonra varsayılan olarak, temel Latin yalnızca güvenli liste kullanılır.</span><span class="sxs-lookup"><span data-stu-id="51d02-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="51d02-172">Kodlama Al nereye yerleştirmeniz gerekir?</span><span class="sxs-lookup"><span data-stu-id="51d02-172">Where should encoding take place?</span></span>

<span data-ttu-id="51d02-173">Genel Uygulama kodlama çıkış noktasında gerçekleşir ve kodlanmış değerler hiçbir zaman bir veritabanında depolanacak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="51d02-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="51d02-174">Çıkış noktasında kodlama kullanımı verileri, örneğin, bir sorgu dizesi değeri için HTML değiştirmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="51d02-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="51d02-175">Ayrıca, arama yapmadan önce değerleri kodlamak zorunda kalmadan verilerinizi kolayca aramanızı sağlar ve herhangi bir değişiklik veya hata düzeltmeleri kodlayıcıya yapılan avantajlarından yararlanmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="51d02-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="51d02-176">Bir XSS önleme teknik olarak doğrulama</span><span class="sxs-lookup"><span data-stu-id="51d02-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="51d02-177">Doğrulama XSS saldırılarını sınırlama yararlı bir aracı olabilir.</span><span class="sxs-lookup"><span data-stu-id="51d02-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="51d02-178">Örneğin, yalnızca 0-9 karakterleri içeren bir sayısal dize XSS saldırının tetiklemez.</span><span class="sxs-lookup"><span data-stu-id="51d02-178">For example, a numeric string containing only the characters 0-9 won't trigger an XSS attack.</span></span> <span data-ttu-id="51d02-179">Doğrulama, kullanıcı girişini HTML kabul ederken daha karmaşık hale gelir.</span><span class="sxs-lookup"><span data-stu-id="51d02-179">Validation becomes more complicated when accepting HTML in user input.</span></span> <span data-ttu-id="51d02-180">HTML girişine ayrıştırma, imkansız, zordur.</span><span class="sxs-lookup"><span data-stu-id="51d02-180">Parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="51d02-181">Gömülü HTML şeritler bir ayrıştırıcı ile birlikte, markdown, zengin giriş kabul etmek için daha güvenli bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="51d02-181">Markdown, coupled with a parser that strips embedded HTML, is a safer option for accepting rich input.</span></span> <span data-ttu-id="51d02-182">Hiçbir zaman tek başına doğrulamasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="51d02-182">Never rely on validation alone.</span></span> <span data-ttu-id="51d02-183">Her zaman önce hangi doğrulama veya temizleme gerçekleştirilen ne olursa olsun çıktı, güvenilmeyen girişler kodlayın.</span><span class="sxs-lookup"><span data-stu-id="51d02-183">Always encode untrusted input before output, no matter what validation or sanitization has been performed.</span></span>
