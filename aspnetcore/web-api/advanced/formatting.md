---
title: ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirme
author: ardalis
description: ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirmeyi öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 05/29/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: 8bee4efdae5341ddab5bd3aec278ecfef37f0c08
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082358"
---
<!-- DO NOT EDIT BEFORE https://github.com/aspnet/AspNetCore.Docs/pull/12077 MERGES -->
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="1ad3d-103">ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirme</span><span class="sxs-lookup"><span data-stu-id="1ad3d-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="1ad3d-104">[Steve Smith](https://ardalis.com/) tarafından</span><span class="sxs-lookup"><span data-stu-id="1ad3d-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1ad3d-105">ASP.NET Core MVC, yanıt verilerini biçimlendirme, sabit biçimleri kullanma veya istemci belirtimlerine yanıt verme için yerleşik desteğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="1ad3d-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1ad3d-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="1ad3d-107">Formata özgü eylem sonuçları</span><span class="sxs-lookup"><span data-stu-id="1ad3d-107">Format-Specific Action Results</span></span>

<span data-ttu-id="1ad3d-108">Bazı eylem sonuç türleri, `JsonResult` ve `ContentResult`gibi belirli bir biçime özgüdür.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="1ad3d-109">Eylemler, her zaman belirli bir şekilde biçimlendirilen belirli sonuçları döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="1ad3d-110">Örneğin, döndürme `JsonResult` , istemci tercihlerinden bağımsız olarak JSON biçimli verileri döndürür.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="1ad3d-111">Benzer şekilde, döndürme `ContentResult` düz metin biçimli dize verileri döndürür (gibi, yalnızca bir dize döndürür).</span><span class="sxs-lookup"><span data-stu-id="1ad3d-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="1ad3d-112">Belirli bir türü döndürmek için bir eylem gerekli değildir; MVC tüm nesne dönüş değerlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="1ad3d-113">Bir eylem bir `IActionResult` uygulama döndürürse ve denetleyici öğesinden `Controller`devralırsa, geliştiricilerin birçok seçeneğe karşılık gelen birçok yardımcı yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="1ad3d-114">Tür olmayan `IActionResult` nesneleri döndüren eylemlerin sonuçları, uygun `IOutputFormatter` uygulama kullanılarak serileştirilir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="1ad3d-115">Verileri, `Controller` temel sınıftan devralan bir denetleyiciden belirli bir biçimde döndürmek için, JSON döndürmek ve `Content` düz metin için yerleşik yardımcı yöntemi `Json` kullanın.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="1ad3d-116">Eylem yönteminiz, belirli sonuç türünü (örneğin, `JsonResult`) ya `IActionResult`da döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="1ad3d-117">JSON biçimli veriler döndürülüyor:</span><span class="sxs-lookup"><span data-stu-id="1ad3d-117">Returning JSON-formatted data:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="1ad3d-118">Bu eylemden örnek yanıt:</span><span class="sxs-lookup"><span data-stu-id="1ad3d-118">Sample response from this action:</span></span>

