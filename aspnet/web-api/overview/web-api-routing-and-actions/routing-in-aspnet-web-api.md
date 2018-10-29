---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: ASP.NET Web API'de yönlendirme | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a7bc998fc23c0453fc9cd6ac1e7b9af7bd516225
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207309"
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="e0123-102">ASP.NET Web API'de yönlendirme</span><span class="sxs-lookup"><span data-stu-id="e0123-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="e0123-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e0123-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e0123-104">Bu makalede, ASP.NET Web API HTTP isteklerini denetleyicilerine nasıl yönlendirdiğini açıklanır.</span><span class="sxs-lookup"><span data-stu-id="e0123-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="e0123-105">ASP.NET MVC ile bilginiz varsa, Web API yönlendirmeye MVC yönlendirme için çok benzer.</span><span class="sxs-lookup"><span data-stu-id="e0123-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="e0123-106">Web API eylemi seçmek için HTTP fiili, URI yolu kullanır ana farktır.</span><span class="sxs-lookup"><span data-stu-id="e0123-106">The main difference is that Web API uses the HTTP verb, not the URI path, to select the action.</span></span> <span data-ttu-id="e0123-107">MVC stili Web API'de yönlendirme de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0123-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="e0123-108">Bu makalede, ASP.NET MVC, herhangi bir Bilgi Bankası varsaymaz.</span><span class="sxs-lookup"><span data-stu-id="e0123-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>

## <a name="routing-tables"></a><span data-ttu-id="e0123-109">Yönlendirme tabloları</span><span class="sxs-lookup"><span data-stu-id="e0123-109">Routing Tables</span></span>

