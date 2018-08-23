---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: ASP.NET Web API 2 OData eylemleri destekleyen | Microsoft Docs
author: MikeWasson
description: 'OData, varlıkları CRUD işlemleri olarak kolayca tanımlanmayan bir sunucu tarafı davranışları eklemek için bir yol eylemlerdir. Bazı eylemler kullanımları şunlardır: uygulama...'
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: 71c0f91f709aca0deb5548bdbcad60d79a2702f6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756500"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="cac34-104">ASP.NET Web API 2 OData eylemleri destekleme</span><span class="sxs-lookup"><span data-stu-id="cac34-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="cac34-105">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cac34-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="cac34-106">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="cac34-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="cac34-107">Odata'da, *eylemleri* varlıkları CRUD işlemleri olarak kolayca tanımlanmayan bir sunucu tarafı davranışları eklemek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="cac34-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="cac34-108">Bazı eylemler kullanımları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="cac34-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="cac34-109">Karmaşık işlemleri uygulama.</span><span class="sxs-lookup"><span data-stu-id="cac34-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="cac34-110">Çeşitli varlıklar aynı anda işleniyor.</span><span class="sxs-lookup"><span data-stu-id="cac34-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="cac34-111">Güncelleştirmeleri yalnızca belirli özellikleri bir varlığın izin verme.</span><span class="sxs-lookup"><span data-stu-id="cac34-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="cac34-112">Bir varlıkta tanımlı değil sunucu bilgileri gönderiliyor.</span><span class="sxs-lookup"><span data-stu-id="cac34-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="cac34-113">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="cac34-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="cac34-114">Web API 2</span><span class="sxs-lookup"><span data-stu-id="cac34-114">Web API 2</span></span>
> - <span data-ttu-id="cac34-115">OData sürüm 3</span><span class="sxs-lookup"><span data-stu-id="cac34-115">OData Version 3</span></span>
> - <span data-ttu-id="cac34-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="cac34-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="cac34-117">Örnek: ürün değerlendirme</span><span class="sxs-lookup"><span data-stu-id="cac34-117">Example: Rating a Product</span></span>

<span data-ttu-id="cac34-118">Bu örnekte, ürünleri oranı ve her ürün için ortalama derecelendirme sunarsınız kullanıcılara bildirmek istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="cac34-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="cac34-119">Derecelendirme, ürün anahtarlı listesini veritabanında depolarız.</span><span class="sxs-lookup"><span data-stu-id="cac34-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="cac34-120">Entity Framework derecelendirmelerini temsil etmek için kullanabilir model şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="cac34-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="cac34-121">Ancak biz posta istemcilerine istemiyorsanız bir `ProductRating` "Derecelendirmeleri" koleksiyonu için nesne.</span><span class="sxs-lookup"><span data-stu-id="cac34-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="cac34-122">Sezgisel, derecelendirme ürünleri koleksiyonla ilişkilidir ve istemci yalnızca derecelendirme değeri gerekir.</span><span class="sxs-lookup"><span data-stu-id="cac34-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="cac34-123">Bu nedenle, normal CRUD işlemleri kullanmak yerine, bir istemci çağırabileceği bir eylem bir ürün üzerinde tanımlarız.</span><span class="sxs-lookup"><span data-stu-id="cac34-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="cac34-124">OData terminolojisinde eylemdir *bağlı* ürün varlıkları için.</span><span class="sxs-lookup"><span data-stu-id="cac34-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="cac34-125">Eylemler, sunucuda yan etkilere sahip.</span><span class="sxs-lookup"><span data-stu-id="cac34-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="cac34-126">Bu nedenle, bunlar HTTP POST istekleri kullanılarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="cac34-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="cac34-127">Eylemler, parametreler ve dönüş türleri, hizmet meta verilerde tanımlanan olabilir.</span><span class="sxs-lookup"><span data-stu-id="cac34-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="cac34-128">İstek gövdesinde parametreleri istemciye gönderir ve sunucu yanıt gövdesinde döndürülen değer gönderir.</span><span class="sxs-lookup"><span data-stu-id="cac34-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="cac34-129">"Oranı Product" eylemi çağırmak için posta istemci aşağıdaki gibi bir URI gönderir:</span><span class="sxs-lookup"><span data-stu-id="cac34-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="cac34-130">POST isteğinde verilerdir yalnızca ürün derecelendirmesi:</span><span class="sxs-lookup"><span data-stu-id="cac34-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="cac34-131">Varlık veri modeli eylemi bildirme</span><span class="sxs-lookup"><span data-stu-id="cac34-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="cac34-132">Web API yapılandırmanızda eylemi varlık veri modeli (EDM) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="cac34-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="cac34-133">Bu kod, "RateProduct" Ürün varlıklar üzerinde gerçekleştirilecek bir eylem olarak tanımlar.</span><span class="sxs-lookup"><span data-stu-id="cac34-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="cac34-134">Eylemin kullandığı da bildirir bir **int** parametresi "Sıralama" adlı ve döndüren bir **int** değeri.</span><span class="sxs-lookup"><span data-stu-id="cac34-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="cac34-135">Denetleyici için eylem ekleme</span><span class="sxs-lookup"><span data-stu-id="cac34-135">Add the Action to the Controller</span></span>

