---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: "ASP.NET Web sayfaları (Razor) API hızlı başvuru | Microsoft Docs"
author: tfitzmac
description: "Bu sayfanın en yaygın olarak kullanılan nesneler, özellikleri ve yöntemleri Razor sözdizimi ile ASP.NET Web sayfaları programlama için kısa örnekleri içeren bir listesini içerir."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 35f91f4dbea4881d9dabc4ab7c6b96dbb6a01ea2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a><span data-ttu-id="9e3a8-103">ASP.NET Web sayfaları (Razor) API hızlı başvuru</span><span class="sxs-lookup"><span data-stu-id="9e3a8-103">ASP.NET Web Pages (Razor) API Quick Reference</span></span>
====================
<span data-ttu-id="9e3a8-104">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9e3a8-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="9e3a8-105">Bu sayfanın en yaygın olarak kullanılan nesneler, özellikleri ve yöntemleri Razor sözdizimi ile ASP.NET Web sayfaları programlama için kısa örnekleri içeren bir listesini içerir.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-105">This page contains a list with brief examples of the most commonly used objects, properties, and methods for programming ASP.NET Web Pages with Razor syntax.</span></span>
> 
> <span data-ttu-id="9e3a8-106">ASP.NET Web sayfaları sürüm 2 "(v2)" ile işaretlenmiş açıklamaları tanıtıldı.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-106">Descriptions marked with "(v2)" were introduced in ASP.NET Web Pages version 2.</span></span>
> 
> <span data-ttu-id="9e3a8-107">API başvuru belgeleri için bkz: [ASP.NET Web sayfaları başvuru belgeleri](https://go.microsoft.com/fwlink/?LinkId=208659) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-107">For API reference documentation, see the [ASP.NET Web Pages Reference Documentation](https://go.microsoft.com/fwlink/?LinkId=208659) on MSDN.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="9e3a8-108">Yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="9e3a8-108">Software versions</span></span>
> 
> 
> - <span data-ttu-id="9e3a8-109">ASP.NET Web sayfaları (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="9e3a8-109">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="9e3a8-110">Bu öğretici, (v2 işaretlenmiş özellikleri dışında) ASP.NET Web Pages 2 ve ASP.NET Web sayfaları 1.0 ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-110">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0 (except for features marked v2).</span></span>


<span data-ttu-id="9e3a8-111">Bu sayfa aşağıdakiler için başvuru bilgileri içerir:</span><span class="sxs-lookup"><span data-stu-id="9e3a8-111">This page contains reference information for the following:</span></span>

- [<span data-ttu-id="9e3a8-112">Sınıfları</span><span class="sxs-lookup"><span data-stu-id="9e3a8-112">Classes</span></span>](#Classes)
- [<span data-ttu-id="9e3a8-113">Veri</span><span class="sxs-lookup"><span data-stu-id="9e3a8-113">Data</span></span>](#Data)
- [<span data-ttu-id="9e3a8-114">Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="9e3a8-114">Helpers</span></span>](#Helpers)
- [<span data-ttu-id="9e3a8-115">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="9e3a8-115">Validation</span></span>](#Validation)

<a id="Classes"></a>
## <a name="classes"></a><span data-ttu-id="9e3a8-116">Sınıflar</span><span class="sxs-lookup"><span data-stu-id="9e3a8-116">Classes</span></span>

### `AppState[key], AppState[index],App`

<span data-ttu-id="9e3a8-117">Uygulamadaki tüm sayfalar tarafından paylaşılabilir veri içeriyor.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-117">Contains data that can be shared by any pages in the application.</span></span> <span data-ttu-id="9e3a8-118">Dinamik kullanabilirsiniz `App` özelliği aşağıdaki örnekte olduğu gibi aynı verilere erişmek için:</span><span class="sxs-lookup"><span data-stu-id="9e3a8-118">You can use the dynamic `App` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

<span data-ttu-id="9e3a8-119">Bir dize değeri bir Boole (true/false) değerine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-119">Converts a string value to a Boolean value (true/false).</span></span> <span data-ttu-id="9e3a8-120">Yanlış değerini döndürür veya dize true/false göstermiyor olduğunda belirtilen değeri.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-120">Returns false or the specified value if the string does not represent true/false.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

<span data-ttu-id="9e3a8-121">Tarih/saat için bir dize değerini dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-121">Converts a string value to date/time.</span></span> <span data-ttu-id="9e3a8-122">Döndürür `DateTime.MinValue` veya dize bir tarih/saat göstermiyor olduğunda belirtilen değeri.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-122">Returns `DateTime.MinValue` or the specified value if the string does not represent a date/time.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

<span data-ttu-id="9e3a8-123">Bir dize değeri bir ondalık değerine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-123">Converts a string value to a decimal value.</span></span> <span data-ttu-id="9e3a8-124">0,0 döndürür veya belirtilen değer dizesi ondalık bir değeri temsil etmiyor varsa.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-124">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

<span data-ttu-id="9e3a8-125">Bir dize değeri kayana dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-125">Converts a string value to a float.</span></span> <span data-ttu-id="9e3a8-126">0,0 döndürür veya belirtilen değer dizesi ondalık bir değeri temsil etmiyor varsa.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-126">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

<span data-ttu-id="9e3a8-127">Bir dize değeri bir tamsayıya dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-127">Converts a string value to an integer.</span></span> <span data-ttu-id="9e3a8-128">0 döndürür veya dize, tamsayı göstermiyor olduğunda belirtilen değeri.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-128">Returns 0 or the specified value if the string does not represent an integer.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

<span data-ttu-id="9e3a8-129">İsteğe bağlı ek yol bölümleri içeren bir yerel dosya yolu, bir tarayıcı uyumlu URL oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-129">Creates a browser-compatible URL from a local file path, with optional additional path parts.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

<span data-ttu-id="9e3a8-130">İşler *değeri* olarak HTML ile kodlanmış çıkış işleme yerine bir HTML biçimlendirmesi olarak.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-130">Renders *value* as HTML markup instead of rendering it as HTML-encoded output.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

<span data-ttu-id="9e3a8-131">Değer bir dizeden belirtilen türe dönüştürülebilir ise true döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-131">Returns true if the value can be converted from a string to the specified type.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

<span data-ttu-id="9e3a8-132">Nesne veya değişken değer yoksa true döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-132">Returns true if the object or variable has no value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

<span data-ttu-id="9e3a8-133">Bir POST isteğiyse true döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-133">Returns true if the request is a POST.</span></span> <span data-ttu-id="9e3a8-134">(İlk genellikle bir GET isteklerdir.)</span><span class="sxs-lookup"><span data-stu-id="9e3a8-134">(Initial requests are usually a GET.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

<span data-ttu-id="9e3a8-135">Bu sayfaya uygulamak için bir düzen sayfasının yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-135">Specifies the path of a layout page to apply to this page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

<span data-ttu-id="9e3a8-136">Geçerli istekte sayfası, Düzen sayfaları ve kısmi sayfalar arasında paylaşılan veriler içerir.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-136">Contains data shared between the page, layout pages, and partial pages in the current request.</span></span> <span data-ttu-id="9e3a8-137">Dinamik kullanabilirsiniz `Page` özelliği aşağıdaki örnekte olduğu gibi aynı verilere erişmek için:</span><span class="sxs-lookup"><span data-stu-id="9e3a8-137">You can use the dynamic `Page` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

<span data-ttu-id="9e3a8-138">(Düzen sayfaları) Adlandırılmış tüm bölümlerde değil bir içerik sayfasını içeriğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-138">(Layout pages) Renders the content of a content page that is not in any named sections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

<span data-ttu-id="9e3a8-139">Belirtilen yol ve isteğe bağlı ek verileri kullanarak bir içerik sayfasını işler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-139">Renders a content page using the specified path and optional extra data.</span></span> <span data-ttu-id="9e3a8-140">Ek parametreler değerlerini alabilir `PageData` konumlarına göre (örnek: 1) veya anahtar (örnek: 2).</span><span class="sxs-lookup"><span data-stu-id="9e3a8-140">You can get the values of the extra parameters from `PageData` by position (example 1) or key (example 2).</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

<span data-ttu-id="9e3a8-141">(Düzen sayfaları) Bir ada sahip bir içerik bölümü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-141">(Layout pages) Renders a content section that has a name.</span></span> <span data-ttu-id="9e3a8-142">Ayarlama *gerekli* bir bölümün isteğe bağlı yapmak için false.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-142">Set *required* to false to make a section optional.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

<span data-ttu-id="9e3a8-143">Alır veya bir HTTP tanımlama bilgisi değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-143">Gets or sets the value of an HTTP cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

<span data-ttu-id="9e3a8-144">Geçerli istekte yüklenen dosyaları alır.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-144">Gets the files that were uploaded in the current request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

<span data-ttu-id="9e3a8-145">Bir formda (dize olarak) gönderilen verileri alır.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-145">Gets data that was posted in a form (as strings).</span></span> <span data-ttu-id="9e3a8-146">`Request[key]`her ikisi de denetler `Request.Form` ve `Request.QueryString` koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-146">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

<span data-ttu-id="9e3a8-147">URL sorgu dizesinde belirtilen veri alır.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-147">Gets data that was specified in the URL query string.</span></span> <span data-ttu-id="9e3a8-148">`Request[key]`her ikisi de denetler `Request.Form` ve `Request.QueryString` koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-148">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

<span data-ttu-id="9e3a8-149">Seçmeli olarak devre dışı bırakır form öğesi, sorgu dizesi değeri, tanımlama bilgisi veya üstbilgi değeri doğrulamasını isteyin.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-149">Selectively disables request validation for a form element, query-string value, cookie, or header value.</span></span> <span data-ttu-id="9e3a8-150">İstek doğrulama varsayılan olarak etkindir ve kullanıcıların işaretleme veya diğer potansiyel olarak tehlikeli olabilecek içeriğe nakil engeller.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-150">Request validation is enabled by default and prevents users from posting markup or other potentially dangerous content.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

<span data-ttu-id="9e3a8-151">Bir HTTP sunucu üst bilgisi yanıta ekler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-151">Adds an HTTP server header to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

<span data-ttu-id="9e3a8-152">Sayfa çıktısının belirli bir süre boyunca önbelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-152">Caches the page output for a specified time.</span></span> <span data-ttu-id="9e3a8-153">İsteğe bağlı olarak ayarlama *kayan* her sayfası erişimi aşımında sıfırlanır ve *varyByParams* sayfa sayfa isteğindeki her farklı sorgu dizesi için farklı sürümlerini önbelleğe alacak şekilde.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-153">Optionally set *sliding* to reset the timeout on each page access and *varyByParams* to cache different versions of the page for each different query string in the page request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

<span data-ttu-id="9e3a8-154">Tarayıcı isteğini bir konuma yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-154">Redirects the browser request to a new location.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

<span data-ttu-id="9e3a8-155">Tarayıcıya gönderilen HTTP durum kodunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-155">Sets the HTTP status code sent to the browser.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

<span data-ttu-id="9e3a8-156">İçeriğini Yazar *veri* yanıtına bir isteğe bağlı bir MIME türüne sahip.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-156">Writes the contents of *data* to the response with an optional MIME type.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

<span data-ttu-id="9e3a8-157">Bir dosyanın içeriğini yanıta yazar.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-157">Writes the contents of a file to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

<span data-ttu-id="9e3a8-158">(Düzen sayfaları) Bir ada sahip bir içerik bölümü tanımlar.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-158">(Layout pages) Defines a content section that has a name.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

<span data-ttu-id="9e3a8-159">HTML kodlu bir dize kodunu çözer.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-159">Decodes a string that is HTML encoded.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

<span data-ttu-id="9e3a8-160">HTML biçimlendirmede işleme için bir dize olarak kodlar.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-160">Encodes a string for rendering in HTML markup.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

<span data-ttu-id="9e3a8-161">Belirtilen sanal yol için sunucu fiziksel yolunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-161">Returns the server physical path for the specified virtual path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

<span data-ttu-id="9e3a8-162">Bir URL'den metnin kodunu çözer.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-162">Decodes text from a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

<span data-ttu-id="9e3a8-163">Bir URL'de yerleştirilecek metin kodlar.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-163">Encodes text to put in a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

<span data-ttu-id="9e3a8-164">Alır veya kullanıcı tarayıcıyı kapatıncaya kadar var olan bir değer ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-164">Gets or sets a value that exists until the user closes the browser.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

<span data-ttu-id="9e3a8-165">Nesnenin değeri bir dize gösterimini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-165">Displays a string representation of the object's value.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

<span data-ttu-id="9e3a8-166">Ek veri URL'den alır (örneğin, */sayfa/ExtraData*).</span><span class="sxs-lookup"><span data-stu-id="9e3a8-166">Gets additional data from the URL (for example, */MyPage/ExtraData*).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

<span data-ttu-id="9e3a8-167">Belirtilen kullanıcının parolasını değiştirir.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-167">Changes the password for the specified user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

<span data-ttu-id="9e3a8-168">Hesap onay belirteci kullanarak bir hesap onaylar.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-168">Confirms an account using the account confirmation token.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

<span data-ttu-id="9e3a8-169">Belirtilen kullanıcı adı ve parola ile yeni bir kullanıcı hesabı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-169">Creates a new user account with the specified user name and password.</span></span> <span data-ttu-id="9e3a8-170">Bir onay belirteci gerektirecek şekilde true geçirmek *requireConfirmationToken.*</span><span class="sxs-lookup"><span data-stu-id="9e3a8-170">To require a confirmation token, pass true for *requireConfirmationToken.*</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

<span data-ttu-id="9e3a8-171">Şu anda oturum açmış kullanıcının için tamsayı tanımlayıcısını alır.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-171">Gets the integer identifier for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

<span data-ttu-id="9e3a8-172">Şu anda oturum açma kullanıcı adını alır.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-172">Gets the name for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

<span data-ttu-id="9e3a8-173">Bir kullanıcıya e-posta ile gönderilebilir ve böylece kullanıcının parolasını sıfırlamak bir parola sıfırlama belirteci oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-173">Generates a password-reset token that can be sent in email to a user so that the user can reset the password.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

<span data-ttu-id="9e3a8-174">Kullanıcı kimliği, kullanıcı adını döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-174">Returns the user ID from the user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

<span data-ttu-id="9e3a8-175">Geçerli kullanıcının oturum açtığı true değerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-175">Returns true if the current user is logged in.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

<span data-ttu-id="9e3a8-176">Kullanıcı (örneğin, bir onay e-posta yoluyla) onaylandıktan true değerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-176">Returns true if the user has been confirmed (for example, through a confirmation email).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

<span data-ttu-id="9e3a8-177">Belirtilen kullanıcı adı geçerli kullanıcının adıyla eşleşen varsa true değerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-177">Returns true if the current user's name matches the specified user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

<span data-ttu-id="9e3a8-178">Kullanıcı kimlik doğrulama belirtecini tanımlama bilgisinde ayarlayarak günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-178">Logs the user in by setting an authentication token in the cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

<span data-ttu-id="9e3a8-179">Kullanıcı, kimlik doğrulama belirteci tanımlama kaldırarak out günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-179">Logs the user out by removing the authentication token cookie.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

<span data-ttu-id="9e3a8-180">Kullanıcının kimliği doğrulanmazsa, HTTP durumunu 401 (yetkisiz) olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-180">If the user is not authenticated, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

<span data-ttu-id="9e3a8-181">Geçerli kullanıcı belirtilen rollerin üyesi değilse, HTTP durumunu 401 (yetkisiz) olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-181">If the current user is not a member of one of the specified roles, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

<span data-ttu-id="9e3a8-182">Geçerli kullanıcı tarafından belirtilen kullanıcı değilse, *kullanıcıadı*, HTTP durumunu 401 (yetkisiz) olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-182">If the current user is not the user specified by *username*, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

<span data-ttu-id="9e3a8-183">Parola sıfırlama simgesi geçerliyse, yeni parola kullanıcının parolasını değiştirir.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-183">If the password reset token is valid, changes the user's password to the new password.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a><span data-ttu-id="9e3a8-184">Veri</span><span class="sxs-lookup"><span data-stu-id="9e3a8-184">Data</span></span>

### `Database.Execute(SQLstatement [,parameters]`

<span data-ttu-id="9e3a8-185">Yürütür *SQLstatement* (isteğe bağlı parametrelerle) ekleme, silme veya güncelleştirme gibi ve etkilenen kayıtların sayısını döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-185">Executes *SQLstatement* (with optional parameters) such as INSERT, DELETE, or UPDATE and returns a count of affected records.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

<span data-ttu-id="9e3a8-186">Kimlik sütunu, en son eklenen satırın döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-186">Returns the identity column from the most recently inserted row.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

<span data-ttu-id="9e3a8-187">Belirtilen veritabanı dosyası veya bir adlandırılmış bağlantı dizesi kullanılarak belirtilen veritabanı açılır *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-187">Opens either the specified database file or the database specified using a named connection string from the *Web.config* file.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

<span data-ttu-id="9e3a8-188">Bağlantı dizesi kullanarak bir veritabanı açılır.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-188">Opens a database using the connection string.</span></span> <span data-ttu-id="9e3a8-189">(Bu ile karşılaştırır `Database.Open`, bir bağlantı dizesi adı kullanır.)</span><span class="sxs-lookup"><span data-stu-id="9e3a8-189">(This contrasts with `Database.Open`, which uses a connection string name.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

<span data-ttu-id="9e3a8-190">Kullanarak veritabanını sorgular *SQLstatement* (isteğe bağlı parametreleri geçirme) ve sonuçları bir koleksiyon olarak döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-190">Queries the database using *SQLstatement* (optionally passing parameters) and returns the results as a collection.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

<span data-ttu-id="9e3a8-191">Yürütür *SQLstatement* (isteğe bağlı parametrelerle) ve tek bir kayıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-191">Executes *SQLstatement* (with optional parameters) and returns a single record.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

<span data-ttu-id="9e3a8-192">Yürütür *SQLstatement* (isteğe bağlı parametrelerle) ve tek bir değer döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-192">Executes *SQLstatement* (with optional parameters) and returns a single value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a><span data-ttu-id="9e3a8-193">Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="9e3a8-193">Helpers</span></span>

### `Analytics.GetGoogleHtml(webPropertyId)`

<span data-ttu-id="9e3a8-194">Google Analytics JavaScript kodunu belirtilen kimliği için işler</span><span class="sxs-lookup"><span data-stu-id="9e3a8-194">Renders the Google Analytics JavaScript code for the specified ID.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

<span data-ttu-id="9e3a8-195">Belirtilen proje StatCounter Analytics JavaScript kodunu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-195">Renders the StatCounter Analytics JavaScript code for the specified project.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

<span data-ttu-id="9e3a8-196">Belirtilen hesap Yahoo Analytics JavaScript kodunu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-196">Renders the Yahoo Analytics JavaScript code for the specified account.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

<span data-ttu-id="9e3a8-197">Bir arama için Bing geçirir.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-197">Passes a search to Bing.</span></span> <span data-ttu-id="9e3a8-198">Arama ve arama kutusu için bir başlık siteye belirtmek için ayarlayabileceğiniz `Bing.SiteUrl` ve `Bing.SiteTitle` özellikleri.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-198">To specify the site to search and a title for the search box, you can set the `Bing.SiteUrl` and `Bing.SiteTitle` properties.</span></span> <span data-ttu-id="9e3a8-199">Normalde bu özellikler kümesinde  *\_AppStart* sayfası.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-199">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

<span data-ttu-id="9e3a8-200">Bir grafik başlatır.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-200">Initializes a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

<span data-ttu-id="9e3a8-201">Grafiğe bir gösterge ekler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-201">Adds a legend to a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

<span data-ttu-id="9e3a8-202">Değerleri bir dizi grafiğe ekler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-202">Adds a series of values to the chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

<span data-ttu-id="9e3a8-203">Belirtilen veriler için bir karma değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-203">Returns a hash for the specified data.</span></span> <span data-ttu-id="9e3a8-204">Varsayılan algoritmasıdır `sha256`.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-204">The default algorithm is `sha256`.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

<span data-ttu-id="9e3a8-205">Facebook kullanıcıların sayfaları bağlantısı sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-205">Lets Facebook users make a connection to pages.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

<span data-ttu-id="9e3a8-206">Dosyaları karşıya yükleme için kullanıcı Arabirimi işler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-206">Renders UI for uploading files.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

<span data-ttu-id="9e3a8-207">Belirtilen Xbox oyuncu etiketi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-207">Renders the specified Xbox gamer tag.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

<span data-ttu-id="9e3a8-208">Belirtilen e-posta adresi için Gravatar görüntüsü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-208">Renders the Gravatar image for the specified email address.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

<span data-ttu-id="9e3a8-209">Bir veri nesnesini JavaScript nesne gösterimi (JSON) biçiminde bir dizeye dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-209">Converts a data object to a string in the JavaScript Object Notation (JSON) format.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

<span data-ttu-id="9e3a8-210">JSON olarak kodlanmış giriş dizesi üzerinden yineleme ya da bir veritabanına ekleme bir veri nesnesine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-210">Converts a JSON-encoded input string to a data object that you can iterate over or insert into a database.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

<span data-ttu-id="9e3a8-211">Belirtilen başlık ve isteğe bağlı URL'yi kullanarak sosyal ağ bağlantılarını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-211">Renders social networking links using the specified title and optional URL.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

<span data-ttu-id="9e3a8-212">Bir hata iletisi, bir form alanıyla ilişkilendirir.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-212">Associates an error message with a form field.</span></span> <span data-ttu-id="9e3a8-213">Kullanım `ModelState` bu üye erişmek için yardımcı.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-213">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

<span data-ttu-id="9e3a8-214">Bir hata iletisi, bir form ile ilişkilendirir.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-214">Associates an error message with a form.</span></span> <span data-ttu-id="9e3a8-215">Kullanım `ModelState` bu üye erişmek için yardımcı.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-215">Use the `ModelState` helper to access this member.</span></span>

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

<span data-ttu-id="9e3a8-216">Doğrulama hataları varsa true değerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-216">Returns true if there are no validation errors.</span></span> <span data-ttu-id="9e3a8-217">Kullanım `ModelState` bu üye erişmek için yardımcı.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-217">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

<span data-ttu-id="9e3a8-218">Özellikleri ve değerleri, bir nesne ve tüm alt nesneleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-218">Renders the properties and values of an object and any child objects.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

<span data-ttu-id="9e3a8-219">ReCAPTCHA doğrulama testi işler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-219">Renders the reCAPTCHA verification test.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

<span data-ttu-id="9e3a8-220">Ortak ve özel anahtarlar reCAPTCHA hizmeti için ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-220">Sets public and private keys for the reCAPTCHA service.</span></span> <span data-ttu-id="9e3a8-221">Normalde bu özellikler kümesinde  *\_AppStart* sayfası.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-221">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

<span data-ttu-id="9e3a8-222">ReCAPTCHA test sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-222">Returns the result of the reCAPTCHA test.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

<span data-ttu-id="9e3a8-223">ASP.NET Web sayfaları hakkında durum bilgilerini işler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-223">Renders status information about ASP.NET Web Pages.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

<span data-ttu-id="9e3a8-224">Belirtilen kullanıcı için bir Twitter akışı işler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-224">Renders a Twitter stream for the specified user.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

<span data-ttu-id="9e3a8-225">Belirtilen arama metnini bir Twitter akışı işler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-225">Renders a Twitter stream for the specified search text.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

<span data-ttu-id="9e3a8-226">İsteğe bağlı genişliği ve yüksekliği ile belirtilen dosyayı bir Flash video oynatıcı işler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-226">Renders a Flash video player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

<span data-ttu-id="9e3a8-227">İsteğe bağlı genişliği ve yüksekliği ile belirtilen dosya için bir Windows Media player işler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-227">Renders a Windows Media player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

<span data-ttu-id="9e3a8-228">Silverlight player için belirtilen işler *.xap* dosyasıyla gerekli genişlik ve yükseklik.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-228">Renders a Silverlight player for the specified *.xap* file with required width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

<span data-ttu-id="9e3a8-229">Tarafından belirtilen nesnesi döndüren *anahtar*, veya nesne bulunmazsa null.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-229">Returns the object specified by *key*, or null if the object is not found.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

<span data-ttu-id="9e3a8-230">Belirtilen nesneyi kaldırır *anahtar* önbellekten.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-230">Removes the object specified by *key* from the cache.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

<span data-ttu-id="9e3a8-231">Koyar *değeri* tarafından belirtilen adla önbelleğine *anahtar*.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-231">Puts *value* into the cache under the name specified by *key*.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

<span data-ttu-id="9e3a8-232">Yeni bir `WebGrid` kullanarak bir sorgudan veri nesnesi.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-232">Creates a new `WebGrid` object using data from a query.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

<span data-ttu-id="9e3a8-233">Bir HTML tablosunda verileri görüntülemek için biçimlendirme oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-233">Renders markup to display data in an HTML table.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

<span data-ttu-id="9e3a8-234">Çağrı cihazı için işler `WebGrid` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-234">Renders a pager for the `WebGrid` object.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

<span data-ttu-id="9e3a8-235">Görüntüyü belirtilen yoldan yükler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-235">Loads an image from the specified path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

<span data-ttu-id="9e3a8-236">Belirtilen görüntü filigran olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-236">Adds the specified image as a watermark.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

<span data-ttu-id="9e3a8-237">Belirtilen metni görüntüye ekler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-237">Adds the specified text to the image.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

<span data-ttu-id="9e3a8-238">Resmi yatay veya dikey olarak döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-238">Flips the image horizontally or vertically.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

<span data-ttu-id="9e3a8-239">Görüntüyü bir dosyayı karşıya yükleme sırasında bir sayfa gönderildiğinde görüntüyü yükler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-239">Loads an image when an image is posted to a page during a file upload.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

<span data-ttu-id="9e3a8-240">Yeniden boyutlandırır bir görüntü.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-240">Resizes an the image.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

<span data-ttu-id="9e3a8-241">Resmi sola veya sağa döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-241">Rotates the image to the left or the right.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

<span data-ttu-id="9e3a8-242">Görüntüyü belirtilen yola kaydeder.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-242">Saves the image to the specified path.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

<span data-ttu-id="9e3a8-243">SMTP sunucusu için parolayı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-243">Sets the password for the SMTP server.</span></span> <span data-ttu-id="9e3a8-244">Bu özellik kümesinde normalde  *\_AppStart* sayfası.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-244">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

<span data-ttu-id="9e3a8-245">Bir e-posta iletisi gönderir.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-245">Sends an email message.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

<span data-ttu-id="9e3a8-246">SMTP sunucusu adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-246">Sets the SMTP server name.</span></span> <span data-ttu-id="9e3a8-247">Bu özellik kümesinde normalde*\_AppStart* sayfası.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-247">Normally you set this property in the*\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

<span data-ttu-id="9e3a8-248">SMTP sunucusu için kullanıcı adını belirler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-248">Sets the user name for the SMTP server.</span></span> <span data-ttu-id="9e3a8-249">Bu özelliğin normalde ayarlamalısınız  *\_AppStart* sayfası.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-249">Normally you should set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a><span data-ttu-id="9e3a8-250">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="9e3a8-250">Validation</span></span>

### `Html.ValidationMessage(field)`

<span data-ttu-id="9e3a8-251">(v2) Belirtilen alan için bir doğrulama hata iletisini işler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-251">(v2) Renders a validation error message for the specified field.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

<span data-ttu-id="9e3a8-252">(v2) Tüm doğrulama hatalarının listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-252">(v2) Displays a list of all validation errors.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

<span data-ttu-id="9e3a8-253">(v2) Bir kullanıcı giriş öğesini doğrulama belirtilen tür için kaydeder.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-253">(v2) Registers a user input element for the specified type of validation.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

<span data-ttu-id="9e3a8-254">(v2) Dinamik olarak doğrulama hatası iletilerini biçimlendirmek için istemci tarafı doğrulama için CSS sınıf öznitelikleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-254">(v2) Dynamically renders CSS class attributes for client-side validation so that you can format validation error messages.</span></span> <span data-ttu-id="9e3a8-255">(Uygun istemci-komut dosyası kitaplıkları başvuru ve CSS sınıflarının tanımlanması gerekir.)</span><span class="sxs-lookup"><span data-stu-id="9e3a8-255">(Requires that you reference the appropriate client-script libraries and that you define CSS classes.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

<span data-ttu-id="9e3a8-256">(v2) Kullanıcı giriş alanı için istemci tarafı doğrulamasını etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-256">(v2) Enables client-side validation for the user input field.</span></span> <span data-ttu-id="9e3a8-257">(Uygun istemci-komut dosyası kitaplıkları başvuru gerektirir.)</span><span class="sxs-lookup"><span data-stu-id="9e3a8-257">(Requires that you reference the appropriate client-script libraries.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

<span data-ttu-id="9e3a8-258">(v2) Doğrulama için registred olan tüm kullanıcı girişi öğelerinin geçerli değerler içeriyorsa, true döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-258">(v2) Returns true if all user input elements that are registred for validation contain valid values.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

<span data-ttu-id="9e3a8-259">(v2) Kullanıcıların kullanıcı girişi öğesi için bir değer sağlamanız gerekir belirtir.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-259">(v2) Specifies that users must provide a value for the user input element.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

<span data-ttu-id="9e3a8-260">(v2) Kullanıcıların değerleri her kullanıcı girişi öğelerinin için sağlaması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-260">(v2) Specifies that users must provide values for each of the user input elements.</span></span> <span data-ttu-id="9e3a8-261">Bu yöntem bir özel hata iletisi belirtmenize izin vermez.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-261">This method does not let you specify a custom error message.</span></span>

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

<span data-ttu-id="9e3a8-262">(v2) Kullanırken bir doğrulama testi belirtir `Validation.Add` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9e3a8-262">(v2) Specifies a validation test when you use the `Validation.Add` method.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]