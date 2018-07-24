---
title: ASP.NET core'da - veri modeli - 8'in 5 EF çekirdekli Razor sayfaları
author: rick-anderson
description: Bu öğreticide, daha fazla varlıklar ve ilişkiler ekleyin ve veri modelini, doğrulama, biçimlendirme ve eşleme kuralları belirterek özelleştirin.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 88d727b0545f1dacb56ea889e45b02f947867b19
ms.sourcegitcommit: 6425baa92cec4537368705f8d27f3d0e958e43cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39220605"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a>ASP.NET core'da - veri modeli - 8'in 5 EF çekirdekli Razor sayfaları

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

Önceki öğreticilerde, üç varlıklarının oluşturulma bir temel veri modeli ile çalışmıştır. Bu öğreticide:

* Daha fazla varlıklar ve ilişkiler eklenir.
* Veri modeli, biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirterek özelleştirilir.

Varlık sınıflarının tamamlanmış veri modeli için aşağıdaki çizimde gösterilmiştir:

![Varlık diyagramı](complex-data-model/_static/diagram.png)

Olamaz çözmek sorunlarla karşılaşırsanız, indirme [tamamlanan uygulama](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

## <a name="customize-the-data-model-with-attributes"></a>Veri modeli öznitelikleri ile özelleştirme

Bu bölümde, veri modeli, öznitelikleri kullanarak özelleştirilir.

### <a name="the-datatype-attribute"></a>Veri türü özniteliği

Öğrenci sayfaları, şu anda kayıt tarihi zamanı görüntüler. Genellikle, yalnızca tarih ve saat tarih alanları gösterin.

Güncelleştirme *Models/Student.cs* aşağıdaki vurgulanmış kodu:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

[DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) özniteliği veritabanı iç türünden daha belirli bir veri türü belirtir. Yalnızca tarih görüntülenmesi gerekir, bu durumda değil tarih ve saati. [Veri türü sabit listesi](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) tarih, saat, telefon numarası, para birimi, EmailAddress, vb. gibi çok sayıda veri türleri için sağlar. `DataType` Özniteliğini de otomatik olarak türe özgü özellikler sağlamak için uygulamayı etkinleştirin. Örneğin:

* `mailto:` Bağlantısını için otomatik olarak oluşturulur `DataType.EmailAddress`.
* Tarih Seçici için sağlanan `DataType.Date` çoğu tarayıcılarda.

`DataType` Öznitelik HTML 5 yayan `data-` HTML 5 tarayıcılar kullanma (okunur veri dash) öznitelikleri. `DataType` Öznitelikleri doğrulama sağlamadığınızdan.

`DataType.Date` Görüntülenen tarih biçimi belirtmiyor. Varsayılan olarak, sunucu üzerinde temel alan varsayılan biçimler göre tarih alanı görüntülenir [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).

`DisplayFormat` Açıkça tarih biçimini belirtmek için özniteliği kullanılır:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` Ayar biçimlendirme de düzenleme UI uygulanması gerektiğini belirtir. Bazı alanlar kullanmamalısınız `ApplyFormatInEditMode`. Örneğin, para birimi simgesi genellikle bir düzenleme kutusuna görüntülenmemelidir.

`DisplayFormat` Özniteliği kendisi tarafından kullanılabilir. Genel olarak kullanmak iyi bir fikir olduğunu `DataType` özniteliğini `DisplayFormat` özniteliği. `DataType` Özniteliği bir ekranda işlenecek nasıl aksine veri semantiği iletmez. `DataType` Özniteliği de kullanılamayan aşağıdaki faydaları sağlar `DisplayFormat`:

* Tarayıcı HTML5 özellikleri etkinleştirebilirsiniz. Örneğin, bir Takvim denetimi, yerel ayara uygun bir para birimi simgesi, e-posta bağlantılarını, istemci tarafı giriş doğrulaması vb. gösterir.
* Varsayılan olarak, tarayıcının yerel ayarları temel alarak doğru biçimde kullanarak verileri işler.

Daha fazla bilgi için [ \<Giriş > etiketi Yardımcısı belgeleri](xref:mvc/views/working-with-forms#the-input-tag-helper).

Uygulamayı çalıştırın. Öğrenciler dizin sayfasına gidin. Süreleri artık gösterilir. Kullanan her görünüm `Student` model zaman olmadan tarihi görüntüler.

![Tarihleri gösteren Öğrenciler dizin sayfası](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>StringLength özniteliği

Veri doğrulama kuralları ve doğrulama hata iletilerinin özniteliklerle belirtilebilir. [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) özniteliği minimum ve maksimum veri alanında izin verilen karakter uzunluğunu belirtir. `StringLength` Özniteliği, istemci ve sunucu tarafı doğrulama da sağlar. En düşük değer veritabanı şema üzerinde bir etkisi yoktur.

Güncelleştirme `Student` model aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

Yukarıdaki kod, adları en fazla 50 karakter sınırlar. `StringLength` Özniteliği değil önlemek bir kullanıcı için bir ad boşluk girmesini. [Yanıtta normal ifade](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) özniteliği giriş kısıtlamaları uygulamak için kullanılır. Örneğin, aşağıdaki kod, ilk karakterin büyük harf olacak ve alfabetik olarak kalan karakterler gerektirir:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

Uygulamayı çalıştırın:

* Öğrenciler sayfasına gidin.
* Seçin **Yeni Oluştur**ve en fazla 50 karakter uzunluğunda bir ad girin.
* Seçin **Oluştur**, istemci tarafı doğrulama hata iletisi gösterir.

![Dize uzunluğu hataları gösteren sayfa Öğrenciler dizin](complex-data-model/_static/string-length-errors.png)

İçinde **SQL Server Nesne Gezgini** (SSOX) Öğrenci Tablo Tasarımcısı çift tıklayarak açın **Öğrenci** tablo.

![Öğrenciler SSOX tablosunda geçişleri önce](complex-data-model/_static/ssox-before-migration.png)

Bir önceki resimde için şema gösterir `Student` tablo. Ad alanlarını türüne sahip `nvarchar(MAX)` geçişler değil çalıştırıldıysa çünkü DB'de. Geçişleri daha sonra Bu öğreticide çalıştırdığınızda, ad alanlarını haline `nvarchar(50)`.

### <a name="the-column-attribute"></a>Sütun özniteliği

Öznitelikler, sınıfları ve özellikleri veritabanına nasıl eşleştiğini kontrol edebilirsiniz. Bu bölümde, `Column` eşlemek için kullanılan öznitelik `FirstMidName` DB'de "FirstName" özelliği.

Bir veritabanı oluşturulduğunda, model üzerinde özellik adlarını sütun adları için kullanılır (olmadığı dışında `Column` özniteliği kullanılır).

`Student` Modeli kullanan `FirstMidName` -ad alanı da ikinci adı içeriyor olabileceğinden alan.

Güncelleştirme *Student.cs* aşağıdaki vurgulanmış kodu dosyası:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Önceki değişiklikle `Student.FirstMidName` uygulamada eşlendiğini `FirstName` sütununun `Student` tablo.

Ek `Column` özniteliği değişiklikleri model yedekleme `SchoolContext`. Model yedekleme `SchoolContext` veritabanı artık eşleşmiyor. Geçişleri uygulamadan önce uygulamayı çalıştırmak, şu özel durum oluşturulur:

```SQL
SqlException: Invalid column name 'FirstName'.
```
DB güncelleştirmek için:

* Projeyi oluşturun.
* Proje klasöründe bir komut penceresi açın. DB yeni bir geçiş oluşturmak için aşağıdaki komutları girin:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

------

`migrations add ColumnFirstName` Komutu aşağıdaki uyarı iletisi oluşturur:

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

Ad alanları artık olduğundan uyarısı oluşturulur 50 karakterle sınırlıdır. Bir DB adı 50 karakterden uzun olsaydı, son karakter için 51 kaybolur.

* Uygulamayı test etme.

Öğrenci tablosu içinde SSOX açın:

![Geçiş sonrasında SSOX Öğrenciler tabloda](complex-data-model/_static/ssox-after-migration.png)

Geçiş uygulanmadan önce ad sütunu türü olan [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql). Ad sütunu sunulmuştur `nvarchar(50)`. Sütun adı değiştiğinde `FirstMidName` için `FirstName`.

> [!Note]
> Aşağıdaki bölümde bazı aşamalarında bir uygulama oluşturmak, derleyici hataları oluşturur. Uygulama derleme zamanı yönergeleri belirtin.

## <a name="student-entity-update"></a>Öğrenci varlık güncelleştirme

![Öğrenci varlık](complex-data-model/_static/student-entity.png)

Güncelleştirme *Models/Student.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Gerekli öznitelik

`Required` Öznitelik adı özellikleri gerekli alanları yapar. `Required` Öznitelik değer türleri gibi atanamaz türler için gerekli olmayan (`DateTime`, `int`, `double`vb..). Null olamaz türleri gerekli alanları otomatik olarak kabul edilir.

`Required` Özniteliği yerine en küçük uzunluk parametre `StringLength` özniteliği:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Görüntüleme özniteliği

`Display` Özniteliği, metin kutuları için açıklama yazısını "Ad", "Soyadı", "Tam adı" ve "Kayıt tarihi" olması gerektiğini belirtir Örneğin "Lastname." sözcük bölme boşluk varsayılan açıklamalı alt yazılar vardı

### <a name="the-fullname-calculated-property"></a>Hesaplanan FullName özelliği

`FullName` diğer iki özellikleri birleştirilerek oluşturulur bir değer döndüren bir hesaplanan özelliktir. `FullName` , yalnızca bir get erişimcisine sahip, ayarlanmış olamaz. Hayır `FullName` sütunu veritabanında oluşturulur.

## <a name="create-the-instructor-entity"></a>Eğitmen varlık oluşturma

![Eğitmen varlık](complex-data-model/_static/instructor-entity.png)

Oluşturma *Models/Instructor.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

Birden çok öznitelik, bir satırda olabilir. `HireDate` Öznitelikleri yazılması gibi:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>CourseAssignments ve OfficeAssignment Gezinti özellikleri

`CourseAssignments` Ve `OfficeAssignment` Gezinti özellikleri özelliklerdir.

Bir eğitmen kursları herhangi bir sayıda öğretebiliriz, bu nedenle `CourseAssignments` bir koleksiyon olarak tanımlandı.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Bir gezinme özelliği birden çok varlık tutuyorsa:

* Burada girişler eklenir, silindi, güncelleştirilmiş ve bir liste türü olmalıdır.

Gezinti özelliği türleri şunlardır:

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

Varsa `ICollection<T>` belirtilirse, EF Core oluşturur bir `HashSet<T>` varsayılan olarak koleksiyon.

`CourseAssignment` Varlık çoktan çoğa ilişkilerde bölümünde açıklanmaktadır.

Contoso University iş durumu bir eğitmen en fazla bir office olabilir kuralları. `OfficeAssignment` Özelliği tek bir tutar `OfficeAssignment` varlık. `OfficeAssignment` hiçbir office atanırsa null olur.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>OfficeAssignment varlık oluşturma

![OfficeAssignment varlık](complex-data-model/_static/officeassignment-entity.png)

Oluşturma *Models/OfficeAssignment.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Anahtar öznitelik

`[Key]` Özniteliği özellik adını bir şey olduğunda bir özelliği birincil anahtarla (PK) Classnameıd veya kimliği dışındaki tanımlamak için kullanılır

Arasında bir sıfır-veya-bire bir ilişki yoktur `Instructor` ve `OfficeAssignment` varlıklar. Atanan Eğitmeni ile ilgili bir ofis ataması yalnızca var. `OfficeAssignment` PK etmektir aynı zamanda, yabancı anahtar (FK) `Instructor` varlık. EF Core otomatik olarak tanıyamıyor `InstructorID` PK olarak `OfficeAssignment` çünkü:

* `InstructorID` Kimliği veya Classnameıd adlandırma kuralını uyguladığınızdan değil.

Bu nedenle, `Key` tanımlamak için kullanılan öznitelik `InstructorID` PK olarak:

```csharp
[Key]
public int InstructorID { get; set; }
```

Sütunu için tanımlayıcı bir ilişkisi olduğundan varsayılan olarak, olmayan-veritabanında oluşturulmuş olarak anahtar EF Core değerlendirir.

### <a name="the-instructor-navigation-property"></a>Eğitmen gezinme özelliği

`OfficeAssignment` Gezinme özelliğinin bağını `Instructor` varlık boş değer atanabilir olmadığından:

* (Boş değer atanabilir sınıflardır gibi) başvuru türleri.
* Bir eğitmen bir ofis ataması olmayabilir.


`OfficeAssignment` Atanamayan bir varlık olan `Instructor` gezinme özelliği için:

* `InstructorID` null atanamaz olduğundan.
* Bir ofis ataması bir eğitmen var olamaz.

Olduğunda bir `Instructor` varlık ilgili olan `OfficeAssignment` varlık, her varlık, gezinti özelliğinin diğer bir başvuru içerir.

`[Required]` Özniteliği uygulandığı `Instructor` gezinti özelliği:

```csharp
[Required]
public Instructor Instructor { get; set; }
```

Yukarıdaki kod, ilgili Eğitmen olması gerektiğini belirtir. Yukarıdaki kod gereksizdir çünkü `InstructorID` (aynı zamanda olan PK) yabancı anahtar null atanamaz.

## <a name="modify-the-course-entity"></a>Kurs varlığı değiştirme

![Kurs varlık](complex-data-model/_static/course-entity.png)

Güncelleştirme *Models/Course.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

`Course` Varlık sahip bir yabancı anahtar (FK) özelliği `DepartmentID`. `DepartmentID` işaret ilgili `Department` varlık. `Course` Varlık sahip bir `Department` gezinme özelliği.

Gezinme özelliğinin bağını ilgili varlık modeli sahip olduğunda EF Core için bir veri modeli FK özelliğini gerektirmez.

İhtiyaç duyulan yere EF Core FKs veritabanında otomatik olarak oluşturur. EF Core oluşturur [gölge Özellikler](https://docs.microsoft.com/ef/core/modeling/shadow-properties) otomatik olarak oluşturulan FKs için. FK veri modelinde sahip güncelleştirmeleri daha basit ve daha verimli hale getirir. Örneğin, bir model düşünün burada FK özelliği `DepartmentID` olduğu *değil* dahil. Ne zaman bir kurs varlığı düzenlemek için getirilir:

* `Department` Varlığa açıkça yüklü değilse null.
* Kurs varlığı güncelleştirmek için `Department` varlık gereken ilk getirildi.

Zaman FK özelliği `DepartmentID` dahildir veri modelindeki getirilecek gerek yoktur `Department` bir güncelleştirmeden önce varlık.

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated özniteliği

`[DatabaseGenerated(DatabaseGeneratedOption.None)]` PK uygulama tarafından sağlanan yerine, veritabanı tarafından oluşturulan özniteliğini belirtir.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Varsayılan olarak EF Core PK değerleri DB tarafından oluşturulan varsayar. DB PK oluşturulan değeri genellikle en iyi yaklaşım. İçin `Course` varlıklar, kullanıcı yinelenir belirtir Örneğin, bir kurs sayı 1000 serisi matematik departmanı, 2000 serisi İngilizce departmanı gibi.

`DatabaseGenerated` Özniteliği de varsayılan değerleri oluşturmak için kullanılabilir. Örneğin, DB, otomatik olarak bir satır oluşturulduğunda veya güncelleştirildiğinde tarihi kaydetmek için bir tarih alanı oluşturabilirsiniz. Daha fazla bilgi için [üretilen özellikleri](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Gezinti özellikleri ve yabancı anahtar (FK) özellikleri `Course` varlık ilişkileri takip yansıtır:

Bu yüzden bir kurs bir bölüme atanan bir `DepartmentID` FK ve `Department` gezinme özelliği.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Herhangi bir sayıda Öğrenciler içinde kayıtlı bir kurs olabilir böylece `Enrollments` olan bir koleksiyon gezinme özelliği:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Bir kurs birden çok Eğitmenler tarafından verilen böylece `CourseAssignments` olan bir koleksiyon gezinme özelliği:

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment` açıklanan [daha sonra](#many-to-many-relationships).

## <a name="create-the-department-entity"></a>Departman varlık oluşturma

![Departman varlık](complex-data-model/_static/department-entity.png)

Oluşturma *Models/Department.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Sütun özniteliği

Daha önce `Column` özniteliği sütun adı eşlemesini değiştirmek için kullanıldı. Kodunda `Department` varlık `Column` özniteliği SQL veri türü eşlemesi değiştirmek için kullanılır. `Budget` Sütun, DB SQL Server para türü kullanılarak tanımlanır:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Sütun eşlemesi genellikle gerekli değildir. EF Core özelliği için CLR türüne göre uygun SQL Server veri türü genellikle seçer. CLR `decimal` türü bir SQL Server eşlenir `decimal` türü. `Budget` para birimi, ve para veri türü için para birimi daha uygundur.

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

FK ve gezinti özellikleri, aşağıdaki ilişkileri yansıtır:

* Bir bölüm olabilir veya bir yönetici olmayabilir.
* Bir yönetici, her zaman bir eğitmen olur. Bu nedenle `InstructorID` özelliği için FK olarak dahil edilen `Instructor` varlık.

Gezinme özelliğini adlı `Administrator` ancak tutan bir `Instructor` varlık:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Önceki kodda soru işareti (?), boş değer atanabilir bir özelliktir belirtir.

Kursları gezinti özelliği bu nedenle departman birçok kursları olabilir:

```csharp
public ICollection<Course> Courses { get; set; }
```

Not: Kural gereği, EF Core art arda silme için alamayan FKs ve çoktan çoğa ilişkiler için etkinleştirir. Basamaklı silme döngüsel cascade delete kurallarında neden olabilir. Döngüsel art arda silme kuralları nedenleri özel bir durum geçiş eklendiğinde.

Örneğin, varsa `Department.InstructorID` özelliği değildi tanımlı olarak boş değer atanabilir:

* EF Core departman silindiğinde Eğitmen silmek için bir cascade delete kuralı yapılandırır.
* Eğitmen departman silindiğinde, silme, amaçlanan bir davranış değildir.

İş kuralları gerekirse `InstructorID` özelliği NULL olmayan, aşağıdaki fluent API'si deyimi kullanın:

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

Yukarıdaki kod art arda silme departmanı Eğitmen ilişkiyi devre dışı bırakır.

## <a name="update-the-enrollment-entity"></a>Güncelleştirme kayıt varlık

Bir kaydı bir öğrenci tarafından gerçekleştirilen bir kurs içindir.

![Kayıt varlık](complex-data-model/_static/enrollment-entity.png)

Güncelleştirme *Models/Enrollment.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Aşağıdaki ilişkileri ve gezinti özelliklerini FK özelliklerini yansıtır:

Bir kaydı var. bir kursa olduğundan bir `CourseID` FK özelliği ve `Course` gezinti özelliği:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Bu yüzden bir kaydı bir öğrenci için olan bir `StudentID` FK özelliği ve `Student` gezinti özelliği:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Çoktan çoğa ilişkiler

Bir çoktan çoğa ilişki arasında `Student` ve `Course` varlıklar. `Enrollment` Varlık işlevleri bire çok birleşme tablo olarak *yüküyle* veritabanında. "Yüke sahip" anlamına gelir `Enrollment` tablo FKs yanı sıra birleştirilmiş tablolar için ek veri içerir (Bu durumda, PK ve `Grade`).

Aşağıdaki çizim, bu ilişkiler bir varlık diyagramda nasıl göründüğünü gösterir. (Bu diyagramda EF için EF güç araçları kullanılarak oluşturulan 6.x. Diyagram oluşturma öğreticinin bir parçası değildir.)

![Öğrenci Kursluk çok-çok ilişkisi](complex-data-model/_static/student-course.png)

Her ilişki ucu ve bir yıldız işareti (*) 1 diğer sırasında bir-çok ilişkisi belirten bulunur.

Varsa `Enrollment` tablo vermedi sınıf bilgileri eklemeyi unutmayın, yalnızca iki FKs içermesi gerekir (`CourseID` ve `StudentID`). Bire çok birleşme tablo yükü olmadan bazen saf birleşim tablosu (PJT) olarak adlandırılır.

`Instructor` Ve `Course` varlıkların saf birleştirme tablo kullanarak bir çoktan çoğa ilişki.

Not: EF 6.x destekler çoktan çoğa ilişkiler ancak EF Core için örtük birleşim tabloları değil. Daha fazla bilgi için [çoktan çoğa ilişkilerde EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).

## <a name="the-courseassignment-entity"></a>CourseAssignment varlık

![CourseAssignment varlık](complex-data-model/_static/courseassignment-entity.png)

Oluşturma *Models/CourseAssignment.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a>Eğitmen kursları

![Eğitmen kursları m:M](complex-data-model/_static/courseassignment.png)

Eğitmen kursları çoktan çoğa ilişki:

* Bir varlık kümesi tarafından temsil edilen bir birleşim tablo gerektirir.
* Saf birleşim tablosu (tablo yükü olmadan) var.

Bir birleşim varlık adı yaygındır `EntityName1EntityName2`. Örneğin, bu düzeni kullanmak Eğitmen kursları birleştirme tablodur `CourseInstructor`. Ancak, ilişkiyi tanımlayan bir ad kullanmanızı öneririz.

Veri modelleri basit ' ı başlatın ve büyütün. Hayır-yükü birleşimler (PJTs) yükü dahil etmek için sık değişime uğramaktadır. Bir tanımlayıcı varlık adı ile başlatarak ad birleştirme tablo değiştiğinde değiştirmek gerekmez. Birleştirme varlık ideal olarak, iş etki alanında kendi doğal (muhtemelen tek sözcük) adına sahip. Örneğin, kitaplar ve müşterilerin derecelendirmeleri adlı birleştirme varlıkla bağ kurulamadı. Eğitmen kursları çoktan çoğa ilişki `CourseAssignment` üzerinden tercih edilen `CourseInstructor`.

### <a name="composite-key"></a>Bileşik anahtar

FKs boş değer atanabilir değil. İçinde iki FKs `CourseAssignment` (`InstructorID` ve `CourseID`) birlikte her satırının benzersiz olarak tanımlanabilmesi `CourseAssignment` tablo. `CourseAssignment` adanmış bir yinelenir gerektirmez `InstructorID` Ve `CourseID` özellikleri bir bileşik PK olarak işlevi EF Core için bileşik BA belirtmek için tek yolu olan *fluent API'si*. Sonraki bölümde, bileşik PK yapılandırma işlemi gösterilmektedir

Bileşik anahtarın sağlar:

* Birden çok satır bir kurs için izin verilir.
* Birden çok satır bir eğitmen için izin verilir.
* Birden çok satır aynı eğitmen ve kurs için kullanılamaz.

`Enrollment` Birleştirme varlık çoğaltmaları bu tür mümkün olduğundan, kendi PK tanımlar. Bu tür çoğaltmaları engellemek için:

* FK alanlarda benzersiz bir dizin eklemek veya
* Yapılandırma `Enrollment` benzer şekilde birincil Bileşik anahtarı `CourseAssignment`. Daha fazla bilgi için [dizinleri](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-db-context"></a>DB bağlamı güncelleştir

Aşağıdaki vurgulanmış kodu ekleyin *Data/SchoolContext.cs*:

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Yukarıdaki kod, yeni varlıkları ekleyen ve yapılandırır `CourseAssignment` varlığın bileşik yinelenir

## <a name="fluent-api-alternative-to-attributes"></a>Fluent API'si alternatif öznitelikleri

`OnModelCreating` Önceki yöntemidir kod *fluent API'si* EF Core davranışı yapılandırmak için. API, genellikle bir dizi yöntem çağrılarını birleştirerek tek bir deyimde stringing kullanıldığı için "fluent" olarak adlandırılır. [Koddan](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) fluent API'si örneğidir:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

Bu öğreticide, yalnızca özniteliklerle yapılamaz DB eşlemesi için fluent API'si kullanılır. Ancak, fluent API'si biçimlendirmeyi, doğrulama ve öznitelikleri ile yapılabilir eşleme kurallarını çoğu belirtebilirsiniz.

Bazı öznitelikler gibi `MinimumLength` fluent API'si ile uygulanamaz. `MinimumLength` Şema değişmez yalnızca bir minimum uzunluğu doğrulama kuralı uygular.

Bazı geliştiriciler özel olarak bunlar kendi varlık sınıfları "temiz" olan fluent API'sini kullanmayı tercih edin Öznitelikleri ve fluent API'si karma olabilir. (Bir bileşik PK belirtme) fluent API'si ile yalnızca yapılabilir bazı yapılandırmalar vardır. Özniteliklerle yalnızca yapılabilir bazı yapılandırmalar vardır (`MinimumLength`). Fluent API'si veya öznitelikleri kullanarak için önerilen uygulama:

* Bu iki yaklaşımdan birini seçin.
* Tutarlı bir şekilde mümkün olduğunca seçilen yaklaşımı kullanın.

Bu konuda kullanılan öznitelikler bazıları öğreticisi için kullanılır:

* Yalnızca doğrulama (örneğin, `MinimumLength`).
* EF Core yalnızca yapılandırmayı (örneğin, `HasKey`).
* Doğrulama ve EF Core yapılandırma (örneğin, `[StringLength(50)]`).

Fluent API'si ve öznitelikler hakkında daha fazla bilgi için bkz. [yapılandırma yöntemleri,](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Varlık diyagramda gösteren ilişkileri

Aşağıdaki resimde tamamlanmış Okul modelini EF Power Tools oluşturduğunuz diyagramda gösterilmektedir.

![Varlık diyagramı](complex-data-model/_static/diagram.png)

Yukarıdaki diyagramda gösterilmektedir:

* Bire çok ilişkisi birkaç satır (1 \*).
* Arasında bir sıfır-veya-bir ilişki satırı (için 1 0..1) `Instructor` ve `OfficeAssignment` varlıklar.
* Sıfır-veya-bir-çok ilişki çizgisi (0..1 için *) arasında `Instructor` ve `Department` varlıklar.

## <a name="seed-the-db-with-test-data"></a>Çekirdek DB Test verileri ile

Kodu güncelleştirme *Data/DbInitializer.cs*:

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

Yukarıdaki kod yeni varlıklar için çekirdek veri sağlar. Bu kod çoğu, yeni varlık nesnesi oluşturur ve örnek verileri yükler. Örnek verileri, test etmek için kullanılır. Yukarıdaki kod, aşağıdaki bir çoktan çoğa ilişkiler oluşturur:

* `Enrollments`
* `CourseAssignment`

## <a name="add-a-migration"></a>Bir geçiş ekleyin

Projeyi oluşturun.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet ef migrations add ComplexDataModel
```

------

Yukarıdaki komut, olası veri kaybı ile ilgili bir uyarı görüntüler.

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Varsa `database update` komutu çalıştırın, aşağıdaki hata oluşturulur:

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

Geçişleri mevcut verilerle çalışırken mevcut verilerle karşılanmıyor FK kısıtlamaları olabilir. FK bir kısıtlama ihlali olduğundan bu öğreticide, yeni bir veritabanı oluşturulur. Bkz: [eski verilerle yabancı anahtar kısıtlamalarını düzeltme](#fk) geçerli bir veritabanı üzerinde FK ihlallerini düzeltmek yönergeler.

### <a name="drop-and-update-the-database"></a>Bırakın ve veritabanını güncelleştir

Güncelleştirilmiş kod `DbInitializer` yeni varlıklar için çekirdek veri ekler. Yeni bir veritabanı oluşturmak için EF Core zorlamak için bırakın ve DB güncelleştirin:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

İçinde **Paket Yöneticisi Konsolu** (PMC), aşağıdaki komutu çalıştırın:

```PMC
Drop-Database
Update-Database
```

Çalıştırma `Get-Help about_EntityFrameworkCore` Yardım bilgilerini almak için PMC'yi öğesinden.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Bir komut penceresi açın ve proje klasörüne gidin. Proje klasörünü içeren *Startup.cs* dosya.

Komut penceresinde aşağıdakileri girin:

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

------

Uygulamayı çalıştırın. Uygulama çalıştırmaları çalıştıran `DbInitializer.Initialize` yöntemi. `DbInitializer.Initialize` Yeni bir veritabanı doldurur.

Bir veritabanı içinde SSOX açın:

* SSOX daha önce açıldı, tıklayın **Yenile** düğmesi.
* Genişletin **tabloları** düğümü. Oluşturulan bir tablo görüntülenir.

![SSOX tablolarında](complex-data-model/_static/ssox-tables.png)

İnceleme **CourseAssignment** tablosu:

* Sağ **CourseAssignment** tablosunu seçip **görünüm verilerini**.
* Doğrulama **CourseAssignment** tablo verilerini içerir.

![SSOX CourseAssignment verileri](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a>Yabancı anahtar kısıtlamaları ile eski verileri düzeltiliyor

Bu bölüm isteğe bağlıdır.

Geçişleri mevcut verilerle çalışırken mevcut verilerle karşılanmıyor FK kısıtlamaları olabilir. Üretim verileriyle, mevcut verileri geçirmek için adım atılmalıdır. Bu bölümde, FK sabiti ihlallerini düzeltmek bir örnek sağlar. Bir yedek olmadan bu kod değişiklikleri yapmayın. Önceki bölümde tamamlandı ve veritabanını, bu kod değişiklikleri yapmayın.

*{Timestamp}_ComplexDataModel.cs* dosyası aşağıdaki kodu içerir:

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

Yukarıdaki kod atanamayan bir ekler `DepartmentID` FK için `Course` tablo. Önceki öğreticide Veritabanından satır içeren `Course`, bu tabloyu geçişleri tarafından güncelleştirilemez.

Yapmak `ComplexDataModel` mevcut verilerle geçiş iş:

* Yeni bir sütun vermek için kodu değiştirin (`DepartmentID`) varsayılan bir değer.
* Varsayılan departman olarak görev yapacak "Temp" adlı sahte bir bölüm oluşturun.

### <a name="fix-the-foreign-key-constraints"></a>Yabancı anahtar kısıtlamalarını düzeltme

Güncelleştirme `ComplexDataModel` sınıfları `Up` yöntemi:

* Açık *{timestamp}_ComplexDataModel.cs* dosya.
* Yorum ekleyen bir kod satırının `DepartmentID` sütuna `Course` tablo.

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

Aşağıdaki vurgulanmış kodu ekleyin. Sonra yeni kodu gider `.CreateTable( name: "Department"` engelle: [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

Varolan önceki değişikliklerle `Course` satır ilgili sonra "Temp" departman `ComplexDataModel` `Up` yöntemi çalışır.

Bir üretim uygulaması gerekir:

* Kod veya betiklerde eklemek için dahil `Department` satırları ve ilişkili `Course` yeni satır `Department` satır.
* "Temp" departman veya için varsayılan değer kullanmamanız `Course.DepartmentID`.

Sonraki öğreticiye ilgili verileri içerir.

::: moniker-end

> [!div class="step-by-step"]
> [Önceki](xref:data/ef-rp/migrations)
> [İleri](xref:data/ef-rp/read-related-data)
