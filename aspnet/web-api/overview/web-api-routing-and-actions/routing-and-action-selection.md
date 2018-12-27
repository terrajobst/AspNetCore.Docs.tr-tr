---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Yönlendirme ve eylem seçimi ASP.NET Web API'de | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 12/14/2018
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: ce54181996376cb5dde3b91c10c16f33b3c6a570
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425178"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="5404e-102">Yönlendirme ve eylem seçimi ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="5404e-102">Routing and Action Selection in ASP.NET Web API</span></span>
====================
<span data-ttu-id="5404e-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5404e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5404e-104">Bu makalede, bir HTTP isteği ASP.NET Web API denetleyicisine belirli bir eylemi için nasıl yönlendirdiğini açıklanır.</span><span class="sxs-lookup"><span data-stu-id="5404e-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="5404e-105">Yönlendirme ilişkin üst düzey genel bakış için bkz: [ASP.NET Web API'de yönlendirme](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="5404e-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>


<span data-ttu-id="5404e-106">Bu makalede, yönlendirme işleminin ayrıntılarını da ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5404e-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="5404e-107">Umarım bu makalede bir Web API projesi oluşturursanız ve beklediğiniz gibi bazı istekler elde etmezsiniz Bul yönlendirilmesini yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="5404e-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="5404e-108">Yönlendirme üç ana aşama içerir:</span><span class="sxs-lookup"><span data-stu-id="5404e-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="5404e-109">Bir rota şablonu için URI eşleşen.</span><span class="sxs-lookup"><span data-stu-id="5404e-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="5404e-110">Bir denetleyici seçin.</span><span class="sxs-lookup"><span data-stu-id="5404e-110">Selecting a controller.</span></span>
3. <span data-ttu-id="5404e-111">Bir eylem seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5404e-111">Selecting an action.</span></span>

<span data-ttu-id="5404e-112">İşlemin bazı bölümleri, kendi özel davranışlarınızı ile değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5404e-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="5404e-113">Bu makalede, ben varsayılan davranışını açıklar.</span><span class="sxs-lookup"><span data-stu-id="5404e-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="5404e-114">Sonunda, ben burada davranışını özelleştirebilirsiniz yerleri unutmayın.</span><span class="sxs-lookup"><span data-stu-id="5404e-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="5404e-115">Rota şablonlarının</span><span class="sxs-lookup"><span data-stu-id="5404e-115">Route Templates</span></span>

<span data-ttu-id="5404e-116">Bir rota şablonu için bir URI yolu gibi görünür, ancak küme ayracı ile gösterilen, yer tutucu değerlerini sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="5404e-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="5404e-117">Bir rota oluşturduğunuzda, bazılarını veya tümünü yer tutucular için varsayılan değerler sağlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5404e-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="5404e-118">Ayrıca, bir URI segmenti yer tutucu nasıl eşleşebilir kısıtlama kısıtlamaları, sağlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5404e-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="5404e-119">Framework, şablon URI'si yoldaki kesimlerin eşleştirmeyi dener.</span><span class="sxs-lookup"><span data-stu-id="5404e-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="5404e-120">Şablondaki değişmez değerler tam olarak eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="5404e-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="5404e-121">Bir yer tutucu kısıtlamaları belirtmediğiniz sürece herhangi bir değer ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="5404e-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="5404e-122">Framework URI, ana bilgisayar adı veya sorgu parametreleri gibi diğer bölümleri eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="5404e-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="5404e-123">Framework ilk yönlendirme URI'si ile eşleşen rota tablosunda seçer.</span><span class="sxs-lookup"><span data-stu-id="5404e-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="5404e-124">İki özel yer tutucuları vardır: "{denetleyici}" ve "{action}".</span><span class="sxs-lookup"><span data-stu-id="5404e-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="5404e-125">"{denetleyici}" denetleyicinin adı sağlar.</span><span class="sxs-lookup"><span data-stu-id="5404e-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="5404e-126">"{action}" eyleminin adını sağlar.</span><span class="sxs-lookup"><span data-stu-id="5404e-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="5404e-127">Web API'de normal "{action}" atlamak için kuralıdır.</span><span class="sxs-lookup"><span data-stu-id="5404e-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="5404e-128">Varsayılanları</span><span class="sxs-lookup"><span data-stu-id="5404e-128">Defaults</span></span>

