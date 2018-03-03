---
title: "ASP.NET Core MVC yanıt verileri biçimlendirme"
author: ardalis
description: "ASP.NET Core mvc'de yanıt verinin nasıl biçimlendirileceğini öğrenin."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/formatting
ms.openlocfilehash: 704ca4f1ea6e0acd14dfa4175b61d8e2acf8f3c7
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a><span data-ttu-id="34614-103">ASP.NET Core MVC biçimlendirme yanıt verileri giriş</span><span class="sxs-lookup"><span data-stu-id="34614-103">Introduction to formatting response data in ASP.NET Core MVC</span></span>

<span data-ttu-id="34614-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="34614-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="34614-105">ASP.NET Core MVC yanıt istemci belirtimlerine veya sabit biçimlerini kullanarak yanıt verileri biçimlendirme için yerleşik desteğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="34614-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="34614-106">[Görüntülemek veya örnek Github'dan indirdiğinizde](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span><span class="sxs-lookup"><span data-stu-id="34614-106">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="34614-107">Biçim özel eylem sonuçlarını</span><span class="sxs-lookup"><span data-stu-id="34614-107">Format-Specific Action Results</span></span>

<span data-ttu-id="34614-108">Bazı eylem sonucu türleri gibi belirli bir biçime belirli `JsonResult` ve `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="34614-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="34614-109">Eylemler her zaman belirli bir şekilde biçimlendirilmiş belirli sonuçlar döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="34614-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="34614-110">Örneğin, döndüren bir `JsonResult` istemci Tercihler bakılmaksızın JSON biçimli verileri döndürür.</span><span class="sxs-lookup"><span data-stu-id="34614-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="34614-111">Benzer şekilde, döndüren bir `ContentResult` düz metin biçimli dize verilerini (yalnızca döndüren bir dize olarak) döndürür.</span><span class="sxs-lookup"><span data-stu-id="34614-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="34614-112">Belirli bir türü döndürmek için bir eylem gerekli değildir; MVC hiçbir nesne dönüş değeri destekler.</span><span class="sxs-lookup"><span data-stu-id="34614-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="34614-113">Bir eylem döndürürse bir `IActionResult` uygulama ve denetleyici devraldığı `Controller`, geliştiricilerin birçok seçenek karşılık gelen çok sayıda yardımcı yöntemler vardır.</span><span class="sxs-lookup"><span data-stu-id="34614-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="34614-114">Nesneler döndürmeye Eylemler sonuçlarından olmayan `IActionResult` türleri serileştirilmiş uygun kullanarak `IOutputFormatter` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="34614-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="34614-115">Belirli bir biçimde veri öğesinden devralınan bir denetleyici döndürmek için `Controller` temel sınıfı, yerleşik yardımcı yöntemi `Json` JSON dönün ve `Content` düz metin için.</span><span class="sxs-lookup"><span data-stu-id="34614-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="34614-116">Eylem yönteminin belirli sonuç türü döndürmelidir (örneğin, `JsonResult`) veya `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="34614-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="34614-117">JSON biçimli veriyor:</span><span class="sxs-lookup"><span data-stu-id="34614-117">Returning JSON-formatted data:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="34614-118">Bu eylem yanıttan örnek:</span><span class="sxs-lookup"><span data-stu-id="34614-118">Sample response from this action:</span></span>

![Yanıtın içerik türü gösteren Microsoft edge'de Geliştirici Araçları'nın Ağ sekmesi application/json şeklindedir](formatting/_static/json-response.png)

<span data-ttu-id="34614-120">Yanıtın içerik türü olduğuna dikkat edin `application/json`hem ağ isteklerin listesini ve yanıt üstbilgileri bölümünde gösterilen.</span><span class="sxs-lookup"><span data-stu-id="34614-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="34614-121">Ayrıca, tarayıcıda (Bu durumda, Microsoft Edge) istek üstbilgileri bölümünde Accept üstbilgisindeki tarafından sunulan seçeneklerinin listesini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="34614-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="34614-122">Geçerli teknik bu başlığı sayıyor; Bu obeying aşağıda ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="34614-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="34614-123">Düz metin olarak biçimlendirilmiş verileri döndürmek için kullanmak `ContentResult` ve `Content` Yardımcısı:</span><span class="sxs-lookup"><span data-stu-id="34614-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="34614-124">Bu eylem yanıttan:</span><span class="sxs-lookup"><span data-stu-id="34614-124">A response from this action:</span></span>

