---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Test denetleyicileri ASP.NET Web API 2 birim | Microsoft Docs
author: MikeWasson
description: Bu konu, Web API 2'de test denetleyicileri birim için belirli bazı teknikleri açıklar. Bu konuda okumadan önce öğretici birim okumak isteyebilirsiniz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 1a3cfa1962a5f914fd2393088bec4424f6453d07
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389389"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="5cae5-104">Test denetleyicileri ASP.NET Web API 2 birim</span><span class="sxs-lookup"><span data-stu-id="5cae5-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="5cae5-105">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5cae5-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="5cae5-106">Bu konu, Web API 2'de test denetleyicileri birim için belirli bazı teknikleri açıklar.</span><span class="sxs-lookup"><span data-stu-id="5cae5-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="5cae5-107">Bu konuda okumadan önce öğretici okumak isteyebilirsiniz [birim testi ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), nasıl bir birim test projesi çözümünüze ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5cae5-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5cae5-108">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="5cae5-108">Software versions used in the tutorial</span></span>
> 
> - [<span data-ttu-id="5cae5-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="5cae5-109">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="5cae5-110">Web API 2</span><span class="sxs-lookup"><span data-stu-id="5cae5-110">Web API 2</span></span>
> - <span data-ttu-id="5cae5-111">[Moq](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="5cae5-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="5cae5-112">Moq kullandım, ancak aynı fikir sahte herhangi bir çerçeveyi için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="5cae5-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="5cae5-113">Moq 4.5.30 (ve sonraki sürümler) Visual Studio 2017, Roslyn ve .NET 4.5 ve sonraki sürümleri destekler.</span><span class="sxs-lookup"><span data-stu-id="5cae5-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="5cae5-114">Birim testlerinde bir ortak desendir &quot;düzenleme-act-assert&quot;:</span><span class="sxs-lookup"><span data-stu-id="5cae5-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="5cae5-115">Düzenleme: testin çalışması için tüm önkoşulları ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="5cae5-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="5cae5-116">ACT: test gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="5cae5-116">Act: Perform the test.</span></span>
- <span data-ttu-id="5cae5-117">Assert: test başarılı olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5cae5-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="5cae5-118">Düzenle adımda sahte veya sık kullandığınız saptama nesneleri.</span><span class="sxs-lookup"><span data-stu-id="5cae5-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="5cae5-119">Bir şey testlere test odaklı için bağımlılık sayısı, en aza indirir.</span><span class="sxs-lookup"><span data-stu-id="5cae5-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="5cae5-120">Birim testi, Web APİ'si denetleyicilerinin içinde gereken bazı noktalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5cae5-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="5cae5-121">Eylem türü doğru yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="5cae5-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="5cae5-122">Geçersiz parametreler doğru hata yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="5cae5-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="5cae5-123">Eylem deposuna veya hizmet katmanı doğru yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="5cae5-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="5cae5-124">Yanıt, bir etki alanı modeli içeriyorsa, model türü doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5cae5-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="5cae5-125">Bazı test etmek için gereken genel noktalar şunlardır ancak özellikleri denetleyicisi uygulamanıza bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="5cae5-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="5cae5-126">Denetleyici eylemleri dönüş özellikle büyük bir fark netleştirir **HttpResponseMessage** veya **Ihttpactionresult**.</span><span class="sxs-lookup"><span data-stu-id="5cae5-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="5cae5-127">Bu sonuç türleri hakkında daha fazla bilgi için bkz. [Web API 2'de eylem sonuçları](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="5cae5-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="5cae5-128">Test HttpResponseMessage dönüş eylemleri</span><span class="sxs-lookup"><span data-stu-id="5cae5-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="5cae5-129">İşte bir örnek etki alanı denetleyicisinin, Eylemler dönüş **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="5cae5-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="5cae5-130">Bildirim denetleyicisi eklemesine bağımlılık ekleme kullanan bir `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="5cae5-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="5cae5-131">Sahte bir depo ekleyemezsiniz çünkü, denetleyici daha test edilebilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="5cae5-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="5cae5-132">Aşağıdaki birim sınama doğrular `Get` yönteminin yazma bir `Product` yanıt gövdesi için.</span><span class="sxs-lookup"><span data-stu-id="5cae5-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="5cae5-133">Varsayımında `repository` bir sahte olduğu `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="5cae5-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="5cae5-134">Ayarlamak önemlidir **istek** ve **yapılandırma** denetleyicisinde.</span><span class="sxs-lookup"><span data-stu-id="5cae5-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="5cae5-135">Aksi halde, test başarısız olur bir **ArgumentNullException** veya **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="5cae5-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="5cae5-136">Sınama bağlantısı oluşturma</span><span class="sxs-lookup"><span data-stu-id="5cae5-136">Testing Link Generation</span></span>

<span data-ttu-id="5cae5-137">`Post` Yöntem çağrılarını **UrlHelper.Link** yanıtta bağlantılar oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="5cae5-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="5cae5-138">Bu işlem biraz daha fazla birim testi ayarında gerektirir:</span><span class="sxs-lookup"><span data-stu-id="5cae5-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="5cae5-139">**UrlHelper** sınıfı istek URL'si ve rota verilerini test için bu değerleri ayarlamak sahip olması gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="5cae5-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="5cae5-140">Saplama sahte veya başka bir seçenek olan **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="5cae5-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="5cae5-141">Bu yaklaşımda, varsayılan değeri değiştirin [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) sabit bir değer döndüren saptama sahte veya sürüme sahip.</span><span class="sxs-lookup"><span data-stu-id="5cae5-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="5cae5-142">Şimdi test kullanarak yeniden [Moq](https://github.com/Moq) framework.</span><span class="sxs-lookup"><span data-stu-id="5cae5-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="5cae5-143">Yükleme `Moq` test projesinde NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="5cae5-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="5cae5-144">Bu sürümde, tüm rota verileri, ayarlayın çünkü gerekmez sahte **UrlHelper** sabit bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="5cae5-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="5cae5-145">Test Ihttpactionresult dönüş eylemleri</span><span class="sxs-lookup"><span data-stu-id="5cae5-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="5cae5-146">Web API 2'de bir denetleyici eylemi döndürebilir **Ihttpactionresult**, alınmak üzere olduğu **ActionResult** ASP.NET mvc'de.</span><span class="sxs-lookup"><span data-stu-id="5cae5-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="5cae5-147">**Ihttpactionresult** arabirimi HTTP yanıt oluşturmak için bir komut desen tanımlar.</span><span class="sxs-lookup"><span data-stu-id="5cae5-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="5cae5-148">Yanıt doğrudan oluşturmak yerine, denetleyici döndürür bir **Ihttpactionresult**.</span><span class="sxs-lookup"><span data-stu-id="5cae5-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="5cae5-149">Daha sonra işlem hattını çağıran **Ihttpactionresult** yanıt oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="5cae5-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="5cae5-150">Kurulum için gereken çok fazla atlayabilirsiniz çünkü bu yaklaşım, birim testleri yazma kolaylaştırır **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="5cae5-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="5cae5-151">Olan eylemler dönüş örnek controller işte **Ihttpactionresult**.</span><span class="sxs-lookup"><span data-stu-id="5cae5-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="5cae5-152">Bu örnek kullanan bazı ortak desenleri gösterir **Ihttpactionresult**.</span><span class="sxs-lookup"><span data-stu-id="5cae5-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="5cae5-153">Bakalım nasıl birimine bunları test edin.</span><span class="sxs-lookup"><span data-stu-id="5cae5-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="5cae5-154">Eylem 200 (Tamam) ile bir yanıt gövdesini döndürür</span><span class="sxs-lookup"><span data-stu-id="5cae5-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="5cae5-155">`Get` Yöntem çağrılarını `Ok(product)` ürün bulunursa.</span><span class="sxs-lookup"><span data-stu-id="5cae5-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="5cae5-156">Birim test, dönüş türü olduğundan emin olun **OkNegotiatedContentResult** ve döndürülen ürün doğru kimliği.</span><span class="sxs-lookup"><span data-stu-id="5cae5-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="5cae5-157">Birim testi dikkat edin, eylem sonucu yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="5cae5-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="5cae5-158">Eylem sonucu doğru HTTP yanıtını oluşturur varsayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5cae5-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="5cae5-159">(Nedeni kendi birim testleri Web API çerçevesi olan!)</span><span class="sxs-lookup"><span data-stu-id="5cae5-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="5cae5-160">Eylem 404 (bulunamadı) döndürür.</span><span class="sxs-lookup"><span data-stu-id="5cae5-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="5cae5-161">`Get` Yöntem çağrılarını `NotFound()` ürüne bulunamazsa.</span><span class="sxs-lookup"><span data-stu-id="5cae5-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="5cae5-162">Bu durumda, dönüş türü ise yalnızca denetimleri birim testi **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="5cae5-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="5cae5-163">Eylem hiç yanıt gövdesi ile 200 (Tamam) döndürür.</span><span class="sxs-lookup"><span data-stu-id="5cae5-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="5cae5-164">`Delete` Yöntem çağrılarını `Ok()` boş bir HTTP 200 yanıtı dönün.</span><span class="sxs-lookup"><span data-stu-id="5cae5-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="5cae5-165">Bu durumda dönüş türü, önceki örnekteki gibi birim testi denetler **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="5cae5-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="5cae5-166">Eylemi bir konum üst bilgisi ile 201 (oluşturuldu) döndürür.</span><span class="sxs-lookup"><span data-stu-id="5cae5-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="5cae5-167">`Post` Yöntem çağrılarını `CreatedAtRoute` konum üst bilgisinde bir URI ile bir HTTP 201 yanıtı dönün.</span><span class="sxs-lookup"><span data-stu-id="5cae5-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="5cae5-168">Birim test, eylemin doğru yönlendirme değerleri ayarlar doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5cae5-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="5cae5-169">Yanıt gövdesi olan başka bir 2xx eylem döndürür</span><span class="sxs-lookup"><span data-stu-id="5cae5-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="5cae5-170">`Put` Yöntem çağrılarını `Content` yanıt gövdesi ile bir HTTP 202 (kabul edildi) yanıtı dönün.</span><span class="sxs-lookup"><span data-stu-id="5cae5-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="5cae5-171">Bu durumda, 200 (Tamam) döndürmek için benzer, ancak birim testi ayrıca durum kodunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="5cae5-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="5cae5-172">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5cae5-172">Additional Resources</span></span>

- [<span data-ttu-id="5cae5-173">Sahte Entity Framework, birim testi ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="5cae5-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="5cae5-174">[Bir ASP.NET Web API'si hizmeti için testleri yazma](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog gönderisi Youssef Moussaoui tarafından).</span><span class="sxs-lookup"><span data-stu-id="5cae5-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="5cae5-175">ASP.NET Web API rota hata ayıklayıcısı ile hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="5cae5-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
