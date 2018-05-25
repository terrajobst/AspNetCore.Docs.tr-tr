---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Yönlendirme ve eylem seçimi ASP.NET Web API'de | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 997582263bd48590b74434ee0ffc6be928fa1e08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="d735a-102">Yönlendirme ve eylem seçimi ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="d735a-102">Routing and Action Selection in ASP.NET Web API</span></span>
====================
<span data-ttu-id="d735a-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d735a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="d735a-104">Bu makalede, nasıl ASP.NET Web API HTTP isteğinden denetleyicisinde belirli bir eylem yönlendirir açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d735a-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="d735a-105">Yönlendirme ilişkin üst düzey genel bakış için bkz: [ASP.NET Web API'de yönlendirme](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="d735a-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>


<span data-ttu-id="d735a-106">Bu makalede aşağıdaki adresten yönlendirme işlemi ayrıntıları arar.</span><span class="sxs-lookup"><span data-stu-id="d735a-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="d735a-107">Bir Web API projesi oluşturursanız ve beklediğiniz gibi bazı istekleri almadım Bul yönlendirilen umarız bu makalede yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="d735a-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="d735a-108">Yönlendirme üç ana aşamadan oluşur:</span><span class="sxs-lookup"><span data-stu-id="d735a-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="d735a-109">Eşleşen bir rota şablonu için URI.</span><span class="sxs-lookup"><span data-stu-id="d735a-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="d735a-110">Bir denetleyici seçme.</span><span class="sxs-lookup"><span data-stu-id="d735a-110">Selecting a controller.</span></span>
3. <span data-ttu-id="d735a-111">Bir eylem seçin.</span><span class="sxs-lookup"><span data-stu-id="d735a-111">Selecting an action.</span></span>

<span data-ttu-id="d735a-112">İşlem bazı bölümleri, kendi özel davranışlar ile değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d735a-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="d735a-113">Bu makalede, t varsayılan davranışı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d735a-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="d735a-114">Sonunda, ı burada davranışını özelleştirebilirsiniz yerler unutmayın.</span><span class="sxs-lookup"><span data-stu-id="d735a-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="d735a-115">Rota şablonlarının</span><span class="sxs-lookup"><span data-stu-id="d735a-115">Route Templates</span></span>

<span data-ttu-id="d735a-116">Bir rota şablonu için bir URI yolu benzer, ancak yer tutucu değerlerini, süslü ayraçlar ile belirtilen olabilir:</span><span class="sxs-lookup"><span data-stu-id="d735a-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="d735a-117">Bir rota oluşturduğunuzda bazılarını veya tümünü yer tutucuları için varsayılan değerler sağlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d735a-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="d735a-118">Bir URI segmenti bir yer tutucu nasıl eşleştirebilirsiniz kısıtlamak kısıtlamaları de sağlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d735a-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="d735a-119">Framework şablona URI'si yoldaki kesimlerin eşleştirmeyi dener.</span><span class="sxs-lookup"><span data-stu-id="d735a-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="d735a-120">Değişmez değerler şablondaki tam olarak eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="d735a-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="d735a-121">Bir yer tutucu kısıtlamaları belirtmediğiniz sürece herhangi bir değer ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="d735a-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="d735a-122">Framework'te diğer ana bilgisayar adı veya sorgu parametreleri gibi URI'SİNİN bölümlerini eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="d735a-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="d735a-123">Framework URI eşleşen rota tablosunda ilk yolu seçer.</span><span class="sxs-lookup"><span data-stu-id="d735a-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="d735a-124">İki özel yer tutucuları vardır: "{controller}" ve "{action}".</span><span class="sxs-lookup"><span data-stu-id="d735a-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="d735a-125">"{controller}" denetleyicinin adı sağlar.</span><span class="sxs-lookup"><span data-stu-id="d735a-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="d735a-126">"{action}" eylemin adı sağlar.</span><span class="sxs-lookup"><span data-stu-id="d735a-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="d735a-127">Web API'si "{action}" atlayın normal kuralıdır.</span><span class="sxs-lookup"><span data-stu-id="d735a-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="d735a-128">Varsayılanları</span><span class="sxs-lookup"><span data-stu-id="d735a-128">Defaults</span></span>

