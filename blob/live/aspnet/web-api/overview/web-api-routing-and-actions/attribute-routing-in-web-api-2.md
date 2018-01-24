---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: "Öznitelik ASP.NET Web API 2 yönlendirme | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 67ab1536b4a72abf8c0d3ed5aa0c48bc79a8fb5f
ms.sourcegitcommit: 3d512ea991ac36dfd4c800b7d1f8a27bfc50635e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="56f6a-102">ASP.NET Web API 2 özniteliği yönlendirme</span><span class="sxs-lookup"><span data-stu-id="56f6a-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="56f6a-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="56f6a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="56f6a-104">*Yönlendirme* bir eylem için bir URI Web API'sini nasıl eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="56f6a-105">Web API 2 destekleyen yeni bir tür yönlendirme biri olarak adlandırılan *özniteliği yönlendirme*.</span><span class="sxs-lookup"><span data-stu-id="56f6a-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="56f6a-106">Adından da anlaşılacağı gibi özniteliği yönlendirme rotaları tanımlamak için özniteliklerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="56f6a-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="56f6a-107">Öznitelik yönlendirme, web API URI'ler üzerinde daha fazla denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="56f6a-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="56f6a-108">Örneğin, kaynak hiyerarşileri tanımlayan URI kolayca oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="56f6a-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="56f6a-109">Kurala dayalı olarak adlandırılan yönlendirme, önceki stili yönlendirme, hala tam olarak desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="56f6a-110">Aslında, her iki tekniği aynı projede de birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="56f6a-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="56f6a-111">Bu konu, öznitelik yönlendirme etkinleştirme gösterir ve öznitelik yönlendirme çeşitli seçenekler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="56f6a-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="56f6a-112">Öznitelik yönlendirmesi kullanan bir uçtan uca öğretici için bkz [özniteliği yönlendirme Web API 2'deki bir REST API'si oluşturma](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="56f6a-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="56f6a-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="56f6a-113">Prerequisites</span></span>

<span data-ttu-id="56f6a-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional veya Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="56f6a-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition</span></span>

<span data-ttu-id="56f6a-115">Alternatif olarak, gerekli paketleri yüklemek için NuGet paket yöneticisini kullanın.</span><span class="sxs-lookup"><span data-stu-id="56f6a-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="56f6a-116">Gelen **Araçları** menü Visual Studio'da seçin **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="56f6a-116">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="56f6a-117">Paket Yöneticisi konsol penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="56f6a-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="56f6a-118">Neden yönlendirme özniteliği?</span><span class="sxs-lookup"><span data-stu-id="56f6a-118">Why Attribute Routing?</span></span>

