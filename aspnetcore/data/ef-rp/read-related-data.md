---
title: "Razor sayfalarının EF çekirdek ile-ilgili verileri - 8'in 6 okuma"
author: rick-anderson
description: "Bu öğreticide okuyun ve ilgili verileri--diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler verileri görüntüleyin."
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 532020a8fe4c5a0312cbd89278e61f614b1825f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="f7fde-103">Okuma data - EF çekirdek Razor sayfaları (8 6) ile ilgili</span><span class="sxs-lookup"><span data-stu-id="f7fde-103">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="f7fde-104">Tarafından [zel Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f7fde-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="f7fde-105">Bu öğreticide, ilgili veri okumak ve görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="f7fde-106">İlgili verileri EF çekirdek Gezinti özelliklerini yükleyen verilerdir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="f7fde-107">Olamaz çözmek sorunlarla karşılaşırsanız, indirme [Bu aşama için tamamlanan uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="f7fde-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="f7fde-108">Aşağıdaki çizimler, Bu öğretici için tamamlanan sayfaları göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="f7fde-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Kurslar dizin sayfası](read-related-data/_static/courses-index.png)

![Eğitmen dizin sayfası](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="f7fde-111">İstekli, açık ve geç ilgili veri yükleme</span><span class="sxs-lookup"><span data-stu-id="f7fde-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="f7fde-112">EF çekirdek ilgili verileri bir varlık Gezinti özellikleri yükleyebilirsiniz birkaç yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="f7fde-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="f7fde-113">[İstekli yükleme](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="f7fde-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="f7fde-114">Bir varlık türü için bir sorgu aynı zamanda ilgili varlıklar yüklediğinde istekli Yükleme ' dir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="f7fde-115">Varlık okurken ilgili verileri alınır.</span><span class="sxs-lookup"><span data-stu-id="f7fde-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="f7fde-116">Bu, genellikle tüm gerekli olan verileri alan bir tek birleştirme sorgusunda sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="f7fde-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="f7fde-117">EF çekirdek istekli yükleme bazı türleri için birden çok sorgu gönderirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f7fde-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="f7fde-118">Birden çok sorgu verme EF6 bazı sorgularda söz konusu olandan daha etkili olabilir oluştu burada tek bir sorgu.</span><span class="sxs-lookup"><span data-stu-id="f7fde-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="f7fde-119">İstekli yükleme ile belirtilen `Include` ve `ThenInclude` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="f7fde-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![İstekli yükleme örneği](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="f7fde-121">Bir koleksiyon nvavigation dahil edildiğinde istekli yükleme birden çok sorgu gönderir:</span><span class="sxs-lookup"><span data-stu-id="f7fde-121">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="f7fde-122">Ana sorgu için bir sorgu</span><span class="sxs-lookup"><span data-stu-id="f7fde-122">One query for the main query</span></span> 
 * <span data-ttu-id="f7fde-123">Her bir koleksiyon yük ağacında "kenar" için bir sorgu.</span><span class="sxs-lookup"><span data-stu-id="f7fde-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="f7fde-124">Ayrı sorgularıyla `Load`: verileri ayrı sorgularda alınabilir ve EF çekirdek "düzeltmelerini" Gezinti özellikleri.</span><span class="sxs-lookup"><span data-stu-id="f7fde-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="f7fde-125">"düzeltmeler yukarı" anlamına gelir EF çekirdek Gezinti özellikleri otomatik olarak doldurur.</span><span class="sxs-lookup"><span data-stu-id="f7fde-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="f7fde-126">Ayrı sorgularıyla `Load` daha çok istekli yüklemekten yüklenirken explict gibi.</span><span class="sxs-lookup"><span data-stu-id="f7fde-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![Ayrı sorgulara örneği](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="f7fde-128">Not: EF çekirdek Gezinti özellikleri bağlam örneğine önceden yüklenmiş herhangi bir varlık için otomatik olarak düzeltir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="f7fde-129">Bir gezinme özelliği için veri olsa bile *değil* açıkça dahil, özellik hala bazıları doldurulmuş ya da tüm ilgili varlıklar önceden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="f7fde-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="f7fde-130">[Açık yükleme](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="f7fde-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="f7fde-131">Varlık ilk okunduğunda ilgili verileri alınan değil.</span><span class="sxs-lookup"><span data-stu-id="f7fde-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="f7fde-132">Kod gerektiğinde ilgili verileri almak üzere yazılmış olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f7fde-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="f7fde-133">Birden çok sorgu Veritabanına gönderilen ayrı sorgularla açık yükleme sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="f7fde-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="f7fde-134">Açık yükleme ile kod yüklenecek Gezinti özellikleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="f7fde-135">Kullanım `Load` açık yükleme yapmak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="f7fde-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="f7fde-136">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f7fde-136">For example:</span></span>

 ![Açık yükleme örneği](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="f7fde-138">[Yavaş Yükleniyor](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="f7fde-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="f7fde-139">[EF çekirdek geç yükleme şu anda desteklemiyor](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="f7fde-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="f7fde-140">Varlık ilk okunduğunda ilgili verileri alınan değil.</span><span class="sxs-lookup"><span data-stu-id="f7fde-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="f7fde-141">Bir gezinme özelliği ilk erişildiğinde bu gezinti özelliği için gerekli olan veriler otomatik olarak alınır.</span><span class="sxs-lookup"><span data-stu-id="f7fde-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="f7fde-142">Bir sorgu her zaman ilk olarak bir gezinti özelliği erişilir DB gönderilir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="f7fde-143">`Select` İşleci yalnızca gereken ilgili verileri yükler.</span><span class="sxs-lookup"><span data-stu-id="f7fde-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="f7fde-144">Bölüm adı görüntüler kurslar sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="f7fde-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="f7fde-145">İndirmelere varlık içeren bir gezinme özelliği içeren `Department` varlık.</span><span class="sxs-lookup"><span data-stu-id="f7fde-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="f7fde-146">`Department` Varlık indirmelere atandığı bölüm içerir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="f7fde-147">Atanan departmanı adını kurslar listesini görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="f7fde-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="f7fde-148">Alma `Name` özelliğinden `Department` varlık.</span><span class="sxs-lookup"><span data-stu-id="f7fde-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="f7fde-149">`Department` Varlık gelir `Course.Department` gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="f7fde-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse. Bölüm](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="f7fde-151">İskele indirmelere modeli</span><span class="sxs-lookup"><span data-stu-id="f7fde-151">Scaffold the Course model</span></span>

* <span data-ttu-id="f7fde-152">Visual Studio'dan çıkın.</span><span class="sxs-lookup"><span data-stu-id="f7fde-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="f7fde-153">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="f7fde-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f7fde-154">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="f7fde-154">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="f7fde-155">Yukarıdaki komut iskelesini kurar `Course` modeli.</span><span class="sxs-lookup"><span data-stu-id="f7fde-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="f7fde-156">Projesini Visual Studio'da açın.</span><span class="sxs-lookup"><span data-stu-id="f7fde-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="f7fde-157">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f7fde-157">Build the project.</span></span> <span data-ttu-id="f7fde-158">Derleme hataları aşağıdaki gibi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="f7fde-158">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="f7fde-159">Genel olarak değiştirmek `_context.Course` için `_context.Courses` ("s" eklemek diğer bir deyişle, `Course`).</span><span class="sxs-lookup"><span data-stu-id="f7fde-159">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="f7fde-160">7 oluşumu bulundu ve güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="f7fde-160">7 occurrences are found and updated.</span></span>

<span data-ttu-id="f7fde-161">Açık *Pages/Courses/Index.cshtml.cs* ve inceleyin `OnGetAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f7fde-161">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="f7fde-162">Yapı iskelesi altyapısı istekli yükleme için belirtilen `Department` gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="f7fde-162">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="f7fde-163">`Include` Yöntemi istekli yüklenmesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-163">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="f7fde-164">Uygulamayı çalıştırın ve seçin **kurslar** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="f7fde-164">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="f7fde-165">Bölüm sütunu görüntüler `DepartmentID`, hangi kullanışlı değildir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-165">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="f7fde-166">Güncelleştirme `OnGetAsync` aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f7fde-166">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="f7fde-167">Önceki kod ekler `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="f7fde-167">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="f7fde-168">`AsNoTracking`döndürülen varlıkları değil izlendiği için performansı geliştirir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-168">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="f7fde-169">Geçerli bağlamda güncelleştirilir değil çünkü varlıkları izlenmez.</span><span class="sxs-lookup"><span data-stu-id="f7fde-169">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="f7fde-170">Güncelleştirme *Views/Courses/Index.cshtml* aşağıdaki vurgulanmış biçimlendirmeyi ile:</span><span class="sxs-lookup"><span data-stu-id="f7fde-170">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="f7fde-171">İskele kurulmuş kod aşağıdaki değişiklikler yapılmıştır:</span><span class="sxs-lookup"><span data-stu-id="f7fde-171">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="f7fde-172">Başlık dizinden kurslara değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="f7fde-172">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="f7fde-173">Eklenen bir **numarası** gösterir sütun `CourseID` özellik değeri.</span><span class="sxs-lookup"><span data-stu-id="f7fde-173">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="f7fde-174">Normalde son kullanıcılara anlamsız oldukları için varsayılan olarak, birincil anahtarlar iskele kurulmuş değil.</span><span class="sxs-lookup"><span data-stu-id="f7fde-174">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="f7fde-175">Ancak, bu durumda birincil anahtarı anlamlıdır.</span><span class="sxs-lookup"><span data-stu-id="f7fde-175">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="f7fde-176">Değiştirilen **departmanı** bölüm adını görüntülemek için sütun.</span><span class="sxs-lookup"><span data-stu-id="f7fde-176">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="f7fde-177">Kod görüntüler `Name` özelliği `Department` yüklenen varlık `Department` gezinti özelliği:</span><span class="sxs-lookup"><span data-stu-id="f7fde-177">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="f7fde-178">Uygulamayı çalıştırın ve seçin **kurslar** bölüm adlarını listesiyle görmek için sekmesini.</span><span class="sxs-lookup"><span data-stu-id="f7fde-178">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kurslar dizin sayfası](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="f7fde-180">Yükleme Seç verilerle ilgili</span><span class="sxs-lookup"><span data-stu-id="f7fde-180">Loading related data with Select</span></span>

<span data-ttu-id="f7fde-181">`OnGetAsync` Yöntemi ile ilgili verileri yükler `Include` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f7fde-181">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="f7fde-182">`Select` İşleci yalnızca gereken ilgili verileri yükler.</span><span class="sxs-lookup"><span data-stu-id="f7fde-182">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="f7fde-183">Tek öğelerin gibi `Department.Name` bir SQL INNER JOIN kullanır.</span><span class="sxs-lookup"><span data-stu-id="f7fde-183">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="f7fde-184">Koleksiyonlar için başka bir veritabanı erişimi kullanır, ancak bu nedenle yapar.`Include`</span><span class="sxs-lookup"><span data-stu-id="f7fde-184">For collections it uses another database access, but so does the .`Include`</span></span> <span data-ttu-id="f7fde-185">koleksiyonlarda işleci.</span><span class="sxs-lookup"><span data-stu-id="f7fde-185">operator on collections.</span></span>

<span data-ttu-id="f7fde-186">Aşağıdaki kod ile ilgili verileri yükler `Select` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f7fde-186">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="f7fde-187">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="f7fde-187">The `CourseViewModel`:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="f7fde-188">Bkz: [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) ve [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) tam bir örnek için.</span><span class="sxs-lookup"><span data-stu-id="f7fde-188">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="f7fde-189">Kurslar ve kayıtları gösteren bir eğitmen sayfası oluşturun</span><span class="sxs-lookup"><span data-stu-id="f7fde-189">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="f7fde-190">Bu bölümde, eğitmen sayfa oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f7fde-190">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="f7fde-191">![Eğitmen dizin sayfası](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="f7fde-191">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="f7fde-192">Bu sayfayı okur ve ilgili verileri aşağıdaki yollarla görüntüler:</span><span class="sxs-lookup"><span data-stu-id="f7fde-192">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="f7fde-193">İlgili verileri Eğitmen listesini görüntüleyen `OfficeAssignment` varlık (önceki görüntüde Office).</span><span class="sxs-lookup"><span data-stu-id="f7fde-193">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="f7fde-194">`Instructor` Ve `OfficeAssignment` varlıklardır bir tane-sıfır-veya-bir ilişkisi.</span><span class="sxs-lookup"><span data-stu-id="f7fde-194">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="f7fde-195">İstekli yükleme için kullanıldığından `OfficeAssignment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="f7fde-195">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="f7fde-196">İlgili verileri görüntülenecek gerektiğinde istekli yükleme genellikle daha verimli olur.</span><span class="sxs-lookup"><span data-stu-id="f7fde-196">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="f7fde-197">Bu durumda, eğitmen office atamaları görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-197">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="f7fde-198">Kullanıcı ilişkili bir eğitmen (Harui) önceki görüntüde seçtiğinde `Course` varlıkları görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-198">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="f7fde-199">`Instructor` Ve `Course` varlıklardır bir çok-çok ilişkisi.</span><span class="sxs-lookup"><span data-stu-id="f7fde-199">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="f7fde-200">İçin yükleme eager `Course` varlıkları ve bunların ilgili `Department` varlıklar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f7fde-200">Eager loading for the `Course` entities and their related `Department` entities is used.</span></span> <span data-ttu-id="f7fde-201">Bu durumda, yalnızca seçili Eğitmen kursları gerekli olduğu için ayrı sorgulara daha etkili olabilir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-201">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="f7fde-202">Bu örnek gezinme özelliklerinin gezinme özellikleri varlıklardaki istekli yükleme kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-202">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="f7fde-203">Kullanıcı indirmelere (Kimya önceki görüntüde) seçtiğinde ilgili verileri `Enrollments` varlık görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-203">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="f7fde-204">Önceki görüntüde Öğrenci adı ve düzeyde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-204">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="f7fde-205">`Course` Ve `Enrollment` varlıklardır bir-çok ilişkisi.</span><span class="sxs-lookup"><span data-stu-id="f7fde-205">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="f7fde-206">Eğitmen dizin görünüm için Görünüm modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="f7fde-206">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="f7fde-207">Eğitmen sayfanın üç farklı tablolardan verileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-207">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="f7fde-208">Bir görünüm modeli üç tabloyu temsil eden üç varlıkları içeren oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f7fde-208">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="f7fde-209">İçinde *SchoolViewModels* klasörü oluşturmak *InstructorIndexData.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="f7fde-209">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="f7fde-210">İskele Eğitmen modeli</span><span class="sxs-lookup"><span data-stu-id="f7fde-210">Scaffold the Instructor model</span></span>

* <span data-ttu-id="f7fde-211">Visual Studio'dan çıkın.</span><span class="sxs-lookup"><span data-stu-id="f7fde-211">Exit Visual Studio.</span></span>
* <span data-ttu-id="f7fde-212">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="f7fde-212">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f7fde-213">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="f7fde-213">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="f7fde-214">Yukarıdaki komut iskelesini kurar `Instructor` modeli.</span><span class="sxs-lookup"><span data-stu-id="f7fde-214">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="f7fde-215">Projesini Visual Studio'da açın.</span><span class="sxs-lookup"><span data-stu-id="f7fde-215">Open the project in Visual Studio.</span></span>

<span data-ttu-id="f7fde-216">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f7fde-216">Build the project.</span></span> <span data-ttu-id="f7fde-217">Derleme hataları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f7fde-217">The build generates errors.</span></span>

<span data-ttu-id="f7fde-218">Genel olarak değiştirmek `_context.Instructor` için `_context.Instructors` ("s" eklemek diğer bir deyişle, `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="f7fde-218">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="f7fde-219">7 oluşumu bulundu ve güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="f7fde-219">7 occurrences are found and updated.</span></span>

<span data-ttu-id="f7fde-220">Uygulamayı çalıştırın ve Eğitmen sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="f7fde-220">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="f7fde-221">Değiştir *Pages/Instructors/Index.cshtml.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="f7fde-221">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

<span data-ttu-id="f7fde-222">`OnGetAsync` Yöntemi seçili Eğitmen kimliği için isteğe bağlı rota veri kabul eder.</span><span class="sxs-lookup"><span data-stu-id="f7fde-222">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="f7fde-223">Üzerinde sorgu inceleyin *Pages/Instructors/Index.cshtml* sayfa:</span><span class="sxs-lookup"><span data-stu-id="f7fde-223">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="f7fde-224">Sorgu iki sahip içerir:</span><span class="sxs-lookup"><span data-stu-id="f7fde-224">The query has two includes:</span></span>

* <span data-ttu-id="f7fde-225">`OfficeAssignment`: Görüntülenen [Eğitmen Görünüm](#IP).</span><span class="sxs-lookup"><span data-stu-id="f7fde-225">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="f7fde-226">`CourseAssignments`: Hangi öğrettin kursları getirir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-226">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="f7fde-227">Güncelleştirme Eğitmen dizin sayfası</span><span class="sxs-lookup"><span data-stu-id="f7fde-227">Update the instructors Index page</span></span>

<span data-ttu-id="f7fde-228">Güncelleştirme *Pages/Instructors/Index.cshtml* aşağıdaki biçimlendirme ile:</span><span class="sxs-lookup"><span data-stu-id="f7fde-228">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="f7fde-229">Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="f7fde-229">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="f7fde-230">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="f7fde-230">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="f7fde-231">`"{id:int?}"`bir rota şablonudur.</span><span class="sxs-lookup"><span data-stu-id="f7fde-231">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="f7fde-232">Rota şablonu için rota verilerini tamsayı URL'deki sorgu dizelerini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-232">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="f7fde-233">Örneğin, tıklayarak **seçin** bağlantı sayfa yönergesi aşağıdaki gibi bir URL vermediğinde bir eğitmen için:</span><span class="sxs-lookup"><span data-stu-id="f7fde-233">For example, clicking on the **Select** link for an instructor when the page directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="f7fde-234">Sayfa yönergesi olduğunda `@page "{id:int?}"`, önceki URL'si:</span><span class="sxs-lookup"><span data-stu-id="f7fde-234">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="f7fde-235">Sayfa başlığı **Eğitmen**.</span><span class="sxs-lookup"><span data-stu-id="f7fde-235">Page title is **Instructors**.</span></span>
* <span data-ttu-id="f7fde-236">Eklenen bir **Office** görüntüleyen sütun `item.OfficeAssignment.Location` yalnızca `item.OfficeAssignment` null değil.</span><span class="sxs-lookup"><span data-stu-id="f7fde-236">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="f7fde-237">Bu bir tane-sıfır-veya-bir ilişkisi olduğundan, ilgili OfficeAssignment varlık olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-237">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="f7fde-238">Eklenen bir **kurslar** kurslar görüntüleyen sütun tarafından her Eğitmen öğrettin.</span><span class="sxs-lookup"><span data-stu-id="f7fde-238">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="f7fde-239">Bkz: [açık satır geçişle `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) bu razor sözdizimi hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="f7fde-239">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="f7fde-240">Dinamik olarak ekleyen eklenen kod `class="success"` için `tr` seçili Eğitmen öğesidir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-240">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="f7fde-241">Bu önyükleme sınıfını kullanarak seçili olan satır için bir arka plan rengini belirler.</span><span class="sxs-lookup"><span data-stu-id="f7fde-241">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="f7fde-242">Etiketli yeni bir köprü eklenen **seçin**.</span><span class="sxs-lookup"><span data-stu-id="f7fde-242">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="f7fde-243">Bu bağlantı için seçilen Eğitmen kimliği gönderir `Index` yöntemi ve arka plan rengini belirler.</span><span class="sxs-lookup"><span data-stu-id="f7fde-243">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="f7fde-244">Uygulamayı çalıştırın ve seçin **Eğitmen** sekmesi. Sayfa görüntüler `Location` (office) ilgili öğesinden `OfficeAssignment` varlık.</span><span class="sxs-lookup"><span data-stu-id="f7fde-244">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="f7fde-245">Varsa OfficeAssignment' olan bir boş tablo hücresi null görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-245">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Seçili hiçbir şey Eğitmen dizin sayfası](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="f7fde-247">Tıklayın **seçin** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="f7fde-247">Click on the **Select** link.</span></span> <span data-ttu-id="f7fde-248">Satır stili değişiklikleri.</span><span class="sxs-lookup"><span data-stu-id="f7fde-248">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="f7fde-249">Seçili Eğitmen tarafından öğrettin kurslar ekleme</span><span class="sxs-lookup"><span data-stu-id="f7fde-249">Add courses taught by selected instructor</span></span>

<span data-ttu-id="f7fde-250">Güncelleştirme `OnGetAsync` yönteminde *Pages/Instructors/Index.cshtml.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="f7fde-250">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

<span data-ttu-id="f7fde-251">Güncelleştirilmiş sorgu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="f7fde-251">Examine the updated query:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="f7fde-252">Önceki sorgunun ekler `Department` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="f7fde-252">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="f7fde-253">Aşağıdaki kod bir eğitmen seçildiğinde yürütür (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="f7fde-253">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="f7fde-254">Seçili Eğitmen görünüm modeli Eğitmen listesi alınır.</span><span class="sxs-lookup"><span data-stu-id="f7fde-254">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="f7fde-255">Görünüm modelinin `Courses` özelliği ile yüklenir `Course` Bu eğitmen varlıklardan `CourseAssignments` gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="f7fde-255">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="f7fde-256">`Where` Yöntem koleksiyonu döndürür.</span><span class="sxs-lookup"><span data-stu-id="f7fde-256">The `Where` method returns a collection.</span></span> <span data-ttu-id="f7fde-257">Yukarıdaki içinde `Where` yöntemi, tek bir `Instructor` varlık döndürülür.</span><span class="sxs-lookup"><span data-stu-id="f7fde-257">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="f7fde-258">`Single` Yöntemi tek bir koleksiyon dönüştürür `Instructor` varlık.</span><span class="sxs-lookup"><span data-stu-id="f7fde-258">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="f7fde-259">`Instructor` Varlık erişim sağlar `CourseAssignments` özelliği.</span><span class="sxs-lookup"><span data-stu-id="f7fde-259">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="f7fde-260">`CourseAssignments`ilgili erişim sağlayan `Course` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="f7fde-260">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Eğitmen kurslar m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="f7fde-262">`Single` Koleksiyon yalnızca bir öğe varsa, yöntemi bir koleksiyonda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f7fde-262">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="f7fde-263">`Single` Yöntemi koleksiyonu boş ise veya birden çok öğe varsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f7fde-263">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="f7fde-264">Bir alternatif `SingleOrDefault`, hangi varsayılan değeri döndürür (Bu durumda null) koleksiyonu boş ise.</span><span class="sxs-lookup"><span data-stu-id="f7fde-264">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="f7fde-265">Kullanarak `SingleOrDefault` boş bir koleksiyon üzerinde:</span><span class="sxs-lookup"><span data-stu-id="f7fde-265">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="f7fde-266">Bir özel durum oluşur (bulunmaya çalışılırken gelen bir `Courses` özelliği bir null başvuru).</span><span class="sxs-lookup"><span data-stu-id="f7fde-266">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="f7fde-267">Özel durum iletisi daha az açıkça sorunun nedenini gösterir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-267">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="f7fde-268">Aşağıdaki kod görünüm modelinin doldurur `Enrollments` bir indirmelere seçildiğinde özelliği:</span><span class="sxs-lookup"><span data-stu-id="f7fde-268">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="f7fde-269">Aşağıdaki biçimlendirmede sonuna ekleyin *Pages/Courses/Index.cshtml* Razor sayfasını:</span><span class="sxs-lookup"><span data-stu-id="f7fde-269">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

<span data-ttu-id="f7fde-270">Önceki biçimlendirme bir eğitmen seçildiğinde bir eğitmen ilgili kurslar listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="f7fde-270">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="f7fde-271">Uygulamayı test etme.</span><span class="sxs-lookup"><span data-stu-id="f7fde-271">Test the app.</span></span> <span data-ttu-id="f7fde-272">Tıklayın bir **seçin** bağlantısı Eğitmen sayfası.</span><span class="sxs-lookup"><span data-stu-id="f7fde-272">Click on a **Select** link on the instructors page.</span></span>

![Seçili Eğitmen dizin sayfası Eğitmen](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="f7fde-274">Öğrenci verileri göster</span><span class="sxs-lookup"><span data-stu-id="f7fde-274">Show student data</span></span>

<span data-ttu-id="f7fde-275">Bu bölümde, uygulama seçili indirmelere Öğrenci verileri gösterecek biçimde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-275">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="f7fde-276">Sorguda güncelleştirme `OnGetAsync` yönteminde *Pages/Instructors/Index.cshtml.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="f7fde-276">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="f7fde-277">Güncelleştirme *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f7fde-277">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="f7fde-278">Aşağıdaki biçimlendirmede dosyanın sonuna ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f7fde-278">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="f7fde-279">Önceki biçimlendirme seçili indirmelere kaydedilen Öğrenciler listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="f7fde-279">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="f7fde-280">Sayfayı yenileyin ve bir eğitmen seçin.</span><span class="sxs-lookup"><span data-stu-id="f7fde-280">Refresh the page and select an instructor.</span></span> <span data-ttu-id="f7fde-281">Kayıtlı Öğrenciler ve bunların dereceleri listesini görmek için bir indirmelere seçin.</span><span class="sxs-lookup"><span data-stu-id="f7fde-281">Select a course to see the list of enrolled students and their grades.</span></span>

![Eğitmen dizin sayfası Eğitmen ve seçili indirmelere](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="f7fde-283">Tek kullanma</span><span class="sxs-lookup"><span data-stu-id="f7fde-283">Using Single</span></span>

<span data-ttu-id="f7fde-284">`Single` Yöntemi geçirebilir `Where` çağırmak yerine koşulu `Where` yöntemi ayrı olarak:</span><span class="sxs-lookup"><span data-stu-id="f7fde-284">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="f7fde-285">Yukarıdaki `Single` yaklaşım kullanarak üzerinden hiçbir yararları sağlar `Where`.</span><span class="sxs-lookup"><span data-stu-id="f7fde-285">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="f7fde-286">Bazı geliştiriciler tercih `Single` yaklaşımını stili.</span><span class="sxs-lookup"><span data-stu-id="f7fde-286">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="f7fde-287">Açık yükleniyor</span><span class="sxs-lookup"><span data-stu-id="f7fde-287">Explicit loading</span></span>

<span data-ttu-id="f7fde-288">İstekli yükleme için geçerli kod belirtir `Enrollments` ve `Students`:</span><span class="sxs-lookup"><span data-stu-id="f7fde-288">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="f7fde-289">Kullanıcıların nadiren bir seyrinde kayıtları görmek istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="f7fde-289">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="f7fde-290">Bu durumda, bir en iyi duruma getirme istenirse kayıt verileri yalnızca yüklemek olacaktır.</span><span class="sxs-lookup"><span data-stu-id="f7fde-290">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="f7fde-291">Bu bölümde `OnGetAsync` açık yüklenmesini kullanmak için güncelleştirilmiş `Enrollments` ve `Students`.</span><span class="sxs-lookup"><span data-stu-id="f7fde-291">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="f7fde-292">Güncelleştirme `OnGetAsync` aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="f7fde-292">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="f7fde-293">Önceki kod bırakır *ThenInclude* için kayıt ve Öğrenci verileri yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="f7fde-293">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="f7fde-294">Bir indirmelere seçtiyseniz vurgulanmış kodu alır:</span><span class="sxs-lookup"><span data-stu-id="f7fde-294">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="f7fde-295">`Enrollment` Seçili indirmelere için varlıklar.</span><span class="sxs-lookup"><span data-stu-id="f7fde-295">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="f7fde-296">`Student` Varlıklar her `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="f7fde-296">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="f7fde-297">Yorumlar çıkış kodu yukarıdaki fark `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="f7fde-297">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="f7fde-298">Gezinti özellikleri, izlenen varlıklar için yalnızca bir açıkça yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-298">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="f7fde-299">Uygulamayı test etme.</span><span class="sxs-lookup"><span data-stu-id="f7fde-299">Test the app.</span></span> <span data-ttu-id="f7fde-300">Kullanıcılar açısından bakıldığında, uygulamanın önceki sürüme geri aynı şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="f7fde-300">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="f7fde-301">Sonraki öğretici ilgili verileri güncelleştirmek nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="f7fde-301">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="f7fde-302">[Önceki](xref:data/ef-rp/complex-data-model)
>[sonraki](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="f7fde-302">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
