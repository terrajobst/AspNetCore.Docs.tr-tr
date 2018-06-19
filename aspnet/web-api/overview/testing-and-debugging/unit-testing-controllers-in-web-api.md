---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Birim ASP.NET Web API 2 denetleyicisi testi | Microsoft Docs
author: MikeWasson
description: Bu konu, birim denetleyicileri Web API 2'de test etmek için bazı belirli teknikleri açıklar. Bu konu okumadan önce birim öğretici okumak isteyebilirsiniz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: bda5148a4c1553d70f3173de66371fbb8576e83f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28039550"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="47f8e-104">Birim ASP.NET Web API 2 denetleyicisi testi</span><span class="sxs-lookup"><span data-stu-id="47f8e-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="47f8e-105">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="47f8e-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="47f8e-106">Bu konu, birim denetleyicileri Web API 2'de test etmek için bazı belirli teknikleri açıklar.</span><span class="sxs-lookup"><span data-stu-id="47f8e-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="47f8e-107">Bu konu okumadan önce öğreticiyi okumak isteyebilirsiniz [birim testi ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), çözümünüz için bir birim testi projesi eklemek nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="47f8e-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="47f8e-108">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="47f8e-108">Software versions used in the tutorial</span></span>
> 
> - [<span data-ttu-id="47f8e-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="47f8e-109">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="47f8e-110">Web API 2</span><span class="sxs-lookup"><span data-stu-id="47f8e-110">Web API 2</span></span>
> - <span data-ttu-id="47f8e-111">[Moq](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="47f8e-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="47f8e-112">Moq kullanılan, ancak aynı fikir herhangi mocking framework için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="47f8e-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="47f8e-113">Moq 4.5.30 (ve daha sonra) Visual Studio 2017, Roslyn ve .NET 4.5 ve sonraki sürümleri destekler.</span><span class="sxs-lookup"><span data-stu-id="47f8e-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="47f8e-114">Birim testlerinde ortak bir deseni &quot;Düzenle-act-assert&quot;:</span><span class="sxs-lookup"><span data-stu-id="47f8e-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="47f8e-115">Düzenleyin: çalıştırmak test için herhangi bir önkoşulu ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="47f8e-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="47f8e-116">ACT: test gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="47f8e-116">Act: Perform the test.</span></span>
- <span data-ttu-id="47f8e-117">Assert: sınama başarılı olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="47f8e-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="47f8e-118">Yerleştir adımda mock veya sık kullandığınız nesneleri saplama.</span><span class="sxs-lookup"><span data-stu-id="47f8e-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="47f8e-119">Test tek şey sınama odaklanmıştır şekilde bağımlılıkları, sayısı, en aza indirir.</span><span class="sxs-lookup"><span data-stu-id="47f8e-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="47f8e-120">Birim testi Web API denetleyicilerinizi gereken bazı noktalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="47f8e-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="47f8e-121">Eylem yanıt doğru türünü döndürür.</span><span class="sxs-lookup"><span data-stu-id="47f8e-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="47f8e-122">Geçersiz parametreler doğru hata yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="47f8e-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="47f8e-123">Eylem havuzu veya hizmet katmanı doğru yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="47f8e-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="47f8e-124">Yanıt etki alanı modeli içeriyorsa, model türü doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="47f8e-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="47f8e-125">Test etmek için genel şeylerden bazıları şunlardır ancak özellikleri denetleyicisi uygulamanızı bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="47f8e-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="47f8e-126">Denetleyici eylemleri dönüş olup olmadığını özellikle büyük fark kolaylaştırır **httpresponsemessage öğesini** veya **Ihttpactionresult**.</span><span class="sxs-lookup"><span data-stu-id="47f8e-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="47f8e-127">Bu sonuç türlerinden hakkında daha fazla bilgi için bkz: [eylem sonuçlarını Web API 2'deki](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="47f8e-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="47f8e-128">Bilgisayarın HttpResponseMessage dönüş eylemleri test etme</span><span class="sxs-lookup"><span data-stu-id="47f8e-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="47f8e-129">İşte bir örnek denetleyicisinin olan eylemler return **httpresponsemessage öğesini**.</span><span class="sxs-lookup"><span data-stu-id="47f8e-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="47f8e-130">Bildirim denetleyicisi eklemesine bağımlılık ekleme kullanan bir `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="47f8e-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="47f8e-131">Sahte bir depo ekleyemezsiniz çünkü denetleyicisi fazla sınanabilir yapıyorsa.</span><span class="sxs-lookup"><span data-stu-id="47f8e-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="47f8e-132">Aşağıdaki birim testi doğrular `Get` yöntemi yazma bir `Product` yanıt gövdesi için.</span><span class="sxs-lookup"><span data-stu-id="47f8e-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="47f8e-133">Varsayımında `repository` bir mock olduğu `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="47f8e-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="47f8e-134">Ayarlamak önemlidir **isteği** ve **yapılandırma** denetleyicisinde.</span><span class="sxs-lookup"><span data-stu-id="47f8e-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="47f8e-135">Aksi takdirde, test başarısız bir **ArgumentNullException** veya **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="47f8e-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="47f8e-136">Test bağlantısı oluşturma</span><span class="sxs-lookup"><span data-stu-id="47f8e-136">Testing Link Generation</span></span>

<span data-ttu-id="47f8e-137">`Post` Yöntem çağrılarını **UrlHelper.Link** yanıtta bağlantılar oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="47f8e-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="47f8e-138">Bu, birim testi biraz daha fazla kurulumunda gerektirir:</span><span class="sxs-lookup"><span data-stu-id="47f8e-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="47f8e-139">**UrlHelper** sınıf gereken istek URL'si ve rota verilerini bu değerleri ayarlamak test sahiptir.</span><span class="sxs-lookup"><span data-stu-id="47f8e-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="47f8e-140">Başka bir seçenek mock saplama mi **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="47f8e-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="47f8e-141">Bu yaklaşımda, varsayılan değeri değiştirin [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) sabit bir değer döndürür mock veya saplama sürüme sahip.</span><span class="sxs-lookup"><span data-stu-id="47f8e-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="47f8e-142">Şimdi kullanarak test yeniden [Moq](https://github.com/Moq) framework.</span><span class="sxs-lookup"><span data-stu-id="47f8e-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="47f8e-143">Yükleme `Moq` test projesinin NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="47f8e-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="47f8e-144">Bu sürümde, herhangi bir rota verileri kurmak çünkü gerekmez mock **UrlHelper** sabit bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="47f8e-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="47f8e-145">Ihttpactionresult dönüş eylemleri test etme</span><span class="sxs-lookup"><span data-stu-id="47f8e-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="47f8e-146">Web API 2'de bir denetleyici eylemi döndürebilir **Ihttpactionresult**, benzer olduğu **ActionResult** ASP.NET mvc'de.</span><span class="sxs-lookup"><span data-stu-id="47f8e-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="47f8e-147">**Ihttpactionresult** arabirimi HTTP yanıtları oluşturmak için bir komut desen tanımlar.</span><span class="sxs-lookup"><span data-stu-id="47f8e-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="47f8e-148">Denetleyici döndürür yanıtı doğrudan oluşturmak yerine, bir **Ihttpactionresult**.</span><span class="sxs-lookup"><span data-stu-id="47f8e-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="47f8e-149">Daha sonra ardışık düzen çağırır **Ihttpactionresult** yanıtı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="47f8e-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="47f8e-150">İçin gerekli Kurulumu çok atlayabilirsiniz çünkü bu yaklaşım, birim testleri yazma kolaylaştırır **httpresponsemessage öğesini**.</span><span class="sxs-lookup"><span data-stu-id="47f8e-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="47f8e-151">Olan eylemler return bir örnek controller işte **Ihttpactionresult**.</span><span class="sxs-lookup"><span data-stu-id="47f8e-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="47f8e-152">Bu örnek kullanan bazı ortak desenler gösterir **Ihttpactionresult**.</span><span class="sxs-lookup"><span data-stu-id="47f8e-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="47f8e-153">Görelim nasıl birimine bunları sınayın.</span><span class="sxs-lookup"><span data-stu-id="47f8e-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="47f8e-154">Eylem 200 (Tamam) ile bir yanıt gövdesi döndürür</span><span class="sxs-lookup"><span data-stu-id="47f8e-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="47f8e-155">`Get` Yöntem çağrılarını `Ok(product)` ürün bulunursa.</span><span class="sxs-lookup"><span data-stu-id="47f8e-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="47f8e-156">Birim testi dönüş türü olduğundan emin olun **OkNegotiatedContentResult** ve döndürülen ürün sağ kimliği vardır.</span><span class="sxs-lookup"><span data-stu-id="47f8e-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="47f8e-157">Birim testi eylem sonucu yürütmez dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="47f8e-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="47f8e-158">Eylem sonucunu doğru HTTP yanıtı oluşturur varsayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47f8e-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="47f8e-159">(Neden olan kendi birim testleri Web API çerçevesi sahip!)</span><span class="sxs-lookup"><span data-stu-id="47f8e-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="47f8e-160">Eylem 404 (bulunamadı) döndürür</span><span class="sxs-lookup"><span data-stu-id="47f8e-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="47f8e-161">`Get` Yöntem çağrılarını `NotFound()` ürün bulunmazsa.</span><span class="sxs-lookup"><span data-stu-id="47f8e-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="47f8e-162">Dönüş türü ise bu durumda, yalnızca denetimleri birim test **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="47f8e-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="47f8e-163">Eylem hiçbir yanıt gövdesi ile 200 (Tamam) döndürür</span><span class="sxs-lookup"><span data-stu-id="47f8e-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="47f8e-164">`Delete` Yöntem çağrılarını `Ok()` boş bir HTTP 200 yanıtı dönün.</span><span class="sxs-lookup"><span data-stu-id="47f8e-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="47f8e-165">Dönüş türü, birim testi gibi önceki örnekte, bu durumda denetler **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="47f8e-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="47f8e-166">Eylem bir konum üstbilgisi ile 201 (oluşturuldu) döndürür</span><span class="sxs-lookup"><span data-stu-id="47f8e-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="47f8e-167">`Post` Yöntem çağrılarını `CreatedAtRoute` konumu üstbilgisinde bir URI ile HTTP 201 yanıtını dönün.</span><span class="sxs-lookup"><span data-stu-id="47f8e-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="47f8e-168">Birim testi eylemi doğru yönlendirme değerlerini ayarlar doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="47f8e-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="47f8e-169">Yanıt gövdesi olan başka bir 2xx eylem döndürür</span><span class="sxs-lookup"><span data-stu-id="47f8e-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="47f8e-170">`Put` Yöntem çağrılarını `Content` yanıt gövdesi HTTP 202 (kabul edilen) yanıtıyla dönün.</span><span class="sxs-lookup"><span data-stu-id="47f8e-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="47f8e-171">Bu durumda, 200 (Tamam) döndürmek için benzer ancak birim testi durum kodunu denetlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="47f8e-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="47f8e-172">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="47f8e-172">Additional Resources</span></span>

- [<span data-ttu-id="47f8e-173">Entity Framework mocking zaman birim ASP.NET Web API 2 testi</span><span class="sxs-lookup"><span data-stu-id="47f8e-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="47f8e-174">[Bir ASP.NET Web API hizmeti için testleri yazma](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog yayını Youssef Moussaoui tarafından).</span><span class="sxs-lookup"><span data-stu-id="47f8e-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="47f8e-175">ASP.NET Web API rota hata ayıklayıcısı ile hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="47f8e-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
