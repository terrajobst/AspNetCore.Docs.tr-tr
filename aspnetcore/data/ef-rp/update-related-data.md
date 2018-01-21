---
title: "Razor sayfalarının EF çekirdek ile-ilgili verileri - 8'in 7 güncelleştir"
author: rick-anderson
description: "Bu öğreticide yabancı anahtar alanları ve gezinti özellikleri güncelleştirerek ilgili verileri güncelleştirin."
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 817bfd48dce94e7dbad96cb6f822494e3adfae1d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a>İlgili verileri - EF çekirdek Razor sayfalarının (7 8'in) güncelleştirme

Tarafından [zel Dykstra](https://github.com/tdykstra), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Bu öğretici, ilgili verileri güncelleştirme gösterir. Olamaz çözmek sorunlarla karşılaşırsanız, indirme [Bu aşama için tamamlanan uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).

Aşağıdaki çizimler tamamlanmış sayfaları bazıları gösterir.

![İndirmelere düzenleme sayfasını](update-related-data/_static/course-edit.png)
![sayfasını Eğitmen Düzenle](update-related-data/_static/instructor-edit-courses.png)

İnceleyin ve oluşturma ve düzenleme indirmelere sayfaları test edin. Yeni bir indirmelere oluşturun. Departman birincil anahtara göre (tamsayı) adı değil seçilir. Yeni indirmelere düzenleyin. Testi tamamladığınızda, yeni indirmelere silin.

## <a name="create-a-base-class-to-share-common-code"></a>Ortak kodun paylaşmak için temel sınıf oluşturma

Kurslar/oluşturma ve kurslar/düzenleme sayfaları her bölüm adlarının bir listesini gerekir. Create *Pages/Courses/DepartmentNamePageModel.cshtml.cs* oluşturma ve düzenleme sayfalar için temel sınıf:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

Önceki kod oluşturur bir [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) bölüm adları listesi içerecek şekilde. Varsa `selectedDepartment` belirtilirse, bu bölüm seçildiğinde, `SelectList`.

Oluşturma ve düzenleme sayfası modeli sınıfları öğesinden türetilen `DepartmentNamePageModel`.

## <a name="customize-the-courses-pages"></a>Kurslar sayfalarını özelleştirme

Yeni bir indirmelere varlık oluşturulduğunda, var olan bir bölüm için bir ilişki olması gerekir. Bir indirmelere oluşturulurken bir bölüm eklemek için departman seçmek için aşağı açılan liste oluşturma ve düzenleme için temel sınıfı içerir. Aşağı açılan liste kümeleri `Course.DepartmentID` yabancı anahtar (FK) özelliği. EF çekirdek kullanan `Course.DepartmentID` yüklemek için FK `Department` gezinti özelliği.

![İndirmelere oluşturma](update-related-data/_static/ddl.png)

Oluştur sayfası modeli aşağıdaki kod ile güncelleştirin:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

Önceki kod:

* Türetilen `DepartmentNamePageModel`.
* Kullanan `TryUpdateModelAsync` önlemek için [overposting](xref:data/ef-rp/crud#overposting).
* Değiştirir `ViewData["DepartmentID"]` ile `DepartmentNameSL` (temel sınıfından).

`ViewData["DepartmentID"]`değiştirilir kesin türü belirtilmiş ile `DepartmentNameSL`. Kesin türü belirtilmiş modeller üzerinden zayıf yazılmış tercih ettiği. Daha fazla bilgi için bkz: [verileri (ViewData ve ViewBag)'zayıf yazılmış](xref:mvc/views/overview#VD_VB).

### <a name="update-the-courses-create-page"></a>Güncelleştirme kurslar oluşturma sayfası

Güncelleştirme *Pages/Courses/Create.cshtml* aşağıdaki biçimlendirme ile:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:

* Resim yazısı gelen değişiklikler **DepartmentID** için **departmanı**.
* Değiştirir `"ViewBag.DepartmentID"` ile `DepartmentNameSL` (temel sınıfından).
* "Select departmanı" seçeneği ekler. Bu değişiklik "Select departman" yerine ilk bölümü oluşturur.
* Departman seçili olmadığında bir doğrulama ileti ekler.

Razor sayfasını kullanan [seçin etiket Yardımcısı](xref:mvc/views/working-with-forms#the-select-tag-helper):

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

Oluştur sayfası sınayın. Oluştur sayfası bölüm kimliği yerine bölüm adını görüntüler

### <a name="update-the-courses-edit-page"></a>Sayfa kurslar Düzenle güncelleştirin.

Düzen sayfası modeli aşağıdaki kod ile güncelleştirin:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

Değişiklikleri Oluştur sayfası modelinde yapılan benzerdir. Önceki kod `PopulateDepartmentsDropDownList` , aşağı açılan listesinde belirtilen departmanı seçin bölüm kimliği, geçirir.

Güncelleştirme *Pages/Courses/Edit.cshtml* aşağıdaki biçimlendirme ile:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:

* İndirmelere kimliğini görüntüler Genellikle birincil anahtarı (PK) bir varlığın görüntülenmez. BA kullanıcılara genellikle anlamsızdır. Bu durumda, PK indirmelere sayısıdır.
* Resim yazısı gelen değişiklikler **DepartmentID** için **departmanı**.
* Değiştirir `"ViewBag.DepartmentID"` ile `DepartmentNameSL` (temel sınıfından).
* "Select departmanı" seçeneği ekler. Bu değişiklik "Select departman" yerine ilk bölümü oluşturur.
* Departman seçili olmadığında bir doğrulama ileti ekler.

Sayfa gizli bir alan içeriyor (`<input type="hidden">`) indirmelere numarası. Ekleme bir `<label>` Yardımcıyla etiketi `asp-for="Course.CourseID"` gizli alan gereksinimini ortadan kaldırmak değil. `<input type="hidden">`Kullanıcı tıklattığında gönderilen veriler dahil edilecek indirmelere numarası gereklidir **kaydetmek**.

Güncelleştirilmiş kod sınayın. Oluşturma, düzenleme ve bir indirmelere silin.

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>Ayrıntılara AsNoTracking ekleme ve silme sayfa modelleri

[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) izleme gerekli olmadığında, performansı artırabilir. Ekleme `AsNoTracking` silebilir ve Ayrıntılar sayfası modeli. Aşağıdaki kod güncelleştirilmiş Delete sayfa modeli gösterir:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

Güncelleştirme `OnGetAsync` yönteminde *Pages/Courses/Details.cshtml.cs* dosyası:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>Sil ve ayrıntıları sayfaları değiştirme

Aşağıdaki biçimlendirmede silme Razor sayfasıyla güncelleştirmesi:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

Ayrıntılar sayfası aynı değişiklikleri yapın.

### <a name="test-the-course-pages"></a>İndirmelere sayfalarını test

Test oluşturma, ayrıntı, düzenleme ve silme.

## <a name="update-the-instructor-pages"></a>Eğitmen sayfaları güncelleştir

Aşağıdaki bölümlerde Eğitmen sayfaları güncelleştirin.

### <a name="add-office-location"></a>Office Konumu Ekle

Eğitmen kaydı düzenlerken, eğitmen office atama güncelleştirmek isteyebilirsiniz. `Instructor` Varlık bir tane sıfır-veya-bire ilişkisine sahip olan `OfficeAssignment` varlık. Eğitmen kodu işlemesi gerekir:

* Kullanıcı office atama temizler, silin `OfficeAssignment` varlık.
* Kullanıcı bir office atama girer ve boş varsa, yeni bir oluşturun `OfficeAssignment` varlık.
* Kullanıcı office atama değiştirirse, güncelleştirme `OfficeAssignment` varlık.

Eğitmen Düzenle sayfası modeli aşağıdaki kod ile güncelleştirin:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

Önceki kod:

- Geçerli alır `Instructor` istekli yükleme için kullanarak veritabanını varlıktan `OfficeAssignment` gezinti özelliği.
- Alınan güncelleştirmeleri `Instructor` model bağlayıcı değerlerle varlık. `TryUpdateModel`engeller [overposting](xref:data/ef-rp/crud#overposting).
- Ofis konumu boş ise, ayarlar `Instructor.OfficeAssignment` null. Zaman `Instructor.OfficeAssignment` null, ilgili satırda olduğundan `OfficeAssignment` tablo silinir.

### <a name="update-the-instructor-edit-page"></a>Eğitmen düzenleme sayfasını güncelleştir

Güncelleştirme *Pages/Instructors/Edit.cshtml* office konum:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

Eğitmen ofis konumu değiştirebilir doğrulayın.

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Eğitmen Düzenle sayfasına indirmelere atamaları ekleme

Eğitmen kurslar herhangi bir sayıda öğretir. Bu bölümde, indirmelere atamalarını değiştirme yeteneğini ekleyin. Aşağıdaki resimde, düzenleme sayfasını güncelleştirilmiş Eğitmen gösterilmektedir:

![Kurslar Eğitmen Düzenle sayfası](update-related-data/_static/instructor-edit-courses.png)

`Course`ve `Instructor` bir çok-çok ilişkisi vardır. İlişkileri kaldırın ve eklemek için ekleme ve kaldırma varlıklardan `CourseAssignments` katılma varlık kümesi.

Onay kutuları bir eğitmen atandığı kurslar değişiklikler etkinleştirin. Her indirmelere veritabanındaki için bir onay kutusu görüntülenir. Eğitmen atandığı kurslar denetlenir. Kullanıcı seçin veya indirmelere atamalarını değiştirmek için onay kutularını temizleyin. Kurslar sayısı çok büyük ise:

* Farklı bir kullanıcı arabirimi, kursları görüntülemek için büyük olasılıkla kullanırsınız.
* İlişki oluşturma veya silme için birleştirme varlığa düzenleme yöntemi değiştirmemeniz.

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>Destek sınıfları eklemek oluşturma ve düzenleme Eğitmen sayfaları

Oluşturma *SchoolViewModels/AssignedCourseData.cs* aşağıdaki kod ile:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

`AssignedCourseData` Sınıfı bir eğitmen tarafından atanan kurslar onay kutularını oluşturmak için verileri içerir.

Oluşturma *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* temel sınıfı:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

`InstructorCoursesPageModel` Taban sınıf, düzenleme için kullanabilir ve sayfa modelleri oluşturun. `PopulateAssignedCourseData`Tüm okur `Course` doldurmak için varlıklar `AssignedCourseDataList`. Kod her Elbette, ayarlar `CourseID`, başlık ve karşılamadığını Eğitmen indirmelere atanır. A [Hashset'i](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) verimli aramalar oluşturmak için kullanılır.

### <a name="instructors-edit-page-model"></a>Eğitmen Düzenle sayfası modeli

Eğitmen Düzenle sayfası modeli aşağıdaki kod ile güncelleştirin:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

Önceki kod office atama değişiklikleri işler.

Razor görünüm Eğitmen güncelleştirin:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> Visual Studio'da kodu yapıştırın, satır sonları kodu keser bir biçimde değiştirilir. CTRL + Z otomatik biçimlendirme geri almak için bir kez basın. Burada gördüğünüz gibi göründükleri böylece Ctrl + Z satır sonları giderir. Girinti kusursuz, olmak zorunda değildir ancak `@</tr><tr>`, `@:<td>`, `@:</td>`, ve `@:</tr>` satırlarını her olmalıdır tek bir satıra gösterildiği gibi. Seçili yeni kod bloğu ile sekme yeni kodu var olan kodu ile hizalamak için üç kez basın. Oy ya da bu hata durumunu gözden [bu bağlantıyla](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

Önceki kod üç sütunları içeren bir HTML tablo oluşturur. Her sütunun bir onay kutusu ve indirmelere numarası ve başlığı içeren bir resim yazısı vardır. Tüm onay kutularını ("selectedCourses") aynı ada sahip. Aynı adı kullanarak bir grup olarak işlemek için model bağlayıcı bildirir. Value özniteliği her onay kutusunun kümesine `CourseID`. Sayfa gönderildiğinde, model bağlayıcı oluşan bir dizi geçirir `CourseID` yalnızca seçilen onay kutuları için değerleri.

Onay kutularını başlangıçta işlendiğinde, eğitmen atanan kurslar öznitelikleri kullanıma.

Uygulamayı çalıştırın ve güncelleştirilmiş Eğitmen düzenleme sayfasını sınayın. Bazı indirmelere atamalarını değiştirin. Dizin sayfasında değişiklikleri yansıtır.

Not: Eğitmen indirmelere verileri düzenlemek için burada uygulanan yaklaşıma iyi kurslar sınırlı sayıda olduğunda çalışır. Çok daha büyük olan koleksiyonları için farklı bir kullanıcı Arabirimi ve farklı bir güncelleştirme yöntemi daha kullanışlı ve etkili olacaktır.

### <a name="update-the-instructors-create-page"></a>Güncelleştirme Eğitmen Oluştur sayfası

Eğitmen Oluştur sayfası modeli aşağıdaki kod ile güncelleştirin:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

Önceki kod benzer *Pages/Instructors/Edit.cshtml.cs* kodu.

Eğitmen oluşturmak Razor sayfasını aşağıdaki biçimlendirme ile güncelleştirin:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

Eğitmen Oluştur sayfası sınayın.

## <a name="update-the-delete-page"></a>Güncelleştirme Sil sayfası

Delete sayfa modeli aşağıdaki kod ile güncelleştirin:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

Yukarıdaki kod aşağıdaki değişiklikleri yapar:

* İstekli yükleme için kullandığı `CourseAssignments` gezinti özelliği. `CourseAssignments`dahil edilmelidir veya Eğitmen silindiğinde silinmez. Bunları okuyun gerek önlemek için veritabanında art arda silme yapılandırın.

* Silinecek Eğitmen herhangi Departmanlar yönetici olarak atanmış ise bu bölümlerden Eğitmen atama kaldırır.

>[!div class="step-by-step"]
[Önceki](xref:data/ef-rp/read-related-data)
[sonraki](xref:data/ef-rp/concurrency)
