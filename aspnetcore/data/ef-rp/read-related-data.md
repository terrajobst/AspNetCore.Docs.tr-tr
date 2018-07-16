---
title: ASP.NET core'da - EF çekirdekli Razor sayfaları 6 8 - ilgili verileri okuma
author: rick-anderson
description: Bu öğreticide okuyun ve ilgili verileri--diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler verileri görüntüler.
ms.author: riande
ms.date: 11/05/2017
uid: data/ef-rp/read-related-data
ms.openlocfilehash: fa3147cc4ad121784911eef802e04ca91f16448f
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063318"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="8e97e-103">ASP.NET core'da - EF çekirdekli Razor sayfaları 6 8 - ilgili verileri okuma</span><span class="sxs-lookup"><span data-stu-id="8e97e-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="8e97e-104">Tarafından [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8e97e-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="8e97e-105">Bu öğreticide, ilgili verileri okuma ve görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="8e97e-106">İlgili verileri EF Core Gezinti özelliklerini yüklenen verilerdir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="8e97e-107">Olamaz çözmek sorunlarla karşılaşırsanız, indirme [Bu aşama için tamamlanan uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="8e97e-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="8e97e-108">Aşağıdaki çizimler, Bu öğretici için tamamlanmış sayfaların göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="8e97e-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Kursları dizin sayfası](read-related-data/_static/courses-index.png)

![Eğitmenler dizin sayfası](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="8e97e-111">İlgili veri eager, açık ve yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="8e97e-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="8e97e-112">EF Core Gezinti özelliklerini bir varlığın ilgili verileri yükleyebilir birkaç yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="8e97e-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="8e97e-113">[İstekli yükleme](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="8e97e-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="8e97e-114">Bir varlık türü için sorgu ilgili varlıkları yüklediğinde istekli Yükleme ' dir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="8e97e-115">Varlık okunduğunda, ilgili verileri alınır.</span><span class="sxs-lookup"><span data-stu-id="8e97e-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="8e97e-116">Bu, tek bir birleşim sorguda tüm gerekli olan verileri alır. genellikle sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="8e97e-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="8e97e-117">EF Core istekli yükleme bazı türleri için birden çok sorgu verecek.</span><span class="sxs-lookup"><span data-stu-id="8e97e-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="8e97e-118">Birden çok sorgu göndermeye çalışması için bazı sorguları EF6 olandan daha verimli olabilir dönemler tek bir sorgu.</span><span class="sxs-lookup"><span data-stu-id="8e97e-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="8e97e-119">İstekli yükleme belirtilirse `Include` ve `ThenInclude` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="8e97e-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![İstekli yükleme örneği](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="8e97e-121">Bir koleksiyon Gezinti dahil edildiğinde istekli yükleme birden çok sorgu gönderir:</span><span class="sxs-lookup"><span data-stu-id="8e97e-121">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="8e97e-122">Ana sorgu için bir sorgu</span><span class="sxs-lookup"><span data-stu-id="8e97e-122">One query for the main query</span></span> 
  * <span data-ttu-id="8e97e-123">"Edge" Yük ağacında her bir koleksiyon için bir sorgu.</span><span class="sxs-lookup"><span data-stu-id="8e97e-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="8e97e-124">Ayrı sorgularla `Load`: veriler ayrı sorgularda alınabilir ve EF Core "düzeltmeler" Gezinti özellikleri.</span><span class="sxs-lookup"><span data-stu-id="8e97e-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="8e97e-125">"düzeltmeler yukarı" anlamına gelir EF Core gezinme özelliklerini otomatik olarak doldurur.</span><span class="sxs-lookup"><span data-stu-id="8e97e-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="8e97e-126">Ayrı sorgularla `Load` işinizi daha istekli yükleme yükleme gibi daha.</span><span class="sxs-lookup"><span data-stu-id="8e97e-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

  ![Ayrı sorguları örneği](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="8e97e-128">Not: EF Core Gezinti özellikleri bağlam örneğine önceden yüklenmiş herhangi bir varlık için otomatik olarak düzeltir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="8e97e-129">Bir gezinme özelliği için veri olsa bile *değil* açıkça dahil, özellik hala bazıları doldurulabilir veya tüm ilişkili varlıkları daha önce yüklenmiş.</span><span class="sxs-lookup"><span data-stu-id="8e97e-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="8e97e-130">[Açık yükleme](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="8e97e-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="8e97e-131">Varlığın ilk okunduğunda, ilgili verileri alınan değil.</span><span class="sxs-lookup"><span data-stu-id="8e97e-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="8e97e-132">Kod gerektiğinde ilgili verileri almak üzere yazılmış olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8e97e-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="8e97e-133">Birden çok sorgu Veritabanına gönderilir açık yükleme ayrı sorgular ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="8e97e-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="8e97e-134">Açık yükleme ile kod yüklenmesine, gezinti özellikleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="8e97e-135">Kullanım `Load` açık yükleme yapmak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8e97e-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="8e97e-136">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8e97e-136">For example:</span></span>

  ![Açık yükleme örneği](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="8e97e-138">[Yavaş Yükleniyor](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="8e97e-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="8e97e-139">[EF Core yavaş yükleme şu anda desteklemiyor](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="8e97e-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="8e97e-140">Varlığın ilk okunduğunda, ilgili verileri alınan değil.</span><span class="sxs-lookup"><span data-stu-id="8e97e-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="8e97e-141">Gezinti özelliğine erişinceye, ilk kez bu gezinti özelliği için gerekli verileri otomatik olarak alınır.</span><span class="sxs-lookup"><span data-stu-id="8e97e-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="8e97e-142">Her zaman için ilk kez bir gezinti özelliğine erişinceye veritabanına bir sorgu gönderilir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="8e97e-143">`Select` İşleci yalnızca gerekli ilgili verileri yükler.</span><span class="sxs-lookup"><span data-stu-id="8e97e-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="8e97e-144">Bölüm adını görüntüleyen bir kursları sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="8e97e-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="8e97e-145">Kurs varlığı içeren bir gezinme özelliği içeren `Department` varlık.</span><span class="sxs-lookup"><span data-stu-id="8e97e-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="8e97e-146">`Department` Varlık kursu atanır bölümü içerir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="8e97e-147">Atanan bölüm adını kursları listesini görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="8e97e-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="8e97e-148">Alma `Name` özelliğinden `Department` varlık.</span><span class="sxs-lookup"><span data-stu-id="8e97e-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="8e97e-149">`Department` Varlık geldiği `Course.Department` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="8e97e-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse. Bölüm](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="8e97e-151">İskele kurs modeli</span><span class="sxs-lookup"><span data-stu-id="8e97e-151">Scaffold the Course model</span></span>

* <span data-ttu-id="8e97e-152">Visual Studio'dan çıkın.</span><span class="sxs-lookup"><span data-stu-id="8e97e-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="8e97e-153">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="8e97e-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="8e97e-154">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="8e97e-154">Run the following command:</span></span>

  ```console
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

<span data-ttu-id="8e97e-155">Önceki komut iskelesini kurar `Course` modeli.</span><span class="sxs-lookup"><span data-stu-id="8e97e-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="8e97e-156">Projeyi Visual Studio'da açın.</span><span class="sxs-lookup"><span data-stu-id="8e97e-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="8e97e-157">Açık *Pages/Courses/Index.cshtml.cs* ve incelemek `OnGetAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8e97e-157">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="8e97e-158">Yapı iskelesi altyapısı istekli yükleme için belirtilen `Department` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="8e97e-158">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="8e97e-159">`Include` İstekli yükleme yöntemini belirtir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-159">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="8e97e-160">Uygulamayı çalıştırmak ve seçmek **kursları** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="8e97e-160">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="8e97e-161">Bölüm sütun görüntüler `DepartmentID`, hangi kullanışlı değildir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-161">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="8e97e-162">Güncelleştirme `OnGetAsync` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="8e97e-162">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="8e97e-163">Yukarıdaki kod ekler `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="8e97e-163">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="8e97e-164">`AsNoTracking` döndürülen varlıkları izlenmez performansı artırır.</span><span class="sxs-lookup"><span data-stu-id="8e97e-164">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="8e97e-165">Geçerli bağlamda güncelleştirilir değil çünkü varlıkları izlenmez.</span><span class="sxs-lookup"><span data-stu-id="8e97e-165">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="8e97e-166">Güncelleştirme *Pages/Courses/Index.cshtml* aşağıdaki vurgulanmış biçimlendirmeye sahip:</span><span class="sxs-lookup"><span data-stu-id="8e97e-166">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="8e97e-167">İskele kurulan kodu için aşağıdaki değişiklikler yapılmıştır:</span><span class="sxs-lookup"><span data-stu-id="8e97e-167">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="8e97e-168">Başlık dizinden kurslara değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="8e97e-168">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="8e97e-169">Eklenen bir **numarası** gösteren sütun `CourseID` özellik değeri.</span><span class="sxs-lookup"><span data-stu-id="8e97e-169">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="8e97e-170">Normalde son kullanıcılara anlamsız olduğunuz için varsayılan olarak, birincil anahtarlar iskele kurulmuş değildir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-170">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="8e97e-171">Ancak, bu durumda birincil anahtarı anlam ifade eder.</span><span class="sxs-lookup"><span data-stu-id="8e97e-171">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="8e97e-172">Değiştirilen **departmanı** bölüm adını görüntülemek için sütun.</span><span class="sxs-lookup"><span data-stu-id="8e97e-172">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="8e97e-173">Kod görüntüler `Name` özelliği `Department` yüklendiği varlık `Department` gezinti özelliği:</span><span class="sxs-lookup"><span data-stu-id="8e97e-173">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="8e97e-174">Uygulamayı çalıştırmak ve seçmek **kursları** bölüm adları listesini görmek için sekmesinde.</span><span class="sxs-lookup"><span data-stu-id="8e97e-174">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kursları dizin sayfası](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="8e97e-176">İlgili belirli verileri yükleniyor</span><span class="sxs-lookup"><span data-stu-id="8e97e-176">Loading related data with Select</span></span>

<span data-ttu-id="8e97e-177">`OnGetAsync` Yöntemi ile ilgili verileri yükler `Include` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8e97e-177">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="8e97e-178">`Select` İşleci yalnızca gerekli ilgili verileri yükler.</span><span class="sxs-lookup"><span data-stu-id="8e97e-178">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="8e97e-179">Tek öğeleri gibi `Department.Name` bir SQL INNER JOIN kullanır.</span><span class="sxs-lookup"><span data-stu-id="8e97e-179">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="8e97e-180">Koleksiyon, başka bir veritabanı erişimi kullanır, ancak bu nedenle mu `Include` koleksiyonlarda işleci.</span><span class="sxs-lookup"><span data-stu-id="8e97e-180">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="8e97e-181">Aşağıdaki kod ile ilgili verileri yükler `Select` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8e97e-181">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="8e97e-182">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="8e97e-182">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="8e97e-183">Bkz: [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) ve [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) tam bir örnek.</span><span class="sxs-lookup"><span data-stu-id="8e97e-183">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="8e97e-184">Kursları ve kayıtları gösterir bir eğitmen sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="8e97e-184">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="8e97e-185">Bu bölümde, eğitmenler sayfa oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8e97e-185">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="8e97e-186">![Eğitmenler dizin sayfası](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="8e97e-186">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="8e97e-187">Bu sayfada okur ve ilgili verileri aşağıdaki yollarla görüntüler:</span><span class="sxs-lookup"><span data-stu-id="8e97e-187">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="8e97e-188">Eğitmenler listesini ilgili verileri görüntüleyen `OfficeAssignment` varlık (önceki görüntüde Office).</span><span class="sxs-lookup"><span data-stu-id="8e97e-188">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="8e97e-189">`Instructor` Ve `OfficeAssignment` bir sıfır-veya-bir ilişkide varlıklardır.</span><span class="sxs-lookup"><span data-stu-id="8e97e-189">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="8e97e-190">İstekli yükleme için kullanılan `OfficeAssignment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="8e97e-190">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="8e97e-191">İlgili veriler görüntülenecek gerektiğinde istekli yükleme genellikle daha verimli olur.</span><span class="sxs-lookup"><span data-stu-id="8e97e-191">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="8e97e-192">Bu durumda, eğitmenler office atamaları görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-192">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="8e97e-193">Kullanıcı, ilişkili bir eğitmen (önceki görüntüde Harui) seçtiğinde `Course` varlıkları görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-193">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="8e97e-194">`Instructor` Ve `Course` bir çoktan çoğa ilişki içinde varlıklardır.</span><span class="sxs-lookup"><span data-stu-id="8e97e-194">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="8e97e-195">İstekli yükleme için kullanılan `Course` varlıkları ve bunların ilgili `Department` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="8e97e-195">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="8e97e-196">Bu durumda, yalnızca seçili Eğitmen kursları gerekli olduğu ayrı sorgular daha verimli olabilir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-196">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="8e97e-197">Bu örnek gezinme özelliklerinin gezinme özelliklerinde varlıklardaki istekli yükleme kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-197">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="8e97e-198">Kullanıcı bir kurs (önceki görüntüde Kimya) seçtiğinde ilgili verileri `Enrollments` varlık görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-198">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="8e97e-199">Yukarıdaki görüntüde, Öğrenci adı ve sınıf görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-199">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="8e97e-200">`Course` Ve `Enrollment` bir bire çok ilişkiye varlıklardır.</span><span class="sxs-lookup"><span data-stu-id="8e97e-200">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="8e97e-201">Eğitmen dizini görünümü için bir görünüm modeli oluşturun</span><span class="sxs-lookup"><span data-stu-id="8e97e-201">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="8e97e-202">Eğitmenler sayfanın üç farklı tablolardaki verileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-202">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="8e97e-203">Üç tabloyu temsil eden üç varlıkları içeren bir görünüm modeli oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8e97e-203">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="8e97e-204">İçinde *SchoolViewModels* klasör oluşturma *InstructorIndexData.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="8e97e-204">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="8e97e-205">İskele Eğitmen modeli</span><span class="sxs-lookup"><span data-stu-id="8e97e-205">Scaffold the Instructor model</span></span>

* <span data-ttu-id="8e97e-206">Visual Studio'dan çıkın.</span><span class="sxs-lookup"><span data-stu-id="8e97e-206">Exit Visual Studio.</span></span>
* <span data-ttu-id="8e97e-207">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="8e97e-207">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="8e97e-208">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="8e97e-208">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

<span data-ttu-id="8e97e-209">Önceki komut iskelesini kurar `Instructor` modeli.</span><span class="sxs-lookup"><span data-stu-id="8e97e-209">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="8e97e-210">Projeyi Visual Studio'da açın.</span><span class="sxs-lookup"><span data-stu-id="8e97e-210">Open the project in Visual Studio.</span></span>

<span data-ttu-id="8e97e-211">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8e97e-211">Build the project.</span></span> <span data-ttu-id="8e97e-212">Derleme hataları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8e97e-212">The build generates errors.</span></span>

<span data-ttu-id="8e97e-213">Genel olarak değiştirme `_context.Instructor` için `_context.Instructors` ("s" eklemek diğer bir deyişle, `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="8e97e-213">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="8e97e-214">7 oluşum bulundu ve güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="8e97e-214">7 occurrences are found and updated.</span></span>

<span data-ttu-id="8e97e-215">Uygulamayı çalıştırın ve Eğitmenler sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="8e97e-215">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="8e97e-216">Değiştirin *Pages/Instructors/Index.cshtml.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="8e97e-216">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-99)]

<span data-ttu-id="8e97e-217">`OnGetAsync` Yöntemi seçili Eğitmen kimliği için isteğe bağlı rota verileri kabul eder.</span><span class="sxs-lookup"><span data-stu-id="8e97e-217">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="8e97e-218">Sorguyu inceleyin *Pages/Instructors/Index.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8e97e-218">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="8e97e-219">İki sorgu içeren içerir:</span><span class="sxs-lookup"><span data-stu-id="8e97e-219">The query has two includes:</span></span>

* <span data-ttu-id="8e97e-220">`OfficeAssignment`: İçinde görüntülenen [Eğitmenler görünümü](#IP).</span><span class="sxs-lookup"><span data-stu-id="8e97e-220">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="8e97e-221">`CourseAssignments`: Bu, verilen derslerimiz getirir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-221">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="8e97e-222">Eğitmenler dizin sayfası</span><span class="sxs-lookup"><span data-stu-id="8e97e-222">Update the instructors Index page</span></span>

<span data-ttu-id="8e97e-223">Güncelleştirme *Pages/Instructors/Index.cshtml* aşağıdaki işaretlemeyle:</span><span class="sxs-lookup"><span data-stu-id="8e97e-223">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="8e97e-224">Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="8e97e-224">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="8e97e-225">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="8e97e-225">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="8e97e-226">`"{id:int?}"` bir rota şablonudur.</span><span class="sxs-lookup"><span data-stu-id="8e97e-226">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="8e97e-227">Rota şablonu tamsayı sorgu dizelerine URL rota verilerini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-227">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="8e97e-228">Örneğin, tıklayarak **seçin** bağlantı için yalnızca bir eğitmen `@page` yönergesi, aşağıdaki gibi bir URL oluşturur:</span><span class="sxs-lookup"><span data-stu-id="8e97e-228">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="8e97e-229">Sayfa yönergesi olduğunda `@page "{id:int?}"`, önceki URL'si:</span><span class="sxs-lookup"><span data-stu-id="8e97e-229">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="8e97e-230">Sayfa başlığı **Eğitmenler**.</span><span class="sxs-lookup"><span data-stu-id="8e97e-230">Page title is **Instructors**.</span></span>
* <span data-ttu-id="8e97e-231">Eklenen bir **Office** görüntüleyen sütun `item.OfficeAssignment.Location` yalnızca `item.OfficeAssignment` null değil.</span><span class="sxs-lookup"><span data-stu-id="8e97e-231">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="8e97e-232">Bu bir sıfır-veya-bir ilişkisi olduğundan, ilgili OfficeAssignment varlık olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-232">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="8e97e-233">Eklenen bir **kursları** kursları görüntüleyen sütun her Eğitmenler tarafından verilen.</span><span class="sxs-lookup"><span data-stu-id="8e97e-233">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="8e97e-234">Bkz: [açık satır geçişle `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) bu razor sözdizimi hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="8e97e-234">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="8e97e-235">Dinamik olarak ekleyen, eklenen kod `class="success"` için `tr` seçili Eğitmen öğesidir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-235">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="8e97e-236">Bu önyükleme sınıfını kullanarak seçili satır için bir arka plan rengini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="8e97e-236">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="8e97e-237">Etiketli yeni köprü eklenmiş **seçin**.</span><span class="sxs-lookup"><span data-stu-id="8e97e-237">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="8e97e-238">Bu bağlantı için seçilen Eğitmen kodu gönderir `Index` yöntemi ve bir arka plan rengini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="8e97e-238">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="8e97e-239">Uygulamayı çalıştırmak ve seçmek **Eğitmenler** sekmesi. Sayfa görüntüler `Location` (office) ilgili gelen `OfficeAssignment` varlık.</span><span class="sxs-lookup"><span data-stu-id="8e97e-239">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="8e97e-240">Varsa OfficeAssignment' olan null, boş bir tablo hücresi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-240">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Hiçbir şey seçili Eğitmenler dizin sayfası](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="8e97e-242">Tıklayarak **seçin** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="8e97e-242">Click on the **Select** link.</span></span> <span data-ttu-id="8e97e-243">Satır stili değişiklikler.</span><span class="sxs-lookup"><span data-stu-id="8e97e-243">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="8e97e-244">Seçili Eğitmenler tarafından verilen derslerimiz Ekle</span><span class="sxs-lookup"><span data-stu-id="8e97e-244">Add courses taught by selected instructor</span></span>

<span data-ttu-id="8e97e-245">Güncelleştirme `OnGetAsync` yönteminde *Pages/Instructors/Index.cshtml.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="8e97e-245">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="8e97e-246">Ekleme `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="8e97e-246">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="8e97e-247">Güncelleştirilen sorguyu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="8e97e-247">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="8e97e-248">Önceki sorgunun ekler `Department` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="8e97e-248">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="8e97e-249">Bir eğitmen seçildiğinde aşağıdaki kodu çalıştırır (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="8e97e-249">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="8e97e-250">Seçili Eğitmen görünüm modeli eğitmenlerini listesi alınır.</span><span class="sxs-lookup"><span data-stu-id="8e97e-250">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="8e97e-251">Görünüm modeli `Courses` özelliğinin yüklü olan `Course` Bu eğitmen varlıklardan `CourseAssignments` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="8e97e-251">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="8e97e-252">`Where` Yöntemi koleksiyonunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="8e97e-252">The `Where` method returns a collection.</span></span> <span data-ttu-id="8e97e-253">Önceki içinde `Where` yöntemi, tek bir `Instructor` varlık döndürülür.</span><span class="sxs-lookup"><span data-stu-id="8e97e-253">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="8e97e-254">`Single` Yöntemi, tek bir koleksiyon dönüştürür `Instructor` varlık.</span><span class="sxs-lookup"><span data-stu-id="8e97e-254">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="8e97e-255">`Instructor` Varlık erişim sağlar `CourseAssignments` özelliği.</span><span class="sxs-lookup"><span data-stu-id="8e97e-255">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="8e97e-256">`CourseAssignments` ilgili erişim sağlayan `Course` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="8e97e-256">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Eğitmen kursları m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="8e97e-258">`Single` Yöntemi yalnızca bir öğe koleksiyonu olduğunda üzerindeki bir koleksiyona kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8e97e-258">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="8e97e-259">`Single` Yöntemi koleksiyonu boş ise veya birden fazla öğe varsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8e97e-259">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="8e97e-260">Alternatif `SingleOrDefault`, hangi varsayılan değeri döndürür (Bu durumda null) koleksiyonu boş ise.</span><span class="sxs-lookup"><span data-stu-id="8e97e-260">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="8e97e-261">Kullanarak `SingleOrDefault` boş bir koleksiyon üzerinde:</span><span class="sxs-lookup"><span data-stu-id="8e97e-261">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="8e97e-262">Bir özel durum oluşur (bulunmaya çalışılırken gelen bir `Courses` null başvuru özelliği).</span><span class="sxs-lookup"><span data-stu-id="8e97e-262">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="8e97e-263">Özel durum iletisi sorunun nedenini daha net bir şekilde gösterir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-263">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="8e97e-264">Aşağıdaki kod görünümü modelin doldurur `Enrollments` kurs seçildiğinde özelliği:</span><span class="sxs-lookup"><span data-stu-id="8e97e-264">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="8e97e-265">Sonuna aşağıdaki işaretlemeyi ekleyin *Pages/Instructors/Index.cshtml* Razor sayfası:</span><span class="sxs-lookup"><span data-stu-id="8e97e-265">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="8e97e-266">Önceki biçimlendirme bir eğitmen seçildiğinde bir eğitmen için ilgili kurslar listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="8e97e-266">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="8e97e-267">Uygulamayı test etme.</span><span class="sxs-lookup"><span data-stu-id="8e97e-267">Test the app.</span></span> <span data-ttu-id="8e97e-268">Tıklayarak bir **seçin** Eğitmenler sayfasında bağlantı.</span><span class="sxs-lookup"><span data-stu-id="8e97e-268">Click on a **Select** link on the instructors page.</span></span>

![Seçili Eğitmenler dizin sayfası Eğitmen](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="8e97e-270">Öğrenci verileri göster</span><span class="sxs-lookup"><span data-stu-id="8e97e-270">Show student data</span></span>

<span data-ttu-id="8e97e-271">Bu bölümde, uygulama, seçilen bir kurs Öğrenci verilerini göstermek için güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-271">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="8e97e-272">Sorguda güncelleştirme `OnGetAsync` yönteminde *Pages/Instructors/Index.cshtml.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="8e97e-272">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="8e97e-273">Güncelleştirme *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8e97e-273">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="8e97e-274">Aşağıdaki biçimlendirmede dosyanın sonuna ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8e97e-274">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="8e97e-275">Önceki biçimlendirme seçili kursun kayıtlı öğrencilere listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="8e97e-275">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="8e97e-276">Sayfayı yenileyin ve bir eğitmen seçin.</span><span class="sxs-lookup"><span data-stu-id="8e97e-276">Refresh the page and select an instructor.</span></span> <span data-ttu-id="8e97e-277">Kayıtlı Öğrenci ve kendi derece listesini görmek için bir kurs seçin.</span><span class="sxs-lookup"><span data-stu-id="8e97e-277">Select a course to see the list of enrolled students and their grades.</span></span>

![Eğitmenler dizin sayfası Eğitmen ve seçilen kursu](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="8e97e-279">Tek kullanma</span><span class="sxs-lookup"><span data-stu-id="8e97e-279">Using Single</span></span>

<span data-ttu-id="8e97e-280">`Single` Yöntemi geçirebilir `Where` koşul çağırmak yerine `Where` yöntemi ayrı olarak:</span><span class="sxs-lookup"><span data-stu-id="8e97e-280">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="8e97e-281">Önceki `Single` yaklaşımı kullanarak üzerinde hiçbir yararları sağlar `Where`.</span><span class="sxs-lookup"><span data-stu-id="8e97e-281">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="8e97e-282">Bazı geliştiriciler tercih `Single` yaklaşımını stili.</span><span class="sxs-lookup"><span data-stu-id="8e97e-282">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="8e97e-283">Açık yükleme</span><span class="sxs-lookup"><span data-stu-id="8e97e-283">Explicit loading</span></span>

<span data-ttu-id="8e97e-284">İstekli yükleme için geçerli kod belirtir `Enrollments` ve `Students`:</span><span class="sxs-lookup"><span data-stu-id="8e97e-284">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="8e97e-285">Kullanıcılar nadiren kayıtları bir kursta görmek istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="8e97e-285">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="8e97e-286">Bu durumda, istenirse yalnızca kayıt verileri yüklemek için bir en iyi duruma getirme olacaktır.</span><span class="sxs-lookup"><span data-stu-id="8e97e-286">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="8e97e-287">Bu bölümde, `OnGetAsync` açık yükleme kullanmak için güncelleştirilmiş `Enrollments` ve `Students`.</span><span class="sxs-lookup"><span data-stu-id="8e97e-287">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="8e97e-288">Güncelleştirme `OnGetAsync` aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="8e97e-288">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="8e97e-289">Yukarıdaki kod bıraktığı *ThenInclude* için verileri kayıt ve Öğrenci yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="8e97e-289">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="8e97e-290">Bir kurs seçtiyseniz vurgulanmış kodu alır:</span><span class="sxs-lookup"><span data-stu-id="8e97e-290">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="8e97e-291">`Enrollment` Seçili kurs için varlıklar.</span><span class="sxs-lookup"><span data-stu-id="8e97e-291">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="8e97e-292">`Student` Varlıklar her `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="8e97e-292">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="8e97e-293">Açıklama çıkış kodu önceki fark `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="8e97e-293">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="8e97e-294">Gezinti özellikleri, izlenen varlıklar için yalnızca bir açıkça yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-294">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="8e97e-295">Uygulamayı test etme.</span><span class="sxs-lookup"><span data-stu-id="8e97e-295">Test the app.</span></span> <span data-ttu-id="8e97e-296">Kullanıcılar açısından bakıldığında, app, önceki sürüme aynı şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="8e97e-296">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="8e97e-297">Sonraki öğreticide, ilgili verileri güncelleştirme işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8e97e-297">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="8e97e-298">[Önceki](xref:data/ef-rp/complex-data-model)
>[İleri](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="8e97e-298">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
