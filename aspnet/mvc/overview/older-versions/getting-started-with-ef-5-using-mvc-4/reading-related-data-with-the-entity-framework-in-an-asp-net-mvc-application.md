---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Bir ASP.NET MVC uygulamasındaki (5 / 10) Entity Framework ile ilgili verileri okuma | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamaları oluşturmak nasıl gösteren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 831f5e0a8b57907ea012c10c1d1f8ff166f5e88b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876689"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Okuma ilgili verileri Entity Framework bir ASP.NET MVC uygulamasındaki (5 / 10)
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Eğitmen serisi baştan başlayabilirsiniz veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.
> 
> > [!NOTE] 
> > 
> > Olamaz gidermek, bir sorunla çalıştırırsanız [tamamlanmış bölüm karşıdan](building-the-ef5-mvc4-chapter-downloads.md) ve sorunu yeniden deneyin. Tamamlanan kodu kodunuzu karşılaştırarak genellikle soruna çözüm bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümler.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Önceki öğreticide Okul veri modeli tamamlandı. Bu öğreticide okuma ve ilgili verileri görüntüleme — diğer bir deyişle, Entity Framework Gezinti özelliklerini yükler veri.

Aşağıdaki çizimler ile karşılaşmayacağınızı sayfalarında gösterilir.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Yavaş, istekli ve açık ilgili veri yükleme

Entity Framework bir varlık Gezinti özellikleri ilgili verileri yükleyebilir birkaç yolu vardır:

