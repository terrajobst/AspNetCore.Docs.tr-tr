---
title: ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirme
author: ardalis
description: ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirmeyi öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/05/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: cab383053751598b882f3716943d3d9392c56f4a
ms.sourcegitcommit: 29ace642ca0e1f0b48a18d66de266d8811df2b83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74987964"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="7f29a-103">ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirme</span><span class="sxs-lookup"><span data-stu-id="7f29a-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="7f29a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="7f29a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="7f29a-105">ASP.NET Core MVC, yanıt verilerini biçimlendirme desteğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-105">ASP.NET Core MVC has support for formatting response data.</span></span> <span data-ttu-id="7f29a-106">Yanıt verileri, belirli biçimler kullanılarak veya istemci tarafından istenen biçime yanıt olarak biçimlendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-106">Response data can be formatted using specific formats or in response to client requested format.</span></span>

<span data-ttu-id="7f29a-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7f29a-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="7f29a-108">Formata özgü eylem sonuçları</span><span class="sxs-lookup"><span data-stu-id="7f29a-108">Format-specific Action Results</span></span>

<span data-ttu-id="7f29a-109">Bazı eylem sonuç türleri, <xref:Microsoft.AspNetCore.Mvc.JsonResult> ve <xref:Microsoft.AspNetCore.Mvc.ContentResult>gibi belirli bir biçime özgüdür.</span><span class="sxs-lookup"><span data-stu-id="7f29a-109">Some action result types are specific to a particular format, such as <xref:Microsoft.AspNetCore.Mvc.JsonResult> and <xref:Microsoft.AspNetCore.Mvc.ContentResult>.</span></span> <span data-ttu-id="7f29a-110">Eylemler, istemci tercihlerinden bağımsız olarak belirli bir biçimde biçimlendirilen sonuçları döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-110">Actions can return results that are formatted in a particular format, regardless of client preferences.</span></span> <span data-ttu-id="7f29a-111">Örneğin, döndürme `JsonResult` JSON biçimli verileri döndürür.</span><span class="sxs-lookup"><span data-stu-id="7f29a-111">For example, returning `JsonResult` returns JSON-formatted data.</span></span> <span data-ttu-id="7f29a-112">`ContentResult` veya dize döndürüldüğünde düz metin biçimli dize verileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="7f29a-112">Returning `ContentResult` or a string returns plain-text-formatted string data.</span></span>

<span data-ttu-id="7f29a-113">Belirli bir tür döndürmek için bir eylem gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-113">An action isn't required to return any specific type.</span></span> <span data-ttu-id="7f29a-114">ASP.NET Core, tüm nesne dönüş değerlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="7f29a-114">ASP.NET Core supports any object return value.</span></span>  <span data-ttu-id="7f29a-115"><xref:Microsoft.AspNetCore.Mvc.IActionResult> türler olmayan nesneleri döndüren eylemlerin sonuçları, uygun <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> uygulamasını kullanarak serileştirilir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-115">Results from actions that return objects that are not <xref:Microsoft.AspNetCore.Mvc.IActionResult> types are serialized using the appropriate <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> implementation.</span></span> <span data-ttu-id="7f29a-116">Daha fazla bilgi için bkz. <xref:web-api/action-return-types>.</span><span class="sxs-lookup"><span data-stu-id="7f29a-116">For more information, see <xref:web-api/action-return-types>.</span></span>

