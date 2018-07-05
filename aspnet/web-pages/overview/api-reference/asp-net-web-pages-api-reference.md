---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: ASP.NET Web sayfaları (Razor) API hızlı başvurusu | Microsoft Docs
author: tfitzmac
description: Bu sayfa, en sık kullanılan nesnelerin, özellikleri ve yöntemleri için Razor sözdizimi olan ASP.NET Web sayfaları programlama örnekleri kısa bir listeyle içerir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 3bf706cefe5302cf1085e0f814dc6654e42ae917
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378669"
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a><span data-ttu-id="f4da4-103">ASP.NET Web sayfaları (Razor) API hızlı başvurusu</span><span class="sxs-lookup"><span data-stu-id="f4da4-103">ASP.NET Web Pages (Razor) API Quick Reference</span></span>
====================
<span data-ttu-id="f4da4-104">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f4da4-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f4da4-105">Bu sayfa, en sık kullanılan nesnelerin, özellikleri ve yöntemleri için Razor sözdizimi olan ASP.NET Web sayfaları programlama örnekleri kısa bir listeyle içerir.</span><span class="sxs-lookup"><span data-stu-id="f4da4-105">This page contains a list with brief examples of the most commonly used objects, properties, and methods for programming ASP.NET Web Pages with Razor syntax.</span></span>
> 
> <span data-ttu-id="f4da4-106">ASP.NET Web sayfaları'nda sürüm 2 "(v2)" ile işaretlenmiş açıklamaları sunulur.</span><span class="sxs-lookup"><span data-stu-id="f4da4-106">Descriptions marked with "(v2)" were introduced in ASP.NET Web Pages version 2.</span></span>
> 
> <span data-ttu-id="f4da4-107">API başvuru belgeleri için bkz. [ASP.NET Web sayfaları başvuru belgeleri](https://go.microsoft.com/fwlink/?LinkId=208659) MSDN'de.</span><span class="sxs-lookup"><span data-stu-id="f4da4-107">For API reference documentation, see the [ASP.NET Web Pages Reference Documentation](https://go.microsoft.com/fwlink/?LinkId=208659) on MSDN.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="f4da4-108">Yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="f4da4-108">Software versions</span></span>
> 
> 
> - <span data-ttu-id="f4da4-109">ASP.NET Web sayfaları (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="f4da4-109">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="f4da4-110">Bu öğreticide, (v2 işaretlenmiş özellikleri dışında) ASP.NET Web sayfaları 1.0 ve ASP.NET Web Pages 2 ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="f4da4-110">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0 (except for features marked v2).</span></span>


<span data-ttu-id="f4da4-111">Bu sayfa aşağıdaki yönelik başvuru bilgileri içerir:</span><span class="sxs-lookup"><span data-stu-id="f4da4-111">This page contains reference information for the following:</span></span>

- [<span data-ttu-id="f4da4-112">Sınıflar</span><span class="sxs-lookup"><span data-stu-id="f4da4-112">Classes</span></span>](#Classes)
- [<span data-ttu-id="f4da4-113">Veri</span><span class="sxs-lookup"><span data-stu-id="f4da4-113">Data</span></span>](#Data)
- [<span data-ttu-id="f4da4-114">Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="f4da4-114">Helpers</span></span>](#Helpers)
- [<span data-ttu-id="f4da4-115">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="f4da4-115">Validation</span></span>](#Validation)

<a id="Classes"></a>
## <a name="classes"></a><span data-ttu-id="f4da4-116">Sınıflar</span><span class="sxs-lookup"><span data-stu-id="f4da4-116">Classes</span></span>

### `AppState[key], AppState[index],App`

<span data-ttu-id="f4da4-117">Uygulamadaki tüm sayfalar tarafından paylaşılabilen veriler içerir.</span><span class="sxs-lookup"><span data-stu-id="f4da4-117">Contains data that can be shared by any pages in the application.</span></span> <span data-ttu-id="f4da4-118">Dinamik kullanabileceğiniz `App` özelliği aşağıdaki örnekteki gibi aynı verilere erişmek için:</span><span class="sxs-lookup"><span data-stu-id="f4da4-118">You can use the dynamic `App` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

<span data-ttu-id="f4da4-119">Bir dize değerini Boolean (true/false) değerine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-119">Converts a string value to a Boolean value (true/false).</span></span> <span data-ttu-id="f4da4-120">Yanlış değerini döndürür veya belirtilen değer dizesi true/false temsil etmiyorsa.</span><span class="sxs-lookup"><span data-stu-id="f4da4-120">Returns false or the specified value if the string does not represent true/false.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

<span data-ttu-id="f4da4-121">Bir dize değerini tarih/saat değerine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-121">Converts a string value to date/time.</span></span> <span data-ttu-id="f4da4-122">Döndürür `DateTime.MinValue` veya belirtilen değer, dizeyi tarih/saat temsil etmiyor.</span><span class="sxs-lookup"><span data-stu-id="f4da4-122">Returns `DateTime.MinValue` or the specified value if the string does not represent a date/time.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

<span data-ttu-id="f4da4-123">Bir dize değeri bir ondalık değere dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-123">Converts a string value to a decimal value.</span></span> <span data-ttu-id="f4da4-124">0,0 döndürür veya belirtilen değer, dize ondalık bir değeri temsil etmiyor.</span><span class="sxs-lookup"><span data-stu-id="f4da4-124">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

<span data-ttu-id="f4da4-125">Bir dize değeri kayana dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-125">Converts a string value to a float.</span></span> <span data-ttu-id="f4da4-126">0,0 döndürür veya belirtilen değer, dize ondalık bir değeri temsil etmiyor.</span><span class="sxs-lookup"><span data-stu-id="f4da4-126">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

<span data-ttu-id="f4da4-127">Bir dize değeri bir tamsayıya dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-127">Converts a string value to an integer.</span></span> <span data-ttu-id="f4da4-128">0 döndürür veya belirtilen değer, dize bir tamsayı temsil etmiyor.</span><span class="sxs-lookup"><span data-stu-id="f4da4-128">Returns 0 or the specified value if the string does not represent an integer.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

<span data-ttu-id="f4da4-129">İsteğe bağlı ek yol bölümleri içeren bir yerel dosya yolundan tarayıcı ile uyumlu bir URL oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4da4-129">Creates a browser-compatible URL from a local file path, with optional additional path parts.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

<span data-ttu-id="f4da4-130">İşler *değer* HTML biçimlendirmesi olarak HTML olarak kodlanan işleme yerine olarak.</span><span class="sxs-lookup"><span data-stu-id="f4da4-130">Renders *value* as HTML markup instead of rendering it as HTML-encoded output.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

<span data-ttu-id="f4da4-131">Değer bir dizeden belirtilen türe dönüştürülebilir ise true döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-131">Returns true if the value can be converted from a string to the specified type.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

<span data-ttu-id="f4da4-132">Nesne veya değişkenin değeri yoksa true döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-132">Returns true if the object or variable has no value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

<span data-ttu-id="f4da4-133">Bir POST isteğiyse true döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-133">Returns true if the request is a POST.</span></span> <span data-ttu-id="f4da4-134">(İlk istekler genellikle bir GET içindir.)</span><span class="sxs-lookup"><span data-stu-id="f4da4-134">(Initial requests are usually a GET.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

<span data-ttu-id="f4da4-135">Bu sayfaya uygulamak için bir düzen sayfasının yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="f4da4-135">Specifies the path of a layout page to apply to this page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

<span data-ttu-id="f4da4-136">Geçerli istekte sayfası, Düzen sayfaları ve kısmi sayfalar arasında paylaşılan veriler içerir.</span><span class="sxs-lookup"><span data-stu-id="f4da4-136">Contains data shared between the page, layout pages, and partial pages in the current request.</span></span> <span data-ttu-id="f4da4-137">Dinamik kullanabileceğiniz `Page` özelliği aşağıdaki örnekteki gibi aynı verilere erişmek için:</span><span class="sxs-lookup"><span data-stu-id="f4da4-137">You can use the dynamic `Page` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

<span data-ttu-id="f4da4-138">(Düzen sayfaları) Adlandırılmış bir bölümde yer almayan bir içerik sayfası içeriğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4da4-138">(Layout pages) Renders the content of a content page that is not in any named sections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

<span data-ttu-id="f4da4-139">Belirtilen yol ve isteğe bağlı ek verileri kullanarak bir içerik sayfasını işler.</span><span class="sxs-lookup"><span data-stu-id="f4da4-139">Renders a content page using the specified path and optional extra data.</span></span> <span data-ttu-id="f4da4-140">Ek parametreler değerlerini alabilirsiniz `PageData` konumu (örnek: 1) veya anahtar (örnek 2).</span><span class="sxs-lookup"><span data-stu-id="f4da4-140">You can get the values of the extra parameters from `PageData` by position (example 1) or key (example 2).</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

<span data-ttu-id="f4da4-141">(Düzen sayfaları) Bir ada sahip bir içerik bölümü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4da4-141">(Layout pages) Renders a content section that has a name.</span></span> <span data-ttu-id="f4da4-142">Ayarlama *gerekli* bir bölümün isteğe bağlı yapmak için false.</span><span class="sxs-lookup"><span data-stu-id="f4da4-142">Set *required* to false to make a section optional.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

<span data-ttu-id="f4da4-143">Alır veya bir HTTP tanımlama bilgisi değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-143">Gets or sets the value of an HTTP cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

<span data-ttu-id="f4da4-144">Geçerli istekte yüklenen dosyaları alır.</span><span class="sxs-lookup"><span data-stu-id="f4da4-144">Gets the files that were uploaded in the current request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

<span data-ttu-id="f4da4-145">Ndaki bir forma (dize olarak) gönderilen verileri alır.</span><span class="sxs-lookup"><span data-stu-id="f4da4-145">Gets data that was posted in a form (as strings).</span></span> <span data-ttu-id="f4da4-146">`Request[key]` her ikisi de denetler `Request.Form` ve `Request.QueryString` koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="f4da4-146">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

<span data-ttu-id="f4da4-147">URL sorgu dizesi belirtildi verilerini alır.</span><span class="sxs-lookup"><span data-stu-id="f4da4-147">Gets data that was specified in the URL query string.</span></span> <span data-ttu-id="f4da4-148">`Request[key]` her ikisi de denetler `Request.Form` ve `Request.QueryString` koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="f4da4-148">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

<span data-ttu-id="f4da4-149">Seçmeli olarak devre dışı bırakır, bir form öğesi, sorgu dizesi değeri, tanımlama bilgisi veya üst bilgi değeri için doğrulama isteyin.</span><span class="sxs-lookup"><span data-stu-id="f4da4-149">Selectively disables request validation for a form element, query-string value, cookie, or header value.</span></span> <span data-ttu-id="f4da4-150">İstek doğrulamanın, varsayılan olarak etkindir ve kullanıcıların biçimlendirme veya diğer potansiyel olarak tehlikeli olabilecek içeriğe nakil engeller.</span><span class="sxs-lookup"><span data-stu-id="f4da4-150">Request validation is enabled by default and prevents users from posting markup or other potentially dangerous content.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

<span data-ttu-id="f4da4-151">Bir HTTP sunucusu üst yanıta ekler.</span><span class="sxs-lookup"><span data-stu-id="f4da4-151">Adds an HTTP server header to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

<span data-ttu-id="f4da4-152">Sayfa çıktısının belirli bir süre boyunca önbelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="f4da4-152">Caches the page output for a specified time.</span></span> <span data-ttu-id="f4da4-153">İsteğe bağlı olarak *kayan* zaman aşımı her sayfa erişimi sıfırlama ve *varyByParams* sayfanın her sayfa isteğinde farklı bir sorgu dizesi için farklı sürümlerini önbelleğe almak için.</span><span class="sxs-lookup"><span data-stu-id="f4da4-153">Optionally set *sliding* to reset the timeout on each page access and *varyByParams* to cache different versions of the page for each different query string in the page request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

<span data-ttu-id="f4da4-154">Tarayıcı isteğini bir konuma yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="f4da4-154">Redirects the browser request to a new location.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

<span data-ttu-id="f4da4-155">Tarayıcıya gönderilen HTTP durum kodunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-155">Sets the HTTP status code sent to the browser.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

<span data-ttu-id="f4da4-156">İçeriğini Yazar *veri* yanıta isteğe bağlı bir MIME türü.</span><span class="sxs-lookup"><span data-stu-id="f4da4-156">Writes the contents of *data* to the response with an optional MIME type.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

<span data-ttu-id="f4da4-157">Bir dosyanın içeriğini yanıta yazar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-157">Writes the contents of a file to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

<span data-ttu-id="f4da4-158">(Düzen sayfaları) Bir ada sahip bir içerik bölümü tanımlar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-158">(Layout pages) Defines a content section that has a name.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

<span data-ttu-id="f4da4-159">HTML ile kodlanmış bir dizenin kodunu çözer.</span><span class="sxs-lookup"><span data-stu-id="f4da4-159">Decodes a string that is HTML encoded.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

<span data-ttu-id="f4da4-160">HTML biçimlendirmede işleme için bir dize kodlar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-160">Encodes a string for rendering in HTML markup.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

<span data-ttu-id="f4da4-161">Belirtilen sanal yol için sunucu fiziksel yolunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-161">Returns the server physical path for the specified virtual path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

<span data-ttu-id="f4da4-162">Bir URL'den metnin kodunu çözer.</span><span class="sxs-lookup"><span data-stu-id="f4da4-162">Decodes text from a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

<span data-ttu-id="f4da4-163">URL'de koymak için metin kodlar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-163">Encodes text to put in a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

<span data-ttu-id="f4da4-164">Alır veya kullanıcının tarayıcıyı kapatmasına kadar var olan bir değer ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-164">Gets or sets a value that exists until the user closes the browser.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

<span data-ttu-id="f4da4-165">Bir nesnenin değerinin dize gösterimini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="f4da4-165">Displays a string representation of the object's value.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

<span data-ttu-id="f4da4-166">Ek veri URL'den alır (örneğin, */sayfa/ExtraData*).</span><span class="sxs-lookup"><span data-stu-id="f4da4-166">Gets additional data from the URL (for example, */MyPage/ExtraData*).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

<span data-ttu-id="f4da4-167">Belirtilen kullanıcının parolasını değiştirir.</span><span class="sxs-lookup"><span data-stu-id="f4da4-167">Changes the password for the specified user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

<span data-ttu-id="f4da4-168">Hesap onayı belirtecini kullanarak bir hesap onaylar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-168">Confirms an account using the account confirmation token.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

<span data-ttu-id="f4da4-169">Belirtilen kullanıcı adı ve parola ile yeni bir kullanıcı hesabı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4da4-169">Creates a new user account with the specified user name and password.</span></span> <span data-ttu-id="f4da4-170">Bir onay belirteci istemek için true geçirin *requireConfirmationToken.*</span><span class="sxs-lookup"><span data-stu-id="f4da4-170">To require a confirmation token, pass true for *requireConfirmationToken.*</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

<span data-ttu-id="f4da4-171">Şu anda oturum açmış kullanıcı için tamsayı tanımlayıcısını alır.</span><span class="sxs-lookup"><span data-stu-id="f4da4-171">Gets the integer identifier for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

<span data-ttu-id="f4da4-172">Şu anda oturum açma kullanıcı adını alır.</span><span class="sxs-lookup"><span data-stu-id="f4da4-172">Gets the name for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

<span data-ttu-id="f4da4-173">E-posta içinde kullanıcıya gönderilebilir ve böylece kullanıcı, parola sıfırlama bir parola sıfırlama simgesi üretir.</span><span class="sxs-lookup"><span data-stu-id="f4da4-173">Generates a password-reset token that can be sent in email to a user so that the user can reset the password.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

<span data-ttu-id="f4da4-174">Kullanıcı kimliği, kullanıcı adını döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-174">Returns the user ID from the user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

<span data-ttu-id="f4da4-175">Geçerli kullanıcının oturum açtıysa true döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-175">Returns true if the current user is logged in.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

<span data-ttu-id="f4da4-176">Kullanıcı (örneğin, bir onay e-posta) onaylanıp true değerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-176">Returns true if the user has been confirmed (for example, through a confirmation email).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

<span data-ttu-id="f4da4-177">Belirtilen kullanıcı adı geçerli kullanıcı adının eşleşmesi durumunda true döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-177">Returns true if the current user's name matches the specified user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

<span data-ttu-id="f4da4-178">Kullanıcı bir kimlik doğrulama belirteci tanımlama bilgisinde ayarlayarak günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="f4da4-178">Logs the user in by setting an authentication token in the cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

<span data-ttu-id="f4da4-179">Kullanıcı, kimlik doğrulama belirteci tanımlama kaldırarak out günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="f4da4-179">Logs the user out by removing the authentication token cookie.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

<span data-ttu-id="f4da4-180">Kullanıcının kimliği doğrulanmazsa HTTP durumunu 401 (yetkisiz) olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-180">If the user is not authenticated, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

<span data-ttu-id="f4da4-181">Geçerli kullanıcı belirtilen rollerden birinin üyesi değilse HTTP durumunu 401 (yetkisiz) olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-181">If the current user is not a member of one of the specified roles, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

<span data-ttu-id="f4da4-182">Geçerli kullanıcı tarafından belirtilen kullanıcı değilse *kullanıcıadı*, HTTP durumunu 401 (yetkisiz) olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-182">If the current user is not the user specified by *username*, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

<span data-ttu-id="f4da4-183">Parola sıfırlama simgesi geçerliyse, yeni parola kullanıcının parolasını değiştirir.</span><span class="sxs-lookup"><span data-stu-id="f4da4-183">If the password reset token is valid, changes the user's password to the new password.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a><span data-ttu-id="f4da4-184">Veri</span><span class="sxs-lookup"><span data-stu-id="f4da4-184">Data</span></span>

### `Database.Execute(SQLstatement [,parameters]`

<span data-ttu-id="f4da4-185">Yürütür *sqldeyimi* (isteğe bağlı parametrelerle) ekleme, silme veya güncelleştirme gibi ve etkilenen kayıtların sayısını döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-185">Executes *SQLstatement* (with optional parameters) such as INSERT, DELETE, or UPDATE and returns a count of affected records.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

<span data-ttu-id="f4da4-186">Kimlik sütunu, en son eklenen satırdan döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-186">Returns the identity column from the most recently inserted row.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

<span data-ttu-id="f4da4-187">Belirtilen veritabanı dosyasını ya da bir adlandırılmış bağlantı dizesini kullanarak belirtilen veritabanı açılır *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="f4da4-187">Opens either the specified database file or the database specified using a named connection string from the *Web.config* file.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

<span data-ttu-id="f4da4-188">Bir veritabanı bağlantı dizesi kullanılarak açılır.</span><span class="sxs-lookup"><span data-stu-id="f4da4-188">Opens a database using the connection string.</span></span> <span data-ttu-id="f4da4-189">(Bu ile karşılaştırır `Database.Open`, bağlantı dizesi adı kullanır.)</span><span class="sxs-lookup"><span data-stu-id="f4da4-189">(This contrasts with `Database.Open`, which uses a connection string name.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

<span data-ttu-id="f4da4-190">Veritabanı kullanan sorgular *sqldeyimi* (isteğe bağlı parametre geçirme) ve sonuçları koleksiyon olarak döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-190">Queries the database using *SQLstatement* (optionally passing parameters) and returns the results as a collection.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

<span data-ttu-id="f4da4-191">Yürütür *sqldeyimi* (isteğe bağlı parametrelerle) ve tek bir kayıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-191">Executes *SQLstatement* (with optional parameters) and returns a single record.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

<span data-ttu-id="f4da4-192">Yürütür *sqldeyimi* (isteğe bağlı parametrelerle) ve tek bir değer döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-192">Executes *SQLstatement* (with optional parameters) and returns a single value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a><span data-ttu-id="f4da4-193">Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="f4da4-193">Helpers</span></span>

### `Analytics.GetGoogleHtml(webPropertyId)`

<span data-ttu-id="f4da4-194">Belirtilen kimlik için Google Analytics JavaScript kodu oluşturur</span><span class="sxs-lookup"><span data-stu-id="f4da4-194">Renders the Google Analytics JavaScript code for the specified ID.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

<span data-ttu-id="f4da4-195">Belirtilen proje StatCounter Analytics JavaScript kodunu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4da4-195">Renders the StatCounter Analytics JavaScript code for the specified project.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

<span data-ttu-id="f4da4-196">Belirtilen hesabın Yahoo Analytics JavaScript kodu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4da4-196">Renders the Yahoo Analytics JavaScript code for the specified account.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

<span data-ttu-id="f4da4-197">Bir arama Bing'e geçirir.</span><span class="sxs-lookup"><span data-stu-id="f4da4-197">Passes a search to Bing.</span></span> <span data-ttu-id="f4da4-198">Arama ve arama kutusu için bir başlık siteye belirtmek için ayarlayabileceğiniz `Bing.SiteUrl` ve `Bing.SiteTitle` özellikleri.</span><span class="sxs-lookup"><span data-stu-id="f4da4-198">To specify the site to search and a title for the search box, you can set the `Bing.SiteUrl` and `Bing.SiteTitle` properties.</span></span> <span data-ttu-id="f4da4-199">Normalde bu özellikler kümesinde  *\_AppStart* sayfası.</span><span class="sxs-lookup"><span data-stu-id="f4da4-199">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

<span data-ttu-id="f4da4-200">Bir grafik başlatır.</span><span class="sxs-lookup"><span data-stu-id="f4da4-200">Initializes a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

<span data-ttu-id="f4da4-201">Grafiğe bir açıklama ekler.</span><span class="sxs-lookup"><span data-stu-id="f4da4-201">Adds a legend to a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

<span data-ttu-id="f4da4-202">Grafiğe bir dizi değer ekler.</span><span class="sxs-lookup"><span data-stu-id="f4da4-202">Adds a series of values to the chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

<span data-ttu-id="f4da4-203">Belirtilen veriler için bir karma değer döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-203">Returns a hash for the specified data.</span></span> <span data-ttu-id="f4da4-204">Varsayılan algoritmadır `sha256`.</span><span class="sxs-lookup"><span data-stu-id="f4da4-204">The default algorithm is `sha256`.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

<span data-ttu-id="f4da4-205">Facebook kullanıcıların sayfalarına bağlantı sağlar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-205">Lets Facebook users make a connection to pages.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

<span data-ttu-id="f4da4-206">Dosyaları karşıya yükleme için kullanıcı Arabirimi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4da4-206">Renders UI for uploading files.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

<span data-ttu-id="f4da4-207">Belirtilen Xbox oyuncu etiketi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4da4-207">Renders the specified Xbox gamer tag.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

<span data-ttu-id="f4da4-208">Belirtilen e-posta adresi için Gravatar görüntü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4da4-208">Renders the Gravatar image for the specified email address.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

<span data-ttu-id="f4da4-209">Bir veri nesnesini JavaScript nesne gösterimi (JSON) biçiminde bir dizeye dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-209">Converts a data object to a string in the JavaScript Object Notation (JSON) format.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

<span data-ttu-id="f4da4-210">JSON olarak kodlanmış giriş dizesi, üzerinden yineleme yapma veya bir veritabanına ekleme veri nesnesine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-210">Converts a JSON-encoded input string to a data object that you can iterate over or insert into a database.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

<span data-ttu-id="f4da4-211">Belirtilen başlık ve isteğe bağlı bir URL kullanarak sosyal ağ bağlantıları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4da4-211">Renders social networking links using the specified title and optional URL.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

<span data-ttu-id="f4da4-212">Bir hata iletisi, bir form alanıyla ilişkilendirir.</span><span class="sxs-lookup"><span data-stu-id="f4da4-212">Associates an error message with a form field.</span></span> <span data-ttu-id="f4da4-213">Kullanım `ModelState` bu üyeye erişmek için yardımcı.</span><span class="sxs-lookup"><span data-stu-id="f4da4-213">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

<span data-ttu-id="f4da4-214">Bir hata iletisi, bir form ile ilişkilendirir.</span><span class="sxs-lookup"><span data-stu-id="f4da4-214">Associates an error message with a form.</span></span> <span data-ttu-id="f4da4-215">Kullanım `ModelState` bu üyeye erişmek için yardımcı.</span><span class="sxs-lookup"><span data-stu-id="f4da4-215">Use the `ModelState` helper to access this member.</span></span>

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

<span data-ttu-id="f4da4-216">Doğrulama hataları varsa true değerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-216">Returns true if there are no validation errors.</span></span> <span data-ttu-id="f4da4-217">Kullanım `ModelState` bu üyeye erişmek için yardımcı.</span><span class="sxs-lookup"><span data-stu-id="f4da4-217">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

<span data-ttu-id="f4da4-218">Özelliklerini ve değerlerini bir nesne ve tüm alt nesneleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4da4-218">Renders the properties and values of an object and any child objects.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

<span data-ttu-id="f4da4-219">ReCAPTCHA doğrulama testinden işler.</span><span class="sxs-lookup"><span data-stu-id="f4da4-219">Renders the reCAPTCHA verification test.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

<span data-ttu-id="f4da4-220">Ortak ve özel anahtarlar reCAPTCHA hizmeti için ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-220">Sets public and private keys for the reCAPTCHA service.</span></span> <span data-ttu-id="f4da4-221">Normalde bu özellikler kümesinde  *\_AppStart* sayfası.</span><span class="sxs-lookup"><span data-stu-id="f4da4-221">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

<span data-ttu-id="f4da4-222">ReCAPTCHA test sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-222">Returns the result of the reCAPTCHA test.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

<span data-ttu-id="f4da4-223">ASP.NET Web sayfaları hakkında durum bilgileri işler.</span><span class="sxs-lookup"><span data-stu-id="f4da4-223">Renders status information about ASP.NET Web Pages.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

<span data-ttu-id="f4da4-224">Belirtilen kullanıcı için bir Twitter akışı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4da4-224">Renders a Twitter stream for the specified user.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

<span data-ttu-id="f4da4-225">Belirtilen arama metnini için bir Twitter akışı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4da4-225">Renders a Twitter stream for the specified search text.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

<span data-ttu-id="f4da4-226">İsteğe bağlı bir genişlik ve yükseklik ile belirtilen dosyayı bir Flash video oynatıcı işler.</span><span class="sxs-lookup"><span data-stu-id="f4da4-226">Renders a Flash video player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

<span data-ttu-id="f4da4-227">İsteğe bağlı bir genişlik ve yükseklik ile belirtilen dosya için bir Windows Media player yapar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-227">Renders a Windows Media player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

<span data-ttu-id="f4da4-228">Belirtilen bir Silverlight player işler *.xap* dosyasıyla gerekli genişlik ve yükseklik.</span><span class="sxs-lookup"><span data-stu-id="f4da4-228">Renders a Silverlight player for the specified *.xap* file with required width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

<span data-ttu-id="f4da4-229">Belirtilen nesneyi döndürür *anahtarı*, ya da nesne bulunamazsa null.</span><span class="sxs-lookup"><span data-stu-id="f4da4-229">Returns the object specified by *key*, or null if the object is not found.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

<span data-ttu-id="f4da4-230">Tarafından belirtilen nesnede kaldırır *anahtar* önbellekten.</span><span class="sxs-lookup"><span data-stu-id="f4da4-230">Removes the object specified by *key* from the cache.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

<span data-ttu-id="f4da4-231">Koyar *değer* tarafından belirtilen adla önbelleğine *anahtar*.</span><span class="sxs-lookup"><span data-stu-id="f4da4-231">Puts *value* into the cache under the name specified by *key*.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

<span data-ttu-id="f4da4-232">Yeni bir oluşturur `WebGrid` kullanarak bir sorgudan veri nesnesi.</span><span class="sxs-lookup"><span data-stu-id="f4da4-232">Creates a new `WebGrid` object using data from a query.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

<span data-ttu-id="f4da4-233">HTML tablosu halinde verileri görüntülemek için biçimlendirme oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4da4-233">Renders markup to display data in an HTML table.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

<span data-ttu-id="f4da4-234">İşler için bir çağrı `WebGrid` nesne.</span><span class="sxs-lookup"><span data-stu-id="f4da4-234">Renders a pager for the `WebGrid` object.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

<span data-ttu-id="f4da4-235">Belirtilen yoldan görüntüyü yükler.</span><span class="sxs-lookup"><span data-stu-id="f4da4-235">Loads an image from the specified path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

<span data-ttu-id="f4da4-236">Belirtilen görüntü filigran olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="f4da4-236">Adds the specified image as a watermark.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

<span data-ttu-id="f4da4-237">Belirtilen metin görüntüye ekler.</span><span class="sxs-lookup"><span data-stu-id="f4da4-237">Adds the specified text to the image.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

<span data-ttu-id="f4da4-238">Görüntüyü yatay veya dikey çevirir.</span><span class="sxs-lookup"><span data-stu-id="f4da4-238">Flips the image horizontally or vertically.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

<span data-ttu-id="f4da4-239">Görüntüyü bir dosyayı karşıya yükleme sırasında bir sayfa gönderildiğinde, görüntüyü yükler.</span><span class="sxs-lookup"><span data-stu-id="f4da4-239">Loads an image when an image is posted to a page during a file upload.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

<span data-ttu-id="f4da4-240">Yeniden boyutlandırır bir görüntüsü.</span><span class="sxs-lookup"><span data-stu-id="f4da4-240">Resizes an the image.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

<span data-ttu-id="f4da4-241">Resmi sağa veya sola döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-241">Rotates the image to the left or the right.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

<span data-ttu-id="f4da4-242">Belirtilen yola resmi kaydeder.</span><span class="sxs-lookup"><span data-stu-id="f4da4-242">Saves the image to the specified path.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

<span data-ttu-id="f4da4-243">SMTP sunucusu için parolayı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-243">Sets the password for the SMTP server.</span></span> <span data-ttu-id="f4da4-244">Bu özellik normalde ayarladığınız  *\_AppStart* sayfası.</span><span class="sxs-lookup"><span data-stu-id="f4da4-244">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

<span data-ttu-id="f4da4-245">Bir e-posta iletisi gönderir.</span><span class="sxs-lookup"><span data-stu-id="f4da4-245">Sends an email message.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

<span data-ttu-id="f4da4-246">SMTP sunucusu adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-246">Sets the SMTP server name.</span></span> <span data-ttu-id="f4da4-247">Bu özellik normalde ayarladığınız<em>\_AppStart</em> sayfası.</span><span class="sxs-lookup"><span data-stu-id="f4da4-247">Normally you set this property in the<em>\_AppStart</em> page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

<span data-ttu-id="f4da4-248">SMTP sunucusu için kullanıcı adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f4da4-248">Sets the user name for the SMTP server.</span></span> <span data-ttu-id="f4da4-249">Bu özellik normalde ayarlamanız gerekir  *\_AppStart* sayfası.</span><span class="sxs-lookup"><span data-stu-id="f4da4-249">Normally you should set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a><span data-ttu-id="f4da4-250">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="f4da4-250">Validation</span></span>

### `Html.ValidationMessage(field)`

<span data-ttu-id="f4da4-251">(v2) Belirtilen alan için bir doğrulama hatası iletisini işler.</span><span class="sxs-lookup"><span data-stu-id="f4da4-251">(v2) Renders a validation error message for the specified field.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

<span data-ttu-id="f4da4-252">(v2) Tüm doğrulama hatalarının listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="f4da4-252">(v2) Displays a list of all validation errors.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

<span data-ttu-id="f4da4-253">(v2) Belirtilen doğrulama türü için bir kullanıcı girişi öğesini kaydeder.</span><span class="sxs-lookup"><span data-stu-id="f4da4-253">(v2) Registers a user input element for the specified type of validation.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

<span data-ttu-id="f4da4-254">(v2) Doğrulama hatası iletilerini biçimlendirmek için istemci tarafı doğrulama CSS sınıfı öznitelikleri dinamik olarak işler.</span><span class="sxs-lookup"><span data-stu-id="f4da4-254">(v2) Dynamically renders CSS class attributes for client-side validation so that you can format validation error messages.</span></span> <span data-ttu-id="f4da4-255">(Uygun istemci-komut dosyası Kitaplığı Başvurusu ve CSS sınıfları tanımlama gerektirir.)</span><span class="sxs-lookup"><span data-stu-id="f4da4-255">(Requires that you reference the appropriate client-script libraries and that you define CSS classes.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

<span data-ttu-id="f4da4-256">(v2) Kullanıcı giriş alanı için istemci tarafı doğrulamasını etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="f4da4-256">(v2) Enables client-side validation for the user input field.</span></span> <span data-ttu-id="f4da4-257">(Uygun istemci-komut dosyası kitaplığı başvurusu gerektirir.)</span><span class="sxs-lookup"><span data-stu-id="f4da4-257">(Requires that you reference the appropriate client-script libraries.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

<span data-ttu-id="f4da4-258">(v2) Doğrulama için registred olan tüm kullanıcı girişi öğelerinin geçerli değerlerini içeriyorsa true döndürür.</span><span class="sxs-lookup"><span data-stu-id="f4da4-258">(v2) Returns true if all user input elements that are registred for validation contain valid values.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

<span data-ttu-id="f4da4-259">(v2) Kullanıcıların kullanıcı girişi öğesi için bir değer belirtmeniz gerekir belirtir.</span><span class="sxs-lookup"><span data-stu-id="f4da4-259">(v2) Specifies that users must provide a value for the user input element.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

<span data-ttu-id="f4da4-260">(v2) Kullanıcılar değerler her kullanıcı girişi öğelerinin sağlamanız gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="f4da4-260">(v2) Specifies that users must provide values for each of the user input elements.</span></span> <span data-ttu-id="f4da4-261">Bu yöntem bir özel hata iletisi belirtmenize izin vermez.</span><span class="sxs-lookup"><span data-stu-id="f4da4-261">This method does not let you specify a custom error message.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

<span data-ttu-id="f4da4-262">(v2) Kullanırken bir doğrulama testi belirtir `Validation.Add` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f4da4-262">(v2) Specifies a validation test when you use the `Validation.Add` method.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
