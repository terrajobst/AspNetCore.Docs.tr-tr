---
title: ASP.NET Core Web API'si yanıtı verileri biçimlendirme
author: ardalis
description: ASP.NET Core Web API'si yanıtı verilerinde biçimlendirmeyi öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 05/29/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: 7628565d8646c0a057e28aa54dc9ce9198750c15
ms.sourcegitcommit: 9ae1fd11f39b0a72b2ae42f0b450345e6e306bc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66415684"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="86a4c-103">ASP.NET Core Web API'si yanıtı verileri biçimlendirme</span><span class="sxs-lookup"><span data-stu-id="86a4c-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="86a4c-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="86a4c-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="86a4c-105">ASP.NET Core MVC, sabit biçimlerini kullanarak, yanıt verilerini biçimlendirme veya yanıt istemci belirtimleri için yerleşik destek sunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="86a4c-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="86a4c-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="86a4c-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="86a4c-107">Biçim özel eylem sonuçları</span><span class="sxs-lookup"><span data-stu-id="86a4c-107">Format-Specific Action Results</span></span>

<span data-ttu-id="86a4c-108">Bazı eylem sonucu türleri gibi belirli bir biçimde belirli `JsonResult` ve `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="86a4c-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="86a4c-109">Eylemler, her zaman belirli bir şekilde biçimlendirilmiş belirli sonuçlar döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="86a4c-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="86a4c-110">Örneğin, döndüren bir `JsonResult` istemci tercihleri bağımsız olarak, JSON biçimli verileri döndürür.</span><span class="sxs-lookup"><span data-stu-id="86a4c-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="86a4c-111">Benzer şekilde, döndüren bir `ContentResult` (yalnızca bir dize döndüren gibi) düz metin biçimli dize verileri döndürür.</span><span class="sxs-lookup"><span data-stu-id="86a4c-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="86a4c-112">Herhangi bir türü için bir eylem gerekli değildir; MVC herhangi bir nesne dönüş değeri destekler.</span><span class="sxs-lookup"><span data-stu-id="86a4c-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="86a4c-113">Bir eylem döndürürse bir `IActionResult` uygulama ve denetleyici devraldığı `Controller`, geliştiricilere seçimleri çoğuna karşılık gelen çok sayıda yardımcı yöntemler vardır.</span><span class="sxs-lookup"><span data-stu-id="86a4c-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="86a4c-114">İlgili nesneler dönüş eylemleri sonuçlardan olmayan `IActionResult` türleri seri hale getirilemiyor uygun kullanarak `IOutputFormatter` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="86a4c-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="86a4c-115">Devralınan denetleyicisinden belirli bir biçimde veri döndürülecek `Controller` temel sınıfı, yerleşik yardımcı yöntemi `Json` JSON döndürülecek ve `Content` düz metin.</span><span class="sxs-lookup"><span data-stu-id="86a4c-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="86a4c-116">Eylem yöntemi, belirli bir sonuç türü döndürmesi gerekir (örneğin, `JsonResult`) veya `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="86a4c-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="86a4c-117">JSON biçimli veriler döndürme:</span><span class="sxs-lookup"><span data-stu-id="86a4c-117">Returning JSON-formatted data:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="86a4c-118">Bu eylem örnek yanıttan:</span><span class="sxs-lookup"><span data-stu-id="86a4c-118">Sample response from this action:</span></span>

![Yanıtın içerik türü gösteren Microsoft edge'de Geliştirici Araçları'nın Ağ sekmesi application/json şeklindedir.](formatting/_static/json-response.png)

<span data-ttu-id="86a4c-120">Yanıtın içerik türü olduğuna dikkat edin `application/json`hem de ağ isteklerinin listesini yanıt üst kısmında gösterilen.</span><span class="sxs-lookup"><span data-stu-id="86a4c-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="86a4c-121">Ayrıca istek üstbilgileri bölümünde Accept üst bilgisi tarayıcıda (Bu durumda, Microsoft Edge) tarafından sunulan seçeneklerin listesini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="86a4c-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="86a4c-122">Geçerli teknik bu üst bilgi yok sayıyor; Bunu obeying aşağıda ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="86a4c-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="86a4c-123">Düz metin olarak biçimlendirilmiş veri döndürmek için `ContentResult` ve `Content` yardımcı:</span><span class="sxs-lookup"><span data-stu-id="86a4c-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="86a4c-124">Bu eylem için bir yanıt:</span><span class="sxs-lookup"><span data-stu-id="86a4c-124">A response from this action:</span></span>

