---
title: ASP.NET Core ile EF Core Razor Pages-Ilgili verileri güncelleştirme-7/8
author: tdykstra
description: Bu öğreticide, yabancı anahtar alanlarını ve gezinti özelliklerini güncelleştirerek ilgili verileri güncelleyerek.
ms.author: riande
ms.date: 07/22/2019
uid: data/ef-rp/update-related-data
ms.openlocfilehash: bc237cf928d852b92c5c1984527129404f88018d
ms.sourcegitcommit: 257cc3fe8c1d61341aa3b07e5bc0fa3d1c1c1d1c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69583501"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a><span data-ttu-id="35bfd-103">ASP.NET Core ile EF Core Razor Pages-Ilgili verileri güncelleştirme-7/8</span><span class="sxs-lookup"><span data-stu-id="35bfd-103">Razor Pages with EF Core in ASP.NET Core - Update Related Data - 7 of 8</span></span>

<span data-ttu-id="35bfd-104">, [Tom Dykstra](https://github.com/tdykstra)ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="35bfd-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="35bfd-105">Bu öğreticide ilgili verileri güncelleştirme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-105">This tutorial shows how to update related data.</span></span> <span data-ttu-id="35bfd-106">Aşağıdaki çizimler tamamlanan sayfalardan bazılarını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-106">The following illustrations show some of the completed pages.</span></span>

<span data-ttu-id="35bfd-107">![Kurs düzenleme sayfası](update-related-data/_static/course-edit30.png)
![eğitmeni düzenleme sayfası](update-related-data/_static/instructor-edit-courses30.png)</span><span class="sxs-lookup"><span data-stu-id="35bfd-107">![Course Edit page](update-related-data/_static/course-edit30.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses30.png)</span></span>

## <a name="update-the-course-create-and-edit-pages"></a><span data-ttu-id="35bfd-108">Kurs oluşturma ve düzenleme sayfalarını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="35bfd-108">Update the Course Create and Edit pages</span></span>

<span data-ttu-id="35bfd-109">Kurs oluşturma ve düzenleme sayfaları için yapı iskelesi kodu, departman KIMLIĞI (tamsayı) gösteren bir departman açılan listesi içerir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-109">The scaffolded code for the Course Create and Edit pages has a Department drop-down list that shows Department ID (an integer).</span></span> <span data-ttu-id="35bfd-110">Açılan listede bölüm adı gösterilmeli, bu nedenle her iki sayfanın da bir departman adları listesi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-110">The drop-down should show the Department name, so both of these pages need a list of department names.</span></span> <span data-ttu-id="35bfd-111">Bu listeyi sağlamak için, oluşturma ve düzenleme sayfaları için bir temel sınıf kullanın.</span><span class="sxs-lookup"><span data-stu-id="35bfd-111">To provide that list, use a base class for the Create and Edit pages.</span></span>

### <a name="create-a-base-class-for-course-create-and-edit"></a><span data-ttu-id="35bfd-112">Kurs oluşturma ve düzenleme için bir temel sınıf oluşturun</span><span class="sxs-lookup"><span data-stu-id="35bfd-112">Create a base class for Course Create and Edit</span></span>

<span data-ttu-id="35bfd-113">Aşağıdaki kodla bir *Pages/kurslar/DepartmentNamePageModel. cs* dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="35bfd-113">Create a *Pages/Courses/DepartmentNamePageModel.cs* file with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/DepartmentNamePageModel.cs)]

