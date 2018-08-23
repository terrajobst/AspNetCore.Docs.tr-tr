---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Web API 2'de eylem sonuçları | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/03/2014
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: b2b5ae5e5cef19e75a184aa28ac838a31e5ef1fd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754725"
---
<a name="action-results-in-web-api-2"></a><span data-ttu-id="c4120-102">Web API 2'de eylem sonuçları</span><span class="sxs-lookup"><span data-stu-id="c4120-102">Action Results in Web API 2</span></span>
====================
<span data-ttu-id="c4120-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c4120-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c4120-104">Bu konuda, nasıl ASP.NET Web API dönüş değeri bir denetleyici eylemi bir HTTP yanıt iletisine dönüştürür açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c4120-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="c4120-105">Bir Web API denetleyici eylemi aşağıdakilerden herhangi birini geri dönebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c4120-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="c4120-106">void</span><span class="sxs-lookup"><span data-stu-id="c4120-106">void</span></span>
2. <span data-ttu-id="c4120-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="c4120-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="c4120-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="c4120-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="c4120-109">Başka bir türü</span><span class="sxs-lookup"><span data-stu-id="c4120-109">Some other type</span></span>

<span data-ttu-id="c4120-110">Bu bağlı olarak getirilen, Web API'si, HTTP yanıtı oluşturmak için farklı bir mekanizma kullanır.</span><span class="sxs-lookup"><span data-stu-id="c4120-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="c4120-111">Dönüş türü</span><span class="sxs-lookup"><span data-stu-id="c4120-111">Return type</span></span> | <span data-ttu-id="c4120-112">Web API'si yanıtı nasıl oluşturur?</span><span class="sxs-lookup"><span data-stu-id="c4120-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="c4120-113">void</span><span class="sxs-lookup"><span data-stu-id="c4120-113">void</span></span> | <span data-ttu-id="c4120-114">Boş dönüş 204 (içerik yok)</span><span class="sxs-lookup"><span data-stu-id="c4120-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="c4120-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="c4120-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="c4120-116">Bir HTTP yanıt iletisi doğrudan dönüştürün.</span><span class="sxs-lookup"><span data-stu-id="c4120-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="c4120-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="c4120-117">**IHttpActionResult**</span></span> | <span data-ttu-id="c4120-118">Çağrı **ExecuteAsync** oluşturmak için bir **HttpResponseMessage**, ardından bir HTTP yanıt iletisi dönüştürün.</span><span class="sxs-lookup"><span data-stu-id="c4120-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="c4120-119">Diğer tür</span><span class="sxs-lookup"><span data-stu-id="c4120-119">Other type</span></span> | <span data-ttu-id="c4120-120">Yanıt gövdesine seri hale getirilmiş dönüş değeri yazabilirsiniz. 200 (Tamam) döndürür.</span><span class="sxs-lookup"><span data-stu-id="c4120-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="c4120-121">Bu konunun geri kalanında her bir seçenek daha ayrıntılı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c4120-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="c4120-122">void</span><span class="sxs-lookup"><span data-stu-id="c4120-122">void</span></span>

<span data-ttu-id="c4120-123">Dönüş türü ise `void`, Web API'sini yalnızca boş bir HTTP yanıtının durum kodu 204 (içerik yok) döndürür.</span><span class="sxs-lookup"><span data-stu-id="c4120-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="c4120-124">Örnek denetleyicisi:</span><span class="sxs-lookup"><span data-stu-id="c4120-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="c4120-125">HTTP yanıtı:</span><span class="sxs-lookup"><span data-stu-id="c4120-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="c4120-126">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="c4120-126">HttpResponseMessage</span></span>

