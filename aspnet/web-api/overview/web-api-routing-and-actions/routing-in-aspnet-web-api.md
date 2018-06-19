---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: ASP.NET Web API'de yönlendirme | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26567084"
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="47d1e-102">ASP.NET Web API'de yönlendirme</span><span class="sxs-lookup"><span data-stu-id="47d1e-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="47d1e-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="47d1e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="47d1e-104">Bu makalede, nasıl ASP.NET Web API HTTP isteklerini denetleyicilerine yolları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="47d1e-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="47d1e-105">ASP.NET MVC ile bilginiz varsa, Web API yönlendirmeye MVC yönlendirme için çok benzer.</span><span class="sxs-lookup"><span data-stu-id="47d1e-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="47d1e-106">Web API eylemini seçmek için URI yolu HTTP yöntemini kullanır ana farktır.</span><span class="sxs-lookup"><span data-stu-id="47d1e-106">The main difference is that Web API uses the HTTP method, not the URI path, to select the action.</span></span> <span data-ttu-id="47d1e-107">MVC stili Web API'de yönlendirme de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47d1e-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="47d1e-108">Bu makalede, ASP.NET MVC bilgisi varsaymaz.</span><span class="sxs-lookup"><span data-stu-id="47d1e-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>


## <a name="routing-tables"></a><span data-ttu-id="47d1e-109">Yönlendirme tabloları</span><span class="sxs-lookup"><span data-stu-id="47d1e-109">Routing Tables</span></span>

<span data-ttu-id="47d1e-110">ASP.NET Web API içinde bir *denetleyicisi* HTTP isteklerini işleyen sınıftır.</span><span class="sxs-lookup"><span data-stu-id="47d1e-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="47d1e-111">Genel yöntemler denetleyicisinin adlı *eylem yöntemleri* ya da yalnızca *Eylemler*.</span><span class="sxs-lookup"><span data-stu-id="47d1e-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="47d1e-112">Web API çerçevesi bir istek aldığında, istek için bir eylem yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="47d1e-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="47d1e-113">Çağrılacak hangi eylemini belirlemek için framework kullanan bir *yönlendirme tablosu*.</span><span class="sxs-lookup"><span data-stu-id="47d1e-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="47d1e-114">Web API için Visual Studio Proje şablonu, bir varsayılan yol oluşturur:</span><span class="sxs-lookup"><span data-stu-id="47d1e-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="47d1e-115">Bu rota uygulamada yerleştirilir WebApiConfig.cs dosyasında tanımlanan\_başlangıç dizini:</span><span class="sxs-lookup"><span data-stu-id="47d1e-115">This route is defined in the WebApiConfig.cs file, which is placed in the App\_Start directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="47d1e-116">Hakkında daha fazla bilgi için **WebApiConfig** sınıfı için bkz: [yapılandırma ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="47d1e-116">For more information about the **WebApiConfig** class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="47d1e-117">Web API Self barındırıyorsanız, doğrudan yönlendirme tablosu ayarlamalısınız **HttpSelfHostConfiguration** nesnesi.</span><span class="sxs-lookup"><span data-stu-id="47d1e-117">If you self-host Web API, you must set the routing table directly on the **HttpSelfHostConfiguration** object.</span></span> <span data-ttu-id="47d1e-118">Daha fazla bilgi için bkz: [bir Web API Self konak](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="47d1e-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="47d1e-119">Yönlendirme tablosundaki her giriş içeren bir *rota şablonu*.</span><span class="sxs-lookup"><span data-stu-id="47d1e-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="47d1e-120">Web API için varsayılan rota şablonudur &quot;API / {controller} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="47d1e-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="47d1e-121">Bu şablonda &quot;API&quot; bir değişmez değer yol kesimi ve {controller} ve {id} yer tutucusu değişkenlerdir.</span><span class="sxs-lookup"><span data-stu-id="47d1e-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="47d1e-122">Web API çerçevesi bir HTTP isteği aldığında, yönlendirme tablosundaki rota şablonlarından birini karşı URI eşleştirmeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="47d1e-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="47d1e-123">Hiçbir yol eşleşirse, istemci bir 404 hatası alır.</span><span class="sxs-lookup"><span data-stu-id="47d1e-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="47d1e-124">Örneğin, aşağıdaki URI, varsayılan yol eşleştir:</span><span class="sxs-lookup"><span data-stu-id="47d1e-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="47d1e-125">/ api/contacts</span><span class="sxs-lookup"><span data-stu-id="47d1e-125">/api/contacts</span></span>
- <span data-ttu-id="47d1e-126">/api/Contacts/1</span><span class="sxs-lookup"><span data-stu-id="47d1e-126">/api/contacts/1</span></span>
- <span data-ttu-id="47d1e-127">/api/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="47d1e-127">/api/products/gizmo1</span></span>

