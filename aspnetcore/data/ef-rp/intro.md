---
title: Razor sayfalarının Entity Framework Çekirdek ASP.NET Core - 8'in Öğreticisi 1 ile
author: rick-anderson
description: Entity Framework Çekirdek kullanarak Razor sayfalarının uygulamasının nasıl oluşturulacağını gösterir
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/intro
ms.openlocfilehash: 97ae8a0f268d3ca6fb2deee72128714ab6e89266
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961366"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>Razor sayfalarının Entity Framework Çekirdek ASP.NET Core - 8'in Öğreticisi 1 ile

::: moniker range="= aspnetcore-2.0"
Bu öğretici ASP.NET Core 2.0 sürümünü bulunabilir [bu PDF dosyası](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/PDF-6-18-18.pdf).
::: moniker-end
::: moniker range=">= aspnetcore-2.1"

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması Entity Framework (EF) çekirdeği kullanarak bir ASP.NET Core Razor sayfalarının uygulamasının nasıl oluşturulacağını gösterir.

Örnek uygulaması, kurgusal bir Contoso üniversite için bir web sitesidir. Öğrenci giriş, indirmelere oluşturma ve Eğitmen atamaları gibi işlevselliği içerir. Contoso University örnek uygulamasının nasıl oluşturulacağını açıklayan eğitim serileri ilk sayfasıdır.

