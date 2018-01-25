---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "ASP.NET MVC uygulamasındaki Entity Framework ile ilgili verileri okuma | Microsoft Docs"
author: tdykstra
description: /ajax/tutorials/using-ajax-control-toolkit-controls-and-control-extenders-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 7a74d01f306abeeac5ac28c942f03001e0fe00f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>ASP.NET MVC uygulamasındaki Entity Framework Veri Okuma ilgili
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) veya [PDF indirin](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio 2013 kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Önceki öğreticide Okul veri modeli tamamlandı. Bu öğreticide okuma ve ilgili verileri görüntüleme — diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler veri.

Aşağıdaki çizimler ile karşılaşmayacağınızı sayfalarında gösterilir.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Yavaş, istekli ve açık ilgili veri yükleme

Entity Framework bir varlık Gezinti özellikleri ilgili verileri yükleyebilir birkaç yolu vardır:

- *Yavaş Yükleniyor*. Varlık ilk okunduğunda ilgili verileri alınan değil. Ancak, bir gezinti özelliği erişme girişimi ilk kez, o gezinti özelliği için gerekli olan veriler otomatik olarak alınır. Bu veritabanına gönderilen birden çok sorgu sonuçlanır — bir varlık için ve bir varlık için veri ilgili her zaman alınması gerekir. `DbContext` Sınıfı, varsayılan olarak yavaş yüklenmesini sağlar. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *İstekli yükleme*. Varlık okurken ilgili verileri onunla birlikte alınır. Bu, genellikle tüm gerekli olan verileri alan bir tek birleştirme sorgusunda sonuçlanır. Kullanarak istekli yükleme belirtin `Include` yöntemi.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Açık yükleme*. Açıkça kod ilgili verileri almak bu yavaş yüklemeye benzerdir; bir gezinti özelliğine erişirken otomatik olarak gerçekleşmez. İlgili verileri el ile bir varlık ve arama için nesne durumu Yöneticisi girdisi alarak yüklediğiniz [Collection.Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx) koleksiyonlar için yöntemi veya [Reference.Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx) yöntemi tutun özellikleri için bir tek bir varlık. (Aşağıdaki örnekte, yönetici gezinti özelliği yüklemek istiyorsanız, değiştirirler `Collection(x => x.Courses)` ile `Reference(x => x.Administrator)`.) Genellikle, yalnızca devre dışı yüklenirken yavaş açtığınız zaman açık yüklenirken kullanırsınız.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Özellik değerlerini hemen almak yoktur çünkü yavaş yükleniyor ve açık yükleme da her ikisini de olarak da bilinir *yüklenirken ertelenmiş*.

### <a name="performance-considerations"></a>Performans değerlendirmeleri

İlgili verileri alınan her varlık için gereksinim duyduğunuz biliyorsanız, veritabanına gönderilen tek bir sorgu genellikle daha verimlidir alınan her varlık için ayrı sorgulara olduğundan istekli yüklenirken genellikle en iyi performansı sunar. Örneğin, Yukarıdaki örneklerde, her bölüm on ilgili kursları olduğunu varsayalım. İstekli yükleme örneği yalnızca bir tek (birleştirme) sorgu ve tek bir gidiş dönüş veritabanına neden olur. Yavaş yükleniyor ve açık yükleme örnekleri hem de on sorgular ve on gidiş dönüş veritabanına neden olur. Gecikmenin yüksek olduğu zaman veritabanına ek gidiş dönüş performansı için özellikle detrimental.

Diğer taraftan, bazı senaryolarda yavaş yükleniyor daha etkilidir. İstekli yükleme, SQL Server verimli bir şekilde işleyemiyor oluşturulması, bir çok karmaşık birleştirme neden olabilir. Veya yalnızca bir alt kümesinin varlık için bir varlığın gezinme özelliklerinin erişmeniz gerekiyorsa işleme istekli yükleme ihtiyacınız daha fazla veri almak çünkü yavaş yükleniyor daha iyi gerçekleştirebilir. Performans önemliyse, performansı en iyi seçim yapmak için her iki yönde test en iyisidir.

Yavaş yükleniyor performans sorunlarına neden olan kod maskeleyebilirsiniz. Örneğin, açık veya istekli yükleme belirtmiyor ancak varlıklar yüksek hacimli işler ve her yinelemede birkaç Gezinti özellikleri kullanan kodu (nedeniyle, çok sayıda gidiş dönüş veritabanına) çok verimsiz olabilir. İyi bir şirket içi SQL server kullanarak geliştirme gerçekleştiren bir uygulama için Azure SQL veritabanı daha yüksek gecikme süresi ve yavaş yükleniyor nedeniyle taşındıklarında performans sorunları olabilir. Gerçekçi test yük veritabanı sorgularıyla profil yavaş yükleniyor uygun olup olmadığını belirleme yardımcı olur. Daha fazla bilgi için bkz: [Demystifying Entity Framework stratejileri: ilgili veri yükleme](https://msdn.microsoft.com/magazine/hh205756.aspx) ve [SQL Azure ağ gecikmesini azaltmak için Entity Framework kullanarak](https://msdn.microsoft.com/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Seri hale getirme önce geç yükleme devre dışı bırak

Geç yükleme seri hale getirme sırasında etkin bırakırsanız amaçladığınız önemli ölçüde daha fazla veri sorgulama yukarı sona erdirebilirsiniz. Seri hale getirme, genellikle bir türünün bir örneği üzerinde her bir özellik erişerek çalışır. Özellik erişimi yavaş yükleniyor tetikler ve bu yavaş yüklenen varlıkların seri. Seri hale getirme işlemi daha sonra her bir özellik daha da yavaş yükleniyor ve Serileştirme olası neden yüklenen yavaş varlıkların erişir. Bu run-away zinciri tepki önlemek için devre dışı bir varlık seri önce yükleme yavaş açın.

Serileştirme Ayrıca karmaşık Entity Framework kullanan proxy sınıfları tarafından açıklandığı şekilde [Gelişmiş senaryoları Öğreticisi](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

Seri hale getirme sorunları önlemek için bir yoldur varlık nesnesi yerine veri aktarımı nesneleri (DTOs) serileştirmek için gösterildiği gibi [Entity Framework ile Web API kullanarak](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) Öğreticisi.

DTOs kullanmıyorsanız, yavaş yükleniyor devre dışı bırakabilir ve proxy sorunlarından kaçınmak [proxy oluşturma devre dışı bırakma](https://msdn.microsoft.com/data/jj592886.aspx).

Bazı diğer işte [geç yükleme devre dışı bırakmak için yollar](https://msdn.microsoft.com/data/jj574232):

- Belirli Gezinti özellikleri için atlayın `virtual` özelliği bildirirken anahtar sözcüğü.
- Tüm gezinti özelliklerini ayarlama `LazyLoadingEnabled` için `false`, aşağıdaki kodu bağlamı sınıfınız oluşturucuda koyun: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page-that-displays-department-name"></a>Bu görüntüler bölüm adı kurslar sayfa oluşturma

`Course` Varlık içeren bir gezinme özelliği içeren `Department` indirmelere atandığı bölümünün varlık. Atanan departmanı adını kurslar listesinde görüntülenecek almanız gereken `Name` özelliğinden `Department` olan varlık `Course.Department` gezinti özelliği.

Adlı bir denetleyicisi oluşturmak `CourseController` (CoursesController değil) için `Course` varlık türü için aynı seçenekleri kullanarak **Entity Framework kullanarak MVC 5 denetleyici, görünümleri olan** içindahaönceyaptığınıziskelekurucu`Student` aşağıdaki çizimde gösterildiği gibi denetleyicisi:

![Add_Controller_dialog_box_for_Course_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Açık *Controllers\CourseController.cs* ve bakmak `Index` yöntemi:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Otomatik yapı iskelesi istekli yükleme için belirtilen sahip `Department` kullanarak gezinti özelliği `Include` yöntemi.

Açık *Views\Course\Index.cshtml* ve şablon kodu aşağıdaki kodla değiştirin. Değişiklikleri vurgulanmıştır:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

İskele kurulmuş kod aşağıdaki değişiklikleri yaptınız:

- Başlığından değiştirilen **dizin** için **kurslar**.
- Eklenen bir **numarası** gösterir sütun `CourseID` özellik değeri. Normalde son kullanıcılara anlamsız çünkü bunlar varsayılan olarak, birincil anahtarlar iskele kurulmuş değil. Ancak, bu durumda birincil anlamlı anahtarıdır ve bunu göstermek istediğiniz.
- Taşınan **departmanı** sağ tarafında sütununa ve başlığını değiştirildi. İskele kurucu düzgün görüntülemek seçtiğiniz `Name` özelliğinden `Department` varlık, ancak sütun başlığı olmalıdır indirmelere sayfa burada **departmanı** yerine **adı**.

Bölüm sütunu için kurulmuş kodu görüntülendiğine dikkat edin `Name` özelliği `Department` yüklenen varlık `Department` gezinti özelliği:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Sayfayı çalıştırın (seçin **kurslar** Contoso University giriş sayfası sekmesinde) bölüm adlarını listesiyle görmek için.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Kurslar ve kayıtları gösteren bir eğitmen sayfası oluşturun

Bu bölümde bir denetleyici oluşturacak ve görüntüleme `Instructor` varlık Eğitmen sayfasını görüntülemek için:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Bu sayfayı okur ve ilgili verileri aşağıdaki yollarla görüntüler:

- İlgili verileri Eğitmen listesini görüntüleyen `OfficeAssignment` varlık. `Instructor` Ve `OfficeAssignment` varlıklardır bir tane-sıfır-veya-bir ilişkisi. İstekli yükleme için kullanacağınız `OfficeAssignment` varlıklar. Birincil tablonun tüm alınan satırları için ilgili verileri gerektiğinde daha önce açıklandığı gibi istekli yükleme genellikle daha verimli olur. Bu durumda, tüm görüntülenen Eğitmen office atamaları görüntülemek istediğiniz.
- Kullanıcı ilişkili bir eğitmen seçtiğinde `Course` varlıkları görüntülenir. `Instructor` Ve `Course` varlıklardır bir çok-çok ilişkisi. İstekli yükleme için kullanacağınız `Course` varlıkları ve bunların ilgili `Department` varlıklar. Bu durumda, yalnızca seçili eğitmen için kurslar gerektiğinden yavaş yükleniyor daha etkili olabilir. Ancak, bu örnek istekli yükleme için Gezinti özellikleri kendilerini Gezinti özelliklerdir varlıkları içinde nasıl kullanıldığını gösterir.
- Kullanıcı bir indirmelere seçtiğinde verilerden ilgili `Enrollments` varlık kümesi görüntülenir. `Course` Ve `Enrollment` varlıklardır bir-çok ilişkisi. Açık yükleme için ekleyeceksiniz `Enrollment` varlıkları ve bunların ilgili `Student` varlıklar. (Açık yükleme yavaş yükleniyor etkindir, ancak bu açık yükleme yapmak nasıl gösterilir çünkü gerekli değildir.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Bir görünüm modeli Eğitmen dizini görünümünü oluşturun

Eğitmen sayfanın üç farklı tabloları gösterir. Bu nedenle, her biri tablolar için verileri tutan üç özellik içeren bir görünüm modeli oluşturacaksınız.

İçinde *ViewModels* klasörü oluşturmak *InstructorIndexData.cs* ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Eğitmen denetleyicisi ve görünümler oluşturma

Oluşturma bir `InstructorController` (InstructorsController değil) aşağıdaki çizimde gösterildiği gibi EF okuma/yazma eylemleri ile denetleyicisi:

![Add_Controller_dialog_box_for_Instructor_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Açık *Controllers\InstructorController.cs* ve ekleme bir `using` bildirimi `ViewModels` ad alanı:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

İskele kurulmuş kodu `Index` yöntemi belirtir. yalnızca istekli yükleme `OfficeAssignment` gezinti özelliği:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Değiştir `Index` yöntemi ek yüklemek için aşağıdaki kod ile ilgili verilerin ve görünüm modelinde yerleştirin:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Yöntem isteğe bağlı rota veri kabul eder (`id`) ve bir sorgu dizesi parametresi (`courseID`), seçilen Eğitmen ve seçili indirmelere kimliği değerlerini sağlayın ve tüm gerekli veri görünümüne geçirir. Parametreleri tarafından sağlanan **seçin** sayfasında bağlar.

Kod görünüm modeli örneği oluşturmayı ve bunu Eğitmen listesi koyma başlar. İstekli yükleme için kod belirtir `Instructor.OfficeAssignment` ve `Instructor.Courses` gezinti özelliği.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

İkinci `Include` yöntemi kurslar yükler ve yüklenen her indirmelere için istekli yükleme mevcut `Course.Department` gezinti özelliği.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Daha önce belirtildiği gibi istekli yüklenmesi gerekli değildir ancak performansı artırmak için yapılır. Görünümü her zaman gerektirdiğinden `OfficeAssignment` varlık, onu aynı sorguda fetch daha verimli. `Course`web sayfasında bir eğitmen seçildiğinde istekli yükleme yalnızca sayfa daha sık olmadan daha seçili indirmelere ile görüntüleniyorsa, yavaş yüklemekten daha iyi şekilde varlıklar gereklidir.

Eğitmen kimliği seçildiyse, seçili Eğitmen görünüm modeli Eğitmen listesi alınır. Görünüm modelinin `Courses` özelliği ile yüklenen sonra `Course` Bu eğitmen varlıklardan `Courses` gezinti özelliği.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`Where` Yöntem koleksiyonu döndürür, ancak bu yöntem sonucu yalnızca tek bir ölçüt bu durumda geçirilen `Instructor` döndürülen varlık. `Single` Yöntemi tek bir koleksiyon dönüştürür `Instructor` bu varlığın erişmenizi varlık `Courses` özelliği.

Kullandığınız [tek](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) koleksiyonu bildiğiniz durumlarda bir koleksiyon yöntemi yalnızca bir öğe olacaktır. `Single` Yöntem kendisine geçirilen koleksiyonu boş ise veya birden çok öğe varsa bir özel durum oluşturur. Bir alternatif [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), varsayılan değeri döndürür (`null` bu durumda) koleksiyonu boş ise. Ancak, bu durumda hala sonuçlanacak bir özel durum (bulunmaya çalışılırken gelen bir `Courses` özelliği bir `null` başvuru), ve özel durum iletisi daha az açıkça sorunun nedenini gösterir. Çağırdığınızda `Single` yöntemi, siz de geçirebilir `Where` çağırmak yerine koşulu `Where` yöntemi ayrı olarak:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Onun yerine:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Ardından, bir indirmelere seçildiyse, görünüm modeli kurslar listesinden seçili indirmelere alınır. Ardından Görünüm modelinin `Enrollments` özelliği ile yüklenir `Enrollment` bu indirmelere 's varlıklardan `Enrollments` gezinti özelliği.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Eğitmen dizini görünümünü değiştirme

İçinde *Views\Instructor\Index.cshtml*, şablon kodu aşağıdaki kodla değiştirin. Değişiklikleri vurgulanmıştır:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Var olan kodu aşağıdaki değişiklikleri yaptınız:

- Model sınıfı değiştirilmiş `InstructorIndexData`.
- Sayfa başlığının değiştirilen **dizin** için **Eğitmen**.
- Eklenen bir **Office** görüntüleyen sütun `item.OfficeAssignment.Location` yalnızca `item.OfficeAssignment` null değil. (Bu bir tane-sıfır-veya-bir ilişkisi olduğundan, olmayabilir ilgili `OfficeAssignment` varlık.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Dinamik olarak ekleyecek eklenen kod `class="success"` için `tr` seçili Eğitmen öğesidir. Bu ayarlar kullanarak seçili satır için bir arka plan rengi bir [önyükleme](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) sınıfı. 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Yeni eklenen `ActionLink` etiketli **seçin** hemen diğer bağlantıları önce her satır, neden olan gönderilmesini seçili Eğitmen kimliği `Index` yöntemi.

Uygulamayı çalıştırın ve seçin **Eğitmen** sekmesi. Sayfa görüntüler `Location` ilgili özelliğinin `OfficeAssignment` varlıkları ve boş bir tablo hücresi olduğunda ilgili Hayır `OfficeAssignment` varlık.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

İçinde *Views\Instructor\Index.cshtml* kapatma sonra dosyayı `table` öğesi (sonunda dosyası), aşağıdaki kodu ekleyin. Bu kod bir eğitmen seçildiğinde bir eğitmen ilgili kurslar listesini görüntüler.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Bu kod okuyan `Courses` kurslar listesini görüntülemek için görünümü modelin özelliği. Ayrıca sağlayan bir `Select` seçili indirmelere Kimliğini gönderir köprü `Index` eylem yöntemi.

Sayfayı çalıştırın ve bir eğitmen seçin. Artık seçili Eğitmen atanan kurslar görüntüleyen bir kılavuz görebilir ve her indirmelere için atanan departmanı adını görebilirsiniz.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Yeni eklediğiniz kod bloğu sonra aşağıdaki kodu ekleyin. Bu indirmelere seçildiğinde bu bir indirmelere kaydedilen Öğrenciler listesini görüntüler.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Bu kod okuyan `Enrollments` Öğrenciler listesini görüntülemek için görünümü modelin özelliği indirmelere kayıtlı.

Sayfayı çalıştırın ve bir eğitmen seçin. Ardından bir indirmelere kayıtlı Öğrenciler ve bunların dereceleri listesini görmek için seçin.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

### <a name="adding-explicit-loading"></a>Açık yükleme ekleme

Açık *InstructorController.cs* ve nasıl göz `Index` yöntemi kayıtları için seçilen indirmelere listesini alır:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Eğitmen listesi alınırken istekli yükleme için belirtilen `Courses` gezinti özelliği ve `Department` her indirmelere özelliği. Yerleştirdiğiniz sonra `Courses` koleksiyonu görünüm modeli ve eriştiğiniz artık `Enrollments` koleksiyondaki bir varlıktan gezinti özelliği. İstekli yükleme için belirtmediğiniz çünkü `Course.Enrollments` gezinti özelliği, bu özellik verileri görünmesini yavaş yükleniyor sonucunda sayfasındaki.

Diğer herhangi bir yolla kod değiştirmeden geç yükleme devre dışı bırakılırsa `Enrollments` özelliği indirmelere aslında vardı kaç kayıtları bağımsız olarak boş olacaktır. Yüklemek için bu durumda, `Enrollments` özelliği zorunda istekli yükleme ya da açık yükleme belirtin. Zaten istekli yükleme yapmak öğrendiniz. Açık yükleyen bir örnek görmek için değiştirmek `Index` açıkça yükler aşağıdaki kod ile yöntemi `Enrollments` özelliği. Değiştirilen kod vurgulanır.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

Seçili alma sonra `Course` varlık, yeni kod açıkça yükler, indirmelere 's `Enrollments` gezinti özelliği:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

Her açıkça yüklerken sonra `Enrollment` ilgili varlık `Student` varlık:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Kullandığınız bildirimi `Collection` yöntemi bir koleksiyon özelliği yüklemek için ancak yalnızca bir varlık tutan bir özellik için kullandığınız `Reference` yöntemi.

Eğitmen dizin sayfası şimdi çalıştırmak ve verileri nasıl alınıp değiştirdiniz ancak sayfasında görüntülenenleri içinde herhangi bir fark görürsünüz.

## <a name="summary"></a>Özet

Şimdi, ilgili veri gezinme özelliklerini yüklemek için tüm üç yol (yavaş, istekli ve açık) kullandığınız. Sonraki öğreticide ilgili verileri güncelleştirmek öğreneceksiniz.

Lütfen geri bildirim, Bu öğretici beğendiğinizi nasıl ve ne biz artabileceğini bırakın. En yeni konular da isteğinde bulunabilirsiniz [Göster bana nasıl kodu ile](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access - kaynakları önerilen](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Önceki](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
[sonraki](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
