---
title: ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirme
author: ardalis
description: ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirmeyi öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 8/22/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: 5861a8e353b8fac95ca51aca7b44a768d3c2ffb7
ms.sourcegitcommit: 0365af91518004c4a44a30dc3a8ac324558a399b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71199054"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="606cd-103">ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirme</span><span class="sxs-lookup"><span data-stu-id="606cd-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="606cd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="606cd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="606cd-105">ASP.NET Core MVC, yanıt verilerini biçimlendirme desteğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="606cd-105">ASP.NET Core MVC has support for formatting response data.</span></span> <span data-ttu-id="606cd-106">Yanıt verileri, belirli biçimler kullanılarak veya istemci tarafından istenen biçime yanıt olarak biçimlendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="606cd-106">Response data can be formatted using specific formats or in response to client requested format.</span></span>

<span data-ttu-id="606cd-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="606cd-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="606cd-108">Formata özgü eylem sonuçları</span><span class="sxs-lookup"><span data-stu-id="606cd-108">Format-specific Action Results</span></span>

<span data-ttu-id="606cd-109">Bazı eylem sonuç türleri, <xref:Microsoft.AspNetCore.Mvc.JsonResult> ve <xref:Microsoft.AspNetCore.Mvc.ContentResult>gibi belirli bir biçime özgüdür.</span><span class="sxs-lookup"><span data-stu-id="606cd-109">Some action result types are specific to a particular format, such as <xref:Microsoft.AspNetCore.Mvc.JsonResult> and <xref:Microsoft.AspNetCore.Mvc.ContentResult>.</span></span> <span data-ttu-id="606cd-110">Eylemler, istemci tercihlerinden bağımsız olarak belirli bir biçimde biçimlendirilen sonuçları döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="606cd-110">Actions can return results that are formatted in a particular format, regardless of client preferences.</span></span> <span data-ttu-id="606cd-111">Örneğin, döndürme `JsonResult` JSON biçimli verileri döndürür.</span><span class="sxs-lookup"><span data-stu-id="606cd-111">For example, returning `JsonResult` returns JSON-formatted data.</span></span> <span data-ttu-id="606cd-112">Döndürme `ContentResult` veya dize, düz metin biçimli dize verileri döndürür.</span><span class="sxs-lookup"><span data-stu-id="606cd-112">Returning `ContentResult` or a string returns plain-text-formatted string data.</span></span>

<span data-ttu-id="606cd-113">Belirli bir tür döndürmek için bir eylem gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="606cd-113">An action isn't required to return any specific type.</span></span> <span data-ttu-id="606cd-114">ASP.NET Core, tüm nesne dönüş değerlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="606cd-114">ASP.NET Core supports any object return value.</span></span>  <span data-ttu-id="606cd-115">Tür olmayan <xref:Microsoft.AspNetCore.Mvc.IActionResult> nesneleri döndüren eylemlerin sonuçları, uygun <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> uygulama kullanılarak serileştirilir.</span><span class="sxs-lookup"><span data-stu-id="606cd-115">Results from actions that return objects that are not <xref:Microsoft.AspNetCore.Mvc.IActionResult> types are serialized using the appropriate <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> implementation.</span></span> <span data-ttu-id="606cd-116">Daha fazla bilgi için bkz. <xref:web-api/action-return-types>.</span><span class="sxs-lookup"><span data-stu-id="606cd-116">For more information, see <xref:web-api/action-return-types>.</span></span>

