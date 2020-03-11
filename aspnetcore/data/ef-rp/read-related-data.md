---
title: ASP.NET Core EF Core ile Razor Pages-Ilgili verileri oku-6/8
author: rick-anderson
description: Bu öğreticide ilgili verileri okuyabilir ve görüntüleriz, yani Entity Framework gezinti özelliklerine yüklediği veriler.
ms.author: riande
ms.custom: mvc
ms.date: 09/28/2019
uid: data/ef-rp/read-related-data
ms.openlocfilehash: d244ce1527486466bcbc6557ec35869aa206bc4f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656578"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a>ASP.NET Core EF Core ile Razor Pages-Ilgili verileri oku-6/8

, [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog)ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

Bu öğreticide, ilgili verilerin nasıl okunacağı ve görüntüleneceği gösterilmektedir. İlgili veriler, EF Core gezinti özelliklerine yüklediği veri.

Aşağıdaki çizimler, Bu öğreticinin tamamlanan sayfalarını göstermektedir:

![Kurslar Dizin sayfası](read-related-data/_static/courses-index30.png)

![Eğitmenler Dizin sayfası](read-related-data/_static/instructors-index30.png)

## <a name="eager-explicit-and-lazy-loading"></a>Eager, açık ve yavaş yükleme

EF Core bir varlığın gezinti özelliklerine ilgili verileri yükleyebilmenin birkaç yolu vardır:

* [Eager yükleniyor](/ef/core/querying/related-data#eager-loading). Ekip yükleme, bir varlık türü için bir sorgu aynı zamanda ilgili varlıkları de yüklediğinde oluşur. Bir varlık okunmadığınızda ilgili veriler alınır. Bu, genellikle gereken tüm verileri alan tek bir JOIN sorgusuna neden olur. EF Core, bazı Eager yükleme türleri için birden çok sorgu yayımlayacak. Birden çok sorgu verme, çok büyük paketlerini tek sorgusundan daha verimli olabilir. `Include` ve `ThenInclude` yöntemleriyle Eager yüklemesi belirtildi.

  ![Eager yükleme örneği](read-related-data/_static/eager-loading.png)
 
  Bir koleksiyon gezintisi eklendiğinde Eager yüklemesi birden çok sorgu gönderir:

  * Ana sorgu için bir sorgu 
  * Yükleme ağacındaki her koleksiyon "Edge" için bir sorgu.

* Sorguları `Load`ile ayırın: veriler ayrı sorgularda alınabilir ve EF Core "düzeltmeler" ' i "düzeltir". "Düzeltmeler", EF Core gezinti özelliklerini otomatik olarak dolduran anlamına gelir. `Load` ile ayrı sorgular, ek yükleme Eager 'dan daha da benzer.

  ![Ayrı sorgular örneği](read-related-data/_static/separate-queries.png)

  Note: EF Core, daha önce bağlam örneğine yüklenmiş olan diğer varlıklar için gezinti özelliklerini otomatik olarak düzeltir. Bir gezinti özelliği için veriler açıkça dahil *edilmese* bile, ilgili varlıkların bazıları veya tümü daha önce yüklenmişse Özellik yine de doldurulabilir.

* [Açık yükleme](/ef/core/querying/related-data#explicit-loading). Varlık ilk kez okunmadıysa ilgili veriler alınmadı. Gerektiğinde ilgili verileri almak için kodun yazılması gerekir. Ayrı sorgularla açık yükleme, veritabanına birden çok sorgu gönderilmesine neden olur. Açık yükleme ile kod, yüklenecek gezinti özelliklerini belirtir. Açık yükleme yapmak için `Load` yöntemini kullanın. Örnek:

  ![Açık yükleme örneği](read-related-data/_static/explicit-loading.png)

* [Yavaş yükleme](/ef/core/querying/related-data#lazy-loading). [Sürüm 2,1 ' de EF Core geç yükleme eklendi](/ef/core/querying/related-data#lazy-loading). Varlık ilk kez okunmadıysa ilgili veriler alınmadı. Gezinti özelliğine ilk kez erişildiğinde, bu gezinti özelliği için gereken veriler otomatik olarak alınır. Bir gezinti özelliğine ilk kez erişildiğinde bir sorgu veritabanına gönderilir.

## <a name="create-course-pages"></a>Kurs sayfaları oluşturma

`Course` varlığı, ilgili `Department` varlığını içeren bir gezinti özelliği içerir.

![Kurs. Departmanı](read-related-data/_static/dep-crs.png)

Bir kurs için atanan departmanın adını göstermek için:

* İlgili `Department` varlığını `Course.Department` gezinti özelliğine yükleyin.
* `Department` varlığın `Name` özelliğinden adı alın.

<a name="scaffold"></a>

### <a name="scaffold-course-pages"></a>Yapı iskelesi kurs sayfaları

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Aşağıdaki özel durumlarla birlikte [Yapı Fkatlama öğrenci sayfalarındaki](xref:data/ef-rp/intro#scaffold-student-pages) yönergeleri izleyin:

  * Bir *Sayfalar/kurslar* klasörü oluşturun.
  * Model sınıfı için `Course` kullanın.
  * Yeni bir tane oluşturmak yerine var olan bağlam sınıfını kullanın.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Bir *Sayfalar/kurslar* klasörü oluşturun.

* Kurs sayfalarını iskele almak için aşağıdaki komutu çalıştırın.

  **Windows'da:**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

  **Linux veya macOS 'ta:**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages/Courses --referenceScriptLibraries
  ```

---

* *Pages/kurslar/Index. cshtml. cs* dosyasını açın ve `OnGetAsync` metodunu inceleyin. Yapı iskelesi altyapısı, `Department` gezinti özelliği için bir yükleme belirtti. `Include` yöntemi Eager yüklemeyi belirtir.

* Uygulamayı çalıştırın ve **Kurslar** bağlantısını seçin. Departman sütunu, yararlı olmayan `DepartmentID`görüntüler.

### <a name="display-the-department-name"></a>Departmanın adını görüntüleme

Pages/kurslar/Index. cshtml. cs dosyasını aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu30/Pages/Courses/Index.cshtml.cs?highlight=18,22,24)]

Yukarıdaki kod `Course` özelliğini `Courses` olarak değiştirir ve `AsNoTracking`ekler. `AsNoTracking`, döndürülen varlıklar izlenmediğinden performansı geliştirir. Varlıkların geçerli bağlamda güncelleştirilmediği için izlenmesi gerekmez.

*Pages/kurslar/Index. cshtml* 'yi aşağıdaki kodla güncelleştirin.

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Index.cshtml?highlight=5,8,16-18,20,23,26,32,35-37,45)]

Yapı iskelesi kodunda aşağıdaki değişiklikler yapılmıştır:

* `Course` Özellik adı `Courses`olarak değiştirildi.
* `CourseID` özellik değerini gösteren bir **sayı** sütunu eklendi. Birincil anahtarlar, genellikle son kullanıcılara anlamlı olduklarından, varsayılan olarak yapı iskelesi göstermemektedir. Ancak, bu durumda birincil anahtar anlamlı olur.
* Departman adını göstermek için **Departman** sütunu değiştirildi. Kod, `Department` gezinti özelliğine yüklenen `Department` varlığının `Name` özelliğini görüntüler:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Uygulamayı çalıştırın ve bölüm adlarıyla listeyi görmek için **Kurslar** sekmesini seçin.

![Kurslar Dizin sayfası](read-related-data/_static/courses-index30.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a>Select ile ilgili verileri yükleme

`OnGetAsync` yöntemi, `Include` yöntemiyle ilgili verileri yükler. `Select` yöntemi, yalnızca gerekli ilgili verileri yükleyen bir alternatiftir. Tek öğeler için, `Department.Name` gibi bir SQL Iç BIRLEŞIMI kullanır. Koleksiyonlar için, başka bir veritabanı erişimi kullanır, ancak koleksiyonlar üzerinde `Include` işleci olur.

Aşağıdaki kod `Select` yöntemiyle ilgili verileri yükler:

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=6)]

`CourseViewModel`:

[!code-csharp[](intro/samples/cu30snapshots/6-related/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

Tüm örnek için bkz. [ındexselect. cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) ve [IndexSelect.cshtml.cs](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs) .

## <a name="create-instructor-pages"></a>Eğitmen sayfaları oluşturma

Bu bölüm, eğitmen sayfalarını derler ve ilgili Kurslar ve kayıtları eğitmenler dizin sayfasına ekler.

<a name="IP"></a>
![eğitmenler Dizin sayfası](read-related-data/_static/instructors-index30.png)

Bu sayfa aşağıdaki yollarla ilgili verileri okur ve görüntüler:

* Eğitmenler listesi `OfficeAssignment` varlığındaki ilgili verileri (önceki görüntüde Office) görüntüler. `Instructor` ve `OfficeAssignment` varlıkları bire sıfır veya-bir ilişkidir. `OfficeAssignment` varlıkları için Eager yüklemesi kullanılır. Eager yüklemesi, ilgili verilerin görüntülenmesi gerektiğinde genellikle daha etkilidir. Bu durumda, Eğitmenler için Office atamaları görüntülenir.
* Kullanıcı bir eğitmen seçtiğinde ilgili `Course` varlıkları görüntülenir. `Instructor` ve `Course` varlıkları çoktan çoğa bir ilişkidir. Eager yüklemesi, `Course` varlıkları ve ilgili `Department` varlıkları için kullanılır. Bu durumda, yalnızca seçili eğitmen için kurslar gerektiğinden ayrı sorgular daha verimli olabilir. Bu örnek, gezinti özelliklerinde olan varlıklarda gezinti özellikleri için Eager yükleme 'nin nasıl kullanılacağını gösterir.
* Kullanıcı bir kurs seçtiğinde `Enrollments` varlığındaki ilgili veriler görüntülenir. Önceki görüntüde öğrenci adı ve sınıf görüntülenir. `Course` ve `Enrollment` varlıkları bire çok ilişkidir.

### <a name="create-a-view-model"></a>Görünüm modeli oluşturma

Eğitmenler sayfasında, üç farklı tablodan alınan veriler gösterilir. Üç tabloyu temsil eden üç özellik içeren bir görünüm modeli gerekir.

Aşağıdaki kodla *SchoolViewModels/ıncpctorındexdata. cs* oluşturun:

[!code-csharp[](intro/samples/cu30/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-instructor-pages"></a>Yapı iskelesi eğitmeni sayfaları

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Öğrenci sayfalarını aşağıdaki özel durumlarla birlikte [Scaffold](xref:data/ef-rp/intro#scaffold-student-pages) içindeki yönergeleri izleyin:

  * Bir *sayfa/eğitmenler* klasörü oluşturun.
  * Model sınıfı için `Instructor` kullanın.
  * Yeni bir tane oluşturmak yerine var olan bağlam sınıfını kullanın.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Bir *sayfa/eğitmenler* klasörü oluşturun.

* Eğitmen sayfalarını iskele almak için aşağıdaki komutu çalıştırın.

  **Windows'da:**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

  **Linux veya macOS 'ta:**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages/Instructors --referenceScriptLibraries
  ```

---

Yapı iskelesi sayfasının güncelleştirmeden önce nasıl göründüğünü görmek için, uygulamayı çalıştırın ve eğitmenler sayfasına gidin.

*Pages/eğitmenler/Index. cshtml. cs* dosyasını aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,19-53)]

`OnGetAsync` yöntemi, seçilen eğitmenin KIMLIĞI için isteğe bağlı rota verilerini kabul eder.

*Pages/eğitmenler/Index. cshtml. cs* dosyasındaki sorguyu inceleyin:

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading)]

