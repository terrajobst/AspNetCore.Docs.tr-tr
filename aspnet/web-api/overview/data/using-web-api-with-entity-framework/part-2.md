---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Model ve denetleyici ekleme | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 162ef2cd4ba11040e1bc938617a36495489ba5bc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755174"
---
<a name="add-models-and-controllers"></a><span data-ttu-id="c453e-102">Model ve denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="c453e-102">Add Models and Controllers</span></span>
====================
<span data-ttu-id="c453e-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c453e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c453e-104">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="c453e-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="c453e-105">Bu bölümde, veritabanı varlıklar tanımlayan bir model sınıfları ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="c453e-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="c453e-106">Ardından bu varlıkların CRUD işlemleri Web APİ'si denetleyicilerinin ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="c453e-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="c453e-107">Model sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="c453e-107">Add Model Classes</span></span>

<span data-ttu-id="c453e-108">Bu öğreticide, veritabanı Entity Framework (EF) "Code First" yaklaşımı kullanarak oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="c453e-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="c453e-109">Code First ile veritabanı tablolarında karşılık gelen C# sınıfları yazma ve veritabanı EF oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c453e-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="c453e-110">(Daha fazla bilgi için [Entity Framework Geliştirme yaklaşımları](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span><span class="sxs-lookup"><span data-stu-id="c453e-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="c453e-111">Biz, bizim etki alanı nesnelerini POCOs (düz eski CLR nesneler) tanımlayarak başlatın.</span><span class="sxs-lookup"><span data-stu-id="c453e-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="c453e-112">Aşağıdaki POCOs oluşturacağız:</span><span class="sxs-lookup"><span data-stu-id="c453e-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="c453e-113">Yazar</span><span class="sxs-lookup"><span data-stu-id="c453e-113">Author</span></span>
- <span data-ttu-id="c453e-114">Rehberi</span><span class="sxs-lookup"><span data-stu-id="c453e-114">Book</span></span>

<span data-ttu-id="c453e-115">Çözüm Gezgini'nde modeller klasörü sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="c453e-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="c453e-116">Seçin **Ekle**, ardından **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="c453e-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="c453e-117">Sınıf adını `Author`.</span><span class="sxs-lookup"><span data-stu-id="c453e-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="c453e-118">Tüm Author.cs Demirbaş kod, aşağıdaki kod ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="c453e-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="c453e-119">Adlı başka bir sınıf ekleyin `Book`, aşağıdaki kod ile.</span><span class="sxs-lookup"><span data-stu-id="c453e-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="c453e-120">Entity Framework veritabanı tabloları oluşturmak için bu modelleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="c453e-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="c453e-121">Her model için `Id` özelliği, veritabanı tablosunun birincil anahtar sütunu olacak.</span><span class="sxs-lookup"><span data-stu-id="c453e-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="c453e-122">Kitap sınıfında `AuthorId` içine bir yabancı anahtar tanımlar `Author` tablo.</span><span class="sxs-lookup"><span data-stu-id="c453e-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="c453e-123">(Kolaylık olması için miyim her kitabın tek bir yazar olduğunu varsayarak.) Kitap sınıfı da bir gezinti özelliğine ilgili içeren `Author`.</span><span class="sxs-lookup"><span data-stu-id="c453e-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="c453e-124">Gezinme özelliğini ilgili erişmek için kullanabileceğiniz `Author` kod.</span><span class="sxs-lookup"><span data-stu-id="c453e-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="c453e-125">Bölüm 4, gezinti özellikleri hakkında daha fazla söyleyin [varlık ilişkilerini işleme](part-4.md).</span><span class="sxs-lookup"><span data-stu-id="c453e-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="c453e-126">Web API denetleyicisi Ekle</span><span class="sxs-lookup"><span data-stu-id="c453e-126">Add Web API Controllers</span></span>