<span data-ttu-id="7f29a-117">Yerleşik yardımcı yöntem <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> JSON biçimli verileri döndürür: [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span><span class="sxs-lookup"><span data-stu-id="7f29a-117">The built-in helper method <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> returns JSON-formatted data: [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span></span>

<span data-ttu-id="7f29a-118">Örnek indirme, yazarların listesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="7f29a-118">The sample download returns the list of authors.</span></span> <span data-ttu-id="7f29a-119">Önceki kodla F12 tarayıcı geliştirici araçları veya [Postman](https://www.getpostman.com/tools) kullanma:</span><span class="sxs-lookup"><span data-stu-id="7f29a-119">Using the F12 browser developer tools or [Postman](https://www.getpostman.com/tools) with the previous code:</span></span>

* <span data-ttu-id="7f29a-120">**Content-Type:** `application/json; charset=utf-8` içeren yanıt üst bilgisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-120">The response header containing **content-type:** `application/json; charset=utf-8` is displayed.</span></span>
* <span data-ttu-id="7f29a-121">İstek üst bilgileri görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-121">The request headers are displayed.</span></span> <span data-ttu-id="7f29a-122">Örneğin, `Accept` üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="7f29a-122">For example, the `Accept` header.</span></span> <span data-ttu-id="7f29a-123">`Accept` üst bilgisi, önceki kod tarafından yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-123">The `Accept` header is ignored by the preceding code.</span></span>

<span data-ttu-id="7f29a-124">Düz metin biçimli verileri döndürmek için <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> ve <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> yardımcısını kullanın:</span><span class="sxs-lookup"><span data-stu-id="7f29a-124">To return plain text formatted data, use <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> and the <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

<span data-ttu-id="7f29a-125">Yukarıdaki kodda, döndürülen `Content-Type` `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="7f29a-125">In the preceding code, the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="7f29a-126">Bir dize döndürmek `text/plain``Content-Type` sağlar:</span><span class="sxs-lookup"><span data-stu-id="7f29a-126">Returning a string delivers `Content-Type` of `text/plain`:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

<span data-ttu-id="7f29a-127">Birden çok dönüş türüne sahip eylemler için `IActionResult`döndürün.</span><span class="sxs-lookup"><span data-stu-id="7f29a-127">For actions with multiple return types, return `IActionResult`.</span></span> <span data-ttu-id="7f29a-128">Örneğin, gerçekleştirilen işlemlerin sonucuna göre farklı HTTP durum kodları döndürülüyor.</span><span class="sxs-lookup"><span data-stu-id="7f29a-128">For example, returning different HTTP status codes based on the result of operations performed.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="7f29a-129">İçerik anlaşması</span><span class="sxs-lookup"><span data-stu-id="7f29a-129">Content negotiation</span></span>

<span data-ttu-id="7f29a-130">İstemci bir [Accept üst bilgisi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)belirttiğinde içerik anlaşması oluşur.</span><span class="sxs-lookup"><span data-stu-id="7f29a-130">Content negotiation occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="7f29a-131">ASP.NET Core tarafından kullanılan varsayılan biçim [JSON](https://json.org/)'dir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-131">The default format used by ASP.NET Core is [JSON](https://json.org/).</span></span> <span data-ttu-id="7f29a-132">İçerik anlaşması:</span><span class="sxs-lookup"><span data-stu-id="7f29a-132">Content negotiation is:</span></span>

* <span data-ttu-id="7f29a-133"><xref:Microsoft.AspNetCore.Mvc.ObjectResult>tarafından uygulandı.</span><span class="sxs-lookup"><span data-stu-id="7f29a-133">Implemented by <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.</span></span>
* <span data-ttu-id="7f29a-134">Yardımcı metotlarından döndürülen durum koduna özgü eylem sonuçlarına yerleşik olarak.</span><span class="sxs-lookup"><span data-stu-id="7f29a-134">Built into the status code-specific action results returned from the helper methods.</span></span> <span data-ttu-id="7f29a-135">Eylem sonuçları yardımcı yöntemleri `ObjectResult`tabanlıdır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-135">The action results helper methods are based on `ObjectResult`.</span></span>

<span data-ttu-id="7f29a-136">Bir model türü döndürüldüğünde, dönüş türü `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="7f29a-136">When a model type is returned,  the return type is `ObjectResult`.</span></span>

<span data-ttu-id="7f29a-137">Aşağıdaki eylem yöntemi `Ok` ve `NotFound` yardımcı yöntemlerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="7f29a-137">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

<span data-ttu-id="7f29a-138">Varsayılan olarak, ASP.NET Core `application/json`, `text/json`ve `text/plain` medya türlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="7f29a-138">By default, ASP.NET Core supports `application/json`, `text/json`, and `text/plain` media types.</span></span> <span data-ttu-id="7f29a-139">[Fiddler](https://www.telerik.com/fiddler) veya [Postman](https://www.getpostman.com/tools) gibi araçlar, dönüş biçimini belirtmek için `Accept` istek üst bilgisini ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-139">Tools such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/tools) can set the `Accept` request header to specify the return format.</span></span> <span data-ttu-id="7f29a-140">`Accept` üst bilgisi sunucunun desteklediği bir tür içerdiğinde, bu tür döndürülür.</span><span class="sxs-lookup"><span data-stu-id="7f29a-140">When the `Accept` header contains a type the server supports, that type is returned.</span></span> <span data-ttu-id="7f29a-141">Sonraki bölümde, ek Biçimlendiriciler ekleme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-141">The next section shows how to add additional formatters.</span></span>

<span data-ttu-id="7f29a-142">Denetleyici eylemleri POCOs (düz eski CLR nesneleri) döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-142">Controller actions can return POCOs (Plain Old CLR Objects).</span></span> <span data-ttu-id="7f29a-143">POCO döndürüldüğünde, çalışma zamanı otomatik olarak nesneyi sarmalayan bir `ObjectResult` oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7f29a-143">When a POCO is returned, the runtime automatically creates an `ObjectResult` that wraps the object.</span></span> <span data-ttu-id="7f29a-144">İstemci, biçimlendirilen serileştirilmiş nesneyi alır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-144">The client gets the formatted serialized object.</span></span> <span data-ttu-id="7f29a-145">Döndürülen nesne `null`, bir `204 No Content` yanıtı döndürülür.</span><span class="sxs-lookup"><span data-stu-id="7f29a-145">If the object being returned is `null`, a `204 No Content` response is returned.</span></span>

<span data-ttu-id="7f29a-146">Nesne türü döndürülüyor:</span><span class="sxs-lookup"><span data-stu-id="7f29a-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

<span data-ttu-id="7f29a-147">Önceki kodda, geçerli bir yazar diğer adı için bir istek yazarın verileriyle `200 OK` bir yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="7f29a-147">In the preceding code, a request for a valid author alias returns a `200 OK` response with the author's data.</span></span> <span data-ttu-id="7f29a-148">Geçersiz bir diğer ad isteği bir `204 No Content` yanıtı döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="7f29a-148">A request for an invalid alias returns a `204 No Content` response.</span></span>

### <a name="the-accept-header"></a><span data-ttu-id="7f29a-149">Accept üstbilgisi</span><span class="sxs-lookup"><span data-stu-id="7f29a-149">The Accept header</span></span>

<span data-ttu-id="7f29a-150">İstekte bir `Accept` üst bilgisi göründüğünde içerik *anlaşması* gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-150">Content *negotiation* takes place when an `Accept` header appears in the request.</span></span> <span data-ttu-id="7f29a-151">Bir istek bir Accept üst bilgisi içerdiğinde ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="7f29a-151">When a request contains an accept header, ASP.NET Core:</span></span>

* <span data-ttu-id="7f29a-152">Kabul üst bilgisindeki medya türlerini tercih sırasına göre numaralandırır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-152">Enumerates the media types in the accept header in preference order.</span></span>
* <span data-ttu-id="7f29a-153">Belirtilen biçimlerden birinde yanıt üretemeyen bir biçimlendirici bulmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-153">Tries to find a formatter that can produce a response in one of the formats specified.</span></span>

<span data-ttu-id="7f29a-154">İstemcinin isteğini karşılayabilen bir biçimlendirici bulunmazsa ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="7f29a-154">If no formatter is found that can satisfy the client's request, ASP.NET Core:</span></span>

* <span data-ttu-id="7f29a-155"><xref:Microsoft.AspNetCore.Mvc.MvcOptions> ayarlandıysa `406 Not Acceptable` döndürür veya-</span><span class="sxs-lookup"><span data-stu-id="7f29a-155">Returns `406 Not Acceptable` if <xref:Microsoft.AspNetCore.Mvc.MvcOptions> has been set, or -</span></span>
* <span data-ttu-id="7f29a-156">Yanıt üreten ilk biçimlendirici bulmayı dener.</span><span class="sxs-lookup"><span data-stu-id="7f29a-156">Tries to find the first formatter that can produce a response.</span></span>

<span data-ttu-id="7f29a-157">İstenen biçim için bir biçimlendirici yapılandırılmamışsa, nesneyi biçimlendirebileceğini ilk biçimlendirici kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-157">If no formatter is configured for the requested format, the first formatter that can format the object is used.</span></span> <span data-ttu-id="7f29a-158">İstekte hiçbir `Accept` üstbilgisi görünürse:</span><span class="sxs-lookup"><span data-stu-id="7f29a-158">If no `Accept` header appears in the request:</span></span>

* <span data-ttu-id="7f29a-159">Nesneyi işleyebilen ilk biçimlendirici, yanıtı seri hale getirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-159">The first formatter that can handle the object is used to serialize the response.</span></span>
* <span data-ttu-id="7f29a-160">Hiçbir anlaşma gerçekleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="7f29a-160">There isn't any negotiation taking place.</span></span> <span data-ttu-id="7f29a-161">Sunucu hangi biçimin döneceğine karar verir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-161">The server is determining what format to return.</span></span>

<span data-ttu-id="7f29a-162">Accept üst bilgisi `*/*`içeriyorsa, <xref:Microsoft.AspNetCore.Mvc.MvcOptions>`RespectBrowserAcceptHeader` true olarak ayarlanmadığı takdirde başlık yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-162">If the Accept header contains `*/*`, the Header is ignored unless `RespectBrowserAcceptHeader` is set to true on <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="7f29a-163">Tarayıcılar ve içerik anlaşması</span><span class="sxs-lookup"><span data-stu-id="7f29a-163">Browsers and content negotiation</span></span>

<span data-ttu-id="7f29a-164">Tipik API istemcilerinin aksine Web tarayıcıları `Accept` üst bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="7f29a-164">Unlike typical API clients, web browsers supply `Accept` headers.</span></span> <span data-ttu-id="7f29a-165">Web tarayıcısı, joker karakterler dahil olmak üzere birçok biçim belirtir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-165">Web browser specify many formats, including wildcards.</span></span> <span data-ttu-id="7f29a-166">Varsayılan olarak, Framework isteğin bir tarayıcıdan geldiğini algıladığında:</span><span class="sxs-lookup"><span data-stu-id="7f29a-166">By default, when the framework detects that the request is coming from a browser:</span></span>

* <span data-ttu-id="7f29a-167">`Accept` üst bilgisi yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-167">The `Accept` header is ignored.</span></span>
* <span data-ttu-id="7f29a-168">Aksi yapılandırılmadığı takdirde içerik JSON içinde döndürülür.</span><span class="sxs-lookup"><span data-stu-id="7f29a-168">The content is returned in JSON, unless otherwise configured.</span></span>

<span data-ttu-id="7f29a-169">Bu, API 'Leri tükettiren tarayıcılarda daha tutarlı bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="7f29a-169">This provides a more consistent experience across browsers when consuming APIs.</span></span>

<span data-ttu-id="7f29a-170">Bir uygulamayı tarayıcı onay üstbilgilerini kabul edecek şekilde yapılandırmak için <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> `true`olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="7f29a-170">To configure an app to honor browser accept headers, set <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> to `true`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a><span data-ttu-id="7f29a-171">Biçimleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7f29a-171">Configure formatters</span></span>

<span data-ttu-id="7f29a-172">Ek biçimleri desteklemesi gereken uygulamalar uygun NuGet paketlerini ekleyebilir ve desteği yapılandırabilir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-172">Apps that need to support additional formats can add the appropriate NuGet packages and configure support.</span></span> <span data-ttu-id="7f29a-173">Giriş ve çıkış için ayrı biçimlendirme vardır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-173">There are separate formatters for input and output.</span></span> <span data-ttu-id="7f29a-174">Giriş formatlayıcıları [model bağlama](xref:mvc/models/model-binding)tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-174">Input formatters are used by [Model Binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="7f29a-175">Çıkış biçimleri, yanıtları biçimlendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-175">Output formatters are used to format responses.</span></span> <span data-ttu-id="7f29a-176">Özel bir biçimlendirici oluşturma hakkında daha fazla bilgi için bkz. [özel Formatlayıcılar](xref:web-api/advanced/custom-formatters).</span><span class="sxs-lookup"><span data-stu-id="7f29a-176">For information on creating a custom formatter, see [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a><span data-ttu-id="7f29a-177">XML biçimi desteği ekle</span><span class="sxs-lookup"><span data-stu-id="7f29a-177">Add XML format support</span></span>

<span data-ttu-id="7f29a-178"><xref:System.Xml.Serialization.XmlSerializer> kullanılarak uygulanan XML formatlayıcıları <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>çağırarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="7f29a-178">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

<span data-ttu-id="7f29a-179">Yukarıdaki kod, `XmlSerializer`kullanarak sonuçları seri hale getirir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-179">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="7f29a-180">Önceki kodu kullanırken, denetleyici yöntemleri isteğin `Accept` üst bilgisine göre uygun biçimi döndürür.</span><span class="sxs-lookup"><span data-stu-id="7f29a-180">When using the preceding code, controller methods return the appropriate format based on the request's `Accept` header.</span></span>

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="7f29a-181">System. Text. JSON tabanlı formatlayıcıları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7f29a-181">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="7f29a-182">`System.Text.Json`tabanlı formatlayıcılar için özellikler, `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`kullanılarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-182">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span></span>

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="7f29a-183">Çıkış serileştirme seçenekleri, eylem başına temelinde, `JsonResult`kullanılarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-183">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="7f29a-184">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="7f29a-184">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="7f29a-185">Newtonsoft. JSON tabanlı JSON biçimi desteği ekleyin</span><span class="sxs-lookup"><span data-stu-id="7f29a-185">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="7f29a-186">ASP.NET Core 3,0 ' dan önce, varsayılan olarak kullanılan JSON formatlayıcıları `Newtonsoft.Json` paketi kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-186">Prior to ASP.NET Core 3.0, the default used JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="7f29a-187">ASP.NET Core 3,0 veya üzeri sürümlerde, varsayılan JSON biçimleri `System.Text.Json`temel alır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-187">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="7f29a-188">`Newtonsoft.Json` tabanlı formatlayıcılar ve özellikler için destek, [Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet paketini yükleyerek ve `Startup.ConfigureServices`yapılandırılarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-188">Support for `Newtonsoft.Json` based formatters and features is available by installing the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

<span data-ttu-id="7f29a-189">Bazı özellikler `System.Text.Json`tabanlı formatlayıcılar ile iyi çalışmayabilir ve `Newtonsoft.Json`tabanlı Biçimlendiriciler için bir başvuru gerektirir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-189">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters.</span></span> <span data-ttu-id="7f29a-190">Uygulama şu durumlarda `Newtonsoft.Json`tabanlı formatlayıcıları kullanmaya devam edin:</span><span class="sxs-lookup"><span data-stu-id="7f29a-190">Continue using the `Newtonsoft.Json`-based formatters if the app:</span></span>

* <span data-ttu-id="7f29a-191">`Newtonsoft.Json` özniteliklerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-191">Uses `Newtonsoft.Json` attributes.</span></span> <span data-ttu-id="7f29a-192">Örneğin, `[JsonProperty]` veya `[JsonIgnore]`.</span><span class="sxs-lookup"><span data-stu-id="7f29a-192">For example, `[JsonProperty]` or `[JsonIgnore]`.</span></span>
* <span data-ttu-id="7f29a-193">Serileştirme ayarlarını özelleştirir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-193">Customizes the serialization settings.</span></span>
* <span data-ttu-id="7f29a-194">`Newtonsoft.Json` sağladığı özellikleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-194">Relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="7f29a-195">`Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-195">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="7f29a-196">ASP.NET Core 3,0 ' dan önce `JsonResult.SerializerSettings`, `Newtonsoft.Json`özgü bir `JsonSerializerSettings` örneğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="7f29a-196">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="7f29a-197">[Openapı](<xref:tutorials/web-api-help-pages-using-swagger>) belgeleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7f29a-197">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

<span data-ttu-id="7f29a-198">`Newtonsoft.Json`tabanlı formatlayıcılar için özellikler `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`kullanılarak yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="7f29a-198">Features for the `Newtonsoft.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span></span>

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefaultContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="7f29a-199">Çıkış serileştirme seçenekleri, eylem başına temelinde, `JsonResult`kullanılarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-199">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="7f29a-200">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="7f29a-200">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerSettings
    {
        options.Formatting = Formatting.Indented,
    });
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

### <a name="add-xml-format-support"></a><span data-ttu-id="7f29a-201">XML biçimi desteği ekle</span><span class="sxs-lookup"><span data-stu-id="7f29a-201">Add XML format support</span></span>

<span data-ttu-id="7f29a-202">XML biçimlendirme, [Microsoft. AspNetCore. Mvc. Formatters. xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet paketini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-202">XML formatting requires the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

<span data-ttu-id="7f29a-203"><xref:System.Xml.Serialization.XmlSerializer> kullanılarak uygulanan XML formatlayıcıları <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>çağırarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="7f29a-203">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

<span data-ttu-id="7f29a-204">Yukarıdaki kod, `XmlSerializer`kullanarak sonuçları seri hale getirir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-204">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="7f29a-205">Önceki kodu kullanırken, denetleyici yöntemleri isteğin `Accept` üst bilgisine göre uygun biçimi döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-205">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

::: moniker-end

### <a name="specify-a-format"></a><span data-ttu-id="7f29a-206">Biçim belirtin</span><span class="sxs-lookup"><span data-stu-id="7f29a-206">Specify a format</span></span>

<span data-ttu-id="7f29a-207">Yanıt biçimlerini kısıtlamak için [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filtresini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="7f29a-207">To restrict the response formats, apply the [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter.</span></span> <span data-ttu-id="7f29a-208">Çoğu [filtre](xref:mvc/controllers/filters)gibi `[Produces]` eylem, denetleyici veya genel kapsamda uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="7f29a-208">Like most [Filters](xref:mvc/controllers/filters), `[Produces]` can be applied at the action, controller, or global scope:</span></span>

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

<span data-ttu-id="7f29a-209">Önceki [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filtresi:</span><span class="sxs-lookup"><span data-stu-id="7f29a-209">The preceding [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter:</span></span>

* <span data-ttu-id="7f29a-210">Denetleyici içindeki tüm eylemleri JSON biçimli yanıtları döndürecek şekilde zorlar.</span><span class="sxs-lookup"><span data-stu-id="7f29a-210">Forces all actions within the controller to return JSON-formatted responses.</span></span>
* <span data-ttu-id="7f29a-211">Diğer formatlayıcılar yapılandırıldıysa ve istemci farklı bir biçim belirtiyorsa JSON döndürülür.</span><span class="sxs-lookup"><span data-stu-id="7f29a-211">If other formatters are configured and the client specifies a different format, JSON is returned.</span></span>

<span data-ttu-id="7f29a-212">Daha fazla bilgi için bkz. [Filtreler](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="7f29a-212">For more information, see [Filters](xref:mvc/controllers/filters).</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="7f29a-213">Özel durum formatları</span><span class="sxs-lookup"><span data-stu-id="7f29a-213">Special case formatters</span></span>

<span data-ttu-id="7f29a-214">Bazı özel durumlar, yerleşik formatlayıcılar kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-214">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="7f29a-215">`string` dönüş türleri varsayılan olarak *metin/düz* (`Accept` üst bilgisi ile isteniyorsa*metin/html* ) olarak biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-215">By default, `string` return types are formatted as *text/plain* (*text/html* if requested via the `Accept` header).</span></span> <span data-ttu-id="7f29a-216">Bu davranış, <xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter>kaldırılarak silinebilir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-216">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter>.</span></span> <span data-ttu-id="7f29a-217">Biçimlendiriciler `ConfigureServices` yönteminde kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-217">Formatters are removed in the `ConfigureServices` method.</span></span> <span data-ttu-id="7f29a-218">Model nesne dönüş türü olan eylemler `null`döndürürken `204 No Content` döndürür.</span><span class="sxs-lookup"><span data-stu-id="7f29a-218">Actions that have a model object return type return `204 No Content` when returning `null`.</span></span> <span data-ttu-id="7f29a-219">Bu davranış, <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>kaldırılarak silinebilir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-219">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span></span> <span data-ttu-id="7f29a-220">Aşağıdaki kod `StringOutputFormatter` ve `HttpNoContentOutputFormatter`kaldırır.</span><span class="sxs-lookup"><span data-stu-id="7f29a-220">The following code removes the `StringOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end

<span data-ttu-id="7f29a-221">`StringOutputFormatter`olmadan, yerleşik JSON biçimlendirici `string` dönüş türlerini biçimlendirir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-221">Without the `StringOutputFormatter`, the built-in JSON formatter formats `string` return types.</span></span> <span data-ttu-id="7f29a-222">Yerleşik JSON biçimlendiricisi kaldırılırsa ve bir XML biçimlendirici varsa, XML biçimlendirici `string` dönüş türlerini biçimlendirir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-222">If the built-in JSON formatter is removed and an XML formatter is available, the XML formatter formats `string` return types.</span></span> <span data-ttu-id="7f29a-223">Aksi takdirde, `string` dönüş türleri `406 Not Acceptable`döndürür.</span><span class="sxs-lookup"><span data-stu-id="7f29a-223">Otherwise, `string` return types return `406 Not Acceptable`.</span></span>

<span data-ttu-id="7f29a-224">`HttpNoContentOutputFormatter`olmadan, null nesneler yapılandırılmış biçimlendirici kullanılarak biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-224">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="7f29a-225">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="7f29a-225">For example:</span></span>

* <span data-ttu-id="7f29a-226">JSON biçimlendiricisi `null`gövdesiyle bir yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="7f29a-226">The JSON formatter returns a response with a body of `null`.</span></span>
* <span data-ttu-id="7f29a-227">XML biçimlendiricisi, `xsi:nil="true"` ayarlanan özniteliğe sahip boş bir XML öğesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="7f29a-227">The XML formatter returns an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="7f29a-228">Yanıt biçimi URL eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="7f29a-228">Response format URL mappings</span></span>

<span data-ttu-id="7f29a-229">İstemciler URL 'nin bir parçası olarak belirli bir biçim talep edebilir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="7f29a-229">Clients can request a particular format as part of the URL, for example:</span></span>

* <span data-ttu-id="7f29a-230">Sorgu dizesinde veya yolun bir bölümünde.</span><span class="sxs-lookup"><span data-stu-id="7f29a-230">In the query string or part of the path.</span></span>
* <span data-ttu-id="7f29a-231">. Xml veya. JSON gibi formata özgü bir dosya uzantısı kullanarak.</span><span class="sxs-lookup"><span data-stu-id="7f29a-231">By using a format-specific file extension such as .xml or .json.</span></span>

<span data-ttu-id="7f29a-232">İstek yolundan eşleme, API 'nin kullandığı rotada belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="7f29a-232">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="7f29a-233">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="7f29a-233">For example:</span></span>

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="7f29a-234">Önceki yol, istenen biçimin isteğe bağlı bir dosya uzantısı olarak belirtilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="7f29a-234">The preceding route allows the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="7f29a-235">[`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) özniteliği, `RouteData` biçim değerinin varlığını denetler ve yanıt oluşturulduğu zaman yanıt biçimini uygun biçimlendirici ile eşler.</span><span class="sxs-lookup"><span data-stu-id="7f29a-235">The [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) attribute checks for the existence of the format value in the `RouteData` and maps the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="7f29a-236">Yol</span><span class="sxs-lookup"><span data-stu-id="7f29a-236">Route</span></span>        |             <span data-ttu-id="7f29a-237">Biçimlendirici</span><span class="sxs-lookup"><span data-stu-id="7f29a-237">Formatter</span></span>              |
|------------------------|------------------------------------|
|   `/api/products/5`    |    <span data-ttu-id="7f29a-238">Varsayılan çıkış biçimlendiricisi</span><span class="sxs-lookup"><span data-stu-id="7f29a-238">The default output formatter</span></span>    |
| `/api/products/5.json` | <span data-ttu-id="7f29a-239">JSON biçimlendiricisi (yapılandırıldıysa)</span><span class="sxs-lookup"><span data-stu-id="7f29a-239">The JSON formatter (if configured)</span></span> |
| `/api/products/5.xml`  | <span data-ttu-id="7f29a-240">XML biçimlendiricisi (yapılandırıldıysa)</span><span class="sxs-lookup"><span data-stu-id="7f29a-240">The XML formatter (if configured)</span></span>  |
