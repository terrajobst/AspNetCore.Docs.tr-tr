---
title: "Siteler arası komut dosyası önleme"
author: rick-anderson
description: "Bu belge, siteler arası komut dosyası (XSS) ve ASP.NET Core uygulama bu güvenlik açığı adresleme teknikleri tanıtır."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cross-site-scripting
ms.openlocfilehash: 3aaab9d4fecd3f0d0da6a0df4d83bee090b329ea
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="preventing-cross-site-scripting"></a><span data-ttu-id="d9623-103">Siteler arası komut dosyası önleme</span><span class="sxs-lookup"><span data-stu-id="d9623-103">Preventing Cross-Site Scripting</span></span>

<span data-ttu-id="d9623-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d9623-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d9623-105">Siteler arası komut dosyası (XSS) istemci tarafı komut dosyalarını (genellikle JavaScript) web sayfalarına yerleştirmek bir saldırgan sağlayan bir güvenlik açığı bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d9623-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="d9623-106">Diğer kullanıcıların saldırganlar komut dosyaları çalıştırılır etkilenen sayfaları yüklediğinizde ve bu da saldırganın tanımlama bilgilerini ve oturum belirteçleri çalmak etkinleştirme DOM işleme aracılığıyla web sayfasının içeriği değiştirmek veya başka bir sayfaya tarayıcı yönlendirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9623-106">When other users load affected pages the attackers scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="d9623-107">Uygulamanın kullanıcı girişini alır ve doğrulama, kodlama veya onu kaçış olmadan bir sayfasında çıkarır XSS Güvenlik Açıkları genellikle oluşur.</span><span class="sxs-lookup"><span data-stu-id="d9623-107">XSS vulnerabilities generally occur when an application takes user input and outputs it in a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="d9623-108">Uygulamanızı XSS karşı koruma</span><span class="sxs-lookup"><span data-stu-id="d9623-108">Protecting your application against XSS</span></span>

