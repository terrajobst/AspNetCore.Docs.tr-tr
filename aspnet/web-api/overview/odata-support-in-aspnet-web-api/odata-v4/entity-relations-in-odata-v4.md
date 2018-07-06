---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: OData v4 sürümünde varlık ilişkilerini kullanarak ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: 'Çoğu veri kümesinde varlıklar arasındaki ilişkileri tanımlamak: imajlarını siparişler; Books Yazar; yine de olan istiyor musunuz? Ürünler tedarikçileri sahip. OData kullanarak istemcileri üzerinden gidebilirsiniz...'
ms.author: aspnetcontent
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 98f65b068d8f22e3eeef48ca7fa441434939db8b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827952"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="3ff99-104">OData v4 sürümünde varlık ilişkilerini kullanarak ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="3ff99-104">Entity Relations in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="3ff99-105">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3ff99-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="3ff99-106">Çoğu veri kümesinde varlıklar arasındaki ilişkileri tanımlamak: imajlarını siparişler; Books Yazar; yine de olan istiyor musunuz? Ürünler tedarikçileri sahip.</span><span class="sxs-lookup"><span data-stu-id="3ff99-106">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="3ff99-107">İstemciler, OData kullanarak, varlık ilişkileri gidebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3ff99-107">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="3ff99-108">Bir ürün göz önünde bulundurulduğunda, tedarikçi bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3ff99-108">Given a product, you can find the supplier.</span></span> <span data-ttu-id="3ff99-109">Ayrıca, oluşturmak veya ilişkileri kaldırın.</span><span class="sxs-lookup"><span data-stu-id="3ff99-109">You can also create or remove relationships.</span></span> <span data-ttu-id="3ff99-110">Örneğin, bir ürünün sağlayıcısına ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3ff99-110">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="3ff99-111">Bu öğreticide, ASP.NET Web API'sini kullanarak bu işlemleri OData v4 sürümünde destekleyen gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="3ff99-111">This tutorial shows how to support these operations in OData v4 using ASP.NET Web API.</span></span> <span data-ttu-id="3ff99-112">Öğretici öğreticiyi genişletip yapılar [OData v4 uç noktası kullanarak ASP.NET Web API 2 oluşturma](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="3ff99-112">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3ff99-113">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="3ff99-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3ff99-114">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="3ff99-114">Web API 2.1</span></span>
> - <span data-ttu-id="3ff99-115">OData v4</span><span class="sxs-lookup"><span data-stu-id="3ff99-115">OData v4</span></span>
> - [<span data-ttu-id="3ff99-116">Visual Studio 2013 Güncelleştirme 2</span><span class="sxs-lookup"><span data-stu-id="3ff99-116">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="3ff99-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="3ff99-117">Entity Framework 6</span></span>
> - <span data-ttu-id="3ff99-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="3ff99-118">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="3ff99-119">Öğretici sürümleri</span><span class="sxs-lookup"><span data-stu-id="3ff99-119">Tutorial versions</span></span>
> 
> <span data-ttu-id="3ff99-120">OData sürüm 3 için bkz: [OData v3 sürümünde varlık ilişkileri'ı destekleyen](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span><span class="sxs-lookup"><span data-stu-id="3ff99-120">For the OData Version 3, see [Supporting Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="3ff99-121">Tedarikçi varlık ekleme</span><span class="sxs-lookup"><span data-stu-id="3ff99-121">Add a Supplier Entity</span></span>

> [!NOTE]
> <span data-ttu-id="3ff99-122">Öğretici öğreticiyi genişletip yapılar [OData v4 uç noktası kullanarak ASP.NET Web API 2 oluşturma](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="3ff99-122">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>


<span data-ttu-id="3ff99-123">İlk olarak, ilgili varlık gerekir.</span><span class="sxs-lookup"><span data-stu-id="3ff99-123">First, we need a related entity.</span></span> <span data-ttu-id="3ff99-124">Adlı bir sınıf ekleyin `Supplier` modeller klasörü içinde.</span><span class="sxs-lookup"><span data-stu-id="3ff99-124">Add a class named `Supplier` in the Models folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

<span data-ttu-id="3ff99-125">Bir gezinme özelliği için ekleme `Product` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="3ff99-125">Add a navigation property to the `Product` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

<span data-ttu-id="3ff99-126">Yeni bir **olan DB** için `ProductsContext` Entity Framework Sağlayıcısı tablo veritabanında içermesi sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3ff99-126">Add a new **DbSet** to the `ProductsContext` class, so that Entity Framework will include the Supplier table in the database.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

<span data-ttu-id="3ff99-127">WebApiConfig.cs içinde ekleme bir &quot;tedarikçileri&quot; varlık kümesi için varlık veri modeli:</span><span class="sxs-lookup"><span data-stu-id="3ff99-127">In WebApiConfig.cs, add a &quot;Suppliers&quot; entity set to the entity data model:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a><span data-ttu-id="3ff99-128">Üreticiler denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="3ff99-128">Add a Suppliers Controller</span></span>

<span data-ttu-id="3ff99-129">Ekleme bir `SuppliersController` denetleyicileri klasörüne sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3ff99-129">Add a `SuppliersController` class to the Controllers folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="3ff99-130">Ben CRUD işlemleri Bu denetleyici ekleme gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="3ff99-130">I won't show how to add CRUD operations for this controller.</span></span> <span data-ttu-id="3ff99-131">Adımları ürünleri denetleyicisi aynıdır (bkz [bir OData v4 uç noktası oluşturma](create-an-odata-v4-endpoint.md)).</span><span class="sxs-lookup"><span data-stu-id="3ff99-131">The steps are the same as for the Products controller (see [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md)).</span></span>

## <a name="getting-related-entities"></a><span data-ttu-id="3ff99-132">Başlangıç ilgili varlıklar</span><span class="sxs-lookup"><span data-stu-id="3ff99-132">Getting Related Entities</span></span>

<span data-ttu-id="3ff99-133">Sağlayıcı için bir ürün almak için istemci bir GET isteği gönderir:</span><span class="sxs-lookup"><span data-stu-id="3ff99-133">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

<span data-ttu-id="3ff99-134">Bu istek desteklemek için aşağıdaki yöntemi ekleyin. `ProductsController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="3ff99-134">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

<span data-ttu-id="3ff99-135">Bu yöntem varsayılan bir adlandırma kuralını kullanır.</span><span class="sxs-lookup"><span data-stu-id="3ff99-135">This method uses a default naming convention</span></span>

- <span data-ttu-id="3ff99-136">Yöntem adı: GetX, burada X, gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="3ff99-136">Method name: GetX, where X is the navigation property.</span></span>
- <span data-ttu-id="3ff99-137">Parametre adı: *anahtarı*</span><span class="sxs-lookup"><span data-stu-id="3ff99-137">Parameter name: *key*</span></span>

<span data-ttu-id="3ff99-138">Bu adlandırma kuralını izler, Web API denetleyicisi yöntemi HTTP isteği otomatik olarak eşler.</span><span class="sxs-lookup"><span data-stu-id="3ff99-138">If you follow this naming convention, Web API automatically maps the HTTP request to the controller method.</span></span>

<span data-ttu-id="3ff99-139">Örnek HTTP istek:</span><span class="sxs-lookup"><span data-stu-id="3ff99-139">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

<span data-ttu-id="3ff99-140">Örnek HTTP yanıtı:</span><span class="sxs-lookup"><span data-stu-id="3ff99-140">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a><span data-ttu-id="3ff99-141">İlgili koleksiyonu alma</span><span class="sxs-lookup"><span data-stu-id="3ff99-141">Getting a related collection</span></span>

<span data-ttu-id="3ff99-142">Önceki örnekte, bir ürün bir tedarikçi sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3ff99-142">In the previous example, a product has one supplier.</span></span> <span data-ttu-id="3ff99-143">Bir gezinme özelliği, bir koleksiyon da döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="3ff99-143">A navigation property can also return a collection.</span></span> <span data-ttu-id="3ff99-144">Aşağıdaki kod ürünleri için bir sağlayıcı alır:</span><span class="sxs-lookup"><span data-stu-id="3ff99-144">The following code gets the products for a supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

<span data-ttu-id="3ff99-145">Bu durumda, yöntem döndürür bir **Iqueryable** yerine bir **SingleResult&lt;T&gt;**</span><span class="sxs-lookup"><span data-stu-id="3ff99-145">In this case, the method returns an **IQueryable** instead of a **SingleResult&lt;T&gt;**</span></span>

<span data-ttu-id="3ff99-146">Örnek HTTP istek:</span><span class="sxs-lookup"><span data-stu-id="3ff99-146">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

<span data-ttu-id="3ff99-147">Örnek HTTP yanıtı:</span><span class="sxs-lookup"><span data-stu-id="3ff99-147">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a><span data-ttu-id="3ff99-148">Varlıklar arasında ilişki oluşturma</span><span class="sxs-lookup"><span data-stu-id="3ff99-148">Creating a Relationship Between Entities</span></span>

<span data-ttu-id="3ff99-149">OData oluşturmaya veya var olan iki varlık arasında ilişkiler destekler.</span><span class="sxs-lookup"><span data-stu-id="3ff99-149">OData supports creating or removing relationships between two existing entities.</span></span> <span data-ttu-id="3ff99-150">OData v4 terminolojisinde ilişkidir bir &quot;başvuru&quot;.</span><span class="sxs-lookup"><span data-stu-id="3ff99-150">In OData v4 terminology, the relationship is a &quot;reference&quot;.</span></span> <span data-ttu-id="3ff99-151">(OData v3 sürümünde ilişki çağrıldı bir *bağlantı*.</span><span class="sxs-lookup"><span data-stu-id="3ff99-151">(In OData v3, the relationship was called a *link*.</span></span> <span data-ttu-id="3ff99-152">Protokol farklar Bu öğretici için önemli değildir.)</span><span class="sxs-lookup"><span data-stu-id="3ff99-152">The protocol differences don't matter for this tutorial.)</span></span>

<span data-ttu-id="3ff99-153">Başvuru formunu kendi URI'sine sahip `/Entity/NavigationProperty/$ref`.</span><span class="sxs-lookup"><span data-stu-id="3ff99-153">A reference has its own URI, with the form `/Entity/NavigationProperty/$ref`.</span></span> <span data-ttu-id="3ff99-154">Örneğin, bir ürün, tedarikçi arasındaki başvuru adres için URI şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="3ff99-154">For example, here is the URI to address the reference between a product and its supplier:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

<span data-ttu-id="3ff99-155">İstemci bir ilişki eklemek için bu adrese bir POST veya PUT İsteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="3ff99-155">To add a relationship, the client sends a POST or PUT request to this address.</span></span>

- <span data-ttu-id="3ff99-156">Gezinti özelliği tek bir varlık gibi ise PUT `Product.Supplier`.</span><span class="sxs-lookup"><span data-stu-id="3ff99-156">PUT if the navigation property is a single entity, such as `Product.Supplier`.</span></span>
- <span data-ttu-id="3ff99-157">Bir koleksiyon gezinme özelliği olduğu gibi istiyorsanız `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="3ff99-157">POST if the navigation property is a collection, such as `Supplier.Products`.</span></span>

<span data-ttu-id="3ff99-158">İstek gövdesi, başka bir varlık ilişkisi URI'sini içerir.</span><span class="sxs-lookup"><span data-stu-id="3ff99-158">The body of the request contains the URI of the other entity in the relation.</span></span> <span data-ttu-id="3ff99-159">Bir örnek İstek şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="3ff99-159">Here is an example request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

<span data-ttu-id="3ff99-160">Bu örnekte, istemci bir PUT İsteği gönderir. `/Products(6)/Supplier/$ref`, $ref URI'sini olduğu `Supplier` = 6 ürünün Kimliğine sahip.</span><span class="sxs-lookup"><span data-stu-id="3ff99-160">In this example, the client sends a PUT request to `/Products(6)/Supplier/$ref`, which is the $ref URI for the `Supplier` of the product with ID = 6.</span></span> <span data-ttu-id="3ff99-161">İstek başarılı olursa sunucu 204 (içerik yok) yanıt gönderir:</span><span class="sxs-lookup"><span data-stu-id="3ff99-161">If the request succeeds, the server sends a 204 (No Content) response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

<span data-ttu-id="3ff99-162">Bir ilişki eklemek için denetleyici yöntemi İşte bir `Product`:</span><span class="sxs-lookup"><span data-stu-id="3ff99-162">Here is the controller method to add a relationship to a `Product`:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

<span data-ttu-id="3ff99-163">*NavigationProperty* parametre ayarlamak için hangi ilişki belirtir.</span><span class="sxs-lookup"><span data-stu-id="3ff99-163">The *navigationProperty* parameter specifies which relationship to set.</span></span> <span data-ttu-id="3ff99-164">(Varlık üzerinde birden fazla gezinti özelliği varsa, daha fazla ekleyebilirsiniz `case` deyimleri.)</span><span class="sxs-lookup"><span data-stu-id="3ff99-164">(If there is more than one navigation property on the entity, you can add more `case` statements.)</span></span>

<span data-ttu-id="3ff99-165">*Bağlantı* parametresi tedarikçi URI'sini içerir.</span><span class="sxs-lookup"><span data-stu-id="3ff99-165">The *link* parameter contains the URI of the supplier.</span></span> <span data-ttu-id="3ff99-166">Web API'si, otomatik olarak bu parametre için değer almak için istek gövdesini ayrıştırır.</span><span class="sxs-lookup"><span data-stu-id="3ff99-166">Web API automatically parses the request body to get the value for this parameter.</span></span>

<span data-ttu-id="3ff99-167">Tedarikçi bakmak için kimliği (veya anahtar) ihtiyacımız parçası olduğu *bağlantı* parametresi.</span><span class="sxs-lookup"><span data-stu-id="3ff99-167">To look up the supplier, we need the ID (or key), which is part of the *link* parameter.</span></span> <span data-ttu-id="3ff99-168">Bunu yapmak için aşağıdaki yardımcı yöntemini kullanın:</span><span class="sxs-lookup"><span data-stu-id="3ff99-168">To do this, use the following helper method:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

<span data-ttu-id="3ff99-169">Temel olarak, bu yöntem, URI yolu parçalara bölmek için anahtarı içeren kesim bulup anahtarı doğru türe dönüştürmeniz OData kitaplığı kullanır.</span><span class="sxs-lookup"><span data-stu-id="3ff99-169">Basically, this method uses the OData library to split the URI path into segments, find the segment that contains the key, and convert the key into the correct type.</span></span>

## <a name="deleting-a-relationship-between-entities"></a><span data-ttu-id="3ff99-170">Varlıklar arasında ilişki siliniyor</span><span class="sxs-lookup"><span data-stu-id="3ff99-170">Deleting a Relationship Between Entities</span></span>

<span data-ttu-id="3ff99-171">Bir ilişkiyi silmek için istemci için $ref URI HTTP DELETE isteği gönderir:</span><span class="sxs-lookup"><span data-stu-id="3ff99-171">To delete a relationship, the client sends an HTTP DELETE request to the $ref URI:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

<span data-ttu-id="3ff99-172">Bir ürün ve tedarikçi arasındaki ilişkiyi silmek için denetleyici yöntemi aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="3ff99-172">Here is the controller method to delete the relationship between a Product and a Supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

<span data-ttu-id="3ff99-173">Bu durumda, `Product.Supplier` olduğu &quot;1&quot; yalnızca ayarlayarak ilişki kaldırabilmeniz için 1-çok ilişkisi sonuna `Product.Supplier` için `null`.</span><span class="sxs-lookup"><span data-stu-id="3ff99-173">In this case, `Product.Supplier` is the &quot;1&quot; end of a 1-to-many relation, so you can remove the relationship just by setting `Product.Supplier` to `null`.</span></span>

<span data-ttu-id="3ff99-174">İçinde &quot;birçok&quot; istemci bir ilişki sonu, varlık kaldırmak için ilgili belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="3ff99-174">In the &quot;many&quot; end of a relationship, the client must specify which related entity to remove.</span></span> <span data-ttu-id="3ff99-175">Bunu yapmak için istemci isteğinin sorgu dizesinde ilgili varlığın URI'si gönderir.</span><span class="sxs-lookup"><span data-stu-id="3ff99-175">To do so, the client sends the URI of the related entity in the query string of the request.</span></span> <span data-ttu-id="3ff99-176">Örneğin, "Ürün 1" kaldırmak için "1" sağlayıcısı:</span><span class="sxs-lookup"><span data-stu-id="3ff99-176">For example, to remove "Product 1" from "Supplier 1":</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

<span data-ttu-id="3ff99-177">Web API'de Bunu desteklemek için ek bir parametre içerecek şekilde ihtiyacımız `DeleteRef` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3ff99-177">To support this in Web API, we need to include an extra parameter in the `DeleteRef` method.</span></span> <span data-ttu-id="3ff99-178">İşte bir üründen kaldırmak için denetleyici yöntemi `Supplier.Products` ilişkisi.</span><span class="sxs-lookup"><span data-stu-id="3ff99-178">Here is the controller method to remove a product from the `Supplier.Products` relation.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

<span data-ttu-id="3ff99-179">*Anahtarı* tedarikçi, anahtarı bir parametredir ve *relatedKey* parametredir kaldırmak ürün anahtarı `Products` ilişki.</span><span class="sxs-lookup"><span data-stu-id="3ff99-179">The *key* parameter is the key for the supplier, and the *relatedKey* parameter is the key for the product to remove from the `Products` relationship.</span></span> <span data-ttu-id="3ff99-180">Web API Sorgu dizesinden otomatik olarak anahtarı alır unutmayın.</span><span class="sxs-lookup"><span data-stu-id="3ff99-180">Note that Web API automatically gets the key from the query string.</span></span>
