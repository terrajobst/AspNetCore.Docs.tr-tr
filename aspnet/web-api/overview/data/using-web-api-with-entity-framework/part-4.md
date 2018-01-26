---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: "Varlık İlişkileriyle işleme | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 58a9dfb621630f23b37247b96ed3a19a661857f1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="handling-entity-relations"></a><span data-ttu-id="800bb-102">İşleme varlık ilişkileri</span><span class="sxs-lookup"><span data-stu-id="800bb-102">Handling Entity Relations</span></span>
====================
<span data-ttu-id="800bb-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="800bb-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="800bb-104">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="800bb-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="800bb-105">Bu bölümde bazı ayrıntılarını nasıl EF ilgili varlıklar yükler ve model sınıflarınızı döngüsel Gezinti özellikleri nasıl ele alınacağını açıklar.</span><span class="sxs-lookup"><span data-stu-id="800bb-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="800bb-106">(Bu bölümde arka plan bilgi sağlar ve öğreticiyi tamamlamak için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="800bb-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="800bb-107">İsterseniz, geçin [bölümü 5.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="800bb-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="800bb-108">Eager karşı geç yükleme yükleniyor</span><span class="sxs-lookup"><span data-stu-id="800bb-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="800bb-109">EF ilişkisel veritabanı ile kullanırken, nasıl EF ilgili verileri yükler anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="800bb-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="800bb-110">EF oluşturur SQL sorgularını görmek yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="800bb-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="800bb-111">SQL izleme için aşağıdaki kod satırını ekleyin `BookServiceContext` Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="800bb-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="800bb-112">/Api/books için bir GET isteği gönderirseniz, JSON aşağıdaki gibi döndürür:</span><span class="sxs-lookup"><span data-stu-id="800bb-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="800bb-113">Geçerli AuthorId defteri içeriyor olsa bile Yazar özelliği null olduğunu görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="800bb-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="800bb-114">EF ilgili Yazar varlıklar yüklenmemesi olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="800bb-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="800bb-115">SQL sorgusunun izleme günlüğü bu onaylar:</span><span class="sxs-lookup"><span data-stu-id="800bb-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="800bb-116">SELECT deyimi Books tablosundan alır ve yazar tablo başvurmuyor.</span><span class="sxs-lookup"><span data-stu-id="800bb-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="800bb-117">Başvuru için işte yönteminde `BooksController` books listesini döndürür sınıfı.</span><span class="sxs-lookup"><span data-stu-id="800bb-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="800bb-118">Biz JSON verilerini bir parçası olarak yazar nasıl döndürebilir görelim.</span><span class="sxs-lookup"><span data-stu-id="800bb-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="800bb-119">Entity Framework ilgili veri yüklemek için üç yol vardır: istekli yükleme, yavaş yükleniyor ve açık yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="800bb-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="800bb-120">Nasıl çalıştığını anlamak önemlidir dengelemeler ile her tekniği vardır.</span><span class="sxs-lookup"><span data-stu-id="800bb-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="800bb-121">İstekli yükleniyor</span><span class="sxs-lookup"><span data-stu-id="800bb-121">Eager Loading</span></span>

<span data-ttu-id="800bb-122">İle *istekli yükleme*, EF ilgili varlıklar ilk veritabanı sorgusunun bir parçası olarak yükler.</span><span class="sxs-lookup"><span data-stu-id="800bb-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="800bb-123">İstekli yükleme gerçekleştirmek için kullanın **System.Data.Entity.Include** genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="800bb-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="800bb-124">Bu sorguda Yazar verileri içerecek şekilde EF bildirir.</span><span class="sxs-lookup"><span data-stu-id="800bb-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="800bb-125">Şimdi, bu değişikliği yapmak ve uygulama çalıştırırsanız, JSON verilerini şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="800bb-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="800bb-126">İzleme günlüğü EF kitap ve yazar tablolarında birleştirme gerçekleştirilen gösterir.</span><span class="sxs-lookup"><span data-stu-id="800bb-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="800bb-127">Yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="800bb-127">Lazy Loading</span></span>

<span data-ttu-id="800bb-128">Bu varlık için gezinme özelliği başvuru yapıldı zaman yavaş yükleniyor ile EF ilgili varlık otomatik olarak yükler.</span><span class="sxs-lookup"><span data-stu-id="800bb-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="800bb-129">Yavaş yükleniyor etkinleştirmek için gezinti özelliği sanal olun.</span><span class="sxs-lookup"><span data-stu-id="800bb-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="800bb-130">Örneğin, kitap sınıfında:</span><span class="sxs-lookup"><span data-stu-id="800bb-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="800bb-131">Şimdi aşağıdaki kodu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="800bb-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="800bb-132">Yavaş yükleniyor etkinleştirildiğinde, erişme `Author` özellikte `books[0]` yazarına veritabanını sorgulamak EF neden olur.</span><span class="sxs-lookup"><span data-stu-id="800bb-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="800bb-133">EF ilgili varlık alır her zaman bir sorgu gönderdiğinden yavaş yükleniyor birden çok veritabanı dönüşle gerektirir.</span><span class="sxs-lookup"><span data-stu-id="800bb-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="800bb-134">Genellikle, seri hale nesneler için devre dışı yavaş yükleniyor istersiniz.</span><span class="sxs-lookup"><span data-stu-id="800bb-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="800bb-135">Tüm ilgili varlıklar yüklenirken tetikler model üzerinde özelliklerini okumak seri hale getirici sahiptir.</span><span class="sxs-lookup"><span data-stu-id="800bb-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="800bb-136">Örneğin, burada alırken SQL sorguları EF defterleri listesi etkin yavaş yükleniyor ile serileştirir.</span><span class="sxs-lookup"><span data-stu-id="800bb-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="800bb-137">EF üç yazarlar için üç ayrı sorgulara yapar görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="800bb-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="800bb-138">Ne zaman yavaş yükleniyor kullanmak isteyebilirsiniz zamanlar hala vardır.</span><span class="sxs-lookup"><span data-stu-id="800bb-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="800bb-139">İstekli yükleme çok karmaşık bir birleşim oluşturmak EF neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="800bb-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="800bb-140">Veya küçük bir alt veri kümesine ilgili varlıklar gerekebilir ve yavaş yükleniyor daha verimli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="800bb-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="800bb-141">Seri hale getirme sorunları önlemek için bir veri aktarımı nesneleri (DTOs) yerine varlık nesnesi seri hale getirmek için yoludur.</span><span class="sxs-lookup"><span data-stu-id="800bb-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="800bb-142">Bu yaklaşım makalenin sonraki bölümlerinde göster.</span><span class="sxs-lookup"><span data-stu-id="800bb-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="800bb-143">Açık yükleniyor</span><span class="sxs-lookup"><span data-stu-id="800bb-143">Explicit Loading</span></span>

<span data-ttu-id="800bb-144">Kod içinde açıkça ilgili verileri almak açık yükleme yavaş yüklemeye benzerdir; bir gezinti özelliğine erişirken otomatik olarak gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="800bb-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="800bb-145">Açık yükleme zaman ilgili verileri yüklemek daha fazla denetim sağlar, ancak ek kod gerektirir.</span><span class="sxs-lookup"><span data-stu-id="800bb-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="800bb-146">Açık yükleme hakkında daha fazla bilgi için bkz: [yüklenirken ilgili varlıklar](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="800bb-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="800bb-147">Gezinti özellikleri ve dairesel başvurular</span><span class="sxs-lookup"><span data-stu-id="800bb-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="800bb-148">Kitap ve yazar modelleri tanımlandığında, t bir gezinti özelliği tanımlanan `Book` kitap yazarı ilişki sınıfı ancak t bir gezinti özelliği ters yönde tanımlamıyor.</span><span class="sxs-lookup"><span data-stu-id="800bb-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="800bb-149">Karşılık gelen gezinme özelliğini eklerseniz ne olur `Author` sınıfı?</span><span class="sxs-lookup"><span data-stu-id="800bb-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="800bb-150">Modelleri seri ne yazık ki, bu bir sorun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="800bb-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="800bb-151">İlgili verileri yüklerseniz, döngüsel bir nesne grafiğinin oluşturur.</span><span class="sxs-lookup"><span data-stu-id="800bb-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="800bb-152">Grafik serileştirmek JSON veya XML biçimlendiricisi çalıştığında, bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="800bb-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="800bb-153">İki biçimlendiricileri farklı özel durum iletileri durum.</span><span class="sxs-lookup"><span data-stu-id="800bb-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="800bb-154">JSON biçimlendirici örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="800bb-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="800bb-155">XML biçimlendirici şöyledir:</span><span class="sxs-lookup"><span data-stu-id="800bb-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="800bb-156">Bir çözüm, sonraki bölümde açıklanmaktadır DTOs kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="800bb-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="800bb-157">Alternatif olarak, grafik döngüleri işlemek için JSON ve XML biçimlendiricileri yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="800bb-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="800bb-158">Daha fazla bilgi için bkz: [işleme döngüsel nesne başvuruları](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="800bb-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="800bb-159">Bu öğretici için gerekli olmayan `Author.Book` bırakılabilir için gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="800bb-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="800bb-160">[Önceki](part-3.md)
[sonraki](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="800bb-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
