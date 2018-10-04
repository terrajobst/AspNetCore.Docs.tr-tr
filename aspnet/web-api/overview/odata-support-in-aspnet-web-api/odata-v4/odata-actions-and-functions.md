---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Eylemler ve OData v4 sürümünde işlevleri kullanarak ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: OData, eylemleri ve işlevleri varlıkları CRUD işlemleri olarak kolayca tanımlanmayan bir sunucu tarafı davranışları eklemek için bir yoludur. Bu öğreticide gösterilmiştir nasıl yapılır...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 45b84ec4ee76e83ece99bf6841c28e13c3ab7728
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795273"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="60cf2-104">Eylemler ve OData v4 sürümünde işlevleri kullanarak ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="60cf2-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="60cf2-105">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="60cf2-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="60cf2-106">OData, eylemleri ve işlevleri varlıkları CRUD işlemleri olarak kolayca tanımlanmayan bir sunucu tarafı davranışları eklemek için bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="60cf2-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="60cf2-107">Bu öğreticide, Web API 2.2 kullanan bir OData v4 uç noktasına, eylemleri ve işlevleri eklemek gösterilir.</span><span class="sxs-lookup"><span data-stu-id="60cf2-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="60cf2-108">Öğretici öğreticiyi genişletip yapılar [OData v4 uç noktası kullanarak ASP.NET Web API 2 oluşturma](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="60cf2-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="60cf2-109">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="60cf2-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="60cf2-110">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="60cf2-110">Web API 2.2</span></span>
> - <span data-ttu-id="60cf2-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="60cf2-111">OData v4</span></span>
> - <span data-ttu-id="60cf2-112">Visual Studio 2013 (Visual Studio 2017'yi indirin [burada](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="60cf2-112">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="60cf2-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="60cf2-113">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="60cf2-114">Öğretici sürümleri</span><span class="sxs-lookup"><span data-stu-id="60cf2-114">Tutorial versions</span></span>
>
> <span data-ttu-id="60cf2-115">OData sürüm 3 için bkz: [ASP.NET Web API 2'de OData eylemleri](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="60cf2-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>

<span data-ttu-id="60cf2-116">Arasındaki fark *eylemleri* ve *işlevleri* Eylemler, yan etkileri olabilir ve İşlevler yapın.</span><span class="sxs-lookup"><span data-stu-id="60cf2-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="60cf2-117">Hem Eylemler hem de işlevleri veri döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="60cf2-117">Both actions and functions can return data.</span></span> <span data-ttu-id="60cf2-118">Bazı eylemler kullanımları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="60cf2-118">Some uses for actions include:</span></span>

- <span data-ttu-id="60cf2-119">Karmaşık işlemleri.</span><span class="sxs-lookup"><span data-stu-id="60cf2-119">Complex transactions.</span></span>
- <span data-ttu-id="60cf2-120">Çeşitli varlıklar aynı anda işleniyor.</span><span class="sxs-lookup"><span data-stu-id="60cf2-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="60cf2-121">Güncelleştirmeleri yalnızca belirli özellikleri bir varlığın izin verme.</span><span class="sxs-lookup"><span data-stu-id="60cf2-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="60cf2-122">Bir varlık değil veri gönderiliyor.</span><span class="sxs-lookup"><span data-stu-id="60cf2-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="60cf2-123">İşlevler, bir varlık veya koleksiyon için gelmediğinden bilgilerini döndürmek için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="60cf2-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="60cf2-124">Bir eylem (ya da işlevin) tek bir varlık veya koleksiyon hedefleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="60cf2-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="60cf2-125">Bu OData terminolojisinde, *bağlama*.</span><span class="sxs-lookup"><span data-stu-id="60cf2-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="60cf2-126">Bulundurabilirsiniz &quot;ilişkisiz&quot; hizmetteki işlemleri statik olarak adlandırılan Eylemler/İşlevler.</span><span class="sxs-lookup"><span data-stu-id="60cf2-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="60cf2-127">Örnek: bir eylem ekleme</span><span class="sxs-lookup"><span data-stu-id="60cf2-127">Example: Adding an Action</span></span>

<span data-ttu-id="60cf2-128">Şimdi bir ürün derecelendirmek için bir eylem tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="60cf2-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="60cf2-129">Bu öğreticide öğreticiyi genişletip yapılar [OData v4 uç noktası kullanarak ASP.NET Web API 2 oluşturma](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="60cf2-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="60cf2-130">İlk olarak, ekleme bir `ProductRating` derecelendirmeleri temsil etmek için model.</span><span class="sxs-lookup"><span data-stu-id="60cf2-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="60cf2-131">Ayrıca bir **olan DB** için `ProductsContext` EF veritabanında bir derecelendirme tablo oluşturacaksınız böylece sınıf.</span><span class="sxs-lookup"><span data-stu-id="60cf2-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="60cf2-132">EDM için eylem ekleme</span><span class="sxs-lookup"><span data-stu-id="60cf2-132">Add the Action to the EDM</span></span>

<span data-ttu-id="60cf2-133">WebApiConfig.cs içinde aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="60cf2-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="60cf2-134">**EntityTypeConfiguration.Action** yöntemi, varlık veri modeli (EDM) için bir eylem ekler.</span><span class="sxs-lookup"><span data-stu-id="60cf2-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="60cf2-135">**Parametre** yöntemi eylemi için belirtilmiş bir parametre belirtir.</span><span class="sxs-lookup"><span data-stu-id="60cf2-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="60cf2-136">Bu kod, ad alanı için EDM de ayarlar.</span><span class="sxs-lookup"><span data-stu-id="60cf2-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="60cf2-137">Ad alanı URI eylem için eylem tam olarak nitelenmiş adını içerdiğinden önemlidir:</span><span class="sxs-lookup"><span data-stu-id="60cf2-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="60cf2-138">Tipik bir IIS yapılandırması bu URL'de nokta 404 hatası döndürmek IIS neden olur.</span><span class="sxs-lookup"><span data-stu-id="60cf2-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="60cf2-139">Aşağıdaki bölümü Web.Config dosyasına ekleyerek bunu çözebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="60cf2-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="60cf2-140">Bir denetleyici yöntemi için eylem ekleme</span><span class="sxs-lookup"><span data-stu-id="60cf2-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="60cf2-141">Etkinleştirmek için &quot;oranı&quot; eylemi, aşağıdaki yöntemi ekleyin `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="60cf2-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="60cf2-142">Yöntem adı eylem adı eşleştiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="60cf2-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="60cf2-143">**[HttpPost]** özniteliği belirtir yöntemi bir HTTP POST yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="60cf2-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="60cf2-144">Bir eylemi çağırmak için istemci aşağıdaki gibi bir HTTP POST isteği gönderir:</span><span class="sxs-lookup"><span data-stu-id="60cf2-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="60cf2-145">&quot;Oranı&quot; eylem URI eylem için varlık URI eklenmiş tam eylem adı, bu nedenle ürün örneklerine bağlı.</span><span class="sxs-lookup"><span data-stu-id="60cf2-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="60cf2-146">(Biz EDM ad alanı ayarlama geri çağırma &quot;ProductService&quot;, tam olarak nitelenmiş eylem adı, bu nedenle &quot;ProductService.Rate&quot;.)</span><span class="sxs-lookup"><span data-stu-id="60cf2-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="60cf2-147">İstek gövdesi JSON yükü olarak eylem parametrelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="60cf2-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="60cf2-148">Web API'si, JSON yükü için otomatik olarak dönüştürür bir **ODataActionParameters** yalnızca parametre değerleri sözlüğü nesne.</span><span class="sxs-lookup"><span data-stu-id="60cf2-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="60cf2-149">Bu sözlük, parametreleri, denetleyici yöntemi erişmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="60cf2-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="60cf2-150">İstemci yanlış eylem parametrelerini gönderirse biçimi, değerini **ModelState.IsValid** false'tur.</span><span class="sxs-lookup"><span data-stu-id="60cf2-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="60cf2-151">Bu bayrak denetleyicisi yönteminizde denetleyin ve hata döndürür **IsValid** false'tur.</span><span class="sxs-lookup"><span data-stu-id="60cf2-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="60cf2-152">Örnek: bir işlev ekleme</span><span class="sxs-lookup"><span data-stu-id="60cf2-152">Example: Adding a Function</span></span>

<span data-ttu-id="60cf2-153">Şimdi en pahalı ürün döndüren bir OData işlevini ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="60cf2-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="60cf2-154">Önce ilk adımı için EDM işlevi ekleme olduğu gibi.</span><span class="sxs-lookup"><span data-stu-id="60cf2-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="60cf2-155">WebApiConfig.cs içinde aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="60cf2-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="60cf2-156">Bu durumda, işlev ürünleri koleksiyonu yerine tek tek ürün örneklerine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="60cf2-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="60cf2-157">İstemciler bir GET isteği göndererek bu işlevi çağırın:</span><span class="sxs-lookup"><span data-stu-id="60cf2-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="60cf2-158">Bu işlev için denetleyici yöntemi aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="60cf2-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="60cf2-159">Yöntem adı işlev adı eşleştiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="60cf2-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="60cf2-160">**[HttpGet]** özniteliği, yöntemdir bir HTTP GET yöntemini belirtir.</span><span class="sxs-lookup"><span data-stu-id="60cf2-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="60cf2-161">HTTP yanıtı aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="60cf2-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="60cf2-162">Örnek: ilişkisiz bir işlev ekleme</span><span class="sxs-lookup"><span data-stu-id="60cf2-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="60cf2-163">Önceki örnekte, bir koleksiyona bağlı bir işlev oluştu.</span><span class="sxs-lookup"><span data-stu-id="60cf2-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="60cf2-164">Bu sonraki örnekte oluşturacağız bir *ilişkisiz* işlevi.</span><span class="sxs-lookup"><span data-stu-id="60cf2-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="60cf2-165">İlişkisiz işlevleri statik hizmetteki işlemleri olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="60cf2-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="60cf2-166">Bu örnekte işlevi verilen bir posta kodu için vergi döndürür.</span><span class="sxs-lookup"><span data-stu-id="60cf2-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="60cf2-167">WebApiConfig dosyasında için EDM işlevi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="60cf2-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="60cf2-168">Çağrı yaptığınız dikkat edin **işlevi** doğrudan üzerinde **ODataModelBuilder**, varlık türünün veya koleksiyon yerine.</span><span class="sxs-lookup"><span data-stu-id="60cf2-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="60cf2-169">Bu, model oluşturucu işlevi ilişkisiz olduğunu bildirir.</span><span class="sxs-lookup"><span data-stu-id="60cf2-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="60cf2-170">İşlev uygular denetleyici yöntemi aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="60cf2-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="60cf2-171">Bu yöntemde yerleştirdiğiniz hangi Web API denetleyicisi önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="60cf2-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="60cf2-172">İçine girdiğiniz `ProductsController`, veya ayrı denetleyicisinin tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="60cf2-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="60cf2-173">**[ODataRoute]** özniteliği, işlev için URI şablonu tanımlar.</span><span class="sxs-lookup"><span data-stu-id="60cf2-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="60cf2-174">Bir örnek istemci isteği şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="60cf2-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="60cf2-175">HTTP yanıtı:</span><span class="sxs-lookup"><span data-stu-id="60cf2-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
