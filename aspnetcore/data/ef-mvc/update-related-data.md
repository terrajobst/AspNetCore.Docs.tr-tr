---
title: ASP.NET Core MVC EF Core - güncelleştirme ile ilgili verileri - 10 7
author: rick-anderson
description: Bu öğreticide yabancı anahtar alanları ve gezinti özellikleri güncelleştirerek ilgili verileri güncelleştirin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: 37985c945f2e4b15cfcefb0c126c3209e0bdeac4
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090739"
---
# <a name="aspnet-core-mvc-with-ef-core---update-related-data---7-of-10"></a>ASP.NET Core MVC EF Core - güncelleştirme ile ilgili verileri - 10 7

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması, Entity Framework Core ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](intro.md).

Önceki öğreticide ilgili veriler görüntülenecek; Bu öğreticide yabancı anahtar alanları ve gezinti özellikleri güncelleştirerek ilgili verileri güncelleştirin.

Aşağıdaki çizimler ile çalışma sayfaları bazılarını göstermektedir.

![Kurs düzenleme sayfası](update-related-data/_static/course-edit.png)

![Eğitmen düzenleme sayfası](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Kursları oluşturma ve düzenleme sayfalarını özelleştirme

Yeni bir kurs varlık oluşturulduğunda, var olan bir bölüm arasında bir ilişki olması gerekir. Bunu kolaylaştırmak için iskele kurulan kodu denetleyici metotları ve departman seçmek için aşağı açılan listede yer oluşturma ve düzenleme görünümleri içerir. Açılır listede kümeleri `Course.DepartmentID` yabancı anahtar özellik ve tüm yük için Entity Framework gereken `Department` uygun departmanı varlık sahip gezinme özelliği. İskele kurulan kodu kullanır ancak biraz hata işleme eklemek ve açılan listeyi sıralamak için değiştirmeniz.

İçinde *CoursesController.cs*, dört oluşturma ve düzenleme yöntemlerini silin ve bunları aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

Sonra `Edit` HttpPost yöntemi departmanı bilgilerine aşağı açılan liste için yüklenen yeni bir yöntem oluşturun.

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

`PopulateDepartmentsDropDownList` Yöntemi tüm Departmanlar adına göre sıralanmış bir listesini alır, oluşturur bir `SelectList` koleksiyonu için bir açılan liste ve toplama görünümüne geçirir `ViewBag`. Yöntemi isteğe bağlı olarak kabul `selectedDepartment` açılır listede işlendiğinde seçilir öğeyi belirtmek çağıran kod veren parametresi. ' % S'adı "DepartmentID" geçirir görünümü `<select>` etiketi Yardımcısı ve yardımcı ardından bilir aranacak `ViewBag` nesnesi bir `SelectList` "DepartmentID" adlı.

HttpGet `Create` yöntem çağrılarını `PopulateDepartmentsDropDownList` için yeni bir kurs departman henüz kurulan değildir çünkü seçilen öğe ayarlama olmadan yöntemi:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

HttpGet `Edit` yöntemi düzenlenmekte olan kursa zaten atanmış bölüm Kimliğini temel öğeye ayarlar:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

Her ikisi için de HttpPost yöntemleri `Create` ve `Edit` de bunlar sayfanın bir hatanın ardından yeniden seçili öğe ayarlar kodu içerir. Bu hata mesajını göstermeye sayfası görüntülendiğinde için her bölüm seçilmedi seçili kalmasını sağlar.

### <a name="add-asnotracking-to-details-and-delete-methods"></a>Ekleyin. Ayrıntılar için AsNoTracking ve Delete metotlarını

Kurs Details ve Delete sayfaları, performansı iyileştirmek için ekleme `AsNoTracking` çağrıları `Details` ve HttpGet `Delete` yöntemleri.

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a>Kurs görünümleri değiştirin

İçinde *Views/Courses/Create.cshtml*, "Bölüm seçin" seçeneğine ekleyin **departmanı** aşağı açılan listesinde, gelen başlığını değiştirme **DepartmentID** için  **Departman**, doğrulama iletisi ekleyin.

[!code-html[](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

İçinde *Views/Courses/Edit.cshtml*, yaptığınız yalnızca bölüm alan için aynı değişikliği yapmanız *Create.cshtml*.

Ayrıca *Views/Courses/Edit.cshtml*, önce bir kurs sayı alan eklemek **başlık** alan. Kurs numarası birincil anahtarı olduğundan görüntülenir, ancak değiştirilemez.

[!code-html[](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

Gizli bir alan zaten var. (`<input type="hidden">`) düzenleme Görünümü'nde kurs numarası. Ekleme bir `<label>` etiketi Yardımcısı kullanıcı tıkladığında, deftere nakledilen verileri eklenecek kurs sayısı neden olmayan gizli alan gereksinimini ortadan değil **Kaydet** üzerinde **Düzenle** sayfası.

İçinde *Views/Courses/Delete.cshtml*, kurs sayı alanı en üstüne ekleyin ve bölüm kimliği departmanı adla değiştirin.

[!code-html[](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

İçinde *Views/Courses/Details.cshtml*, yalnızca yaptığınız için aynı değişikliği yapmanız *Delete.cshtml*.

### <a name="test-the-course-pages"></a>Kurs sayfaları test

Uygulamayı çalıştırın, seçin **kursları** sekmesinde **Yeni Oluştur**, yeni bir kurs için veri girin:

![Kurs oluşturma sayfası](update-related-data/_static/course-create.png)

**Oluştur**'u tıklatın. Listeye eklenen yeni kurs ile kursları dizin sayfası görüntülenir. Bölüm adı dizin sayfası listesinde ilişki doğru şekilde kurulduğundan gösteren navigation özelliğinden gelir.

Tıklayın **Düzenle** kursları dizin sayfası kurs üzerinde.

![Kurs düzenleme sayfası](update-related-data/_static/course-edit.png)

Sayfadaki verileri değiştirip'ı **Kaydet**. Güncelleştirilmiş kurs verilerle kursları dizin sayfası görüntülenir.

## <a name="add-an-edit-page-for-instructors"></a>Eğitmen için bir düzen sayfası Ekle

Bir eğitmen kaydı düzenlediğinizde, eğitmen ofis ataması güncelleştirilecek yönetebilmek istiyorsunuz. Eğitmen varlık aşağıdaki durumlarda işlemek kodunuzu sahip olduğu anlamına gelir OfficeAssignment varlıkla biri sıfır-veya-bir ilişkisi vardır:

* Kullanıcı ofis ataması temizler ve ilk olarak bir değer atanmayan OfficeAssignment varlığı silin.

* Kullanıcı bir office atama değer girer ve başlangıçta boş, yeni OfficeAssignment varlık oluşturun.

* Kullanıcı bir ofis ataması değeri değişirse, var olan bir OfficeAssignment varlık değeri değiştirin.

### <a name="update-the-instructors-controller"></a>Eğitmenler denetleyici güncelleştirmesi

İçinde *InstructorsController.cs*, HttpGet kodda değişiklik `Edit` BT'nin Eğitmen varlığın yükler. Bu nedenle yöntemi `OfficeAssignment` gezinti özelliği ve çağrıları `AsNoTracking`:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

HttpPost değiştirin `Edit` office atama güncelleştirmeleri işlemek için aşağıdaki kod ile yöntemi:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

Kod şunları yapar:

-  Yöntem adına değişiklikleri `EditPost` imza artık HttpGet aynı olduğu için `Edit` yöntemi ( `ActionName` özniteliği belirtir `/Edit/` URL'si hala kullanılmaktadır).

-  Veritabanı kullanımından geçerli Eğitmen varlık için yükleme istekli alır `OfficeAssignment` gezinme özelliği. Bu HttpGet ne yaptığınızı ile aynı olur `Edit` yöntemi.

-  Model bağlayıcı değerlerle alınan Eğitmen varlığı güncelleştirir. `TryUpdateModel` Aşırı sağlar beyaz listeye eklemek istediğiniz özellikleri. Bu aşırı posta açıklandığı şekilde engeller [ikinci öğreticide](crud.md).

    <!-- Snippets don't play well with <ul> [!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```

-   Ofis konumu boş ise, böylece OfficeAssignment tabloda ilgili satır silinecek null olarak Instructor.OfficeAssignment özelliği ayarlar.

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- Değişiklikleri veritabanına kaydeder.

### <a name="update-the-instructor-edit-view"></a>Eğitmen düzenleme görünümü güncelleştirme

İçinde *Views/Instructors/Edit.cshtml*, önce sonunda ofis konumu düzenlemek için yeni bir alan eklemek **Kaydet** düğmesi:

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

Uygulamayı çalıştırın, seçin **Eğitmenler** sekmesine ve ardından **Düzenle** bir eğitmen üzerinde. Değişiklik **ofis konumu** tıklatıp **Kaydet**.

![Eğitmen düzenleme sayfası](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Kurs atamaları Eğitmen Düzen sayfasına ekleme

Eğitmenler kursları herhangi bir sayıda öğretin. Artık aşağıdaki ekran görüntüsünde gösterildiği gibi bir grup onay kutularını kullanarak kurs atamalarını değiştirme olanağı ekleyerek Eğitmen Düzenle sayfasında geliştirmek:

![Eğitmen kurslarıyla düzenleme sayfası](update-related-data/_static/instructor-edit-courses.png)

Çoktan çoğa kurs ve Eğitmenler varlıklar arasındaki ilişkidir. Ekleme ve ilişkilerini kaldırmak için eklemek ve kaldırmak için ve CourseAssignments birleştirme varlık kümesindeki varlıklar.

Hangi kursları değiştirmenize olanak sağlayan bir eğitmen UI'dir atanan onay kutularını grubudur. Her kurs sonunda verilen veritabanında bir onay kutusu görüntülenir ve Eğitmen şu anda atandığı olanları seçilir. Kullanıcı seçin veya kurs atamaları değiştirmek için onay kutularının işaretini kaldırın. Kursları sayısı çok büyük, görünüm verileri sunmak için farklı bir yöntem kullanmak istiyorsunuz, ancak oluşturmak veya ilişkileri silmek için bir birleşim varlık düzenleme aynı yöntemini kullanırsınız.

### <a name="update-the-instructors-controller"></a>Eğitmenler denetleyici güncelleştirmesi

Verileri görüntülemek için onay kutusu listesi sağlamak için bir görünüm modeli sınıfı kullanacaksınız.

Oluşturma *AssignedCourseData.cs* içinde *SchoolViewModels* klasörü ve varolan kodu aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

İçinde *InstructorsController.cs*, HttpGet değiştirin `Edit` yöntemini aşağıdaki kod ile. Değişiklikler vurgulanır.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

İstekli yükleme için kod ekler `Courses` gezinme özelliğini ve yeni çağırır `PopulateAssignedCourseData` dizi onay kutusunu kullanma hakkında bilgi sağlamak için yöntemi `AssignedCourseData` model sınıfı görüntüleyin.

Kodda `PopulateAssignedCourseData` yöntemi görünüm model sınıfı kullanarak kurslar listesini yüklemek için tüm kurs varlıklar okur. Her kurs için kod kursu Eğitmen içinde mevcut olup olmadığını denetler `Courses` gezinme özelliği. Bir kurs eğitmen için atanmış olup olmadığını denetlerken verimli arama oluşturmak için eğitmen için atanan kursları içine yerleştirilir bir `HashSet` koleksiyonu. `Assigned` Özelliği true kurslara Eğitmen atanır. Görünüm, bu özellik, hangi onay kutuları olarak görüntülenmelidir seçili belirlemek için kullanır. Son olarak, liste görünümüne geçirilir `ViewData`.

Ardından, kullanıcı tıkladığında yürütülen kod ekleme **Kaydet**. Değiştirin `EditPost` yöntemini aşağıdaki kod ve güncelleştiren bir yeni yöntem ekleyin `Courses` Eğitmen varlık gezinme özelliği.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

Yöntem imzası HttpGet farklıdır `Edit` yöntemi için yöntem adını değişiklikleri `EditPost` geri `Edit`.

Görünüm kurs varlıklar koleksiyonu sahip olmadığından, model bağlayıcı otomatik olarak güncelleştirilemiyor `CourseAssignments` gezinme özelliği. Güncelleştirilecek model bağlayıcısını kullanmak yerine `CourseAssignments` gezinti özelliği, yeni bunu `UpdateInstructorCourses` yöntemi. Bu nedenle hariç tutmak ihtiyacınız `CourseAssignments` model bağlama özelliği. Bunu çağıran kodu herhangi bir değişiklik gerektirmez `TryUpdateModel` beyaz listeye ekleme aşırı kullandığınızdan ve `CourseAssignments` ekleme listesinde değil.

Onay kutuları seçilmedi, kodda `UpdateInstructorCourses` başlatır `CourseAssignments` gezinti özelliği boş bir koleksiyon ve döndürür:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

Kod sonra veritabanındaki tüm kurslara döngü ve her kurs sonunda verilen görünümünde seçilen olanları karşı Eğitmen atanmış olanlar karşı denetler. Verimli aramaları kolaylaştırmak için ikinci iki koleksiyon depolanır `HashSet` nesneleri.

Bir kurs için onay kutusu seçili ancak kursu değil `Instructor.CourseAssignments` gezinti özelliği, kurs gezinme özelliğini koleksiyona eklenir.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

Bir kurs için onay kutusu seçili değildi, ancak kursu bulunduğu `Instructor.CourseAssignments` Gezinti özelliğine, kurs navigation özelliğinden kaldırılır.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a>Eğitmen görünümleri güncelleştirme

İçinde *Views/Instructors/Edit.cshtml*, ekleme bir **kursları** aşağıdakileri ekleyerek onay kutularını bir alan hemen sonra kod `div` için öğeleri **Office**  alan ve önce `div` öğesi için **Kaydet** düğmesi.

<a id="notepad"></a>
> [!NOTE]
> Visual Studio'da kod yapıştırdığınızda, satır sonları kodları keser şekilde değiştirilecektir.  CTRL + Z, otomatik biçimlendirme geri almak için bir kez basın.  Burada gördüğünüz gibi görünürler, bu satır sonları düzeltir. Girinti mükemmel, olması gerekmez ancak `@</tr><tr>`, `@:<td>`, `@:</td>`, ve `@:</tr>` satırları her tek bir satırda gösterilen gibi olmalıdır veya bir çalışma zamanı hatası alırsınız. Seçili yeni kod bloğu ile sekmesindeki yeni kodu mevcut kodu ile hizalamak için üç kez basın. Bu sorunun durumu kontrol edebilirsiniz [burada](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

Bu kod, üç sütuna sahip bir HTML tablosu oluşturur. Her kurs numarası ve başlığı oluşan bir resim yazısı tarafından izlenen bir onay kutusu sütunundadır. Tüm onay kutularını, model bağlayıcı grup olarak kabul edilmesi için oldukları konusunda bilgilendirir. aynı ada ("selectedCourses") sahip. Her bir onay kutusu değeri öznitelik değeri olarak ayarlanır `CourseID`. Sayfa gönderildiğinde, model bağlayıcı dizisi içeren denetleyicisine geçirir. `CourseID` yalnızca, seçilen onay kutularına için değerler.

Onay kutularını başlangıçta işlendiğinde, bunları (bunları kullanıma gösterir) seçer kurslara eğitmen için atanmış olan öznitelikleri kullanıma.

Uygulamayı çalıştırın, seçin **Eğitmenler** sekmesine ve tıklayın **Düzenle** görmek için bir eğitmen üzerinde **Düzenle** sayfası.

![Eğitmen kurslarıyla düzenleme sayfası](update-related-data/_static/instructor-edit-courses.png)

Bazı kurs atamaları değiştirin ve Kaydet'e tıklayın. Dizin sayfasında, yaptığınız değişiklikler yansıtılır.

> [!NOTE]
> Eğitmen kurs verileri düzenlemek için burada uygulanan yaklaşıma de sınırlı sayıda kursları olduğunda çalışır. Farklı bir kullanıcı Arabirimi ve farklı bir güncelleştirme yöntemi, daha büyük olan koleksiyonları için gerekli olacaktır.

## <a name="update-the-delete-page"></a>Silme sayfası

İçinde *InstructorsController.cs*, silme `DeleteConfirmed` aşağıdaki kodu yerine yöntemi ve ekleme.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

Bu kod, aşağıdaki değişiklikleri yapar:

* İçin yükleme mu eager `CourseAssignments` gezinme özelliği.  Bu eklemek zorunda veya EF hakkında ilgili bilmemektedir `CourseAssignment` varlıkları ve bunları silmez.  Onları okumanıza gerek kalmadan Burada, art arda silme veritabanında yapılandırabilirsiniz.

* Eğitmen silinecek tüm bölümlerin bir yönetici olarak atanmış ise bu bölümlerden Eğitmen atama kaldırır.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Ofis konumu ve kurslar oluşturma sayfasına ekleme

İçinde *InstructorsController.cs*, HttpGet silip HttpPost `Create` yöntemleri ve bunun yerine aşağıdaki kodu ekleyin:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

Bu kod ne için gördüğünüz üzere benzer `Edit` yöntemleri sağlayan kursları dışında seçilir. HttpGet `Create` yöntem çağrılarını `PopulateAssignedCourseData` değil olabileceğinden ancak seçilmiş yöntemi sipariş sağlamak için boş bir koleksiyon için `foreach` döngü görünümünde (Aksi halde kodu görüntüle throw bir null başvurusu özel durumu).

HttpPost `Create` yöntemi ekler için seçilen her kurs sonunda verilen `CourseAssignments` gezinti özelliği önce doğrulama hatalarını denetler ve yeni Eğitmen veritabanına ekler. Olsa bile model hataları modeli hatalar (için örnek, geçersiz tarih Anahtarlanan kullanıcı) ve bir hata iletisiyle sayfası yeniden görüntülenir, yapılan herhangi bir kurs seçimi otomatik olarak geri yüklenir, böylece kursları eklenir.

Kursa eklemeniz mümkün olması için dikkat `CourseAssignments` Gezinti özelliğine sahip olarak boş bir koleksiyon özelliğini başlatmak için:

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

Denetleyici kodda bunu alternatif olarak, eğitmen modelde mevcut değilse otomatik olarak koleksiyonu aşağıdaki örnekte gösterildiği gibi oluşturmak için özellik Get yordamı değiştirerek bunu:

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

Değiştirirseniz `CourseAssignments` özelliği bu şekilde, denetleyicinin açık özellik başlatma kodu kaldırabilirsiniz.

İçinde *Views/Instructor/Create.cshtml*, bir office konumu metin kutusu ekleyin ve Gönder düğmesini önce Kurslar için onay kutularını işaretleyin. Düzenleme sayfası olduğu gibi söz konusu olduğunda [Visual Studio, bu yapıştırdığınızda, kod yeniden biçimlendirir, biçimlendirme düzeltme](#notepad).

[!code-html[](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

Uygulamayı çalıştıran ve bir eğitmen oluşturarak test edin.

## <a name="handling-transactions"></a>İşlem işleme

İçinde anlatıldığı gibi [CRUD öğretici](crud.md), Entity Framework örtük olarak işlemler uygular. Daha denetlediğiniz--Örneğin, bir işlemde--Entity Framework dışında yapılan işlemler dahil etmek istiyorsanız senaryolar görmek için [işlemleri](/ef/core/saving/transactions).

## <a name="summary"></a>Özet

İlgili verilerle çalışmaya giriş tamamladınız. Sonraki öğreticide, eşzamanlılık çakışmalarını nasıl ele alınacağını görürsünüz.

::: moniker-end

> [!div class="step-by-step"]
> [Önceki](read-related-data.md)
> [İleri](concurrency.md)
