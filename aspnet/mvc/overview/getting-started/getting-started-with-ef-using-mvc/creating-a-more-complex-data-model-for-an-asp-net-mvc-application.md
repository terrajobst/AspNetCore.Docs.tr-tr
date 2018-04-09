---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: Daha karmaşık bir veri modeli için bir ASP.NET MVC uygulaması oluşturma | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: fd8bf6502b0dd261505a86a2ed86d4c3f42e8755
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application"></a>Daha karmaşık bir veri modeli için bir ASP.NET MVC uygulaması oluşturma
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) veya [PDF indirin](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio 2013 kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Önceki eğitimlerine üç varlıklarının oluşan basit veri modeli ile çalışmıştır. Bu öğreticide daha fazla varlıkları ve ilişkileri ekleyeceksiniz ve biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirterek veri modelini özelleştirmek. Veri modeli özelleştirmek için iki yol görürsünüz: sınıflar ve veritabanı bağlamı sınıfına kod ekleme öznitelikleri ekleyerek.

İşlemi tamamladığınızda, sınıflar aşağıdaki çizimde gösterilen tamamlanmış veri modeli oluşturan:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Veri modeli öznitelikleri kullanarak özelleştirme

Bu bölümde biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirtin öznitelikleri kullanarak veri modelini özelleştirmek nasıl görürsünüz. Aşağıdaki bölümlerde çeşitli tam oluşturduktan sonra `School` ekleyerek veri modeli öznitelikleri sınıfları, zaten modelinde kalan varlık türleri için oluşturulan ve oluşturma yeni sınıflar.

### <a name="the-datatype-attribute"></a>Veri türü özniteliği

Tüm bu alan için ilgilendiğiniz olmasına rağmen tarih Öğrenci kayıt tarihler için tüm web sayfalarının şu anda süreyi tarihi ile birlikte gösterir. Veri ek açıklaması öznitelikleri kullanarak bir Yapabileceğiniz kod görüntüleme biçimi verileri görüntüler her görünümünde düzeltecektir Değiştir. Bir öznitelik ekleyeceksiniz, yapmak nasıl bir örnek görmek için `EnrollmentDate` özelliğinde `Student` sınıfı.

İçinde *Models\Student.cs*, ekleme bir `using` bildirimi `System.ComponentModel.DataAnnotations` ad alanı ekleyin `DataType` ve `DisplayFormat` özniteliklerini `EnrollmentDate` özelliği, aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) özniteliği veritabanı geçerli bir tür daha fazla belirli bir veri türünü belirtmek için kullanılır. Bu durumda yalnızca tarihi, tarih ve saat değil izlemek istiyoruz. [DataType numaralandırma](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) birçok veri türleri gibi sağlar *tarih, saat, PhoneNumber, para birimi, EmailAddress* ve daha fazlası. `DataType` Özniteliği de otomatik olarak türüne özgü özellikleri sağlamak uygulama etkinleştir. Örneğin, bir `mailto:` bağlantı için oluşturulabilir [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), ve bir tarih seçici için sağlanan [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) destekleyen tarayıcılarda [HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) öznitelikleri yayar HTML 5 [veri](http://ejohn.org/blog/html-5-data-attributes/) (belirgin *veri tire*) HTML 5 tarayıcılar anlayabileceği öznitelikleri. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) öznitelikleri tüm doğrulama sağlamaz.

`DataType.Date` Görüntülenen tarih biçimi belirtmiyor. Varsayılan olarak, sunucu üzerinde temel alan varsayılan biçimler göre veri alanı görüntülenir [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

`DisplayFormat` Özniteliği açıkça tarih biçimini belirtmek için kullanılır:


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


`ApplyFormatInEditMode` Ayar değeri düzenlemek için bir metin kutusu görüntülendiğinde belirtilen biçimlendirmeyi de uygulanması gerektiğini belirtir. (, Bazı alanlar için istemeyebilirsiniz — Örneğin, para birimi değerleri için metin kutusuna para birimi simgesini düzenleme için istemeyebilirsiniz.)

Kullanabileceğiniz [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) kendisi, ancak tarafından özniteliktir genellikle kullanmak iyi bir fikir [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) de özniteliği. `DataType` Özniteliği ileten *semantiği* verilerin olarak ekranda işlemek nasıl değil ve ile elde etmezsiniz aşağıdaki yararları sağlar `DisplayFormat`:

- Tarayıcı HTML5 özellikleri etkinleştirebilirsiniz (örneğin, bir Takvim denetimi, yerel ayar uygun para birimi simgesini, e-posta bağlantıları göstermek, bazı istemci-tarafı giriş doğrulama, vs.).
- Varsayılan olarak, tarayıcı göre doğru biçimi kullanarak bir veri oluşturmaz, [yerel ayar](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) özniteliği verileri işlemek için sağ alan şablon seçmek MVC etkinleştirebilir ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) dize şablonu kullanır). Daha fazla bilgi için Brad Wilson'ın bkz [ASP.NET MVC 2 şablonları](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (MVC 2 için yazılmış olsa, bu makalede hala ASP.NET MVC geçerli sürümü için geçerlidir.)