![Microsoft Edge 'de, yanıtın Içerik türünü gösteren Geliştirici Araçları ağ sekmesi (uygulamanın/JSON)](formatting/_static/json-response.png)

<span data-ttu-id="1ad3d-120">Yanıtın `application/json`içerik türünün hem ağ istekleri listesinde hem de yanıt üstbilgileri bölümünde gösterildiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="1ad3d-121">Ayrıca, Istek üst bilgilerinde onay üst bilgisinde tarayıcı (Bu durumda, Microsoft Edge) tarafından sunulan seçenekler listesini de göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="1ad3d-122">Geçerli teknik bu üstbilgiyi yok sayıyor; Aşağıda ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="1ad3d-123">Düz metin biçimli verileri döndürmek için `ContentResult` `Content` ve yardımcısını kullanın:</span><span class="sxs-lookup"><span data-stu-id="1ad3d-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="1ad3d-124">Bu eylemden bir yanıt:</span><span class="sxs-lookup"><span data-stu-id="1ad3d-124">A response from this action:</span></span>

![Microsoft Edge 'de Geliştirici Araçları yanıtın Içerik türünü gösteren ağ sekmesi metin/düz](formatting/_static/text-response.png)

<span data-ttu-id="1ad3d-126">Bu örnekte `Content-Type` `text/plain`döndürülen ' i aklınızda edin.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="1ad3d-127">Yalnızca bir dize yanıt türü kullanarak aynı davranışa de ulaşabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1ad3d-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="1ad3d-128">Birden çok dönüş türü veya seçeneği olan önemsiz olmayan eylemler için (örneğin, gerçekleştirilen işlemlerin sonucuna göre farklı http durum kodları), dönüş türü olarak tercih `IActionResult` edin.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="1ad3d-129">İçerik anlaşması</span><span class="sxs-lookup"><span data-stu-id="1ad3d-129">Content Negotiation</span></span>

<span data-ttu-id="1ad3d-130">İstemci bir [Accept üst bilgisi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)belirttiğinde içerik anlaşması (kısaca*conneg* ) oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="1ad3d-131">ASP.NET Core MVC tarafından kullanılan varsayılan biçim JSON 'dir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="1ad3d-132">İçerik anlaşması tarafından `ObjectResult`uygulanır.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="1ad3d-133">Ayrıca, yardımcı metotlarından döndürülen durum koduna özgü eylem sonuçlarının (tümü, temel `ObjectResult`alınan) içinde de yerleşiktir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-133">It's also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="1ad3d-134">Ayrıca bir model türü (veri aktarım türü olarak tanımladığınız bir sınıf) döndürebilir ve çerçeve sizin için otomatik olarak bir `ObjectResult` ile kaydırılır.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="1ad3d-135">Aşağıdaki eylem yöntemi `Ok` ve `NotFound` yardımcı yöntemlerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="1ad3d-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="1ad3d-136">Başka bir biçim istenmediği ve sunucu istenen biçimi döndüremediğinde JSON biçimli bir yanıt döndürülür.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="1ad3d-137">Bir Accept üst bilgisi içeren bir istek oluşturmak ve başka bir biçim belirtmek için [Fiddler](https://www.telerik.com/fiddler) gibi bir araç kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-137">You can use a tool like [Fiddler](https://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="1ad3d-138">Bu durumda, sunucuda istenen biçimde yanıt üretemeyen bir *biçimlendirici* varsa, sonuç istemci tarafından tercih edilen biçimde döndürülür.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Uygulama/XML Accept üst bilgisi değeri ile el ile oluşturulmuş bir GET isteği gösteren Fiddler konsolu](formatting/_static/fiddler-composer.png)

<span data-ttu-id="1ad3d-140">Yukarıdaki ekran görüntüsünde, bir istek oluşturmak için Fiddler Oluşturucu kullanılmıştır, belirterek `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="1ad3d-141">Varsayılan olarak, ASP.NET Core MVC yalnızca JSON 'ı destekler, bu nedenle başka bir biçim belirtildiğinde bile döndürülen sonuç hala JSON biçimli olur.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="1ad3d-142">Sonraki bölümde nasıl ek biçimlendirme ekleneceğini göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="1ad3d-143">Denetleyici eylemleri Pocos (düz eski CLR nesneleri) döndürebilir, bu durumda ASP.NET Core MVC nesneyi sarmalayan sizin `ObjectResult` için otomatik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET Core MVC automatically creates an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="1ad3d-144">İstemci, biçimlendirilen serileştirilmiş nesneyi alır (JSON biçimi varsayılandır; XML veya diğer biçimleri yapılandırabilirsiniz).</span><span class="sxs-lookup"><span data-stu-id="1ad3d-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="1ad3d-145">Döndürülen nesne ise `null`, Framework bir `204 No Content` yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="1ad3d-146">Nesne türü döndürülüyor:</span><span class="sxs-lookup"><span data-stu-id="1ad3d-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="1ad3d-147">Örnekte, geçerli bir yazar diğer adı için bir istek yazarın verileriyle 200 OK yanıtı alır.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="1ad3d-148">Geçersiz bir diğer ad isteği bir 204 Içerik yanıtı alacak.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="1ad3d-149">XML ve JSON biçimlerinde yanıtı gösteren ekran görüntüleri aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="1ad3d-150">İçerik anlaşma Işlemi</span><span class="sxs-lookup"><span data-stu-id="1ad3d-150">Content Negotiation Process</span></span>

<span data-ttu-id="1ad3d-151">İçerik *anlaşması* yalnızca, istekte bir `Accept` başlık görünürse gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="1ad3d-152">Bir istek bir Accept üst bilgisi içerdiğinde, çerçeve kabul üst bilgisindeki medya türlerini tercih sırasına göre numaralandırır ve Accept üst bilgisi tarafından belirtilen biçimlerden birinde yanıt üreten bir biçimlendirici bulmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="1ad3d-153">İstemcinin isteğini karşılayabilen bir biçimlendirici bulunmazsa, çerçeve bir yanıt üreten ilk biçimlendirici bulmayı dener (Geliştirici, bunun yerine 406 döndürmek `MvcOptions` için seçeneği yapılandırmamışsa).</span><span class="sxs-lookup"><span data-stu-id="1ad3d-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="1ad3d-154">İstek XML belirtiyorsa, ancak XML biçimlendirici yapılandırılmamışsa, JSON biçimlendiricisi kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="1ad3d-155">Daha genel olarak, istenen biçimi sağlayabilecek bir biçimlendirici yapılandırılmamışsa, nesneyi biçimlendirebileceğini ilk biçimlendirici kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="1ad3d-156">Üst bilgi verilmezse, döndürülecek nesneyi işleyebilen ilk biçimlendirici, yanıtı seri hale getirmek için kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="1ad3d-157">Bu durumda, hiçbir anlaşma gerçekleşmez; sunucu hangi biçimi kullanacağınızı belirliyor.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="1ad3d-158">Accept üst bilgisi içeriyorsa `*/*`üstbilgi, true `MvcOptions`olarak ayarlanmadığı müddetçe `RespectBrowserAcceptHeader` yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="1ad3d-159">Tarayıcılar ve Içerik anlaşması</span><span class="sxs-lookup"><span data-stu-id="1ad3d-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="1ad3d-160">Tipik API istemcilerinin aksine, Web tarayıcıları joker karakterler de `Accept` dahil olmak üzere geniş bir biçim dizisi içeren Üstbilgiler sağlamayı eğilimindedir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="1ad3d-161">Varsayılan olarak, çerçeve isteğin bir tarayıcıdan geldiğini algıladığında, `Accept` üstbilgiyi yoksayar ve bunun yerine uygulamanın yapılandırılmış varsayılan biçimindeki içeriği döndürür (aksi yapılandırılmamışsa JSON).</span><span class="sxs-lookup"><span data-stu-id="1ad3d-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="1ad3d-162">Bu, API 'Leri kullanmak için farklı tarayıcılar kullanırken daha tutarlı bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="1ad3d-163">Uygulamanızın tarayıcı için kabul etme üst `RespectBrowserAcceptHeader` bilgilerini benimseme seçeneğini tercih ediyorsanız, bunu *Startup.cs*içindeki `ConfigureServices` yönteminde olarak `true` ayarlayarak MVC 'nin yapılandırmasının bir parçası olarak yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="1ad3d-164">Biçimleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="1ad3d-164">Configuring Formatters</span></span>

<span data-ttu-id="1ad3d-165">Uygulamanızın varsayılan JSON dışında ek biçimleri desteklemesi gerekiyorsa, NuGet paketleri ekleyebilir ve MVC 'yi destekleyecek şekilde yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="1ad3d-166">Giriş ve çıkış için ayrı biçimlendirme vardır.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="1ad3d-167">Giriş formatlayıcıları [model bağlama](xref:mvc/models/model-binding)tarafından kullanılır; çıkış biçimleri, yanıtları biçimlendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-167">Input formatters are used by [Model Binding](xref:mvc/models/model-binding); output formatters are used to format responses.</span></span> <span data-ttu-id="1ad3d-168">Ayrıca, [özel biçimleri](xref:web-api/advanced/custom-formatters)de yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-168">You can also configure [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="1ad3d-169">System. Text. JSON tabanlı formatlayıcıları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="1ad3d-169">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="1ad3d-170">Tabanlı formatlayıcılar `System.Text.Json`için özellikler kullanılarak `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-170">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span></span>

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="1ad3d-171">İşlem başına temelinde çıkış serileştirme seçenekleri kullanılarak `JsonResult`yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-171">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="1ad3d-172">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1ad3d-172">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="1ad3d-173">Newtonsoft. JSON tabanlı JSON biçimi desteği ekleyin</span><span class="sxs-lookup"><span data-stu-id="1ad3d-173">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="1ad3d-174">ASP.NET Core 3,0 ' dan önce, MVC, `Newtonsoft.Json` paket kullanılarak uygulanan JSON formatlarını kullanmaya göre varsayılan.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-174">Prior to ASP.NET Core 3.0, MVC defaulted to using JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="1ad3d-175">ASP.NET Core 3,0 veya sonraki bir sürümde, varsayılan JSON formatlayıcıları temel `System.Text.Json`alınır.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-175">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="1ad3d-176">[Microsoft. aspnetcore. Mvc. newtonsoftjson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet paketini yükleyerek ve ' de `Startup.ConfigureServices`yapılandırarak, tabanlı biçimlendirme ve özellikler için `Newtonsoft.Json`destek bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-176">Support for `Newtonsoft.Json`-based formatters and features is available by installing the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddControllers()
    .AddNewtonsoftJson();
```

<span data-ttu-id="1ad3d-177">Bazı özellikler, tabanlı formatlayıcılar ile `System.Text.Json`düzgün çalışmayabilir ve ASP.NET Core 3,0 sürümü için tabanlı formatlayıcılar `Newtonsoft.Json`için bir başvuru gerektirir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-177">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters for the ASP.NET Core 3.0 release.</span></span> <span data-ttu-id="1ad3d-178">ASP.NET Core 3,0 veya `Newtonsoft.Json`sonraki bir sürüm uygulamanız varsa, tabanlı formatlayıcıları kullanmaya devam edin:</span><span class="sxs-lookup"><span data-stu-id="1ad3d-178">Continue using the `Newtonsoft.Json`-based formatters if your ASP.NET Core 3.0 or later app:</span></span>

* <span data-ttu-id="1ad3d-179">Öznitelikleri `Newtonsoft.Json` kullanır (örneğin, `[JsonProperty]` veya `[JsonIgnore]`), `Newtonsoft.Json` serileştirme ayarlarını özelleştirir veya sağlayan özelliklere bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-179">Uses `Newtonsoft.Json` attributes (for example, `[JsonProperty]` or `[JsonIgnore]`), customizes the serialization settings, or relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="1ad3d-180">Yapılandırır `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-180">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="1ad3d-181">ASP.NET Core 3,0 ' dan önce `JsonResult.SerializerSettings` , öğesine `Newtonsoft.Json`özgü bir `JsonSerializerSettings` örneğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-181">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="1ad3d-182">[Openapı](<xref:tutorials/web-api-help-pages-using-swagger>) belgeleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-182">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

<span data-ttu-id="1ad3d-183">Tabanlı formatlayıcılar `Newtonsoft.Json`için özellikler şu kullanılarak `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="1ad3d-183">Features for the `Newtonsoft.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span></span>

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefautlContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="1ad3d-184">İşlem başına temelinde çıkış serileştirme seçenekleri kullanılarak `JsonResult`yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-184">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="1ad3d-185">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1ad3d-185">For example:</span></span>

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

### <a name="add-xml-format-support"></a><span data-ttu-id="1ad3d-186">XML biçimi desteği ekle</span><span class="sxs-lookup"><span data-stu-id="1ad3d-186">Add XML format support</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="1ad3d-187">ASP.NET Core 2,2 veya önceki sürümlerde XML biçimlendirme desteği eklemek için [Microsoft. AspNetCore. Mvc. Formatters. xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet paketini yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-187">To add XML formatting support in ASP.NET Core 2.2 or earlier, install the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

::: moniker-end

<span data-ttu-id="1ad3d-188">Kullanılarak `System.Xml.Serialization.XmlSerializer` uygulanan XML formatlayıcıları, ' de `Startup.ConfigureServices`çağırarak <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="1ad3d-188">XML formatters implemented using `System.Xml.Serialization.XmlSerializer` can be configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="1ad3d-189">Alternatif olarak, kullanılarak `System.Runtime.Serialization.DataContractSerializer` uygulanan XML formatlayıcıları, ' de `Startup.ConfigureServices`çağırarak <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="1ad3d-189">Alternatively, XML formatters implemented using `System.Runtime.Serialization.DataContractSerializer` can be configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddXmlDataContractSerializerFormatters();
```

<span data-ttu-id="1ad3d-190">XML biçimlendirme desteğini ekledikten sonra, bu Fiddler örneğinde gösterildiği gibi, denetleyici yöntemlerinizi isteğin `Accept` üstbilgisine göre uygun biçimi döndürmelidir:</span><span class="sxs-lookup"><span data-stu-id="1ad3d-190">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Fiddler konsolu: İsteğin ham sekmesi, Accept üst bilgisini, application/xml değerini gösterir.](formatting/_static/xml-response.png)

<span data-ttu-id="1ad3d-193">, Ham GET isteğinin bir `Accept: application/xml` başlık kümesiyle yapıldığını Inspectors sekmesinde görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-193">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="1ad3d-194">Yanıt bölmesi `Content-Type: application/xml` üstbilgiyi gösterir `Author` ve nesne XML olarak serileştirilir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-194">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="1ad3d-195">`Accept` Üst bilgide belirtmek `application/json` üzere isteği değiştirmek için besteci sekmesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-195">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="1ad3d-196">İsteği yürütün ve yanıt JSON olarak biçimlendirilir:</span><span class="sxs-lookup"><span data-stu-id="1ad3d-196">Execute the request, and the response will be formatted as JSON:</span></span>

![Fiddler konsolu: İsteğin ham sekmesi, Accept üst bilgisi değerini bir Application/JSON olarak gösterir.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="1ad3d-199">Bu ekran görüntüsünde, istek bir başlık `Accept: application/json` olduğunu ve yanıtın onunla `Content-Type`aynı olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-199">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="1ad3d-200">`Author` Nesne, yanıtın gövdesinde, JSON biçiminde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-200">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="1ad3d-201">Belirli bir biçimi zorlama</span><span class="sxs-lookup"><span data-stu-id="1ad3d-201">Forcing a Particular Format</span></span>

<span data-ttu-id="1ad3d-202">Belirli bir eylem için yanıt biçimlerini kısıtlamak isterseniz, `[Produces]` filtre uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-202">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="1ad3d-203">`[Produces]` Filtre, belirli bir eyleme (veya denetleyiciye) ait yanıt biçimlerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-203">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="1ad3d-204">Çoğu [filtre](xref:mvc/controllers/filters)gibi, bu işlem eylem, denetleyici veya genel kapsamda uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-204">Like most [Filters](xref:mvc/controllers/filters), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="1ad3d-205">Filtre, uygulama için başka formatlayıcılar yapılandırılmış `AuthorsController` olsa bile `Accept` ve istemci farklı bir başlık sağladıysa, içindeki tüm eylemleri JSON biçimli yanıtları döndürecek şekilde zorlayacaktır. `[Produces]` formatını.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-205">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="1ad3d-206">Genel olarak filtrelerin nasıl uygulanacağı dahil olmak üzere daha fazla bilgi için [filtrelere](xref:mvc/controllers/filters) bakın.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-206">See [Filters](xref:mvc/controllers/filters) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="1ad3d-207">Özel durum formatları</span><span class="sxs-lookup"><span data-stu-id="1ad3d-207">Special Case Formatters</span></span>

<span data-ttu-id="1ad3d-208">Bazı özel durumlar, yerleşik formatlayıcılar kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-208">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="1ad3d-209">Varsayılan olarak, `string` dönüş türleri *metin/düz* olarak biçimlendirilir (üst bilgi ile `Accept` isteniyorsa*metin/html* ).</span><span class="sxs-lookup"><span data-stu-id="1ad3d-209">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="1ad3d-210">Bu davranış, `TextOutputFormatter`' ı kaldırılarak kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-210">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="1ad3d-211">*Startup.cs* (aşağıda gösterildiği gibi) `Configure` yöntemindeki formatlayıcıları kaldırırsınız.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-211">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="1ad3d-212">Model nesnesi dönüş türüne sahip eylemler, döndürme `null`sırasında bir 204 içerik yanıtı döndürmez.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-212">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="1ad3d-213">Bu davranış, `HttpNoContentOutputFormatter`' ı kaldırılarak kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-213">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="1ad3d-214">Aşağıdaki kod `TextOutputFormatter` ve `HttpNoContentOutputFormatter`öğesini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-214">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="1ad3d-215">`TextOutputFormatter` Olmadan,dönüştürleri406dönüş`string` kabul edilemez, örneğin.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-215">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="1ad3d-216">Bir XML biçimlendiricisi varsa, kaldırılırsa dönüş türlerini `string` `TextOutputFormatter` biçimlendirir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-216">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="1ad3d-217">`HttpNoContentOutputFormatter`Olmadan, null nesneler yapılandırılmış biçimlendirici kullanılarak biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-217">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="1ad3d-218">Örneğin, JSON biçimlendiricisi yalnızca gövdesi `null`olan bir yanıt döndürür, XML biçimlendirici öznitelik `xsi:nil="true"` kümesi ile boş bir XML öğesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-218">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="1ad3d-219">Yanıt biçimi URL eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="1ad3d-219">Response Format URL Mappings</span></span>

<span data-ttu-id="1ad3d-220">İstemciler, URL 'nin bir parçası olarak, sorgu dizesinde veya yolun bir bölümünde veya. xml veya. JSON gibi formata özgü bir dosya uzantısı kullanarak belirli bir biçim talep edebilir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-220">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="1ad3d-221">İstek yolundan eşleme, API 'nin kullandığı rotada belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-221">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="1ad3d-222">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1ad3d-222">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="1ad3d-223">Bu yol, istenen biçimin isteğe bağlı bir dosya uzantısı olarak belirtilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-223">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="1ad3d-224">`[FormatFilter]` Özniteliği ,`RouteData` içindeki biçim değerinin varlığını denetler ve yanıt oluşturulduğu zaman yanıt biçimini uygun biçimlendirici ile eşler.</span><span class="sxs-lookup"><span data-stu-id="1ad3d-224">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="1ad3d-225">Yolu</span><span class="sxs-lookup"><span data-stu-id="1ad3d-225">Route</span></span>            |             <span data-ttu-id="1ad3d-226">Biçimlendirici</span><span class="sxs-lookup"><span data-stu-id="1ad3d-226">Formatter</span></span>              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    <span data-ttu-id="1ad3d-227">Varsayılan çıkış biçimlendiricisi</span><span class="sxs-lookup"><span data-stu-id="1ad3d-227">The default output formatter</span></span>    |
| `/products/GetById/5.json` | <span data-ttu-id="1ad3d-228">JSON biçimlendiricisi (yapılandırıldıysa)</span><span class="sxs-lookup"><span data-stu-id="1ad3d-228">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="1ad3d-229">XML biçimlendiricisi (yapılandırıldıysa)</span><span class="sxs-lookup"><span data-stu-id="1ad3d-229">The XML formatter (if configured)</span></span>  |