<span data-ttu-id="d735a-129">Rota Varsayılanları sağlarsanız, bu kesimleri eksik bir URI eşleşir.</span><span class="sxs-lookup"><span data-stu-id="d735a-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="d735a-130">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="d735a-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="d735a-131">URI "`http://localhost/api/products`" Bu yolla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="d735a-131">The URI "`http://localhost/api/products`" matches this route.</span></span> <span data-ttu-id="d735a-132">"{Kategori}" kesimine, varsayılan değer "tümü" atanır.</span><span class="sxs-lookup"><span data-stu-id="d735a-132">The "{category}" segment is assigned the default value "all".</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="d735a-133">Rota sözlüğünü</span><span class="sxs-lookup"><span data-stu-id="d735a-133">Route Dictionary</span></span>

<span data-ttu-id="d735a-134">Çerçeve bir URI yönelik bir eşleşme bulursa, için her bir yer tutucu değerini içeren bir sözlük oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d735a-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="d735a-135">Anahtarlar süslü ayraçlar hariç yer tutucu adlarıdır.</span><span class="sxs-lookup"><span data-stu-id="d735a-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="d735a-136">Değerler URI yolu veya Varsayılanları alınır.</span><span class="sxs-lookup"><span data-stu-id="d735a-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="d735a-137">Sözlük depolanan **IHttpRouteData** nesnesi.</span><span class="sxs-lookup"><span data-stu-id="d735a-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="d735a-138">Bu rota eşleşen aşamasında, yalnızca diğer yer tutucuları gibi özel "{controller}" ve "{action}" yer tutucuları kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="d735a-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="d735a-139">Bunlar yalnızca sözlük diğer değerler ile depolanır.</span><span class="sxs-lookup"><span data-stu-id="d735a-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="d735a-140">Varsayılan özel değerine sahip **RouteParameter.Optional**.</span><span class="sxs-lookup"><span data-stu-id="d735a-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="d735a-141">Bir yer tutucu bu değer atanmış, değer rota sözlüğünü eklenmez.</span><span class="sxs-lookup"><span data-stu-id="d735a-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="d735a-142">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="d735a-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="d735a-143">URI yolu "API/ürünleri" rota sözlüğünü içerir:</span><span class="sxs-lookup"><span data-stu-id="d735a-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="d735a-144">controller: "products"</span><span class="sxs-lookup"><span data-stu-id="d735a-144">controller: "products"</span></span>
- <span data-ttu-id="d735a-145">Kategori: "tümü"</span><span class="sxs-lookup"><span data-stu-id="d735a-145">category: "all"</span></span>

<span data-ttu-id="d735a-146">"API/ürünler/toys/123 için", ancak rota sözlüğünü içerir:</span><span class="sxs-lookup"><span data-stu-id="d735a-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="d735a-147">controller: "products"</span><span class="sxs-lookup"><span data-stu-id="d735a-147">controller: "products"</span></span>
- <span data-ttu-id="d735a-148">Kategori: "toys"</span><span class="sxs-lookup"><span data-stu-id="d735a-148">category: "toys"</span></span>
- <span data-ttu-id="d735a-149">id: "123"</span><span class="sxs-lookup"><span data-stu-id="d735a-149">id: "123"</span></span>

<span data-ttu-id="d735a-150">Varsayılan rota şablonu herhangi bir yerde görünmez bir değer dahil edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d735a-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="d735a-151">Rota eşleşiyorsa, bu değeri sözlükte depolanır.</span><span class="sxs-lookup"><span data-stu-id="d735a-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="d735a-152">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="d735a-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="d735a-153">URI yolu "kök/api/8" ise, sözlük iki değerlerini içerir:</span><span class="sxs-lookup"><span data-stu-id="d735a-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="d735a-154">Denetleyici: "Müşteri"</span><span class="sxs-lookup"><span data-stu-id="d735a-154">controller: "customers"</span></span>
- <span data-ttu-id="d735a-155">id: "8"</span><span class="sxs-lookup"><span data-stu-id="d735a-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="d735a-156">Bir denetleyici seçme</span><span class="sxs-lookup"><span data-stu-id="d735a-156">Selecting a Controller</span></span>

