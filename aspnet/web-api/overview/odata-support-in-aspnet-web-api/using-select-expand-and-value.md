---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: "$Select kullanarak $genişletin ve ASP.NET Web API 2 OData $value | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="8d098-102">$Select kullanarak $genişletin ve ASP.NET Web API 2 OData $value</span><span class="sxs-lookup"><span data-stu-id="8d098-102">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="8d098-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8d098-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8d098-104">İçin $expand, $select ve OData $value seçeneklerinde web API 2 desteği ekler.</span><span class="sxs-lookup"><span data-stu-id="8d098-104">Web API 2 adds support for the $expand, $select, and $value options in OData.</span></span> <span data-ttu-id="8d098-105">Bu seçenekler sunucudan geri alır gösterimi denetlemek bir istemci sağlar.</span><span class="sxs-lookup"><span data-stu-id="8d098-105">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="8d098-106">**$expand** yanıta dahil satır içi olarak ilgili varlıklar neden olur.</span><span class="sxs-lookup"><span data-stu-id="8d098-106">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="8d098-107">**$select** yanıta dahil edilecek özellik kümesini seçer.</span><span class="sxs-lookup"><span data-stu-id="8d098-107">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="8d098-108">**$value** özelliğinin ham değeri alır.</span><span class="sxs-lookup"><span data-stu-id="8d098-108">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="8d098-109">Şema örneği</span><span class="sxs-lookup"><span data-stu-id="8d098-109">Example Schema</span></span>

<span data-ttu-id="8d098-110">Bu makalede, üç varlık tanımlayan OData hizmetine kullanmam: Ürün, üretici ve kategori.</span><span class="sxs-lookup"><span data-stu-id="8d098-110">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="8d098-111">Her ürünün bir kategori ve bir sağlayıcı vardır.</span><span class="sxs-lookup"><span data-stu-id="8d098-111">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="8d098-112">Varlık modelleri tanımlayan C# sınıfları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="8d098-112">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="8d098-113">Dikkat `Product` sınıfı tanımlayan Gezinti özellikleri için `Supplier` ve `Category`.</span><span class="sxs-lookup"><span data-stu-id="8d098-113">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="8d098-114">`Category` Sınıfı, her kategoride ürünlere yönelik bir gezinti özelliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8d098-114">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="8d098-115">Bu şema için bir OData uç nokta oluşturmak için Visual Studio 2013 yapı iskelesi açıklandığı gibi kullanın [ASP.NET Web API'de OData uç noktası oluşturma](odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="8d098-115">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="8d098-116">Ürün, kategori ve sağlayıcı için ayrı denetleyicileri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8d098-116">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="8d098-117">Etkinleştirme $genişletin ve $select</span><span class="sxs-lookup"><span data-stu-id="8d098-117">Enabling $expand and $select</span></span>

