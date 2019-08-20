---
title: ASP.NET Core EF Core ile Razor Pages-Ilgili verileri oku-6/8
author: tdykstra
description: Bu öğreticide ilgili verileri okuyabilir ve görüntüleriz, yani Entity Framework gezinti özelliklerine yüklediği veriler.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/read-related-data
ms.openlocfilehash: c9da404b1bbd072d3e033f18a7366169082dac06
ms.sourcegitcommit: 257cc3fe8c1d61341aa3b07e5bc0fa3d1c1c1d1c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69583553"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="b1d8c-103">ASP.NET Core EF Core ile Razor Pages-Ilgili verileri oku-6/8</span><span class="sxs-lookup"><span data-stu-id="b1d8c-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="b1d8c-104">, [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog)ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b1d8c-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b1d8c-105">Bu öğreticide, ilgili verilerin nasıl okunacağı ve görüntüleneceği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-105">This tutorial shows how to read and display related data.</span></span> <span data-ttu-id="b1d8c-106">İlgili veriler, EF Core gezinti özelliklerine yüklediği veri.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="b1d8c-107">Aşağıdaki çizimler, Bu öğreticinin tamamlanan sayfalarını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-107">The following illustrations show the completed pages for this tutorial:</span></span>

![Kurslar Dizin sayfası](read-related-data/_static/courses-index30.png)

![Eğitmenler Dizin sayfası](read-related-data/_static/instructors-index30.png)

## <a name="eager-explicit-and-lazy-loading"></a><span data-ttu-id="b1d8c-110">Eager, açık ve yavaş yükleme</span><span class="sxs-lookup"><span data-stu-id="b1d8c-110">Eager, explicit, and lazy loading</span></span>

