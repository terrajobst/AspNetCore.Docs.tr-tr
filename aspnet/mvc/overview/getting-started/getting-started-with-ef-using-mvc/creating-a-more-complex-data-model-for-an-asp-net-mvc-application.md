---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: 'Öğretici: ASP.NET MVC uygulaması için daha karmaşık bir veri modeli oluşturma'
author: tdykstra
description: Bu öğreticide, daha fazla varlıklar ve ilişkiler ekleyeceksiniz ve biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirterek veri modeli özelleştireceksiniz.
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5d05d456315c019c545a91b527bb05510126c06b
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836638"
---
# <a name="tutorial-create-a-more-complex-data-model-for-an-aspnet-mvc-app"></a>Öğretici: ASP.NET MVC uygulaması için daha karmaşık bir veri modeli oluşturma

Önceki öğreticilerde, üç varlıklarının oluşturulmuş bir basit veri modeli ile çalışmıştır. Bu öğreticide daha fazla varlıklar ve ilişkiler ekleyin ve biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirterek veri modelini özelleştirin. Bu makalede veri modelini özelleştirmek için iki yolunu gösterir: Varlık sınıfları ve veritabanı bağlamı sınıfının için kod ekleyerek öznitelikleri ekleyerek.

İşlemi tamamladığınızda, varlık sınıfları, aşağıdaki çizimde gösterilen tamamlanmış veri modeli oluşturan:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Veri modelini özelleştirin
> * Öğrenci varlık güncelleştir
> * Eğitmen varlık oluşturma
> * OfficeAssignment varlık oluşturma
> * Kurs varlığı değiştirme
> * Departman varlık oluşturma
> * Kayıt varlığı değiştirme
> * Veritabanı bağlamı için kod ekleyin
> * Çekirdek veritabanı test verileri ile
> * Bir geçiş ekleyin
> * Veritabanını Güncelleştir

## <a name="prerequisites"></a>Önkoşullar

* [Kod ilk geçişleri ve dağıtımı](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-the-data-model"></a>Veri modelini özelleştirin

Bu bölümde, veri modeli, biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirten öznitelikleri kullanarak özelleştirmek nasıl görürsünüz. Aşağıdaki bölümler birkaç tam oluşturacağınız sonra `School` ekleyerek veri modeli öznitelikleri için sınıflar, zaten modelinde kalan varlık türleri için yeni sınıflar oluşturma ve oluşturulur.

### <a name="the-datatype-attribute"></a>Veri türü özniteliği

Tüm bu alan için önem verdiğiniz rağmen tarih Öğrenci kayıt tarihler için tüm web sayfalarının şu anda zaman tarihi ile birlikte görüntüler. Veri ek açıklama öznitelikleri kullanarak bir Yapabileceğiniz kodu görüntüleme biçimini gösteren veriler her görünümde düzeltir Değiştir. Bir özniteliğe ekleyeceksiniz, yapmak nasıl bir örnek görmek için `EnrollmentDate` özelliğinde `Student` sınıfı.

İçinde *Models\Student.cs*, ekleme bir `using` bildirimi `System.ComponentModel.DataAnnotations` ad alanı ve ekleme `DataType` ve `DisplayFormat` özniteliklerini `EnrollmentDate` özelliği, aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) özniteliği veritabanı iç türünden daha belirli bir veri türü belirtmek için kullanılır. Bu durumda yalnızca tarih değil tarih ve saati izlemek istiyoruz. [Veri türü sabit listesi](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) gibi çok sayıda veri türleri için sağlar *tarih, saat, telefon numarası, para birimi, EmailAddress* ve daha fazlası. `DataType` Özniteliğini de otomatik olarak türe özgü özellikler sağlamak için uygulamayı etkinleştirin. Örneğin, bir `mailto:` bağlantısını için oluşturulamaz [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), ve bir tarih seçici için sağlanan [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) destekleyen tarayıcılarda [HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) öznitelikleri yayan HTML 5 [veri](http://ejohn.org/blog/html-5-data-attributes/) (telaffuz *veri dash*) HTML 5 tarayıcılar anlamak için öznitelikler. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) öznitelikleri, tüm doğrulama sağlamaz.

`DataType.Date` Görüntülenen tarih biçimi belirtmiyor. Varsayılan olarak, sunucu üzerinde temel alan varsayılan biçimler göre veri alanı görüntülenir [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

`DisplayFormat` Açıkça tarih biçimini belirtmek için özniteliği kullanılır:


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


`ApplyFormatInEditMode` Ayar değeri düzenlemek için metin kutusunda görüntülendiğinde belirtilen biçimlendirme da uygulanması gerektiğini belirtir. (, Bazı alanlar için istemeyebilirsiniz — Örneğin, para birimi değerleri için metin kutusuna para birimi simgesi düzenleme için istemeyebilirsiniz.)

Kullanabileceğiniz [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) özniteliği kendisi, ancak bu, genellikle kullanmak iyi bir fikir [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) ayrıca özniteliği. `DataType` Özniteliği iletmez *semantiği* verilerin farklı bir ekranda işlenecek nasıl karşılık ve ile elde etmezsiniz aşağıdaki avantajları sağlar `DisplayFormat`:

- Tarayıcı HTML5 özellikleri etkinleştirebilirsiniz (örneğin, bir Takvim denetimi, yerel ayara uygun bir para birimi simgesi, e-posta bağlantıları göstermek, bazı istemci-tarafı giriş doğrulama, vs.).
- Varsayılan olarak, tarayıcıya göre doğru biçimini kullanarak veri işlenir, [yerel](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) özniteliğini verileri işlemek için sağ alanı şablon seçmek MVC etkinleştirin ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) dize şablonu kullanır). Daha fazla bilgi için Brad Wilson'ın bkz [ASP.NET MVC 2 şablonları](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (MVC 2'için yazılmış olsa, bu makalede hala geçerli sürümü, ASP.NET MVC için geçerlidir.)