<span data-ttu-id="d9623-109">En temel düzey XSS çalışır ekleme içine uygulamanızın kullanmak üzere kandırarak bir `<script>` etiketi, işlenen sayfasına veya ekleyerek bir `On*` bir öğenin içine olay.</span><span class="sxs-lookup"><span data-stu-id="d9623-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="d9623-110">Geliştiriciler kendi uygulamasına XSS önlemek için aşağıdaki önleme adımları kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d9623-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="d9623-111">Hiçbir zaman aşağıdaki adımları izlemeden sürece güvenilmeyen verileri, HTML giriş yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="d9623-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="d9623-112">Denetlenmesi bir saldırgan, HTML form girişleri, sorgu dizeleri, HTTP üstbilgileri, bir saldırgan, uygulamanızın ihlal olamaz olsa bile, veritabanınızı ihlal mümkün olabilir gibi bir veritabanından kaynaklanan bile veri tarafından herhangi bir veri güvenilmeyen verilerdir.</span><span class="sxs-lookup"><span data-stu-id="d9623-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="d9623-113">Bir HTML öğesi içindeki güvenilmeyen verileri geçirmeden önce HTML kodlu olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="d9623-113">Before putting untrusted data inside an HTML element ensure it's HTML encoded.</span></span> <span data-ttu-id="d9623-114">HTML kodlamasını alır karakterler gibi &lt; ve bunları gibi güvenli bir forma değiştirir &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="d9623-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="d9623-115">Bir HTML öznitelik güvenilmeyen veri geçirmeden önce kodlanmış HTML öznitelik olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="d9623-115">Before putting untrusted data into an HTML attribute ensure it's HTML attribute encoded.</span></span> <span data-ttu-id="d9623-116">HTML öznitelik kodlaması HTML kodlaması bir üst kümesidir ve ek karakterleri gibi kodlar "ve '.</span><span class="sxs-lookup"><span data-stu-id="d9623-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="d9623-117">JavaScript ile güvenilmeyen veri geçirmeden önce verileri içeriği çalışma zamanında almak bir HTML öğesi yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="d9623-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="d9623-118">Bu mümkün değilse daha sonra verileri olun JavaScript kodlanır.</span><span class="sxs-lookup"><span data-stu-id="d9623-118">If this isn't possible then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="d9623-119">JavaScript kodlama için JavaScript tehlikeli olabilecek karakterler alır ve bunları kendi onaltılık ile örneğin değiştirir &lt; olarak kodlanması `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="d9623-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="d9623-120">Bir URL sorgu dizesine güvenilmeyen veri geçirmeden önce URL kodlanmış olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="d9623-120">Before putting untrusted data into a URL query string ensure it's URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="d9623-121">Razor kullanarak HTML kodlama</span><span class="sxs-lookup"><span data-stu-id="d9623-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="d9623-122">MVC'de otomatik olarak kullanılan Razor altyapısının tüm kodlar çıkış kaynaklanan değişkenlerinden, böylece önlemek için gerçekten çok çalışan sürece.</span><span class="sxs-lookup"><span data-stu-id="d9623-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="d9623-123">HTML kodlama kurallarını, kullandığınızda özniteliğini kullanır  *@*  yönergesi.</span><span class="sxs-lookup"><span data-stu-id="d9623-123">It uses HTML Attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="d9623-124">HTML olarak bunu kendiniz, HTML kodlaması veya HTML öznitelik kodlaması kullanmanız gerekir ile ilgili gerekmediği anlamına gelir HTML kodlaması bir üst öznitelik kodlaması kümesidir.</span><span class="sxs-lookup"><span data-stu-id="d9623-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="d9623-125">Yalnızca @ bir HTML bağlamında doğrudan JavaScript ile güvenilmeyen giriş eklemek değil çalışırken kullandığınız emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d9623-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="d9623-126">Etiket Yardımcıları giriş etiketi parametrelerinde kullanmak da kodlar.</span><span class="sxs-lookup"><span data-stu-id="d9623-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="d9623-127">Aşağıdaki Razor görünüm alın;</span><span class="sxs-lookup"><span data-stu-id="d9623-127">Take the following Razor view;</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="d9623-128">Bu görünüm içeriğini çıkarır *untrustedInput* değişkeni.</span><span class="sxs-lookup"><span data-stu-id="d9623-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="d9623-129">Bu değişken XSS saldırılarında, yani kullanılan bazı karakterler içeren &lt;, "ve &gt;.</span><span class="sxs-lookup"><span data-stu-id="d9623-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="d9623-130">Kaynak inceleniyor olarak kodlanmış işlenmiş çıkış şunları gösterir:</span><span class="sxs-lookup"><span data-stu-id="d9623-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="d9623-131">ASP.NET Core MVC sağlayan bir `HtmlString` çıkış sırasında otomatik olarak kodlanmış değil sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d9623-131">ASP.NET Core MVC provides an `HtmlString` class which isn't automatically encoded upon output.</span></span> <span data-ttu-id="d9623-132">Bu XSS Güvenlik Açığı maruz bu asla güvenilmeyen giriş ile birlikte kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d9623-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="d9623-133">Razor kullanarak JavaScript kodlama</span><span class="sxs-lookup"><span data-stu-id="d9623-133">Javascript Encoding using Razor</span></span>

<span data-ttu-id="d9623-134">JavaScript görünümünüzde işlemek için bir değer eklemek istediğiniz durumlar olabilir.</span><span class="sxs-lookup"><span data-stu-id="d9623-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="d9623-135">Bunu yapmanın iki yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="d9623-135">There are two ways to do this.</span></span> <span data-ttu-id="d9623-136">Basit değerler eklemek için güvenli bir veri özniteliği bir etiket değeri koyun ve, JavaScript almak için yoludur.</span><span class="sxs-lookup"><span data-stu-id="d9623-136">The safest way to insert simple values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="d9623-137">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="d9623-137">For example:</span></span>

