---
title: Razor sayfalarının ASP.NET Core - CRUD - 2 8'in EF çekirdek ile
author: rick-anderson
description: Oluşturma, okuma, güncelleştirme, EF çekirdek ile silmek nasıl gösterir
ms.author: riande
ms.date: 10/15/2017
uid: data/ef-rp/crud
ms.openlocfilehash: 157257d10306ded3456cd66c186a82edf0ba5d65
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961327"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a><span data-ttu-id="7c5ba-103">Razor sayfalarının ASP.NET Core - CRUD - 2 8'in EF çekirdek ile</span><span class="sxs-lookup"><span data-stu-id="7c5ba-103">Razor Pages with EF Core in ASP.NET Core - CRUD - 2 of 8</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="7c5ba-104">Bu öğretici ASP.NET Core 2.0 sürümünü bulunabilir [bu PDF dosyası](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/PDF-6-18-18.pdf).</span><span class="sxs-lookup"><span data-stu-id="7c5ba-104">The ASP.NET Core 2.0 version of this tutorial can be found in [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/PDF-6-18-18.pdf).</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7c5ba-105">Tarafından [zel Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7c5ba-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="7c5ba-106">Bu öğreticide, kurulmuş CRUD (Oluştur, oku, Güncelleştir, Sil) kodu gözden ve özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-106">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="7c5ba-107">Karmaşıklık en aza indirmek ve EF çekirdek üzerine odaklanan bu öğreticileri korumak için EF çekirdek kod sayfası modellerinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-107">To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the page models.</span></span> <span data-ttu-id="7c5ba-108">Bazı geliştiriciler, kullanıcı Arabirimi (Razor sayfaları) ve veri erişim katmanı arasındaki bir Soyutlama Katmanı oluşturmak için bir hizmet katmanı veya depo desende kullanın.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-108">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="7c5ba-109">Bu öğretici, oluşturma, düzenleme, silme ve ayrıntıları Razor sayfalarında *Öğrenci* klasörü incelenmesini.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-109">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are examined.</span></span>

<span data-ttu-id="7c5ba-110">İskele kurulmuş kod oluşturma, düzenleme ve silme sayfalar için şu biçimi kullanır:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-110">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="7c5ba-111">Alma ve HTTP GET yöntemiyle istenen veri görüntüleme `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-111">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="7c5ba-112">HTTP POST yöntemi ile verilerde yapılan değişiklikleri kaydetmek `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-112">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="7c5ba-113">Dizin ve ayrıntıları sayfaları alma ve HTTP GET yöntemiyle istenen verileri görüntüleme `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="7c5ba-113">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="singleordefaultasync-vs-firstordefaultasync"></a><span data-ttu-id="7c5ba-114">SingleOrDefaultAsync vs FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="7c5ba-114">SingleOrDefaultAsync vs FirstOrDefaultAsync</span></span>

<span data-ttu-id="7c5ba-115">Oluşturulan kodun kullanan [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), üzerinden genellikle tercih edilen olduğu [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="7c5ba-115">The generated code uses [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), which is generally preferred over [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>

 <span data-ttu-id="7c5ba-116">`FirstOrDefaultAsync` ' den daha etkilidir `SingleOrDefaultAsync` tek bir varlık getirme sırasında:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-116">`FirstOrDefaultAsync` is more efficient than `SingleOrDefaultAsync` at fetching one entity:</span></span>

* <span data-ttu-id="7c5ba-117">Kod doğrulamak gerekmedikçe sorgudan döndürülen birden fazla varlık yok.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-117">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span>
* <span data-ttu-id="7c5ba-118">`SingleOrDefaultAsync` Daha fazla veri getirir ve gereksiz çalışır.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-118">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="7c5ba-119">`SingleOrDefaultAsync` Filtre bölümü uyan birden fazla varlık ise özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-119">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
* <span data-ttu-id="7c5ba-120">`FirstOrDefaultAsync` Filtre bölümü uyan birden fazla varlık ise throw değil.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-120">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="7c5ba-121">Zaman uyumsuz olarak bulur</span><span class="sxs-lookup"><span data-stu-id="7c5ba-121">FindAsync</span></span>

<span data-ttu-id="7c5ba-122">Çok kurulmuş kod [zaman uyumsuz olarak bulur](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) yerine kullanılan `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-122">In much of the scaffolded code, [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync`.</span></span>

<span data-ttu-id="7c5ba-123">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-123">`FindAsync`:</span></span>

* <span data-ttu-id="7c5ba-124">Bir varlığın birincil anahtarının (PK) bulur.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-124">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="7c5ba-125">BA sahip bir varlık bağlamı tarafından izleniyorsa, Veritabanına bir isteği bu olmadan döndürülür.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-125">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="7c5ba-126">Basit ve kısa olur.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-126">Is simple and concise.</span></span>
* <span data-ttu-id="7c5ba-127">Tek bir varlık aramak için optimize edilmiştir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-127">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="7c5ba-128">Bazı durumlarda perf avantajlara sahiptir, ancak nadiren normal web uygulamaları için oyuna geldikleri.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-128">Can have perf benefits in some situations, but they rarely come into play for typical web apps.</span></span>
* <span data-ttu-id="7c5ba-129">Örtük olarak kullanan [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) yerine [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="7c5ba-129">Implicitly uses [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="7c5ba-130">Ancak isterseniz `Include` diğer varlıklar, ardından `FindAsync` artık uygun değil.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-130">But if you want to `Include` other entities, then `FindAsync` is no longer appropriate.</span></span> <span data-ttu-id="7c5ba-131">Bu abandon gerekebilir anlamına gelir `FindAsync` ve bir sorgu uygulama ilerledikçe taşıyın.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-131">This means that you may need to abandon `FindAsync` and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="7c5ba-132">Ayrıntılar sayfasını özelleştirme</span><span class="sxs-lookup"><span data-stu-id="7c5ba-132">Customize the Details page</span></span>

<span data-ttu-id="7c5ba-133">Gözat `Pages/Students` sayfası.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-133">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="7c5ba-134">**Düzenle**, **ayrıntıları**, ve **silmek** bağlantılar tarafından üretilen [yer işareti etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) içinde *sayfaları/Öğrenciler / Index.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-134">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Index1.cshtml?name=snippet)]

<span data-ttu-id="7c5ba-135">Uygulamayı çalıştırın ve seçin bir **ayrıntıları** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-135">Run the app and select a **Details** link.</span></span> <span data-ttu-id="7c5ba-136">URL biçimidir `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-136">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="7c5ba-137">Bir sorgu dizesi kullanılarak geçirilmiş Öğrenci Kimliği (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="7c5ba-137">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="7c5ba-138">Düzenleme, Ayrıntılar ve Razor Sayfaları Sil kullanmak için güncelleştirme `"{id:int}"` rota şablonu.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-138">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="7c5ba-139">Sayfa yönergesi her bu sayfalarından değiştirme `@page` için `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-139">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="7c5ba-140">İsteği yapan "{kimliği: int}" rota şablonuyla sayfasına **değil** bir tamsayı rota değeri döndürür bir HTTP 404 (bulunamadı) hata içerir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-140">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="7c5ba-141">Örneğin, `http://localhost:5000/Students/Details` 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-141">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="7c5ba-142">Kimliği isteğe bağlı yapmak için ekleme `?` rota kısıtlaması için:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-142">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="7c5ba-143">Uygulamayı çalıştırın, bir ayrıntıları bağlantısına tıklayın ve URL rota verilerini kimliği geçirme doğrulayın (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="7c5ba-143">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="7c5ba-144">Genel değişiklik yapmayın `@page` için `@page "{id:int}"`, bunu sonları giriş bağlantılara yapılması ve sayfaları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-144">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="7c5ba-145">İlgili veri ekleme</span><span class="sxs-lookup"><span data-stu-id="7c5ba-145">Add related data</span></span>

<span data-ttu-id="7c5ba-146">Öğrenciler dizin sayfası için kurulmuş kod içermeyen `Enrollments` özelliği.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-146">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="7c5ba-147">Bu bölümde, içeriğini `Enrollments` koleksiyonu Ayrıntılar sayfasında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-147">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="7c5ba-148">`OnGetAsync` Yöntemi *Pages/Students/Details.cshtml.cs* kullanan `FirstOrDefaultAsync` tek bir alma yöntemi `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-148">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="7c5ba-149">Aşağıdaki vurgulanmış kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-149">Add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="7c5ba-150">[INCLUDE](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) ve [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) yöntemleri neden yüklemek bağlam `Student.Enrollments` gezinti özelliği ve her kayıt içinde `Enrollment.Course` gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-150">The [Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) and [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="7c5ba-151">Bu yöntemleri ayrıntılı olarak okuma ilgili verileri öğreticide incelenir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-151">These methods are examined in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="7c5ba-152">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) yöntemi varlıkları güncelleştirilmiyor geçerli bağlamda döndürüldüğünde senaryolarda performansı artırır.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-152">The [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="7c5ba-153">`AsNoTracking` Bu öğreticinin ilerleyen bölümlerinde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-153">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="7c5ba-154">Ayrıntıları sayfasında ilgili kayıtları görüntüleme</span><span class="sxs-lookup"><span data-stu-id="7c5ba-154">Display related enrollments on the Details page</span></span>

<span data-ttu-id="7c5ba-155">Açık *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-155">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="7c5ba-156">Kayıtları listesini görüntülemek için aşağıdaki vurgulanmış kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-156">Add the following highlighted code to display a list of enrollments:</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Details.cshtml?highlight=32-53)]

<span data-ttu-id="7c5ba-157">Kod yapıştırılan sonra kod girintileme yanlışsa, düzeltmek için CTRL-K-D tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-157">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="7c5ba-158">Önceki kod varlıklarda döngü `Enrollments` gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-158">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="7c5ba-159">Her kayıt için indirmelere başlık ve sınıf görüntüler.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-159">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="7c5ba-160">İndirmelere başlık depolanan indirmelere varlık alınır `Course` kayıtları varlık gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-160">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="7c5ba-161">Uygulama, belirleyin **Öğrenciler** sekmesine ve tıklayın **ayrıntıları** Öğrenci için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-161">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="7c5ba-162">Kurslar ve seçili Öğrenci dereceleri listesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-162">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="7c5ba-163">Güncelleştirme Oluştur sayfası</span><span class="sxs-lookup"><span data-stu-id="7c5ba-163">Update the Create page</span></span>

<span data-ttu-id="7c5ba-164">Güncelleştirme `OnPostAsync` yönteminde *Pages/Students/Create.cshtml.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-164">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="7c5ba-165">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="7c5ba-165">TryUpdateModelAsync</span></span>

<span data-ttu-id="7c5ba-166">İncelemek [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) kod:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-166">Examine the [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="7c5ba-167">Önceki kod `TryUpdateModelAsync<Student>` güncelleştirmeyi dener `emptyStudent` gönderilen form değerleri kullanarak nesne [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) özelliğinde [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span><span class="sxs-lookup"><span data-stu-id="7c5ba-167">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span></span> <span data-ttu-id="7c5ba-168">`TryUpdateModelAsync` Yalnızca listelenen özellikler güncelleştirmeleri (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="7c5ba-168">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="7c5ba-169">Önceki örnekte:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-169">In the preceding sample:</span></span>

* <span data-ttu-id="7c5ba-170">İkinci bağımsız değişken (` "student", // Prefix`) öneki değerleri aramak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-170">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="7c5ba-171">Büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-171">It's not case sensitive.</span></span>
* <span data-ttu-id="7c5ba-172">Gönderilen form değerleri türler dönüştürülür `Student` kullanarak model [model bağlama](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="7c5ba-172">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="7c5ba-173">Overposting</span><span class="sxs-lookup"><span data-stu-id="7c5ba-173">Overposting</span></span>

<span data-ttu-id="7c5ba-174">Kullanarak `TryUpdateModel` overposting engellediğinden alanları gönderilen değerlerle güncelleştirmek için bir güvenlik en iyi uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-174">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="7c5ba-175">Örneğin, Öğrenci varlık içerdiğini varsayın bir `Secret` , bu web sayfası ekleme veya döndürmemelidir güncelleştirme özelliği:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-175">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="7c5ba-176">Uygulama olmasa dahi bir `Secret` oluşturma/güncelleştirme Razor sayfasını, bir bilgisayar korsanının alan kümesi `Secret` overposting tarafından değeri.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-176">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="7c5ba-177">Bir bilgisayar korsanının Fiddler gibi bir araç kullanın veya göndermek için bazı JavaScript Yazma bir `Secret` form değeri.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-177">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="7c5ba-178">Özgün kod Öğrenci örneğini oluşturduğunda, model Bağlayıcısı kullandığını alanları sınırlamak değil.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-178">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="7c5ba-179">Ne olursa olsun değer için belirtilen korsan `Secret` form alanı DB'de güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-179">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="7c5ba-180">Fiddler aracı ekleme aşağıdaki görüntüde gösterir `Secret` gönderilen form değerlerine alanı ("OverPost" değeriyle).</span><span class="sxs-lookup"><span data-stu-id="7c5ba-180">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Fiddler ekleme gizli alan](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="7c5ba-182">"OverPost" eklenen başarıyla değeri `Secret` eklenen satırın özelliği.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-182">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="7c5ba-183">Hiçbir zaman amaçlı Uygulama Tasarımcısı `Secret` Oluştur sayfası ayarlanacak özelliği.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-183">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>

### <a name="view-model"></a><span data-ttu-id="7c5ba-184">Görünüm modeli</span><span class="sxs-lookup"><span data-stu-id="7c5ba-184">View model</span></span>

<span data-ttu-id="7c5ba-185">Bir görünüm modeli genellikle uygulama tarafından kullanılan modelinde bulunan özellikler kümesini içerir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-185">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="7c5ba-186">Uygulama modeli genellikle etki alanı modeli adı verilir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-186">The application model is often called the domain model.</span></span> <span data-ttu-id="7c5ba-187">Etki alanı modeli genellikle veritabanında karşılık gelen varlığın gerektirdiği tüm özellikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-187">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="7c5ba-188">Görünüm modeli yalnızca UI katmanında (örneğin, Oluştur sayfası) için gereken özellikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-188">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="7c5ba-189">Görünüm modeli ek olarak, bazı uygulamalar, Razor sayfalarının sayfa model sınıfı ve tarayıcı arasında veri iletmek için bir bağlama modelini veya giriş modeli kullanın.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-189">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="7c5ba-190">Aşağıdakileri göz önünde bulundurun `Student` görünüm modeli:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-190">Consider the following `Student` view model:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentVM.cs)]

<span data-ttu-id="7c5ba-191">Görünüm modelleri overposting önlemek için alternatif bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-191">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="7c5ba-192">Görünüm modeli (Görüntüle) görüntülemek veya güncelleştirmek için yalnızca özellikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-192">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="7c5ba-193">Aşağıdaki kod `StudentVM` görünüm modeli yeni Öğrenci oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-193">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="7c5ba-194">[SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) yöntemi diğerinden değerleri okuyarak bu nesnenin değerlerini ayarlar [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) nesnesi.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-194">The [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="7c5ba-195">`SetValues` özellik adıyla eşleşen kullanır.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-195">`SetValues` uses property name matching.</span></span> <span data-ttu-id="7c5ba-196">Görünüm model türü için model türü ilgili gerekmez, bu yalnızca eşleşen özelliklere sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-196">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="7c5ba-197">Kullanarak `StudentVM` gerektirir [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) kullanacak şekilde güncelleştirilmesi `StudentVM` yerine `Student`.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-197">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="7c5ba-198">Razor sayfalarında `PageModel` türetilmiş sınıf, görünüm modeli.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-198">In Razor Pages, the `PageModel` derived class is the view model.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="7c5ba-199">Güncelleştirmeyi Düzenle sayfası</span><span class="sxs-lookup"><span data-stu-id="7c5ba-199">Update the Edit page</span></span>

<span data-ttu-id="7c5ba-200">Düzen sayfası için sayfa modeli güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-200">Update the page model for the Edit page.</span></span> <span data-ttu-id="7c5ba-201">Önemli değişiklikler vurgulanmıştır:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-201">The major changes are highlighted:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="7c5ba-202">Kod değişiklikleri Oluştur sayfası birkaç istisna dışında benzerdir:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-202">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="7c5ba-203">`OnPostAsync` İsteğe bağlı bir sahip `id` parametresi.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-203">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="7c5ba-204">Geçerli Öğrenci DB'den getirilen boş Öğrenci oluşturmak yerine.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-204">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="7c5ba-205">`FirstOrDefaultAsync` ile değiştirilmiştir [zaman uyumsuz olarak bulur](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span><span class="sxs-lookup"><span data-stu-id="7c5ba-205">`FirstOrDefaultAsync` has been replaced with [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span></span> <span data-ttu-id="7c5ba-206">`FindAsync` bir varlığın birincil anahtardan seçerken olduğunda iyi bir seçimdir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-206">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="7c5ba-207">Bkz: [zaman uyumsuz olarak bulur](#FindAsync) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-207">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="7c5ba-208">Düzen sınamak ve sayfa oluşturma</span><span class="sxs-lookup"><span data-stu-id="7c5ba-208">Test the Edit and Create pages</span></span>

<span data-ttu-id="7c5ba-209">Oluşturun ve birkaç Öğrenci varlıklar düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-209">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="7c5ba-210">Varlık durumları</span><span class="sxs-lookup"><span data-stu-id="7c5ba-210">Entity States</span></span>

<span data-ttu-id="7c5ba-211">Veritabanı bağlamını varlıkları bellekte DB karşılık gelen satırlarında ile eşit olup olmadığını izler.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-211">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="7c5ba-212">DB bağlamı eşitleme bilgileri ne olacağını belirler zaman [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-212">The DB context sync information determines what happens when [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span> <span data-ttu-id="7c5ba-213">Örneğin, ne zaman yeni bir varlık iletilir [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) varlığın durumu kümesine yöntemi, [eklenen](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span><span class="sxs-lookup"><span data-stu-id="7c5ba-213">For example, when a new entity is passed to the [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) method, that entity's state is set to [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span></span> <span data-ttu-id="7c5ba-214">Zaman `SaveChangesAsync` çağrılır, DB bağlamı sorunları SQL INSERT komutu.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-214">When `SaveChangesAsync` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="7c5ba-215">Bir varlık birinde olabilir [durumları aşağıdaki](/dotnet/api/microsoft.entityframeworkcore.entitystate):</span><span class="sxs-lookup"><span data-stu-id="7c5ba-215">An entity may be in one of the [following states](/dotnet/api/microsoft.entityframeworkcore.entitystate):</span></span>

* <span data-ttu-id="7c5ba-216">`Added`: Varlık DB'de henüz yok.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-216">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="7c5ba-217">`SaveChanges` Yöntemi INSERT deyimi verir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-217">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="7c5ba-218">`Unchanged`: Hiçbir değişiklik bu varlıkla kaydedilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-218">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="7c5ba-219">DB'den okurken bir varlık bu durum vardır.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-219">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="7c5ba-220">`Modified`: Bazılarını veya tümünü varlığın özellik değerlerini değiştirilmiş.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-220">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="7c5ba-221">`SaveChanges` Yöntemi bir güncelleştirme deyimi sorunları.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-221">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="7c5ba-222">`Deleted`: Varlık silinmek üzere işaretlendi.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-222">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="7c5ba-223">`SaveChanges` Yöntemi DELETE deyimi verir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-223">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="7c5ba-224">`Detached`: Varlık DB bağlamı tarafından izleniyor değil.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-224">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="7c5ba-225">Bir masaüstü uygulamasının durumu değişiklikleri genellikle otomatik olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-225">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="7c5ba-226">Bir varlık okuma, değişiklik yapıldıysa ve otomatik olarak değiştirilmesi varlık durumu `Modified`.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-226">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="7c5ba-227">Çağırma `SaveChanges` yalnızca değiştirilen özellikler güncelleştirmeleri SQL UPDATE deyimi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-227">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="7c5ba-228">Bir web uygulamasında `DbContext` bir varlık ve verileri bir sayfa oluşturulduğunda atıldı görüntüler okur.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-228">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="7c5ba-229">Bir sayfa zaman 's `OnPostAsync` yöntemi çağrıldığında, yeni bir web istek yapıldığında ve yeni bir örneğini ile `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-229">When a page's `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="7c5ba-230">Bu yeni bağlam varlıkta yeniden okuma Masaüstü işleme benzetimini yapar.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-230">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="7c5ba-231">Güncelleştirme Sil sayfası</span><span class="sxs-lookup"><span data-stu-id="7c5ba-231">Update the Delete page</span></span>

<span data-ttu-id="7c5ba-232">Bu bölümde, bir özel hata uygulamak için kod eklenir ne zaman ileti çağrısı `SaveChanges` başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-232">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="7c5ba-233">Olası hata iletilerini içeren bir dize ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-233">Add a string to contain possible error messages:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="7c5ba-234">Değiştir `OnGetAsync` aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-234">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="7c5ba-235">Önceki kod isteğe bağlı bir parametre içeren `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-235">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="7c5ba-236">`saveChangesError` yöntem Öğrenci nesnesini silmek için bir hatadan sonra çağrıldı olup olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-236">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="7c5ba-237">Silme işlemi, geçici ağ sorunları nedeniyle başarısız olabilir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-237">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="7c5ba-238">Geçici ağ hataları bulutta daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-238">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="7c5ba-239">`saveChangesError`yanlış olduğunda Delete sayfa `OnGetAsync` kullanıcı Arabiriminden olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-239">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="7c5ba-240">Zaman `OnGetAsync` tarafından çağrılır `OnPostAsync` (silme işlemi başarısız oldu çünkü), `saveChangesError` parametresi doğrudur.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-240">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="7c5ba-241">Delete sayfaları OnPostAsync yöntemi</span><span class="sxs-lookup"><span data-stu-id="7c5ba-241">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="7c5ba-242">Değiştir `OnPostAsync` aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-242">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="7c5ba-243">Seçilen varlığın önceki kod alır sonra çağırır [kaldırmak](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) varlığın durumu ayarlamak için yöntemi `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-243">The preceding code retrieves the selected entity, then calls the [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="7c5ba-244">Zaman `SaveChanges` çağrılır SQL DELETE komutu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-244">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="7c5ba-245">Varsa `Remove` başarısız olur:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-245">If `Remove` fails:</span></span>

* <span data-ttu-id="7c5ba-246">DB özel durum yakalandı.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-246">The DB exception is caught.</span></span>
* <span data-ttu-id="7c5ba-247">Delete sayfaları `OnGetAsync` yöntemi ile çağrılır `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-247">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="7c5ba-248">Delete Razor sayfasını güncelleştir</span><span class="sxs-lookup"><span data-stu-id="7c5ba-248">Update the Delete Razor Page</span></span>

<span data-ttu-id="7c5ba-249">Aşağıdaki vurgulanan hata iletisini silme Razor sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-249">Add the following highlighted error message to the Delete Razor Page.</span></span>
<!--
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?name=snippet&highlight=11)]
-->
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="7c5ba-250">Delete sınayın.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-250">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="7c5ba-251">Sık karşılaşılan hataları</span><span class="sxs-lookup"><span data-stu-id="7c5ba-251">Common errors</span></span>

<span data-ttu-id="7c5ba-252">Öğrenci/Home veya diğer bağlantılar çalışmıyor:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-252">Student/Home or other links don't work:</span></span>

<span data-ttu-id="7c5ba-253">Razor sayfasını içeren doğru doğrulayın `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-253">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="7c5ba-254">Örneğin, Öğrenci/giriş Razor sayfa gereken **değil** rota şablonu içerir:</span><span class="sxs-lookup"><span data-stu-id="7c5ba-254">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="7c5ba-255">Her Razor sayfasını içermelidir `@page` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="7c5ba-255">Each Razor Page must include the `@page` directive.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="7c5ba-256">[Önceki](xref:data/ef-rp/intro)
> [sonraki](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="7c5ba-256">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