[İndirme veya tamamlanan uygulama görüntüleyin.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Yükleme yönergeleri](xref:tutorials/index#how-to-download-a-sample).

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[! [] (~/İncludes/net-core-prereqs-windows.md) içerir [](~/includes/net-core-prereqs-windows.md)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

[! INCLUDE [] (~ / includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]

------

Aşina [Razor sayfalarının](xref:razor-pages/index). Yeni programcıları tamamlamanız gereken [Razor sayfalarının ile çalışmaya başlama](xref:tutorials/razor-pages/razor-pages-start) bu seri başlatmadan önce.

## <a name="troubleshooting"></a>Sorun giderme

Olamaz çözmek bir sorunla çalıştırırsanız, genellikle çözümün kodunuzu karşılaştırarak bulabilirsiniz [projeyi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples). Bir soruyu göndererek Yardım almak için en iyi yolu olan [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) için [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF çekirdek](https://stackoverflow.com/questions/tagged/entity-framework-core).

## <a name="the-contoso-university-web-app"></a>Contoso University web uygulaması

Bu öğreticiler oluşturulmuş uygulamaya bir temel university web sitesidir.

Kullanıcıların görüntüleyebileceği ve Öğrenci, indirmelere ve Eğitmen bilgileri güncelleştirin. Öğreticide oluşturulan ekranlar bazılarını aşağıda verilmiştir.

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

![Öğrenciler Düzenle sayfası](intro/_static/student-edit.png)

Bu sitenin UI Stili yerleşik şablonlar tarafından oluşturulan yakın ' dir. Razor sayfalarının, UI EF çekirdek üzerinde öğretici odak noktasıdır.

## <a name="create-the-contosouniversity-razor-pages-web-app"></a>ContosoUniversity Razor sayfalarının web uygulaması oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio'dan **dosya** menüsünde, select **yeni** > **proje**.
* Yeni bir ASP.NET çekirdek Web uygulaması oluşturun. Proje adı **ContosoUniversity**. Proje adı önemlidir *ContosoUniversity* kod kopyalanıp yapıştırılmış ad alanları eşleştirmek için.
* Seçin **ASP.NET Core 2.1** açılır ve ardından **Web uygulaması**.

Yukarıdaki adımları görüntüleri için bkz: [bir Razor web uygulaması oluşturma](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).
Uygulamayı çalıştırın.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a>Site stil ayarlama

Birkaç değişiklik sitesi menüsü, Düzen ve giriş sayfası ayarlayın. Güncelleştirme *Pages/Shared/_Layout.cshtml* aşağıdaki değişiklikleri:

* Her oluşumu "ContosoUniversity", "Contoso University" değiştirin. Üç oluşum vardır.

* Menü girişlerini eklemek **Öğrenciler**, **kurslar**, **Eğitmen**, ve **Departmanlar**ve silme **kişi** menü girişi.

Değişiklikler vurgulanır. (Tüm biçimlendirme *değil* görüntülenir.)

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

İçinde *Pages/Index.cshtml*, dosyanın içeriğini bu uygulama hakkında metinle ASP.NET MVC hakkında metni değiştirmek için aşağıdaki kod ile değiştirin:

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a>Veri modeli oluşturma

Sınıflar Contoso University uygulama oluşturun. Aşağıdaki üç varlıklarıyla başlatın:

![İndirmelere kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

Bir-çok ilişkisi arasında `Student` ve `Enrollment` varlıklar. Bir-çok ilişkisi arasında `Course` ve `Enrollment` varlıklar. Öğrencinin herhangi bir sayıda kurslar kaydedebilir. Bir indirmelere içinde kayıtlı Öğrenciler herhangi bir sayıda olabilir.

Aşağıdaki bölümlerde, bu varlıkların her biri için bir sınıf oluşturulur.

### <a name="the-student-entity"></a>Öğrenci varlık

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

Oluşturma bir *modelleri* klasör. İçinde *modelleri* klasörünü adlı bir sınıf dosyası oluşturma *Student.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

`ID` Özelliği bu sınıfa karşılık gelen veritabanı (DB) tablosunun birincil anahtar sütunu haline gelir. Varsayılan olarak, EF çekirdek adlı bir özellik yorumlar `ID` veya `classnameID` birincil anahtar olarak. İçinde `classnameID`, `classname` sınıfı adı. Birincil anahtar alternatif otomatik olarak kabul edilen `StudentID` önceki örnekte.

`Enrollments` Özelliği bir gezinti özelliğidir. Bu varlığa ilgili diğer varlıklar için Gezinti özellikleri bağlayın. Bu durumda, `Enrollments` özelliği bir `Student entity` tüm tutan `Enrollment` için ilişkili olan varlıkların `Student`. Örneğin, bir öğrenci satır varsa DB iki kayıt, ilişkili satırları sahip `Enrollments` gezinti özelliği içerdiğinden bu iki `Enrollment` varlıklar. İlgili `Enrollment` satırıdır bu öğrencinin birincil anahtar değerini içeren bir satır `StudentID` sütun. Örneğin, Öğrenci kimlikli varsayalım = 1 sahip iki satır `Enrollment` tablo. `Enrollment` Tablolu sahip iki satır `StudentID` = 1. `StudentID` bir yabancı anahtar `Enrollment` Öğrenci belirtir tablo `Student` tablo.

Bir gezinme özelliği birden çok varlık tutarsanız gezinti özelliği bir liste türü gibi olmalıdır `ICollection<T>`. `ICollection<T>` belirtilebilir, ya da bir türü `List<T>` veya `HashSet<T>`. Zaman `ICollection<T>` olduğu EF çekirdek oluşturur kullanıldığında, bir `HashSet<T>` varsayılan koleksiyon. Birden çok varlık tutun Gezinti özellikleri çok- ve -çok ilişkileri gelir.

### <a name="the-enrollment-entity"></a>Kayıt varlık

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

İçinde *modelleri* klasörü oluşturmak *Enrollment.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` Birincil anahtarı bir özelliktir. Bu varlığı kullanan `classnameID` yerine desen `ID` gibi `Student` varlık. Genellikle geliştiriciler bir deseni seçin ve veri modelini kullanır. Sonraki öğreticide, veri modelinde devralma uygulamak daha kolay classname Kimliğini kullanarak gösterilir.

`Grade` Özelliği bir `enum`. Soru işareti sonra `Grade` türü bildirimi gösterir `Grade` özelliği boş değer atanabilir. Null bir düzeyde bir sıfır ataması farklı.--null anlamına gelir bir düzeyde bilinen değil veya henüz atanmamış.

`StudentID` Özelliği bir yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`. Bir `Enrollment` varlıktır biriyle ilişkili `Student` tek bir özelliği içerecek şekilde varlık `Student` varlık. `Student` Varlık farklı olarak `Student.Enrollments` birden çok içeren gezinti özelliği `Enrollment` varlıklar.

`CourseID` Özelliği bir yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`. Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.

EF çekirdek, ise bu özellik yabancı anahtar olarak yorumlar `<navigation property name><primary key property name>`. Örneğin,`StudentID` için `Student` gezinti özelliği, bu yana `Student` varlığın birincil anahtarının `ID`. Yabancı anahtar özellikleri de adlı `<primary key property name>`. Örneğin, `CourseID` beri `Course` varlığın birincil anahtarının `CourseID`.

### <a name="the-course-entity"></a>İndirmelere varlık

![İndirmelere varlık diyagramı](intro/_static/course-entity.png)

İçinde *modelleri* klasörü oluşturmak *Course.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

`Enrollments` Özelliği bir gezinti özelliğidir. A `Course` varlık herhangi bir sayıda için ilgili olabileceğini `Enrollment` varlıklar.

`DatabaseGenerated` DB sahip, oluşturmak, yerine özniteliği birincil anahtarı belirtmek uygulama izin verir.

## <a name="scaffold-the-student-model"></a>İskele Öğrenci modeli

Bu bölümde, Öğrenci modeli iskele kurulmuş. Diğer bir deyişle, yapı iskelesi aracı Öğrenci modeli için oluşturma, okuma, güncelleştirme ve Sil (CRUD) işlemleri için sayfaları oluşturur.

* Projeyi oluşturun.
* Oluşturma *sayfaları/Öğrenciler* klasör.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları/Öğrenciler* klasörü > **Ekle** > **yeni iskele kurulmuş öğe**.
* İçinde **İskele Ekle** iletişim kutusunda **Razor Entity Framework (CRUD) kullanarak sayfaları** > **eklemek**.

Tamamlamak **Razor Entity Framework (CRUD) kullanarak Sayfa Ekle** iletişim:

* İçinde **Model sınıfı** açılan listesinde, select **Öğrenci (ContosoUniversity.Models)**.
* İçinde **veri bağlamı sınıfı** satır, select **+** (artı) oturum açın ve oluşturulan adı değiştirmek **ContosoUniversity.Models.SchoolContext**.
* İçinde **veri bağlamı sınıfı** açılan listesinde, select **ContosoUniversity.Models.SchoolContext**
* Seçin **eklemek**.

![CRUD iletişim](intro/_static/s1.png)

Bkz: [film modeli İskele](xref:tutorials/razor-pages/model#scaffold-the-movie-model) önceki adımı ile ilgili bir sorun varsa.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Öğrenci modeli iskele için aşağıdaki komutları çalıştırın.

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
```
------

İskele işlemi oluşturulan ve aşağıdaki dosyaları değişti:

### <a name="files-created"></a>Oluşturulan dosyalar

* *Sayfa/Öğrenciler* oluşturma, silme, ayrıntı, düzenleme, dizin.
* *Data/ContosoUniversityContext.cs*

### <a name="files-updates"></a>Güncelleştirme dosyaları

* *Haline* : Bu dosyada yapılan değişiklikler ayrıntılı bir sonraki bölüm.
* *appSettings.JSON* : yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi eklendi.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Bağımlılık ekleme ile kayıtlı bağlamını İnceleme

ASP.NET Core ile oluşturulan [bağımlılık ekleme](xref:fundamentals/dependency-injection). Hizmetleri (örneğin, EF çekirdek DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu Hizmetleri (örneğin, Razor sayfalarının) gerektiren bileşenler bu hizmetlere Oluşturucu parametreleri yoluyla sağlanır. Bir db bağlamı örneği alır Oluşturucusu kodu daha sonra öğreticide gösterilir.

Yapı iskelesi Aracı'nı otomatik olarak bir veritabanı bağlamını oluşturulur ve bağımlılık ekleme kapsayıcısını ile kayıtlı.

İncelemek `ConfigureServices` yönteminde *haline*. Vurgulanan satırı iskele kurucu tarafından eklendi:

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

Bağlantı dizesinin adını bağlamına üzerinde bir yöntemini çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesi. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.

## <a name="update-main"></a>Ana güncelleştir

İçinde *Program.cs*, değişiklik `Main` yöntemi aşağıdakileri yapın:

* Bir DB bağlamı örneği bağımlılık ekleme kapsayıcıdan alın.
* Çağrı [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).
* Bağlamda dispose zaman `EnsureCreated` yöntemi tamamlar.

Aşağıdaki kod güncelleştirilmiş gösterir *Program.cs* dosya.

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

`EnsureCreated` Veritabanı bağlamı için varolduğunu sağlar. Varsa, hiçbir işlem yapılmadı. Yoksa, tüm şema ve veritabanı oluşturulur. `EnsureCreated` geçişler, veritabanı oluşturmak için kullanmaz. İle oluşturulmuş bir veritabanı `EnsureCreated` daha sonra geçişler kullanılarak güncelleştirilemez.

`EnsureCreated` Aşağıdaki iş akışı sağlayan uygulama Başlat'çağrılır:

* DB silin.
* DB şeması değiştirin (örneğin, bir `EmailAddress` alan).
* Uygulamayı çalıştırın.
* `EnsureCreated` bir DB ile oluşturur`EmailAddress` sütun.

`EnsureCreated` Şema hızla gelişen erken geliştirme yararlı olur. Daha sonra öğreticide DB silinir ve geçişleri kullanılır.

### <a name="test-the-app"></a>Uygulamayı test etme

Uygulamayı çalıştırın ve tanımlama bilgisi ilkesini kabul edin. Bu uygulamayı, kişisel bilgi korumayarak. Tanımlama bilgisi ilkesi hakkında bilgi edinebilirsiniz [AB genel veri koruma düzenleme (GDPR) Destek](xref:security/gdpr).

* Seçin **Öğrenciler** bağlantısını ve ardından **Yeni Oluştur**.
* Düzenleme, Ayrıntılar, test ve bağlantıları silin.

## <a name="examine-the-schoolcontext-db-context"></a>SchoolContext DB bağlamını İnceleme

Verilen veri modeli için EF temel işlevleri koordinatları ana sınıfı DB bağlamı sınıftır. Veri bağlamı türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Veri bağlamı hangi varlıkların veri modelinde dahil edildiğini belirtir. Bu projede adlı sınıfı `SchoolContext`.

Güncelleştirme *SchoolContext.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

Vurgulanmış kodu oluşturur bir [DbSet\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği her bir varlık kümesi. EF çekirdek terminolojisinde:

* Bir varlık kümesine genellikle DB tabloya karşılık gelir.
* Bir varlık tablosunda bir satırı karşılık gelir.

`DbSet<Enrollment>` ve `DbSet<Course>` kullanılmayabilir. EF çekirdek içerir bunları örtük olarak çünkü `Student` varlık başvuruları `Enrollment` varlık ve `Enrollment` varlık başvuruları `Course` varlık. Bu öğretici için tutmak `DbSet<Enrollment>` ve `DbSet<Course>` içinde `SchoolContext`.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Bağlantı dizesi belirtir [SQL Server yerel veritabanı](/sql/database-engine/configure-windows/sql-server-2016-express-localdb). Yerel veritabanı, SQL Server Express veritabanı Motoru'nu hafif sürümüdür ve uygulama geliştirme, üretim kullanımı için amaçlanmıştır. Yerel veritabanı isteğe bağlı olarak başlar ve bu yüzden karmaşık yapılandırma kullanıcı modunda çalışır. Varsayılan olarak, yerel veritabanı oluşturur *.mdf* DB dosyaları `C:/Users/<user>` dizin.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>DB test verileri ile başlatmak için kod ekleme

EF çekirdek boş bir veritabanı oluşturur. Bu bölümde, bir `Initialize` yöntemi test veri ile doldurulacak yazılır.

İçinde *veri* klasörünü adlı yeni bir sınıf dosyası oluşturma *DbInitializer.cs* ve aşağıdaki kodu ekleyin:

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

Kod DB'de herhangi Öğrenciler olup olmadığını denetler. DB içinde hiçbir Öğrenciler varsa, DB test verilerle başlatıldı. Diziye test verileri yükler yerine `List<T>` performansı iyileştirmek için koleksiyonları.

`EnsureCreated` Yöntemi DB DB bağlamı için otomatik olarak oluşturur. Veritabanı varsa `EnsureCreated` DB değiştirmeden döndürür.

İçinde *Program.cs*, değişiklik `Main` çağrılacak yöntem `Initialize`:

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

Tüm Öğrenci kayıtlarını silin ve uygulamayı yeniden başlatın. DB başlatılmadı, bir kesme noktası kümesinde `Initialize` sorunu tanılamak için.

## <a name="view-the-db"></a>DB görüntüleyin

Açık **SQL Server Nesne Gezgini** (SSOX) gelen **Görünüm** Visual Studio menüsünde.
SSOX içinde tıklatın **(localdb) \MSSQLLocalDB > veritabanları > ContosoUniversity1**.

Genişletme **tabloları** düğümü.

Sağ **Öğrenci** tablosu ve'ı tıklatın **görünüm verilerini** oluşturulan sütunlar ve tabloya eklenen satır görmek için.

## <a name="asynchronous-code"></a>Zaman uyumsuz kodu

Zaman uyumsuz programlama ASP.NET Core ve EF çekirdek için varsayılan moddur.

Bir web sunucusu kullanılabilir iş parçacıklarının sınırlı sayıda sahiptir ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir. Bu durum oluştuğunda sunucu yukarı serbest iş parçacığı kadar yeni istekleri işleyemiyor. G/ç tamamlamak bekliyorsunuz çünkü bunlar herhangi bir iş gerçekte yapmamanız sırasında zaman uyumlu koduyla birçok iş parçacığı bağlıdır. Bir işlemin g/ç tamamlanması beklenirken zaman uyumsuz koduyla diğer istekleri işlemek için kullanılacak sunucuyu kaydınızı kendi iş parçacığı serbest bırakılır. Sonuç olarak, zaman uyumsuz kod sunucu kaynaklarını daha verimli bir şekilde kullanılmak üzere etkinleştirir ve sunucu gecikme olmadan daha fazla trafik işlemek için etkinleştirilir.

Zaman uyumsuz kod yükünü az miktarda çalışma zamanında tanıtır. Trafiğin düşük durumlar için performans isabet yüksek trafik durumlarda düşünülerek, çalışırken, olası performans geliştirmesi önemli.

Aşağıdaki kodda, [zaman uyumsuz](/dotnet/csharp/language-reference/keywords/async) anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi zaman uyumsuz yürütme kodu olun.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* `async` Anahtar sözcüğü derleyiciye bildirir:
  * Geri aramalar yöntemi gövde bölümlerinin oluşturur.
  * Otomatik olarak oluştur [görev](/dotnet/api/system.threading.tasks.task) döndürülen nesne. Daha fazla bilgi için bkz: [görev dönüş türü](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* Örtük dönüş türü `Task` devam eden iş temsil eder.
* `await` Anahtar sözcüğü yöntemi iki parçalara bölmek derleyici neden olur. İlk bölümü ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır. İkinci bölümü işlem tamamlandığında çağrılan bir geri çağırma yöntemi konur.
* `ToListAsync` zaman uyumsuz sürümü `ToList` genişletme yöntemi.

EF çekirdek kullanan zaman uyumsuz kodu yazarken dikkat edilmesi gereken bazı noktalar:

* Sorguları ya da Veritabanına gönderilen komutları neden deyimleri zaman uyumsuz olarak çalıştırılır. İçeren, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, ve `SaveChangesAsync`. Yalnızca değiştirme deyimleri içermeyen bir `IQueryable`, gibi `var students = context.Students.Where(s => s.LastName == "Davolio")`.
* İş parçacığı içinde korumalı bir EF çekirdek bağlamı değil: paralel birden çok işlemleri yapmak denemeyin.
* Zaman uyumsuz kod performans yararlarını yararlanmak için DB sorguları göndermek EF çekirdek yöntemleri çağırırsanız paketleri kitaplığı (disk belleği ettirilmesi gibi) zaman uyumsuz kullandığını doğrulayın.

. NET'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz: [zaman uyumsuz genel bakış](/dotnet/articles/standard/async) ve [zaman uyumsuz programlama ile zaman uyumsuz ve bekleme](/dotnet/csharp/programming-guide/concepts/async/).

Sonraki öğreticide, temel CRUD (Oluştur, oku, Güncelleştir, Sil) işlemleri incelenmesini.
::: moniker-end

> [!div class="step-by-step"]
> [Next](xref:data/ef-rp/crud)