```none
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

<span data-ttu-id="d9623-138">Bu aşağıdaki HTML oluşturur</span><span class="sxs-lookup"><span data-stu-id="d9623-138">This will produce the following HTML</span></span>

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

<span data-ttu-id="d9623-139">Çalıştırıldığında, aşağıdaki oluşturmaz;</span><span class="sxs-lookup"><span data-stu-id="d9623-139">Which, when it runs, will render the following;</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="d9623-140">JavaScript Kodlayıcı doğrudan çağırabilir,</span><span class="sxs-lookup"><span data-stu-id="d9623-140">You can also call the JavaScript encoder directly,</span></span>

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

<span data-ttu-id="d9623-141">Bu tarayıcıda şu şekilde oluşturulur;</span><span class="sxs-lookup"><span data-stu-id="d9623-141">This will render in the browser as follows;</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="d9623-142">DOM öğeleri oluşturmak için JavaScript güvenilmeyen girişinde birleştirme yok.</span><span class="sxs-lookup"><span data-stu-id="d9623-142">Don't concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="d9623-143">Kullanmanız gereken `createElement()` ve özellik değerlerini uygun şekilde gibi atayın `node.TextContent=`, veya kullanmak `element.SetAttribute()` / `element[attribute]=` Aksi takdirde, kendiniz DOM tabanlı XSS ortaya.</span><span class="sxs-lookup"><span data-stu-id="d9623-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="d9623-144">Kodda kodlayıcılar erişme</span><span class="sxs-lookup"><span data-stu-id="d9623-144">Accessing encoders in code</span></span>

<span data-ttu-id="d9623-145">HTML, JavaScript ve URL kodlayıcılar kodunuzu iki yolla kullanılabilir, bunları aracılığıyla Ekle [bağımlılık ekleme](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) veya içinde yer alan varsayılan Kodlayıcıları kullanabilirsiniz `System.Text.Encodings.Web` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="d9623-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="d9623-146">Herhangi bir için uygulanan sonra varsayılan kodlayıcılar kullanırsanız, güvenli olarak kabul edilmesi için karakter aralıkları uygulanmayacak - varsayılan kodlayıcılar olası güvenli kodlama kuralları kullanın.</span><span class="sxs-lookup"><span data-stu-id="d9623-146">If you use the default encoders then any  you applied to character ranges to be treated as safe won't take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="d9623-147">DI, Oluşturucular almalıdır aracılığıyla yapılandırılabilir kodlayıcılar kullanmak için bir *HtmlEncoder*, *JavaScriptEncoder* ve *UrlEncoder* uygun şekilde parametresi.</span><span class="sxs-lookup"><span data-stu-id="d9623-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="d9623-148">Örneğin;</span><span class="sxs-lookup"><span data-stu-id="d9623-148">For example;</span></span>

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

## <a name="encoding-url-parameters"></a><span data-ttu-id="d9623-149">URL parametreleri kodlama</span><span class="sxs-lookup"><span data-stu-id="d9623-149">Encoding URL Parameters</span></span>

<span data-ttu-id="d9623-150">Güvenilmeyen giriş olarak bir değer kullanımı bir URL sorgu dizesi oluşturmak istiyorsanız `UrlEncoder` değeri kodlamak için.</span><span class="sxs-lookup"><span data-stu-id="d9623-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="d9623-151">Örneğin,</span><span class="sxs-lookup"><span data-stu-id="d9623-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="d9623-152">EncodedValue kodlama sonra değişken içerecek `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="d9623-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="d9623-153">Onaltılık değerlerine kodlanmış yüzde olacaktır, boşluk, tırnak işareti, noktalama ve diğer güvenli olmayan karakterler, örneğin bir boşluk karakteri % 20 olur.</span><span class="sxs-lookup"><span data-stu-id="d9623-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="d9623-154">Güvenilmeyen girdi bir URL yolu bir parçası olarak kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="d9623-154">Don't use untrusted input as part of a URL path.</span></span> <span data-ttu-id="d9623-155">Her zaman güvenilmeyen Giriş bir sorgu dizesi değerini geçirin.</span><span class="sxs-lookup"><span data-stu-id="d9623-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="d9623-156">Kodlayıcılar özelleştirme</span><span class="sxs-lookup"><span data-stu-id="d9623-156">Customizing the Encoders</span></span>

