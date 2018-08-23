---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: '2. Bölüm: etki alanı modellerini oluşturma | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: cb98f42df411a7ba12ff4566c30ddfbf253253d4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756318"
---
<a name="part-2-creating-the-domain-models"></a><span data-ttu-id="f91f8-102">2. Bölüm: etki alanı modellerini oluşturma</span><span class="sxs-lookup"><span data-stu-id="f91f8-102">Part 2: Creating the Domain Models</span></span>
====================
<span data-ttu-id="f91f8-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f91f8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f91f8-104">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="f91f8-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="f91f8-105">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="f91f8-105">Add Models</span></span>

<span data-ttu-id="f91f8-106">Yaklaşım Entity Framework için üç yol vardır:</span><span class="sxs-lookup"><span data-stu-id="f91f8-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="f91f8-107">Veritabanı ilk: bir veritabanı ile başlayın ve Entity Framework kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f91f8-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="f91f8-108">Model-first: görsel bir model ile başlayın ve Entity Framework, hem veritabanı hem de kodu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f91f8-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="f91f8-109">Kod öncelikli: kodla başlayın ve Entity Framework veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f91f8-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="f91f8-110">Bizim etki alanı nesnelerini POCOs (düz eski CLR nesneler) tanımlayarak başlatmak için ilk kod yaklaşımı kullanıyoruz.</span><span class="sxs-lookup"><span data-stu-id="f91f8-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="f91f8-111">Code first yaklaşımıyla etki alanı nesnelerini, veritabanı katmanı, işlem veya Kalıcılık gibi desteklemek için ek bir kod gerekmez.</span><span class="sxs-lookup"><span data-stu-id="f91f8-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="f91f8-112">(Özellikle, bunlar devralınacak gerekmez [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) sınıfı.) Entity Framework veritabanı şeması nasıl oluşturduğunu denetlemek için veri ek açıklamaları kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f91f8-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="f91f8-113">POCOs açıklayan herhangi bir ek özellikleri taşımayan çünkü [veritabanı durumu](https://msdn.microsoft.com/library/system.data.entitystate.aspx), bunlar kolayca JSON veya XML seri hale getirilebilir.</span><span class="sxs-lookup"><span data-stu-id="f91f8-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="f91f8-114">Öğreticinin ilerleyen bölümlerinde anlatıldığı gibi Bununla birlikte, her zaman doğrudan istemcilere Entity Framework Modellerinizi sunmalıdır anlamına gelmez.</span><span class="sxs-lookup"><span data-stu-id="f91f8-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="f91f8-115">Aşağıdaki POCOs oluşturacağız:</span><span class="sxs-lookup"><span data-stu-id="f91f8-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="f91f8-116">Ürün</span><span class="sxs-lookup"><span data-stu-id="f91f8-116">Product</span></span>
- <span data-ttu-id="f91f8-117">Sırası</span><span class="sxs-lookup"><span data-stu-id="f91f8-117">Order</span></span>
- <span data-ttu-id="f91f8-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="f91f8-118">OrderDetail</span></span>

<span data-ttu-id="f91f8-119">Her bir sınıf oluşturmak için Çözüm Gezgini'nde modeller klasörü sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="f91f8-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="f91f8-120">Bağlam menüsünden seçin **Ekle** seçip **sınıfı.**</span><span class="sxs-lookup"><span data-stu-id="f91f8-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="f91f8-121">Ekleme bir `Product` aşağıdaki uygulama ile sınıfı:</span><span class="sxs-lookup"><span data-stu-id="f91f8-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="f91f8-122">Kural gereği, Entity Framework kullanan `Id` özelliği birincil anahtarı olarak ve bir veritabanı tablosundaki kimlik sütunu eşlenir.</span><span class="sxs-lookup"><span data-stu-id="f91f8-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="f91f8-123">Yeni bir oluşturduğunuzda `Product` örneği için bir değer ayarlamanız gerekmez `Id`, veritabanı değeri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f91f8-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="f91f8-124">**ScaffoldColumn** özniteliği söyler atlamak için ASP.NET MVC `Id` Düzenleyicisi form oluşturulurken kullanılan özellik.</span><span class="sxs-lookup"><span data-stu-id="f91f8-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="f91f8-125">**Gerekli** özniteliği model doğrulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f91f8-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="f91f8-126">Belirtir `Name` özelliği, boş olmayan bir dize olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f91f8-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="f91f8-127">Ekleme `Order` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="f91f8-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="f91f8-128">Ekleme `OrderDetail` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="f91f8-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="f91f8-129">Yabancı anahtar ilişkileri</span><span class="sxs-lookup"><span data-stu-id="f91f8-129">Foreign Key Relations</span></span>