<span data-ttu-id="35bfd-114">Yukarıdaki kod, bölüm adlarının listesini içeren bir [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="35bfd-114">The preceding code creates a [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="35bfd-115">Belirtilmişse, bu departman `SelectList`öğesinde seçilir. `selectedDepartment`</span><span class="sxs-lookup"><span data-stu-id="35bfd-115">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="35bfd-116">Oluşturma ve düzenleme sayfa modeli sınıfları öğesinden `DepartmentNamePageModel`türetilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-116">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

### <a name="update-the-course-create-page-model"></a><span data-ttu-id="35bfd-117">Kursu güncelleştirme sayfa modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="35bfd-117">Update the Course Create page model</span></span>

<span data-ttu-id="35bfd-118">Bir kurs bir departmana atanır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-118">A Course is assigned to a Department.</span></span> <span data-ttu-id="35bfd-119">Oluşturma ve düzenleme sayfaları için temel sınıf, departmanı seçmek için `SelectList` bir sağlar.</span><span class="sxs-lookup"><span data-stu-id="35bfd-119">The base class for the Create and Edit pages provides a `SelectList` for selecting the department.</span></span> <span data-ttu-id="35bfd-120">Yabancı anahtar (FK) özelliğini kullanan `SelectList` açılan liste. `Course.DepartmentID`</span><span class="sxs-lookup"><span data-stu-id="35bfd-120">The drop-down list that uses the `SelectList` sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="35bfd-121">EF Core, `Course.DepartmentID` `Department` gezinti özelliğini yüklemek için FK kullanır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-121">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Kurs oluştur](update-related-data/_static/ddl30.png)

<span data-ttu-id="35bfd-123">*Sayfaları/kursları/oluşturma. cshtml. cs* dosyasını şu kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-123">Update *Pages/Courses/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Create.cshtml.cs?highlight=7,18,27-41)]

<span data-ttu-id="35bfd-124">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="35bfd-124">The preceding code:</span></span>

* <span data-ttu-id="35bfd-125">Türetiliyor `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="35bfd-125">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="35bfd-126">`TryUpdateModelAsync` [Aşırı nakletmeyi](xref:data/ef-rp/crud#overposting)engellemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-126">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="35bfd-127">Kaldırır `ViewData["DepartmentID"]`.</span><span class="sxs-lookup"><span data-stu-id="35bfd-127">Removes `ViewData["DepartmentID"]`.</span></span> <span data-ttu-id="35bfd-128">`DepartmentNameSL`temel sınıftan türü kesin belirlenmiş bir modeldir ve Razor sayfası tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-128">`DepartmentNameSL` from the base class is a strongly typed model and will be used by the Razor page.</span></span> <span data-ttu-id="35bfd-129">Kesin olarak belirlenmiş modeller, kesin olarak yazılan zayıf bir şekilde tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-129">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="35bfd-130">Daha fazla bilgi için bkz. [zayıf yazılmış veriler (ViewData ve ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="35bfd-130">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-course-create-razor-page"></a><span data-ttu-id="35bfd-131">Kursu güncelleştirme Razor oluşturma sayfası</span><span class="sxs-lookup"><span data-stu-id="35bfd-131">Update the Course Create Razor page</span></span>

<span data-ttu-id="35bfd-132">*Sayfaları/kursları/Create. cshtml* 'yi aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-132">Update *Pages/Courses/Create.cshtml* with the following code:</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="35bfd-133">Yukarıdaki kod aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="35bfd-133">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="35bfd-134">**DepartmentID** etiketini **departmana**dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="35bfd-134">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="35bfd-135">`"ViewBag.DepartmentID"` İle`DepartmentNameSL` değiştirir (taban sınıfından).</span><span class="sxs-lookup"><span data-stu-id="35bfd-135">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="35bfd-136">"Departmanı Seç" seçeneğini ekler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-136">Adds the "Select Department" option.</span></span> <span data-ttu-id="35bfd-137">Bu değişiklik, ilk departman yerine henüz bir departman seçilmedikçe açılan kutuda "departmanı Seç" i işler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-137">This change renders "Select Department" in the drop-down when no department has been selected yet, rather than the first department.</span></span>
* <span data-ttu-id="35bfd-138">Departman seçili olmadığında bir doğrulama iletisi ekler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-138">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="35bfd-139">Razor sayfası [seçme etiketi yardımcısını](xref:mvc/views/working-with-forms#the-select-tag-helper)kullanır:</span><span class="sxs-lookup"><span data-stu-id="35bfd-139">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="35bfd-140">Oluştur sayfasını test edin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-140">Test the Create page.</span></span> <span data-ttu-id="35bfd-141">Oluştur sayfası departman KIMLIĞI yerine departman adını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-141">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-course-edit-page-model"></a><span data-ttu-id="35bfd-142">Kurs düzenleme sayfası modelini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="35bfd-142">Update the Course Edit page model</span></span>

<span data-ttu-id="35bfd-143">*Pages/kurslar/Edit. cshtml. cs* dosyasını aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-143">Update *Pages/Courses/Edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40-66)]

<span data-ttu-id="35bfd-144">Değişiklikler, oluşturma sayfası modelinde yapılanlarla benzerdir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-144">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="35bfd-145">Yukarıdaki kodda, `PopulateDepartmentsDropDownList` bu departmanı açılan listede seçen departman kimliği ' nde geçirilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-145">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which selects that department in the drop-down list.</span></span>

### <a name="update-the-course-edit-razor-page"></a><span data-ttu-id="35bfd-146">Kurs düzenleme Razor sayfasını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="35bfd-146">Update the Course Edit Razor page</span></span>

<span data-ttu-id="35bfd-147">*Pages/kurslar/Edit. cshtml* dosyasını aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-147">Update *Pages/Courses/Edit.cshtml* with the following code:</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="35bfd-148">Yukarıdaki kod aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="35bfd-148">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="35bfd-149">Kurs KIMLIĞINI görüntüler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-149">Displays the course ID.</span></span> <span data-ttu-id="35bfd-150">Genellikle bir varlığın birincil anahtarı (PK) gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="35bfd-150">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="35bfd-151">PKs 'ler genellikle kullanıcılara daha az anlamlı olur.</span><span class="sxs-lookup"><span data-stu-id="35bfd-151">PKs are usually meaningless to users.</span></span> <span data-ttu-id="35bfd-152">Bu durumda, PK kurs numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-152">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="35bfd-153">Bölüm açılan başlığını **DepartmentID** ' dan **departmana**dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="35bfd-153">Changes the caption for the Department drop-down from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="35bfd-154">`"ViewBag.DepartmentID"` İle`DepartmentNameSL` değiştirir (taban sınıfından).</span><span class="sxs-lookup"><span data-stu-id="35bfd-154">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="35bfd-155">Sayfa, kurs numarası için gizli bir`<input type="hidden">`alan () içerir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-155">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="35bfd-156">`<label>` Etiket`asp-for="Course.CourseID"` Yardımcısı ekleme, gizli alan gereksinimini ortadan kaldırmaz.</span><span class="sxs-lookup"><span data-stu-id="35bfd-156">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="35bfd-157">`<input type="hidden">`Kullanıcı **Kaydet**' e tıkladığında, gönderilen veriler için kurs numarasının dahil olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-157">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

## <a name="update-the-course-details-and-delete-pages"></a><span data-ttu-id="35bfd-158">Kurs ayrıntılarını güncelleştirme ve sayfaları silme</span><span class="sxs-lookup"><span data-stu-id="35bfd-158">Update the Course Details and Delete pages</span></span>

<span data-ttu-id="35bfd-159">[Anotracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) , izleme gerekli olmadığında performansı iyileştirebilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-159">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span>

### <a name="update-the-course-page-models"></a><span data-ttu-id="35bfd-160">Kurs sayfası modellerini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="35bfd-160">Update the Course page models</span></span>

<span data-ttu-id="35bfd-161">*Sayfa/kurslar/delete. cshtml. cs* öğesini aşağıdaki kodla `AsNoTracking`güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-161">Update *Pages/Courses/Delete.cshtml.cs* with the following code to add `AsNoTracking`:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Delete.cshtml.cs?highlight=29)]

<span data-ttu-id="35bfd-162">*Sayfalar/kurslar/details. cshtml. cs* dosyasında aynı değişikliği yapın:</span><span class="sxs-lookup"><span data-stu-id="35bfd-162">Make the same change in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Details.cshtml.cs?highlight=28)]

### <a name="update-the-course-razor-pages"></a><span data-ttu-id="35bfd-163">Kurs Razor sayfalarını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="35bfd-163">Update the Course Razor pages</span></span>

<span data-ttu-id="35bfd-164">*Pages/kurslar/delete. cshtml* dosyasını aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-164">Update *Pages/Courses/Delete.cshtml* with the following code:</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Delete.cshtml?highlight=15-20,37)]

<span data-ttu-id="35bfd-165">Ayrıntılar sayfasında aynı değişiklikleri yapın.</span><span class="sxs-lookup"><span data-stu-id="35bfd-165">Make the same changes to the Details page.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Details.cshtml?highlight=14-19,36)]

## <a name="test-the-course-pages"></a><span data-ttu-id="35bfd-166">Kurs sayfalarını test etme</span><span class="sxs-lookup"><span data-stu-id="35bfd-166">Test the Course pages</span></span>

<span data-ttu-id="35bfd-167">Oluşturma, düzenleme, ayrıntıları ve silme sayfalarını test edin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-167">Test the create, edit, details, and delete pages.</span></span>

## <a name="update-the-instructor-create-and-edit-pages"></a><span data-ttu-id="35bfd-168">Eğitmen oluşturma ve düzenleme sayfalarını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="35bfd-168">Update the instructor Create and Edit pages</span></span>

<span data-ttu-id="35bfd-169">Eğitmenler, istediğiniz sayıda kurs öğretebilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-169">Instructors may teach any number of courses.</span></span> <span data-ttu-id="35bfd-170">Aşağıdaki görüntüde, bir dizi kurs onay kutusu ile eğitmen düzenleme sayfası gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-170">The following image shows the instructor Edit page with an array of course checkboxes.</span></span>

![Kurslar ile eğitmen düzenleme sayfası](update-related-data/_static/instructor-edit-courses30.png)

<span data-ttu-id="35bfd-172">Onay kutuları, bir eğitmenin atandığı kurslara değişiklikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="35bfd-172">The checkboxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="35bfd-173">Veritabanındaki her kurs için bir onay kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-173">A checkbox is displayed for every course in the database.</span></span> <span data-ttu-id="35bfd-174">Eğitmenin atandığı kurslar seçilidir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-174">Courses that the instructor is assigned to are selected.</span></span> <span data-ttu-id="35bfd-175">Kullanıcı kurs atamalarını değiştirmek için onay kutularını seçebilir veya temizleyebilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-175">The user can select or clear checkboxes to change course assignments.</span></span> <span data-ttu-id="35bfd-176">Kurs sayısı çok büyükse, farklı bir kullanıcı arabirimi daha iyi çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-176">If the number of courses were much greater, a different UI might work better.</span></span> <span data-ttu-id="35bfd-177">Ancak burada gösterilen çoktan çoğa ilişkiyi yönetme yöntemi değişmez.</span><span class="sxs-lookup"><span data-stu-id="35bfd-177">But the method of managing a many-to-many relationship shown here wouldn't change.</span></span> <span data-ttu-id="35bfd-178">İlişki oluşturmak veya silmek için bir JOIN varlığını işlersiniz.</span><span class="sxs-lookup"><span data-stu-id="35bfd-178">To create or delete relationships, you manipulate a join entity.</span></span>

### <a name="create-a-class-for-assigned-courses-data"></a><span data-ttu-id="35bfd-179">Atanan kurslar verileri için bir sınıf oluşturma</span><span class="sxs-lookup"><span data-stu-id="35bfd-179">Create a class for assigned courses data</span></span>

<span data-ttu-id="35bfd-180">Aşağıdaki kodla *SchoolViewModels/AssignedCourseData. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="35bfd-180">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="35bfd-181">Sınıfı `AssignedCourseData` , bir eğitmene atanan kurslar için onay kutularını oluşturmak üzere verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-181">The `AssignedCourseData` class contains data to create the check boxes for courses assigned to an instructor.</span></span>

### <a name="create-an-instructor-page-model-base-class"></a><span data-ttu-id="35bfd-182">Bir eğitmen sayfa modeli temel sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="35bfd-182">Create an Instructor page model base class</span></span>

<span data-ttu-id="35bfd-183">*Pages/eğitmenler/Komutctorcoursespagemodel. cs* temel sınıfını oluşturun:</span><span class="sxs-lookup"><span data-stu-id="35bfd-183">Create the *Pages/Instructors/InstructorCoursesPageModel.cs* base class:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/InstructorCoursesPageModel.cs?name=snippet_All)]

<span data-ttu-id="35bfd-184">, `InstructorCoursesPageModel` Düzenleme ve oluşturma sayfa modelleri için kullanacağınız temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-184">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="35bfd-185">`PopulateAssignedCourseData``Course` doldurmak`AssignedCourseDataList`için tüm varlıkları okur.</span><span class="sxs-lookup"><span data-stu-id="35bfd-185">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="35bfd-186">Her kurs için kod, başlığı ve eğitmenin `CourseID`kursa atanıp atanmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-186">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="35bfd-187">Bir [diyez kümesi](/dotnet/api/system.collections.generic.hashset-1) etkili aramalar için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-187">A [HashSet](/dotnet/api/system.collections.generic.hashset-1) is used for efficient lookups.</span></span>

<span data-ttu-id="35bfd-188">Razor sayfasında bir kurs varlıkları koleksiyonu olmadığından, model Bağlayıcısı `CourseAssignments` gezinti özelliğini otomatik olarak güncelleştiremez.</span><span class="sxs-lookup"><span data-stu-id="35bfd-188">Since the Razor page doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="35bfd-189">`CourseAssignments` Gezinti özelliğini güncelleştirmek için model cildi kullanmak yerine, bunu yeni `UpdateInstructorCourses` yöntemde yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-189">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="35bfd-190">Bu nedenle, `CourseAssignments` özelliği model bağlamadan hariç bırakmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-190">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="35bfd-191">Bu, beyaz liste aşırı yüklemesini kullandığınız ve `TryUpdateModel` `CourseAssignments` içerme listesinde olmadığı için çağıran kodda herhangi bir değişiklik yapılmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="35bfd-191">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="35bfd-192">Hiçbir onay kutusu seçili değilse, içindeki `UpdateInstructorCourses` kod `CourseAssignments` gezinti özelliğini boş bir koleksiyonla başlatır ve döndürür:</span><span class="sxs-lookup"><span data-stu-id="35bfd-192">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/InstructorCoursesPageModel.cs?name=snippet_IfNull)]

<span data-ttu-id="35bfd-193">Kod daha sonra, veritabanındaki tüm kurslardan geçer ve bu her kursu, sayfada seçili olanlar ile ilgili olarak, eğitmene atanmış olanlara karşı denetler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-193">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the page.</span></span> <span data-ttu-id="35bfd-194">Etkili aramaları kolaylaştırmak için, ikinci iki koleksiyon `HashSet` nesnelerde depolanır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-194">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="35bfd-195">Kurs onay kutusu seçilmişse ancak kurs, `Instructor.CourseAssignments` gezinti özelliğinde değilse kurs, Gezinti özelliğindeki koleksiyona eklenir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-195">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/InstructorCoursesPageModel.cs?name=snippet_UpdateCourses)]

<span data-ttu-id="35bfd-196">Kurs onay kutusu seçili değilse, ancak kurs `Instructor.CourseAssignments` gezinti özelliği ise, kurs, gezinti özelliğinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-196">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/InstructorCoursesPageModel.cs?name=snippet_UpdateCoursesElse)]

### <a name="handle-office-location"></a><span data-ttu-id="35bfd-197">Office konumunu işle</span><span class="sxs-lookup"><span data-stu-id="35bfd-197">Handle office location</span></span>

<span data-ttu-id="35bfd-198">Düzenleme sayfasının işlemesi gereken başka bir ilişki ise, eğitmen varlığının `OfficeAssignment` varlığa sahip olduğu bire sıfır veya-bir ilişkidir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-198">Another relationship the edit page has to handle is the one-to-zero-or-one relationship that the Instructor entity has with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="35bfd-199">Eğitmen düzenleme kodu aşağıdaki senaryoları işlemelidir:</span><span class="sxs-lookup"><span data-stu-id="35bfd-199">The instructor edit code must handle the following scenarios:</span></span> 

* <span data-ttu-id="35bfd-200">Kullanıcı Office atamasını temizlediğinde `OfficeAssignment` varlığı silin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-200">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="35bfd-201">Kullanıcı bir Office ataması girerse ve boşsa, yeni `OfficeAssignment` bir varlık oluşturun.</span><span class="sxs-lookup"><span data-stu-id="35bfd-201">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="35bfd-202">Kullanıcı Office atamasını değiştirirse `OfficeAssignment` varlığı güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-202">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

### <a name="update-the-instructor-edit-page-model"></a><span data-ttu-id="35bfd-203">Eğitmen düzenleme sayfası modelini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="35bfd-203">Update the Instructor Edit page model</span></span>

<span data-ttu-id="35bfd-204">*Pages/eğitmenler/Edit. cshtml. cs* dosyasını aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-204">Update *Pages/Instructors/Edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Edit.cshtml.cs?name=snippet_All&highlight=9,28-32,38,42-77)]

<span data-ttu-id="35bfd-205">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="35bfd-205">The preceding code:</span></span>

* <span data-ttu-id="35bfd-206">`OfficeAssignment`, `Instructor` ,Ve`CourseAssignment.Course` gezinme özellikleri için Eager yükleme kullanarak geçerli varlığı veritabanından alır. `CourseAssignment`</span><span class="sxs-lookup"><span data-stu-id="35bfd-206">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment`, `CourseAssignment`, and `CourseAssignment.Course` navigation properties.</span></span>
* <span data-ttu-id="35bfd-207">Alınan `Instructor` varlığı model Ciltçideki değerlerle güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-207">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="35bfd-208">`TryUpdateModel`[fazla nakletmeyi](xref:data/ef-rp/crud#overposting)önler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-208">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="35bfd-209">Office konumu boşsa, null olarak ayarlar `Instructor.OfficeAssignment` .</span><span class="sxs-lookup"><span data-stu-id="35bfd-209">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="35bfd-210">Null `Instructor.OfficeAssignment` olduğunda, `OfficeAssignment` tablodaki ilgili satır silinir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-210">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>
* <span data-ttu-id="35bfd-211">Görüntüleme modeli `OnGetAsync` sınıfını kullanarak onay kutuları `PopulateAssignedCourseData` için bilgi sağlamak üzere ' de çağırır. `AssignedCourseData`</span><span class="sxs-lookup"><span data-stu-id="35bfd-211">Calls `PopulateAssignedCourseData` in `OnGetAsync` to provide information for the checkboxes using the `AssignedCourseData` view model class.</span></span>
* <span data-ttu-id="35bfd-212">Onay `UpdateInstructorCourses` kutularından `OnPostAsync` düzenlenmekte olan eğitmen varlığına bilgi uygulamak için ' de çağırır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-212">Calls `UpdateInstructorCourses` in `OnPostAsync` to apply information from the checkboxes to the Instructor entity being edited.</span></span>
* <span data-ttu-id="35bfd-213">Başarısız olursa `UpdateInstructorCourses` , `OnPostAsync` ve ' de `PopulateAssignedCourseData` çağırır. `TryUpdateModel`</span><span class="sxs-lookup"><span data-stu-id="35bfd-213">Calls `PopulateAssignedCourseData` and `UpdateInstructorCourses` in `OnPostAsync` if `TryUpdateModel` fails.</span></span> <span data-ttu-id="35bfd-214">Bu yöntem çağrıları, bir hata iletisiyle yeniden görüntülendiğinde sayfaya girilen atanan kurs verilerini geri yükler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-214">These method calls restore the assigned course data entered on the page when it is redisplayed with an error message.</span></span>

### <a name="update-the-instructor-edit-razor-page"></a><span data-ttu-id="35bfd-215">Eğitmen düzenleme Razor sayfasını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="35bfd-215">Update the Instructor Edit Razor page</span></span>

<span data-ttu-id="35bfd-216">*Pages/eğitmenler/Edit. cshtml* dosyasını aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-216">Update *Pages/Instructors/Edit.cshtml* with the following code:</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Edit.cshtml?highlight=29-59)]

<span data-ttu-id="35bfd-217">Yukarıdaki kod, üç sütun içeren bir HTML tablosu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="35bfd-217">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="35bfd-218">Her sütunda, kurs numarasını ve başlığını içeren bir CheckBox ve bir açıklamalı alt yazı vardır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-218">Each column has a checkbox and a caption containing the course number and title.</span></span> <span data-ttu-id="35bfd-219">Onay kutularının hepsi aynı ada ("Selectedkurslar") sahiptir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-219">The checkboxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="35bfd-220">Aynı adı kullanmak model cildi bir grup olarak kabul etmek üzere bilgilendirir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-220">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="35bfd-221">Her onay kutusunun değer özniteliği olarak `CourseID`ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-221">The value attribute of each checkbox is set to `CourseID`.</span></span> <span data-ttu-id="35bfd-222">Sayfa gönderildiğinde, model Ciltçi yalnızca seçili onay kutularının `CourseID` değerlerinden oluşan bir dizi geçirir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-222">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the checkboxes that are selected.</span></span>

<span data-ttu-id="35bfd-223">Onay kutuları başlangıçta işlendiğinde, eğitmen 'e atanan kurslar seçilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-223">When the checkboxes are initially rendered, courses assigned to the instructor are selected.</span></span>

<span data-ttu-id="35bfd-224">Not: Eğitim kursu verilerini düzenlemek için buradaki yaklaşım, sınırlı sayıda kurs olduğunda iyi bir şekilde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-224">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="35bfd-225">Çok daha büyük olan koleksiyonlar için, farklı bir kullanıcı arabirimi ve farklı bir güncelleştirme yöntemi daha erişilebilir ve verimli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-225">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

<span data-ttu-id="35bfd-226">Uygulamayı çalıştırın ve güncelleştirilmiş eğitmenler düzenleme sayfasını test edin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-226">Run the app and test the updated Instructors Edit page.</span></span> <span data-ttu-id="35bfd-227">Bazı kurs atamalarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-227">Change some course assignments.</span></span> <span data-ttu-id="35bfd-228">Değişiklikler Dizin sayfasında yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-228">The changes are reflected on the Index page.</span></span>

### <a name="update-the-instructor-create-page"></a><span data-ttu-id="35bfd-229">Eğitmen oluştur sayfasını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="35bfd-229">Update the Instructor Create page</span></span>

<span data-ttu-id="35bfd-230">, Düzenleme sayfasına benzer kodla, bir sayfa modeli ve Razor sayfası oluşturma adlı eğitmeni güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-230">Update the Instructor Create page model and Razor page with code similar to the Edit page:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Create.cshtml.cs)]

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Create.cshtml)]