Kullanırsanız `DataType` özniteliği belirtmek zorunda tarih alanıyla `DisplayFormat` ayrıca alanın doğru Chrome tarayıcılarda işler sağlamak için öznitelik. Daha fazla bilgi için bkz: [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Diğer tarih biçimleri mvc'de nasıl ele alınacağını hakkında daha fazla bilgi için Git [MVC 5 giriş: düzenleme yöntemleri ve Görünümü Düzenle inceleniyor](../introduction/examining-the-edit-methods-and-edit-view.md) ve arama için sayfasından &quot;uluslararası hale getirme&quot;.

Öğrenci dizin sayfası yeniden çalıştırın ve zamanlar için kayıt tarihleri artık görüntülenir dikkat edin. Aynı kullanan herhangi bir görünüm için true olur `Student` modeli.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

Veri doğrulama kuralları ve öznitelikleri kullanarak bir doğrulama hata iletisi de belirtebilirsiniz. [StringLength özniteliği](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) veritabanında uzunluk üst sınırını ayarlar ve istemci tarafı ve sunucu tarafı sağlar ASP.NET MVC için doğrulama. En az dize uzunluğu Bu öznitelikte belirtebilirsiniz, ancak en düşük değer veritabanı şemasını temel bir etkisi yoktur.

Kullanıcılar için bir ad 50'den fazla karakter girmeyin sağlamak istediğinizi varsayın. Bu sınırlama eklemek için Ekle [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) özniteliklerini `LastName` ve `FirstMidName` aşağıdaki örnekte gösterildiği gibi özellikleri:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) özniteliği olmaz önlemek bir kullanıcı için bir ad boşluk girerek. Kullanabileceğiniz [yanıtta normal ifade](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) özniteliği girişine kısıtlamalar getirmek için. Örneğin aşağıdaki kod, büyük harf olması için ilk karakter ve alfabetik olarak kalan karakterler gerektirir:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

[MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) özniteliği, benzer işlevsellik sağlar [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) özniteliği ancak istemci tarafı sağlamaz doğrulama.

Uygulamayı çalıştırın ve tıklayın **Öğrenciler** sekmesi. Aşağıdaki hatayı alıyorsunuz:

*Veritabanının oluşturulmasından 'SchoolContext' bağlamını destekleyen model değişti. Veritabanını güncelleştirmek için Code First Migrations kullanmayı düşünün ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Veritabanı modeli veritabanı şeması değişikliği gerektirdiği şekilde değişti ve Entity Framework algılandı. Kullanıcı arabirimini kullanarak veritabanına eklenen herhangi bir veri kaybetmeden şemasını güncelleştirmek için geçişler kullanacaksınız. Tarafından oluşturulan veri değiştirdiyseniz `Seed` nedeniyle özgün durumuna geri dön değiştirilecek yöntemi, [örnek](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) , kullanmakta olduğunuz yöntemi `Seed` yöntemi. ([Örnek](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) veritabanı terminolojisi bir "upsert" işlem eşdeğerdir.)

Paket Yöneticisi Konsolu (PMC)'da, aşağıdaki komutları girin:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration` Komut adlı bir dosya oluşturur  *&lt;zaman damgası&gt;\_MaxLengthOnNames.cs*. Kod bu dosyayı içeren `Up` geçerli veri modeli eşleşecek şekilde veritabanını güncelleştirmek yöntemi. `update-database` Komutu çalıştırılmadan bu kodu.

Geçişler dosya adına $a zaman damgası geçişler düzenlemek için Entity Framework tarafından kullanılır. Birden çok geçiş çalıştırmadan önce oluşturabilirsiniz `update-database` komutu ve ardından tüm geçişler, oluşturuldukları sırada uygulanır.

Çalıştırma **oluşturma** sayfasında ve en fazla 50 karakter uzunluğunda ya da bir ad girin. Tıkladığınızda **oluşturma**, istemci tarafı doğrulama hata iletisi gösterir.

![istemci tarafı val hata](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>Sütun özniteliği

Sınıfları ve özellikleri veritabanına nasıl eşlendiğini denetlemek için öznitelikler de kullanabilirsiniz. Kullanılan adı olduğunu varsayın `FirstMidName` -ad alanı ikinci adı içeriyor olabilir çünkü alan. Ancak veritabanı sütunun adlandırılması istediğiniz `FirstName`, veritabanı geçici sorguları yazma kullanıcılar bu adı alışmanızı. Bu eşleme yapmak için kullanabileceğiniz `Column` özniteliği.

`Column` Özniteliği belirtir, bu, veritabanı oluşturulduktan sonra sütunu `Student` eşlendiği tablo `FirstMidName` özelliği adlı `FirstName`. Diğer bir deyişle, ne zaman kodunuzu başvurduğu `Student.FirstMidName`, veri öğesinden gelir veya içinde güncelleştirilmesi `FirstName` sütunu `Student` tablo. Sütun adları belirtmezseniz, bunlar özellik adı olarak aynı adı verilir.

İçinde *Student.cs* dosya, ekleme bir `using` deyimi için [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) ve sütun adı özniteliğe eklemek `FirstMidName` gösterildiği gibi özelliği Aşağıdaki vurgulanmış kodu:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

Eklenmesi [sütun özniteliği](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) veritabanı eşleşmeyecektir şekilde SchoolContext yedekleme modeli değiştirir. Başka bir geçiş oluşturmak için PMC aşağıdaki komutları girin:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

İçinde **Sunucu Gezgini**, açık *Öğrenci* çift tıklatarak Tablo Tasarımcısı *Öğrenci* tablo.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

İlk iki geçiş uygulamadan önce olduğu gibi aşağıdaki görüntüde orijinal sütun adını gösterir. Gelen değiştirme sütun adından `FirstMidName` için `FirstName`, iki ad sütunu dekinden `MAX` 50 karakter uzunluğunda.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Ayrıca veritabanı kullanarak eşleme değişiklik yapabilirsiniz [Fluent API](https://msdn.microsoft.com/data/jj591617), daha sonra Bu öğreticide gördüğünüz gibi.

> [!NOTE]
> Tüm sınıflar aşağıdaki bölümlerde oluşturma bitirmeden derlemek çalışırsanız, derleyici hataları alabilirsiniz.


## <a name="complete-changes-to-the-student-entity"></a>Değişiklikleri Öğrenci varlığa tamamlayın

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

İçinde *Models\Student.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>Gerekli özniteliği

[Gerekli öznitelik](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) ad özellikler gerekli alanlar yapar. `Required attribute` DateTime, int, gibi değer türleri için çift, gerekli değildir ve kayan noktalı sayı. Değer türleri kendiliğinden gerekli alanları olarak ele şekilde bir null değer atanamaz. Kullanarak kaldırabilirsiniz [gerekli öznitelik](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) ve en az uzunluk parametresi için WITH replace `StringLength` özniteliği:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

### <a name="the-display-attribute"></a>Görüntüleme özniteliği

`Display` Özniteliği, metin kutuları için resim yazısı "Ad", "Son adı", "Tam adı" ve "Kayıt tarihi" özellik adı yerine (Sözcük bölünmesi boşluk olan) her örnek olması gerektiğini belirtir.

### <a name="the-fullname-calculated-property"></a>FullName özelliği hesaplanan

`FullName` diğer iki özellik birleştirerek oluşturulan bir değer döndürür hesaplanan bir özelliktir. Bu nedenle yalnızca sahip bir `get` erişimci ve Hayır `FullName` sütun veritabanında oluşturulmayacak.

## <a name="create-the-instructor-entity"></a>Eğitmen varlık oluştur

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Oluşturma *Models\Instructor.cs*, şablonu kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

Çeşitli özelliklerin aynı olmasına dikkat edin `Student` ve `Instructor` varlıklar. İçinde [uygulama devralma](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) bu serideki sonraki öğretici, artıklık ortadan kaldırmak için bu kodu yeniden düzenlemeniz.

Eğitmen sınıfı şu şekilde yazabilirsiniz şekilde tek bir çizgi birden çok öznitelik koyabilirsiniz:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Kurslar ve OfficeAssignment Gezinti özellikleri

`Courses` Ve `OfficeAssignment` özelliklerdir Gezinti özellikleri. Daha önce açıklandığı gibi bunlar genellikle olarak tanımlanır [sanal](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) adlı bir Entity Framework özelliği avantajlarından yararlanabilirsiniz [yavaş Yükleniyor](https://msdn.microsoft.com/magazine/hh205756.aspx). Ayrıca, bir gezinti özelliği birden çok varlık tutarsanız türünü uygulamalıdır [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx) arabirimi. Örneğin [IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx) niteleyen ancak [IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx) çünkü `IEnumerable<T>` uygulamaz [Ekle ](https://msdn.microsoft.com/library/63ywd54z.aspx).

Bir eğitmen kurslar herhangi bir sayıda öğretmek, bu nedenle `Courses` koleksiyonu olarak tanımlanan `Course` varlıklar.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

Bizim iş kuralları bir eğitmen yalnızca olabilir en çok bir office, böylece durum `OfficeAssignment` tek bir olarak tanımlanan `OfficeAssignment` varlık (hangi olabilir `null` hiçbir office atanmışsa).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>OfficeAssignment varlık oluştur

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Oluşturma *Models\OfficeAssignment.cs* aşağıdaki kod ile:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Yaptığınız değişiklikleri kaydeder ve tüm kopyanın henüz doğrular projeyi oluşturun ve derleyici yakalayabilir hataları yapıştırın.

### <a name="the-key-attribute"></a>Anahtar özniteliği

Arasında bir-sıfır-veya-bir ilişkisi olduğundan `Instructor` ve `OfficeAssignment` varlıklar. Office atama yalnızca atanan için Eğitmen bağlantılı olarak var ve bu nedenle birincil anahtarı aynı zamanda, yabancı anahtar `Instructor` varlık. Ancak Entity Framework otomatik olarak tanıyamıyor `InstructorID` birincil olarak bu varlığın anahtar adını IU çünkü `ID` veya *classname* `ID` adlandırma kuralı. Bu nedenle, `Key` özniteliği anahtar olarak tanımlamak için kullanılır:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Aynı zamanda `Key` varlık birincil anahtarı yok ancak name özelliği farklı bir şey istiyorsanız özniteliği `classnameID` veya `ID`. Sütunu için bir tanımlayıcı ilişkisi olduğundan varsayılan olmayan veritabanı-üretilmiş EF anahtar değerlendirir.

### <a name="the-foreignkey-attribute"></a>ForeignKey özniteliği

Bir-sıfır-veya-bir ilişkisi veya iki varlık arasında bire bir ilişki olduğunda (arasında böyle `OfficeAssignment` ve `Instructor`), hangi son ilişkinin asıl ve hangi uç bağımlı çıkışı EF çalışamıyor. Bire bir ilişkiler bir başvuru gezinti özelliği başka bir sınıfın her sınıfına sahip. [ForeignKey özniteliği](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) ilişkisi kurmak için bağımlı sınıfa uygulanabilir. Atlarsanız [ForeignKey özniteliği](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), geçiş oluşturmayı denediğinizde aşağıdaki hatayı alıyorsunuz:

*'ContosoUniversity.Models.OfficeAssignment' ve 'ContosoUniversity.Models.Instructor' türleri arasındaki ilişkinin asıl ucu belirlenemiyor. Bu ilişkinin asıl ucu, ilişki fluent API'si veya veri ek açıklamaları kullanılarak açıkça yapılandırılmalıdır.*

Öğreticide daha sonra bu ilişki fluent API'si ile yapılandırmak nasıl görürsünüz.

### <a name="the-instructor-navigation-property"></a>Eğitmen gezinti özelliği

`Instructor` Varlık null atanabilir sahip `OfficeAssignment` gezinti özelliği (bir eğitmen office atama olmayabilir nedeniyle) ve `OfficeAssignment` varlık sahip atanamayan bir `Instructor` gezinti özelliği (office atama yapılamıyor çünkü bir eğitmen--mevcut `InstructorID` NULL olmayan olabilir). Zaman bir `Instructor` varlık sahip ilgili `OfficeAssignment` varlık, her bir varlık, gezinti özelliğinin başka bir başvuru olacaktır.

Put bir `[Required]` ilgili Eğitmen olmalıdır, ancak (aynı zamanda olan bu tablo anahtarı) InstructorID yabancı anahtar null atanamaz olduğundan, yapmanıza gerek yoktur belirtmek için Eğitmen gezinti özelliği özniteliği.

## <a name="modify-the-course-entity"></a>İndirmelere varlık değiştirme

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

İçinde *Models\Course.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

İndirmelere varlık yabancı anahtar özelliğine `DepartmentID` işaret ettiği ilgili `Department` varlık ve bir `Department` gezinti özelliği. Entity Framework ilgili varlık gezinme özelliğinin olduğunda yabancı anahtar özelliği, veri modeline Ekle gerektirmez. Gerekli olan her yerde EF veritabanında yabancı anahtarları otomatik olarak oluşturur. Ancak veri modelinde yabancı anahtar olan güncelleştirmeler daha basit ve daha verimli hale getirebilir. Örneğin, ne zaman, fetch düzenlemek için bir indirmelere varlık `Department` varlıktır null, yükleme böylece indirmelere varlık güncelleştirdiğinizde varsa ilk getirmek `Department` varlık. Zaman yabancı anahtar özelliği `DepartmentID` bulunan veri modelinde fetch gerekmez `Department` güncelleştirmeden önce varlık.

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated özniteliği

[DatabaseGenerated özniteliği](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) ile [hiçbiri](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) parametresini `CourseID` özelliği, birincil anahtar değerlerini kullanıcı tarafından sağlanan yerine veritabanı tarafından oluşturulan belirtir.

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

## <a name="create-the-department-entity"></a>Departman varlık oluştur

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

Oluşturma *Models\Department.cs* aşağıdaki kod ile:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Sütun özniteliği

Daha önce kullandığınız [sütun özniteliği](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) sütun adı eşlemesi değiştirmek için. Kodunda `Department` varlık, `Column` özniteliği, böylece SQL Server'ı kullanarak sütun tanımlanacak SQL veri türü eşlemesi. değiştirmek için kullanılıyor [para](https://msdn.microsoft.com/library/ms179882.aspx) veritabanındaki türü:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Sütun eşlemesi Entity Framework özelliği için tanımladığınız CLR türüne göre uygun SQL Server veri türü genellikle seçtiği için genellikle gerekli değil. CLR `decimal` yazın eşlemeleri SQL Server'a `decimal` türü. Ancak bu durumda para birimi miktarları sütun bulunduran olduğunu bildiğiniz ve [para](https://msdn.microsoft.com/library/ms179882.aspx) veri türü için daha uygun olan. CLR veri türleri ve SQL Server veri türleri ile eşleştiğinden nasıl hakkında daha fazla bilgi için bkz: [varlık FrameworkTypes SqlClient](https://msdn.microsoft.com/library/bb896344.aspx).

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Yabancı anahtar ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:

- Bir bölüm olabilir veya bir yönetici olmayabilir ve yönetici her zaman bir eğitmen. Bu nedenle `InstructorID` yabancı anahtar olarak özellik eklenmiştir `Instructor` sonra varlık ve bir soru işareti eklenir `int` özelliği null olarak işaretlemek için ataması yazın. Gezinti özelliği adlı `Administrator` ancak tutan bir `Instructor` varlık: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Bu yüzden bir bölüm birçok kurslar olabilir bir `Courses` gezinti özelliği: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Kurala göre art arda silme çok-çok ilişkileri ve null yabancı anahtarlar için Entity Framework sağlar. Bu, bir geçiş eklemeye çalıştığınızda, bir özel durum neden olacak döngüsel cascade delete kuralları'nda neden olabilir. Örneğin, tanımlamadığınız, `Department.InstructorID` özelliği null olarak, aşağıdaki özel durum iletisi almak: "başvuru ilişkisi verilmeyen döngüsel başvuru neden olur." İş kuralları gerekirse `InstructorID` özelliğini alamayan, art arda silme ilişkiyi devre dışı bırakmak için aşağıdaki fluent API deyimi kullanması gerekir: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modify-the-enrollment-entity"></a>Kayıt varlık değiştirme

![Enrollment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

 İçinde *Models\Enrollment.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Yabancı anahtar özellikleri ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:

- Bu yüzden bir kaydolma kaydı için tek bir yol, kullanmamaktır bir `CourseID` yabancı anahtar özellik ve `Course` gezinti özelliği: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- Bu yüzden bir kaydı tek bir öğrenci için olan bir `StudentID` yabancı anahtar özellik ve `Student` gezinti özelliği: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>Çok-çok ilişkileri

Arasında çok-çok ilişkisi olduğundan `Student` ve `Course` varlıkları ve `Enrollment` varlık işlevleri çoktan bire çok birleşme tablo olarak *yükü ile* veritabanındaki. Bunun anlamı `Enrollment` tablo birleştirilmiş tablolar için yabancı anahtarları yanı sıra ek verileri içerir (Bu durumda, birincil anahtar ve bir `Grade` özelliği).

Aşağıdaki çizimde, bu tür bir varlık şemada nasıl göründüğünü gösterir. (Bu diyagramda kullanılarak oluşturulan [Entity Framework güç araçları](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); diyagram oluşturma öğreticinin bir parçası değil, yalnızca kullanılıyor burada çizim olarak.)

![Many_relationship için Öğrenci Course_many](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Bir uç ve bir yıldız işareti 1 her ilişki bulunur (\*) diğer sırasında bir-çok ilişkisi belirten.

Varsa `Enrollment` tablosunu kaydetmedi düzeyde bilgi dahil etmek, yalnızca iki yabancı anahtarlara gerekir `CourseID` ve `StudentID`. Bu durumda, çoktan bire çok birleşme tabloya karşılık gelir *yükü olmadan* (veya bir *saf birleşim tablosundan*) veritabanında ve bir model sınıfı hiç oluşturmak sahip olmayacaktır. `Instructor` Ve `Course` varlıklar, bu tür bir çok-çok ilişkisi vardır ve gördüğünüz gibi yoksa hiçbir varlık sınıfı aralarında:

![Eğitmen Course_many için many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Birleştirme tablosu ancak veritabanında gerekli veritabanı Aşağıdaki diyagramda gösterildiği gibi:

![Eğitmen Course_many için many_relationship_tables](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Entity Framework otomatik olarak oluşturur `CourseInstructor` tablo ve okuma ve okuma ve güncelleştirme dolaylı güncelleştirme `Instructor.Courses` ve `Course.Instructors` Gezinti özellikleri.

## <a name="entity-diagram-showing-relationships"></a>Varlık diyagram gösteren ilişkileri

Aşağıdaki çizimde Entity Framework güç araçları için tamamlanan Okul modeli oluşturma diyagramda gösterilmektedir.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

Çok-çok ilişkisi satırları yanı sıra (\* için \*) ve bir-çok ilişkisi satırları (1'e \*), arasında bir-sıfır-veya-bir ilişkisi satır (1 için 0.. 1 çokluğa) burada görebilirsiniz `Instructor` ve `OfficeAssignment` varlıklar ve sıfır-veya-bir-çok ilişkisi satır (0.. 1 çokluğa için \*) Eğitmen ve departman varlıklar arasında.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Veritabanı bağlamı kodu ekleyerek veri modeli özelleştirme

Yeni varlıklar sonraki ekleyeceksiniz `SchoolContext` sınıfı ve bazı kullanarak eşlemeyi özelleştirme [fluent API](https://msdn.microsoft.com/data/jj591617) çağrıları. Genellikle bir dizi yöntem çağrısı aşağıdaki örnekteki gibi tek bir deyimde birleştirerek stringing tarafından kullanılmakta olduğu için "fluent" bir API'dir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

Bu öğreticide yalnızca özniteliklerle yapamayacağı veritabanı eşleme fluent API kullanacaksınız. Ancak, çoğu biçimlendirmeyi, doğrulama ve öznitelikleri kullanarak yapabilirsiniz eşleme kurallarını belirtmek için fluent API kullanabilirsiniz. Gibi bazı öznitelikler `MinimumLength` fluent API'si ile uygulanamaz. Daha önce belirtildiği gibi `MinimumLength` şema değiştirmez yalnızca bir istemci ve sunucu tarafı doğrulama kuralı uygular

Bazı geliştiriciler özel olarak bunlar kendi sınıflar "temiz." tutabilirsiniz böylece fluent API kullanmayı tercih eder İstediğiniz ve yalnızca fluent API kullanılarak yapılabilir birkaç özelleştirmeler varsa öznitelikleri ve fluent API karıştırabilirsiniz, ancak genel olarak önerilen uygulama bu iki yaklaşımlardan birini seçin ve bu tutarlı bir şekilde olabildiğince kullanmaktır.

Yeni varlıklar veri modeli ve öznitelikleri kullanarak bunu siz veritabanı eşleme gerçekleştirmek eklemek için kodu değiştirin *DAL\SchoolContext.cs* aşağıdaki kod ile:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

Yeni deyiminde [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) yöntemi çoktan bire çok birleşme tablo yapılandırır:

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

PMC girin `add-migration` komutu (yapmayın `update-database` henüz komutu):

`add-Migration ComplexDataModel`

Çalıştırmak çalıştıysanız `update-database` bu noktada komutu (yapma, henüz), aşağıdaki hatayı alıyorsunuz:

*ALTER TABLE deyimi yabancı anahtar kısıtlaması ile çakıştı "FK\_dbo. İndirmelere\_dbo. Departman\_DepartmentID ". Veritabanı "ContosoUniversity" Tablo "dbo. çakışma oluştu Departman", sütun 'DepartmentID'.*

Bazen geçişler var olan verilerle yürüttüğünüzde saplama veri yabancı anahtar kısıtlamaları karşılamak için veritabanına eklemeniz gerekir ve şimdi yapmak zorunda olmasıdır. Oluşturulan kod ComplexDataModel `Up` yöntemi ekler atanamayan bir `DepartmentID` yabancı anahtar `Course` tablo. Satır zaten olduğundan `Course` tablo kod çalıştığında `AddColumn` SQL Server ne null olamaz sütununda yerleştirilecek değer bilmiyor olduğundan işlem başarısız olur. Bu nedenle yeni bir sütun varsayılan bir değer verin ve varsayılan Departman olarak davranacak şekilde "Temp" adlı bir saplama bölüm oluşturmak için kodu değiştirmeniz gerekir. Sonuç olarak, varolan `Course` satır tüm ilgili sonra "Temp" departman `Up` yöntemi çalışır. Doğru departmanlara ilişkilendirebilirsiniz `Seed` yöntemi.

Düzen &lt; *zaman damgası&gt;\_ComplexDataModel.cs* , yorum DepartmentID sütun indirmelere tabloya ekler kod satırı çıkışı dosya ve (açıklamalı aşağıdaki vurgulanmış kodu ekleyin Satır de vurgulanır):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

Zaman `Seed` yöntemi çalıştığında, satır ekleyecektir `Department` tablo ve ilgili varolan `Course` bu yeni satır `Department` satır. Kullanıcı Arabiriminde herhangi kurslar eklemediyseniz, sonra artık "Temp" departman veya varsayılan değer üzerinde gerekir `Course.DepartmentID` sütun. Birisi kurslar uygulamayı kullanarak eklemiş olasılığı izin vermek üzere de güncelleştirmek istediğiniz `Seed` yöntemi kodu emin olmak için tüm `Course` satır (yalnızca önceki çalıştırılan tarafından eklenen olanları `Seed` yöntemi) sahip Geçerli `DepartmentID` varsayılan kaldırmadan önce değerleri değer sütundan ve "Temp" bölümü silin.

Düzenleme işlemi tamamlandıktan sonra &lt; *zaman damgası&gt;\_ComplexDataModel.cs* dosya, girin `update-database` geçiş yürütmek için PMC komutu.

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> Veri ve yapma şema değişiklikleri geçirilirken diğer hatalarıyla mümkündür. Çözümlenemiyor Geçiş hataları alırsanız, bağlantı dizesindeki veritabanı adını değiştirin veya veritabanını silin. Veritabanında yeniden adlandırmak için basit yaklaşımdır *Web.config* dosya. Aşağıdaki örnek adı için CU değiştirilmiş gösterir\_Test:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
> 
> Yeni bir veritabanı ile geçirmek için veri yok ve `update-database` komut hatasız tamamlamak çok daha büyük bir olasılıkla. Veritabanını silmek yönergeler için bkz: [Visual Studio 2012'den bir veritabanını bırakmak nasıl](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).
> 
> Başarısız olursa, başka bir şey deneyebilirsiniz PMC aşağıdaki komutu girerek veritabanını yeniden başlatın.:
> 
> `update-database -TargetMigration:0`


Veritabanında açmak **Sunucu Gezgini** daha önce yaptığınız ve genişletin **tabloları** tabloların tümü oluşturulmuş olduğunu görmek için düğüm. (Yaşamaya devam ediyorsanız **Sunucu Gezgini** önceki süreyi açın, **yenileme** düğmesini.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

Bir model sınıfı için oluşturmamışsınızdır `CourseInstructor` tablo. Daha önce açıklandığı gibi bir birleştirme tablosu arasında çok-çok ilişkisi için budur `Instructor` ve `Course` varlıklar.

Sağ `CourseInstructor` tablo ve seçin **Show Table Data** sonucu olarak da veri olduğunu doğrulamak için `Instructor` eklediğiniz varlıklar `Course.Instructors` gezinti özelliği.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="summary"></a>Özet

Artık daha karmaşık veri modeli ve karşılık gelen veritabanı vardır. Aşağıdaki öğreticide ilgili verilere erişmek için farklı yollar hakkında daha fazla bilgi edineceksiniz.

Lütfen geri bildirim, Bu öğretici beğendiğinizi nasıl ve ne biz artabileceğini bırakın. En yeni konular da isteğinde bulunabilirsiniz [Göster bana nasıl kodu ile](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access - kaynakları önerilen](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [sonraki](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