- *Yavaş Yükleniyor*. Varlık ilk okunduğunda ilgili verileri alınan değil. Ancak, bir gezinti özelliği erişme girişimi ilk kez, o gezinti özelliği için gerekli olan veriler otomatik olarak alınır. Bu veritabanına gönderilen birden çok sorgu sonuçlanır — bir varlık için ve bir varlık için veri ilgili her zaman alınması gerekir. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *İstekli yükleme*. Varlık okurken ilgili verileri onunla birlikte alınır. Bu, genellikle tüm gerekli olan verileri alan bir tek birleştirme sorgusunda sonuçlanır. Kullanarak istekli yükleme belirtin `Include` yöntemi.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Açık yükleme*. Açıkça kod ilgili verileri almak bu yavaş yüklemeye benzerdir; bir gezinti özelliğine erişirken otomatik olarak gerçekleşmez. İlgili verileri el ile bir varlık ve arama için nesne durumu Yöneticisi girdisi alarak yüklediğiniz `Collection.Load` koleksiyonlar için yöntemi veya `Reference.Load` yöntemi tek bir varlık tutun özellikleri. (Aşağıdaki örnekte, yönetici gezinti özelliği yüklemek istiyorsanız, değiştirirler `Collection(x => x.Courses)` ile `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Özellik değerlerini hemen almak yoktur çünkü yavaş yükleniyor ve açık yükleme da her ikisini de olarak da bilinir *yüklenirken ertelenmiş*.

Biliyorsanız veritabanına gönderilen tek bir sorgu genellikle daha verimlidir alınan her varlık için ayrı sorgulara olduğundan genel olarak, ilgili veri alınan, istekli yüklenirken en iyi performansı sunar her varlık için ihtiyacınız var. Örneğin, Yukarıdaki örneklerde, her bölüm on ilgili kursları olduğunu varsayalım. İstekli yükleme örneği yalnızca bir tek (birleştirme) sorgu ve tek bir gidiş dönüş veritabanına neden olur. Yavaş yükleniyor ve açık yükleme örnekleri hem de on sorgular ve on gidiş dönüş veritabanına neden olur. Gecikmenin yüksek olduğu zaman veritabanına ek gidiş dönüş performansı için özellikle detrimental.

Diğer taraftan, bazı senaryolarda yavaş yükleniyor daha etkilidir. İstekli yükleme, SQL Server verimli bir şekilde işleyemiyor oluşturulması, bir çok karmaşık birleştirme neden olabilir. Veya yalnızca bir alt kümesini varlık kümesi için bir varlığın gezinme özelliklerinin erişmeniz gerekiyorsa işleme istekli yükleme ihtiyacınız daha fazla veri almak çünkü yavaş yükleniyor daha iyi gerçekleştirebilir. Performans önemliyse, performansı en iyi seçim yapmak için her iki yönde test en iyisidir.

Genellikle, yalnızca devre dışı yüklenirken yavaş açtığınız zaman açık yüklenirken kullanırsınız. Kapalı yüklenirken yavaş kapatmalısınız olduğunda bir seri hale getirme sırasında bir senaryodur. Yavaş yükleniyor ve Serileştirme iyi birleştirmez ve yavaş olduğunda istenenden önemli ölçüde daha fazla veri sorgulama yukarı sonlandırabilirsiniz dikkatli yoksa yükleme etkindir. Seri hale getirme, genellikle bir türünün bir örneği üzerinde her bir özellik erişerek çalışır. Özellik erişimi yavaş yükleniyor tetikler ve bu yavaş yüklenen varlıkların seri. Seri hale getirme işlemi daha sonra her bir özellik daha da yavaş yükleniyor ve Serileştirme olası neden yüklenen yavaş varlıkların erişir. Bu run-away zinciri tepki önlemek için devre dışı bir varlık seri önce yükleme yavaş açın.

Veritabanı bağlamı sınıfının yavaş yükleniyor varsayılan olarak gerçekleştirir. Geç yükleme devre dışı bırakmak için iki yolu vardır:

- Belirli Gezinti özellikleri için atlayın `virtual` özelliği bildirirken anahtar sözcüğü.
- Tüm gezinti özelliklerini ayarlama `LazyLoadingEnabled` için `false`. Örneğin, aşağıdaki kodu bağlamı sınıfınız oluşturucuda koyabilirsiniz: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Yavaş yükleniyor performans sorunlarına neden olan kod maskeleyebilirsiniz. Örneğin, açık veya istekli yükleme belirtmiyor ancak varlıklar yüksek hacimli işler ve her yinelemede birkaç Gezinti özellikleri kullanan kodu (nedeniyle, çok sayıda gidiş dönüş veritabanına) çok verimsiz olabilir. İyi bir şirket içi SQL server kullanarak geliştirme gerçekleştiren bir uygulama için Azure SQL veritabanı daha yüksek gecikme süresi ve yavaş yükleniyor nedeniyle taşındıklarında performans sorunları olabilir. Gerçekçi test yük veritabanı sorgularıyla profil yavaş yükleniyor uygun olup olmadığını belirleme yardımcı olur. Daha fazla bilgi için bkz: [Demystifying Entity Framework stratejileri: ilgili veri yükleme](https://msdn.microsoft.com/magazine/hh205756.aspx) ve [SQL Azure ağ gecikmesini azaltmak için Entity Framework kullanarak](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Bu görüntüler bölüm adı kurslar dizin sayfası oluşturma

`Course` Varlık içeren bir gezinme özelliği içeren `Department` indirmelere atandığı bölümünün varlık. Atanan departmanı adını kurslar listesinde görüntülenecek almanız gereken `Name` özelliğinden `Department` olan varlık `Course.Department` gezinti özelliği.

Adlı bir denetleyicisi oluşturmak `CourseController` için `Course` varlık türü, daha önce yaptığınız için aynı seçenekleri kullanarak `Student` aşağıdaki çizimde gösterildiği gibi denetleyici (, bağlam sınıfınız DAL ad alanında görüntüdür dışında değil modelleri ad):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Açık *Controllers\CourseController.cs* ve bakmak `Index` yöntemi:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Otomatik yapı iskelesi istekli yükleme için belirtilen sahip `Department` kullanarak gezinti özelliği `Include` yöntemi.

Açık *Views\Course\Index.cshtml* ve var olan kodu aşağıdaki kodla değiştirin. Değişiklikleri vurgulanmıştır:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

İskele kurulmuş kod aşağıdaki değişiklikleri yaptınız:

- Başlığından değiştirilen **dizin** için **kurslar**.
- Satır bağlantılar sola taşındı.
- Sütun başlığı altında eklenen **numarası** gösterir `CourseID` özellik değeri. (Normalde son kullanıcılara anlamsız çünkü bunlar varsayılan olarak, birincil anahtarlar iskele kurulmuş değil. Ancak, bu durumda birincil anahtar anlamlı ve bunu göstermek istediğiniz.)
- Son sütun başlığı değiştirilen **DepartmentID** (yabancı anahtar adını `Department` varlık) için **departmanı**.

Son sütunu için kurulmuş kodu görüntülendiğine dikkat edin `Name` özelliği `Department` yüklenen varlık `Department` gezinti özelliği:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Sayfayı çalıştırın (seçin **kurslar** Contoso University giriş sayfası sekmesinde) bölüm adlarını listesiyle görmek için.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Kurslar ve kayıtları gösteren Eğitmen dizin sayfası oluşturma

Bu bölümde bir denetleyici oluşturacak ve görüntüleme `Instructor` varlık Eğitmen dizin sayfasını görüntülemek için:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Bu sayfayı okur ve ilgili verileri aşağıdaki yollarla görüntüler:

- İlgili verileri Eğitmen listesini görüntüleyen `OfficeAssignment` varlık. `Instructor` Ve `OfficeAssignment` varlıklardır bir tane-sıfır-veya-bir ilişkisi. İstekli yükleme için kullanacağınız `OfficeAssignment` varlıklar. Birincil tablonun tüm alınan satırları için ilgili verileri gerektiğinde daha önce açıklandığı gibi istekli yükleme genellikle daha verimli olur. Bu durumda, tüm görüntülenen Eğitmen office atamaları görüntülemek istediğiniz.
- Kullanıcı ilişkili bir eğitmen seçtiğinde `Course` varlıkları görüntülenir. `Instructor` Ve `Course` varlıklardır bir çok-çok ilişkisi. İstekli yükleme için kullanacağınız `Course` varlıkları ve bunların ilgili `Department` varlıklar. Bu durumda, yalnızca seçili eğitmen için kurslar gerektiğinden yavaş yükleniyor daha etkili olabilir. Ancak, bu örnek istekli yükleme için Gezinti özellikleri kendilerini Gezinti özelliklerdir varlıkları içinde nasıl kullanıldığını gösterir.
- Kullanıcı bir indirmelere seçtiğinde verilerden ilgili `Enrollments` varlık kümesi görüntülenir. `Course` Ve `Enrollment` varlıklardır bir-çok ilişkisi. Açık yükleme için ekleyeceksiniz `Enrollment` varlıkları ve bunların ilgili `Student` varlıklar. (Açık yükleme yavaş yükleniyor etkindir, ancak bu açık yükleme yapmak nasıl gösterilir çünkü gerekli değildir.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Bir görünüm modeli Eğitmen dizini görünümünü oluşturun

Eğitmen dizin sayfası üç farklı tabloları gösterir. Bu nedenle, her biri tablolar için verileri tutan üç özellik içeren bir görünüm modeli oluşturacaksınız.

İçinde *ViewModels* klasörü oluşturmak *InstructorIndexData.cs* ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Seçilen satırlar için bir stil ekleme

Seçili satırları işaretlemek için farklı arka plan rengi gerekir. Bu kullanıcı Arabirimi için bir stil sağlamak için aşağıdaki vurgulanmış kodu bölümüne ekleyin `/* info and errors */` içinde *Content\Site.css*, aşağıda gösterildiği gibi:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Eğitmen denetleyicisi ve görünümler oluşturma

Oluşturma bir `InstructorController` aşağıdaki çizimde gösterildiği gibi denetleyicisi:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Açık *Controllers\InstructorController.cs* ve ekleme bir `using` bildirimi `ViewModels` ad alanı:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

İskele kurulmuş kodu `Index` yöntemi belirtir. yalnızca istekli yükleme `OfficeAssignment` gezinti özelliği:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Değiştir `Index` yöntemi ek yüklemek için aşağıdaki kod ile ilgili verilerin ve görünüm modelinde yerleştirin:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Yöntem isteğe bağlı rota veri kabul eder (`id`) ve bir sorgu dizesi parametresi (`courseID`), seçilen Eğitmen ve seçili indirmelere kimliği değerlerini sağlayın ve tüm gerekli veri görünümüne geçirir. Parametreleri tarafından sağlanan **seçin** sayfasında bağlar.

> [!TIP]
> 
> **Rota verileri**
> 
> Rota verilerini, model bağlayıcı yönlendirme tablosunda belirtilen URL kesimi bulunan verilerdir. Örneğin, varsayılan yolu belirtir `controller`, `action`, ve `id` kesimleri:
> 
> routes.MapRoute(  
>  Ad: "Varsayılan"  
>  URL: "{controller} / {action} / {id}",  
>  Varsayılan: yeni {denetleyicisi "Home", Eylem = "Dizin" = kimliği UrlParameter.Optional =}  
> );
> 
> Aşağıdaki URL'de varsayılan rotayı eşler `Instructor` olarak `controller`, `Index` olarak `action` ve 1 olarak `id`; bunlar rota veri değerlerdir.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? courseID 2021 =" sorgu dizesi değeridir. Geçirdiğiniz model bağlayıcı ayrıca çalışmayacak `id` bir sorgu dizesi değeri olarak:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> URL'leri tarafından oluşturulan `ActionLink` Razor görünüm deyimlerinde. Aşağıdaki kodda, `id` parametreyle eşleşen varsayılan yol, bu nedenle `id` rota verilerini eklenir.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> Aşağıdaki kodda, `courseID` sorgu dizesi olarak eklenmiş şekilde varsayılan yol parametresinde eşleşmiyor.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


Kod görünüm modeli örneği oluşturmayı ve bunu Eğitmen listesi koyma başlar. İstekli yükleme için kod belirtir `Instructor.OfficeAssignment` ve `Instructor.Courses` gezinti özelliği.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

İkinci `Include` yöntemi kurslar yükler ve yüklenen her indirmelere için istekli yükleme mevcut `Course.Department` gezinti özelliği.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Daha önce belirtildiği gibi istekli yüklenmesi gerekli değildir ancak performansı artırmak için yapılır. Görünümü her zaman gerektirdiğinden `OfficeAssignment` varlık, onu aynı sorguda fetch daha verimli. `Course` web sayfasında bir eğitmen seçildiğinde istekli yükleme yalnızca sayfa daha sık olmadan daha seçili indirmelere ile görüntüleniyorsa, yavaş yüklemekten daha iyi şekilde varlıklar gereklidir.

Eğitmen kimliği seçildiyse, seçili Eğitmen görünüm modeli Eğitmen listesi alınır. Görünüm modelinin `Courses` özelliği ile yüklenen sonra `Course` Bu eğitmen varlıklardan `Courses` gezinti özelliği.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

`Where` Yöntem koleksiyonu döndürür, ancak bu yöntem sonucu yalnızca tek bir ölçüt bu durumda geçirilen `Instructor` döndürülen varlık. `Single` Yöntemi tek bir koleksiyon dönüştürür `Instructor` bu varlığın erişmenizi varlık `Courses` özelliği.

Kullandığınız [tek](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) koleksiyonu bildiğiniz durumlarda bir koleksiyon yöntemi yalnızca bir öğe olacaktır. `Single` Yöntem kendisine geçirilen koleksiyonu boş ise veya birden çok öğe varsa bir özel durum oluşturur. Bir alternatif [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), varsayılan değeri döndürür (`null` bu durumda) koleksiyonu boş ise. Ancak, bu durumda hala sonuçlanacak bir özel durum (bulunmaya çalışılırken gelen bir `Courses` özelliği bir `null` başvuru), ve özel durum iletisi daha az açıkça sorunun nedenini gösterir. Çağırdığınızda `Single` yöntemi, siz de geçirebilir `Where` çağırmak yerine koşulu `Where` yöntemi ayrı olarak:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Onun yerine:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Ardından, bir indirmelere seçildiyse, görünüm modeli kurslar listesinden seçili indirmelere alınır. Ardından Görünüm modelinin `Enrollments` özelliği ile yüklenir `Enrollment` bu indirmelere 's varlıklardan `Enrollments` gezinti özelliği.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Eğitmen dizini görünümünü değiştirme

İçinde *Views\Instructor\Index.cshtml*, var olan kodu aşağıdaki kodla değiştirin. Değişiklikleri vurgulanmıştır:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Var olan kodu aşağıdaki değişiklikleri yaptınız:

- Model sınıfı değiştirilmiş `InstructorIndexData`.
- Sayfa başlığının değiştirilen **dizin** için **Eğitmen**.
- Satır bağlantı sütunları sola taşındı.
- Kaldırılan **FullName** sütun.
- Eklenen bir **Office** görüntüleyen sütun `item.OfficeAssignment.Location` yalnızca `item.OfficeAssignment` null değil. (Bu bir tane-sıfır-veya-bir ilişkisi olduğundan, olmayabilir ilgili `OfficeAssignment` varlık.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Dinamik olarak ekleyecek eklenen kod `class="selectedrow"` için `tr` seçili Eğitmen öğesidir. Bu, daha önce oluşturduğunuz CSS sınıfı kullanarak seçili olan satır için bir arka plan rengini belirler. ( `valign` Tabloya çok satır sütun eklediğinizde özniteliği aşağıdaki öğreticide yararlı olacaktır.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Yeni eklenen `ActionLink` etiketli **seçin** hemen diğer bağlantıları önce her satır, neden olan gönderilmesini seçili Eğitmen kimliği `Index` yöntemi.

Uygulamayı çalıştırın ve seçin **Eğitmen** sekmesi. Sayfa görüntüler `Location` ilgili özelliğinin `OfficeAssignment` varlıkları ve boş bir tablo hücresi olduğunda ilgili Hayır `OfficeAssignment` varlık.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

İçinde *Views\Instructor\Index.cshtml* kapatma sonra dosyayı `table` öğesi (sonunda dosyası), aşağıdaki vurgulanmış kodu ekleyin. Bu bir eğitmen seçildiğinde bir eğitmen ilgili kurslar listesini görüntüler.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Bu kod okuyan `Courses` kurslar listesini görüntülemek için görünümü modelin özelliği. Ayrıca sağlayan bir `Select` seçili indirmelere Kimliğini gönderir köprü `Index` eylem yöntemi.

> [!NOTE]
> *.Css* dosya tarayıcılar tarafından önbelleğe alınır. Uygulamayı çalıştırdığınızda değişiklikleri görmüyorsanız, sabit bir yenileme yapın (CTRL tuşunu basılı tutun **yenileme** düğmesini veya CTRL + F5'e basın).


Sayfayı çalıştırın ve bir eğitmen seçin. Artık seçili Eğitmen atanan kurslar görüntüleyen bir kılavuz görebilir ve her indirmelere için atanan departmanı adını görebilirsiniz.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Yeni eklediğiniz kod bloğu sonra aşağıdaki kodu ekleyin. Bu indirmelere seçildiğinde bu bir indirmelere kaydedilen Öğrenciler listesini görüntüler.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Bu kod okuyan `Enrollments` Öğrenciler listesini görüntülemek için görünümü modelin özelliği indirmelere kayıtlı.

Sayfayı çalıştırın ve bir eğitmen seçin. Ardından bir indirmelere kayıtlı Öğrenciler ve bunların dereceleri listesini görmek için seçin.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Açık yükleme ekleme

Açık *InstructorController.cs* ve nasıl göz `Index` yöntemi kayıtları için seçilen indirmelere listesini alır:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Eğitmen listesi alınırken istekli yükleme için belirtilen `Courses` gezinti özelliği ve `Department` her indirmelere özelliği. Yerleştirdiğiniz sonra `Courses` koleksiyonu görünüm modeli ve eriştiğiniz artık `Enrollments` koleksiyondaki bir varlıktan gezinti özelliği. İstekli yükleme için belirtmediğiniz çünkü `Course.Enrollments` gezinti özelliği, bu özellik verileri görünmesini yavaş yükleniyor sonucunda sayfasındaki.

Diğer herhangi bir yolla kod değiştirmeden geç yükleme devre dışı bırakılırsa `Enrollments` özelliği indirmelere aslında vardı kaç kayıtları bağımsız olarak boş olacaktır. Yüklemek için bu durumda, `Enrollments` özelliği zorunda istekli yükleme ya da açık yükleme belirtin. Zaten istekli yükleme yapmak öğrendiniz. Açık yükleyen bir örnek görmek için değiştirmek `Index` açıkça yükler aşağıdaki kod ile yöntemi `Enrollments` özelliği. Değiştirilen kod vurgulanır.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Seçili alma sonra `Course` varlık, yeni kod açıkça yükler, indirmelere 's `Enrollments` gezinti özelliği:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Her açıkça yüklerken sonra `Enrollment` ilgili varlık `Student` varlık:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Kullandığınız bildirimi `Collection` yöntemi bir koleksiyon özelliği yüklemek için ancak yalnızca bir varlık tutan bir özellik için kullandığınız `Reference` yöntemi. Eğitmen dizin sayfası şimdi çalıştırmak ve verileri nasıl alınıp değiştirdiniz ancak sayfasında görüntülenenleri içinde herhangi bir fark görürsünüz.

## <a name="summary"></a>Özet

Şimdi, ilgili veri gezinme özelliklerini yüklemek için tüm üç yol (yavaş, istekli ve açık) kullandığınız. Sonraki öğreticide ilgili verileri güncelleştirmek öğreneceksiniz.

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [sonraki](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
