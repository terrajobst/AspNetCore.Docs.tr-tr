---
title: -EF çekirdekli ASP.NET Core MVC 6 10 - ilgili verileri okuma
author: rick-anderson
description: Bu öğreticide okuyun ve ilgili verileri--diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler verileri görüntüler.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: d5c9b665a80003ef5029754d7ad1780b3254e97e
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38216300"
---
# <a name="aspnet-core-mvc-with-ef-core---read-related-data---6-of-10"></a><span data-ttu-id="b68af-103">-EF çekirdekli ASP.NET Core MVC 6 10 - ilgili verileri okuma</span><span class="sxs-lookup"><span data-stu-id="b68af-103">ASP.NET Core MVC with EF Core - Read Related Data - 6 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b68af-104">Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b68af-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b68af-105">Contoso University örnek web uygulaması, Entity Framework Core ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="b68af-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="b68af-106">Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](intro.md).</span><span class="sxs-lookup"><span data-stu-id="b68af-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="b68af-107">Önceki öğreticide, okul veri modeli tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="b68af-107">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="b68af-108">Bu öğreticide, okuma ve ilgili verileri--diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler verileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b68af-108">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="b68af-109">Aşağıdaki çizimler ile çalışmak sayfaları göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="b68af-109">The following illustrations show the pages that you'll work with.</span></span>

![Kursları dizin sayfası](read-related-data/_static/courses-index.png)

