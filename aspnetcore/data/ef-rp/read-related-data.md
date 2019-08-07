---
title: ASP.NET Core EF Core ile Razor Pages-Ilgili verileri oku-6/8
author: rick-anderson
description: Bu öğreticide ilgili verileri okuyabilir ve görüntüleriz, yani Entity Framework gezinti özelliklerine yüklediği veriler.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 93fbb4741f476368d75d23162d6e2597de7b263e
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819909"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="20b3e-103">ASP.NET Core EF Core ile Razor Pages-Ilgili verileri oku-6/8</span><span class="sxs-lookup"><span data-stu-id="20b3e-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="20b3e-104">, [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog)ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="20b3e-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="20b3e-105">Bu öğreticide ilgili veriler okundu ve görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="20b3e-106">İlgili veriler, EF Core gezinti özelliklerine yüklediği veri.</span><span class="sxs-lookup"><span data-stu-id="20b3e-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="20b3e-107">Olamaz çözmenize, sorunlarla karşılaşırsanız, [indirin veya tamamlanmış uygulamayı görüntüleyin.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="20b3e-107">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="20b3e-108">[Yükleme yönergeleri](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="20b3e-108">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="20b3e-109">Aşağıdaki çizimler, Bu öğreticinin tamamlanan sayfalarını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="20b3e-109">The following illustrations show the completed pages for this tutorial:</span></span>

![Kurslar Dizin sayfası](read-related-data/_static/courses-index.png)

![Eğitmenler Dizin sayfası](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="20b3e-112">İlgili verilerin Eager, açık ve geç yüklemesi</span><span class="sxs-lookup"><span data-stu-id="20b3e-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="20b3e-113">EF Core bir varlığın gezinti özelliklerine ilgili verileri yükleyebilmenin birkaç yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="20b3e-113">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="20b3e-114">[Eager yükleniyor](/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="20b3e-114">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="20b3e-115">Ekip yükleme, bir varlık türü için bir sorgu aynı zamanda ilgili varlıkları de yüklediğinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="20b3e-115">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="20b3e-116">Varlık okunmadığınızda ilgili veriler alınır.</span><span class="sxs-lookup"><span data-stu-id="20b3e-116">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="20b3e-117">Bu, genellikle gereken tüm verileri alan tek bir JOIN sorgusuna neden olur.</span><span class="sxs-lookup"><span data-stu-id="20b3e-117">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="20b3e-118">EF Core, bazı Eager yükleme türleri için birden çok sorgu yayımlayacak.</span><span class="sxs-lookup"><span data-stu-id="20b3e-118">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="20b3e-119">Birden çok sorgu verilmesi, EF6 içindeki bazı sorgular için tek bir sorgunun bulunduğu durumda daha verimli olabilir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-119">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="20b3e-120">Eager yüklemesi `Include` ve `ThenInclude` yöntemleriyle birlikte belirtilir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-120">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Eager yükleme örneği](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="20b3e-122">Bir koleksiyon gezintisi eklendiğinde Eager yüklemesi birden çok sorgu gönderir:</span><span class="sxs-lookup"><span data-stu-id="20b3e-122">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="20b3e-123">Ana sorgu için bir sorgu</span><span class="sxs-lookup"><span data-stu-id="20b3e-123">One query for the main query</span></span> 
  * <span data-ttu-id="20b3e-124">Yükleme ağacındaki her koleksiyon "Edge" için bir sorgu.</span><span class="sxs-lookup"><span data-stu-id="20b3e-124">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="20b3e-125">Sorguları ile `Load`ayır: Veriler ayrı sorgularda alınabilir ve gezinti özellikleri ' EF Core "düzeltir".</span><span class="sxs-lookup"><span data-stu-id="20b3e-125">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="20b3e-126">"düzeltmeler", EF Core gezinti özelliklerini otomatik olarak dolduran anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-126">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="20b3e-127">İle `Load` ayrı sorgular, yükleme sonrasında yükleme öncesi yükleme gibidir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-127">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

  ![Ayrı sorgular örneği](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="20b3e-129">Not: EF Core, daha önce bağlam örneğine yüklenmiş olan diğer varlıklara gezinti özelliklerini otomatik olarak düzeltir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-129">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="20b3e-130">Bir gezinti özelliği için veriler açıkça dahil edilmese bile, ilgili varlıkların bazıları veya tümü daha önce yüklenmişse Özellik yine de doldurulabilir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-130">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="20b3e-131">[Açık yükleme](/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="20b3e-131">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="20b3e-132">Varlık ilk kez okunmadıysa ilgili veriler alınmadı.</span><span class="sxs-lookup"><span data-stu-id="20b3e-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="20b3e-133">Gerektiğinde ilgili verileri almak için kodun yazılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-133">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="20b3e-134">Ayrı sorgularla açık yükleme, VERITABANıNA birden çok sorgu gönderilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="20b3e-134">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="20b3e-135">Açık yükleme ile kod, yüklenecek gezinti özelliklerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-135">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="20b3e-136">Açık yükleme yapmak için yönteminikullanın.`Load`</span><span class="sxs-lookup"><span data-stu-id="20b3e-136">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="20b3e-137">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="20b3e-137">For example:</span></span>

  ![Açık yükleme örneği](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="20b3e-139">[Yavaş yükleme](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="20b3e-139">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="20b3e-140">[Sürüm 2,1 ' de EF Core geç yükleme eklendi](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="20b3e-140">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="20b3e-141">Varlık ilk kez okunmadıysa ilgili veriler alınmadı.</span><span class="sxs-lookup"><span data-stu-id="20b3e-141">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="20b3e-142">Gezinti özelliğine ilk kez erişildiğinde, bu gezinti özelliği için gereken veriler otomatik olarak alınır.</span><span class="sxs-lookup"><span data-stu-id="20b3e-142">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="20b3e-143">Bir gezinti özelliğine ilk kez erişildiğinde bir sorgu VERITABANıNA gönderilir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-143">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="20b3e-144">`Select` İşleci yalnızca gerekli ilgili verileri yükler.</span><span class="sxs-lookup"><span data-stu-id="20b3e-144">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-course-page-that-displays-department-name"></a><span data-ttu-id="20b3e-145">Bölüm adını görüntüleyen bir kurs sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="20b3e-145">Create a Course page that displays department name</span></span>

<span data-ttu-id="20b3e-146">Kurs varlığı, `Department` varlığı içeren bir gezinti özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-146">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="20b3e-147">`Department` Varlık, kursun atandığı departmanı içerir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-147">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="20b3e-148">Bir kurs listesinde atanan departmanın adını göstermek için:</span><span class="sxs-lookup"><span data-stu-id="20b3e-148">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="20b3e-149">`Department` Varlıktaki `Name` özelliği alın.</span><span class="sxs-lookup"><span data-stu-id="20b3e-149">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="20b3e-150">Varlık, `Course.Department` gezinti özelliğinden gelir. `Department`</span><span class="sxs-lookup"><span data-stu-id="20b3e-150">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Kurs. Departmanı](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>

### <a name="scaffold-the-course-model"></a><span data-ttu-id="20b3e-152">Kurs modelini temklesi</span><span class="sxs-lookup"><span data-stu-id="20b3e-152">Scaffold the Course model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20b3e-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20b3e-153">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="20b3e-154">Bölümündeki yönergeleri [Öğrenci modeli iskelesini](xref:data/ef-rp/intro#scaffold-the-student-model) ve `Course` model sınıfı için.</span><span class="sxs-lookup"><span data-stu-id="20b3e-154">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Course` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="20b3e-155">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="20b3e-155">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="20b3e-156">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="20b3e-156">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

---

<span data-ttu-id="20b3e-157">Önceki komut iskelesini kurar `Course` modeli.</span><span class="sxs-lookup"><span data-stu-id="20b3e-157">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="20b3e-158">Projeyi Visual Studio'da açın.</span><span class="sxs-lookup"><span data-stu-id="20b3e-158">Open the project in Visual Studio.</span></span>

<span data-ttu-id="20b3e-159">*Pages/kurslar/Index. cshtml. cs* dosyasını açın ve `OnGetAsync` metodunu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="20b3e-159">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="20b3e-160">Yapı iskelesi altyapısı, `Department` gezinti özelliği için belirtilen Eager 'yi belirtti.</span><span class="sxs-lookup"><span data-stu-id="20b3e-160">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="20b3e-161">`Include` Yöntemi Eager yüklemeyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-161">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="20b3e-162">Uygulamayı çalıştırın ve **Kurslar** bağlantısını seçin.</span><span class="sxs-lookup"><span data-stu-id="20b3e-162">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="20b3e-163">Departman sütunu, yararlı olmayan `DepartmentID`öğesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="20b3e-163">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="20b3e-164">`OnGetAsync` Yöntemi aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="20b3e-164">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="20b3e-165">Yukarıdaki kod ekler `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="20b3e-165">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="20b3e-166">`AsNoTracking`Döndürülen varlıklar izlenmediğinden performansı geliştirir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-166">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="20b3e-167">Geçerli bağlamda güncelleştirilmemiş oldukları için varlıklar izlenmiyor.</span><span class="sxs-lookup"><span data-stu-id="20b3e-167">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="20b3e-168">*Pages/kurslar/Index. cshtml* 'yi aşağıdaki vurgulanmış işaretlerle güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="20b3e-168">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="20b3e-169">Yapı iskelesi kodunda aşağıdaki değişiklikler yapılmıştır:</span><span class="sxs-lookup"><span data-stu-id="20b3e-169">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="20b3e-170">Başlık dizinden kurslar olarak değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="20b3e-170">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="20b3e-171">`CourseID` Özellik değerini gösteren bir **sayı** sütunu eklendi.</span><span class="sxs-lookup"><span data-stu-id="20b3e-171">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="20b3e-172">Birincil anahtarlar, genellikle son kullanıcılara anlamlı olduklarından, varsayılan olarak yapı iskelesi göstermemektedir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-172">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="20b3e-173">Ancak, bu durumda birincil anahtar anlamlı olur.</span><span class="sxs-lookup"><span data-stu-id="20b3e-173">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="20b3e-174">Departman adını göstermek için **Departman** sütunu değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="20b3e-174">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="20b3e-175">Kod, `Department` gezinti özelliğine `Name` yüklenen `Department` varlığın özelliğini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="20b3e-175">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="20b3e-176">Uygulamayı çalıştırın ve bölüm adlarıyla listeyi görmek için **Kurslar** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="20b3e-176">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kurslar Dizin sayfası](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="20b3e-178">Select ile ilgili verileri yükleme</span><span class="sxs-lookup"><span data-stu-id="20b3e-178">Loading related data with Select</span></span>

<span data-ttu-id="20b3e-179">`OnGetAsync` Yöntemi ,`Include` yöntemiyle ilgili verileri yükler:</span><span class="sxs-lookup"><span data-stu-id="20b3e-179">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="20b3e-180">`Select` İşleci yalnızca gerekli ilgili verileri yükler.</span><span class="sxs-lookup"><span data-stu-id="20b3e-180">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="20b3e-181">Tek öğeler için, `Department.Name` örneğin bir SQL iç birleşimi kullanır.</span><span class="sxs-lookup"><span data-stu-id="20b3e-181">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="20b3e-182">Koleksiyonlar için başka bir veritabanı erişimi kullanır, ancak koleksiyonlar üzerindeki `Include` işleci bu şekilde yapar.</span><span class="sxs-lookup"><span data-stu-id="20b3e-182">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="20b3e-183">Aşağıdaki kod, `Select` yöntemiyle ilgili verileri yükler:</span><span class="sxs-lookup"><span data-stu-id="20b3e-183">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="20b3e-184">`CourseViewModel`Şunları yapın:</span><span class="sxs-lookup"><span data-stu-id="20b3e-184">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="20b3e-185">Tüm örnek için bkz. [ındexselect. cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) ve [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) .</span><span class="sxs-lookup"><span data-stu-id="20b3e-185">See [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="20b3e-186">Kurslar ve kayıtları gösteren bir eğitmenler sayfası oluşturun</span><span class="sxs-lookup"><span data-stu-id="20b3e-186">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="20b3e-187">Bu bölümde, Eğitmenler sayfası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="20b3e-187">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="20b3e-188">![Eğitmenler Dizin sayfası](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="20b3e-188">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="20b3e-189">Bu sayfa aşağıdaki yollarla ilgili verileri okur ve görüntüler:</span><span class="sxs-lookup"><span data-stu-id="20b3e-189">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="20b3e-190">Eğitmenler listesi, `OfficeAssignment` varlıktaki ilgili verileri (önceki görüntüde Office) görüntüler.</span><span class="sxs-lookup"><span data-stu-id="20b3e-190">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="20b3e-191">`Instructor` Ve`OfficeAssignment` varlıkları bire sıfır veya-bir ilişkidir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-191">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="20b3e-192">`OfficeAssignment` Varlıklar için Eager yüklemesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="20b3e-192">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="20b3e-193">Eager yüklemesi, ilgili verilerin görüntülenmesi gerektiğinde genellikle daha etkilidir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-193">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="20b3e-194">Bu durumda, Eğitmenler için Office atamaları görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-194">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="20b3e-195">Kullanıcı bir eğitmen (önceki görüntüde haruı) seçtiğinde ilgili `Course` varlıklar görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-195">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="20b3e-196">`Instructor` Ve`Course` varlıkları çoktan çoğa bir ilişkidir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-196">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="20b3e-197">Eager yüklemesi, `Course` varlıklar ve ilgili `Department` varlıklar için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="20b3e-197">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="20b3e-198">Bu durumda, yalnızca seçili eğitmen için kurslar gerektiğinden ayrı sorgular daha verimli olabilir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-198">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="20b3e-199">Bu örnek, gezinti özelliklerinde olan varlıklarda gezinti özellikleri için Eager yükleme 'nin nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-199">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="20b3e-200">Kullanıcı bir kurs seçtiğinde (önceki görüntüde Chemistry), `Enrollments` varlıktaki ilgili veriler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-200">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="20b3e-201">Önceki görüntüde öğrenci adı ve sınıf görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-201">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="20b3e-202">`Course` Ve`Enrollment` varlıkları bire çok ilişkidir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-202">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="20b3e-203">Eğitmen dizini görünümü için bir görünüm modeli oluşturun</span><span class="sxs-lookup"><span data-stu-id="20b3e-203">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="20b3e-204">Eğitmenler sayfasında, üç farklı tablodan alınan veriler gösterilir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-204">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="20b3e-205">Üç tabloyu temsil eden üç varlığı içeren bir görünüm modeli oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="20b3e-205">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="20b3e-206">*SchoolViewModels* klasöründe, aşağıdaki kodla *InstructorIndexData.cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="20b3e-206">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="20b3e-207">Eğitmen modelini scafkatlama</span><span class="sxs-lookup"><span data-stu-id="20b3e-207">Scaffold the Instructor model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20b3e-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20b3e-208">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="20b3e-209">Bölümündeki yönergeleri [Öğrenci modeli iskelesini](xref:data/ef-rp/intro#scaffold-the-student-model) ve `Instructor` model sınıfı için.</span><span class="sxs-lookup"><span data-stu-id="20b3e-209">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Instructor` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="20b3e-210">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="20b3e-210">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="20b3e-211">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="20b3e-211">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="20b3e-212">Önceki komut iskelesini kurar `Instructor` modeli.</span><span class="sxs-lookup"><span data-stu-id="20b3e-212">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="20b3e-213">Uygulamayı çalıştırın ve eğitmenler sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="20b3e-213">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="20b3e-214">*Pages/eğitmenler/Index. cshtml. cs* öğesini şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="20b3e-214">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="20b3e-215">`OnGetAsync` Yöntemi, seçilen eğitmenin kimliği için isteğe bağlı rota verilerini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="20b3e-215">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="20b3e-216">*Pages/eğitmenler/Index. cshtml. cs* dosyasındaki sorguyu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="20b3e-216">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="20b3e-217">Sorgunun iki içerme vardır:</span><span class="sxs-lookup"><span data-stu-id="20b3e-217">The query has two includes:</span></span>

* <span data-ttu-id="20b3e-218">`OfficeAssignment`: [Eğitmenler görünümünde](#IP)görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-218">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="20b3e-219">`CourseAssignments`: Bu, kurs dünyasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="20b3e-219">`CourseAssignments`: Which brings in the courses taught.</span></span>

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="20b3e-220">Eğitmenler Dizin sayfasını Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="20b3e-220">Update the instructors Index page</span></span>

<span data-ttu-id="20b3e-221">*Sayfaları/eğitmenler/Index. cshtml* 'yi şu biçimlendirmeyle güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="20b3e-221">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="20b3e-222">Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="20b3e-222">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="20b3e-223">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="20b3e-223">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="20b3e-224">`"{id:int?}"`bir yol şablonudur.</span><span class="sxs-lookup"><span data-stu-id="20b3e-224">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="20b3e-225">Yol şablonu, verileri yönlendirmek için URL 'deki tamsayı Sorgu dizelerini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-225">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="20b3e-226">Örneğin, yalnızca `@page` yönergeyle bir eğitmenin **Select** bağlantısına tıklanması aşağıdakine benzer bir URL oluşturur:</span><span class="sxs-lookup"><span data-stu-id="20b3e-226">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `http://localhost:1234/Instructors?id=2`

  <span data-ttu-id="20b3e-227">Sayfa yönergesi `@page "{id:int?}"`olduğunda, önceki URL şu şekilde olur:</span><span class="sxs-lookup"><span data-stu-id="20b3e-227">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

  `http://localhost:1234/Instructors/2`

* <span data-ttu-id="20b3e-228">Sayfa başlığı **eğitmenler**' dir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-228">Page title is **Instructors**.</span></span>
* <span data-ttu-id="20b3e-229">Yalnızca null olmaması halinde `item.OfficeAssignment` görüntülenen bir **Office** sütunu eklendi. `item.OfficeAssignment.Location`</span><span class="sxs-lookup"><span data-stu-id="20b3e-229">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="20b3e-230">Bu bire sıfır veya-bir ilişki olduğundan ilgili bir OfficeAssignment varlığı bulunmayabilir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-230">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="20b3e-231">Her bir eğitmen tarafından taders kurslarını görüntüleyen bir **Kurslar** sütunu eklendi.</span><span class="sxs-lookup"><span data-stu-id="20b3e-231">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="20b3e-232">Bu Razor sözdizimi hakkında daha fazla bilgi için bkz. [açık hat geçişi `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) .</span><span class="sxs-lookup"><span data-stu-id="20b3e-232">See [Explicit line transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="20b3e-233">Seçilen eğitmenin `tr` öğesine dinamik olarak `class="success"` ekleyen kod eklendi.</span><span class="sxs-lookup"><span data-stu-id="20b3e-233">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="20b3e-234">Bu, bir önyükleme sınıfı kullanarak seçili satır için bir arka plan rengi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="20b3e-234">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="20b3e-235">**Select**etiketli yeni bir köprü eklendi.</span><span class="sxs-lookup"><span data-stu-id="20b3e-235">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="20b3e-236">Bu bağlantı, seçilen eğitmenin kimliğini `Index` yönteme gönderir ve bir arka plan rengi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="20b3e-236">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="20b3e-237">Uygulamayı çalıştırın ve **eğitmenler** sekmesini seçin. Sayfa, `Location` ilgili `OfficeAssignment` varlıktaki (Office) öğesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="20b3e-237">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="20b3e-238">Eğer OfficeAssignment ' null ise boş bir tablo hücresi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-238">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Eğitmenler Dizin sayfası seçili öğe yok](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="20b3e-240">**Seç** bağlantısına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="20b3e-240">Click on the **Select** link.</span></span> <span data-ttu-id="20b3e-241">Satır stili değişir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-241">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="20b3e-242">Seçili eğitmen 'e göre kurslar ekleyin</span><span class="sxs-lookup"><span data-stu-id="20b3e-242">Add courses taught by selected instructor</span></span>

<span data-ttu-id="20b3e-243">*Pages/eğitmenler/Index. cshtml. cs* dosyasındaki yöntemiaşağıdakikodlagüncelleştirin:`OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="20b3e-243">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="20b3e-244">Ekleyemiyorum`public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="20b3e-244">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="20b3e-245">Güncelleştirilmiş sorguyu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="20b3e-245">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="20b3e-246">Önceki sorgu `Department` varlıkları ekler.</span><span class="sxs-lookup"><span data-stu-id="20b3e-246">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="20b3e-247">Aşağıdaki kod, bir eğitmen seçildiğinde (`id != null`) yürütülür.</span><span class="sxs-lookup"><span data-stu-id="20b3e-247">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="20b3e-248">Seçilen eğitmen, görünüm modelindeki eğitmenler listesinden alınır.</span><span class="sxs-lookup"><span data-stu-id="20b3e-248">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="20b3e-249">Görünüm modelinin `Courses` özelliği, bu eğitmenin `CourseAssignments` gezinti özelliğinden alınan `Course` varlıklarla birlikte yüklenir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-249">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="20b3e-250">`Where` Yöntemi bir koleksiyon döndürür.</span><span class="sxs-lookup"><span data-stu-id="20b3e-250">The `Where` method returns a collection.</span></span> <span data-ttu-id="20b3e-251">Önceki `Where` yöntemde yalnızca tek `Instructor` bir varlık döndürülür.</span><span class="sxs-lookup"><span data-stu-id="20b3e-251">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="20b3e-252">Yöntemi, koleksiyonu tek `Instructor` bir varlığa dönüştürür. `Single`</span><span class="sxs-lookup"><span data-stu-id="20b3e-252">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="20b3e-253">Varlık, `CourseAssignments` özelliğine erişim sağlar. `Instructor`</span><span class="sxs-lookup"><span data-stu-id="20b3e-253">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="20b3e-254">`CourseAssignments`ilgili `Course` varlıklara erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="20b3e-254">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Eğitmenden kurslar M:d](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="20b3e-256">Koleksiyonda yalnızca bir öğe olduğunda Yöntembirkoleksiyondakullanılır.`Single`</span><span class="sxs-lookup"><span data-stu-id="20b3e-256">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="20b3e-257">Koleksiyon boşsa veya birden fazla öğe varsa Yöntembirözeldurumoluşturur.`Single`</span><span class="sxs-lookup"><span data-stu-id="20b3e-257">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="20b3e-258">Koleksiyon boşsa varsayılan `SingleOrDefault`bir değer (Bu durumda null) döndüren alternatif bir alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-258">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="20b3e-259">Boş `SingleOrDefault` bir koleksiyonda kullanma:</span><span class="sxs-lookup"><span data-stu-id="20b3e-259">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="20b3e-260">Bir özel durum sonucu oluşur (null başvuru üzerinde bir `Courses` Özellik bulmaya çalışırken).</span><span class="sxs-lookup"><span data-stu-id="20b3e-260">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="20b3e-261">Özel durum iletisi sorunun nedenini daha az gösterir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-261">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="20b3e-262">Aşağıdaki kod, bir kurs seçildiğinde görünüm modelinin `Enrollments` özelliğini doldurur:</span><span class="sxs-lookup"><span data-stu-id="20b3e-262">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="20b3e-263">*Pages/eğitmenler/Index. cshtml* Razor sayfasının sonuna aşağıdaki biçimlendirmeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="20b3e-263">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="20b3e-264">Yukarıdaki biçimlendirme, bir eğitmen seçildiğinde bir eğitmenin ilgili kursların bir listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="20b3e-264">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="20b3e-265">Uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="20b3e-265">Test the app.</span></span> <span data-ttu-id="20b3e-266">Eğitmenler sayfasında bir **seçme** bağlantısına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="20b3e-266">Click on a **Select** link on the instructors page.</span></span>

![Eğitmenler Dizin sayfası eğitmeni seçildi](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="20b3e-268">Öğrenci verilerini göster</span><span class="sxs-lookup"><span data-stu-id="20b3e-268">Show student data</span></span>

<span data-ttu-id="20b3e-269">Bu bölümde, uygulama seçili bir kurs için öğrenci verilerini gösterecek şekilde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-269">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="20b3e-270">`OnGetAsync` *Pages/eğitmenler/Index. cshtml. cs* içindeki yöntemdeki sorguyu aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="20b3e-270">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="20b3e-271">Güncelleştirme *sayfaları/eğitmenler/Index. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="20b3e-271">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="20b3e-272">Aşağıdaki biçimlendirmeyi dosyanın sonuna ekleyin:</span><span class="sxs-lookup"><span data-stu-id="20b3e-272">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="20b3e-273">Yukarıdaki biçimlendirme, seçili kursa kayıtlı öğrencilerin bir listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="20b3e-273">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="20b3e-274">Sayfayı yenileyin ve bir eğitmen seçin.</span><span class="sxs-lookup"><span data-stu-id="20b3e-274">Refresh the page and select an instructor.</span></span> <span data-ttu-id="20b3e-275">Kayıtlı öğrenciler ve bunların onların listesini görmek için bir kurs seçin.</span><span class="sxs-lookup"><span data-stu-id="20b3e-275">Select a course to see the list of enrolled students and their grades.</span></span>

![Eğitmenler Dizin sayfası eğitmeni ve kursu seçildi](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="20b3e-277">Tek kullanımı</span><span class="sxs-lookup"><span data-stu-id="20b3e-277">Using Single</span></span>

<span data-ttu-id="20b3e-278">Yöntemi yöntemi ayrı çağırmak yerine `Where` koşulu geçirebilir: `Where` `Single`</span><span class="sxs-lookup"><span data-stu-id="20b3e-278">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="20b3e-279">Yukarıdaki `Single` yaklaşım, kullanarak `Where`herhangi bir avantaj sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="20b3e-279">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="20b3e-280">Bazı geliştiriciler `Single` yaklaşım stilini tercih eder.</span><span class="sxs-lookup"><span data-stu-id="20b3e-280">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="20b3e-281">Açık yükleme</span><span class="sxs-lookup"><span data-stu-id="20b3e-281">Explicit loading</span></span>

<span data-ttu-id="20b3e-282">Geçerli kod ve `Enrollments` `Students`için Eager yüklemeyi belirtir:</span><span class="sxs-lookup"><span data-stu-id="20b3e-282">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="20b3e-283">Kullanıcıların bir kursa kayıtları nadiren görmek istediğini varsayalım.</span><span class="sxs-lookup"><span data-stu-id="20b3e-283">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="20b3e-284">Bu durumda, bir iyileştirme yalnızca isteniyorsa kayıt verilerini yüklemek olacaktır.</span><span class="sxs-lookup"><span data-stu-id="20b3e-284">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="20b3e-285">Bu bölümde `OnGetAsync` , ve `Students`' nin `Enrollments` açık yüklemesini kullanacak şekilde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-285">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="20b3e-286">`OnGetAsync` Aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="20b3e-286">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="20b3e-287">Yukarıdaki kod, kayıt ve öğrenci verileri için *Thenınclude* Yöntem çağrılarını bırakır.</span><span class="sxs-lookup"><span data-stu-id="20b3e-287">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="20b3e-288">Bir kurs seçilirse vurgulanan kod şunu alır:</span><span class="sxs-lookup"><span data-stu-id="20b3e-288">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="20b3e-289">Seçili kurs için varlıklar. `Enrollment`</span><span class="sxs-lookup"><span data-stu-id="20b3e-289">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="20b3e-290">Her biri`Enrollment`için varlıklar. `Student`</span><span class="sxs-lookup"><span data-stu-id="20b3e-290">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="20b3e-291">Yukarıdaki kod açıklamalarının `.AsNoTracking()`olduğunu fark edin.</span><span class="sxs-lookup"><span data-stu-id="20b3e-291">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="20b3e-292">Gezinti özellikleri yalnızca izlenen varlıklar için açık bir şekilde yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-292">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="20b3e-293">Uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="20b3e-293">Test the app.</span></span> <span data-ttu-id="20b3e-294">Bir kullanıcının perspektifinden, uygulama önceki sürümle aynı şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="20b3e-294">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="20b3e-295">Sonraki öğreticide ilgili verileri güncelleştirme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="20b3e-295">The next tutorial shows how to update related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="20b3e-296">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="20b3e-296">Additional resources</span></span>

* [<span data-ttu-id="20b3e-297">Bu öğreticinin YouTube sürümü (part1)</span><span class="sxs-lookup"><span data-stu-id="20b3e-297">YouTube version of this tutorial (part1)</span></span>](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [<span data-ttu-id="20b3e-298">Bu öğreticinin YouTube sürümü (part2)</span><span class="sxs-lookup"><span data-stu-id="20b3e-298">YouTube version of this tutorial (part2)</span></span>](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
><span data-ttu-id="20b3e-299">[Önceki](xref:data/ef-rp/complex-data-model)İleri
>[](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="20b3e-299">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
