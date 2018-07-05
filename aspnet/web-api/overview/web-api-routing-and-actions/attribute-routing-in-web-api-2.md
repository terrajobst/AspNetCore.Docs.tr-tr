---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Öznitelik ASP.NET Web API 2 yönlendirme | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: e15f89ba98acef68279e51b278e3c7045569607a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373236"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="73414-102">ASP.NET Web API 2'de öznitelik yönlendirme</span><span class="sxs-lookup"><span data-stu-id="73414-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="73414-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="73414-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="73414-104">*Yönlendirme* Web API'sini bir eylem için bir URI nasıl eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="73414-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="73414-105">Web API 2 destekleyen yeni bir tür adı yönlendirme *öznitelik yönlendirme*.</span><span class="sxs-lookup"><span data-stu-id="73414-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="73414-106">Adından da anlaşılacağı gibi öznitelik yönlendirme yollarını tanımlamak için öznitelikleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="73414-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="73414-107">Öznitelik yönlendirme, web API'nize bir URI'leri üzerinde daha fazla denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="73414-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="73414-108">Örneğin, kaynak hiyerarşileri açıklayan bir URI'leri kolayca oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="73414-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="73414-109">Adlı kural tabanlı yönlendirme, önceki stil yönlendirme, yine de tam olarak desteklenir.</span><span class="sxs-lookup"><span data-stu-id="73414-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="73414-110">Aslında, aynı projede iki tekniği birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="73414-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="73414-111">Bu konu, öznitelik yönlendirme etkinleştirme gösterir ve öznitelik yönlendirme için çeşitli seçenekler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="73414-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="73414-112">Öznitelik yönlendirme kullanan bir uçtan uca öğretici için bkz [Web API 2'de öznitelik yönlendirme ile REST API oluşturma](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="73414-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="73414-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="73414-113">Prerequisites</span></span>

<span data-ttu-id="73414-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional veya Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="73414-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition</span></span>

<span data-ttu-id="73414-115">Alternatif olarak, gerekli paketleri yüklemek için NuGet paket yöneticisini kullanın.</span><span class="sxs-lookup"><span data-stu-id="73414-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="73414-116">Gelen **Araçları** Visual Studio'da seçim menüsünde **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="73414-116">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="73414-117">Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="73414-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="73414-118">Neden yönlendirme özniteliği?</span><span class="sxs-lookup"><span data-stu-id="73414-118">Why Attribute Routing?</span></span>

<span data-ttu-id="73414-119">Web API kullanılan ilk sürümü *kural tabanlı* yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="73414-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="73414-120">Yönlendirme, bu tür bir tane tanımlayacaksınız veya temel olarak daha fazla rota şablonlarının dizeleri parametreli.</span><span class="sxs-lookup"><span data-stu-id="73414-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="73414-121">Framework, bir istek aldığında, rota şablonu karşı URI ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="73414-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="73414-122">(Kural tabanlı yönlendirme hakkında daha fazla bilgi için bkz. [ASP.NET Web API'de yönlendirme](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="73414-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="73414-123">Kural tabanlı yönlendirme bir avantajı, tek bir yerde tanımlanmış şablonları ve yönlendirme kurallarını tüm denetleyicileri arasında tutarlı bir şekilde uygulandığından olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="73414-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="73414-124">Ne yazık ki, kural tabanlı yönlendirme RESTful API'leri ortak olan belirli bir URI düzenleri desteği zorlaştırır.</span><span class="sxs-lookup"><span data-stu-id="73414-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="73414-125">Genellikle alt kaynakları gibi kaynakları içeren: imajlarını siparişler, filmler aktörler sahip, books Yazar sahip ve VS.</span><span class="sxs-lookup"><span data-stu-id="73414-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="73414-126">Bu ilişkileri yansıtan bir URI'leri oluşturmak için doğal bir:</span><span class="sxs-lookup"><span data-stu-id="73414-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="73414-127">Bu tür bir URI, kural tabanlı yönlendirme kullanarak oluşturmak zordur.</span><span class="sxs-lookup"><span data-stu-id="73414-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="73414-128">Bu yapılabilir olsa da, sonuçları da birçok denetleyicileri veya kaynak türleri varsa ölçeklendirilemediği.</span><span class="sxs-lookup"><span data-stu-id="73414-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="73414-129">Öznitelik yönlendirme ile basit bir iş bir rota için bu URI tanımlar.</span><span class="sxs-lookup"><span data-stu-id="73414-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="73414-130">Denetleyici eylemi için yalnızca bir özniteliği ekleyin:</span><span class="sxs-lookup"><span data-stu-id="73414-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="73414-131">Öznitelik yönlendirme yapar kolay bazı diğer desenleri aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="73414-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="73414-132">**API sürümü oluşturma**</span><span class="sxs-lookup"><span data-stu-id="73414-132">**API versioning**</span></span>

<span data-ttu-id="73414-133">Bu örnekte, "/ v1/API/ürünleri" olacaktır farklı bir denetleyicisine yönlendirilen "/ v2/API/ürünleri".</span><span class="sxs-lookup"><span data-stu-id="73414-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`  
`/api/v2/products`

<span data-ttu-id="73414-134">**Aşırı yüklenmiş URI segmentleri**</span><span class="sxs-lookup"><span data-stu-id="73414-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="73414-135">Bu örnekte, "1" sipariş numarası olduğu halde "bekliyor" bir koleksiyona eşler.</span><span class="sxs-lookup"><span data-stu-id="73414-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`  
`/orders/pending`

<span data-ttu-id="73414-136">**Birden çok parametre türleri**</span><span class="sxs-lookup"><span data-stu-id="73414-136">**Multiple parameter types**</span></span>

<span data-ttu-id="73414-137">Bu örnekte, "1" sipariş numarası olduğu halde "06/2013/16" bir tarihi belirtir.</span><span class="sxs-lookup"><span data-stu-id="73414-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="73414-138">Öznitelik yönlendirmeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="73414-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="73414-139">Öznitelik yönlendirmeyi etkinleştirmek için çağrı **MapHttpAttributeRoutes** yapılandırma sırasında.</span><span class="sxs-lookup"><span data-stu-id="73414-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="73414-140">Bu genişletme yöntemi tanımlanan **System.Web.Http.HttpConfigurationExtensions** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="73414-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="73414-141">İle öznitelik yönlendirme birleştirilebilir [kural tabanlı](routing-in-aspnet-web-api.md) yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="73414-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="73414-142">Kural tabanlı rotaları tanımlamak için çağrı **MapHttpRoute** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="73414-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="73414-143">Web API'sini yapılandırma hakkında daha fazla bilgi için bkz. [ASP.NET Web API 2 yapılandırma](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="73414-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="73414-144">Not: Web API 1 ' geçiş</span><span class="sxs-lookup"><span data-stu-id="73414-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="73414-145">Web API 2 önce Web API proje şablonları aşağıdakine benzer bir kod oluşturuldu:</span><span class="sxs-lookup"><span data-stu-id="73414-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="73414-146">Bu kod, öznitelik yönlendirme etkinse, bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="73414-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="73414-147">Öznitelik yönlendirmeyi kullanmak için mevcut bir Web API projesini yükseltirseniz, bu yapılandırma kodu aşağıdakine güncelleştirmeyi unutmayın:</span><span class="sxs-lookup"><span data-stu-id="73414-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="73414-148">Daha fazla bilgi için [barındırma ASP.NET ile Web API'sini yapılandırma](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="73414-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="73414-149">Rota özniteliklerini ekleme</span><span class="sxs-lookup"><span data-stu-id="73414-149">Adding Route Attributes</span></span>

<span data-ttu-id="73414-150">Bir rotanın bir özniteliği kullanılarak tanımlanan bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="73414-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="73414-151">Dize &quot;müşteriler / {CustomerID} / orders&quot; rota için URI şablonudur.</span><span class="sxs-lookup"><span data-stu-id="73414-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="73414-152">Web API'si, ' % s'istek URI'SİNDEN şablon eşleştirmeyi dener.</span><span class="sxs-lookup"><span data-stu-id="73414-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="73414-153">Bu örnekte, "Müşteri" ve "Siparişler" değişmez değer segmentler ve "{CustomerID}" bir değişken bir parametredir.</span><span class="sxs-lookup"><span data-stu-id="73414-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="73414-154">Bu şablon aşağıdaki bir URI'leri eşleşir:</span><span class="sxs-lookup"><span data-stu-id="73414-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="73414-155">Kullanarak eşleşen kısıtlayabilirsiniz [kısıtlamaları](#constraints), bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="73414-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="73414-156">Dikkat &quot;{CustomerID}&quot; rota şablonu parametre adı ile eşleşen *CustomerID* yöntem parametresi.</span><span class="sxs-lookup"><span data-stu-id="73414-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="73414-157">Web API denetleyici eylemini çalıştırdığında, rota parametrelerine bağlamak çalışır.</span><span class="sxs-lookup"><span data-stu-id="73414-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="73414-158">Örneğin, URI ise `http://example.com/customers/1/orders`, Web API'sini çalışır değerini "1" bağlamak *CustomerID* eylem parametresi.</span><span class="sxs-lookup"><span data-stu-id="73414-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="73414-159">Bir URI şablonu, birkaç parametre sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="73414-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="73414-160">Kural tabanlı yönlendirme, yol özniteliğine sahip olmayan herhangi bir denetleyici yöntemin kullanın.</span><span class="sxs-lookup"><span data-stu-id="73414-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="73414-161">Böylece, her iki tür aynı projede yönlendirme birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="73414-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="73414-162">HTTP yöntemleri</span><span class="sxs-lookup"><span data-stu-id="73414-162">HTTP Methods</span></span>

<span data-ttu-id="73414-163">Web API'si işlemleri (GET, POST, vb.) isteğin HTTP yöntemine dayalı seçilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="73414-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="73414-164">Varsayılan olarak, Web API denetleyicisi yöntem adı başlangıcını harf olarak eşleşen arar.</span><span class="sxs-lookup"><span data-stu-id="73414-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="73414-165">Örneğin, adında bir denetleyici yöntemi `PutCustomers` eşleşen bir HTTP PUT İsteği.</span><span class="sxs-lookup"><span data-stu-id="73414-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="73414-166">Aşağıdaki özniteliklerden herhangi bir yöntemle tasarlayarak bu kuralı kılabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="73414-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="73414-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="73414-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="73414-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="73414-168">**[HttpGet]**</span></span>
- <span data-ttu-id="73414-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="73414-169">**[HttpHead]**</span></span>
- <span data-ttu-id="73414-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="73414-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="73414-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="73414-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="73414-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="73414-172">**[HttpPost]**</span></span>
- <span data-ttu-id="73414-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="73414-173">**[HttpPut]**</span></span>

<span data-ttu-id="73414-174">Aşağıdaki örnek, HTTP POST istekleri CreateBook yöntemi eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="73414-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="73414-175">Standart olmayan yöntemler de dahil olmak üzere tüm diğer HTTP yöntemleri kullanmak için **AcceptVerbs** özniteliği HTTP yöntemlerinin listesini alır.</span><span class="sxs-lookup"><span data-stu-id="73414-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="73414-176">Yol ön ekleri</span><span class="sxs-lookup"><span data-stu-id="73414-176">Route Prefixes</span></span>

<span data-ttu-id="73414-177">Genellikle, tümü aynı önekiyle başlayan bir denetleyici yolları.</span><span class="sxs-lookup"><span data-stu-id="73414-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="73414-178">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="73414-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="73414-179">Tüm bir denetleyici için ortak bir öneki kullanarak ayarlayabilirsiniz **[routeprefix öğesi]** özniteliği:</span><span class="sxs-lookup"><span data-stu-id="73414-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="73414-180">Bir tilde (~) üzerinde method özniteliği rota öneki geçersiz kılmak için kullanın:</span><span class="sxs-lookup"><span data-stu-id="73414-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="73414-181">Rota öneki parametreleri şunları içerebilir:</span><span class="sxs-lookup"><span data-stu-id="73414-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="73414-182">Rota kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="73414-182">Route Constraints</span></span>

<span data-ttu-id="73414-183">Rota kısıtlamalarını nasıl yol şablonunda parametreler olarak eşleştirilir kısıtlamanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="73414-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="73414-184">Genel söz dizimi &quot;{parametresi: kısıtlaması}&quot;.</span><span class="sxs-lookup"><span data-stu-id="73414-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="73414-185">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="73414-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="73414-186">Burada, ilk yolun yalnızca, seçili olur &quot;kimliği&quot; URI parçası olan bir tamsayı.</span><span class="sxs-lookup"><span data-stu-id="73414-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="73414-187">Aksi takdirde, ikinci rota seçilir.</span><span class="sxs-lookup"><span data-stu-id="73414-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="73414-188">Aşağıdaki tablo desteklenen kısıtlamaları listeler.</span><span class="sxs-lookup"><span data-stu-id="73414-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="73414-189">Kısıtlama</span><span class="sxs-lookup"><span data-stu-id="73414-189">Constraint</span></span> | <span data-ttu-id="73414-190">Açıklama</span><span class="sxs-lookup"><span data-stu-id="73414-190">Description</span></span> | <span data-ttu-id="73414-191">Örnek</span><span class="sxs-lookup"><span data-stu-id="73414-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73414-192">alfa</span><span class="sxs-lookup"><span data-stu-id="73414-192">alpha</span></span> | <span data-ttu-id="73414-193">Eşleşme büyük veya küçük harf Latin alfabesi karakterler (a-z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="73414-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="73414-194">{alpha: x}</span><span class="sxs-lookup"><span data-stu-id="73414-194">{x:alpha}</span></span> |
| <span data-ttu-id="73414-195">bool</span><span class="sxs-lookup"><span data-stu-id="73414-195">bool</span></span> | <span data-ttu-id="73414-196">Bir Boolean değeri eşler.</span><span class="sxs-lookup"><span data-stu-id="73414-196">Matches a Boolean value.</span></span> | <span data-ttu-id="73414-197">{x: bool}</span><span class="sxs-lookup"><span data-stu-id="73414-197">{x:bool}</span></span> |
| <span data-ttu-id="73414-198">datetime</span><span class="sxs-lookup"><span data-stu-id="73414-198">datetime</span></span> | <span data-ttu-id="73414-199">Eşleşen bir **DateTime** değeri.</span><span class="sxs-lookup"><span data-stu-id="73414-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="73414-200">{x:datetime}</span><span class="sxs-lookup"><span data-stu-id="73414-200">{x:datetime}</span></span> |
| <span data-ttu-id="73414-201">decimal</span><span class="sxs-lookup"><span data-stu-id="73414-201">decimal</span></span> | <span data-ttu-id="73414-202">Ondalık bir değeri eşler.</span><span class="sxs-lookup"><span data-stu-id="73414-202">Matches a decimal value.</span></span> | <span data-ttu-id="73414-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="73414-203">{x:decimal}</span></span> |
| <span data-ttu-id="73414-204">çift</span><span class="sxs-lookup"><span data-stu-id="73414-204">double</span></span> | <span data-ttu-id="73414-205">Bir 64-bit kayan nokta değeri eşler.</span><span class="sxs-lookup"><span data-stu-id="73414-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="73414-206">{x:double}</span><span class="sxs-lookup"><span data-stu-id="73414-206">{x:double}</span></span> |
| <span data-ttu-id="73414-207">float</span><span class="sxs-lookup"><span data-stu-id="73414-207">float</span></span> | <span data-ttu-id="73414-208">Bir 32-bit kayan nokta değeri eşler.</span><span class="sxs-lookup"><span data-stu-id="73414-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="73414-209">x: kayan {}</span><span class="sxs-lookup"><span data-stu-id="73414-209">{x:float}</span></span> |
| <span data-ttu-id="73414-210">GUID</span><span class="sxs-lookup"><span data-stu-id="73414-210">guid</span></span> | <span data-ttu-id="73414-211">Bir GUID değeri eşler.</span><span class="sxs-lookup"><span data-stu-id="73414-211">Matches a GUID value.</span></span> | <span data-ttu-id="73414-212">{x: GUID}</span><span class="sxs-lookup"><span data-stu-id="73414-212">{x:guid}</span></span> |
| <span data-ttu-id="73414-213">int</span><span class="sxs-lookup"><span data-stu-id="73414-213">int</span></span> | <span data-ttu-id="73414-214">Bir 32-bit tamsayı değeri eşler.</span><span class="sxs-lookup"><span data-stu-id="73414-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="73414-215">{x:int}</span><span class="sxs-lookup"><span data-stu-id="73414-215">{x:int}</span></span> |
| <span data-ttu-id="73414-216">length</span><span class="sxs-lookup"><span data-stu-id="73414-216">length</span></span> | <span data-ttu-id="73414-217">Belirtilen uzunlukta veya uzunluklarının belirtilen bir aralık içindeki bir dizeyle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="73414-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="73414-218">{x: length(6)} {x: length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="73414-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="73414-219">long</span><span class="sxs-lookup"><span data-stu-id="73414-219">long</span></span> | <span data-ttu-id="73414-220">Bir 64-bit tamsayı değeri eşler.</span><span class="sxs-lookup"><span data-stu-id="73414-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="73414-221">{x: uzun}</span><span class="sxs-lookup"><span data-stu-id="73414-221">{x:long}</span></span> |
| <span data-ttu-id="73414-222">max</span><span class="sxs-lookup"><span data-stu-id="73414-222">max</span></span> | <span data-ttu-id="73414-223">Bir tamsayı en yüksek bir değerle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="73414-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="73414-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="73414-224">{x:max(10)}</span></span> |
| <span data-ttu-id="73414-225">MaxLength</span><span class="sxs-lookup"><span data-stu-id="73414-225">maxlength</span></span> | <span data-ttu-id="73414-226">En fazla bir dizeyle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="73414-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="73414-227">{x:maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="73414-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="73414-228">min</span><span class="sxs-lookup"><span data-stu-id="73414-228">min</span></span> | <span data-ttu-id="73414-229">Bir tamsayı en az bir değerle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="73414-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="73414-230">{x: min(10)}</span><span class="sxs-lookup"><span data-stu-id="73414-230">{x:min(10)}</span></span> |
| <span data-ttu-id="73414-231">MinLength</span><span class="sxs-lookup"><span data-stu-id="73414-231">minlength</span></span> | <span data-ttu-id="73414-232">Minimum uzunluk bir dizeyle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="73414-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="73414-233">{x: minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="73414-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="73414-234">aralık</span><span class="sxs-lookup"><span data-stu-id="73414-234">range</span></span> | <span data-ttu-id="73414-235">Tamsayı değerleri aralığı içinde eşleşir.</span><span class="sxs-lookup"><span data-stu-id="73414-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="73414-236">{x: range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="73414-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="73414-237">Normal ifade</span><span class="sxs-lookup"><span data-stu-id="73414-237">regex</span></span> | <span data-ttu-id="73414-238">Normal bir ifadeyle eşleşiyor.</span><span class="sxs-lookup"><span data-stu-id="73414-238">Matches a regular expression.</span></span> | <span data-ttu-id="73414-239">{x: regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="73414-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="73414-240">Bildirimi, bazı kısıtlamaları gibi &quot;min&quot;, parantez içine bağımsız değişken almaz.</span><span class="sxs-lookup"><span data-stu-id="73414-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="73414-241">Virgül ile ayrılmış bir parametre, birden çok kısıtlaması uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="73414-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="73414-242">Özel rota kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="73414-242">Custom Route Constraints</span></span>

<span data-ttu-id="73414-243">Özel rota kısıtlamalarını uygulayarak oluşturabilirsiniz **IHttpRouteConstraint** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="73414-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="73414-244">Örneğin, aşağıdaki kısıtlaması bir parametre sıfır olmayan bir tamsayı değerine kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="73414-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="73414-245">Aşağıdaki kod, kısıtlama kaydettirmek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="73414-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="73414-246">Artık yollarınızı kısıtlaması uygulayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="73414-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="73414-247">Ayrıca tüm değiştirebilirsiniz **DefaultInlineConstraintResolver** uygulayarak sınıfı **IInlineConstraintResolver** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="73414-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="73414-248">Bunun yapılması yerini alacak tüm yerleşik kısıtlamalar sürece uygulamanıza **IInlineConstraintResolver** özellikle ekler.</span><span class="sxs-lookup"><span data-stu-id="73414-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="73414-249">İsteğe bağlı URI parametreleri ve varsayılan değerler</span><span class="sxs-lookup"><span data-stu-id="73414-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="73414-250">Bir soru işareti için bir rota parametresini ekleyerek bir URI parametresinin isteğe bağlı yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="73414-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="73414-251">Bir rota parametresini isteğe bağlı ise, yöntem parametresi için varsayılan bir değer tanımlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="73414-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="73414-252">Bu örnekte, `/api/books/locale/1033` ve `/api/books/locale` aynı kaynak döndürür.</span><span class="sxs-lookup"><span data-stu-id="73414-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="73414-253">Alternatif olarak, rota şablonu içinde bir varsayılan değer şu şekilde belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="73414-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="73414-254">Bu hemen önceki örnekle aynıdır, ancak varsayılan değer uygulandığında izinlerde davranış olduğu.</span><span class="sxs-lookup"><span data-stu-id="73414-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="73414-255">Parametresi bu değere sahiptir; bu nedenle ilk örnekte ("{LCID?}"), doğrudan yöntem parametresi için 1033 varsayılan değeri atanır.</span><span class="sxs-lookup"><span data-stu-id="73414-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="73414-256">İkinci örnekte ("{lcid = 1033}"), "1033" varsayılan değeri model bağlama işlemi boyunca geçer.</span><span class="sxs-lookup"><span data-stu-id="73414-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="73414-257">Varsayılan model bağlayıcısını "1033" 1033 sayısal değerine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="73414-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="73414-258">Ancak, farklı bir şey yapabilir ve özel bir model bağlayıcı eklenebilecek.</span><span class="sxs-lookup"><span data-stu-id="73414-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="73414-259">(Özel model bağlayıcıları, işlem hattınızda olmadığı sürece çoğu durumda, iki eşdeğer olacaktır.)</span><span class="sxs-lookup"><span data-stu-id="73414-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="73414-260">Rota adları</span><span class="sxs-lookup"><span data-stu-id="73414-260">Route Names</span></span>

<span data-ttu-id="73414-261">Web API'de her yol bir adı vardır.</span><span class="sxs-lookup"><span data-stu-id="73414-261">In Web API, every route has a name.</span></span> <span data-ttu-id="73414-262">Bir HTTP yanıtında bir bağlantı ekleyebilirsiniz, böylece rota adları, bağlantı oluşturmak için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="73414-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="73414-263">Rota adını belirtmek için ayarlayın **adı** öznitelik özelliği.</span><span class="sxs-lookup"><span data-stu-id="73414-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="73414-264">Aşağıdaki örnek nasıl ayarlanacağı rota adı ve rota adını, bağlantı oluşturulurken kullanılacak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="73414-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="73414-265">Rota sırası</span><span class="sxs-lookup"><span data-stu-id="73414-265">Route Order</span></span>

<span data-ttu-id="73414-266">Framework bir URI bir rotayla eşleşen çalıştığında, belirli bir sırada yolları değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="73414-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="73414-267">Sıra belirtmek için ayarlanmış **RouteOrder** rota özniteliğinin özelliği.</span><span class="sxs-lookup"><span data-stu-id="73414-267">To specify the order, set the **RouteOrder** property on the route attribute.</span></span> <span data-ttu-id="73414-268">Düşük değerler, ilk olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="73414-268">Lower values are evaluated first.</span></span> <span data-ttu-id="73414-269">Varsayılan sıra değeri sıfırdır.</span><span class="sxs-lookup"><span data-stu-id="73414-269">The default order value is zero.</span></span>

<span data-ttu-id="73414-270">Toplam sıralama nasıl belirlendiğini aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="73414-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="73414-271">Karşılaştırma **RouteOrder** rota özniteliğinin bir özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="73414-271">Compare the **RouteOrder** property of the route attribute.</span></span>
2. <span data-ttu-id="73414-272">Rota şablonu içinde her bir URI segmenti bakın.</span><span class="sxs-lookup"><span data-stu-id="73414-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="73414-273">Her bir kesim için şu şekilde sıralayın:</span><span class="sxs-lookup"><span data-stu-id="73414-273">For each segment, order as follows:</span></span> 

    1. <span data-ttu-id="73414-274">Değişmez değer kesimi.</span><span class="sxs-lookup"><span data-stu-id="73414-274">Literal segments.</span></span>
    2. <span data-ttu-id="73414-275">Rota parametrelerinin kısıtlamaları.</span><span class="sxs-lookup"><span data-stu-id="73414-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="73414-276">Rota parametrelerinin kısıtlamaları olmadan.</span><span class="sxs-lookup"><span data-stu-id="73414-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="73414-277">Joker karakter parametresi kesimleri kısıtlamalarına sahip.</span><span class="sxs-lookup"><span data-stu-id="73414-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="73414-278">Joker karakter parametresi kesimleri kısıtlamaları olmadan.</span><span class="sxs-lookup"><span data-stu-id="73414-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="73414-279">Bağ olması durumunda, yolların bir büyük küçük harf duyarsız sıralı dize karşılaştırmasına göre sıralanır ([Ordinalıgnorecase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) rota şablonu.</span><span class="sxs-lookup"><span data-stu-id="73414-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="73414-280">Aşağıda bir örnek vardır.</span><span class="sxs-lookup"><span data-stu-id="73414-280">Here is an example.</span></span> <span data-ttu-id="73414-281">Aşağıdaki denetleyicisi tanımladığınız varsayalım:</span><span class="sxs-lookup"><span data-stu-id="73414-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="73414-282">Bu yolları şu şekilde sıralanır.</span><span class="sxs-lookup"><span data-stu-id="73414-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="73414-283">Sipariş/Ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="73414-283">orders/details</span></span>
2. <span data-ttu-id="73414-284">Siparişler / {id}</span><span class="sxs-lookup"><span data-stu-id="73414-284">orders/{id}</span></span>
3. <span data-ttu-id="73414-285">Siparişler / {customerName}</span><span class="sxs-lookup"><span data-stu-id="73414-285">orders/{customerName}</span></span>
4. <span data-ttu-id="73414-286">Siparişler / {\*tarih}</span><span class="sxs-lookup"><span data-stu-id="73414-286">orders/{\*date}</span></span>
5. <span data-ttu-id="73414-287">Siparişler / beklemede</span><span class="sxs-lookup"><span data-stu-id="73414-287">orders/pending</span></span>

<span data-ttu-id="73414-288">"Details" bir değişmez değer kesimi ve önce "{id}" görünür ancak "bekliyor" son görünür, çünkü bildirimi **RouteOrder** özelliği 1'dir.</span><span class="sxs-lookup"><span data-stu-id="73414-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **RouteOrder** property is 1.</span></span> <span data-ttu-id="73414-289">(Bu örnekte "details" adlı hiçbir müşterinin var. varsayılır veya "bekliyor".</span><span class="sxs-lookup"><span data-stu-id="73414-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="73414-290">Genel olarak, belirsiz yollar kaçınmaya çalışın.</span><span class="sxs-lookup"><span data-stu-id="73414-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="73414-291">Bu örnekte, daha iyi bir rota şablonu için `GetByCustomer` olan "müşteriler / {customerName}")</span><span class="sxs-lookup"><span data-stu-id="73414-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
