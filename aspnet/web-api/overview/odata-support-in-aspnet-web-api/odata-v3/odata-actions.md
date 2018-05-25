---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: ASP.NET Web API 2 OData eylemleri destekleyen | Microsoft Docs
author: MikeWasson
description: 'OData içinde eylemler kolayca CRUD işlemleri varlıklar üzerinde olarak tanımlanmamış olan sunucu tarafı davranışları eklemek için bir yoldur. Bazı eylemler kullanımları şunlardır: uygulama...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="6fd9c-104">ASP.NET Web API 2 OData eylemlerinin destekleme</span><span class="sxs-lookup"><span data-stu-id="6fd9c-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="6fd9c-105">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6fd9c-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="6fd9c-106">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="6fd9c-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="6fd9c-107">Odata'da, *Eylemler* kolayca CRUD işlemleri varlıklar üzerinde olarak tanımlanmamış olan sunucu tarafı davranışları eklemek için bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="6fd9c-108">Bazı eylemler kullanımları şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="6fd9c-109">Karmaşık işlemleri uygulama.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="6fd9c-110">Aynı anda birden fazla varlıkların düzenleme.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="6fd9c-111">Bir varlığın belirli özellikleri için yalnızca güncelleştirmelere izin verme.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="6fd9c-112">Bir varlığı tanımlı değil sunucu bilgileri gönderme.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6fd9c-113">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="6fd9c-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6fd9c-114">Web API 2</span><span class="sxs-lookup"><span data-stu-id="6fd9c-114">Web API 2</span></span>
> - <span data-ttu-id="6fd9c-115">OData sürüm 3</span><span class="sxs-lookup"><span data-stu-id="6fd9c-115">OData Version 3</span></span>
> - <span data-ttu-id="6fd9c-116">Varlık Çerçevesi 6</span><span class="sxs-lookup"><span data-stu-id="6fd9c-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="6fd9c-117">Örnek: ürün değerlendirme</span><span class="sxs-lookup"><span data-stu-id="6fd9c-117">Example: Rating a Product</span></span>

<span data-ttu-id="6fd9c-118">Bu örnekte, ürünleri oranı ve her ürün için Ortalama Derecelendirmelerini kullanıma kullanıcıların izin vermek istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="6fd9c-119">Veritabanında, biz derecelendirmeleri, ürün için anahtarlı listesini depolar.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="6fd9c-120">Entity Framework derecelendirmeleri temsil etmek için kullanabilir modeli şöyledir:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="6fd9c-121">Ancak biz POST istemcilere istemediğiniz bir `ProductRating` "Derecelendirme" koleksiyonuna nesne.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="6fd9c-122">Sezgisel, derecelendirme ürünleri koleksiyonuyla ilişkilendirilen ve istemcinin yalnızca derecelendirme değerini sonrası gerekir.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="6fd9c-123">Bu nedenle, normal CRUD işlemleri kullanmak yerine, bir istemci çağırabileceği bir eylem bir ürün üzerinde tanımlarız.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="6fd9c-124">OData terminolojisinde eylemdir *bağlı* ürün varlıklara.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="6fd9c-125">Eylemler sunucu üzerinde yan etkileri olması.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="6fd9c-126">Bu nedenle, bunlar HTTP POST istekleri kullanılarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="6fd9c-127">Eylemler, parametreler ve hizmet meta verilerde tanımlanan dönüş türleri olabilir.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="6fd9c-128">İstemci istek gövdesinde parametreleri gönderir ve sunucu yanıt gövdesi içinde dönüş değeri gönderir.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="6fd9c-129">"Oranı ürün" eylemi çağırmak için istemci bir POST aşağıdaki gibi bir URI gönderir:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="6fd9c-130">POST isteğini veriler yalnızca ürün derecelendirme bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="6fd9c-131">Varlık veri modeli eylemde bildirme</span><span class="sxs-lookup"><span data-stu-id="6fd9c-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="6fd9c-132">Web API yapılandırmanızda eylem varlık veri modeli (EDM) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="6fd9c-133">Bu kod "RateProduct" Ürün varlıklar üzerinde gerçekleştirilen eylem olarak tanımlar.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="6fd9c-134">Ayrıca eylem aldığını bildiren bir **int** parametresi "Derecelendirme" adlı ve döndürür bir **int** değeri.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="6fd9c-135">Denetleyici için eylem ekleme</span><span class="sxs-lookup"><span data-stu-id="6fd9c-135">Add the Action to the Controller</span></span>

