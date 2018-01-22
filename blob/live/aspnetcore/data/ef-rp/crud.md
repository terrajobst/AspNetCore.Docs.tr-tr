---
title: "Razor sayfalarının EF temel - CRUD - 2 8"
author: rick-anderson
description: "Oluşturma, okuma, güncelleştirme, EF çekirdek ile silmek nasıl gösterir"
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/crud
ms.openlocfilehash: c26ba75f6a401d50a6b46bd7ee40500c5736f20f
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a><span data-ttu-id="e3775-103">Oluşturma, okuma, güncelleştirme ve silme - Razor sayfaları (8, 2) ile EF çekirdek</span><span class="sxs-lookup"><span data-stu-id="e3775-103">Create, Read, Update, and Delete - EF Core with Razor Pages (2 of 8)</span></span>

<span data-ttu-id="e3775-104">Tarafından [zel Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e3775-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="e3775-105">Bu öğreticide, kurulmuş CRUD (Oluştur, oku, Güncelleştir, Sil) kodu gözden ve özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="e3775-105">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="e3775-106">Not: karmaşıklık en aza indirmek ve EF çekirdek üzerine odaklanan bu öğreticileri korumak için EF çekirdek kodu Razor sayfalarının arka plan kod dosyalarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e3775-106">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages code-behind files.</span></span> <span data-ttu-id="e3775-107">Bazı geliştiriciler, kullanıcı Arabirimi (Razor sayfaları) ve veri erişim katmanı arasındaki bir Soyutlama Katmanı oluşturmak için bir hizmet katmanı veya depo desende kullanın.</span><span class="sxs-lookup"><span data-stu-id="e3775-107">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="e3775-108">Bu öğretici, oluşturma, düzenleme, silme ve ayrıntıları Razor sayfalarında *Öğrenci* klasörünü değiştirdi.</span><span class="sxs-lookup"><span data-stu-id="e3775-108">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="e3775-109">İskele kurulmuş kod oluşturma, düzenleme ve silme sayfalar için şu biçimi kullanır:</span><span class="sxs-lookup"><span data-stu-id="e3775-109">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="e3775-110">Alma ve HTTP GET yöntemiyle istenen veri görüntüleme `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="e3775-110">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="e3775-111">HTTP POST yöntemi ile verilerde yapılan değişiklikleri kaydetmek `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="e3775-111">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="e3775-112">Dizin ve ayrıntıları sayfaları alma ve HTTP GET yöntemiyle istenen verileri görüntüleme`OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="e3775-112">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="e3775-113">SingleOrDefaultAsync FirstOrDefaultAsync ile değiştir</span><span class="sxs-lookup"><span data-stu-id="e3775-113">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="e3775-114">Oluşturulan kodun kullanan [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) istenen varlığın getirilemedi.</span><span class="sxs-lookup"><span data-stu-id="e3775-114">The generated code uses [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="e3775-115">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) tek bir varlık getirme sırasında daha verimli olur:</span><span class="sxs-lookup"><span data-stu-id="e3775-115">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="e3775-116">Kod doğrulamak gerekmedikçe sorgudan döndürülen birden fazla varlık yok.</span><span class="sxs-lookup"><span data-stu-id="e3775-116">Unless the code needs to verify that there is not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="e3775-117">`SingleOrDefaultAsync`Daha fazla veri getirir ve gereksiz çalışır.</span><span class="sxs-lookup"><span data-stu-id="e3775-117">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="e3775-118">`SingleOrDefaultAsync`Filtre bölümü uyan birden fazla varlık ise özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e3775-118">`SingleOrDefaultAsync` throws an exception if there is more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="e3775-119">`FirstOrDefaultAsync`Filtre bölümü uyan birden fazla varlık ise throw değil.</span><span class="sxs-lookup"><span data-stu-id="e3775-119">`FirstOrDefaultAsync` doesn't throw if there is more than one entity that fits the filter part.</span></span>

<span data-ttu-id="e3775-120">Genel olarak değiştirmek `SingleOrDefaultAsync` ile `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="e3775-120">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="e3775-121">`SingleOrDefaultAsync`5 yerde kullanılır:</span><span class="sxs-lookup"><span data-stu-id="e3775-121">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="e3775-122">`OnGetAsync`Ayrıntılar sayfası.</span><span class="sxs-lookup"><span data-stu-id="e3775-122">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="e3775-123">`OnGetAsync`ve `OnPostAsync` düzenleme ve silme sayfalarında.</span><span class="sxs-lookup"><span data-stu-id="e3775-123">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="e3775-124">FindAsync</span><span class="sxs-lookup"><span data-stu-id="e3775-124">FindAsync</span></span>

<span data-ttu-id="e3775-125">Çok kurulmuş kod [zaman uyumsuz olarak bulur](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) yerine kullanılabilir `FirstOrDefaultAsync` veya `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="e3775-125">In much of the scaffolded code, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="e3775-126">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="e3775-126">`FindAsync`:</span></span>

* <span data-ttu-id="e3775-127">Bir varlığın birincil anahtarının (PK) bulur.</span><span class="sxs-lookup"><span data-stu-id="e3775-127">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="e3775-128">BA sahip bir varlık bağlamı tarafından izleniyorsa, Veritabanına bir isteği bu olmadan döndürülür.</span><span class="sxs-lookup"><span data-stu-id="e3775-128">If an entity with the PK is being tracked by the context, it is returned without a request to the DB.</span></span>
* <span data-ttu-id="e3775-129">Basit ve kısa olur.</span><span class="sxs-lookup"><span data-stu-id="e3775-129">Is simple and concise.</span></span>
* <span data-ttu-id="e3775-130">Tek bir varlık aramak için optimize edilmiştir.</span><span class="sxs-lookup"><span data-stu-id="e3775-130">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="e3775-131">Bazı durumlarda perf avantajlara sahiptir, ancak nadiren normal web senaryoları için oyuna geldikleri.</span><span class="sxs-lookup"><span data-stu-id="e3775-131">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="e3775-132">Örtük olarak kullanan [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) yerine [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="e3775-132">Implicitly uses [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="e3775-133">Ancak, diğer varlıklar dahil etmek istediğiniz sonra bulma artık değil uygun.</span><span class="sxs-lookup"><span data-stu-id="e3775-133">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="e3775-134">Başka bir deyişle, Bul bırakın ve bir sorgu uygulama ilerledikçe taşımak gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="e3775-134">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="e3775-135">Ayrıntılar sayfasını özelleştirme</span><span class="sxs-lookup"><span data-stu-id="e3775-135">Customize the Details page</span></span>

<span data-ttu-id="e3775-136">Gözat `Pages/Students` sayfası.</span><span class="sxs-lookup"><span data-stu-id="e3775-136">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="e3775-137">**Düzenle**, **ayrıntıları**, ve **silmek** bağlantılar tarafından üretilen [yer işareti etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) içinde *sayfaları/Öğrenciler / Index.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="e3775-137">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="e3775-138">Ayrıntıları bağlantısını seçin.</span><span class="sxs-lookup"><span data-stu-id="e3775-138">Select a Details link.</span></span> <span data-ttu-id="e3775-139">URL biçimidir `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="e3775-139">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="e3775-140">Bir sorgu dizesi kullanılarak geçirilmiş Öğrenci Kimliği (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="e3775-140">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="e3775-141">Düzenleme, Ayrıntılar ve Razor Sayfaları Sil kullanmak için güncelleştirme `"{id:int}"` rota şablonu.</span><span class="sxs-lookup"><span data-stu-id="e3775-141">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="e3775-142">Sayfa yönergesi her bu sayfalarından değiştirme `@page` için `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="e3775-142">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="e3775-143">İsteği yapan "{kimliği: int}" rota şablonuyla sayfasına **değil** bir tamsayı rota değeri döndürür bir HTTP 404 (bulunamadı) hata içerir.</span><span class="sxs-lookup"><span data-stu-id="e3775-143">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="e3775-144">Örneğin, `http://localhost:5000/Students/Details` 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="e3775-144">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="e3775-145">Kimliği isteğe bağlı yapmak için ekleme `?` rota kısıtlaması için:</span><span class="sxs-lookup"><span data-stu-id="e3775-145">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="e3775-146">Uygulamayı çalıştırın, bir ayrıntıları bağlantısına tıklayın ve URL rota verilerini kimliği geçirme doğrulayın (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="e3775-146">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="e3775-147">Genel değişiklik yapmayın `@page` için `@page "{id:int}"`, bunu sonları giriş bağlantılara yapılması ve sayfaları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e3775-147">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="e3775-148">İlgili veri ekleme</span><span class="sxs-lookup"><span data-stu-id="e3775-148">Add related data</span></span>

<span data-ttu-id="e3775-149">Öğrenciler dizin sayfası için kurulmuş kodu içermemesi `Enrollments` özelliği.</span><span class="sxs-lookup"><span data-stu-id="e3775-149">The scaffolded code for the Students Index page does not include the `Enrollments` property.</span></span> <span data-ttu-id="e3775-150">Bu bölümde, içeriğini `Enrollments` koleksiyonu Ayrıntılar sayfasında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e3775-150">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="e3775-151">`OnGetAsync` Yöntemi *Pages/Students/Details.cshtml.cs* kullanan `FirstOrDefaultAsync` tek bir alma yöntemi `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="e3775-151">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="e3775-152">Aşağıdaki vurgulanmış kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e3775-152">Add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="e3775-153">`Include` Ve `ThenInclude` yöntemleri neden yüklemek bağlam `Student.Enrollments` gezinti özelliği ve her kayıt içinde `Enrollment.Course` gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="e3775-153">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="e3775-154">Bu ayrıntılı examinied okuma ilgili verileri öğreticide yöntemleridir.</span><span class="sxs-lookup"><span data-stu-id="e3775-154">These methods are examinied in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="e3775-155">`AsNoTracking` Yöntemi varlıkları güncelleştirilmiyor geçerli bağlamda döndürüldüğünde senaryolarda performansı artırır.</span><span class="sxs-lookup"><span data-stu-id="e3775-155">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="e3775-156">`AsNoTracking`Bu öğreticinin ilerleyen bölümlerinde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="e3775-156">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="e3775-157">Ayrıntıları sayfasında ilgili kayıtları görüntüleme</span><span class="sxs-lookup"><span data-stu-id="e3775-157">Display related enrollments on the Details page</span></span>

<span data-ttu-id="e3775-158">Açık *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e3775-158">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="e3775-159">Kayıtları listesini görüntülemek için aşağıdaki vurgulanmış kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e3775-159">Add the following highlighted code to display a list of enrollments:</span></span>

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

<span data-ttu-id="e3775-160">Kod yapıştırılan sonra kod girintileme yanlışsa, düzeltmek için CTRL-K-D tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="e3775-160">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="e3775-161">Önceki kod varlıklarda döngü `Enrollments` gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="e3775-161">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="e3775-162">Her kayıt için indirmelere başlık ve sınıf görüntüler.</span><span class="sxs-lookup"><span data-stu-id="e3775-162">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="e3775-163">İndirmelere başlık depolanan indirmelere varlık alınır `Course` kayıtları varlık gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="e3775-163">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="e3775-164">Uygulama, belirleyin **Öğrenciler** sekmesine ve tıklayın **ayrıntıları** Öğrenci için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="e3775-164">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="e3775-165">Kurslar ve seçili Öğrenci dereceleri listesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e3775-165">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="e3775-166">Güncelleştirme Oluştur sayfası</span><span class="sxs-lookup"><span data-stu-id="e3775-166">Update the Create page</span></span>

<span data-ttu-id="e3775-167">Güncelleştirme `OnPostAsync` yönteminde *Pages/Students/Create.cshtml.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="e3775-167">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="e3775-168">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="e3775-168">TryUpdateModelAsync</span></span>

<span data-ttu-id="e3775-169">İncelemek [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) kod:</span><span class="sxs-lookup"><span data-stu-id="e3775-169">Examine the [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="e3775-170">Önceki kod `TryUpdateModelAsync<Student>` güncelleştirmeyi dener `emptyStudent` gönderilen form değerleri kullanarak nesne [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) özelliğinde [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="e3775-170">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="e3775-171">`TryUpdateModelAsync`Yalnızca listelenen özellikler güncelleştirmeleri (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="e3775-171">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="e3775-172">Önceki örnekte:</span><span class="sxs-lookup"><span data-stu-id="e3775-172">In the preceding sample:</span></span>

* <span data-ttu-id="e3775-173">İkinci bağımsız değişken (` "student", // Prefix`) öneki değerleri aramak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e3775-173">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="e3775-174">Büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="e3775-174">It's not case sensitive.</span></span>
* <span data-ttu-id="e3775-175">Gönderilen form değerleri türler dönüştürülür `Student` kullanarak model [model bağlama](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="e3775-175">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="e3775-176">Overposting</span><span class="sxs-lookup"><span data-stu-id="e3775-176">Overposting</span></span>

<span data-ttu-id="e3775-177">Kullanarak `TryUpdateModel` overposting engellediğinden alanları gönderilen değerlerle güncelleştirmek için bir güvenlik en iyi uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="e3775-177">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="e3775-178">Örneğin, Öğrenci varlık içerdiğini varsayın bir `Secret` bu web sayfası güncelleştirme ekleyin veya değil özelliği:</span><span class="sxs-lookup"><span data-stu-id="e3775-178">For example, suppose the Student entity includes a `Secret` property that this web page should not update or add:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="e3775-179">Uygulama olmasa dahi bir `Secret` oluşturma/güncelleştirme Razor sayfasını, bir bilgisayar korsanının alan kümesi `Secret` overposting tarafından değeri.</span><span class="sxs-lookup"><span data-stu-id="e3775-179">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="e3775-180">Bir bilgisayar korsanının Fiddler gibi bir araç kullanın veya göndermek için bazı JavaScript Yazma bir `Secret` form değeri.</span><span class="sxs-lookup"><span data-stu-id="e3775-180">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="e3775-181">Özgün kod Öğrenci örneğini oluşturduğunda, model Bağlayıcısı kullandığını alanları sınırlamak değil.</span><span class="sxs-lookup"><span data-stu-id="e3775-181">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="e3775-182">Ne olursa olsun değer için belirtilen korsan `Secret` form alanı DB'de güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e3775-182">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="e3775-183">Fiddler aracı ekleme aşağıdaki görüntüde gösterir `Secret` gönderilen form değerlerine alanı ("OverPost" değeriyle).</span><span class="sxs-lookup"><span data-stu-id="e3775-183">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Fiddler ekleme gizli alan](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="e3775-185">"OverPost" eklenen başarıyla değeri `Secret` eklenen satırın özelliği.</span><span class="sxs-lookup"><span data-stu-id="e3775-185">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="e3775-186">Hiçbir zaman amaçlı Uygulama Tasarımcısı `Secret` Oluştur sayfası ayarlanacak özelliği.</span><span class="sxs-lookup"><span data-stu-id="e3775-186">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="e3775-187">Görünüm modeli</span><span class="sxs-lookup"><span data-stu-id="e3775-187">View model</span></span>

<span data-ttu-id="e3775-188">Bir görünüm modeli genellikle uygulama tarafından kullanılan modelinde bulunan özellikler kümesini içerir.</span><span class="sxs-lookup"><span data-stu-id="e3775-188">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="e3775-189">Uygulama modeli genellikle etki alanı modeli adı verilir.</span><span class="sxs-lookup"><span data-stu-id="e3775-189">The application model is often called the domain model.</span></span> <span data-ttu-id="e3775-190">Etki alanı modeli genellikle veritabanında karşılık gelen varlığın gerektirdiği tüm özellikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="e3775-190">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="e3775-191">Görünüm modeli yalnızca UI katmanında (örneğin, Oluştur sayfası) için gereken özellikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="e3775-191">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="e3775-192">Görünüm modeli ek olarak, bazı uygulamalar, Razor sayfalarının arka plandaki kod sınıfı ve tarayıcı arasında veri iletmek için bir bağlama modelini veya giriş modeli kullanın.</span><span class="sxs-lookup"><span data-stu-id="e3775-192">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages code-behind class and the browser.</span></span> <span data-ttu-id="e3775-193">Aşağıdakileri göz önünde bulundurun `Student` görünüm modeli:</span><span class="sxs-lookup"><span data-stu-id="e3775-193">Consider the following `Student` view model:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="e3775-194">Görünüm modelleri overposting önlemek için alternatif bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="e3775-194">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="e3775-195">Görünüm modeli (Görüntüle) görüntülemek veya güncelleştirmek için yalnızca özellikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="e3775-195">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="e3775-196">Aşağıdaki kod `StudentVM` görünüm modeli yeni Öğrenci oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="e3775-196">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="e3775-197">[SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) yöntemi diğerinden değerleri okuyarak bu nesnenin değerlerini ayarlar [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) nesnesi.</span><span class="sxs-lookup"><span data-stu-id="e3775-197">The [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="e3775-198">`SetValues`özellik adıyla eşleşen kullanır.</span><span class="sxs-lookup"><span data-stu-id="e3775-198">`SetValues` uses property name matching.</span></span> <span data-ttu-id="e3775-199">Görünüm model türü için model türü ilgili gerekmez, bu yalnızca eşleşen özelliklere sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e3775-199">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="e3775-200">Kullanarak `StudentVM` gerektirir [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) kullanacak şekilde güncelleştirilmesi `StudentVM` yerine `Student`.</span><span class="sxs-lookup"><span data-stu-id="e3775-200">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="e3775-201">Razor sayfalarında `PageModel` türetilmiş sınıf, görünüm modeli.</span><span class="sxs-lookup"><span data-stu-id="e3775-201">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="e3775-202">Güncelleştirmeyi Düzenle sayfası</span><span class="sxs-lookup"><span data-stu-id="e3775-202">Update the Edit page</span></span>

<span data-ttu-id="e3775-203">Düzen sayfası arka plan kodu dosyasını güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="e3775-203">Update the Edit page code-behind file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="e3775-204">Kod değişiklikleri Oluştur sayfası birkaç istisna dışında benzerdir:</span><span class="sxs-lookup"><span data-stu-id="e3775-204">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="e3775-205">`OnPostAsync`İsteğe bağlı bir sahip `id` parametresi.</span><span class="sxs-lookup"><span data-stu-id="e3775-205">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="e3775-206">Geçerli Öğrenci DB'den getirilen boş Öğrenci oluşturmak yerine.</span><span class="sxs-lookup"><span data-stu-id="e3775-206">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="e3775-207">`FirstOrDefaultAsync`ile değiştirilmiştir [zaman uyumsuz olarak bulur](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="e3775-207">`FirstOrDefaultAsync` has been replaced with [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="e3775-208">`FindAsync`bir varlığın birincil anahtardan seçerken olduğunda iyi bir seçimdir.</span><span class="sxs-lookup"><span data-stu-id="e3775-208">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="e3775-209">Bkz: [zaman uyumsuz olarak bulur](#FindAsync) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="e3775-209">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="e3775-210">Düzen sınamak ve sayfa oluşturma</span><span class="sxs-lookup"><span data-stu-id="e3775-210">Test the Edit and Create pages</span></span>

<span data-ttu-id="e3775-211">Oluşturun ve birkaç Öğrenci varlıklar düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="e3775-211">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="e3775-212">Varlık durumları</span><span class="sxs-lookup"><span data-stu-id="e3775-212">Entity States</span></span>

<span data-ttu-id="e3775-213">Veritabanı bağlamını varlıkları bellekte DB karşılık gelen satırlarında ile eşit olup olmadığını izler.</span><span class="sxs-lookup"><span data-stu-id="e3775-213">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="e3775-214">DB bağlamı eşitleme bilgileri ne olacağını belirler, `SaveChanges` olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="e3775-214">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="e3775-215">Örneğin, ne zaman yeni bir varlık iletilir `Add` varlığın durumu kümesine yöntemi, `Added`.</span><span class="sxs-lookup"><span data-stu-id="e3775-215">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="e3775-216">Zaman `SaveChanges` çağrılır, DB bağlamı sorunları SQL INSERT komutu.</span><span class="sxs-lookup"><span data-stu-id="e3775-216">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="e3775-217">Bir varlık şu durumlardan birinde olabilir:</span><span class="sxs-lookup"><span data-stu-id="e3775-217">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="e3775-218">`Added`: Varlık DB'de henüz yok.</span><span class="sxs-lookup"><span data-stu-id="e3775-218">`Added`: The entity does not yet exist in the DB.</span></span> <span data-ttu-id="e3775-219">`SaveChanges` Yöntemi INSERT deyimi verir.</span><span class="sxs-lookup"><span data-stu-id="e3775-219">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="e3775-220">`Unchanged`: Hiçbir değişiklik bu varlıkla kaydedilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="e3775-220">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="e3775-221">DB'den okurken bir varlık bu durum vardır.</span><span class="sxs-lookup"><span data-stu-id="e3775-221">An entity has this status when it is read from the DB.</span></span>

* <span data-ttu-id="e3775-222">`Modified`: Bazılarını veya tümünü varlığın özellik değerlerini değiştirilmiş.</span><span class="sxs-lookup"><span data-stu-id="e3775-222">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="e3775-223">`SaveChanges` Yöntemi bir güncelleştirme deyimi sorunları.</span><span class="sxs-lookup"><span data-stu-id="e3775-223">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="e3775-224">`Deleted`: Varlık silinmek üzere işaretlendi.</span><span class="sxs-lookup"><span data-stu-id="e3775-224">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="e3775-225">`SaveChanges` Yöntemi DELETE deyimi verir.</span><span class="sxs-lookup"><span data-stu-id="e3775-225">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="e3775-226">`Detached`: Varlık DB bağlamı tarafından izleniyor değil.</span><span class="sxs-lookup"><span data-stu-id="e3775-226">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="e3775-227">Bir masaüstü uygulamasının durumu değişiklikleri genellikle otomatik olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="e3775-227">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="e3775-228">Bir varlık okuma, değişiklik yapıldıysa ve otomatik olarak değiştirilmesi varlık durumu `Modified`.</span><span class="sxs-lookup"><span data-stu-id="e3775-228">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="e3775-229">Çağırma `SaveChanges` yalnızca değiştirilen özellikler güncelleştirmeleri SQL UPDATE deyimi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e3775-229">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="e3775-230">Bir web uygulamasında `DbContext` bir varlık ve verileri bir sayfa oluşturulduğunda atıldı görüntüler okur.</span><span class="sxs-lookup"><span data-stu-id="e3775-230">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="e3775-231">Bir sayfa zaman `OnPostAsync` yöntemi çağrıldığında, yeni bir web istek yapıldığında ve yeni bir örneğini ile `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="e3775-231">When a pages `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="e3775-232">Bu yeni bağlam varlıkta yeniden okuma Masaüstü işleme benzetimini yapar.</span><span class="sxs-lookup"><span data-stu-id="e3775-232">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="e3775-233">Güncelleştirme Sil sayfası</span><span class="sxs-lookup"><span data-stu-id="e3775-233">Update the Delete page</span></span>

<span data-ttu-id="e3775-234">Bu bölümde, bir özel hata uygulamak için kod eklenir ne zaman ileti çağrısı `SaveChanges` başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="e3775-234">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="e3775-235">Olası hata iletilerini içeren bir dize ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e3775-235">Add a string to contain possible error messages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="e3775-236">Değiştir `OnGetAsync` aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="e3775-236">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="e3775-237">Önceki kod isteğe bağlı bir parametre içeren `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="e3775-237">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="e3775-238">`saveChangesError`yöntem Öğrenci nesnesini silmek için bir hatadan sonra çağrıldı olup olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="e3775-238">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="e3775-239">Silme işlemi, geçici ağ sorunları nedeniyle başarısız olabilir.</span><span class="sxs-lookup"><span data-stu-id="e3775-239">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="e3775-240">Geçici ağ hataları bulutta daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="e3775-240">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="e3775-241">`saveChangesError`yanlış olduğunda Delete sayfa `OnGetAsync` kullanıcı Arabiriminden olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="e3775-241">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="e3775-242">Zaman `OnGetAsync` tarafından çağrılır `OnPostAsync` (silme işlemi başarısız oldu çünkü), `saveChangesError` parametresi doğrudur.</span><span class="sxs-lookup"><span data-stu-id="e3775-242">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="e3775-243">Delete sayfaları OnPostAsync yöntemi</span><span class="sxs-lookup"><span data-stu-id="e3775-243">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="e3775-244">Değiştir `OnPostAsync` aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="e3775-244">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="e3775-245">Seçilen varlığın önceki kod alır sonra çağırır `Remove` varlığın durumu ayarlamak için yöntemi `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="e3775-245">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="e3775-246">Zaman `SaveChanges` çağrılır SQL DELETE komutu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e3775-246">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="e3775-247">Varsa `Remove` başarısız olur:</span><span class="sxs-lookup"><span data-stu-id="e3775-247">If `Remove` fails:</span></span>

* <span data-ttu-id="e3775-248">DB özel durum yakalandı.</span><span class="sxs-lookup"><span data-stu-id="e3775-248">The DB exception is caught.</span></span>
* <span data-ttu-id="e3775-249">Delete sayfaları `OnGetAsync` yöntemi ile çağrılır `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="e3775-249">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="e3775-250">Delete Razor sayfasını güncelleştir</span><span class="sxs-lookup"><span data-stu-id="e3775-250">Update the Delete Razor Page</span></span>

<span data-ttu-id="e3775-251">Aşağıdaki vurgulanan hata iletisini silme Razor sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e3775-251">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="e3775-252">Delete sınayın.</span><span class="sxs-lookup"><span data-stu-id="e3775-252">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="e3775-253">Sık karşılaşılan hataları</span><span class="sxs-lookup"><span data-stu-id="e3775-253">Common errors</span></span>

<span data-ttu-id="e3775-254">Öğrenci/Home veya diğer bağlantılar çalışmıyor:</span><span class="sxs-lookup"><span data-stu-id="e3775-254">Student/Home or other links don't work:</span></span>

<span data-ttu-id="e3775-255">Razor sayfasını içeren doğru doğrulayın `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="e3775-255">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="e3775-256">Örneğin, Öğrenci/giriş Razor sayfa gereken **değil** rota şablonu içerir:</span><span class="sxs-lookup"><span data-stu-id="e3775-256">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="e3775-257">Her Razor sayfasını içermelidir `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="e3775-257">Each Razor Page must include the `@page` directive.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e3775-258">[Önceki](xref:data/ef-rp/intro)
[sonraki](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="e3775-258">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
