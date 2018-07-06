---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Varlık ilişkilerini işleme | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: fcfddb3d56d0be641d2df9d92c334776975621be
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826163"
---
<a name="handling-entity-relations"></a><span data-ttu-id="078c6-102">Varlık ilişkilerini işleme</span><span class="sxs-lookup"><span data-stu-id="078c6-102">Handling Entity Relations</span></span>
====================
<span data-ttu-id="078c6-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="078c6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="078c6-104">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="078c6-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="078c6-105">Bu bölümde bazı ayrıntılar EF ilgili varlıkları nasıl yükler ve model sınıflarınızı döngüsel Gezinti özelliklerini nasıl ele alınacağını açıklar.</span><span class="sxs-lookup"><span data-stu-id="078c6-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="078c6-106">(Bu bölümde arka plan bilgileri sağlar ve bu öğreticiyi tamamlamak için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="078c6-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="078c6-107">Tercih ederseniz, atlamak [5. bölüm.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="078c6-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="078c6-108">Eager yavaş yükleme karşılaştırması yükleniyor</span><span class="sxs-lookup"><span data-stu-id="078c6-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="078c6-109">EF sahip ilişkisel bir veritabanı kullanılırken EF ilgili verileri nasıl yükler anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="078c6-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="078c6-110">EF oluşturur SQL sorgularını görmek kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="078c6-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="078c6-111">SQL izleme için aşağıdaki kod satırını ekleyin. `BookServiceContext` Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="078c6-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="078c6-112">/Api/books için bir GET isteği gönderirseniz, JSON aşağıdaki gibi döndürür:</span><span class="sxs-lookup"><span data-stu-id="078c6-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="078c6-113">Kitap geçerli AuthorId içerse Yazar özelliği null olduğunu görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="078c6-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="078c6-114">EF ilgili Yazar varlıkları yüklenmemesi olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="078c6-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="078c6-115">İzleme günlüğü SQL sorgusu bu onaylar:</span><span class="sxs-lookup"><span data-stu-id="078c6-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="078c6-116">SELECT deyimi Books tablosundan alır ve yazar tabloya başvurmuyor.</span><span class="sxs-lookup"><span data-stu-id="078c6-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="078c6-117">Başvuru için işte yöntemi `BooksController` kitap listesi döndüren sınıfı.</span><span class="sxs-lookup"><span data-stu-id="078c6-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="078c6-118">Biz Yazar JSON verilerini bir parçası olarak nasıl döndürebilir görelim.</span><span class="sxs-lookup"><span data-stu-id="078c6-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="078c6-119">Entity Framework ilgili verileri yüklemek için üç yol vardır: açık yükleme istekli yükleme ve yavaş yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="078c6-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="078c6-120">Nasıl çalıştığını anlamak önemlidir her yöntem ile dengelemeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="078c6-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="078c6-121">İstekli yükleme</span><span class="sxs-lookup"><span data-stu-id="078c6-121">Eager Loading</span></span>

<span data-ttu-id="078c6-122">İle *istekli yükleme*, EF ilgili varlıkları ilk veritabanı sorgusunun bir parçası olarak yükler.</span><span class="sxs-lookup"><span data-stu-id="078c6-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="078c6-123">İstekli yükleme gerçekleştirmek için **System.Data.Entity.Include** genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="078c6-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="078c6-124">Bu sorguda Yazar verileri içerecek şekilde EF bildirir.</span><span class="sxs-lookup"><span data-stu-id="078c6-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="078c6-125">Şimdi, bu değişikliği yapın ve uygulamayı çalıştırın, JSON verilerini şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="078c6-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="078c6-126">İzleme günlüğü EF kitap ve yazar tablolarında birleştirme gerçekleştirilen gösterir.</span><span class="sxs-lookup"><span data-stu-id="078c6-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="078c6-127">Yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="078c6-127">Lazy Loading</span></span>

<span data-ttu-id="078c6-128">Söz konusu varlığa ilişkin gezinme özelliğini başvurusu kaldırıldığında yavaş yükleniyor ile EF ilgili varlık otomatik olarak yükler.</span><span class="sxs-lookup"><span data-stu-id="078c6-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="078c6-129">Yavaş yükleniyor etkinleştirmek için gezinme özelliği sanal olun.</span><span class="sxs-lookup"><span data-stu-id="078c6-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="078c6-130">Örneğin, kitap sınıfında:</span><span class="sxs-lookup"><span data-stu-id="078c6-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="078c6-131">Aşağıdaki kodu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="078c6-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="078c6-132">Gecikmeli yükleme etkinleştirildiğinde erişme `Author` özelliği `books[0]` Yazar veritabanını sorgulamak EF neden olur.</span><span class="sxs-lookup"><span data-stu-id="078c6-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="078c6-133">EF isteğe bağlı olarak her bir ilgili varlığı alır bir sorgu gönderdiğinden yavaş yükleniyor birden çok veritabanı dönüşle gerektirir.</span><span class="sxs-lookup"><span data-stu-id="078c6-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="078c6-134">Genellikle, seri hale getirme nesneleri için devre dışı yavaş yükleniyor istersiniz.</span><span class="sxs-lookup"><span data-stu-id="078c6-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="078c6-135">Tüm özellikler ilgili varlıkları yükleme tetikler modeli okumak seri hale getirici sahiptir.</span><span class="sxs-lookup"><span data-stu-id="078c6-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="078c6-136">Örneğin, EF kitap listesi ile yavaş yükleniyor etkin serileştiren olduğunda SQL sorguları aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="078c6-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="078c6-137">EF üç yazarlar için üç ayrı sorgular yapar görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="078c6-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="078c6-138">Karşılaşılmaya devam süreleri, yavaş yükleniyor kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="078c6-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="078c6-139">İstekli yükleme çok karmaşık bir birleştirme oluşturulacak EF neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="078c6-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="078c6-140">İlgili varlıkları verilerin küçük bir kısmı için ihtiyacınız olabilecek veya yavaş yükleniyor daha verimli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="078c6-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="078c6-141">Serileştirme sorunları önlemek için bir veri aktarımı nesneleri (Dto) yerine varlık nesneleri serileştirmek için yoludur.</span><span class="sxs-lookup"><span data-stu-id="078c6-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="078c6-142">Bu yaklaşım makalenin sonraki bölümlerinde aktaracağınızı göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="078c6-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="078c6-143">Açık yükleme</span><span class="sxs-lookup"><span data-stu-id="078c6-143">Explicit Loading</span></span>

<span data-ttu-id="078c6-144">Kod içinde açıkça ilgili verileri alma açık yükleme yavaş yükleniyor için benzer; bir gezinti özelliğine erişirken otomatik olarak gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="078c6-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="078c6-145">Açık yükleme ilgili verileri yüklemek ne zaman üzerinde daha fazla denetim verir ancak ek bir kod gerektirir.</span><span class="sxs-lookup"><span data-stu-id="078c6-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="078c6-146">Açık yükleme hakkında daha fazla bilgi için bkz: [ilgili varlıkları yükleme](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="078c6-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="078c6-147">Gezinti özellikleri ve döngüsel başvurular</span><span class="sxs-lookup"><span data-stu-id="078c6-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="078c6-148">Ben kitap ve yazma modelleri tanımlandığında, ben bir gezinti özelliği tanımlanmış `Book` kitap yazarı ilişki sınıfı ancak ben bir gezinti özelliği diğer yönde tanımlamıyor.</span><span class="sxs-lookup"><span data-stu-id="078c6-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="078c6-149">Karşılık gelen gezinme özelliğini eklerseniz ne olur `Author` sınıfı?</span><span class="sxs-lookup"><span data-stu-id="078c6-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="078c6-150">Ne yazık ki, modeli seri olduğunda bu sorun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="078c6-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="078c6-151">İlgili verileri yükleme, döngüsel Nesne grafiği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="078c6-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="078c6-152">Grafik seri hale getirmek JSON veya XML biçimlendiricisi çalıştığında bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="078c6-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="078c6-153">İki biçimlendiricileri farklı bir özel durum iletileri görüntülemeyi tercih edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="078c6-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="078c6-154">JSON biçimlendirici için bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="078c6-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="078c6-155">XML biçimlendirici şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="078c6-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="078c6-156">Tek bir çözüm, sonraki bölümde açıklayan ı Dto'lar kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="078c6-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="078c6-157">Alternatif olarak, JSON ve XML biçimlendiricileri grafı döngüler işlemek için yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="078c6-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="078c6-158">Daha fazla bilgi için [işleme döngüsel nesne başvuruları](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="078c6-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="078c6-159">Bu öğreticide, ihtiyacınız olmayan `Author.Book` bırakılabilir için gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="078c6-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="078c6-160">[Önceki](part-3.md)
> [İleri](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="078c6-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
