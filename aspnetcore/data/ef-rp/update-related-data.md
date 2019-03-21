---
title: ASP.NET core'da - EF çekirdekli Razor sayfaları, ilgili verileri - 7, 8 güncelleştirme
author: rick-anderson
description: Bu öğreticide yabancı anahtar alanları ve gezinti özellikleri güncelleştirerek ilgili verileri güncelleştirin.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/update-related-data
ms.openlocfilehash: e1407048157fccdbfdd854071a6fbf9785356026
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208615"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a>ASP.NET core'da - EF çekirdekli Razor sayfaları, ilgili verileri - 7, 8 güncelleştirme

Tarafından [Tom Dykstra](https://github.com/tdykstra), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

Bu öğreticide, ilgili verileri güncelleştirme gösterilmektedir. Olamaz çözmenize, sorunlarla karşılaşırsanız, [indirin veya tamamlanmış uygulamayı görüntüleyin.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Yükleme yönergeleri](xref:index#how-to-download-a-sample).

Aşağıdaki çizimler tamamlanmış sayfaların bazılarını gösterir.

![Kurs düzenleme sayfası](update-related-data/_static/course-edit.png)
![sayfasını Eğitmen Düzenle](update-related-data/_static/instructor-edit-courses.png)

İnceleyin ve oluşturma ve düzenleme kurs sayfaları test. Yeni bir kurs oluşturun. Departman birincil anahtara göre (tamsayı) adını seçilir. Yeni kursu düzenleyin. Testi bitirdikten sonra yeni kurs silin.

## <a name="create-a-base-class-to-share-common-code"></a>Ortak kod paylaşmak için bir temel sınıf oluşturma

Kursları/oluşturma ve kursları/düzenleme sayfaları her departman adlarının bir listesi gerekir. Oluşturma *Pages/Courses/DepartmentNamePageModel.cshtml.cs* oluşturma ve düzenleme sayfaları için temel sınıf:

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

Yukarıdaki kod oluşturur bir [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) bölüm adları listesi içerecek. Varsa `selectedDepartment` belirtilirse, bu bölümün seçili `SelectList`.

Sayfa modeli sınıfları oluşturma ve düzenleme türetilmesi `DepartmentNamePageModel`.

## <a name="customize-the-courses-pages"></a>Kursları sayfalarını özelleştirme

Yeni bir kurs varlık oluşturulduğunda, var olan bir bölüm arasında bir ilişki olması gerekir. Bir kurs oluştururken bir bölüm eklemek için departmanı seçmek için aşağı açılan liste oluşturma ve düzenleme için temel sınıfı içerir. Açılır listede kümeleri `Course.DepartmentID` yabancı anahtar (FK) özelliği. EF Core kullanan `Course.DepartmentID` yüklenecek FK `Department` gezinme özelliği.

![Kurs oluşturma](update-related-data/_static/ddl.png)

Oluşturma sayfa modeli aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

Yukarıdaki kod:

* Öğesinden türetilen `DepartmentNamePageModel`.
* Kullanan `TryUpdateModelAsync` önlemek için [overposting](xref:data/ef-rp/crud#overposting).
* Değiştirir `ViewData["DepartmentID"]` ile `DepartmentNameSL` (temel sınıftan).

`ViewData["DepartmentID"]` değiştirilir kesin olarak belirlenmiş ile `DepartmentNameSL`. Kesin olarak belirlenmiş Model üzerinden zayıf yazılmış tercih edilen. Daha fazla bilgi için [verileri (ViewData ve ViewBag)'zayıf yazılmış](xref:mvc/views/overview#VD_VB).

### <a name="update-the-courses-create-page"></a>Kurslar oluşturmaları sayfası

Güncelleştirme *Pages/Courses/Create.cshtml* aşağıdaki işaretlemeyle:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:

* Resim yazısı gelen değişiklikleri **DepartmentID** için **departmanı**.
* Değiştirir `"ViewBag.DepartmentID"` ile `DepartmentNameSL` (temel sınıftan).
* "Bölüm seçin" seçeneği ekler. Bu değişiklik, "Bölüm seçin" yerine ilk bölümü oluşturur.
* Departman seçili değilse, doğrulama iletisi ekler.

Razor sayfası kullanan [seçin etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-select-tag-helper):

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

Test oluşturma sayfası. Bölüm kimliği yerine bölüm adını Oluştur sayfasında görüntüler.

### <a name="update-the-courses-edit-page"></a>Kursları Düzenle sayfasında güncelleştirin.

Düzenleme sayfa modeli aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

Değişiklikleri Oluştur sayfası modelinde yapılan benzerdir. Önceki kodda, `PopulateDepartmentsDropDownList` , aşağı açılan listesinde belirtilen bölüm seçin bölüm kimliği, geçirir.

Güncelleştirme *Pages/Courses/Edit.cshtml* aşağıdaki işaretlemeyle:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:

* Kurs kimliğini görüntüler Genellikle birincil anahtarı (PK), bir varlığın görüntülenmiyor. Kullanıcılar genellikle anlamsız BA. Bu durumda, PK kurs sayısıdır.
* Resim yazısı gelen değişiklikleri **DepartmentID** için **departmanı**.
* Değiştirir `"ViewBag.DepartmentID"` ile `DepartmentNameSL` (temel sınıftan).

Sayfa gizli bir alan içeriyor (`<input type="hidden">`) kurs numarası. Ekleme bir `<label>` Yardımcıyla etiketi `asp-for="Course.CourseID"` gizli alan gerekmemesi değil. `<input type="hidden">` kullanıcı tıkladığında, deftere nakledilen verileri eklenmesi için kurs numarası gereklidir **Kaydet**.

Güncelleştirilen kodun test edin. Oluşturun, düzenleyin ve bir kurs silin.

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>Ayrıntılara AsNoTracking ekleme ve silme sayfası modelleri

[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) izleme zorunlu olmayan durumlarda performansını iyileştirebilir. Ekleme `AsNoTracking` silin ve Ayrıntıları sayfası modeli. Aşağıdaki kod, güncelleştirilmiş silme sayfa modeli gösterir:

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

Güncelleştirme `OnGetAsync` yönteminde *Pages/Courses/Details.cshtml.cs* dosyası:

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>Silme ve ayrıntıları sayfalarını değiştirin

Güncelleştirme aşağıdaki işaretlemeyle Sil Razor sayfası:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

Ayrıntıları sayfasına aynı değişiklikleri yapın.

### <a name="test-the-course-pages"></a>Kurs sayfaları test

Test oluşturma, Ayrıntılar, düzenleme ve silme.

## <a name="update-the-instructor-pages"></a>Eğitmen sayfaları güncelleştirme

Aşağıdaki bölümlerde, eğitmen sayfaları güncelleştirin.

### <a name="add-office-location"></a>Ofis konumu Ekle

Bir eğitmen kaydı düzenlerken, eğitmen ofis ataması güncelleştirmek isteyebilirsiniz. `Instructor` Varlığı ile bir sıfır-veya-bir ilişkisi vardır `OfficeAssignment` varlık. Eğitmen kodu işlemesi gerekir:

* Kullanıcının office atama temizler, silin `OfficeAssignment` varlık.
* Kullanıcı bir ofis ataması girer ve boş, yeni bir oluşturma `OfficeAssignment` varlık.
* Kullanıcı bir ofis ataması değişirse, güncelleştirme `OfficeAssignment` varlık.

Eğitmenler düzenleme sayfa modeli aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

Yukarıdaki kod:

* Geçerli alır `Instructor` istekli yükleme için kullanarak veritabanını bir varlıktan `OfficeAssignment` gezinme özelliği.
* Alınan güncelleştirmeler `Instructor` varlık değerlerle model bağlayıcı. `TryUpdateModel` engeller [overposting](xref:data/ef-rp/crud#overposting).
* Ofis konumu boş ise, ayarlar `Instructor.OfficeAssignment` null. Zaman `Instructor.OfficeAssignment` null, ilgili satırdaki olan `OfficeAssignment` tablo silinir.

### <a name="update-the-instructor-edit-page"></a>Eğitmen düzenleme sayfası

Güncelleştirme *Pages/Instructors/Edit.cshtml* office konum:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

Bir eğitmen ofis konumu değiştirebilirsiniz doğrulayın.

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Kurs atamaları Eğitmen Düzen sayfasına ekleme

Eğitmenler kursları herhangi bir sayıda öğretin. Bu bölümde, kurs atamalarını değiştirme özelliği ekleyin. Aşağıdaki görüntüde, güncelleştirilmiş Eğitmen düzenleme sayfası gösterilmektedir:

![Eğitmen kurslarıyla düzenleme sayfası](update-related-data/_static/instructor-edit-courses.png)

`Course` ve `Instructor` bir çok-çok ilişkisi vardır. Ekleme ve ilişkilerini kaldırmak için ekleyip varlıklardan `CourseAssignments` katılın varlık kümesi.

Değişiklikler bir eğitmen atandığı kursları onay kutularını işaretleyin. Her kurs sonunda verilen veritabanında bir onay kutusu görüntülenir. Eğitmen atandığı kursları denetlenir. Kullanıcı seçin veya kurs atamaları değiştirmek için onay kutularının işaretini kaldırın. Kursları sayısı çok büyük ise:

* Kursları görüntülemek için muhtemelen farklı bir kullanıcı arabiriminin kullanmanız gerekir.
* Bir birleştirme varlığı oluşturma veya ilişkileri silme düzenleme yöntemi değiştirmez.

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>Destekleyecek şekilde sınıfları eklemek oluşturma ve düzenleme Eğitmen sayfaları

Oluşturma *SchoolViewModels/AssignedCourseData.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

`AssignedCourseData` Sınıfı bir eğitmen tarafından atanan Kurslar için onay kutularını oluşturmak için veri içerir.

Oluşturma *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* temel sınıfı:

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

`InstructorCoursesPageModel` Temel sınıf için düzenleme kullanacağınızı ve sayfa modeller oluşturun. `PopulateAssignedCourseData` tüm `Course` doldurmak için varlıklar `AssignedCourseDataList`. Her kurs için kod ayarlar `CourseID`, başlık ve alınıp alınmayacağını Eğitmen kursa atanır. A [HashSet](/dotnet/api/system.collections.generic.hashset-1) verimli aramaları oluşturmak için kullanılır.

### <a name="instructors-edit-page-model"></a>Eğitmenler düzenleme sayfa modeli

Eğitmen düzenleme sayfa modeli aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

Yukarıdaki kod, office atama değişikliklerinin işler.

Razor görünümü Eğitmen güncelleştirin:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> Visual Studio'da kod yapıştırdığınızda, satır sonları kodları keser şekilde değiştirilir. CTRL + Z, otomatik biçimlendirme geri almak için bir kez basın. Burada gördüğünüz gibi görünürler, Ctrl + Z satır sonları düzeltir. Girinti mükemmel, olması gerekmez ancak `@</tr><tr>`, `@:<td>`, `@:</td>`, ve `@:</tr>` satırları her tek bir satırda gösterilen gibi olmalıdır. Seçili yeni kod bloğu ile sekmesindeki yeni kodu mevcut kodu ile hizalamak için üç kez basın. Oy kullanın veya bu hatanın durumunu gözden [bu bağlantıyla](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

Yukarıdaki kod, üç sütuna sahip bir HTML tablosu oluşturur. Her sütun, bir onay kutusu ve kursu sayısını ve başlığını içeren bir başlık sahiptir. Tüm onay kutularını ("selectedCourses") aynı ada sahip. Aynı adı kullanarak bir grup olarak değerlendirilecek model bağlayıcı bildirir. Her onay kutusunun değer özniteliği kümesine `CourseID`. Sayfa gönderildiğinde, model bağlayıcı oluşan bir dizi iletir `CourseID` yalnızca seçilen onay kutularına değerleri.

Onay kutularını başlangıçta işlendiğinde, eğitmen için atanan kursları öznitelikleri kullanıma.

Uygulamayı çalıştırın ve güncelleştirilmiş Eğitmenler düzenleme sayfası test edin. Bazı kurs atamalarını değiştirin. Dizin sayfasında değişiklikler yansıtılır.

Not: Eğitmen kurs verileri düzenlemek için burada uygulanan yaklaşıma de sınırlı sayıda kursları olduğunda çalışır. Çok daha büyük olan koleksiyonları için farklı bir kullanıcı Arabirimi ve farklı bir güncelleştirme yöntemi daha kullanılabilir ve daha verimli olacaktır.

### <a name="update-the-instructors-create-page"></a>Eğitmenler Oluştur sayfası

Eğitmen Oluştur sayfa modeli aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

Yukarıdaki kod benzer *Pages/Instructors/Edit.cshtml.cs* kod.

Eğitmen oluşturma Razor sayfası aşağıdaki biçimlendirme güncelleştirin:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

Eğitmen Oluştur sayfasında test edin.

## <a name="update-the-delete-page"></a>Silme sayfası

Delete sayfa modeli aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

Yukarıdaki kod, aşağıdaki değişiklikleri yapar:

* İstekli yükleme için kullandığı `CourseAssignments` gezinme özelliği. `CourseAssignments` dahil edilmesi gereken veya Eğitmen silindiğinde silinmez. Onları okumanıza gerek kalmadan için veritabanını art arda silme yapılandırın.

* Eğitmen silinecek tüm bölümlerin bir yönetici olarak atanmış ise bu bölümlerden Eğitmen atama kaldırır.

## <a name="additional-resources"></a>Ek kaynaklar

* [Bu öğreticide (Kısım 1) YouTube sürümü](https://www.youtube.com/watch?v=Csh6gkmwc9E)
* [Bu öğreticide (Bölüm 2) YouTube sürümü](https://www.youtube.com/watch?v=mOAankB_Zgc)

> [!div class="step-by-step"]
> [Önceki](xref:data/ef-rp/read-related-data)
> [İleri](xref:data/ef-rp/concurrency)
