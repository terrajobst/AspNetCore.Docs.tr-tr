---
title: EF çekirdek - veri modeli - 5, 10 ile ASP.NET Core MVC
author: rick-anderson
description: Bu öğreticide daha fazla varlıkları ve ilişkileri ekleyebilir ve veri modeli, doğrulama, biçimlendirme ve eşleme kurallarını belirterek özelleştirebilirsiniz.
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: 8f7b0d45962e5ca04d8f4d32d9c80270fb1daa72
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-mvc-with-ef-core---data-model---5-of-10"></a>EF çekirdek - veri modeli - 5, 10 ile ASP.NET Core MVC

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması Entity Framework Çekirdek ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](intro.md).

Önceki eğitimlerine üç varlıklarının oluşan basit veri modeli ile çalışmıştır. Bu öğreticide daha fazla varlıkları ve ilişkileri ekleyeceksiniz ve biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirterek veri modelini özelleştirmek.

İşlemi tamamladığınızda, sınıflar aşağıdaki çizimde gösterilen tamamlanmış veri modeli oluşturan:

![Varlık diyagramı](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Veri modeli öznitelikleri kullanarak özelleştirme

Bu bölümde biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirtin öznitelikleri kullanarak veri modelini özelleştirmek nasıl görürsünüz. Ardından birkaç oluşturacağınız aşağıdaki bölümlerde tam Okul veri modeline ekleyerek sınıfları, önceden oluşturulmuş ve kalan varlık türleri için yeni sınıflar modelde oluşturma öznitelikleri.

### <a name="the-datatype-attribute"></a>Veri türü özniteliği

Tüm bu alan için ilgilendiğiniz olmasına rağmen tarih Öğrenci kayıt tarihler için tüm web sayfalarının şu anda süreyi tarihi ile birlikte gösterir. Veri ek açıklaması öznitelikleri kullanarak bir Yapabileceğiniz kod görüntüleme biçimi verileri görüntüler her görünümünde düzeltecektir Değiştir. Bir öznitelik ekleyeceksiniz, yapmak nasıl bir örnek görmek için `EnrollmentDate` özelliğinde `Student` sınıfı.

İçinde *Models/Student.cs*, ekleme bir `using` bildirimi `System.ComponentModel.DataAnnotations` ad alanı ekleyin `DataType` ve `DisplayFormat` özniteliklerini `EnrollmentDate` özelliği, aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

`DataType` Özniteliği veritabanı geçerli bir tür daha fazla belirli bir veri türünü belirtmek için kullanılır. Bu durumda yalnızca tarihi, tarih ve saat değil izlemek istiyoruz. `DataType` Tarih, saat, PhoneNumber, para birimi, EmailAddress ve daha fazla gibi birçok veri türleri için numaralandırma sağlar. `DataType` Özniteliği de otomatik olarak türüne özgü özellikleri sağlamak uygulama etkinleştir. Örneğin, bir `mailto:` bağlantı için oluşturulabilir `DataType.EmailAddress`, ve bir tarih seçici için sağlanan `DataType.Date` HTML5 destekleyen tarayıcılarda. `DataType` Özniteliği yayar HTML 5 `data-` HTML 5 tarayıcılar anlayabilirsiniz (okunur veri tire) öznitelikler. `DataType` Öznitelikler olmayan tüm doğrulama sağlar.

`DataType.Date` Görüntülenen tarih biçimi belirtmiyor. Varsayılan olarak, sunucunun CultureInfo üzerinde temel alan varsayılan biçimler göre veri alanı görüntülenir.

`DisplayFormat` Özniteliği açıkça tarih biçimini belirtmek için kullanılır:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` Ayar değeri düzenlemek için bir metin kutusu görüntülendiğinde biçimlendirmeyi de uygulanması gerektiğini belirtir. (, Bazı alanlar--Örneğin, para birimi değerleri için metin kutusuna para birimi simgesini düzenleme için istemeyebilirsiniz olduğunu istemeyebilirsiniz.)

Kullanabileceğiniz `DisplayFormat` kendisi, ancak tarafından özniteliktir genellikle kullanmak iyi bir fikir `DataType` de özniteliği. `DataType` Özniteliği bir ekranda işlemesini nasıl aksine veri semantiği iletir ve ile elde etmezsiniz aşağıdaki yararları sağlar `DisplayFormat`:

* Tarayıcı HTML5 özellikleri etkinleştirebilirsiniz (örneğin, bir Takvim denetimi, yerel ayar uygun para birimi simgesini, e-posta bağlantıları göstermek, bazı istemci-tarafı giriş doğrulama, vs.).

* Varsayılan olarak, tarayıcı, bölgesel ayarına göre doğru biçimi kullanarak bir veri oluşturmaz.

Daha fazla bilgi için bkz: [ \<Giriş > etiketi yardımcı belgelerine](../../mvc/views/working-with-forms.md#the-input-tag-helper).

Uygulamayı çalıştırın, Öğrenciler dizin sayfasına gidin ve zamanlar için kayıt tarihleri artık görüntülenir dikkat edin. Aynı Öğrenci modelini kullanan herhangi bir görünüm için geçerli olacaktır.

![Tarihleri gösteren Öğrenciler dizin sayfası](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>StringLength özniteliği

Veri doğrulama kuralları ve öznitelikleri kullanarak bir doğrulama hata iletisi de belirtebilirsiniz. `StringLength` Özniteliği veritabanında uzunluk üst sınırını ayarlar ve istemci tarafı ve sunucu tarafı sağlar ASP.NET MVC için doğrulama. En az dize uzunluğu Bu öznitelikte belirtebilirsiniz, ancak en düşük değer veritabanı şemasını temel bir etkisi yoktur.

Kullanıcılar için bir ad 50'den fazla karakter girmeyin sağlamak istediğinizi varsayın. Bu sınırlama eklemek için Ekle `StringLength` özniteliklerini `LastName` ve `FirstMidName` aşağıdaki örnekte gösterildiği gibi özellikleri:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

`StringLength` Özniteliği olmaz önlemek bir kullanıcı için bir ad boşluk girerek. Kullanabileceğiniz `RegularExpression` özniteliği girişine kısıtlamalar getirmek için. Örneğin, aşağıdaki kod, büyük harf olması için ilk karakter ve alfabetik olarak geriye kalan karakterler gerektirir:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

`MaxLength` Özniteliği benzer işlevleri sağlayan `StringLength` özniteliği ancak istemci tarafı sağlamaz doğrulama.

Veritabanı modeli artık veritabanı şeması değişikliği gerektirdiği şekilde değiştirilmiştir. Geçişler, veritabanına uygulama kullanıcı Arabirimi kullanarak eklemiş olabileceğiniz herhangi bir veri kaybetmeden şemasını güncelleştirmek için kullanacağız.

Yaptığınız değişiklikleri kaydedin ve projeyi oluşturun. Sonra proje klasöründe komut penceresi açın ve aşağıdaki komutları girin:

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

`migrations add` Komutu uyarır veri kaybına neden olabilir, çünkü değişiklik iki sütun için daha kısa uzunluk sağlar.  Geçişler adlı bir dosya oluşturur  *\<zaman damgası > _MaxLengthOnNames.cs*. Kod bu dosyayı içeren `Up` geçerli veri modeli eşleşecek şekilde veritabanını güncelleştirmek yöntemi. `database update` Komutu çalıştırılmadan bu kodu.

Geçişler dosya adına önek zaman damgası geçişler düzenlemek için Entity Framework tarafından kullanılır. Update-database komutunu çalıştırmadan önce birden çok geçiş oluşturabilir ve ardından tüm geçişler, oluşturuldukları sırada uygulanır.

Uygulama, belirleyin **Öğrenciler** sekmesini tıklatın, **Yeni Oluştur**ve en fazla 50 karakter uzunluğunda ya da bir ad girin. Tıkladığınızda **oluşturma**, istemci tarafı doğrulama hata iletisi gösterir.

![Dize uzunluğu hataları gösteren sayfa Öğrenciler dizin](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a>Sütun özniteliği

Sınıfları ve özellikleri veritabanına nasıl eşlendiğini denetlemek için öznitelikler de kullanabilirsiniz. Kullanılan adı olduğunu varsayın `FirstMidName` -ad alanı ikinci adı içeriyor olabilir çünkü alan. Ancak veritabanı sütunun adlandırılması istediğiniz `FirstName`, veritabanı geçici sorguları yazma kullanıcılar bu adı alışmanızı. Bu eşleme yapmak için kullanabileceğiniz `Column` özniteliği.

`Column` Özniteliği belirtir, bu, veritabanı oluşturulduktan sonra sütunu `Student` eşlendiği tablo `FirstMidName` özelliği adlı `FirstName`. Diğer bir deyişle, ne zaman kodunuzu başvurduğu `Student.FirstMidName`, veri öğesinden gelir veya içinde güncelleştirilmesi `FirstName` sütunu `Student` tablo. Sütun adları belirtmezseniz, bunlar özellik adı olarak aynı adı verilir.

İçinde *Student.cs* dosya, ekleme bir `using` deyimi için `System.ComponentModel.DataAnnotations.Schema` ve sütun adı özniteliğe eklemek `FirstMidName` vurgulanan aşağıdaki kodda gösterildiği gibi özelliği:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Eklenmesi `Column` özniteliği değiştirir destekleyen model `SchoolContext`, veritabanı eşleşmeyecektir.

Yaptığınız değişiklikleri kaydedin ve projeyi oluşturun. Sonra proje klasöründe komut penceresi açın ve başka bir geçiş oluşturmak için aşağıdaki komutları girin:

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

İçinde **SQL Server Nesne Gezgini**, Öğrenci Tablo Tasarımcısı'nı çift tıklatarak açın **Öğrenci** tablo.

![SSOX Öğrenciler tabloda geçişler sonra](complex-data-model/_static/ssox-after-migration.png)

İlk iki geçiş uygulamadan önce adı sütun türü nvarchar(MAX) bulunuyordu. FirstName FirstMidName nvarchar(50) ve sütun adı değişti artık oldukları.

> [!Note]
> Tüm sınıflar aşağıdaki bölümlerde oluşturma bitirmeden derlemek çalışırsanız, derleyici hataları alabilirsiniz.

## <a name="final-changes-to-the-student-entity"></a>Öğrenci varlık son değişiklikler

![Öğrenci varlık](complex-data-model/_static/student-entity.png)

İçinde *Models/Student.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Gerekli özniteliği

`Required` Öznitelik adı özellikleri gerekli alanlar yapar. `Required` Öznitelik değer türleri gibi null türleri için gerekli değil (DateTime, int, çift, float, vb..). Null olamaz türleri gerekli alanlar olarak otomatik olarak kabul edilir.

Kullanarak kaldırabilirsiniz `Required` özniteliği ve en az uzunluk parametresi için WITH replace `StringLength` özniteliği:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Görüntüleme özniteliği

`Display` Özniteliği, metin kutuları için resim yazısı "Ad", "Son adı", "Tam adı" ve "Kayıt tarihi" özellik adı yerine (Sözcük bölünmesi boşluk olan) her örnek olması gerektiğini belirtir.

### <a name="the-fullname-calculated-property"></a>Hesaplanan FullName özelliği

`FullName` diğer iki özellik birleştirerek oluşturulan bir değer döndürür hesaplanan bir özelliktir. Bu nedenle yalnızca get erişimcisi ve Hayır sahip `FullName` sütun veritabanında oluşturulmayacak.

## <a name="create-the-instructor-entity"></a>Eğitmen varlık oluştur

![Eğitmen varlık](complex-data-model/_static/instructor-entity.png)

Oluşturma *Models/Instructor.cs*, şablonu kodu aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Çeşitli özellikler Öğrenci ve Eğitmen varlıkları aynı olmasına dikkat edin. İçinde [uygulama devralma](inheritance.md) bu serideki sonraki öğretici, artıklık ortadan kaldırmak için bu kodu yeniden düzenlemeniz.

Ayrıca yazabilirsiniz şekilde tek bir çizgi birden çok öznitelik koyabilirsiniz `HireDate` gibi öznitelikleri:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>CourseAssignments ve OfficeAssignment Gezinti özellikleri

`CourseAssignments` Ve `OfficeAssignment` özelliklerdir Gezinti özellikleri.

Bir eğitmen kurslar herhangi bir sayıda öğretmek, bu nedenle `CourseAssignments` koleksiyonu olarak tanımlanır.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Bir gezinti özelliği birden çok varlık tutarsanız, türü, girişleri eklenmiş, silinmiş, güncelleştirilen ve bir liste olması gerekir.  Belirleyebileceğiniz `ICollection<T>` veya gibi bir tür `List<T>` veya `HashSet<T>`. Belirtirseniz `ICollection<T>`, EF oluşturur bir `HashSet<T>` varsayılan koleksiyon.

Bunlar neden neden `CourseAssignment` varlıklar açıklanmıştır aşağıda çok-çok ilişkileri hakkında bölümünde.

Contoso University iş kuralları durum bir eğitmen en çok bir office yalnızca olabilir böylece `OfficeAssignment` özelliği (hangi hiçbir office atanmışsa boş olabilir) tek bir OfficeAssignment varlık tutar.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>OfficeAssignment varlık oluştur

![OfficeAssignment varlık](complex-data-model/_static/officeassignment-entity.png)

Oluşturma *Models/OfficeAssignment.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Key özniteliği

Eğitmen ve OfficeAssignment varlıklar arasındaki bir-sıfır-veya-bir ilişkisi yoktur. Office atama yalnızca atandığı Eğitmen bağlantılı olarak var ve bu nedenle birincil anahtarı da kendi Eğitmen varlığa yabancı anahtar. Ancak adını kimliği veya classnameID adlandırma kuralını uyguladığınızdan değil çünkü Entity Framework otomatik olarak InstructorID bu varlığın birincil anahtarı olarak tanıyamıyor. Bu nedenle, `Key` özniteliği anahtar olarak tanımlamak için kullanılır:

```csharp
[Key]
public int InstructorID { get; set; }
```

Aynı zamanda `Key` varlık birincil anahtarı yok ancak bir şey classnameID veya kimliği dışında name özelliği istiyorsanız özniteliği

Sütunu için bir tanımlayıcı ilişkisi olduğundan varsayılan olmayan veritabanı-üretilmiş EF anahtar değerlendirir.

### <a name="the-instructor-navigation-property"></a>Eğitmen gezinti özelliği

Eğitmen varlık null atanabilir sahip `OfficeAssignment` gezinti özelliği (bir eğitmen office atama olmayabilir nedeniyle), ve OfficeAssignment varlık atanamayan bir sahip `Instructor` gezinti özelliği (office atama yapılamıyor çünkü bir eğitmen--mevcut `InstructorID` NULL olmayan olabilir). Eğitmen varlık ilgili OfficeAssignment varlık olduğunda, her bir varlık, gezinti özelliğinin başka bir başvuru olacaktır.

Put bir `[Required]` ilgili Eğitmen olmalıdır, ancak, çünkü bunu yapmanız gerekmez belirtmek için Eğitmen gezinti özelliği öznitelikte `InstructorID` (aynı zamanda olan bu tablo anahtarı) yabancı anahtar null.

## <a name="modify-the-course-entity"></a>İndirmelere varlık değiştirme

![İndirmelere varlık](complex-data-model/_static/course-entity.png)

İçinde *Models/Course.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

İndirmelere varlık yabancı anahtar özelliğine `DepartmentID` ilgili departmanı varlık ve bu işaret ettiği sahip bir `Department` gezinti özelliği.

Entity Framework ilgili varlık gezinme özelliğinin olduğunda yabancı anahtar özelliği, veri modeline Ekle gerektirmez.  EF otomatik olarak gerekli nerede olursa olsun veritabanındaki yabancı anahtarlar oluşturur ve oluşturur [gölge özellikleri](https://docs.microsoft.com/ef/core/modeling/shadow-properties) bunlar için. Ancak veri modelinde yabancı anahtar olan güncelleştirmeler daha basit ve daha verimli hale getirebilir. Düzenlemek için bir indirmelere varlık getirme, örneğin, departman varlık null olduğunda, yükleme böylece indirmelere varlık güncelleştirdiğinizde varsa ilk bölüm varlık getirilemedi. Zaman yabancı anahtar özelliği `DepartmentID` bulunan veri modelinde güncelleştirmeden önce departmanı varlık fetch gerekmez.

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated özniteliği

`DatabaseGenerated` İle öznitelik `None` parametresini `CourseID` özelliği, birincil anahtar değerlerini kullanıcı tarafından sağlanan yerine veritabanı tarafından oluşturulan belirtir.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Varsayılan olarak, birincil anahtar değerlerinin veritabanı tarafından oluşturulan Entity Framework varsayar. Çoğu senaryoda istediğiniz olmasıdır. Ancak, indirmelere varlıklar için 1000 serisi gibi bir kullanıcı tarafından belirtilen indirmelere numarası bir bölüm, başka bir bölüm için bir 2000 serisi için kullanabilir ve benzeri.

`DatabaseGenerated` Özniteliği de söz konusu olduğunda veritabanı sütunlarının tarih kaydetmek için kullanılan bir satır oluşturulurken veya güncelleştirilirken gibi varsayılan değerlerini oluşturmak için kullanılabilir.  Daha fazla bilgi için bkz: [oluşturulan Özellikler](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Yabancı anahtar özellikleri ve gezinti özellikleri indirmelere varlıktaki aşağıdaki ilişkileri yansıtır:

Bu yüzden bir indirmelere bir bölüme atanan bir `DepartmentID` yabancı anahtar ve `Department` yukarıda belirtilen nedenlerden için gezinme özelliği.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Bir indirmelere içinde kayıtlı Öğrenciler herhangi bir sayıda olabilir böylece `Enrollments` gezinti özelliği olduğundan koleksiyonu:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Bir indirmelere birden çok Eğitmen tarafından öğrettin böylece `CourseAssignments` bir koleksiyon gezinti özelliği olduğundan (tür `CourseAssignment` anlatılmıştır [daha sonra](#many-to-many-relationships)):

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a>Departman varlık oluştur

![Departman varlık](complex-data-model/_static/department-entity.png)


Oluşturma *Models/Department.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Sütun özniteliği

Daha önce kullandığınız `Column` sütun adı eşlemesi değiştirmek için öznitelik. Departman varlık kodunda `Column` özniteliği kullanılan sütun veritabanında SQL Server para türü kullanılarak tanımlanacağı için SQL veri türü eşlemesi değiştirmek için:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Sütun eşlemesi Entity Framework özelliği için tanımladığınız CLR türüne göre uygun SQL Server veri türü seçtiği için genellikle gerekli değil. CLR `decimal` yazın eşlemeleri SQL Server'a `decimal` türü. Ancak bu durumda sütun para birimi miktarları bulunduran ve para veri türü için daha uygun olan biliyor.

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Yabancı anahtar ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:

Bir bölüm olabilir veya bir yönetici olmayabilir ve yönetici her zaman bir eğitmen. Bu nedenle `InstructorID` Eğitmen varlığa yabancı anahtar olarak özellik eklenmiştir ve sonra bir soru işareti eklenir `int` özelliği null olarak işaretlemek için ataması yazın. Gezinti özelliği adlı `Administrator` ancak Eğitmen varlık tutar:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Bu yüzden kurslar gezinti özelliği bir bölüm birçok kurslar sahip olabilir:

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> Kurala göre art arda silme çok-çok ilişkileri ve null yabancı anahtarlar için Entity Framework sağlar. Bu, bir geçiş eklemeye çalıştığınızda, bir özel durum neden olacak döngüsel cascade delete kuralları'nda neden olabilir. Örneğin, Department.InstructorID özelliği null olarak tanımlamadığınız, EF durum olmasını istediğiniz değil departmanı sildiğinizde Eğitmen silmek için bir cascade delete kuralı yapılandırın. İş kuralları gerekirse `InstructorID` özelliğini alamayan, art arda silme ilişkiyi devre dışı bırakmak için aşağıdaki fluent API deyimi kullanması gerekir:
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a>Kayıt varlık değiştirme

![Kayıt varlık](complex-data-model/_static/enrollment-entity.png)

İçinde *Models/Enrollment.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Yabancı anahtar özellikleri ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:

Bu yüzden bir kaydolma kaydı için tek bir yol, kullanmamaktır bir `CourseID` yabancı anahtar özellik ve `Course` gezinti özelliği:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Bu yüzden bir kaydı tek bir öğrenci için olan bir `StudentID` yabancı anahtar özellik ve `Student` gezinti özelliği:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Çok-çok ilişkileri

Öğrenci ve indirmelere varlıklar arasında çok-çok ilişkisi vardır ve kayıt varlık işlevleri çoktan bire çok birleşme tablo olarak *yükü ile* veritabanındaki. "Yükünü" anlamına gelir kayıt tablo birleştirilmiş tablolarında (Bu durumda, bir birincil anahtar ve bir sınıf özelliği) için yabancı anahtarları yanı sıra ek veriler içerir.

Aşağıdaki çizimde, bu tür bir varlık şemada nasıl göründüğünü gösterir. (Bu diyagramda EF için Entity Framework güç araçları kullanılarak oluşturulan 6.x; diyagram oluşturma öğreticinin bir parçası değil, yalnızca kullanılıyor burada çizim olarak.)

![Öğrenci indirmelere çoklu ilişki çok](complex-data-model/_static/student-course.png)

Her ilişki ucu ve bir yıldız işareti (*) 1 diğer sırasında bir-çok ilişkisi belirten bulunur.

Kayıt tablo düzeyde bilgi eklemediyseniz, yalnızca iki yabancı anahtarları CourseID ve StudentID içerecek şekilde gerekir. Bu durumda, bir çok-çok birleştirme tablo yükü olmadan (veya bir saf birleştirme tablo) veritabanında olacaktır. Eğitmen ve indirmelere varlıkları bu tür bir çok-çok ilişkisi vardır ve sonraki adımınız yükü olmadan birleştirme tablosu olarak çalışması için bir varlık sınıfı oluşturmaktır.

(Örtük birleştirme tablolarını çok-çok ilişkileri ancak EF çekirdek değildir EF 6.x destekler. Daha fazla bilgi için bkz: [EF çekirdek GitHub deposuna tartışmada](https://github.com/aspnet/EntityFramework/issues/1368).) 

## <a name="the-courseassignment-entity"></a>CourseAssignment varlık

![CourseAssignment varlık](complex-data-model/_static/courseassignment-entity.png)

Oluşturma *Models/CourseAssignment.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a>Varlık adları birleştirme

Eğitmen kurslar çok-çok ilişkisi için veritabanında bir birleşim tablosundan gereklidir ve bir varlık kümesi tarafından temsil edilebilir gerekir. Bir birleştirme varlık adı yaygındır `EntityName1EntityName2`, bu durumda olacak `CourseInstructor`. Ancak, ilişkiyi tanımlayan bir ad seçmeniz önerilir. Veri modelleri basit başlatın ve, sık yükü daha sonra alma Hayır yükü birleştirmeler ile büyütün. Açıklayıcı varlık adı ile başlatırsanız, adı daha sonra değiştirmek zorunda kalmazsınız. Birleşim varlık ideal olarak, iş etki alanında kendi doğal (büyük olasılıkla tek sözcüklük) adına sahip. Örneğin, Books ve müşteriler derecelendirmeleri ilişkilendirilemiyor. Bu ilişki için `CourseAssignment` değerinden daha iyi bir seçimdir `CourseInstructor`.

### <a name="composite-key"></a>Bileşik anahtar

Yabancı anahtarlar benzersiz olarak boş değer atanabilir ve birlikte olmadığından tablonun her satırında belirlemek, ayrı bir birincil anahtar için gerek yoktur. *InstructorID* ve *CourseID* özellikleri birleşik birincil anahtar olarak çalışması. EF için birleşik birincil anahtarlar tanımlamak için tek yolu kullanmaktır *fluent API* (bunu öznitelikleri kullanarak yapılamaz). Sonraki bölümde birleşik birincil anahtar yapılandırmak nasıl görürsünüz.

Bileşik anahtarın bir indirmelere ve bir eğitmen için birden çok satır için birden çok satır sahip olsa da, aynı eğitmen ve indirmelere için birden fazla satır bulunamaz sağlar. `Enrollment` Birleştirme varlık çoğaltmaları bu tür olası şekilde kendi birincil anahtar tanımlar. Bu tür çoğaltmaları önlemek için benzersiz bir dizin yabancı anahtar alanları eklediğinizde veya yapılandırma `Enrollment` benzer birincil bileşik anahtar ile `CourseAssignment`. Daha fazla bilgi için bkz: [dizinleri](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-database-context"></a>Veritabanı bağlamı güncelleştir

Aşağıdaki vurgulanmış kodu ekleyin *Data/SchoolContext.cs* dosyası:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Bu kod, yeni varlıklar ekler ve CourseAssignment varlığın birleşik birincil anahtar yapılandırır.

## <a name="fluent-api-alternative-to-attributes"></a>Öznitelikleri Fluent API alternatifi

Kodda `OnModelCreating` yöntemi `DbContext` sınıfını kullanan *fluent API* EF davranışı yapılandırmak için. Genellikle bu örnekte olduğu gibi tek bir deyimde içine bir dizi yöntem çağrısı birlikte stringing tarafından kullanılmakta olduğu için API "fluent" olarak adlandırılır [EF Core belgeleri](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

Bu öğreticide, yalnızca özniteliklerle yapamayacağı veritabanı eşleme fluent API kullanıyorsunuz. Ancak, çoğu biçimlendirmeyi, doğrulama ve öznitelikleri kullanarak yapabilirsiniz eşleme kurallarını belirtmek için fluent API kullanabilirsiniz. Gibi bazı öznitelikler `MinimumLength` fluent API'si ile uygulanamaz. Daha önce belirtildiği gibi `MinimumLength` şema değiştirmez yalnızca bir istemci ve sunucu tarafı doğrulama kuralı uygular.

Bazı geliştiriciler özel olarak bunlar kendi sınıflar "temiz." tutabilirsiniz böylece fluent API kullanmayı tercih eder İstediğiniz ve yalnızca fluent API kullanılarak yapılabilir birkaç özelleştirmeler varsa öznitelikleri ve fluent API karıştırabilirsiniz, ancak genel olarak önerilen uygulama bu iki yaklaşımlardan birini seçin ve bu tutarlı bir şekilde olabildiğince kullanmaktır. Her ikisi de kullanırsanız, bir çakışma olduğunda Fluent API öznitelikleri geçersiz kılar.

Öznitelikler fluent API karşılaştırması hakkında daha fazla bilgi için bkz: [yapılandırma yöntemlerini](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Varlık diyagram gösteren ilişkileri

Aşağıdaki çizimde Entity Framework güç araçları için tamamlanan Okul modeli oluşturma diyagramda gösterilmektedir.

![Varlık diyagramı](complex-data-model/_static/diagram.png)

Bir-çok ilişkisi satırları yanı sıra (1 \*), eğitmen ve OfficeAssignment varlıkları ve sıfır-veya-bir-çok ilişkisi satırı arasında bir-sıfır-veya-bir ilişkisi satır (1 için 0.. 1 çokluğa) burada görebilirsiniz (0.. 1 çokluğa için *) arasında Eğitmen ve departman varlıklar.

## <a name="seed-the-database-with-test-data"></a>Çekirdek Test verileri veritabanıyla

Kodla *Data/DbInitializer.cs* dosya ile oluşturduğunuz yeni varlıklar için çekirdek veri sağlamak için aşağıdaki kodu.

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

İlk öğreticide gördüğünüz gibi bu kod çoğunu sadece yeni varlık nesnesi oluşturur ve örnek veri özelliklerini test etmek için gerektiği gibi yükler. Çok-çok ilişkileri işlenme dikkat edin: varlıklarda oluşturarak ilişkileri kod oluşturur `Enrollments` ve `CourseAssignment` katılma varlık kümeleri.

## <a name="add-a-migration"></a>Bir geçiş ekleme

Yaptığınız değişiklikleri kaydedin ve projeyi oluşturun. Sonra proje klasöründe komut penceresi açın ve girin `migrations add` komutu (yapma update-database komutunu henüz):

```console
dotnet ef migrations add ComplexDataModel
```

Olası veri kaybı hakkında bir uyarı alırsınız.

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Çalıştırmak çalıştıysanız `database update` bu noktada komutu (yapma, henüz), aşağıdaki hatayı alıyorsunuz:

> ALTER TABLE deyimi "FK_dbo. yabancı anahtar kısıtlaması ile çakıştı. Course_dbo. Department_DepartmentID". Veritabanı "ContosoUniversity" Tablo "dbo. çakışma oluştu Departman", sütun 'DepartmentID'.

Bazen geçişler var olan verilerle yürüttüğünüzde saplama veri yabancı anahtar kısıtlamaları karşılamak için veritabanına eklemeniz gerekir. Oluşturulan kod `Up` yöntemi null DepartmentID yabancı anahtar indirmelere tabloya ekler. Zaten satır yoksa indirmelere tabloda kod çalıştığında `AddColumn` SQL Server ne null olamaz sütununda yerleştirilecek değer bilmiyor olduğundan işlem başarısız olur. Bu öğretici için yeni bir veritabanı üzerinde geçiş çalıştıracaksınız ancak bir üretim uygulamasında aşağıdaki yönergeleri nasıl örneği göstermek için var olan verileri işlemek geçiş yapmanız gerekir.

Varsayılan departman olarak davranacak şekilde "Temp" adlı yeni bir sütun varsayılan bir değer vermek için kodu değiştirmek zorunda olan verilerle çalışmak ve bir saplama bölüm oluşturma bu geçiş yapmak için. Sonuç olarak, indirmelere satırları varolan tüm sonra "Temp" departmanına ilişkili olacağı `Up` yöntemi çalışır.

* Açık *{timestamp}_ComplexDataModel.cs* dosya. 

* DepartmentID sütun indirmelere tabloya ekler kod satırı çıkışı açıklama.

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* Aşağıdaki vurgulanmış kodu sonra departman tablosu yaratır kodu ekleyin:

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

Bir üretim uygulamasında, kod veya komut departmanı satır eklemek ve yeni bölüm satırlar indirmelere satırları ilişkili yazarsınız. Ardından artık "Temp" departman veya Course.DepartmentID sütunda varsayılan değer gerekir.

Yaptığınız değişiklikleri kaydedin ve projeyi oluşturun.

## <a name="change-the-connection-string-and-update-the-database"></a>Bağlantı dizesini değiştirin ve veritabanı güncelleştirme

Bir artık yeni kod sahip `DbInitializer` çekirdek verileri yeni varlıklar için boş bir veritabanı ekler sınıfı. Yeni bir boş veritabanı oluşturmak EF yapmak için bağlantı dizesinde veritabanında adını değiştirmek *appsettings.json* ContosoUniversity3 veya kullanmakta olduğunuz bilgisayarda kullandığınız parolalardan başka bir adı.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

Değişiklik Kaydet *appsettings.json*.

> [!NOTE]
> Veritabanı adının değiştirilmesi alternatif olarak, veritabanını silebilirsiniz. Kullanım **SQL Server Nesne Gezgini** (SSOX) veya `database drop` CLI komutu:
> ```console
> dotnet ef database drop
> ```

Veritabanı adı değiştirilmiş veya silinmiş veritabanı sonra çalıştırmak `database update` geçişler yürütülecek komut penceresinde komutu.

```console
dotnet ef database update
```

Uygulama neden çalıştırma `DbInitializer.Initialize` çalıştırın ve yeni veritabanı doldurmak için yöntem.

Daha önce yaptığınız gibi veritabanı içinde SSOX açın ve genişletin **tabloları** tabloların tümü oluşturulmuş olduğunu görmek için düğüm. (Önceki süreyi açmak SSOX hala varsa, tıklatın **yenileme** düğmesini.)

![SSOX tablolarında](complex-data-model/_static/ssox-tables.png)

Veritabanı çekirdeğini oluşturur Başlatıcı kodu tetiklemek için uygulamayı çalıştırın.

Sağ **CourseAssignment** tablo ve seçin **görünüm verilerini** veri içinde olduğunu doğrulamak için.

![SSOX CourseAssignment verileri](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a>Özet

Artık daha karmaşık veri modeli ve karşılık gelen veritabanı vardır. Aşağıdaki öğreticide, ilgili veri erişimi hakkında daha fazla bilgi edineceksiniz.

> [!div class="step-by-step"]
> [Önceki](migrations.md)
> [sonraki](read-related-data.md)  