<span data-ttu-id="f91f8-130">Çok sayıda sipariş ayrıntılarını sipariş içerir ve her Sipariş Ayrıntısı için tek bir ürün ifade eder.</span><span class="sxs-lookup"><span data-stu-id="f91f8-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="f91f8-131">Bu ilişkileri göstermek için `OrderDetail` sınıf adında özelliklere tanımlar `OrderId` ve `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="f91f8-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="f91f8-132">Entity Framework, bu özellikleri temsil eden yabancı anahtarlar ve yabancı anahtar kısıtlamaları veritabanına ekler çıkarımlar.</span><span class="sxs-lookup"><span data-stu-id="f91f8-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="f91f8-133">`Order` Ve `OrderDetail` sınıfları, ilgili nesnelere başvurular "Gezinti" özelliklerini de içerir.</span><span class="sxs-lookup"><span data-stu-id="f91f8-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="f91f8-134">Bir sipariş göz önünde bulundurulduğunda, siparişteki ürünleri gezinme özelliklerini izleyerek gidebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f91f8-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="f91f8-135">Şimdi, projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="f91f8-135">Compile the project now.</span></span> <span data-ttu-id="f91f8-136">Entity Framework veritabanı şeması oluşturmak derlenmiş bir bütünleştirilmiş kod gerektirir böylece modellerinin özelliklerini bulmak için yansıtma kullanır.</span><span class="sxs-lookup"><span data-stu-id="f91f8-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="f91f8-137">Medya türü Biçimlendiricileri yapılandırın</span><span class="sxs-lookup"><span data-stu-id="f91f8-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="f91f8-138">A [medya türü biçimlendiricisi](../../formats-and-model-binding/media-formatters.md) Web API HTTP yanıt gövdesinde yazdığında, verilerinizi serileştiren bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="f91f8-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="f91f8-139">Yerleşik biçimlendiricileri JSON ve XML çıkışı destekler.</span><span class="sxs-lookup"><span data-stu-id="f91f8-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="f91f8-140">Varsayılan olarak, her ikisi de bu biçimlendiricileri değere göre tüm nesneleri serileştirmek.</span><span class="sxs-lookup"><span data-stu-id="f91f8-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="f91f8-141">Bir nesne grafiğinin döngüsel başvurular içeriyorsa serileştirme değeriyle ilgili bir sorun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f91f8-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="f91f8-142">Durum tam olarak olan `Order` ve `OrderDetail` her diğerine bir başvuru bulunduğundan, sınıfları.</span><span class="sxs-lookup"><span data-stu-id="f91f8-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="f91f8-143">Biçimlendirici başvuruları değere göre her nesne yazarken izleyin ve daire olarak gidin.</span><span class="sxs-lookup"><span data-stu-id="f91f8-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="f91f8-144">Bu nedenle, varsayılan davranışı değiştirmek ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="f91f8-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="f91f8-145">Çözüm Gezgini'nde uygulama genişletin\_başlangıç klasörü ve WebApiConfig.cs adlı dosyayı açın.</span><span class="sxs-lookup"><span data-stu-id="f91f8-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="f91f8-146">Aşağıdaki kodu ekleyin `WebApiConfig` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="f91f8-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="f91f8-147">Bu kod, nesne başvuruları korumak için JSON biçimlendiricisini ayarlar ve XML biçimlendiricisi ardışık düzen tarafından tamamen kaldırır.</span><span class="sxs-lookup"><span data-stu-id="f91f8-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="f91f8-148">(Nesne başvuruları korumak için XML biçimlendiricisi yapılandırabilirsiniz ancak biraz daha fazla iş olduğunu ve bu uygulama için yalnızca JSON gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="f91f8-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="f91f8-149">Daha fazla bilgi için [işleme döngüsel nesne başvuruları](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="f91f8-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f91f8-150">[Önceki](using-web-api-with-entity-framework-part-1.md)
> [İleri](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="f91f8-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
