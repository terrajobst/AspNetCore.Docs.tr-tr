---
title: "Razor sayfalarının Entity Framework Çekirdek - 8'in Öğreticisi 1 ile"
author: rick-anderson
description: "Entity Framework Çekirdek kullanarak Razor sayfalarının uygulamasının nasıl oluşturulacağını gösterir"
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/intro
ms.openlocfilehash: 091f34da347d52ba8e3e87779ddc4aeb790c2800
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2018
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a>Razor sayfalarının ve Entity Framework Visual Studio (1 / 8) kullanarak çekirdek ile çalışmaya başlama

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması Entity Framework (EF) çekirdek 2.0 ve Visual Studio 2017 kullanarak ASP.NET Core 2.0 MVC web uygulamalarının nasıl oluşturulacağını gösterir.

Örnek uygulaması, kurgusal bir Contoso üniversite için bir web sitesidir. Öğrenci giriş, indirmelere oluşturma ve Eğitmen atamaları gibi işlevselliği içerir. Contoso University örnek uygulamasının nasıl oluşturulacağını açıklayan eğitim serileri ilk sayfasıdır.

[İndirme veya tamamlanan uygulama görüntüleyin.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Yükleme yönergeleri](xref:tutorials/index#how-to-download-a-sample).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

Aşina [Razor sayfalarının](xref:mvc/razor-pages/index). Yeni programcıları tamamlamanız gereken [Razor sayfalarının ile çalışmaya başlama](xref:tutorials/razor-pages/razor-pages-start) bu seri başlatmadan önce.

## <a name="troubleshooting"></a>Sorun giderme

Olamaz çözmek bir sorunla çalıştırırsanız, genellikle çözümün kodunuzu karşılaştırarak bulabilirsiniz [tamamlandı aşama](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) veya [projeyi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final). Sık karşılaşılan hataları ve bunları çözmek nasıl listesi için bkz: [serideki son Öğreticisi sorun giderme bölümüne](xref:data/ef-mvc/advanced#common-errors). Var. gerekenler bulamazsanız, bir soru nakledebilirsiniz [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) için [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF çekirdek](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP]
> Bu öğreticiler dizi önceki eğitimlerine bitti üzerinde oluşturur. Her başarılı öğretici tamamlandıktan sonra projeyi bir kopyasını kaydetme göz önünde bulundurun. Sorunlarla karşılaşırsanız, başlangıcına geri dönerseniz yerine önceki öğreticiden üzerinden başlatabilirsiniz. Alternatif olarak, karşıdan yükleyebileceğiniz bir [tamamlandı aşama](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) ve tamamlanmış aşama kullanarak üzerinden başlatın.

## <a name="the-contoso-university-web-app"></a>Contoso University web uygulaması

Bu öğreticiler oluşturulmuş uygulamaya bir temel university web sitesidir.

Kullanıcıların görüntüleyebileceği ve Öğrenci, indirmelere ve Eğitmen bilgileri güncelleştirin. Öğreticide oluşturulan ekranlar bazılarını aşağıda verilmiştir.

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

![Öğrenciler Düzenle sayfası](intro/_static/student-edit.png)

Bu sitenin UI Stili yerleşik şablonlar tarafından oluşturulan yakın ' dir. Razor sayfalarının, UI EF çekirdek üzerinde öğretici odak noktasıdır.

## <a name="create-a-razor-pages-web-app"></a>Bir Razor sayfalarının web uygulaması oluşturma

* Visual Studio'dan **dosya** menüsünde, select **yeni** > **proje**.
* Yeni bir ASP.NET çekirdek Web uygulaması oluşturun. Proje adı **ContosoUniversity**. Proje adı önemlidir *ContosoUniversity* kod kopyalanıp yapıştırılmış ad alanları eşleştirmek için.
 ![Yeni ASP.NET çekirdek Web uygulaması](intro/_static/np.png)
* Seçin **ASP.NET Core 2.0** açılır ve ardından **Web uygulaması**.
 ![Web uygulaması (Razor sayfalarının)](../../mvc/razor-pages/index/_static/np2.png)

Tuşuna **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** hata ayıklayıcı eklemeden çalıştırmak için

## <a name="set-up-the-site-style"></a>Site stil ayarlama

Birkaç değişiklik sitesi menüsü, Düzen ve giriş sayfası ayarlayın.

Açık *Pages/_Layout.cshtml* ve aşağıdaki değişiklikleri yapın:

* "Contoso University." "ContosoUniversity" her oluşumu değiştirme Üç oluşum vardır.

* Menü girişlerini eklemek **Öğrenciler**, **kurslar**, **Eğitmen**, ve **Departmanlar**ve silme **kişi** menü girişi.

Değişiklikler vurgulanır. (Tüm biçimlendirme *değil* görüntülenir.)

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

İçinde *Pages/Index.cshtml*, dosyanın içeriğini bu uygulama hakkında metinle ASP.NET MVC hakkında metni değiştirmek için aşağıdaki kod ile değiştirin:

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

Projeyi çalıştırmak için CTRL + F5 tuşuna basın. Giriş sayfası aşağıdaki öğreticilerde oluşturulan sekmelerle görüntülenir:

![Contoso University giriş sayfası](intro/_static/home-page.png)

## <a name="create-the-data-model"></a>Veri modeli oluşturma

Sınıflar Contoso University uygulama oluşturun. Aşağıdaki üç varlıklarıyla başlatın:

![İndirmelere kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

Bir-çok ilişkisi arasında `Student` ve `Enrollment` varlıklar. Bir-çok ilişkisi arasında `Course` ve `Enrollment` varlıklar. Öğrencinin herhangi bir sayıda kurslar kaydedebilir. Bir indirmelere içinde kayıtlı Öğrenciler herhangi bir sayıda olabilir.

Aşağıdaki bölümlerde, bu varlıkların her biri için bir sınıf oluşturulur.

### <a name="the-student-entity"></a>Öğrenci varlık

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

Oluşturma bir *modelleri* klasör. İçinde *modelleri* klasörünü adlı bir sınıf dosyası oluşturma *Student.cs* aşağıdaki kod ile:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID` Özelliği bu sınıfa karşılık gelen veritabanı (DB) tablosunun birincil anahtar sütunu haline gelir. Varsayılan olarak, EF çekirdek adlı bir özellik yorumlar `ID` veya `classnameID` birincil anahtar olarak.

`Enrollments` Özelliği bir gezinti özelliğidir. Bu varlığa ilgili diğer varlıklar için Gezinti özellikleri bağlayın. Bu durumda, `Enrollments` özelliği bir `Student entity` tüm tutan `Enrollment` için ilişkili olan varlıkların `Student`. Örneğin, bir öğrenci satır varsa DB iki kayıt, ilişkili satırları sahip `Enrollments` gezinti özelliği içerdiğinden bu iki `Enrollment` varlıklar. İlgili `Enrollment` satırıdır bu öğrencinin birincil anahtar değerini içeren bir satır `StudentID` sütun. Örneğin, Öğrenci kimlikli varsayalım = 1 sahip iki satır `Enrollment` tablo. `Enrollment` Tablolu sahip iki satır `StudentID` = 1. `StudentID`bir yabancı anahtar `Enrollment` Öğrenci belirtir tablo `Student` tablo.

Bir gezinme özelliği birden çok varlık tutarsanız gezinti özelliği bir liste türü gibi olmalıdır `ICollection<T>`. `ICollection<T>`belirtilebilir, ya da bir türü `List<T>` veya `HashSet<T>`. Zaman `ICollection<T>` olduğu EF çekirdek oluşturur kullanıldığında, bir `HashSet<T>` varsayılan koleksiyon. Birden çok varlık tutun Gezinti özellikleri çok- ve -çok ilişkileri gelir.

### <a name="the-enrollment-entity"></a>Kayıt varlık

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

İçinde *modelleri* klasörü oluşturmak *Enrollment.cs* aşağıdaki kod ile:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` Birincil anahtarı bir özelliktir. Bu varlığı kullanan `classnameID` yerine desen `ID` gibi `Student` varlık. Genellikle geliştiriciler bir deseni seçin ve veri modelini kullanır. Sonraki öğreticide, veri modelinde devralma uygulamak daha kolay classname Kimliğini kullanarak gösterilir.

`Grade` Özelliği bir `enum`. Soru işareti sonra `Grade` türü bildirimi gösterir `Grade` özelliği boş değer atanabilir. Null bir düzeyde bir sıfır ataması farklı.--null anlamına gelir bir düzeyde bilinen değil veya henüz atanmamış.

`StudentID` Özelliği bir yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`. Bir `Enrollment` varlıktır biriyle ilişkili `Student` tek bir özelliği içerecek şekilde varlık `Student` varlık. `Student` Varlık farklı olarak `Student.Enrollments` birden çok içeren gezinti özelliği `Enrollment` varlıklar.

`CourseID` Özelliği bir yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`. Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.

EF çekirdek, ise bu özellik yabancı anahtar olarak yorumlar `<navigation property name><primary key property name>`. Örneğin,`StudentID` için `Student` gezinti özelliği, bu yana `Student` varlığın birincil anahtarının `ID`. Yabancı anahtar özellikleri de adlı `<primary key property name>`. Örneğin, `CourseID` beri `Course` varlığın birincil anahtarının `CourseID`.

### <a name="the-course-entity"></a>İndirmelere varlık

![İndirmelere varlık diyagramı](intro/_static/course-entity.png)

İçinde *modelleri* klasörü oluşturmak *Course.cs* aşağıdaki kod ile:

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments` Özelliği bir gezinti özelliğidir. A `Course` varlık herhangi bir sayıda için ilgili olabileceğini `Enrollment` varlıklar.

`DatabaseGenerated` DB sahip, oluşturmak, yerine özniteliği birincil anahtarı belirtmek uygulama izin verir.

## <a name="create-the-schoolcontext-db-context"></a>SchoolContext DB bağlamı oluşturma

Verilen veri modeli için EF temel işlevleri koordinatları ana sınıfı DB bağlamı sınıftır. Veri bağlamı türetilir `Microsoft.EntityFrameworkCore.DbContext`. Veri bağlamı hangi varlıkların veri modelinde dahil edildiğini belirtir. Bu projede adlı sınıfı `SchoolContext`.

Proje klasöründe adlı bir klasör oluşturun *veri*.

İçinde *veri* klasör oluşturma *SchoolContext.cs* aşağıdaki kod ile:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Bu kod oluşturur bir `DbSet` özelliği her bir varlık kümesi. EF çekirdek terminolojisinde:

* Bir varlık kümesine genellikle DB tabloya karşılık gelir.
* Bir varlık tablosunda bir satırı karşılık gelir.

`DbSet<Enrollment>`ve `DbSet<Course>` atlanabilir. EF çekirdek içerir bunları örtük olarak çünkü `Student` varlık başvuruları `Enrollment` varlık ve `Enrollment` varlık başvuruları `Course` varlık. Bu öğretici için tutmak `DbSet<Enrollment>` ve `DbSet<Course>` içinde `SchoolContext`.

DB oluşturulduğunda EF çekirdek aynı adlara sahip tablolar oluşturur `DbSet` özellik adları. Koleksiyonlar için özellik adları genellikle çoğul (Öğrenciler yerine Öğrenci). Geliştiriciler olup tablo adlarının çoğul olmalıdır hakkında katılmıyorum. Bu öğreticileri için DbContext tekil tablo adları belirterek varsayılan davranışı geçersiz kılınır. Tekil tablo adları belirtmek için aşağıdaki vurgulanmış kodu ekleyin:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Bağlam bağımlılık ekleme ile kaydetme

ASP.NET Core içeren [bağımlılık ekleme](xref:fundamentals/dependency-injection). Hizmetleri (örneğin, EF çekirdek DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu Hizmetleri (örneğin, Razor sayfalarının) gerektiren bileşenler bu hizmetlere Oluşturucu parametreleri yoluyla sağlanır. Bir db bağlamı örneği alır Oluşturucusu kodu daha sonra öğreticide gösterilir.

Kaydetmek için `SchoolContext` bir hizmet olarak açmak *haline*, vurgulanan satırlar ekleyin `ConfigureServices` yöntemi.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

Bağlantı dizesinin adını bağlamına üzerinde bir yöntemini çağırarak geçirilen bir `DbContextOptionsBuilder` nesnesi. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.

Ekleme `using` deyimleri için `ContosoUniversity.Data` ve `Microsoft.EntityFrameworkCore` ad alanları. Projeyi oluşturun.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Açık *appsettings.json* dosyası ve bir bağlantı dizesi aşağıdaki kodda gösterildiği gibi ekleyin:

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

Önceki bağlantı dizesini kullanır `ConnectRetryCount=0` önlemek için [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) asılı gelen.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Bağlantı dizesini bir SQL Server yerel veritabanı DB belirtir. Yerel veritabanı, SQL Server Express veritabanı Motoru'nu hafif sürümüdür ve uygulama geliştirme, üretim kullanımı için amaçlanmıştır. Yerel veritabanı isteğe bağlı olarak başlar ve bu yüzden karmaşık yapılandırma kullanıcı modunda çalışır. Varsayılan olarak, yerel veritabanı oluşturur *.mdf* DB dosyaları `C:/Users/<user>` dizin.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>DB test verileri ile başlatmak için kod ekleme

EF çekirdek boş bir veritabanı oluşturur. Bu bölümde, bir *çekirdek* yöntemi test veri ile doldurulacak yazılır.

İçinde *veri* klasörünü adlı yeni bir sınıf dosyası oluşturma *DbInitializer.cs* ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

Kod DB'de herhangi Öğrenciler olup olmadığını denetler. DB içinde hiçbir Öğrenciler varsa, DB test verilerle sağlanmış. Diziye test verileri yükler yerine `List<T>` performansı iyileştirmek için koleksiyonları.

`EnsureCreated` Yöntemi DB DB bağlamı için otomatik olarak oluşturur. Veritabanı varsa `EnsureCreated` DB değiştirmeden döndürür.

İçinde *Program.cs*, değişiklik `Main` yöntemi aşağıdakileri yapın:

* Bir DB bağlamı örneği bağımlılık ekleme kapsayıcıdan alın.
* İçin bağlamı geçirme çekirdek yöntemini çağırın.
* Seed yöntemi tamamlandığında bağlamını siler.

Aşağıdaki kod güncelleştirilmiş gösterir *Program.cs* dosya.

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

Uygulama bir ilk çalıştırıldığında DB oluşturulur ve test verilerle sağlanmış. Veri modeli güncelleştirildiğinde:
* DB silin.
* Seed yöntemi güncelleştirin.
* Uygulamayı çalıştırın ve yeni bir kapsanan veritabanı oluşturulur. 

Sonraki öğreticileri, veri değişiklikleri, silme ve DB yeniden oluşturma modeli DB güncelleştirilir.

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a>İskele araçları Ekle

Bu bölümde, Visual Studio web kod oluşturma paketi eklemek için Paket Yöneticisi Konsolu (PMC) kullanılır. Bu paket, yapı iskelesi altyapısı çalıştırmak için gereklidir.

Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.

Paket Yöneticisi Konsolu (PMC)'da, aşağıdaki komutları girin:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

Önceki komutu NuGet paketlerini *.csproj dosyasına ekler:

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a>İskele modeli

* Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).
* Aşağıdaki komutları çalıştırın:


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
Aşağıdaki hata oluşturulursa:

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

Komutu yeniden çalıştırın ve sayfanın sonundaki bir yorum yazın.

Hatayı alırsanız:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).


Projeyi oluşturun. Derleme hataları aşağıdaki gibi oluşturur:

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 Genel olarak değiştirmek `_context.Student` için `_context.Students` ("s" eklemek diğer bir deyişle, `Student`). 7 oluşumu bulundu ve güncelleştirildi. Düzeltme umuyoruz [bu hatayı](https://github.com/aspnet/Scaffolding/issues/633) sonraki sürümdeki.

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a>Uygulamayı test etme

Uygulamayı çalıştırın ve seçin **Öğrenciler** bağlantı. Tarayıcı genişliği bağlı olarak **Öğrenciler** bağlantı sayfanın en üstünde görünür. Varsa **Öğrenciler** bağlantı görünür değilse, sağ üst köşesinde Gezinti simgeyi tıklatın.

![Contoso University giriş sayfası dar](intro/_static/home-page-narrow.png)

Test **oluşturma**, **Düzenle**, ve **ayrıntıları** bağlantılar.

## <a name="view-the-db"></a>DB görüntüleyin

Uygulama başlatıldığında `DbInitializer.Initialize` çağrıları `EnsureCreated`. `EnsureCreated`DB var ve bir gerekirse oluşturur olmadığını algılar. DB içinde hiçbir Öğrenciler varsa `Initialize` yöntemi Öğrenciler ekler.

Açık **SQL Server Nesne Gezgini** (SSOX) gelen **Görünüm** Visual Studio menüsünde.
SSOX içinde tıklatın **(localdb) \MSSQLLocalDB > veritabanları > ContosoUniversity1**.

Genişletme **tabloları** düğümü.

Sağ **Öğrenci** tablosu ve'ı tıklatın **görünüm verilerini** oluşturulan sütunlar ve tabloya eklenen satır görmek için.

*.Mdf* ve *.ldf* DB dosyalarıdır içinde *C:\Users\\ <yourusername>*  klasör.

`EnsureCreated`Aşağıdaki iş akışı sağlayan uygulama Başlat'çağrılır:

* DB silin.
* DB şeması değiştirin (örneğin, bir `EmailAddress` alan).
* Uygulamayı çalıştırın.

`EnsureCreated`bir DB ile oluşturur`EmailAddress` sütun.

## <a name="conventions"></a>Kurallar

EF tam bir veritabanı oluşturmak çekirdek için sırayla yazılan kod miktarını kuralları veya EF çekirdek yapar varsayımlar kullanımı nedeniyle düzeydedir.

* Adlarını `DbSet` özellikleri Tablo adları olarak kullanılır. Tarafından başvurulan olmayan varlıklar için bir `DbSet` özelliği, varlık sınıfı adları tablo adları olarak kullanılır.

* Varlık özellik adlarını sütun adları için kullanılır.

* Kimliği veya classnameID adlı varlık özellikleri birincil anahtar özelliği kabul edilir.

* Bu adlı bir özelliği bir yabancı anahtar özellik olarak yorumlanır  *<navigation property name> <primary key property name>*  (örneğin, `StudentID` için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtar `ID`). Yabancı anahtar özellikleri adlı  *<primary key property name>*  (örneğin, `EnrollmentID` beri `Enrollment` varlığın birincil anahtarının `EnrollmentID`).

Geleneksel davranışı geçersiz kılınabilir. Örneğin, tablo adlarının açıkça, bu öğreticide daha önce gösterildiği gibi belirtilebilir. Sütun adlarını açıkça ayarlayabilirsiniz. Birincil anahtarları ve yabancı anahtarları açıkça ayarlanabilir.

## <a name="asynchronous-code"></a>Zaman uyumsuz kodu

Zaman uyumsuz programlama ASP.NET Core ve EF çekirdek için varsayılan moddur.

Bir web sunucusu kullanılabilir iş parçacıklarının sınırlı sayıda sahiptir ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir. Bu durum oluştuğunda sunucu yukarı serbest iş parçacığı kadar yeni istekleri işleyemiyor. G/ç tamamlamak bekliyorsunuz çünkü bunlar herhangi bir iş gerçekte yapmamanız sırasında zaman uyumlu koduyla birçok iş parçacığı bağlıdır. Bir işlemin g/ç tamamlanması beklenirken zaman uyumsuz koduyla diğer istekleri işlemek için kullanılacak sunucuyu kaydınızı kendi iş parçacığı serbest bırakılır. Sonuç olarak, zaman uyumsuz kod sunucu kaynaklarını daha verimli bir şekilde kullanılmak üzere etkinleştirir ve sunucu gecikme olmadan daha fazla trafik işlemek için etkinleştirilir.

Zaman uyumsuz kod yükünü az miktarda çalışma zamanında tanıtır. Trafiğin düşük durumlar için performans isabet yüksek trafik durumlarda düşünülerek, çalışırken, olası performans geliştirmesi önemli.

Aşağıdaki kodda, `async` anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi zaman uyumsuz yürütme kodu olun.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* `async` Anahtar sözcüğü derleyiciye bildirir:

  * Geri aramalar yöntemi gövde bölümlerinin oluşturur.
  * Otomatik olarak oluştur [görev](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) döndürülen nesne. Daha fazla bilgi için bkz: [görev dönüş türü](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* Örtük dönüş türü `Task` devam eden iş temsil eder.

* `await` Anahtar sözcüğü yöntemi iki parçalara bölmek derleyici neden olur. İlk bölümü ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır. İkinci bölümü işlem tamamlandığında çağrılan bir geri çağırma yöntemi konur.

* `ToListAsync`zaman uyumsuz sürümü `ToList` genişletme yöntemi.

EF çekirdek kullanan zaman uyumsuz kodu yazarken dikkat edilmesi gereken bazı noktalar:

* Sorguları ya da Veritabanına gönderilen komutları neden deyimleri zaman uyumsuz olarak çalıştırılır. İçeren, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, ve `SaveChangesAsync`. Yalnızca değiştirme deyimleri içermeyen bir `IQueryable`, gibi `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* İş parçacığı içinde korumalı bir EF çekirdek bağlamı değil: paralel birden çok işlemleri yapmak denemeyin. 

* Zaman uyumsuz kod performans yararlarını yararlanmak için DB sorguları göndermek EF çekirdek yöntemleri çağırırsanız paketleri kitaplığı (disk belleği ettirilmesi gibi) zaman uyumsuz kullandığını doğrulayın.

. NET'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz: [zaman uyumsuz genel bakış](https://docs.microsoft.com/dotnet/articles/standard/async).

Sonraki öğreticide, temel CRUD (Oluştur, oku, Güncelleştir, Sil) işlemleri incelenmesini.

>[!div class="step-by-step"]
[Next](xref:data/ef-rp/crud)