<span data-ttu-id="606cd-117">Yerleşik yardımcı yöntemi <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> JSON biçimli verileri döndürür:[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span><span class="sxs-lookup"><span data-stu-id="606cd-117">The built-in helper method <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> returns JSON-formatted data: [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span></span>

<span data-ttu-id="606cd-118">Örnek indirme, yazarların listesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="606cd-118">The sample download returns the list of authors.</span></span> <span data-ttu-id="606cd-119">Önceki kodla F12 tarayıcı geliştirici araçları veya [Postman](https://www.getpostman.com/tools) kullanma:</span><span class="sxs-lookup"><span data-stu-id="606cd-119">Using the F12 browser developer tools or [Postman](https://www.getpostman.com/tools) with the previous code:</span></span>

* <span data-ttu-id="606cd-120">**Content-Type:** `application/json; charset=utf-8` içeren yanıt üst bilgisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="606cd-120">The response header containing **content-type:** `application/json; charset=utf-8` is displayed.</span></span>
* <span data-ttu-id="606cd-121">İstek üst bilgileri görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="606cd-121">The request headers are displayed.</span></span> <span data-ttu-id="606cd-122">Örneğin, `Accept` üst bilgi.</span><span class="sxs-lookup"><span data-stu-id="606cd-122">For example, the `Accept` header.</span></span> <span data-ttu-id="606cd-123">`Accept` Üst bilgi, önceki kod tarafından yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="606cd-123">The `Accept` header is ignored by the preceding code.</span></span>

<span data-ttu-id="606cd-124">Düz metin biçimli verileri döndürmek için <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> ve yardımcısını kullanın:</span><span class="sxs-lookup"><span data-stu-id="606cd-124">To return plain text formatted data, use <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> and the <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

<span data-ttu-id="606cd-125">Yukarıdaki kodda, `Content-Type` `text/plain`döndürülen.</span><span class="sxs-lookup"><span data-stu-id="606cd-125">In the preceding code, the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="606cd-126">Şu şekilde bir dize `Content-Type` `text/plain`döndürür:</span><span class="sxs-lookup"><span data-stu-id="606cd-126">Returning a string delivers `Content-Type` of `text/plain`:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

<span data-ttu-id="606cd-127">Birden çok dönüş türüne sahip eylemler için, `IActionResult`döndürün.</span><span class="sxs-lookup"><span data-stu-id="606cd-127">For actions with multiple return types, return `IActionResult`.</span></span> <span data-ttu-id="606cd-128">Örneğin, gerçekleştirilen işlemlerin sonucuna göre farklı HTTP durum kodları döndürülüyor.</span><span class="sxs-lookup"><span data-stu-id="606cd-128">For example, returning different HTTP status codes based on the result of operations performed.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="606cd-129">İçerik anlaşması</span><span class="sxs-lookup"><span data-stu-id="606cd-129">Content negotiation</span></span>

<span data-ttu-id="606cd-130">İstemci bir [Accept üst bilgisi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)belirttiğinde içerik anlaşması oluşur.</span><span class="sxs-lookup"><span data-stu-id="606cd-130">Content negotiation occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="606cd-131">ASP.NET Core tarafından kullanılan varsayılan biçim [JSON](https://json.org/)'dir.</span><span class="sxs-lookup"><span data-stu-id="606cd-131">The default format used by ASP.NET Core is [JSON](https://json.org/).</span></span> <span data-ttu-id="606cd-132">İçerik anlaşması:</span><span class="sxs-lookup"><span data-stu-id="606cd-132">Content negotiation is:</span></span>

* <span data-ttu-id="606cd-133"><xref:Microsoft.AspNetCore.Mvc.ObjectResult>Uygulayan.</span><span class="sxs-lookup"><span data-stu-id="606cd-133">Implemented by <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.</span></span>
* <span data-ttu-id="606cd-134">Yardımcı metotlarından döndürülen durum koduna özgü eylem sonuçlarına yerleşik olarak.</span><span class="sxs-lookup"><span data-stu-id="606cd-134">Built into the status code-specific action results returned from the helper methods.</span></span> <span data-ttu-id="606cd-135">Eylem sonuçları yardımcı yöntemleri temel `ObjectResult`alınır.</span><span class="sxs-lookup"><span data-stu-id="606cd-135">The action results helper methods are based on `ObjectResult`.</span></span>

<span data-ttu-id="606cd-136">Bir model türü döndürüldüğünde, dönüş türü olur `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="606cd-136">When a model type is returned,  the return type is `ObjectResult`.</span></span>

<span data-ttu-id="606cd-137">Aşağıdaki eylem yöntemi `Ok` ve `NotFound` yardımcı yöntemlerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="606cd-137">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

<span data-ttu-id="606cd-138">Başka bir biçim istenmediği ve sunucu istenen biçimi döndüremediğinde JSON biçimli bir yanıt döndürülür.</span><span class="sxs-lookup"><span data-stu-id="606cd-138">A JSON-formatted response is returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="606cd-139">[Fiddler](https://www.telerik.com/fiddler) veya [Postman](https://www.getpostman.com/tools) gibi araçlar, `Accept` dönüş biçimini belirtmek için üst bilgiyi ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="606cd-139">Tools such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/tools) can set the `Accept` header to specify the return format.</span></span> <span data-ttu-id="606cd-140">, `Accept` Sunucunun desteklediği bir tür içerdiğinde, bu tür döndürülür.</span><span class="sxs-lookup"><span data-stu-id="606cd-140">When the `Accept` contains a type the server supports, that type is returned.</span></span>

<span data-ttu-id="606cd-141">Varsayılan olarak, ASP.NET Core yalnızca JSON 'ı destekler.</span><span class="sxs-lookup"><span data-stu-id="606cd-141">By default, ASP.NET Core only supports JSON.</span></span> <span data-ttu-id="606cd-142">Varsayılan, JSON biçimli yanıtları değiştirmemiş uygulamalar için, istemci isteğinden bağımsız olarak her zaman döndürülür.</span><span class="sxs-lookup"><span data-stu-id="606cd-142">For apps not changing the default, JSON-formatted responses are alway returned regardless of the client request.</span></span> <span data-ttu-id="606cd-143">Sonraki bölümde, ek Biçimlendiriciler ekleme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="606cd-143">The next section shows how to add additional formatters.</span></span>

<span data-ttu-id="606cd-144">Denetleyici eylemleri POCOs (düz eski CLR nesneleri) döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="606cd-144">Controller actions can return POCOs (Plain Old CLR Objects).</span></span> <span data-ttu-id="606cd-145">Poco döndürüldüğünde, çalışma zamanı nesneyi sarmalayan bir `ObjectResult` otomatik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="606cd-145">When a POCO is returned, the runtime automatically creates an `ObjectResult` that wraps the object.</span></span> <span data-ttu-id="606cd-146">İstemci, biçimlendirilen serileştirilmiş nesneyi alır.</span><span class="sxs-lookup"><span data-stu-id="606cd-146">The client gets the formatted serialized object.</span></span> <span data-ttu-id="606cd-147">Döndürülen nesne ise `null` `204 No Content` yanıt döndürülür.</span><span class="sxs-lookup"><span data-stu-id="606cd-147">If the object being returned is `null`, a `204 No Content` response is returned.</span></span>

<span data-ttu-id="606cd-148">Nesne türü döndürülüyor:</span><span class="sxs-lookup"><span data-stu-id="606cd-148">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

<span data-ttu-id="606cd-149">Önceki kodda, geçerli bir yazar diğer adı için bir istek yazarın verileriyle bir `200 OK` yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="606cd-149">In the preceding code, a request for a valid author alias returns a `200 OK` response with the author's data.</span></span> <span data-ttu-id="606cd-150">Geçersiz bir diğer ad isteği bir `204 No Content` yanıt döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="606cd-150">A request for an invalid alias returns a `204 No Content` response.</span></span>

### <a name="the-accept-header"></a><span data-ttu-id="606cd-151">Accept üst bilgisi</span><span class="sxs-lookup"><span data-stu-id="606cd-151">The Accept header</span></span>

<span data-ttu-id="606cd-152">İstekte bir `Accept` üst bilgi göründüğünde içerik anlaşması gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="606cd-152">Content *negotiation* takes place when an `Accept` header appears in the request.</span></span> <span data-ttu-id="606cd-153">Bir istek bir Accept üst bilgisi içerdiğinde ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="606cd-153">When a request contains an accept header, ASP.NET Core:</span></span>

* <span data-ttu-id="606cd-154">Kabul üst bilgisindeki medya türlerini tercih sırasına göre numaralandırır.</span><span class="sxs-lookup"><span data-stu-id="606cd-154">Enumerates the media types in the accept header in preference order.</span></span>
* <span data-ttu-id="606cd-155">Belirtilen biçimlerden birinde yanıt üretemeyen bir biçimlendirici bulmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="606cd-155">Tries to find a formatter that can produce a response in one of the formats specified.</span></span>

<span data-ttu-id="606cd-156">İstemcinin isteğini karşılayabilen bir biçimlendirici bulunmazsa ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="606cd-156">If no formatter is found that can satisfy the client's request, ASP.NET Core:</span></span>

* <span data-ttu-id="606cd-157">Ayarlanmışsa döndürür `406 Not Acceptable`veya- <xref:Microsoft.AspNetCore.Mvc.MvcOptions></span><span class="sxs-lookup"><span data-stu-id="606cd-157">Returns `406 Not Acceptable` if <xref:Microsoft.AspNetCore.Mvc.MvcOptions> has been set, or -</span></span>
* <span data-ttu-id="606cd-158">Yanıt üreten ilk biçimlendirici bulmayı dener.</span><span class="sxs-lookup"><span data-stu-id="606cd-158">Tries to find the first formatter that can produce a response.</span></span>

<span data-ttu-id="606cd-159">İstenen biçim için bir biçimlendirici yapılandırılmamışsa, nesneyi biçimlendirebileceğini ilk biçimlendirici kullanılır.</span><span class="sxs-lookup"><span data-stu-id="606cd-159">If no formatter is configured for the requested format, the first formatter that can format the object is used.</span></span> <span data-ttu-id="606cd-160">İstekte hiçbir `Accept` başlık görünürse:</span><span class="sxs-lookup"><span data-stu-id="606cd-160">If no `Accept` header appears in the request:</span></span>

* <span data-ttu-id="606cd-161">Nesneyi işleyebilen ilk biçimlendirici, yanıtı seri hale getirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="606cd-161">The first formatter that can handle the object is used to serialize the response.</span></span>
* <span data-ttu-id="606cd-162">Hiçbir anlaşma gerçekleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="606cd-162">There isn't any negotiation taking place.</span></span> <span data-ttu-id="606cd-163">Sunucu hangi biçimin döneceğine karar verir.</span><span class="sxs-lookup"><span data-stu-id="606cd-163">The server is determining what format to return.</span></span>

<span data-ttu-id="606cd-164">Accept üst bilgisi içeriyorsa `*/*`üstbilgi, true <xref:Microsoft.AspNetCore.Mvc.MvcOptions>olarak ayarlanmadığı müddetçe `RespectBrowserAcceptHeader` yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="606cd-164">If the Accept header contains `*/*`, the Header is ignored unless `RespectBrowserAcceptHeader` is set to true on <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="606cd-165">Tarayıcılar ve içerik anlaşması</span><span class="sxs-lookup"><span data-stu-id="606cd-165">Browsers and content negotiation</span></span>

<span data-ttu-id="606cd-166">Tipik API istemcilerinin aksine, Web tarayıcıları üst `Accept` bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="606cd-166">Unlike typical API clients, web browsers supply `Accept` headers.</span></span> <span data-ttu-id="606cd-167">Web tarayıcısı, joker karakterler dahil olmak üzere birçok biçim belirtir.</span><span class="sxs-lookup"><span data-stu-id="606cd-167">Web browser specify many formats, including wildcards.</span></span> <span data-ttu-id="606cd-168">Varsayılan olarak, Framework isteğin bir tarayıcıdan geldiğini algıladığında:</span><span class="sxs-lookup"><span data-stu-id="606cd-168">By default, when the framework detects that the request is coming from a browser:</span></span>

* <span data-ttu-id="606cd-169">`Accept` Üst bilgi yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="606cd-169">The `Accept` header is ignored.</span></span>
* <span data-ttu-id="606cd-170">Aksi yapılandırılmadığı takdirde içerik JSON içinde döndürülür.</span><span class="sxs-lookup"><span data-stu-id="606cd-170">The content is returned in JSON, unless otherwise configured.</span></span>

<span data-ttu-id="606cd-171">Bu, API 'Leri tükettiren tarayıcılarda daha tutarlı bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="606cd-171">This provides a more consistent experience across browsers when consuming APIs.</span></span>

<span data-ttu-id="606cd-172">Bir uygulamayı tarayıcı onay üstbilgilerini kabul edecek şekilde yapılandırmak için, <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> şu `true`şekilde ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="606cd-172">To configure an app to honor browser accept headers, set <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> to `true`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a><span data-ttu-id="606cd-173">Biçimleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="606cd-173">Configure formatters</span></span>

<span data-ttu-id="606cd-174">Ek biçimleri desteklemesi gereken uygulamalar uygun NuGet paketlerini ekleyebilir ve desteği yapılandırabilir.</span><span class="sxs-lookup"><span data-stu-id="606cd-174">Apps that need to support additional formats can add the appropriate NuGet packages and configure support.</span></span> <span data-ttu-id="606cd-175">Giriş ve çıkış için ayrı biçimlendirme vardır.</span><span class="sxs-lookup"><span data-stu-id="606cd-175">There are separate formatters for input and output.</span></span> <span data-ttu-id="606cd-176">Giriş formatlayıcıları [model bağlama](xref:mvc/models/model-binding)tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="606cd-176">Input formatters are used by [Model Binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="606cd-177">Çıkış biçimleri, yanıtları biçimlendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="606cd-177">Output formatters are used to format responses.</span></span> <span data-ttu-id="606cd-178">Özel bir biçimlendirici oluşturma hakkında daha fazla bilgi için bkz. [özel Formatlayıcılar](xref:web-api/advanced/custom-formatters).</span><span class="sxs-lookup"><span data-stu-id="606cd-178">For information on creating a custom formatter, see [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a><span data-ttu-id="606cd-179">XML biçimi desteği ekle</span><span class="sxs-lookup"><span data-stu-id="606cd-179">Add XML format support</span></span>

<span data-ttu-id="606cd-180">Kullanılarak <xref:System.Xml.Serialization.XmlSerializer> uygulanan XML formatlayıcıları, çağırarak <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="606cd-180">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

<span data-ttu-id="606cd-181">Yukarıdaki kod, kullanılarak `XmlSerializer`sonuçları seri hale getirir.</span><span class="sxs-lookup"><span data-stu-id="606cd-181">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="606cd-182">Önceki kodu kullanırken, denetleyici yöntemleri isteğin `Accept` üstbilgisine göre uygun biçimi döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="606cd-182">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="606cd-183">System. Text. JSON tabanlı formatlayıcıları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="606cd-183">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="606cd-184">Tabanlı formatlayıcılar `System.Text.Json`için özellikler kullanılarak `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="606cd-184">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span></span>

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="606cd-185">İşlem başına temelinde çıkış serileştirme seçenekleri kullanılarak `JsonResult`yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="606cd-185">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="606cd-186">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="606cd-186">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="606cd-187">Newtonsoft. JSON tabanlı JSON biçimi desteği ekleyin</span><span class="sxs-lookup"><span data-stu-id="606cd-187">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="606cd-188">ASP.NET Core 3,0 ' dan önce, varsayılan olarak kullanılan JSON formatlayıcıları `Newtonsoft.Json` paket kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="606cd-188">Prior to ASP.NET Core 3.0, the default used JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="606cd-189">ASP.NET Core 3,0 veya sonraki bir sürümde, varsayılan JSON formatlayıcıları temel `System.Text.Json`alınır.</span><span class="sxs-lookup"><span data-stu-id="606cd-189">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="606cd-190">Tabanlı formatlayıcılar ve özellikler için `Newtonsoft.Json` destek, [Microsoft. aspnetcore. Mvc. newtonsoftjson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet paketini yükleyerek ve içinde `Startup.ConfigureServices`yapılandırılarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="606cd-190">Support for `Newtonsoft.Json` based formatters and features is available by installing the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

<span data-ttu-id="606cd-191">Bazı özellikler, `Newtonsoft.Json`tabanlı formatlayıcılar ile `System.Text.Json`düzgün çalışmayabilir ve tabanlı formatlara bir başvuru gerektirir.</span><span class="sxs-lookup"><span data-stu-id="606cd-191">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters.</span></span> <span data-ttu-id="606cd-192">Uygulama şu durumlarda `Newtonsoft.Json`tabanlı formatlayıcıları kullanmaya devam edin:</span><span class="sxs-lookup"><span data-stu-id="606cd-192">Continue using the `Newtonsoft.Json`-based formatters if the app:</span></span>

* <span data-ttu-id="606cd-193">Öznitelikleri `Newtonsoft.Json` kullanır.</span><span class="sxs-lookup"><span data-stu-id="606cd-193">Uses `Newtonsoft.Json` attributes.</span></span> <span data-ttu-id="606cd-194">Örneğin, `[JsonProperty]` veya `[JsonIgnore]`.</span><span class="sxs-lookup"><span data-stu-id="606cd-194">For example, `[JsonProperty]` or `[JsonIgnore]`.</span></span>
* <span data-ttu-id="606cd-195">Serileştirme ayarlarını özelleştirir.</span><span class="sxs-lookup"><span data-stu-id="606cd-195">Customizes the serialization settings.</span></span>
* <span data-ttu-id="606cd-196">, Tarafından `Newtonsoft.Json` sağlanan özellikleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="606cd-196">Relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="606cd-197">Yapılandırır `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span><span class="sxs-lookup"><span data-stu-id="606cd-197">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="606cd-198">ASP.NET Core 3,0 ' dan önce `JsonResult.SerializerSettings` , öğesine `Newtonsoft.Json`özgü bir `JsonSerializerSettings` örneğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="606cd-198">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="606cd-199">[Openapı](<xref:tutorials/web-api-help-pages-using-swagger>) belgeleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="606cd-199">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

<span data-ttu-id="606cd-200">Tabanlı formatlayıcılar `Newtonsoft.Json`için özellikler şu kullanılarak `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="606cd-200">Features for the `Newtonsoft.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span></span>

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefautlContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="606cd-201">İşlem başına temelinde çıkış serileştirme seçenekleri kullanılarak `JsonResult`yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="606cd-201">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="606cd-202">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="606cd-202">For example:</span></span>

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

### <a name="add-xml-format-support"></a><span data-ttu-id="606cd-203">XML biçimi desteği ekle</span><span class="sxs-lookup"><span data-stu-id="606cd-203">Add XML format support</span></span>

<span data-ttu-id="606cd-204">XML biçimlendirme, [Microsoft. AspNetCore. Mvc. Formatters. xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet paketini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="606cd-204">XML formatting requires the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

<span data-ttu-id="606cd-205">Kullanılarak <xref:System.Xml.Serialization.XmlSerializer> uygulanan XML formatlayıcıları, çağırarak <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="606cd-205">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

<span data-ttu-id="606cd-206">Yukarıdaki kod, kullanılarak `XmlSerializer`sonuçları seri hale getirir.</span><span class="sxs-lookup"><span data-stu-id="606cd-206">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="606cd-207">Önceki kodu kullanırken, denetleyici yöntemleri isteğin `Accept` üstbilgisine göre uygun biçimi döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="606cd-207">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

::: moniker-end

### <a name="specify-a-format"></a><span data-ttu-id="606cd-208">Biçim belirtin</span><span class="sxs-lookup"><span data-stu-id="606cd-208">Specify a format</span></span>

<span data-ttu-id="606cd-209">Yanıt biçimlerini kısıtlamak için [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filtreyi uygulayın.</span><span class="sxs-lookup"><span data-stu-id="606cd-209">To restrict the response formats, apply the [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter.</span></span> <span data-ttu-id="606cd-210">Çoğu [filtre](xref:mvc/controllers/filters) `[Produces]` gibi eylem, denetleyici veya genel kapsamda de uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="606cd-210">Like most [Filters](xref:mvc/controllers/filters), `[Produces]` can be applied at the action, controller, or global scope:</span></span>

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

<span data-ttu-id="606cd-211">Önceki [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filtre:</span><span class="sxs-lookup"><span data-stu-id="606cd-211">The preceding [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter:</span></span>

* <span data-ttu-id="606cd-212">Denetleyici içindeki tüm eylemleri JSON biçimli yanıtları döndürecek şekilde zorlar.</span><span class="sxs-lookup"><span data-stu-id="606cd-212">Forces all actions within the controller to return JSON-formatted responses.</span></span>
* <span data-ttu-id="606cd-213">Diğer formatlayıcılar yapılandırıldıysa ve istemci farklı bir biçim belirtiyorsa JSON döndürülür.</span><span class="sxs-lookup"><span data-stu-id="606cd-213">If other formatters are configured and the client specifies a different format, JSON is returned.</span></span>

<span data-ttu-id="606cd-214">Daha fazla bilgi için bkz. [Filtreler](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="606cd-214">For more information, see [Filters](xref:mvc/controllers/filters).</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="606cd-215">Özel durum formatları</span><span class="sxs-lookup"><span data-stu-id="606cd-215">Special case formatters</span></span>

<span data-ttu-id="606cd-216">Bazı özel durumlar, yerleşik formatlayıcılar kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="606cd-216">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="606cd-217">Varsayılan olarak, `string` dönüş türleri *metin/düz* olarak biçimlendirilir ( `Accept` üst bilgi ile isteniyorsa*metin/html* ).</span><span class="sxs-lookup"><span data-stu-id="606cd-217">By default, `string` return types are formatted as *text/plain* (*text/html* if requested via the `Accept` header).</span></span> <span data-ttu-id="606cd-218">Bu davranış, <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter>' ı kaldırılarak silinebilir.</span><span class="sxs-lookup"><span data-stu-id="606cd-218">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter>.</span></span> <span data-ttu-id="606cd-219">Biçimlendiriciler `Configure` yönteminde kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="606cd-219">Formatters are removed in the `Configure` method.</span></span> <span data-ttu-id="606cd-220">Bir model nesne dönüş türü döndürme `204 No Content` `null`sırasında döndürülen eylemler.</span><span class="sxs-lookup"><span data-stu-id="606cd-220">Actions that have a model object return type return `204 No Content` when returning `null`.</span></span> <span data-ttu-id="606cd-221">Bu davranış, <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>' ı kaldırılarak silinebilir.</span><span class="sxs-lookup"><span data-stu-id="606cd-221">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span></span> <span data-ttu-id="606cd-222">Aşağıdaki kod `TextOutputFormatter` ve `HttpNoContentOutputFormatter`öğesini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="606cd-222">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end

<span data-ttu-id="606cd-223">`TextOutputFormatter`Olmadan, `string` dönüş türleri döndürülür `406 Not Acceptable`.</span><span class="sxs-lookup"><span data-stu-id="606cd-223">Without the `TextOutputFormatter`, `string` return types return `406 Not Acceptable`.</span></span> <span data-ttu-id="606cd-224">Bir XML biçimlendiricisi varsa, bu, kaldırılırsa `string` dönüş türlerini `TextOutputFormatter` biçimlendirir.</span><span class="sxs-lookup"><span data-stu-id="606cd-224">If an XML formatter exists, it formats `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="606cd-225">`HttpNoContentOutputFormatter`Olmadan, null nesneler yapılandırılmış biçimlendirici kullanılarak biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="606cd-225">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="606cd-226">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="606cd-226">For example:</span></span>

* <span data-ttu-id="606cd-227">JSON biçimlendiricisi, gövdesi `null`olan bir yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="606cd-227">The JSON formatter returns a response with a body of `null`.</span></span>
* <span data-ttu-id="606cd-228">XML biçimlendiricisi özniteliği `xsi:nil="true"` ayarlanmış bir boş XML öğesi döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="606cd-228">The XML formatter returns an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="606cd-229">Yanıt biçimi URL eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="606cd-229">Response format URL mappings</span></span>

<span data-ttu-id="606cd-230">İstemciler URL 'nin bir parçası olarak belirli bir biçim talep edebilir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="606cd-230">Clients can request a particular format as part of the URL, for example:</span></span>

* <span data-ttu-id="606cd-231">Sorgu dizesinde veya yolun bir bölümünde.</span><span class="sxs-lookup"><span data-stu-id="606cd-231">In the query string or part of the path.</span></span>
* <span data-ttu-id="606cd-232">. Xml veya. JSON gibi formata özgü bir dosya uzantısı kullanarak.</span><span class="sxs-lookup"><span data-stu-id="606cd-232">By using a format-specific file extension such as .xml or .json.</span></span>

<span data-ttu-id="606cd-233">İstek yolundan eşleme, API 'nin kullandığı rotada belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="606cd-233">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="606cd-234">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="606cd-234">For example:</span></span>

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="606cd-235">Önceki yol, istenen biçimin isteğe bağlı bir dosya uzantısı olarak belirtilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="606cd-235">The preceding route allows the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="606cd-236">[`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) Özniteliği ,`RouteData` içindeki biçim değerinin varlığını denetler ve yanıt oluşturulduğunda yanıt biçimini uygun biçimlendirici ile eşler.</span><span class="sxs-lookup"><span data-stu-id="606cd-236">The [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) attribute checks for the existence of the format value in the `RouteData` and maps the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="606cd-237">Yolu</span><span class="sxs-lookup"><span data-stu-id="606cd-237">Route</span></span>        |             <span data-ttu-id="606cd-238">Biçimlendirici</span><span class="sxs-lookup"><span data-stu-id="606cd-238">Formatter</span></span>              |
|------------------------|------------------------------------|
|   `/api/products/5`    |    <span data-ttu-id="606cd-239">Varsayılan çıkış biçimlendiricisi</span><span class="sxs-lookup"><span data-stu-id="606cd-239">The default output formatter</span></span>    |
| `/api/products/5.json` | <span data-ttu-id="606cd-240">JSON biçimlendiricisi (yapılandırıldıysa)</span><span class="sxs-lookup"><span data-stu-id="606cd-240">The JSON formatter (if configured)</span></span> |
| `/api/products/5.xml`  | <span data-ttu-id="606cd-241">XML biçimlendiricisi (yapılandırıldıysa)</span><span class="sxs-lookup"><span data-stu-id="606cd-241">The XML formatter (if configured)</span></span>  |