<span data-ttu-id="b1d8c-111">EF Core bir varlığın gezinti özelliklerine ilgili verileri yükleyebilmenin birkaç yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-111">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="b1d8c-112">[Eager yükleniyor](/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="b1d8c-112">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="b1d8c-113">Ekip yükleme, bir varlık türü için bir sorgu aynı zamanda ilgili varlıkları de yüklediğinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-113">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="b1d8c-114">Bir varlık okunmadığınızda ilgili veriler alınır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-114">When an entity is read, its related data is retrieved.</span></span> <span data-ttu-id="b1d8c-115">Bu, genellikle gereken tüm verileri alan tek bir JOIN sorgusuna neden olur.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-115">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="b1d8c-116">EF Core, bazı Eager yükleme türleri için birden çok sorgu yayımlayacak.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-116">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="b1d8c-117">Birden çok sorgu verme, çok büyük paketlerini tek sorgusundan daha verimli olabilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-117">Issuing multiple queries can be more efficient than a giant single query.</span></span> <span data-ttu-id="b1d8c-118">Eager yüklemesi `Include` ve `ThenInclude` yöntemleriyle birlikte belirtilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-118">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Eager yükleme örneği](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="b1d8c-120">Bir koleksiyon gezintisi eklendiğinde Eager yüklemesi birden çok sorgu gönderir:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-120">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="b1d8c-121">Ana sorgu için bir sorgu</span><span class="sxs-lookup"><span data-stu-id="b1d8c-121">One query for the main query</span></span> 
  * <span data-ttu-id="b1d8c-122">Yükleme ağacındaki her koleksiyon "Edge" için bir sorgu.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-122">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="b1d8c-123">Sorguları ile `Load`ayır: Veriler ayrı sorgularda alınabilir ve gezinti özellikleri ' EF Core "düzeltir".</span><span class="sxs-lookup"><span data-stu-id="b1d8c-123">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="b1d8c-124">"Düzeltmeler", EF Core gezinti özelliklerini otomatik olarak dolduran anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-124">"Fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="b1d8c-125">İle `Load` ayrı sorgular, ek yükleme Eager yüklemeden daha benzer.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-125">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![Ayrı sorgular örneği](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="b1d8c-127">Not: EF Core, daha önce bağlam örneğine yüklenmiş olan diğer varlıklara gezinti özelliklerini otomatik olarak düzeltir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-127">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="b1d8c-128">Bir gezinti özelliği için veriler açıkça dahil edilmese bile, ilgili varlıkların bazıları veya tümü daha önce yüklenmişse Özellik yine de doldurulabilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-128">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="b1d8c-129">[Açık yükleme](/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="b1d8c-129">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="b1d8c-130">Varlık ilk kez okunmadıysa ilgili veriler alınmadı.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-130">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="b1d8c-131">Gerektiğinde ilgili verileri almak için kodun yazılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-131">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="b1d8c-132">Ayrı sorgularla açık yükleme, veritabanına birden çok sorgu gönderilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-132">Explicit loading with separate queries results in multiple queries sent to the database.</span></span> <span data-ttu-id="b1d8c-133">Açık yükleme ile kod, yüklenecek gezinti özelliklerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-133">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="b1d8c-134">Açık yükleme yapmak için yönteminikullanın.`Load`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-134">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="b1d8c-135">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-135">For example:</span></span>

  ![Açık yükleme örneği](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="b1d8c-137">[Yavaş yükleme](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="b1d8c-137">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="b1d8c-138">[Sürüm 2,1 ' de EF Core geç yükleme eklendi](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="b1d8c-138">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="b1d8c-139">Varlık ilk kez okunmadıysa ilgili veriler alınmadı.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-139">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="b1d8c-140">Gezinti özelliğine ilk kez erişildiğinde, bu gezinti özelliği için gereken veriler otomatik olarak alınır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-140">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="b1d8c-141">Bir gezinti özelliğine ilk kez erişildiğinde bir sorgu veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-141">A query is sent to the database each time a navigation property is accessed for the first time.</span></span>

## <a name="create-course-pages"></a><span data-ttu-id="b1d8c-142">Kurs sayfaları oluşturma</span><span class="sxs-lookup"><span data-stu-id="b1d8c-142">Create Course pages</span></span>

<span data-ttu-id="b1d8c-143">Varlık, ilgili `Department` varlığı içeren bir gezinti özelliği içerir. `Course`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-143">The `Course` entity includes a navigation property that contains the related `Department` entity.</span></span>

![Kurs. Departmanı](read-related-data/_static/dep-crs.png)

<span data-ttu-id="b1d8c-145">Bir kurs için atanan departmanın adını göstermek için:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-145">To display the name of the assigned department for a course:</span></span>

* <span data-ttu-id="b1d8c-146">İlgili `Department` varlığı`Course.Department` gezinti özelliğine yükleyin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-146">Load the related `Department` entity into the `Course.Department` navigation property.</span></span>
* <span data-ttu-id="b1d8c-147">`Department` Varlığın özelliğindenadıalın.`Name`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-147">Get the name from the `Department` entity's `Name` property.</span></span>

<a name="scaffold"></a>

### <a name="scaffold-course-pages"></a><span data-ttu-id="b1d8c-148">Yapı iskelesi kurs sayfaları</span><span class="sxs-lookup"><span data-stu-id="b1d8c-148">Scaffold Course pages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b1d8c-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1d8c-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b1d8c-150">Aşağıdaki özel durumlarla birlikte [Yapı Fkatlama öğrenci sayfalarındaki](xref:data/ef-rp/intro#scaffold-student-pages) yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-150">Follow the instructions in [Scaffold Student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="b1d8c-151">Bir *Sayfalar/kurslar* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-151">Create a *Pages/Courses* folder.</span></span>
  * <span data-ttu-id="b1d8c-152">Model `Course` sınıfı için kullanın.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-152">Use `Course` for the model class.</span></span>
  * <span data-ttu-id="b1d8c-153">Yeni bir tane oluşturmak yerine var olan bağlam sınıfını kullanın.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-153">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b1d8c-154">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b1d8c-154">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b1d8c-155">Bir *Sayfalar/kurslar* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-155">Create a *Pages/Courses* folder.</span></span>

* <span data-ttu-id="b1d8c-156">Kurs sayfalarını iskele almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-156">Run the following command to scaffold the Course pages.</span></span>

  <span data-ttu-id="b1d8c-157">**Windows üzerinde:**</span><span class="sxs-lookup"><span data-stu-id="b1d8c-157">**On Windows:**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

  <span data-ttu-id="b1d8c-158">**Linux veya macOS 'ta:**</span><span class="sxs-lookup"><span data-stu-id="b1d8c-158">**On Linux or macOS:**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages/Courses --referenceScriptLibraries
  ```

---

* <span data-ttu-id="b1d8c-159">*Pages/kurslar/Index. cshtml. cs* dosyasını açın ve `OnGetAsync` metodunu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-159">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="b1d8c-160">Yapı iskelesi altyapısı, `Department` gezinti özelliği için belirtilen Eager 'yi belirtti.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-160">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="b1d8c-161">`Include` Yöntemi Eager yüklemeyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-161">The `Include` method specifies eager loading.</span></span>

* <span data-ttu-id="b1d8c-162">Uygulamayı çalıştırın ve **Kurslar** bağlantısını seçin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-162">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="b1d8c-163">Departman sütunu, yararlı olmayan `DepartmentID`öğesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-163">The department column displays the `DepartmentID`, which isn't useful.</span></span>

### <a name="display-the-department-name"></a><span data-ttu-id="b1d8c-164">Departmanın adını görüntüleme</span><span class="sxs-lookup"><span data-stu-id="b1d8c-164">Display the department name</span></span>

<span data-ttu-id="b1d8c-165">Pages/kurslar/Index. cshtml. cs dosyasını aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-165">Update Pages/Courses/Index.cshtml.cs with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Index.cshtml.cs?highlight=18,22,24)]

<span data-ttu-id="b1d8c-166">Önceki kod, `Course` özelliği olarak `Courses` değiştirir ve ekler `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-166">The preceding code changes the `Course` property to `Courses` and adds `AsNoTracking`.</span></span> <span data-ttu-id="b1d8c-167">`AsNoTracking`Döndürülen varlıklar izlenmediğinden performansı geliştirir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-167">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="b1d8c-168">Varlıkların geçerli bağlamda güncelleştirilmediği için izlenmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-168">The entities don't need to be tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="b1d8c-169">*Pages/kurslar/Index. cshtml* 'yi aşağıdaki kodla güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-169">Update *Pages/Courses/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Index.cshtml?highlight=5,8,16-18,20,23,26,32,35-37,45)]

<span data-ttu-id="b1d8c-170">Yapı iskelesi kodunda aşağıdaki değişiklikler yapılmıştır:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-170">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="b1d8c-171">Özellik adı olarak `Courses`değiştirildi. `Course`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-171">Changed the `Course` property name to `Courses`.</span></span>
* <span data-ttu-id="b1d8c-172">`CourseID` Özellik değerini gösteren bir **sayı** sütunu eklendi.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-172">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="b1d8c-173">Birincil anahtarlar, genellikle son kullanıcılara anlamlı olduklarından, varsayılan olarak yapı iskelesi göstermemektedir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-173">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="b1d8c-174">Ancak, bu durumda birincil anahtar anlamlı olur.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-174">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="b1d8c-175">Departman adını göstermek için **Departman** sütunu değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-175">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="b1d8c-176">Kod, `Department` gezinti özelliğine `Name` yüklenen `Department` varlığın özelliğini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-176">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="b1d8c-177">Uygulamayı çalıştırın ve bölüm adlarıyla listeyi görmek için **Kurslar** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-177">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kurslar Dizin sayfası](read-related-data/_static/courses-index30.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="b1d8c-179">Select ile ilgili verileri yükleme</span><span class="sxs-lookup"><span data-stu-id="b1d8c-179">Loading related data with Select</span></span>

<span data-ttu-id="b1d8c-180">`OnGetAsync` Yöntemi ,`Include` yöntemiyle ilgili verileri yükler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-180">The `OnGetAsync` method loads related data with the `Include` method.</span></span> <span data-ttu-id="b1d8c-181">`Select` Yöntemi, yalnızca gerekli ilgili verileri yükleyen bir alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-181">The `Select` method is an alternative that loads only the related data needed.</span></span> <span data-ttu-id="b1d8c-182">Tek öğeler için, `Department.Name` örneğin bir SQL iç birleşimi kullanır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-182">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="b1d8c-183">Koleksiyonlar için başka bir veritabanı erişimi kullanır, ancak koleksiyonlar üzerindeki `Include` işleci bu şekilde yapar.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-183">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="b1d8c-184">Aşağıdaki kod, `Select` yöntemiyle ilgili verileri yükler:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-184">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=6)]

<span data-ttu-id="b1d8c-185">`CourseViewModel`Şunları yapın:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-185">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="b1d8c-186">Tüm örnek için bkz. [ındexselect. cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) ve [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs) .</span><span class="sxs-lookup"><span data-stu-id="b1d8c-186">See [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-instructor-pages"></a><span data-ttu-id="b1d8c-187">Eğitmen sayfaları oluşturma</span><span class="sxs-lookup"><span data-stu-id="b1d8c-187">Create Instructor pages</span></span>

<span data-ttu-id="b1d8c-188">Bu bölüm, eğitmen sayfalarını derler ve ilgili Kurslar ve kayıtları eğitmenler dizin sayfasına ekler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-188">This section scaffolds Instructor pages and adds related Courses and Enrollments to the Instructors Index page.</span></span>

<a name="IP"></a>
<span data-ttu-id="b1d8c-189">![Eğitmenler Dizin sayfası](read-related-data/_static/instructors-index30.png)</span><span class="sxs-lookup"><span data-stu-id="b1d8c-189">![Instructors Index page](read-related-data/_static/instructors-index30.png)</span></span>

<span data-ttu-id="b1d8c-190">Bu sayfa aşağıdaki yollarla ilgili verileri okur ve görüntüler:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-190">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="b1d8c-191">Eğitmenler listesi, `OfficeAssignment` varlıktaki ilgili verileri (önceki görüntüde Office) görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-191">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="b1d8c-192">`Instructor` Ve`OfficeAssignment` varlıkları bire sıfır veya-bir ilişkidir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-192">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="b1d8c-193">`OfficeAssignment` Varlıklar için Eager yüklemesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-193">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="b1d8c-194">Eager yüklemesi, ilgili verilerin görüntülenmesi gerektiğinde genellikle daha etkilidir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-194">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="b1d8c-195">Bu durumda, Eğitmenler için Office atamaları görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-195">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="b1d8c-196">Kullanıcı bir eğitmen seçtiğinde ilgili `Course` varlıklar görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-196">When the user selects an instructor, related `Course` entities are displayed.</span></span> <span data-ttu-id="b1d8c-197">`Instructor` Ve`Course` varlıkları çoktan çoğa bir ilişkidir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-197">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="b1d8c-198">Eager yüklemesi, `Course` varlıklar ve ilgili `Department` varlıklar için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-198">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="b1d8c-199">Bu durumda, yalnızca seçili eğitmen için kurslar gerektiğinden ayrı sorgular daha verimli olabilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-199">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="b1d8c-200">Bu örnek, gezinti özelliklerinde olan varlıklarda gezinti özellikleri için Eager yükleme 'nin nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-200">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="b1d8c-201">Kullanıcı bir kurs seçtiğinde, `Enrollments` varlıktaki ilgili veriler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-201">When the user selects a course, related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="b1d8c-202">Önceki görüntüde öğrenci adı ve sınıf görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-202">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="b1d8c-203">`Course` Ve`Enrollment` varlıkları bire çok ilişkidir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-203">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model"></a><span data-ttu-id="b1d8c-204">Görünüm modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="b1d8c-204">Create a view model</span></span>

<span data-ttu-id="b1d8c-205">Eğitmenler sayfasında, üç farklı tablodan alınan veriler gösterilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-205">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="b1d8c-206">Üç tabloyu temsil eden üç özellik içeren bir görünüm modeli gerekir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-206">A view model is needed that includes three properties representing the three tables.</span></span>

<span data-ttu-id="b1d8c-207">Aşağıdaki kodla *SchoolViewModels/ıncpctorındexdata. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-207">Create *SchoolViewModels/InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-instructor-pages"></a><span data-ttu-id="b1d8c-208">Yapı iskelesi eğitmeni sayfaları</span><span class="sxs-lookup"><span data-stu-id="b1d8c-208">Scaffold Instructor pages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b1d8c-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1d8c-209">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b1d8c-210">Öğrenci sayfalarını aşağıdaki özel durumlarla birlikte [Scaffold](xref:data/ef-rp/intro#scaffold-student-pages) içindeki yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-210">Follow the instructions in [Scaffold the student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="b1d8c-211">Bir *sayfa/eğitmenler* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-211">Create a *Pages/Instructors* folder.</span></span>
  * <span data-ttu-id="b1d8c-212">Model `Instructor` sınıfı için kullanın.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-212">Use `Instructor` for the model class.</span></span>
  * <span data-ttu-id="b1d8c-213">Yeni bir tane oluşturmak yerine var olan bağlam sınıfını kullanın.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-213">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b1d8c-214">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b1d8c-214">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b1d8c-215">Bir *sayfa/eğitmenler* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-215">Create a *Pages/Instructors* folder.</span></span>

* <span data-ttu-id="b1d8c-216">Eğitmen sayfalarını iskele almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-216">Run the following command to scaffold the Instructor pages.</span></span>

  <span data-ttu-id="b1d8c-217">**Windows üzerinde:**</span><span class="sxs-lookup"><span data-stu-id="b1d8c-217">**On Windows:**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

  <span data-ttu-id="b1d8c-218">**Linux veya macOS 'ta:**</span><span class="sxs-lookup"><span data-stu-id="b1d8c-218">**On Linux or macOS:**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages/Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="b1d8c-219">Yapı iskelesi sayfasının güncelleştirmeden önce nasıl göründüğünü görmek için, uygulamayı çalıştırın ve eğitmenler sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-219">To see what the scaffolded page looks like before you update it, run the app and navigate to the Instructors page.</span></span>

<span data-ttu-id="b1d8c-220">*Pages/eğitmenler/Index. cshtml. cs* dosyasını aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-220">Update *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,19-53)]

<span data-ttu-id="b1d8c-221">`OnGetAsync` Yöntemi, seçilen eğitmenin kimliği için isteğe bağlı rota verilerini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-221">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="b1d8c-222">*Pages/eğitmenler/Index. cshtml. cs* dosyasındaki sorguyu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-222">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading)]

<span data-ttu-id="b1d8c-223">Kod, aşağıdaki gezinti özellikleri için Eager yüklemeyi belirtir:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-223">The code specifies eager loading for the following navigation properties:</span></span>

* `Instructor.OfficeAssignment`
* `Instructor.CourseAssignments`
  * `CourseAssignments.Course`
    * `Course.Department`
    * `Course.Enrollments`
      * `Enrollment.Student`

<span data-ttu-id="b1d8c-224">`Include` Ve için `ThenInclude` ve`Course`yöntemlerininyinelendiğine dikkat edin. `CourseAssignments`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-224">Notice the repetition of `Include` and `ThenInclude` methods for `CourseAssignments` and `Course`.</span></span> <span data-ttu-id="b1d8c-225">Bu yineleme, `Course` varlığın iki gezinti özelliği için Eager yüklemesi belirtmek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-225">This repetition is necessary to specify eager loading for two navigation properties of the `Course` entity.</span></span>

<span data-ttu-id="b1d8c-226">Aşağıdaki kod, bir eğitmen seçildiğinde (`id != null`) yürütülür.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-226">The following code executes when an instructor is selected (`id != null`).</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectInstructor)]

<span data-ttu-id="b1d8c-227">Seçilen eğitmen, görünüm modelindeki eğitmenler listesinden alınır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-227">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="b1d8c-228">Görünüm modelinin `Courses` özelliği, bu eğitmenin `CourseAssignments` gezinti özelliğinden alınan `Course` varlıklarla birlikte yüklenir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-228">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="b1d8c-229">`Where` Yöntemi bir koleksiyon döndürür.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-229">The `Where` method returns a collection.</span></span> <span data-ttu-id="b1d8c-230">Ancak bu durumda filtre tek bir varlık seçer.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-230">But in this case, the filter will select a single entity.</span></span> <span data-ttu-id="b1d8c-231">Bu nedenle `Single` , yöntemi koleksiyonu tek `Instructor` bir varlığa dönüştürmek için çağırılır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-231">so the `Single` method is called to convert the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="b1d8c-232">Varlık, `CourseAssignments` özelliğine erişim sağlar. `Instructor`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-232">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="b1d8c-233">`CourseAssignments`ilgili `Course` varlıklara erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-233">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Eğitmenden kurslar M:d](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="b1d8c-235">Koleksiyonda yalnızca bir öğe olduğunda Yöntembirkoleksiyondakullanılır.`Single`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-235">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="b1d8c-236">Koleksiyon boşsa veya birden fazla öğe varsa Yöntembirözeldurumoluşturur.`Single`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-236">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="b1d8c-237">Koleksiyon boşsa varsayılan `SingleOrDefault`bir değer (Bu durumda null) döndüren alternatif bir alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-237">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span>

<span data-ttu-id="b1d8c-238">Aşağıdaki kod, bir kurs seçildiğinde görünüm modelinin `Enrollments` özelliğini doldurur:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-238">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectCourse)]

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="b1d8c-239">Eğitmenler Dizin sayfasını Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="b1d8c-239">Update the instructors Index page</span></span>

<span data-ttu-id="b1d8c-240">*Pages/eğitmenler/Index. cshtml* dosyasını aşağıdaki kodla güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-240">Update *Pages/Instructors/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Index.cshtml?highlight=1,5,8,16-21,25-32,43-57,67-102,104-126)]

<span data-ttu-id="b1d8c-241">Yukarıdaki kod aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-241">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="b1d8c-242">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-242">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="b1d8c-243">`"{id:int?}"`bir yol şablonudur.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-243">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="b1d8c-244">Yol şablonu, verileri yönlendirmek için URL 'deki tamsayı Sorgu dizelerini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-244">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="b1d8c-245">Örneğin, yalnızca `@page` yönergeyle bir eğitmenin **Select** bağlantısına tıklanması aşağıdakine benzer bir URL oluşturur:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-245">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `https://localhost:5001/Instructors?id=2`

  <span data-ttu-id="b1d8c-246">Sayfa yönergesi `@page "{id:int?}"`olduğunda URL şu şekilde olur:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-246">When the page directive is `@page "{id:int?}"`, the URL is:</span></span>

  `https://localhost:5001/Instructors/2`

* <span data-ttu-id="b1d8c-247">Yalnızca `item.OfficeAssignment.Location` nulldeğilsegörüntülenenbirOfficesütunu`item.OfficeAssignment` ekler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-247">Adds an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="b1d8c-248">Bu bire sıfır veya-bir ilişki olduğundan ilgili bir OfficeAssignment varlığı bulunmayabilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-248">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="b1d8c-249">Her bir eğitmen tarafından taders kurslarını görüntüleyen bir **Kurslar** sütunu ekler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-249">Adds a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="b1d8c-250">Bu Razor sözdizimi hakkında daha fazla bilgi için bkz. [açık hat geçişi `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) .</span><span class="sxs-lookup"><span data-stu-id="b1d8c-250">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="b1d8c-251">Seçilen eğitmenin ve kursun `class="success"` `tr` öğesine dinamik olarak ekleyen kodu ekler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-251">Adds code that dynamically adds `class="success"` to the `tr` element of the selected instructor and course.</span></span> <span data-ttu-id="b1d8c-252">Bu, bir önyükleme sınıfı kullanarak seçili satır için bir arka plan rengi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-252">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="b1d8c-253">**Select**etiketli yeni bir köprü ekler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-253">Adds a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="b1d8c-254">Bu bağlantı, seçilen eğitmenin kimliğini `Index` yönteme gönderir ve bir arka plan rengi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-254">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

* <span data-ttu-id="b1d8c-255">Seçili eğitmen için bir kurs tablosu ekler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-255">Adds a table of courses for the selected Instructor.</span></span>

* <span data-ttu-id="b1d8c-256">Seçili kurs için bir öğrenci kayıtları tablosu ekler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-256">Adds a table of student enrollments for the selected course.</span></span>

<span data-ttu-id="b1d8c-257">Uygulamayı çalıştırın ve **eğitmenler** sekmesini seçin. Sayfa, `Location` ilgili `OfficeAssignment` varlıktaki (Office) öğesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-257">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="b1d8c-258">`OfficeAssignment` Null ise boş bir tablo hücresi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-258">If `OfficeAssignment` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="b1d8c-259">Bir eğitmenin **Seç** bağlantısına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-259">Click on the **Select** link for an instructor.</span></span> <span data-ttu-id="b1d8c-260">Bu eğitmenin atandığı satır stili değişiklikleri ve kurslar görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-260">The row style changes and courses assigned to that instructor are displayed.</span></span>

<span data-ttu-id="b1d8c-261">Kayıtlı öğrenciler ve bunların onların listesini görmek için bir kurs seçin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-261">Select a course to see the list of enrolled students and their grades.</span></span>

![Eğitmenler Dizin sayfası eğitmeni ve kursu seçildi](read-related-data/_static/instructors-index30.png)

## <a name="using-single"></a><span data-ttu-id="b1d8c-263">Tek kullanımı</span><span class="sxs-lookup"><span data-stu-id="b1d8c-263">Using Single</span></span>

<span data-ttu-id="b1d8c-264">Yöntemi yöntemi ayrı çağırmak yerine `Where` koşulu geçirebilir: `Where` `Single`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-264">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="b1d8c-265">Bir Where koşulunun kullanımı, kişisel tercihdenbağımsızolarakkullanılır.`Single`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-265">Use of `Single` with a Where condition is a matter of personal preference.</span></span> <span data-ttu-id="b1d8c-266">`Where` Yöntemi kullanılarak herhangi bir avantaj sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-266">It provides no benefits over using the `Where` method.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="b1d8c-267">Açık yükleme</span><span class="sxs-lookup"><span data-stu-id="b1d8c-267">Explicit loading</span></span>

<span data-ttu-id="b1d8c-268">Geçerli kod ve `Enrollments` `Students`için Eager yüklemeyi belirtir:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-268">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading&highlight=6-9)]

<span data-ttu-id="b1d8c-269">Kullanıcıların bir kursa kayıtları nadiren görmek istediğini varsayalım.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-269">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="b1d8c-270">Bu durumda, bir iyileştirme yalnızca isteniyorsa kayıt verilerini yüklemek olacaktır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-270">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="b1d8c-271">Bu bölümde `OnGetAsync` , ve `Students`' nin `Enrollments` açık yüklemesini kullanacak şekilde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-271">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="b1d8c-272">*Pages/eğitmenler/Index. cshtml. cs* dosyasını aşağıdaki kodla güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-272">Update *Pages/Instructors/Index.cshtml.cs* with the following code.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Index.cshtml.cs?highlight=31-35,52-56)]

<span data-ttu-id="b1d8c-273">Yukarıdaki kod, kayıt ve öğrenci verileri için *Thenınclude* Yöntem çağrılarını bırakır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-273">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="b1d8c-274">Bir kurs seçilirse, açık yükleme kodu alır:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-274">If a course is selected, the explicit loading code retrieves:</span></span>

* <span data-ttu-id="b1d8c-275">Seçili kurs için varlıklar. `Enrollment`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-275">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="b1d8c-276">Her biri`Enrollment`için varlıklar. `Student`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-276">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="b1d8c-277">Yukarıdaki kodun yorumdığına `.AsNoTracking()`dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-277">Notice that the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="b1d8c-278">Gezinti özellikleri yalnızca izlenen varlıklar için açık bir şekilde yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-278">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="b1d8c-279">Uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-279">Test the app.</span></span> <span data-ttu-id="b1d8c-280">Bir kullanıcının perspektifinden, uygulama önceki sürümle aynı şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-280">From a user's perspective, the app behaves identically to the previous version.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b1d8c-281">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="b1d8c-281">Next steps</span></span>

<span data-ttu-id="b1d8c-282">Sonraki öğreticide ilgili verileri güncelleştirme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-282">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="b1d8c-283">[Önceki öğretici](xref:data/ef-rp/complex-data-model)
>[sonraki öğretici](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="b1d8c-283">[Previous tutorial](xref:data/ef-rp/complex-data-model)
[Next tutorial](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b1d8c-284">Bu öğreticide ilgili veriler okundu ve görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-284">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="b1d8c-285">İlgili veriler, EF Core gezinti özelliklerine yüklediği veri.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-285">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="b1d8c-286">Olamaz çözmenize, sorunlarla karşılaşırsanız, [indirin veya tamamlanmış uygulamayı görüntüleyin.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="b1d8c-286">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="b1d8c-287">[Yükleme yönergeleri](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="b1d8c-287">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="b1d8c-288">Aşağıdaki çizimler, Bu öğreticinin tamamlanan sayfalarını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-288">The following illustrations show the completed pages for this tutorial:</span></span>

![Kurslar Dizin sayfası](read-related-data/_static/courses-index.png)

![Eğitmenler Dizin sayfası](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="b1d8c-291">İlgili verilerin Eager, açık ve geç yüklemesi</span><span class="sxs-lookup"><span data-stu-id="b1d8c-291">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="b1d8c-292">EF Core bir varlığın gezinti özelliklerine ilgili verileri yükleyebilmenin birkaç yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-292">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="b1d8c-293">[Eager yükleniyor](/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="b1d8c-293">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="b1d8c-294">Ekip yükleme, bir varlık türü için bir sorgu aynı zamanda ilgili varlıkları de yüklediğinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-294">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="b1d8c-295">Varlık okunmadığınızda ilgili veriler alınır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-295">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="b1d8c-296">Bu, genellikle gereken tüm verileri alan tek bir JOIN sorgusuna neden olur.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-296">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="b1d8c-297">EF Core, bazı Eager yükleme türleri için birden çok sorgu yayımlayacak.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-297">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="b1d8c-298">Birden çok sorgu verilmesi, EF6 içindeki bazı sorgular için tek bir sorgunun bulunduğu durumda daha verimli olabilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-298">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="b1d8c-299">Eager yüklemesi `Include` ve `ThenInclude` yöntemleriyle birlikte belirtilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-299">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Eager yükleme örneği](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="b1d8c-301">Bir koleksiyon gezintisi eklendiğinde Eager yüklemesi birden çok sorgu gönderir:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-301">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="b1d8c-302">Ana sorgu için bir sorgu</span><span class="sxs-lookup"><span data-stu-id="b1d8c-302">One query for the main query</span></span> 
  * <span data-ttu-id="b1d8c-303">Yükleme ağacındaki her koleksiyon "Edge" için bir sorgu.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-303">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="b1d8c-304">Sorguları ile `Load`ayır: Veriler ayrı sorgularda alınabilir ve gezinti özellikleri ' EF Core "düzeltir".</span><span class="sxs-lookup"><span data-stu-id="b1d8c-304">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="b1d8c-305">"düzeltmeler", EF Core gezinti özelliklerini otomatik olarak dolduran anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-305">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="b1d8c-306">İle `Load` ayrı sorgular, ek yükleme Eager yüklemeden daha benzer.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-306">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![Ayrı sorgular örneği](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="b1d8c-308">Not: EF Core, daha önce bağlam örneğine yüklenmiş olan diğer varlıklara gezinti özelliklerini otomatik olarak düzeltir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-308">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="b1d8c-309">Bir gezinti özelliği için veriler açıkça dahil edilmese bile, ilgili varlıkların bazıları veya tümü daha önce yüklenmişse Özellik yine de doldurulabilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-309">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="b1d8c-310">[Açık yükleme](/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="b1d8c-310">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="b1d8c-311">Varlık ilk kez okunmadıysa ilgili veriler alınmadı.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-311">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="b1d8c-312">Gerektiğinde ilgili verileri almak için kodun yazılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-312">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="b1d8c-313">Ayrı sorgularla açık yükleme, VERITABANıNA birden çok sorgu gönderilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-313">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="b1d8c-314">Açık yükleme ile kod, yüklenecek gezinti özelliklerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-314">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="b1d8c-315">Açık yükleme yapmak için yönteminikullanın.`Load`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-315">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="b1d8c-316">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-316">For example:</span></span>

  ![Açık yükleme örneği](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="b1d8c-318">[Yavaş yükleme](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="b1d8c-318">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="b1d8c-319">[Sürüm 2,1 ' de EF Core geç yükleme eklendi](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="b1d8c-319">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="b1d8c-320">Varlık ilk kez okunmadıysa ilgili veriler alınmadı.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-320">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="b1d8c-321">Gezinti özelliğine ilk kez erişildiğinde, bu gezinti özelliği için gereken veriler otomatik olarak alınır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-321">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="b1d8c-322">Bir gezinti özelliğine ilk kez erişildiğinde bir sorgu VERITABANıNA gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-322">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="b1d8c-323">`Select` İşleci yalnızca gerekli ilgili verileri yükler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-323">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-course-page-that-displays-department-name"></a><span data-ttu-id="b1d8c-324">Bölüm adını görüntüleyen bir kurs sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="b1d8c-324">Create a Course page that displays department name</span></span>

<span data-ttu-id="b1d8c-325">Kurs varlığı, `Department` varlığı içeren bir gezinti özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-325">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="b1d8c-326">`Department` Varlık, kursun atandığı departmanı içerir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-326">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="b1d8c-327">Bir kurs listesinde atanan departmanın adını göstermek için:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-327">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="b1d8c-328">`Department` Varlıktaki `Name` özelliği alın.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-328">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="b1d8c-329">Varlık, `Course.Department` gezinti özelliğinden gelir. `Department`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-329">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Kurs. Departmanı](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>

### <a name="scaffold-the-course-model"></a><span data-ttu-id="b1d8c-331">Kurs modelini temklesi</span><span class="sxs-lookup"><span data-stu-id="b1d8c-331">Scaffold the Course model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b1d8c-332">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1d8c-332">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="b1d8c-333">Bölümündeki yönergeleri [Öğrenci modeli iskelesini](xref:data/ef-rp/intro#scaffold-student-pages) ve `Course` model sınıfı için.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-333">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-student-pages) and use `Course` for the model class.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b1d8c-334">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b1d8c-334">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="b1d8c-335">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-335">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

---

<span data-ttu-id="b1d8c-336">Önceki komut iskelesini kurar `Course` modeli.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-336">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="b1d8c-337">Projeyi Visual Studio'da açın.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-337">Open the project in Visual Studio.</span></span>

<span data-ttu-id="b1d8c-338">*Pages/kurslar/Index. cshtml. cs* dosyasını açın ve `OnGetAsync` metodunu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-338">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="b1d8c-339">Yapı iskelesi altyapısı, `Department` gezinti özelliği için belirtilen Eager 'yi belirtti.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-339">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="b1d8c-340">`Include` Yöntemi Eager yüklemeyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-340">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="b1d8c-341">Uygulamayı çalıştırın ve **Kurslar** bağlantısını seçin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-341">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="b1d8c-342">Departman sütunu, yararlı olmayan `DepartmentID`öğesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-342">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="b1d8c-343">`OnGetAsync` Yöntemi aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-343">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="b1d8c-344">Yukarıdaki kod ekler `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-344">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="b1d8c-345">`AsNoTracking`Döndürülen varlıklar izlenmediğinden performansı geliştirir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-345">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="b1d8c-346">Geçerli bağlamda güncelleştirilmemiş oldukları için varlıklar izlenmiyor.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-346">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="b1d8c-347">*Pages/kurslar/Index. cshtml* 'yi aşağıdaki vurgulanmış işaretlerle güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-347">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="b1d8c-348">Yapı iskelesi kodunda aşağıdaki değişiklikler yapılmıştır:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-348">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="b1d8c-349">Başlık dizinden kurslar olarak değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-349">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="b1d8c-350">`CourseID` Özellik değerini gösteren bir **sayı** sütunu eklendi.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-350">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="b1d8c-351">Birincil anahtarlar, genellikle son kullanıcılara anlamlı olduklarından, varsayılan olarak yapı iskelesi göstermemektedir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-351">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="b1d8c-352">Ancak, bu durumda birincil anahtar anlamlı olur.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-352">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="b1d8c-353">Departman adını göstermek için **Departman** sütunu değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-353">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="b1d8c-354">Kod, `Department` gezinti özelliğine `Name` yüklenen `Department` varlığın özelliğini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-354">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="b1d8c-355">Uygulamayı çalıştırın ve bölüm adlarıyla listeyi görmek için **Kurslar** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-355">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kurslar Dizin sayfası](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="b1d8c-357">Select ile ilgili verileri yükleme</span><span class="sxs-lookup"><span data-stu-id="b1d8c-357">Loading related data with Select</span></span>

<span data-ttu-id="b1d8c-358">`OnGetAsync` Yöntemi ,`Include` yöntemiyle ilgili verileri yükler:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-358">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="b1d8c-359">`Select` İşleci yalnızca gerekli ilgili verileri yükler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-359">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="b1d8c-360">Tek öğeler için, `Department.Name` örneğin bir SQL iç birleşimi kullanır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-360">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="b1d8c-361">Koleksiyonlar için başka bir veritabanı erişimi kullanır, ancak koleksiyonlar üzerindeki `Include` işleci bu şekilde yapar.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-361">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="b1d8c-362">Aşağıdaki kod, `Select` yöntemiyle ilgili verileri yükler:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-362">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="b1d8c-363">`CourseViewModel`Şunları yapın:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-363">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="b1d8c-364">Tüm örnek için bkz. [ındexselect. cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) ve [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) .</span><span class="sxs-lookup"><span data-stu-id="b1d8c-364">See [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="b1d8c-365">Kurslar ve kayıtları gösteren bir eğitmenler sayfası oluşturun</span><span class="sxs-lookup"><span data-stu-id="b1d8c-365">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="b1d8c-366">Bu bölümde, Eğitmenler sayfası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-366">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="b1d8c-367">![Eğitmenler Dizin sayfası](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="b1d8c-367">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="b1d8c-368">Bu sayfa aşağıdaki yollarla ilgili verileri okur ve görüntüler:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-368">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="b1d8c-369">Eğitmenler listesi, `OfficeAssignment` varlıktaki ilgili verileri (önceki görüntüde Office) görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-369">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="b1d8c-370">`Instructor` Ve`OfficeAssignment` varlıkları bire sıfır veya-bir ilişkidir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-370">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="b1d8c-371">`OfficeAssignment` Varlıklar için Eager yüklemesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-371">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="b1d8c-372">Eager yüklemesi, ilgili verilerin görüntülenmesi gerektiğinde genellikle daha etkilidir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-372">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="b1d8c-373">Bu durumda, Eğitmenler için Office atamaları görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-373">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="b1d8c-374">Kullanıcı bir eğitmen (önceki görüntüde haruı) seçtiğinde ilgili `Course` varlıklar görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-374">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="b1d8c-375">`Instructor` Ve`Course` varlıkları çoktan çoğa bir ilişkidir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-375">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="b1d8c-376">Eager yüklemesi, `Course` varlıklar ve ilgili `Department` varlıklar için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-376">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="b1d8c-377">Bu durumda, yalnızca seçili eğitmen için kurslar gerektiğinden ayrı sorgular daha verimli olabilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-377">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="b1d8c-378">Bu örnek, gezinti özelliklerinde olan varlıklarda gezinti özellikleri için Eager yükleme 'nin nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-378">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="b1d8c-379">Kullanıcı bir kurs seçtiğinde (önceki görüntüde Chemistry), `Enrollments` varlıktaki ilgili veriler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-379">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="b1d8c-380">Önceki görüntüde öğrenci adı ve sınıf görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-380">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="b1d8c-381">`Course` Ve`Enrollment` varlıkları bire çok ilişkidir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-381">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="b1d8c-382">Eğitmen dizini görünümü için bir görünüm modeli oluşturun</span><span class="sxs-lookup"><span data-stu-id="b1d8c-382">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="b1d8c-383">Eğitmenler sayfasında, üç farklı tablodan alınan veriler gösterilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-383">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="b1d8c-384">Üç tabloyu temsil eden üç varlığı içeren bir görünüm modeli oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-384">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="b1d8c-385">*SchoolViewModels* klasöründe, aşağıdaki kodla *InstructorIndexData.cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-385">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="b1d8c-386">Eğitmen modelini scafkatlama</span><span class="sxs-lookup"><span data-stu-id="b1d8c-386">Scaffold the Instructor model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b1d8c-387">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1d8c-387">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="b1d8c-388">Bölümündeki yönergeleri [Öğrenci modeli iskelesini](xref:data/ef-rp/intro#scaffold-student-pages) ve `Instructor` model sınıfı için.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-388">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-student-pages) and use `Instructor` for the model class.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b1d8c-389">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b1d8c-389">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="b1d8c-390">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-390">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="b1d8c-391">Önceki komut iskelesini kurar `Instructor` modeli.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-391">The preceding command scaffolds the `Instructor` model.</span></span> 
<span data-ttu-id="b1d8c-392">Uygulamayı çalıştırın ve eğitmenler sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-392">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="b1d8c-393">*Pages/eğitmenler/Index. cshtml. cs* öğesini şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-393">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="b1d8c-394">`OnGetAsync` Yöntemi, seçilen eğitmenin kimliği için isteğe bağlı rota verilerini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-394">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="b1d8c-395">*Pages/eğitmenler/Index. cshtml. cs* dosyasındaki sorguyu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-395">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="b1d8c-396">Sorgunun iki içerme vardır:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-396">The query has two includes:</span></span>

* <span data-ttu-id="b1d8c-397">`OfficeAssignment`: [Eğitmenler görünümünde](#IP)görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-397">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="b1d8c-398">`CourseAssignments`: Bu, kurs dünyasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-398">`CourseAssignments`: Which brings in the courses taught.</span></span>

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="b1d8c-399">Eğitmenler Dizin sayfasını Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="b1d8c-399">Update the instructors Index page</span></span>

<span data-ttu-id="b1d8c-400">*Sayfaları/eğitmenler/Index. cshtml* 'yi şu biçimlendirmeyle güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-400">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="b1d8c-401">Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-401">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="b1d8c-402">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-402">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="b1d8c-403">`"{id:int?}"`bir yol şablonudur.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-403">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="b1d8c-404">Yol şablonu, verileri yönlendirmek için URL 'deki tamsayı Sorgu dizelerini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-404">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="b1d8c-405">Örneğin, yalnızca `@page` yönergeyle bir eğitmenin **Select** bağlantısına tıklanması aşağıdakine benzer bir URL oluşturur:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-405">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `http://localhost:1234/Instructors?id=2`

  <span data-ttu-id="b1d8c-406">Sayfa yönergesi `@page "{id:int?}"`olduğunda, önceki URL şu şekilde olur:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-406">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

  `http://localhost:1234/Instructors/2`

* <span data-ttu-id="b1d8c-407">Sayfa başlığı **eğitmenler**' dir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-407">Page title is **Instructors**.</span></span>
* <span data-ttu-id="b1d8c-408">Yalnızca null olmaması halinde `item.OfficeAssignment` görüntülenen bir **Office** sütunu eklendi. `item.OfficeAssignment.Location`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-408">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="b1d8c-409">Bu bire sıfır veya-bir ilişki olduğundan ilgili bir OfficeAssignment varlığı bulunmayabilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-409">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="b1d8c-410">Her bir eğitmen tarafından taders kurslarını görüntüleyen bir **Kurslar** sütunu eklendi.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-410">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="b1d8c-411">Bu Razor sözdizimi hakkında daha fazla bilgi için bkz. [açık hat geçişi `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) .</span><span class="sxs-lookup"><span data-stu-id="b1d8c-411">See [Explicit line transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="b1d8c-412">Seçilen eğitmenin `tr` öğesine dinamik olarak `class="success"` ekleyen kod eklendi.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-412">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="b1d8c-413">Bu, bir önyükleme sınıfı kullanarak seçili satır için bir arka plan rengi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-413">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="b1d8c-414">**Select**etiketli yeni bir köprü eklendi.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-414">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="b1d8c-415">Bu bağlantı, seçilen eğitmenin kimliğini `Index` yönteme gönderir ve bir arka plan rengi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-415">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="b1d8c-416">Uygulamayı çalıştırın ve **eğitmenler** sekmesini seçin. Sayfa, `Location` ilgili `OfficeAssignment` varlıktaki (Office) öğesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-416">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="b1d8c-417">Eğer OfficeAssignment ' null ise boş bir tablo hücresi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-417">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="b1d8c-418">**Seç** bağlantısına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-418">Click on the **Select** link.</span></span> <span data-ttu-id="b1d8c-419">Satır stili değişir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-419">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="b1d8c-420">Seçili eğitmen 'e göre kurslar ekleyin</span><span class="sxs-lookup"><span data-stu-id="b1d8c-420">Add courses taught by selected instructor</span></span>

<span data-ttu-id="b1d8c-421">*Pages/eğitmenler/Index. cshtml. cs* dosyasındaki yöntemiaşağıdakikodlagüncelleştirin:`OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-421">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="b1d8c-422">Ekleyemiyorum`public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-422">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="b1d8c-423">Güncelleştirilmiş sorguyu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-423">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="b1d8c-424">Önceki sorgu `Department` varlıkları ekler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-424">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="b1d8c-425">Aşağıdaki kod, bir eğitmen seçildiğinde (`id != null`) yürütülür.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-425">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="b1d8c-426">Seçilen eğitmen, görünüm modelindeki eğitmenler listesinden alınır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-426">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="b1d8c-427">Görünüm modelinin `Courses` özelliği, bu eğitmenin `CourseAssignments` gezinti özelliğinden alınan `Course` varlıklarla birlikte yüklenir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-427">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="b1d8c-428">`Where` Yöntemi bir koleksiyon döndürür.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-428">The `Where` method returns a collection.</span></span> <span data-ttu-id="b1d8c-429">Önceki `Where` yöntemde yalnızca tek `Instructor` bir varlık döndürülür.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-429">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="b1d8c-430">Yöntemi, koleksiyonu tek `Instructor` bir varlığa dönüştürür. `Single`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-430">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="b1d8c-431">Varlık, `CourseAssignments` özelliğine erişim sağlar. `Instructor`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-431">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="b1d8c-432">`CourseAssignments`ilgili `Course` varlıklara erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-432">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Eğitmenden kurslar M:d](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="b1d8c-434">Koleksiyonda yalnızca bir öğe olduğunda Yöntembirkoleksiyondakullanılır.`Single`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-434">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="b1d8c-435">Koleksiyon boşsa veya birden fazla öğe varsa Yöntembirözeldurumoluşturur.`Single`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-435">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="b1d8c-436">Koleksiyon boşsa varsayılan `SingleOrDefault`bir değer (Bu durumda null) döndüren alternatif bir alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-436">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="b1d8c-437">Boş `SingleOrDefault` bir koleksiyonda kullanma:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-437">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="b1d8c-438">Bir özel durum sonucu oluşur (null başvuru üzerinde bir `Courses` Özellik bulmaya çalışırken).</span><span class="sxs-lookup"><span data-stu-id="b1d8c-438">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="b1d8c-439">Özel durum iletisi sorunun nedenini daha az gösterir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-439">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="b1d8c-440">Aşağıdaki kod, bir kurs seçildiğinde görünüm modelinin `Enrollments` özelliğini doldurur:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-440">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="b1d8c-441">*Pages/eğitmenler/Index. cshtml* Razor sayfasının sonuna aşağıdaki biçimlendirmeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-441">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="b1d8c-442">Yukarıdaki biçimlendirme, bir eğitmen seçildiğinde bir eğitmenin ilgili kursların bir listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-442">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="b1d8c-443">Uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-443">Test the app.</span></span> <span data-ttu-id="b1d8c-444">Eğitmenler sayfasında bir **seçme** bağlantısına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-444">Click on a **Select** link on the instructors page.</span></span>

### <a name="show-student-data"></a><span data-ttu-id="b1d8c-445">Öğrenci verilerini göster</span><span class="sxs-lookup"><span data-stu-id="b1d8c-445">Show student data</span></span>

<span data-ttu-id="b1d8c-446">Bu bölümde, uygulama seçili bir kurs için öğrenci verilerini gösterecek şekilde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-446">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="b1d8c-447">`OnGetAsync` *Pages/eğitmenler/Index. cshtml. cs* içindeki yöntemdeki sorguyu aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-447">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="b1d8c-448">Güncelleştirme *sayfaları/eğitmenler/Index. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-448">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="b1d8c-449">Aşağıdaki biçimlendirmeyi dosyanın sonuna ekleyin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-449">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="b1d8c-450">Yukarıdaki biçimlendirme, seçili kursa kayıtlı öğrencilerin bir listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-450">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="b1d8c-451">Sayfayı yenileyin ve bir eğitmen seçin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-451">Refresh the page and select an instructor.</span></span> <span data-ttu-id="b1d8c-452">Kayıtlı öğrenciler ve bunların onların listesini görmek için bir kurs seçin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-452">Select a course to see the list of enrolled students and their grades.</span></span>

![Eğitmenler Dizin sayfası eğitmeni ve kursu seçildi](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="b1d8c-454">Tek kullanımı</span><span class="sxs-lookup"><span data-stu-id="b1d8c-454">Using Single</span></span>

<span data-ttu-id="b1d8c-455">Yöntemi yöntemi ayrı çağırmak yerine `Where` koşulu geçirebilir: `Where` `Single`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-455">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="b1d8c-456">Yukarıdaki `Single` yaklaşım, kullanarak `Where`herhangi bir avantaj sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-456">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="b1d8c-457">Bazı geliştiriciler `Single` yaklaşım stilini tercih eder.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-457">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="b1d8c-458">Açık yükleme</span><span class="sxs-lookup"><span data-stu-id="b1d8c-458">Explicit loading</span></span>

<span data-ttu-id="b1d8c-459">Geçerli kod ve `Enrollments` `Students`için Eager yüklemeyi belirtir:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-459">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="b1d8c-460">Kullanıcıların bir kursa kayıtları nadiren görmek istediğini varsayalım.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-460">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="b1d8c-461">Bu durumda, bir iyileştirme yalnızca isteniyorsa kayıt verilerini yüklemek olacaktır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-461">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="b1d8c-462">Bu bölümde `OnGetAsync` , ve `Students`' nin `Enrollments` açık yüklemesini kullanacak şekilde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-462">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="b1d8c-463">`OnGetAsync` Aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-463">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="b1d8c-464">Yukarıdaki kod, kayıt ve öğrenci verileri için *Thenınclude* Yöntem çağrılarını bırakır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-464">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="b1d8c-465">Bir kurs seçilirse vurgulanan kod şunu alır:</span><span class="sxs-lookup"><span data-stu-id="b1d8c-465">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="b1d8c-466">Seçili kurs için varlıklar. `Enrollment`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-466">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="b1d8c-467">Her biri`Enrollment`için varlıklar. `Student`</span><span class="sxs-lookup"><span data-stu-id="b1d8c-467">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="b1d8c-468">Yukarıdaki kod açıklamalarının `.AsNoTracking()`olduğunu fark edin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-468">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="b1d8c-469">Gezinti özellikleri yalnızca izlenen varlıklar için açık bir şekilde yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-469">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="b1d8c-470">Uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-470">Test the app.</span></span> <span data-ttu-id="b1d8c-471">Bir kullanıcının perspektifinden, uygulama önceki sürümle aynı şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-471">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="b1d8c-472">Sonraki öğreticide ilgili verileri güncelleştirme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="b1d8c-472">The next tutorial shows how to update related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1d8c-473">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b1d8c-473">Additional resources</span></span>

* [<span data-ttu-id="b1d8c-474">Bu öğreticinin YouTube sürümü (part1)</span><span class="sxs-lookup"><span data-stu-id="b1d8c-474">YouTube version of this tutorial (part1)</span></span>](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [<span data-ttu-id="b1d8c-475">Bu öğreticinin YouTube sürümü (part2)</span><span class="sxs-lookup"><span data-stu-id="b1d8c-475">YouTube version of this tutorial (part2)</span></span>](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
><span data-ttu-id="b1d8c-476">[Önceki](xref:data/ef-rp/complex-data-model)İleri
>[](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="b1d8c-476">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end