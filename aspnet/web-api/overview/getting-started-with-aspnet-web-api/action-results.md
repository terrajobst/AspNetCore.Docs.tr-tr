---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Eylem sonuçlarını Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: d0db5c6d45020861d7295ab1db989caee525fff9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036472"
---
<a name="action-results-in-web-api-2"></a><span data-ttu-id="20c90-102">Eylem sonuçlarını Web API 2</span><span class="sxs-lookup"><span data-stu-id="20c90-102">Action Results in Web API 2</span></span>
====================
<span data-ttu-id="20c90-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="20c90-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="20c90-104">Bu konu, nasıl ASP.NET Web API dönüş değeri bir denetleyici eylemi bir HTTP yanıt iletisine dönüştürür açıklar.</span><span class="sxs-lookup"><span data-stu-id="20c90-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="20c90-105">Bir Web API denetleyici eylemi aşağıdakilerden herhangi birini döndürebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="20c90-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="20c90-106">void</span><span class="sxs-lookup"><span data-stu-id="20c90-106">void</span></span>
2. <span data-ttu-id="20c90-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="20c90-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="20c90-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="20c90-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="20c90-109">Başka bir türü</span><span class="sxs-lookup"><span data-stu-id="20c90-109">Some other type</span></span>

<span data-ttu-id="20c90-110">Bunlar hangisinin bağlı olarak, Web API HTTP yanıtı oluşturmak için farklı bir mekanizma kullanan döndürülür.</span><span class="sxs-lookup"><span data-stu-id="20c90-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="20c90-111">Dönüş türü</span><span class="sxs-lookup"><span data-stu-id="20c90-111">Return type</span></span> | <span data-ttu-id="20c90-112">Web API yanıt nasıl oluşturur</span><span class="sxs-lookup"><span data-stu-id="20c90-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="20c90-113">void</span><span class="sxs-lookup"><span data-stu-id="20c90-113">void</span></span> | <span data-ttu-id="20c90-114">Dönüş boş 204 (No içerik)</span><span class="sxs-lookup"><span data-stu-id="20c90-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="20c90-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="20c90-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="20c90-116">Bir HTTP yanıt iletisini doğrudan dönüştürün.</span><span class="sxs-lookup"><span data-stu-id="20c90-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="20c90-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="20c90-117">**IHttpActionResult**</span></span> | <span data-ttu-id="20c90-118">Çağrı **ExecuteAsync** oluşturmak için bir **httpresponsemessage öğesini**, bir HTTP yanıt iletisini dönüştürün.</span><span class="sxs-lookup"><span data-stu-id="20c90-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="20c90-119">Diğer türü</span><span class="sxs-lookup"><span data-stu-id="20c90-119">Other type</span></span> | <span data-ttu-id="20c90-120">Serileştirilmiş dönüş değeri yanıt gövdesi yazma; 200 (Tamam) döndürür.</span><span class="sxs-lookup"><span data-stu-id="20c90-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="20c90-121">Bu konunun geri kalanında her seçeneği daha ayrıntılı açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="20c90-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="20c90-122">void</span><span class="sxs-lookup"><span data-stu-id="20c90-122">void</span></span>

<span data-ttu-id="20c90-123">Dönüş türü ise `void`, Web API yalnızca boş bir HTTP yanıt durum koduyla 204 (No içerik) döndürür.</span><span class="sxs-lookup"><span data-stu-id="20c90-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="20c90-124">Örnek denetleyicisi:</span><span class="sxs-lookup"><span data-stu-id="20c90-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="20c90-125">HTTP yanıtı:</span><span class="sxs-lookup"><span data-stu-id="20c90-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="20c90-126">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="20c90-126">HttpResponseMessage</span></span>

