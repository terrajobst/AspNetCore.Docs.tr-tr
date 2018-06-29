---
title: ASP.NET Core MVC EF çekirdek ile-ilgili verileri - 6 10 okuma
author: rick-anderson
description: Bu öğreticide okuyun ve ilgili verileri--diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler verileri görüntüleyin.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: d5c9b665a80003ef5029754d7ad1780b3254e97e
ms.sourcegitcommit: c6ed2f00c7a08223d79090396b85793718b0dd69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37092990"
---
# <a name="aspnet-core-mvc-with-ef-core---read-related-data---6-of-10"></a><span data-ttu-id="cf659-103">ASP.NET Core MVC EF çekirdek ile-ilgili verileri - 6 10 okuma</span><span class="sxs-lookup"><span data-stu-id="cf659-103">ASP.NET Core MVC with EF Core - Read Related Data - 6 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="cf659-104">Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cf659-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cf659-105">Contoso University örnek web uygulaması Entity Framework Çekirdek ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="cf659-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="cf659-106">Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](intro.md).</span><span class="sxs-lookup"><span data-stu-id="cf659-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="cf659-107">Önceki öğreticide Okul veri modeli tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="cf659-107">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="cf659-108">Bu öğreticide, okuma ve ilgili verileri--diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler verileri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="cf659-108">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="cf659-109">Aşağıdaki çizimler ile karşılaşmayacağınızı sayfalarında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="cf659-109">The following illustrations show the pages that you'll work with.</span></span>

![Kurslar dizin sayfası](read-related-data/_static/courses-index.png)