<span data-ttu-id="6fd9c-136">"RateProduct" eylemi ürün varlıklara bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="6fd9c-137">Eylem uygulamak için adlandırılmış bir yöntem eklemek `RateProduct` ürünleri denetleyicisine:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="6fd9c-138">Yöntem adı EDM eylemde adıyla eşleşen dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="6fd9c-139">Yöntemi iki parametreye sahiptir:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-139">The method has two parameters:</span></span>

- <span data-ttu-id="6fd9c-140">*anahtar*: derecelendirmek için ürün anahtarı.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="6fd9c-141">*parametreleri*: eylem parametre değerleri sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="6fd9c-142">Varsayılan yönlendirme kuralları kullanıyorsanız, anahtar parametreyi "anahtar" olarak adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="6fd9c-143">Eklenmesi önemlidir **[FromOdataUri]** gösterildiği gibi öznitelik.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="6fd9c-144">Bu öznitelik, istek URI'si anahtarından ayrıştırırken OData sözdizimi kurallarına kullanmak için Web API söyler.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="6fd9c-145">Kullanım *parametreleri* sözlük eylem parametrelerini almak için:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="6fd9c-146">İstemci eylem parametrelerini doğru durumda gönderirse biçimi, değeri **ModelState.IsValid** doğrudur.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="6fd9c-147">Bu durumda, kullanabileceğiniz **ODataActionParameters** parametre değerlerini almak için Sözlük.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="6fd9c-148">Bu örnekte, `RateProduct` eylemde "Derecelendirme" adlı tek bir parametre.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="6fd9c-149">Eylem meta verileri</span><span class="sxs-lookup"><span data-stu-id="6fd9c-149">Action Metadata</span></span>

<span data-ttu-id="6fd9c-150">Hizmet meta verilerini görüntülemek için /odata/$ meta verileri için bir GET isteği gönderin.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="6fd9c-151">Bildirir meta veri bölümü işte `RateProduct` eylem:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="6fd9c-152">**Functionımport** öğesi eylem bildirir.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="6fd9c-153">Alanların çoğu kendinden açıklamalıdır, ancak iki belirtmeye değerinde şunlardır:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="6fd9c-154">**IsBindable** eylemi çağrılabilir hedef varlık üzerinde en az anlamına gelir sürenin bir kısmı.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="6fd9c-155">**Isalwaysbindable** eylem her zaman hedef varlık üzerinde çağrılacak anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="6fd9c-156">Bazı eylemler her zaman istemciler için kullanılabilir, ancak diğer eylemler varlık durumuna bağlı olabilir farktır.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="6fd9c-157">Örneğin, "Satın Al" eylemi tanımlamak varsayalım.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="6fd9c-158">Yalnızca stokta olan bir öğeyi satın alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="6fd9c-159">Madde hisse senedi dışında ise, bir istemci bu eylemi çağıramaz.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="6fd9c-160">EDM tanımlarken **eylem** yöntemi her zaman bağlanabilirse bir eylem oluşturur:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="6fd9c-161">Not-her zaman-bağlanabilirse eylemler hakkında konuşun (olarak da bilinir *geçici* Eylemler) Bu konuda daha sonra.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="6fd9c-162">Eylemi Çağırma</span><span class="sxs-lookup"><span data-stu-id="6fd9c-162">Invoking the Action</span></span>