Kod, aşağıdaki gezinti özellikleri için Eager yüklemeyi belirtir:

* `Instructor.OfficeAssignment`
* `Instructor.CourseAssignments`
  * `CourseAssignments.Course`
    * `Course.Department`
    * `Course.Enrollments`
      * `Enrollment.Student`

`CourseAssignments` ve `Course`için `Include` ve `ThenInclude` yöntemlerinin yinelendiğine dikkat edin. Bu yineleme, `Course` varlığının iki gezinti özelliği için Eager yüklemesi belirtmek için gereklidir.

Aşağıdaki kod, bir eğitmen seçildiğinde (`id != null`) yürütülür.

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectInstructor)]

Seçilen eğitmen, görünüm modelindeki eğitmenler listesinden alınır. Görünüm modelinin `Courses` özelliği, bu eğitmenin `CourseAssignments` gezinti özelliğinden `Course` varlıklarla birlikte yüklenir.

`Where` yöntemi bir koleksiyon döndürür. Ancak bu durumda filtre tek bir varlık seçer. Bu nedenle, koleksiyonu tek bir `Instructor` varlığına dönüştürmek için `Single` yöntemi çağırılır. `Instructor` varlığı `CourseAssignments` özelliğine erişim sağlar. `CourseAssignments` ilgili `Course` varlıklarına erişim sağlar.

![Eğitmenden kurslar M:d](complex-data-model/_static/courseassignment.png)

Koleksiyonda yalnızca bir öğe olduğunda `Single` yöntemi bir koleksiyonda kullanılır. Koleksiyon boşsa veya birden fazla öğe varsa `Single` yöntemi bir özel durum oluşturur. Bir alternatif, koleksiyon boşsa varsayılan bir değer (Bu durumda null) döndüren `SingleOrDefault`.