<span data-ttu-id="c4120-127">Eylem döndürürse bir [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API'si dönüştürür dönüş değeri doğrudan bir HTTP yanıt iletisine, özelliklerini kullanarak **HttpResponseMessage** doldurmak için nesne yanıt.</span><span class="sxs-lookup"><span data-stu-id="c4120-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="c4120-128">Bu seçenek birçok yanıt iletisi üzerinde denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="c4120-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="c4120-129">Örneğin, aşağıdaki denetleyici eylemi Cache-Control üst bilgisi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c4120-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="c4120-130">Yanıt:</span><span class="sxs-lookup"><span data-stu-id="c4120-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="c4120-131">Bir etki alanı modeline geçirirseniz **CreateResponse** yöntemi, Web API'sini kullanan bir [medya biçimlendiricisi](../formats-and-model-binding/media-formatters.md) serileştirilmiş modeli yanıt gövdesine yazılacak.</span><span class="sxs-lookup"><span data-stu-id="c4120-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="c4120-132">Web API'si, biçimlendirici seçmek için istekte Accept üst bilgisi kullanır.</span><span class="sxs-lookup"><span data-stu-id="c4120-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="c4120-133">Daha fazla bilgi için [içerik anlaşması](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="c4120-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="c4120-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="c4120-134">IHttpActionResult</span></span>

<span data-ttu-id="c4120-135">**Ihttpactionresult** arabirimi, Web API 2'de tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="c4120-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="c4120-136">Esas olarak tanımlayan bir **HttpResponseMessage** üreteci.</span><span class="sxs-lookup"><span data-stu-id="c4120-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="c4120-137">Kullanmanın bazı avantajları şunlardır **Ihttpactionresult** arabirimi:</span><span class="sxs-lookup"><span data-stu-id="c4120-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="c4120-138">Basitleştirir [birim testi](../testing-and-debugging/unit-testing-controllers-in-web-api.md) denetleyicilerinizi.</span><span class="sxs-lookup"><span data-stu-id="c4120-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="c4120-139">HTTP yanıt ayrı sınıflara oluşturmak için sık kullanılan mantıksal taşır.</span><span class="sxs-lookup"><span data-stu-id="c4120-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="c4120-140">Yanıt oluşturmak, alt düzey ayrıntıları gizleyerek amacı, NET, denetleyici eylemi sağlar.</span><span class="sxs-lookup"><span data-stu-id="c4120-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="c4120-141">**Ihttpactionresult** içeren tek bir yöntem **ExecuteAsync**, zaman uyumsuz olarak oluşturan bir **HttpResponseMessage** örneği.</span><span class="sxs-lookup"><span data-stu-id="c4120-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="c4120-142">Bir denetleyici eylemi döndürürse bir **Ihttpactionresult**, Web API'si çağıran **ExecuteAsync** yöntemi oluşturmak için bir **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="c4120-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="c4120-143">Bunu dönüştürür **HttpResponseMessage** içine bir HTTP yanıt iletisi.</span><span class="sxs-lookup"><span data-stu-id="c4120-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="c4120-144">İşte, basit bir implementaton **Ihttpactionresult** düz metin yanıtı oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c4120-144">Here is a simple implementaton of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="c4120-145">Örnek denetleyici eylem:</span><span class="sxs-lookup"><span data-stu-id="c4120-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="c4120-146">Yanıt:</span><span class="sxs-lookup"><span data-stu-id="c4120-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="c4120-147">Daha sık kullanacağınız **Ihttpactionresult** tanımlanan uygulamaları **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** ad alanı.</span><span class="sxs-lookup"><span data-stu-id="c4120-147">More often, you will use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="c4120-148">**ApiController** sınıfı, bu yerleşik eylem sonuçları döndüren yardımcı yöntemler tanımlar.</span><span class="sxs-lookup"><span data-stu-id="c4120-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="c4120-149">Aşağıdaki örnekte, istek var olan bir ürün kimliği eşleşmiyorsa denetleyicisi çağıran [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) 404 (bulunamadı) yanıt oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="c4120-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="c4120-150">Aksi takdirde, denetleyiciyi çağıran [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), ürün, 200 (Tamam) bir yanıt oluşturan içerir.</span><span class="sxs-lookup"><span data-stu-id="c4120-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="c4120-151">Diğer dönüş türleri</span><span class="sxs-lookup"><span data-stu-id="c4120-151">Other Return Types</span></span>

<span data-ttu-id="c4120-152">Diğer tüm dönüş türleri için Web API'si kullanan bir [medya biçimlendiricisi](../formats-and-model-binding/media-formatters.md) dönüş değeri serileştirmek için.</span><span class="sxs-lookup"><span data-stu-id="c4120-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="c4120-153">Web API'si, yanıt gövdesine seri hale getirilmiş değer yazar.</span><span class="sxs-lookup"><span data-stu-id="c4120-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="c4120-154">Yanıt durum kodu 200 (Tamam) ' dir.</span><span class="sxs-lookup"><span data-stu-id="c4120-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="c4120-155">Bu yaklaşımın bir dezavantajı, 404 gibi bir hata kodu doğrudan döndüremez ' dir.</span><span class="sxs-lookup"><span data-stu-id="c4120-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="c4120-156">Ancak, oluşturabilecek bir **HttpResponseException** hata kodları.</span><span class="sxs-lookup"><span data-stu-id="c4120-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="c4120-157">Daha fazla bilgi için [ASP.NET Web API'de özel durum işleme](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="c4120-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="c4120-158">Web API'si, biçimlendirici seçmek için istekte Accept üst bilgisi kullanır.</span><span class="sxs-lookup"><span data-stu-id="c4120-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="c4120-159">Daha fazla bilgi için [içerik anlaşması](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="c4120-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="c4120-160">Örnek istek</span><span class="sxs-lookup"><span data-stu-id="c4120-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="c4120-161">Örnek yanıt:</span><span class="sxs-lookup"><span data-stu-id="c4120-161">Example response:</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