<span data-ttu-id="e0123-110">ASP.NET Web API, bir *denetleyicisi* HTTP isteklerini işleyen sınıftır.</span><span class="sxs-lookup"><span data-stu-id="e0123-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="e0123-111">Denetleyicinin genel yöntem de çağrıldığında *eylem yöntemleri* ya da yalnızca *eylemleri*.</span><span class="sxs-lookup"><span data-stu-id="e0123-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="e0123-112">Web API çerçevesi, bir istek aldığında, bir eyleme isteği yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="e0123-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="e0123-113">Çağrılacak eylemi belirlemek için framework kullanan bir *yönlendirme tablosunu*.</span><span class="sxs-lookup"><span data-stu-id="e0123-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="e0123-114">Web API'si için Visual Studio Proje şablonu, varsayılan bir yol oluşturur:</span><span class="sxs-lookup"><span data-stu-id="e0123-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="e0123-115">Bu rota tanımlanan *WebApiConfig.cs* yerleştirilen dosya *uygulama\_Başlat* dizini:</span><span class="sxs-lookup"><span data-stu-id="e0123-115">This route is defined in the *WebApiConfig.cs* file, which is placed in the *App\_Start* directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="e0123-116">Hakkında daha fazla bilgi için `WebApiConfig` sınıfı [ASP.NET Web API'sini yapılandırma](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e0123-116">For more information about the `WebApiConfig` class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="e0123-117">Web API Self barındırıyorsanız, doğrudan yönlendirme tablosu ayarlamalısınız `HttpSelfHostConfiguration` nesne.</span><span class="sxs-lookup"><span data-stu-id="e0123-117">If you self-host Web API, you must set the routing table directly on the `HttpSelfHostConfiguration` object.</span></span> <span data-ttu-id="e0123-118">Daha fazla bilgi için [barındırılan Web API'si](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e0123-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="e0123-119">Yönlendirme tablosundaki her bir giriş içeren bir *rota şablonu*.</span><span class="sxs-lookup"><span data-stu-id="e0123-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="e0123-120">Web API'si için varsayılan rota şablonudur &quot;API / {denetleyici} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="e0123-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="e0123-121">Bu şablona &quot;API&quot; değişmez bir yol kesimi ve {denetleyici} ve {id} yer tutucusu değişkenlerdir.</span><span class="sxs-lookup"><span data-stu-id="e0123-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="e0123-122">Web API çerçevesi, bir HTTP isteği aldığında, URI yönlendirme tablosundaki rota şablonlardan birini karşı eşleştirmeyi dener.</span><span class="sxs-lookup"><span data-stu-id="e0123-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="e0123-123">Hiçbir yol eşleşirse, istemci bir 404 hatası alır.</span><span class="sxs-lookup"><span data-stu-id="e0123-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="e0123-124">Örneğin, aşağıdaki URI, varsayılan yolu eşleşmiyor:</span><span class="sxs-lookup"><span data-stu-id="e0123-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="e0123-125">/ api/kişiler</span><span class="sxs-lookup"><span data-stu-id="e0123-125">/api/contacts</span></span>
- <span data-ttu-id="e0123-126">/api/Contacts/1</span><span class="sxs-lookup"><span data-stu-id="e0123-126">/api/contacts/1</span></span>
- <span data-ttu-id="e0123-127">/api/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="e0123-127">/api/products/gizmo1</span></span>

<span data-ttu-id="e0123-128">Eksik olduğundan ancak, aşağıdaki URI, eşleşmiyor &quot;API&quot; segment:</span><span class="sxs-lookup"><span data-stu-id="e0123-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="e0123-129">/ kişileri/1</span><span class="sxs-lookup"><span data-stu-id="e0123-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="e0123-130">"API" yolda kullanma nedeni, ASP.NET MVC yönlendirme çarpışmalardan kaçınmak için olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="e0123-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="e0123-131">Bu şekilde, elinizde &quot;/kişiler&quot; bir MVC denetleyicisi gidin ve &quot;/api/kişileri&quot; bir Web API denetleyicisi gidin.</span><span class="sxs-lookup"><span data-stu-id="e0123-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="e0123-132">Elbette, bu yöntemi kullanmak istemiyorsanız, varsayılan rota tablosu değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0123-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="e0123-133">Eşleşen bir rota bulunduktan sonra Web API denetleyici ve eylem seçer:</span><span class="sxs-lookup"><span data-stu-id="e0123-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="e0123-134">Web API denetleyicisi ekler &quot;denetleyicisi&quot; değerine *{denetleyici}* değişkeni.</span><span class="sxs-lookup"><span data-stu-id="e0123-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="e0123-135">Eylem bulmak için Web API'si HTTP fiili arar ve bir eylem adı, HTTP fiili adı ile başlayan arar.</span><span class="sxs-lookup"><span data-stu-id="e0123-135">To find the action, Web API looks at the HTTP verb, and then looks for an action whose name begins with that HTTP verb name.</span></span> <span data-ttu-id="e0123-136">Örneğin, bir eylem ön eki için bir GET isteği ile Web API görünür &quot;alma&quot;, gibi &quot;GetContact&quot; veya &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="e0123-136">For example, with a GET request, Web API looks for an action prefixed with &quot;Get&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="e0123-137">Bu kuralı, yalnızca GET, sonrası, PUT, DELETE, HEAD, seçenekleri ve fiilleri düzeltme eki uygular.</span><span class="sxs-lookup"><span data-stu-id="e0123-137">This convention applies only to GET, POST, PUT, DELETE, HEAD, OPTIONS, and PATCH verbs.</span></span> <span data-ttu-id="e0123-138">Diğer HTTP fiilleri denetleyicinizde öznitelikleri kullanarak etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0123-138">You can enable other HTTP verbs by using attributes on your controller.</span></span> <span data-ttu-id="e0123-139">Bu örnek daha sonra göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="e0123-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="e0123-140">Rota şablonu diğer yer tutucu değişkenleri gibi *{id}* eylem parametrelerini eşlenir.</span><span class="sxs-lookup"><span data-stu-id="e0123-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="e0123-141">Bir örneğe göz atalım.</span><span class="sxs-lookup"><span data-stu-id="e0123-141">Let's look at an example.</span></span> <span data-ttu-id="e0123-142">Aşağıdaki denetleyicisi tanımladığınız varsayalım:</span><span class="sxs-lookup"><span data-stu-id="e0123-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="e0123-143">Her biri için çağrılır eylemi yanı sıra bazı olası HTTP isteklerini şunlardır:</span><span class="sxs-lookup"><span data-stu-id="e0123-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="e0123-144">HTTP fiili</span><span class="sxs-lookup"><span data-stu-id="e0123-144">HTTP Verb</span></span> | <span data-ttu-id="e0123-145">URI yolu</span><span class="sxs-lookup"><span data-stu-id="e0123-145">URI Path</span></span> | <span data-ttu-id="e0123-146">Eylem</span><span class="sxs-lookup"><span data-stu-id="e0123-146">Action</span></span> | <span data-ttu-id="e0123-147">Parametre</span><span class="sxs-lookup"><span data-stu-id="e0123-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e0123-148">AL</span><span class="sxs-lookup"><span data-stu-id="e0123-148">GET</span></span> | <span data-ttu-id="e0123-149">API/ürünleri</span><span class="sxs-lookup"><span data-stu-id="e0123-149">api/products</span></span> | <span data-ttu-id="e0123-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="e0123-150">GetAllProducts</span></span> | <span data-ttu-id="e0123-151">*(hiçbiri)*</span><span class="sxs-lookup"><span data-stu-id="e0123-151">*(none)*</span></span> |
| <span data-ttu-id="e0123-152">AL</span><span class="sxs-lookup"><span data-stu-id="e0123-152">GET</span></span> | <span data-ttu-id="e0123-153">API/ürünler/4</span><span class="sxs-lookup"><span data-stu-id="e0123-153">api/products/4</span></span> | <span data-ttu-id="e0123-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="e0123-154">GetProductById</span></span> | <span data-ttu-id="e0123-155">4</span><span class="sxs-lookup"><span data-stu-id="e0123-155">4</span></span> |
| <span data-ttu-id="e0123-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="e0123-156">DELETE</span></span> | <span data-ttu-id="e0123-157">API/ürünler/4</span><span class="sxs-lookup"><span data-stu-id="e0123-157">api/products/4</span></span> | <span data-ttu-id="e0123-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="e0123-158">DeleteProduct</span></span> | <span data-ttu-id="e0123-159">4</span><span class="sxs-lookup"><span data-stu-id="e0123-159">4</span></span> |
| <span data-ttu-id="e0123-160">YAYINLA</span><span class="sxs-lookup"><span data-stu-id="e0123-160">POST</span></span> | <span data-ttu-id="e0123-161">API/ürünleri</span><span class="sxs-lookup"><span data-stu-id="e0123-161">api/products</span></span> | <span data-ttu-id="e0123-162">*(eşleşme yok)*</span><span class="sxs-lookup"><span data-stu-id="e0123-162">*(no match)*</span></span> |  |

<span data-ttu-id="e0123-163">Dikkat *{id}* URI segmenti varsa, eşlenmiş durumda *kimliği* eylem parametresi.</span><span class="sxs-lookup"><span data-stu-id="e0123-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="e0123-164">Bu örnekte, denetleyici, bir iki GET yöntemleri tanımlar. bir *kimliği* parametre ve parametre olmadan.</span><span class="sxs-lookup"><span data-stu-id="e0123-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="e0123-165">Ayrıca, denetleyici tanımlamaz çünkü POST isteği reddeder unutmayın bir &quot;Post... &quot; yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e0123-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="e0123-166">Yönlendirme farklılıkları</span><span class="sxs-lookup"><span data-stu-id="e0123-166">Routing Variations</span></span>

<span data-ttu-id="e0123-167">Önceki bölümde, ASP.NET Web API'si için temel yönlendirme mekanizması açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="e0123-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="e0123-168">Bu bölümde, bazı farklılıklar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="e0123-168">This section describes some variations.</span></span>

### <a name="http-verbs"></a><span data-ttu-id="e0123-169">HTTP fiilleri</span><span class="sxs-lookup"><span data-stu-id="e0123-169">HTTP verbs</span></span>

<span data-ttu-id="e0123-170">HTTP fiilleri için adlandırma kuralı kullanmak yerine, açıkça HTTP edimi bir eylem için eylem yöntemine aşağıdaki özniteliklerden birini tasarlayarak belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e0123-170">Instead of using the naming convention for HTTP verbs, you can explicitly specify the HTTP verb for an action by decorating the action method with one of the following attributes:</span></span>

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

<span data-ttu-id="e0123-171">Aşağıdaki örnekte, `FindProduct` yöntemi GET istekleri için eşleştirilen:</span><span class="sxs-lookup"><span data-stu-id="e0123-171">In the following example, the `FindProduct` method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="e0123-172">Bir eylem için birden çok HTTP fiillere izin verme veya GET, PUT, POST, DELETE, HEAD, SEÇENEKLERİNİ ve düzeltme eki dışındaki HTTP fiillerine izin vermek için `[AcceptVerbs]` özniteliği HTTP fiilleri listesini alır.</span><span class="sxs-lookup"><span data-stu-id="e0123-172">To allow multiple HTTP verbs for an action, or to allow HTTP verbs other than GET, PUT, POST, DELETE, HEAD, OPTIONS, and PATCH, use the `[AcceptVerbs]` attribute, which takes a list of HTTP verbs.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="e0123-173">Eylem adına göre yönlendirme</span><span class="sxs-lookup"><span data-stu-id="e0123-173">Routing by Action Name</span></span>

<span data-ttu-id="e0123-174">Varsayılan yönlendirme şablonu ile bir eylem seçmek için HTTP fiili Web API'sini kullanır.</span><span class="sxs-lookup"><span data-stu-id="e0123-174">With the default routing template, Web API uses the HTTP verb to select the action.</span></span> <span data-ttu-id="e0123-175">Bununla birlikte, eylem adı URI'de burada dahil bir yol oluşturabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e0123-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="e0123-176">Bu yol şablonunda *{action}* parametre adları denetleyici eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e0123-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="e0123-177">Yönlendirme bu stil ile öznitelikleri izin verilen HTTP fiilleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="e0123-177">With this style of routing, use attributes to specify the allowed HTTP verbs.</span></span> <span data-ttu-id="e0123-178">Örneğin, aşağıdaki yöntemi denetleyicinizin olduğunu varsayın:</span><span class="sxs-lookup"><span data-stu-id="e0123-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="e0123-179">Bu durumda, "API/ürünler/Ayrıntılar/1" için bir GET isteği eşlemek `Details` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e0123-179">In this case, a GET request for "api/products/details/1" would map to the `Details` method.</span></span> <span data-ttu-id="e0123-180">Bu stil, yönlendirme, ASP.NET MVC için benzer ve bir RPC-style API'si için uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="e0123-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="e0123-181">Eylem adı kullanılarak kılabilirsiniz `[ActionName]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e0123-181">You can override the action name by using the `[ActionName]` attribute.</span></span> <span data-ttu-id="e0123-182">Aşağıdaki örnekte, eşlenen iki bir eylem olmadığından &quot;ürünler/API/küçük/*kimliği*. Bir GET ve POST diğer destekler:</span><span class="sxs-lookup"><span data-stu-id="e0123-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="e0123-183">Eylemleri</span><span class="sxs-lookup"><span data-stu-id="e0123-183">Non-Actions</span></span>

<span data-ttu-id="e0123-184">Bir yöntem bir eylem olarak çağrılan önlemek için `[NonAction]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e0123-184">To prevent a method from getting invoked as an action, use the `[NonAction]` attribute.</span></span> <span data-ttu-id="e0123-185">Aksi takdirde yönlendirme kurallarını BC olsa bile bu framework yöntemin bir eylem değil bildirir.</span><span class="sxs-lookup"><span data-stu-id="e0123-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="e0123-186">Daha Fazla Bilgi</span><span class="sxs-lookup"><span data-stu-id="e0123-186">Further Reading</span></span>

<span data-ttu-id="e0123-187">Bu konuda, yönlendirme üst düzey bir görünüm sağlanır.</span><span class="sxs-lookup"><span data-stu-id="e0123-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="e0123-188">Daha fazla ayrıntı için [Yönlendirme ve eylem seçimi](routing-and-action-selection.md), tam olarak nasıl framework bir URI bir rota için eşleşen bir denetleyiciyi seçer ve ardından çağırmak için eylemi seçer açıklar.</span><span class="sxs-lookup"><span data-stu-id="e0123-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