Aşağıdaki kod, bir kurs seçildiğinde görünüm modelinin `Enrollments` özelliğini doldurur:

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectCourse)]

### <a name="update-the-instructors-index-page"></a>Eğitmenler Dizin sayfasını Güncelleştir

*Pages/eğitmenler/Index. cshtml* dosyasını aşağıdaki kodla güncelleştirin.

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Index.cshtml?highlight=1,5,8,16-21,25-32,43-57,67-102,104-126)]

Yukarıdaki kod aşağıdaki değişiklikleri yapar:

* `@page` `page` yönergesini `@page "{id:int?}"`olarak güncelleştirir. `"{id:int?}"` bir rota şablonudur. Yol şablonu, verileri yönlendirmek için URL 'deki tamsayı Sorgu dizelerini değiştirir. Örneğin, yalnızca `@page` yönergesine sahip bir eğitmenin **Select** bağlantısına tıkladığınızda aşağıdakiler gıbı bir URL üretilir:

  `https://localhost:5001/Instructors?id=2`

  Sayfa yönergesi `@page "{id:int?}"`olduğunda URL şu şekilde olur:

  `https://localhost:5001/Instructors/2`

* Yalnızca `item.OfficeAssignment` null olmaması durumunda `item.OfficeAssignment.Location` görüntüleyen bir **Office** sütunu ekler. Bu bire sıfır veya-bir ilişki olduğundan ilgili bir OfficeAssignment varlığı bulunmayabilir.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Her bir eğitmen tarafından taders kurslarını görüntüleyen bir **Kurslar** sütunu ekler. Bu Razor sözdizimi hakkında daha fazla bilgi için bkz. [açık hat geçişi](xref:mvc/views/razor#explicit-line-transition) .

* Seçilen eğitmenin ve kursun `tr` öğesine dinamik olarak `class="success"` ekleyen kodu ekler. Bu, bir önyükleme sınıfı kullanarak seçili satır için bir arka plan rengi ayarlar.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* **Select**etiketli yeni bir köprü ekler. Bu bağlantı, seçilen eğitmenin KIMLIĞINI `Index` yöntemine gönderir ve bir arka plan rengi ayarlar.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

* Seçili eğitmen için bir kurs tablosu ekler.

* Seçili kurs için bir öğrenci kayıtları tablosu ekler.

Uygulamayı çalıştırın ve **eğitmenler** sekmesini seçin. Sayfada ilgili `OfficeAssignment` varlığındaki `Location` (Office) görüntülenir. `OfficeAssignment` null ise boş bir tablo hücresi görüntülenir.

Bir eğitmenin **Seç** bağlantısına tıklayın. Bu eğitmenin atandığı satır stili değişiklikleri ve kurslar görüntülenir.

Kayıtlı öğrenciler ve bunların onların listesini görmek için bir kurs seçin.

![Eğitmenler Dizin sayfası eğitmeni ve kursu seçildi](read-related-data/_static/instructors-index30.png)

## <a name="using-single"></a>Tek kullanımı

`Single` yöntemi, `Where` yöntemini ayrı çağırmak yerine `Where` koşulunu geçirebilir:

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

Where koşulunun kişisel tercihle ilgili olduğu `Single` kullanımı. `Where` yöntemi kullanılarak herhangi bir avantaj sağlamaz.

## <a name="explicit-loading"></a>açık yükleme

Geçerli kod `Enrollments` ve `Students`için Eager yüklemeyi belirtir:

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading&highlight=6-9)]

Kullanıcıların bir kursa kayıtları nadiren görmek istediğini varsayalım. Bu durumda, bir iyileştirme yalnızca isteniyorsa kayıt verilerini yüklemek olacaktır. Bu bölümde `OnGetAsync`, `Enrollments` ve `Students`açık yüklemesini kullanacak şekilde güncelleştirilir.

*Pages/eğitmenler/Index. cshtml. cs* dosyasını aşağıdaki kodla güncelleştirin.

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Index.cshtml.cs?highlight=31-35,52-56)]

Yukarıdaki kod, kayıt ve öğrenci verileri için *Thenınclude* Yöntem çağrılarını bırakır. Bir kurs seçilirse, açık yükleme kodu alır:

* Seçili kurs için `Enrollment` varlıkları.
* Her bir `Enrollment`için `Student` varlıkları.