<span data-ttu-id="5404e-129">Rota Varsayılanları sağlarsanız, bu kesimleri eksik bir URI eşleşir.</span><span class="sxs-lookup"><span data-stu-id="5404e-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="5404e-130">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5404e-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="5404e-131">Bir URI'leri `http://localhost/api/products/all` ve `http://localhost/api/products` önceki rota ile eşleşmekte.</span><span class="sxs-lookup"><span data-stu-id="5404e-131">The URIs `http://localhost/api/products/all` and `http://localhost/api/products` match the preceding route.</span></span> <span data-ttu-id="5404e-132">İkinci uri'sindeki eksik `{category}` segment, varsayılan değer atanır `all`.</span><span class="sxs-lookup"><span data-stu-id="5404e-132">In the latter URI, the missing `{category}` segment is assigned the default value `all`.</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="5404e-133">Rota sözlüğünü</span><span class="sxs-lookup"><span data-stu-id="5404e-133">Route Dictionary</span></span>

<span data-ttu-id="5404e-134">Framework bir URI için bir eşleşme bulduğunda, için her bir yer tutucu değeri içeren bir sözlük oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5404e-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="5404e-135">Anahtarlar ve küme ayraçlarının dahil değil yer tutucu adlarıdır.</span><span class="sxs-lookup"><span data-stu-id="5404e-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="5404e-136">Değerler, URI yolu veya Varsayılanları alınır.</span><span class="sxs-lookup"><span data-stu-id="5404e-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="5404e-137">Sözlüğüne depolanan **IHttpRouteData** nesne.</span><span class="sxs-lookup"><span data-stu-id="5404e-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="5404e-138">Bu rota için eşleşen aşamasında, özel "{denetleyici}" ve "{action}" yer tutucuları yalnızca diğer yer tutucuları gibi kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="5404e-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="5404e-139">Bunlar yalnızca diğer değerlerle sözlükte depolanır.</span><span class="sxs-lookup"><span data-stu-id="5404e-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="5404e-140">Özel değeri, bir varsayılan olabilir **RouteParameter.Optional**.</span><span class="sxs-lookup"><span data-stu-id="5404e-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="5404e-141">Bu değer bir yer tutucu atandığını, değer için rota sözlüğünü eklenmez.</span><span class="sxs-lookup"><span data-stu-id="5404e-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="5404e-142">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5404e-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="5404e-143">URI yolu "API/ürünleri" rota sözlüğünü içerir:</span><span class="sxs-lookup"><span data-stu-id="5404e-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="5404e-144">Denetleyici: "Ürünler"</span><span class="sxs-lookup"><span data-stu-id="5404e-144">controller: "products"</span></span>
- <span data-ttu-id="5404e-145">Kategori: "tüm"</span><span class="sxs-lookup"><span data-stu-id="5404e-145">category: "all"</span></span>

<span data-ttu-id="5404e-146">"API/ürünler/toys/123 için", ancak rota sözlüğünü içerir:</span><span class="sxs-lookup"><span data-stu-id="5404e-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="5404e-147">Denetleyici: "Ürünler"</span><span class="sxs-lookup"><span data-stu-id="5404e-147">controller: "products"</span></span>
- <span data-ttu-id="5404e-148">Kategori: "toys"</span><span class="sxs-lookup"><span data-stu-id="5404e-148">category: "toys"</span></span>
- <span data-ttu-id="5404e-149">Kimliği: "123"</span><span class="sxs-lookup"><span data-stu-id="5404e-149">id: "123"</span></span>

