---
title: "ASP.NET Core MVC yanıt verileri biçimlendirme"
author: ardalis
description: "ASP.NET Core mvc'de yanıt verinin nasıl biçimlendirileceğini öğrenin."
keywords: "ASP.NET Core, yanıt verilerini, IOutputFormatter, IActionResult"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c056df45-d013-4826-91a1-4a092bae1ea5
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: abc125a093ff2cd5a38a537ecdfc795ff03e23f7
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2017
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a><span data-ttu-id="8f4ed-104">ASP.NET Core MVC biçimlendirme yanıt verileri giriş</span><span class="sxs-lookup"><span data-stu-id="8f4ed-104">Introduction to formatting response data in ASP.NET Core MVC</span></span>

<span data-ttu-id="8f4ed-105">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="8f4ed-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="8f4ed-106">ASP.NET Core MVC yanıt istemci belirtimlerine veya sabit biçimlerini kullanarak yanıt verileri biçimlendirme için yerleşik desteğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-106">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="8f4ed-107">[Görüntülemek veya örnek Github'dan indirdiğinizde](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span><span class="sxs-lookup"><span data-stu-id="8f4ed-107">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="8f4ed-108">Biçim özel eylem sonuçlarını</span><span class="sxs-lookup"><span data-stu-id="8f4ed-108">Format-Specific Action Results</span></span>

<span data-ttu-id="8f4ed-109">Bazı eylem sonucu türleri gibi belirli bir biçime belirli `JsonResult` ve `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-109">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="8f4ed-110">Eylemler her zaman belirli bir şekilde biçimlendirilmiş belirli sonuçlar döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-110">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="8f4ed-111">Örneğin, döndüren bir `JsonResult` istemci Tercihler bakılmaksızın JSON biçimli verileri döndürür.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-111">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="8f4ed-112">Benzer şekilde, döndüren bir `ContentResult` düz metin biçimli dize verilerini (yalnızca döndüren bir dize olarak) döndürür.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-112">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="8f4ed-113">Belirli bir türü döndürmek için bir eylem gerekli değildir; MVC hiçbir nesne dönüş değeri destekler.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-113">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="8f4ed-114">Bir eylem döndürürse bir `IActionResult` uygulama ve denetleyici devraldığı `Controller`, geliştiricilerin birçok seçenek karşılık gelen çok sayıda yardımcı yöntemler vardır.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-114">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="8f4ed-115">Nesneler döndürmeye Eylemler sonuçlarından olmayan `IActionResult` türleri serileştirilmiş uygun kullanarak `IOutputFormatter` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-115">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="8f4ed-116">Belirli bir biçimde veri öğesinden devralınan bir denetleyici döndürmek için `Controller` temel sınıfı, yerleşik yardımcı yöntemi `Json` JSON dönün ve `Content` düz metin için.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-116">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="8f4ed-117">Eylem yönteminin belirli sonuç türü döndürmelidir (örneğin, `JsonResult`) veya `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-117">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="8f4ed-118">JSON biçimli veriyor:</span><span class="sxs-lookup"><span data-stu-id="8f4ed-118">Returning JSON-formatted data:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="8f4ed-119">Bu eylem yanıttan örnek:</span><span class="sxs-lookup"><span data-stu-id="8f4ed-119">Sample response from this action:</span></span>

![Yanıtın içerik türü gösteren Microsoft edge'de Geliştirici Araçları'nın Ağ sekmesi application/json şeklindedir](formatting/_static/json-response.png)

<span data-ttu-id="8f4ed-121">Yanıtın içerik türü olduğuna dikkat edin `application/json`hem ağ isteklerin listesini ve yanıt üstbilgileri bölümünde gösterilen.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-121">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="8f4ed-122">Ayrıca, tarayıcıda (Bu durumda, Microsoft Edge) istek üstbilgileri bölümünde Accept üstbilgisindeki tarafından sunulan seçeneklerinin listesini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-122">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="8f4ed-123">Geçerli teknik bu başlığı sayıyor; Bu obeying aşağıda ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-123">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="8f4ed-124">Düz metin olarak biçimlendirilmiş verileri döndürmek için kullanmak `ContentResult` ve `Content` Yardımcısı:</span><span class="sxs-lookup"><span data-stu-id="8f4ed-124">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="8f4ed-125">Bu eylem yanıttan:</span><span class="sxs-lookup"><span data-stu-id="8f4ed-125">A response from this action:</span></span>

![Yanıtın içerik türü gösteren Microsoft edge'de Geliştirici Araçları'nın Ağ sekmesi metin/düz değil](formatting/_static/text-response.png)

<span data-ttu-id="8f4ed-127">Bu durumda Not `Content-Type` döndürülen olduğu `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-127">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="8f4ed-128">Ayrıca, yalnızca bir dize yanıt türünü kullanarak bu aynı davranışı elde edebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="8f4ed-128">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="8f4ed-129">Birden çok önemsiz olmayan eylemler için dönüş türleri veya seçenekler (örneğin, farklı HTTP durum kodları sonucuna göre gerçekleştirilen işlemler), tercih ettiğiniz `IActionResult` dönüş türü.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-129">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="8f4ed-130">İçerik anlaşması</span><span class="sxs-lookup"><span data-stu-id="8f4ed-130">Content Negotiation</span></span>

<span data-ttu-id="8f4ed-131">İçerik anlaşması (*conneg* kısaca) oluşur. istemcinin belirttiği bir [Accept üstbilgisi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="8f4ed-131">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="8f4ed-132">ASP.NET Core MVC tarafından kullanılan varsayılan JSON biçimidir.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-132">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="8f4ed-133">İçerik anlaşması tarafından gerçekleştirilir `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-133">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="8f4ed-134">Ayrıca belirli eylem sonuçlarını Yardımcısı yöntemleri döndürülen durum kodu içinde yerleşik (hangi tüm temel `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="8f4ed-134">It is also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="8f4ed-135">Model türü (veri aktarımı türü olarak tanımlanmış bir sınıf) geri dönebilirsiniz ve framework içinde otomatik olarak kaydırılır bir `ObjectResult` sizin için.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-135">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="8f4ed-136">Aşağıdaki bir eylem yöntem `Ok` ve `NotFound` yardımcı yöntemler:</span><span class="sxs-lookup"><span data-stu-id="8f4ed-136">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="8f4ed-137">JSON biçimli bir yanıt, başka bir biçime istendi ve sunucu istenen biçim döndürebilirsiniz sürece döndürülür.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-137">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="8f4ed-138">Gibi bir araç kullanabilirsiniz [Fiddler](http://www.telerik.com/fiddler) bir Accept üstbilgisi içeren bir isteği oluşturun ve başka bir biçim belirtin.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-138">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="8f4ed-139">Sunucusu varsa, bu durumda, bir *biçimlendirici* , istenen biçiminde bir yanıt oluşturabilirsiniz, sonuç istemci tercih edilen biçimde döndürülür.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-139">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![El ile oluşturulan bir gösteren fiddler konsol uygulama/xml bir Accept üstbilgi değeri ile isteği alma](formatting/_static/fiddler-composer.png)

<span data-ttu-id="8f4ed-141">Yukarıdaki ekran görüntüsünde, Fiddler Oluşturucusu bir isteği oluşturmak için kullanılmış belirtme `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-141">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="8f4ed-142">Varsayılan olarak, ASP.NET Core MVC yalnızca JSON, bu nedenle bile destekleyen başka bir biçime belirtildiğinde, hala JSON biçimli döndürülen sonuç.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-142">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="8f4ed-143">Sonraki bölümde ek biçimlendiricileri ekleme görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-143">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="8f4ed-144">Denetleyici eylemleri döndürmeyebilir POCOs (düz eski CLR nesneler), ASP.NET MVC; bu durumda otomatik olarak oluşturacak bir `ObjectResult` , nesne sarmalar.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-144">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET MVC will automatically create an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="8f4ed-145">İstemci biçimlendirilmiş serileştirilmiş nesne alır (JSON biçiminde varsayılan değerdir; XML veya diğer biçimlere yapılandırabilirsiniz).</span><span class="sxs-lookup"><span data-stu-id="8f4ed-145">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="8f4ed-146">Nesne olan döndürülür, `null`, framework döndürecektir bir `204 No Content` yanıt.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-146">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="8f4ed-147">Bir nesne türü döndüren:</span><span class="sxs-lookup"><span data-stu-id="8f4ed-147">Returning an object type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="8f4ed-148">Örnekte, geçerli Yazar diğer adı için bir istek yazarın verilerle 200 Tamam bir yanıt alır.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-148">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="8f4ed-149">Geçersiz bir diğer ad için bir istek 204 Hayır içerik yanıtı alır.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-149">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="8f4ed-150">XML ve JSON biçimlerde yanıtı gösteren ekran görüntüsü aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-150">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="8f4ed-151">İçerik anlaşma işlemi</span><span class="sxs-lookup"><span data-stu-id="8f4ed-151">Content Negotiation Process</span></span>

<span data-ttu-id="8f4ed-152">İçerik *anlaşma* yalnızca gerçekleşir, bir `Accept` üstbilgisi istekte görünür.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-152">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="8f4ed-153">Bir accept üstbilgisi bir istek içerdiğinde, framework tercih sırasına göre accept üstbilgisindeki medya türlerini numaralandırır ve kabul etme üstbilgisi tarafından belirtilen biçimlerden birinde bir yanıt oluşturmak üzere bir biçimlendirici bulmayı dener.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-153">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="8f4ed-154">İstemcinin isteği karşılamak biçimlendirici bulunamazsa durumda framework yanıt oluşturmak üzere ilk biçimlendirici bulmayı dener (Geliştirici üzerinde seçeneği yapılandırmamışsa `MvcOptions` 406 döndürmek için kabul edilebilir değil yerine).</span><span class="sxs-lookup"><span data-stu-id="8f4ed-154">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="8f4ed-155">XML isteği belirtir, ancak XML biçimlendiricisi yapılandırılmazsa, JSON biçimlendirici kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-155">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="8f4ed-156">Daha genel olarak, bir biçimlendirici yoksa yapılandırılmışsa, istenen biçim sağlayabilir ve ardından nesne biçimlendirebilirsiniz ilk biçimlendirici kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-156">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="8f4ed-157">Üst bilgi verilirse, döndürülecek nesnesi işleyebilen ilk biçimlendirici yanıtı serileştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-157">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="8f4ed-158">Bu durumda, herhangi bir anlaşma alma yeri hiç - sunucu kullanacağı biçimi belirliyor.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-158">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="8f4ed-159">Accept üst bilgisi içeriyorsa `*/*`, üstbilgi sürece göz ardı edilir `RespectBrowserAcceptHeader` ayarlandığında true `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-159">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="8f4ed-160">Tarayıcılar ve içerik anlaşması</span><span class="sxs-lookup"><span data-stu-id="8f4ed-160">Browsers and Content Negotiation</span></span>

<span data-ttu-id="8f4ed-161">Tipik API istemcilerden farklı olarak, web tarayıcıları tedarik eğilimindedir `Accept` biçimlerinin joker karakterleri dahil olmak üzere çok çeşitli dahil üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-161">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="8f4ed-162">Varsayılan olarak, framework isteğinin tarayıcıdan geldiğini algıladığında, göz ardı eder `Accept` üstbilgi ve bunun yerine dönüş uygulamadaki içerik varsayılan biçimi yapılandırılmış (JSON aksi belirtilmedikçe).</span><span class="sxs-lookup"><span data-stu-id="8f4ed-162">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="8f4ed-163">Bu API'ları kullanmak için farklı tarayıcılar kullanırken, daha tutarlı bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-163">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="8f4ed-164">Uygulama Uy tarayıcınızın kabul üstbilgileri tercih ederseniz, bu MVC'ın yapılandırmasının bir parçası ayarlayarak yapılandırabileceğiniz `RespectBrowserAcceptHeader` için `true` içinde `ConfigureServices` yönteminde *haline*.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-164">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="8f4ed-165">Biçimlendiricileri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8f4ed-165">Configuring Formatters</span></span>

<span data-ttu-id="8f4ed-166">Uygulamanızın JSON varsayılan ötesinde ek biçimleri desteklemesi gerekiyorsa, NuGet paketleri ekleyebilir ve onları desteklemek için MVC yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-166">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="8f4ed-167">Giriş ve çıkış için ayrı biçimlendiricileri vardır.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-167">There are separate formatters for input and output.</span></span> <span data-ttu-id="8f4ed-168">Tarafından kullanılan giriş biçimlendiricileri [Model bağlama](model-binding.md); çıktı biçimlendiricileri yanıtları biçimlendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-168">Input formatters are used by [Model Binding](model-binding.md); output formatters are used to format responses.</span></span> <span data-ttu-id="8f4ed-169">Ayrıca yapılandırabilirsiniz [özel Biçimlendiricileri](../advanced/custom-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="8f4ed-169">You can also configure [Custom Formatters](../advanced/custom-formatters.md).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="8f4ed-170">XML biçim desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="8f4ed-170">Adding XML Format Support</span></span>

<span data-ttu-id="8f4ed-171">XML biçimlendirme için destek eklemek için yükleme `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-171">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="8f4ed-172">MVC'ın yapılandırmasında XmlSerializerFormatters eklemek *haline*:</span><span class="sxs-lookup"><span data-stu-id="8f4ed-172">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="8f4ed-173">Alternatif olarak, yalnızca çıktı biçimlendirici ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="8f4ed-173">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="8f4ed-174">Bu iki yaklaşım kullanarak sonuçları seri hale `System.Xml.Serialization.XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-174">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="8f4ed-175">İsterseniz, kullanabileceğiniz `System.Runtime.Serialization.DataContractSerializer` ilişkili biçimlendirici ekleyerek:</span><span class="sxs-lookup"><span data-stu-id="8f4ed-175">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="8f4ed-176">XML biçimlendirme desteği ekledikten sonra denetleyici yöntemlerine isteğin üzerinde göre uygun biçimde döndürmelidir `Accept` üstbilgi örnek gösterir bu Fiddler olarak:</span><span class="sxs-lookup"><span data-stu-id="8f4ed-176">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Fiddler konsol: istek için ham sekmesi gösterir Accept üstbilgi değeri uygulama/xml.](formatting/_static/xml-response.png)

<span data-ttu-id="8f4ed-179">Ham GET isteği ile yapıldığı denetçiler sekmesinde görebilirsiniz bir `Accept: application/xml` üstbilgi kümesi.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-179">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="8f4ed-180">Yanıt bölmesi gösterir `Content-Type: application/xml` başlık ve `Author` nesnesi, XML serileştirilmiş.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-180">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="8f4ed-181">Belirtmek için istek değiştirmek için oluşturucu sekmesini kullanın `application/json` içinde `Accept` üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-181">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="8f4ed-182">İsteğin yürütülmesi ve yanıt JSON biçiminde olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="8f4ed-182">Execute the request, and the response will be formatted as JSON:</span></span>

![Fiddler konsol: istek için ham sekmesi gösterir Accept üstbilgi değeri uygulama/json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="8f4ed-185">Bu ekran görüntüsünde, istek bir üstbilgisinin ayarlar görebilirsiniz `Accept: application/json` ve yanıt aynı belirtir, `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-185">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="8f4ed-186">`Author` Nesnesi, JSON biçiminde yanıt gövdesine gösterilir.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-186">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="8f4ed-187">Belirli bir biçimde zorlama</span><span class="sxs-lookup"><span data-stu-id="8f4ed-187">Forcing a Particular Format</span></span>

<span data-ttu-id="8f4ed-188">Belirli bir eylem için yanıt formatları kısıtlamak istiyorsanız şunları yapabilirsiniz, uygulayabileceğiniz `[Produces]` Filtresi.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-188">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="8f4ed-189">`[Produces]` Filtresi özel bir eylem (veya denetleyicisi) yanıt biçimi belirtir.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-189">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="8f4ed-190">Çoğu gibi [filtreleri](../controllers/filters.md), bu eylem, denetleyici veya genel kapsam uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-190">Like most [Filters](../controllers/filters.md), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="8f4ed-191">`[Produces]` Filtre içinde tüm eylemler zorlayacak `AuthorsController` diğer biçimlendiricileri uygulama ve sağlanan istemci için yapılandırılmış olsa bile yanıtları, JSON biçimli döndürmek için bir `Accept` farklı, isteyen üstbilgi kullanılabilir biçimi.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-191">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="8f4ed-192">Bkz: [filtreleri](../controllers/filters.md) genel filtre uygulamak da dahil olmak üzere daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-192">See [Filters](../controllers/filters.md) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="8f4ed-193">Özel büyük/küçük harf Biçimlendiricileri</span><span class="sxs-lookup"><span data-stu-id="8f4ed-193">Special Case Formatters</span></span>

<span data-ttu-id="8f4ed-194">Bazı özel durumlar, yerleşik biçimlendiricileri kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-194">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="8f4ed-195">Varsayılan olarak, `string` dönüş türleri olarak biçimlendirilir *metin/düz* (*metin/html* aracılığıyla istediyseniz `Accept` üstbilgi).</span><span class="sxs-lookup"><span data-stu-id="8f4ed-195">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="8f4ed-196">Bu davranış kaldırarak kaldırılabilir `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-196">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="8f4ed-197">İçinde biçimlendiricileri kaldırmak `Configure` yönteminde *haline* (aşağıda gösterilen).</span><span class="sxs-lookup"><span data-stu-id="8f4ed-197">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="8f4ed-198">Model nesnesi olan eylemler dönüş türü döndürecektir bir 204 İçerik yanıt döndürülürken `null`.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-198">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="8f4ed-199">Bu davranış kaldırarak kaldırılabilir `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-199">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="8f4ed-200">Aşağıdaki kod kaldırır `TextOutputFormatter` ve `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-200">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="8f4ed-201">Olmadan `TextOutputFormatter`, `string` türleri 406 iade edilemez, örneğin.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-201">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="8f4ed-202">Bir XML biçimlendiricisi varsa, onu biçimlendirecek Not `string` , dönüş türü `TextOutputFormatter` kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-202">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="8f4ed-203">Olmadan `HttpNoContentOutputFormatter`, null nesneleri yapılandırılmış biçimlendirici kullanılarak biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-203">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="8f4ed-204">Örneğin, JSON biçimlendirici gövdesine sahip bir yanıtta yalnızca döndürülecek `null`XML biçimlendiricisi özniteliğiyle boş bir XML öğesi döndürülecek yaparken `xsi:nil="true"` ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-204">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="8f4ed-205">Yanıt biçimi URL eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="8f4ed-205">Response Format URL Mappings</span></span>

<span data-ttu-id="8f4ed-206">İstemcileri belirli bir biçimde URL'SİNİN bir parçası gibi sorgu dizesi veya bölümü yolunun veya .xml veya .json gibi bir biçim özel dosya uzantısını kullanarak isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-206">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="8f4ed-207">İstek yolu eşlemesinden API kullanarak rotada belirtilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-207">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="8f4ed-208">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8f4ed-208">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="8f4ed-209">Bu yol, bir isteğe bağlı bir dosya uzantısı olarak belirtilmesi için istenen biçimde olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-209">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="8f4ed-210">`[FormatFilter]` Özniteliği biçimi değeri varlığını denetler `RouteData` ve yanıt oluşturulduğunda, yanıt biçimi için uygun bir biçimlendirici eşler.</span><span class="sxs-lookup"><span data-stu-id="8f4ed-210">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

| <span data-ttu-id="8f4ed-211">Rota</span><span class="sxs-lookup"><span data-stu-id="8f4ed-211">Route</span></span>                      | <span data-ttu-id="8f4ed-212">Biçimlendirici</span><span class="sxs-lookup"><span data-stu-id="8f4ed-212">Formatter</span></span>                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | <span data-ttu-id="8f4ed-213">Varsayılan çıkış biçimlendirici</span><span class="sxs-lookup"><span data-stu-id="8f4ed-213">The default output formatter</span></span>       |
| `/products/GetById/5.json` | <span data-ttu-id="8f4ed-214">JSON biçimlendirici (yapılandırıldıysa)</span><span class="sxs-lookup"><span data-stu-id="8f4ed-214">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="8f4ed-215">XML biçimlendirici (yapılandırıldıysa)</span><span class="sxs-lookup"><span data-stu-id="8f4ed-215">The XML formatter (if configured)</span></span>  |
