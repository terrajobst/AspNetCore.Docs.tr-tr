---
title: 'Öğretici: İlgili verileri - EF çekirdekli ASP.NET MVC güncelleştirme'
description: Bu öğreticide yabancı anahtar alanları ve gezinti özellikleri güncelleştirerek ilgili verileri güncelleştirin.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: 6add725430380f0855fe660a70b90a4546ef0637
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750906"
---
# <a name="tutorial-update-related-data---aspnet-mvc-with-ef-core"></a><span data-ttu-id="01e24-103">Öğretici: İlgili verileri - EF çekirdekli ASP.NET MVC güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="01e24-103">Tutorial: Update related data - ASP.NET MVC with EF Core</span></span>

<span data-ttu-id="01e24-104">Önceki öğreticide ilgili veriler görüntülenecek; Bu öğreticide yabancı anahtar alanları ve gezinti özellikleri güncelleştirerek ilgili verileri güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="01e24-104">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="01e24-105">Aşağıdaki çizimler ile çalışma sayfaları bazılarını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="01e24-105">The following illustrations show some of the pages that you'll work with.</span></span>

![Kurs düzenleme sayfası](update-related-data/_static/course-edit.png)

![Eğitmen düzenleme sayfası](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="01e24-108">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="01e24-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="01e24-109">Kursları sayfalarını özelleştirme</span><span class="sxs-lookup"><span data-stu-id="01e24-109">Customize Courses pages</span></span>
> * <span data-ttu-id="01e24-110">Eğitmenler düzenleme sayfası Ekle</span><span class="sxs-lookup"><span data-stu-id="01e24-110">Add Instructors Edit page</span></span>
> * <span data-ttu-id="01e24-111">Kursları Düzen sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="01e24-111">Add courses to Edit page</span></span>
> * <span data-ttu-id="01e24-112">Güncelleştirme silme sayfası</span><span class="sxs-lookup"><span data-stu-id="01e24-112">Update Delete page</span></span>
> * <span data-ttu-id="01e24-113">Ofis konumu ve kurslar oluşturma sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="01e24-113">Add office location and courses to Create page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01e24-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="01e24-114">Prerequisites</span></span>

* [<span data-ttu-id="01e24-115">İlgili verileri okuma</span><span class="sxs-lookup"><span data-stu-id="01e24-115">Read related data</span></span>](read-related-data.md)

## <a name="customize-courses-pages"></a><span data-ttu-id="01e24-116">Kursları sayfalarını özelleştirme</span><span class="sxs-lookup"><span data-stu-id="01e24-116">Customize Courses pages</span></span>

<span data-ttu-id="01e24-117">Yeni bir kurs varlık oluşturulduğunda, var olan bir bölüm arasında bir ilişki olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="01e24-117">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="01e24-118">Bunu kolaylaştırmak için iskele kurulan kodu denetleyici metotları ve departman seçmek için aşağı açılan listede yer oluşturma ve düzenleme görünümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="01e24-118">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="01e24-119">Açılır listede kümeleri `Course.DepartmentID` yabancı anahtar özellik ve tüm yük için Entity Framework gereken `Department` uygun departmanı varlık sahip gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="01e24-119">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="01e24-120">İskele kurulan kodu kullanır ancak biraz hata işleme eklemek ve açılan listeyi sıralamak için değiştirmeniz.</span><span class="sxs-lookup"><span data-stu-id="01e24-120">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="01e24-121">İçinde *CoursesController.cs*, dört oluşturma ve düzenleme yöntemlerini silin ve bunları aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="01e24-121">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

<span data-ttu-id="01e24-122">Sonra `Edit` HttpPost yöntemi departmanı bilgilerine aşağı açılan liste için yüklenen yeni bir yöntem oluşturun.</span><span class="sxs-lookup"><span data-stu-id="01e24-122">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

<span data-ttu-id="01e24-123">`PopulateDepartmentsDropDownList` Yöntemi tüm Departmanlar adına göre sıralanmış bir listesini alır, oluşturur bir `SelectList` koleksiyonu için bir açılan liste ve toplama görünümüne geçirir `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="01e24-123">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="01e24-124">Yöntemi isteğe bağlı olarak kabul `selectedDepartment` açılır listede işlendiğinde seçilir öğeyi belirtmek çağıran kod veren parametresi.</span><span class="sxs-lookup"><span data-stu-id="01e24-124">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="01e24-125">' % S'adı "DepartmentID" geçirir görünümü `<select>` etiketi Yardımcısı ve yardımcı ardından bilir aranacak `ViewBag` nesnesi bir `SelectList` "DepartmentID" adlı.</span><span class="sxs-lookup"><span data-stu-id="01e24-125">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="01e24-126">HttpGet `Create` yöntem çağrılarını `PopulateDepartmentsDropDownList` için yeni bir kurs departman henüz kurulan değildir çünkü seçilen öğe ayarlama olmadan yöntemi:</span><span class="sxs-lookup"><span data-stu-id="01e24-126">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department isn't established yet:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

<span data-ttu-id="01e24-127">HttpGet `Edit` yöntemi düzenlenmekte olan kursa zaten atanmış bölüm Kimliğini temel öğeye ayarlar:</span><span class="sxs-lookup"><span data-stu-id="01e24-127">The HttpGet `Edit` method sets the selected item, based on the ID of the department that's already assigned to the course being edited:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

<span data-ttu-id="01e24-128">Her ikisi için de HttpPost yöntemleri `Create` ve `Edit` de bunlar sayfanın bir hatanın ardından yeniden seçili öğe ayarlar kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="01e24-128">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="01e24-129">Bu hata mesajını göstermeye sayfası görüntülendiğinde için her bölüm seçilmedi seçili kalmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="01e24-129">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="01e24-130">Ekleyin. Ayrıntılar için AsNoTracking ve Delete metotlarını</span><span class="sxs-lookup"><span data-stu-id="01e24-130">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="01e24-131">Kurs Details ve Delete sayfaları, performansı iyileştirmek için ekleme `AsNoTracking` çağrıları `Details` ve HttpGet `Delete` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="01e24-131">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a><span data-ttu-id="01e24-132">Kurs görünümleri değiştirin</span><span class="sxs-lookup"><span data-stu-id="01e24-132">Modify the Course views</span></span>

<span data-ttu-id="01e24-133">İçinde *Views/Courses/Create.cshtml*, "Bölüm seçin" seçeneğine ekleyin **departmanı** aşağı açılan listesinde, gelen başlığını değiştirme **DepartmentID** için  **Departman**, doğrulama iletisi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="01e24-133">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

<span data-ttu-id="01e24-134">İçinde *Views/Courses/Edit.cshtml*, yaptığınız yalnızca bölüm alan için aynı değişikliği yapmanız *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="01e24-134">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="01e24-135">Ayrıca *Views/Courses/Edit.cshtml*, önce bir kurs sayı alan eklemek **başlık** alan.</span><span class="sxs-lookup"><span data-stu-id="01e24-135">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="01e24-136">Kurs numarası birincil anahtarı olduğundan görüntülenir, ancak değiştirilemez.</span><span class="sxs-lookup"><span data-stu-id="01e24-136">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

<span data-ttu-id="01e24-137">Gizli bir alan zaten var. (`<input type="hidden">`) düzenleme Görünümü'nde kurs numarası.</span><span class="sxs-lookup"><span data-stu-id="01e24-137">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="01e24-138">Ekleme bir `<label>` etiketi Yardımcısı kullanıcı tıkladığında, deftere nakledilen verileri eklenecek kurs sayısı neden olmayan gizli alan gereksinimini ortadan değil **Kaydet** üzerinde **Düzenle** sayfası.</span><span class="sxs-lookup"><span data-stu-id="01e24-138">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="01e24-139">İçinde *Views/Courses/Delete.cshtml*, kurs sayı alanı en üstüne ekleyin ve bölüm kimliği departmanı adla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="01e24-139">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

<span data-ttu-id="01e24-140">İçinde *Views/Courses/Details.cshtml*, yalnızca yaptığınız için aynı değişikliği yapmanız *Delete.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="01e24-140">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="01e24-141">Kurs sayfaları test</span><span class="sxs-lookup"><span data-stu-id="01e24-141">Test the Course pages</span></span>

<span data-ttu-id="01e24-142">Uygulamayı çalıştırın, seçin **kursları** sekmesinde **Yeni Oluştur**, yeni bir kurs için veri girin:</span><span class="sxs-lookup"><span data-stu-id="01e24-142">Run the app, select the **Courses** tab, click **Create New**, and enter data for a new course:</span></span>

![Kurs oluşturma sayfası](update-related-data/_static/course-create.png)

<span data-ttu-id="01e24-144">**Oluştur**'u tıklatın.</span><span class="sxs-lookup"><span data-stu-id="01e24-144">Click **Create**.</span></span> <span data-ttu-id="01e24-145">Listeye eklenen yeni kurs ile kursları dizin sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="01e24-145">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="01e24-146">Bölüm adı dizin sayfası listesinde ilişki doğru şekilde kurulduğundan gösteren navigation özelliğinden gelir.</span><span class="sxs-lookup"><span data-stu-id="01e24-146">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="01e24-147">Tıklayın **Düzenle** kursları dizin sayfası kurs üzerinde.</span><span class="sxs-lookup"><span data-stu-id="01e24-147">Click **Edit** on a course in the Courses Index page.</span></span>

![Kurs düzenleme sayfası](update-related-data/_static/course-edit.png)

<span data-ttu-id="01e24-149">Sayfadaki verileri değiştirip'ı **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="01e24-149">Change data on the page and click **Save**.</span></span> <span data-ttu-id="01e24-150">Güncelleştirilmiş kurs verilerle kursları dizin sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="01e24-150">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-instructors-edit-page"></a><span data-ttu-id="01e24-151">Eğitmenler düzenleme sayfası Ekle</span><span class="sxs-lookup"><span data-stu-id="01e24-151">Add Instructors Edit page</span></span>

<span data-ttu-id="01e24-152">Bir eğitmen kaydı düzenlediğinizde, eğitmen ofis ataması güncelleştirilecek yönetebilmek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="01e24-152">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="01e24-153">Eğitmen varlık aşağıdaki durumlarda işlemek kodunuzu sahip olduğu anlamına gelir OfficeAssignment varlıkla biri sıfır-veya-bir ilişkisi vardır:</span><span class="sxs-lookup"><span data-stu-id="01e24-153">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="01e24-154">Kullanıcı ofis ataması temizler ve ilk olarak bir değer atanmayan OfficeAssignment varlığı silin.</span><span class="sxs-lookup"><span data-stu-id="01e24-154">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="01e24-155">Kullanıcı bir office atama değer girer ve başlangıçta boş, yeni OfficeAssignment varlık oluşturun.</span><span class="sxs-lookup"><span data-stu-id="01e24-155">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="01e24-156">Kullanıcı bir ofis ataması değeri değişirse, var olan bir OfficeAssignment varlık değeri değiştirin.</span><span class="sxs-lookup"><span data-stu-id="01e24-156">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="01e24-157">Eğitmenler denetleyici güncelleştirmesi</span><span class="sxs-lookup"><span data-stu-id="01e24-157">Update the Instructors controller</span></span>

<span data-ttu-id="01e24-158">İçinde *InstructorsController.cs*, HttpGet kodda değişiklik `Edit` BT'nin Eğitmen varlığın yükler. Bu nedenle yöntemi `OfficeAssignment` gezinti özelliği ve çağrıları `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="01e24-158">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=8-11&name=snippet_EditGetOA)]