Kullanırsanız `DataType` özniteliği belirtmek zorunda bir tarih alanı ile `DisplayFormat` da alanın Chrome tarayıcılarında doğru şekilde işlediğinden emin olmak için özniteliği. Daha fazla bilgi için [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Diğer tarih biçimleri mvc'de nasıl ele alınacağını hakkında daha fazla bilgi için Git [MVC 5 giriş: Düzenleme metotlarını ve düzenleme görünümünü İnceleme](../introduction/examining-the-edit-methods-and-edit-view.md) ve arama sayfasında &quot;uluslararası duruma getirme&quot;.

Öğrenci dizin sayfasını yeniden çalıştırın ve süreleri artık kayıt tarihlerini gösterilir dikkat edin. Aynı kullanan herhangi bir görünümü için doğru olacaktır `Student` modeli.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

Veri doğrulama kuralları ve öznitelikleri kullanarak bir doğrulama hata iletisi de belirtebilirsiniz. [StringLength özniteliği](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) veritabanında en fazla uzunluk ayarlar ve istemci tarafı ve sunucu tarafı sağlayan ASP.NET MVC için doğrulama. En az dize uzunluğu Bu öznitelikte belirtebilirsiniz, ancak en düşük değer, veritabanı şema üzerinde hiçbir etkisi yoktur.

Kullanıcılar için bir ad 50 karakterden uzun girmeyin sağlamak istediğinizi varsayın. Bu kısıtlama eklemek için Ekle [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) özniteliklerini `LastName` ve `FirstMidName` aşağıdaki örnekte gösterildiği gibi özellikleri:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) özniteliği olmaz önlemek bir kullanıcı için bir ad boşluk girmesini. Kullanabileceğiniz [yanıtta normal ifade](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) girişi kısıtlamaları uygulamak için özniteliği. Örneğin aşağıdaki kod, ilk karakterin büyük harf olacak ve alfabetik olarak kalan karakterler gerektirir:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

[MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) özniteliği benzer bir işlevsellik sağlar [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) özniteliği ancak istemci tarafı sağlamaz doğrulama.

Uygulamayı çalıştırmak ve tıklayın **Öğrenciler** sekmesi. Aşağıdaki hatayı alıyorsunuz:

*Veritabanı oluşturulduktan sonra 'SchoolContext' bağlam yedekleme modeli değişti. Veritabanını güncellemek için Code First Migrations'ı kullanmayı deneyin ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Entity Framework, algılanan ve veritabanı modeli, veritabanı şema değişikliği gerektiren bir şekilde değişti. Geçişler, kullanıcı arabirimini kullanarak veritabanına eklenen herhangi bir veri kaybetmeden şemasını güncelleştirmek için kullanacaksınız. Tarafından oluşturulan verileri değiştirdiyseniz `Seed` nedeniyle özgün durumuna geri dön değiştirmesi gereken yöntemini [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) kullanmakta olduğunuz yöntemi `Seed` yöntemi. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) veritabanı terminolojisi bir "upsert" işleme eşdeğerdir.)

