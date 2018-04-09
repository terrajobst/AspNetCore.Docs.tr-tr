---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Bölüm 6: Ürün ve sipariş denetleyicileri oluşturma | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6bd485d29821af12b9ebe31b2d04a2d9ab826731
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="0ccc4-102">Bölüm 6: Ürün oluşturma ve sipariş denetleyicileri</span><span class="sxs-lookup"><span data-stu-id="0ccc4-102">Part 6: Creating Product and Order Controllers</span></span>
====================
<span data-ttu-id="0ccc4-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0ccc4-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="0ccc4-104">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="0ccc4-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="0ccc4-105">Ürünler denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="0ccc4-105">Add a Products Controller</span></span>

<span data-ttu-id="0ccc4-106">Yönetim Denetleyicisi yönetici ayrıcalıklarına sahip kullanıcılar için değil.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="0ccc4-107">Müşteriler, diğer yandan, ürünleri görüntüleyebilir ancak olamaz oluşturmak, güncelleştirmek veya silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="0ccc4-108">Biz kolay erişim, Get yöntemleri açık bırakarak Post, Put ve silme yöntemleri kısıtlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="0ccc4-109">Ancak, bir ürün döndürülen verileri bakın:</span><span class="sxs-lookup"><span data-stu-id="0ccc4-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="0ccc4-110">`ActualCost` Özelliği müşterilerin görebildiği olmalıdır!</span><span class="sxs-lookup"><span data-stu-id="0ccc4-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="0ccc4-111">Çözüm tanımlamaktır bir *veri aktarım nesnesini* (DTO) içeren bir alt kümesini müşterilere görünmesi gereken özellikleri.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="0ccc4-112">LINQ projeye kullanacağız `Product` için örnekleri `ProductDTO` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="0ccc4-113">Adlı bir sınıf ekleyin `ProductDTO` modeller klasörü için.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="0ccc4-114">Şimdi denetleyici ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-114">Now add the controller.</span></span> <span data-ttu-id="0ccc4-115">Çözüm Gezgini'nde denetleyicileri klasörü sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="0ccc4-116">Seçin **Ekle**seçeneğini belirleyip **denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="0ccc4-117">İçinde **denetleyici Ekle** iletişim kutusunda, denetleyici adı &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="0ccc4-118">Altında **şablonu**seçin **boş API denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="0ccc4-119">Kaynak dosyasında her şeyi aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="0ccc4-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="0ccc4-120">Denetleyici hala kullanan `OrdersContext` veritabanını sorgulamak için.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="0ccc4-121">Ancak döndürme yerine `Product` örnekleri doğrudan diyoruz `MapProducts` açtığına projeye `ProductDTO` örnekleri:</span><span class="sxs-lookup"><span data-stu-id="0ccc4-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="0ccc4-122">`MapProducts` Yöntemi döndürür bir **Iqueryable**, biz sonucu diğer sorgu parametreleri ile oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="0ccc4-123">Bu konuda bkz `GetProduct` ekler yöntemi bir **burada** sorgu yan tümcesini:</span><span class="sxs-lookup"><span data-stu-id="0ccc4-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="0ccc4-124">Siparişleri denetleyici ekleyin</span><span class="sxs-lookup"><span data-stu-id="0ccc4-124">Add an Orders Controller</span></span>