<span data-ttu-id="01e24-159">HttpPost değiştirin `Edit` office atama güncelleştirmeleri işlemek için aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="01e24-159">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

<span data-ttu-id="01e24-160">Kod şunları yapar:</span><span class="sxs-lookup"><span data-stu-id="01e24-160">The code does the following:</span></span>

* <span data-ttu-id="01e24-161">Yöntem adına değişiklikleri `EditPost` imza artık HttpGet aynı olduğu için `Edit` yöntemi ( `ActionName` özniteliği belirtir `/Edit/` URL'si hala kullanılmaktadır).</span><span class="sxs-lookup"><span data-stu-id="01e24-161">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

* <span data-ttu-id="01e24-162">Veritabanı kullanımından geçerli Eğitmen varlık için yükleme istekli alır `OfficeAssignment` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="01e24-162">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="01e24-163">Bu HttpGet ne yaptığınızı ile aynı olur `Edit` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="01e24-163">This is the same as what you did in the HttpGet `Edit` method.</span></span>

* <span data-ttu-id="01e24-164">Model bağlayıcı değerlerle alınan Eğitmen varlığı güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="01e24-164">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="01e24-165">`TryUpdateModel` Aşırı sağlar beyaz listeye eklemek istediğiniz özellikleri.</span><span class="sxs-lookup"><span data-stu-id="01e24-165">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="01e24-166">Bu aşırı posta açıklandığı şekilde engeller [ikinci öğreticide](crud.md).</span><span class="sxs-lookup"><span data-stu-id="01e24-166">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets don't play well with <ul> [!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```

* <span data-ttu-id="01e24-167">Ofis konumu boş ise, böylece OfficeAssignment tabloda ilgili satır silinecek null olarak Instructor.OfficeAssignment özelliği ayarlar.</span><span class="sxs-lookup"><span data-stu-id="01e24-167">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

* <span data-ttu-id="01e24-168">Değişiklikleri veritabanına kaydeder.</span><span class="sxs-lookup"><span data-stu-id="01e24-168">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="01e24-169">Eğitmen düzenleme görünümü güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="01e24-169">Update the Instructor Edit view</span></span>

<span data-ttu-id="01e24-170">İçinde *Views/Instructors/Edit.cshtml*, önce sonunda ofis konumu düzenlemek için yeni bir alan eklemek **Kaydet** düğmesi:</span><span class="sxs-lookup"><span data-stu-id="01e24-170">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

<span data-ttu-id="01e24-171">Uygulamayı çalıştırın, seçin **Eğitmenler** sekmesine ve ardından **Düzenle** bir eğitmen üzerinde.</span><span class="sxs-lookup"><span data-stu-id="01e24-171">Run the app, select the **Instructors** tab, and then click **Edit** on an instructor.</span></span> <span data-ttu-id="01e24-172">Değişiklik **ofis konumu** tıklatıp **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="01e24-172">Change the **Office Location** and click **Save**.</span></span>

![Eğitmen düzenleme sayfası](update-related-data/_static/instructor-edit-office.png)

## <a name="add-courses-to-edit-page"></a><span data-ttu-id="01e24-174">Kursları Düzen sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="01e24-174">Add courses to Edit page</span></span>

<span data-ttu-id="01e24-175">Eğitmenler kursları herhangi bir sayıda öğretin.</span><span class="sxs-lookup"><span data-stu-id="01e24-175">Instructors may teach any number of courses.</span></span> <span data-ttu-id="01e24-176">Artık aşağıdaki ekran görüntüsünde gösterildiği gibi bir grup onay kutularını kullanarak kurs atamalarını değiştirme olanağı ekleyerek Eğitmen Düzenle sayfasında geliştirmek:</span><span class="sxs-lookup"><span data-stu-id="01e24-176">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Eğitmen kurslarıyla düzenleme sayfası](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="01e24-178">Çoktan çoğa kurs ve Eğitmenler varlıklar arasındaki ilişkidir.</span><span class="sxs-lookup"><span data-stu-id="01e24-178">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="01e24-179">Ekleme ve ilişkilerini kaldırmak için eklemek ve kaldırmak için ve CourseAssignments birleştirme varlık kümesindeki varlıklar.</span><span class="sxs-lookup"><span data-stu-id="01e24-179">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="01e24-180">Hangi kursları değiştirmenize olanak sağlayan bir eğitmen UI'dir atanan onay kutularını grubudur.</span><span class="sxs-lookup"><span data-stu-id="01e24-180">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="01e24-181">Her kurs sonunda verilen veritabanında bir onay kutusu görüntülenir ve Eğitmen şu anda atandığı olanları seçilir.</span><span class="sxs-lookup"><span data-stu-id="01e24-181">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="01e24-182">Kullanıcı seçin veya kurs atamaları değiştirmek için onay kutularının işaretini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="01e24-182">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="01e24-183">Kursları sayısı çok büyük, görünüm verileri sunmak için farklı bir yöntem kullanmak istiyorsunuz, ancak oluşturmak veya ilişkileri silmek için bir birleşim varlık düzenleme aynı yöntemini kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="01e24-183">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="01e24-184">Eğitmenler denetleyici güncelleştirmesi</span><span class="sxs-lookup"><span data-stu-id="01e24-184">Update the Instructors controller</span></span>

<span data-ttu-id="01e24-185">Verileri görüntülemek için onay kutusu listesi sağlamak için bir görünüm modeli sınıfı kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="01e24-185">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="01e24-186">Oluşturma *AssignedCourseData.cs* içinde *SchoolViewModels* klasörü ve varolan kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="01e24-186">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="01e24-187">İçinde *InstructorsController.cs*, HttpGet değiştirin `Edit` yöntemini aşağıdaki kod ile.</span><span class="sxs-lookup"><span data-stu-id="01e24-187">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="01e24-188">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="01e24-188">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

<span data-ttu-id="01e24-189">İstekli yükleme için kod ekler `Courses` gezinme özelliğini ve yeni çağırır `PopulateAssignedCourseData` dizi onay kutusunu kullanma hakkında bilgi sağlamak için yöntemi `AssignedCourseData` model sınıfı görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="01e24-189">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="01e24-190">Kodda `PopulateAssignedCourseData` yöntemi görünüm model sınıfı kullanarak kurslar listesini yüklemek için tüm kurs varlıklar okur.</span><span class="sxs-lookup"><span data-stu-id="01e24-190">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="01e24-191">Her kurs için kod kursu Eğitmen içinde mevcut olup olmadığını denetler `Courses` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="01e24-191">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="01e24-192">Bir kurs eğitmen için atanmış olup olmadığını denetlerken verimli arama oluşturmak için eğitmen için atanan kursları içine yerleştirilir bir `HashSet` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="01e24-192">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="01e24-193">`Assigned` Özelliği true kurslara Eğitmen atanır.</span><span class="sxs-lookup"><span data-stu-id="01e24-193">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="01e24-194">Görünüm, bu özellik, hangi onay kutuları olarak görüntülenmelidir seçili belirlemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="01e24-194">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="01e24-195">Son olarak, liste görünümüne geçirilir `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="01e24-195">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="01e24-196">Ardından, kullanıcı tıkladığında yürütülen kod ekleme **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="01e24-196">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="01e24-197">Değiştirin `EditPost` yöntemini aşağıdaki kod ve güncelleştiren bir yeni yöntem ekleyin `Courses` Eğitmen varlık gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="01e24-197">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

<span data-ttu-id="01e24-198">Yöntem imzası HttpGet farklıdır `Edit` yöntemi için yöntem adını değişiklikleri `EditPost` geri `Edit`.</span><span class="sxs-lookup"><span data-stu-id="01e24-198">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="01e24-199">Görünüm kurs varlıklar koleksiyonu sahip olmadığından, model bağlayıcı otomatik olarak güncelleştirilemiyor `CourseAssignments` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="01e24-199">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="01e24-200">Güncelleştirilecek model bağlayıcısını kullanmak yerine `CourseAssignments` gezinti özelliği, yeni bunu `UpdateInstructorCourses` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="01e24-200">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="01e24-201">Bu nedenle hariç tutmak ihtiyacınız `CourseAssignments` model bağlama özelliği.</span><span class="sxs-lookup"><span data-stu-id="01e24-201">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="01e24-202">Bunu çağıran kodu herhangi bir değişiklik gerektirmez `TryUpdateModel` beyaz listeye ekleme aşırı kullandığınızdan ve `CourseAssignments` ekleme listesinde değil.</span><span class="sxs-lookup"><span data-stu-id="01e24-202">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="01e24-203">Onay kutuları seçilmedi, kodda `UpdateInstructorCourses` başlatır `CourseAssignments` gezinti özelliği boş bir koleksiyon ve döndürür:</span><span class="sxs-lookup"><span data-stu-id="01e24-203">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

<span data-ttu-id="01e24-204">Kod sonra veritabanındaki tüm kurslara döngü ve her kurs sonunda verilen görünümünde seçilen olanları karşı Eğitmen atanmış olanlar karşı denetler.</span><span class="sxs-lookup"><span data-stu-id="01e24-204">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="01e24-205">Verimli aramaları kolaylaştırmak için ikinci iki koleksiyon depolanır `HashSet` nesneleri.</span><span class="sxs-lookup"><span data-stu-id="01e24-205">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="01e24-206">Bir kurs için onay kutusu seçili ancak kursu değil `Instructor.CourseAssignments` gezinti özelliği, kurs gezinme özelliğini koleksiyona eklenir.</span><span class="sxs-lookup"><span data-stu-id="01e24-206">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

<span data-ttu-id="01e24-207">Bir kurs için onay kutusu seçili değildi, ancak kursu bulunduğu `Instructor.CourseAssignments` Gezinti özelliğine, kurs navigation özelliğinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="01e24-207">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a><span data-ttu-id="01e24-208">Eğitmen görünümleri güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="01e24-208">Update the Instructor views</span></span>

<span data-ttu-id="01e24-209">İçinde *Views/Instructors/Edit.cshtml*, ekleme bir **kursları** aşağıdakileri ekleyerek onay kutularını bir alan hemen sonra kod `div` için öğeleri **Office**  alan ve önce `div` öğesi için **Kaydet** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="01e24-209">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="01e24-210">Visual Studio'da kod yapıştırdığınızda, satır sonları kodları keser bir biçimde değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="01e24-210">When you paste the code in Visual Studio, line breaks might be changed in a way that breaks the code.</span></span> <span data-ttu-id="01e24-211">Kod yapıştırma sonrasında farklı görünüyor olsa da otomatik biçimlendirme geri almak için bir kez Ctrl + Z tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="01e24-211">If the code looks different after pasting, press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="01e24-212">Burada gördüğünüz gibi görünürler, bu satır sonları düzeltir.</span><span class="sxs-lookup"><span data-stu-id="01e24-212">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="01e24-213">Girinti mükemmel, olması gerekmez ancak `@</tr><tr>`, `@:<td>`, `@:</td>`, ve `@:</tr>` satırları her tek bir satırda gösterilen gibi olmalıdır veya bir çalışma zamanı hatası alırsınız.</span><span class="sxs-lookup"><span data-stu-id="01e24-213">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="01e24-214">Seçili yeni kod bloğu ile sekmesindeki yeni kodu mevcut kodu ile hizalamak için üç kez basın.</span><span class="sxs-lookup"><span data-stu-id="01e24-214">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="01e24-215">Bu sorun Visual Studio 2019 içinde düzeltilmiştir.</span><span class="sxs-lookup"><span data-stu-id="01e24-215">This problem is fixed in Visual Studio 2019.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

<span data-ttu-id="01e24-216">Bu kod, üç sütuna sahip bir HTML tablosu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="01e24-216">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="01e24-217">Her kurs numarası ve başlığı oluşan bir resim yazısı tarafından izlenen bir onay kutusu sütunundadır.</span><span class="sxs-lookup"><span data-stu-id="01e24-217">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="01e24-218">Tüm onay kutularını, model bağlayıcı grup olarak kabul edilmesi için oldukları konusunda bilgilendirir. aynı ada ("selectedCourses") sahip.</span><span class="sxs-lookup"><span data-stu-id="01e24-218">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they're to be treated as a group.</span></span> <span data-ttu-id="01e24-219">Her bir onay kutusu değeri öznitelik değeri olarak ayarlanır `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="01e24-219">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="01e24-220">Sayfa gönderildiğinde, model bağlayıcı dizisi içeren denetleyicisine geçirir. `CourseID` yalnızca, seçilen onay kutularına için değerler.</span><span class="sxs-lookup"><span data-stu-id="01e24-220">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="01e24-221">Onay kutularını başlangıçta işlendiğinde, bunları (bunları kullanıma gösterir) seçer kurslara eğitmen için atanmış olan öznitelikleri kullanıma.</span><span class="sxs-lookup"><span data-stu-id="01e24-221">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="01e24-222">Uygulamayı çalıştırın, seçin **Eğitmenler** sekmesine ve tıklayın **Düzenle** görmek için bir eğitmen üzerinde **Düzenle** sayfası.</span><span class="sxs-lookup"><span data-stu-id="01e24-222">Run the app, select the **Instructors** tab, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![Eğitmen kurslarıyla düzenleme sayfası](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="01e24-224">Bazı kurs atamaları değiştirin ve Kaydet'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="01e24-224">Change some course assignments and click Save.</span></span> <span data-ttu-id="01e24-225">Dizin sayfasında, yaptığınız değişiklikler yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="01e24-225">The changes you make are reflected on the Index page.</span></span>

> [!NOTE]
> <span data-ttu-id="01e24-226">Eğitmen kurs verileri düzenlemek için burada uygulanan yaklaşıma de sınırlı sayıda kursları olduğunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="01e24-226">The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="01e24-227">Farklı bir kullanıcı Arabirimi ve farklı bir güncelleştirme yöntemi, daha büyük olan koleksiyonları için gerekli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="01e24-227">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-delete-page"></a><span data-ttu-id="01e24-228">Güncelleştirme silme sayfası</span><span class="sxs-lookup"><span data-stu-id="01e24-228">Update Delete page</span></span>

<span data-ttu-id="01e24-229">İçinde *InstructorsController.cs*, silme `DeleteConfirmed` aşağıdaki kodu yerine yöntemi ve ekleme.</span><span class="sxs-lookup"><span data-stu-id="01e24-229">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

<span data-ttu-id="01e24-230">Bu kod, aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="01e24-230">This code makes the following changes:</span></span>

* <span data-ttu-id="01e24-231">İçin yükleme mu eager `CourseAssignments` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="01e24-231">Does eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="01e24-232">Bu eklemek zorunda veya EF hakkında ilgili bilmemektedir `CourseAssignment` varlıkları ve bunları silmez.</span><span class="sxs-lookup"><span data-stu-id="01e24-232">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span> <span data-ttu-id="01e24-233">Onları okumanıza gerek kalmadan Burada, art arda silme veritabanında yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="01e24-233">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="01e24-234">Eğitmen silinecek tüm bölümlerin bir yönetici olarak atanmış ise bu bölümlerden Eğitmen atama kaldırır.</span><span class="sxs-lookup"><span data-stu-id="01e24-234">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-create-page"></a><span data-ttu-id="01e24-235">Ofis konumu ve kurslar oluşturma sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="01e24-235">Add office location and courses to Create page</span></span>

<span data-ttu-id="01e24-236">İçinde *InstructorsController.cs*, HttpGet silip HttpPost `Create` yöntemleri ve bunun yerine aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="01e24-236">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

<span data-ttu-id="01e24-237">Bu kod ne için gördüğünüz üzere benzer `Edit` yöntemleri sağlayan kursları dışında seçilir.</span><span class="sxs-lookup"><span data-stu-id="01e24-237">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="01e24-238">HttpGet `Create` yöntem çağrılarını `PopulateAssignedCourseData` değil olabileceğinden ancak seçilmiş yöntemi sipariş sağlamak için boş bir koleksiyon için `foreach` döngü görünümünde (Aksi halde kodu görüntüle throw bir null başvurusu özel durumu).</span><span class="sxs-lookup"><span data-stu-id="01e24-238">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="01e24-239">HttpPost `Create` yöntemi ekler için seçilen her kurs sonunda verilen `CourseAssignments` gezinti özelliği önce doğrulama hatalarını denetler ve yeni Eğitmen veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="01e24-239">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="01e24-240">Olsa bile model hataları modeli hatalar (için örnek, geçersiz tarih Anahtarlanan kullanıcı) ve bir hata iletisiyle sayfası yeniden görüntülenir, yapılan herhangi bir kurs seçimi otomatik olarak geri yüklenir, böylece kursları eklenir.</span><span class="sxs-lookup"><span data-stu-id="01e24-240">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="01e24-241">Kursa eklemeniz mümkün olması için dikkat `CourseAssignments` Gezinti özelliğine sahip olarak boş bir koleksiyon özelliğini başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="01e24-241">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="01e24-242">Denetleyici kodda bunu alternatif olarak, eğitmen modelde mevcut değilse otomatik olarak koleksiyonu aşağıdaki örnekte gösterildiği gibi oluşturmak için özellik Get yordamı değiştirerek bunu:</span><span class="sxs-lookup"><span data-stu-id="01e24-242">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

```csharp
private ICollection<CourseAssignment> _courseAssignments;
public ICollection<CourseAssignment> CourseAssignments
{
    get
    {
        return _courseAssignments ?? (_courseAssignments = new List<CourseAssignment>());
    }
    set
    {
        _courseAssignments = value;
    }
}
```

<span data-ttu-id="01e24-243">Değiştirirseniz `CourseAssignments` özelliği bu şekilde, denetleyicinin açık özellik başlatma kodu kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="01e24-243">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="01e24-244">İçinde *Views/Instructor/Create.cshtml*, bir office konumu metin kutusu ekleyin ve Gönder düğmesini önce Kurslar için onay kutularını işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="01e24-244">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="01e24-245">Düzenleme sayfası olduğu gibi söz konusu olduğunda [Visual Studio, bu yapıştırdığınızda, kod yeniden biçimlendirir, biçimlendirme düzeltme](#notepad).</span><span class="sxs-lookup"><span data-stu-id="01e24-245">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

<span data-ttu-id="01e24-246">Uygulamayı çalıştıran ve bir eğitmen oluşturarak test edin.</span><span class="sxs-lookup"><span data-stu-id="01e24-246">Test by running the app and creating an instructor.</span></span>

## <a name="handling-transactions"></a><span data-ttu-id="01e24-247">İşlem işleme</span><span class="sxs-lookup"><span data-stu-id="01e24-247">Handling Transactions</span></span>

<span data-ttu-id="01e24-248">İçinde anlatıldığı gibi [CRUD öğretici](crud.md), Entity Framework örtük olarak işlemler uygular.</span><span class="sxs-lookup"><span data-stu-id="01e24-248">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="01e24-249">Daha denetlediğiniz--Örneğin, bir işlemde--Entity Framework dışında yapılan işlemler dahil etmek istiyorsanız senaryolar görmek için [işlemleri](/ef/core/saving/transactions).</span><span class="sxs-lookup"><span data-stu-id="01e24-249">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](/ef/core/saving/transactions).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="01e24-250">Kodu alma</span><span class="sxs-lookup"><span data-stu-id="01e24-250">Get the code</span></span>

[<span data-ttu-id="01e24-251">İndirme veya tamamlanmış uygulamanın görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="01e24-251">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a><span data-ttu-id="01e24-252">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="01e24-252">Next steps</span></span>

<span data-ttu-id="01e24-253">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="01e24-253">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="01e24-254">Özelleştirilmiş kursları sayfaları</span><span class="sxs-lookup"><span data-stu-id="01e24-254">Customized Courses pages</span></span>
> * <span data-ttu-id="01e24-255">Eğitmenler düzenleme sayfası eklendi</span><span class="sxs-lookup"><span data-stu-id="01e24-255">Added Instructors Edit page</span></span>
> * <span data-ttu-id="01e24-256">Düzen sayfasına eklenen kursları</span><span class="sxs-lookup"><span data-stu-id="01e24-256">Added courses to Edit page</span></span>
> * <span data-ttu-id="01e24-257">Güncelleştirilmiş silme sayfası</span><span class="sxs-lookup"><span data-stu-id="01e24-257">Updated Delete page</span></span>
> * <span data-ttu-id="01e24-258">Eklenen ofis konumu ve kurslar Oluştur sayfası</span><span class="sxs-lookup"><span data-stu-id="01e24-258">Added office location and courses to Create page</span></span>

<span data-ttu-id="01e24-259">Eşzamanlılık çakışmalarını işleme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="01e24-259">Advance to the next tutorial to learn how to handle concurrency conflicts.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="01e24-260">Eşzamanlılık çakışmalarını işleme</span><span class="sxs-lookup"><span data-stu-id="01e24-260">Handle concurrency conflicts</span></span>](concurrency.md)