<span data-ttu-id="56f6a-119">Web API kullanılan ilk sürümünü *kurala dayalı* yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="56f6a-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="56f6a-120">Yönlendirme bu tür, temel daha fazla yol şablon dizeleri parametreli veya bir tanımlar.</span><span class="sxs-lookup"><span data-stu-id="56f6a-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="56f6a-121">Çerçeve bir istek aldığında, rota şablonu karşı URI eşleşir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="56f6a-122">(Kurala dayalı yönlendirme hakkında daha fazla bilgi için bkz: [ASP.NET Web API'de yönlendirme](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="56f6a-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="56f6a-123">Kurala dayalı yönlendirme bir avantajı, şablonları tek bir yerde tanımlanır ve yönlendirme kurallarını tüm denetleyicilerinde tutarlı bir şekilde uygulandığından olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="56f6a-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="56f6a-124">Ne yazık ki, kurala dayalı yönlendirme sabit RESTful API'leri ortak olan belirli URI desenleri desteklemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="56f6a-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="56f6a-125">Örneğin, kaynak alt kaynakları genellikle içerir: müşterilerin sahip siparişleri, filmler sahip aktörler, books yazarlarının vardır ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="56f6a-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="56f6a-126">Bu ilişkileri yansıtacak URI oluşturmak için doğal şöyledir:</span><span class="sxs-lookup"><span data-stu-id="56f6a-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="56f6a-127">Bu tür bir URI, kurala dayalı yönlendirme kullanarak oluşturduğunuz zordur.</span><span class="sxs-lookup"><span data-stu-id="56f6a-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="56f6a-128">Yapılabilir rağmen iyi birçok denetleyicileri veya kaynak türleri varsa sonuçları ölçeklendirmeyin.</span><span class="sxs-lookup"><span data-stu-id="56f6a-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="56f6a-129">Öznitelik yönlendirme ile bir rota için bu URI tanımlamak için kısmı oldukça kolaydır.</span><span class="sxs-lookup"><span data-stu-id="56f6a-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="56f6a-130">Yalnızca denetleyici eylemi bir özniteliği ekleyin:</span><span class="sxs-lookup"><span data-stu-id="56f6a-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="56f6a-131">Burada, yönlendirme yapar kolay özniteliği bazı bir desenleri bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="56f6a-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="56f6a-132">**API sürümü oluşturma**</span><span class="sxs-lookup"><span data-stu-id="56f6a-132">**API versioning**</span></span>

<span data-ttu-id="56f6a-133">Bu örnekte, "/ v1/api/ürünleri" olacaktır farklı bir denetleyici yönlendirilmiş "/ v2/api/ürünleri".</span><span class="sxs-lookup"><span data-stu-id="56f6a-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`  
`/api/v2/products`

<span data-ttu-id="56f6a-134">**Aşırı yüklenmiş URI parçaları**</span><span class="sxs-lookup"><span data-stu-id="56f6a-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="56f6a-135">Bu örnekte, "1" bir sıra numarasıdır, ancak "bekliyor" bir koleksiyona eşler.</span><span class="sxs-lookup"><span data-stu-id="56f6a-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`  
`/orders/pending`

<span data-ttu-id="56f6a-136">**Birden çok parametre türleri**</span><span class="sxs-lookup"><span data-stu-id="56f6a-136">**Multiple parameter types**</span></span>

<span data-ttu-id="56f6a-137">Bu örnekte, "1" bir sıra numarasıdır, ancak "06/2013/16" bir tarihi belirtir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="56f6a-138">Öznitelik yönlendirmeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="56f6a-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="56f6a-139">Öznitelik yönlendirmeyi etkinleştirmek için arama **MapHttpAttributeRoutes** yapılandırma sırasında.</span><span class="sxs-lookup"><span data-stu-id="56f6a-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="56f6a-140">Bu uzantı metodu tanımlanan **System.Web.Http.HttpConfigurationExtensions** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="56f6a-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="56f6a-141">İle öznitelik yönlendirme birleştirilebilir [kurala dayalı](routing-in-aspnet-web-api.md) yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="56f6a-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="56f6a-142">Kurala dayalı rotaları tanımlamak için arama **MapHttpRoute** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="56f6a-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="56f6a-143">Web API yapılandırma hakkında daha fazla bilgi için bkz: [yapılandırma ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="56f6a-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="56f6a-144">Not: Web API 1 ' geçiş</span><span class="sxs-lookup"><span data-stu-id="56f6a-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="56f6a-145">Web API 2 önce Web API projesi şablonları aşağıdakine benzer bir kod oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="56f6a-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="56f6a-146">Öznitelik yönlendirme etkinse, bu kod bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="56f6a-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="56f6a-147">Öznitelik yönlendirmeyi kullanmak için mevcut bir Web API projesini yükseltirseniz, bu yapılandırma kodunu aşağıdaki güncelleştirdiğinizden emin olun:</span><span class="sxs-lookup"><span data-stu-id="56f6a-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="56f6a-148">Daha fazla bilgi için bkz: [ASP.NET barındırma ile Web API yapılandırma](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="56f6a-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="56f6a-149">Rota öznitelikleri ekleme</span><span class="sxs-lookup"><span data-stu-id="56f6a-149">Adding Route Attributes</span></span>

<span data-ttu-id="56f6a-150">Bir öznitelik kullanılarak tanımlanmış bir yol bir örneği burada verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="56f6a-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="56f6a-151">Dize &quot;müşteriler / {CustomerID} / siparişleri&quot; rota için URI şablonudur.</span><span class="sxs-lookup"><span data-stu-id="56f6a-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="56f6a-152">Web API, istek URI'si şablon eşleştirmeyi dener.</span><span class="sxs-lookup"><span data-stu-id="56f6a-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="56f6a-153">Bu örnekte, "müşteriler" ve "Siparişler" değişmez değer kesimleri olan ve "{CustomerID}" değişken bir parametredir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="56f6a-154">Bu şablon aşağıdaki URI'ler eşleşir:</span><span class="sxs-lookup"><span data-stu-id="56f6a-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="56f6a-155">Kullanarak eşleşen kısıtlayabilirsiniz [kısıtlamaları](#constraints), bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="56f6a-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="56f6a-156">Dikkat &quot;{CustomerID}&quot; rota şablonu parametresinde eşleşen adını *CustomerID* yöntemi parametresi.</span><span class="sxs-lookup"><span data-stu-id="56f6a-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="56f6a-157">Web API denetleyici eylemi çalıştırdığında, rota parametrelerinin bağlamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="56f6a-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="56f6a-158">Örneğin, URI ise `http://example.com/customers/1/orders`, "1" değeri bağlamak Web API dener *CustomerID* eylem parametresi.</span><span class="sxs-lookup"><span data-stu-id="56f6a-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="56f6a-159">Bir URI şablonunu birkaç parametre sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="56f6a-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="56f6a-160">Bir rota özniteliğine sahip olmayan herhangi bir denetleyici yöntemin kurala dayalı yönlendirme kullanın.</span><span class="sxs-lookup"><span data-stu-id="56f6a-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="56f6a-161">Bu şekilde, her iki tür aynı projede yönlendirme birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="56f6a-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="56f6a-162">HTTP yöntemleri</span><span class="sxs-lookup"><span data-stu-id="56f6a-162">HTTP Methods</span></span>

<span data-ttu-id="56f6a-163">Web API (GET, POST, vb.) İstek HTTP yöntemine dayalı eylemleri de seçer.</span><span class="sxs-lookup"><span data-stu-id="56f6a-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="56f6a-164">Varsayılan olarak, Web API denetleyicisi yöntem adı başlangıcı küçük harf olarak eşleşen arar.</span><span class="sxs-lookup"><span data-stu-id="56f6a-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="56f6a-165">Örneğin, adlandırılmış bir denetleyici yöntemi `PutCustomers` bir HTTP PUT İsteği eşleşir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="56f6a-166">Aşağıdaki öznitelikler herhangi bir yöntemle tasarlayarak bu kuralı kılabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="56f6a-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="56f6a-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="56f6a-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="56f6a-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="56f6a-168">**[HttpGet]**</span></span>
- <span data-ttu-id="56f6a-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="56f6a-169">**[HttpHead]**</span></span>
- <span data-ttu-id="56f6a-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="56f6a-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="56f6a-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="56f6a-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="56f6a-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="56f6a-172">**[HttpPost]**</span></span>
- <span data-ttu-id="56f6a-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="56f6a-173">**[HttpPut]**</span></span>

<span data-ttu-id="56f6a-174">Aşağıdaki örnek CreateBook yöntemini HTTP POST isteklerini eşler.</span><span class="sxs-lookup"><span data-stu-id="56f6a-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="56f6a-175">Standart olmayan yöntemleri de dahil olmak üzere tüm diğer HTTP yöntemleri kullanmak için **AcceptVerbs** özniteliği HTTP yöntemlerinin listesini alır.</span><span class="sxs-lookup"><span data-stu-id="56f6a-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="56f6a-176">Rota önekleri</span><span class="sxs-lookup"><span data-stu-id="56f6a-176">Route Prefixes</span></span>

<span data-ttu-id="56f6a-177">Genellikle, yollar tüm aynı önekiyle başlayan bir denetleyici.</span><span class="sxs-lookup"><span data-stu-id="56f6a-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="56f6a-178">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="56f6a-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="56f6a-179">Tüm bir denetleyici için ortak bir önek kullanarak ayarlayabilirsiniz **[routeprefix öğesi]** özniteliği:</span><span class="sxs-lookup"><span data-stu-id="56f6a-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="56f6a-180">Bir tilde (~) yöntemi öznitelikte rota öneki geçersiz kılmak için kullanın:</span><span class="sxs-lookup"><span data-stu-id="56f6a-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="56f6a-181">Rota öneki parametreleri içerebilir:</span><span class="sxs-lookup"><span data-stu-id="56f6a-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="56f6a-182">Rota kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="56f6a-182">Route Constraints</span></span>

<span data-ttu-id="56f6a-183">Rota kısıtlamalarını, rota şablonu parametrelerinde nasıl eşleştirilir kısıtlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="56f6a-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="56f6a-184">Genel sözdizimi &quot;{parametresi: kısıtlaması}&quot;.</span><span class="sxs-lookup"><span data-stu-id="56f6a-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="56f6a-185">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="56f6a-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="56f6a-186">Burada, ilk yol yalnızca durumunda seçilecektir &quot;kimliği&quot; URI parçası olan bir tamsayı.</span><span class="sxs-lookup"><span data-stu-id="56f6a-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="56f6a-187">Aksi takdirde, ikinci yol seçilir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="56f6a-188">Aşağıdaki tabloda, desteklenen kısıtlamaları listeler.</span><span class="sxs-lookup"><span data-stu-id="56f6a-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="56f6a-189">Kısıtlama</span><span class="sxs-lookup"><span data-stu-id="56f6a-189">Constraint</span></span> | <span data-ttu-id="56f6a-190">Açıklama</span><span class="sxs-lookup"><span data-stu-id="56f6a-190">Description</span></span> | <span data-ttu-id="56f6a-191">Örnek</span><span class="sxs-lookup"><span data-stu-id="56f6a-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="56f6a-192">Alfa</span><span class="sxs-lookup"><span data-stu-id="56f6a-192">alpha</span></span> | <span data-ttu-id="56f6a-193">Eşleşme büyük veya küçük harf Latin alfabesi karakterler (a-z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="56f6a-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="56f6a-194">{x:alpha}</span><span class="sxs-lookup"><span data-stu-id="56f6a-194">{x:alpha}</span></span> |
| <span data-ttu-id="56f6a-195">bool</span><span class="sxs-lookup"><span data-stu-id="56f6a-195">bool</span></span> | <span data-ttu-id="56f6a-196">Bir Boole değeri ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-196">Matches a Boolean value.</span></span> | <span data-ttu-id="56f6a-197">{x:bool}</span><span class="sxs-lookup"><span data-stu-id="56f6a-197">{x:bool}</span></span> |
| <span data-ttu-id="56f6a-198">datetime</span><span class="sxs-lookup"><span data-stu-id="56f6a-198">datetime</span></span> | <span data-ttu-id="56f6a-199">Eşleşen bir **DateTime** değeri.</span><span class="sxs-lookup"><span data-stu-id="56f6a-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="56f6a-200">{x:datetime}</span><span class="sxs-lookup"><span data-stu-id="56f6a-200">{x:datetime}</span></span> |
| <span data-ttu-id="56f6a-201">decimal</span><span class="sxs-lookup"><span data-stu-id="56f6a-201">decimal</span></span> | <span data-ttu-id="56f6a-202">Ondalık bir değeri ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-202">Matches a decimal value.</span></span> | <span data-ttu-id="56f6a-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="56f6a-203">{x:decimal}</span></span> |
| <span data-ttu-id="56f6a-204">çift</span><span class="sxs-lookup"><span data-stu-id="56f6a-204">double</span></span> | <span data-ttu-id="56f6a-205">64-bit kayan nokta değeri eşleşir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="56f6a-206">{x:double}</span><span class="sxs-lookup"><span data-stu-id="56f6a-206">{x:double}</span></span> |
| <span data-ttu-id="56f6a-207">float</span><span class="sxs-lookup"><span data-stu-id="56f6a-207">float</span></span> | <span data-ttu-id="56f6a-208">32 bit kayan nokta değeri eşleşir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="56f6a-209">{x:float}</span><span class="sxs-lookup"><span data-stu-id="56f6a-209">{x:float}</span></span> |
| <span data-ttu-id="56f6a-210">GUID</span><span class="sxs-lookup"><span data-stu-id="56f6a-210">guid</span></span> | <span data-ttu-id="56f6a-211">GUID değeri eşleşir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-211">Matches a GUID value.</span></span> | <span data-ttu-id="56f6a-212">{x: GUID}</span><span class="sxs-lookup"><span data-stu-id="56f6a-212">{x:guid}</span></span> |
| <span data-ttu-id="56f6a-213">int</span><span class="sxs-lookup"><span data-stu-id="56f6a-213">int</span></span> | <span data-ttu-id="56f6a-214">32 bit tamsayı değeri eşleşir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="56f6a-215">{x:int}</span><span class="sxs-lookup"><span data-stu-id="56f6a-215">{x:int}</span></span> |
| <span data-ttu-id="56f6a-216">length</span><span class="sxs-lookup"><span data-stu-id="56f6a-216">length</span></span> | <span data-ttu-id="56f6a-217">Belirtilen uzunluğa sahip veya uzunlukları belirtilen aralığı içinde bir dizeyle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="56f6a-218">{x:length(6)} {x:length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="56f6a-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="56f6a-219">long</span><span class="sxs-lookup"><span data-stu-id="56f6a-219">long</span></span> | <span data-ttu-id="56f6a-220">64 bit tamsayı değeri eşleşir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="56f6a-221">{x:long}</span><span class="sxs-lookup"><span data-stu-id="56f6a-221">{x:long}</span></span> |
| <span data-ttu-id="56f6a-222">max</span><span class="sxs-lookup"><span data-stu-id="56f6a-222">max</span></span> | <span data-ttu-id="56f6a-223">Tamsayı bir maksimum değer ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="56f6a-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="56f6a-224">{x:max(10)}</span></span> |
| <span data-ttu-id="56f6a-225">MaxLength</span><span class="sxs-lookup"><span data-stu-id="56f6a-225">maxlength</span></span> | <span data-ttu-id="56f6a-226">En fazla bir dizeyle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="56f6a-227">{x:maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="56f6a-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="56f6a-228">min</span><span class="sxs-lookup"><span data-stu-id="56f6a-228">min</span></span> | <span data-ttu-id="56f6a-229">Tamsayı en az bir değerle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="56f6a-230">{x:min(10)}</span><span class="sxs-lookup"><span data-stu-id="56f6a-230">{x:min(10)}</span></span> |
| <span data-ttu-id="56f6a-231">MinLength</span><span class="sxs-lookup"><span data-stu-id="56f6a-231">minlength</span></span> | <span data-ttu-id="56f6a-232">Minimum uzunluk bir dizeyle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="56f6a-233">{x:minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="56f6a-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="56f6a-234">aralık</span><span class="sxs-lookup"><span data-stu-id="56f6a-234">range</span></span> | <span data-ttu-id="56f6a-235">Tamsayı değerleri aralığı içinde eşleşir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="56f6a-236">{x:range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="56f6a-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="56f6a-237">Regex</span><span class="sxs-lookup"><span data-stu-id="56f6a-237">regex</span></span> | <span data-ttu-id="56f6a-238">Normal ifadeyle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-238">Matches a regular expression.</span></span> | <span data-ttu-id="56f6a-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="56f6a-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="56f6a-240">Bildirimi, bazı kısıtlamaları gibi &quot;min&quot;, bağımsız değişkenleri parantez içine alır.</span><span class="sxs-lookup"><span data-stu-id="56f6a-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="56f6a-241">Virgülle ayrılmış bir parametre birden çok kısıtlama uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="56f6a-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="56f6a-242">Özel rota kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="56f6a-242">Custom Route Constraints</span></span>

<span data-ttu-id="56f6a-243">Özel rota kısıtlamaları uygulayarak oluşturabileceğiniz **IHttpRouteConstraint** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="56f6a-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="56f6a-244">Örneğin, aşağıdaki kısıtlama parametre sıfır tamsayı değerine kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="56f6a-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="56f6a-245">Aşağıdaki kod kısıtlaması nasıl gösterir:</span><span class="sxs-lookup"><span data-stu-id="56f6a-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="56f6a-246">Şimdi yollarınızı kısıtlaması uygulayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="56f6a-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="56f6a-247">Ayrıca tüm değiştirin **DefaultInlineConstraintResolver** uygulayarak sınıfı **IInlineConstraintResolver** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="56f6a-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="56f6a-248">Bunun yapılması yerini alacak tüm yerleşik kısıtlamalar sürece uygulamanıza **IInlineConstraintResolver** özellikle bunları ekler.</span><span class="sxs-lookup"><span data-stu-id="56f6a-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="56f6a-249">İsteğe bağlı URI parametreleri ve varsayılan değerler</span><span class="sxs-lookup"><span data-stu-id="56f6a-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="56f6a-250">Bir URI parametre isteğe bağlı bir soru işareti rota parametresini ekleyerek yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="56f6a-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="56f6a-251">Bir rota parametresini isteğe bağlı ise, yöntem parametresi için varsayılan bir değer tanımlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="56f6a-252">Bu örnekte, `/api/books/locale/1033` ve `/api/books/locale` aynı kaynak döndürür.</span><span class="sxs-lookup"><span data-stu-id="56f6a-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="56f6a-253">Alternatif olarak, varsayılan değer rota şablonu içinde aşağıdaki gibi belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="56f6a-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="56f6a-254">Bu neredeyse önceki örnekle aynıdır, ancak varsayılan değer uygulandığında küçük bir fark davranış olduğu.</span><span class="sxs-lookup"><span data-stu-id="56f6a-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="56f6a-255">Parametresi bu tam değerine sahip şekilde ilk örnekte ("{LCID?}") 1033 varsayılan değerini doğrudan yöntem parametresi için atanır.</span><span class="sxs-lookup"><span data-stu-id="56f6a-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="56f6a-256">İkinci örnekte ("{LCID 1033 =}"), "1033" varsayılan değerini model bağlama süreci devam ettiği.</span><span class="sxs-lookup"><span data-stu-id="56f6a-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="56f6a-257">Varsayılan model bağlayıcısını "1033" 1033 sayısal değerine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="56f6a-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="56f6a-258">Ancak, farklı bir şey yapabilecek özel model bağlayıcı içinde takın.</span><span class="sxs-lookup"><span data-stu-id="56f6a-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="56f6a-259">(Özel model bağlayıcıları, ardışık düzeninde yoksa çoğu durumda, iki biçim eşdeğer olacaktır.)</span><span class="sxs-lookup"><span data-stu-id="56f6a-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="56f6a-260">Yol adları</span><span class="sxs-lookup"><span data-stu-id="56f6a-260">Route Names</span></span>

<span data-ttu-id="56f6a-261">Web API'de her rotanın bir adı vardır.</span><span class="sxs-lookup"><span data-stu-id="56f6a-261">In Web API, every route has a name.</span></span> <span data-ttu-id="56f6a-262">Böylece bir HTTP yanıt olarak bir bağlantı ekleyebilirsiniz rota adları bağlantıları oluşturmak için faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="56f6a-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="56f6a-263">Rota adını belirtmek için ayarlayın **adı** öznitelik özelliği.</span><span class="sxs-lookup"><span data-stu-id="56f6a-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="56f6a-264">Aşağıdaki örnek, rota adı ayarlama ve ayrıca bağlantı oluştururken rota adı kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="56f6a-265">Rota sırası</span><span class="sxs-lookup"><span data-stu-id="56f6a-265">Route Order</span></span>

<span data-ttu-id="56f6a-266">URI'sı bir yol ile eşleşecek şekilde framework çalıştığında, belirli bir sırada yollar değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="56f6a-267">Sıra belirtmek üzere ayarlayın **RouteOrder** rota özniteliğinin özelliği.</span><span class="sxs-lookup"><span data-stu-id="56f6a-267">To specify the order, set the **RouteOrder** property on the route attribute.</span></span> <span data-ttu-id="56f6a-268">Düşük değerler önce değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-268">Lower values are evaluated first.</span></span> <span data-ttu-id="56f6a-269">Varsayılan sıra değeri sıfır olur.</span><span class="sxs-lookup"><span data-stu-id="56f6a-269">The default order value is zero.</span></span>

<span data-ttu-id="56f6a-270">İşte toplam sıralama nasıl belirlenir:</span><span class="sxs-lookup"><span data-stu-id="56f6a-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="56f6a-271">Karşılaştırma **RouteOrder** rota özniteliğinin özelliği.</span><span class="sxs-lookup"><span data-stu-id="56f6a-271">Compare the **RouteOrder** property of the route attribute.</span></span>
2. <span data-ttu-id="56f6a-272">Rota şablonu her URI kesimdeki bakın.</span><span class="sxs-lookup"><span data-stu-id="56f6a-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="56f6a-273">Her segment için aşağıdaki gibi sipariş:</span><span class="sxs-lookup"><span data-stu-id="56f6a-273">For each segment, order as follows:</span></span> 

    1. <span data-ttu-id="56f6a-274">Değişmez değer kesimi.</span><span class="sxs-lookup"><span data-stu-id="56f6a-274">Literal segments.</span></span>
    2. <span data-ttu-id="56f6a-275">Rota parametrelerine kısıtlamalarına sahip.</span><span class="sxs-lookup"><span data-stu-id="56f6a-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="56f6a-276">Rota parametrelerine kısıtlamaları olmadan.</span><span class="sxs-lookup"><span data-stu-id="56f6a-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="56f6a-277">Joker karakter parametresi kesimleri kısıtlamalarına sahip.</span><span class="sxs-lookup"><span data-stu-id="56f6a-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="56f6a-278">Joker karakter parametresi kesimleri kısıtlamaları olmadan.</span><span class="sxs-lookup"><span data-stu-id="56f6a-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="56f6a-279">Bağ durumunda yollar büyük küçük harf duyarsız sıralı dize karşılaştırma tarafından sıralanır ([Ordinalıgnorecase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) rota şablonu.</span><span class="sxs-lookup"><span data-stu-id="56f6a-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="56f6a-280">Aşağıda bir örnek vardır.</span><span class="sxs-lookup"><span data-stu-id="56f6a-280">Here is an example.</span></span> <span data-ttu-id="56f6a-281">Aşağıdaki denetleyicisiyle tanımladığınız varsayın:</span><span class="sxs-lookup"><span data-stu-id="56f6a-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="56f6a-282">Bu yolları şu şekilde sıralanır.</span><span class="sxs-lookup"><span data-stu-id="56f6a-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="56f6a-283">Sipariş/Ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="56f6a-283">orders/details</span></span>
2. <span data-ttu-id="56f6a-284">siparişleri / {id}</span><span class="sxs-lookup"><span data-stu-id="56f6a-284">orders/{id}</span></span>
3. <span data-ttu-id="56f6a-285">siparişleri / {customerName}</span><span class="sxs-lookup"><span data-stu-id="56f6a-285">orders/{customerName}</span></span>
4. <span data-ttu-id="56f6a-286">siparişleri / {\*tarih}</span><span class="sxs-lookup"><span data-stu-id="56f6a-286">orders/{\*date}</span></span>
5. <span data-ttu-id="56f6a-287">siparişleri / beklemede</span><span class="sxs-lookup"><span data-stu-id="56f6a-287">orders/pending</span></span>

<span data-ttu-id="56f6a-288">"Ayrıntılar" değişmez değer kesimi ise ve "{id}" önce görünür ancak "bekliyor" son görünür, çünkü bildirimi **RouteOrder** 1 bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="56f6a-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **RouteOrder** property is 1.</span></span> <span data-ttu-id="56f6a-289">(Bu örnekte "Ayrıntılar" adlı hiçbir müşterinin var. varsayılır veya "bekliyor".</span><span class="sxs-lookup"><span data-stu-id="56f6a-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="56f6a-290">Belirsiz yollar önlemek genel olarak, deneyin.</span><span class="sxs-lookup"><span data-stu-id="56f6a-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="56f6a-291">Bu örnekte, daha iyi bir yol şablonu için `GetByCustomer` olan "müşteriler / {customerName}")</span><span class="sxs-lookup"><span data-stu-id="56f6a-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