<span data-ttu-id="cac34-136">"RateProduct" eylemi için ürün varlıkları bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="cac34-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="cac34-137">Eylemi uygulamak için adında bir yöntem ekleyin `RateProduct` ürünleri denetleyicisi:</span><span class="sxs-lookup"><span data-stu-id="cac34-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="cac34-138">Yöntem adı EDM eylemin adını eşleştiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="cac34-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="cac34-139">Yöntemi iki parametreye sahiptir:</span><span class="sxs-lookup"><span data-stu-id="cac34-139">The method has two parameters:</span></span>

- <span data-ttu-id="cac34-140">*anahtar*: derecelendirmek için bir ürün anahtarı.</span><span class="sxs-lookup"><span data-stu-id="cac34-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="cac34-141">*parametreleri*: eylem parametresi değerleri sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="cac34-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="cac34-142">Varsayılan rota kuralları kullanıyorsanız, anahtar parametresi "önemli" olarak adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="cac34-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="cac34-143">Eklenmesi önemlidir **[FromOdataUri]** gösterildiği gibi öznitelik.</span><span class="sxs-lookup"><span data-stu-id="cac34-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="cac34-144">Bu öznitelik, istek URI'si anahtarından ayrıştırırken OData sözdizimi kurallarını kullanmak için Web API'si söyler.</span><span class="sxs-lookup"><span data-stu-id="cac34-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="cac34-145">Kullanım *parametreleri* sözlük eylem parametrelerini almak için:</span><span class="sxs-lookup"><span data-stu-id="cac34-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="cac34-146">İstemci eylem parametrelerini doğru gönderirse biçimi, değerini **ModelState.IsValid** geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="cac34-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="cac34-147">Bu durumda, kullanabileceğiniz **ODataActionParameters** parametre değerlerini almak için bir sözlük.</span><span class="sxs-lookup"><span data-stu-id="cac34-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="cac34-148">Bu örnekte, `RateProduct` eylem "Sıralama" adlı tek bir parametre alır.</span><span class="sxs-lookup"><span data-stu-id="cac34-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="cac34-149">Eylem meta verileri</span><span class="sxs-lookup"><span data-stu-id="cac34-149">Action Metadata</span></span>

<span data-ttu-id="cac34-150">Hizmet meta verileri görüntülemek için /odata/$ meta verileri için bir GET isteği gönderin.</span><span class="sxs-lookup"><span data-stu-id="cac34-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="cac34-151">Bildiren bir meta veri bölümünü işte `RateProduct` eylem:</span><span class="sxs-lookup"><span data-stu-id="cac34-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="cac34-152">**Functionımport** eylemi öğe bildirir.</span><span class="sxs-lookup"><span data-stu-id="cac34-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="cac34-153">Alanların çoğu kendinden açıklamalıdır ancak iki çıkarabileceğini şunlardır:</span><span class="sxs-lookup"><span data-stu-id="cac34-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="cac34-154">**İsbindable olduğunda** eylemi çağrılabilir hedef varlık üzerinde en az anlamına gelir sürenin bir kısmı.</span><span class="sxs-lookup"><span data-stu-id="cac34-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="cac34-155">**Isalwaysbindable** eylem her zaman hedef varlık üzerinde çağrılacak anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="cac34-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="cac34-156">Bazı eylemler her zaman istemciler için kullanılabilir, ancak diğer eylemler varlık durumuna bağlı olabilir farktır.</span><span class="sxs-lookup"><span data-stu-id="cac34-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="cac34-157">Örneğin, "Satın Al" eylemini tanımladığınız varsayalım.</span><span class="sxs-lookup"><span data-stu-id="cac34-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="cac34-158">Yalnızca stokta olan bir öğeyi satın alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cac34-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="cac34-159">Öğe sağlanamıyor ise, bir istemci o eylemi çağrılamıyor.</span><span class="sxs-lookup"><span data-stu-id="cac34-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="cac34-160">EDM tanımlarken **eylem** yöntemi her zaman bağlanabilirse bir eylem oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cac34-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="cac34-161">Not-her zaman-bağlanabilir eylemleri hakkında ele alacağız (olarak da bilinir *geçici* eylemleri) Bu konuda.</span><span class="sxs-lookup"><span data-stu-id="cac34-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="cac34-162">Eylemi Çağırma</span><span class="sxs-lookup"><span data-stu-id="cac34-162">Invoking the Action</span></span>

