---
title: ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirme
author: ardalis
description: ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirmeyi öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 8/22/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: 78fe620ea8fdd681a276253f77939bcb2a56ebb9
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391287"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="f94d3-103">ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirme</span><span class="sxs-lookup"><span data-stu-id="f94d3-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="f94d3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f94d3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f94d3-105">ASP.NET Core MVC, yanıt verilerini biçimlendirme desteğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-105">ASP.NET Core MVC has support for formatting response data.</span></span> <span data-ttu-id="f94d3-106">Yanıt verileri, belirli biçimler kullanılarak veya istemci tarafından istenen biçime yanıt olarak biçimlendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-106">Response data can be formatted using specific formats or in response to client requested format.</span></span>

<span data-ttu-id="f94d3-107">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f94d3-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="f94d3-108">Formata özgü eylem sonuçları</span><span class="sxs-lookup"><span data-stu-id="f94d3-108">Format-specific Action Results</span></span>

<span data-ttu-id="f94d3-109">Bazı eylem sonuç türleri, <xref:Microsoft.AspNetCore.Mvc.JsonResult> ve <xref:Microsoft.AspNetCore.Mvc.ContentResult> gibi belirli bir biçime özgüdür.</span><span class="sxs-lookup"><span data-stu-id="f94d3-109">Some action result types are specific to a particular format, such as <xref:Microsoft.AspNetCore.Mvc.JsonResult> and <xref:Microsoft.AspNetCore.Mvc.ContentResult>.</span></span> <span data-ttu-id="f94d3-110">Eylemler, istemci tercihlerinden bağımsız olarak belirli bir biçimde biçimlendirilen sonuçları döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-110">Actions can return results that are formatted in a particular format, regardless of client preferences.</span></span> <span data-ttu-id="f94d3-111">Örneğin, `JsonResult` ' ı döndürmek JSON biçimli verileri döndürür.</span><span class="sxs-lookup"><span data-stu-id="f94d3-111">For example, returning `JsonResult` returns JSON-formatted data.</span></span> <span data-ttu-id="f94d3-112">@No__t-0 veya dize döndürmek düz metin biçimli dize verileri döndürür.</span><span class="sxs-lookup"><span data-stu-id="f94d3-112">Returning `ContentResult` or a string returns plain-text-formatted string data.</span></span>

<span data-ttu-id="f94d3-113">Belirli bir tür döndürmek için bir eylem gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-113">An action isn't required to return any specific type.</span></span> <span data-ttu-id="f94d3-114">ASP.NET Core, tüm nesne dönüş değerlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="f94d3-114">ASP.NET Core supports any object return value.</span></span>  <span data-ttu-id="f94d3-115">@No__t-0 türünde olmayan nesneleri döndüren eylemlerin sonuçları, uygun <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> uygulamasını kullanarak serileştirilir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-115">Results from actions that return objects that are not <xref:Microsoft.AspNetCore.Mvc.IActionResult> types are serialized using the appropriate <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> implementation.</span></span> <span data-ttu-id="f94d3-116">Daha fazla bilgi için bkz. <xref:web-api/action-return-types>.</span><span class="sxs-lookup"><span data-stu-id="f94d3-116">For more information, see <xref:web-api/action-return-types>.</span></span>