<span data-ttu-id="0ccc4-125">Ardından, oluşturma ve siparişleri görüntüleme kullanıcıların olanak sağlayan bir denetleyici ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="0ccc4-126">Başka bir DTO ile başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-126">We'll start with another DTO.</span></span> <span data-ttu-id="0ccc4-127">Çözüm Gezgini'nde, modeller klasörü sağ tıklatın ve adlı bir sınıf ekleyin `OrderDTO` aşağıdaki uygulama kullanın:</span><span class="sxs-lookup"><span data-stu-id="0ccc4-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="0ccc4-128">Şimdi denetleyici ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-128">Now add the controller.</span></span> <span data-ttu-id="0ccc4-129">Çözüm Gezgini'nde denetleyicileri klasörü sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="0ccc4-130">Seçin **Ekle**seçeneğini belirleyip **denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="0ccc4-131">İçinde **denetleyici Ekle** iletişim kutusunda, aşağıdaki seçenekleri belirleyin:</span><span class="sxs-lookup"><span data-stu-id="0ccc4-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="0ccc4-132">Altında **Denetleyici adı**, "OrdersController" girin.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="0ccc4-133">Altında **şablonu**seçin "Entity Framework kullanarak okuma/yazma eylemlerine sahip API denetleyicisi".</span><span class="sxs-lookup"><span data-stu-id="0ccc4-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="0ccc4-134">Altında **Model sınıfı**seçin &quot;sipariş (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="0ccc4-135">Altında **veri bağlamı sınıfı**seçin &quot;OrdersContext (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="0ccc4-136">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-136">Click **Add**.</span></span> <span data-ttu-id="0ccc4-137">Bu OrdersController.cs adlı bir dosya ekler.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="0ccc4-138">Ardından, biz denetleyicisi varsayılan uygulaması değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="0ccc4-139">İlk olarak, Sil `PutOrder` ve `DeleteOrder` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="0ccc4-140">Bu örnek, müşterilere değiştiremez veya varolan siparişleri Sil.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="0ccc4-141">Gerçek bir uygulamada bu gibi durumlarda arka uç mantığı çok sayıda gerekir.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="0ccc4-142">(Örneğin, sipariş zaten sevk edilen?)</span><span class="sxs-lookup"><span data-stu-id="0ccc4-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="0ccc4-143">Değişiklik `GetOrders` yöntemi yalnızca bir kullanıcıya ait siparişleri döndürmek için:</span><span class="sxs-lookup"><span data-stu-id="0ccc4-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="0ccc4-144">Değişiklik `GetOrder` yöntemini aşağıdaki şekilde:</span><span class="sxs-lookup"><span data-stu-id="0ccc4-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="0ccc4-145">Biz yönteme yapılan değişiklikler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="0ccc4-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="0ccc4-146">Dönüş değeri bir `OrderDTO` örneği yerine bir `Order`.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="0ccc4-147">Biz sipariş için veritabanı sorguladığınızda kullanırız [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) ilgili getirilemedi yöntemi `OrderDetail` ve `Product` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="0ccc4-148">Biz, sonuç bir yansıtma kullanarak düzleştirmek.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="0ccc4-149">HTTP yanıtı miktarları ürünleriyle dizisi içerir:</span><span class="sxs-lookup"><span data-stu-id="0ccc4-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="0ccc4-150">Bu biçim, iç içe geçmiş varlıkları (sipariş, Ayrıntılar ve ürünleri) içeren özgün nesne grafiği, kullanmak istemcileri için kolaydır.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="0ccc4-151">Bu dikkate alınması gereken son yöntemi `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="0ccc4-152">Şu anda, bu yöntem alır bir `Order` örneği.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="0ccc4-153">Ancak ne olacağını düşünün bir istemci şöyle bir istek gövdesi gönderirse:</span><span class="sxs-lookup"><span data-stu-id="0ccc4-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="0ccc4-154">Bu iyi yapılandırılmış bir sipariş ve Entity Framework sonsuza dek bu veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="0ccc4-155">Ancak önceden var olmayan bir ürün varlık içeriyor.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="0ccc4-156">Yalnızca yeni bir ürün bizim veritabanında oluşturulan istemci!</span><span class="sxs-lookup"><span data-stu-id="0ccc4-156">The client just created a new product in our database!</span></span> <span data-ttu-id="0ccc4-157">Koala bears için bir sıra gördüğünüzde Bu sipariş fullfilment departmanına beklenmedik biçimde olacaktır.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-157">This will be a suprise to the order fullfilment department, when they see an order for koala bears.</span></span> <span data-ttu-id="0ccc4-158">Ahlaki, gerçekten bir POST veya PUT isteği kabul veriler hakkında dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="0ccc4-159">Bu sorunu önlemek için değiştirme `PostOrder` yapılacak yöntemi bir `OrderDTO` örneği.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="0ccc4-160">Kullanım `OrderDTO` oluşturmak için `Order`.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="0ccc4-161">Kullanırız bildirimi `ProductID` ve `Quantity` özellikleri ve biz yoksay ürün adı veya fiyat için istemciye gönderilen herhangi bir değeri.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="0ccc4-162">Ürün Kimliği geçerli değilse, veritabanındaki yabancı anahtar kısıtlamasını ihlal ve gerektiği gibi ekleme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="0ccc4-163">İşte tam `PostOrder` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="0ccc4-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="0ccc4-164">Son olarak, ekleme **Authorize** özniteliği denetleyiciye:</span><span class="sxs-lookup"><span data-stu-id="0ccc4-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="0ccc4-165">Artık yalnızca kayıtlı kullanıcıların oluşturun veya siparişleri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="0ccc4-165">Now only registered users can create or view orders.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0ccc4-166">[Önceki](using-web-api-with-entity-framework-part-5.md)
> [sonraki](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="0ccc4-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