Paket Yöneticisi Konsolu (PMC'de), aşağıdaki komutları girin:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration` Komut adlı bir dosya oluşturur  *&lt;zaman damgası&gt;\_MaxLengthOnNames.cs*. Kod içinde bu dosyayı içeren `Up` yönteminin veritabanı geçerli bir veri modeli eşleşecek şekilde güncelleştirir. `update-database` Komutun çalıştığını, kod.

Başına geçişleri dosya adına zaman damgası, Entity Framework tarafından geçişlerin sıralamak için kullanılır. Birden çok geçiş çalıştırmadan önce oluşturduğunuz `update-database` komutunu ve ardından tüm geçişlerde, oluşturuldukları sırada uygulanır.

Çalıştırma **Oluştur** sayfasında ve 50 karakterden uzun ya da bir ad girin. Tıkladığınızda **Oluştur**, istemci tarafı doğrulama hata iletisi gösterir: *Soyadı alan 50 maksimum uzunlukta bir dize olmalıdır.*

### <a name="the-column-attribute"></a>Sütun özniteliği

Sınıfları ve özellikleri veritabanına nasıl eşlendiğini denetlemek için öznitelikleri de kullanabilirsiniz. Adı kullanmışsınız varsayalım `FirstMidName` -ad alanı da ikinci adı içeriyor olabileceğinden alan. Ancak, veritabanı sütununun adı için istediğiniz `FirstName`veritabanına karşı özel sorgular yazmak kullanıcılar bu adı alışmanızı sağlamak için. Bu eşleme yapmak için kullanabileceğiniz `Column` özniteliği.

`Column` Özniteliği belirtir, bu, veritabanı oluşturulduktan sonra sütunun `Student` eşleyen tablo `FirstMidName` özelliği adlandırılacak `FirstName`. Diğer bir deyişle, ne zaman kodunuzu başvurduğu `Student.FirstMidName`, veri kaynağından gelir veya içinde güncelleştirilmesi `FirstName` sütununun `Student` tablo. Sütun adları belirtmezseniz, kendisine aynı adı özellik adı olarak verilir.

İçinde *Student.cs* ekleyin bir `using` bildirimi [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) ve sütun ad özniteliğini eklemek `FirstMidName` gösterildiği özelliği Aşağıdaki vurgulanmış kodu:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

Ek [sütun özniteliği](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) veritabanı eşleşmeyecektir şekilde SchoolContext yedekleme modeli değiştirir. PMC'de başka bir geçiş oluşturmak için aşağıdaki komutları girin:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

İçinde **Sunucu Gezgini**açın *Öğrenci* çift tıklayarak Tablo Tasarımcısı *Öğrenci* tablo.

İlk iki geçişlerin uygulanmadan önceki haliyle orijinal sütun adı aşağıdaki resimde gösterilmektedir. Gelen değiştirme sütun adına ek olarak `FirstMidName` için `FirstName`, iki ad sütunu değişmiş `MAX` 50 karakter uzunluğunda.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Ayrıca veritabanı kullanarak eşleme değişiklik yapabilirsiniz [Fluent API'si](https://msdn.microsoft.com/data/jj591617), bu öğreticinin ilerleyen bölümlerinde anlatıldığı gibi.

> [!NOTE]
> Aşağıdaki bölümlerde tüm varlık sınıfları oluşturma tamamlanmadan önce derlemek denerseniz derleyici hataları alabilirsiniz.

## <a name="update-student-entity"></a>Öğrenci varlık güncelleştir

İçinde *Models\Student.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>Gerekli öznitelik

[Gerekli öznitelik](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) adı özellikleri gerekli alanları yapar. `Required attribute` Gibi DateTime, int, değer türleri için double, gerekli değildir ve kayan noktalı sayı. Değer türleri, gerekli alanları olarak doğal olarak kabul edilir şekilde bir null değer atanamaz. Kullanarak kaldırabilirsiniz [gerekli öznitelik](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) için en az uzunluk parametresi ile değiştirin `StringLength` özniteliği:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

### <a name="the-display-attribute"></a>Görüntüleme özniteliği

`Display` Özniteliği, metin kutuları için açıklama yazısını "Ad", "Soyadı", "Tam adı" ve "Kayıt tarihi" özellik adı yerine (Sözcük bölünmesi boşluk olan) her örnek olması gerektiğini belirtir.

### <a name="the-fullname-calculated-property"></a>FullName hesaplanan özellik

`FullName` diğer iki özellikleri birleştirilerek oluşturulur bir değer döndüren bir hesaplanan özelliktir. Bu nedenle yalnızca sahip bir `get` erişimcisi ve Hayır `FullName` sütunu veritabanında oluşturulur.

## <a name="create-instructor-entity"></a>Eğitmen varlık oluşturma

Oluşturma *Models\Instructor.cs*, şablon kodunu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

Çeşitli özelliklerin aynı olduğuna dikkat edin `Student` ve `Instructor` varlıklar. İçinde [uygulama devralma](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) bu serideki sonraki öğretici, artıklığı ortadan kaldırmak için bu kodu yeniden düzenleyin.

Eğitmen sınıfı şu şekilde yazabilirsiniz için birden çok öznitelik bir satıra koyabilirsiniz:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>OfficeAssignment Gezinti özellikleri ve kursları

`Courses` Ve `OfficeAssignment` Gezinti özellikleri özelliklerdir. Daha önce açıklandığı şekilde, bunlar genellikle olarak tanımlanan [sanal](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) bunlar adlı bir Entity Framework özelliğin avantajlarından yararlanabilmeniz [yavaş Yükleniyor](https://msdn.microsoft.com/magazine/hh205756.aspx). Ayrıca, bir gezinti özelliği birden çok varlık tutarsanız türünü uygulamalıdır [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx) arabirimi. Örneğin [IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx) niteleyen ancak [IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx) çünkü `IEnumerable<T>` uygulamayan [Ekle ](https://msdn.microsoft.com/library/63ywd54z.aspx).

Bir eğitmen kursları herhangi bir sayıda öğretebiliriz, bu nedenle `Courses` koleksiyonu olarak tanımlanan `Course` varlıklar.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

Bir eğitmen en fazla bir office, bu nedenle dbmigrationsconfiguration iş kurallarımızın durum `OfficeAssignment` tek bir tanımlanır `OfficeAssignment` varlık (gösterebilir `null` hiçbir office atanmışsa).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-officeassignment-entity"></a>OfficeAssignment varlık oluşturma

Oluşturma *Models\OfficeAssignment.cs* aşağıdaki kod ile:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Yaptığınız değişiklikleri kaydeder ve bir kopya oluşturmadıysanız doğrular projeyi oluşturmak ve derleyici yakalayabilir hataları yapıştırın.

### <a name="the-key-attribute"></a>Anahtar özniteliği

Arasında bir sıfır-veya-bire bir ilişki yoktur `Instructor` ve `OfficeAssignment` varlıklar. Bir ofis ataması yalnızca atandığı Eğitmeni ile ilgili olarak var ve bu nedenle birincil anahtarı aynı zamanda, yabancı anahtar `Instructor` varlık. Ancak Entity Framework otomatik olarak tanıyamaz `InstructorID` birincil olarak bu varlığın anahtar adını izleyin değil çünkü `ID` veya *classname* `ID` adlandırma kuralı. Bu nedenle, `Key` özniteliği anahtar olarak tanımlamak için kullanılır:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Ayrıca `Key` varlığın birincil anahtarı yok ancak name özelliği farklı bir şey istiyorsanız özniteliği `classnameID` veya `ID`. Sütunu için tanımlayıcı bir ilişkisi olduğundan varsayılan olarak EF anahtar olmayan-veritabanında oluşturulmuş olarak ele alır.

### <a name="the-foreignkey-attribute"></a>ForeignKey özniteliği

Bir sıfır-veya-bire bir ilişki veya iki varlık arasında bire bir ilişki olduğunda (arasında tür `OfficeAssignment` ve `Instructor`), hangi ilişki sonu sorumlu olduğu ve hangi uç bağlıdır out EF çalışamıyor. Bire bir ilişkiler için başka bir sınıfın her bir sınıftaki bir başvuru gezinti özelliği vardır. [ForeignKey özniteliği](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) ilişkisi oluşturmak için bağımlı sınıfa uygulanabilir. Atlarsanız [ForeignKey özniteliği](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), geçiş oluşturmayı denediğinizde aşağıdaki hatayı alıyorsunuz:

*'ContosoUniversity.Models.OfficeAssignment' ve 'ContosoUniversity.Models.Instructor' türleri arasında bir ilişki birincil ucu belirlenemiyor. Veri ek açıklamaları veya ilişki fluent API'sini kullanarak bu ilişkiyi birincil ucu açıkça yapılandırılmalıdır.*

Öğreticinin ilerleyen bölümlerinde, bu ilişkiyi fluent API'si ile yapılandırma görürsünüz.

### <a name="the-instructor-navigation-property"></a>Eğitmen gezinme özelliği

`Instructor` Varlık içeriyor, boş değer atanabilir bir `OfficeAssignment` gezinti özelliği (bir eğitmen bir ofis ataması olmayabilir çünkü) ve `OfficeAssignment` atanamayan bir varlık olan `Instructor` gezinti özelliği (bir ofis ataması yapılamıyor çünkü Mevcut bir eğitmen-- `InstructorID` null değer alamaz). Olduğunda bir `Instructor` varlık ilgili olan `OfficeAssignment` varlık, her varlık, gezinti özelliğinin diğer bir başvuru gerekir.

Put bir `[Required]` Eğitmen Gezinti özelliğindeki ilgili Eğitmen olmalıdır, ancak (aynı zamanda olan bu tablo anahtarı) Instructorıd yabancı anahtar null atanamaz olduğundan, yapmanız gerekmez belirtmek için özniteliği.

## <a name="modify-the-course-entity"></a>Kurs varlığı değiştirme

İçinde *Models\Course.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

Kurs varlık bir yabancı anahtar özelliğine sahip `DepartmentID` işaret ettiği ilgili `Department` varlık ve bir `Department` gezinme özelliği. Entity Framework gezinme özelliğinin bağını ilgili varlık olduğunda veri modelinizi yabancı bir anahtar özellik eklemenize gerek yoktur. Gerekli olurlarsa olsunlar EF veritabanında yabancı anahtarlar otomatik olarak oluşturur. Ancak yabancı anahtar veri modelinde sahip güncelleştirmeleri daha basit ve daha verimli hale getirebilir. Örneğin, ne zaman getirme düzenlemek için bir kurs varlık `Department` varlık, null, yükleme şekilde kurs varlık güncelleştirdiğinizde varsa ilk getirilecek `Department` varlık. Yabancı anahtar özelliği `DepartmentID` dahildir veri modelindeki getirmek gerekmez `Department` güncelleştirmeden önce varlık.

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated özniteliği

[DatabaseGenerated özniteliği](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) ile [hiçbiri](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) parametresi `CourseID` birincil anahtar değerlerini kullanıcı tarafından sağlanan yerine veritabanı tarafından oluşturulan özelliği belirtir.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Varsayılan olarak Entity Framework, birincil anahtar değerlerini veritabanı tarafından oluşturulan varsayar. Çoğu senaryoda, istediğiniz olmasıdır. Ancak, `Course` varlıkları tek bir departmanda, başka bir bölüme, 2000 serilerinin için 1000 serisi gibi bir kullanıcı tarafından belirtilen kurs numarası kullanın ve benzeri.

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Gezinti özellikleri ve yabancı anahtar özelliklerini `Course` varlık ilişkileri takip yansıtır:

- Bu yüzden bir kurs bir bölüme atanan bir `DepartmentID` yabancı anahtar ve `Department` yukarıda belirtilen nedenlerden dolayı gezinme özelliği.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Herhangi bir sayıda Öğrenciler içinde kayıtlı bir kurs olabilir böylece `Enrollments` olan bir koleksiyon gezinme özelliği:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Bir kurs birden çok Eğitmenler tarafından verilen böylece `Instructors` olan bir koleksiyon gezinme özelliği:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>Departman varlık oluşturma

Oluşturma *Models\Department.cs* aşağıdaki kod ile:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Sütun özniteliği

Daha önce kullandığınız [sütun özniteliği](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) sütun adı eşlemesini değiştirmek. Kodunda `Department` varlık `Column` sütunu, SQL Server'ı kullanarak tanımlanacak için SQL veri türü eşlemesi değiştirmek için özniteliği kullanılıyor [para](https://msdn.microsoft.com/library/ms179882.aspx) veritabanı türü:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Sütun eşlemesi Entity Framework özellik için tanımladığınız CLR türüne göre uygun SQL Server veri türü genellikle seçtiği için genelde gerekli değildir. CLR `decimal` türü bir SQL Server eşlenir `decimal` türü. Ancak bu durumda sütunu para birimi miktarları bulunduran bildiğiniz ve [para](https://msdn.microsoft.com/library/ms179882.aspx) veri türü için daha uygun olan. CLR veri türleri ve SQL Server veri türleri ile nasıl eşleştiğini hakkında daha fazla bilgi için bkz. [Entity FrameworkTypes için SqlClient](https://msdn.microsoft.com/library/bb896344.aspx).

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Yabancı anahtar ve gezinti özellikleri, aşağıdaki ilişkileri yansıtır:

- Bir bölüm olabilir veya bir yönetici olmayabilir ve yönetici her zaman bir eğitmen. Bu nedenle `InstructorID` yabancı anahtarı olarak özelliği eklenmiştir `Instructor` sonra varlık ve soru işareti eklenir `int` özelliği null olarak işaretlemek için ataması yazın. Gezinme özelliğini adlı `Administrator` ancak tutan bir `Instructor` varlık:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Bu yüzden bir departman birçok kursları olabilir bir `Courses` gezinti özelliği:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Kural gereği, art arda silme için alamayan yabancı anahtarlar ve çoktan çoğa ilişkiler için Entity Framework sağlar. Bu, bir geçiş eklemeye çalıştığınızda bir özel durum neden olur, döngüsel art arda silme kuralları'nda neden olabilir. Örneğin, siz tanımlarsanız `Department.InstructorID` özelliği null olarak, şu özel durum iletisini almak: "Başvuru ilişkisi verilmeyen döngüsel başvuru neden olur." İş kurallarınızı gerekirse `InstructorID` özelliğini alamayan, art arda silme ilişkiyi devre dışı bırakmak için aşağıdaki fluent API'si deyimi kullanılacak olurdu:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]

## <a name="modify-the-enrollment-entity"></a>Kayıt varlığı değiştirme

 İçinde *Models\Enrollment.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Aşağıdaki ilişkileri ve gezinti özelliklerini yabancı anahtar özelliklerini yansıtır:

- Yani bir kaydı tek bir kurs için olan bir `CourseID` yabancı anahtar özellik ve `Course` gezinti özelliği:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- Bu yüzden bir kayıt için tek bir öğrenci, kaydıdır bir `StudentID` yabancı anahtar özellik ve `Student` gezinti özelliği:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>Çoktan çoğa ilişkiler

Arasında bir çoktan çoğa ilişki `Student` ve `Course` varlıkları ve `Enrollment` varlık işlevleri bire çok birleşme tablo olarak *yüküyle* veritabanında. Diğer bir deyişle `Enrollment` tablosu yabancı anahtarlar birleştirilmiş tablolar için yanı sıra ek veri içerir (Bu durumda, birincil anahtar ve `Grade` özelliği).

Aşağıdaki çizim, bu ilişkiler bir varlık diyagramda nasıl göründüğünü gösterir. (Bu diyagramda kullanılarak oluşturulan [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); diyagram oluşturma, öğreticinin bir parçası değil, yalnızca kullanıldığı bir çizim olarak burada.)

![Öğrenci Course_many için many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Her ilişki çizgisine sahip 1 ucu ve bir yıldız işareti (\*) ve diğer uçta, bire çok ilişkisi belirten.

Varsa `Enrollment` tablo vermedi sınıf bilgileri eklemeyi unutmayın, yalnızca iki yabancı anahtarlara gerekecektir `CourseID` ve `StudentID`. Bu durumda, bire çok birleşme tabloya karşılık gelir *yükü olmadan* (veya *saf birleşim tablosundan*) veritabanında ve bir model sınıfı için hiç oluşturmak zorunda mıydı. `Instructor` Ve `Course` varlıklar olduğunu, bu tür bir çoktan çoğa ilişki, ve gördüğünüz gibi hiçbir varlık sınıfı bunlar arasında:

![Eğitmen Course_many için many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Birleşim tablosu ancak veritabanında gerekli veritabanı Aşağıdaki diyagramda gösterildiği gibi:

![Eğitmen Course_many için many_relationship_tables](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Entity Framework otomatik olarak oluşturur `CourseInstructor` tablo ve okuma ve dolaylı olarak okuma ve güncelleştirme güncelleştirme `Instructor.Courses` ve `Course.Instructors` Gezinti özellikleri.

## <a name="entity-relationship-diagram"></a>Varlık ilişkisi şeması

Aşağıdaki resimde tamamlanmış Okul model için Entity Framework Power Tools oluşturduğunuz diyagramda gösterilmektedir.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

Çoktan çoğa ilişki yanı sıra (\* için \*) ve çoğa bir ilişki (1 \*), bir sıfır-veya-bir ilişki satırı (için 1 0..1) arasında gördüğünüz gibi `Instructor` ve `OfficeAssignment` varlıklar ve sıfır-veya-bir-çok ilişki çizgisi (0..1 için \*) Eğitmen ve departman varlıklar arasında.

## <a name="add-code-to-database-context"></a>Veritabanı bağlamı için kod ekleyin

Sonra yeni varlıklarına ekleyeceksiniz `SchoolContext` sınıfı ve eşlemesini kullanmanın bazı özelleştirme [fluent API'si](https://msdn.microsoft.com/data/jj591617) çağırır. API "fluent" çünkü genellikle bir dizi yöntem çağrılarını aşağıdaki örnekte olduğu gibi tek bir deyimde birleştirerek stringing kullanılır:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

Bu öğreticide, özniteliklerle yapamazsınız veritabanı eşleme fluent API'sini kullanırsınız. Ancak, çoğu biçimlendirmeyi, doğrulama ve öznitelikleri kullanarak yapabileceğiniz eşleme kurallarını belirtmek için fluent API'sini kullanabilirsiniz. Bazı öznitelikler gibi `MinimumLength` fluent API'si ile uygulanamaz. Daha önce de belirtildiği `MinimumLength` şemasını değiştirmez, yalnızca geçerli bir istemci ve sunucu tarafı doğrulama kuralı

Bazı geliştiriciler özel olarak bunlar kendi varlık sınıfları "temiz" olan fluent API'sini kullanmayı tercih edin İstediğiniz ve fluent API'sini kullanarak yalnızca yapılabilir birkaç özelleştirmeleri öznitelikleri ve fluent API'si karıştırabilirsiniz, ancak genel olarak önerilen uygulama bu iki yaklaşımdan birini seçin ve bu tutarlı bir şekilde mümkün olduğunca kullanmaktır.

Yeni varlık veri modeli ve öznitelikleri kullanarak başarmadık veritabanı eşleme gerçekleştirmek eklemek için kodu değiştirin *DAL\SchoolContext.cs* aşağıdaki kod ile:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

Yeni deyiminde [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) yöntemi bire çok birleşme tablo yapılandırır:

- Çok-çok ilişkisi için `Instructor` ve `Course` varlıklar, kod birleştirme tablosu için tablo ve sütun adları belirtir. Kod öncelikle yapılandırabilirsiniz çoktan çoğa ilişki sizin için bu kod olmadan, ancak Remove() çağırmayın, varsayılan adları gibi alırsınız `InstructorInstructorID` için `InstructorID` sütun.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Aşağıdaki kod nasıl fluent API'si yerine öznitelikleri arasında bir ilişki belirtmek için kullandığınız bir örnek sağlar `Instructor` ve `OfficeAssignment` varlıkları:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

"Fluent API'si" deyimleri perde arkasında neler yaptığını hakkında daha fazla bilgi için bkz: [Fluent API'si](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) blog gönderisi.

## <a name="seed-database-with-test-data"></a>Çekirdek veritabanı test verileri ile

Değiştirin *Migrations\Configuration.cs* oluşturduğunuz yeni varlıklar için çekirdek veri sağlamak için aşağıdaki kodu dosyası.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

İlk öğreticide gördüğünüz gibi çoğu bu kod yalnızca güncelleştirir veya yeni bir varlık nesnesi oluşturur ve örnek veri özelliklerini test etmek için gerektiği gibi yükler. Ancak, fark nasıl `Course` bir çoktan çoğa ilişkisi varlığı ile `Instructor` varlık gerçekleştirilir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Oluştururken bir `Course` nesnesini başlatır `Instructors` kodu kullanarak boş bir koleksiyon gezinme özelliğini `Instructors = new List<Instructor>()`. Bu eklemek olanaklı kılar `Instructor` bu konuyla ilgili varlıkları `Course` kullanarak `Instructors.Add` yöntemi. Boş bir liste oluşturmadıysanız, çünkü bu ilişkileri eklemek saptayamazdınız `Instructors` özelliği null olur ve verilmeyen bir `Add` yöntemi. Liste başlatma oluşturucuya de ekleyebilirsiniz.

## <a name="add-a-migration"></a>Bir geçiş ekleyin

PMC girin `add-migration` komut (çalışmıyor `update-database` henüz komut):

`add-Migration ComplexDataModel`

Çalıştırmayı denediyseniz `update-database` bu noktada komut (yapma, henüz), şu hatayı elde edersiniz:

*ALTER TABLE deyimini FOREIGN KEY kısıtlaması ile çakıştı. "FK\_dbo. Kurs\_dbo. Departman\_DepartmentID ". Veritabanı "ContosoUniversity" Tablo "dbo. çakışma oluştu Departman", sütun 'DepartmentID'.*

Bazen mevcut verilerle geçişleri yürüttüğünüzde saplama verisini yabancı anahtar kısıtlamaları karşılamak için veritabanına eklemek ihtiyacınız ve şimdi yapmak zorunda olmasıdır. Oluşturulan kod ComplexDataModel `Up` atanamayan bir yöntem ekler `DepartmentID` yabancı anahtar `Course` tablo. Satırlarda zaten olduğundan `Course` tablo kod çalıştığında `AddColumn` SQL Server bilmediği hangi değerin null olamaz sütununda yerleştirme işlemi başarısız olur. Bu nedenle yeni bir sütun varsayılan bir değer verin ve varsayılan Departman olarak görev yapacak "Temp" adlı bir saplama bölüm oluşturmak için kodu değiştirmeniz gerekir. Sonuç olarak, varolan `Course` satır tüm ilgili sonra "Temp" departman `Up` yöntemi çalışır. Doğru departmanları için ilişkilendirebilirsiniz `Seed` yöntemi.

Düzen &lt; *zaman damgası&gt;\_ComplexDataModel.cs* dosya, kurs tabloya DepartmentID sütunu ekleyen bir kod satırının yorum ve (açıklamalı aşağıdaki vurgulanmış kodu ekleyin satır da vurgulanmıştır):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

Zaman `Seed` yöntemi çalıştığında, satır ekleyecek `Department` tablo ve ilgili mevcut `Course` bu yeni satırlara `Department` satır. Kullanıcı Arabiriminde kurslar eklemediyseniz, ardından artık "Temp" departman veya varsayılan değer üzerinde ihtiyacınız olacaktı `Course.DepartmentID` sütun. Birisi kursları uygulamayı kullanarak eklemiş olasılığını olanak tanımak için de güncelleştirmek istediğiniz `Seed` yöntemi kodu emin olmak için tüm `Course` satırları (yalnızca önceki çalıştırıcıları tarafından eklenmiş olanları `Seed` yöntemi) sahip Geçerli `DepartmentID` varsayılan kaldırmadan önce değerleri değer sütunu ve "Temp" bölümü silin.

## <a name="update-the-database"></a>Veritabanını Güncelleştir

Düzenlemeyi bitirdikten sonra &lt; *zaman damgası&gt;\_ComplexDataModel.cs* dosya, girin `update-database` geçiş yürütülecek PMC'yi komutunu.

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> Veri ve yapma şema değişiklikleri geçiş sırasında diğer hatalarıyla mümkündür. Gideremezsiniz Geçiş hataları alırsanız, bağlantı dizesi içinde veritabanı adını değiştirin veya veritabanını silin. Veritabanında yeniden adlandırmak için en basit yaklaşımdır *Web.config* dosya. Aşağıdaki örnekte gösterilen CU için değiştirilen adı\_Test:
>
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
>
> Yeni bir veritabanı ile geçirmek için veri yoktur ve `update-database` hatasız tamamlanması çok daha büyük olasılıkla komutu. Veritabanı silme hakkında yönergeler için bkz: [Visual Studio 2012'den bir veritabanını bırakmak nasıl](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).
>
> Bu başarısız olursa başka bir şey deneyebilirsiniz PMC'de aşağıdaki komutu girerek veritabanı yeniden başlatmanız şöyledir:
>
> `update-database -TargetMigration:0`

Veritabanında açın **Sunucu Gezgini** daha önce yaptığınız ve genişletin **tabloları** tüm tabloların oluşturulduğunu görmek için düğümü. (Hala varsa **Sunucu Gezgini** önceki süreyi açın, **Yenile** düğmesi.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

Bir model sınıfı için oluşturmamışsınızdır `CourseInstructor` tablo. Daha önce açıklandığı şekilde, bu bir birleştirme tablo arasında çok-çok ilişkisi için `Instructor` ve `Course` varlıklar.

Sağ `CourseInstructor` tablosunu seçip **tablo verilerini Göster** sonucu olarak da veri olduğunu doğrulamak için `Instructor` eklediğiniz varlıkları `Course.Instructors` gezinme özelliği.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="get-the-code"></a>Kodu alma

[Projeyi yükle](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a>Ek kaynaklar

Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET veri erişimi - önerilen kaynaklar](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Özelleştirilmiş veri modeli
> * Güncelleştirilmiş Öğrenci varlık
> * Oluşturulan Eğitmen varlık
> * Oluşturulan OfficeAssignment varlık
> * Kurs varlığı değiştirme
> * Departman varlık oluşturuldu
> * Kayıt varlığı değiştirme
> * Veritabanı bağlamı için eklenen kodu
> * Kapsanan veritabanı test verileri ile
> * Bir geçiş eklendi
> * Veritabanı güncelleştirildi

Okuma ve Entity Framework Gezinti özelliklerini yükler ilgili verileri görüntüleme hakkında bilgi edinmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [İlgili verileri okuma](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