<span data-ttu-id="47d1e-128">Eksik olduğundan ancak, aşağıdaki URI, eşleşmiyor &quot;API&quot; segment:</span><span class="sxs-lookup"><span data-stu-id="47d1e-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="47d1e-129">/ Kişiler/1</span><span class="sxs-lookup"><span data-stu-id="47d1e-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="47d1e-130">ASP.NET MVC yönlendirmesinin çakışmaları önlemek için "API" rotada kullanmanın nedeni gerekir.</span><span class="sxs-lookup"><span data-stu-id="47d1e-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="47d1e-131">Böylece, elinizde &quot;/kişiler&quot; bir MVC denetleyicisi gidin ve &quot;/api/contacts&quot; Web API denetleyicisi gidin.</span><span class="sxs-lookup"><span data-stu-id="47d1e-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="47d1e-132">Elbette, bu kural hoşlanmıyorsanız, varsayılan rota tablosu değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47d1e-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="47d1e-133">Eşleşen bir rota bulunduktan sonra Web API denetleyici ve eylem seçer:</span><span class="sxs-lookup"><span data-stu-id="47d1e-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="47d1e-134">Web API denetleyicisi bulma ekler &quot;denetleyicisi&quot; değerine *{controller}* değişkeni.</span><span class="sxs-lookup"><span data-stu-id="47d1e-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="47d1e-135">Eylem bulmak için Web API at HTTP yöntemi arar ve adı, HTTP yöntem adı ile başlayan bir eylem arar.</span><span class="sxs-lookup"><span data-stu-id="47d1e-135">To find the action, Web API looks at the HTTP method, and then looks for an action whose name begins with that HTTP method name.</span></span> <span data-ttu-id="47d1e-136">Örneğin, bir GET isteği ile Web API ile başlayan bir eylem arar &quot;Al... &quot;, gibi &quot;GetContact&quot; veya &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="47d1e-136">For example, with a GET request, Web API looks for an action that starts with &quot;Get...&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="47d1e-137">Bu kural, yalnızca GET, POST, koy ve Sil yöntemlerini uygular.</span><span class="sxs-lookup"><span data-stu-id="47d1e-137">This convention applies only to GET, POST, PUT, and DELETE methods.</span></span> <span data-ttu-id="47d1e-138">Denetleyicinizde öznitelikleri kullanarak diğer HTTP yöntemleri etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47d1e-138">You can enable other HTTP methods by using attributes on your controller.</span></span> <span data-ttu-id="47d1e-139">Bu örnek daha sonra göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="47d1e-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="47d1e-140">Rota şablonunu diğer yer tutucu değişkenler gibi *{id}* eylem parametreler eşlenmedi.</span><span class="sxs-lookup"><span data-stu-id="47d1e-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="47d1e-141">Bir örneğe bakalım.</span><span class="sxs-lookup"><span data-stu-id="47d1e-141">Let's look at an example.</span></span> <span data-ttu-id="47d1e-142">Aşağıdaki denetleyicisiyle tanımladığınız varsayın:</span><span class="sxs-lookup"><span data-stu-id="47d1e-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="47d1e-143">Her biri için çağrılır eylem birlikte bazı olası HTTP isteklerini şunlardır:</span><span class="sxs-lookup"><span data-stu-id="47d1e-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="47d1e-144">HTTP yöntemi</span><span class="sxs-lookup"><span data-stu-id="47d1e-144">HTTP Method</span></span> | <span data-ttu-id="47d1e-145">URI yolu</span><span class="sxs-lookup"><span data-stu-id="47d1e-145">URI Path</span></span> | <span data-ttu-id="47d1e-146">Eylem</span><span class="sxs-lookup"><span data-stu-id="47d1e-146">Action</span></span> | <span data-ttu-id="47d1e-147">Parametre</span><span class="sxs-lookup"><span data-stu-id="47d1e-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="47d1e-148">AL</span><span class="sxs-lookup"><span data-stu-id="47d1e-148">GET</span></span> | <span data-ttu-id="47d1e-149">API/ürünleri</span><span class="sxs-lookup"><span data-stu-id="47d1e-149">api/products</span></span> | <span data-ttu-id="47d1e-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="47d1e-150">GetAllProducts</span></span> | <span data-ttu-id="47d1e-151">*(hiçbiri)*</span><span class="sxs-lookup"><span data-stu-id="47d1e-151">*(none)*</span></span> |
| <span data-ttu-id="47d1e-152">AL</span><span class="sxs-lookup"><span data-stu-id="47d1e-152">GET</span></span> | <span data-ttu-id="47d1e-153">Ürünler/api/4</span><span class="sxs-lookup"><span data-stu-id="47d1e-153">api/products/4</span></span> | <span data-ttu-id="47d1e-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="47d1e-154">GetProductById</span></span> | <span data-ttu-id="47d1e-155">4</span><span class="sxs-lookup"><span data-stu-id="47d1e-155">4</span></span> |
| <span data-ttu-id="47d1e-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="47d1e-156">DELETE</span></span> | <span data-ttu-id="47d1e-157">Ürünler/api/4</span><span class="sxs-lookup"><span data-stu-id="47d1e-157">api/products/4</span></span> | <span data-ttu-id="47d1e-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="47d1e-158">DeleteProduct</span></span> | <span data-ttu-id="47d1e-159">4</span><span class="sxs-lookup"><span data-stu-id="47d1e-159">4</span></span> |
| <span data-ttu-id="47d1e-160">YAYINLA</span><span class="sxs-lookup"><span data-stu-id="47d1e-160">POST</span></span> | <span data-ttu-id="47d1e-161">API/ürünleri</span><span class="sxs-lookup"><span data-stu-id="47d1e-161">api/products</span></span> | <span data-ttu-id="47d1e-162">*(eşleşme)*</span><span class="sxs-lookup"><span data-stu-id="47d1e-162">*(no match)*</span></span> |  |

