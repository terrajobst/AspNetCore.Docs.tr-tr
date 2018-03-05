---
title: "Razor sayfalarının EF çekirdek ile-ilgili verileri - 8'in 6 okuma"
author: rick-anderson
description: "Bu öğreticide okuyun ve ilgili verileri--diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler verileri görüntüleyin."
manager: wpickett
ms.author: riande
ms.date: 11/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 6e71e9c01a58c3f60dacce8959ac4502a3690690
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a>Okuma data - EF çekirdek Razor sayfaları (8 6) ile ilgili

Tarafından [zel Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Bu öğreticide, ilgili veri okumak ve görüntülenir. İlgili verileri EF çekirdek Gezinti özelliklerini yükleyen verilerdir.

Olamaz çözmek sorunlarla karşılaşırsanız, indirme [Bu aşama için tamamlanan uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).

Aşağıdaki çizimler, Bu öğretici için tamamlanan sayfaları göstermektedir:

![Kurslar dizin sayfası](read-related-data/_static/courses-index.png)

![Eğitmen dizin sayfası](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>İstekli, açık ve geç ilgili veri yükleme

EF çekirdek ilgili verileri bir varlık Gezinti özellikleri yükleyebilirsiniz birkaç yolu vardır:

* [İstekli yükleme](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading). Bir varlık türü için bir sorgu aynı zamanda ilgili varlıklar yüklediğinde istekli Yükleme ' dir. Varlık okurken ilgili verileri alınır. Bu, genellikle tüm gerekli olan verileri alan bir tek birleştirme sorgusunda sonuçlanır. EF çekirdek istekli yükleme bazı türleri için birden çok sorgu gönderirsiniz. Birden çok sorgu verme EF6 bazı sorgularda söz konusu olandan daha etkili olabilir oluştu burada tek bir sorgu. İstekli yükleme ile belirtilen `Include` ve `ThenInclude` yöntemleri.

 ![İstekli yükleme örneği](read-related-data/_static/eager-loading.png)
 
 Bir koleksiyon nvavigation dahil edildiğinde istekli yükleme birden çok sorgu gönderir:

 * Ana sorgu için bir sorgu 
 * Her bir koleksiyon yük ağacında "kenar" için bir sorgu.

* Ayrı sorgularıyla `Load`: verileri ayrı sorgularda alınabilir ve EF çekirdek "düzeltmelerini" Gezinti özellikleri. "düzeltmeler yukarı" anlamına gelir EF çekirdek Gezinti özellikleri otomatik olarak doldurur. Ayrı sorgularıyla `Load` daha çok istekli yüklemekten yüklenirken explict gibi.

 ![Ayrı sorgulara örneği](read-related-data/_static/separate-queries.png)

 Not: EF çekirdek Gezinti özellikleri bağlam örneğine önceden yüklenmiş herhangi bir varlık için otomatik olarak düzeltir. Bir gezinme özelliği için veri olsa bile *değil* açıkça dahil, özellik hala bazıları doldurulmuş ya da tüm ilgili varlıklar önceden yüklendi.

* [Açık yükleme](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading). Varlık ilk okunduğunda ilgili verileri alınan değil. Kod gerektiğinde ilgili verileri almak üzere yazılmış olmalıdır. Birden çok sorgu Veritabanına gönderilen ayrı sorgularla açık yükleme sonuçlanır. Açık yükleme ile kod yüklenecek Gezinti özellikleri belirtir. Kullanım `Load` açık yükleme yapmak için yöntem. Örneğin:

 ![Açık yükleme örneği](read-related-data/_static/explicit-loading.png)

* [Yavaş Yükleniyor](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading). [EF çekirdek geç yükleme şu anda desteklemiyor](https://github.com/aspnet/EntityFrameworkCore/issues/3797). Varlık ilk okunduğunda ilgili verileri alınan değil. Bir gezinme özelliği ilk erişildiğinde bu gezinti özelliği için gerekli olan veriler otomatik olarak alınır. Bir sorgu her zaman ilk olarak bir gezinti özelliği erişilir DB gönderilir.

* `Select` İşleci yalnızca gereken ilgili verileri yükler.

## <a name="create-a-courses-page-that-displays-department-name"></a>Bölüm adı görüntüler kurslar sayfası oluşturma

İndirmelere varlık içeren bir gezinme özelliği içeren `Department` varlık. `Department` Varlık indirmelere atandığı bölüm içerir.

Atanan departmanı adını kurslar listesini görüntülemek için:

* Alma `Name` özelliğinden `Department` varlık.
* `Department` Varlık gelir `Course.Department` gezinti özelliği.

![ourse. Bölüm](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a>İskele indirmelere modeli

* Visual Studio'dan çıkın.
* Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).
* Şu komutu çalıştırın:

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

Yukarıdaki komut iskelesini kurar `Course` modeli. Projesini Visual Studio'da açın.

Projeyi oluşturun. Derleme hataları aşağıdaki gibi oluşturur:

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 Genel olarak değiştirmek `_context.Course` için `_context.Courses` ("s" eklemek diğer bir deyişle, `Course`). 7 oluşumu bulundu ve güncelleştirildi.

Açık *Pages/Courses/Index.cshtml.cs* ve inceleyin `OnGetAsync` yöntemi. Yapı iskelesi altyapısı istekli yükleme için belirtilen `Department` gezinti özelliği. `Include` Yöntemi istekli yüklenmesini belirtir.

Uygulamayı çalıştırın ve seçin **kurslar** bağlantı. Bölüm sütunu görüntüler `DepartmentID`, hangi kullanışlı değildir.

Güncelleştirme `OnGetAsync` aşağıdaki kod ile yöntemi:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

Önceki kod ekler `AsNoTracking`. `AsNoTracking` döndürülen varlıkları değil izlendiği için performansı geliştirir. Geçerli bağlamda güncelleştirilir değil çünkü varlıkları izlenmez.

Güncelleştirme *Views/Courses/Index.cshtml* aşağıdaki vurgulanmış biçimlendirmeyi ile:

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

İskele kurulmuş kod aşağıdaki değişiklikler yapılmıştır:

* Başlık dizinden kurslara değiştirildi.
* Eklenen bir **numarası** gösterir sütun `CourseID` özellik değeri. Normalde son kullanıcılara anlamsız oldukları için varsayılan olarak, birincil anahtarlar iskele kurulmuş değil. Ancak, bu durumda birincil anahtarı anlamlıdır.
* Değiştirilen **departmanı** bölüm adını görüntülemek için sütun. Kod görüntüler `Name` özelliği `Department` yüklenen varlık `Department` gezinti özelliği:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Uygulamayı çalıştırın ve seçin **kurslar** bölüm adlarını listesiyle görmek için sekmesini.

![Kurslar dizin sayfası](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a>Yükleme Seç verilerle ilgili

`OnGetAsync` Yöntemi ile ilgili verileri yükler `Include` yöntemi:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`Select` İşleci yalnızca gereken ilgili verileri yükler. Tek öğelerin gibi `Department.Name` bir SQL INNER JOIN kullanır. Koleksiyon başka bir veritabanı erişimi kullanır, ancak bu nedenle mu `Include` koleksiyonlarda işleci.

Aşağıdaki kod ile ilgili verileri yükler `Select` yöntemi:

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`CourseViewModel`:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

Bkz: [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) ve [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) tam bir örnek için.

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Kurslar ve kayıtları gösteren bir eğitmen sayfası oluşturun

Bu bölümde, eğitmen sayfa oluşturulur.

<a name="IP"></a>
![Eğitmen dizin sayfası](read-related-data/_static/instructors-index.png)

Bu sayfayı okur ve ilgili verileri aşağıdaki yollarla görüntüler:

* İlgili verileri Eğitmen listesini görüntüleyen `OfficeAssignment` varlık (önceki görüntüde Office). `Instructor` Ve `OfficeAssignment` varlıklardır bir tane-sıfır-veya-bir ilişkisi. İstekli yükleme için kullanıldığından `OfficeAssignment` varlıklar. İlgili verileri görüntülenecek gerektiğinde istekli yükleme genellikle daha verimli olur. Bu durumda, eğitmen office atamaları görüntülenir.
* Kullanıcı ilişkili bir eğitmen (Harui) önceki görüntüde seçtiğinde `Course` varlıkları görüntülenir. `Instructor` Ve `Course` varlıklardır bir çok-çok ilişkisi. İstekli yükleme için kullanıldığından `Course` varlıkları ve bunların ilgili `Department` varlıklar. Bu durumda, yalnızca seçili Eğitmen kursları gerekli olduğu için ayrı sorgulara daha etkili olabilir. Bu örnek gezinme özelliklerinin gezinme özellikleri varlıklardaki istekli yükleme kullanmayı gösterir.
* Kullanıcı indirmelere (Kimya önceki görüntüde) seçtiğinde ilgili verileri `Enrollments` varlık görüntülenir. Önceki görüntüde Öğrenci adı ve düzeyde görüntülenir. `Course` Ve `Enrollment` varlıklardır bir-çok ilişkisi.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Eğitmen dizin görünüm için Görünüm modeli oluşturma

Eğitmen sayfanın üç farklı tablolardan verileri gösterir. Bir görünüm modeli üç tabloyu temsil eden üç varlıkları içeren oluşturulur.

İçinde *SchoolViewModels* klasörü oluşturmak *InstructorIndexData.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>İskele Eğitmen modeli

* Visual Studio'dan çıkın.
* Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).
* Şu komutu çalıştırın:

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

Yukarıdaki komut iskelesini kurar `Instructor` modeli. Projesini Visual Studio'da açın.

Projeyi oluşturun. Derleme hataları oluşturur.

Genel olarak değiştirmek `_context.Instructor` için `_context.Instructors` ("s" eklemek diğer bir deyişle, `Instructor`). 7 oluşumu bulundu ve güncelleştirildi.

Uygulamayı çalıştırın ve Eğitmen sayfasına gidin.

Değiştir *Pages/Instructors/Index.cshtml.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-99)]

`OnGetAsync` Yöntemi seçili Eğitmen kimliği için isteğe bağlı rota veri kabul eder.

Üzerinde sorgu inceleyin *Pages/Instructors/Index.cshtml* sayfa:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

Sorgu iki sahip içerir:

* `OfficeAssignment`: Görüntülenen [Eğitmen Görünüm](#IP).
* `CourseAssignments`: Hangi öğrettin kursları getirir.


### <a name="update-the-instructors-index-page"></a>Güncelleştirme Eğitmen dizin sayfası

Güncelleştirme *Pages/Instructors/Index.cshtml* aşağıdaki biçimlendirme ile:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:

* Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int?}"`. `"{id:int?}"` bir rota şablonudur. Rota şablonu için rota verilerini tamsayı URL'deki sorgu dizelerini değiştirir. Örneğin, tıklayarak **seçin** bağlantı için yalnızca bir eğitmen `@page` yönergesi aşağıdaki gibi bir URL oluşturur:

    `http://localhost:1234/Instructors?id=2`

    Sayfa yönergesi olduğunda `@page "{id:int?}"`, önceki URL'si:

    `http://localhost:1234/Instructors/2`

* Sayfa başlığı **Eğitmen**.
* Eklenen bir **Office** görüntüleyen sütun `item.OfficeAssignment.Location` yalnızca `item.OfficeAssignment` null değil. Bu bir tane-sıfır-veya-bir ilişkisi olduğundan, ilgili OfficeAssignment varlık olmayabilir.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Eklenen bir **kurslar** kurslar görüntüleyen sütun tarafından her Eğitmen öğrettin. Bkz: [açık satır geçişle `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) bu razor sözdizimi hakkında daha fazla bilgi için.

* Dinamik olarak ekleyen eklenen kod `class="success"` için `tr` seçili Eğitmen öğesidir. Bu önyükleme sınıfını kullanarak seçili olan satır için bir arka plan rengini belirler.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Etiketli yeni bir köprü eklenen **seçin**. Bu bağlantı için seçilen Eğitmen kimliği gönderir `Index` yöntemi ve arka plan rengini belirler.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Uygulamayı çalıştırın ve seçin **Eğitmen** sekmesi. Sayfa görüntüler `Location` (office) ilgili öğesinden `OfficeAssignment` varlık. Varsa OfficeAssignment' olan bir boş tablo hücresi null görüntülenir.

![Seçili hiçbir şey Eğitmen dizin sayfası](read-related-data/_static/instructors-index-no-selection.png)

Tıklayın **seçin** bağlantı. Satır stili değişiklikleri.

### <a name="add-courses-taught-by-selected-instructor"></a>Seçili Eğitmen tarafından öğrettin kurslar ekleme

Güncelleştirme `OnGetAsync` yönteminde *Pages/Instructors/Index.cshtml.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

Güncelleştirilmiş sorgu inceleyin:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

Önceki sorgunun ekler `Department` varlıklar.

Aşağıdaki kod bir eğitmen seçildiğinde yürütür (`id != null`). Seçili Eğitmen görünüm modeli Eğitmen listesi alınır. Görünüm modelinin `Courses` özelliği ile yüklenir `Course` Bu eğitmen varlıklardan `CourseAssignments` gezinti özelliği.

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

`Where` Yöntem koleksiyonu döndürür. Yukarıdaki içinde `Where` yöntemi, tek bir `Instructor` varlık döndürülür. `Single` Yöntemi tek bir koleksiyon dönüştürür `Instructor` varlık. `Instructor` Varlık erişim sağlar `CourseAssignments` özelliği. `CourseAssignments` ilgili erişim sağlayan `Course` varlıklar.

![Eğitmen kurslar m:M](complex-data-model/_static/courseassignment.png)

`Single` Koleksiyon yalnızca bir öğe varsa, yöntemi bir koleksiyonda kullanılır. `Single` Yöntemi koleksiyonu boş ise veya birden çok öğe varsa bir özel durum oluşturur. Bir alternatif `SingleOrDefault`, hangi varsayılan değeri döndürür (Bu durumda null) koleksiyonu boş ise. Kullanarak `SingleOrDefault` boş bir koleksiyon üzerinde:

* Bir özel durum oluşur (bulunmaya çalışılırken gelen bir `Courses` özelliği bir null başvuru).
* Özel durum iletisi daha az açıkça sorunun nedenini gösterir.

Aşağıdaki kod görünüm modelinin doldurur `Enrollments` bir indirmelere seçildiğinde özelliği:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

Aşağıdaki biçimlendirmede sonuna ekleyin *Pages/Courses/Index.cshtml* Razor sayfasını:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

Önceki biçimlendirme bir eğitmen seçildiğinde bir eğitmen ilgili kurslar listesini görüntüler.

Uygulamayı test etme. Tıklayın bir **seçin** bağlantısı Eğitmen sayfası.

![Seçili Eğitmen dizin sayfası Eğitmen](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>Öğrenci verileri göster

Bu bölümde, uygulama seçili indirmelere Öğrenci verileri gösterecek biçimde güncelleştirilir.

Sorguda güncelleştirme `OnGetAsync` yönteminde *Pages/Instructors/Index.cshtml.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Güncelleştirme *Pages/Instructors/Index.cshtml*. Aşağıdaki biçimlendirmede dosyanın sonuna ekleyin:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

Önceki biçimlendirme seçili indirmelere kaydedilen Öğrenciler listesini görüntüler.

Sayfayı yenileyin ve bir eğitmen seçin. Kayıtlı Öğrenciler ve bunların dereceleri listesini görmek için bir indirmelere seçin.

![Eğitmen dizin sayfası Eğitmen ve seçili indirmelere](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>Tek kullanma

`Single` Yöntemi geçirebilir `Where` çağırmak yerine koşulu `Where` yöntemi ayrı olarak:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

Yukarıdaki `Single` yaklaşım kullanarak üzerinden hiçbir yararları sağlar `Where`. Bazı geliştiriciler tercih `Single` yaklaşımını stili.

## <a name="explicit-loading"></a>Açık yükleniyor

İstekli yükleme için geçerli kod belirtir `Enrollments` ve `Students`:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Kullanıcıların nadiren bir seyrinde kayıtları görmek istediğinizi varsayalım. Bu durumda, bir en iyi duruma getirme istenirse kayıt verileri yalnızca yüklemek olacaktır. Bu bölümde `OnGetAsync` açık yüklenmesini kullanmak için güncelleştirilmiş `Enrollments` ve `Students`.

Güncelleştirme `OnGetAsync` aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

Önceki kod bırakır *ThenInclude* için kayıt ve Öğrenci verileri yöntemini çağırır. Bir indirmelere seçtiyseniz vurgulanmış kodu alır:

* `Enrollment` Seçili indirmelere için varlıklar.
* `Student` Varlıklar her `Enrollment`.

Yorumlar çıkış kodu yukarıdaki fark `.AsNoTracking()`. Gezinti özellikleri, izlenen varlıklar için yalnızca bir açıkça yüklenebilir.

Uygulamayı test etme. Kullanıcılar açısından bakıldığında, uygulamanın önceki sürüme geri aynı şekilde davranır.

Sonraki öğretici ilgili verileri güncelleştirmek nasıl gösterir.

>[!div class="step-by-step"]
>[Önceki](xref:data/ef-rp/complex-data-model)
>[sonraki](xref:data/ef-rp/update-related-data)