<span data-ttu-id="5404e-150">Varsayılan değerleri, rota şablonu herhangi bir yerde görünmez bir değer de içerebilir.</span><span class="sxs-lookup"><span data-stu-id="5404e-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="5404e-151">Rotanın eşleşmesi durumunda bu değer sözlükte depolanır.</span><span class="sxs-lookup"><span data-stu-id="5404e-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="5404e-152">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5404e-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="5404e-153">URI yolu "kök/API/8" ise, sözlük iki değerlerini içerir:</span><span class="sxs-lookup"><span data-stu-id="5404e-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="5404e-154">Denetleyici: "Müşteri"</span><span class="sxs-lookup"><span data-stu-id="5404e-154">controller: "customers"</span></span>
- <span data-ttu-id="5404e-155">Kimliği: "8"</span><span class="sxs-lookup"><span data-stu-id="5404e-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="5404e-156">Bir denetleyici seçme</span><span class="sxs-lookup"><span data-stu-id="5404e-156">Selecting a Controller</span></span>

<span data-ttu-id="5404e-157">Denetleyici seçimi tarafından işlenir **IHttpControllerSelector.SelectController** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5404e-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="5404e-158">Bu yöntem bir **HttpRequestMessage** örneği ve döndürür bir **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="5404e-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="5404e-159">Varsayılan uygulama tarafından sağlanan **DefaultHttpControllerSelector** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5404e-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="5404e-160">Bu sınıf, bir basit algoritması kullanır:</span><span class="sxs-lookup"><span data-stu-id="5404e-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="5404e-161">"Controller" anahtarı için rota sözlüğünü bakın.</span><span class="sxs-lookup"><span data-stu-id="5404e-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="5404e-162">Bu anahtar için değeri alabilir ve denetleyici türü adını almak için "Controller" dizesini.</span><span class="sxs-lookup"><span data-stu-id="5404e-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="5404e-163">Bu tür adı ile bir Web API denetleyicisi arayın.</span><span class="sxs-lookup"><span data-stu-id="5404e-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="5404e-164">Denetleyici türü "ProductsController" ise, "Ürünler" Örneğin, rota sözlüğünü "controller" anahtar-değer çifti içeriyorsa =.</span><span class="sxs-lookup"><span data-stu-id="5404e-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="5404e-165">Eşleşen bir türü veya birden çok eşleşme varsa, framework istemciye bir hata döndürür.</span><span class="sxs-lookup"><span data-stu-id="5404e-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="5404e-166">Adım 3 ' ü **DefaultHttpControllerSelector** kullanan **IHttpControllerTypeResolver** Web API denetleyicisi türleri listesini almak için arabirim.</span><span class="sxs-lookup"><span data-stu-id="5404e-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="5404e-167">Varsayılan uygulaması **IHttpControllerTypeResolver** tüm ortak sınıfları döndürür (a) uygulayan **IHttpController**, (b) olan değil soyut ve (c) "Denetleyicisi" ile biten bir adı vardır.</span><span class="sxs-lookup"><span data-stu-id="5404e-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="5404e-168">Eylem Seçimi</span><span class="sxs-lookup"><span data-stu-id="5404e-168">Action Selection</span></span>

