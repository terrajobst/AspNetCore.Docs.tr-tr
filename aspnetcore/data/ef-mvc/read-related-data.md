---
title: "ASP.NET Core MVC EF çekirdek ile-ilgili verileri - 6 10 okuma"
author: tdykstra
description: "Bu öğreticide okuyun ve ilgili verileri--diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler verileri görüntüleyin."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 1321cb00a432669b4a97ad20063b6cf9ea75f24c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="reading-related-data---ef-core-with-aspnet-core-mvc-tutorial-6-of-10"></a>Okuma ilgili verileri - EF çekirdek ASP.NET Core MVC Öğreticisi (6 10)

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması Entity Framework Çekirdek ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](intro.md).

Önceki öğreticide Okul veri modeli tamamlandı. Bu öğreticide, okuma ve ilgili verileri--diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler verileri görüntüleyin.

Aşağıdaki çizimler ile karşılaşmayacağınızı sayfalarında gösterilir.

![Kurslar dizin sayfası](read-related-data/_static/courses-index.png)

![Eğitmen dizin sayfası](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>İstekli, açık ve geç ilgili veri yükleme

Çeşitli yolları vardır, nesne ilişkisel eşleme (ORM) yazılım gibi Entity Framework ilgili verileri bir varlık Gezinti özellikleri yükleyebilirsiniz:

* İstekli yükleniyor. Varlık okurken ilgili verileri onunla birlikte alınır. Bu, genellikle tüm gerekli olan verileri alan bir tek birleştirme sorgusunda sonuçlanır. Kullanarak Entity Framework Çekirdek belirttiğiniz istekli yükleme `Include` ve `ThenInclude` yöntemleri.

  ![İstekli yükleme örneği](read-related-data/_static/eager-loading.png)

  Bazı ayrı sorgulardaki verileri alabilir ve EF "Gezinti özellikleri düzeltmelerini".  Diğer bir deyişle, EF daha önce alınan varlık Gezinti özellikleri ait oldukları ayrı olarak alınan varlıklar otomatik olarak ekler. İlgili verileri alır sorgu için kullandığınız `Load` yöntemi gibi bir liste veya nesnesi döndüren bir yöntem yerine `ToList` veya `Single`.

  ![Ayrı sorgulara örneği](read-related-data/_static/separate-queries.png)

* Açık yükleniyor. Varlık ilk okunduğunda ilgili verileri alınan değil. Gerekli olup olmadığını ilgili verileri alır kod yazın. Ayrı sorgularıyla yüklenirken eager durumda olduğu gibi birden çok sorgu sonuçlarında yüklenirken açık veritabanına gönderilir. Açık yükleme ile yüklenmesi Gezinti özellikleri kod belirtir farktır. Entity Framework Çekirdek 1.1 kullandığınız `Load` açık yükleme yapmak için yöntem. Örneğin:

  ![Açık yükleme örneği](read-related-data/_static/explicit-loading.png)

* Yavaş yükleniyor. Varlık ilk okunduğunda ilgili verileri alınan değil. Ancak, bir gezinti özelliği erişme girişimi ilk kez, o gezinti özelliği için gerekli olan veriler otomatik olarak alınır. Bir sorgu her zaman ilk olarak bir gezinti özelliğinden veri almaya çalışın veritabanına gönderilir. Entity Framework Çekirdek 1.0 yavaş yüklenmesini desteklemez.

### <a name="performance-considerations"></a>Performans değerlendirmeleri

İlgili verileri alınan her varlık için gereksinim duyduğunuz biliyorsanız, veritabanına gönderilen tek bir sorgu genellikle daha verimlidir alınan her varlık için ayrı sorgulara olduğundan istekli yüklenirken genellikle en iyi performansı sunar. Örneğin, her bölüm on ilgili kursları olduğunu varsayalım. Tüm ilgili veriler istekli yüklenmesini yalnızca bir tek (birleştirme) sorgu ve tek bir gidiş dönüş veritabanına neden olur. Her bölüm için kurslara ayrı bir sorgu içinde on gidiş dönüş veritabanına neden olur. Gecikmenin yüksek olduğu zaman veritabanına ek gidiş dönüş performansı için özellikle detrimental.

Diğer taraftan, bazı senaryolarda ayrı sorgulara daha verimli. Bir sorgu tüm ilgili veriler istekli yüklenmesini hangi SQL Server verimli bir şekilde işleyemiyor oluşturulması, bir çok karmaşık birleştirme neden olabilir. Veya yalnızca bir alt kümesini işleme varlık kümesi için bir varlığın gezinme özelliklerinin erişmeniz gerekiyorsa, ayrı sorgulara Önden her şeyi istekli yüklenmesini ihtiyacınız daha fazla veri almak çünkü daha iyi gerçekleştirebilir. Performans önemliyse, performansı en iyi seçim yapmak için her iki yönde test en iyisidir.

## <a name="create-a-courses-page-that-displays-department-name"></a>Bölüm adı görüntüler kurslar sayfası oluşturma

İndirmelere varlık indirmelere atandığı bölümünün departman varlığı içeren bir gezinme özelliği içerir. Atanan departmanı adını kurslar listesini görüntülemek için olan departman varlık Name özelliği almanız gereken `Course.Department` gezinti özelliği.

Aynı seçenekleri kullanarak indirmelere varlık türü için CoursesController adlı bir denetleyicisi oluşturmak **görünümleri ile MVC Entity Framework kullanarak denetleyicisi** önceki Öğrenciler denetleyici için gösterildiği gibi yaptığınız iskele kurucu Aşağıdaki çizimde:

![Kurslar denetleyici ekleyin](read-related-data/_static/add-courses-controller.png)

Açık *CoursesController.cs* ve inceleyin `Index` yöntemi. Otomatik yapı iskelesi istekli yükleme için belirtilen sahip `Department` kullanarak gezinti özelliği `Include` yöntemi.

Değiştir `Index` yöntemi için daha uygun bir ad kullanan aşağıdaki kod ile `IQueryable` indirmelere varlıklar döndürüyor (`courses` yerine `schoolContext`):

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

Açık *Views/Courses/Index.cshtml* ve şablon kodu aşağıdaki kodla değiştirin. Değişiklikleri vurgulanmıştır:

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

İskele kurulmuş kod aşağıdaki değişiklikleri yaptınız:

* Başlık dizinden kurslara değiştirildi.

* Eklenen bir **numarası** gösterir sütun `CourseID` özellik değeri. Normalde son kullanıcılara anlamsız çünkü bunlar varsayılan olarak, birincil anahtarlar iskele kurulmuş değil. Ancak, bu durumda birincil anlamlı anahtarıdır ve bunu göstermek istediğiniz.

* Değiştirilen **departmanı** bölüm adını görüntülemek için sütun. Kod görüntüler `Name` yüklenen departmanı varlık özelliği `Department` gezinti özelliği:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Uygulamayı çalıştırın ve seçin **kurslar** bölüm adlarını listesiyle görmek için sekmesini.

![Kurslar dizin sayfası](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Kurslar ve kayıtları gösteren bir eğitmen sayfası oluşturun

Bu bölümde Eğitmen sayfasını görüntülemek için bir denetleyici ve görünüm Eğitmen varlığın oluşturacaksınız:

![Eğitmen dizin sayfası](read-related-data/_static/instructors-index.png)

Bu sayfayı okur ve ilgili verileri aşağıdaki yollarla görüntüler:

* Eğitmen listesini OfficeAssignment varlığın ilgili verileri görüntüler. Eğitmen ve OfficeAssignment bir tane-sıfır-veya-bir ilişkide varlıklardır. İstekli yükleme OfficeAssignment varlıklar için kullanacaksınız. Birincil tablonun tüm alınan satırları için ilgili verileri gerektiğinde daha önce açıklandığı gibi istekli yükleme genellikle daha verimli olur. Bu durumda, tüm görüntülenen Eğitmen office atamaları görüntülemek istediğiniz.

* Kullanıcı bir eğitmen seçtiğinde ilgili indirmelere varlıkları görüntülenir. Eğitmen ve indirmelere bir çok-çok ilişkisi varlıklardır. İstekli yükleme indirmelere varlıkları ve bunların ilişkili departmanı varlıklar için kullanacaksınız. Bu durumda, yalnızca seçili eğitmen için kurslar gerektiğinden ayrı sorgulara daha etkili olabilir. Ancak, bu örnek istekli yükleme için Gezinti özellikleri kendilerini Gezinti özelliklerdir varlıkları içinde nasıl kullanıldığını gösterir.

* Kullanıcı bir indirmelere seçtiğinde kayıtları varlık kümesindeki ilgili veriler görüntülenir. İndirmelere ve kayıt bir-çok ilişkisi varlıklardır. Kayıt varlıkları ve bunların ilgili Öğrenci varlıklar için ayrı sorgulara kullanacaksınız.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Eğitmen dizin görünüm için Görünüm modeli oluşturma

Eğitmen sayfanın üç farklı tablolardan verileri gösterir. Bu nedenle, her biri tablolar için verileri tutan üç özellik içeren bir görünüm modeli oluşturacaksınız.

İçinde *SchoolViewModels* klasörü oluşturmak *InstructorIndexData.cs* ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Eğitmen denetleyicisi ve görünümler oluşturma

Aşağıdaki çizimde gösterildiği gibi EF okuma/yazma eylemleri ile bir eğitmen denetleyicisi oluşturun:

![Eğitmen denetleyici ekleyin](read-related-data/_static/add-instructors-controller.png)

Açık *InstructorsController.cs* ve kullanarak bir ekleme deyimi ViewModels ad alanı için:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

Dizin yöntemi, ilgili verilerin istekli yükleme yapmak ve görünüm modelinde put aşağıdaki kodla değiştirin.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

Yöntem isteğe bağlı rota veri kabul eder (`id`) ve bir sorgu dizesi parametresi (`courseID`) seçili Eğitmen ve seçili indirmelere kimliği değerlerini sağlayın. Parametreleri tarafından sağlanan **seçin** sayfasında bağlar.

Kod görünüm modeli örneği oluşturmayı ve bunu Eğitmen listesi koyma başlar. İstekli yükleme için kod belirtir `Instructor.OfficeAssignment` ve `Instructor.CourseAssignments` Gezinti özellikleri. İçinde `CourseAssignments` özelliği, `Course` özelliği yüklü ve, içinde `Enrollments` ve `Department` özellikleri yüklenir ve her içinde `Enrollment` varlık `Student` özelliği yüklenir.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

Görünümü her zaman OfficeAssignment varlık gerektirdiğinden, aynı sorguda fetch daha etkilidir. Yalnızca sayfa daha sık olmadan daha seçili indirmelere ile görüntüleniyorsa, tek bir sorgu birden fazla sorgu iyi olacak şekilde bir eğitmen web sayfasında seçildiğinde indirmelere varlıklar gereklidir.

Kod yineler `CourseAssignments` ve `Course` iki özelliklerinden gerektiğinden `Course`. İlk dizesi `ThenInclude` alır çağırır `CourseAssignment.Course`, `Course.Enrollments`, ve `Enrollment.Student`.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

Bu noktada kodda başka bir `ThenInclude` Gezinti özellikleri için olacaktır `Student`, gereken yok. Ancak arama `Include` üzerinden başlayan `Instructor` zinciri yeniden, bu zaman belirtme gitmek zorunda şekilde özellikleri `Course.Department` yerine `Course.Enrollments`.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

Bir eğitmen seçildiğinde aşağıdaki kodu yürütür. Seçili Eğitmen görünüm modeli Eğitmen listesi alınır. Görünüm modelinin `Courses` özelliği Bu eğitmen indirmelere varlıklardan ile yüklenen sonra `CourseAssignments` gezinti özelliği.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

`Where` Yöntem koleksiyonu döndürür, ancak ölçütlere bu durumda döndürülen yalnızca tek bir eğitmen varlık o yöntemi sonuç. `Single` Yöntemi bu varlığın erişim sağlayan tek bir eğitmen varlık koleksiyonu dönüştürür `CourseAssignments` özelliği. `CourseAssignments` Özelliği içeren `CourseAssignment` istediğiniz yalnızca ilgili varlıklar `Course` varlıklar.

Kullandığınız `Single` koleksiyonu bildiğiniz durumlarda bir koleksiyon yöntemi yalnızca bir öğe olacaktır. Tek bir yöntem kendisine geçirilen koleksiyonu boş ise veya birden çok öğe varsa bir özel durum oluşturur. Bir alternatif `SingleOrDefault`, hangi varsayılan değeri döndürür (Bu durumda null) koleksiyonu boş ise. Ancak, bu durumda hala sonuçlanacak bir özel durum (bulunmaya çalışılırken gelen bir `Courses` özelliği bir null başvuru), ve özel durum iletisi daha az açıkça sorunun nedenini gösterir. Çağırdığınızda `Single` yöntemi, nerede ayrıca iletebilir koşul çağırmak yerine `Where` yöntemi ayrı olarak:

```csharp
.Single(i => i.ID == id.Value)
```

Onun yerine:

```csharp
.Where(I => i.ID == id.Value).Single()
```

Ardından, bir indirmelere seçildiyse, görünüm modeli kurslar listesinden seçili indirmelere alınır. Ardından Görünüm modelinin `Enrollments` özelliği bu indirmelere 's kayıt varlıklardan olarak yüklenen `Enrollments` gezinti özelliği.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a>Eğitmen dizini görünümünü değiştirme

İçinde *Views/Instructors/Index.cshtml*, şablon kodu aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

Var olan kodu aşağıdaki değişiklikleri yaptınız:

* Model sınıfı değiştirilmiş `InstructorIndexData`.

* Sayfa başlığının değiştirilen **dizin** için **Eğitmen**.

* Eklenen bir **Office** görüntüleyen sütun `item.OfficeAssignment.Location` yalnızca `item.OfficeAssignment` null değil. (Bu bir tane-sıfır-veya-bir ilişkisi olduğundan, olmayabilir ilgili OfficeAssignment varlık.)

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
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Etiketli yeni bir köprü eklenen **seçin** hemen diğer bağlantıları önce her satır, neden olan gönderilmesi için seçilen Eğitmen kimliği `Index` yöntemi.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Uygulamayı çalıştırın ve seçin **Eğitmen** sekmesi. Hiçbir ilgili OfficeAssignment varlık olduğunda ilgili OfficeAssignment varlıkları ve boş tablo hücresi konum özelliği sayfasını görüntüler.

![Seçili hiçbir şey Eğitmen dizin sayfası](read-related-data/_static/instructors-index-no-selection.png)

İçinde *Views/Instructors/Index.cshtml* kapatma tablo sonra öğesi (sonunda dosyası), dosya aşağıdaki kodu ekleyin. Bu kod bir eğitmen seçildiğinde bir eğitmen ilgili kurslar listesini görüntüler.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

Bu kod okuyan `Courses` kurslar listesini görüntülemek için görünümü modelin özelliği. Ayrıca sağlayan bir **seçin** seçili indirmelere Kimliğini gönderir köprü `Index` eylem yöntemi.

Sayfayı yenileyin ve bir eğitmen seçin. Artık seçili Eğitmen atanan kurslar görüntüleyen bir kılavuz görebilir ve her indirmelere için atanan departmanı adını görebilirsiniz.

![Seçili Eğitmen dizin sayfası Eğitmen](read-related-data/_static/instructors-index-instructor-selected.png)

Yeni eklediğiniz kod bloğu sonra aşağıdaki kodu ekleyin. Bu indirmelere seçildiğinde bu bir indirmelere kaydedilen Öğrenciler listesini görüntüler.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

Bu kod, indirmelere kayıtlı Öğrenciler listesini görüntülemek için Görünüm modeli kayıtları özelliği okur.

Sayfayı yenileyin ve bir eğitmen seçin. Ardından bir indirmelere kayıtlı Öğrenciler ve bunların dereceleri listesini görmek için seçin.

![Eğitmen dizin sayfası Eğitmen ve seçili indirmelere](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a>Açık yükleniyor

Alınan zaman içinde Eğitmen listesi *InstructorsController.cs*, istekli yükleme için belirtilen `CourseAssignments` gezinti özelliği.

Nadiren seçili Eğitmen ve indirmelere kayıtları görmek istediğiniz kullanıcıları beklenen varsayalım. Bu durumda, yalnızca isteniyorsa kayıt verileri yüklemek isteyebilirsiniz. Açık yükleme yapmak nasıl bir örnek görmek için değiştirme `Index` yöntemini aşağıdaki kodla istekli yükleme kayıtları için kaldırır ve bu özellik açıkça yükler. Kod değişiklikleri vurgulanır.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

Yeni kod bırakır *ThenInclude* yöntemi Eğitmen varlıklar alır kodundan için kayıt verilerini çağırır. Eğitmen ve indirmelere seçtiyseniz, seçili indirmelere için kayıt varlıkları ve Öğrenci varlıklar her kayıt için vurgulanmış kodu alır.

Çalışma Veri nasıl alındığını değiştirdiniz ancak şimdi Eğitmen dizin sayfasına gidin ve ve, uygulama sayfasında, görüntülenen içinde herhangi bir fark görürsünüz.

## <a name="summary"></a>Özet

Şimdi istekli yükleme birden çok sorgu ile bir sorgu ile ilgili verileri Gezinti özelliklerini okumak için kullandığınız. Sonraki öğreticide ilgili verileri güncelleştirmek öğreneceksiniz.

>[!div class="step-by-step"]
>[Önceki](complex-data-model.md)
>[sonraki](update-related-data.md)  