<span data-ttu-id="8d098-118">Visual Studio 2013'te Web API OData yapı iskelesi bu otomatik olarak $expand destekler ve $select bir denetleyiciyi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8d098-118">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="8d098-119">Başvuru için burada açıklanmıştır desteklemeyi $ gereksinimleri genişletin ve bir denetleyicide $select.</span><span class="sxs-lookup"><span data-stu-id="8d098-119">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="8d098-120">Koleksiyonlar için denetleyici 's `Get` yöntemi döndürmelidir bir **Iqueryable**.</span><span class="sxs-lookup"><span data-stu-id="8d098-120">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="8d098-121">Tek varlıklar için dönüş bir **SingleResult&lt;T&gt;**, T değerinin olduğu bir **Iqueryable** sıfır veya bir varlık içeriyor.</span><span class="sxs-lookup"><span data-stu-id="8d098-121">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="8d098-122">Ayrıca, tasarlamanız, `Get` yöntemleriyle **[Queryable]** önceki kod parçacıkları gösterildiği gibi öznitelik.</span><span class="sxs-lookup"><span data-stu-id="8d098-122">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="8d098-123">Alternatif olarak, arama **EnableQuerySupport** üzerinde **HttpConfiguration** başlangıçta nesnesi.</span><span class="sxs-lookup"><span data-stu-id="8d098-123">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="8d098-124">(Daha fazla bilgi için bkz: [etkinleştirme OData sorgu seçeneklerini](supporting-odata-query-options.md#enable).)</span><span class="sxs-lookup"><span data-stu-id="8d098-124">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="8d098-125">Kullanarak $Genişlet</span><span class="sxs-lookup"><span data-stu-id="8d098-125">Using $expand</span></span>

<span data-ttu-id="8d098-126">Bir OData varlık veya koleksiyon sorguladığınızda, varsayılan yanıt ilgili varlıklar içermez.</span><span class="sxs-lookup"><span data-stu-id="8d098-126">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="8d098-127">Örneğin, kategoriler varlık kümesi için varsayılan yanıt şöyledir:</span><span class="sxs-lookup"><span data-stu-id="8d098-127">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="8d098-128">Gördüğünüz gibi ürünler Gezinti bağlantısı kategori varlık sahip olsa bile yanıt herhangi bir ürünü içermez.</span><span class="sxs-lookup"><span data-stu-id="8d098-128">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="8d098-129">Ancak istemcinin kullanabileceği $ her kategori için ürünlerin listesini almak için genişletin.</span><span class="sxs-lookup"><span data-stu-id="8d098-129">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="8d098-130">$Expand seçeneği isteğinin sorgu dizesinde geçer:</span><span class="sxs-lookup"><span data-stu-id="8d098-130">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="8d098-131">Artık sunucu ürünleri kategorileri satır içi her kategori için dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="8d098-131">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="8d098-132">Yanıt yükü şöyledir:</span><span class="sxs-lookup"><span data-stu-id="8d098-132">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="8d098-133">"Value" dizisindeki her giriş ürünlerin listesini içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="8d098-133">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="8d098-134">$Expand genişletilecek Gezinti özelliklerini seçeneği alır virgülle ayrılmış listesi.</span><span class="sxs-lookup"><span data-stu-id="8d098-134">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="8d098-135">Aşağıdaki isteği, kategori ve bir ürün için tedarikçiye genişletir.</span><span class="sxs-lookup"><span data-stu-id="8d098-135">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="8d098-136">Yanıt gövdesi şöyledir:</span><span class="sxs-lookup"><span data-stu-id="8d098-136">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="8d098-137">Gezinti özelliğinin birden fazla düzey genişletebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8d098-137">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="8d098-138">Aşağıdaki örnek, bir kategorideki tüm ürünleri ve ayrıca her ürünün sağlayıcısına içerir.</span><span class="sxs-lookup"><span data-stu-id="8d098-138">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="8d098-139">Yanıt gövdesi şöyledir:</span><span class="sxs-lookup"><span data-stu-id="8d098-139">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="8d098-140">Varsayılan olarak, Web API 2 en büyük genişletme derinliğini sınırlar.</span><span class="sxs-lookup"><span data-stu-id="8d098-140">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="8d098-141">Engelleyen istemci gibi karmaşık istekleri göndermesinin `$expand=Orders/OrderDetails/Product/Supplier/Region`, hangi olabilir sorgulamak ve geniş yanıtları oluşturmak için yetersiz.</span><span class="sxs-lookup"><span data-stu-id="8d098-141">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="8d098-142">Varsayılan yerine geçecek şekilde ayarlanmış **MaxExpansionDepth** özelliği **[Queryable]** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8d098-142">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="8d098-143">Seçeneğini genişletin $ hakkında daha fazla bilgi için bkz: [sistem sorgulama ($expand) seçeneğini genişletin](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) resmi OData belgelerinde.</span><span class="sxs-lookup"><span data-stu-id="8d098-143">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="8d098-144">$Select kullanma</span><span class="sxs-lookup"><span data-stu-id="8d098-144">Using $select</span></span>

<span data-ttu-id="8d098-145">$Select seçeneği yanıt gövdesinde dahil edilecek özellik kümesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="8d098-145">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="8d098-146">Örneğin, yalnızca adı ve her ürünün fiyatı almak için aşağıdaki sorguyu kullanın:</span><span class="sxs-lookup"><span data-stu-id="8d098-146">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="8d098-147">Yanıt gövdesi şöyledir:</span><span class="sxs-lookup"><span data-stu-id="8d098-147">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="8d098-148">$Select birleştirebilirsiniz ve $ aynı sorguda genişletin.</span><span class="sxs-lookup"><span data-stu-id="8d098-148">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="8d098-149">$Select seçeneğinde genişletilmiş özellik eklediğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="8d098-149">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="8d098-150">Örneğin, aşağıdaki isteği üretici ve ürün adını alır.</span><span class="sxs-lookup"><span data-stu-id="8d098-150">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="8d098-151">Yanıt gövdesi şöyledir:</span><span class="sxs-lookup"><span data-stu-id="8d098-151">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="8d098-152">Özellikleri genişletilmiş bir özellik içinde de seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8d098-152">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="8d098-153">Aşağıdaki isteği ürünleri genişletir ve kategori adı ve ürün adı seçer.</span><span class="sxs-lookup"><span data-stu-id="8d098-153">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="8d098-154">Yanıt gövdesi şöyledir:</span><span class="sxs-lookup"><span data-stu-id="8d098-154">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="8d098-155">$Select seçeneği hakkında daha fazla bilgi için bkz: [seçin sistem sorgusu seçeneği ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) resmi OData belgelerinde.</span><span class="sxs-lookup"><span data-stu-id="8d098-155">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="8d098-156">Tek bir varlık ($value) özelliklerini alma</span><span class="sxs-lookup"><span data-stu-id="8d098-156">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="8d098-157">Tek bir özellik bir varlıktan almak bir OData istemcisi için iki yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="8d098-157">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="8d098-158">İstemci değeri OData biçiminde almak veya özelliğin ham değeri alır.</span><span class="sxs-lookup"><span data-stu-id="8d098-158">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="8d098-159">Aşağıdaki isteği OData biçiminde bir özelliğini alır.</span><span class="sxs-lookup"><span data-stu-id="8d098-159">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="8d098-160">JSON biçiminde bir örnek yanıt şöyledir:</span><span class="sxs-lookup"><span data-stu-id="8d098-160">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="8d098-161">Özelliğin ham değeri almak için $value uri'ye ekler:</span><span class="sxs-lookup"><span data-stu-id="8d098-161">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="8d098-162">Burada, yanıt verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8d098-162">Here is the response.</span></span> <span data-ttu-id="8d098-163">İçerik türü "metin/düz" JSON olmadığına dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="8d098-163">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="8d098-164">Bu sorgular, OData denetleyicisi desteklemek için adlı bir yöntem eklemek `GetProperty`, burada `Property` özelliği adıdır.</span><span class="sxs-lookup"><span data-stu-id="8d098-164">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="8d098-165">Örneğin, Name özelliği almak için yöntemi adlı `GetName`.</span><span class="sxs-lookup"><span data-stu-id="8d098-165">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="8d098-166">Yöntemi, o özelliğin değerini döndürmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="8d098-166">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