Yukarıdaki kod yorumlarının `.AsNoTracking()`dikkat edin. Gezinti özellikleri yalnızca izlenen varlıklar için açık bir şekilde yüklenebilir.

Uygulamayı test etme. Bir kullanıcının perspektifinden, uygulama önceki sürümle aynı şekilde davranır.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki öğreticide ilgili verileri güncelleştirme gösterilmektedir.

>[!div class="step-by-step"]
>[Önceki öğretici](xref:data/ef-rp/complex-data-model)
>[sonraki öğretici](xref:data/ef-rp/update-related-data)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu öğreticide ilgili veriler okundu ve görüntülenir. İlgili veriler, EF Core gezinti özelliklerine yüklediği veri.

Sorun yaşıyorsanız, bu [uygulamayı indiremez veya görüntüleyemezsiniz.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Yönergeleri indirin](xref:index#how-to-download-a-sample).

Aşağıdaki çizimler, Bu öğreticinin tamamlanan sayfalarını göstermektedir:

![Kurslar Dizin sayfası](read-related-data/_static/courses-index.png)

![Eğitmenler Dizin sayfası](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>İlgili verilerin Eager, açık ve geç yüklemesi

EF Core bir varlığın gezinti özelliklerine ilgili verileri yükleyebilmenin birkaç yolu vardır:

* [Eager yükleniyor](/ef/core/querying/related-data#eager-loading). Ekip yükleme, bir varlık türü için bir sorgu aynı zamanda ilgili varlıkları de yüklediğinde oluşur. Varlık okunmadığınızda ilgili veriler alınır. Bu, genellikle gereken tüm verileri alan tek bir JOIN sorgusuna neden olur. EF Core, bazı Eager yükleme türleri için birden çok sorgu yayımlayacak. Birden çok sorgu verilmesi, EF6 içindeki bazı sorgular için tek bir sorgunun bulunduğu durumda daha verimli olabilir. `Include` ve `ThenInclude` yöntemleriyle Eager yüklemesi belirtildi.

  ![Eager yükleme örneği](read-related-data/_static/eager-loading.png)
 
  Bir koleksiyon gezintisi eklendiğinde Eager yüklemesi birden çok sorgu gönderir:

  * Ana sorgu için bir sorgu 
  * Yükleme ağacındaki her koleksiyon "Edge" için bir sorgu.

* Sorguları `Load`ile ayırın: veriler ayrı sorgularda alınabilir ve EF Core "düzeltmeler" ' i "düzeltir". "düzeltmeler", EF Core gezinti özelliklerini otomatik olarak dolduran anlamına gelir. `Load` ile ayrı sorgular, ek yükleme Eager 'dan daha da benzer.

  ![Ayrı sorgular örneği](read-related-data/_static/separate-queries.png)

  Note: EF Core, daha önce bağlam örneğine yüklenmiş olan diğer varlıklar için gezinti özelliklerini otomatik olarak düzeltir. Bir gezinti özelliği için veriler açıkça dahil *edilmese* bile, ilgili varlıkların bazıları veya tümü daha önce yüklenmişse Özellik yine de doldurulabilir.

* [Açık yükleme](/ef/core/querying/related-data#explicit-loading). Varlık ilk kez okunmadıysa ilgili veriler alınmadı. Gerektiğinde ilgili verileri almak için kodun yazılması gerekir. Ayrı sorgularla açık yükleme, VERITABANıNA birden çok sorgu gönderilmesine neden olur. Açık yükleme ile kod, yüklenecek gezinti özelliklerini belirtir. Açık yükleme yapmak için `Load` yöntemini kullanın. Örnek:

  ![Açık yükleme örneği](read-related-data/_static/explicit-loading.png)

* [Yavaş yükleme](/ef/core/querying/related-data#lazy-loading). [Sürüm 2,1 ' de EF Core geç yükleme eklendi](/ef/core/querying/related-data#lazy-loading). Varlık ilk kez okunmadıysa ilgili veriler alınmadı. Gezinti özelliğine ilk kez erişildiğinde, bu gezinti özelliği için gereken veriler otomatik olarak alınır. Bir gezinti özelliğine ilk kez erişildiğinde bir sorgu VERITABANıNA gönderilir.

* `Select` işleci yalnızca gerekli ilgili verileri yükler.

## <a name="create-a-course-page-that-displays-department-name"></a>Bölüm adını görüntüleyen bir kurs sayfası oluşturma

Kurs varlığı, `Department` varlığını içeren bir gezinti özelliği içerir. `Department` varlığı, kursun atandığı departmanı içerir.

Bir kurs listesinde atanan departmanın adını göstermek için:

* `Department` varlığındaki `Name` özelliğini alın.
* `Department` varlık `Course.Department` gezinti özelliğinden gelir.

![Kurs. Departmanı](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>

### <a name="scaffold-the-course-model"></a>Kurs modelini temklesi

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio) 

[Öğrenci modelini Scafkatlama](xref:data/ef-rp/intro#scaffold-the-student-model) ve model sınıfı için `Course` kullanma bölümündeki yönergeleri izleyin.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

 Şu komutu çalıştırın:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

---

Yukarıdaki komut, `Course` modelini yasaklıyor. Projeyi Visual Studio'da açın.

*Pages/kurslar/Index. cshtml. cs* dosyasını açın ve `OnGetAsync` metodunu inceleyin. Yapı iskelesi altyapısı, `Department` gezinti özelliği için bir yükleme belirtti. `Include` yöntemi Eager yüklemeyi belirtir.

Uygulamayı çalıştırın ve **Kurslar** bağlantısını seçin. Departman sütunu, yararlı olmayan `DepartmentID`görüntüler.

`OnGetAsync` yöntemini aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

Yukarıdaki kod `AsNoTracking`ekler. `AsNoTracking`, döndürülen varlıklar izlenmediğinden performansı geliştirir. Geçerli bağlamda güncelleştirilmemiş oldukları için varlıklar izlenmiyor.

*Pages/kurslar/Index. cshtml* 'yi aşağıdaki vurgulanmış işaretlerle güncelleştirin:

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

Yapı iskelesi kodunda aşağıdaki değişiklikler yapılmıştır:

* Başlık dizinden kurslar olarak değiştirildi.
* `CourseID` özellik değerini gösteren bir **sayı** sütunu eklendi. Birincil anahtarlar, genellikle son kullanıcılara anlamlı olduklarından, varsayılan olarak yapı iskelesi göstermemektedir. Ancak, bu durumda birincil anahtar anlamlı olur.
* Departman adını göstermek için **Departman** sütunu değiştirildi. Kod, `Department` gezinti özelliğine yüklenen `Department` varlığının `Name` özelliğini görüntüler:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Uygulamayı çalıştırın ve bölüm adlarıyla listeyi görmek için **Kurslar** sekmesini seçin.

![Kurslar Dizin sayfası](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a>Select ile ilgili verileri yükleme

`OnGetAsync` yöntemi `Include` yöntemiyle ilgili verileri yükler:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`Select` işleci yalnızca gerekli ilgili verileri yükler. Tek öğeler için, `Department.Name` gibi bir SQL Iç BIRLEŞIMI kullanır. Koleksiyonlar için, başka bir veritabanı erişimi kullanır, ancak koleksiyonlar üzerinde `Include` işleci olur.

Aşağıdaki kod `Select` yöntemiyle ilgili verileri yükler:

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`CourseViewModel`:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

Tüm örnek için bkz. [ındexselect. cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) ve [IndexSelect.cshtml.cs](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) .

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Kurslar ve kayıtları gösteren bir eğitmenler sayfası oluşturun

Bu bölümde, Eğitmenler sayfası oluşturulur.

<a name="IP"></a>
![eğitmenler Dizin sayfası](read-related-data/_static/instructors-index.png)

Bu sayfa aşağıdaki yollarla ilgili verileri okur ve görüntüler:

* Eğitmenler listesi `OfficeAssignment` varlığındaki ilgili verileri (önceki görüntüde Office) görüntüler. `Instructor` ve `OfficeAssignment` varlıkları bire sıfır veya-bir ilişkidir. `OfficeAssignment` varlıkları için Eager yüklemesi kullanılır. Eager yüklemesi, ilgili verilerin görüntülenmesi gerektiğinde genellikle daha etkilidir. Bu durumda, Eğitmenler için Office atamaları görüntülenir.
* Kullanıcı bir eğitmen (önceki görüntüde Haruı) seçtiğinde ilgili `Course` varlıkları görüntülenir. `Instructor` ve `Course` varlıkları çoktan çoğa bir ilişkidir. Eager yüklemesi, `Course` varlıkları ve ilgili `Department` varlıkları için kullanılır. Bu durumda, yalnızca seçili eğitmen için kurslar gerektiğinden ayrı sorgular daha verimli olabilir. Bu örnek, gezinti özelliklerinde olan varlıklarda gezinti özellikleri için Eager yükleme 'nin nasıl kullanılacağını gösterir.
* Kullanıcı bir kurs seçtiğinde (önceki görüntüde kimsiz deneme), `Enrollments` varlığındaki ilgili veriler görüntülenir. Önceki görüntüde öğrenci adı ve sınıf görüntülenir. `Course` ve `Enrollment` varlıkları bire çok ilişkidir.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Eğitmen dizini görünümü için bir görünüm modeli oluşturun

Eğitmenler sayfasında, üç farklı tablodan alınan veriler gösterilir. Üç tabloyu temsil eden üç varlığı içeren bir görünüm modeli oluşturulur.

*SchoolViewModels* klasöründe, aşağıdaki kodla *InstructorIndexData.cs* oluşturun:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Eğitmen modelini scafkatlama

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio) 

[Öğrenci modelini Scafkatlama](xref:data/ef-rp/intro#scaffold-the-student-model) ve model sınıfı için `Instructor` kullanma bölümündeki yönergeleri izleyin.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

 Şu komutu çalıştırın:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

Yukarıdaki komut, `Instructor` modelini yasaklıyor. 
Uygulamayı çalıştırın ve eğitmenler sayfasına gidin.

*Pages/eğitmenler/Index. cshtml. cs* öğesini şu kodla değiştirin:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

`OnGetAsync` yöntemi, seçilen eğitmenin KIMLIĞI için isteğe bağlı rota verilerini kabul eder.

*Pages/eğitmenler/Index. cshtml. cs* dosyasındaki sorguyu inceleyin:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

Sorgunun iki içerme vardır:

* `OfficeAssignment`: [eğitmenler görünümünde](#IP)görüntülenir.
* `CourseAssignments`: kurslar taöğretme.

### <a name="update-the-instructors-index-page"></a>Eğitmenler Dizin sayfasını Güncelleştir

*Sayfaları/eğitmenler/Index. cshtml* 'yi şu biçimlendirmeyle güncelleştirin:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:

* `@page` `page` yönergesini `@page "{id:int?}"`olarak güncelleştirir. `"{id:int?}"` bir rota şablonudur. Yol şablonu, verileri yönlendirmek için URL 'deki tamsayı Sorgu dizelerini değiştirir. Örneğin, yalnızca `@page` yönergesine sahip bir eğitmenin **Select** bağlantısına tıkladığınızda aşağıdakiler gıbı bir URL üretilir:

  `http://localhost:1234/Instructors?id=2`

  Sayfa yönergesi `@page "{id:int?}"`olduğunda, önceki URL şu şekilde olur:

  `http://localhost:1234/Instructors/2`

* Sayfa başlığı **eğitmenler**' dir.
* Yalnızca `item.OfficeAssignment` null olmaması durumunda `item.OfficeAssignment.Location` görüntüleyen bir **Office** sütunu eklendi. Bu bire sıfır veya-bir ilişki olduğundan ilgili bir OfficeAssignment varlığı bulunmayabilir.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Her bir eğitmen tarafından taders kurslarını görüntüleyen bir **Kurslar** sütunu eklendi. Bu Razor sözdizimi hakkında daha fazla bilgi için bkz. [açık hat geçişi](xref:mvc/views/razor#explicit-line-transition) .

* Seçilen eğitmenin `tr` öğesine dinamik olarak `class="success"` ekleyen kod eklendi. Bu, bir önyükleme sınıfı kullanarak seçili satır için bir arka plan rengi ayarlar.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* **Select**etiketli yeni bir köprü eklendi. Bu bağlantı, seçilen eğitmenin KIMLIĞINI `Index` yöntemine gönderir ve bir arka plan rengi ayarlar.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Uygulamayı çalıştırın ve **eğitmenler** sekmesini seçin. Sayfada ilgili `OfficeAssignment` varlığındaki `Location` (Office) görüntülenir. Eğer OfficeAssignment ' null ise boş bir tablo hücresi görüntülenir.

**Seç** bağlantısına tıklayın. Satır stili değişir.

### <a name="add-courses-taught-by-selected-instructor"></a>Seçili eğitmen 'e göre kurslar ekleyin

*Pages/eğitmenler/Index. cshtml. cs* dosyasındaki `OnGetAsync` yöntemini aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

`public int CourseID { get; set; }` Ekle

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

Güncelleştirilmiş sorguyu inceleyin:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

Yukarıdaki sorgu `Department` varlıklarını ekler.

Aşağıdaki kod, bir eğitmen seçildiğinde (`id != null`) yürütülür. Seçilen eğitmen, görünüm modelindeki eğitmenler listesinden alınır. Görünüm modelinin `Courses` özelliği, bu eğitmenin `CourseAssignments` gezinti özelliğinden `Course` varlıklarla birlikte yüklenir.

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

`Where` yöntemi bir koleksiyon döndürür. Önceki `Where` yönteminde yalnızca tek bir `Instructor` varlığı döndürülür. `Single` yöntemi, koleksiyonu tek bir `Instructor` varlığına dönüştürür. `Instructor` varlığı `CourseAssignments` özelliğine erişim sağlar. `CourseAssignments` ilgili `Course` varlıklarına erişim sağlar.

![Eğitmenden kurslar M:d](complex-data-model/_static/courseassignment.png)

Koleksiyonda yalnızca bir öğe olduğunda `Single` yöntemi bir koleksiyonda kullanılır. Koleksiyon boşsa veya birden fazla öğe varsa `Single` yöntemi bir özel durum oluşturur. Bir alternatif, koleksiyon boşsa varsayılan bir değer (Bu durumda null) döndüren `SingleOrDefault`. Boş bir koleksiyonda `SingleOrDefault` kullanma:

* Bir özel durumla sonuçlanır (null başvuru üzerinde `Courses` özelliği bulmaya çalışırken).
* Özel durum iletisi sorunun nedenini daha az gösterir.

Aşağıdaki kod, bir kurs seçildiğinde görünüm modelinin `Enrollments` özelliğini doldurur:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

*Pages/eğitmenler/Index. cshtml* Razor sayfasının sonuna aşağıdaki biçimlendirmeyi ekleyin:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

Yukarıdaki biçimlendirme, bir eğitmen seçildiğinde bir eğitmenin ilgili kursların bir listesini görüntüler.

Uygulamayı test etme. Eğitmenler sayfasında bir **seçme** bağlantısına tıklayın.

### <a name="show-student-data"></a>Öğrenci verilerini göster

Bu bölümde, uygulama seçili bir kurs için öğrenci verilerini gösterecek şekilde güncelleştirilir.

*Pages/eğitmenler/Index. cshtml. cs* içindeki `OnGetAsync` yönteminde bulunan sorguyu aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Güncelleştirme *sayfaları/eğitmenler/Index. cshtml*. Aşağıdaki biçimlendirmeyi dosyanın sonuna ekleyin:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

Yukarıdaki biçimlendirme, seçili kursa kayıtlı öğrencilerin bir listesini görüntüler.

Sayfayı yenileyin ve bir eğitmen seçin. Kayıtlı öğrenciler ve bunların onların listesini görmek için bir kurs seçin.

![Eğitmenler Dizin sayfası eğitmeni ve kursu seçildi](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>Tek kullanımı

`Single` yöntemi, `Where` yöntemini ayrı çağırmak yerine `Where` koşulunu geçirebilir:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

Yukarıdaki `Single` yaklaşım, `Where`kullanmaktan faydalanır. Bazı geliştiriciler `Single` yaklaşım stilini tercih eder.

## <a name="explicit-loading"></a>açık yükleme

Geçerli kod `Enrollments` ve `Students`için Eager yüklemeyi belirtir:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Kullanıcıların bir kursa kayıtları nadiren görmek istediğini varsayalım. Bu durumda, bir iyileştirme yalnızca isteniyorsa kayıt verilerini yüklemek olacaktır. Bu bölümde `OnGetAsync`, `Enrollments` ve `Students`açık yüklemesini kullanacak şekilde güncelleştirilir.

`OnGetAsync` aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

Yukarıdaki kod, kayıt ve öğrenci verileri için *Thenınclude* Yöntem çağrılarını bırakır. Bir kurs seçilirse vurgulanan kod şunu alır:

* Seçili kurs için `Enrollment` varlıkları.
* Her bir `Enrollment`için `Student` varlıkları.

Yukarıdaki kod açıklamalarının `.AsNoTracking()`dikkat edin. Gezinti özellikleri yalnızca izlenen varlıklar için açık bir şekilde yüklenebilir.

Uygulamayı test etme. Bir kullanıcının perspektifinden, uygulama önceki sürümle aynı şekilde davranır.

Sonraki öğreticide ilgili verileri güncelleştirme gösterilmektedir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Bu öğreticinin YouTube sürümü (part1)](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [Bu öğreticinin YouTube sürümü (part2)](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
>[Önceki](xref:data/ef-rp/complex-data-model)
>[İleri](xref:data/ef-rp/update-related-data)

::: moniker-end