<span data-ttu-id="35bfd-231">Eğitmen oluşturma sayfasını test edin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-231">Test the instructor Create page.</span></span>

## <a name="update-the-instructor-delete-page"></a><span data-ttu-id="35bfd-232">Eğitmen silme sayfasını Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="35bfd-232">Update the Instructor Delete page</span></span>

<span data-ttu-id="35bfd-233">*Sayfaları/eğitmenler/delete. cshtml. cs* dosyasını şu kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-233">Update *Pages/Instructors/Delete.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Delete.cshtml.cs?highlight=45-61)]

<span data-ttu-id="35bfd-234">Yukarıdaki kod aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="35bfd-234">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="35bfd-235">`CourseAssignments` Gezinti özelliği için Eager yüklemesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-235">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="35bfd-236">`CourseAssignments`eğitmen silindiğinde, dahil edilmiş veya silinmemelidir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-236">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="35bfd-237">Bunları okumaktan kaçınmak için, veritabanında basamaklı silme 'yı yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="35bfd-237">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="35bfd-238">Silinecek eğitmen herhangi bir departmanların Yöneticisi olarak atanırsa, bu departmanlardan eğitmen atamasını kaldırır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-238">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

<span data-ttu-id="35bfd-239">Uygulamayı çalıştırın ve silme sayfasını test edin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-239">Run the app and test the Delete page.</span></span>

