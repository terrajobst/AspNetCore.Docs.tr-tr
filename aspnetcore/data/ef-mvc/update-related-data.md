---
title: "Veri - 10 7 ASP.NET Core MVC EF çekirdek - güncelleştirme ile ilgili"
author: tdykstra
description: "Bu öğreticide yabancı anahtar alanları ve gezinti özellikleri güncelleştirerek ilgili verileri güncelleştirin."
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: ac9dc6f08bbcd890c5848e7cc5cb4ee93713a559
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="updating-related-data---ef-core-with-aspnet-core-mvc-tutorial-7-of-10"></a>İlgili verileri - EF çekirdek ASP.NET Core MVC Öğreticisi (10 7) ile güncelleştirme

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması Entity Framework Çekirdek ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](intro.md).

Önceki öğreticide ilgili verileri görüntülenir; Bu öğreticide yabancı anahtar alanları ve gezinti özellikleri güncelleştirerek ilgili verileri güncelleştirin.

Aşağıdaki çizimler ile çalışma sayfaları bazıları göstermektedir.

![İndirmelere Düzenle sayfası](update-related-data/_static/course-edit.png)

![Eğitmen Düzenle sayfası](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Oluşturma ve düzenleme sayfaları kursları özelleştirme

Yeni bir indirmelere varlık oluşturulduğunda, var olan bir bölüm için bir ilişki olması gerekir. Bu kolaylaştırmak için kurulmuş kodu denetleyicisi yöntemleri ve departman seçmek için aşağı açılan listede yer oluşturma ve düzenleme görünümleri içerir. Aşağı açılan liste kümeleri `Course.DepartmentID` yabancı anahtar özellik ve tüm Entity Framework gereken yüklemek için olan `Department` uygun departmanı varlığı ile gezinti özelliği. Kurulmuş kodu kullanın ancak biraz hata işleme ekleme ve açılan listeyi sıralayın değiştirmeniz.

İçinde *CoursesController.cs*, dört oluşturma ve düzenleme yöntemlerini silin ve bunları aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

Sonra `Edit` HttpPost yöntemi, aşağı açılan liste departmanı bilgilerini yükler yeni bir yöntem oluşturun.

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

`PopulateDepartmentsDropDownList` Yöntemi adına göre sıralanmış tüm bölümleri listesini alır, oluşturur bir `SelectList` açılır listesi, koleksiyon ve koleksiyon görünümüne geçirir `ViewBag`. Yöntemi isteğe bağlı kabul `selectedDepartment` aşağı açılan liste işlendiğinde seçilir öğeyi seçmek arama kodu verir parametresi. Adı "DepartmentID" geçirir görünümü `<select>` etiket Yardımcısı ve yardımcı sonra bilir Bakılacak `ViewBag` için nesne bir `SelectList` "DepartmentID" adlı.

HttpGet `Create` yöntem çağrılarını `PopulateDepartmentsDropDownList` için yeni bir indirmelere departman henüz kurulan değil çünkü seçili öğe ayarlama olmadan yöntemi:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

HttpGet `Edit` yöntemi, seçili öğe düzenlenmekte indirmelere zaten atanmış bölüm Kimliğini temel ayarlar:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

Her ikisi için de HttpPost yöntemleri `Create` ve `Edit` da bunlar sayfa bir hatanın ardından yeniden görüntüleyin seçili öğe ayarlar kodunu içerir. Bu hata iletisini görüntülemek için sayfanın görüntülendiğinde için hangi departmanı seçilmedi seçili kalmasını sağlar.

### <a name="add-asnotracking-to-details-and-delete-methods"></a>Ekleyin. Ayrıntılar için AsNoTracking ve silme yöntemleri

İndirmelere ayrıntıları ve Delete sayfaları performansını iyileştirmek için ekleme `AsNoTracking` çağrıları `Details` ve HttpGet `Delete` yöntemleri.

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a>İndirmelere görünümleri değiştirme

İçinde *Views/Courses/Create.cshtml*, "Select departmanı" seçeneğine ekleyin **departmanı** aşağı açılan listesinde, gelen başlığını değiştirme **DepartmentID** için  **Departman**ve bir doğrulama ileti ekleyin.

[!code-html[](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

İçinde *Views/Courses/Edit.cshtml*, yaptığınız yalnızca bölüm alan için aynı değişiklik *Create.cshtml*.

De *Views/Courses/Edit.cshtml*, önce bir indirmelere numarası alanı ekleme **başlık** alan. İndirmelere birincil anahtar olduğundan, bu görüntülenir, ancak değiştirilemez.

[!code-html[](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

Gizli bir alan zaten var. (`<input type="hidden">`) düzenleme görünümü indirmelere numaralı. Ekleme bir `<label>` kullanıcı tıklattığında gönderilen veriler dahil edilecek indirmelere numarası neden değil, etiket Yardımcısı gizli alan gereksinimini ortadan değil **kaydetmek** üzerinde **Düzenle** sayfası.

İçinde *Views/Courses/Delete.cshtml*, indirmelere sayı alanı üstüne ekleyin ve departman adına bölüm Kimliğini değiştirin.

[!code-html[](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

İçinde *Views/Courses/Details.cshtml*, yalnızca yaptığınız için aynı değişiklik *Delete.cshtml*.

### <a name="test-the-course-pages"></a>İndirmelere sayfalarını test

Uygulama, belirleyin **kurslar** sekmesini tıklatın, **Yeni Oluştur**ve verileri için yeni bir indirmelere girin:

![İndirmelere Oluştur sayfası](update-related-data/_static/course-create.png)

**Oluştur**'u tıklatın. Listeye eklenen yeni indirmelere kurslar dizin sayfası görüntülenir. Bölüm adı dizin sayfası listesinde ilişki doğru şekilde kurulduğundan gösteren Gezinti özelliğinden gelir.

Tıklatın **Düzenle** indirmelere kurslar dizin sayfasındaki üzerinde.

![İndirmelere Düzenle sayfası](update-related-data/_static/course-edit.png)

Sayfadaki verileri değiştirip'ı **kaydetmek**. Güncelleştirilmiş indirmelere verilerle kurslar dizin sayfası görüntülenir.

## <a name="add-an-edit-page-for-instructors"></a>Eğitmen için bir düzen sayfası Ekle

Eğitmen kaydı düzenlediğinizde, eğitmen office atama güncelleştirmek kullanabilmek ister. Eğitmen entity aşağıdaki durumlarda işlemek kodunuzu olduğu anlamına gelir OfficeAssignment varlık ile bir-sıfır-veya-bir ilişkisi vardır:

* İlk olarak bir değere sahip ve kullanıcı office atama temizler, OfficeAssignment varlığı silin.

* Kullanıcı bir office atama değeri girer ve başlangıçta boş varsa, yeni bir OfficeAssignment varlık oluşturun.

* Kullanıcı bir office atama değeri değiştirirse, var olan bir OfficeAssignment varlığı değeri değiştirin.

### <a name="update-the-instructors-controller"></a>Eğitmen denetleyicisini güncelleştirin

İçinde *InstructorsController.cs*, HttpGet kodda değişiklik `Edit` onun Eğitmen varlığın yükler şekilde yöntemi `OfficeAssignment` gezinti özelliği ve çağrıları `AsNoTracking`:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

HttpPost Değiştir `Edit` office atama güncelleştirmeleri işlemek için aşağıdaki kod ile yöntemi:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

Kod şunları yapar:

-  Yöntem adı değiştirir `EditPost` imza artık HttpGet aynı olduğundan `Edit` yöntemi ( `ActionName` özniteliği belirtir `/Edit/` URL hala kullanılan).

-  Veritabanı kullanımından geçerli Eğitmen varlık için yükleme istekli alır `OfficeAssignment` gezinti özelliği. Bu HttpGet yaptıklarınızın aynıdır `Edit` yöntemi.

-  Model bağlayıcı değerlerle alınan Eğitmen varlığı güncelleştirir. `TryUpdateModel` Aşırı sağlar beyaz liste ile dahil etmek istediğiniz özellikleri. Bu açıklandığı gibi atlayarak nakil engeller [ikinci Öğreticisi](crud.md).

    <!-- Snippets don't play well with <ul> [!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   Ofis konumu boş ise, böylece OfficeAssignment tablodaki ilgili satırda silinecek null olarak Instructor.OfficeAssignment özelliğini ayarlar.

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- Değişiklikleri veritabanına kaydeder.

### <a name="update-the-instructor-edit-view"></a>Güncelleştirme görünümü Eğitmen Düzenle

İçinde *Views/Instructors/Edit.cshtml*, ofis konumunda sona önce düzenlemek için yeni bir alan eklemek **kaydetmek** düğmesi:

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

Uygulama, belirleyin **Eğitmen** sekmesini ve ardından **Düzenle** bir eğitmen üzerinde. Değişiklik **ofis konumu** tıklatıp **kaydetmek**.

![Eğitmen Düzenle sayfası](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Eğitmen Düzenle sayfasına indirmelere atamaları ekleme

Eğitmen kurslar herhangi bir sayıda öğretir. Şimdi aşağıdaki ekran görüntüsünde gösterildiği gibi onay kutuları, bir grup kullanarak indirmelere atamalarını değiştirme yeteneğini ekleyerek sayfasını Eğitmen Düzenle geliştirmek:

![Kurslar Eğitmen Düzenle sayfası](update-related-data/_static/instructor-edit-courses.png)

Çok-indirmelere ve Eğitmen varlıklar arasındaki ilişkidir. İlişkileri kaldırın ve eklemek için Ekle ve varlıkları CourseAssignments birleştirme varlık kümesinden ve kaldırın.

Hangi kurslar değiştirmenize olanak sağlayan bir eğitmen UI'dir atanan onay kutularını grubudur. Her indirmelere veritabanındaki bir onay kutusu görüntülenir ve Eğitmen şu anda atanmış olanları seçilir. Kullanıcı seçin veya indirmelere atamalarını değiştirmek için onay kutularını temizleyin. Kurslar sayısı çok büyük, büyük olasılıkla veri görünümü sunan, farklı bir yöntem kullanmak isteyebilirsiniz, ancak ilişki oluşturma veya silme için bir birleşim varlık düzenleme için aynı yöntem kullanırsınız.

### <a name="update-the-instructors-controller"></a>Eğitmen denetleyicisini güncelleştirin

Onay kutuları listesi için veri görünümüne sağlamak için bir görünüm model sınıfı kullanacaksınız.

Oluşturma *AssignedCourseData.cs* içinde *SchoolViewModels* klasörü ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

İçinde *InstructorsController.cs*, HttpGet Değiştir `Edit` aşağıdaki kod ile yöntemi. Değişiklikler vurgulanır.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

İstekli yükleme için kod ekler `Courses` gezinti özelliği ve yeni çağırır `PopulateAssignedCourseData` onay kutusunu dizi kullanma hakkında bilgi sağlamak için yöntemi `AssignedCourseData` görüntülemek model sınıfı.

Kodda `PopulateAssignedCourseData` yöntemi görünüm model sınıfı kullanarak kurslar yüklenmesi için tüm indirmelere varlıklar okur. Her Elbette, kod indirmelere Eğitmen içinde var olup olmadığını denetler `Courses` gezinti özelliği. Bir indirmelere Eğitmen atanmış olup olmadığını denetlerken verimli arama oluşturmak için Eğitmen atanan kurslar içine konur bir `HashSet` koleksiyonu. `Assigned` Özelliği ayarlanmış kurslara true Eğitmen atanır. Görünüm, hangi onay kutuları olarak görüntülenmelidir seçili belirlemek için bu özelliği kullanır. Son olarak, liste görünümünde geçirilecek `ViewData`.

Ardından, kullanıcı tıklattığında yürütülen kod ekleme **kaydetmek**. Değiştir `EditPost` yöntemi aşağıdaki kod ve güncelleştirmeleri yeni bir yöntem ekleyin `Courses` Eğitmen varlık gezinti özelliği.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

Yöntem imzası HttpGet farklıdır `Edit` yöntem adını değişiklikleri şekilde yöntemi `EditPost` geri `Edit`.

Görünüm indirmelere varlıklar koleksiyonu sahip olmadığından, model bağlayıcı otomatik olarak güncelleştirilemiyor `CourseAssignments` gezinti özelliği. Güncelleştirilecek model bağlayıcısını kullanmak yerine `CourseAssignments` gezinti özelliği, yeni bunu `UpdateInstructorCourses` yöntemi. Bu nedenle hariç gerek `CourseAssignments` model bağlama özelliğinden. Bu çağıran kodu herhangi bir değişiklik gerektirmez `TryUpdateModel` uygulamaları güvenilir listeye almayı aşırı kullandığımızdan ve `CourseAssignments` ekleme listesinde değil.

Hiçbir onay kutuları seçilmedi, varsa kodda `UpdateInstructorCourses` başlatır `CourseAssignments` gezinti özelliği boş bir koleksiyon ve döndürür:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

Kod sonra veritabanındaki tüm kursları aracılığıyla döngüler ve her indirmelere görünümünde seçilen dosyalardan karşı Eğitmen atanmış olanları karşı denetler. Verimli aramalar kolaylaştırmak için sonraki iki koleksiyon depolanmış `HashSet` nesneleri.

Bir indirmelere onay kutusunu seçildi ancak indirmelere değil `Instructor.CourseAssignments` gezinti özelliği, koleksiyon gezinti özelliği içinde indirmelere eklenir.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

Bir indirmelere onay kutusunun seçili edilmedi ancak indirmelere bulunduğu `Instructor.CourseAssignments` gezinti özelliği, indirmelere Gezinti özelliğinden kaldırılır.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a>Eğitmen görünümleri güncelleştirme

İçinde *Views/Instructors/Edit.cshtml*, ekleme bir **kurslar** aşağıdakileri ekleyerek onay kutularını dizisi ile alan hemen sonra kod `div` için öğeleri **Office**  alan ve önce `div` öğesi için **kaydetmek** düğmesi.

<a id="notepad"></a>
> [!NOTE] 
> Visual Studio'da kodu yapıştırın, satır sonları kodu keser bir biçimde değiştirilir.  CTRL + Z otomatik biçimlendirme geri almak için bir kez basın.  Burada gördüğünüz gibi göründükleri böylece satır sonları düzeltir. Girinti kusursuz, olmak zorunda değildir ancak `@</tr><tr>`, `@:<td>`, `@:</td>`, ve `@:</tr>` satırlarını her tek bir satıra gösterildiği gibi veya olmalıdır, bir çalışma zamanı hatasıyla karşılaşırsınız. Seçili yeni kod bloğu ile sekme yeni kodu var olan kodu ile hizalamak için üç kez basın. Bu sorunu durumunu kontrol edebilirsiniz [burada](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

Bu kod, üç sütun içeren bir HTML tablosu oluşturur. Her sütunda, indirmelere numarası ve başlığı oluşan bir resim yazısı tarafından izlenen bir onay kutusudur. Tüm onay kutularını grup olarak kabul edilmesi için oldukları model Bağlayıcısı bilgi veren aynı adı ("selectedCourses") sahip. Her onay kutusunun değer özniteliği değerine ayarlanmış `CourseID`. Sayfa gönderildiğinde, model bağlayıcı bir dizi oluşan denetleyicisine geçirir. `CourseID` yalnızca, seçilen onay kutuları için değerleri.

Onay kutularını başlangıçta işlendiğinde, eğitmen atanan kurslara olanlar bunları (bunları denetlenen gösterir) seçer öznitelikleri kullanıma.

Uygulama, belirleyin **Eğitmen** sekmesine ve tıklayın **Düzenle** görmek için bir eğitmen üzerinde **Düzenle** sayfası.

![Kurslar Eğitmen Düzenle sayfası](update-related-data/_static/instructor-edit-courses.png)

Bazı indirmelere atamalarını değiştirin ve Kaydet'e tıklayın. Dizin sayfasında yaptığınız değişiklikleri yansıtır.

> [!NOTE] 
> Eğitmen indirmelere verileri düzenlemek için burada uygulanan yaklaşıma iyi kurslar sınırlı sayıda olduğunda çalışır. Farklı bir kullanıcı Arabirimi ve farklı bir güncelleştirme yöntemi, çok daha büyük olan koleksiyonları için gerekli olacak.

## <a name="update-the-delete-page"></a>Güncelleştirme Sil sayfası

İçinde *InstructorsController.cs*, silme `DeleteConfirmed` aşağıdaki kodu yerine yöntemi ve Ekle.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

Bu kod, aşağıdaki değişiklikleri yapar:

* İçin yükleme mu eager `CourseAssignments` gezinti özelliği.  Bu eklemek zorunda veya EF olmaz biliyorsanız hakkında ilgili `CourseAssignment` varlıkları ve bunları silmez.  Bunları okuyun gerek önlemek için burada, art arda silme veritabanında yapılandırabilirsiniz.

* Silinecek Eğitmen herhangi Departmanlar yönetici olarak atanmış ise bu bölümlerden Eğitmen atama kaldırır.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Ofis konumu ve kurslar Oluştur sayfası ekleme

İçinde *InstructorsController.cs*, HttpGet silip HttpPost `Create` yöntemleri ve ardından yerine aşağıdaki kodu ekleyin:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

Bu kod ne için gördüğünüz için benzer `Edit` , başlangıçta hiçbir kurslar dışındaki yöntemleri seçilir. HttpGet `Create` yöntem çağrılarını `PopulateAssignedCourseData` değil olabileceğinden ancak seçilen kurslar yöntemi sipariş için boş bir koleksiyon sağlamak için `foreach` (Aksi halde görünümü kodu throw bir null başvuru özel durumu) görünümünde döngü.

HttpPost `Create` yöntemi her seçili indirmelere ekler `CourseAssignments` doğrulama hatalarını denetler ve yeni Eğitmen veritabanına ekler önce gezinti özelliği. Bile model hatalar varsa (örneğin bir, geçersiz bir tarih anahtarlı kullanıcı) modeli hataları var ve sayfaya bir hata iletisi ile yeniden yapılan indirmelere seçimleri otomatik olarak geri böylece kurslar eklenir.

Kurslara eklemek için dikkat `CourseAssignments` sahip özellik boş bir koleksiyon olarak başlatmak için gezinti özelliği:

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

Denetleyici kodda bunu alternatif olarak, eğitmen modelinde yoksa, otomatik olarak koleksiyonu aşağıdaki örnekte gösterildiği gibi oluşturmak için özellik alıcısı değiştirerek bunu:

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

Değiştirirseniz `CourseAssignments` özelliği bu şekilde, denetleyici açık özellik başlatma kodda kaldırabilirsiniz.

İçinde *Views/Instructor/Create.cshtml*, bir office konumu metin kutusu ekleyin ve Gönder düğmesine önce kurslara onay kutularını işaretleyin. Düzen sayfası olduğu gibi söz konusu olduğunda [kopyalayıp yapıştırın, Visual Studio kodu yeniden biçimlendirir, biçimlendirme düzeltme](#notepad).

[!code-html[](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

Uygulama çalışırken ve bir eğitmen oluşturma sınayın. 

## <a name="handling-transactions"></a>İşleme işlemleri

İçinde anlatıldığı gibi [CRUD öğretici](crud.md), Entity Framework örtük olarak işlemleri uygular. Burada, daha fazla denetim--Örneğin, bir işlem içinde--Entity Framework dışında yapılan işlemleri dahil etmek istiyorsanız senaryolar görmek için [işlemleri](https://docs.microsoft.com/ef/core/saving/transactions).

## <a name="summary"></a>Özet

İlgili verilerle çalışmaya giriş tamamladınız. Sonraki öğreticide eşzamanlılık çakışmaları nasıl ele alınacağını görürsünüz.

>[!div class="step-by-step"]
[Önceki](read-related-data.md)
[sonraki](concurrency.md)  