![Eğitmen dizin sayfası](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="cf659-112">İstekli, açık ve geç ilgili veri yükleme</span><span class="sxs-lookup"><span data-stu-id="cf659-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="cf659-113">Çeşitli yolları vardır, nesne ilişkisel eşleme (ORM) yazılım gibi Entity Framework ilgili verileri bir varlık Gezinti özellikleri yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="cf659-113">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="cf659-114">İstekli yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="cf659-114">Eager loading.</span></span> <span data-ttu-id="cf659-115">Varlık okurken ilgili verileri onunla birlikte alınır.</span><span class="sxs-lookup"><span data-stu-id="cf659-115">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="cf659-116">Bu, genellikle tüm gerekli olan verileri alan bir tek birleştirme sorgusunda sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="cf659-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="cf659-117">Kullanarak Entity Framework Çekirdek belirttiğiniz istekli yükleme `Include` ve `ThenInclude` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="cf659-117">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![İstekli yükleme örneği](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="cf659-119">Bazı ayrı sorgulardaki verileri alabilir ve EF "Gezinti özellikleri düzeltmelerini".</span><span class="sxs-lookup"><span data-stu-id="cf659-119">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="cf659-120">Diğer bir deyişle, EF daha önce alınan varlık Gezinti özellikleri ait oldukları ayrı olarak alınan varlıklar otomatik olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="cf659-120">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="cf659-121">İlgili verileri alır sorgu için kullandığınız `Load` yöntemi gibi bir liste veya nesnesi döndüren bir yöntem yerine `ToList` veya `Single`.</span><span class="sxs-lookup"><span data-stu-id="cf659-121">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![Ayrı sorgulara örneği](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="cf659-123">Açık yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="cf659-123">Explicit loading.</span></span> <span data-ttu-id="cf659-124">Varlık ilk okunduğunda ilgili verileri alınan değil.</span><span class="sxs-lookup"><span data-stu-id="cf659-124">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="cf659-125">Gerekli olup olmadığını ilgili verileri alır kod yazın.</span><span class="sxs-lookup"><span data-stu-id="cf659-125">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="cf659-126">Ayrı sorgularıyla yüklenirken eager durumda olduğu gibi birden çok sorgu sonuçlarında yüklenirken açık veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="cf659-126">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="cf659-127">Açık yükleme ile yüklenmesi Gezinti özellikleri kod belirtir farktır.</span><span class="sxs-lookup"><span data-stu-id="cf659-127">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="cf659-128">Entity Framework Çekirdek 1.1 kullandığınız `Load` açık yükleme yapmak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="cf659-128">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="cf659-129">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cf659-129">For example:</span></span>

  ![Açık yükleme örneği](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="cf659-131">Yavaş yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="cf659-131">Lazy loading.</span></span> <span data-ttu-id="cf659-132">Varlık ilk okunduğunda ilgili verileri alınan değil.</span><span class="sxs-lookup"><span data-stu-id="cf659-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="cf659-133">Ancak, bir gezinti özelliği erişme girişimi ilk kez, o gezinti özelliği için gerekli olan veriler otomatik olarak alınır.</span><span class="sxs-lookup"><span data-stu-id="cf659-133">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="cf659-134">Bir sorgu her zaman ilk olarak bir gezinti özelliğinden veri almaya çalışın veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="cf659-134">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="cf659-135">Entity Framework Çekirdek 1.0 yavaş yüklenmesini desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="cf659-135">Entity Framework Core 1.0 doesn't support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="cf659-136">Performans değerlendirmeleri</span><span class="sxs-lookup"><span data-stu-id="cf659-136">Performance considerations</span></span>

<span data-ttu-id="cf659-137">İlgili verileri alınan her varlık için gereksinim duyduğunuz biliyorsanız, veritabanına gönderilen tek bir sorgu genellikle daha verimlidir alınan her varlık için ayrı sorgulara olduğundan istekli yüklenirken genellikle en iyi performansı sunar.</span><span class="sxs-lookup"><span data-stu-id="cf659-137">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="cf659-138">Örneğin, her bölüm on ilgili kursları olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="cf659-138">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="cf659-139">Tüm ilgili veriler istekli yüklenmesini yalnızca bir tek (birleştirme) sorgu ve tek bir gidiş dönüş veritabanına neden olur.</span><span class="sxs-lookup"><span data-stu-id="cf659-139">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="cf659-140">Her bölüm için kurslara ayrı bir sorgu içinde on gidiş dönüş veritabanına neden olur.</span><span class="sxs-lookup"><span data-stu-id="cf659-140">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="cf659-141">Gecikmenin yüksek olduğu zaman veritabanına ek gidiş dönüş performansı için özellikle detrimental.</span><span class="sxs-lookup"><span data-stu-id="cf659-141">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="cf659-142">Diğer taraftan, bazı senaryolarda ayrı sorgulara daha verimli.</span><span class="sxs-lookup"><span data-stu-id="cf659-142">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="cf659-143">Bir sorgu tüm ilgili veriler istekli yüklenmesini hangi SQL Server verimli bir şekilde işleyemiyor oluşturulması, bir çok karmaşık birleştirme neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="cf659-143">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="cf659-144">Veya yalnızca bir alt kümesini işleme varlık kümesi için bir varlığın gezinme özelliklerinin erişmeniz gerekiyorsa, ayrı sorgulara Önden her şeyi istekli yüklenmesini ihtiyacınız daha fazla veri almak çünkü daha iyi gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="cf659-144">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="cf659-145">Performans önemliyse, performansı en iyi seçim yapmak için her iki yönde test en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="cf659-145">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="cf659-146">Bölüm adı görüntüler kurslar sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="cf659-146">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="cf659-147">İndirmelere varlık indirmelere atandığı bölümünün departman varlığı içeren bir gezinme özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="cf659-147">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="cf659-148">Atanan departmanı adını kurslar listesini görüntülemek için alanında departmanı varlık Name özelliği almanız gereken `Course.Department` gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="cf659-148">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that's in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="cf659-149">Aynı seçenekleri kullanarak indirmelere varlık türü için CoursesController adlı bir denetleyicisi oluşturmak **görünümleri ile MVC Entity Framework kullanarak denetleyicisi** önceki Öğrenciler denetleyici için gösterildiği gibi yaptığınız iskele kurucu Aşağıdaki çizimde:</span><span class="sxs-lookup"><span data-stu-id="cf659-149">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![Kurslar denetleyici ekleyin](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="cf659-151">Açık *CoursesController.cs* ve inceleyin `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cf659-151">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="cf659-152">Otomatik yapı iskelesi istekli yükleme için belirtilen sahip `Department` kullanarak gezinti özelliği `Include` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cf659-152">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="cf659-153">Değiştir `Index` yöntemi için daha uygun bir ad kullanan aşağıdaki kod ile `IQueryable` indirmelere varlıklar döndürüyor (`courses` yerine `schoolContext`):</span><span class="sxs-lookup"><span data-stu-id="cf659-153">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="cf659-154">Açık *Views/Courses/Index.cshtml* ve şablon kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="cf659-154">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="cf659-155">Değişiklikleri vurgulanmıştır:</span><span class="sxs-lookup"><span data-stu-id="cf659-155">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="cf659-156">İskele kurulmuş kod aşağıdaki değişiklikleri yaptınız:</span><span class="sxs-lookup"><span data-stu-id="cf659-156">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="cf659-157">Başlık dizinden kurslara değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="cf659-157">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="cf659-158">Eklenen bir **numarası** gösterir sütun `CourseID` özellik değeri.</span><span class="sxs-lookup"><span data-stu-id="cf659-158">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="cf659-159">Normalde son kullanıcılara anlamsız oldukları için varsayılan olarak, birincil anahtarlar iskele kurulmuş değil.</span><span class="sxs-lookup"><span data-stu-id="cf659-159">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="cf659-160">Ancak, bu durumda birincil anlamlı anahtarıdır ve bunu göstermek istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="cf659-160">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="cf659-161">Değiştirilen **departmanı** bölüm adını görüntülemek için sütun.</span><span class="sxs-lookup"><span data-stu-id="cf659-161">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="cf659-162">Kod görüntüler `Name` yüklenen departmanı varlık özelliği `Department` gezinti özelliği:</span><span class="sxs-lookup"><span data-stu-id="cf659-162">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="cf659-163">Uygulamayı çalıştırın ve seçin **kurslar** bölüm adlarını listesiyle görmek için sekmesini.</span><span class="sxs-lookup"><span data-stu-id="cf659-163">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kurslar dizin sayfası](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="cf659-165">Kurslar ve kayıtları gösteren bir eğitmen sayfası oluşturun</span><span class="sxs-lookup"><span data-stu-id="cf659-165">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="cf659-166">Bu bölümde Eğitmen sayfasını görüntülemek için bir denetleyici ve görünüm Eğitmen varlığın oluşturacaksınız:</span><span class="sxs-lookup"><span data-stu-id="cf659-166">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Eğitmen dizin sayfası](read-related-data/_static/instructors-index.png)

<span data-ttu-id="cf659-168">Bu sayfayı okur ve ilgili verileri aşağıdaki yollarla görüntüler:</span><span class="sxs-lookup"><span data-stu-id="cf659-168">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="cf659-169">Eğitmen listesini OfficeAssignment varlığın ilgili verileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="cf659-169">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="cf659-170">Eğitmen ve OfficeAssignment bir tane-sıfır-veya-bir ilişkide varlıklardır.</span><span class="sxs-lookup"><span data-stu-id="cf659-170">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="cf659-171">İstekli yükleme OfficeAssignment varlıklar için kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="cf659-171">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="cf659-172">Birincil tablonun tüm alınan satırları için ilgili verileri gerektiğinde daha önce açıklandığı gibi istekli yükleme genellikle daha verimli olur.</span><span class="sxs-lookup"><span data-stu-id="cf659-172">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="cf659-173">Bu durumda, tüm görüntülenen Eğitmen office atamaları görüntülemek istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="cf659-173">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="cf659-174">Kullanıcı bir eğitmen seçtiğinde ilgili indirmelere varlıkları görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="cf659-174">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="cf659-175">Eğitmen ve indirmelere bir çok-çok ilişkisi varlıklardır.</span><span class="sxs-lookup"><span data-stu-id="cf659-175">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="cf659-176">İstekli yükleme indirmelere varlıkları ve bunların ilişkili departmanı varlıklar için kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="cf659-176">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="cf659-177">Bu durumda, yalnızca seçili eğitmen için kurslar gerektiğinden ayrı sorgulara daha etkili olabilir.</span><span class="sxs-lookup"><span data-stu-id="cf659-177">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="cf659-178">Ancak, bu örnek istekli yükleme için Gezinti özellikleri kendilerini Gezinti özelliklerdir varlıkları içinde nasıl kullanıldığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="cf659-178">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="cf659-179">Kullanıcı bir indirmelere seçtiğinde kayıtları varlık kümesindeki ilgili veriler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="cf659-179">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="cf659-180">İndirmelere ve kayıt bir-çok ilişkisi varlıklardır.</span><span class="sxs-lookup"><span data-stu-id="cf659-180">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="cf659-181">Kayıt varlıkları ve bunların ilgili Öğrenci varlıklar için ayrı sorgulara kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="cf659-181">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="cf659-182">Eğitmen dizin görünüm için Görünüm modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="cf659-182">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="cf659-183">Eğitmen sayfanın üç farklı tablolardan verileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="cf659-183">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="cf659-184">Bu nedenle, her biri tablolar için verileri tutan üç özellik içeren bir görünüm modeli oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="cf659-184">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="cf659-185">İçinde *SchoolViewModels* klasörü oluşturmak *InstructorIndexData.cs* ve var olan kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="cf659-185">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="cf659-186">Eğitmen denetleyicisi ve görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="cf659-186">Create the Instructor controller and views</span></span>

<span data-ttu-id="cf659-187">Aşağıdaki çizimde gösterildiği gibi EF okuma/yazma eylemleri ile bir eğitmen denetleyicisi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="cf659-187">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![Eğitmen denetleyici ekleyin](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="cf659-189">Açık *InstructorsController.cs* ve kullanarak bir ekleme deyimi ViewModels ad alanı için:</span><span class="sxs-lookup"><span data-stu-id="cf659-189">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="cf659-190">Dizin yöntemi, ilgili verilerin istekli yükleme yapmak ve görünüm modelinde put aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="cf659-190">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="cf659-191">Yöntem isteğe bağlı rota veri kabul eder (`id`) ve bir sorgu dizesi parametresi (`courseID`) seçili Eğitmen ve seçili indirmelere kimliği değerlerini sağlayın.</span><span class="sxs-lookup"><span data-stu-id="cf659-191">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="cf659-192">Parametreleri tarafından sağlanan **seçin** sayfasında bağlar.</span><span class="sxs-lookup"><span data-stu-id="cf659-192">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="cf659-193">Kod görünüm modeli örneği oluşturmayı ve bunu Eğitmen listesi koyma başlar.</span><span class="sxs-lookup"><span data-stu-id="cf659-193">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="cf659-194">İstekli yükleme için kod belirtir `Instructor.OfficeAssignment` ve `Instructor.CourseAssignments` Gezinti özellikleri.</span><span class="sxs-lookup"><span data-stu-id="cf659-194">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="cf659-195">İçinde `CourseAssignments` özelliği, `Course` özelliği yüklü ve, içinde `Enrollments` ve `Department` özellikleri yüklenir ve her içinde `Enrollment` varlık `Student` özelliği yüklenir.</span><span class="sxs-lookup"><span data-stu-id="cf659-195">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="cf659-196">Görünümü her zaman OfficeAssignment varlık gerektirdiğinden, aynı sorguda fetch daha etkilidir.</span><span class="sxs-lookup"><span data-stu-id="cf659-196">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="cf659-197">Yalnızca sayfa daha sık olmadan daha seçili indirmelere ile görüntüleniyorsa, tek bir sorgu birden fazla sorgu iyi olacak şekilde bir eğitmen web sayfasında seçildiğinde indirmelere varlıklar gereklidir.</span><span class="sxs-lookup"><span data-stu-id="cf659-197">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="cf659-198">Kod yineler `CourseAssignments` ve `Course` iki özelliklerinden gerektiğinden `Course`.</span><span class="sxs-lookup"><span data-stu-id="cf659-198">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="cf659-199">İlk dizesi `ThenInclude` alır çağırır `CourseAssignment.Course`, `Course.Enrollments`, ve `Enrollment.Student`.</span><span class="sxs-lookup"><span data-stu-id="cf659-199">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="cf659-200">Bu noktada kodda başka bir `ThenInclude` Gezinti özellikleri için olacaktır `Student`, gereken yok.</span><span class="sxs-lookup"><span data-stu-id="cf659-200">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="cf659-201">Ancak arama `Include` üzerinden başlayan `Instructor` zinciri yeniden, bu zaman belirtme gitmek zorunda şekilde özellikleri `Course.Department` yerine `Course.Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="cf659-201">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="cf659-202">Bir eğitmen seçildiğinde aşağıdaki kodu yürütür.</span><span class="sxs-lookup"><span data-stu-id="cf659-202">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="cf659-203">Seçili Eğitmen görünüm modeli Eğitmen listesi alınır.</span><span class="sxs-lookup"><span data-stu-id="cf659-203">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="cf659-204">Görünüm modelinin `Courses` özelliği Bu eğitmen indirmelere varlıklardan ile yüklenen sonra `CourseAssignments` gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="cf659-204">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="cf659-205">`Where` Yöntem koleksiyonu döndürür, ancak ölçütlere bu durumda döndürülen yalnızca tek bir eğitmen varlık o yöntemi sonuç.</span><span class="sxs-lookup"><span data-stu-id="cf659-205">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="cf659-206">`Single` Yöntemi bu varlığın erişim sağlayan tek bir eğitmen varlık koleksiyonu dönüştürür `CourseAssignments` özelliği.</span><span class="sxs-lookup"><span data-stu-id="cf659-206">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="cf659-207">`CourseAssignments` Özelliği içeren `CourseAssignment` istediğiniz yalnızca ilgili varlıklar `Course` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="cf659-207">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="cf659-208">Kullandığınız `Single` koleksiyonu bildiğiniz durumlarda bir koleksiyon yöntemi yalnızca bir öğe olacaktır.</span><span class="sxs-lookup"><span data-stu-id="cf659-208">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="cf659-209">Tek bir yöntem kendisine geçirilen koleksiyonu boş ise veya birden çok öğe varsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cf659-209">The Single method throws an exception if the collection passed to it's empty or if there's more than one item.</span></span> <span data-ttu-id="cf659-210">Bir alternatif `SingleOrDefault`, hangi varsayılan değeri döndürür (Bu durumda null) koleksiyonu boş ise.</span><span class="sxs-lookup"><span data-stu-id="cf659-210">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="cf659-211">Ancak, bu durumda hala sonuçlanacak bir özel durum (bulunmaya çalışılırken gelen bir `Courses` özelliği bir null başvuru), ve özel durum iletisi daha az açıkça sorunun nedenini gösterir.</span><span class="sxs-lookup"><span data-stu-id="cf659-211">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="cf659-212">Çağırdığınızda `Single` yöntemi, nerede ayrıca iletebilir koşul çağırmak yerine `Where` yöntemi ayrı olarak:</span><span class="sxs-lookup"><span data-stu-id="cf659-212">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="cf659-213">Onun yerine:</span><span class="sxs-lookup"><span data-stu-id="cf659-213">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="cf659-214">Ardından, bir indirmelere seçildiyse, görünüm modeli kurslar listesinden seçili indirmelere alınır.</span><span class="sxs-lookup"><span data-stu-id="cf659-214">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="cf659-215">Ardından Görünüm modelinin `Enrollments` özelliği bu indirmelere 's kayıt varlıklardan olarak yüklenen `Enrollments` gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="cf659-215">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="cf659-216">Eğitmen dizini görünümünü değiştirme</span><span class="sxs-lookup"><span data-stu-id="cf659-216">Modify the Instructor Index view</span></span>

<span data-ttu-id="cf659-217">İçinde *Views/Instructors/Index.cshtml*, şablon kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="cf659-217">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="cf659-218">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="cf659-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="cf659-219">Var olan kodu aşağıdaki değişiklikleri yaptınız:</span><span class="sxs-lookup"><span data-stu-id="cf659-219">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="cf659-220">Model sınıfı değiştirilmiş `InstructorIndexData`.</span><span class="sxs-lookup"><span data-stu-id="cf659-220">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="cf659-221">Sayfa başlığının değiştirilen **dizin** için **Eğitmen**.</span><span class="sxs-lookup"><span data-stu-id="cf659-221">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="cf659-222">Eklenen bir **Office** görüntüleyen sütun `item.OfficeAssignment.Location` yalnızca `item.OfficeAssignment` null değil.</span><span class="sxs-lookup"><span data-stu-id="cf659-222">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="cf659-223">(Bu bir tane-sıfır-veya-bir ilişkisi olduğundan, olmayabilir ilgili OfficeAssignment varlık.)</span><span class="sxs-lookup"><span data-stu-id="cf659-223">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="cf659-224">Eklenen bir **kurslar** kurslar görüntüleyen sütun tarafından her Eğitmen öğrettin.</span><span class="sxs-lookup"><span data-stu-id="cf659-224">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="cf659-225">Bkz: [açık satır geçişle `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) bu razor sözdizimi hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="cf659-225">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="cf659-226">Dinamik olarak ekleyen eklenen kod `class="success"` için `tr` seçili Eğitmen öğesidir.</span><span class="sxs-lookup"><span data-stu-id="cf659-226">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="cf659-227">Bu önyükleme sınıfını kullanarak seçili olan satır için bir arka plan rengini belirler.</span><span class="sxs-lookup"><span data-stu-id="cf659-227">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="cf659-228">Etiketli yeni bir köprü eklenen **seçin** hemen diğer bağlantıları önce her satır, neden olan gönderilmesi için seçilen Eğitmen kimliği `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cf659-228">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="cf659-229">Uygulamayı çalıştırın ve seçin **Eğitmen** sekmesi. Hiçbir ilgili OfficeAssignment varlık olduğunda ilgili OfficeAssignment varlıkları ve boş tablo hücresi konum özelliği sayfasını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="cf659-229">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Seçili hiçbir şey Eğitmen dizin sayfası](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="cf659-231">İçinde *Views/Instructors/Index.cshtml* kapatma tablo sonra öğesi (sonunda dosyası), dosya aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cf659-231">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="cf659-232">Bu kod bir eğitmen seçildiğinde bir eğitmen ilgili kurslar listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="cf659-232">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="cf659-233">Bu kod okuyan `Courses` kurslar listesini görüntülemek için görünümü modelin özelliği.</span><span class="sxs-lookup"><span data-stu-id="cf659-233">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="cf659-234">Ayrıca sağlayan bir **seçin** seçili indirmelere Kimliğini gönderir köprü `Index` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cf659-234">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="cf659-235">Sayfayı yenileyin ve bir eğitmen seçin.</span><span class="sxs-lookup"><span data-stu-id="cf659-235">Refresh the page and select an instructor.</span></span> <span data-ttu-id="cf659-236">Artık seçili Eğitmen atanan kurslar görüntüleyen bir kılavuz görebilir ve her indirmelere için atanan departmanı adını görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cf659-236">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Seçili Eğitmen dizin sayfası Eğitmen](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="cf659-238">Yeni eklediğiniz kod bloğu sonra aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cf659-238">After the code block you just added, add the following code.</span></span> <span data-ttu-id="cf659-239">Bu indirmelere seçildiğinde bu bir indirmelere kaydedilen Öğrenciler listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="cf659-239">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="cf659-240">Bu kod, indirmelere kayıtlı Öğrenciler listesini görüntülemek için Görünüm modeli kayıtları özelliği okur.</span><span class="sxs-lookup"><span data-stu-id="cf659-240">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="cf659-241">Sayfayı yenileyin ve bir eğitmen seçin.</span><span class="sxs-lookup"><span data-stu-id="cf659-241">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="cf659-242">Ardından bir indirmelere kayıtlı Öğrenciler ve bunların dereceleri listesini görmek için seçin.</span><span class="sxs-lookup"><span data-stu-id="cf659-242">Then select a course to see the list of enrolled students and their grades.</span></span>

![Eğitmen dizin sayfası Eğitmen ve seçili indirmelere](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="cf659-244">Açık yükleniyor</span><span class="sxs-lookup"><span data-stu-id="cf659-244">Explicit loading</span></span>

<span data-ttu-id="cf659-245">Alınan zaman içinde Eğitmen listesi *InstructorsController.cs*, istekli yükleme için belirtilen `CourseAssignments` gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="cf659-245">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="cf659-246">Nadiren seçili Eğitmen ve indirmelere kayıtları görmek istediğiniz kullanıcıları beklenen varsayalım.</span><span class="sxs-lookup"><span data-stu-id="cf659-246">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="cf659-247">Bu durumda, yalnızca isteniyorsa kayıt verileri yüklemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cf659-247">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="cf659-248">Açık yükleme yapmak nasıl bir örnek görmek için değiştirme `Index` yöntemini aşağıdaki kodla istekli yükleme kayıtları için kaldırır ve bu özellik açıkça yükler.</span><span class="sxs-lookup"><span data-stu-id="cf659-248">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="cf659-249">Kod değişiklikleri vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="cf659-249">The code changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="cf659-250">Yeni kod bırakır *ThenInclude* yöntemi Eğitmen varlıklar alır kodundan için kayıt verilerini çağırır.</span><span class="sxs-lookup"><span data-stu-id="cf659-250">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="cf659-251">Eğitmen ve indirmelere seçtiyseniz, seçili indirmelere için kayıt varlıkları ve Öğrenci varlıklar her kayıt için vurgulanmış kodu alır.</span><span class="sxs-lookup"><span data-stu-id="cf659-251">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="cf659-252">Çalışma Veri nasıl alındığını değiştirdiniz ancak şimdi Eğitmen dizin sayfasına gidin ve ve, uygulama sayfasında, görüntülenen içinde herhangi bir fark görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="cf659-252">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="cf659-253">Özet</span><span class="sxs-lookup"><span data-stu-id="cf659-253">Summary</span></span>

<span data-ttu-id="cf659-254">Şimdi istekli yükleme birden çok sorgu ile bir sorgu ile ilgili verileri Gezinti özelliklerini okumak için kullandığınız.</span><span class="sxs-lookup"><span data-stu-id="cf659-254">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="cf659-255">Sonraki öğreticide ilgili verileri güncelleştirmek öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="cf659-255">In the next tutorial you'll learn how to update related data.</span></span>

::: moniker-end

>[!div class="step-by-step"]
><span data-ttu-id="cf659-256">[Önceki](complex-data-model.md)
>[sonraki](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="cf659-256">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>