<span data-ttu-id="5404e-169">Denetleyici seçtikten sonra framework eylemi çağırarak seçer **IHttpActionSelector.SelectAction** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5404e-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="5404e-170">Bu yöntem bir **HttpControllerContext** ve döndüren bir **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="5404e-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="5404e-171">Varsayılan uygulama tarafından sağlanan **ApiControllerActionSelector** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5404e-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="5404e-172">Bir eylem seçmek için aşağıdaki konumda görünür:</span><span class="sxs-lookup"><span data-stu-id="5404e-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="5404e-173">İsteğin HTTP yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5404e-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="5404e-174">Varsa rota şablonu içinde "{action}" yer tutucu.</span><span class="sxs-lookup"><span data-stu-id="5404e-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="5404e-175">Denetleyici eylemleri parametreleri.</span><span class="sxs-lookup"><span data-stu-id="5404e-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="5404e-176">Seçimi algoritması aramadan önce denetleyici eylemleri hakkında bazı şeyleri anlamak ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="5404e-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="5404e-177">**Hangi yöntemlerin denetleyicisinde "Eylemler" olarak kabul edilir?**</span><span class="sxs-lookup"><span data-stu-id="5404e-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="5404e-178">Bir eylem seçerken framework denetleyicisinde yalnızca ortak örnek yöntem ele arar.</span><span class="sxs-lookup"><span data-stu-id="5404e-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="5404e-179">Ayrıca, dışlar ["özel adı"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) yöntemleri (Oluşturucular, olaylar, İşleç aşırı yüklemeleri ve diğerleri) ve devralınan yöntemleri **ApiController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5404e-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="5404e-180">**HTTP yöntemleri.**</span><span class="sxs-lookup"><span data-stu-id="5404e-180">**HTTP Methods.**</span></span> <span data-ttu-id="5404e-181">Framework, aşağıdaki gibi belirlenen isteğin HTTP yöntemi ile eşleşen eylemleri yalnızca seçer:</span><span class="sxs-lookup"><span data-stu-id="5404e-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="5404e-182">HTTP yöntemi ile bir öznitelik belirtebilirsiniz: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, veya **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="5404e-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="5404e-183">Denetleyici yöntemin adı "Get", "Post", "Put", "Delete", "Ana", "Seçenekler" veya "Düzeltme" ile başlıyorsa, aksi takdirde, sonra Kural gereği eylemi, HTTP yöntemini destekler.</span><span class="sxs-lookup"><span data-stu-id="5404e-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="5404e-184">Yukarıdakilerin hiçbiri, POST yöntemi destekler.</span><span class="sxs-lookup"><span data-stu-id="5404e-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="5404e-185">**Parametre bağlama.**</span><span class="sxs-lookup"><span data-stu-id="5404e-185">**Parameter Bindings.**</span></span> <span data-ttu-id="5404e-186">Parametre bağlaması, Web API'si, bir parametre için bir değer nasıl oluşturur? ' dir.</span><span class="sxs-lookup"><span data-stu-id="5404e-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="5404e-187">Parametre bağlaması için varsayılan kuralı şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="5404e-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="5404e-188">Basit türler URI'SİNDEN alınır.</span><span class="sxs-lookup"><span data-stu-id="5404e-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="5404e-189">Karmaşık türler, istek gövdesinden alınır.</span><span class="sxs-lookup"><span data-stu-id="5404e-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="5404e-190">Basit türler dahil tüm [.NET Framework ilkel türleri](https://msdn.microsoft.com/library/system.type.isprimitive), artı **DateTime**, **ondalık**, **GUID**, **dize** , ve **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="5404e-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="5404e-191">En fazla bir parametre, her eylem için istek gövdesi okuyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5404e-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="5404e-192">Varsayılan bağlama kurallarını geçersiz kılmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="5404e-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="5404e-193">Bkz: [başlık altında Webapı parametre bağlaması](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="5404e-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>


