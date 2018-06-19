---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Varlık İlişkileriyle ile Web API 2 OData v3 destekleme | Microsoft Docs
author: MikeWasson
description: 'Varlıkları arasındaki ilişkileri çoğu veri kümelerini tanımlayın: müşterilerin sahip siparişleri; Books yazarlar; yine de sahip istiyor musunuz? Ürünler Üreticiler vardır. OData kullanarak, istemciler üzerinde gidebilirsiniz...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566829"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="9b3ac-104">Varlık İlişkileriyle ile Web API 2 OData v3 destekleme</span><span class="sxs-lookup"><span data-stu-id="9b3ac-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>
====================
<span data-ttu-id="9b3ac-105">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9b3ac-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="9b3ac-106">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="9b3ac-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="9b3ac-107">Varlıkları arasındaki ilişkileri çoğu veri kümelerini tanımlayın: müşterilerin sahip siparişleri; Books yazarlar; yine de sahip istiyor musunuz? Ürünler Üreticiler vardır.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="9b3ac-108">OData'ı kullanarak istemcileri varlık ilişkileriyle gidin.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="9b3ac-109">Bir ürün verildiğinde, tedarikçi bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="9b3ac-110">Ayrıca, oluşturabilir veya ilişkileri kaldırın.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-110">You can also create or remove relationships.</span></span> <span data-ttu-id="9b3ac-111">Örneğin, bir ürün için tedarikçiye ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="9b3ac-112">Bu öğreticide, ASP.NET Web API işlemlerini desteklemek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="9b3ac-113">Öğretici öğreticiyi derlemeler [ile Web API 2 OData v3 uç noktası oluşturma](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="9b3ac-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9b3ac-114">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="9b3ac-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="9b3ac-115">Web API 2</span><span class="sxs-lookup"><span data-stu-id="9b3ac-115">Web API 2</span></span>
> - <span data-ttu-id="9b3ac-116">OData sürüm 3</span><span class="sxs-lookup"><span data-stu-id="9b3ac-116">OData Version 3</span></span>
> - <span data-ttu-id="9b3ac-117">Varlık Çerçevesi 6</span><span class="sxs-lookup"><span data-stu-id="9b3ac-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="9b3ac-118">Bir sağlayıcı varlık ekleme</span><span class="sxs-lookup"><span data-stu-id="9b3ac-118">Add a Supplier Entity</span></span>

<span data-ttu-id="9b3ac-119">İlk olarak biz bizim OData akışı için yeni bir varlık türü eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="9b3ac-120">Ekleyeceğiz bir `Supplier` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="9b3ac-121">Bu sınıf, bir dize için varlık anahtarı kullanır.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="9b3ac-122">Uygulamada, bir tamsayı anahtarı kullanmaktan daha az görülen olabilir.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="9b3ac-123">Ancak OData tamsayılar yanı sıra anahtar diğer türleri nasıl işlediğini görmesini değerindedir.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="9b3ac-124">Ardından, bir ilişki ekleyerek oluşturacağız bir `Supplier` özelliğine `Product` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="9b3ac-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="9b3ac-125">Yeni bir ekleme **DbSet** için `ProductServiceContext` Entity Framework içerecek şekilde, sınıfının `Supplier` veritabanındaki tablo.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="9b3ac-126">WebApiConfig.cs içinde "Üreticiler" varlığı için EDM modeline ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9b3ac-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="9b3ac-127">Gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="9b3ac-127">Navigation Properties</span></span>

<span data-ttu-id="9b3ac-128">Sağlayıcı bir ürün almak için istemci bir GET isteği gönderir:</span><span class="sxs-lookup"><span data-stu-id="9b3ac-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="9b3ac-129">Burada "Sağlayıcı" bir gezinti özelliği açıktır `Product` türü.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="9b3ac-130">Bu durumda, `Supplier` başvuruyor tek bir öğe, ancak bir gezinti özelliği bir koleksiyon (bir çok veya çok-çok ilişkisi) de geri dönebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="9b3ac-131">Bu istek desteklemek için aşağıdaki yöntemi ekleyin `ProductsController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="9b3ac-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="9b3ac-132">*Anahtar* parametresi, ürün anahtarıdır.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="9b3ac-133">Bu durumda, ilgili varlığın & #8212 yöntemi döndürür bir `Supplier` örneği.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-133">The method returns the related entity&#8212in this case, a `Supplier` instance.</span></span> <span data-ttu-id="9b3ac-134">Yöntem adı parametre adı hem de önemli olması.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="9b3ac-135">Genel olarak, gezinme özelliğini "X" olarak adlandırılmışsa "GetX" adlı bir yöntem eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="9b3ac-136">Yöntem adlı bir parametre almalıdır "*anahtar*" üst öğenin anahtarın veri türü ile eşleşen.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="9b3ac-137">Eklenmesi önemlidir **[FromOdataUri]** özniteliğini *anahtar* parametresi.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="9b3ac-138">Bu öznitelik, istek URI'si anahtarından ayrıştırırken OData sözdizimi kurallarına kullanmak için Web API söyler.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="9b3ac-139">Oluşturma ve bağlantıları silme</span><span class="sxs-lookup"><span data-stu-id="9b3ac-139">Creating and Deleting Links</span></span>

<span data-ttu-id="9b3ac-140">OData iki varlık arasında oluşturma veya kaldırma ilişkileri destekler.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="9b3ac-141">OData terminolojisinde ilişki "." bağlantıdır</span><span class="sxs-lookup"><span data-stu-id="9b3ac-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="9b3ac-142">Her bir bağlantının form ile bir URI'ya sahip *varlık*/$links /*varlık*.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="9b3ac-143">Örneğin, ürün bağlantıdan tedarikçiye şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="9b3ac-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="9b3ac-144">Yeni bir bağlantı oluşturmak için istemci bağlantısını URI bir POST isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="9b3ac-145">İstek gövdesini hedef varlığın URI'si değil.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="9b3ac-146">Örneğin, "CTSO" anahtarına sahip bir sağlayıcı yok varsayalım.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="9b3ac-147">"Supplier('CTSO')" "Product(1)" öğesinden bir bağlantı oluşturmak için istemci aşağıdaki gibi bir isteği gönderir:</span><span class="sxs-lookup"><span data-stu-id="9b3ac-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="9b3ac-148">Bir bağlantıyı silmek için istemci bağlantısını URI silme isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="9b3ac-149">**Bağlantı oluşturma**</span><span class="sxs-lookup"><span data-stu-id="9b3ac-149">**Creating Links**</span></span>

<span data-ttu-id="9b3ac-150">Ürün tedarikçi bağlantılar oluşturmak bir istemci etkinleştirmek için aşağıdaki kodu ekleyin `ProductsController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="9b3ac-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="9b3ac-151">Bu yöntem üç parametreleri alır:</span><span class="sxs-lookup"><span data-stu-id="9b3ac-151">This method takes three parameters:</span></span>

- <span data-ttu-id="9b3ac-152">*anahtar*: üst varlık (ürün) anahtarı</span><span class="sxs-lookup"><span data-stu-id="9b3ac-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="9b3ac-153">*navigationProperty*: Gezinti özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="9b3ac-154">Bu örnekte, "Sağlayıcı" yalnızca geçerli bir gezinti özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="9b3ac-155">*Bağlantı*: ilgili varlığın OData URI.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="9b3ac-156">Bu değer, istek gövdesinden alınır.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-156">This value is taken from the request body.</span></span> <span data-ttu-id="9b3ac-157">Örneğin, bağlantı URI olabilir "`http://localhost/odata/Suppliers('CTSO')`, tedarikçi kimlikli anlamına = 'CTSO'.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="9b3ac-158">Yöntemi bağlantı tedarikçi aramak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="9b3ac-159">Eşleşen tedarikçi bulunursa, yöntem ayarlar `Product.Supplier` özelliği ve sonucu veritabanına kaydeder.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="9b3ac-160">En zor bölümü bağlantı URI ayrıştırma.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="9b3ac-161">Temel olarak, bu URI bir GET isteği göndererek sonucunu benzetimini gerekir.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="9b3ac-162">Aşağıdaki yardımcı yöntemi bunun nasıl yapılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="9b3ac-163">Bu yöntem Web API yönlendirme işlemi çağırır ve geri alır bir **ODataPath** ayrıştırılmış OData yolunu temsil eden örneği.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="9b3ac-164">Bağlantı için URI, kesimleri birini Varlık anahtarı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="9b3ac-165">(Aksi durumda, istemci geçersiz bir URI gönderilen.)</span><span class="sxs-lookup"><span data-stu-id="9b3ac-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="9b3ac-166">**Bağlantıları silme**</span><span class="sxs-lookup"><span data-stu-id="9b3ac-166">**Deleting Links**</span></span>

<span data-ttu-id="9b3ac-167">Bir bağlantıyı silmek için aşağıdaki kodu ekleyin `ProductsController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="9b3ac-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="9b3ac-168">Bu örnekte, tek bir gezinti özelliği olduğundan `Supplier` varlık.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="9b3ac-169">Bir koleksiyon gezinti özelliği olduğundan, bir bağlantıyı silmek için URI ilgili varlık için bir anahtar içermelidir.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="9b3ac-170">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="9b3ac-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="9b3ac-171">Bu istek sırası 1 müşteri 1 ' kaldırır.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="9b3ac-172">Bu durumda, DeleteLink yöntemi aşağıdaki imzası olacaktır:</span><span class="sxs-lookup"><span data-stu-id="9b3ac-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="9b3ac-173">*RelatedKey* parametresi ilgili varlık için anahtar sağlar.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="9b3ac-174">Bunu içinde `DeleteLink` yöntemi, birincil varlık tarafından Ara *anahtar* parametresi, ilgili varlık tarafından Bul *relatedKey* parametre ve ardından Kaldır ilişkilendirme.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="9b3ac-175">Veri modeline bağlı olarak, her iki sürümünün de uygulamanız gerekebilir `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="9b3ac-176">Web API istek URI'SİNDEKİ doğru sürümü çağırır.</span><span class="sxs-lookup"><span data-stu-id="9b3ac-176">Web API will call the correct version based on the request URI.</span></span>
