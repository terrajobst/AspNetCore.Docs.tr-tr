---
title: ASP.NET Core - Öğreticisi 1. 8'de Entity Framework Core ile Razor sayfaları
author: rick-anderson
description: Entity Framework Core kullanan bir Razor sayfaları uygulamasının nasıl oluşturulacağını gösterir
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/intro
ms.openlocfilehash: d74d0909f03fe4d504f21c3880ec2447cadc8b13
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336100"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>ASP.NET Core - Öğreticisi 1. 8'de Entity Framework Core ile Razor sayfaları

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulamasını, Entity Framework (EF) çekirdek kullanarak bir ASP.NET Core Razor sayfalar uygulamasının nasıl oluşturulacağını gösterir.

Örnek uygulama, kurgusal Contoso üniversite için bir web sitesidir. Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir. Contoso University örnek uygulamasının nasıl oluşturulacağını açıklayan öğreticileri serisinin ilk sayfadır.

[İndirme veya tamamlanmış uygulamayı görüntüleyin.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Yükleme yönergeleri](xref:tutorials/index#how-to-download-a-sample).

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

[!INCLUDE [](~/includes/2.1-SDK.md)]

------

Konusunda [Razor sayfaları](xref:razor-pages/index). Yeni programcılar tamamlamanız gereken [Razor sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) Bu seriyi başlatmadan önce.

## <a name="troubleshooting"></a>Sorun giderme

Bir sorunla karşılaşırsanız, çözümleyemiyor çalıştırırsanız, genel olarak çözüm kodunuzda karşılaştırarak bulabilirsiniz [projeyi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples). Soru göndererek Yardım almak için en iyi yolu olan [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) için [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

## <a name="the-contoso-university-web-app"></a>Contoso University web uygulaması

Aşağıdaki öğreticilerde oluşturulan bir uygulamayı bir temel university web sitesidir.

Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin. Öğreticide oluşturulan ekranlar birkaçını aşağıda verilmiştir.

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit.png)

Bu sitenin UI Stili yerleşik şablonları tarafından üretilen yakın ' dir. EF çekirdekli Razor sayfaları, UI ile öğretici odağı açıktır.

## <a name="create-the-contosouniversity-razor-pages-web-app"></a>ContosoUniversity Razor sayfaları web uygulaması oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.
* Yeni bir ASP.NET Core Web uygulaması oluşturun. Projeyi adlandırın **ContosoUniversity**. Projeyi adlandırın önemlidir *ContosoUniversity* kod kopyalanıp/yapıştırılmış ad alanları eşleştirmek için.
* Seçin **ASP.NET Core 2.1** açılır ve ardından **Web uygulaması**.

Önceki adımlarda görüntüleri için bkz: [Razor web uygulaması oluşturma](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).
Uygulamayı çalıştırın.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a>Site stili Ayarla

Birkaç değişiklik site menü, Düzen ve giriş sayfası ayarlayın. Güncelleştirme *Pages/Shared/_Layout.cshtml* aşağıdaki değişikliklerle birlikte:

* "Contoso Üniversitesi" için "ContosoUniversity" her örneğini değiştirin. Üç örnekleri vardır.

* Menü girdileri eklemek **Öğrenciler**, **kursları**, **Eğitmenler**, ve **Departmanlar**ve silme **başvurun** menüsü girişi.

Değişiklikler vurgulanır. (Tüm biçimlendirme *değil* görüntülenir.)

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

İçinde *sayfalar/dizin.cshtml*, dosyanın içeriğini ASP.NET ve MVC hakkında metnin bu uygulama hakkında metinle değiştirmek için aşağıdaki kodla değiştirin:

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a>Veri modeli oluşturma

Varlık sınıflarının Contoso University uygulama oluşturun. Aşağıdaki üç varlıklarla başlayın:

![Kurs kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

Bir-çok ilişkisi arasında `Student` ve `Enrollment` varlıklar. Bir-çok ilişkisi arasında `Course` ve `Enrollment` varlıklar. Bir öğrenci herhangi bir sayıda kursları kaydedebilirsiniz. Bir kurs herhangi bir sayıda Öğrenciler içinde kayıtlı olabilir.

Aşağıdaki bölümlerde, bu varlıkların her biri için bir sınıf oluşturulur.

### <a name="the-student-entity"></a>Öğrenci varlık

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

Oluşturma bir *modelleri* klasör. İçinde *modelleri* klasöründe adlı bir sınıf dosyası oluşturma *Student.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

`ID` Özelliği bu sınıfa karşılık gelen veritabanı (DB) tablosunun birincil anahtar sütunu duruma gelir. Varsayılan olarak EF Core adlı bir özellik yorumlar `ID` veya `classnameID` birincil anahtar olarak. İçinde `classnameID`, `classname` sınıf adıdır. Alternatif birincil anahtarı otomatik olarak kabul edilen `StudentID` önceki örnekte.

`Enrollments` Özelliği bir [gezinti özelliği](/ef/core/modeling/relationship). Gezinti özellikleri bu varlıkla ilgili diğer varlıkları bağlayın. Bu durumda, `Enrollments` özelliği bir `Student entity` tüm tutan `Enrollment` olarak ilişkili varlıkları `Student`. Örneğin, bir öğrenci satır DB'de iki kayıt satırları ilgili olan `Enrollments` gezinti özelliği içerir, bu iki `Enrollment` varlıklar. İlgili `Enrollment` satırdır bu öğrencinin birincil anahtar değerini içeren bir satır `StudentID` sütun. Örneğin, Öğrenci kimlikli varsayalım = 1 olan iki satır `Enrollment` tablo. `Enrollment` Tablosunda var olan iki satır `StudentID` = 1. `StudentID` içinde bir yabancı anahtar `Enrollment` içinde Öğrenci belirten tablo `Student` tablo.

Bir gezinme özelliği birden çok varlık tutarsanız gezinme özelliğini bir liste türü gibi olmalıdır `ICollection<T>`. `ICollection<T>` belirtilebilir, ya da bir tür gibi `List<T>` veya `HashSet<T>`. Zaman `ICollection<T>` olduğu EF Core kullanıldığında, oluşturur bir `HashSet<T>` varsayılan olarak koleksiyon. Birden çok varlık tutun Gezinti özellikleri çoktan çoğa ve bire çok ilişkileri gelir.

### <a name="the-enrollment-entity"></a>Kayıt varlık

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

İçinde *modelleri* klasör oluşturma *Enrollment.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` Birincil anahtar özelliğidir. Bu varlığı kullanan `classnameID` yerine desen `ID` gibi `Student` varlık. Genellikle geliştiriciler bir düzen seçin ve veri modelini kullanın. Bir sonraki öğreticide, classname Kimliğini kullanarak, veri modelinde aktarma uygulamak daha kolay hale getirmek için gösterilir.

`Grade` Özelliği bir `enum`. Sonra soru işareti `Grade` türü bildirimi gösterir `Grade` özelliği boş değer atanabilir. Boş bir sınıf bir sıfır sınıf farklı--null anlamına gelir bir sınıf bilinen değil veya henüz atanmamış.

`StudentID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`. Bir `Enrollment` varlıktır biriyle ilişkili `Student` tek bir özellik içerecek şekilde varlık `Student` varlık. `Student` Varlık farklıdır `Student.Enrollments` içeren birden çok gezinti özelliği `Enrollment` varlıklar.

`CourseID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`. Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.

EF Core adlandırılmışsa, bu özellik bir yabancı anahtar olarak yorumlar `<navigation property name><primary key property name>`. Örneğin,`StudentID` için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`. Yabancı anahtar özellikleri de adı `<primary key property name>`. Örneğin, `CourseID` beri `Course` varlığın birincil anahtarı `CourseID`.

### <a name="the-course-entity"></a>Kurs varlık

![Kurs varlık diyagramı](intro/_static/course-entity.png)

İçinde *modelleri* klasör oluşturma *Course.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

`Enrollments` Özelliktir bir gezinme özelliği. A `Course` varlık dilediğiniz sayıda ilgili olabileceğini `Enrollment` varlıklar.

`DatabaseGenerated` DB sahip, oluşturmak, yerine özniteliği birincil anahtarı belirtmek için uygulamayı sağlar.

## <a name="scaffold-the-student-model"></a>İskele Öğrenci modeli

Bu bölümde, Öğrenci modeli iskele kurulmuş. Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik Öğrenci modeli oluşturur.

* Projeyi oluşturun.
* Oluşturma *sayfaları/Öğrenciler* klasör.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları/Öğrenciler* klasör > **Ekle** > **yeni iskele kurulmuş öğe**.
* İçinde **İskele Ekle** iletişim kutusunda **Entity Framework (CRUD) kullanarak Razor sayfaları** > **ekleme**.

Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:

* İçinde **Model sınıfı** açılan listesinde, select **Öğrenci (ContosoUniversity.Models)**.
* İçinde **veri bağlamı sınıfının** satır, select **+** (artı) oturum açın ve oluşturulan bir adla değiştirin **ContosoUniversity.Models.SchoolContext**.
* İçinde **veri bağlamı sınıfının** açılan listesinde, select **ContosoUniversity.Models.SchoolContext**
* Seçin **ekleme**.

![CRUD iletişim](intro/_static/s1.png)

Bkz: [film modeli iskelesini](xref:tutorials/razor-pages/model#scaffold-the-movie-model) önceki adımı ile ilgili bir sorun varsa.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Öğrenci modeli iskelesini için aşağıdaki komutları çalıştırın.

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
```
------

İskele işlem oluşturulur ve aşağıdaki dosya değişti:

### <a name="files-created"></a>Oluşturulan dosyalar

* *Sayfa/Öğrenciler* oluşturma, silme, Ayrıntılar, düzenleme, dizin.
* *Data/SchoolContext.cs*

### <a name="files-updates"></a>Güncelleştirme dosyaları

* *Startup.cs* : Bu dosyada yapılan ayrıntılı bir sonraki bölüm.
* *appSettings.JSON* : yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi eklenir.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Bağımlılık ekleme ile kayıtlı bağlamını İnceleme

ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection). Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır. Bir db bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.

Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.

İnceleme `ConfigureServices` yönteminde *Startup.cs*. Vurgulanan satırı iskele kurucu tarafından eklendi:

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.

## <a name="update-main"></a>Ana güncelleştir

İçinde *Program.cs*, değişiklik `Main` yöntemi aşağıdakileri yapmak için:

* Bir DB bağlamı örneği bağımlılık ekleme kapsayıcısını alın.
* Çağrı [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).
* Bağlam dispose olduğunda `EnsureCreated` yöntemi tamamlar.

Aşağıdaki kod güncelleştirilmiş gösterir *Program.cs* dosya.

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

`EnsureCreated` Veritabanı bağlamının var olmasını sağlar. Varsa, hiçbir işlem yapılmaz. Yoksa, veritabanı ve tüm şema oluşturulur. `EnsureCreated` veritabanı oluşturmaya geçişleri kullanmaz. İle oluşturulmuş bir veritabanı `EnsureCreated` daha sonra geçişleri kullanılarak güncelleştirilemez.

`EnsureCreated` Aşağıdaki iş akışı sağlayan uygulama Başlat menüsünde çağrılır:

* DB silin.
* DB şema değiştirme (örneğin, bir `EmailAddress` alan).
* Uygulamayı çalıştırın.
* `EnsureCreated` bir DB ile oluşturur`EmailAddress` sütun.

`EnsureCreated` Şema hızla gelişirken erken geliştirme uygundur. Daha sonra öğreticide DB silinir ve geçişler kullanılır.

### <a name="test-the-app"></a>Uygulamayı test etme

Uygulamayı çalıştırın ve tanımlama bilgisi ilkesini kabul edin. Bu uygulama, kişisel bilgileri tutmak değil. Tanımlama bilgisi ilkesi hakkında bilgi edinebilirsiniz [AB genel veri koruma yönetmeliği (GDPR) Destek](xref:security/gdpr).

* Seçin **Öğrenciler** bağlantısını ve ardından **Yeni Oluştur**.
* Ayrıntılar, düzenleme, test edin ve bağlantılarını silin.

## <a name="examine-the-schoolcontext-db-context"></a>SchoolContext DB bağlamını İnceleme

Verilen veri modeli için EF Core işlevselliği koordine eden ana DB bağlamı sınıfının sınıftır. Veri bağlamı türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir. Bu projede adlı sınıfı `SchoolContext`.

Güncelleştirme *SchoolContext.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

Vurgulanan kod oluşturur bir [olan DB\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) her varlık kümesi özelliği. EF Core terminolojisinde:

* Bir varlık kümesini genellikle DB tabloya karşılık gelir.
* Bir varlık tablosunda bir satıra karşılık gelir.

`DbSet<Enrollment>` ve `DbSet<Course>` atlanmış. EF Core içeren bunları örtük olarak çünkü `Student` varlık başvuruları `Enrollment` varlığı ve `Enrollment` varlık başvuruları `Course` varlık. Bu öğretici için tutmak `DbSet<Enrollment>` ve `DbSet<Course>` içinde `SchoolContext`.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Bağlantı dizesini belirtir [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb). LocalDB, SQL Server Express veritabanı Motoru'nu hafif bir sürümüdür ve uygulama geliştirme, üretim kullanımı için tasarlanmıştır. LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır. Varsayılan olarak LocalDB oluşturur *.mdf* DB dosyaları `C:/Users/<user>` dizin.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Bir veritabanı test verileri ile başlatmak için kod ekleyin

EF Core boş bir veritabanı oluşturur. Bu bölümde, bir `Initialize` yöntemi test verileriyle doldurma işlemine yazılır.

İçinde *veri* klasöründe adlı yeni bir sınıf dosyası oluşturma *DbInitializer.cs* ve aşağıdaki kodu ekleyin:

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

Kod DB'de tüm Öğrenciler olup olmadığını denetler. DB'de Öğrenci varsa, bir veritabanı test verileri ile başlatılır. Diziye test verileri yükler yerine `List<T>` performansını iyileştirmek için koleksiyonları.

`EnsureCreated` Yöntemi DB bağlamı için bir veritabanı otomatik olarak oluşturur. Veritabanı varsa, `EnsureCreated` DB değiştirmeden döndürür.

İçinde *Program.cs*, değişiklik `Main` çağrılacak yöntem `Initialize`:

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

Tüm Öğrenci kayıtlarını silin ve uygulamayı yeniden başlatın. DB başlatılmadı, bir kesme noktası ayarlayın `Initialize` sorunu tanılamak için.

## <a name="view-the-db"></a>DB görüntüleyin

Açık **SQL Server Nesne Gezgini** (SSOX) öğesinden **görünümü** Visual Studio'daki menü.
SSOX içinde tıklayın **(localdb) \MSSQLLocalDB > veritabanları > ContosoUniversity1**.

Genişletin **tabloları** düğümü.

Sağ **Öğrenci** tablosu ve'ı tıklatın **görünüm verilerini** oluşturulan sütunları ve tabloya eklenen satırları görebilirsiniz.

## <a name="asynchronous-code"></a>Zaman uyumsuz kod

Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.

Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir. Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor. G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması. İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır. Sonuç olarak, sunucu kaynaklarının daha etkin kullanılması zaman uyumsuz kod sağlar ve sunucu gecikmeler olmadan daha fazla trafik işlemek için etkinleştirilir.

Zaman uyumsuz kod, çalışma zamanında az miktarda bir ek yükü sunar. Düşük trafiğe durumlar, performans düşüşüne yüksek trafik durumlar için göz ardı edilebilir, çalışırken, olası performans geliştirmesi önemli.

Aşağıdaki kodda, [zaman uyumsuz](/dotnet/csharp/language-reference/keywords/async) anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi zaman uyumsuz yürütülen kod olun.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* `async` Anahtar sözcüğü, derleyiciye bildirir:
  * Yöntem gövdesini bölümleri için geri çağırmaları oluşturur.
  * Otomatik olarak oluşturmasını [görev](/dotnet/api/system.threading.tasks.task) döndürülen nesne. Daha fazla bilgi için [görev dönüş türü](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* Örtük dönüş türü `Task` devam eden çalışmayı temsil eder.
* `await` Anahtar sözcüğü, derleyicinin yöntemin iki parçalara bölmek neden olur. İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır. İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.
* `ToListAsync` zaman uyumsuz sürümüdür `ToList` genişletme yöntemi.

EF Core kullanan zaman uyumsuz kodu yazarken dikkat edilmesi gereken bazı noktalar şunlardır:

* Sorguları veya Veritabanına gönderilecek komutları neden deyimleri zaman uyumsuz olarak yürütülür. İçeren, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, ve `SaveChangesAsync`. Yalnızca değiştirmek deyimleri içermeyen bir `IQueryable`, gibi `var students = context.Students.Where(s => s.LastName == "Davolio")`.
* EF Core bağlam iş parçacığı güvenli olmayan: paralel birden çok işlem yapmak yeniden denemeyin.
* Zaman uyumsuz kodun performans avantajlarından yararlanmak için bunlar için bir veritabanı sorguları göndermek EF Core yöntemleri çağırırsanız kitaplığı paketlerinin (disk belleği sunamıyoruz gibi) zaman uyumsuz kullandığını doğrulayın.

. NET'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz genel bakış](/dotnet/articles/standard/async) ve [zaman uyumsuz programlama ile async ve await](/dotnet/csharp/programming-guide/concepts/async/).

Sonraki öğreticide, temel CRUD (oluşturma, okuma, güncelleştirme ve silme) işlemleri incelenir.
::: moniker-end

> [!div class="step-by-step"]
> [Next](xref:data/ef-rp/crud)