<span data-ttu-id="5404e-194">Bu bilgileri, eylem seçimi algoritma aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="5404e-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="5404e-195">HTTP istek yöntemi eşleşen denetleyicisinde tüm eylemlerin bir listesini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5404e-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="5404e-196">Rota sözlüğünü "action" girişi varsa, eylem adı, bu değeri eşleşmiyor kaldırın.</span><span class="sxs-lookup"><span data-stu-id="5404e-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="5404e-197">Eylem parametreleri için URI şu şekilde eşleşecek şekilde deneyin:</span><span class="sxs-lookup"><span data-stu-id="5404e-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="5404e-198">Her eylem için burada bağlama parametresi URI'SİNDEN alır bir basit türü parametreler listesini alın.</span><span class="sxs-lookup"><span data-stu-id="5404e-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="5404e-199">İsteğe bağlı parametreler hariç tutun.</span><span class="sxs-lookup"><span data-stu-id="5404e-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="5404e-200">Rota sözlüğünü veya URI sorgu dizesinde her parametre adı için bir eşleşme bulmak bu listeden deneyin.</span><span class="sxs-lookup"><span data-stu-id="5404e-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="5404e-201">Eşleşme büyük/küçük harfe duyarsızdır ve parametre sırasını bağımlı olmadan.</span><span class="sxs-lookup"><span data-stu-id="5404e-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="5404e-202">Her parametre listesinde URI'de bir eşleşme bulunduğu bir eylem seçin.</span><span class="sxs-lookup"><span data-stu-id="5404e-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="5404e-203">Daha fazla, bir eylem aşağıdaki ölçütleri karşılıyorsa, çoğu parametresi eşleşme ile seçin.</span><span class="sxs-lookup"><span data-stu-id="5404e-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="5404e-204">Eylemler Yoksay **[NonAction]** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5404e-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="5404e-205">#3. adım büyük olasılıkla en karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="5404e-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="5404e-206">Bir parametre değeri ya da urı'sinden, istek gövdesinden veya özel bir bağlama alabilirsiniz temel olur.</span><span class="sxs-lookup"><span data-stu-id="5404e-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="5404e-207">URİ'den gelen parametre URI gerçekten yolu (aracılığıyla rota sözlüğünü) veya sorgu dizesinde, parametre için bir değer içerdiğinden emin olmak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="5404e-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="5404e-208">Örneğin, aşağıdaki eylemi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5404e-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="5404e-209">*Kimliği* URI parametreyi bağlar.</span><span class="sxs-lookup"><span data-stu-id="5404e-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="5404e-210">Bu nedenle, bu eylem yalnızca "id", rota sözlüğünü veya sorgu dizesi için bir değer içeren bir URI ile eşleşebilir.</span><span class="sxs-lookup"><span data-stu-id="5404e-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="5404e-211">İsteğe bağlı oldukları için isteğe bağlı bir özel durum parametrelerdir.</span><span class="sxs-lookup"><span data-stu-id="5404e-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="5404e-212">Bağlama değeri URI'SİNDEN alınamıyor, isteğe bağlı bir parametre için Tamam olduğunu.</span><span class="sxs-lookup"><span data-stu-id="5404e-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="5404e-213">Karmaşık türler, farklı bir neden için bir özel durumdur.</span><span class="sxs-lookup"><span data-stu-id="5404e-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="5404e-214">Karmaşık bir tür, yalnızca üzerinden özel bağlama için URI bağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5404e-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="5404e-215">Ancak bu durumda, framework önceden parametresi için belirli bir URI bağlayın olup olmadığını bilemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="5404e-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="5404e-216">Öğrenmek için bu bağlama çağırmasına gerekir.</span><span class="sxs-lookup"><span data-stu-id="5404e-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="5404e-217">Seçimi algoritması amacı herhangi bir bağlama çağırmadan önce statik açıklamasından eylem seçmektir.</span><span class="sxs-lookup"><span data-stu-id="5404e-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="5404e-218">Bu nedenle, eşleştirme algoritmasını kullanarak karmaşık türler hariç tutulur.</span><span class="sxs-lookup"><span data-stu-id="5404e-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="5404e-219">Tüm parametre bağlamaları eylem seçildikten sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5404e-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="5404e-220">Özet:</span><span class="sxs-lookup"><span data-stu-id="5404e-220">Summary:</span></span>