<span data-ttu-id="d9623-157">Varsayılan olarak kodlayıcılar temel Latin Unicode aralığı için sınırlı güvenli bir listesini kullanın ve bu aralığın dışında tüm karakterleri Karakter kodu eşdeğerlerine olarak kodlayın.</span><span class="sxs-lookup"><span data-stu-id="d9623-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="d9623-158">Dizelerinizi çıktısını almak için kodlayıcılar kullanacak şekilde bu davranış Razor TagHelper ve HtmlHelper işleme de etkiler.</span><span class="sxs-lookup"><span data-stu-id="d9623-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="d9623-159">Bu arkasındaki mantığı (önceki tarayıcı hatalar üzerindeki İngilizce olmayan karakterler işleme dayalı ayrıştırma yukarı dönüş) bilinmeyen veya gelecekteki tarayıcı hatalar karşı korumaktır.</span><span class="sxs-lookup"><span data-stu-id="d9623-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="d9623-160">Web sitenizi Çince gibi Latin olmayan karakterleri kullanımına ağırlık yaparsa Kiril veya başkalarının istediğiniz davranış budur olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="d9623-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="d9623-161">Unicode içinde aralıkları uygun başlatma sırasında uygulamanıza eklenecek Kodlayıcı güvenli listeler özelleştirebilirsiniz `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="d9623-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="d9623-162">Örneğin, bir Razor HtmlHelper kullanabilecekleri varsayılan yapılandırması'nı kullanarak şu şekilde;</span><span class="sxs-lookup"><span data-stu-id="d9623-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="d9623-163">Kaynak web sayfası görüntülediğinizde, aşağıdaki gibi kodlanmış Çince metinle işlenip işlenmediğini görürsünüz;</span><span class="sxs-lookup"><span data-stu-id="d9623-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="d9623-164">Kabul edilen karakterler genişletmek için güvenli Kodlayıcı tarafından aşağıdaki satır içine ekler `ConfigureServices()` yönteminde `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="d9623-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="d9623-165">Bu örnek Unicode aralığı CjkUnifiedIdeographs dahil etmek için güvenli listesi widens.</span><span class="sxs-lookup"><span data-stu-id="d9623-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="d9623-166">İşlenmiş çıkış şimdi olur</span><span class="sxs-lookup"><span data-stu-id="d9623-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="d9623-167">Güvenli listesi aralıkları Unicode kod grafikleri, değil diller belirtilir.</span><span class="sxs-lookup"><span data-stu-id="d9623-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="d9623-168">[Unicode standart](http://unicode.org/) listesini içeren [kod grafikleri](http://www.unicode.org/charts/index.html) , karakterler içeren grafik bulmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9623-168">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="d9623-169">Html, JavaScript ve Url, her Kodlayıcı ayrı olarak yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d9623-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="d9623-170">Güvenli listesi özelleştirilmesi yalnızca dı kaynaklanan kodlayıcılar etkiler.</span><span class="sxs-lookup"><span data-stu-id="d9623-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="d9623-171">Bir kodlayıcı aracılığıyla doğrudan erişim `System.Text.Encodings.Web.*Encoder.Default` sonra varsayılan olarak, temel Latin yalnızca güvenli liste kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d9623-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="d9623-172">Kodlama Al nereye?</span><span class="sxs-lookup"><span data-stu-id="d9623-172">Where should encoding take place?</span></span>

<span data-ttu-id="d9623-173">Genel kodlama çıkış noktasında gerçekleşir ve kodlanmış değerler hiçbir zaman bir veritabanında saklanmalıdır uygulamadır kabul edildi.</span><span class="sxs-lookup"><span data-stu-id="d9623-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="d9623-174">Çıktı noktasında kodlama bir sorgu dizesi değerini HTML gelen verilerin, örneğin, kullanımını değiştirmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="d9623-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="d9623-175">Ayrıca, arama yapmadan önce değerleri kodlamak zorunda kalmadan, verilerinizi kolayca aramanıza olanak tanır ve herhangi bir değişiklik veya hata düzeltmeleri kodlayıcıya yapılan yararlanmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="d9623-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="d9623-176">Bir XSS önleme teknik olarak doğrulama</span><span class="sxs-lookup"><span data-stu-id="d9623-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="d9623-177">Doğrulama XSS saldırılarını sınırlama de yararlı bir aracı olabilir.</span><span class="sxs-lookup"><span data-stu-id="d9623-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="d9623-178">Örneğin, yalnızca 0-9 karakter içeren basit bir sayısal dize XSS saldırısı tetiklemez.</span><span class="sxs-lookup"><span data-stu-id="d9623-178">For example, a simple numeric string containing only the characters 0-9 won't trigger an XSS attack.</span></span> <span data-ttu-id="d9623-179">Doğrulama HTML uygulamasında kullanıcı girdisi - kabul etmek istediğiniz HTML giriş ayrıştırma imkansız olmasa zordur daha karmaşık hale gelir.</span><span class="sxs-lookup"><span data-stu-id="d9623-179">Validation becomes more complicated should you wish to accept HTML in user input - parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="d9623-180">MarkDown ve diğer metin biçimleri zengin girişi için daha güvenli bir seçenek olabilir.</span><span class="sxs-lookup"><span data-stu-id="d9623-180">MarkDown and other text formats would be a safer option for rich input.</span></span> <span data-ttu-id="d9623-181">Hiçbir zaman tek başına doğrulama yararlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d9623-181">You should never rely on validation alone.</span></span> <span data-ttu-id="d9623-182">Her zaman güvenilmeyen giriş çıkış önce kodlama, ne olursa olsun, doğrulamanın.</span><span class="sxs-lookup"><span data-stu-id="d9623-182">Always encode untrusted input before output, no matter what validation you have performed.</span></span>