<span data-ttu-id="f94d3-117">@No__t-0 yerleşik yardımcı yöntemi JSON biçimli verileri döndürüyor: [!code-csharp @ no__t-2</span><span class="sxs-lookup"><span data-stu-id="f94d3-117">The built-in helper method <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> returns JSON-formatted data: [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span></span>

<span data-ttu-id="f94d3-118">Örnek indirme, yazarların listesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="f94d3-118">The sample download returns the list of authors.</span></span> <span data-ttu-id="f94d3-119">Önceki kodla F12 tarayıcı geliştirici araçları veya [Postman](https://www.getpostman.com/tools) kullanma:</span><span class="sxs-lookup"><span data-stu-id="f94d3-119">Using the F12 browser developer tools or [Postman](https://www.getpostman.com/tools) with the previous code:</span></span>

* <span data-ttu-id="f94d3-120">**Content-Type:** `application/json; charset=utf-8` içeren yanıt üst bilgisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-120">The response header containing **content-type:** `application/json; charset=utf-8` is displayed.</span></span>
* <span data-ttu-id="f94d3-121">İstek üst bilgileri görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-121">The request headers are displayed.</span></span> <span data-ttu-id="f94d3-122">Örneğin, `Accept` üstbilgisi.</span><span class="sxs-lookup"><span data-stu-id="f94d3-122">For example, the `Accept` header.</span></span> <span data-ttu-id="f94d3-123">@No__t-0 üstbilgisi, önceki kod tarafından yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-123">The `Accept` header is ignored by the preceding code.</span></span>

<span data-ttu-id="f94d3-124">Düz metin biçimli verileri döndürmek için <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> ve <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> yardımcısını kullanın:</span><span class="sxs-lookup"><span data-stu-id="f94d3-124">To return plain text formatted data, use <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> and the <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

<span data-ttu-id="f94d3-125">Yukarıdaki kodda, döndürülen `Content-Type` `text/plain` ' dir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-125">In the preceding code, the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="f94d3-126">Dize döndürmek `Content-Type` ' dan `text/plain` ' i sunar:</span><span class="sxs-lookup"><span data-stu-id="f94d3-126">Returning a string delivers `Content-Type` of `text/plain`:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

<span data-ttu-id="f94d3-127">Birden çok dönüş türüne sahip eylemler için `IActionResult` döndürün.</span><span class="sxs-lookup"><span data-stu-id="f94d3-127">For actions with multiple return types, return `IActionResult`.</span></span> <span data-ttu-id="f94d3-128">Örneğin, gerçekleştirilen işlemlerin sonucuna göre farklı HTTP durum kodları döndürülüyor.</span><span class="sxs-lookup"><span data-stu-id="f94d3-128">For example, returning different HTTP status codes based on the result of operations performed.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="f94d3-129">İçerik anlaşması</span><span class="sxs-lookup"><span data-stu-id="f94d3-129">Content negotiation</span></span>

<span data-ttu-id="f94d3-130">İstemci bir [Accept üst bilgisi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)belirttiğinde içerik anlaşması oluşur.</span><span class="sxs-lookup"><span data-stu-id="f94d3-130">Content negotiation occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="f94d3-131">ASP.NET Core tarafından kullanılan varsayılan biçim [JSON](https://json.org/)'dir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-131">The default format used by ASP.NET Core is [JSON](https://json.org/).</span></span> <span data-ttu-id="f94d3-132">İçerik anlaşması:</span><span class="sxs-lookup"><span data-stu-id="f94d3-132">Content negotiation is:</span></span>

* <span data-ttu-id="f94d3-133">@No__t-0 tarafından uygulandı.</span><span class="sxs-lookup"><span data-stu-id="f94d3-133">Implemented by <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.</span></span>
* <span data-ttu-id="f94d3-134">Yardımcı metotlarından döndürülen durum koduna özgü eylem sonuçlarına yerleşik olarak.</span><span class="sxs-lookup"><span data-stu-id="f94d3-134">Built into the status code-specific action results returned from the helper methods.</span></span> <span data-ttu-id="f94d3-135">Eylem sonuçları yardımcı yöntemleri `ObjectResult` ' a dayalıdır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-135">The action results helper methods are based on `ObjectResult`.</span></span>

<span data-ttu-id="f94d3-136">Bir model türü döndürüldüğünde, dönüş türü `ObjectResult` ' dır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-136">When a model type is returned,  the return type is `ObjectResult`.</span></span>

<span data-ttu-id="f94d3-137">Aşağıdaki eylem yöntemi `Ok` ve `NotFound` yardımcı yöntemlerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="f94d3-137">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

<span data-ttu-id="f94d3-138">Varsayılan olarak, ASP.NET Core `application/json`, `text/json` ve `text/plain` medya türlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="f94d3-138">By default, ASP.NET Core supports `application/json`, `text/json`, and `text/plain` media types.</span></span> <span data-ttu-id="f94d3-139">[Fiddler](https://www.telerik.com/fiddler) veya [Postman](https://www.getpostman.com/tools) gibi araçlar, `Accept` istek üst bilgisini dönüş biçimini belirtecek şekilde ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-139">Tools such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/tools) can set the `Accept` request header to specify the return format.</span></span> <span data-ttu-id="f94d3-140">@No__t-0 üstbilgisi sunucunun desteklediği bir tür içerdiğinde, bu tür döndürülür.</span><span class="sxs-lookup"><span data-stu-id="f94d3-140">When the `Accept` header contains a type the server supports, that type is returned.</span></span> <span data-ttu-id="f94d3-141">Sonraki bölümde, ek Biçimlendiriciler ekleme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-141">The next section shows how to add additional formatters.</span></span>

<span data-ttu-id="f94d3-142">Denetleyici eylemleri POCOs (düz eski CLR nesneleri) döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-142">Controller actions can return POCOs (Plain Old CLR Objects).</span></span> <span data-ttu-id="f94d3-143">POCO döndürüldüğünde, çalışma zamanı nesneyi sarmalayan bir @no__t otomatik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f94d3-143">When a POCO is returned, the runtime automatically creates an `ObjectResult` that wraps the object.</span></span> <span data-ttu-id="f94d3-144">İstemci, biçimlendirilen serileştirilmiş nesneyi alır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-144">The client gets the formatted serialized object.</span></span> <span data-ttu-id="f94d3-145">Döndürülen nesne `null` ise, bir `204 No Content` yanıtı döndürülür.</span><span class="sxs-lookup"><span data-stu-id="f94d3-145">If the object being returned is `null`, a `204 No Content` response is returned.</span></span>

<span data-ttu-id="f94d3-146">Nesne türü döndürülüyor:</span><span class="sxs-lookup"><span data-stu-id="f94d3-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

<span data-ttu-id="f94d3-147">Önceki kodda, geçerli bir yazar diğer adı için bir istek yazarın verileriyle birlikte `200 OK` yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="f94d3-147">In the preceding code, a request for a valid author alias returns a `200 OK` response with the author's data.</span></span> <span data-ttu-id="f94d3-148">Geçersiz bir diğer ad isteği bir `204 No Content` yanıtı döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="f94d3-148">A request for an invalid alias returns a `204 No Content` response.</span></span>

### <a name="the-accept-header"></a><span data-ttu-id="f94d3-149">Accept üst bilgisi</span><span class="sxs-lookup"><span data-stu-id="f94d3-149">The Accept header</span></span>

<span data-ttu-id="f94d3-150">İstekte bir `Accept` üst bilgisi göründüğünde içerik *anlaşması* gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-150">Content *negotiation* takes place when an `Accept` header appears in the request.</span></span> <span data-ttu-id="f94d3-151">Bir istek bir Accept üst bilgisi içerdiğinde ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="f94d3-151">When a request contains an accept header, ASP.NET Core:</span></span>

* <span data-ttu-id="f94d3-152">Kabul üst bilgisindeki medya türlerini tercih sırasına göre numaralandırır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-152">Enumerates the media types in the accept header in preference order.</span></span>
* <span data-ttu-id="f94d3-153">Belirtilen biçimlerden birinde yanıt üretemeyen bir biçimlendirici bulmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-153">Tries to find a formatter that can produce a response in one of the formats specified.</span></span>

<span data-ttu-id="f94d3-154">İstemcinin isteğini karşılayabilen bir biçimlendirici bulunmazsa ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="f94d3-154">If no formatter is found that can satisfy the client's request, ASP.NET Core:</span></span>

* <span data-ttu-id="f94d3-155">@No__t-1 ayarlandıysa `406 Not Acceptable` döndürür veya-</span><span class="sxs-lookup"><span data-stu-id="f94d3-155">Returns `406 Not Acceptable` if <xref:Microsoft.AspNetCore.Mvc.MvcOptions> has been set, or -</span></span>
* <span data-ttu-id="f94d3-156">Yanıt üreten ilk biçimlendirici bulmayı dener.</span><span class="sxs-lookup"><span data-stu-id="f94d3-156">Tries to find the first formatter that can produce a response.</span></span>

<span data-ttu-id="f94d3-157">İstenen biçim için bir biçimlendirici yapılandırılmamışsa, nesneyi biçimlendirebileceğini ilk biçimlendirici kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-157">If no formatter is configured for the requested format, the first formatter that can format the object is used.</span></span> <span data-ttu-id="f94d3-158">İstekte `Accept` üstbilgisi görünürse:</span><span class="sxs-lookup"><span data-stu-id="f94d3-158">If no `Accept` header appears in the request:</span></span>

* <span data-ttu-id="f94d3-159">Nesneyi işleyebilen ilk biçimlendirici, yanıtı seri hale getirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-159">The first formatter that can handle the object is used to serialize the response.</span></span>
* <span data-ttu-id="f94d3-160">Hiçbir anlaşma gerçekleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="f94d3-160">There isn't any negotiation taking place.</span></span> <span data-ttu-id="f94d3-161">Sunucu hangi biçimin döneceğine karar verir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-161">The server is determining what format to return.</span></span>

<span data-ttu-id="f94d3-162">Accept üst bilgisi `*/*` içeriyorsa, `RespectBrowserAcceptHeader` <xref:Microsoft.AspNetCore.Mvc.MvcOptions> üzerinde true olarak ayarlanmadığı takdirde başlık yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-162">If the Accept header contains `*/*`, the Header is ignored unless `RespectBrowserAcceptHeader` is set to true on <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="f94d3-163">Tarayıcılar ve içerik anlaşması</span><span class="sxs-lookup"><span data-stu-id="f94d3-163">Browsers and content negotiation</span></span>

<span data-ttu-id="f94d3-164">Tipik API istemcilerinin aksine Web tarayıcıları `Accept` üst bilgilerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="f94d3-164">Unlike typical API clients, web browsers supply `Accept` headers.</span></span> <span data-ttu-id="f94d3-165">Web tarayıcısı, joker karakterler dahil olmak üzere birçok biçim belirtir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-165">Web browser specify many formats, including wildcards.</span></span> <span data-ttu-id="f94d3-166">Varsayılan olarak, Framework isteğin bir tarayıcıdan geldiğini algıladığında:</span><span class="sxs-lookup"><span data-stu-id="f94d3-166">By default, when the framework detects that the request is coming from a browser:</span></span>

* <span data-ttu-id="f94d3-167">@No__t-0 üstbilgisi yoksayıldı.</span><span class="sxs-lookup"><span data-stu-id="f94d3-167">The `Accept` header is ignored.</span></span>
* <span data-ttu-id="f94d3-168">Aksi yapılandırılmadığı takdirde içerik JSON içinde döndürülür.</span><span class="sxs-lookup"><span data-stu-id="f94d3-168">The content is returned in JSON, unless otherwise configured.</span></span>

<span data-ttu-id="f94d3-169">Bu, API 'Leri tükettiren tarayıcılarda daha tutarlı bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="f94d3-169">This provides a more consistent experience across browsers when consuming APIs.</span></span>

<span data-ttu-id="f94d3-170">Bir uygulamayı tarayıcı onay üstbilgilerini kabul edecek şekilde yapılandırmak için <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> ' ı `true` olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="f94d3-170">To configure an app to honor browser accept headers, set <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> to `true`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a><span data-ttu-id="f94d3-171">Biçimleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f94d3-171">Configure formatters</span></span>

<span data-ttu-id="f94d3-172">Ek biçimleri desteklemesi gereken uygulamalar uygun NuGet paketlerini ekleyebilir ve desteği yapılandırabilir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-172">Apps that need to support additional formats can add the appropriate NuGet packages and configure support.</span></span> <span data-ttu-id="f94d3-173">Giriş ve çıkış için ayrı biçimlendirme vardır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-173">There are separate formatters for input and output.</span></span> <span data-ttu-id="f94d3-174">Giriş formatlayıcıları [model bağlama](xref:mvc/models/model-binding)tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-174">Input formatters are used by [Model Binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="f94d3-175">Çıkış biçimleri, yanıtları biçimlendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-175">Output formatters are used to format responses.</span></span> <span data-ttu-id="f94d3-176">Özel bir biçimlendirici oluşturma hakkında daha fazla bilgi için bkz. [özel Formatlayıcılar](xref:web-api/advanced/custom-formatters).</span><span class="sxs-lookup"><span data-stu-id="f94d3-176">For information on creating a custom formatter, see [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a><span data-ttu-id="f94d3-177">XML biçimi desteği ekle</span><span class="sxs-lookup"><span data-stu-id="f94d3-177">Add XML format support</span></span>

<span data-ttu-id="f94d3-178">@No__t-0 kullanılarak uygulanan XML formatlayıcıları <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> çağırarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="f94d3-178">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

<span data-ttu-id="f94d3-179">Yukarıdaki kod `XmlSerializer` kullanarak sonuçları seri hale getirir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-179">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="f94d3-180">Önceki kodu kullanırken, denetleyici yöntemlerinin isteğin `Accept` üstbilgisine göre uygun biçimi döndürmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-180">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="f94d3-181">System. Text. JSON tabanlı formatlayıcıları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f94d3-181">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="f94d3-182">@No__t -0 tabanlı formatıcılar için özellikler `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions` kullanılarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-182">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span></span>

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="f94d3-183">Çıkış serileştirme seçenekleri, eylem başına temelinde, `JsonResult` kullanılarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-183">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="f94d3-184">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f94d3-184">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="f94d3-185">Newtonsoft. JSON tabanlı JSON biçimi desteği ekleyin</span><span class="sxs-lookup"><span data-stu-id="f94d3-185">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="f94d3-186">ASP.NET Core 3,0 ' dan önce, varsayılan olarak kullanılan JSON formatlayıcıları `Newtonsoft.Json` paketi kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-186">Prior to ASP.NET Core 3.0, the default used JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="f94d3-187">ASP.NET Core 3,0 veya üzeri sürümlerde, varsayılan JSON biçimleri `System.Text.Json` ' ı temel alır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-187">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="f94d3-188">@No__t-0 tabanlı formatcılar ve özellikler için destek, [Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet paketi yüklenerek ve bunu `Startup.ConfigureServices` ' de yapılandırarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-188">Support for `Newtonsoft.Json` based formatters and features is available by installing the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

<span data-ttu-id="f94d3-189">Bazı özellikler @no__t -0 tabanlı formatıcılar ile iyi çalışmayabilir ve @no__t -1 tabanlı formatlayıcılar için bir başvuru gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-189">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters.</span></span> <span data-ttu-id="f94d3-190">Uygulama şu durumlarda @no__t -0 tabanlı formatlayıcıları kullanmaya devam edin:</span><span class="sxs-lookup"><span data-stu-id="f94d3-190">Continue using the `Newtonsoft.Json`-based formatters if the app:</span></span>

* <span data-ttu-id="f94d3-191">@No__t-0 özniteliklerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-191">Uses `Newtonsoft.Json` attributes.</span></span> <span data-ttu-id="f94d3-192">Örneğin, `[JsonProperty]` veya `[JsonIgnore]`.</span><span class="sxs-lookup"><span data-stu-id="f94d3-192">For example, `[JsonProperty]` or `[JsonIgnore]`.</span></span>
* <span data-ttu-id="f94d3-193">Serileştirme ayarlarını özelleştirir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-193">Customizes the serialization settings.</span></span>
* <span data-ttu-id="f94d3-194">@No__t-0 ' ın sağladığı özellikleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-194">Relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="f94d3-195">@No__t yapılandırır-0.</span><span class="sxs-lookup"><span data-stu-id="f94d3-195">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="f94d3-196">ASP.NET Core 3,0 ' dan önce, `JsonResult.SerializerSettings` @no__t 2 ' ye özgü bir `JsonSerializerSettings` örneğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="f94d3-196">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="f94d3-197">[Openapı](<xref:tutorials/web-api-help-pages-using-swagger>) belgeleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f94d3-197">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

<span data-ttu-id="f94d3-198">@No__t -0 tabanlı formatıcılar için özellikler `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings` kullanılarak yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="f94d3-198">Features for the `Newtonsoft.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span></span>

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefaultContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="f94d3-199">Çıkış serileştirme seçenekleri, eylem başına temelinde, `JsonResult` kullanılarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-199">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="f94d3-200">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f94d3-200">For example:</span></span>

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

### <a name="add-xml-format-support"></a><span data-ttu-id="f94d3-201">XML biçimi desteği ekle</span><span class="sxs-lookup"><span data-stu-id="f94d3-201">Add XML format support</span></span>

<span data-ttu-id="f94d3-202">XML biçimlendirme, [Microsoft. AspNetCore. Mvc. Formatters. xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet paketini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-202">XML formatting requires the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

<span data-ttu-id="f94d3-203">@No__t-0 kullanılarak uygulanan XML formatlayıcıları <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> çağırarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="f94d3-203">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

<span data-ttu-id="f94d3-204">Yukarıdaki kod `XmlSerializer` kullanarak sonuçları seri hale getirir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-204">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="f94d3-205">Önceki kodu kullanırken, denetleyici yöntemlerinin isteğin `Accept` üstbilgisine göre uygun biçimi döndürmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-205">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

::: moniker-end

### <a name="specify-a-format"></a><span data-ttu-id="f94d3-206">Biçim belirtin</span><span class="sxs-lookup"><span data-stu-id="f94d3-206">Specify a format</span></span>

<span data-ttu-id="f94d3-207">Yanıt biçimlerini kısıtlamak için [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filtresini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="f94d3-207">To restrict the response formats, apply the [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter.</span></span> <span data-ttu-id="f94d3-208">Çoğu [filtre](xref:mvc/controllers/filters)gibi `[Produces]` eylem, denetleyici veya genel kapsamda uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="f94d3-208">Like most [Filters](xref:mvc/controllers/filters), `[Produces]` can be applied at the action, controller, or global scope:</span></span>

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

<span data-ttu-id="f94d3-209">Önceki [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filtresi:</span><span class="sxs-lookup"><span data-stu-id="f94d3-209">The preceding [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter:</span></span>

* <span data-ttu-id="f94d3-210">Denetleyici içindeki tüm eylemleri JSON biçimli yanıtları döndürecek şekilde zorlar.</span><span class="sxs-lookup"><span data-stu-id="f94d3-210">Forces all actions within the controller to return JSON-formatted responses.</span></span>
* <span data-ttu-id="f94d3-211">Diğer formatlayıcılar yapılandırıldıysa ve istemci farklı bir biçim belirtiyorsa JSON döndürülür.</span><span class="sxs-lookup"><span data-stu-id="f94d3-211">If other formatters are configured and the client specifies a different format, JSON is returned.</span></span>

<span data-ttu-id="f94d3-212">Daha fazla bilgi için bkz. [Filtreler](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="f94d3-212">For more information, see [Filters](xref:mvc/controllers/filters).</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="f94d3-213">Özel durum formatları</span><span class="sxs-lookup"><span data-stu-id="f94d3-213">Special case formatters</span></span>

<span data-ttu-id="f94d3-214">Bazı özel durumlar, yerleşik formatlayıcılar kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-214">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="f94d3-215">Varsayılan olarak, `string` dönüş türleri *metin/düz* (`Accept` üst bilgisi ile isteniyorsa*metin/html* ) olarak biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-215">By default, `string` return types are formatted as *text/plain* (*text/html* if requested via the `Accept` header).</span></span> <span data-ttu-id="f94d3-216">Bu davranış, <xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter> kaldırılarak silinebilir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-216">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter>.</span></span> <span data-ttu-id="f94d3-217">Biçimlendiriciler `ConfigureServices` yönteminde kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-217">Formatters are removed in the `ConfigureServices` method.</span></span> <span data-ttu-id="f94d3-218">Model nesne dönüş türü olan eylemler, `null` ' i döndürürken `204 No Content` döndürür.</span><span class="sxs-lookup"><span data-stu-id="f94d3-218">Actions that have a model object return type return `204 No Content` when returning `null`.</span></span> <span data-ttu-id="f94d3-219">Bu davranış, <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter> kaldırılarak silinebilir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-219">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span></span> <span data-ttu-id="f94d3-220">Aşağıdaki kod `StringOutputFormatter` ve `HttpNoContentOutputFormatter` ' i kaldırır.</span><span class="sxs-lookup"><span data-stu-id="f94d3-220">The following code removes the `StringOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end

<span data-ttu-id="f94d3-221">@No__t-0 olmadan, yerleşik JSON biçimlendiricisi `string` dönüş türlerini biçimlendirir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-221">Without the `StringOutputFormatter`, the built-in JSON formatter formats `string` return types.</span></span> <span data-ttu-id="f94d3-222">Yerleşik JSON biçimlendiricisi kaldırılırsa ve bir XML biçimlendirici varsa, XML biçimlendirici `string` dönüş türlerini biçimlendirir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-222">If the built-in JSON formatter is removed and an XML formatter is available, the XML formatter formats `string` return types.</span></span> <span data-ttu-id="f94d3-223">Aksi takdirde, @no__t 0 dönüş türleri `406 Not Acceptable` döndürür.</span><span class="sxs-lookup"><span data-stu-id="f94d3-223">Otherwise, `string` return types return `406 Not Acceptable`.</span></span>

<span data-ttu-id="f94d3-224">@No__t-0 olmadan null nesneler yapılandırılmış biçimlendirici kullanılarak biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-224">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="f94d3-225">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f94d3-225">For example:</span></span>

* <span data-ttu-id="f94d3-226">JSON biçimlendiricisi `null` gövdesi olan bir yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="f94d3-226">The JSON formatter returns a response with a body of `null`.</span></span>
* <span data-ttu-id="f94d3-227">XML biçimlendiricisi `xsi:nil="true"` kümesi özniteliğine sahip boş bir XML öğesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="f94d3-227">The XML formatter returns an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="f94d3-228">Yanıt biçimi URL eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="f94d3-228">Response format URL mappings</span></span>

<span data-ttu-id="f94d3-229">İstemciler URL 'nin bir parçası olarak belirli bir biçim talep edebilir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="f94d3-229">Clients can request a particular format as part of the URL, for example:</span></span>

* <span data-ttu-id="f94d3-230">Sorgu dizesinde veya yolun bir bölümünde.</span><span class="sxs-lookup"><span data-stu-id="f94d3-230">In the query string or part of the path.</span></span>
* <span data-ttu-id="f94d3-231">. Xml veya. JSON gibi formata özgü bir dosya uzantısı kullanarak.</span><span class="sxs-lookup"><span data-stu-id="f94d3-231">By using a format-specific file extension such as .xml or .json.</span></span>

<span data-ttu-id="f94d3-232">İstek yolundan eşleme, API 'nin kullandığı rotada belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="f94d3-232">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="f94d3-233">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f94d3-233">For example:</span></span>

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="f94d3-234">Önceki yol, istenen biçimin isteğe bağlı bir dosya uzantısı olarak belirtilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="f94d3-234">The preceding route allows the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="f94d3-235">[@No__t-1](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) özniteliği, `RouteData` ' deki biçim değerinin varlığını denetler ve yanıt oluşturulduğunda yanıt biçimini uygun biçimlendirici ile eşler.</span><span class="sxs-lookup"><span data-stu-id="f94d3-235">The [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) attribute checks for the existence of the format value in the `RouteData` and maps the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="f94d3-236">Yolu</span><span class="sxs-lookup"><span data-stu-id="f94d3-236">Route</span></span>        |             <span data-ttu-id="f94d3-237">Biçimlendirici</span><span class="sxs-lookup"><span data-stu-id="f94d3-237">Formatter</span></span>              |
|------------------------|------------------------------------|
|   `/api/products/5`    |    <span data-ttu-id="f94d3-238">Varsayılan çıkış biçimlendiricisi</span><span class="sxs-lookup"><span data-stu-id="f94d3-238">The default output formatter</span></span>    |
| `/api/products/5.json` | <span data-ttu-id="f94d3-239">JSON biçimlendiricisi (yapılandırıldıysa)</span><span class="sxs-lookup"><span data-stu-id="f94d3-239">The JSON formatter (if configured)</span></span> |
| `/api/products/5.xml`  | <span data-ttu-id="f94d3-240">XML biçimlendiricisi (yapılandırıldıysa)</span><span class="sxs-lookup"><span data-stu-id="f94d3-240">The XML formatter (if configured)</span></span>  |