- <span data-ttu-id="5404e-221">Eylem, isteğin HTTP yöntemi ile eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="5404e-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="5404e-222">Eylem adı için rota sözlüğünü "action" girişi varsa eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="5404e-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="5404e-223">Parametresi URI'den alınmışsa eylemin her parametre için sonra parametre adı için rota sözlüğünü veya URI sorgu dizesinde bulunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5404e-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="5404e-224">(İsteğe bağlı parametreler ve karmaşık türler parametrelerle bırakılır.)</span><span class="sxs-lookup"><span data-stu-id="5404e-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="5404e-225">Parametre sayısı en çok eşleşen deneyin.</span><span class="sxs-lookup"><span data-stu-id="5404e-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="5404e-226">En iyi eşleşmeyi parametresiz bir yöntem olabilir.</span><span class="sxs-lookup"><span data-stu-id="5404e-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="5404e-227">Genişletilmiş örneği</span><span class="sxs-lookup"><span data-stu-id="5404e-227">Extended Example</span></span>

<span data-ttu-id="5404e-228">Yollar:</span><span class="sxs-lookup"><span data-stu-id="5404e-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="5404e-229">Denetleyici:</span><span class="sxs-lookup"><span data-stu-id="5404e-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="5404e-230">HTTP isteği:</span><span class="sxs-lookup"><span data-stu-id="5404e-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="5404e-231">Eşleşen yol</span><span class="sxs-lookup"><span data-stu-id="5404e-231">Route Matching</span></span>

<span data-ttu-id="5404e-232">URI "DefaultApi" adlı rota eşleşir.</span><span class="sxs-lookup"><span data-stu-id="5404e-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="5404e-233">Rota sözlüğünü aşağıdaki girdileri içerir:</span><span class="sxs-lookup"><span data-stu-id="5404e-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="5404e-234">Denetleyici: "Ürünler"</span><span class="sxs-lookup"><span data-stu-id="5404e-234">controller: "products"</span></span>
- <span data-ttu-id="5404e-235">Kimliği: "1"</span><span class="sxs-lookup"><span data-stu-id="5404e-235">id: "1"</span></span>

<span data-ttu-id="5404e-236">Sorgu dizesi parametreleri, "Sürüm" ve "details" rota sözlüğünü içermiyor, ancak bunlar yine de sırasında eylem seçimi kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="5404e-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="5404e-237">Denetleyici seçimi</span><span class="sxs-lookup"><span data-stu-id="5404e-237">Controller Selection</span></span>

<span data-ttu-id="5404e-238">Rota sözlüğünde "controller" girdisinden denetleyicisi türüdür `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="5404e-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="5404e-239">Eylem Seçimi</span><span class="sxs-lookup"><span data-stu-id="5404e-239">Action Selection</span></span>

<span data-ttu-id="5404e-240">Bir GET isteği HTTP isteğidir.</span><span class="sxs-lookup"><span data-stu-id="5404e-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="5404e-241">GET işlemini denetleyici eylemleri `GetAll`, `GetById`, ve `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="5404e-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="5404e-242">Biz eylem adıyla eşleşmesi gerekmez rota sözlüğünü "action" için bir giriş içermiyor.</span><span class="sxs-lookup"><span data-stu-id="5404e-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="5404e-243">Ardından, biz eylemleri için parametre adlarını eşleştirmek yalnızca GET eylemlerini bakarak deneyin.</span><span class="sxs-lookup"><span data-stu-id="5404e-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="5404e-244">Eylem</span><span class="sxs-lookup"><span data-stu-id="5404e-244">Action</span></span> | <span data-ttu-id="5404e-245">Eşleştirme parametreleri</span><span class="sxs-lookup"><span data-stu-id="5404e-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="5404e-246">yok</span><span class="sxs-lookup"><span data-stu-id="5404e-246">none</span></span> |
| `GetById` | <span data-ttu-id="5404e-247">"id"</span><span class="sxs-lookup"><span data-stu-id="5404e-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="5404e-248">"name"</span><span class="sxs-lookup"><span data-stu-id="5404e-248">"name"</span></span> |

