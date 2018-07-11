---
title: -EF çekirdekli ASP.NET Core MVC 6 10 - ilgili verileri okuma
author: rick-anderson
description: Bu öğreticide okuyun ve ilgili verileri--diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler verileri görüntüler.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: d5c9b665a80003ef5029754d7ad1780b3254e97e
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38216300"
---
# <a name="aspnet-core-mvc-with-ef-core---read-related-data---6-of-10"></a>-EF çekirdekli ASP.NET Core MVC 6 10 - ilgili verileri okuma

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması, Entity Framework Core ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](intro.md).

Önceki öğreticide, okul veri modeli tamamlandı. Bu öğreticide, okuma ve ilgili verileri--diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler verileri görüntüler.

Aşağıdaki çizimler ile çalışmak sayfaları göstermektedir.

![Kursları dizin sayfası](read-related-data/_static/courses-index.png)

![Eğitmenler dizin sayfası](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>İlgili veri eager, açık ve yavaş yükleniyor

Birkaç yolu vardır, nesne ilişkisel eşleme (ORM) yazılım gibi Entity Framework ilgili verileri bir varlığın Gezinti özelliklerini yükleyebilirsiniz:

* İstekli yükleme. Varlık okunduğunda, onunla birlikte ilgili verileri alınır. Bu, tek bir birleşim sorguda tüm gerekli olan verileri alır. genellikle sonuçlanır. İstekli yükleme kullanarak Entity Framework Core içinde belirttiğiniz `Include` ve `ThenInclude` yöntemleri.

  ![İstekli yükleme örneği](read-related-data/_static/eager-loading.png)

  Bazı ayrı sorgularda veri alabilirsiniz ve EF "gezinme özelliklerini düzeltmeler".  Diğer bir deyişle, EF daha önce alınan varlık Gezinti özellikleri ait oldukları ayrı olarak alınan varlıkları otomatik olarak ekler. İlgili verileri alır. sorgu için kullanabileceğiniz `Load` yöntemi gibi bir liste veya nesnesi döndüren bir yöntem yerine `ToList` veya `Single`.

  ![Ayrı sorguları örneği](read-related-data/_static/separate-queries.png)

* Açık yükleme. Varlığın ilk okunduğunda, ilgili verileri alınan değil. Gerekli olup olmadığını ilgili verileri alır. kod yazacaksınız. Ayrı sorgular yüklenirken eager durumda olduğu gibi birden çok sorgu sonuçları yükleniyor açık veritabanına gönderilir. Açık yükleme ile kod yüklenmesine, gezinti özellikleri belirtir farktır. Entity Framework Core 1.1 içinde kullanabileceğiniz `Load` açık yükleme yapmak için yöntemi. Örneğin:

  ![Açık yükleme örneği](read-related-data/_static/explicit-loading.png)

* Yavaş yükleniyor. Varlığın ilk okunduğunda, ilgili verileri alınan değil. Ancak, bir gezinti özelliği erişmeye ilk kez otomatik olarak bu gezinti özelliği için gerekli olan veriler alınır. Bir sorgu, bir gezinti özelliği ilk kez veri almayı deneyin her zaman veritabanına gönderilir. Entity Framework Core 1.0, yavaş yüklenmesini desteklemez.

### <a name="performance-considerations"></a>Performans değerlendirmeleri

İlgili verileri alınan her varlık için gereksinim duyduğunuz biliyorsanız, veritabanına gönderilen tek bir sorgu genellikle daha verimli alınan her varlık için ayrı sorgulara olduğundan istekli yükleme genellikle en iyi performansı sunar. Örneğin, her departman on ilgili kurslar olduğunu varsayalım. İstekli yükleme tüm ilgili verilerin yalnızca bir tek (birleştirme) sorgu ve tek bir gidiş dönüş veritabanına neden olur. Kursları her departman için ayrı bir sorgu içinde on gidiş dönüş veritabanına neden olur. Gecikme süresi yüksek olduğunda veritabanına ek gidiş dönüş özellikle performansı düşürür.

Öte yandan, bazı senaryolarda ayrı sorgulara daha verimlidir. Bir sorgudaki tüm ilgili verilerin istekli yükleme, SQL Server'ın etkili bir şekilde işleyemiyor oluşturulması çok karmaşık birleştirme neden olabilir. Veya, bir varlığın yalnızca bir alt kümesini bir dizi işlem varlık Gezinti özellikleri erişmeniz gerekiyorsa, ayrı sorgulara Önden her şeyin istekli yükleme ihtiyacınız olandan daha fazla veri alması için daha iyi gerçekleştirebilir. Performans kritik ise, en iyi seçim yapmak için her iki yönde performansını test etmek idealdir.

## <a name="create-a-courses-page-that-displays-department-name"></a>Bölüm adını görüntüleyen bir kursları sayfası oluşturma

Kurs varlık kursu atandığı departmanı departmanı varlığı içeren bir gezinme özelliği içerir. Atanan bölüm adını kursları listesini görüntülemek için Name özelliği içinde departmanı varlıktan almanız gereken `Course.Department` gezinme özelliği.

Aynı seçenekleri kullanarak kurs varlık türü için CoursesController adlı bir denetleyicisi oluşturmak **MVC denetleyici Entity Framework kullanarak görünümler ile** önceki Öğrenciler denetleyici için gösterildiği gibi yaptığınız iskele kurucu Aşağıdaki çizim:

![Kursları denetleyici ekleme](read-related-data/_static/add-courses-controller.png)

Açık *CoursesController.cs* ve incelemek `Index` yöntemi. Otomatik yapı iskelesi istekli yükleme için belirtilmiş `Department` gezinme özelliğini kullanarak `Include` yöntemi.

Değiştirin `Index` yöntemi için daha uygun bir ad kullanan aşağıdaki kodla `IQueryable` kurs varlıkları döndürür (`courses` yerine `schoolContext`):

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

Açık *Views/Courses/Index.cshtml* ve şablon kodunu aşağıdaki kodla değiştirin. Değişiklikleri vurgulanmıştır:

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

İskele kurulan kodu için aşağıdaki değişiklikler yaptınız:

* Başlık dizinden kurslara değiştirildi.

* Eklenen bir **numarası** gösteren sütun `CourseID` özellik değeri. Normalde son kullanıcılara anlamsız olduğunuz için varsayılan olarak, birincil anahtarlar iskele kurulmuş değildir. Ancak, bu durumda birincil anahtarı anlamlı ve göstermek istiyorsunuz.

* Değiştirilen **departmanı** bölüm adını görüntülemek için sütun. Kod görüntüler `Name` yüklendiği departmanı varlığın özelliği `Department` gezinti özelliği:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Uygulamayı çalıştırmak ve seçmek **kursları** bölüm adları listesini görmek için sekmesinde.

![Kursları dizin sayfası](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Kursları ve kayıtları gösterir bir eğitmen sayfası oluşturma

Bu bölümde, eğitmenler sayfasını görüntülemek için bir denetleyici ve görünüm Eğitmen varlığın oluşturacaksınız:

![Eğitmenler dizin sayfası](read-related-data/_static/instructors-index.png)

Bu sayfada okur ve ilgili verileri aşağıdaki yollarla görüntüler:

* Eğitmenler listesini OfficeAssignment varlıktan ilgili verileri görüntüler. Eğitmen ve OfficeAssignment bir sıfır-veya-bir ilişkide varlıklardır. İstekli yükleme OfficeAssignment varlıklar için kullanacaksınız. Birincil tablo alınan tüm satırlarının için ilgili verileri gerektiğinde daha önce açıklandığı gibi istekli yükleme genellikle daha verimli olur. Bu durumda, tüm görüntülenen Eğitmenler office atamalarını görüntülemek istiyorsunuz.

* Kullanıcı bir eğitmen seçtiğinde ilgili kurs varlıkları görüntülenir. Eğitmen ve kurs bir çoktan çoğa ilişki varlıklardır. İstekli yükleme kurs varlıkları ve bunların ilgili bölüm varlıkları için kullanacaksınız. Bu durumda, yalnızca seçili eğitmen için kursları gerektiğinden ayrı sorgular daha verimli olabilir. Ancak, bu örnek, kendilerini Gezinti özelliklerdir varlıkların içinde gezinme özelliklerinin istekli yükleme kullanmayı gösterir.

* Kullanıcı bir kurs seçtiğinde ilgili verileri kayıtları varlık kümesindeki görüntülenir. Kursa ve kayıt bir bire çok ilişkiye varlıklardır. Kayıt varlıkları ve bunların ilişkili Öğrenci varlıklar için ayrı sorgulara kullanacaksınız.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Eğitmen dizini görünümü için bir görünüm modeli oluşturun

Eğitmenler sayfanın üç farklı tablolardaki verileri gösterir. Bu nedenle, her bir tablo için verileri tutan üç özellik içeren bir görünüm modeli oluşturmayı öğreneceksiniz.

İçinde *SchoolViewModels* klasör oluşturma *InstructorIndexData.cs* ve varolan kodu aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Eğitmen denetleyici ve görünümler oluşturma

Aşağıdaki çizimde gösterildiği gibi EF okuma/yazma eylemleri olan bir eğitmen denetleyicisi oluşturun:

![Eğitmenler denetleyici ekleme](read-related-data/_static/add-instructors-controller.png)

Açık *InstructorsController.cs* ve kullanarak bir ekleme Viewmodel'lar isim uzayı için:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

Index yöntemi istekli yükleme ilgili veri görünüm modelinde koymak için aşağıdaki kod ile değiştirin.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

Yöntemi, isteğe bağlı rota verileri kabul eder (`id`) ve bir sorgu dizesi parametresi (`courseID`) seçilen Eğitmen ve seçili kurs kimliği değerini sağlayın. Parametreleri tarafından sağlanan **seçin** sayfadaki köprüler.

Görünüm modeli örneği oluşturmayı ve bunu Eğitmenler listesini koyarak kod başlar. İstekli yükleme için kod belirtir `Instructor.OfficeAssignment` ve `Instructor.CourseAssignments` Gezinti özellikleri. İçinde `CourseAssignments` özelliği `Course` özelliği yüklü ve o, `Enrollments` ve `Department` özellikleri yüklenir ve her içinde `Enrollment` varlık `Student` özelliğinin yüklü.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

Görünümü her zaman OfficeAssignment varlık gerektirdiğinden, aynı sorgu içinde getirmek için daha verimlidir. Yalnızca sayfanın daha sık olmadan daha seçili bir kurs ile görüntüleniyorsa, tek bir sorguda birden çok sorguyu iyi, bu nedenle bir eğitmen web sayfasında seçildiğinde kurs varlıkları gereklidir.

Kod yineler `CourseAssignments` ve `Course` iki özelliklerinden gerektiğinden `Course`. İlk dizenin `ThenInclude` alır çağırır `CourseAssignment.Course`, `Course.Enrollments`, ve `Enrollment.Student`.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

Bu noktada kodda, başka bir `ThenInclude` Gezinti özellikleri için olacaktır `Student`, hangi gerekmez. Ancak arama `Include` ile baştan başlayıp `Instructor` domainproperty'leri yeniden, bu durumda belirtilmesinden gitmek zorunda özellikleri `Course.Department` yerine `Course.Enrollments`.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

Bir eğitmen seçildiğinde aşağıdaki kodu çalıştırır. Seçili Eğitmen görünüm modeli eğitmenlerini listesi alınır. Görünüm modeli `Courses` özelliğinin Bu eğitmen kurs varlıkların ile yüklü ardından `CourseAssignments` gezinme özelliği.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

`Where` Yöntem bir koleksiyon döndürür, ancak döndürülen yalnızca tek bir eğitmen varlık yöntemi sonuçlanan ölçütler bu durumda geçirilen. `Single` Yöntemi erişmenizi sağlar, varlığın tek bir eğitmen varlık koleksiyon dönüştürür `CourseAssignments` özelliği. `CourseAssignments` Özelliği içeren `CourseAssignment` istediğiniz yalnızca ilgili varlıkları `Course` varlıklar.

Kullandığınız `Single` koleksiyon bildiğiniz durumlarda bir koleksiyon üzerinde yöntemi, yalnızca bir öğe olacaktır. Tek bir yöntem kendisine geçirilen koleksiyonu boş ise veya birden fazla öğe varsa bir özel durum oluşturur. Alternatif `SingleOrDefault`, hangi varsayılan değeri döndürür (Bu durumda null) koleksiyonu boş ise. Ancak, bu durumda, hala sonuçlanır bir özel durum (bulunmaya çalışılırken gelen bir `Courses` özelliği null bir başvuru), ve özel durum iletisi sorunun nedenini daha net bir şekilde gösterir. Çağırdığınızda `Single` yöntemi, nerede ayrıca iletebilir çağırmak yerine koşul `Where` yöntemi ayrı olarak:

```csharp
.Single(i => i.ID == id.Value)
```

Onun yerine:

```csharp
.Where(I => i.ID == id.Value).Single()
```

Ardından, bir kurs seçildiyse, görünüm modeli eğitim listesinden seçilen kursu alınır. Ardından Görünüm modelinin `Enrollments` özelliğinin yüklü o kursun kayıt varlıkların ile `Enrollments` gezinme özelliği.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a>Eğitmen Index görünümünü değiştirme

İçinde *Views/Instructors/Index.cshtml*, şablonu kodu aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

Varolan kodu aşağıdaki değişiklikler yaptınız:

* Model sınıfı için değiştirilen `InstructorIndexData`.

* Sayfa başlığı değiştirilen **dizin** için **Eğitmenler**.

* Eklenen bir **Office** görüntüleyen sütun `item.OfficeAssignment.Location` yalnızca `item.OfficeAssignment` null değil. (Bu bir sıfır-veya-bir ilişkisi olduğundan, olmayabilir ilgili OfficeAssignment varlık.)

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
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Etiketli yeni köprü eklenmiş **seçin** hemen diğer bağlantıları önce her bir satırdaki neden olan gönderilmesini seçili Eğitmen kimliği `Index` yöntemi.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Uygulamayı çalıştırmak ve seçmek **Eğitmenler** sekmesi. İlgili hiçbir OfficeAssignment varlık olduğunda ilgili OfficeAssignment varlıkları ve bir boş tablo hücresi Location özelliği sayfasında görüntüler.

![Hiçbir şey seçili Eğitmenler dizin sayfası](read-related-data/_static/instructors-index-no-selection.png)

İçinde *Views/Instructors/Index.cshtml* kapatma tablo sonra öğesi (sonunda dosyası), dosya aşağıdaki kodu ekleyin. Bu kod bir eğitmen seçildiğinde bir eğitmen için ilgili kurslar listesini görüntüler.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

Bu kodu okuyan `Courses` özelliği kursları listesini görüntülemek için Görünüm modeli. Ayrıca sağlar bir **seçin** seçili kursa kimliği gönderen köprü `Index` eylem yöntemi.

Sayfayı yenileyin ve bir eğitmen seçin. Seçili eğitmen için atanan kursları görüntüleyen bir kılavuz göreceksiniz ve her kurs için atanan bölüm adını görürsünüz.

![Seçili Eğitmenler dizin sayfası Eğitmen](read-related-data/_static/instructors-index-instructor-selected.png)

Yeni eklediğiniz kod bloğundan sonra aşağıdaki kodu ekleyin. Bu kurs seçildiğinde bu kurs kayıtlı öğrencilere listesini görüntüler.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

Bu kod, kursun kayıtlı öğrencilerin listesini görüntülemek için Görünüm modeli kayıtları özelliği okur.

Sayfayı yenileyin ve bir eğitmen seçin. Ardından bir kurs kayıtlı Öğrenci ve kendi derece listesini görmek için seçin.

![Eğitmenler dizin sayfası Eğitmen ve seçilen kursu](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a>Açık yükleme

Alınan ne zaman eğitmenlerini listesini *InstructorsController.cs*, istekli yükleme için belirttiğiniz `CourseAssignments` gezinme özelliği.

Nadiren kayıtları seçili Eğitmen ve kursu görmek istediğiniz kullanıcıların beklenen varsayalım. Bu durumda, yalnızca isteniyorsa kayıt verileri yüklemek isteyebilirsiniz. Açık yükleme yapmak nasıl bir örnek görmek isterseniz değiştirme `Index` yöntemini aşağıdaki kodla istekli yükleme kayıtları için kaldırır ve bu özelliği açıkça yükler. Kod değişikliklerini vurgulanır.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

Yeni kod bıraktığı *ThenInclude* yöntemini çağırır için kayıt verilerini koddan Eğitmen varlıkları alır. Bir eğitmen ve kurs seçtiyseniz, seçili kurs için kayıt varlıkları ve her kayıt için Öğrenci varlıklar vurgulanmış kodu alır.

Çalıştırma verileri nasıl alınıp değiştirdik ancak Eğitmenler dizin sayfası artık ve, go uygulaması sayfasında, görüntülenen içinde herhangi bir fark görürsünüz.

## <a name="summary"></a>Özet

Artık istekli yükleme birden fazla sorgu içeren bir sorgu ile ilgili verileri Gezinti özelliklerini okumak için kullandınız. Sonraki öğreticide ilgili verileri güncelleştirme öğreneceksiniz.

::: moniker-end

>[!div class="step-by-step"]
>[Önceki](complex-data-model.md)
>[İleri](update-related-data.md)