<span data-ttu-id="cac34-163">Artık bu eylem bir istemci nasıl çağıracaktır görelim.</span><span class="sxs-lookup"><span data-stu-id="cac34-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="cac34-164">İstemci Kimliğine sahip ürün derecelendirmesi 2 vermek istediğini varsayalım = 4.</span><span class="sxs-lookup"><span data-stu-id="cac34-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="cac34-165">İstek gövdesi JSON biçimini kullanarak bir örnek istek iletisi aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="cac34-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="cac34-166">Yanıt iletisi aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="cac34-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="cac34-167">Bir varlık kümesi için bir eylem bağlama</span><span class="sxs-lookup"><span data-stu-id="cac34-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="cac34-168">Önceki örnekte, tek bir varlığa bağlı eylemi: tek bir ürün istemci derecelendirir.</span><span class="sxs-lookup"><span data-stu-id="cac34-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="cac34-169">Ayrıca, bir eylem bir varlık koleksiyonuna bağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cac34-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="cac34-170">Yalnızca aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="cac34-170">Just make the following changes:</span></span>

<span data-ttu-id="cac34-171">EDM içinde varlığın Eylem Ekle **koleksiyon** özelliği.</span><span class="sxs-lookup"><span data-stu-id="cac34-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="cac34-172">Denetleyici yöntemin atlamak *anahtar* parametresi.</span><span class="sxs-lookup"><span data-stu-id="cac34-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="cac34-173">Artık istemci ürünleri varlık kümesini eylemini çağırır:</span><span class="sxs-lookup"><span data-stu-id="cac34-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="cac34-174">Toplama parametrelerini eylemleri</span><span class="sxs-lookup"><span data-stu-id="cac34-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="cac34-175">Eylemler, bir değerler topluluğunu parametreleri olabilir.</span><span class="sxs-lookup"><span data-stu-id="cac34-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="cac34-176">EDM içinde kullanmak **CollectionParameter&lt;T&gt;**  parametre bildirmek için.</span><span class="sxs-lookup"><span data-stu-id="cac34-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="cac34-177">Bu "koleksiyonunu alır derecelendirmeleri" adlı bir parametre bildirir **int** değerleri.</span><span class="sxs-lookup"><span data-stu-id="cac34-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="cac34-178">Denetleyici yöntemin almaya devam ediyorsanız parametre değerinin **ODataActionParameters** nesne, ancak artık değeri bir **ICollection&lt;int&gt;**  değeri:</span><span class="sxs-lookup"><span data-stu-id="cac34-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="cac34-179">Geçici eylemleri</span><span class="sxs-lookup"><span data-stu-id="cac34-179">Transient Actions</span></span>

<span data-ttu-id="cac34-180">Eylemin her zaman kullanılabilir olacak şekilde "RateProduct" örnekte, kullanıcıların her zaman bir ürün derecelendirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cac34-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="cac34-181">Ancak, bazı eylemler varlık durumuna bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="cac34-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="cac34-182">Örneğin, bir video kiralama hizmetinde "Kullanıma Al" eylemini her zaman kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="cac34-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="cac34-183">(Bu, söz konusu videoyu bir kopyasını kullanılabilir olup olmadığını bağlıdır.) Bu tür bir eylem olarak adlandırılan bir *geçici* eylem.</span><span class="sxs-lookup"><span data-stu-id="cac34-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="cac34-184">Hizmet meta verileri geçici bir eyleme sahip **Isalwaysbindable** false eşittir.</span><span class="sxs-lookup"><span data-stu-id="cac34-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="cac34-185">Meta veriler aşağıdaki gibi görünmesi için gerçekten varsayılan değer olan:</span><span class="sxs-lookup"><span data-stu-id="cac34-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="cac34-186">Bu neden önemli olduğunu aşağıdadır: sunucunun eylem geçici ise, istemci eylemi kullanılabilir olduğunda söylemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="cac34-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="cac34-187">Varlık içinde eylem bağlantısı ekleyerek bunu yapar.</span><span class="sxs-lookup"><span data-stu-id="cac34-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="cac34-188">Bir film varlık için bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="cac34-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="cac34-189">"#CheckOut" özelliği CheckOut eylemi bir bağlantı içerir.</span><span class="sxs-lookup"><span data-stu-id="cac34-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="cac34-190">Eylem kullanılamıyorsa, sunucunun bağlantı atlar.</span><span class="sxs-lookup"><span data-stu-id="cac34-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="cac34-191">EDM geçici bir eylemde bildirmek için çağrı **TransientAction** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="cac34-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="cac34-192">Ayrıca, belirli bir varlık için bir eylem bağlantısı döndüren bir işlev sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="cac34-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="cac34-193">Bu işlev çağırarak ayarlamak **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="cac34-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="cac34-194">İşlevi bir lambda ifadesi yazabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="cac34-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="cac34-195">Eylem varsa, lambda ifadesi bir bağlantı için eylem döndürür.</span><span class="sxs-lookup"><span data-stu-id="cac34-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="cac34-196">OData serileştirici varlık serileştiren bu bağlantıyı içerir.</span><span class="sxs-lookup"><span data-stu-id="cac34-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="cac34-197">Eylemi kullanılabilir olmadığı durumlarda, bir işlevi döndürür `null`.</span><span class="sxs-lookup"><span data-stu-id="cac34-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cac34-198">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="cac34-198">Additional Resources</span></span>

[<span data-ttu-id="cac34-199">OData eylem örneği</span><span class="sxs-lookup"><span data-stu-id="cac34-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