<span data-ttu-id="5404e-249">Dikkat *sürüm* parametresinin `GetById` isteğe bağlı parametresi olduğundan, olarak kabul edilmez.</span><span class="sxs-lookup"><span data-stu-id="5404e-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="5404e-250">`GetAll` Yöntemi artık önemsiz olarak eşleşir.</span><span class="sxs-lookup"><span data-stu-id="5404e-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="5404e-251">`GetById` Yöntemi ayrıca eşleşen, çünkü "id" rota sözlüğünü içeriyor.</span><span class="sxs-lookup"><span data-stu-id="5404e-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="5404e-252">`FindProductsByName` Yöntemi eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="5404e-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="5404e-253">`GetById` Yöntemi WINS parametresi yerine bir parametre eşleştiğinden `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="5404e-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="5404e-254">Yöntemi, aşağıdaki parametre değerleriniz ile çağrılır:</span><span class="sxs-lookup"><span data-stu-id="5404e-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="5404e-255">*Kimliği* = 1</span><span class="sxs-lookup"><span data-stu-id="5404e-255">*id* = 1</span></span>
- <span data-ttu-id="5404e-256">*Sürüm* 1.5 =</span><span class="sxs-lookup"><span data-stu-id="5404e-256">*version* = 1.5</span></span>

<span data-ttu-id="5404e-257">Rağmen dikkat *sürüm* kullanılmadığı seçimi algoritması URI sorgu dizesi parametresinin değeri gelir.</span><span class="sxs-lookup"><span data-stu-id="5404e-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="5404e-258">Uzantı noktaları</span><span class="sxs-lookup"><span data-stu-id="5404e-258">Extension Points</span></span>

<span data-ttu-id="5404e-259">Web API'si, bazı bölümlerini yönlendirme işlemi için uzantı noktaları sağlar.</span><span class="sxs-lookup"><span data-stu-id="5404e-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="5404e-260">Arabirim</span><span class="sxs-lookup"><span data-stu-id="5404e-260">Interface</span></span> | <span data-ttu-id="5404e-261">Açıklama</span><span class="sxs-lookup"><span data-stu-id="5404e-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5404e-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="5404e-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="5404e-263">Denetleyiciyi seçer.</span><span class="sxs-lookup"><span data-stu-id="5404e-263">Selects the controller.</span></span> |
| <span data-ttu-id="5404e-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="5404e-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="5404e-265">Denetleyici türleri listesini alır.</span><span class="sxs-lookup"><span data-stu-id="5404e-265">Gets the list of controller types.</span></span> <span data-ttu-id="5404e-266">**DefaultHttpControllerSelector** denetleyici türü bu listeden seçer.</span><span class="sxs-lookup"><span data-stu-id="5404e-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="5404e-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="5404e-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="5404e-268">Proje derleme listesini alır.</span><span class="sxs-lookup"><span data-stu-id="5404e-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="5404e-269">**IHttpControllerTypeResolver** arabirimi denetleyici türlerini bulmak için bu listeyi kullanır.</span><span class="sxs-lookup"><span data-stu-id="5404e-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="5404e-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="5404e-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="5404e-271">Yeni denetleyici örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5404e-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="5404e-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="5404e-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="5404e-273">Eylemi seçer.</span><span class="sxs-lookup"><span data-stu-id="5404e-273">Selects the action.</span></span> |
| <span data-ttu-id="5404e-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="5404e-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="5404e-275">Eylemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="5404e-275">Invokes the action.</span></span> |

<span data-ttu-id="5404e-276">Şu arabirimlerin herhangi birinde için kendi uygulamasını sağlamak için kullanın **Hizmetleri** koleksiyonunda **HttpConfiguration** nesnesi:</span><span class="sxs-lookup"><span data-stu-id="5404e-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