<span data-ttu-id="d735a-157">Denetleyici seçimi tarafından işlenir **IHttpControllerSelector.SelectController** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d735a-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="d735a-158">Bu yöntem alır bir **HttpRequestMessage** örneği ve döndürür bir **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="d735a-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="d735a-159">Varsayılan uygulama tarafından sağlanan **DefaultHttpControllerSelector** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d735a-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="d735a-160">Bu sınıf, bir basit algoritması kullanır:</span><span class="sxs-lookup"><span data-stu-id="d735a-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="d735a-161">"Denetleyici" anahtarı için rota sözlüğünü bakın.</span><span class="sxs-lookup"><span data-stu-id="d735a-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="d735a-162">Bu anahtar değeri alın ve denetleyici türü adını almak için "denetleyici" dizesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d735a-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="d735a-163">Bu tür adı ile Web API denetleyicisi arayın.</span><span class="sxs-lookup"><span data-stu-id="d735a-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="d735a-164">Örneğin, rota sözlüğünü "denetleyici" anahtar-değer çifti içeriyorsa denetleyici türü "ProductsController" sonra "Ürünler" =.</span><span class="sxs-lookup"><span data-stu-id="d735a-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="d735a-165">Eşleşen bir türü veya birden çok eşleşme varsa framework istemciye bir hata döndürür.</span><span class="sxs-lookup"><span data-stu-id="d735a-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="d735a-166">Adım 3 ' için **DefaultHttpControllerSelector** kullanan **IHttpControllerTypeResolver** Web API denetleyicisi türleri listesini almak için arabirim.</span><span class="sxs-lookup"><span data-stu-id="d735a-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="d735a-167">Varsayılan uygulaması **IHttpControllerTypeResolver** tüm ortak sınıflar (a) uygulayan döndürür **IHttpController**, (b) değil soyut ve (c) içinde "denetleyici" ile biten bir ada sahip olduğundan.</span><span class="sxs-lookup"><span data-stu-id="d735a-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="d735a-168">Eylem Seçimi</span><span class="sxs-lookup"><span data-stu-id="d735a-168">Action Selection</span></span>

<span data-ttu-id="d735a-169">Denetleyici seçtikten sonra framework eylemi çağırarak seçer **IHttpActionSelector.SelectAction** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d735a-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="d735a-170">Bu yöntem alır bir **HttpControllerContext** ve döndüren bir **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="d735a-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="d735a-171">Varsayılan uygulama tarafından sağlanan **ApiControllerActionSelector** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d735a-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="d735a-172">Bir eylem seçmek için şu görünür:</span><span class="sxs-lookup"><span data-stu-id="d735a-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="d735a-173">İsteğin HTTP yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d735a-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="d735a-174">"{Action}" yer tutucu rota şablonuyla varsa.</span><span class="sxs-lookup"><span data-stu-id="d735a-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="d735a-175">Denetleyici eylemleri parametreleri.</span><span class="sxs-lookup"><span data-stu-id="d735a-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="d735a-176">Seçimi algoritması aramadan önce biz denetleyici eylemleri hakkında bazı şeyleri anlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d735a-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="d735a-177">**Hangi yöntemlerin denetleyicisinde "Eylemler" olarak kabul edilir?**</span><span class="sxs-lookup"><span data-stu-id="d735a-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="d735a-178">Bir eylem seçerken, framework ortak örnek yöntemleri denetleyicisinde yalnızca arar.</span><span class="sxs-lookup"><span data-stu-id="d735a-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="d735a-179">Ayrıca, dışlar ["özel adı"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) (Oluşturucular, olaylar, işlecin ve benzeri) ve devralınan yöntemleri **ApiController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d735a-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="d735a-180">**HTTP yöntemleri.**</span><span class="sxs-lookup"><span data-stu-id="d735a-180">**HTTP Methods.**</span></span> <span data-ttu-id="d735a-181">Framework yalnızca aşağıdaki gibi belirlenen isteğin HTTP yöntemi ile eşleşen Eylemler seçti:</span><span class="sxs-lookup"><span data-stu-id="d735a-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="d735a-182">HTTP yöntemi ile bir öznitelik belirtebilirsiniz: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **HttpOptions**, **HttpPatch**, **HttpPost**, veya **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="d735a-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="d735a-183">Denetleyici yönteminin adı "Get", "Post", "Put", "Delete", "Head", "Seçenekleri" veya "Patch" ile başlıyorsa, aksi takdirde, sonra kurala göre eylemi, HTTP yöntemini destekler.</span><span class="sxs-lookup"><span data-stu-id="d735a-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="d735a-184">Yukarıdakilerin hiçbiri, POST yöntemini destekler.</span><span class="sxs-lookup"><span data-stu-id="d735a-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="d735a-185">**Parametre bağlamaları.**</span><span class="sxs-lookup"><span data-stu-id="d735a-185">**Parameter Bindings.**</span></span> <span data-ttu-id="d735a-186">Parametre bağlaması, bir parametre için bir değer Web API'sini nasıl oluşturduğunu ' dir.</span><span class="sxs-lookup"><span data-stu-id="d735a-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="d735a-187">Parametre bağlama için varsayılan kuralı şöyledir:</span><span class="sxs-lookup"><span data-stu-id="d735a-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="d735a-188">Basit türler URI'den alınır.</span><span class="sxs-lookup"><span data-stu-id="d735a-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="d735a-189">Karmaşık türler isteği gövdesinden alınır.</span><span class="sxs-lookup"><span data-stu-id="d735a-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="d735a-190">Basit türler tüm [.NET Framework ilkel türler](https://msdn.microsoft.com/library/system.type.isprimitive), artı **DateTime**, **ondalık**, **GUID**, **dize** , ve **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="d735a-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="d735a-191">Her bir eylem için en fazla bir parametre istek gövdesi okuyabilir.</span><span class="sxs-lookup"><span data-stu-id="d735a-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="d735a-192">Varsayılan bağlama kurallarını geçersiz kılmasına mümkündür.</span><span class="sxs-lookup"><span data-stu-id="d735a-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="d735a-193">Bkz: [Webapı parametre bağlaması başlık altında](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="d735a-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>