![Metin/düz içerik yanıtının türünü gösteren Microsoft edge'de Geliştirici Araçları'nın Ağ sekmesi olduğu](formatting/_static/text-response.png)

<span data-ttu-id="86a4c-126">Bu durumda Not `Content-Type` döndürülen olan `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="86a4c-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="86a4c-127">Ayrıca, yalnızca dize yanıt türünü kullanarak bu aynı davranışı elde edebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="86a4c-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="86a4c-128">Birden çok önemsiz olmayan eylemler için dönüş türü veya seçenekler (örneğin, gerçekleştirilen işlemleri sonucuna göre farklı HTTP durum kodları), tercih ettiğiniz `IActionResult` dönüş türü.</span><span class="sxs-lookup"><span data-stu-id="86a4c-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="86a4c-129">İçerik anlaşması</span><span class="sxs-lookup"><span data-stu-id="86a4c-129">Content Negotiation</span></span>

<span data-ttu-id="86a4c-130">İçerik anlaşması (*conneg* kısaca) istemci oluşur bir [Accept üst bilgisi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="86a4c-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="86a4c-131">ASP.NET Core MVC tarafından kullanılan varsayılan JSON biçimidir.</span><span class="sxs-lookup"><span data-stu-id="86a4c-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="86a4c-132">İçerik anlaşması tarafından gerçekleştirilen `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="86a4c-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="86a4c-133">Ayrıca belirli bir eylem sonuçlarını yardımcı yöntemlerinden döndürülen durum kodu ile oluşturulur (tüm alan üzerinde `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="86a4c-133">It's also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="86a4c-134">Ayrıca, bir model türü (veri aktarımı türünüz olarak tanımlanmış bir sınıf) döndürebilir ve framework içinde otomatik olarak kaydırılır bir `ObjectResult` sizin için.</span><span class="sxs-lookup"><span data-stu-id="86a4c-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="86a4c-135">Aşağıdaki eylem yöntemini kullanan `Ok` ve `NotFound` yardımcı yöntemler:</span><span class="sxs-lookup"><span data-stu-id="86a4c-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="86a4c-136">Başka bir biçime istendi ve sunucu istenen biçimi döndürebilir sürece JSON biçimli bir yanıt döndürülür.</span><span class="sxs-lookup"><span data-stu-id="86a4c-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="86a4c-137">Gibi bir araç kullanabilirsiniz [Fiddler](http://www.telerik.com/fiddler) bir Accept üst bilgisi içeren bir istek oluşturun ve başka bir biçim belirtin.</span><span class="sxs-lookup"><span data-stu-id="86a4c-137">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="86a4c-138">Sunucusu varsa, bu durumda, bir *biçimlendirici* , istenen biçimde yanıt üretebilir, sonuç istemci tercih edilmeyen biçiminde döndürülür.</span><span class="sxs-lookup"><span data-stu-id="86a4c-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![El ile oluşturulan bir gösteren fiddler konsol GET isteği bir Accept üstbilgi değeri uygulama/XML ile](formatting/_static/fiddler-composer.png)

<span data-ttu-id="86a4c-140">Bir isteği oluşturmak için yukarıdaki ekran görüntüsünde, Fiddler Oluşturucu kullanıldı belirtme `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="86a4c-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="86a4c-141">Varsayılan olarak, ASP.NET Core MVC yalnızca JSON, böylece destekleyen başka bir biçime belirtildiğinde, JSON biçimli hala döndürülen sonuç.</span><span class="sxs-lookup"><span data-stu-id="86a4c-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="86a4c-142">Sonraki bölümde ek biçimlendiricileri ekleme görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="86a4c-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="86a4c-143">Denetleyici eylemleri, bu durumda ASP.NET Core MVC otomatik olarak oluşturur POCOs (düz eski CLR nesneleri) döndürebilir bir `ObjectResult` nesneyi sarmalayan sizin için.</span><span class="sxs-lookup"><span data-stu-id="86a4c-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET Core MVC automatically creates an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="86a4c-144">İstemci biçimlendirilmiş serileştirilmiş nesne alırsınız (JSON biçiminde varsayılan değerdir; XML veya diğer biçimlere yapılandırabileceğiniz).</span><span class="sxs-lookup"><span data-stu-id="86a4c-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="86a4c-145">Döndürülen nesne olması durumunda `null`, framework döndürecektir bir `204 No Content` yanıt.</span><span class="sxs-lookup"><span data-stu-id="86a4c-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="86a4c-146">Bir nesne türü döndürüyor:</span><span class="sxs-lookup"><span data-stu-id="86a4c-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="86a4c-147">Aşağıdaki örnekte, geçerli Yazar diğer adı için bir istek yazarın verilerle 200 Tamam yanıtı alırsınız.</span><span class="sxs-lookup"><span data-stu-id="86a4c-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="86a4c-148">Geçersiz diğer ad isteği 204 İçerik yok yanıt alırsınız.</span><span class="sxs-lookup"><span data-stu-id="86a4c-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="86a4c-149">XML ve JSON biçimlerde yanıtı gösteren ekran görüntüsü aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="86a4c-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="86a4c-150">İçerik anlaşma işlemi</span><span class="sxs-lookup"><span data-stu-id="86a4c-150">Content Negotiation Process</span></span>

<span data-ttu-id="86a4c-151">İçerik *anlaşma* yalnızca gerçekleşir, bir `Accept` istekte üstbilgi görünür.</span><span class="sxs-lookup"><span data-stu-id="86a4c-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="86a4c-152">İstek bir accept üst bilgisi içeriyorsa, framework accept üst bilgisi tercih sırasına göre medya türlerini numaralandırır ve accept üst bilgisi tarafından belirtilen biçimlerden birinde bir yanıt oluşturan bir biçimlendirici bulmayı dener.</span><span class="sxs-lookup"><span data-stu-id="86a4c-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="86a4c-153">Biçimlendirici istemcinin isteği karşılamak bulunan durumda framework bir yanıt oluşturan ilk biçimlendiriciyi bulmayı dener (Geliştirici üzerinde seçeneği yapılandırmamışsa `MvcOptions` 406 döndürmek için kabul edilebilir değil yerine).</span><span class="sxs-lookup"><span data-stu-id="86a4c-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="86a4c-154">İstek XML belirtiyor, ancak XML biçimlendiricisi yapılandırılmamış, JSON biçimlendirici kullanılır.</span><span class="sxs-lookup"><span data-stu-id="86a4c-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="86a4c-155">Daha genel olarak, biçimlendirici yapılandırıldıysa, istenen biçimi sağlayın, sonra nesneyi biçimlendirebilirsiniz ilk biçimlendiriciyi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="86a4c-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="86a4c-156">Üst bilgi belirtilmezse, döndürülecek nesnesi işleyebilen ilk biçimlendiriciyi yanıtı serileştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="86a4c-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="86a4c-157">Bu durumda, herhangi bir anlaşma alma yerde yok - sunucu kullanacağı hangi biçimde belirliyor.</span><span class="sxs-lookup"><span data-stu-id="86a4c-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="86a4c-158">Accept üst bilgisi içeriyorsa `*/*`, üstbilgi sürece yok sayılacak `RespectBrowserAcceptHeader` ayarlandığında true `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="86a4c-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="86a4c-159">Tarayıcılar ve içerik anlaşması</span><span class="sxs-lookup"><span data-stu-id="86a4c-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="86a4c-160">Tipik API istemcilerden farklı olarak, web tarayıcıları tedarik eğilimindedir `Accept` biçimleri, joker karakterler dahil olmak üzere geniş bir yelpazede içeren üst bilgiler.</span><span class="sxs-lookup"><span data-stu-id="86a4c-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="86a4c-161">Varsayılan olarak, çerçeve isteğinin bir tarayıcıdan geldiğini algıladığında yok `Accept` üstbilgi ve bunun yerine dönüş uygulamadaki içerik varsayılan biçimi yapılandırılmış (JSON aksi şekilde yapılandırılmadıkça).</span><span class="sxs-lookup"><span data-stu-id="86a4c-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="86a4c-162">Bu, farklı tarayıcılar API'lerini tüketmesi kullanırken daha tutarlı bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="86a4c-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="86a4c-163">Uygulama Uy tarayıcınızın kabul üst bilgileri tercih ederseniz, bu MVC'nin yapılandırmasının bir parçası olarak ayarlayarak yapılandırabilirsiniz `RespectBrowserAcceptHeader` için `true` içinde `ConfigureServices` yönteminde *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="86a4c-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="86a4c-164">Biçimlendiricileri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="86a4c-164">Configuring Formatters</span></span>

<span data-ttu-id="86a4c-165">Uygulamanız varsayılan değer olan JSON ek biçimleri için destek gerekiyorsa, NuGet paketlerini ekleyip, bunları desteklemek için MVC yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="86a4c-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="86a4c-166">Girdi ve çıktı ayrı biçimlendiricileri vardır.</span><span class="sxs-lookup"><span data-stu-id="86a4c-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="86a4c-167">Tarafından kullanılan giriş biçimlendiricileri [Model bağlama](xref:mvc/models/model-binding); çıkış biçimlendiricileri yanıtları biçimlendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="86a4c-167">Input formatters are used by [Model Binding](xref:mvc/models/model-binding); output formatters are used to format responses.</span></span> <span data-ttu-id="86a4c-168">Ayrıca [özel Biçimlendiricileri](xref:web-api/advanced/custom-formatters).</span><span class="sxs-lookup"><span data-stu-id="86a4c-168">You can also configure [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="86a4c-169">Biçimlendiricileri System.Text.Json tabanlı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="86a4c-169">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="86a4c-170">Özellikleri `System.Text.Json`-tabanlı biçimlendiricileri kullanarak yapılandırılabilir `Microsoft.AspNetCore.Mvc.MvcOptions.SerializerOptions`.</span><span class="sxs-lookup"><span data-stu-id="86a4c-170">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcOptions.SerializerOptions`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.SerializerOptions.WriterSettings.Indented = true;
});
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="86a4c-171">Newtonsoft.Json tabanlı JSON biçimi desteği eklendi</span><span class="sxs-lookup"><span data-stu-id="86a4c-171">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="86a4c-172">ASP.NET Core 3.0 önce MVC kullanarak JSON biçimlendiricileri kullanılarak uygulanan varsayılan `Newtonsoft.Json` paket.</span><span class="sxs-lookup"><span data-stu-id="86a4c-172">Prior to ASP.NET Core 3.0, MVC defaulted to using JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="86a4c-173">ASP.NET Core 3.0 veya sonraki sürümlerde, varsayılan JSON biçimlendiricileri dayalı `System.Text.Json`.</span><span class="sxs-lookup"><span data-stu-id="86a4c-173">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="86a4c-174">Destek `Newtonsoft.Json`-tabanlı biçimlendiricileri ve özellikler kullanılabilir yükleyerek [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet paketi ve onu yapılandırma `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="86a4c-174">Support for `Newtonsoft.Json`-based formatters and features is available by installing the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddMvc()
    .AddNewtonsoftJson();
```

<span data-ttu-id="86a4c-175">İle bazı özellikler çalışmayabilir `System.Text.Json`-biçimlendiricileri temel alır ve bir başvuru gerektirir `Newtonsoft.Json`-tabanlı ASP.NET Core 3.0 sürümü için biçimlendiricileri.</span><span class="sxs-lookup"><span data-stu-id="86a4c-175">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters for the ASP.NET Core 3.0 release.</span></span> <span data-ttu-id="86a4c-176">Kullanmaya devam `Newtonsoft.Json`-biçimlendiricileri, temel ASP.NET Core 3.0 veya üzeri uygulama:</span><span class="sxs-lookup"><span data-stu-id="86a4c-176">Continue using the `Newtonsoft.Json`-based formatters if your ASP.NET Core 3.0 or later app:</span></span>

* <span data-ttu-id="86a4c-177">Kullanan `Newtonsoft.Json` öznitelikleri (örneğin, `[JsonProperty]` veya `[JsonIgnore]`), serileştirme ayarları özelleştirdikten veya kullanır üzerinde özelliklerinin `Newtonsoft.Json` sağlar.</span><span class="sxs-lookup"><span data-stu-id="86a4c-177">Uses `Newtonsoft.Json` attributes (for example, `[JsonProperty]` or `[JsonIgnore]`), customizes the serialization settings, or relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="86a4c-178">Yapılandırır `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span><span class="sxs-lookup"><span data-stu-id="86a4c-178">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="86a4c-179">ASP.NET Core 3.0 önce `JsonResult.SerializerSettings` kabul eden bir örneğini `JsonSerializerSettings` özgü olan `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="86a4c-179">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="86a4c-180">Oluşturur [Openapı](<xref:tutorials/web-api-help-pages-using-swagger>) belgeleri.</span><span class="sxs-lookup"><span data-stu-id="86a4c-180">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

::: moniker-end

### <a name="add-xml-format-support"></a><span data-ttu-id="86a4c-181">XML biçimi desteği eklendi</span><span class="sxs-lookup"><span data-stu-id="86a4c-181">Add XML format support</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="86a4c-182">XML biçimlendirme desteği ASP.NET Core 2.2 veya önceki eklemek için yükleme [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="86a4c-182">To add XML formatting support in ASP.NET Core 2.2 or earlier, install the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

::: moniker-end

<span data-ttu-id="86a4c-183">XML biçimlendiricileri kullanılarak uygulanan `System.Xml.Serialization.XmlSerializer` çağırarak yapılandırılabilir <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="86a4c-183">XML formatters implemented using `System.Xml.Serialization.XmlSerializer` can be configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="86a4c-184">Alternatif olarak, XML biçimlendiricileri kullanılarak uygulanan `System.Runtime.Serialization.DataContractSerializer` çağırarak yapılandırılabilir <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="86a4c-184">Alternatively, XML formatters implemented using `System.Runtime.Serialization.DataContractSerializer` can be configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddXmlDataContractSerializerFormatters();
```

<span data-ttu-id="86a4c-185">XML biçimlendirme desteği ekledikten sonra denetleyici yöntemlerinizi isteğin üzerinde göre uygun biçimde döndürmelidir `Accept` üst bilgisi olarak bu Fiddler örnek gösterir:</span><span class="sxs-lookup"><span data-stu-id="86a4c-185">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Fiddler konsolu: Ham sekmesini istek için Accept üstbilgi değeri uygulama/xml gösterilir.](formatting/_static/xml-response.png)

<span data-ttu-id="86a4c-188">Ham GET isteği ile yapılan denetçiler sekmesinde gördüğünüz bir `Accept: application/xml` başlık kümesi.</span><span class="sxs-lookup"><span data-stu-id="86a4c-188">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="86a4c-189">Yanıt bölmesi gösterir `Content-Type: application/xml` başlık ve `Author` nesne, XML'e seri.</span><span class="sxs-lookup"><span data-stu-id="86a4c-189">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="86a4c-190">Belirtmek için istek değiştirmek için oluşturucu sekmesini kullanın `application/json` içinde `Accept` başlığı.</span><span class="sxs-lookup"><span data-stu-id="86a4c-190">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="86a4c-191">İsteği yürütün ve yanıtı JSON olarak biçimlendirilir:</span><span class="sxs-lookup"><span data-stu-id="86a4c-191">Execute the request, and the response will be formatted as JSON:</span></span>

![Fiddler konsolu: İstek için ham sekmesi, uygulama/json Accept üstbilgi değeri gösterir.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="86a4c-194">Bu ekran görüntüsünde, istek üst bilgisine ayarlar görebilirsiniz `Accept: application/json` ve yanıt aynı belirtir, `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="86a4c-194">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="86a4c-195">`Author` Nesnesi, JSON biçiminde yanıt gövdesine gösterilir.</span><span class="sxs-lookup"><span data-stu-id="86a4c-195">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="86a4c-196">Belirli bir biçimde zorlama</span><span class="sxs-lookup"><span data-stu-id="86a4c-196">Forcing a Particular Format</span></span>

<span data-ttu-id="86a4c-197">Belirli bir eylemi yanıt biçimi kısıtlamak istiyorsanız, aşağıdakileri yapabilirsiniz, uygulayabileceğiniz `[Produces]` filtre.</span><span class="sxs-lookup"><span data-stu-id="86a4c-197">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="86a4c-198">`[Produces]` Filtre belirli bir eylem (veya denetleyicisi) yanıt biçimi belirtir.</span><span class="sxs-lookup"><span data-stu-id="86a4c-198">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="86a4c-199">İster en [filtreleri](xref:mvc/controllers/filters), bu eylem, denetleyici veya genel kapsamlı uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="86a4c-199">Like most [Filters](xref:mvc/controllers/filters), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="86a4c-200">`[Produces]` Filtre, içindeki tüm eylemleri zorlayacak `AuthorsController` diğer biçimlendiricileri uygulama ve sağlanan istemci için yapılandırılmış olsa bile, JSON biçimli yanıt döndürülecek bir `Accept` farklı, isteyen başlığı kullanılabilir biçimi.</span><span class="sxs-lookup"><span data-stu-id="86a4c-200">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="86a4c-201">Bkz: [filtreleri](xref:mvc/controllers/filters) nasıl genel filtre uygulamak da dahil olmak üzere daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="86a4c-201">See [Filters](xref:mvc/controllers/filters) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="86a4c-202">Özel durum Biçimlendiricileri</span><span class="sxs-lookup"><span data-stu-id="86a4c-202">Special Case Formatters</span></span>

<span data-ttu-id="86a4c-203">Bazı özel durumlar, yerleşik biçimlendiricileri kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="86a4c-203">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="86a4c-204">Varsayılan olarak, `string` dönüş türleri olarak biçimlendirilir *metin/düz* (*metin/html* aracılığıyla istenirse `Accept` üst bilgisi).</span><span class="sxs-lookup"><span data-stu-id="86a4c-204">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="86a4c-205">Bu davranış, kaldırarak kaldırılabilir `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="86a4c-205">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="86a4c-206">İçinde biçimlendiricileri Kaldır `Configure` yönteminde *Startup.cs* (aşağıda gösterilmiştir).</span><span class="sxs-lookup"><span data-stu-id="86a4c-206">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="86a4c-207">Bir model nesnesi olan eylemler dönüş türü döndürür bir 204 İçerik yanıt döndürülürken `null`.</span><span class="sxs-lookup"><span data-stu-id="86a4c-207">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="86a4c-208">Bu davranış, kaldırarak kaldırılabilir `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="86a4c-208">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="86a4c-209">Aşağıdaki kod kaldırır `TextOutputFormatter` ve `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="86a4c-209">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="86a4c-210">Olmadan `TextOutputFormatter`, `string` döndürür dönüş türleri 406 Kabul edilemez, örneğin.</span><span class="sxs-lookup"><span data-stu-id="86a4c-210">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="86a4c-211">XML biçimlendirici varsa, bunu biçimlendirecek Not `string` , dönüş türü `TextOutputFormatter` kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="86a4c-211">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="86a4c-212">Olmadan `HttpNoContentOutputFormatter`, null nesnelerine yapılandırılmış biçimlendirici kullanılarak biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="86a4c-212">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="86a4c-213">Örneğin, JSON biçimlendirici bir yanıt gövdesi ile yalnızca döndürür `null`, XML biçimlendiricisi özniteliğine sahip boş bir XML öğesi döndürür ancak `xsi:nil="true"` ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="86a4c-213">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="86a4c-214">Yanıt biçim URL eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="86a4c-214">Response Format URL Mappings</span></span>

<span data-ttu-id="86a4c-215">İstemcilerin belirli bir biçimde URL'nin bir parçası gibi sorgu dizesi ya da yolun veya .xml veya .json gibi bir biçim özel dosya uzantısını kullanarak talep edebilir.</span><span class="sxs-lookup"><span data-stu-id="86a4c-215">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="86a4c-216">İstek yolu eşlemesinden API'sini kullanarak bir yolun belirtilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="86a4c-216">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="86a4c-217">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="86a4c-217">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="86a4c-218">Bu yol, bir isteğe bağlı dosya uzantısı olarak belirtilmesi istenen biçimi izin verir.</span><span class="sxs-lookup"><span data-stu-id="86a4c-218">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="86a4c-219">`[FormatFilter]` Öznitelik biçimi değeri varlığını denetler `RouteData` ve yanıt oluşturulduğunda yanıt biçimi için uygun bir biçimlendirici eşler.</span><span class="sxs-lookup"><span data-stu-id="86a4c-219">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="86a4c-220">yol</span><span class="sxs-lookup"><span data-stu-id="86a4c-220">Route</span></span>            |             <span data-ttu-id="86a4c-221">Biçimlendirici</span><span class="sxs-lookup"><span data-stu-id="86a4c-221">Formatter</span></span>              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    <span data-ttu-id="86a4c-222">Varsayılan çıkış biçimlendirici</span><span class="sxs-lookup"><span data-stu-id="86a4c-222">The default output formatter</span></span>    |
| `/products/GetById/5.json` | <span data-ttu-id="86a4c-223">JSON biçimlendirici (yapılandırılmışsa)</span><span class="sxs-lookup"><span data-stu-id="86a4c-223">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="86a4c-224">XML biçimlendirici (yapılandırılmışsa)</span><span class="sxs-lookup"><span data-stu-id="86a4c-224">The XML formatter (if configured)</span></span>  |
