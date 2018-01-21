---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: "Bir ASP.NET MVC uygulaması (10 4) daha karmaşık bir veri modeli oluşturma | Microsoft Docs"
author: tdykstra
description: "Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamaları oluşturmak nasıl gösteren..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f81f3d80-3674-4d8e-a9b1-87feed1a93c9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5283da2786d41c0ae06607185dd416aeb7d2b62a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application-4-of-10"></a>Bir ASP.NET MVC uygulaması (10 4) daha karmaşık bir veri modeli oluşturma
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Eğitmen serisi baştan başlayabilirsiniz veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.
> 
> > [!NOTE] 
> > 
> > Olamaz gidermek, bir sorunla çalıştırırsanız [tamamlanmış bölüm karşıdan](building-the-ef5-mvc4-chapter-downloads.md) ve sorunu yeniden deneyin. Tamamlanan kodu kodunuzu karşılaştırarak genellikle soruna çözüm bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümler.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Önceki eğitimlerine üç varlıklarının oluşan basit veri modeli ile çalışmıştır. Bu öğreticide daha fazla varlıkları ve ilişkileri ekleyeceksiniz ve biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirterek veri modelini özelleştirmek. Veri modeli özelleştirmek için iki yol görürsünüz: sınıflar ve veritabanı bağlamı sınıfına kod ekleme öznitelikleri ekleyerek.

İşlemi tamamladığınızda, sınıflar aşağıdaki çizimde gösterilen tamamlanmış veri modeli oluşturan:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Veri modeli öznitelikleri kullanarak özelleştirme

Bu bölümde biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirtin öznitelikleri kullanarak veri modelini özelleştirmek nasıl görürsünüz. Aşağıdaki bölümlerde çeşitli tam oluşturduktan sonra `School` ekleyerek veri modeli öznitelikleri sınıfları, zaten modelinde kalan varlık türleri için oluşturulan ve oluşturma yeni sınıflar.

### <a name="the-datatype-attribute"></a>Veri türü özniteliği

Tüm bu alan için ilgilendiğiniz olmasına rağmen tarih Öğrenci kayıt tarihler için tüm web sayfalarının şu anda süreyi tarihi ile birlikte gösterir. Veri ek açıklaması öznitelikleri kullanarak bir Yapabileceğiniz kod görüntüleme biçimi verileri görüntüler her görünümünde düzeltecektir Değiştir. Bir öznitelik ekleyeceksiniz, yapmak nasıl bir örnek görmek için `EnrollmentDate` özelliğinde `Student` sınıfı.

İçinde *Models\Student.cs*, ekleme bir `using` bildirimi `System.ComponentModel.DataAnnotations` ad alanı ekleyin `DataType` ve `DisplayFormat` özniteliklerini `EnrollmentDate` özelliği, aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,13-14)]

[DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) özniteliği veritabanı geçerli bir tür daha fazla belirli bir veri türünü belirtmek için kullanılır. Bu durumda yalnızca tarihi, tarih ve saat değil izlemek istiyoruz. [DataType numaralandırma](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) birçok veri türleri gibi sağlar *tarih, saat, PhoneNumber, para birimi, EmailAddress* ve daha fazlası. `DataType` Özniteliği de otomatik olarak türüne özgü özellikleri sağlamak uygulama etkinleştir. Örneğin, bir `mailto:` bağlantı için oluşturulabilir [DataType.EmailAddress](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx), ve bir tarih seçici için sağlanan [DataType.Date](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) destekleyen tarayıcılarda [HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) öznitelikleri yayar HTML 5 [veri](http://ejohn.org/blog/html-5-data-attributes/) (belirgin *veri tire*) HTML 5 tarayıcılar anlayabileceği öznitelikleri. [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) öznitelikleri tüm doğrulama sağlamaz.

`DataType.Date`Görüntülenen tarih biçimi belirtmiyor. Varsayılan olarak, sunucu üzerinde temel alan varsayılan biçimler göre veri alanı görüntülenir [CultureInfo](https://msdn.microsoft.com/en-us/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

`DisplayFormat` Özniteliği açıkça tarih biçimini belirtmek için kullanılır:


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


`ApplyFormatInEditMode` Ayar değeri düzenlemek için bir metin kutusu görüntülendiğinde belirtilen biçimlendirmeyi de uygulanması gerektiğini belirtir. (, Bazı alanlar için istemeyebilirsiniz — Örneğin, para birimi değerleri için metin kutusuna para birimi simgesini düzenleme için istemeyebilirsiniz.)

Kullanabileceğiniz [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) kendisi, ancak tarafından özniteliktir genellikle kullanmak iyi bir fikir [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) de özniteliği. `DataType` Özniteliği ileten *semantiği* verilerin olarak ekranda işlemek nasıl değil ve ile elde etmezsiniz aşağıdaki yararları sağlar `DisplayFormat`:

- Tarayıcı HTML5 özellikleri (örneğin. bir Takvim denetimi, yerel ayar uygun para birimi simgesini, e-posta bağlantılar, vb. göstermek) etkinleştirebilirsiniz.
- Varsayılan olarak, tarayıcı göre doğru biçimi kullanarak bir veri oluşturmaz, [yerel ayar](https://msdn.microsoft.com/en-us/library/vstudio/wyzd2bce.aspx).
- [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) özniteliği verileri işlemek için sağ alan şablon seçmek MVC etkinleştirebilir ( [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) tarafından kullanılan kendisini dize şablonu kullanıyorsa). Daha fazla bilgi için Brad Wilson'ın bkz [ASP.NET MVC 2 şablonları](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (MVC 2 için yazılmış olsa, bu makalede hala ASP.NET MVC geçerli sürümü için geçerlidir.)

Kullanırsanız `DataType` özniteliği belirtmek zorunda tarih alanıyla `DisplayFormat` ayrıca alanın doğru Chrome tarayıcılarda işler sağlamak için öznitelik. Daha fazla bilgi için bkz: [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Öğrenci dizin sayfası yeniden çalıştırın ve zamanlar için kayıt tarihleri artık görüntülenir dikkat edin. Aynı kullanan herhangi bir görünüm için true olur `Student` modeli.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

Veri doğrulama kuralları ve öznitelikleri kullanarak iletileri de belirtebilirsiniz. Kullanıcılar için bir ad 50'den fazla karakter girmeyin sağlamak istediğinizi varsayın. Bu sınırlama eklemek için Ekle [StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) özniteliklerini `LastName` ve `FirstMidName` aşağıdaki örnekte gösterildiği gibi özellikleri:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) özniteliği olmaz önlemek bir kullanıcı için bir ad boşluk girerek. Kullanabileceğiniz [yanıtta normal ifade](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) özniteliği girişine kısıtlamalar getirmek için. Örneğin aşağıdaki kod, büyük harf olması için ilk karakter ve alfabetik olarak kalan karakterler gerektirir:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

[MaxLength](https://msdn.microsoft.com/en-us/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) özniteliği, benzer işlevsellik sağlar [StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) özniteliği ancak istemci tarafı sağlamaz doğrulama.

Uygulamayı çalıştırın ve tıklayın **Öğrenciler** sekmesi. Aşağıdaki hatayı alıyorsunuz:

*Veritabanının oluşturulmasından 'SchoolContext' bağlamını destekleyen model değişti. Veritabanını güncelleştirmek için Code First Migrations kullanmayı düşünün ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Veritabanı modeli veritabanı şeması değişikliği gerektirdiği şekilde değişti ve Entity Framework algılandı. Kullanıcı arabirimini kullanarak veritabanına eklenen herhangi bir veri kaybetmeden şemasını güncelleştirmek için geçişler kullanacaksınız. Tarafından oluşturulan veri değiştirdiyseniz `Seed` nedeniyle özgün durumuna geri dön değiştirilecek yöntemi, [örnek](https://msdn.microsoft.com/en-us/library/hh846520(v=vs.103).aspx) , kullanmakta olduğunuz yöntemi `Seed` yöntemi. ([Örnek](https://msdn.microsoft.com/en-us/library/hh846520(v=vs.103).aspx) veritabanı terminolojisi bir "upsert" işlem eşdeğerdir.)

Paket Yöneticisi Konsolu (PMC)'da, aşağıdaki komutları girin:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration MaxLengthOnNames` Komut adlı bir dosya oluşturur  *&lt;zaman damgası&gt;\_MaxLengthOnNames.cs*. Bu dosya geçerli veri modeli eşleşecek şekilde veritabanını güncelleştirmek kodunu içerir. Geçişler dosya adına $a zaman damgası geçişler düzenlemek için Entity Framework tarafından kullanılır. Veritabanı bırakma durumunda birden çok geçiş oluşturduktan sonra veya geçişler kullanarak projeyi dağıtırsanız, tüm geçişler, oluşturuldukları sırada uygulanır.

Çalıştırma **oluşturma** sayfasında ve en fazla 50 karakter uzunluğunda ya da bir ad girin. 50 karakterden uzun olarak istemci tarafı doğrulama hemen bir hata iletisi gösterir.

![istemci tarafı val hata](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>Sütun özniteliği

Sınıfları ve özellikleri veritabanına nasıl eşlendiğini denetlemek için öznitelikler de kullanabilirsiniz. Kullanılan adı olduğunu varsayın `FirstMidName` -ad alanı ikinci adı içeriyor olabilir çünkü alan. Ancak veritabanı sütunun adlandırılması istediğiniz `FirstName`, veritabanı geçici sorguları yazma kullanıcılar bu adı alışmanızı. Bu eşleme yapmak için kullanabileceğiniz `Column` özniteliği.

`Column` Özniteliği belirtir, bu, veritabanı oluşturulduktan sonra sütunu `Student` eşlendiği tablo `FirstMidName` özelliği adlı `FirstName`. Diğer bir deyişle, ne zaman kodunuzu başvurduğu `Student.FirstMidName`, veri öğesinden gelir veya içinde güncelleştirilmesi `FirstName` sütunu `Student` tablo. Sütun adları belirtmezseniz, bunlar özellik adı olarak aynı adı verilir.

Kullanarak bir ekleme deyimi için [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.aspx) ve sütun adı özniteliği için `FirstMidName` vurgulanan aşağıdaki kodda gösterildiği gibi özelliği:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

Eklenmesi [sütun özniteliği](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) veritabanı eşleşmeyecektir şekilde SchoolContext yedekleme modeli değiştirir. Başka bir geçiş oluşturmak için PMC aşağıdaki komutları girin:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

İçinde **Sunucu Gezgini** (**Database Explorer** Web Express kullanıyorsanız), çift *Öğrenci* tablo.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

İlk iki geçiş uygulamadan önce olduğu gibi aşağıdaki görüntüde orijinal sütun adını gösterir. Gelen değiştirme sütun adından `FirstMidName` için `FirstName`, iki ad sütunu dekinden `MAX` 50 karakter uzunluğunda.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Ayrıca veritabanı kullanarak eşleme değişiklik yapabilirsiniz [Fluent API](https://msdn.microsoft.com/en-us/data/jj591617), daha sonra Bu öğreticide gördüğünüz gibi.

> [!NOTE]
> Tüm bu varlık sınıfları oluşturma bitirmeden derlemek çalışırsanız, derleyici hataları alabilirsiniz.


## <a name="create-the-instructor-entity"></a>Eğitmen varlık oluştur

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Oluşturma *Models\Instructor.cs*, şablonu kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Çeşitli özelliklerin aynı olmasına dikkat edin `Student` ve `Instructor` varlıklar. İçinde [uygulama devralma](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) bu serideki sonraki öğretici, yeniden bu artıklık ortadan kaldırmak için devralmayı kullanma.

### <a name="the-required-and-display-attributes"></a>Gerekli ve öznitelikler görüntüleyin.

Özniteliklerinde `LastName` özelliği gerekli bir alan, metin kutusu başlığı "Soyadı" (özellik adı boşluk olmadan "Soyadı" olacaktır) yerine olmalıdır ve değeri 50 karakterden uzun olamaz olduğunu belirtin.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

[StringLength özniteliği](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) veritabanında uzunluk üst sınırını ayarlar ve istemci tarafı ve sunucu tarafı sağlar ASP.NET MVC için doğrulama. En az dize uzunluğu Bu öznitelikte belirtebilirsiniz, ancak en düşük değer veritabanı şemasını temel bir etkisi yoktur. [Gerekli öznitelik](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) DateTime, int, gibi değer türleri için çift, gerekli değildir ve kayan noktalı sayı. Değer türleri kendiliğinden gerektiği şekilde bir null değer atanamaz. Kullanarak kaldırabilirsiniz [gerekli öznitelik](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) ve en az uzunluk parametresi için WITH replace `StringLength` özniteliği:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs?highlight=2)]

Eğitmen sınıfı şu şekilde yazabilirsiniz şekilde tek bir çizgi birden çok öznitelik koyabilirsiniz:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-fullname-calculated-property"></a>FullName özelliği hesaplanan

`FullName`diğer iki özellik birleştirerek oluşturulan bir değer döndürür hesaplanan bir özelliktir. Bu nedenle yalnızca sahip bir `get` erişimci ve Hayır `FullName` sütun veritabanında oluşturulmayacak.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Kurslar ve OfficeAssignment Gezinti özellikleri

`Courses` Ve `OfficeAssignment` özelliklerdir Gezinti özellikleri. Daha önce açıklandığı gibi bunlar genellikle olarak tanımlanır [sanal](https://msdn.microsoft.com/en-us/library/9fkccyh4(v=vs.110).aspx) adlı bir Entity Framework özelliği avantajlarından yararlanabilirsiniz [yavaş Yükleniyor](https://msdn.microsoft.com/en-us/magazine/hh205756.aspx). Ayrıca, bir gezinti özelliği birden çok varlık tutarsanız türünü uygulamalıdır [ICollection&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/92t2ye13.aspx) arabirimi. (Örneğin [IList&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/5y536ey6.aspx) niteleyen ancak [IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/9eekhta0.aspx) çünkü `IEnumerable<T>` uygulamaz [Ekle ](https://msdn.microsoft.com/en-us/library/63ywd54z.aspx).

Bir eğitmen kurslar herhangi bir sayıda öğretmek, bu nedenle `Courses` koleksiyonu olarak tanımlanan `Course` varlıklar. Bizim iş kuralları bir eğitmen yalnızca olabilir en çok bir office, böylece durum `OfficeAssignment` tek bir olarak tanımlanan `OfficeAssignment` varlık (hangi olabilir `null` hiçbir office atanmışsa).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>OfficeAssignment varlık oluştur

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Oluşturma *Models\OfficeAssignment.cs* aşağıdaki kod ile:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Yaptığınız değişiklikleri kaydeder ve tüm kopyanın henüz doğrular projeyi oluşturun ve derleyici yakalayabilir hataları yapıştırın.

### <a name="the-key-attribute"></a>Anahtar özniteliği

Arasında bir-sıfır-veya-bir ilişkisi olduğundan `Instructor` ve `OfficeAssignment` varlıklar. Office atama yalnızca atanan için Eğitmen bağlantılı olarak var ve bu nedenle birincil anahtarı aynı zamanda, yabancı anahtar `Instructor` varlık. Ancak Entity Framework otomatik olarak tanıyamıyor `InstructorID` birincil olarak bu varlığın anahtar adını IU çünkü `ID` veya *classname* `ID` adlandırma kuralı. Bu nedenle, `Key` özniteliği anahtar olarak tanımlamak için kullanılır:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Aynı zamanda `Key` varlık birincil anahtarı yok ancak name özelliği farklı bir şey istiyorsanız özniteliği `classnameID` veya `ID`. Sütunu için bir tanımlayıcı ilişkisi olduğundan varsayılan olmayan veritabanı-üretilmiş EF anahtar değerlendirir.

### <a name="the-foreignkey-attribute"></a>ForeignKey özniteliği

Bir-sıfır-veya-bir ilişkisi veya iki varlık arasında bire bir ilişki olduğunda (arasında böyle `OfficeAssignment` ve `Instructor`), hangi son ilişkinin asıl ve hangi uç bağımlı çıkışı EF çalışamıyor. Bire bir ilişkiler bir başvuru gezinti özelliği başka bir sınıfın her sınıfına sahip. [ForeignKey özniteliği](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) ilişkisi kurmak için bağımlı sınıfa uygulanabilir. Atlarsanız [ForeignKey özniteliği](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), geçiş oluşturmayı denediğinizde aşağıdaki hatayı alıyorsunuz:

'ContosoUniversity.Models.OfficeAssignment' ve 'ContosoUniversity.Models.Instructor' türleri arasındaki ilişkinin asıl ucu belirlenemiyor. Bu ilişkinin asıl ucu, ilişki fluent API'si veya veri ek açıklamaları kullanılarak açıkça yapılandırılmalıdır.

Bu ilişki fluent API'si ile yapılandırma konusunda daha sonra öğreticide göstereceğiz.

### <a name="the-instructor-navigation-property"></a>Eğitmen gezinti özelliği

`Instructor` Varlık null atanabilir sahip `OfficeAssignment` gezinti özelliği (bir eğitmen office atama olmayabilir nedeniyle) ve `OfficeAssignment` varlık sahip atanamayan bir `Instructor` gezinti özelliği (office atama yapılamıyor çünkü bir eğitmen--mevcut `InstructorID` NULL olmayan olabilir). Zaman bir `Instructor` varlık sahip ilgili `OfficeAssignment` varlık, her bir varlık, gezinti özelliğinin başka bir başvuru olacaktır.

Put bir `[Required]` ilgili Eğitmen olmalıdır, ancak (aynı zamanda olan bu tablo anahtarı) InstructorID yabancı anahtar null atanamaz olduğundan, yapmanıza gerek yoktur belirtmek için Eğitmen gezinti özelliği özniteliği.

## <a name="modify-the-course-entity"></a>İndirmelere varlık değiştirme

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

İçinde *Models\Course.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

İndirmelere varlık yabancı anahtar özelliğine `DepartmentID` işaret ettiği ilgili `Department` varlık ve bir `Department` gezinti özelliği. Entity Framework ilgili varlık gezinme özelliğinin olduğunda yabancı anahtar özelliği, veri modeline Ekle gerektirmez. Gerekli olan her yerde EF veritabanında yabancı anahtarları otomatik olarak oluşturur. Ancak veri modelinde yabancı anahtar olan güncelleştirmeler daha basit ve daha verimli hale getirebilir. Örneğin, ne zaman, fetch düzenlemek için bir indirmelere varlık `Department` varlıktır null, yükleme böylece indirmelere varlık güncelleştirdiğinizde varsa ilk getirmek `Department` varlık. Zaman yabancı anahtar özelliği `DepartmentID` bulunan veri modelinde fetch gerekmez `Department` güncelleştirmeden önce varlık.

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated özniteliği

[DatabaseGenerated özniteliği](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) ile [hiçbiri](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) parametresini `CourseID` özelliği, birincil anahtar değerlerini kullanıcı tarafından sağlanan yerine veritabanı tarafından oluşturulan belirtir.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Varsayılan olarak, birincil anahtar değerlerinin veritabanı tarafından oluşturulan Entity Framework varsayar. Çoğu senaryoda istediğiniz olmasıdır. Ancak, `Course` varlıklar, bir departman, başka bir bölüm için bir 2000 serisi için 1000 serisi gibi bir kullanıcı tarafından belirtilen indirmelere numarası kullanın ve benzeri.

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Yabancı anahtar özellikleri ve gezinti özellikleri `Course` varlık yansıtacak aşağıdaki ilişkileri:

- Bu yüzden bir indirmelere bir bölüme atanan bir `DepartmentID` yabancı anahtar ve `Department` yukarıda belirtilen nedenlerden için gezinme özelliği. 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Bir indirmelere içinde kayıtlı Öğrenciler herhangi bir sayıda olabilir böylece `Enrollments` gezinti özelliği olduğundan koleksiyonu: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Bir indirmelere birden çok Eğitmen tarafından öğrettin böylece `Instructors` gezinti özelliği olduğundan koleksiyonu: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="creating-the-department-entity"></a>Departman varlık oluşturma

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Oluşturma *Models\Department.cs* aşağıdaki kod ile:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Sütun özniteliği

Daha önce kullandığınız [sütun özniteliği](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) sütun adı eşlemesi değiştirmek için. Kodunda `Department` varlık, `Column` özniteliği, böylece SQL Server'ı kullanarak sütun tanımlanacak SQL veri türü eşlemesi. değiştirmek için kullanılıyor [para](https://msdn.microsoft.com/en-us/library/ms179882.aspx) veritabanındaki türü:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Sütun eşlemesi Entity Framework özelliği için tanımladığınız CLR türüne göre uygun SQL Server veri türü genellikle seçtiği için genellikle gerekli değil. CLR `decimal` yazın eşlemeleri SQL Server'a `decimal` türü. Ancak bu durumda para birimi miktarları sütun bulunduran olduğunu bildiğiniz ve [para](https://msdn.microsoft.com/en-us/library/ms179882.aspx) veri türü için daha uygun olan.

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Yabancı anahtar ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:

- Bir bölüm olabilir veya bir yönetici olmayabilir ve yönetici her zaman bir eğitmen. Bu nedenle `InstructorID` yabancı anahtar olarak özellik eklenmiştir `Instructor` sonra varlık ve bir soru işareti eklenir `int` özelliği null olarak işaretlemek için ataması yazın. Gezinti özelliği adlı `Administrator` ancak tutan bir `Instructor` varlık: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Bu yüzden bir bölüm birçok kurslar olabilir bir `Courses` gezinti özelliği: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

 > [!NOTE]
 > Kurala göre art arda silme çok-çok ilişkileri ve null yabancı anahtarlar için Entity Framework sağlar. Bu Başlatıcı kodunuzu çalıştığında, bir özel durum neden olacak döngüsel cascade delete kuralları'nda neden olabilir. Örneğin, tanımlamadığınız, `Department.InstructorID` özelliği null olarak size aşağıdaki özel durum iletisi Başlatıcı çalıştığında: "başvuru ilişkisi verilmeyen döngüsel başvuru neden olur." İş kuralları gerekirse `InstructorID` özelliği null olarak zorunda art arda silme ilişkiyi devre dışı bırakmak için aşağıdaki fluent API'sini kullanın: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modifying-the-student-entity"></a>Öğrenci varlık değiştirme

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

İçinde *Models\Student.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=12,15,24-27)]

## <a name="the-enrollment-entity"></a>Kayıt varlık

 İçinde *Models\Enrollment.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs?highlight=16)]

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Yabancı anahtar özellikleri ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:

- Bu yüzden bir kaydolma kaydı için tek bir yol, kullanmamaktır bir `CourseID` yabancı anahtar özellik ve `Course` gezinti özelliği: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]
- Bu yüzden bir kaydı tek bir öğrenci için olan bir `StudentID` yabancı anahtar özellik ve `Student` gezinti özelliği: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

### <a name="many-to-many-relationships"></a>Çok-çok ilişkileri

Arasında çok-çok ilişkisi olduğundan `Student` ve `Course` varlıkları ve `Enrollment` varlık işlevleri çoktan bire çok birleşme tablo olarak *yükü ile* veritabanındaki. Bunun anlamı `Enrollment` tablo birleştirilmiş tablolar için yabancı anahtarları yanı sıra ek verileri içerir (Bu durumda, birincil anahtar ve bir `Grade` özelliği).

Aşağıdaki çizimde, bu tür bir varlık şemada nasıl göründüğünü gösterir. (Bu diyagramda kullanılarak oluşturulan [Entity Framework güç araçları](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); diyagram oluşturma öğreticinin bir parçası değil, yalnızca kullanılıyor burada çizim olarak.)

![Many_relationship için Öğrenci Course_many](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

Bir uç ve bir yıldız işareti 1 her ilişki bulunur (\*) diğer sırasında bir-çok ilişkisi belirten.

Varsa `Enrollment` tablosunu kaydetmedi düzeyde bilgi dahil etmek, yalnızca iki yabancı anahtarlara gerekir `CourseID` ve `StudentID`. Bu durumda, çoktan bire çok birleşme tabloya karşılık gelir *yükü olmadan* (veya bir *saf birleşim tablosundan*) veritabanında ve bir model sınıfı hiç oluşturmak sahip olmayacaktır. `Instructor` Ve `Course` varlıklar, bu tür bir çok-çok ilişkisi vardır ve gördüğünüz gibi yoksa hiçbir varlık sınıfı aralarında:

![Eğitmen Course_many için many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Birleştirme tablosu ancak veritabanında gerekli veritabanı Aşağıdaki diyagramda gösterildiği gibi:

![Eğitmen Course_many için many_relationship_tables](https://asp.net/media/2577802/Windows-Live-Writer_Creating-a.NET-MVC-Application-4-of-10h1_B662_Instructor-Course_many-to-many_relationship_tables_03e042cf-db89-4b4c-985a-e458351ada76.png)

Entity Framework otomatik olarak oluşturur `CourseInstructor` tablo ve okuma ve okuma ve güncelleştirme dolaylı güncelleştirme `Instructor.Courses` ve `Course.Instructors` Gezinti özellikleri.

## <a name="entity-diagram-showing-relationships"></a>Varlık diyagram gösteren ilişkileri

Aşağıdaki çizimde Entity Framework güç araçları için tamamlanan Okul modeli oluşturma diyagramda gösterilmektedir.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Çok-çok ilişkisi satırları yanı sıra (\* için \*) ve bir-çok ilişkisi satırları (1'e \*), arasında bir-sıfır-veya-bir ilişkisi satır (1 için 0.. 1 çokluğa) burada görebilirsiniz `Instructor` ve `OfficeAssignment` varlıklar ve sıfır-veya-bir-çok ilişkisi satır (0.. 1 çokluğa için \*) Eğitmen ve departman varlıklar arasında.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Veritabanı bağlamı kodu ekleyerek veri modeli özelleştirme

Yeni varlıklar sonraki ekleyeceksiniz `SchoolContext` sınıfı ve bazı kullanarak eşlemeyi özelleştirme [fluent API](https://msdn.microsoft.com/en-us/data/jj591617) çağrıları. (Genellikle bir dizi yöntem çağrısı tek bir deyimde birleştirerek stringing tarafından kullanılmakta olduğu için "fluent" API'dir.)

Bu öğreticide yalnızca özniteliklerle yapamayacağı veritabanı eşleme fluent API kullanacaksınız. Ancak, çoğu biçimlendirmeyi, doğrulama ve öznitelikleri kullanarak yapabilirsiniz eşleme kurallarını belirtmek için fluent API kullanabilirsiniz. Gibi bazı öznitelikler `MinimumLength` fluent API'si ile uygulanamaz. Daha önce belirtildiği gibi `MinimumLength` şema değiştirmez yalnızca bir istemci ve sunucu tarafı doğrulama kuralı uygular

Bazı geliştiriciler özel olarak bunlar kendi sınıflar "temiz." tutabilirsiniz böylece fluent API kullanmayı tercih eder İstediğiniz ve yalnızca fluent API kullanılarak yapılabilir birkaç özelleştirmeler varsa öznitelikleri ve fluent API karıştırabilirsiniz, ancak genel olarak önerilen uygulama bu iki yaklaşımlardan birini seçin ve bu tutarlı bir şekilde olabildiğince kullanmaktır.

Yeni varlıklar veri modeli ve öznitelikleri kullanarak bunu siz veritabanı eşleme gerçekleştirmek eklemek için kodu değiştirin *DAL\SchoolContext.cs* aşağıdaki kod ile:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

Yeni deyiminde [OnModelCreating](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) yöntemi çoktan bire çok birleşme tablo yapılandırır:

- Arasında çok-çok ilişkisi için `Instructor` ve `Course` varlıklar, kodu birleştirme tablosu için tablo ve sütun adlarını belirtir. Kod ilk yapılandırabilir çok-çok ilişkisi sizin için bu kodu olmadan, ancak bunu çağırırsanız yok varsayılan adları gibi alırsınız `InstructorInstructorID` için `InstructorID` sütun.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Aşağıdaki kodu nasıl fluent API yerine öznitelikleri arasındaki ilişkiyi belirtmek için kullandığınız örneğidir `Instructor` ve `OfficeAssignment` varlıklar:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

"Fluent API" deyimleri arka planda ne yaptıklarını hakkında daha fazla bilgi için bkz: [Fluent API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) blog postası.

## <a name="seed-the-database-with-test-data"></a>Çekirdek Test verileri veritabanıyla

Kodla *Migrations\Configuration.cs* dosya ile oluşturduğunuz yeni varlıklar için çekirdek veri sağlamak için aşağıdaki kodu.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

İlk öğreticide gördüğünüz gibi çoğu bu kod yalnızca güncelleştirir veya yeni varlık nesnesi oluşturur ve örnek veri özelliklerini test etmek için gerektiği gibi yükler. Ancak, fark nasıl `Course` bir çok-çok ilişkisi olan varlık ile `Instructor` varlık gerçekleştirilir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Oluştururken bir `Course` nesnesini, başlatma `Instructors` gezinti özelliği kodu kullanarak boş bir koleksiyon olarak `Instructors = new List<Instructor>()`. Bu eklemek mümkün kılar `Instructor` bu konuyla ilgili varlıklar `Course` kullanarak `Instructors.Add` yöntemi. Boş bir liste oluşturmadıysanız, çünkü bu ilişkileri eklemek bağlanamayacak `Instructors` özelliği null olur ve sahip olmayacaktır bir `Add` yöntemi. Oluşturucuya listesi başlatma de ekleyebilirsiniz.

## <a name="add-a-migration-and-update-the-database"></a>Bir geçiş ekleyin ve veritabanını güncelleştirme

PMC girin `add-migration` komutu:

`PM> add-Migration Chap4`

Veritabanı bu noktada güncelleştirmeye çalıştığınızda, aşağıdaki hatayı alırsınız:

*ALTER TABLE deyimi yabancı anahtar kısıtlaması ile çakıştı "FK\_dbo. İndirmelere\_dbo. Departman\_DepartmentID ". Veritabanı "ContosoUniversity" Tablo "dbo. çakışma oluştu Departman", sütun 'DepartmentID'.*

Düzen &lt; *zaman damgası&gt;\_Chap4.cs* dosyasını bulun ve aşağıdaki kod değişiklikleri (bir SQL deyimi ekleme ve değiştirme bir `AddColumn` deyimi):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

(Out yorum yapmak veya varolan silme emin olun `AddColumn` satır yeni bir tane eklemek ya da bunu girdiğinizde bir hata iletisi alırsınız `update-database` komutu.)

Bazen geçişler var olan verilerle yürüttüğünüzde saplama veri yabancı anahtar kısıtlamaları karşılamak için veritabanına eklemeniz gerekir ve şimdi yaptığınız işe olmasıdır. Oluşturulan kod atanamayan bir ekler `DepartmentID` yabancı anahtar `Course` tablo. Satır zaten varsa `Course` tablo kod çalıştığında `AddColumn` SQL Server ne null olamaz sütununda yerleştirilecek değer bilmiyor olduğundan işlem başarısız. Bu nedenle yeni bir sütun varsayılan değer vermek üzere kod değiştirdiyseniz ve varsayılan Departman olarak davranacak şekilde "Temp" adlı bir saplama bölüm oluşturduğunuz. Var olan, sonuç olarak, `Course` ne zaman satırları bu kodu çalıştırır, bunlar tüm "Temp" departmanı ile ilişkilendirilir.

Zaman `Seed` yöntemi çalıştığında, satır ekleyecektir `Department` tablo ve ilgili varolan `Course` bu yeni satır `Department` satır. Kullanıcı Arabiriminde herhangi kurslar eklemediyseniz, sonra artık "Temp" departman veya varsayılan değer üzerinde gerekir `Course.DepartmentID` sütun. Birisi kurslar uygulamayı kullanarak eklemiş olasılığı izin vermek üzere de güncelleştirmek istediğiniz `Seed` yöntemi kodu emin olmak için tüm `Course` satır (yalnızca önceki çalıştırılan tarafından eklenen olanları `Seed` yöntemi) sahip Geçerli `DepartmentID` varsayılan kaldırmadan önce değerleri değer sütundan ve "Temp" bölümü silin.

Düzenleme işlemi tamamlandıktan sonra &lt; *zaman damgası&gt;\_Chap4.cs* dosya, girin `update-database` geçiş yürütmek için PMC komutu.

> [!NOTE]
> Veri ve yapma şema değişiklikleri geçirilirken diğer hatalarıyla mümkündür. Olamaz çözmek Geçiş hataları alırsanız, ya da bağlantı dizesinde değiştirebilirsiniz *Web.config* dosya ya da veritabanını silin. Veritabanında yeniden adlandırmak için basit yaklaşımdır *Web.config* dosya. Örneğin, CU için veritabanı adını değiştirin\_test aşağıda gösterildiği gibi:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.xml?highlight=1-2)]
> 
>  Yeni bir veritabanı ile geçirmek için veri yok ve `update-database` komut hatasız tamamlamak çok daha büyük bir olasılıkla. Veritabanını silmek yönergeler için bkz: [Visual Studio 2012'den bir veritabanını bırakmak nasıl](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).


Veritabanında açmak **Sunucu Gezgini** daha önce yaptığınız ve genişletin **tabloları** tabloların tümü oluşturulmuş olduğunu görmek için düğüm. (Yaşamaya devam ediyorsanız **Sunucu Gezgini** önceki süreyi açın, **yenileme** düğmesini.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Bir model sınıfı için oluşturmamışsınızdır `CourseInstructor` tablo. Daha önce açıklandığı gibi bir birleştirme tablosu arasında çok-çok ilişkisi için budur `Instructor` ve `Course` varlıklar.

Sağ `CourseInstructor` tablo ve seçin **Show Table Data** sonucu olarak da veri olduğunu doğrulamak için `Instructor` eklediğiniz varlıklar `Course.Instructors` gezinti özelliği.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="summary"></a>Özet

Artık daha karmaşık veri modeli ve karşılık gelen veritabanı vardır. Aşağıdaki öğreticide ilgili verilere erişmek için farklı yollar hakkında daha fazla bilgi edineceksiniz.

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Önceki](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[sonraki](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