<span data-ttu-id="d735a-194">Bu arka plan, eylem seçimi algoritma aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d735a-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="d735a-195">HTTP istek yöntemi eşleşen denetleyicisinde tüm eylemlerin bir listesini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d735a-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="d735a-196">Rota sözlüğünü bir "eylem" giriş varsa, adı bu değerle eşleşmiyor Eylemler kaldırın.</span><span class="sxs-lookup"><span data-stu-id="d735a-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="d735a-197">URI için eylem parametrelerini aşağıdaki gibi eşleşecek şekilde deneyin:</span><span class="sxs-lookup"><span data-stu-id="d735a-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="d735a-198">Her bir eylem için burada bağlama parametresi URI'den alır basit bir tür parametreler listesini alın.</span><span class="sxs-lookup"><span data-stu-id="d735a-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="d735a-199">İsteğe bağlı parametreler hariç tutun.</span><span class="sxs-lookup"><span data-stu-id="d735a-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="d735a-200">Bir eşleşme her parametre adı için rota sözlüğünü veya URI sorgu dizesinde bulmak için bu listeden deneyin.</span><span class="sxs-lookup"><span data-stu-id="d735a-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="d735a-201">Eşleşme büyük küçük harfe duyarlı ve parametre sırasını güvenmeyin.</span><span class="sxs-lookup"><span data-stu-id="d735a-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="d735a-202">Listedeki her parametre URI'de bir eşleşme bulunduğu bir eylem seçin.</span><span class="sxs-lookup"><span data-stu-id="d735a-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="d735a-203">Daha o eylemi Bu ölçütleri karşılıyorsa, çoğu parametre eşleşmeleri ile seçin.</span><span class="sxs-lookup"><span data-stu-id="d735a-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="d735a-204">Eylemler ile Yoksay **[NonAction]** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="d735a-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="d735a-205">#3. adım büyük olasılıkla en karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="d735a-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="d735a-206">Bir parametre değeri ya da URI, istek gövdesinden veya özel bağlama alabilirsiniz temel olur.</span><span class="sxs-lookup"><span data-stu-id="d735a-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="d735a-207">URI'den gelen parametreleri için URI gerçekte yolun (aracılığıyla rota sözlüğünü) veya bir sorgu dizesi Bu parametre için bir değer içerdiğinden emin olmak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="d735a-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="d735a-208">Örneğin, aşağıdaki eylemi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="d735a-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="d735a-209">*Kimliği* parametreyi bağlar URI'si.</span><span class="sxs-lookup"><span data-stu-id="d735a-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="d735a-210">Bu nedenle, bu eylem yalnızca "id", rota sözlüğünü veya sorgu dizesi için bir değer içeren bir URI eşleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d735a-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="d735a-211">İsteğe bağlı oldukları için isteğe bağlı parametreler bir özel durumdur.</span><span class="sxs-lookup"><span data-stu-id="d735a-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="d735a-212">Bağlama URI'den değeri alınamıyor isteğe bağlı bir parametre için Tamam iyisidir.</span><span class="sxs-lookup"><span data-stu-id="d735a-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="d735a-213">Karmaşık türler için farklı bir neden bir özel durumdur.</span><span class="sxs-lookup"><span data-stu-id="d735a-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="d735a-214">Karmaşık bir tür, yalnızca URI üzerinden özel bağlama bağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d735a-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="d735a-215">Ancak bu durumda, framework önceden parametresi için belirli bir URI bağlayın olup olmadığını bilemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="d735a-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="d735a-216">Öğrenmek için bir bağlama çağrılacak gerekir.</span><span class="sxs-lookup"><span data-stu-id="d735a-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="d735a-217">Hiçbir bağlama çağırmadan önce statik açıklamasından bir eylem seçmek için seçim algoritması belirtilir.</span><span class="sxs-lookup"><span data-stu-id="d735a-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="d735a-218">Bu nedenle, karmaşık türler eşleşen algoritmadan hariç tutulur.</span><span class="sxs-lookup"><span data-stu-id="d735a-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="d735a-219">Tüm parametre bağlamaları eylem seçildikten sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d735a-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="d735a-220">Özeti:</span><span class="sxs-lookup"><span data-stu-id="d735a-220">Summary:</span></span>