## <a name="next-steps"></a><span data-ttu-id="35bfd-240">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="35bfd-240">Next steps</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="35bfd-241">[Önceki öğretici](xref:data/ef-rp/read-related-data)
> [sonraki öğretici](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="35bfd-241">[Previous tutorial](xref:data/ef-rp/read-related-data)
[Next tutorial](xref:data/ef-rp/concurrency)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="35bfd-242">Bu öğreticide ilgili verilerin güncelleştirilmesi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-242">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="35bfd-243">Olamaz çözmenize, sorunlarla karşılaşırsanız, [indirin veya tamamlanmış uygulamayı görüntüleyin.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="35bfd-243">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="35bfd-244">[Yükleme yönergeleri](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="35bfd-244">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="35bfd-245">Aşağıdaki çizimler tamamlanan sayfalardan bazılarını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-245">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="35bfd-246">![Kurs düzenleme sayfası](update-related-data/_static/course-edit.png)
![eğitmeni düzenleme sayfası](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="35bfd-246">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="35bfd-247">Kurs oluşturma ve düzenleme sayfalarını inceleyin ve test edin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-247">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="35bfd-248">Yeni bir kurs oluşturun.</span><span class="sxs-lookup"><span data-stu-id="35bfd-248">Create a new course.</span></span> <span data-ttu-id="35bfd-249">Departman, adı değil, birincil anahtarı (bir tamsayı) tarafından seçilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-249">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="35bfd-250">Yeni kursu düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-250">Edit the new course.</span></span> <span data-ttu-id="35bfd-251">Sınamayı bitirdiğinizde yeni kursu silin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-251">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="35bfd-252">Ortak kod paylaşmak için bir temel sınıf oluşturun</span><span class="sxs-lookup"><span data-stu-id="35bfd-252">Create a base class to share common code</span></span>

<span data-ttu-id="35bfd-253">Kurslar/oluştur ve kurslar/Düzenle sayfaları, her birinin departman adları listesine ihtiyacı vardır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-253">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="35bfd-254">Oluşturma ve düzenleme sayfaları için *Pages/kurslar/DepartmentNamePageModel. cshtml. cs* temel sınıfını oluşturun:</span><span class="sxs-lookup"><span data-stu-id="35bfd-254">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="35bfd-255">Yukarıdaki kod, bölüm adlarının listesini içeren bir [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="35bfd-255">The preceding code creates a [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="35bfd-256">Belirtilmişse, bu departman `SelectList`öğesinde seçilir. `selectedDepartment`</span><span class="sxs-lookup"><span data-stu-id="35bfd-256">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="35bfd-257">Oluşturma ve düzenleme sayfa modeli sınıfları öğesinden `DepartmentNamePageModel`türetilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-257">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="35bfd-258">Kurslar sayfalarını özelleştirme</span><span class="sxs-lookup"><span data-stu-id="35bfd-258">Customize the Courses Pages</span></span>

<span data-ttu-id="35bfd-259">Yeni bir kurs varlığı oluşturulduğunda, mevcut bir departmanla bir ilişkisi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-259">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="35bfd-260">Bir kurs oluştururken bir departman eklemek için, oluşturma ve düzenleme için temel sınıf, departmanı seçmeye yönelik bir açılan liste içerir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-260">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="35bfd-261">Açılan liste, `Course.DepartmentID` yabancı anahtar (FK) özelliğini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="35bfd-261">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="35bfd-262">EF Core, `Course.DepartmentID` `Department` gezinti özelliğini yüklemek için FK kullanır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-262">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Kurs oluştur](update-related-data/_static/ddl.png)

<span data-ttu-id="35bfd-264">Oluşturma sayfası modelini aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-264">Update the Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

<span data-ttu-id="35bfd-265">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="35bfd-265">The preceding code:</span></span>

* <span data-ttu-id="35bfd-266">Türetiliyor `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="35bfd-266">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="35bfd-267">`TryUpdateModelAsync` [Aşırı nakletmeyi](xref:data/ef-rp/crud#overposting)engellemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-267">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="35bfd-268">`ViewData["DepartmentID"]` İle`DepartmentNameSL` değiştirir (taban sınıfından).</span><span class="sxs-lookup"><span data-stu-id="35bfd-268">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="35bfd-269">`ViewData["DepartmentID"]`, türü kesin belirlenmiş `DepartmentNameSL`olan ile değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-269">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="35bfd-270">Kesin olarak belirlenmiş modeller, kesin olarak yazılan zayıf bir şekilde tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-270">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="35bfd-271">Daha fazla bilgi için bkz. [zayıf yazılmış veriler (ViewData ve ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="35bfd-271">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="35bfd-272">Kurslar oluşturma sayfasını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="35bfd-272">Update the Courses Create page</span></span>

<span data-ttu-id="35bfd-273">*Sayfaları/kursları/Create. cshtml* 'yi aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-273">Update *Pages/Courses/Create.cshtml* with the following code:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="35bfd-274">Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="35bfd-274">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="35bfd-275">**DepartmentID** etiketini **departmana**dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="35bfd-275">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="35bfd-276">`"ViewBag.DepartmentID"` İle`DepartmentNameSL` değiştirir (taban sınıfından).</span><span class="sxs-lookup"><span data-stu-id="35bfd-276">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="35bfd-277">"Departmanı Seç" seçeneğini ekler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-277">Adds the "Select Department" option.</span></span> <span data-ttu-id="35bfd-278">Bu değişiklik, ilk departman yerine "departmanı Seç" i işler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-278">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="35bfd-279">Departman seçili olmadığında bir doğrulama iletisi ekler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-279">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="35bfd-280">Razor sayfası [seçme etiketi yardımcısını](xref:mvc/views/working-with-forms#the-select-tag-helper)kullanır:</span><span class="sxs-lookup"><span data-stu-id="35bfd-280">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="35bfd-281">Oluştur sayfasını test edin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-281">Test the Create page.</span></span> <span data-ttu-id="35bfd-282">Oluştur sayfası departman KIMLIĞI yerine departman adını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-282">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="35bfd-283">Kurslar düzenleme sayfasını güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-283">Update the Courses Edit page.</span></span>

<span data-ttu-id="35bfd-284">*Pages/kurslar/Edit. cshtml. cs* dosyasındaki kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-284">Replace the code in *Pages/Courses/Edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

<span data-ttu-id="35bfd-285">Değişiklikler, oluşturma sayfası modelinde yapılanlarla benzerdir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-285">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="35bfd-286">Yukarıdaki kodda `PopulateDepartmentsDropDownList` , açılan listede belirtilen departmanı belirleyen departman kimliği ' nde geçirilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-286">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="35bfd-287">*Pages/kurslar/Edit. cshtml* 'yi şu biçimlendirmeyle güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-287">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="35bfd-288">Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="35bfd-288">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="35bfd-289">Kurs KIMLIĞINI görüntüler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-289">Displays the course ID.</span></span> <span data-ttu-id="35bfd-290">Genellikle bir varlığın birincil anahtarı (PK) gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="35bfd-290">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="35bfd-291">PKs 'ler genellikle kullanıcılara daha az anlamlı olur.</span><span class="sxs-lookup"><span data-stu-id="35bfd-291">PKs are usually meaningless to users.</span></span> <span data-ttu-id="35bfd-292">Bu durumda, PK kurs numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-292">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="35bfd-293">**DepartmentID** etiketini **departmana**dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="35bfd-293">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="35bfd-294">`"ViewBag.DepartmentID"` İle`DepartmentNameSL` değiştirir (taban sınıfından).</span><span class="sxs-lookup"><span data-stu-id="35bfd-294">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="35bfd-295">Sayfa, kurs numarası için gizli bir`<input type="hidden">`alan () içerir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-295">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="35bfd-296">`<label>` Etiket`asp-for="Course.CourseID"` Yardımcısı ekleme, gizli alan gereksinimini ortadan kaldırmaz.</span><span class="sxs-lookup"><span data-stu-id="35bfd-296">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="35bfd-297">`<input type="hidden">`Kullanıcı **Kaydet**' e tıkladığında, gönderilen veriler için kurs numarasının dahil olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-297">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="35bfd-298">Güncelleştirilmiş kodu test edin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-298">Test the updated code.</span></span> <span data-ttu-id="35bfd-299">Kurs oluşturun, düzenleyin ve silin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-299">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="35bfd-300">Ayrıntılara AsNoTracking ekleme ve sayfa modellerini silme</span><span class="sxs-lookup"><span data-stu-id="35bfd-300">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="35bfd-301">[Anotracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) , izleme gerekli olmadığında performansı iyileştirebilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-301">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="35bfd-302">Sil `AsNoTracking` ve Ayrıntılar sayfa modeline ekleyin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-302">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="35bfd-303">Aşağıdaki kod, güncelleştirilmiş silme sayfası modelini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="35bfd-303">The following code shows the updated Delete page model:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="35bfd-304">*Pages/kurslar/details. cshtml. cs* dosyasındaki yöntemigüncelleştirin:`OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="35bfd-304">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="35bfd-305">Silme ve Ayrıntılar sayfalarını değiştirme</span><span class="sxs-lookup"><span data-stu-id="35bfd-305">Modify the Delete and Details pages</span></span>

<span data-ttu-id="35bfd-306">Razor Sil sayfasını aşağıdaki biçimlendirmeyle güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-306">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="35bfd-307">Ayrıntılar sayfasında aynı değişiklikleri yapın.</span><span class="sxs-lookup"><span data-stu-id="35bfd-307">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="35bfd-308">Kurs sayfalarını test etme</span><span class="sxs-lookup"><span data-stu-id="35bfd-308">Test the Course pages</span></span>

<span data-ttu-id="35bfd-309">Test oluşturma, düzenleme, Ayrıntılar ve silme.</span><span class="sxs-lookup"><span data-stu-id="35bfd-309">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="35bfd-310">Eğitmen sayfalarını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="35bfd-310">Update the instructor pages</span></span>

<span data-ttu-id="35bfd-311">Aşağıdaki bölümlerde, eğitmen sayfaları günceldir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-311">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="35bfd-312">Office konumu Ekle</span><span class="sxs-lookup"><span data-stu-id="35bfd-312">Add office location</span></span>

<span data-ttu-id="35bfd-313">Bir eğitmen kaydını düzenlediğinizde, eğitmenin Office atamasını güncelleştirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="35bfd-313">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="35bfd-314">`Instructor` Varlığın `OfficeAssignment` varlıkla bire sıfır veya-bir ilişkisi vardır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-314">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="35bfd-315">Eğitmen kodu şu şekilde olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="35bfd-315">The instructor code must handle:</span></span>

* <span data-ttu-id="35bfd-316">Kullanıcı Office atamasını temizlediğinde `OfficeAssignment` varlığı silin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-316">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="35bfd-317">Kullanıcı bir Office ataması girerse ve boşsa, yeni `OfficeAssignment` bir varlık oluşturun.</span><span class="sxs-lookup"><span data-stu-id="35bfd-317">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="35bfd-318">Kullanıcı Office atamasını değiştirirse `OfficeAssignment` varlığı güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-318">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="35bfd-319">Eğitmenler düzenleme sayfası modelini aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-319">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

<span data-ttu-id="35bfd-320">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="35bfd-320">The preceding code:</span></span>

* <span data-ttu-id="35bfd-321">Gezinti`OfficeAssignment` özelliği için `Instructor` Eager yüklemesini kullanarak geçerli varlığı veritabanından alır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-321">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
* <span data-ttu-id="35bfd-322">Alınan `Instructor` varlığı model Ciltçideki değerlerle güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-322">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="35bfd-323">`TryUpdateModel`[fazla nakletmeyi](xref:data/ef-rp/crud#overposting)önler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-323">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="35bfd-324">Office konumu boşsa, null olarak ayarlar `Instructor.OfficeAssignment` .</span><span class="sxs-lookup"><span data-stu-id="35bfd-324">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="35bfd-325">Null `Instructor.OfficeAssignment` olduğunda, `OfficeAssignment` tablodaki ilgili satır silinir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-325">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="35bfd-326">Eğitmen düzenleme sayfasını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="35bfd-326">Update the instructor Edit page</span></span>

<span data-ttu-id="35bfd-327">*Sayfaları/eğitmenler/Edit. cshtml* dosyasını Office konumuyla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-327">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="35bfd-328">Bir eğitmenler ofis konumunu değiştirebildiğinizi doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="35bfd-328">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="35bfd-329">Eğitmen düzenleme sayfasına kurs atamaları ekleme</span><span class="sxs-lookup"><span data-stu-id="35bfd-329">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="35bfd-330">Eğitmenler, istediğiniz sayıda kurs öğretebilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-330">Instructors may teach any number of courses.</span></span> <span data-ttu-id="35bfd-331">Bu bölümde kurs atamalarını değiştirme olanağını eklersiniz.</span><span class="sxs-lookup"><span data-stu-id="35bfd-331">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="35bfd-332">Aşağıdaki görüntüde güncelleştirilmiş eğitmen düzenleme sayfası gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="35bfd-332">The following image shows the updated instructor Edit page:</span></span>

![Kurslar ile eğitmen düzenleme sayfası](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="35bfd-334">`Course`ve `Instructor` çoktan çoğa ilişkisine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-334">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="35bfd-335">İlişki eklemek ve kaldırmak için, `CourseAssignments` JOIN varlık kümesinden varlık ekleyin ve kaldırın.</span><span class="sxs-lookup"><span data-stu-id="35bfd-335">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="35bfd-336">Onay kutuları, bir eğitmenin atandığı kurslara değişiklikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="35bfd-336">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="35bfd-337">Veritabanındaki her kurs için bir onay kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-337">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="35bfd-338">Eğitmenin atandığı kurslar denetlenir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-338">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="35bfd-339">Kullanıcı kurs atamalarını değiştirmek için onay kutularını seçebilir veya temizleyebilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-339">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="35bfd-340">Kurs sayısı çok fazlaysa:</span><span class="sxs-lookup"><span data-stu-id="35bfd-340">If the number of courses were much greater:</span></span>

* <span data-ttu-id="35bfd-341">Kursları göstermek için büyük olasılıkla farklı bir kullanıcı arabirimi kullanıyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="35bfd-341">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="35bfd-342">İlişki oluşturmak veya silmek için bir JOIN varlığını düzenleme yöntemi değişmez.</span><span class="sxs-lookup"><span data-stu-id="35bfd-342">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="35bfd-343">Eğitmen sayfaları oluşturma ve düzenleme desteğini desteklemek için sınıflar ekleme</span><span class="sxs-lookup"><span data-stu-id="35bfd-343">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="35bfd-344">Aşağıdaki kodla *SchoolViewModels/AssignedCourseData. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="35bfd-344">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="35bfd-345">Sınıfı `AssignedCourseData` , bir eğitmen tarafından atanan kurslar için onay kutularını oluşturacak verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-345">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="35bfd-346">*Pages/eğitmenler/Komutctorcoursespagemodel. cshtml. cs* temel sınıfını oluşturun:</span><span class="sxs-lookup"><span data-stu-id="35bfd-346">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="35bfd-347">, `InstructorCoursesPageModel` Düzenleme ve oluşturma sayfa modelleri için kullanacağınız temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-347">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="35bfd-348">`PopulateAssignedCourseData``Course` doldurmak`AssignedCourseDataList`için tüm varlıkları okur.</span><span class="sxs-lookup"><span data-stu-id="35bfd-348">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="35bfd-349">Her kurs için kod, başlığı ve eğitmenin `CourseID`kursa atanıp atanmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-349">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="35bfd-350">Bir [diyez kümesi](/dotnet/api/system.collections.generic.hashset-1) , verimli aramalar oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-350">A [HashSet](/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="35bfd-351">Eğitmenler sayfa modelini Düzenle</span><span class="sxs-lookup"><span data-stu-id="35bfd-351">Instructors Edit page model</span></span>

<span data-ttu-id="35bfd-352">Eğitmen düzenleme sayfası modelini aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-352">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

<span data-ttu-id="35bfd-353">Yukarıdaki kod, Office atama değişikliklerini işler.</span><span class="sxs-lookup"><span data-stu-id="35bfd-353">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="35bfd-354">Eğitmen Razor görünümünü güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-354">Update the instructor Razor View:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="35bfd-355">Kodu Visual Studio 'Ya yapıştırdığınızda, satır sonları kodu kesen bir şekilde değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-355">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="35bfd-356">Otomatik biçimlendirmeyi geri almak için CTRL + Z bir kez tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="35bfd-356">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="35bfd-357">CTRL + Z, burada gördüğünüz gibi görünmeleri için satır sonlarını düzeltir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-357">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="35bfd-358">Girintide kusursuz `@:</tr><tr>`olması gerekmez, ancak `@:<td>` `@:</td>`,, ve `@:</tr>` çizgilerinin her biri gösterildiği gibi tek bir satırda olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-358">The indentation doesn't have to be perfect, but the `@:</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="35bfd-359">Yeni kod bloğu seçiliyken, yeni kodu mevcut kodla hizalamak için üç kez Tab tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="35bfd-359">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="35bfd-360">Bu [bağlantı ile](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html)bu hatanın durumunu oylayın veya gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-360">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="35bfd-361">Yukarıdaki kod, üç sütun içeren bir HTML tablosu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="35bfd-361">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="35bfd-362">Her sütunda bir onay kutusu ve kurs numarasını ve başlığını içeren bir açıklamalı alt yazı vardır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-362">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="35bfd-363">Onay kutularının hepsi aynı ada ("Selectedkurslar") sahiptir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-363">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="35bfd-364">Aynı adı kullanmak model cildi bir grup olarak kabul etmek üzere bilgilendirir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-364">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="35bfd-365">Her onay kutusunun değer özniteliği olarak `CourseID`ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-365">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="35bfd-366">Sayfa gönderildiğinde, model Ciltçi yalnızca seçili onay kutularının `CourseID` değerlerinden oluşan bir dizi geçirir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-366">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="35bfd-367">Onay kutuları başlangıçta işlendiğinde, eğitmenin atandığı kursların denetlenen öznitelikleri vardır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-367">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="35bfd-368">Uygulamayı çalıştırın ve güncelleştirilmiş eğitmenler düzenleme sayfasını test edin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-368">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="35bfd-369">Bazı kurs atamalarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-369">Change some course assignments.</span></span> <span data-ttu-id="35bfd-370">Değişiklikler Dizin sayfasında yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-370">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="35bfd-371">Not: Eğitim kursu verilerini düzenlemek için buradaki yaklaşım, sınırlı sayıda kurs olduğunda iyi bir şekilde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-371">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="35bfd-372">Çok daha büyük olan koleksiyonlar için, farklı bir kullanıcı arabirimi ve farklı bir güncelleştirme yöntemi daha erişilebilir ve verimli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-372">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="35bfd-373">Eğitmenler oluştur sayfasını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="35bfd-373">Update the instructors Create page</span></span>

<span data-ttu-id="35bfd-374">Aşağıdaki kodla, eğitmen sayfa modeli oluştur sayfasını güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-374">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="35bfd-375">Yukarıdaki kod, *Pages/eğitmenler/Edit. cshtml. cs* koduna benzerdir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-375">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="35bfd-376">Eğitmen oluşturma Razor sayfasını aşağıdaki biçimlendirmeyle güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-376">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="35bfd-377">Eğitmen oluşturma sayfasını test edin.</span><span class="sxs-lookup"><span data-stu-id="35bfd-377">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="35bfd-378">Silme sayfası</span><span class="sxs-lookup"><span data-stu-id="35bfd-378">Update the Delete page</span></span>

<span data-ttu-id="35bfd-379">Delete sayfa modeli aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="35bfd-379">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

<span data-ttu-id="35bfd-380">Yukarıdaki kod aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="35bfd-380">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="35bfd-381">`CourseAssignments` Gezinti özelliği için Eager yüklemesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-381">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="35bfd-382">`CourseAssignments`eğitmen silindiğinde, dahil edilmiş veya silinmemelidir.</span><span class="sxs-lookup"><span data-stu-id="35bfd-382">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="35bfd-383">Bunları okumaktan kaçınmak için, veritabanında basamaklı silme 'yı yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="35bfd-383">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="35bfd-384">Silinecek eğitmen herhangi bir departmanların Yöneticisi olarak atanırsa, bu departmanlardan eğitmen atamasını kaldırır.</span><span class="sxs-lookup"><span data-stu-id="35bfd-384">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="35bfd-385">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="35bfd-385">Additional resources</span></span>

* [<span data-ttu-id="35bfd-386">Bu öğreticinin YouTube sürümü (Bölüm 1)</span><span class="sxs-lookup"><span data-stu-id="35bfd-386">YouTube version of this tutorial (Part 1)</span></span>](https://www.youtube.com/watch?v=Csh6gkmwc9E)
* [<span data-ttu-id="35bfd-387">Bu öğreticinin YouTube sürümü (Bölüm 2)</span><span class="sxs-lookup"><span data-stu-id="35bfd-387">YouTube version of this tutorial (Part 2)</span></span>](https://www.youtube.com/watch?v=mOAankB_Zgc)

> [!div class="step-by-step"]
> <span data-ttu-id="35bfd-388">[Önceki](xref:data/ef-rp/read-related-data)İleri
> [](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="35bfd-388">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>

::: moniker-end