![Yanıtın içerik türü gösteren Microsoft edge'de Geliştirici Araçları'nın Ağ sekmesi metin/düz değil](formatting/_static/text-response.png)

<span data-ttu-id="34614-126">Bu durumda Not `Content-Type` döndürülen olduğu `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="34614-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="34614-127">Ayrıca, yalnızca bir dize yanıt türünü kullanarak bu aynı davranışı elde edebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="34614-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="34614-128">Birden çok önemsiz olmayan eylemler için dönüş türleri veya seçenekler (örneğin, farklı HTTP durum kodları sonucuna göre gerçekleştirilen işlemler), tercih ettiğiniz `IActionResult` dönüş türü.</span><span class="sxs-lookup"><span data-stu-id="34614-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="34614-129">İçerik anlaşması</span><span class="sxs-lookup"><span data-stu-id="34614-129">Content Negotiation</span></span>

<span data-ttu-id="34614-130">İçerik anlaşması (*conneg* kısaca) oluşur. istemcinin belirttiği bir [Accept üstbilgisi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="34614-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="34614-131">ASP.NET Core MVC tarafından kullanılan varsayılan JSON biçimidir.</span><span class="sxs-lookup"><span data-stu-id="34614-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="34614-132">İçerik anlaşması tarafından gerçekleştirilir `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="34614-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="34614-133">Ayrıca belirli eylem sonuçlarını Yardımcısı yöntemleri döndürülen durum kodu içinde yerleşik (hangi tüm temel `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="34614-133">It's also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="34614-134">Model türü (veri aktarımı türü olarak tanımlanmış bir sınıf) geri dönebilirsiniz ve framework içinde otomatik olarak kaydırılır bir `ObjectResult` sizin için.</span><span class="sxs-lookup"><span data-stu-id="34614-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="34614-135">Aşağıdaki bir eylem yöntem `Ok` ve `NotFound` yardımcı yöntemler:</span><span class="sxs-lookup"><span data-stu-id="34614-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="34614-136">JSON biçimli bir yanıt, başka bir biçime istendi ve sunucu istenen biçim döndürebilirsiniz sürece döndürülür.</span><span class="sxs-lookup"><span data-stu-id="34614-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="34614-137">Gibi bir araç kullanabilirsiniz [Fiddler](http://www.telerik.com/fiddler) bir Accept üstbilgisi içeren bir isteği oluşturun ve başka bir biçim belirtin.</span><span class="sxs-lookup"><span data-stu-id="34614-137">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="34614-138">Sunucusu varsa, bu durumda, bir *biçimlendirici* , istenen biçiminde bir yanıt oluşturabilirsiniz, sonuç istemci tercih edilen biçimde döndürülür.</span><span class="sxs-lookup"><span data-stu-id="34614-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![El ile oluşturulan bir gösteren fiddler konsol uygulama/xml bir Accept üstbilgi değeri ile isteği alma](formatting/_static/fiddler-composer.png)

<span data-ttu-id="34614-140">Yukarıdaki ekran görüntüsünde, Fiddler Oluşturucusu bir isteği oluşturmak için kullanılmış belirtme `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="34614-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="34614-141">Varsayılan olarak, ASP.NET Core MVC yalnızca JSON, bu nedenle bile destekleyen başka bir biçime belirtildiğinde, hala JSON biçimli döndürülen sonuç.</span><span class="sxs-lookup"><span data-stu-id="34614-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="34614-142">Sonraki bölümde ek biçimlendiricileri ekleme görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="34614-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="34614-143">Denetleyici eylemleri döndürmeyebilir POCOs (düz eski CLR nesneler), ASP.NET MVC; bu durumda otomatik olarak oluşturacak bir `ObjectResult` , nesne sarmalar.</span><span class="sxs-lookup"><span data-stu-id="34614-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET MVC will automatically create an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="34614-144">İstemci biçimlendirilmiş serileştirilmiş nesne alır (JSON biçiminde varsayılan değerdir; XML veya diğer biçimlere yapılandırabilirsiniz).</span><span class="sxs-lookup"><span data-stu-id="34614-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="34614-145">Nesne olan döndürülür, `null`, framework döndürecektir bir `204 No Content` yanıt.</span><span class="sxs-lookup"><span data-stu-id="34614-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="34614-146">Bir nesne türü döndüren:</span><span class="sxs-lookup"><span data-stu-id="34614-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="34614-147">Örnekte, geçerli Yazar diğer adı için bir istek yazarın verilerle 200 Tamam bir yanıt alır.</span><span class="sxs-lookup"><span data-stu-id="34614-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="34614-148">Geçersiz bir diğer ad için bir istek 204 Hayır içerik yanıtı alır.</span><span class="sxs-lookup"><span data-stu-id="34614-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="34614-149">XML ve JSON biçimlerde yanıtı gösteren ekran görüntüsü aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="34614-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="34614-150">İçerik anlaşma işlemi</span><span class="sxs-lookup"><span data-stu-id="34614-150">Content Negotiation Process</span></span>

<span data-ttu-id="34614-151">İçerik *anlaşma* yalnızca gerçekleşir, bir `Accept` üstbilgisi istekte görünür.</span><span class="sxs-lookup"><span data-stu-id="34614-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="34614-152">Bir accept üstbilgisi bir istek içerdiğinde, framework tercih sırasına göre accept üstbilgisindeki medya türlerini numaralandırır ve kabul etme üstbilgisi tarafından belirtilen biçimlerden birinde bir yanıt oluşturmak üzere bir biçimlendirici bulmayı dener.</span><span class="sxs-lookup"><span data-stu-id="34614-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="34614-153">İstemcinin isteği karşılamak biçimlendirici bulunamazsa durumda framework yanıt oluşturmak üzere ilk biçimlendirici bulmayı dener (Geliştirici üzerinde seçeneği yapılandırmamışsa `MvcOptions` 406 döndürmek için kabul edilebilir değil yerine).</span><span class="sxs-lookup"><span data-stu-id="34614-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="34614-154">XML isteği belirtir, ancak XML biçimlendiricisi yapılandırılmazsa, JSON biçimlendirici kullanılır.</span><span class="sxs-lookup"><span data-stu-id="34614-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="34614-155">Daha genel olarak, bir biçimlendirici yoksa yapılandırılmışsa, istenen biçim sağlayabilir ve ardından nesne biçimlendirebilirsiniz ilk biçimlendirici kullanılır.</span><span class="sxs-lookup"><span data-stu-id="34614-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="34614-156">Üst bilgi verilirse, döndürülecek nesnesi işleyebilen ilk biçimlendirici yanıtı serileştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="34614-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="34614-157">Bu durumda, herhangi bir anlaşma alma yeri hiç - sunucu kullanacağı biçimi belirliyor.</span><span class="sxs-lookup"><span data-stu-id="34614-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="34614-158">Accept üst bilgisi içeriyorsa `*/*`, üstbilgi sürece göz ardı edilir `RespectBrowserAcceptHeader` ayarlandığında true `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="34614-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="34614-159">Tarayıcılar ve içerik anlaşması</span><span class="sxs-lookup"><span data-stu-id="34614-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="34614-160">Tipik API istemcilerden farklı olarak, web tarayıcıları tedarik eğilimindedir `Accept` biçimlerinin joker karakterleri dahil olmak üzere çok çeşitli dahil üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="34614-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="34614-161">Varsayılan olarak, framework isteğinin tarayıcıdan geldiğini algıladığında, göz ardı eder `Accept` üstbilgi ve bunun yerine dönüş uygulamadaki içerik varsayılan biçimi yapılandırılmış (JSON aksi belirtilmedikçe).</span><span class="sxs-lookup"><span data-stu-id="34614-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="34614-162">Bu API'ları kullanmak için farklı tarayıcılar kullanırken, daha tutarlı bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="34614-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="34614-163">Uygulama Uy tarayıcınızın kabul üstbilgileri tercih ederseniz, bu MVC'ın yapılandırmasının bir parçası ayarlayarak yapılandırabileceğiniz `RespectBrowserAcceptHeader` için `true` içinde `ConfigureServices` yönteminde *haline*.</span><span class="sxs-lookup"><span data-stu-id="34614-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="34614-164">Biçimlendiricileri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="34614-164">Configuring Formatters</span></span>

<span data-ttu-id="34614-165">Uygulamanızın JSON varsayılan ötesinde ek biçimleri desteklemesi gerekiyorsa, NuGet paketleri ekleyebilir ve onları desteklemek için MVC yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="34614-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="34614-166">Giriş ve çıkış için ayrı biçimlendiricileri vardır.</span><span class="sxs-lookup"><span data-stu-id="34614-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="34614-167">Tarafından kullanılan giriş biçimlendiricileri [Model bağlama](model-binding.md); çıktı biçimlendiricileri yanıtları biçimlendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="34614-167">Input formatters are used by [Model Binding](model-binding.md); output formatters are used to format responses.</span></span> <span data-ttu-id="34614-168">Ayrıca yapılandırabilirsiniz [özel Biçimlendiricileri](../advanced/custom-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="34614-168">You can also configure [Custom Formatters](../advanced/custom-formatters.md).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="34614-169">XML biçim desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="34614-169">Adding XML Format Support</span></span>

<span data-ttu-id="34614-170">XML biçimlendirme için destek eklemek için yükleme `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="34614-170">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="34614-171">MVC'ın yapılandırmasında XmlSerializerFormatters eklemek *haline*:</span><span class="sxs-lookup"><span data-stu-id="34614-171">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="34614-172">Alternatif olarak, yalnızca çıktı biçimlendirici ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="34614-172">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="34614-173">Bu iki yaklaşım kullanarak sonuçları seri hale `System.Xml.Serialization.XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="34614-173">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="34614-174">İsterseniz, kullanabileceğiniz `System.Runtime.Serialization.DataContractSerializer` ilişkili biçimlendirici ekleyerek:</span><span class="sxs-lookup"><span data-stu-id="34614-174">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="34614-175">XML biçimlendirme desteği ekledikten sonra denetleyici yöntemlerine isteğin üzerinde göre uygun biçimde döndürmelidir `Accept` üstbilgi örnek gösterir bu Fiddler olarak:</span><span class="sxs-lookup"><span data-stu-id="34614-175">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Fiddler konsol: istek için ham sekmesi gösterir Accept üstbilgi değeri uygulama/xml.](formatting/_static/xml-response.png)

<span data-ttu-id="34614-178">Ham GET isteği ile yapıldığı denetçiler sekmesinde görebilirsiniz bir `Accept: application/xml` üstbilgi kümesi.</span><span class="sxs-lookup"><span data-stu-id="34614-178">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="34614-179">Yanıt bölmesi gösterir `Content-Type: application/xml` başlık ve `Author` nesnesi, XML serileştirilmiş.</span><span class="sxs-lookup"><span data-stu-id="34614-179">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="34614-180">Belirtmek için istek değiştirmek için oluşturucu sekmesini kullanın `application/json` içinde `Accept` üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="34614-180">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="34614-181">İsteğin yürütülmesi ve yanıt JSON biçiminde olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="34614-181">Execute the request, and the response will be formatted as JSON:</span></span>

![Fiddler konsol: istek için ham sekmesi gösterir Accept üstbilgi değeri uygulama/json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="34614-184">Bu ekran görüntüsünde, istek bir üstbilgisinin ayarlar görebilirsiniz `Accept: application/json` ve yanıt aynı belirtir, `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="34614-184">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="34614-185">`Author` Nesnesi, JSON biçiminde yanıt gövdesine gösterilir.</span><span class="sxs-lookup"><span data-stu-id="34614-185">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="34614-186">Belirli bir biçimde zorlama</span><span class="sxs-lookup"><span data-stu-id="34614-186">Forcing a Particular Format</span></span>

<span data-ttu-id="34614-187">Belirli bir eylem için yanıt formatları kısıtlamak istiyorsanız şunları yapabilirsiniz, uygulayabileceğiniz `[Produces]` Filtresi.</span><span class="sxs-lookup"><span data-stu-id="34614-187">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="34614-188">`[Produces]` Filtresi özel bir eylem (veya denetleyicisi) yanıt biçimi belirtir.</span><span class="sxs-lookup"><span data-stu-id="34614-188">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="34614-189">Çoğu gibi [filtreleri](../controllers/filters.md), bu eylem, denetleyici veya genel kapsam uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="34614-189">Like most [Filters](../controllers/filters.md), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="34614-190">`[Produces]` Filtre içinde tüm eylemler zorlayacak `AuthorsController` diğer biçimlendiricileri uygulama ve sağlanan istemci için yapılandırılmış olsa bile yanıtları, JSON biçimli döndürmek için bir `Accept` farklı, isteyen üstbilgi kullanılabilir biçimi.</span><span class="sxs-lookup"><span data-stu-id="34614-190">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="34614-191">Bkz: [filtreleri](../controllers/filters.md) genel filtre uygulamak da dahil olmak üzere daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="34614-191">See [Filters](../controllers/filters.md) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="34614-192">Özel büyük/küçük harf Biçimlendiricileri</span><span class="sxs-lookup"><span data-stu-id="34614-192">Special Case Formatters</span></span>

<span data-ttu-id="34614-193">Bazı özel durumlar, yerleşik biçimlendiricileri kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="34614-193">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="34614-194">Varsayılan olarak, `string` dönüş türleri olarak biçimlendirilir *metin/düz* (*metin/html* aracılığıyla istediyseniz `Accept` üstbilgi).</span><span class="sxs-lookup"><span data-stu-id="34614-194">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="34614-195">Bu davranış kaldırarak kaldırılabilir `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="34614-195">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="34614-196">İçinde biçimlendiricileri kaldırmak `Configure` yönteminde *haline* (aşağıda gösterilen).</span><span class="sxs-lookup"><span data-stu-id="34614-196">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="34614-197">Model nesnesi olan eylemler dönüş türü döndürecektir bir 204 İçerik yanıt döndürülürken `null`.</span><span class="sxs-lookup"><span data-stu-id="34614-197">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="34614-198">Bu davranış kaldırarak kaldırılabilir `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="34614-198">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="34614-199">Aşağıdaki kod kaldırır `TextOutputFormatter` ve `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="34614-199">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="34614-200">Olmadan `TextOutputFormatter`, `string` türleri 406 iade edilemez, örneğin.</span><span class="sxs-lookup"><span data-stu-id="34614-200">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="34614-201">Bir XML biçimlendiricisi varsa, onu biçimlendirecek Not `string` , dönüş türü `TextOutputFormatter` kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="34614-201">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="34614-202">Olmadan `HttpNoContentOutputFormatter`, null nesneleri yapılandırılmış biçimlendirici kullanılarak biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="34614-202">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="34614-203">Örneğin, JSON biçimlendirici gövdesine sahip bir yanıtta yalnızca döndürülecek `null`XML biçimlendiricisi özniteliğiyle boş bir XML öğesi döndürülecek yaparken `xsi:nil="true"` ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="34614-203">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="34614-204">Yanıt biçimi URL eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="34614-204">Response Format URL Mappings</span></span>

<span data-ttu-id="34614-205">İstemcileri belirli bir biçimde URL'SİNİN bir parçası gibi sorgu dizesi veya bölümü yolunun veya .xml veya .json gibi bir biçim özel dosya uzantısını kullanarak isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="34614-205">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="34614-206">İstek yolu eşlemesinden API kullanarak rotada belirtilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="34614-206">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="34614-207">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="34614-207">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="34614-208">Bu yol, bir isteğe bağlı bir dosya uzantısı olarak belirtilmesi için istenen biçimde olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="34614-208">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="34614-209">`[FormatFilter]` Özniteliği biçimi değeri varlığını denetler `RouteData` ve yanıt oluşturulduğunda, yanıt biçimi için uygun bir biçimlendirici eşler.</span><span class="sxs-lookup"><span data-stu-id="34614-209">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

| <span data-ttu-id="34614-210">Rota</span><span class="sxs-lookup"><span data-stu-id="34614-210">Route</span></span>                      | <span data-ttu-id="34614-211">Biçimlendirici</span><span class="sxs-lookup"><span data-stu-id="34614-211">Formatter</span></span>                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | <span data-ttu-id="34614-212">Varsayılan çıkış biçimlendirici</span><span class="sxs-lookup"><span data-stu-id="34614-212">The default output formatter</span></span>       |
| `/products/GetById/5.json` | <span data-ttu-id="34614-213">JSON biçimlendirici (yapılandırıldıysa)</span><span class="sxs-lookup"><span data-stu-id="34614-213">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="34614-214">XML biçimlendirici (yapılandırıldıysa)</span><span class="sxs-lookup"><span data-stu-id="34614-214">The XML formatter (if configured)</span></span>  |
