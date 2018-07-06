---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: (5 / 10) ASP.NET MVC uygulamasındaki Entity Framework ile ilgili verileri okuma | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 750c49572e99c6ab8c84d65e4c18f8da9b5d304c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835252"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Entity Framework (5 / 10) ASP.NET MVC uygulamasındaki verilerle ilgili okuma
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Projeyi yükle](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Öğretici serisinin en baştan başlayın veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.
> 
> > [!NOTE] 
> > 
> > Çözümleyemiyor, bir sorunla karşılaştıysanız [tamamlanmış bölüm indirme](building-the-ef5-mvc4-chapter-downloads.md) ve sorununuzu yeniden oluşturmaya çalışın. Tamamlanan kodu kodunuza karşılaştırarak, sorunun çözümünü genellikle bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümleri bulabilirsiniz.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Önceki öğreticide Okul veri modeli tamamlandı. Bu öğreticide okuma ve ilgili verileri görüntüleyen — diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler veri.

Aşağıdaki çizimler ile çalışmak sayfaları göstermektedir.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Yavaş, duymayı ve açık ilgili verileri yükleme

Entity Framework Gezinti özelliklerini bir varlığın ilgili verileri yükleyebilir birkaç yolu vardır:

- *Yavaş Yükleniyor*. Varlığın ilk okunduğunda, ilgili verileri alınan değil. Ancak, bir gezinti özelliği erişmeye ilk kez otomatik olarak bu gezinti özelliği için gerekli olan veriler alınır. Birden çok sorgu veritabanına gönderilen sonuçlanır — varlık biri ilgili varlık için veriler her zaman alınması gerekir. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *İstekli yükleme*. Varlık okunduğunda, onunla birlikte ilgili verileri alınır. Bu, tek bir birleşim sorguda tüm gerekli olan verileri alır. genellikle sonuçlanır. İstekli yükleme kullanarak belirttiğiniz `Include` yöntemi.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Açık yükleme*. Açıkça kod içinde ilgili verileri alma dışında bu yavaş yükleniyor için benzer; bir gezinti özelliğine erişirken otomatik olarak gerçekleşmez. İlgili verileri el ile bir varlık ve arama için nesne durumu Yöneticisi girişi alarak yüklediğiniz `Collection.Load` koleksiyonlar için yöntemi veya `Reference.Load` tutan tek bir varlık özellikleri için yöntemi. (Aşağıdaki örnekte, yönetici gezinti özelliği yüklemek istiyorsanız, değiştirirler `Collection(x => x.Courses)` ile `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Özellik değerlerini hemen almak yoktur çünkü yavaş yükleniyor ve açık yükleme da her ikisi de olarak da bilinir *Ertelenen yükleme*.

Bildiğiniz veritabanına gönderilen tek bir sorgu genellikle daha verimli alınan her varlık için ayrı sorgulara olduğu için genel olarak, ilgili verileri alınan, istekli yükleme en iyi performansı sunar. her varlık için gereklidir. Örneğin, Yukarıdaki örneklerde, her departman on ilgili kurslar olduğunu varsayalım. İstekli yükleme örnek yalnızca bir tek (birleştirme) sorgu ve tek bir gidiş dönüş veritabanına neden olur. Yavaş yükleniyor ve açık yükleme örnekler hem de on sorgular ve on gidiş dönüş veritabanına neden olur. Gecikme süresi yüksek olduğunda veritabanına ek gidiş dönüş özellikle performansı düşürür.

Öte yandan, bazı senaryolarda yavaş yükleniyor daha verimli olur. İstekli yükleme, SQL Server'ın etkili bir şekilde işleyemiyor oluşturulması çok karmaşık birleştirme neden olabilir. Veya bir varlığın yalnızca bir alt kümesini bir dizi varlık Gezinti özellikleri erişmeniz gerekiyorsa, işleme, istekli yükleme ihtiyacınız olandan daha fazla veri alması nedeniyle yavaş yükleniyor daha iyi gerçekleştirebilir. Performans kritik ise, en iyi seçim yapmak için her iki yönde performansını test etmek idealdir.

Genellikle, yalnızca kapalı yüklenirken yavaş etkinleştirdiyseniz, açık yükleme kullanırsınız. Kapalı yüklenirken yavaş kapatmalısınız senaryolardan biri serileştirme sırasında dir. Yavaş yükleniyor ve Serileştirme iyi karıştırmayın ve yavaş olduğunda istenenden önemli ölçüde daha fazla veri sorgulama yukarı sona erdirebilirsiniz dikkatli olmazsanız yükleme etkindir. Seri hale getirme, genellikle bir tür örneği üzerinde her bir özellik erişerek çalışır. Özellik erişimini yavaş yükleniyor tetikler ve bu yavaş yüklenen varlıklara serileştirilir. Seri hale getirme işlemi daha sonra büyük olasılıkla daha yavaş yükleniyor ve Serileştirme neden yavaş yüklenen varlıkların her bir özellik erişir. Bu run-away ata'daki önlemek için devre dışı bir varlık seri hale getirme önce yükleme yavaş açın.

Veritabanı bağlamı sınıfının varsayılan olarak yavaş yükleme gerçekleştirir. Gecikmeli yükleme devre dışı bırakmak için iki yolu vardır:

- Belirli bir gezinti özelliklerini atlamak `virtual` özelliği bildirdiğinizde anahtar sözcüğü.
- Tüm gezinti özelliklerini ayarlama `LazyLoadingEnabled` için `false`. Örneğin, aşağıdaki kod, bağlam sınıfın oluşturucusunda koyabilirsiniz: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Yavaş yükleniyor, performans sorunlarına neden olan kod maskeleyebilirsiniz. Örneğin, eager veya açık yükleme belirtmeyen ancak varlıkları yüksek hacimli işler ve her yinelemede birkaç Gezinti özellikleri kullanan kodu (nedeniyle veritabanı çok sayıda gidiş dönüş) çok verimsiz olabilir. İyi bir şirket içi SQL server'ı kullanarak geliştirme gerçekleştiren bir uygulama, Azure SQL veritabanı'na daha yüksek gecikme süresi ve yavaş yükleme nedeniyle taşındıklarında performans sorunları olabilir. Veritabanı sorguları gerçekçi test yük ile profil oluşturma, yavaş yükleniyor uygun olup olmadığını belirlemenize yardımcı olur. Daha fazla bilgi için [Demystifying Entity Framework stratejileri: ilgili veri yükleme](https://msdn.microsoft.com/magazine/hh205756.aspx) ve [SQL Azure için ağ gecikme süresini azaltmak için Entity Framework kullanarak](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Bu görüntüler bölüm adı kursları dizin sayfası oluşturma

`Course` Varlığı içeren bir gezinme özelliği içeren `Department` kursu atandığı departmanı varlık. Atanan bölüm adını kursları listesinde görüntülenecek almanız gereken `Name` özelliğinden `Department` olan varlık `Course.Department` gezinme özelliği.

Adlı bir denetleyici oluşturma `CourseController` için `Course` varlık türü, daha önce yaptığınız için aynı seçenekleri kullanarak `Student` denetleyicisi, aşağıdaki çizimde gösterildiği gibi (görüntü, DAL ad alanında bağlam sınıfınıza olması dışında değil modelleri ad alanı):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Açık *Controllers\CourseController.cs* bakın `Index` yöntemi:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Otomatik yapı iskelesi istekli yükleme için belirtilmiş `Department` gezinme özelliğini kullanarak `Include` yöntemi.

Açık *Views\Course\Index.cshtml* ve varolan kodu aşağıdaki kodla değiştirin. Değişiklikleri vurgulanmıştır:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

İskele kurulan kodu için aşağıdaki değişiklikler yaptınız:

- Değiştirilen başlığından **dizin** için **kursları**.
- Satır bağlantıları sola taşındı.
- Bir sütun başlığı altında eklenen **numarası** gösteren `CourseID` özellik değeri. (Normalde son kullanıcılara anlamsız olduğu için varsayılan olarak, birincil anahtarlar iskele kurulmuş değildir. Ancak, bu durumda birincil anahtar anlamlı ve göstermek istediğiniz.)
- Son sütun başlığı değiştirilen **DepartmentID** (yabancı anahtar adını `Department` varlık) için **departmanı**.

Son sütun için iskele kurulan kodu görüntülendiğine dikkat edin `Name` özelliği `Department` yüklendiği varlık `Department` gezinti özelliği:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Çalıştırırsanız (seçin **kursları** Contoso University giriş sayfasında sekmesi) bölüm adları listesi.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Kursları ve kayıtları gösteren Eğitmenler dizin sayfası oluşturma

Bu bölümde bir denetleyici oluşturacak ve görüntüleme `Instructor` varlık Eğitmenler dizin sayfasını görüntülemek için:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Bu sayfada okur ve ilgili verileri aşağıdaki yollarla görüntüler:

- Eğitmenler listesini ilgili verileri görüntüleyen `OfficeAssignment` varlık. `Instructor` Ve `OfficeAssignment` bir sıfır-veya-bir ilişkide varlıklardır. İstekli yükleme için kullanacağınız `OfficeAssignment` varlıklar. Birincil tablo alınan tüm satırlarının için ilgili verileri gerektiğinde daha önce açıklandığı gibi istekli yükleme genellikle daha verimli olur. Bu durumda, tüm görüntülenen Eğitmenler office atamalarını görüntülemek istiyorsunuz.
- Kullanıcı, ilgili eğitmenin seçtiğinde `Course` varlıkları görüntülenir. `Instructor` Ve `Course` bir çoktan çoğa ilişki içinde varlıklardır. İstekli yükleme için kullanacağınız `Course` varlıkları ve bunların ilgili `Department` varlıklar. Bu durumda, yalnızca seçili eğitmen için kursları gerektiğinden Gecikmeli yükleme daha verimli olabilir. Ancak, bu örnek, kendilerini Gezinti özelliklerdir varlıkların içinde gezinme özelliklerinin istekli yükleme kullanmayı gösterir.
- Verileri kullanıcı bir kurs seçtiğinde ilgili `Enrollments` varlık kümesi görüntülenir. `Course` Ve `Enrollment` bir bire çok ilişkiye varlıklardır. Açık yükleme için ekleyeceksiniz `Enrollment` varlıkları ve bunların ilgili `Student` varlıklar. (Açık yükleme yavaş yükleniyor etkindir, ancak bu açık yükleme bunun nasıl yapılacağını göstermektedir gerekli değildir.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Eğitmen dizini görünümünü bir görünüm modeli oluşturun

Üç farklı tabloda Eğitmen dizin sayfasını gösterir. Bu nedenle, her bir tablo için verileri tutan üç özellik içeren bir görünüm modeli oluşturmayı öğreneceksiniz.

İçinde *Viewmodel'lar* klasör oluşturma *InstructorIndexData.cs* ve varolan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Seçili satır için bir stil ekleme

Seçilen satırları işaretlemek için farklı arka plan rengi gerekir. Bu kullanıcı Arabirimi için bir stil sağlamak için bölümüne aşağıdaki vurgulanmış kodu ekleyin `/* info and errors */` içinde *Content\Site.css*, aşağıda gösterildiği gibi:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Eğitmen denetleyici ve görünümler oluşturma

Oluşturma bir `InstructorController` aşağıdaki çizimde gösterildiği gibi denetleyicisi:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Açık *Controllers\InstructorController.cs* ve ekleme bir `using` bildirimi `ViewModels` ad alanı:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

İskele kurulan kodu `Index` yöntemi yalnızca istekli yükleme belirtir `OfficeAssignment` gezinti özelliği:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Değiştirin `Index` yöntemini ek yüklemek için aşağıdaki kod ile ilgili verileri ve görünüm modelinde yerleştirin:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Yöntemi, isteğe bağlı rota verileri kabul eder (`id`) ve bir sorgu dizesi parametresi (`courseID`), seçilen Eğitmen ve seçili kurs kimliği değerini sağlayın ve tüm gerekli veri görünümüne geçirir. Parametreleri tarafından sağlanan **seçin** sayfadaki köprüler.

> [!TIP]
> 
> **Rota verileri**
> 
> Rota verilerini, model Bağlayıcısı yönlendirme tablosunda belirtilen URL kesimi bulunan verilerdir. Örneğin, varsayılan yolu belirtir `controller`, `action`, ve `id` segmentleri:
> 
> yollar. MapRoute)  
>  Ad: "Varsayılan"  
>  URL: "{denetleyici} / {eylem} / {id}",  
>  Varsayılan: yeni {denetleyici = "Home", Eylem = "Dizin" kimliği UrlParameter.Optional =}  
> );
> 
> Aşağıdaki URL'de varsayılan rotayı eşler `Instructor` olarak `controller`, `Index` olarak `action` ve 1 olarak `id`; bunlar rota veri değerlerdir.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? courseID 2021 =" sorgu dizesi değeri. Model bağlayıcıya geçirirseniz da çalışır `id` bir sorgu dizesi değeri olarak:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> URL'leri tarafından oluşturulan `ActionLink` Razor görünüm deyimlerinde. Aşağıdaki kodda, `id` parametreyle eşleşen varsayılan yol, bu nedenle `id` için rota verilerini eklenir.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> Aşağıdaki kodda, `courseID` sorgu dizesi olarak eklenir, böylece varsayılan yol, bir parametre ile eşleşmiyor.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


Görünüm modeli örneği oluşturmayı ve bunu Eğitmenler listesini koyarak kod başlar. İstekli yükleme için kod belirtir `Instructor.OfficeAssignment` ve `Instructor.Courses` gezinme özelliği.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

İkinci `Include` yöntemi kursları yükler ve yüklenen her kursunun istekli yükleme yaptığı `Course.Department` gezinme özelliği.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Daha önce belirtildiği gibi istekli yükleme gerekmez ancak performansını artırmak için gerçekleştirilir. Görünümü her zaman gerektirdiğinden `OfficeAssignment` varlık, bu, aynı sorgu içinde getirilecek daha verimli. `Course` istekli yükleme yalnızca sayfası daha sık olmadan daha seçili bir kurs ile gösterilirse, yavaş yükleniyor daha iyi, bu nedenle bir eğitmen web sayfasında seçildiğinde varlıkları gereklidir.

Eğitmen kimliği seçildiyse, seçili Eğitmen görünüm modeli eğitmenlerini listesi alınır. Görünüm modeli `Courses` ile ardından özelliğinin yüklü `Course` Bu eğitmen varlıklardan `Courses` gezinme özelliği.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

`Where` Yöntem bir koleksiyon döndürür, ancak yalnızca tek bir yöntem sonuçlanan ölçütler bu durumda geçirilen `Instructor` döndürülen varlık. `Single` Yöntemi, tek bir koleksiyon dönüştürür `Instructor` erişim sağlayan, söz konusu varlığın varlık `Courses` özelliği.

Kullandığınız [tek](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) koleksiyon bildiğiniz durumlarda bir koleksiyon üzerinde yöntemi, yalnızca bir öğe olacaktır. `Single` Yöntem kendisine geçirilen koleksiyonu boş ise veya birden fazla öğe varsa bir özel durum oluşturur. Alternatif [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), varsayılan değeri döndürür (`null` bu durumda) koleksiyonu boş ise. Ancak, bu durumda, hala sonuçlanır bir özel durum (bulunmaya çalışılırken gelen bir `Courses` özelliği bir `null` başvuru), ve özel durum iletisi sorunun nedenini daha net bir şekilde gösterir. Çağırdığınızda `Single` yöntemi, aynı zamanda geçirebilir `Where` koşul çağırmak yerine `Where` yöntemi ayrı olarak:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Onun yerine:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Ardından, bir kurs seçildiyse, görünüm modeli eğitim listesinden seçilen kursu alınır. Ardından Görünüm modelinin `Enrollments` özelliğinin yüklü olan `Enrollment` o kursun varlıklardan `Enrollments` gezinme özelliği.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Eğitmen Index görünümünü değiştirme

İçinde *Views\Instructor\Index.cshtml*, mevcut kodu şu kodla değiştirin. Değişiklikleri vurgulanmıştır:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Varolan kodu aşağıdaki değişiklikler yaptınız:

- Model sınıfı için değiştirilen `InstructorIndexData`.
- Sayfa başlığı değiştirilen **dizin** için **Eğitmenler**.
- Satır bağlantı sütunları sola taşındı.
- Kaldırılan **FullName** sütun.
- Eklenen bir **Office** görüntüleyen sütun `item.OfficeAssignment.Location` yalnızca `item.OfficeAssignment` null değil. (Bu bir sıfır-veya-bir ilişkisi olduğundan, olmayabilir ilgili `OfficeAssignment` varlık.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Dinamik olarak ekleyeceğiniz eklenen kodu `class="selectedrow"` için `tr` seçili Eğitmen öğesidir. Bu, daha önce oluşturduğunuz bir CSS sınıfını kullanarak seçili satır için bir arka plan rengini ayarlar. ( `valign` Çok satırlı sütunu tabloya eklediğinizde özniteliği aşağıdaki öğreticide yararlı olacaktır.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Yeni eklenen `ActionLink` etiketli **seçin** hemen diğer bağlantıları önce her bir satırdaki neden olan gönderilmesini seçili Eğitmen kimliği `Index` yöntemi.

Uygulamayı çalıştırmak ve seçmek **Eğitmenler** sekmesi. Sayfa görüntüler `Location` ilgili özelliği `OfficeAssignment` varlıkları ve boş bir tablo hücresi olduğunda ilgili Hayır `OfficeAssignment` varlık.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

İçinde *Views\Instructor\Index.cshtml* Kapanıştan sonra dosyayı `table` öğesi (sonunda dosyası), aşağıdaki vurgulanmış kodu ekleyin. Bu, bir eğitmen seçildiğinde bir eğitmen için ilgili kurslar listesini görüntüler.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Bu kodu okuyan `Courses` özelliği kursları listesini görüntülemek için Görünüm modeli. Ayrıca sağlar bir `Select` seçili kursa kimliği gönderen köprü `Index` eylem yöntemi.

> [!NOTE]
> *.Css* dosya tarayıcı tarafından önbelleğe alınır. Uygulamayı çalıştırdığınızda değişiklikleri görmüyorsanız, sabit bir yenileme yapın (tıklatırken CTRL tuşunu basılı tutun **Yenile** düğmesini ya da CTRL + F5 tuşlarına basın).


Sayfayı çalıştırın ve bir eğitmen seçin. Seçili eğitmen için atanan kursları görüntüleyen bir kılavuz göreceksiniz ve her kurs için atanan bölüm adını görürsünüz.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Yeni eklediğiniz kod bloğundan sonra aşağıdaki kodu ekleyin. Bu kurs seçildiğinde bu kurs kayıtlı öğrencilere listesini görüntüler.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Bu kodu okuyan `Enrollments` Öğrenciler listesini görüntülemek için Görünüm modeli özelliği kursun kayıtlı.

Sayfayı çalıştırın ve bir eğitmen seçin. Ardından bir kurs kayıtlı Öğrenci ve kendi derece listesini görmek için seçin.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Açık yükleme ekleme

Açık *InstructorController.cs* ve nasıl göz `Index` yöntemi kayıtları için seçilen bir kurs listesini alır:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Eğitmenler listesi alınırken istekli yükleme için belirtilen `Courses` gezinti özelliği ve `Department` her kurs sonunda verilen özelliğidir. Yerleştirdiğiniz sonra `Courses` koleksiyon görünüm modeli ve eriştiğiniz artık `Enrollments` o koleksiyon içerisindeki bir varlıktan gezinme özelliği. İstekli yükleme için belirtmediğiniz çünkü `Course.Enrollments` bu özellik verileri gezinti özelliği görünen sayfasında sonucunda yavaş yükleniyor.

Diğer herhangi bir şekilde kodunda değişiklik yapmadan yavaş yükleme devre dışı bırakılırsa `Enrollments` özelliği null bağımsız olarak kaç kayıtları kursu gerçekten sahip olacaktır. Yüklemek için bu durumda, `Enrollments` özelliği, haritamın istekli yükleme ya da açık yükleme belirtmek. İstekli yükleme yapmak nasıl gördünüz. Açık yükleme örneği görmek için değiştirin `Index` yöntemini aşağıdaki kodla açıkça yükleyen `Enrollments` özelliği. Değiştirilen kodu vurgulanır.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Seçili alma sonra `Course` varlık, yeni kodu açıkça o kursun yükler `Enrollments` gezinti özelliği:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Açıkça her yüklendikten sonra `Enrollment` varlık ilgili `Student` varlık:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Kullandığınız bildirimi `Collection` bir koleksiyon özelliği yüklemek için gereken yöntemini ancak yalnızca bir varlık tutan bir özelliği için kullandığınız `Reference` yöntemi. Eğitmen dizin sayfası artık çalıştırabilirsiniz ve veriler nasıl alınır değiştirdik ancak sayfasında, görüntülenen içinde herhangi bir fark görürsünüz.

## <a name="summary"></a>Özet

Şimdi, ilgili verileri Gezinti özelliklerini yüklemek için tüm üç yol (lazy, duymayı ve açık) kullandınız. Sonraki öğreticide ilgili verileri güncelleştirme öğreneceksiniz.

Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [İleri](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
