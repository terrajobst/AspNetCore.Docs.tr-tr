---
title: 'Öğretici: EF Core ile karmaşık veri modeli oluşturma-ASP.NET MVC'
description: Bu öğreticide, daha fazla varlık ve ilişki ekleyin ve biçimlendirme, doğrulama ve eşleme kurallarını belirterek veri modelini özelleştirin.
author: rick-anderson
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: 313d951ccdd45ae1209ffd9612d24738822fbed8
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259600"
---
# <a name="tutorial-create-a-complex-data-model---aspnet-mvc-with-ef-core"></a>Öğretici: EF Core ile karmaşık veri modeli oluşturma-ASP.NET MVC

Önceki öğreticilerde, üç varlıktan oluşan basit bir veri modeliyle çalıştık. Bu öğreticide, daha fazla varlık ve ilişki ekleyeceksiniz ve biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirterek veri modelini özelleştireceksiniz.

İşiniz bittiğinde, varlık sınıfları aşağıdaki çizimde gösterilen tamamlanmış veri modelini oluşturacak:

![Varlık diyagramı](complex-data-model/_static/diagram.png)

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Veri modelini özelleştirme
> * Öğrenci varlığında değişiklik yap
> * Eğitmen varlığı oluşturma
> * OfficeAssignment varlığı oluştur
> * Kurs varlığını değiştirme
> * Departman varlığı oluştur
> * Kayıt varlığını değiştirme
> * Veritabanı bağlamını güncelleştirme
> * Test verileriyle çekirdek veritabanı
> * Geçiş Ekle
> * Bağlantı dizesini değiştirme
> * Veritabanını güncelleştirme

## <a name="prerequisites"></a>Önkoşullar

* [EF Core geçişleri kullanma](migrations.md)

## <a name="customize-the-data-model"></a>Veri modelini özelleştirme

Bu bölümde, biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirten öznitelikleri kullanarak veri modelini nasıl özelleştireceğinizi göreceksiniz. Ardından, aşağıdaki bölümlerde, zaten oluşturduğunuz sınıflara öznitelikler ekleyerek ve modeldeki kalan varlık türleri için yeni sınıflar oluşturarak tüm okul veri modelini oluşturacaksınız.

### <a name="the-datatype-attribute"></a>DataType özniteliği

Öğrenci kayıt tarihleri için tüm Web sayfaları Şu anda tarihle birlikte görüntülenir, ancak bu alan için tüm önemli bir tarih olması gerekir. Veri ek açıklaması özniteliklerini kullanarak, verileri gösteren her görünümde görüntü biçimini giderecek bir kod değişikliği yapabilirsiniz. Bunun nasıl yapılacağını gösteren bir örnek görmek için, `Student` sınıfındaki `EnrollmentDate` özelliğine bir öznitelik ekleyeceksiniz.

*Modeller/öğrenci. cs*' de, `System.ComponentModel.DataAnnotations` ad alanı için `using` ifadesini ekleyin ve aşağıdaki örnekte gösterildiği gibi `EnrollmentDate` özelliğine `DataType` ve `DisplayFormat` öznitelikleri ekleyin:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

@No__t-0 özniteliği, veritabanı iç türünden daha belirgin bir veri türü belirtmek için kullanılır. Bu durumda, tarihi ve saati değil yalnızca tarihi izlemek istiyoruz. @No__t-0 numaralandırması, tarih, saat, PhoneNumber, para birimi, Emaadresi ve daha fazlası gibi birçok veri türü sağlar. @No__t-0 özniteliği Ayrıca uygulamanın türe özgü özellikleri otomatik olarak sağlamasını da sağlayabilir. Örneğin, `DataType.EmailAddress` için `mailto:` bağlantısı oluşturulabilir ve HTML5 'i destekleyen tarayıcılarda `DataType.Date` için bir tarih seçici sağlaneklenebilir. @No__t-0 özniteliği HTML 5 tarayıcıların anlayabilmesi için HTML 5 `data-` (bir veri Dash) öznitelikleri yayar. @No__t-0 öznitelikleri herhangi bir doğrulama sağlamaz.

`DataType.Date`, görüntülenen tarihin biçimini belirtmiyor. Varsayılan olarak, veri alanı sunucunun CultureInfo öğesine göre varsayılan biçimlere göre görüntülenir.

@No__t-0 özniteliği, açıkça tarih biçimini belirtmek için kullanılır:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