<span data-ttu-id="c453e-127">Bu bölümde, CRUD işlemleri destekleyen Web APİ'si denetleyicilerinin ekleyeceğiz (oluşturma, okuma, güncelleştirme ve silme).</span><span class="sxs-lookup"><span data-stu-id="c453e-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="c453e-128">Denetleyici, Entity Framework veritabanı katmanı ile iletişim kurmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="c453e-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="c453e-129">İlk olarak, dosya Controllers/ValuesController.cs silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c453e-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="c453e-130">Bu dosya bir örnek Web API denetleyicisi içerir, ancak bu öğretici için gereksinim duymuyorsanız.</span><span class="sxs-lookup"><span data-stu-id="c453e-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="c453e-131">Ardından, projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="c453e-131">Next, build the project.</span></span> <span data-ttu-id="c453e-132">Web APİ'si yapı iskelesini, derlenen bütünleştirilmiş gerekir model sınıfları bulmak için yansıtma kullanır.</span><span class="sxs-lookup"><span data-stu-id="c453e-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="c453e-133">Çözüm Gezgini'nde denetleyicileri klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="c453e-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="c453e-134">Seçin **Ekle**, ardından **denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="c453e-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="c453e-135">İçinde **İskele Ekle** iletişim kutusunda "Web API 2 denetleyici Entity Framework kullanarak Eylemler ile".</span><span class="sxs-lookup"><span data-stu-id="c453e-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="c453e-136">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="c453e-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="c453e-137">İçinde **denetleyici Ekle** iletişim kutusunda, aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="c453e-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="c453e-138">İçinde **Model sınıfı** açılır menüsünde, select `Author` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c453e-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="c453e-139">(Bu açılır listede görmüyorsanız, proje oluşturulan emin olun.)</span><span class="sxs-lookup"><span data-stu-id="c453e-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="c453e-140">"Zaman uyumsuz denetleyici eylemleri kullanın" denetleyin.</span><span class="sxs-lookup"><span data-stu-id="c453e-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="c453e-141">Denetleyici adı olarak bırakın &quot;AuthorsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="c453e-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="c453e-142">Click artı (+) düğmesine yanındaki **veri bağlamı sınıfının**.</span><span class="sxs-lookup"><span data-stu-id="c453e-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="c453e-143">İçinde **yeni veri bağlamı** iletişim kutusunda varsayılan adı bırakın ve tıklayın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="c453e-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="c453e-144">Tıklayın **Ekle** tamamlanması **denetleyici Ekle** iletişim.</span><span class="sxs-lookup"><span data-stu-id="c453e-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="c453e-145">İletişim kutusu, iki sınıf projenize ekler:</span><span class="sxs-lookup"><span data-stu-id="c453e-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="c453e-146">`AuthorsController` bir Web API denetleyicisi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="c453e-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="c453e-147">İstemcilerin yazarlar listesinde CRUD işlemleri gerçekleştirmek için kullandığı REST API denetleyicisi uygular.</span><span class="sxs-lookup"><span data-stu-id="c453e-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="c453e-148">`BookServiceContext` bir veritabanı, değişiklik izleme ve kalıcı veri verilerini veritabanına nesnelerle doldurmak içeren çalışma zamanı sırasında varlık nesnelerini yönetir.</span><span class="sxs-lookup"><span data-stu-id="c453e-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="c453e-149">Devraldığı `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c453e-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="c453e-150">Bu noktada, projeyi yeniden derleyin.</span><span class="sxs-lookup"><span data-stu-id="c453e-150">At this point, build the project again.</span></span> <span data-ttu-id="c453e-151">Artık bir API denetleyicisi eklemek için aynı adımları inceleyin `Book` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="c453e-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="c453e-152">Bu kez, select `Book` model sınıfı ve mevcut seçim için `BookServiceContext` veri bağlamı sınıfı için sınıf.</span><span class="sxs-lookup"><span data-stu-id="c453e-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="c453e-153">(Yeni bir veri bağlamı oluşturmayın.) Tıklayın **Ekle** denetleyicisi eklemek için.</span><span class="sxs-lookup"><span data-stu-id="c453e-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> <span data-ttu-id="c453e-154">[Önceki](part-1.md)
> [İleri](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="c453e-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