<span data-ttu-id="47d1e-163">Dikkat *{id}* URI segmenti varsa, eşleştirilir *kimliği* eylem parametresi.</span><span class="sxs-lookup"><span data-stu-id="47d1e-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="47d1e-164">Bu örnekte, denetleyici, biriyle iki GET yöntemleri tanımlar. bir *kimliği* parametre ve hiçbir parametre.</span><span class="sxs-lookup"><span data-stu-id="47d1e-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="47d1e-165">Ayrıca, denetleyici tanımlamaz çünkü POST isteği başarısız olur unutmayın bir &quot;Post... &quot; yöntemi.</span><span class="sxs-lookup"><span data-stu-id="47d1e-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="47d1e-166">Yönlendirme farklılıkları</span><span class="sxs-lookup"><span data-stu-id="47d1e-166">Routing Variations</span></span>

<span data-ttu-id="47d1e-167">ASP.NET Web API için temel yönlendirme mekanizması önceki bölümde açıklanan.</span><span class="sxs-lookup"><span data-stu-id="47d1e-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="47d1e-168">Bu bölümde bazı farklılıklar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="47d1e-168">This section describes some variations.</span></span>

### <a name="http-methods"></a><span data-ttu-id="47d1e-169">HTTP yöntemleri</span><span class="sxs-lookup"><span data-stu-id="47d1e-169">HTTP Methods</span></span>

<span data-ttu-id="47d1e-170">Adlandırma kuralı için HTTP yöntemleri kullanmak yerine, açıkça HTTP yöntem bir eylem için eylem yöntemiyle tasarlayarak belirtebilirsiniz **HttpGet**, **HttpPut**, **HttpPost** , veya **HttpDelete** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="47d1e-170">Instead of using the naming convention for HTTP methods, you can explicitly specify the HTTP method for an action by decorating the action method with the **HttpGet**, **HttpPut**, **HttpPost**, or **HttpDelete** attribute.</span></span>

