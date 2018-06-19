---
uid: web-api/overview/advanced/http-message-handlers
title: ASP.NET Web API HTTP ileti işleyicileri | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 931ad3330d5e4dc2424ab37b8a6e2a3d123a8684
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566391"
---
<a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="e4935-102">ASP.NET Web API HTTP ileti işleyicileri</span><span class="sxs-lookup"><span data-stu-id="e4935-102">HTTP Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="e4935-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e4935-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e4935-104">A *ileti işleyicisi* bir HTTP isteğini alır ve bir HTTP yanıtı döndüren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="e4935-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="e4935-105">İleti işleyicileri türetilen Özet **HttpMessageHandler** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e4935-105">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="e4935-106">Genellikle, bir dizi ileti işleyicileri zincirleme birlikte.</span><span class="sxs-lookup"><span data-stu-id="e4935-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="e4935-107">İlk işleyici bir HTTP isteği aldığında, bir işlem yapar ve sonraki işleyicisi isteği verir.</span><span class="sxs-lookup"><span data-stu-id="e4935-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="e4935-108">Belirli bir noktada yanıtı oluşturulur ve tekrar zincirinde geçer.</span><span class="sxs-lookup"><span data-stu-id="e4935-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="e4935-109">Bu desen adlı bir *temsilci* işleyicisi.</span><span class="sxs-lookup"><span data-stu-id="e4935-109">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="e4935-110">Sunucu tarafı ileti işleyicileri</span><span class="sxs-lookup"><span data-stu-id="e4935-110">Server-Side Message Handlers</span></span>

<span data-ttu-id="e4935-111">Sunucu tarafında Web API ardışık düzeni bazı yerleşik ileti işleyicileri kullanır:</span><span class="sxs-lookup"><span data-stu-id="e4935-111">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="e4935-112">**HttpServer** istek ana bilgisayardan alır.</span><span class="sxs-lookup"><span data-stu-id="e4935-112">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="e4935-113">**HttpRoutingDispatcher** Rotaya bağlı isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="e4935-113">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="e4935-114">**HttpControllerDispatcher** bir Web API denetleyicisi isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="e4935-114">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="e4935-115">Özel işleyicileri ardışık düzene ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e4935-115">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="e4935-116">İleti işleyicileri HTTP iletileri (yerine denetleyici eylemleri) düzeyinde çalışır arası kesme sorunları için iyidir.</span><span class="sxs-lookup"><span data-stu-id="e4935-116">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="e4935-117">Örneğin, bir ileti işleyicisi olabilir:</span><span class="sxs-lookup"><span data-stu-id="e4935-117">For example, a message handler might:</span></span>