<span data-ttu-id="6fd9c-163">Şimdi bir istemci bu eylem nasıl çağıracaktır görelim.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="6fd9c-164">Kimlikli Ürün 2 derecesi sağlamak istemcinin istediği varsayalım = 4.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="6fd9c-165">İstek gövdesi için JSON biçimini kullanarak bir örnek istek iletisi şudur:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="6fd9c-166">Yanıt iletisi şudur:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="6fd9c-167">Bir varlık kümesine bir eylem bağlama</span><span class="sxs-lookup"><span data-stu-id="6fd9c-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="6fd9c-168">Önceki örnekte eylem tek bir varlığa bağlıdır: tek bir ürün istemci derecelendirir.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="6fd9c-169">Varlık koleksiyonu için bir eylem de bağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="6fd9c-170">Yalnızca aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-170">Just make the following changes:</span></span>

<span data-ttu-id="6fd9c-171">Varlığın EDM içinde Ekle eylemi **koleksiyonu** özelliği.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="6fd9c-172">Denetleyici yönteminin atlayın *anahtar* parametresi.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="6fd9c-173">Şimdi istemci ürünleri varlık kümesini eylemini çağırır:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="6fd9c-174">Toplama parametrelerini olan eylemler</span><span class="sxs-lookup"><span data-stu-id="6fd9c-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="6fd9c-175">Eylemler değerler koleksiyonu ele parametrelerine sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="6fd9c-176">EDM içinde kullanmak **CollectionParameter&lt;T&gt;**  parametresi bildirmek için.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="6fd9c-177">Bu "bir koleksiyonunu alır derecelendirmeleri" adlı bir parametre bildirir **int** değerleri.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="6fd9c-178">Denetleyici yönteminin, parametre değerinin almaya devam **ODataActionParameters** nesne ancak şimdi değeri olan bir **ICollection&lt;int&gt;**  değeri:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="6fd9c-179">Geçici Eylemler</span><span class="sxs-lookup"><span data-stu-id="6fd9c-179">Transient Actions</span></span>

<span data-ttu-id="6fd9c-180">Eylem her zaman kullanılabilir olacak şekilde "RateProduct" örnekte, kullanıcıların her zaman bir ürün oranı.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="6fd9c-181">Ancak bazı eylemler varlık durumuna bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="6fd9c-182">Örneğin, bir video kiralama hizmetinde "CheckOut" eylem her zaman kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="6fd9c-183">(Bu videonun bir kopyasını kullanılabilir olup olmadığını bağlıdır.) Bu tür eylem olarak adlandırılan bir *geçici* eylem.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="6fd9c-184">Hizmet meta verilerini geçici bir eylemi var. **Isalwaysbindable** false eşittir.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="6fd9c-185">Meta veriler şu şekilde görünmesi için varsayılan değeri gerçekte şudur:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="6fd9c-186">Bu neden önemlidir burada'nın: sunucu bir eylem geçici ise, eylem kullanılabilir olduğunda istemci söyleyin gerekir.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="6fd9c-187">Bu varlıkta eylem bağlantısı dahil olmak üzere yapar.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="6fd9c-188">Film varlık için örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="6fd9c-189">"#CheckOut" özelliği CheckOut eylemi için bir bağlantı içerir.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="6fd9c-190">Eylem kullanılamıyorsa, sunucunun bağlantıyı atlar.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="6fd9c-191">EDM geçici bir eylemde bildirmek için çağrı **TransientAction** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="6fd9c-192">Ayrıca, belirli bir varlık için bir eylem bağlantısı döndüren bir işlev sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="6fd9c-193">Bu işlev aranarak **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="6fd9c-194">Lambda ifadesi işlevi yazabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="6fd9c-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="6fd9c-195">Eylem varsa, lambda ifadesi bir bağlantı eyleme döndürür.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="6fd9c-196">Varlık serileştiren olduğunda OData serileştirici Bu bağlantı içerir.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="6fd9c-197">Eylem kullanılamadığında işlevi döndürür `null`.</span><span class="sxs-lookup"><span data-stu-id="6fd9c-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6fd9c-198">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6fd9c-198">Additional Resources</span></span>

[<span data-ttu-id="6fd9c-199">OData Eylemler örnek</span><span class="sxs-lookup"><span data-stu-id="6fd9c-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