<span data-ttu-id="47d1e-171">Aşağıdaki örnekte, FindProduct yöntemi GET isteklerinin eşlenir:</span><span class="sxs-lookup"><span data-stu-id="47d1e-171">In the following example, the FindProduct method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="47d1e-172">Bir eylem için birden çok HTTP yöntemleri izin vermek veya HTTP yöntemleri GET, PUT, POST ve DELETE dışında izin vermek için kullanın **AcceptVerbs** özniteliği HTTP yöntemlerinin listesini alır.</span><span class="sxs-lookup"><span data-stu-id="47d1e-172">To allow multiple HTTP methods for an action, or to allow HTTP methods other than GET, PUT, POST, and DELETE, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="47d1e-173">Eylem adına göre yönlendirme</span><span class="sxs-lookup"><span data-stu-id="47d1e-173">Routing by Action Name</span></span>

<span data-ttu-id="47d1e-174">Varsayılan yönlendirme şablonu ile Web API eylem seçmek için HTTP yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="47d1e-174">With the default routing template, Web API uses the HTTP method to select the action.</span></span> <span data-ttu-id="47d1e-175">Bununla birlikte, eylem adı URI'de burada bulunan bir yol oluşturabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="47d1e-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="47d1e-176">Bu yol şablonunda *{action}* parametre adları denetleyici eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="47d1e-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="47d1e-177">Yönlendirme bu stili ile öznitelikleri izin verilen HTTP yöntemleri belirtmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="47d1e-177">With this style of routing, use attributes to specify the allowed HTTP methods.</span></span> <span data-ttu-id="47d1e-178">Örneğin, aşağıdaki yöntemi denetleyicinizi olduğunu varsayın:</span><span class="sxs-lookup"><span data-stu-id="47d1e-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="47d1e-179">Bu durumda, "API/ürünler/Ayrıntılar/1" için bir GET isteği map ayrıntıları yöntemi.</span><span class="sxs-lookup"><span data-stu-id="47d1e-179">In this case, a GET request for "api/products/details/1" would map to the Details method.</span></span> <span data-ttu-id="47d1e-180">Yönlendirme bu stili için ASP.NET MVC benzer ve bir RPC-style API için uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="47d1e-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="47d1e-181">Eylem adı kullanılarak kılabilirsiniz **EylemAdı** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="47d1e-181">You can override the action name by using the **ActionName** attribute.</span></span> <span data-ttu-id="47d1e-182">Aşağıdaki örnekte, eşle iki eylem vardır &quot;ürünler/api/küçük/*kimliği*. Bir GET ve POST diğer destekler:</span><span class="sxs-lookup"><span data-stu-id="47d1e-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="47d1e-183">Eylemler olmayan</span><span class="sxs-lookup"><span data-stu-id="47d1e-183">Non-Actions</span></span>

<span data-ttu-id="47d1e-184">Bir yöntem bir eylem olarak çağrılan engellemek için kullanma **NonAction** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="47d1e-184">To prevent a method from getting invoked as an action, use the **NonAction** attribute.</span></span> <span data-ttu-id="47d1e-185">Aksi takdirde yönlendirme kurallarını eşleşir olsa bile bu framework yöntem bir eylem değil işaret eder.</span><span class="sxs-lookup"><span data-stu-id="47d1e-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="47d1e-186">Daha Fazla Bilgi</span><span class="sxs-lookup"><span data-stu-id="47d1e-186">Further Reading</span></span>

<span data-ttu-id="47d1e-187">Bu konuda yönlendirme üst düzey bir görünümü sağlanır.</span><span class="sxs-lookup"><span data-stu-id="47d1e-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="47d1e-188">Daha fazla ayrıntı için [Yönlendirme ve eylem seçimi](routing-and-action-selection.md), tam olarak nasıl framework eşleşen bir rota için bir URI, bir denetleyiciyi seçer ve sonra çağrılacak eylemi seçer açıklar.</span><span class="sxs-lookup"><span data-stu-id="47d1e-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
