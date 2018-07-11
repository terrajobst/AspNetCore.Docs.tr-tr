---
title: Itanium tabanlı sistemler için ASP.NET Core MVC ile EF Core - veri modeli - 5 / 10
author: rick-anderson
description: Bu öğreticide, daha fazla varlıklar ve ilişkiler ekleyin ve veri modelini, doğrulama, biçimlendirme ve eşleme kuralları belirterek özelleştirin.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: 1d3c69c8c658b5ca2f0253b790b0dc75d44d3064
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38194097"
---
# <a name="aspnet-core-mvc-with-ef-core---data-model---5-of-10"></a>Itanium tabanlı sistemler için ASP.NET Core MVC ile EF Core - veri modeli - 5 / 10

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması, Entity Framework Core ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](intro.md).

Önceki öğreticilerde, bir basit veri modeliyle üç varlıklarının oluşturulma çalışmıştır. Bu öğreticide, daha fazla varlıklar ve ilişkiler ekleyeceksiniz ve biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirterek veri modeli özelleştireceksiniz.

İşlemi tamamladığınızda, varlık sınıfları, aşağıdaki çizimde gösterilen tamamlanmış veri modeli oluşturan:

![Varlık diyagramı](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Öznitelikleri kullanarak veri modelini özelleştirin

Bu bölümde, veri modeli, biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirten öznitelikleri kullanarak özelleştirmek nasıl görürsünüz. Daha sonra oluşturacağınız aşağıdaki bölümler birkaç ekleyerek eksiksiz Okul veri modeli sınıfları zaten oluşturduğunuz ve modelde kalan varlık türleri için yeni sınıflar oluşturma öznitelikleri.

### <a name="the-datatype-attribute"></a>Veri türü özniteliği

Tüm bu alan için önem verdiğiniz rağmen tarih Öğrenci kayıt tarihler için tüm web sayfalarının şu anda zaman tarihi ile birlikte görüntüler. Veri ek açıklama öznitelikleri kullanarak bir Yapabileceğiniz kodu görüntüleme biçimini gösteren veriler her görünümde düzeltir Değiştir. Bir özniteliğe ekleyeceksiniz, yapmak nasıl bir örnek görmek için `EnrollmentDate` özelliğinde `Student` sınıfı.

İçinde *Models/Student.cs*, ekleme bir `using` bildirimi `System.ComponentModel.DataAnnotations` ad alanı ve ekleme `DataType` ve `DisplayFormat` özniteliklerini `EnrollmentDate` özelliği, aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

`DataType` Özniteliği veritabanı iç türünden daha belirli bir veri türü belirtmek için kullanılır. Bu durumda yalnızca tarih değil tarih ve saati izlemek istiyoruz. `DataType` Tarih, saat, telefon numarası, para birimi, EmailAddress ve diğer birçok veri türlerini numaralandırma sağlar. `DataType` Özniteliğini de otomatik olarak türe özgü özellikler sağlamak için uygulamayı etkinleştirin. Örneğin, bir `mailto:` bağlantısını için oluşturulamaz `DataType.EmailAddress`, ve bir tarih seçici için sağlanan `DataType.Date` HTML5 destekleyen tarayıcılarda. `DataType` Öznitelik HTML 5 yayan `data-` HTML 5 tarayıcılar anlayabilirsiniz (okunur veri dash) öznitelikleri. `DataType` Öznitelikleri, tüm doğrulama sağlamadığınızdan.

`DataType.Date` Görüntülenen tarih biçimi belirtmiyor. Varsayılan olarak, sunucunun CultureInfo üzerinde temel alan varsayılan biçimler göre veri alanı görüntülenir.

`DisplayFormat` Açıkça tarih biçimini belirtmek için özniteliği kullanılır:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` Ayar değeri düzenlemek için metin kutusunda görüntülendiğinde biçimlendirme da uygulanması gerektiğini belirtir. (, Bazı alanlar için--örneğin para birimi değerleri için para birimi simgesi metin kutusundaki düzenleme için istediğiniz değil istemeyebilirsiniz.)

Kullanabileceğiniz `DisplayFormat` özniteliği kendisi, ancak bu, genellikle kullanmak iyi bir fikir `DataType` ayrıca özniteliği. `DataType` Özniteliği bir ekranda işlenecek nasıl aksine veri semantiği iletmez ve ile elde etmezsiniz aşağıdaki avantajları sağlar `DisplayFormat`:

* Tarayıcı HTML5 özellikleri etkinleştirebilirsiniz (örneğin, bir Takvim denetimi, yerel ayara uygun bir para birimi simgesi, e-posta bağlantıları göstermek, bazı istemci-tarafı giriş doğrulama, vs.).

* Varsayılan olarak, tarayıcı, yerel ayarları temel alarak doğru biçimde kullanarak verileri işlemez.

Daha fazla bilgi için [ \<Giriş > Etiket Yardımcısı belgeleri](../../mvc/views/working-with-forms.md#the-input-tag-helper).

Uygulamayı çalıştırın, Öğrenciler dizin sayfasına gidin ve süreleri artık kayıt tarihlerini gösterilir dikkat edin. Aynı Öğrenci modelini kullanan herhangi bir görünüm için geçerli olacaktır.

![Tarihleri gösteren Öğrenciler dizin sayfası](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>StringLength özniteliği

Veri doğrulama kuralları ve öznitelikleri kullanarak bir doğrulama hata iletisi de belirtebilirsiniz. `StringLength` Özniteliği veritabanında en fazla uzunluk ayarlar ve istemci tarafı ve sunucu tarafı sağlayan ASP.NET MVC için doğrulama. En az dize uzunluğu Bu öznitelikte belirtebilirsiniz, ancak en düşük değer, veritabanı şema üzerinde hiçbir etkisi yoktur.

Kullanıcılar için bir ad 50 karakterden uzun girmeyin sağlamak istediğinizi varsayın. Bu kısıtlama eklemek için Ekle `StringLength` özniteliklerini `LastName` ve `FirstMidName` aşağıdaki örnekte gösterildiği gibi özellikleri:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

`StringLength` Özniteliği olmaz önlemek bir kullanıcı için bir ad boşluk girmesini. Kullanabileceğiniz `RegularExpression` girişi kısıtlamaları uygulamak için özniteliği. Örneğin, aşağıdaki kod, ilk karakterin büyük harf olacak ve alfabetik olarak kalan karakterler gerektirir:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

`MaxLength` Özniteliği benzer işlevsellik sağlar `StringLength` özniteliği ancak istemci tarafı sağlamaz doğrulama.

Veritabanı modeli, artık veritabanı şemasını değişikliği gerektiren bir şekilde değiştirilmiştir. Geçişler, uygulama kullanıcı Arabirimi kullanarak veritabanına eklemiş olabileceğiniz herhangi bir veri kaybetmeden şemasını güncelleştirmek için kullanacaksınız.

Yaptığınız değişiklikleri kaydedin ve projeyi derleyin. Ardından proje klasöründe komut penceresi açın ve aşağıdaki komutları girin:

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

`migrations add` Komut uyarır veri kaybı oluşabileceği uzunluğu en fazla iki sütun için daha kısa değişiklik yapmadığından.  Geçişleri adlı bir dosya oluşturur  *\<zaman damgası > _MaxLengthOnNames.cs*. Kod içinde bu dosyayı içeren `Up` yönteminin veritabanı geçerli bir veri modeli eşleşecek şekilde güncelleştirir. `database update` Komutun çalıştığını, kod.

Geçişleri dosya adının önüne zaman damgası, Entity Framework tarafından geçişlerin sıralamak için kullanılır. Veritabanını Güncelleştir komutu çalıştırmadan önce birden çok geçiş oluşturabilirsiniz ve ardından tüm geçişlerde içinde oluşturuldukları sırada uygulanır.

Uygulamayı çalıştırın, seçin **Öğrenciler** sekmesinde **Yeni Oluştur**, 50 karakterden uzun ya da bir ad girin. Tıkladığınızda **Oluştur**, istemci tarafı doğrulama hata iletisi gösterir.

![Dize uzunluğu hataları gösteren sayfa Öğrenciler dizin](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a>Sütun özniteliği

Sınıfları ve özellikleri veritabanına nasıl eşlendiğini denetlemek için öznitelikleri de kullanabilirsiniz. Adı kullanmışsınız varsayalım `FirstMidName` -ad alanı da ikinci adı içeriyor olabileceğinden alan. Ancak, veritabanı sütununun adı için istediğiniz `FirstName`veritabanına karşı özel sorgular yazmak kullanıcılar bu adı alışmanızı sağlamak için. Bu eşleme yapmak için kullanabileceğiniz `Column` özniteliği.

`Column` Özniteliği belirtir, bu, veritabanı oluşturulduktan sonra sütunun `Student` eşleyen tablo `FirstMidName` özelliği adlandırılacak `FirstName`. Diğer bir deyişle, ne zaman kodunuzu başvurduğu `Student.FirstMidName`, veri kaynağından gelir veya içinde güncelleştirilmesi `FirstName` sütununun `Student` tablo. Sütun adları belirtmezseniz, özellik adı olarak aynı adı verilir.

İçinde *Student.cs* ekleyin bir `using` bildirimi `System.ComponentModel.DataAnnotations.Schema` ve sütun ad özniteliğini ekleyin `FirstMidName` vurgulanan aşağıdaki kodda gösterildiği gibi özelliği:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Ek `Column` özniteliği değişiklikleri model yedekleme `SchoolContext`, veritabanı eşleşmeyecektir.

Yaptığınız değişiklikleri kaydedin ve projeyi derleyin. Ardından proje klasöründe komut penceresi açın ve başka bir geçiş oluşturmak için aşağıdaki komutları girin:

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

İçinde **SQL Server Nesne Gezgini**, Öğrenci Tablo Tasarımcısı çift tıklayarak açın **Öğrenci** tablo.

![Geçiş sonrasında SSOX Öğrenciler tabloda](complex-data-model/_static/ssox-after-migration.png)

İlk iki geçişlerin uygulamadan önce ad sütunu türü nvarchar(MAX) yoktu. FirstName FirstMidName nvarchar(50) ve sütun adı değişti artık oldukları.

> [!Note]
> Aşağıdaki bölümlerde tüm varlık sınıfları oluşturma tamamlanmadan önce derlemek denerseniz derleyici hataları alabilirsiniz.

## <a name="final-changes-to-the-student-entity"></a>Öğrenci varlık için son değişiklikler

![Öğrenci varlık](complex-data-model/_static/student-entity.png)

İçinde *Models/Student.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Gerekli öznitelik

`Required` Öznitelik adı özellikleri gerekli alanları yapar. `Required` Öznitelik değer türleri gibi atanamaz türler için gerekli olmayan (DateTime, int, çift, float, vs.). Null olamaz türleri gerekli alanları otomatik olarak kabul edilir.

Kullanarak kaldırabilirsiniz `Required` için en az uzunluk parametresi ile değiştirin ve öznitelik `StringLength` özniteliği:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Görüntüleme özniteliği

`Display` Özniteliği, metin kutuları için açıklama yazısını "Ad", "Soyadı", "Tam adı" ve "Kayıt tarihi" özellik adı yerine (Sözcük bölünmesi boşluk olan) her örnek olması gerektiğini belirtir.

### <a name="the-fullname-calculated-property"></a>Hesaplanan FullName özelliği

`FullName` diğer iki özellikleri birleştirilerek oluşturulur bir değer döndüren bir hesaplanan özelliktir. Bu nedenle yalnızca bir alma erişimcisi ve Hayır sahip `FullName` sütunu veritabanında oluşturulur.

## <a name="create-the-instructor-entity"></a>Eğitmen varlık oluşturma

![Eğitmen varlık](complex-data-model/_static/instructor-entity.png)

Oluşturma *Models/Instructor.cs*, şablon kodunu aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Çeşitli özellikler Öğrenci ve Eğitmen varlıkları aynı olduğuna dikkat edin. İçinde [uygulama devralma](inheritance.md) bu serideki sonraki öğretici, artıklığı ortadan kaldırmak için bu kodu yeniden düzenleyin.

Yazma böylece birden çok öznitelik bir satıra koyabilirsiniz `HireDate` gibi öznitelikleri:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>CourseAssignments ve OfficeAssignment Gezinti özellikleri

`CourseAssignments` Ve `OfficeAssignment` Gezinti özellikleri özelliklerdir.

Bir eğitmen kursları herhangi bir sayıda öğretebiliriz, bu nedenle `CourseAssignments` bir koleksiyon olarak tanımlandı.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Bir gezinme özelliği birden çok varlık tutarsanız türünü, girişler eklenir, silindi, güncelleştirildi ve bir liste olması gerekir.  Belirtebileceğiniz `ICollection<T>` ya da bir tür gibi `List<T>` veya `HashSet<T>`. Belirtirseniz `ICollection<T>`, EF oluşturur bir `HashSet<T>` varsayılan olarak koleksiyon.

Bunlar neden neden `CourseAssignment` varlıkları açıklanmıştır aşağıda çoktan çoğa ilişkiler hakkında bölümünde.

Contoso University iş kuralları durum bir eğitmen yalnızca en fazla bir office olabilir böylece `OfficeAssignment` özelliği (Bu, hiçbir office atanırsa null olabilir) tek bir OfficeAssignment varlık içerir.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>OfficeAssignment varlık oluşturma

![OfficeAssignment varlık](complex-data-model/_static/officeassignment-entity.png)

Oluşturma *Models/OfficeAssignment.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Anahtar öznitelik

Eğitmen ve OfficeAssignment varlıklar arasındaki bir sıfır-veya-bire bir ilişki yoktur. Atanan Eğitmeni ile ilgili bir ofis ataması yalnızca var ve bu nedenle birincil anahtarı da Eğitmen varlık, yabancı anahtar. Ancak kimliği veya Classnameıd adlandırma kuralı adının izleyin değil çünkü Entity Framework otomatik olarak Instructorıd bu varlığın birincil anahtarı tanınmıyor. Bu nedenle, `Key` özniteliği anahtar olarak tanımlamak için kullanılır:

```csharp
[Key]
public int InstructorID { get; set; }
```

Ayrıca `Key` varlığın birincil anahtarı yok ancak Classnameıd veya kimliği dışındaki ad özelliği denemek istiyorsanız özniteliği

Sütunu için tanımlayıcı bir ilişkisi olduğundan varsayılan olarak, olmayan-veritabanında oluşturulmuş olarak anahtar EF değerlendirir.

### <a name="the-instructor-navigation-property"></a>Eğitmen gezinme özelliği

Boş değer atanabilir bir eğitmen varlık sahip `OfficeAssignment` gezinti özelliği (bir eğitmen bir ofis ataması olmayabilir çünkü), ve OfficeAssignment varlık atanamayan bir sahip `Instructor` gezinti özelliği (bir ofis ataması yapılamıyor çünkü Mevcut bir eğitmen-- `InstructorID` null değer alamaz). Eğitmen varlığın ilgili OfficeAssignment varlık olduğunda, her varlık, gezinti özelliğinin diğer bir başvuru gerekir.

Put bir `[Required]` Eğitmen Gezinti özelliğindeki ilgili Eğitmen olmalıdır, ancak, çünkü bunu yapmak zorunda olmadığınız belirtmek için özniteliği `InstructorID` (aynı zamanda olan bu tablo anahtarı), yabancı anahtar null atanamaz.

## <a name="modify-the-course-entity"></a>Kurs varlığı değiştirme

![Kurs varlık](complex-data-model/_static/course-entity.png)

İçinde *Models/Course.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

Kurs varlık bir yabancı anahtar özelliğine sahip `DepartmentID` ilgili bölüm varlık ve onu işaret ettiği sahip bir `Department` gezinme özelliği.

Entity Framework gezinme özelliğinin bağını ilgili varlık olduğunda veri modelinizi yabancı bir anahtar özellik eklemenize gerek yoktur.  EF otomatik olarak ihtiyaç duyulan yere, veritabanında yabancı anahtarlar oluşturur ve oluşturur [gölge Özellikler](https://docs.microsoft.com/ef/core/modeling/shadow-properties) bunlar için. Ancak yabancı anahtar veri modelinde sahip güncelleştirmeleri daha basit ve daha verimli hale getirebilir. Departman Varlığı düzenlemek için bir kurs varlık getiren, örneğin, null, yükleme şekilde kurs varlık güncelleştirdiğinizde varsa ilk bölüm varlık getirilemedi. Yabancı anahtar özelliği `DepartmentID` içerdiği veri modelinde güncelleştirmeden önce departmanı varlık getiren gerekmez.

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated özniteliği

`DatabaseGenerated` Özniteliğini `None` parametresi `CourseID` birincil anahtar değerlerini kullanıcı tarafından sağlanan yerine veritabanı tarafından oluşturulan özelliği belirtir.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Varsayılan olarak Entity Framework, birincil anahtar değerlerini veritabanı tarafından oluşturulan varsayar. Çoğu senaryoda, istediğiniz olmasıdır. Ancak, kurs varlıklar için 1000 serisi gibi bir kullanıcı tarafından belirtilen kurs numarası bir bölümü, başka bir bölüme, 2000 serilerinin için kullanabilir ve benzeri.

`DatabaseGenerated` Özniteliği de kullanılabilir olması durumunda veritabanı sütunlarını tarihi kaydetmek için kullanılan bir satır oluşturulurken veya güncelleştirilirken gibi varsayılan değerleri oluşturmak için.  Daha fazla bilgi için [üretilen özellikleri](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Kurs varlık Gezinti özellikleri ve yabancı anahtar özelliklerini aşağıdaki ilişkileri yansıtır:

Bu yüzden bir kurs bir bölüme atanan bir `DepartmentID` yabancı anahtar ve `Department` yukarıda belirtilen nedenlerden dolayı gezinme özelliği.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Herhangi bir sayıda Öğrenciler içinde kayıtlı bir kurs olabilir böylece `Enrollments` olan bir koleksiyon gezinme özelliği:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Bir kurs birden çok Eğitmenler tarafından verilen böylece `CourseAssignments` olan bir koleksiyon gezinme özelliği (tür `CourseAssignment` açıklanan [daha sonra](#many-to-many-relationships)):

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a>Departman varlık oluşturma

![Departman varlık](complex-data-model/_static/department-entity.png)


Oluşturma *Models/Department.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Sütun özniteliği

Daha önce kullandığınız `Column` sütun adı eşlemesini değiştirmek için özniteliği. Departman varlık kodunda `Column` sütunu veritabanında SQL Server para türü kullanılarak tanımlanır için SQL veri türü eşlemesi değiştirmek için özniteliği kullanılıyor:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Sütun eşlemesi Entity Framework seçtiği özelliği için tanımladığınız CLR türüne göre uygun SQL Server veri türü için genelde gerekli değildir. CLR `decimal` türü bir SQL Server eşlenir `decimal` türü. Ancak, bu durumda sütunu para birimi miktarları bulunduran ve para veri türü için daha uygun olduğunu biliyor.

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Yabancı anahtar ve gezinti özellikleri, aşağıdaki ilişkileri yansıtır:

Bir bölüm olabilir veya bir yönetici olmayabilir ve yönetici her zaman bir eğitmen. Bu nedenle `InstructorID` Eğitmen varlığı için yabancı anahtar özelliği bulunur ve sonra bir soru işareti eklenir `int` özelliği null olarak işaretlemek için ataması yazın. Gezinme özelliğini adlı `Administrator` ancak bir eğitmen varlık tutar:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Kursları gezinti özelliği bu nedenle departman birçok kursları olabilir:

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> Kural gereği, art arda silme için alamayan yabancı anahtarlar ve çoktan çoğa ilişkiler için Entity Framework sağlar. Bu, bir geçiş eklemeye çalıştığınızda bir özel durum neden olur, döngüsel art arda silme kuralları'nda neden olabilir. Örneğin, Department.InstructorID özelliği null olarak tanımlamadığınız varsa EF sorun olmasını istediğiniz değildir departman sildiğinizde Eğitmen silmek için bir cascade delete kuralı yapılandırın. İş kurallarınızı gerekirse `InstructorID` özelliğini alamayan, art arda silme ilişkiyi devre dışı bırakmak için aşağıdaki fluent API'si deyimi kullanılacak olurdu:
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a>Kayıt varlığı değiştirme

![Kayıt varlık](complex-data-model/_static/enrollment-entity.png)

İçinde *Models/Enrollment.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Aşağıdaki ilişkileri ve gezinti özelliklerini yabancı anahtar özelliklerini yansıtır:

Yani bir kaydı tek bir kurs için olan bir `CourseID` yabancı anahtar özellik ve `Course` gezinti özelliği:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Bu yüzden bir kayıt için tek bir öğrenci, kaydıdır bir `StudentID` yabancı anahtar özellik ve `Student` gezinti özelliği:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Çoktan çoğa ilişkiler

Öğrenci ve kursu varlıklar arasında çoktan çoğa bir ilişki yoktur ve kayıt varlık işlevleri bire çok birleşme tablo olarak *yüküyle* veritabanında. "Yüke sahip" kayıt tablosu yabancı anahtarlar için birleştirilmiş tablolarında (Bu durumda, bir birincil anahtar ve bir sınıf özelliği) yanı sıra ek verileri içerdiğini gösterir.

Aşağıdaki çizim, bu ilişkiler bir varlık diyagramda nasıl göründüğünü gösterir. (Bu diyagramda EF için Entity Framework güç araçları kullanılarak oluşturulan 6.x; diyagram oluşturma, öğreticinin bir parçası değil, yalnızca kullanıldığı bir çizim olarak burada.)

![Öğrenci Kursluk çok-çok ilişkisi](complex-data-model/_static/student-course.png)

Her ilişki ucu ve bir yıldız işareti (*) 1 diğer sırasında bir-çok ilişkisi belirten bulunur.

Sınıf bilgileri kayıt tablosu eklemediyseniz, yalnızca CourseID ve StudentID iki yabancı anahtar içermesi gerekir. Bu durumda, bu bire çok birleşme tablo yükü olmadan (veya saf birleşim tablosu) veritabanında olacaktır. Bu tür çoktan çoğa ilişki bir eğitmen ve kurs varlıkların ve sonraki adımınız yükü olmadan bir JOIN tablo olarak çalışması için bir varlık sınıfı oluşturmaktır.

(Çok-çok ilişkileri ve EF Core için örtük birleşim tabloları değil EF 6.x destekler. Daha fazla bilgi için [tartışma EF Core GitHub deposunda](https://github.com/aspnet/EntityFramework/issues/1368).)

## <a name="the-courseassignment-entity"></a>CourseAssignment varlık

![CourseAssignment varlık](complex-data-model/_static/courseassignment-entity.png)

Oluşturma *Models/CourseAssignment.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a>Varlık adları katılın

Eğitmen kursları çoktan çoğa ilişki için veritabanında bir birleşim tablo gereklidir ve bir varlık kümesi tarafından temsil edilmesini vardır. Bir birleşim varlık adı yaygındır `EntityName1EntityName2`, bu durumda olacak `CourseInstructor`. Ancak, ilişkiyi tanımlayan bir ad seçmenizi öneririz. Veri modelleri basit ' ı başlatın ve, sık yükü daha sonra başlama Hayır yükü birleştirmelerle büyütün. Bir tanımlayıcı varlık adla başlatırsanız, daha sonra adını değiştirmek zorunda kalmazsınız. Birleştirme varlık ideal olarak, iş etki alanında kendi doğal (muhtemelen tek sözcük) adına sahip. Örneğin, kitaplar ve müşterilerin derecelendirmeleri bağ kurulamadı. Bu ilişki için `CourseAssignment` değerinden daha iyi bir seçimdir `CourseInstructor`.

### <a name="composite-key"></a>Bileşik anahtar

Yabancı anahtarlar benzersiz olarak boş değer atanabilir ve birlikte olmadığından tablonun her satırı tanımlamak, ayrı bir birincil anahtar için gerek yoktur. *Instructorıd* ve *CourseID* özellikleri, bileşik bir birincil anahtar olarak çalışması. Kullanarak EF bileşik birincil anahtarları tanımlamak için tek yolu olduğundan *fluent API'si* (Bu öznitelikleri kullanarak yapılamaz). Sonraki bölümde bileşik bir birincil anahtar yapılandırmak nasıl göreceksiniz.

Bileşik anahtarın bir kurs ve bir eğitmen için birden çok satır için birden çok satır olabilse kurs ve aynı eğitmen için birden fazla satır olamaz sağlar. `Enrollment` Birleştirme varlık çoğaltmaları bu tür mümkün olduğundan, kendi birincil anahtar tanımlar. Böyle yinelenen önlemek için benzersiz bir dizin yabancı anahtar alanları eklediğinizde veya yapılandırma `Enrollment` benzer şekilde birincil Bileşik anahtarı `CourseAssignment`. Daha fazla bilgi için [dizinleri](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-database-context"></a>Veritabanı bağlamı güncelleştir

Aşağıdaki vurgulanmış kodu ekleyin *Data/SchoolContext.cs* dosyası:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Bu kod, yeni varlıkları ekleyen ve CourseAssignment varlığın bileşik birincil anahtarını yapılandırır.

## <a name="fluent-api-alternative-to-attributes"></a>Fluent API'si alternatif öznitelikleri

Kodda `OnModelCreating` yöntemi `DbContext` sınıfının kullandığı *fluent API'si* EF davranışı yapılandırmak için. Bu örnekte olduğu gibi tek bir deyimde içine bir dizi yöntem çağrılarını birlikte stringing genellikle kullanıldığından API'sı "fluent" çağrılan [EF Core belgeleri](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

Bu öğreticide, yalnızca özniteliklerle yapamayacağınız veritabanı eşleme fluent API'si kullanıyorsunuz. Ancak, çoğu biçimlendirmeyi, doğrulama ve öznitelikleri kullanarak yapabileceğiniz eşleme kurallarını belirtmek için fluent API'sini kullanabilirsiniz. Bazı öznitelikler gibi `MinimumLength` fluent API'si ile uygulanamaz. Daha önce de belirtildiği `MinimumLength` şemasını değiştirmez, yalnızca bir istemci ve sunucu tarafı doğrulama kuralı uygular.

Bazı geliştiriciler özel olarak bunlar kendi varlık sınıfları "temiz" olan fluent API'sini kullanmayı tercih edin İstediğiniz ve fluent API'sini kullanarak yalnızca yapılabilir birkaç özelleştirmeleri öznitelikleri ve fluent API'si karıştırabilirsiniz, ancak genel olarak önerilen uygulama bu iki yaklaşımdan birini seçin ve bu tutarlı bir şekilde mümkün olduğunca kullanmaktır. Her ikisini de kullanıyorsanız, çakışma olduğunda Fluent API'si özniteliklerini geçersiz kılar.

Fluent API'si ve öznitelikler hakkında daha fazla bilgi için bkz. [yapılandırma yöntemleri,](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Varlık diyagramda gösteren ilişkileri

Aşağıdaki resimde tamamlanmış Okul model için Entity Framework Power Tools oluşturduğunuz diyagramda gösterilmektedir.

![Varlık diyagramı](complex-data-model/_static/diagram.png)

Çoğa bir ilişki yanı sıra (1 \*), eğitmen ve OfficeAssignment varlıkları ve sıfır-veya-bir-çok ilişki çizgisi arasındaki bir sıfır-veya-bir ilişki satırı (için 1 0..1) burada görebilirsiniz (0..1 için *) arasında Eğitmen ve departman varlıklar.

## <a name="seed-the-database-with-test-data"></a>Test verileri ile veritabanının çekirdeğini oluşturma

Değiştirin *Data/DbInitializer.cs* oluşturduğunuz yeni varlıklar için çekirdek veri sağlamak için aşağıdaki kodu dosyası.

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

İlk öğreticide gördüğünüz gibi çoğu bu kod, yalnızca yeni varlık nesnesi oluşturur ve örnek veri özelliklerini test etmek için gerektiği gibi yükler. Çoktan çoğa ilişkilerin nasıl işleneceğini dikkat edin: kod içindeki varlık oluşturarak ilişkileri oluşturur `Enrollments` ve `CourseAssignment` varlık kümeleri katılın.

## <a name="add-a-migration"></a>Bir geçiş ekleyin

Yaptığınız değişiklikleri kaydedin ve projeyi derleyin. Ardından proje klasöründe komut penceresi açın ve girin `migrations add` komut (yapma güncelleştirme database komutu henüz):

```console
dotnet ef migrations add ComplexDataModel
```

Olası veri kaybı hakkında bir uyarı alırsınız.

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Çalıştırmayı denediyseniz `database update` bu noktada komut (yapma, henüz), şu hatayı elde edersiniz:

> ALTER TABLE deyimini "FK_dbo. yabancı anahtar kısıtlaması ile çakıştı. Course_dbo. Department_DepartmentID". Veritabanı "ContosoUniversity" Tablo "dbo. çakışma oluştu Departman", sütun 'DepartmentID'.

Bazen, mevcut verilerle geçişleri yürüttüğünüzde, yabancı anahtar kısıtlamaları karşılamak için veritabanına saplama verisini eklemek gerekir. Oluşturulan kod `Up` yöntemi atanamayan DepartmentID yabancı anahtar kurs tabloya ekler. Varsa satırları kurs tabloda kod çalıştığında `AddColumn` SQL Server hangi değerin null olamaz sütununda put bilmediği işlemi başarısız olur. Bu öğretici için geçiş yeni bir veritabanı üzerinde çalıştırdığınız, ancak bir üretim uygulamasında aşağıdaki yönergeler bunu nasıl yapacağınız örneği böylece mevcut veri işleme geçiş yapması gerekebilir.

Yeni bir sütun bir varsayılan değeri vermek için kodu değiştirmek zorunda mevcut verilerle çalışacak ve bir saplama bölüm oluşturun, bu geçiş yapmak için "varsayılan Departman olarak görev yapacak Temp" adlı. Sonuç olarak, kurs satır var olan tüm sonra "Temp" departmanına ilişkilendirilir `Up` yöntemi çalışır.

* Açık *{timestamp}_ComplexDataModel.cs* dosya.

* DepartmentID sütun kurs tablosuna ekleyen bir kod satırı açıklama satırı yapın.

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* Departman tablosu oluşturan kodu sonra aşağıdaki vurgulanmış kodu ekleyin:

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

Bir üretim uygulamasında, kod veya betiklerde departmanı satır eklemek ve yeni bölüm satırları kurs satırları ilişkili yazmalısınız. Ardından artık "Temp" departman veya Course.DepartmentID sütununda Varsayılan değer ihtiyacınız olacaktı.

Yaptığınız değişiklikleri kaydedin ve projeyi derleyin.

## <a name="change-the-connection-string-and-update-the-database"></a>Bağlantı dizesini değiştirebilir ve veritabanını güncelleştir

Artık bir artık yeni koda sahip `DbInitializer` yeni varlıklar için çekirdek veri için boş bir veritabanı ekler sınıfı. Yeni bir boş veritabanı oluşturma EF yapmak için bağlantı dizesinde veritabanının adını *appsettings.json* ContosoUniversity3 veya kullanmakta olduğunuz bilgisayarda kullanmadığınız bazı diğer adı.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

Değişiklik Kaydet *appsettings.json*.

> [!NOTE]
> Veritabanı adının değiştirilmesi alternatif olarak, veritabanı silebilirsiniz. Kullanım **SQL Server Nesne Gezgini** (SSOX) veya `database drop` CLI komutunu:
> ```console
> dotnet ef database drop
> ```

Veritabanı adı değiştirilmiş veya silinmiş veritabanı sonra çalıştırın `database update` geçişlerin yürütmek için komut penceresinde komutu.

```console
dotnet ef database update
```

Neden için uygulamayı çalıştırın `DbInitializer.Initialize` çalıştırın ve yeni veritabanını doldurmak için yöntem.

Daha önce yaptığınız gibi veritabanı SSOX içinde açın ve genişletin **tabloları** tüm tabloların oluşturulduğunu görmek için düğümü. (Önceki süreyi açın SSOX hala varsa, tıklayın **Yenile** düğmesi.)

![SSOX tablolarında](complex-data-model/_static/ssox-tables.png)

Veritabanının çekirdeğini Başlatıcı kodu tetiklemek için uygulamayı çalıştırın.

Sağ **CourseAssignment** tablosunu seçip **görünüm verilerini** , veri içinde olduğunu doğrulayın.

![SSOX CourseAssignment verileri](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a>Özet

Artık daha karmaşık veri modeli ve karşılık gelen veritabanı vardır. Aşağıdaki öğreticide ilgili veri erişim hakkında daha fazla bilgi edineceksiniz.
::: moniker-end

> [!div class="step-by-step"]
> [Önceki](migrations.md)
> [İleri](read-related-data.md)