<span data-ttu-id="20c90-127">Eylem döndürürse bir [httpresponsemessage öğesini](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API dönüştürür dönüş değeri doğrudan bir HTTP yanıt iletisine, özelliklerini kullanarak **httpresponsemessage öğesini** doldurmak için nesne yanıt.</span><span class="sxs-lookup"><span data-stu-id="20c90-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="20c90-128">Bu seçenek büyük bir yanıt iletisi üzerinde denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="20c90-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="20c90-129">Örneğin, aşağıdaki denetleyici eylemi Cache-Control üstbilgisinin ayarlar.</span><span class="sxs-lookup"><span data-stu-id="20c90-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="20c90-130">Yanıtı:</span><span class="sxs-lookup"><span data-stu-id="20c90-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="20c90-131">Bir etki alanı modeline geçirirseniz **CreateResponse** yöntemi, Web API'sini kullanan bir [medya biçimlendiricisi](../formats-and-model-binding/media-formatters.md) yanıt gövdesi seri hale getirilmiş modeli yazmak için.</span><span class="sxs-lookup"><span data-stu-id="20c90-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="20c90-132">Web API biçimlendirici seçmek için istek kabul etme üstbilgisi kullanır.</span><span class="sxs-lookup"><span data-stu-id="20c90-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="20c90-133">Daha fazla bilgi için bkz: [içerik anlaşması](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="20c90-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="20c90-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="20c90-134">IHttpActionResult</span></span>

<span data-ttu-id="20c90-135">**Ihttpactionresult** arabirimi, Web API 2'de sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="20c90-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="20c90-136">Esas olarak, tanımlayan bir **httpresponsemessage öğesini** üreteci.</span><span class="sxs-lookup"><span data-stu-id="20c90-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="20c90-137">Kullanmanın bazı avantajları şunlardır **Ihttpactionresult** arabirimi:</span><span class="sxs-lookup"><span data-stu-id="20c90-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="20c90-138">Basitleştirir [birim testi](../testing-and-debugging/unit-testing-controllers-in-web-api.md) denetleyicilerinizi.</span><span class="sxs-lookup"><span data-stu-id="20c90-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="20c90-139">HTTP yanıt ayrı sınıflara oluşturmak için ortak mantığı taşır.</span><span class="sxs-lookup"><span data-stu-id="20c90-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="20c90-140">Yanıt oluşturma alt düzey ayrıntılarını gizleme tarafından NET, denetleyici eylem amacı yapar.</span><span class="sxs-lookup"><span data-stu-id="20c90-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="20c90-141">**Ihttpactionresult** tek bir yöntem içerir **ExecuteAsync**, zaman uyumsuz olarak oluşturan bir **httpresponsemessage öğesini** örneği.</span><span class="sxs-lookup"><span data-stu-id="20c90-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="20c90-142">Bir denetleyici eylemi döndürürse bir **Ihttpactionresult**, Web API çağrılarının **ExecuteAsync** yöntemi oluşturmak için bir **httpresponsemessage öğesini**.</span><span class="sxs-lookup"><span data-stu-id="20c90-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="20c90-143">Bunu dönüştüren sonra **httpresponsemessage öğesini** bir HTTP yanıt iletisine.</span><span class="sxs-lookup"><span data-stu-id="20c90-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="20c90-144">Basit bir implementaton, işte **Ihttpactionresult** bir düz metin yanıt oluşturur:</span><span class="sxs-lookup"><span data-stu-id="20c90-144">Here is a simple implementaton of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="20c90-145">Örnek denetleyici eylem:</span><span class="sxs-lookup"><span data-stu-id="20c90-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="20c90-146">Yanıtı:</span><span class="sxs-lookup"><span data-stu-id="20c90-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="20c90-147">Daha sık kullanacağınız **Ihttpactionresult** tanımlanan uygulamaları  **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)**  ad alanı.</span><span class="sxs-lookup"><span data-stu-id="20c90-147">More often, you will use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="20c90-148">**ApiController** sınıfı, bu yerleşik eylem sonuçları döndüren Yardımcısı yöntemleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="20c90-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="20c90-149">Aşağıdaki örnekte, denetleyici istek var olan bir ürün kimliği eşleşmiyorsa çağırır [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) 404 (bulunamadı) yanıt oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="20c90-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="20c90-150">Aksi takdirde, denetleyici çağırır [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), hangi 200 (Tamam) bir yanıt oluşturan ürün içerir.</span><span class="sxs-lookup"><span data-stu-id="20c90-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="20c90-151">Diğer dönüş türleri</span><span class="sxs-lookup"><span data-stu-id="20c90-151">Other Return Types</span></span>

<span data-ttu-id="20c90-152">Diğer tüm dönüş türleri için Web API'sini kullanan bir [medya biçimlendiricisi](../formats-and-model-binding/media-formatters.md) dönüş değeri serileştirmek için.</span><span class="sxs-lookup"><span data-stu-id="20c90-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="20c90-153">Web API serileştirilmiş değer yanıt gövdesi yazar.</span><span class="sxs-lookup"><span data-stu-id="20c90-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="20c90-154">Yanıt durum kodu 200 (Tamam)'dür.</span><span class="sxs-lookup"><span data-stu-id="20c90-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="20c90-155">Bu yaklaşımın bir dezavantajı, 404 gibi bir hata kodu doğrudan döndüremez ' dir.</span><span class="sxs-lookup"><span data-stu-id="20c90-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="20c90-156">Ancak, throw bir **HttpResponseException** hata kodları için.</span><span class="sxs-lookup"><span data-stu-id="20c90-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="20c90-157">Daha fazla bilgi için bkz: [özel durum işleme ASP.NET Web API içinde](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="20c90-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="20c90-158">Web API biçimlendirici seçmek için istek kabul etme üstbilgisi kullanır.</span><span class="sxs-lookup"><span data-stu-id="20c90-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="20c90-159">Daha fazla bilgi için bkz: [içerik anlaşması](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="20c90-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="20c90-160">Örnek istek</span><span class="sxs-lookup"><span data-stu-id="20c90-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="20c90-161">Örnek yanıt:</span><span class="sxs-lookup"><span data-stu-id="20c90-161">Example response:</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