- <span data-ttu-id="e4935-118">Okuma veya istek üstbilgileri değiştirin.</span><span class="sxs-lookup"><span data-stu-id="e4935-118">Read or modify request headers.</span></span>
- <span data-ttu-id="e4935-119">Bir yanıt üstbilgisi yanıtlarını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e4935-119">Add a response header to responses.</span></span>
- <span data-ttu-id="e4935-120">Denetleyici düşmeden önce istekleri doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="e4935-120">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="e4935-121">Bu diyagramda ardışık düzenine eklenen iki özel işleyicileri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="e4935-121">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="e4935-122">İstemci tarafında HttpClient ileti işleyicileri de kullanır.</span><span class="sxs-lookup"><span data-stu-id="e4935-122">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="e4935-123">Daha fazla bilgi için bkz: [HttpClient ileti işleyicileri](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="e4935-123">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="e4935-124">Özel ileti işleyicileri</span><span class="sxs-lookup"><span data-stu-id="e4935-124">Custom Message Handlers</span></span>

<span data-ttu-id="e4935-125">Özel ileti işleyicisi yazma için türetilen **System.Net.Http.DelegatingHandler** ve geçersiz kılma **SendAsync** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e4935-125">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="e4935-126">Bu yöntem aşağıdaki imzası vardır:</span><span class="sxs-lookup"><span data-stu-id="e4935-126">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="e4935-127">Yöntem alır bir **HttpRequestMessage** giriş ve zaman uyumsuz olarak döndürür olarak bir **httpresponsemessage öğesini**.</span><span class="sxs-lookup"><span data-stu-id="e4935-127">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="e4935-128">Tipik bir uygulama şunları yapar:</span><span class="sxs-lookup"><span data-stu-id="e4935-128">A typical implementation does the following:</span></span>

1. <span data-ttu-id="e4935-129">İşlem istek iletisi.</span><span class="sxs-lookup"><span data-stu-id="e4935-129">Process the request message.</span></span>
2. <span data-ttu-id="e4935-130">Çağrı `base.SendAsync` iç işleyici isteği göndermek için.</span><span class="sxs-lookup"><span data-stu-id="e4935-130">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="e4935-131">İç işleyici bir yanıt iletisi döndürür.</span><span class="sxs-lookup"><span data-stu-id="e4935-131">The inner handler returns a response message.</span></span> <span data-ttu-id="e4935-132">(Bu adım, zaman uyumsuz bağlıdır.)</span><span class="sxs-lookup"><span data-stu-id="e4935-132">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="e4935-133">Yanıtı işlemek ve çağırana döndürür.</span><span class="sxs-lookup"><span data-stu-id="e4935-133">Process the response and return it to the caller.</span></span>

<span data-ttu-id="e4935-134">Önemsiz bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="e4935-134">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="e4935-135">Çağrı `base.SendAsync` zaman uyumsuz olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="e4935-135">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="e4935-136">İşleyici bu çağrısından sonra herhangi bir iş varsa, kullanmak **await** gösterildiği gibi anahtar.</span><span class="sxs-lookup"><span data-stu-id="e4935-136">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>


<span data-ttu-id="e4935-137">Temsilci işleyici Ayrıca iç işleyici atlayın ve doğrudan yanıt oluşturun:</span><span class="sxs-lookup"><span data-stu-id="e4935-137">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="e4935-138">Bir temsilci çağırmadan yanıt işleyici oluşturur `base.SendAsync`, istek kalan ardışık düzenini atlar.</span><span class="sxs-lookup"><span data-stu-id="e4935-138">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="e4935-139">(Bir hata yanıtı oluşturma) istek doğrulama için bir işleyici yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="e4935-139">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="e4935-140">Ardışık düzene bir işleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="e4935-140">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="e4935-141">Sunucu tarafında bir ileti işleyicisini eklemek, ekleyebilirsiniz **HttpConfiguration.MessageHandlers** koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="e4935-141">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="e4935-142">Projeyi oluşturmak için "ASP.NET MVC 4 Web uygulaması" şablonu kullandıysanız, bu iç yapabileceğiniz **WebApiConfig** sınıfı:</span><span class="sxs-lookup"><span data-stu-id="e4935-142">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="e4935-143">İleti işleyicileri görüntülendikleri aynı sırayla çağrılır **MessageHandlers** koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="e4935-143">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="e4935-144">İç içe çünkü yanıt iletisi ters yönde dolaşır.</span><span class="sxs-lookup"><span data-stu-id="e4935-144">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="e4935-145">Diğer bir deyişle, son yanıt iletisi ilk işleyicidir.</span><span class="sxs-lookup"><span data-stu-id="e4935-145">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="e4935-146">İç işleyicileri ayarlamanız gerekmez dikkat edin. Web API çerçevesi ileti işleyicileri otomatik olarak bağlanır.</span><span class="sxs-lookup"><span data-stu-id="e4935-146">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="e4935-147">Kullanıyorsanız [kendi kendine barındırma](../older-versions/self-host-a-web-api.md), bir örneğini oluşturmak **HttpSelfHostConfiguration** sınıfı ve işleyicileri ekleyin **MessageHandlers** koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="e4935-147">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="e4935-148">Şimdi özel ileti işleyicileri bazı örnekleri bakalım.</span><span class="sxs-lookup"><span data-stu-id="e4935-148">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="e4935-149">Örnek: X-HTTP-Method-Override</span><span class="sxs-lookup"><span data-stu-id="e4935-149">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="e4935-150">X-HTTP-Method-Override standart olmayan bir HTTP üstbilgisi ' dir.</span><span class="sxs-lookup"><span data-stu-id="e4935-150">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="e4935-151">PUT veya silme gibi belirli HTTP istek türlerini gönderemiyor istemciler için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e4935-151">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="e4935-152">Bunun yerine, istemci bir POST isteği gönderir ve istenen yöntemin X-HTTP-Method-Override üstbilgisini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e4935-152">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="e4935-153">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e4935-153">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="e4935-154">X-HTTP-Method-Override için destek ekler bir ileti işleyicisini şöyledir:</span><span class="sxs-lookup"><span data-stu-id="e4935-154">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="e4935-155">İçinde **SendAsync** yöntemi, işleyici denetler istek iletisi bir POST isteği olup olmadığını, ve X-HTTP-Method-Override üstbilgi içerip içermediğini.</span><span class="sxs-lookup"><span data-stu-id="e4935-155">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="e4935-156">Bu durumda, üstbilgi değeri doğrular ve istek yöntemini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="e4935-156">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="e4935-157">Son olarak, işleyici çağırması `base.SendAsync` sonraki işleyicisine ileti geçirmek için.</span><span class="sxs-lookup"><span data-stu-id="e4935-157">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="e4935-158">İstek ulaştığında **HttpControllerDispatcher** sınıfı, **HttpControllerDispatcher** güncelleştirilmiş isteği yöntemine dayalı istek yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="e4935-158">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="e4935-159">Örnek: bir özel yanıt üstbilgisi ekleme</span><span class="sxs-lookup"><span data-stu-id="e4935-159">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="e4935-160">Her yanıt iletisi için bir özel üst bilgi ekler bir ileti işleyicisini şöyledir:</span><span class="sxs-lookup"><span data-stu-id="e4935-160">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="e4935-161">İlk olarak, işleyici çağırması `base.SendAsync` iç ileti işleyicisi isteği geçirmek için.</span><span class="sxs-lookup"><span data-stu-id="e4935-161">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="e4935-162">Bir yanıt iletisi iç işleyici döndürür, ancak bu nedenle zaman uyumsuz olarak kullanarak yapar bir **görev&lt;T&gt;**  nesnesi.</span><span class="sxs-lookup"><span data-stu-id="e4935-162">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="e4935-163">Yanıt iletisi kadar kullanılamaz `base.SendAsync` zaman uyumsuz olarak tamamlar.</span><span class="sxs-lookup"><span data-stu-id="e4935-163">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="e4935-164">Bu örnekte **await** işlerini yapmak için anahtar sözcüğü zaman uyumsuz olarak sonra `SendAsync` tamamlar.</span><span class="sxs-lookup"><span data-stu-id="e4935-164">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="e4935-165">.NET Framework 4.0 hedefliyorsanız kullanmak **görev**&lt;T&gt;**. Varsa** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="e4935-165">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="e4935-166">Örnek: bir API anahtarı için denetimi</span><span class="sxs-lookup"><span data-stu-id="e4935-166">Example: Checking for an API Key</span></span>

<span data-ttu-id="e4935-167">Bazı web Hizmetleri, istekte bir API anahtarı eklemek istemciler gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e4935-167">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="e4935-168">Aşağıdaki örnek, bir ileti işleyicisi geçerli bir API anahtarı için istekleri nasıl denetleyebilirsiniz gösterir:</span><span class="sxs-lookup"><span data-stu-id="e4935-168">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="e4935-169">Bu işleyici URI sorgu dizesinde API anahtarı arar.</span><span class="sxs-lookup"><span data-stu-id="e4935-169">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="e4935-170">(Bu örnekte, anahtarın statik bir dize olduğunu varsayıyoruz.</span><span class="sxs-lookup"><span data-stu-id="e4935-170">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="e4935-171">Gerçek bir uygulama büyük olasılıkla daha karmaşık doğrulama kullanırsınız.) Sorgu dizesi anahtar içeriyorsa, işleyici isteği iç işleyici geçirir.</span><span class="sxs-lookup"><span data-stu-id="e4935-171">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="e4935-172">İsteğin geçerli bir anahtar yoksa işleyicinin bir yanıt iletisi durumu 403, ile oluşturur. Yasak.</span><span class="sxs-lookup"><span data-stu-id="e4935-172">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="e4935-173">Bu durumda, işleyici arama `base.SendAsync`, bu nedenle iç işleyici hiçbir zaman alan isteği de denetleyicisi çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="e4935-173">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="e4935-174">Bu nedenle, denetleyici gelen tüm istekleri geçerli bir API anahtarı olduğunu kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="e4935-174">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="e4935-175">API anahtarı yalnızca belirli denetleyici eylemleri için geçerliyse, ileti işleyicisi yerine bir eylem filtresi kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="e4935-175">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="e4935-176">URI yönlendirme gerçekleştirildikten sonra eylem filtrelerini çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e4935-176">Action filters run after URI routing is performed.</span></span>


## <a name="per-route-message-handlers"></a><span data-ttu-id="e4935-177">Rota başına ileti işleyicileri</span><span class="sxs-lookup"><span data-stu-id="e4935-177">Per-Route Message Handlers</span></span>

<span data-ttu-id="e4935-178">İşleyiciler **HttpConfiguration.MessageHandlers** koleksiyonu uygulamak genel.</span><span class="sxs-lookup"><span data-stu-id="e4935-178">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="e4935-179">Alternatif olarak, rota tanımlarken özel bir rota ileti işleyicisi ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e4935-179">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="e4935-180">İstek URI'si "Route2" eşleşiyorsa, bu örnekte, istek için gönderilir `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="e4935-180">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="e4935-181">Aşağıdaki diyagramda bu iki yolları için ardışık düzenini gösterir:</span><span class="sxs-lookup"><span data-stu-id="e4935-181">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="e4935-182">Dikkat `MessageHandler2` varsayılan değiştirir **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="e4935-182">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="e4935-183">Bu örnekte, `MessageHandler2` yanıtı oluşturur ve "Route2" hiçbir zaman bir denetleyiciye Git eşleşen ister.</span><span class="sxs-lookup"><span data-stu-id="e4935-183">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="e4935-184">Bu, kendi özel uç noktası ile tüm Web API denetleyicisi mekanizması değiştirmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="e4935-184">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="e4935-185">Alternatif olarak, bir rota başına ileti işleyicisi için temsilci olarak atayabilir miyim **HttpControllerDispatcher**, o bir denetleyiciye gönderir.</span><span class="sxs-lookup"><span data-stu-id="e4935-185">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="e4935-186">Aşağıdaki kod, bu yolu yapılandırmak gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="e4935-186">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
