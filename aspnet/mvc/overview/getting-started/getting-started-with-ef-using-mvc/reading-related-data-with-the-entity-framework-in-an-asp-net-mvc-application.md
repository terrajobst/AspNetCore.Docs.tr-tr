---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Öğretici: ASP.NET MVC uygulaması EF ile ilgili verileri okuma'
description: Bu öğreticide okuma ve ilgili verileri görüntüleyen — diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler veri.
author: tdykstra
ms.author: riande
ms.date: 01/17/2019
ms.topic: tutorial
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 8660a75655b801364cce7c4b59847c5c00562a27
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396213"
---
# <a name="tutorial-read-related-data-with-ef-in-an-aspnet-mvc-app"></a>Öğretici: ASP.NET MVC uygulaması EF ile ilgili verileri okuma

Önceki öğreticide Okul veri modeli tamamlandı. Bu öğreticide okuma ve ilgili verileri görüntüleyen — diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler veri.

Aşağıdaki çizimler ile çalışmak sayfaları göstermektedir.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * İlgili veri yükleme konusunda bilgi edinin
> * Kursları sayfası oluşturma
> * Eğitmenler sayfası oluşturma

## <a name="prerequisites"></a>Önkoşullar

* [Daha karmaşık bir veri modeli oluşturma](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="learn-how-to-load-related-data"></a>İlgili veri yükleme konusunda bilgi edinin

Entity Framework Gezinti özelliklerini bir varlığın ilgili verileri yükleyebilir birkaç yolu vardır:

- *Yavaş Yükleniyor*. Varlığın ilk okunduğunda, ilgili verileri alınan değil. Ancak, bir gezinti özelliği erişmeye ilk kez otomatik olarak bu gezinti özelliği için gerekli olan veriler alınır. Birden çok sorgu veritabanına gönderilen sonuçlanır — varlık biri ilgili varlık için veriler her zaman alınması gerekir. `DbContext` Sınıfı varsayılan olarak yavaş yüklenmesini sağlar.

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *İstekli yükleme*. Varlık okunduğunda, onunla birlikte ilgili verileri alınır. Bu, tek bir birleşim sorguda tüm gerekli olan verileri alır. genellikle sonuçlanır. İstekli yükleme kullanarak belirttiğiniz `Include` yöntemi.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Açık yükleme*. Açıkça kod içinde ilgili verileri alma dışında bu yavaş yükleniyor için benzer; bir gezinti özelliğine erişirken otomatik olarak gerçekleşmez. İlgili verileri el ile bir varlık ve arama için nesne durumu Yöneticisi girişi alarak yüklediğiniz [Collection.Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx) koleksiyonlar için yöntemi veya [Reference.Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx) yöntemi tutun özellikleri için bir tek varlık. (Aşağıdaki örnekte, yönetici gezinti özelliği yüklemek istiyorsanız, değiştirirler `Collection(x => x.Courses)` ile `Reference(x => x.Administrator)`.) Genellikle, yalnızca kapalı yüklenirken yavaş etkinleştirdiyseniz, açık yükleme kullanırsınız.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Özellik değerlerini hemen almak yoktur çünkü yavaş yükleniyor ve açık yükleme da her ikisi de olarak da bilinir *Ertelenen yükleme*.

### <a name="performance-considerations"></a>Performans değerlendirmeleri

İlgili verileri alınan her varlık için gereksinim duyduğunuz biliyorsanız, veritabanına gönderilen tek bir sorgu genellikle daha verimli alınan her varlık için ayrı sorgulara olduğundan istekli yükleme genellikle en iyi performansı sunar. Örneğin, Yukarıdaki örneklerde, her departman on ilgili kurslar olduğunu varsayalım. İstekli yükleme örnek yalnızca bir tek (birleştirme) sorgu ve tek bir gidiş dönüş veritabanına neden olur. Yavaş yükleniyor ve açık yükleme örnekler hem de on sorgular ve on gidiş dönüş veritabanına neden olur. Gecikme süresi yüksek olduğunda veritabanına ek gidiş dönüş özellikle performansı düşürür.

Öte yandan, bazı senaryolarda yavaş yükleniyor daha verimli olur. İstekli yükleme, SQL Server'ın etkili bir şekilde işleyemiyor oluşturulması çok karmaşık birleştirme neden olabilir. Veya bir varlığın yalnızca bir alt kümesini bir dizi varlık Gezinti özellikleri erişmeniz gerekiyorsa, işleme, istekli yükleme ihtiyacınız olandan daha fazla veri alması nedeniyle yavaş yükleniyor daha iyi gerçekleştirebilir. Performans kritik ise, en iyi seçim yapmak için her iki yönde performansını test etmek idealdir.

Yavaş yükleniyor, performans sorunlarına neden olan kod maskeleyebilirsiniz. Örneğin, eager veya açık yükleme belirtmeyen ancak varlıkları yüksek hacimli işler ve her yinelemede birkaç Gezinti özellikleri kullanan kodu (nedeniyle veritabanı çok sayıda gidiş dönüş) çok verimsiz olabilir. İyi bir şirket içi SQL server'ı kullanarak geliştirme gerçekleştiren bir uygulama, Azure SQL veritabanı'na daha yüksek gecikme süresi ve yavaş yükleme nedeniyle taşındıklarında performans sorunları olabilir. Veritabanı sorguları gerçekçi test yük ile profil oluşturma, yavaş yükleniyor uygun olup olmadığını belirlemenize yardımcı olur. Daha fazla bilgi için [Demystifying Entity Framework stratejileri: İlgili verileri yükleme](https://msdn.microsoft.com/magazine/hh205756.aspx) ve [SQL Azure ağ gecikme süresini azaltmak için Entity Framework kullanarak](https://msdn.microsoft.com/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Serileştirme önce Gecikmeli yüklemeyi devre dışı

Yavaş yükleniyor serileştirme sırasında etkin bırakırsanız, amaçladığınız kıyasla önemli ölçüde daha fazla veri sorgulama yukarı sonlandırabilirsiniz. Seri hale getirme, genellikle bir tür örneği üzerinde her bir özellik erişerek çalışır. Özellik erişimini yavaş yükleniyor tetikler ve bu yavaş yüklenen varlıklara serileştirilir. Seri hale getirme işlemi daha sonra büyük olasılıkla daha yavaş yükleniyor ve Serileştirme neden yavaş yüklenen varlıkların her bir özellik erişir. Bu run-away ata'daki önlemek için devre dışı bir varlık seri hale getirme önce yükleme yavaş açın.

Serileştirme de karmaşık Entity Framework kullanan proxy sınıfları tarafından açıklandığı şekilde [Gelişmiş senaryoları Öğreticisi](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

Serileştirme sorunları önlemek için bir yoludur veri aktarımı nesneleri (Dto) yerine varlık nesneleri serileştirmek için gösterildiği [Entity Framework ile Web API kullanarak](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) öğretici.

Dto'lar kullanmazsanız, yavaş yükleme devre dışı bırakabilir ve proxy sorunlarından kaçının [proxy oluşturma devre dışı bırakma](https://msdn.microsoft.com/data/jj592886.aspx).

Diğer bir kısmının işte [Gecikmeli yüklemeyi devre dışı yolları](https://msdn.microsoft.com/data/jj574232):

- Belirli bir gezinti özelliklerini atlamak `virtual` özelliği bildirdiğinizde anahtar sözcüğü.
- Tüm gezinti özelliklerini ayarlama `LazyLoadingEnabled` için `false`, aşağıdaki kod, bağlam sınıfın oluşturucusunda yerleştirin:

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page"></a>Kursları sayfası oluşturma

`Course` Varlığı içeren bir gezinme özelliği içeren `Department` kursu atandığı departmanı varlık. Atanan bölüm adını kursları listesinde görüntülenecek almanız gereken `Name` özelliğinden `Department` olan varlık `Course.Department` gezinme özelliği.

Adlı bir denetleyici oluşturma `CourseController` (CoursesController değil) için `Course` varlık türü, aynı seçenekleri kullanarak **MVC 5 denetleyici Entity Framework kullanarak görünümler ile** içindahaönceyaptığınıziskelekurucu`Student` denetleyicisi:

| Ayar | Değer |
| ------- | ----- |
| Model sınıfı | Seçin **kurs (ContosoUniversity.Models)**. |
| Veri bağlamı sınıfı | Seçin **SchoolContext (ContosoUniversity.DAL)**. |
| Denetleyici adı | Girin *CourseController*. Yeniden değil *CoursesController* ile bir *s*. Seçili olduğunda **kurs (ContosoUniversity.Models)**, **Denetleyici adı** değeri otomatik olarak doldurulur. Değeri değiştirmek zorunda. |

Diğer varsayılan değerleri bırakın ve denetleyici ekleyin.

Açık *Controllers\CourseController.cs* bakın `Index` yöntemi:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Otomatik yapı iskelesi istekli yükleme için belirtilmiş `Department` gezinme özelliğini kullanarak `Include` yöntemi.

Açık *Views\Course\Index.cshtml* ve şablon kodunu aşağıdaki kodla değiştirin. Değişiklikleri vurgulanmıştır:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

İskele kurulan kodu için aşağıdaki değişiklikler yaptınız:

- Değiştirilen başlığından **dizin** için **kursları**.
- Eklenen bir **numarası** gösteren sütun `CourseID` özellik değeri. Varsayılan olarak, birincil anahtarlar normalde son kullanıcılara anlamsız olduğu için iskele kurulmuş değildir. Ancak, bu durumda birincil anahtarı anlamlı ve göstermek istiyorsunuz.
- Taşınan **departmanı** sağ tarafında sütun ve başlığını değiştirildi. İskele kurucu düzgün görüntülemek seçtiğiniz `Name` özelliğinden `Department` varlık ancak burada bir sütunun başlığına olmalıdır kurs sayfasında **departmanı** yerine **adı**.

Bölüm sütun için iskele kurulan kodu görüntülendiğine dikkat edin `Name` özelliği `Department` yüklendiği varlık `Department` gezinti özelliği:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Çalıştırırsanız (seçin **kursları** Contoso University giriş sayfasında sekmesi) bölüm adları listesi.

## <a name="create-an-instructors-page"></a>Eğitmenler sayfası oluşturma

Bu bölümde bir denetleyici oluşturacak ve görüntüleme `Instructor` Eğitmenler sayfasını görüntülemek için varlık. Bu sayfada okur ve ilgili verileri aşağıdaki yollarla görüntüler:

- Eğitmenler listesini ilgili verileri görüntüleyen `OfficeAssignment` varlık. `Instructor` Ve `OfficeAssignment` bir sıfır-veya-bir ilişkide varlıklardır. İstekli yükleme için kullanacağınız `OfficeAssignment` varlıklar. Birincil tablo alınan tüm satırlarının için ilgili verileri gerektiğinde daha önce açıklandığı gibi istekli yükleme genellikle daha verimli olur. Bu durumda, tüm görüntülenen Eğitmenler office atamalarını görüntülemek istiyorsunuz.
- Kullanıcı, ilgili eğitmenin seçtiğinde `Course` varlıkları görüntülenir. `Instructor` Ve `Course` bir çoktan çoğa ilişki içinde varlıklardır. İstekli yükleme için kullanacağınız `Course` varlıkları ve bunların ilgili `Department` varlıklar. Bu durumda, yalnızca seçili eğitmen için kursları gerektiğinden Gecikmeli yükleme daha verimli olabilir. Ancak, bu örnek, kendilerini Gezinti özelliklerdir varlıkların içinde gezinme özelliklerinin istekli yükleme kullanmayı gösterir.
- Verileri kullanıcı bir kurs seçtiğinde ilgili `Enrollments` varlık kümesi görüntülenir. `Course` Ve `Enrollment` bir bire çok ilişkiye varlıklardır. Açık yükleme için ekleyeceksiniz `Enrollment` varlıkları ve bunların ilgili `Student` varlıklar. (Açık yükleme yavaş yükleniyor etkindir, ancak bu açık yükleme bunun nasıl yapılacağını göstermektedir gerekli değildir.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Eğitmen dizini görünümünü bir görünüm modeli oluşturun

Eğitmenler sayfada üç farklı tabloda gösterilir. Bu nedenle, her bir tablo için verileri tutan üç özellik içeren bir görünüm modeli oluşturmayı öğreneceksiniz.

İçinde *Viewmodel'lar* klasör oluşturma *InstructorIndexData.cs* ve varolan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Eğitmen denetleyici ve görünümler oluşturma

Oluşturma bir `InstructorController` (InstructorsController değil) denetleyicisi EF okuma/yazma eylemi:

| Ayar | Değer |
| ------- | ----- |
| Model sınıfı | Seçin **Eğitmen (ContosoUniversity.Models)**. |
| Veri bağlamı sınıfı | Seçin **SchoolContext (ContosoUniversity.DAL)**. |
| Denetleyici adı | Girin *InstructorController*. Yeniden değil *InstructorsController* ile bir *s*. Seçili olduğunda **kurs (ContosoUniversity.Models)**, **Denetleyici adı** değeri otomatik olarak doldurulur. Değeri değiştirmek zorunda. |

Diğer varsayılan değerleri bırakın ve denetleyici ekleyin.

Açık *Controllers\InstructorController.cs* ve ekleme bir `using` bildirimi `ViewModels` ad alanı:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

İskele kurulan kodu `Index` yöntemi yalnızca istekli yükleme belirtir `OfficeAssignment` gezinti özelliği:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Değiştirin `Index` yöntemini ek yüklemek için aşağıdaki kod ile ilgili verileri ve görünüm modelinde yerleştirin:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Yöntemi, isteğe bağlı rota verileri kabul eder (`id`) ve bir sorgu dizesi parametresi (`courseID`), seçilen Eğitmen ve seçili kurs kimliği değerini sağlayın ve tüm gerekli veri görünümüne geçirir. Parametreleri tarafından sağlanan **seçin** sayfadaki köprüler.

Görünüm modeli örneği oluşturmayı ve bunu Eğitmenler listesini koyarak kod başlar. İstekli yükleme için kod belirtir `Instructor.OfficeAssignment` ve `Instructor.Courses` gezinme özelliği.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

İkinci `Include` yöntemi kursları yükler ve yüklenen her kursunun istekli yükleme yaptığı `Course.Department` gezinme özelliği.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Daha önce belirtildiği gibi istekli yükleme gerekmez ancak performansını artırmak için gerçekleştirilir. Görünümü her zaman gerektirdiğinden `OfficeAssignment` varlık, bu, aynı sorgu içinde getirilecek daha verimli. `Course` istekli yükleme yalnızca sayfası daha sık olmadan daha seçili bir kurs ile gösterilirse, yavaş yükleniyor daha iyi, bu nedenle bir eğitmen web sayfasında seçildiğinde varlıkları gereklidir.

Eğitmen kimliği seçildiyse, seçili Eğitmen görünüm modeli eğitmenlerini listesi alınır. Görünüm modeli `Courses` ile ardından özelliğinin yüklü `Course` Bu eğitmen varlıklardan `Courses` gezinme özelliği.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`Where` Yöntem bir koleksiyon döndürür, ancak yalnızca tek bir yöntem sonuçlanan ölçütler bu durumda geçirilen `Instructor` döndürülen varlık. `Single` Yöntemi, tek bir koleksiyon dönüştürür `Instructor` erişim sağlayan, söz konusu varlığın varlık `Courses` özelliği.

Kullandığınız [tek](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) koleksiyon bildiğiniz durumlarda bir koleksiyon üzerinde yöntemi, yalnızca bir öğe olacaktır. `Single` Yöntem kendisine geçirilen koleksiyonu boş ise veya birden fazla öğe varsa bir özel durum oluşturur. Alternatif [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), varsayılan değeri döndürür (`null` bu durumda) koleksiyonu boş ise. Ancak, bu durumda, hala sonuçlanır bir özel durum (bulunmaya çalışılırken gelen bir `Courses` özelliği bir `null` başvuru), ve özel durum iletisi sorunun nedenini daha net bir şekilde gösterir. Çağırdığınızda `Single` yöntemi, aynı zamanda geçirebilir `Where` koşul çağırmak yerine `Where` yöntemi ayrı olarak:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Onun yerine:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Ardından, bir kurs seçildiyse, görünüm modeli eğitim listesinden seçilen kursu alınır. Ardından Görünüm modelinin `Enrollments` özelliğinin yüklü olan `Enrollment` o kursun varlıklardan `Enrollments` gezinme özelliği.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Eğitmen Index görünümünü değiştirme

İçinde *Views\Instructor\Index.cshtml*, şablonu kodu aşağıdaki kodla değiştirin. Değişiklikleri vurgulanmıştır:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Varolan kodu aşağıdaki değişiklikler yaptınız:

- Model sınıfı için değiştirilen `InstructorIndexData`.
- Sayfa başlığı değiştirilen **dizin** için **Eğitmenler**.
- Eklenen bir **Office** görüntüleyen sütun `item.OfficeAssignment.Location` yalnızca `item.OfficeAssignment` null değil. (Bu bir sıfır-veya-bir ilişkisi olduğundan, olmayabilir ilgili `OfficeAssignment` varlık.)

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Dinamik olarak ekleyeceğiniz eklenen kodu `class="success"` için `tr` seçili Eğitmen öğesidir. Bu ayarlar kullanarak seçili satır için arka plan rengi bir [önyükleme](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) sınıfı.

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Yeni eklenen `ActionLink` etiketli **seçin** hemen diğer bağlantıları önce her bir satırdaki neden olan gönderilmesini seçili Eğitmen kimliği `Index` yöntemi.

Uygulamayı çalıştırmak ve seçmek **Eğitmenler** sekmesi. Sayfa görüntüler `Location` ilgili özelliği `OfficeAssignment` varlıkları ve boş bir tablo hücresi olduğunda ilgili Hayır `OfficeAssignment` varlık.

İçinde *Views\Instructor\Index.cshtml* Kapanıştan sonra dosyayı `table` öğesi (sonunda dosyası), aşağıdaki kodu ekleyin. Bu kod bir eğitmen seçildiğinde bir eğitmen için ilgili kurslar listesini görüntüler.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Bu kodu okuyan `Courses` özelliği kursları listesini görüntülemek için Görünüm modeli. Ayrıca sağlar bir `Select` seçili kursa kimliği gönderen köprü `Index` eylem yöntemi.

Sayfayı çalıştırın ve bir eğitmen seçin. Seçili eğitmen için atanan kursları görüntüleyen bir kılavuz göreceksiniz ve her kurs için atanan bölüm adını görürsünüz.

Yeni eklediğiniz kod bloğundan sonra aşağıdaki kodu ekleyin. Bu kurs seçildiğinde bu kurs kayıtlı öğrencilere listesini görüntüler.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Bu kodu okuyan `Enrollments` Öğrenciler listesini görüntülemek için Görünüm modeli özelliği kursun kayıtlı.

Sayfayı çalıştırın ve bir eğitmen seçin. Ardından bir kurs kayıtlı Öğrenci ve kendi derece listesini görmek için seçin.

### <a name="adding-explicit-loading"></a>Açık yükleme ekleme

Açık *InstructorController.cs* ve nasıl göz `Index` yöntemi kayıtları için seçilen bir kurs listesini alır:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Eğitmenler listesi alınırken istekli yükleme için belirtilen `Courses` gezinti özelliği ve `Department` her kurs sonunda verilen özelliğidir. Yerleştirdiğiniz sonra `Courses` koleksiyon görünüm modeli ve eriştiğiniz artık `Enrollments` o koleksiyon içerisindeki bir varlıktan gezinme özelliği. İstekli yükleme için belirtmediğiniz çünkü `Course.Enrollments` bu özellik verileri gezinti özelliği görünen sayfasında sonucunda yavaş yükleniyor.

Diğer herhangi bir şekilde kodunda değişiklik yapmadan yavaş yükleme devre dışı bırakılırsa `Enrollments` özelliği null bağımsız olarak kaç kayıtları kursu gerçekten sahip olacaktır. Yüklemek için bu durumda, `Enrollments` özelliği, haritamın istekli yükleme ya da açık yükleme belirtmek. İstekli yükleme yapmak nasıl gördünüz. Açık yükleme örneği görmek için değiştirin `Index` yöntemini aşağıdaki kodla açıkça yükleyen `Enrollments` özelliği. Değiştirilen kodu vurgulanır.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

Seçili alma sonra `Course` varlık, yeni kodu açıkça o kursun yükler `Enrollments` gezinti özelliği:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

Açıkça her yüklendikten sonra `Enrollment` varlık ilgili `Student` varlık:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Kullandığınız bildirimi `Collection` bir koleksiyon özelliği yüklemek için gereken yöntemini ancak yalnızca bir varlık tutan bir özelliği için kullandığınız `Reference` yöntemi.

Eğitmen dizin sayfası artık çalıştırın ve verileri nasıl alınır değiştirdik ancak sayfasında, görüntülenen içinde herhangi bir fark görürsünüz.

## <a name="additional-resources"></a>Ek kaynaklar

Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET veri erişimi - önerilen kaynaklar](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * İlgili veri yükleme işleminin nasıl yapılacağını öğrendiniz
> * Kursları sayfa oluşturuldu
> * Eğitmenler sayfa oluşturuldu

İlgili verileri güncelleştirme hakkında bilgi edinmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [İlgili verileri güncelleştirme](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)