- <span data-ttu-id="d735a-221">Eylem isteğin HTTP yöntemi ile eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="d735a-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="d735a-222">Eylem adı, varsa rota sözlüğünü "eylem" girdisinde eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="d735a-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="d735a-223">Parametre URI'den alınmışsa her eylem parametresi için sonra parametre adı için rota sözlüğünü veya URI sorgu dizesinde bulunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d735a-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="d735a-224">(İsteğe bağlı parametreler ve karmaşık türler parametrelerle hariç tutulur.)</span><span class="sxs-lookup"><span data-stu-id="d735a-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="d735a-225">En çok parametrelerden biri eşleşecek şekilde deneyin.</span><span class="sxs-lookup"><span data-stu-id="d735a-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="d735a-226">En iyi eşleşen hiçbir parametre bir yöntem olabilir.</span><span class="sxs-lookup"><span data-stu-id="d735a-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="d735a-227">Genişletilmiş örnek</span><span class="sxs-lookup"><span data-stu-id="d735a-227">Extended Example</span></span>

<span data-ttu-id="d735a-228">Yollar:</span><span class="sxs-lookup"><span data-stu-id="d735a-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="d735a-229">Denetleyici:</span><span class="sxs-lookup"><span data-stu-id="d735a-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="d735a-230">HTTP isteği:</span><span class="sxs-lookup"><span data-stu-id="d735a-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="d735a-231">Eşleşen yol</span><span class="sxs-lookup"><span data-stu-id="d735a-231">Route Matching</span></span>

<span data-ttu-id="d735a-232">URI "DefaultApi" adlı rota eşleşir.</span><span class="sxs-lookup"><span data-stu-id="d735a-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="d735a-233">Rota sözlüğünü aşağıdaki girdileri içerir:</span><span class="sxs-lookup"><span data-stu-id="d735a-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="d735a-234">controller: "products"</span><span class="sxs-lookup"><span data-stu-id="d735a-234">controller: "products"</span></span>
- <span data-ttu-id="d735a-235">id: "1"</span><span class="sxs-lookup"><span data-stu-id="d735a-235">id: "1"</span></span>

<span data-ttu-id="d735a-236">Rota sözlüğünü sorgu dizesi parametreleri, "Sürüm" ve "Ayrıntılar" içermiyor, ancak bunlar hala eylem seçimi sırasında olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="d735a-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="d735a-237">Denetleyici seçimi</span><span class="sxs-lookup"><span data-stu-id="d735a-237">Controller Selection</span></span>

<span data-ttu-id="d735a-238">Rota sözlükte "denetleyici" girdisinden denetleyicisi türüdür `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="d735a-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="d735a-239">Eylem Seçimi</span><span class="sxs-lookup"><span data-stu-id="d735a-239">Action Selection</span></span>

<span data-ttu-id="d735a-240">HTTP GET isteği isteğidir.</span><span class="sxs-lookup"><span data-stu-id="d735a-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="d735a-241">GET destek denetleyici eylemleri `GetAll`, `GetById`, ve `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="d735a-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="d735a-242">Biz eylem adı ile eşleşmesi gerek kalmaması için rota sözlüğünü "eylem" için bir giriş içermiyor.</span><span class="sxs-lookup"><span data-stu-id="d735a-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="d735a-243">Ardından, biz eylemleri için parametre adlarını eşleştirmek yalnızca GET eylemlerini bakmayı deneyin.</span><span class="sxs-lookup"><span data-stu-id="d735a-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="d735a-244">Eylem</span><span class="sxs-lookup"><span data-stu-id="d735a-244">Action</span></span> | <span data-ttu-id="d735a-245">Eşleştirilecek parametreleri</span><span class="sxs-lookup"><span data-stu-id="d735a-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="d735a-246">yok</span><span class="sxs-lookup"><span data-stu-id="d735a-246">none</span></span> |
| `GetById` | <span data-ttu-id="d735a-247">"id"</span><span class="sxs-lookup"><span data-stu-id="d735a-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="d735a-248">"name"</span><span class="sxs-lookup"><span data-stu-id="d735a-248">"name"</span></span> |

