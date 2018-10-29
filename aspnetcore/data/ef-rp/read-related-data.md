---
title: ASP.NET core'da - EF çekirdekli Razor sayfaları 6 8 - ilgili verileri okuma
author: rick-anderson
description: Bu öğreticide okuyun ve ilgili verileri--diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler verileri görüntüler.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/read-related-data
ms.openlocfilehash: cf8733e1e806c4be0c4b217fc45c7a338a03a3ce
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207562"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a>ASP.NET core'da - EF çekirdekli Razor sayfaları 6 8 - ilgili verileri okuma

Tarafından [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

Bu öğreticide, ilgili verileri okuma ve görüntülenir. İlgili verileri EF Core Gezinti özelliklerini yüklenen verilerdir.

Olamaz çözmenize, sorunlarla karşılaşırsanız, [indirin veya tamamlanmış uygulamayı görüntüleyin.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Yükleme yönergeleri](xref:index#how-to-download-a-sample).

Aşağıdaki çizimler, Bu öğretici için tamamlanmış sayfaların göstermektedir:

![Kursları dizin sayfası](read-related-data/_static/courses-index.png)

![Eğitmenler dizin sayfası](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>İlgili veri eager, açık ve yavaş yükleniyor

EF Core Gezinti özelliklerini bir varlığın ilgili verileri yükleyebilir birkaç yolu vardır:

* [İstekli yükleme](/ef/core/querying/related-data#eager-loading). Bir varlık türü için sorgu ilgili varlıkları yüklediğinde istekli Yükleme ' dir. Varlık okunduğunda, ilgili verileri alınır. Bu, tek bir birleşim sorguda tüm gerekli olan verileri alır. genellikle sonuçlanır. EF Core istekli yükleme bazı türleri için birden çok sorgu verecek. Birden çok sorgu göndermeye çalışması için bazı sorguları EF6 olandan daha verimli olabilir dönemler tek bir sorgu. İstekli yükleme belirtilirse `Include` ve `ThenInclude` yöntemleri.

  ![İstekli yükleme örneği](read-related-data/_static/eager-loading.png)
 
  Bir koleksiyon Gezinti dahil edildiğinde istekli yükleme birden çok sorgu gönderir:

  * Ana sorgu için bir sorgu 
  * "Edge" Yük ağacında her bir koleksiyon için bir sorgu.

* Ayrı sorgularla `Load`: veriler ayrı sorgularda alınabilir ve EF Core "düzeltmeler" Gezinti özellikleri. "düzeltmeler yukarı" anlamına gelir EF Core gezinme özelliklerini otomatik olarak doldurur. Ayrı sorgularla `Load` işinizi daha istekli yükleme yükleme gibi daha.

  ![Ayrı sorguları örneği](read-related-data/_static/separate-queries.png)

  Not: EF Core Gezinti özellikleri bağlam örneğine önceden yüklenmiş herhangi bir varlık için otomatik olarak düzeltir. Bir gezinme özelliği için veri olsa bile *değil* açıkça dahil, özellik hala bazıları doldurulabilir veya tüm ilişkili varlıkları daha önce yüklenmiş.

* [Açık yükleme](/ef/core/querying/related-data#explicit-loading). Varlığın ilk okunduğunda, ilgili verileri alınan değil. Kod gerektiğinde ilgili verileri almak üzere yazılmış olmalıdır. Birden çok sorgu Veritabanına gönderilir açık yükleme ayrı sorgular ile sonuçlanır. Açık yükleme ile kod yüklenmesine, gezinti özellikleri belirtir. Kullanım `Load` açık yükleme yapmak için yöntemi. Örneğin:

  ![Açık yükleme örneği](read-related-data/_static/explicit-loading.png)

* [Yavaş Yükleniyor](/ef/core/querying/related-data#lazy-loading). [Yavaş yükleniyor, EF Core 2.1 sürümünde eklenen](/ef/core/querying/related-data#lazy-loading). Varlığın ilk okunduğunda, ilgili verileri alınan değil. Gezinti özelliğine erişinceye, ilk kez bu gezinti özelliği için gerekli verileri otomatik olarak alınır. Her zaman için ilk kez bir gezinti özelliğine erişinceye veritabanına bir sorgu gönderilir.

* `Select` İşleci yalnızca gerekli ilgili verileri yükler.

## <a name="create-a-course-page-that-displays-department-name"></a>Bölüm adı görüntüleyen bir kurs sayfa oluşturma

Kurs varlığı içeren bir gezinme özelliği içeren `Department` varlık. `Department` Varlık kursu atanır bölümü içerir.

Atanan bölüm adını kursları listesini görüntülemek için:

* Alma `Name` özelliğinden `Department` varlık.
* `Department` Varlık geldiği `Course.Department` gezinme özelliği.

![ourse. Bölüm](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a>İskele kurs modeli

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Bölümündeki yönergeleri [Öğrenci modeli iskelesini](xref:data/ef-rp/intro#scaffold-the-student-model) ve `Course` model sınıfı için.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

 Şu komutu çalıştırın:

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

------

Önceki komut iskelesini kurar `Course` modeli. Projeyi Visual Studio'da açın.

Açık *Pages/Courses/Index.cshtml.cs* ve incelemek `OnGetAsync` yöntemi. Yapı iskelesi altyapısı istekli yükleme için belirtilen `Department` gezinme özelliği. `Include` İstekli yükleme yöntemini belirtir.

Uygulamayı çalıştırmak ve seçmek **kursları** bağlantı. Bölüm sütun görüntüler `DepartmentID`, hangi kullanışlı değildir.

Güncelleştirme `OnGetAsync` yöntemini aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

Yukarıdaki kod ekler `AsNoTracking`. `AsNoTracking` döndürülen varlıkları izlenmez performansı artırır. Geçerli bağlamda güncelleştirilir değil çünkü varlıkları izlenmez.

Güncelleştirme *Pages/Courses/Index.cshtml* aşağıdaki vurgulanmış biçimlendirmeye sahip:

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

İskele kurulan kodu için aşağıdaki değişiklikler yapılmıştır:

* Başlık dizinden kurslara değiştirildi.
* Eklenen bir **numarası** gösteren sütun `CourseID` özellik değeri. Normalde son kullanıcılara anlamsız olduğunuz için varsayılan olarak, birincil anahtarlar iskele kurulmuş değildir. Ancak, bu durumda birincil anahtarı anlam ifade eder.
* Değiştirilen **departmanı** bölüm adını görüntülemek için sütun. Kod görüntüler `Name` özelliği `Department` yüklendiği varlık `Department` gezinti özelliği:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Uygulamayı çalıştırmak ve seçmek **kursları** bölüm adları listesini görmek için sekmesinde.

![Kursları dizin sayfası](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a>İlgili belirli verileri yükleniyor

`OnGetAsync` Yöntemi ile ilgili verileri yükler `Include` yöntemi:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`Select` İşleci yalnızca gerekli ilgili verileri yükler. Tek öğeleri gibi `Department.Name` bir SQL INNER JOIN kullanır. Koleksiyon, başka bir veritabanı erişimi kullanır, ancak bu nedenle mu `Include` koleksiyonlarda işleci.

Aşağıdaki kod ile ilgili verileri yükler `Select` yöntemi:

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`CourseViewModel`:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

Bkz: [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) ve [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) tam bir örnek.

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Kursları ve kayıtları gösterir bir eğitmen sayfası oluşturma

Bu bölümde, eğitmenler sayfa oluşturulur.

<a name="IP"></a>
![Eğitmenler dizin sayfası](read-related-data/_static/instructors-index.png)

Bu sayfada okur ve ilgili verileri aşağıdaki yollarla görüntüler:

* Eğitmenler listesini ilgili verileri görüntüleyen `OfficeAssignment` varlık (önceki görüntüde Office). `Instructor` Ve `OfficeAssignment` bir sıfır-veya-bir ilişkide varlıklardır. İstekli yükleme için kullanılan `OfficeAssignment` varlıklar. İlgili veriler görüntülenecek gerektiğinde istekli yükleme genellikle daha verimli olur. Bu durumda, eğitmenler office atamaları görüntülenir.
* Kullanıcı, ilişkili bir eğitmen (önceki görüntüde Harui) seçtiğinde `Course` varlıkları görüntülenir. `Instructor` Ve `Course` bir çoktan çoğa ilişki içinde varlıklardır. İstekli yükleme için kullanılan `Course` varlıkları ve bunların ilgili `Department` varlıklar. Bu durumda, yalnızca seçili Eğitmen kursları gerekli olduğu ayrı sorgular daha verimli olabilir. Bu örnek gezinme özelliklerinin gezinme özelliklerinde varlıklardaki istekli yükleme kullanmayı gösterir.
* Kullanıcı bir kurs (önceki görüntüde Kimya) seçtiğinde ilgili verileri `Enrollments` varlık görüntülenir. Yukarıdaki görüntüde, Öğrenci adı ve sınıf görüntülenir. `Course` Ve `Enrollment` bir bire çok ilişkiye varlıklardır.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Eğitmen dizini görünümü için bir görünüm modeli oluşturun

Eğitmenler sayfanın üç farklı tablolardaki verileri gösterir. Üç tabloyu temsil eden üç varlıkları içeren bir görünüm modeli oluşturulur.

İçinde *SchoolViewModels* klasör oluşturma *InstructorIndexData.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>İskele Eğitmen modeli

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Bölümündeki yönergeleri [Öğrenci modeli iskelesini](xref:data/ef-rp/intro#scaffold-the-student-model) ve `Instructor` model sınıfı için.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

 Şu komutu çalıştırın:

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

------

Önceki komut iskelesini kurar `Instructor` modeli. Uygulamayı çalıştırın ve Eğitmenler sayfasına gidin.

Değiştirin *Pages/Instructors/Index.cshtml.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

`OnGetAsync` Yöntemi seçili Eğitmen kimliği için isteğe bağlı rota verileri kabul eder.

Sorguyu inceleyin *Pages/Instructors/Index.cshtml.cs* dosyası:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

İki sorgu içeren içerir:

* `OfficeAssignment`: İçinde görüntülenen [Eğitmenler görünümü](#IP).
* `CourseAssignments`: Bu, verilen derslerimiz getirir.


### <a name="update-the-instructors-index-page"></a>Eğitmenler dizin sayfası

Güncelleştirme *Pages/Instructors/Index.cshtml* aşağıdaki işaretlemeyle:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:

* Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int?}"`. `"{id:int?}"` bir rota şablonudur. Rota şablonu tamsayı sorgu dizelerine URL rota verilerini değiştirir. Örneğin, tıklayarak **seçin** bağlantı için yalnızca bir eğitmen `@page` yönergesi, aşağıdaki gibi bir URL oluşturur:

    `http://localhost:1234/Instructors?id=2`

    Sayfa yönergesi olduğunda `@page "{id:int?}"`, önceki URL'si:

    `http://localhost:1234/Instructors/2`

* Sayfa başlığı **Eğitmenler**.
* Eklenen bir **Office** görüntüleyen sütun `item.OfficeAssignment.Location` yalnızca `item.OfficeAssignment` null değil. Bu bir sıfır-veya-bir ilişkisi olduğundan, ilgili OfficeAssignment varlık olmayabilir.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Eklenen bir **kursları** kursları görüntüleyen sütun her Eğitmenler tarafından verilen. Bkz: [açık satır geçişle `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) bu razor sözdizimi hakkında daha fazla bilgi için.

* Dinamik olarak ekleyen, eklenen kod `class="success"` için `tr` seçili Eğitmen öğesidir. Bu önyükleme sınıfını kullanarak seçili satır için bir arka plan rengini ayarlar.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Etiketli yeni köprü eklenmiş **seçin**. Bu bağlantı için seçilen Eğitmen kodu gönderir `Index` yöntemi ve bir arka plan rengini ayarlar.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Uygulamayı çalıştırmak ve seçmek **Eğitmenler** sekmesi. Sayfa görüntüler `Location` (office) ilgili gelen `OfficeAssignment` varlık. Varsa OfficeAssignment' olan null, boş bir tablo hücresi görüntülenir.

![Hiçbir şey seçili Eğitmenler dizin sayfası](read-related-data/_static/instructors-index-no-selection.png)

Tıklayarak **seçin** bağlantı. Satır stili değişiklikler.

### <a name="add-courses-taught-by-selected-instructor"></a>Seçili Eğitmenler tarafından verilen derslerimiz Ekle

Güncelleştirme `OnGetAsync` yönteminde *Pages/Instructors/Index.cshtml.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

Ekleme `public int CourseID { get; set; }`

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

Güncelleştirilen sorguyu inceleyin:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

Önceki sorgunun ekler `Department` varlıklar.

Bir eğitmen seçildiğinde aşağıdaki kodu çalıştırır (`id != null`). Seçili Eğitmen görünüm modeli eğitmenlerini listesi alınır. Görünüm modeli `Courses` özelliğinin yüklü olan `Course` Bu eğitmen varlıklardan `CourseAssignments` gezinme özelliği.

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

`Where` Yöntemi koleksiyonunu döndürür. Önceki içinde `Where` yöntemi, tek bir `Instructor` varlık döndürülür. `Single` Yöntemi, tek bir koleksiyon dönüştürür `Instructor` varlık. `Instructor` Varlık erişim sağlar `CourseAssignments` özelliği. `CourseAssignments` ilgili erişim sağlayan `Course` varlıklar.

![Eğitmen kursları m:M](complex-data-model/_static/courseassignment.png)

`Single` Yöntemi yalnızca bir öğe koleksiyonu olduğunda üzerindeki bir koleksiyona kullanılır. `Single` Yöntemi koleksiyonu boş ise veya birden fazla öğe varsa bir özel durum oluşturur. Alternatif `SingleOrDefault`, hangi varsayılan değeri döndürür (Bu durumda null) koleksiyonu boş ise. Kullanarak `SingleOrDefault` boş bir koleksiyon üzerinde:

* Bir özel durum oluşur (bulunmaya çalışılırken gelen bir `Courses` null başvuru özelliği).
* Özel durum iletisi sorunun nedenini daha net bir şekilde gösterir.

Aşağıdaki kod görünümü modelin doldurur `Enrollments` kurs seçildiğinde özelliği:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

Sonuna aşağıdaki işaretlemeyi ekleyin *Pages/Instructors/Index.cshtml* Razor sayfası:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

Önceki biçimlendirme bir eğitmen seçildiğinde bir eğitmen için ilgili kurslar listesini görüntüler.

Uygulamayı test etme. Tıklayarak bir **seçin** Eğitmenler sayfasında bağlantı.

![Seçili Eğitmenler dizin sayfası Eğitmen](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>Öğrenci verileri göster

Bu bölümde, uygulama, seçilen bir kurs Öğrenci verilerini göstermek için güncelleştirilir.

Sorguda güncelleştirme `OnGetAsync` yönteminde *Pages/Instructors/Index.cshtml.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Güncelleştirme *Pages/Instructors/Index.cshtml*. Aşağıdaki biçimlendirmede dosyanın sonuna ekleyin:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

Önceki biçimlendirme seçili kursun kayıtlı öğrencilere listesini görüntüler.

Sayfayı yenileyin ve bir eğitmen seçin. Kayıtlı Öğrenci ve kendi derece listesini görmek için bir kurs seçin.

![Eğitmenler dizin sayfası Eğitmen ve seçilen kursu](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>Tek kullanma

`Single` Yöntemi geçirebilir `Where` koşul çağırmak yerine `Where` yöntemi ayrı olarak:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

Önceki `Single` yaklaşımı kullanarak üzerinde hiçbir yararları sağlar `Where`. Bazı geliştiriciler tercih `Single` yaklaşımını stili.

## <a name="explicit-loading"></a>açık yükleme

İstekli yükleme için geçerli kod belirtir `Enrollments` ve `Students`:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Kullanıcılar nadiren kayıtları bir kursta görmek istediğinizi varsayalım. Bu durumda, istenirse yalnızca kayıt verileri yüklemek için bir en iyi duruma getirme olacaktır. Bu bölümde, `OnGetAsync` açık yükleme kullanmak için güncelleştirilmiş `Enrollments` ve `Students`.

Güncelleştirme `OnGetAsync` aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

Yukarıdaki kod bıraktığı *ThenInclude* için verileri kayıt ve Öğrenci yöntemini çağırır. Bir kurs seçtiyseniz vurgulanmış kodu alır:

* `Enrollment` Seçili kurs için varlıklar.
* `Student` Varlıklar her `Enrollment`.

Açıklama çıkış kodu önceki fark `.AsNoTracking()`. Gezinti özellikleri, izlenen varlıklar için yalnızca bir açıkça yüklenebilir.

Uygulamayı test etme. Kullanıcılar açısından bakıldığında, app, önceki sürüme aynı şekilde davranır.

Sonraki öğreticide, ilgili verileri güncelleştirme işlemi gösterilmektedir.

>[!div class="step-by-step"]
>[Önceki](xref:data/ef-rp/complex-data-model)
>[İleri](xref:data/ef-rp/update-related-data)
