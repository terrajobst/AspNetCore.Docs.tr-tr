---
title: "Razor sayfalarının EF çekirdek - veri modeli - 8'in 5 ile"
author: rick-anderson
description: "Bu öğreticide daha fazla varlıkları ve ilişkileri ekleyebilir ve veri modelinin biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirterek özelleştirebilirsiniz."
ms.author: riande
manager: wpickett
ms.date: 10/25/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: c375fe6ea98c621012eb55589c8b174c2a95b697
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="creating-a-complex-data-model---ef-core-with-razor-pages-tutorial-5-of-8"></a>Karmaşık veri model - EF çekirdek Razor sayfalarının öğretici (8'in 5) ile oluşturma

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Önceki öğreticileri üç varlıklarının oluşan bir temel veri modeli ile çalışmıştır. Bu öğreticide:

* Daha fazla varlıkları ve ilişkileri eklenir.
* Veri modeli biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirterek özelleştirilir.

Tamamlanan veri modeli için sınıflar aşağıdaki çizimde gösterilmiştir:

![Varlık diyagramı](complex-data-model/_static/diagram.png)

Olamaz çözmek sorunlarla karşılaşırsanız, indirme [Bu aşama için tamamlanan uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).

## <a name="customize-the-data-model-with-attributes"></a>Veri modeli öznitelikleri ile özelleştirme

Bu bölümde, veri modeli özniteliklerini kullanarak özelleştirilir.

### <a name="the-datatype-attribute"></a>Veri türü özniteliği

Öğrenci sayfaları şu anda kayıt tarihi süresini görüntüler. Genellikle, tarihi alanları, yalnızca tarih ve saati gösterir.

Güncelleştirme *Models/Student.cs* aşağıdaki vurgulanmış kodu:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

[DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) veritabanı geçerli bir tür daha fazla belirli bir veri türü özniteliği belirtir. Yalnızca tarih görüntülenmesi gerekir, bu durumda değil tarih ve saat. [DataType numaralandırma](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) tarih, saat, PhoneNumber, para birimi, EmailAddress, vb. gibi birçok veri türleri için sağlar. `DataType` Özniteliği de otomatik olarak türüne özgü özellikleri sağlamak uygulama etkinleştir. Örneğin:

* `mailto:` Bağlantı için otomatik olarak oluşturulduğunda `DataType.EmailAddress`.
* Tarih Seçici için sağlanan `DataType.Date` çoğu tarayıcılarda.

`DataType` Özniteliği yayar HTML 5 `data-` HTML 5 tarayıcılar tüketebilir (okunur veri tire) öznitelikler. `DataType` Öznitelikleri doğrulama sağlamaz.

`DataType.Date`Görüntülenen tarih biçimi belirtmiyor. Varsayılan olarak, sunucu üzerinde temel alan varsayılan biçimler göre tarih alanı görüntülenir [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).

`DisplayFormat` Özniteliği açıkça tarih biçimini belirtmek için kullanılır:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` Ayarı, biçimlendirmeyi de düzenleme UI uygulanması gerektiğini belirtir. Bazı alanlar kullanılamaz `ApplyFormatInEditMode`. Örneğin, para birimi simgesini genellikle bir düzenleme metin kutusunda görüntülenmemelidir.

`DisplayFormat` Özniteliği kendisi tarafından kullanılabilir. Bu genellikle kullanmak için iyi bir fikirdir `DataType` ile öznitelik `DisplayFormat` özniteliği. `DataType` Özniteliği bir ekranda işlemesini nasıl aksine veri semantiği iletir. `DataType` Özniteliği kullanılamayan aşağıdaki yararları sağlar `DisplayFormat`:

* Tarayıcı HTML5 özellikleri etkinleştirebilirsiniz. Örneğin, bir Takvim denetimi, yerel ayar uygun para birimi simgesini, e-posta bağlantılar, istemci-tarafı giriş doğrulaması vb. gösterir.
* Varsayılan olarak, tarayıcı bölgesel ayarına göre doğru biçimde kullanarak verileri işler.

Daha fazla bilgi için bkz: [ \<Giriş > etiketi yardımcı belgelerine](xref:mvc/views/working-with-forms#the-input-tag-helper).

Uygulamayı çalıştırın. Öğrenciler dizin sayfasına gidin. Zamanlar artık görüntülenir. Kullanan her görünümü `Student` model zaman olmadan tarihi görüntüler.

![Tarihleri gösteren Öğrenciler dizin sayfası](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>StringLength özniteliği

Veri doğrulama kuralları ve doğrulama hata iletilerinin özniteliklerle belirtilebilir. [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) özniteliği karakterden oluşan bir veri alanına izin verilen minimum ve maksimum uzunluğunu belirtir. `StringLength` Özniteliği, istemci ve sunucu tarafı doğrulama da sağlar. En düşük değer veritabanı şemasını temel bir etkisi yoktur.

Güncelleştirme `Student` aşağıdaki kodla model:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

Önceki kod adları en fazla 50 karakter sınırlar. `StringLength` Özniteliği değil engelleyen bir kullanıcı için bir ad boşluk girerek. [Yanıtta normal ifade](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) özniteliği girişine kısıtlamaları uygulamak için kullanılır. Örneğin, aşağıdaki kod, büyük harf olması için ilk karakter ve alfabetik olarak geriye kalan karakterler gerektirir:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

Uygulamayı çalıştırın:

* Öğrenciler sayfasına gidin.
* Seçin **Yeni Oluştur**ve en fazla 50 karakter uzunluğunda bir ad girin.
* Seçin **oluşturma**, istemci tarafı doğrulama hata iletisi gösterir.

![Dize uzunluğu hataları gösteren sayfa Öğrenciler dizin](complex-data-model/_static/string-length-errors.png)

İçinde **SQL Server Nesne Gezgini** (SSOX) Öğrenci Tablo Tasarımcısı'nı çift tıklatarak açın **Öğrenci** tablo.

![SSOX Öğrenciler tabloda geçişler önce](complex-data-model/_static/ssox-before-migration.png)

Önceki resimde için şemayı gösterilir `Student` tablo. Ad alanlarını türüne sahip `nvarchar(MAX)` geçişler değil çalıştırıldıysa çünkü DB'de. Daha sonra Bu öğreticide geçişler çalıştırdığınızda ad alanlarını hale `nvarchar(50)`.

### <a name="the-column-attribute"></a>Sütun özniteliği

Öznitelik sınıfları ve özellikleri veritabanına nasıl eşlendiğini denetleyebilir. Bu bölümde `Column` adını eşleştirmek için kullanılan öznitelik `FirstMidName` DB "FirstName" özelliğine.

DB oluşturulduğunda, model üzerinde özellik adlarını sütun adları için kullanılan (olmadığı dışında `Column` özniteliği kullanılır).

`Student` Modeli kullanır `FirstMidName` -ad alanı ikinci adı içeriyor olabilir çünkü alan.

Güncelleştirme *Student.cs* aşağıdaki vurgulanmış kodu dosyasıyla:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Yukarıdaki değişikliği ile `Student.FirstMidName` uygulamada eşlendiğini `FirstName` sütunu `Student` tablo.

Eklenmesi `Column` özniteliği değiştirir destekleyen model `SchoolContext`. Destekleyen model `SchoolContext` veritabanı artık eşleşmiyor. Geçişler uygulamadan önce uygulamayı çalıştırmak, şu özel durum oluşturulur:

```SQL
SqlException: Invalid column name 'FirstName'.
```
DB güncelleştirmek için:

* Projeyi oluşturun.
* Proje klasöründe bir komut penceresi açın. Yeni geçiş oluştur ve DB güncelleştirmek için aşağıdaki komutları girin:

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

`dotnet ef migrations add ColumnFirstName` Komutu aşağıdaki uyarı iletisini oluşturur:

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

Ad alanları artık olduğundan Uyarı oluşturulduğu 50 karakterle sınırlıdır. Veritabanında bir adı 50'den fazla karakter olsaydı, 51 son karakter olarak kaybolur.

* Uygulamayı test etme.

Öğrenci tablo içinde SSOX açın:

![SSOX Öğrenciler tabloda geçişler sonra](complex-data-model/_static/ssox-after-migration.png)

Geçiş uygulanmadan adı sütun türü olan [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql). Ad sütunlar şimdi: `nvarchar(50)`. Sütun adı değiştiğinden `FirstMidName` için `FirstName`.

> [!Note]
> Aşağıdaki bölümde bazı aşamalarda uygulaması oluşturmaya derleyici hataları oluşturur. Yönergeleri uygulamanızı oluşturmak ne zaman belirtin.

## <a name="student-entity-update"></a>Öğrenci varlık güncelleştirme

![Öğrenci varlık](complex-data-model/_static/student-entity.png)

Güncelleştirme *Models/Student.cs* aşağıdaki kod ile:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Gerekli özniteliği

`Required` Öznitelik adı özellikleri gerekli alanlar yapar. `Required` Öznitelik değer türleri gibi null türleri için gerekli değildir (`DateTime`, `int`, `double`vb..). Null olamaz türleri gerekli alanlar olarak otomatik olarak kabul edilir.

`Required` Özniteliği yerine bir en az uzunluk parametresi `StringLength` özniteliği:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Görüntüleme özniteliği

`Display` Özniteliği belirtir. "Ad", "Son adı", "Tam adı" ve "Kayıt tarihi." Başlık metin kutuları olmalıdır Örneğin "Lastname." sözcük bölme boşluk varsayılan başlıklar vardı

### <a name="the-fullname-calculated-property"></a>Hesaplanan FullName özelliği

`FullName`diğer iki özellik birleştirerek oluşturulan bir değer döndürür hesaplanan bir özelliktir. `FullName`, yalnızca bir get erişimcisine sahip ayarlanamıyor. Hayır `FullName` sütun veritabanında oluşturulur.

## <a name="create-the-instructor-entity"></a>Eğitmen varlık oluştur

![Eğitmen varlık](complex-data-model/_static/instructor-entity.png)

Oluşturma *Models/Instructor.cs* aşağıdaki kod ile:

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Çeşitli özelliklerin aynı olmasına dikkat edin `Student` ve `Instructor` varlıklar. Bu serideki sonraki uygulama devralma öğreticide artıklık ortadan kaldırmak için bu kodu bulunanad.

Birden çok öznitelik tek bir satırda olabilir. `HireDate` Öznitelikleri yazılmaya gibi:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>CourseAssignments ve OfficeAssignment Gezinti özellikleri

`CourseAssignments` Ve `OfficeAssignment` özelliklerdir Gezinti özellikleri.

Bir eğitmen kurslar herhangi bir sayıda öğretmek, bu nedenle `CourseAssignments` koleksiyonu olarak tanımlanır.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Bir gezinme özelliği birden çok varlık barındırıyorsa:

* Burada girişleri eklenmiş, silinmiş, güncelleştirilen ve bir liste türü olmalıdır.

Gezinti özelliği türleri şunlardır:

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

Varsa `ICollection<T>` belirtilirse, EF çekirdek oluşturur bir `HashSet<T>` varsayılan koleksiyon.

`CourseAssignment` Varlık çok-çok ilişkilerde bölümünde açıklanmaktadır.

Contoso University iş durumu bir eğitmen en çok bir office sağlayabilirsiniz kuralları. `OfficeAssignment` Özelliği tek bir tutar `OfficeAssignment` varlık. `OfficeAssignment`hiçbir office atanmışsa null şeklindedir.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>OfficeAssignment varlık oluştur

![OfficeAssignment varlık](complex-data-model/_static/officeassignment-entity.png)

Oluşturma *Models/OfficeAssignment.cs* aşağıdaki kod ile:

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Key özniteliği

`[Key]` Öznitelik, özellik adı bir şey olduğunda bir özelliği (PK) birincil anahtarı olarak classnameID veya kimliği dışında tanımlamak için kullanılır

Arasında bir-sıfır-veya-bir ilişkisi olduğundan `Instructor` ve `OfficeAssignment` varlıklar. Bir office atama yalnızca atandığı Eğitmen bağlantılı olarak zaten var. `OfficeAssignment` PK etmektir Ayrıca, yabancı anahtar (FK) `Instructor` varlık. EF çekirdek olamaz otomatik olarak tanıması `InstructorID` PK olarak `OfficeAssignment` nedeni:

* `InstructorID`Kimliği veya classnameID adlandırma kuralını uyguladığınızdan değil.

Bu nedenle, `Key` tanımlamak için kullanılan öznitelik `InstructorID` PK olarak:

```csharp
[Key]
public int InstructorID { get; set; }
```

Sütunu için bir tanımlayıcı ilişkisi olduğundan varsayılan olmayan veritabanı-üretilmiş EF çekirdek anahtar değerlendirir.

### <a name="the-instructor-navigation-property"></a>Eğitmen gezinti özelliği

`OfficeAssignment` İçin gezinme özelliği `Instructor` varlıktır boş değer atanabilir olduğundan:

* (Sınıfları boş değer atanabilir gibi) başvuru türleri.
* Bir eğitmen office atama sahip olmayabilir.


`OfficeAssignment` Varlık sahip atanamayan bir `Instructor` gezinti özelliği olduğundan:

* `InstructorID`NULL olmayan olabilir.
* Office atama bir eğitmen var olamaz.

Zaman bir `Instructor` varlık sahip ilgili `OfficeAssignment` varlık, her varlık, gezinti özelliğinin başka bir başvuru içeriyor.

`[Required]` Özniteliği uygulandığı `Instructor` gezinti özelliği:

```csharp
[Required]
public Instructor Instructor { get; set; }
```

Önceki kod ilgili Eğitmen olması gerektiğini belirtir. Önceki kod gerekli değildir çünkü `InstructorID` (aynı zamanda olan PK) yabancı anahtar null.

## <a name="modify-the-course-entity"></a>İndirmelere varlık değiştirme

![İndirmelere varlık](complex-data-model/_static/course-entity.png)

Güncelleştirme *Models/Course.cs* aşağıdaki kod ile:

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

`Course` Varlık sahip bir yabancı anahtar (FK) özellik `DepartmentID`. `DepartmentID`işaret ilgili `Department` varlık. `Course` Varlık sahip bir `Department` gezinti özelliği.

İlgili varlık için gezinme özelliği modele sahip olduğunda EF çekirdek FK özelliği için bir veri modeli gerektirmez.

Gerekli olan her yerde EF çekirdek FKs veritabanında otomatik olarak oluşturur. EF çekirdek oluşturur [gölge özellikleri](https://docs.microsoft.com/ef/core/modeling/shadow-properties) otomatik olarak oluşturulan FKs için. Veri modelinde FK sahip güncelleştirmeleri daha basit ve daha verimli hale getirebilir. Örneğin, bir model göz önünde bulundurun burada FK özelliği `DepartmentID` olan *değil* dahil. Ne zaman bir indirmelere varlığı düzenlemek için getirilir:

* `Department` Varlıktır açıkça yüklü değilse null.
* İndirmelere varlık güncelleştirileceğini `Department` varlık gerekir ilk getirildi.

Zaman FK özelliği `DepartmentID` bulunan veri modelinde getirmek için gerek yoktur `Department` bir güncelleştirme öncesi varlık.

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated özniteliği

`[DatabaseGenerated(DatabaseGeneratedOption.None)]` PK uygulama tarafından sağlanan yerine veritabanı tarafından oluşturulan özniteliği belirtir.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Varsayılan olarak, PK değerleri DB tarafından oluşturulan EF çekirdek varsayar. DB oluşturulan PK değerleri olan genellikle en iyi yaklaşımdır. İçin `Course` varlıklar, kullanıcı yinelenir belirtir Örneğin, bir indirmelere sayı matematik departmanı için 1000 serisi, İngilizce departmanı 2000 serisi gibi.

`DatabaseGenerated` Özniteliği de varsayılan değerlerini oluşturmak için kullanılabilir. Örneğin, DB otomatik olarak bir satır oluşturulurken veya güncelleştirilirken tarihi kaydetmek için bir tarih alanı oluşturabilirsiniz. Daha fazla bilgi için bkz: [oluşturulan Özellikler](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Yabancı anahtar (FK) özellikler ve gezinti özellikleri `Course` varlık yansıtacak aşağıdaki ilişkileri:

Bu yüzden bir indirmelere bir bölüme atanan bir `DepartmentID` FK ve `Department` gezinti özelliği.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Bir indirmelere içinde kayıtlı Öğrenciler herhangi bir sayıda olabilir böylece `Enrollments` gezinti özelliği olduğundan koleksiyonu:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Bir indirmelere birden çok Eğitmen tarafından öğrettin böylece `CourseAssignments` gezinti özelliği olduğundan koleksiyonu:

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment`açıklanan [daha sonra](#many-to-many-relationships).

## <a name="create-the-department-entity"></a>Departman varlık oluştur

![Departman varlık](complex-data-model/_static/department-entity.png)

Oluşturma *Models/Department.cs* aşağıdaki kod ile:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Sütun özniteliği

Daha önce `Column` özniteliği sütun adı eşlemesi değiştirmek için kullanıldı. Kodunda `Department` varlık, `Column` özniteliği SQL veri türü eşlemesi değiştirmek için kullanılır. `Budget` Sütun DB'de SQL Server para türü kullanılarak tanımlanır:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Sütun eşlemesi genellikle gerekli değildir. EF çekirdek genellikle özelliği için CLR türüne göre uygun SQL Server veri türünü seçer. CLR `decimal` yazın eşlemeleri SQL Server'a `decimal` türü. `Budget`para birimi, ve para veri türü para birimi için daha uygundur.

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

FK ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:

* Bir bölüm olabilir veya bir yönetici olmayabilir.
* Bir yönetici, her zaman bir eğitmen olur. Bu nedenle `InstructorID` FK olarak özellik eklenmiştir `Instructor` varlık.

Gezinti özelliği adlı `Administrator` ancak tutan bir `Instructor` varlık:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Özelliğin null atanabilir olup önceki kodda soru işareti (?) belirtir.

Bu yüzden kurslar gezinti özelliği bir bölüm birçok kurslar sahip olabilir:

```csharp
public ICollection<Course> Courses { get; set; }
```

Not: kurala göre art arda silme çok-çok ilişkileri ve null FKs için EF çekirdek sağlar. Art arda delete döngüsel cascade delete kurallarında neden olabilir. Döngüsel art arda silme kuralları nedenler özel bir durum geçiş eklendiğinde.

Örneğin, varsa `Department.InstructorID` özelliği tanımlanmamış olarak boş değer atanabilir:

* EF çekirdek departman silindiğinde Eğitmen silmek için bir cascade delete kuralı yapılandırır.
* Departman silindiğinde Eğitmen silinmesi amaçlanan bir davranış değildir.

İş kuralları gerekirse `InstructorID` özelliği null, aşağıdaki fluent API deyimini kullanın:

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

Önceki kod art arda silme departmanı Eğitmen ilişkiyi devre dışı bırakır.

## <a name="update-the-enrollment-entity"></a>Güncelleştirme kayıt varlık

Bir kaydı bir öğrenci tarafından gerçekleştirilen bir indirmelere içindir.

![Kayıt varlık](complex-data-model/_static/enrollment-entity.png)

Güncelleştirme *Models/Enrollment.cs* aşağıdaki kod ile:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

FK özellikler ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:

Bu yüzden bir kaydı bir yol için kullanmamaktır bir `CourseID` FK özelliği ve `Course` gezinti özelliği:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Bu yüzden bir kaydı bir öğrenci için olan bir `StudentID` FK özelliği ve `Student` gezinti özelliği:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Çok-çok ilişkileri

Arasında çok-çok ilişkisi olduğundan `Student` ve `Course` varlıklar. `Enrollment` Varlık işlevleri çoktan bire çok birleşme tablo olarak *yükü ile* veritabanındaki. "Yükünü" anlamına gelir `Enrollment` tablo FKs yanı sıra ek verileri birleştirilmiş tablolar için içerir (Bu durumda, PK ve `Grade`).

Aşağıdaki çizimde, bu tür bir varlık şemada nasıl göründüğünü gösterir. (Bu diyagram için EF EF güç araçları kullanılarak oluşturulan 6.x. Diyagram oluşturma öğreticinin bir parçası değil.)

![Öğrenci indirmelere çoklu ilişki çok](complex-data-model/_static/student-course.png)

Her ilişki ucu ve bir yıldız işareti (*) 1 diğer sırasında bir-çok ilişkisi belirten bulunur.

Varsa `Enrollment` tablosunu kaydetmedi düzeyde bilgi dahil etmek, yalnızca iki FKs içermesi gerekir (`CourseID` ve `StudentID`). Çoktan bire çok birleşme tablo yükü olmadan bazen saf birleşim tablosundan (PJT) adı verilir.

`Instructor` Ve `Course` varlığın saf birleştirme tablosunu kullanarak bir çok-çok ilişkisi vardır.

Not: EF 6.x destekler çok-çok ilişkileri ancak EF çekirdek için örtük birleştirme tabloları desteklemez. Daha fazla bilgi için bkz: [çok-çok ilişkileri EF çekirdek 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).

## <a name="the-courseassignment-entity"></a>CourseAssignment varlık

![CourseAssignment varlık](complex-data-model/_static/courseassignment-entity.png)

Oluşturma *Models/CourseAssignment.cs* aşağıdaki kod ile:

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a>Eğitmen kurslar

![Eğitmen kurslar m:M](complex-data-model/_static/courseassignment.png)

Eğitmen kurslar çok-çok ilişkisi:

* Bir varlık kümesi tarafından temsil edilebilir bir birleşim tablo gerektirir.
* Saf birleşim tablosundan (tablo yükü olmadan) olur.

Bir birleştirme varlık adı yaygındır `EntityName1EntityName2`. Örneğin, bu deseni kullanarak Eğitmen kurslar birleştirme tablodur `CourseInstructor`. Ancak, ilişkiyi tanımlayan bir ad kullanmanızı öneririz.

Veri modelleri basit başlatın ve büyütün. Hayır yükü birleşimler (PJTs) yükü dahil etmek için sık geliştirin. Açıklayıcı varlık adı ile başlatarak adı birleşim tablosundan değiştiğinde değiştirmek zorunda değildir. Birleşim varlık ideal olarak, iş etki alanında kendi doğal (büyük olasılıkla tek sözcüklük) adına sahip. Örneğin, Books ve müşteriler derecelendirmeleri adlı bir birleşim varlık ile ilişkilendirilemiyor. Eğitmen kurslar çoktan çoğa ilişki için `CourseAssignment` üzerinden tercih edilir `CourseInstructor`.

### <a name="composite-key"></a>Bileşik anahtar

FKs boş değer atanabilir değil. İçinde iki FKs `CourseAssignment` (`InstructorID` ve `CourseID`) birlikte her satır benzersizce `CourseAssignment` tablo. `CourseAssignment`ayrılmış yinelenir gerektirmez `InstructorID` Ve `CourseID` özellikleri bileşik yinelenir işlevi EF çekirdek için bileşik BA belirtmek için tek yolu *fluent API*. Sonraki bölümde bileşik yinelenir yapılandırmak nasıl gösterir

Bileşik anahtarın sağlar:

* Birden çok satır için bir indirmelere izin verilir.
* Birden çok satır için bir eğitmen izin verilir.
* Birden çok satır aynı eğitmen ve indirmelere için izin verilmiyor.

`Enrollment` Birleştirme varlık çoğaltmaları bu tür olası şekilde kendi PK tanımlar. Bu tür çoğaltmaları önlemek için:

* FK alanları benzersiz bir dizin ekleyin veya
* Yapılandırma `Enrollment` benzer birincil bileşik anahtar ile `CourseAssignment`. Daha fazla bilgi için bkz: [dizinleri](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-db-context"></a>Güncelleştirme DB bağlamı

Aşağıdaki vurgulanmış kodu ekleyin *Data/SchoolContext.cs*:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Önceki kod yeni varlıklar ekler ve yapılandırır `CourseAssignment` varlığın bileşik yinelenir

## <a name="fluent-api-alternative-to-attributes"></a>Öznitelikleri Fluent API alternatifi

`OnModelCreating` Önceki içinde yöntemi kod *fluent API* EF çekirdek davranışı yapılandırmak için. API, genellikle bir dizi yöntem çağrısı tek bir deyimde birleştirerek stringing tarafından kullanılmakta olduğu için "fluent" olarak adlandırılır. [Koddan](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) fluent API örneğidir:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

Bu öğreticide, fluent API özniteliklerle yapılamayacak DB eşlemesi için kullanılır. Bununla birlikte, fluent API biçimlendirmeyi, doğrulama ve öznitelikleri ile yapılabilir eşleme kurallarını çoğu belirtebilirsiniz.

Gibi bazı öznitelikler `MinimumLength` fluent API'si ile uygulanamaz. `MinimumLength`Şema değiştirmez yalnızca bir minimum uzunluğu doğrulama kuralı uygular.

Bazı geliştiriciler özel olarak bunlar kendi sınıflar "temiz." tutabilirsiniz böylece fluent API kullanmayı tercih eder Öznitelikler ve fluent API karışabilir. (Bileşik PK belirtme) fluent API'si ile yalnızca yapılabilir bazı yapılandırmaları vardır. Yalnızca özniteliklerle yapılabilir bazı yapılandırmalar vardır (`MinimumLength`). Fluent API veya öznitelikleri kullanarak için önerilen yöntem:

* Bu iki yaklaşımlardan birini seçin.
* Tutarlı bir şekilde olabildiğince seçilen yaklaşımı kullanın.

Bu konuda kullanılan öznitelikler bazıları öğretici için kullanılır:

* Yalnızca doğrulama (örneğin, `MinimumLength`).
* EF Çekirdek yapılandırmasını yalnızca (örneğin, `HasKey`).
* Doğrulama ve EF Çekirdek yapılandırmasını (örneğin, `[StringLength(50)]`).

Öznitelikler fluent API karşılaştırması hakkında daha fazla bilgi için bkz: [yapılandırma yöntemlerini](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Varlık diyagram gösteren ilişkileri

Aşağıdaki çizimde EF güç araçları için tamamlanan Okul modeli oluşturma diyagramda gösterilmektedir.

![Varlık diyagramı](complex-data-model/_static/diagram.png)

Önceki diyagramda gösterilmektedir:

* Birden çok-çok ilişkisi satıra (1 \*).
* Bir-sıfır-veya-bir ilişkisi satırın (1 için 0.. 1 çokluğa) arasında `Instructor` ve `OfficeAssignment` varlıklar.
* Sıfır-veya-bir-çok ilişkisi satır (0.. 1 çokluğa için *) arasında `Instructor` ve `Department` varlıklar.

## <a name="seed-the-db-with-test-data"></a>Çekirdek Test verilerle DB

Kodda güncelleştirme *Data/DbInitializer.cs*:

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

Önceki kod yeni varlıklar için çekirdek verileri sağlar. Bu kod çoğunu yeni varlık nesnesi oluşturur ve örnek verileri yükler. Örnek verileri test etmek için kullanılır. Yukarıdaki kod aşağıdaki çok-çok ilişkileri oluşturur:

* `Enrollments`
* `CourseAssignment`

Not: [EF çekirdek 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) destekleyecek [verileri dengeli](https://github.com/aspnet/EntityFrameworkCore/issues/629).

## <a name="add-a-migration"></a>Bir geçiş ekleme

Projeyi oluşturun. Proje klasöründeki bir komut penceresi açın ve aşağıdaki komutu girin:

```console
dotnet ef migrations add ComplexDataModel
```

Yukarıdaki komut, olası veri kaybı hakkında bir uyarı görüntüler.

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Varsa `database update` komutu çalıştırıldığında, aşağıdaki hata oluşturulur:

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

Var olan verilerle geçişler çalıştırdığınızda, mevcut verilerle memnun değil FK kısıtlamaları olabilir. FK bir kısıtlama ihlali olduklarından Bu öğretici için yeni bir veritabanı oluşturulur. Bkz: [eski verilerle yabancı anahtar kısıtlamaları düzelttikten](#fk) geçerli DB'de FK ihlalleri gidermeye yönelik yönergeler için.

## <a name="change-the-connection-string-and-update-the-db"></a>Bağlantı dizesini değiştirin ve DB güncelleştirme

Güncelleştirilmiş kod `DbInitializer` yeni varlıklar için çekirdek veri ekler. EF yeni bir boş veritabanı oluşturmak için çekirdek zorlamak için:

* DB bağlantı dizesi adı değiştirmek *appsettings.json* ContosoUniversity3 için. Yeni bir ad bilgisayarda kullanılmamış bir adı olması gerekir.

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* Alternatif olarak, DB kullanarak silin:

    * **SQL Server Object Explorer** (SSOX).
    * `database drop` CLI komutu:

   ```console
   dotnet ef database drop
   ```

Çalıştırma `database update` komut penceresinde:

```console
dotnet ef database update
```

Yukarıdaki komut, tüm geçişler çalışır.

Uygulamayı çalıştırın. Uygulamanın çalıştığı çalışan `DbInitializer.Initialize` yöntemi. `DbInitializer.Initialize` Yeni DB doldurur.

DB içinde SSOX açın:

* Genişletme **tabloları** düğümü. Oluşturulan tablolar görüntülenir.
* SSOX önceden açılmışsa tıklatın **yenileme** düğmesi.

![SSOX tablolarında](complex-data-model/_static/ssox-tables.png)

İncelemek **CourseAssignment** tablosu:

* Sağ **CourseAssignment** tablo ve seçin **görünüm verilerini**.
* Doğrulama **CourseAssignment** tablo verileri içerir.

![SSOX CourseAssignment verileri](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a>Yabancı anahtar kısıtlamaları eski verilerle çözme

Bu bölümde isteğe bağlıdır.

Var olan verilerle geçişler çalıştırdığınızda, mevcut verilerle memnun değil FK kısıtlamaları olabilir. Var olan verileri geçirmek için üretim verileri ile adımları alınması gerekir. Bu bölümde FK sabiti ihlallerini düzeltmek örneğidir. Bu kod bir yedek olmadan değişiklik yok. Önceki bölümde tamamladıysanız ve veritabanı güncelleştirilmiş Bu kod değişiklikleri yapmayın.

*{Timestamp}_ComplexDataModel.cs* dosyası aşağıdaki kod içerir:

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

Önceki kod atanamayan bir ekler `DepartmentID` için FK `Course` tablo. Önceki öğretici DB'den satırları içeren `Course`, bu tablo geçişler tarafından güncelleştirilemez.

Yapmak için `ComplexDataModel` var olan verilerle geçiş çalışma:

* Yeni bir sütun vermek için kodu değiştirin (`DepartmentID`) varsayılan bir değer.
* Varsayılan departman olarak davranacak şekilde "Temp" adlı sahte bir bölüm oluşturun.

### <a name="fix-the-foreign-key-constraints"></a>Yabancı anahtar kısıtlamaları Düzelt

Güncelleştirme `ComplexDataModel` sınıfları `Up` yöntemi:

* Açık *{timestamp}_ComplexDataModel.cs* dosya.
* Açıklama ekler kod satırı çıkışı `DepartmentID` sütuna `Course` tablo.

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

Aşağıdaki vurgulanmış kodu ekleyin. Sonra yeni kod gider `.CreateTable( name: "Department"` engelle:[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

Varolan önceki değişikliklerle `Course` satır ilgili sonra "Temp" departman `ComplexDataModel` `Up` yöntemi çalışır.

Bir üretim uygulaması gerekir:

* Kod veya eklemek için komut dosyaları dahil `Department` satırlar ve ilişkili `Course` yeni satır `Department` satır.
* "Temp" departman veya varsayılan değeri kullanamayacak `Course.DepartmentID `.

Sonraki öğretici ilgili veriler içerir.

>[!div class="step-by-step"]
[Önceki](xref:data/ef-rp/migrations)
[sonraki](xref:data/ef-rp/read-related-data)