![Eğitmenler dizin sayfası](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="b68af-112">İlgili veri eager, açık ve yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="b68af-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="b68af-113">Birkaç yolu vardır, nesne ilişkisel eşleme (ORM) yazılım gibi Entity Framework ilgili verileri bir varlığın Gezinti özelliklerini yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="b68af-113">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="b68af-114">İstekli yükleme.</span><span class="sxs-lookup"><span data-stu-id="b68af-114">Eager loading.</span></span> <span data-ttu-id="b68af-115">Varlık okunduğunda, onunla birlikte ilgili verileri alınır.</span><span class="sxs-lookup"><span data-stu-id="b68af-115">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="b68af-116">Bu, tek bir birleşim sorguda tüm gerekli olan verileri alır. genellikle sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="b68af-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="b68af-117">İstekli yükleme kullanarak Entity Framework Core içinde belirttiğiniz `Include` ve `ThenInclude` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="b68af-117">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![İstekli yükleme örneği](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="b68af-119">Bazı ayrı sorgularda veri alabilirsiniz ve EF "gezinme özelliklerini düzeltmeler".</span><span class="sxs-lookup"><span data-stu-id="b68af-119">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="b68af-120">Diğer bir deyişle, EF daha önce alınan varlık Gezinti özellikleri ait oldukları ayrı olarak alınan varlıkları otomatik olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="b68af-120">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="b68af-121">İlgili verileri alır. sorgu için kullanabileceğiniz `Load` yöntemi gibi bir liste veya nesnesi döndüren bir yöntem yerine `ToList` veya `Single`.</span><span class="sxs-lookup"><span data-stu-id="b68af-121">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![Ayrı sorguları örneği](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="b68af-123">Açık yükleme.</span><span class="sxs-lookup"><span data-stu-id="b68af-123">Explicit loading.</span></span> <span data-ttu-id="b68af-124">Varlığın ilk okunduğunda, ilgili verileri alınan değil.</span><span class="sxs-lookup"><span data-stu-id="b68af-124">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="b68af-125">Gerekli olup olmadığını ilgili verileri alır. kod yazacaksınız.</span><span class="sxs-lookup"><span data-stu-id="b68af-125">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="b68af-126">Ayrı sorgular yüklenirken eager durumda olduğu gibi birden çok sorgu sonuçları yükleniyor açık veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b68af-126">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="b68af-127">Açık yükleme ile kod yüklenmesine, gezinti özellikleri belirtir farktır.</span><span class="sxs-lookup"><span data-stu-id="b68af-127">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="b68af-128">Entity Framework Core 1.1 içinde kullanabileceğiniz `Load` açık yükleme yapmak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b68af-128">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="b68af-129">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b68af-129">For example:</span></span>

  ![Açık yükleme örneği](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="b68af-131">Yavaş yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="b68af-131">Lazy loading.</span></span> <span data-ttu-id="b68af-132">Varlığın ilk okunduğunda, ilgili verileri alınan değil.</span><span class="sxs-lookup"><span data-stu-id="b68af-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="b68af-133">Ancak, bir gezinti özelliği erişmeye ilk kez otomatik olarak bu gezinti özelliği için gerekli olan veriler alınır.</span><span class="sxs-lookup"><span data-stu-id="b68af-133">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="b68af-134">Bir sorgu, bir gezinti özelliği ilk kez veri almayı deneyin her zaman veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b68af-134">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="b68af-135">Entity Framework Core 1.0, yavaş yüklenmesini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="b68af-135">Entity Framework Core 1.0 doesn't support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="b68af-136">Performans değerlendirmeleri</span><span class="sxs-lookup"><span data-stu-id="b68af-136">Performance considerations</span></span>

<span data-ttu-id="b68af-137">İlgili verileri alınan her varlık için gereksinim duyduğunuz biliyorsanız, veritabanına gönderilen tek bir sorgu genellikle daha verimli alınan her varlık için ayrı sorgulara olduğundan istekli yükleme genellikle en iyi performansı sunar.</span><span class="sxs-lookup"><span data-stu-id="b68af-137">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="b68af-138">Örneğin, her departman on ilgili kurslar olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="b68af-138">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="b68af-139">İstekli yükleme tüm ilgili verilerin yalnızca bir tek (birleştirme) sorgu ve tek bir gidiş dönüş veritabanına neden olur.</span><span class="sxs-lookup"><span data-stu-id="b68af-139">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="b68af-140">Kursları her departman için ayrı bir sorgu içinde on gidiş dönüş veritabanına neden olur.</span><span class="sxs-lookup"><span data-stu-id="b68af-140">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="b68af-141">Gecikme süresi yüksek olduğunda veritabanına ek gidiş dönüş özellikle performansı düşürür.</span><span class="sxs-lookup"><span data-stu-id="b68af-141">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="b68af-142">Öte yandan, bazı senaryolarda ayrı sorgulara daha verimlidir.</span><span class="sxs-lookup"><span data-stu-id="b68af-142">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="b68af-143">Bir sorgudaki tüm ilgili verilerin istekli yükleme, SQL Server'ın etkili bir şekilde işleyemiyor oluşturulması çok karmaşık birleştirme neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="b68af-143">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="b68af-144">Veya, bir varlığın yalnızca bir alt kümesini bir dizi işlem varlık Gezinti özellikleri erişmeniz gerekiyorsa, ayrı sorgulara Önden her şeyin istekli yükleme ihtiyacınız olandan daha fazla veri alması için daha iyi gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="b68af-144">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="b68af-145">Performans kritik ise, en iyi seçim yapmak için her iki yönde performansını test etmek idealdir.</span><span class="sxs-lookup"><span data-stu-id="b68af-145">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="b68af-146">Bölüm adını görüntüleyen bir kursları sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="b68af-146">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="b68af-147">Kurs varlık kursu atandığı departmanı departmanı varlığı içeren bir gezinme özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="b68af-147">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="b68af-148">Atanan bölüm adını kursları listesini görüntülemek için Name özelliği içinde departmanı varlıktan almanız gereken `Course.Department` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="b68af-148">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that's in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="b68af-149">Aynı seçenekleri kullanarak kurs varlık türü için CoursesController adlı bir denetleyicisi oluşturmak **MVC denetleyici Entity Framework kullanarak görünümler ile** önceki Öğrenciler denetleyici için gösterildiği gibi yaptığınız iskele kurucu Aşağıdaki çizim:</span><span class="sxs-lookup"><span data-stu-id="b68af-149">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![Kursları denetleyici ekleme](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="b68af-151">Açık *CoursesController.cs* ve incelemek `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b68af-151">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="b68af-152">Otomatik yapı iskelesi istekli yükleme için belirtilmiş `Department` gezinme özelliğini kullanarak `Include` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b68af-152">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="b68af-153">Değiştirin `Index` yöntemi için daha uygun bir ad kullanan aşağıdaki kodla `IQueryable` kurs varlıkları döndürür (`courses` yerine `schoolContext`):</span><span class="sxs-lookup"><span data-stu-id="b68af-153">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="b68af-154">Açık *Views/Courses/Index.cshtml* ve şablon kodunu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b68af-154">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="b68af-155">Değişiklikleri vurgulanmıştır:</span><span class="sxs-lookup"><span data-stu-id="b68af-155">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="b68af-156">İskele kurulan kodu için aşağıdaki değişiklikler yaptınız:</span><span class="sxs-lookup"><span data-stu-id="b68af-156">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="b68af-157">Başlık dizinden kurslara değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="b68af-157">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="b68af-158">Eklenen bir **numarası** gösteren sütun `CourseID` özellik değeri.</span><span class="sxs-lookup"><span data-stu-id="b68af-158">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="b68af-159">Normalde son kullanıcılara anlamsız olduğunuz için varsayılan olarak, birincil anahtarlar iskele kurulmuş değildir.</span><span class="sxs-lookup"><span data-stu-id="b68af-159">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="b68af-160">Ancak, bu durumda birincil anahtarı anlamlı ve göstermek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="b68af-160">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="b68af-161">Değiştirilen **departmanı** bölüm adını görüntülemek için sütun.</span><span class="sxs-lookup"><span data-stu-id="b68af-161">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="b68af-162">Kod görüntüler `Name` yüklendiği departmanı varlığın özelliği `Department` gezinti özelliği:</span><span class="sxs-lookup"><span data-stu-id="b68af-162">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="b68af-163">Uygulamayı çalıştırmak ve seçmek **kursları** bölüm adları listesini görmek için sekmesinde.</span><span class="sxs-lookup"><span data-stu-id="b68af-163">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kursları dizin sayfası](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="b68af-165">Kursları ve kayıtları gösterir bir eğitmen sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="b68af-165">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="b68af-166">Bu bölümde, eğitmenler sayfasını görüntülemek için bir denetleyici ve görünüm Eğitmen varlığın oluşturacaksınız:</span><span class="sxs-lookup"><span data-stu-id="b68af-166">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Eğitmenler dizin sayfası](read-related-data/_static/instructors-index.png)

<span data-ttu-id="b68af-168">Bu sayfada okur ve ilgili verileri aşağıdaki yollarla görüntüler:</span><span class="sxs-lookup"><span data-stu-id="b68af-168">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="b68af-169">Eğitmenler listesini OfficeAssignment varlıktan ilgili verileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b68af-169">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="b68af-170">Eğitmen ve OfficeAssignment bir sıfır-veya-bir ilişkide varlıklardır.</span><span class="sxs-lookup"><span data-stu-id="b68af-170">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="b68af-171">İstekli yükleme OfficeAssignment varlıklar için kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="b68af-171">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="b68af-172">Birincil tablo alınan tüm satırlarının için ilgili verileri gerektiğinde daha önce açıklandığı gibi istekli yükleme genellikle daha verimli olur.</span><span class="sxs-lookup"><span data-stu-id="b68af-172">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="b68af-173">Bu durumda, tüm görüntülenen Eğitmenler office atamalarını görüntülemek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="b68af-173">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="b68af-174">Kullanıcı bir eğitmen seçtiğinde ilgili kurs varlıkları görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b68af-174">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="b68af-175">Eğitmen ve kurs bir çoktan çoğa ilişki varlıklardır.</span><span class="sxs-lookup"><span data-stu-id="b68af-175">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="b68af-176">İstekli yükleme kurs varlıkları ve bunların ilgili bölüm varlıkları için kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="b68af-176">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="b68af-177">Bu durumda, yalnızca seçili eğitmen için kursları gerektiğinden ayrı sorgular daha verimli olabilir.</span><span class="sxs-lookup"><span data-stu-id="b68af-177">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="b68af-178">Ancak, bu örnek, kendilerini Gezinti özelliklerdir varlıkların içinde gezinme özelliklerinin istekli yükleme kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="b68af-178">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="b68af-179">Kullanıcı bir kurs seçtiğinde ilgili verileri kayıtları varlık kümesindeki görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b68af-179">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="b68af-180">Kursa ve kayıt bir bire çok ilişkiye varlıklardır.</span><span class="sxs-lookup"><span data-stu-id="b68af-180">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="b68af-181">Kayıt varlıkları ve bunların ilişkili Öğrenci varlıklar için ayrı sorgulara kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="b68af-181">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="b68af-182">Eğitmen dizini görünümü için bir görünüm modeli oluşturun</span><span class="sxs-lookup"><span data-stu-id="b68af-182">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="b68af-183">Eğitmenler sayfanın üç farklı tablolardaki verileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="b68af-183">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="b68af-184">Bu nedenle, her bir tablo için verileri tutan üç özellik içeren bir görünüm modeli oluşturmayı öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="b68af-184">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="b68af-185">İçinde *SchoolViewModels* klasör oluşturma *InstructorIndexData.cs* ve varolan kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="b68af-185">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="b68af-186">Eğitmen denetleyici ve görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="b68af-186">Create the Instructor controller and views</span></span>

<span data-ttu-id="b68af-187">Aşağıdaki çizimde gösterildiği gibi EF okuma/yazma eylemleri olan bir eğitmen denetleyicisi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="b68af-187">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![Eğitmenler denetleyici ekleme](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="b68af-189">Açık *InstructorsController.cs* ve kullanarak bir ekleme Viewmodel'lar isim uzayı için:</span><span class="sxs-lookup"><span data-stu-id="b68af-189">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="b68af-190">Index yöntemi istekli yükleme ilgili veri görünüm modelinde koymak için aşağıdaki kod ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b68af-190">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="b68af-191">Yöntemi, isteğe bağlı rota verileri kabul eder (`id`) ve bir sorgu dizesi parametresi (`courseID`) seçilen Eğitmen ve seçili kurs kimliği değerini sağlayın.</span><span class="sxs-lookup"><span data-stu-id="b68af-191">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="b68af-192">Parametreleri tarafından sağlanan **seçin** sayfadaki köprüler.</span><span class="sxs-lookup"><span data-stu-id="b68af-192">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="b68af-193">Görünüm modeli örneği oluşturmayı ve bunu Eğitmenler listesini koyarak kod başlar.</span><span class="sxs-lookup"><span data-stu-id="b68af-193">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="b68af-194">İstekli yükleme için kod belirtir `Instructor.OfficeAssignment` ve `Instructor.CourseAssignments` Gezinti özellikleri.</span><span class="sxs-lookup"><span data-stu-id="b68af-194">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="b68af-195">İçinde `CourseAssignments` özelliği `Course` özelliği yüklü ve o, `Enrollments` ve `Department` özellikleri yüklenir ve her içinde `Enrollment` varlık `Student` özelliğinin yüklü.</span><span class="sxs-lookup"><span data-stu-id="b68af-195">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="b68af-196">Görünümü her zaman OfficeAssignment varlık gerektirdiğinden, aynı sorgu içinde getirmek için daha verimlidir.</span><span class="sxs-lookup"><span data-stu-id="b68af-196">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="b68af-197">Yalnızca sayfanın daha sık olmadan daha seçili bir kurs ile görüntüleniyorsa, tek bir sorguda birden çok sorguyu iyi, bu nedenle bir eğitmen web sayfasında seçildiğinde kurs varlıkları gereklidir.</span><span class="sxs-lookup"><span data-stu-id="b68af-197">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="b68af-198">Kod yineler `CourseAssignments` ve `Course` iki özelliklerinden gerektiğinden `Course`.</span><span class="sxs-lookup"><span data-stu-id="b68af-198">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="b68af-199">İlk dizenin `ThenInclude` alır çağırır `CourseAssignment.Course`, `Course.Enrollments`, ve `Enrollment.Student`.</span><span class="sxs-lookup"><span data-stu-id="b68af-199">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="b68af-200">Bu noktada kodda, başka bir `ThenInclude` Gezinti özellikleri için olacaktır `Student`, hangi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="b68af-200">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="b68af-201">Ancak arama `Include` ile baştan başlayıp `Instructor` domainproperty'leri yeniden, bu durumda belirtilmesinden gitmek zorunda özellikleri `Course.Department` yerine `Course.Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="b68af-201">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="b68af-202">Bir eğitmen seçildiğinde aşağıdaki kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="b68af-202">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="b68af-203">Seçili Eğitmen görünüm modeli eğitmenlerini listesi alınır.</span><span class="sxs-lookup"><span data-stu-id="b68af-203">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="b68af-204">Görünüm modeli `Courses` özelliğinin Bu eğitmen kurs varlıkların ile yüklü ardından `CourseAssignments` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="b68af-204">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="b68af-205">`Where` Yöntem bir koleksiyon döndürür, ancak döndürülen yalnızca tek bir eğitmen varlık yöntemi sonuçlanan ölçütler bu durumda geçirilen.</span><span class="sxs-lookup"><span data-stu-id="b68af-205">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="b68af-206">`Single` Yöntemi erişmenizi sağlar, varlığın tek bir eğitmen varlık koleksiyon dönüştürür `CourseAssignments` özelliği.</span><span class="sxs-lookup"><span data-stu-id="b68af-206">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="b68af-207">`CourseAssignments` Özelliği içeren `CourseAssignment` istediğiniz yalnızca ilgili varlıkları `Course` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="b68af-207">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="b68af-208">Kullandığınız `Single` koleksiyon bildiğiniz durumlarda bir koleksiyon üzerinde yöntemi, yalnızca bir öğe olacaktır.</span><span class="sxs-lookup"><span data-stu-id="b68af-208">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="b68af-209">Tek bir yöntem kendisine geçirilen koleksiyonu boş ise veya birden fazla öğe varsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b68af-209">The Single method throws an exception if the collection passed to it's empty or if there's more than one item.</span></span> <span data-ttu-id="b68af-210">Alternatif `SingleOrDefault`, hangi varsayılan değeri döndürür (Bu durumda null) koleksiyonu boş ise.</span><span class="sxs-lookup"><span data-stu-id="b68af-210">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="b68af-211">Ancak, bu durumda, hala sonuçlanır bir özel durum (bulunmaya çalışılırken gelen bir `Courses` özelliği null bir başvuru), ve özel durum iletisi sorunun nedenini daha net bir şekilde gösterir.</span><span class="sxs-lookup"><span data-stu-id="b68af-211">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="b68af-212">Çağırdığınızda `Single` yöntemi, nerede ayrıca iletebilir çağırmak yerine koşul `Where` yöntemi ayrı olarak:</span><span class="sxs-lookup"><span data-stu-id="b68af-212">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="b68af-213">Onun yerine:</span><span class="sxs-lookup"><span data-stu-id="b68af-213">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="b68af-214">Ardından, bir kurs seçildiyse, görünüm modeli eğitim listesinden seçilen kursu alınır.</span><span class="sxs-lookup"><span data-stu-id="b68af-214">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="b68af-215">Ardından Görünüm modelinin `Enrollments` özelliğinin yüklü o kursun kayıt varlıkların ile `Enrollments` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="b68af-215">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="b68af-216">Eğitmen Index görünümünü değiştirme</span><span class="sxs-lookup"><span data-stu-id="b68af-216">Modify the Instructor Index view</span></span>

<span data-ttu-id="b68af-217">İçinde *Views/Instructors/Index.cshtml*, şablonu kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b68af-217">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="b68af-218">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="b68af-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="b68af-219">Varolan kodu aşağıdaki değişiklikler yaptınız:</span><span class="sxs-lookup"><span data-stu-id="b68af-219">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="b68af-220">Model sınıfı için değiştirilen `InstructorIndexData`.</span><span class="sxs-lookup"><span data-stu-id="b68af-220">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="b68af-221">Sayfa başlığı değiştirilen **dizin** için **Eğitmenler**.</span><span class="sxs-lookup"><span data-stu-id="b68af-221">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="b68af-222">Eklenen bir **Office** görüntüleyen sütun `item.OfficeAssignment.Location` yalnızca `item.OfficeAssignment` null değil.</span><span class="sxs-lookup"><span data-stu-id="b68af-222">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="b68af-223">(Bu bir sıfır-veya-bir ilişkisi olduğundan, olmayabilir ilgili OfficeAssignment varlık.)</span><span class="sxs-lookup"><span data-stu-id="b68af-223">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="b68af-224">Eklenen bir **kursları** kursları görüntüleyen sütun her Eğitmenler tarafından verilen.</span><span class="sxs-lookup"><span data-stu-id="b68af-224">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="b68af-225">Bkz: [açık satır geçişle `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) bu razor sözdizimi hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="b68af-225">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="b68af-226">Dinamik olarak ekleyen, eklenen kod `class="success"` için `tr` seçili Eğitmen öğesidir.</span><span class="sxs-lookup"><span data-stu-id="b68af-226">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="b68af-227">Bu önyükleme sınıfını kullanarak seçili satır için bir arka plan rengini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b68af-227">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="b68af-228">Etiketli yeni köprü eklenmiş **seçin** hemen diğer bağlantıları önce her bir satırdaki neden olan gönderilmesini seçili Eğitmen kimliği `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b68af-228">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="b68af-229">Uygulamayı çalıştırmak ve seçmek **Eğitmenler** sekmesi. İlgili hiçbir OfficeAssignment varlık olduğunda ilgili OfficeAssignment varlıkları ve bir boş tablo hücresi Location özelliği sayfasında görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b68af-229">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Hiçbir şey seçili Eğitmenler dizin sayfası](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="b68af-231">İçinde *Views/Instructors/Index.cshtml* kapatma tablo sonra öğesi (sonunda dosyası), dosya aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b68af-231">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="b68af-232">Bu kod bir eğitmen seçildiğinde bir eğitmen için ilgili kurslar listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b68af-232">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="b68af-233">Bu kodu okuyan `Courses` özelliği kursları listesini görüntülemek için Görünüm modeli.</span><span class="sxs-lookup"><span data-stu-id="b68af-233">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="b68af-234">Ayrıca sağlar bir **seçin** seçili kursa kimliği gönderen köprü `Index` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b68af-234">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="b68af-235">Sayfayı yenileyin ve bir eğitmen seçin.</span><span class="sxs-lookup"><span data-stu-id="b68af-235">Refresh the page and select an instructor.</span></span> <span data-ttu-id="b68af-236">Seçili eğitmen için atanan kursları görüntüleyen bir kılavuz göreceksiniz ve her kurs için atanan bölüm adını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="b68af-236">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Seçili Eğitmenler dizin sayfası Eğitmen](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="b68af-238">Yeni eklediğiniz kod bloğundan sonra aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b68af-238">After the code block you just added, add the following code.</span></span> <span data-ttu-id="b68af-239">Bu kurs seçildiğinde bu kurs kayıtlı öğrencilere listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b68af-239">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="b68af-240">Bu kod, kursun kayıtlı öğrencilerin listesini görüntülemek için Görünüm modeli kayıtları özelliği okur.</span><span class="sxs-lookup"><span data-stu-id="b68af-240">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="b68af-241">Sayfayı yenileyin ve bir eğitmen seçin.</span><span class="sxs-lookup"><span data-stu-id="b68af-241">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="b68af-242">Ardından bir kurs kayıtlı Öğrenci ve kendi derece listesini görmek için seçin.</span><span class="sxs-lookup"><span data-stu-id="b68af-242">Then select a course to see the list of enrolled students and their grades.</span></span>

![Eğitmenler dizin sayfası Eğitmen ve seçilen kursu](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="b68af-244">Açık yükleme</span><span class="sxs-lookup"><span data-stu-id="b68af-244">Explicit loading</span></span>

<span data-ttu-id="b68af-245">Alınan ne zaman eğitmenlerini listesini *InstructorsController.cs*, istekli yükleme için belirttiğiniz `CourseAssignments` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="b68af-245">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="b68af-246">Nadiren kayıtları seçili Eğitmen ve kursu görmek istediğiniz kullanıcıların beklenen varsayalım.</span><span class="sxs-lookup"><span data-stu-id="b68af-246">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="b68af-247">Bu durumda, yalnızca isteniyorsa kayıt verileri yüklemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b68af-247">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="b68af-248">Açık yükleme yapmak nasıl bir örnek görmek isterseniz değiştirme `Index` yöntemini aşağıdaki kodla istekli yükleme kayıtları için kaldırır ve bu özelliği açıkça yükler.</span><span class="sxs-lookup"><span data-stu-id="b68af-248">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="b68af-249">Kod değişikliklerini vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="b68af-249">The code changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="b68af-250">Yeni kod bıraktığı *ThenInclude* yöntemini çağırır için kayıt verilerini koddan Eğitmen varlıkları alır.</span><span class="sxs-lookup"><span data-stu-id="b68af-250">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="b68af-251">Bir eğitmen ve kurs seçtiyseniz, seçili kurs için kayıt varlıkları ve her kayıt için Öğrenci varlıklar vurgulanmış kodu alır.</span><span class="sxs-lookup"><span data-stu-id="b68af-251">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="b68af-252">Çalıştırma verileri nasıl alınıp değiştirdik ancak Eğitmenler dizin sayfası artık ve, go uygulaması sayfasında, görüntülenen içinde herhangi bir fark görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="b68af-252">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="b68af-253">Özet</span><span class="sxs-lookup"><span data-stu-id="b68af-253">Summary</span></span>

<span data-ttu-id="b68af-254">Artık istekli yükleme birden fazla sorgu içeren bir sorgu ile ilgili verileri Gezinti özelliklerini okumak için kullandınız.</span><span class="sxs-lookup"><span data-stu-id="b68af-254">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="b68af-255">Sonraki öğreticide ilgili verileri güncelleştirme öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="b68af-255">In the next tutorial you'll learn how to update related data.</span></span>

::: moniker-end

>[!div class="step-by-step"]
><span data-ttu-id="b68af-256">[Önceki](complex-data-model.md)
>[İleri](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="b68af-256">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>