<span data-ttu-id="d735a-249">Dikkat *sürüm* parametresinin `GetById` isteğe bağlı bir parametre olduğundan, olarak kabul edilmez.</span><span class="sxs-lookup"><span data-stu-id="d735a-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="d735a-250">`GetAll` Yöntemi trivially ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="d735a-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="d735a-251">`GetById` Yöntemi ayrıca eşleşir, rota sözlüğünü "id" içerdiğinden.</span><span class="sxs-lookup"><span data-stu-id="d735a-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="d735a-252">`FindProductsByName` Yöntemi eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="d735a-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="d735a-253">`GetById` Yöntemi WINS için hiçbir parametre karşı bir parametre eşleştiğinden `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="d735a-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="d735a-254">Yöntemi, aşağıdaki parametre değerleri ile çağrılır:</span><span class="sxs-lookup"><span data-stu-id="d735a-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="d735a-255">*id* = 1</span><span class="sxs-lookup"><span data-stu-id="d735a-255">*id* = 1</span></span>
- <span data-ttu-id="d735a-256">*Sürüm* 1.5 =</span><span class="sxs-lookup"><span data-stu-id="d735a-256">*version* = 1.5</span></span>

<span data-ttu-id="d735a-257">Rağmen dikkat *sürüm* kullanılmadığı seçimi algoritması URI sorgu dizesi parametresinin değeri gelir.</span><span class="sxs-lookup"><span data-stu-id="d735a-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="d735a-258">Uzantı noktaları</span><span class="sxs-lookup"><span data-stu-id="d735a-258">Extension Points</span></span>

<span data-ttu-id="d735a-259">Web API yönlendirme işlemi bazı bölümleri için uzantı noktaları sağlar.</span><span class="sxs-lookup"><span data-stu-id="d735a-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="d735a-260">Arabirim</span><span class="sxs-lookup"><span data-stu-id="d735a-260">Interface</span></span> | <span data-ttu-id="d735a-261">Açıklama</span><span class="sxs-lookup"><span data-stu-id="d735a-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d735a-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="d735a-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="d735a-263">Denetleyiciyi seçer.</span><span class="sxs-lookup"><span data-stu-id="d735a-263">Selects the controller.</span></span> |
| <span data-ttu-id="d735a-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="d735a-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="d735a-265">Denetleyici türlerini listesini alır.</span><span class="sxs-lookup"><span data-stu-id="d735a-265">Gets the list of controller types.</span></span> <span data-ttu-id="d735a-266">**DefaultHttpControllerSelector** denetleyici türü bu listeden seçer.</span><span class="sxs-lookup"><span data-stu-id="d735a-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="d735a-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="d735a-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="d735a-268">Proje derleme listesini alır.</span><span class="sxs-lookup"><span data-stu-id="d735a-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="d735a-269">**IHttpControllerTypeResolver** arabirimi denetleyici türlerini bulmak için bu listeyi kullanır.</span><span class="sxs-lookup"><span data-stu-id="d735a-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="d735a-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="d735a-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="d735a-271">Yeni denetleyici örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d735a-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="d735a-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="d735a-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="d735a-273">Eylemi seçer.</span><span class="sxs-lookup"><span data-stu-id="d735a-273">Selects the action.</span></span> |
| <span data-ttu-id="d735a-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="d735a-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="d735a-275">Eylemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="d735a-275">Invokes the action.</span></span> |

<span data-ttu-id="d735a-276">Herhangi bir bu arabirimleri için kendi uygulama sunmak amacıyla kullanmak **Hizmetleri** koleksiyonunda **HttpConfiguration** nesnesi:</span><span class="sxs-lookup"><span data-stu-id="d735a-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