@No__t-0 ayarı, bir metin kutusunda değer görüntülenmek üzere görüntülendiğinde biçimlendirmenin de uygulanacağını belirtir. (Bazı alanlar için bunu istemiyor olabilirsiniz; örneğin, para birimi değerleri için metin kutusundaki para birimi sembolünü düzenlemenin değiştirilmesini istemeyebilirsiniz.)

@No__t-0 özniteliğini kendisi kullanabilirsiniz, ancak `DataType` özniteliğini de kullanmak genellikle iyi bir fikirdir. @No__t-0 özniteliği, verilerin semantiğini bir ekranda nasıl işleneceğini değil ve `DisplayFormat` ile elde olmadığınız avantajları sağlar:

* Tarayıcı HTML5 özelliklerini etkinleştirebilir (örneğin, bir Takvim denetimini, yerel ayara uygun para birimi sembolünü, e-posta bağlantılarını, bazı istemci tarafı giriş doğrulamasını vb. göstermek için).

* Varsayılan olarak tarayıcı, verileri yerel ayarınızı temel alarak doğru biçimi kullanarak işleyebilir.

Daha fazla bilgi için [\<Giriş > etiketi Yardımcısı belgelerine](../../mvc/views/working-with-forms.md#the-input-tag-helper)bakın.

Uygulamayı çalıştırın, öğrenciler dizin sayfasına gidin ve kayıt tarihleri için saatlerin artık gösterilmediğine dikkat edin. Aynı değer öğrenci modelini kullanan herhangi bir görünüm için de geçerli olacaktır.

![Öğrenciler Dizin sayfası tarihleri zamansız gösterme](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>StringLength özniteliği

Ayrıca, öznitelikleri kullanarak veri doğrulama kuralları ve doğrulama hata iletileri de belirtebilirsiniz. @No__t-0 özniteliği, veritabanında en fazla uzunluğu ayarlar ve ASP.NET Core MVC için istemci tarafı ve sunucu tarafı doğrulaması sağlar. Bu öznitelikte en küçük dize uzunluğunu da belirtebilirsiniz, ancak en küçük değerin veritabanı şemasında hiçbir etkisi yoktur.

Kullanıcıların bir ad için 50 ' den fazla karakter girmemesini istediğinizi varsayalım. Bu kısıtlamayı eklemek için, aşağıdaki örnekte gösterildiği gibi, `LastName` ve `FirstMidName` özelliklerine `StringLength` öznitelikleri ekleyin:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

@No__t-0 özniteliği bir kullanıcının ad için boşluk girmesini engellemez. Girişe kısıtlama uygulamak için `RegularExpression` özniteliğini kullanabilirsiniz. Örneğin, aşağıdaki kod ilk karakterin büyük küçük harf olmasını ve geri kalan karakterlerin alfabetik olmasını gerektirir:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

@No__t-0 özniteliği, `StringLength` özniteliğine benzer işlevler sağlar, ancak istemci tarafı doğrulaması sağlamaz.

Veritabanı modeli, veritabanı şemasında değişiklik yapılmasını gerektiren bir şekilde değiştirildi. Uygulama kullanıcı arabirimini kullanarak, veritabanına eklemiş olduğunuz herhangi bir veriyi kaybetmeden Şemayı güncelleştirmek için geçişleri kullanacaksınız.

Değişikliklerinizi kaydedin ve projeyi derleyin. Ardından proje klasöründe komut penceresini açın ve aşağıdaki komutları girin:

```dotnetcli
dotnet ef migrations add MaxLengthOnNames
```

```dotnetcli
dotnet ef database update
```

@No__t-0 komutu, bu değişiklik, iki sütun için en fazla uzunluğu daha kısa hale yaptığından, veri kaybının gerçekleşebileceğini uyarır.  Geçişler *\<zaman damgası > _MaxLengthOnNames. cs*adlı bir dosya oluşturur. Bu dosya, veritabanını geçerli veri modeliyle eşleşecek şekilde güncelleştirecek `Up` yönteminde kod içerir. @No__t-0 komutu bu kodu çalıştırdı.

Geçiş dosyası adının ön eki olan zaman damgası, geçişleri sıralamak için Entity Framework tarafından kullanılır. Update-database komutunu çalıştırmadan önce birden çok geçiş oluşturabilirsiniz ve sonra tüm geçişler oluşturuldukları sırada uygulanır.

Uygulamayı çalıştırın, **öğrenciler** sekmesini seçin, **Yeni oluştur**' a tıklayın ve 50 karakterden daha uzun bir ad girmeyi deneyin. Uygulamanın bunu yapmasını önleyebilmelidir. 

### <a name="the-column-attribute"></a>Column özniteliği

Ayrıca, sınıflarınızın ve özelliklerinin veritabanına nasıl eşlenildiğini denetlemek için özniteliklerini de kullanabilirsiniz. Ad alanı için `FirstMidName` adını kullandığınızı varsayın, çünkü alan de bir orta ad içerebilir. Ancak veritabanına karşı geçici sorgular yazmayacak olan kullanıcılar bu ada alışkın olduğundan, veritabanı sütununun `FirstName` olarak adlandırılması istiyorsunuz. Bu eşlemeyi yapmak için `Column` özniteliğini kullanabilirsiniz.

@No__t-0 özniteliği, veritabanının oluşturulduğu zaman, `FirstMidName` özelliğine eşlenen `Student` tablosunun sütununun `FirstName` olarak adlandırılacağını belirtir. Diğer bir deyişle, kodunuz `Student.FirstMidName` ' a başvurduğunda, veriler `Student` tablosunun `FirstName` sütununda görüntülenir veya güncelleştirilir. Sütun adları belirtmezseniz, bunlar Özellik adı ile aynı ada verilir.

*Student.cs* dosyasında, `System.ComponentModel.DataAnnotations.Schema` için `using` ifadesini ekleyin ve sütun adı özniteliğini aşağıdaki vurgulanmış kodda gösterildiği gibi `FirstMidName` özelliğine ekleyin:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

@No__t-0 özniteliği ekleme modeli, `SchoolContext` ' i yedekleyen bir şekilde değiştirir, bu nedenle veritabanıyla eşleşmez.

Değişikliklerinizi kaydedin ve projeyi derleyin. Ardından proje klasöründe komut penceresini açın ve başka bir geçiş oluşturmak için aşağıdaki komutları girin:

```dotnetcli
dotnet ef migrations add ColumnFirstName
```

```dotnetcli
dotnet ef database update
```

**SQL Server Nesne Gezgini** **, öğrenci tablosuna** çift tıklayarak öğrenci tablosu tasarımcısını açın.

![Geçişlerde SSOX 'teki öğrenciler tablosu](complex-data-model/_static/ssox-after-migration.png)

İlk iki geçişi uygulamadan önce ad sütunları nvarchar (MAX) türünde. Artık nvarchar (50) ve sütun adı FirstMidName iken FirstName olarak değiştirilmiştir.

> [!Note]
> Aşağıdaki bölümlerde tüm varlık sınıflarını oluşturmayı bitirmeden önce derlemeyi denerseniz Derleyici hataları alabilirsiniz.

## <a name="changes-to-student-entity"></a>Öğrenci varlığındaki değişiklikler

![Öğrenci varlığı](complex-data-model/_static/student-entity.png)

*Modeller/öğrenci. cs*' de, daha önce eklediğiniz kodu aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Gerekli öznitelik

@No__t-0 özniteliği, ad özellikleri gereken alanları sağlar. Değer türleri (DateTime, int, Double, float, vb.) gibi null yapılamayan türler için `Required` özniteliği gerekli değildir. Null olmayan türler otomatik olarak gerekli alanlar olarak değerlendirilir.

@No__t-0 özniteliğini kaldırabilir ve `StringLength` özniteliği için minimum length parametresiyle değiştirebilirsiniz:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Display özniteliği

@No__t-0 özniteliği, metin kutularının açıklamalı alt yazısının her örnekteki Özellik adı yerine "Ilk ad", "soyadı", "tam ad" ve "kayıt tarihi" olması gerektiğini belirtir (kelimeleri bir boşluk yoktur).

### <a name="the-fullname-calculated-property"></a>FullName hesaplanmış özelliği

`FullName`, diğer iki özelliği birleştirerek oluşturulmuş bir değer döndüren hesaplanmış bir özelliktir. Bu nedenle, yalnızca bir get erişimcisine sahiptir ve veritabanında `FullName` sütunu oluşturulmaz.

## <a name="create-instructor-entity"></a>Eğitmen varlığı oluşturma

![Eğitmen varlığı](complex-data-model/_static/instructor-entity.png)

Şablon kodunu aşağıdaki kodla değiştirerek *modeller/eğitmen. cs*oluşturun:

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Öğrenci ve eğitmen varlıklarında birçok özellik aynı olduğuna dikkat edin. Devralma öğreticisini bu serinin ilerleyen kısımlarında [uyguladığınızda](inheritance.md) , artıklığı ortadan kaldırmak için bu kodu yeniden düzenlemelisiniz.

Birden çok özniteliği tek bir satıra koyabilirsiniz, bu nedenle `HireDate` özniteliklerini aşağıdaki gibi yazabilirsiniz:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>Courseatamalar ve OfficeAssignment gezinti özellikleri

@No__t-0 ve `OfficeAssignment` özellikleri gezinti özellikleridir.

Bir eğitmen herhangi bir sayıda kurs öğretebilir, bu nedenle `CourseAssignments` bir koleksiyon olarak tanımlanır.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Bir gezinti özelliği birden çok varlık tukiyorsa, türü girişlerin eklenebileceği, silinebileceği ve güncelleştirilemeyebilir bir liste olmalıdır.  @No__t-0 veya `List<T>` veya `HashSet<T>` gibi bir tür belirtebilirsiniz. @No__t-0 belirtirseniz, EF varsayılan olarak bir `HashSet<T>` koleksiyonu oluşturur.

Bunların `CourseAssignment` varlıklar olmasının nedeni, çoktan çoğa ilişkiler hakkında bölümünde aşağıda açıklanmıştır.

Contoso Üniversitesi iş kuralları, bir eğitmenin yalnızca en fazla bir ofise sahip olabileceğini, bu nedenle `OfficeAssignment` özelliği tek bir OfficeAssignment varlığı (Office atanmamışsa null olabilir) bulundurmasıdır.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-officeassignment-entity"></a>OfficeAssignment varlığı oluştur

![OfficeAssignment varlığı](complex-data-model/_static/officeassignment-entity.png)

Aşağıdaki kodla *modeller/OfficeAssignment. cs* oluşturun:

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Anahtar özniteliği

Eğitmen ve OfficeAssignment varlıkları arasında bire sıfır veya-bir ilişki vardır. Office ataması, atandığı eğitmenle ilişkili olarak yalnızca kendi birincil anahtarı da eğitmen varlığına ait yabancı anahtarıdır. Ancak Entity Framework, adı ID veya Classnameıd adlandırma kuralını izlemediği için bu varlığın birincil anahtarı olarak Komutctorıd 'yi otomatik olarak tanıyamaz. Bu nedenle, `Key` özniteliği onu anahtar olarak tanımlamak için kullanılır:

```csharp
[Key]
public int InstructorID { get; set; }
```

Varlık kendi birincil anahtarına sahip olsa da, özelliği Classnameıd veya ID dışında bir şey olarak adlandırmak istiyorsanız `Key` özniteliğini de kullanabilirsiniz.

Varsayılan olarak, tam olarak, sütun tanımlayıcı bir ilişki için olduğundan EF, anahtarı veritabanı olmayan bir şekilde değerlendirir.

### <a name="the-instructor-navigation-property"></a>Eğitmen gezintisi özelliği

Eğitmen varlığı, null yapılabilir bir `OfficeAssignment` gezinti özelliğine sahiptir (bir eğitmenin bir Office ataması olmayabilir) ve OfficeAssignment varlığı null atanamaz bir `Instructor` gezinti özelliğine sahiptir (çünkü bir Office ataması, bir olmadan eğitmen--`InstructorID` null atanamaz). Bir eğitmen varlığı ilgili bir OfficeAssignment varlığına sahip olduğunda, her varlığın gezinti özelliğinde diğer birine bir başvurusu olur.

İlgili bir eğitmen olması gerektiğini belirtmek için, eğitmen gezinti özelliğine bir `[Required]` özniteliği koyabilirsiniz, ancak bunu yapmak zorunda kalmazsınız çünkü bu, `InstructorID` yabancı anahtarı (aynı zamanda bu tablonun anahtarı) null değer atanamaz.

## <a name="modify-course-entity"></a>Kurs varlığını değiştirme

![Kurs varlığı](complex-data-model/_static/course-entity.png)

*Modeller/kurs. cs*' de, daha önce eklediğiniz kodu aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

Kurs varlığı, ilişkili departman varlığına işaret eden ve bir `Department` gezinti özelliğine sahip olan `DepartmentID` yabancı anahtar özelliğine sahiptir.

Entity Framework, ilgili varlık için bir gezinti özelliği olduğunda, veri modelinize yabancı anahtar özelliği eklemenizi gerektirmez.  EF, gerektiğinde otomatik olarak yabancı anahtarlar oluşturur ve bunlar için [gölge Özellikler](/ef/core/modeling/shadow-properties) oluşturur. Ancak veri modelinde yabancı anahtar olması, güncelleştirmeleri daha basit ve daha verimli hale getirir. Örneğin, düzenlemek üzere bir kurs varlığı aldığınızda, bunu yüklemezseniz departman varlığı null olur, bu nedenle kurs varlığını güncelleştirdiğinizde öncelikle departman varlığını almanız gerekir. @No__t-0 yabancı anahtar özelliği veri modeline dahil edildiğinde, güncelleştirmeden önce bölüm varlığını almanız gerekmez.

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated özniteliği

@No__t-2 özelliğindeki `None` parametresine sahip `DatabaseGenerated` özniteliği, birincil anahtar değerlerinin veritabanı tarafından oluşturulması yerine Kullanıcı tarafından sağlandığını belirtir.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Varsayılan olarak, Entity Framework birincil anahtar değerlerinin veritabanı tarafından oluşturulduğunu varsayar. Bu, Çoğu senaryoda istediğiniz şeydir. Ancak, kurs varlıkları için bir departman için 1000 serisi, başka bir departman için 2000 serisi vb. gibi kullanıcı tarafından belirtilen bir kurs numarası kullanırsınız.

@No__t-0 özniteliği, bir satırın oluşturulduğu veya güncelleştirildiği tarihi kaydetmek için kullanılan veritabanı sütunları durumunda olduğu gibi varsayılan değerleri oluşturmak için de kullanılabilir.  Daha fazla bilgi için bkz. [üretilen Özellikler](/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Kurs varlığındaki yabancı anahtar özellikleri ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:

Bir kurs bir departmana atanır, bu nedenle yukarıda bahsedilen nedenlerle `DepartmentID` yabancı anahtar ve `Department` gezinme özelliği vardır.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Bir kurs, kayıtlı sayıda öğrenciye sahip olabilir, bu nedenle `Enrollments` gezinti özelliği bir koleksiyondur:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Bir kurs birden fazla eğitmen tarafından tada olabilir, bu nedenle `CourseAssignments` gezinti özelliği bir koleksiyon (`CourseAssignment` türü [daha sonra](#many-to-many-relationships)açıklanmaktadır):

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-department-entity"></a>Departman varlığı oluştur

![Bölüm varlığı](complex-data-model/_static/department-entity.png)

Aşağıdaki kodla *modeller/departman. cs* oluşturun:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Column özniteliği

Daha önce sütun adı eşlemesini değiştirmek için `Column` özniteliğini kullandınız. Bölüm varlığının kodunda, sütunun veritabanındaki SQL Server para türü kullanılarak tanımlanması için SQL veri türü eşlemesini değiştirmek üzere `Column` özniteliği kullanılır:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Entity Framework, özellik için tanımladığınız CLR türüne göre uygun SQL Server veri türünü seçtiği için sütun eşlemesi genellikle gerekli değildir. CLR `decimal` türü bir SQL Server `decimal` türüne eşlenir. Ancak bu durumda, sütunun para birimi tutarlarını tutuını ve para veri türü bunun için daha uygun olduğunu bilirsiniz.

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Yabancı anahtar ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:

Bir departman yönetici olabilir veya olmayabilir ve yönetici her zaman bir eğitmendir. Bu nedenle `InstructorID` özelliği, eğitmen varlığına yabancı anahtar olarak dahil edilir ve özelliği null yapılabilir olarak işaretlemek için `int` tür atamadan sonra bir soru işareti eklenir. Gezinti özelliği `Administrator` olarak adlandırılmıştır, ancak bir eğitmen varlığı tutar:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Bir departmanın birçok kursu olabilir, bu nedenle bir kurs gezintisi özelliği vardır:

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> Kurala göre Entity Framework, null olamayan yabancı anahtarlar ve çoktan çoğa ilişkiler için art arda silme imkanı sağlar. Bu, bir geçiş eklemeye çalıştığınızda bir özel duruma neden olacak dairesel basamaklı silme kurallarına neden olabilir. Örneğin, Department. Komutctorıd özelliğini null yapılabilir olarak tanımlamadıysanız, bu, bir basamak silme kuralını, eğitmeni sildiğinizde, ne yapmak istediğinize ilişkin olmayan bir şeyi silmek için yapılandırır. İş kurallarınız `InstructorID` özelliğini null değer atanamaz olarak gerektiriyorsa, ilişkide Cascade silmeyi devre dışı bırakmak için aşağıdaki Fluent API ifadesini kullanmanız gerekir:
>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-enrollment-entity"></a>Kayıt varlığını değiştirme

![Kayıt varlığı](complex-data-model/_static/enrollment-entity.png)

*Modeller/kayıt. cs*' de, daha önce eklediğiniz kodu aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Yabancı anahtar ve gezinti özellikleri

Yabancı anahtar özellikleri ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:

Kayıt kaydı tek bir kurs için olduğundan @no__t 0 yabancı anahtar özelliği ve `Course` gezinti özelliği vardır:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Kayıt kaydı tek bir öğrenci içindir, bu nedenle `StudentID` yabancı anahtar özelliği ve `Student` gezinti özelliği vardır:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Çoktan çoğa ilişkiler

Öğrenci ve kurs varlıkları arasında çok-çok ilişkisi vardır ve kayıt varlığı, veritabanında *Yük içeren* çoktan çoğa bir JOIN tablosu gibi çalışır. "Yük ile", kayıt tablosunun birleştirilmiş tablolar için yabancı anahtarlar (Bu durumda bir birincil anahtar ve bir sınıf özelliği) yanında ek veriler içerdiği anlamına gelir.

Aşağıdaki çizimde bu ilişkilerin bir varlık diyagramında nasıl göründüğünü gösterilmektedir. (Bu diyagram EF 6. x için Entity Framework güç araçları kullanılarak oluşturulmuştur; diyagramı oluşturmak öğreticinin bir parçası değildir, burada yalnızca bir çizim olarak kullanılmaktadır.)

![Öğrenci-çok fazla ilişki](complex-data-model/_static/student-course.png)

Her ilişki satırında 1 bir sonda ve diğeri de bir yıldız işareti (*) bulunur. Bu, bire çok ilişkiyi belirtir.

Kayıt tablosu, sınıf bilgilerini içermiyorsa, yalnızca iki yabancı anahtar olan CourseID ve Studentitıd 'yi içermelidir. Bu durumda, veritabanına yük (veya saf bir JOIN tablosu) olmadan çok-çok arasında bir JOIN tablosu olacaktır. Eğitmen ve kurs varlıklarının bu tür çok-çok ilişkisi vardır ve bir sonraki adımınız, yük olmadan bir JOIN tablosu olarak çalışacak bir varlık sınıfı oluşturmaktır.

(EF 6. x, çoktan çoğa ilişkiler için örtük birleştirmeyi destekler, ancak EF Core. Daha fazla bilgi için, [EF Core GitHub deposundaki tartışmaya](https://github.com/aspnet/EntityFramework/issues/1368)bakın.)

## <a name="the-courseassignment-entity"></a>Courseatama varlığı

![Courseatama varlığı](complex-data-model/_static/courseassignment-entity.png)

Aşağıdaki kodla *modeller/Courseatama. cs* oluşturun:

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a>Varlık adlarını Birleştir

Eğitim--çok ilişkisi için veritabanında bir JOIN tablosu gereklidir ve bir varlık kümesi tarafından temsil edilir. @No__t-0 ' a bir JOIN varlığının adı, bu örnekte `CourseInstructor` olacak. Ancak, ilişkiyi açıklayan bir ad seçmenizi öneririz. Veri modelleri, daha sonra yükleri daha sonra almak için, yük olmayan birleşimler olmadan basit ve büyümeye başlar. Açıklayıcı bir varlık adıyla başladıysanız, daha sonra adı değiştirmeniz gerekmez. İdeal olarak, JOIN varlığının iş etki alanında kendi doğal (muhtemelen tek bir kelime) adına sahip olması gerekir. Örneğin, kitaplar ve müşteriler derecelendirmeler aracılığıyla bağlanabilir. Bu ilişki için `CourseAssignment` `CourseInstructor` ' den daha iyi bir seçimdir.

### <a name="composite-key"></a>Bileşik anahtar

Yabancı anahtarlar null değer atanmadığından ve tablodaki her satırı benzersiz bir şekilde tanımladığından, ayrı bir birincil anahtar gerekmez. *Komutctorıd* ve *CourseID* özellikleri bir bileşik birincil anahtar olarak çalışır. EF için bileşik birincil anahtarları tanımlamanın tek yolu *Fluent API* kullanmaktır (öznitelikleri kullanılarak gerçekleştirilemez). Bir sonraki bölümde bileşik birincil anahtarı nasıl yapılandıracağınızı göreceksiniz.

Bileşik anahtar, tek bir kurs için birden çok satır ve bir eğitmen için birden fazla satır, aynı eğitmen ve kurs için birden çok satıra sahip olmanızı sağlar. @No__t-0 JOIN varlığı kendi birincil anahtarını tanımlar, bu nedenle bu sıralamanın yinelemeleri mümkündür. Bu tür yinelemeleri engellemek için yabancı anahtar alanlarına benzersiz bir dizin ekleyebilir veya `Enrollment` ' ı `CourseAssignment` ' e benzer bir birincil bileşik anahtarla yapılandırabilirsiniz. Daha fazla bilgi için bkz. [dizinler](/ef/core/modeling/indexes).

## <a name="update-the-database-context"></a>Veritabanı bağlamını güncelleştirme

*Data/SchoolContext. cs* dosyasına aşağıdaki vurgulanmış kodu ekleyin:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Bu kod yeni varlıkları ekler ve Courseatama varlığının bileşik birincil anahtarını yapılandırır.

## <a name="about-a-fluent-api-alternative"></a>Fluent API alternatifi hakkında

@No__t-1 sınıfının `OnModelCreating` yöntemindeki kod, EF davranışını yapılandırmak için *Fluent API* kullanır. Bu örnekte, [EF Core belgelerinden](/ef/core/modeling/#use-fluent-api-to-configure-a-model)farklı olarak, bir dizi yöntemi çağıran tek bir bildirimde dize tarafından KULLANıLDıĞıNDAN, API "floent" olarak adlandırılır:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

Bu öğreticide, yalnızca öznitelikleri ile yapaamıyoruz veritabanı eşlemesi için Fluent API kullanıyorsunuz. Ancak, özniteliklerini kullanarak yapabileceğiniz biçimlendirme, doğrulama ve eşleme kurallarının çoğunu belirtmek için Fluent API de kullanabilirsiniz. @No__t-0 gibi bazı öznitelikler Fluent API uygulanamaz. Daha önce belirtildiği gibi, `MinimumLength` şemayı değiştirmez, yalnızca bir istemci ve sunucu tarafı doğrulama kuralı uygular.

Bazı geliştiriciler, varlık sınıflarının "temiz" olmasını sağlamak için Fluent API özel olarak kullanmayı tercih eder. İsterseniz öznitelikleri ve Fluent API karıştırabilir ve yalnızca Fluent API kullanılarak gerçekleştirilebilecek bazı özelleştirmeler de vardır ancak genel olarak önerilen uygulama, bu iki yaklaşımdan birini seçmek ve bunları mümkün olduğunca tutarlı bir şekilde kullanmaktır. Her ikisini de kullanırsanız, bir çakışma olduğunda, akıcı API 'nin öznitelikleri geçersiz kıldığını unutmayın.

Öznitelikler ile Fluent API hakkında daha fazla bilgi için bkz. [yapılandırma yöntemleri](/ef/core/modeling/).

## <a name="entity-diagram-showing-relationships"></a>Ilişkileri gösteren varlık diyagramı

Aşağıdaki çizimde, Entity Framework Power Tools 'un tamamlanmış okul modeli için kullandığı diyagram gösterilmektedir.

![Varlık diyagramı](complex-data-model/_static/diagram.png)

Bire çok ilişki çizgilerinin yanı sıra (1 ila \*), burada, eğitmen ve OfficeAssignment varlıkları ile sıfır veya-bire çok ilişki satırı (1 ila 0.. 1) arasında bir ilişki satırını (0.. 1 ile *) Eğitmen ve departman varlıkları.

## <a name="seed-database-with-test-data"></a>Test verileriyle çekirdek veritabanı

*Veri/Dbınınitializer. cs* dosyasındaki kodu, oluşturduğunuz yeni varlıklar için çekirdek verileri sağlamak üzere aşağıdaki kodla değiştirin.

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

İlk öğreticide gördüğünüz gibi, bu kodun çoğu yalnızca yeni varlık nesneleri oluşturur ve test için gereken şekilde, örnek verileri özelliklere yükler. Çoktan çoğa ilişkilerin nasıl işlendiği hakkında dikkat edin: kod, `Enrollments` ve `CourseAssignment` ' i birleştirmek varlık kümelerinde varlık oluşturarak ilişkiler oluşturur.

## <a name="add-a-migration"></a>Geçiş Ekle

Değişikliklerinizi kaydedin ve projeyi derleyin. Ardından proje klasöründe komut penceresini açın ve `migrations add` komutunu girin (henüz Update-database komutunu yapmayın):

```dotnetcli
dotnet ef migrations add ComplexDataModel
```

Olası veri kaybı hakkında bir uyarı alırsınız.

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Bu noktada `database update` komutunu çalıştırmayı denediyseniz (henüz yapmayın), şu hatayı alırsınız:

> ALTER TABLE ifadesi, "FK_dbo" adlı yabancı anahtar kısıtlaması ile çakışıyor. Course_dbo. Department_DepartmentID". "ContosoUniversity" veritabanında, "dbo" tablosunda çakışma oluştu. Bölüm ", sütun ' DepartmentID '.

Bazı durumlarda, mevcut verilerle geçişleri yürüttüğünüzde, yabancı anahtar kısıtlamalarını karşılamak için saplama verilerini veritabanına eklemeniz gerekir. @No__t-0 yönteminde oluşturulan kod, kurs tablosuna null yapılamayan bir DepartmentID yabancı anahtarı ekler. Kurs tablosunda kod çalıştırıldığında zaten satırlar varsa, SQL Server null olmayan sütuna hangi değerin yerleştirileceğini bilmediği için `AddColumn` işlemi başarısız olur. Bu öğreticide, geçişi yeni bir veritabanında çalıştıracaksınız, ancak bir üretim uygulamasında geçiş, mevcut verileri işleyeceğinizden, bu sayede bunun nasıl yapılacağını gösteren bir örnek gösterilmektedir.

Bu geçişi mevcut verilerle birlikte çalışarak, yeni sütuna varsayılan bir değer vermek için kodu değiştirmeniz ve varsayılan departman görevi gören "Temp" adlı bir saplama departmanı oluşturmanız gerekir. Sonuç olarak, `Up` yöntemi çalıştıktan sonra, mevcut kurs satırları "geçici" departmanla ilgili olacaktır.

* *{Timestamp} _ComplexDataModel. cs* dosyasını açın.

* DepartmentID sütununu kurs tablosuna ekleyen kod satırını açıklama satırı yapın.

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* Departman tablosunu oluşturan koddan sonra aşağıdaki vurgulanmış kodu ekleyin:

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

Bir üretim uygulamasında, departman satırları eklemek ve kurs satırlarını yeni departman satırlarıyla ilişkilendirmek için kod veya komut dosyaları yazın. Bundan sonra "geçici" Departmanı veya kurs. DepartmentID sütununda Varsayılan değer gerekmez.

Değişikliklerinizi kaydedin ve projeyi derleyin.

## <a name="change-the-connection-string"></a>Bağlantı dizesini değiştirme

Artık `DbInitializer` sınıfında, yeni varlıklar için çekirdek verileri boş bir veritabanına ekleyen yeni kodunuz var. EF 'in yeni boş bir veritabanı oluşturmasını sağlamak için *appSettings. JSON* içindeki bağlantı dizesinde veritabanının adını ContosoUniversity3 veya kullandığınız bilgisayarda kullanmadığınız başka bir adla değiştirin.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

Değişiklerinizi *appSettings. JSON*' da kaydedin.

> [!NOTE]
> Veritabanı adını değiştirmeye alternatif olarak, veritabanını silebilirsiniz. **SQL Server Nesne Gezgini** (ssox) veya `database drop` CLI komutunu kullanın:
>
> ```dotnetcli
> dotnet ef database drop
> ```

## <a name="update-the-database"></a>Veritabanını güncelleştirme

Veritabanı adını değiştirdikten veya veritabanını sildikten sonra, geçişleri yürütmek için komut penceresinde `database update` komutunu çalıştırın.

```dotnetcli
dotnet ef database update
```

@No__t-0 yönteminin çalışmasına ve yeni veritabanını doldurmasına neden olacak şekilde uygulamayı çalıştırın.

Veritabanını daha önce yaptığınız gibi SSOX içinde açın ve tabloların tümünün oluşturulduğunu görmek için **Tables** düğümünü genişletin. (Yine de bir daha önceki zamanda SSOX açıksa **Yenile** düğmesine tıklayın.)

![SSOX içindeki tablolar](complex-data-model/_static/ssox-tables.png)

Veritabanını gösteren Başlatıcı kodunu tetiklemek için uygulamayı çalıştırın.

**Courseatama** tablosuna sağ tıklayın ve verileri **görüntüle** ' yi seçerek veri içerdiğinden emin olun.

![SSOX 'te Courseatama verileri](complex-data-model/_static/ssox-ci-data.png)

## <a name="get-the-code"></a>Kodu edinin

[Tamamlanmış uygulamayı indirin veya görüntüleyin.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Veri modeli özelleştirildi
> * Öğrenci varlığında değişiklikler yapıldı
> * Eğitmen varlığı oluşturuldu
> * OfficeAssignment varlığı oluşturuldu
> * Değiştirilen kurs varlığı
> * Departman varlığı oluşturuldu
> * Değiştirilen kayıt varlığı
> * Veritabanı bağlamı güncelleştirildi
> * Test verileriyle birlikte sağlanan veritabanı
> * Geçiş eklendi
> * Bağlantı dizesi değiştirildi
> * Veritabanı güncelleştirildi

İlgili verilere erişme hakkında daha fazla bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Sonraki: ilgili verilere erişin](read-related-data.md)
