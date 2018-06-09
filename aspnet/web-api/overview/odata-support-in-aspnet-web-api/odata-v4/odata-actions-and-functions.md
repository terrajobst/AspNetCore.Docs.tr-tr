---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Eylemler ve OData v4 işlevleri kullanarak ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: OData, eylemleri ve işlevleri kolayca CRUD işlemleri varlıklar üzerinde olarak tanımlanmamış olan sunucu tarafı davranışları eklemek için bir yoldur. Bu öğreticide gösterilmiştir nasıl...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566775"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="06c6d-104">Eylemleri ve işlevleri OData v4 ASP.NET Web API 2.2 kullanma</span><span class="sxs-lookup"><span data-stu-id="06c6d-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="06c6d-105">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="06c6d-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="06c6d-106">OData, eylemleri ve işlevleri kolayca CRUD işlemleri varlıklar üzerinde olarak tanımlanmamış olan sunucu tarafı davranışları eklemek için bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="06c6d-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="06c6d-107">Bu öğretici eylemleri ve işlevleri Web API 2.2 kullanarak bir OData v4 uç noktaya nasıl ekleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="06c6d-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="06c6d-108">Öğretici öğreticiyi derlemeler [OData v4 uç nokta kullanarak ASP.NET Web API 2 oluşturma](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="06c6d-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="06c6d-109">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="06c6d-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="06c6d-110">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="06c6d-110">Web API 2.2</span></span>
> - <span data-ttu-id="06c6d-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="06c6d-111">OData v4</span></span>
> - [<span data-ttu-id="06c6d-112">Visual Studio 2013 Güncelleştirme 2</span><span class="sxs-lookup"><span data-stu-id="06c6d-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="06c6d-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="06c6d-113">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="06c6d-114">Eğitmen sürümleri</span><span class="sxs-lookup"><span data-stu-id="06c6d-114">Tutorial versions</span></span>
> 
> <span data-ttu-id="06c6d-115">OData sürüm 3 için bkz: [ASP.NET Web API 2 OData Eylemler](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="06c6d-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>


<span data-ttu-id="06c6d-116">Arasındaki farkı *Eylemler* ve *işlevleri* Eylemler yan etkileri olabilir ve işlevleri yapın.</span><span class="sxs-lookup"><span data-stu-id="06c6d-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="06c6d-117">Eylemleri ve işlevleri veri döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="06c6d-117">Both actions and functions can return data.</span></span> <span data-ttu-id="06c6d-118">Bazı eylemler kullanımları şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="06c6d-118">Some uses for actions include:</span></span>

- <span data-ttu-id="06c6d-119">Karmaşık işlemleri.</span><span class="sxs-lookup"><span data-stu-id="06c6d-119">Complex transactions.</span></span>
- <span data-ttu-id="06c6d-120">Aynı anda birden fazla varlıkların düzenleme.</span><span class="sxs-lookup"><span data-stu-id="06c6d-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="06c6d-121">Bir varlığın belirli özellikleri için yalnızca güncelleştirmelere izin verme.</span><span class="sxs-lookup"><span data-stu-id="06c6d-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="06c6d-122">Bir varlık olmayan veriler gönderme.</span><span class="sxs-lookup"><span data-stu-id="06c6d-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="06c6d-123">İşlevler, doğrudan bir varlık veya koleksiyonuna karşılık gelmediğinden bilgileri döndürmek için faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="06c6d-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="06c6d-124">Bir eylem (veya işlevi) tek bir varlık veya bir koleksiyon hedefleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06c6d-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="06c6d-125">OData terminolojisinde budur *bağlama*.</span><span class="sxs-lookup"><span data-stu-id="06c6d-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="06c6d-126">Ayrıca olabilir &quot;ilişkisiz&quot; hizmetteki statik işlemleri olarak adlandırılır Eylemler/İşlevler,.</span><span class="sxs-lookup"><span data-stu-id="06c6d-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="06c6d-127">Örnek: bir eylem ekleme</span><span class="sxs-lookup"><span data-stu-id="06c6d-127">Example: Adding an Action</span></span>

<span data-ttu-id="06c6d-128">Şimdi bir ürün derecelendirmek için bir eylem tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="06c6d-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="06c6d-129">Bu öğretici öğreticiyi derlemeler [OData v4 uç nokta kullanarak ASP.NET Web API 2 oluşturma](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="06c6d-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="06c6d-130">İlk olarak, ekleyin bir `ProductRating` derecelendirmeleri temsil etmek için model.</span><span class="sxs-lookup"><span data-stu-id="06c6d-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="06c6d-131">Ayrıca bir **DbSet** için `ProductsContext` sınıf böylece EF veritabanında bir derecelendirme tablo oluşturur.</span><span class="sxs-lookup"><span data-stu-id="06c6d-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="06c6d-132">Eylem için EDM ekleme</span><span class="sxs-lookup"><span data-stu-id="06c6d-132">Add the Action to the EDM</span></span>

<span data-ttu-id="06c6d-133">WebApiConfig.cs içinde aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="06c6d-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="06c6d-134">**EntityTypeConfiguration.Action** yöntemi, varlık veri modeli (EDM) için bir eylem ekler.</span><span class="sxs-lookup"><span data-stu-id="06c6d-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="06c6d-135">**Parametresi** yöntemi eylemi için belirtilmiş bir parametre belirtir.</span><span class="sxs-lookup"><span data-stu-id="06c6d-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="06c6d-136">Bu kod, ad alanı için EDM da ayarlar.</span><span class="sxs-lookup"><span data-stu-id="06c6d-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="06c6d-137">Ad alanı URI eylem için eylem tam adı içerdiğinden önemlidir:</span><span class="sxs-lookup"><span data-stu-id="06c6d-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="06c6d-138">Tipik bir IIS yapılandırması bu URL'de nokta Hata 404 döndürmek üzere IIS neden olur.</span><span class="sxs-lookup"><span data-stu-id="06c6d-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="06c6d-139">Bunu, aşağıdaki bölümü Web.Config dosyasına ekleyerek çözümleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="06c6d-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="06c6d-140">Eylem için bir denetleyici yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="06c6d-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="06c6d-141">Etkinleştirmek için &quot;oranı&quot; eylem, aşağıdaki yöntemi ekleyin `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="06c6d-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="06c6d-142">Yöntem adı eylem adı ile eşleşen dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="06c6d-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="06c6d-143">**[HttpPost]** özniteliği, yöntemdir bir HTTP POST yöntemini belirtir.</span><span class="sxs-lookup"><span data-stu-id="06c6d-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="06c6d-144">Eylemi çağırmak için istemci aşağıdaki gibi bir HTTP POST isteği gönderir:</span><span class="sxs-lookup"><span data-stu-id="06c6d-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="06c6d-145">&quot;Oranı&quot; eylem eylemi için URI URI varlığa eklenen tam eylem adı olacak şekilde ürün örneklerine bağlı.</span><span class="sxs-lookup"><span data-stu-id="06c6d-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="06c6d-146">(Biz EDM ad alanı ayarlamak geri çağırma &quot;ProductService&quot;tam eylem adı gelir &quot;ProductService.Rate&quot;.)</span><span class="sxs-lookup"><span data-stu-id="06c6d-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="06c6d-147">İstek gövdesini bir JSON yükü olarak eylem parametrelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="06c6d-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="06c6d-148">Web API otomatik olarak dönüştürür JSON yükü bir **ODataActionParameters** yalnızca parametre değerleri sözlüğü nesne.</span><span class="sxs-lookup"><span data-stu-id="06c6d-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="06c6d-149">Denetleyici yönteminin parametrelerinde erişmek için bu sözlük kullanın.</span><span class="sxs-lookup"><span data-stu-id="06c6d-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="06c6d-150">İstemcisi yanlış eylem parametrelerini gönderirse biçimi, değeri **ModelState.IsValid** false olur.</span><span class="sxs-lookup"><span data-stu-id="06c6d-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="06c6d-151">Bu bayrak, denetleyici yönteminde denetleyin ve hata döndürür **IsValid** false olur.</span><span class="sxs-lookup"><span data-stu-id="06c6d-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="06c6d-152">Örnek: işlev ekleme</span><span class="sxs-lookup"><span data-stu-id="06c6d-152">Example: Adding a Function</span></span>

<span data-ttu-id="06c6d-153">Şimdi en pahalı ürün döndüren bir OData işlevini ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="06c6d-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="06c6d-154">Önce ilk adım işlevi için EDM ekleme gibi.</span><span class="sxs-lookup"><span data-stu-id="06c6d-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="06c6d-155">WebApiConfig.cs içinde aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="06c6d-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="06c6d-156">Bu durumda, işlevi ürünleri koleksiyon yerine tek tek ürün örnekleri bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="06c6d-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="06c6d-157">İstemciler, bir GET isteği göndererek işlevi çağırır:</span><span class="sxs-lookup"><span data-stu-id="06c6d-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="06c6d-158">Bu işlev için denetleyici yöntemi şöyledir:</span><span class="sxs-lookup"><span data-stu-id="06c6d-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="06c6d-159">Yöntem adı işlevi adıyla eşleşen dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="06c6d-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="06c6d-160">**[HttpGet]** özniteliği, yöntemdir bir HTTP GET yöntemi belirtir.</span><span class="sxs-lookup"><span data-stu-id="06c6d-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="06c6d-161">HTTP yanıtı şöyledir:</span><span class="sxs-lookup"><span data-stu-id="06c6d-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="06c6d-162">Örnek: ilişkisiz bir işlev ekleme</span><span class="sxs-lookup"><span data-stu-id="06c6d-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="06c6d-163">Önceki örnekte, bir koleksiyona bağlı bir işlev oluştu.</span><span class="sxs-lookup"><span data-stu-id="06c6d-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="06c6d-164">Bu sonraki örnekte oluşturacağız bir *ilişkisiz* işlevi.</span><span class="sxs-lookup"><span data-stu-id="06c6d-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="06c6d-165">İlişkisiz işlevleri hizmetteki statik işlemleri olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="06c6d-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="06c6d-166">Bu örnekte işlevi verilen bir posta kodu için satış vergi döndürür.</span><span class="sxs-lookup"><span data-stu-id="06c6d-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="06c6d-167">WebApiConfig dosyasında EDM işlevi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="06c6d-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="06c6d-168">Numaralı telefonu bildirimi **işlevi** doğrudan üzerinde **ODataModelBuilder**, varlık türü veya koleksiyon yerine.</span><span class="sxs-lookup"><span data-stu-id="06c6d-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="06c6d-169">Bu model oluşturucu işlevi ilişkisiz olduğunu söyler.</span><span class="sxs-lookup"><span data-stu-id="06c6d-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="06c6d-170">İşlev uygulayan denetleyici yöntemi şöyledir:</span><span class="sxs-lookup"><span data-stu-id="06c6d-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="06c6d-171">Bu yöntemde yerleştirin hangi Web API denetleyicisi önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="06c6d-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="06c6d-172">Bunu içine `ProductsController`, veya ayrı bir denetleyici tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="06c6d-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="06c6d-173">**[ODataRoute]** özniteliği işlevi için URI şablonunu tanımlar.</span><span class="sxs-lookup"><span data-stu-id="06c6d-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="06c6d-174">Bir örnek istemci isteği şöyledir:</span><span class="sxs-lookup"><span data-stu-id="06c6d-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="06c6d-175">HTTP yanıtı:</span><span class="sxs-lookup"><span data-stu-id="06c6d-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
