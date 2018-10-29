---
title: ASP.NET core'da - EF çekirdekli Razor sayfaları, ilgili verileri - 7, 8 güncelleştirme
author: rick-anderson
description: Bu öğreticide yabancı anahtar alanları ve gezinti özellikleri güncelleştirerek ilgili verileri güncelleştirin.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 4306118240c052585a5c2eeb2053ce03534b547c
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207549"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a><span data-ttu-id="d1d9f-103">ASP.NET core'da - EF çekirdekli Razor sayfaları, ilgili verileri - 7, 8 güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="d1d9f-103">Razor Pages with EF Core in ASP.NET Core - Update Related Data - 7 of 8</span></span>

<span data-ttu-id="d1d9f-104">Tarafından [Tom Dykstra](https://github.com/tdykstra), ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d1d9f-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="d1d9f-105">Bu öğreticide, ilgili verileri güncelleştirme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="d1d9f-106">Olamaz çözmenize, sorunlarla karşılaşırsanız, [indirin veya tamamlanmış uygulamayı görüntüleyin.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="d1d9f-106">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="d1d9f-107">[Yükleme yönergeleri](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d1d9f-107">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="d1d9f-108">Aşağıdaki çizimler tamamlanmış sayfaların bazılarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-108">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="d1d9f-109">![Kurs düzenleme sayfası](update-related-data/_static/course-edit.png)
![sayfasını Eğitmen Düzenle](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="d1d9f-109">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="d1d9f-110">İnceleyin ve oluşturma ve düzenleme kurs sayfaları test.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-110">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="d1d9f-111">Yeni bir kurs oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-111">Create a new course.</span></span> <span data-ttu-id="d1d9f-112">Departman birincil anahtara göre (tamsayı) adını seçilir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-112">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="d1d9f-113">Yeni kursu düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-113">Edit the new course.</span></span> <span data-ttu-id="d1d9f-114">Testi bitirdikten sonra yeni kurs silin.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-114">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="d1d9f-115">Ortak kod paylaşmak için bir temel sınıf oluşturma</span><span class="sxs-lookup"><span data-stu-id="d1d9f-115">Create a base class to share common code</span></span>

<span data-ttu-id="d1d9f-116">Kursları/oluşturma ve kursları/düzenleme sayfaları her departman adlarının bir listesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-116">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="d1d9f-117">Oluşturma *Pages/Courses/DepartmentNamePageModel.cshtml.cs* oluşturma ve düzenleme sayfaları için temel sınıf:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-117">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="d1d9f-118">Yukarıdaki kod oluşturur bir [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) bölüm adları listesi içerecek.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-118">The preceding code creates a [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="d1d9f-119">Varsa `selectedDepartment` belirtilirse, bu bölümün seçili `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-119">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="d1d9f-120">Sayfa modeli sınıfları oluşturma ve düzenleme türetilmesi `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-120">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="d1d9f-121">Kursları sayfalarını özelleştirme</span><span class="sxs-lookup"><span data-stu-id="d1d9f-121">Customize the Courses Pages</span></span>

<span data-ttu-id="d1d9f-122">Yeni bir kurs varlık oluşturulduğunda, var olan bir bölüm arasında bir ilişki olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-122">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="d1d9f-123">Bir kurs oluştururken bir bölüm eklemek için departmanı seçmek için aşağı açılan liste oluşturma ve düzenleme için temel sınıfı içerir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-123">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="d1d9f-124">Açılır listede kümeleri `Course.DepartmentID` yabancı anahtar (FK) özelliği.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-124">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="d1d9f-125">EF Core kullanan `Course.DepartmentID` yüklenecek FK `Department` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-125">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Kurs oluşturma](update-related-data/_static/ddl.png)

<span data-ttu-id="d1d9f-127">Oluşturma sayfa modeli aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-127">Update the Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

<span data-ttu-id="d1d9f-128">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-128">The preceding code:</span></span>

* <span data-ttu-id="d1d9f-129">Öğesinden türetilen `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-129">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="d1d9f-130">Kullanan `TryUpdateModelAsync` önlemek için [overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="d1d9f-130">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="d1d9f-131">Değiştirir `ViewData["DepartmentID"]` ile `DepartmentNameSL` (temel sınıftan).</span><span class="sxs-lookup"><span data-stu-id="d1d9f-131">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="d1d9f-132">`ViewData["DepartmentID"]` değiştirilir kesin olarak belirlenmiş ile `DepartmentNameSL`.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-132">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="d1d9f-133">Kesin olarak belirlenmiş Model üzerinden zayıf yazılmış tercih edilen.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-133">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="d1d9f-134">Daha fazla bilgi için [verileri (ViewData ve ViewBag)'zayıf yazılmış](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="d1d9f-134">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="d1d9f-135">Kurslar oluşturmaları sayfası</span><span class="sxs-lookup"><span data-stu-id="d1d9f-135">Update the Courses Create page</span></span>

<span data-ttu-id="d1d9f-136">Güncelleştirme *Pages/Courses/Create.cshtml* aşağıdaki işaretlemeyle:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-136">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="d1d9f-137">Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-137">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="d1d9f-138">Resim yazısı gelen değişiklikleri **DepartmentID** için **departmanı**.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-138">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="d1d9f-139">Değiştirir `"ViewBag.DepartmentID"` ile `DepartmentNameSL` (temel sınıftan).</span><span class="sxs-lookup"><span data-stu-id="d1d9f-139">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="d1d9f-140">"Bölüm seçin" seçeneği ekler.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-140">Adds the "Select Department" option.</span></span> <span data-ttu-id="d1d9f-141">Bu değişiklik, "Bölüm seçin" yerine ilk bölümü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-141">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="d1d9f-142">Departman seçili değilse, doğrulama iletisi ekler.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-142">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="d1d9f-143">Razor sayfası kullanan [seçin etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="d1d9f-143">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="d1d9f-144">Test oluşturma sayfası.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-144">Test the Create page.</span></span> <span data-ttu-id="d1d9f-145">Bölüm kimliği yerine bölüm adını Oluştur sayfasında görüntüler.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-145">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="d1d9f-146">Kursları Düzenle sayfasında güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-146">Update the Courses Edit page.</span></span>

<span data-ttu-id="d1d9f-147">Düzenleme sayfa modeli aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-147">Update the edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

<span data-ttu-id="d1d9f-148">Değişiklikleri Oluştur sayfası modelinde yapılan benzerdir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-148">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="d1d9f-149">Önceki kodda, `PopulateDepartmentsDropDownList` , aşağı açılan listesinde belirtilen bölüm seçin bölüm kimliği, geçirir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-149">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="d1d9f-150">Güncelleştirme *Pages/Courses/Edit.cshtml* aşağıdaki işaretlemeyle:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-150">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="d1d9f-151">Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-151">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="d1d9f-152">Kurs kimliğini görüntüler</span><span class="sxs-lookup"><span data-stu-id="d1d9f-152">Displays the course ID.</span></span> <span data-ttu-id="d1d9f-153">Genellikle birincil anahtarı (PK), bir varlığın görüntülenmiyor.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-153">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="d1d9f-154">Kullanıcılar genellikle anlamsız BA.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-154">PKs are usually meaningless to users.</span></span> <span data-ttu-id="d1d9f-155">Bu durumda, PK kurs sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-155">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="d1d9f-156">Resim yazısı gelen değişiklikleri **DepartmentID** için **departmanı**.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-156">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="d1d9f-157">Değiştirir `"ViewBag.DepartmentID"` ile `DepartmentNameSL` (temel sınıftan).</span><span class="sxs-lookup"><span data-stu-id="d1d9f-157">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="d1d9f-158">Sayfa gizli bir alan içeriyor (`<input type="hidden">`) kurs numarası.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-158">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="d1d9f-159">Ekleme bir `<label>` Yardımcıyla etiketi `asp-for="Course.CourseID"` gizli alan gerekmemesi değil.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-159">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="d1d9f-160">`<input type="hidden">` kullanıcı tıkladığında, deftere nakledilen verileri eklenmesi için kurs numarası gereklidir **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-160">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="d1d9f-161">Güncelleştirilen kodun test edin.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-161">Test the updated code.</span></span> <span data-ttu-id="d1d9f-162">Oluşturun, düzenleyin ve bir kurs silin.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-162">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="d1d9f-163">Ayrıntılara AsNoTracking ekleme ve silme sayfası modelleri</span><span class="sxs-lookup"><span data-stu-id="d1d9f-163">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="d1d9f-164">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) izleme zorunlu olmayan durumlarda performansını iyileştirebilir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-164">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="d1d9f-165">Ekleme `AsNoTracking` silin ve Ayrıntıları sayfası modeli.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-165">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="d1d9f-166">Aşağıdaki kod, güncelleştirilmiş silme sayfa modeli gösterir:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-166">The following code shows the updated Delete page model:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="d1d9f-167">Güncelleştirme `OnGetAsync` yönteminde *Pages/Courses/Details.cshtml.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-167">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="d1d9f-168">Silme ve ayrıntıları sayfalarını değiştirin</span><span class="sxs-lookup"><span data-stu-id="d1d9f-168">Modify the Delete and Details pages</span></span>

<span data-ttu-id="d1d9f-169">Güncelleştirme aşağıdaki işaretlemeyle Sil Razor sayfası:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-169">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="d1d9f-170">Ayrıntıları sayfasına aynı değişiklikleri yapın.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-170">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="d1d9f-171">Kurs sayfaları test</span><span class="sxs-lookup"><span data-stu-id="d1d9f-171">Test the Course pages</span></span>

<span data-ttu-id="d1d9f-172">Test oluşturma, Ayrıntılar, düzenleme ve silme.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-172">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="d1d9f-173">Eğitmen sayfaları güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="d1d9f-173">Update the instructor pages</span></span>

<span data-ttu-id="d1d9f-174">Aşağıdaki bölümlerde, eğitmen sayfaları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-174">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="d1d9f-175">Ofis konumu Ekle</span><span class="sxs-lookup"><span data-stu-id="d1d9f-175">Add office location</span></span>

<span data-ttu-id="d1d9f-176">Bir eğitmen kaydı düzenlerken, eğitmen ofis ataması güncelleştirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-176">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="d1d9f-177">`Instructor` Varlığı ile bir sıfır-veya-bir ilişkisi vardır `OfficeAssignment` varlık.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-177">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="d1d9f-178">Eğitmen kodu işlemesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-178">The instructor code must handle:</span></span>

* <span data-ttu-id="d1d9f-179">Kullanıcının office atama temizler, silin `OfficeAssignment` varlık.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-179">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="d1d9f-180">Kullanıcı bir ofis ataması girer ve boş, yeni bir oluşturma `OfficeAssignment` varlık.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-180">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="d1d9f-181">Kullanıcı bir ofis ataması değişirse, güncelleştirme `OfficeAssignment` varlık.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-181">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="d1d9f-182">Eğitmenler düzenleme sayfa modeli aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-182">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

<span data-ttu-id="d1d9f-183">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-183">The preceding code:</span></span>

- <span data-ttu-id="d1d9f-184">Geçerli alır `Instructor` istekli yükleme için kullanarak veritabanını bir varlıktan `OfficeAssignment` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-184">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="d1d9f-185">Alınan güncelleştirmeler `Instructor` varlık değerlerle model bağlayıcı.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-185">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="d1d9f-186">`TryUpdateModel` engeller [overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="d1d9f-186">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="d1d9f-187">Ofis konumu boş ise, ayarlar `Instructor.OfficeAssignment` null.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-187">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="d1d9f-188">Zaman `Instructor.OfficeAssignment` null, ilgili satırdaki olan `OfficeAssignment` tablo silinir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-188">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="d1d9f-189">Eğitmen düzenleme sayfası</span><span class="sxs-lookup"><span data-stu-id="d1d9f-189">Update the instructor Edit page</span></span>

<span data-ttu-id="d1d9f-190">Güncelleştirme *Pages/Instructors/Edit.cshtml* office konum:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-190">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="d1d9f-191">Bir eğitmen ofis konumu değiştirebilirsiniz doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-191">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="d1d9f-192">Kurs atamaları Eğitmen Düzen sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="d1d9f-192">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="d1d9f-193">Eğitmenler kursları herhangi bir sayıda öğretin.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-193">Instructors may teach any number of courses.</span></span> <span data-ttu-id="d1d9f-194">Bu bölümde, kurs atamalarını değiştirme özelliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-194">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="d1d9f-195">Aşağıdaki görüntüde, güncelleştirilmiş Eğitmen düzenleme sayfası gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-195">The following image shows the updated instructor Edit page:</span></span>

![Eğitmen kurslarıyla düzenleme sayfası](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="d1d9f-197">`Course` ve `Instructor` bir çok-çok ilişkisi vardır.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-197">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="d1d9f-198">Ekleme ve ilişkilerini kaldırmak için ekleyip varlıklardan `CourseAssignments` katılın varlık kümesi.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-198">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="d1d9f-199">Değişiklikler bir eğitmen atandığı kursları onay kutularını işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-199">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="d1d9f-200">Her kurs sonunda verilen veritabanında bir onay kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-200">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="d1d9f-201">Eğitmen atandığı kursları denetlenir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-201">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="d1d9f-202">Kullanıcı seçin veya kurs atamaları değiştirmek için onay kutularının işaretini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-202">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="d1d9f-203">Kursları sayısı çok büyük ise:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-203">If the number of courses were much greater:</span></span>

* <span data-ttu-id="d1d9f-204">Kursları görüntülemek için muhtemelen farklı bir kullanıcı arabiriminin kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-204">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="d1d9f-205">Bir birleştirme varlığı oluşturma veya ilişkileri silme düzenleme yöntemi değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-205">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="d1d9f-206">Destekleyecek şekilde sınıfları eklemek oluşturma ve düzenleme Eğitmen sayfaları</span><span class="sxs-lookup"><span data-stu-id="d1d9f-206">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="d1d9f-207">Oluşturma *SchoolViewModels/AssignedCourseData.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-207">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="d1d9f-208">`AssignedCourseData` Sınıfı bir eğitmen tarafından atanan Kurslar için onay kutularını oluşturmak için veri içerir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-208">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="d1d9f-209">Oluşturma *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* temel sınıfı:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-209">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="d1d9f-210">`InstructorCoursesPageModel` Temel sınıf için düzenleme kullanacağınızı ve sayfa modeller oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-210">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="d1d9f-211">`PopulateAssignedCourseData` tüm `Course` doldurmak için varlıklar `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-211">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="d1d9f-212">Her kurs için kod ayarlar `CourseID`, başlık ve alınıp alınmayacağını Eğitmen kursa atanır.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-212">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="d1d9f-213">A [HashSet](/dotnet/api/system.collections.generic.hashset-1) verimli aramaları oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-213">A [HashSet](/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="d1d9f-214">Eğitmenler düzenleme sayfa modeli</span><span class="sxs-lookup"><span data-stu-id="d1d9f-214">Instructors Edit page model</span></span>

<span data-ttu-id="d1d9f-215">Eğitmen düzenleme sayfa modeli aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-215">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

<span data-ttu-id="d1d9f-216">Yukarıdaki kod, office atama değişikliklerinin işler.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-216">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="d1d9f-217">Razor görünümü Eğitmen güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-217">Update the instructor Razor View:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="d1d9f-218">Visual Studio'da kod yapıştırdığınızda, satır sonları kodları keser şekilde değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-218">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="d1d9f-219">CTRL + Z, otomatik biçimlendirme geri almak için bir kez basın.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-219">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="d1d9f-220">Burada gördüğünüz gibi görünürler, Ctrl + Z satır sonları düzeltir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-220">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="d1d9f-221">Girinti mükemmel, olması gerekmez ancak `@</tr><tr>`, `@:<td>`, `@:</td>`, ve `@:</tr>` satırları her tek bir satırda gösterilen gibi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-221">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="d1d9f-222">Seçili yeni kod bloğu ile sekmesindeki yeni kodu mevcut kodu ile hizalamak için üç kez basın.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-222">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="d1d9f-223">Oy kullanın veya bu hatanın durumunu gözden [bu bağlantıyla](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="d1d9f-223">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="d1d9f-224">Yukarıdaki kod, üç sütuna sahip bir HTML tablosu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-224">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="d1d9f-225">Her sütun, bir onay kutusu ve kursu sayısını ve başlığını içeren bir başlık sahiptir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-225">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="d1d9f-226">Tüm onay kutularını ("selectedCourses") aynı ada sahip.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-226">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="d1d9f-227">Aynı adı kullanarak bir grup olarak değerlendirilecek model bağlayıcı bildirir.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-227">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="d1d9f-228">Her onay kutusunun değer özniteliği kümesine `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-228">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="d1d9f-229">Sayfa gönderildiğinde, model bağlayıcı oluşan bir dizi iletir `CourseID` yalnızca seçilen onay kutularına değerleri.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-229">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="d1d9f-230">Onay kutularını başlangıçta işlendiğinde, eğitmen için atanan kursları öznitelikleri kullanıma.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-230">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="d1d9f-231">Uygulamayı çalıştırın ve güncelleştirilmiş Eğitmenler düzenleme sayfası test edin.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-231">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="d1d9f-232">Bazı kurs atamalarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-232">Change some course assignments.</span></span> <span data-ttu-id="d1d9f-233">Dizin sayfasında değişiklikler yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-233">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="d1d9f-234">Not: Burada Eğitmen kurs verileri düzenlemek için uygulanan yaklaşıma de sınırlı sayıda kursları olduğunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-234">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="d1d9f-235">Çok daha büyük olan koleksiyonları için farklı bir kullanıcı Arabirimi ve farklı bir güncelleştirme yöntemi daha kullanılabilir ve daha verimli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-235">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="d1d9f-236">Eğitmenler Oluştur sayfası</span><span class="sxs-lookup"><span data-stu-id="d1d9f-236">Update the instructors Create page</span></span>

<span data-ttu-id="d1d9f-237">Eğitmen Oluştur sayfa modeli aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-237">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="d1d9f-238">Yukarıdaki kod benzer *Pages/Instructors/Edit.cshtml.cs* kod.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-238">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="d1d9f-239">Eğitmen oluşturma Razor sayfası aşağıdaki biçimlendirme güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-239">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="d1d9f-240">Eğitmen Oluştur sayfasında test edin.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-240">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="d1d9f-241">Silme sayfası</span><span class="sxs-lookup"><span data-stu-id="d1d9f-241">Update the Delete page</span></span>

<span data-ttu-id="d1d9f-242">Delete sayfa modeli aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-242">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

<span data-ttu-id="d1d9f-243">Yukarıdaki kod, aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="d1d9f-243">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="d1d9f-244">İstekli yükleme için kullandığı `CourseAssignments` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-244">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="d1d9f-245">`CourseAssignments` dahil edilmesi gereken veya Eğitmen silindiğinde silinmez.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-245">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="d1d9f-246">Onları okumanıza gerek kalmadan için veritabanını art arda silme yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-246">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="d1d9f-247">Eğitmen silinecek tüm bölümlerin bir yönetici olarak atanmış ise bu bölümlerden Eğitmen atama kaldırır.</span><span class="sxs-lookup"><span data-stu-id="d1d9f-247">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d1d9f-248">[Önceki](xref:data/ef-rp/read-related-data)
> [İleri](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="d1d9f-248">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
