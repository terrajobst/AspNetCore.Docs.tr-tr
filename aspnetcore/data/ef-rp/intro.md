---
title: ASP.NET Core - Öğreticisi 1. 8'de Entity Framework Core ile Razor sayfaları
author: tdykstra
description: Entity Framework Core kullanan bir Razor sayfaları uygulamasının nasıl oluşturulacağını gösterir
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 07/22/2019
uid: data/ef-rp/intro
ms.openlocfilehash: 107b348b4484301b86eeb5528833914fe4c1eaf7
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080916"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>ASP.NET Core - Öğreticisi 1. 8'de Entity Framework Core ile Razor sayfaları

Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Bu, bir ASP.NET Core Razor Pages uygulamasında Entity Framework (EF) çekirdeğini nasıl kullanacağınızı gösteren bir öğretici serisinin ilkisidir. Öğreticiler, kurgusal bir Contoso Üniversitesi için bir Web sitesi oluşturur. Site, öğrenci giriş, kurs oluşturma ve eğitmen atamaları gibi işlevleri içerir.

[İndirme veya tamamlanmış uygulamayı görüntüleyin.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Yükleme yönergeleri](xref:index#how-to-download-a-sample).

## <a name="prerequisites"></a>Önkoşullar

* Razor Pages yeni başladıysanız, bu duruma başlamadan önce Razor Pages öğretici serisini [kullanmaya](xref:tutorials/razor-pages/razor-pages-start) başlayın.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[VS prereqs](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[VS Code prereqs](~/includes/net-core-prereqs-vsc-3.0.md)]

---

## <a name="database-engines"></a>Veritabanı altyapıları

Visual Studio yönergeleri, yalnızca Windows üzerinde çalışan bir SQL Server Express sürümü olan [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)'yi kullanır.

Visual Studio Code yönergeler, platformlar arası bir veritabanı altyapısı olan [SQLite](https://www.sqlite.org/)kullanır.

SQLite kullanmayı seçerseniz, SQLite [Için DB tarayıcısı](https://sqlitebrowser.org/)gibi bir SQLite veritabanını yönetmek ve görüntülemek için üçüncü taraf bir araç indirip yükleyin.

## <a name="troubleshooting"></a>Sorun giderme

Giderebileceğiniz bir sorunla karşılaşırsanız, kodunuzu [Tamamlanan projeyle](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)karşılaştırın. Yardım almanın iyi bir yolu, [ASP.NET Core etiketi](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core etiketi](https://stackoverflow.com/questions/tagged/entity-framework-core)kullanılarak StackOverflow.com 'e bir soru göndererek.

## <a name="the-sample-app"></a>Örnek uygulama

Aşağıdaki öğreticilerde oluşturulan bir uygulamayı bir temel university web sitesidir. Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin. Öğreticide oluşturulan ekranlar birkaçını aşağıda verilmiştir.

![Öğrenciler dizin sayfası](intro/_static/students-index30.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit30.png)

Bu sitenin kullanıcı arabirimi stili yerleşik proje şablonlarına dayalıdır. Öğreticinin odağı, Kullanıcı arabirimini nasıl özelleştireceğinizi değil EF Core kullanma konusunda yer alır.

Tamamlanan projenin kaynak kodunu almak için sayfanın üst kısmındaki bağlantıyı izleyin. *Cu30* klasörü, öğreticinin ASP.NET Core 3,0 sürümü için kod içerir. 1-7 öğreticileri için kodun durumunu yansıtan dosyalar *cu30snapshots* klasöründe bulunabilir.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Tamamlanmış projeyi indirdikten sonra uygulamayı çalıştırmak için:

* Üç dosyayı ve ad içinde *SQLite* içeren bir klasörü silin.
* Projeyi oluşturun.
* Paket Yöneticisi konsolu 'nda (PMC) aşağıdaki komutu çalıştırın:

  ```powershell
  Update-Database
  ```

* Veritabanını temel alarak projeyi çalıştırın.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Tamamlanmış projeyi indirdikten sonra uygulamayı çalıştırmak için:

* *Contosouniversity. csproj*öğesini silin ve *Contosoüniversıtysqlite. csproj* öğesini *contosouniversity. csproj*olarak yeniden adlandırın.
* *Startup.cs*silin ve *StartupSQLite.cs* öğesini *Startup.cs*olarak yeniden adlandırın.
* *AppSettings. JSON*öğesini silin ve *Appsettingssqlite. JSON* öğesini *appSettings. JSON*olarak yeniden adlandırın.
* *Geçişler* klasörünü silin ve *migrationssql* öğesini *geçişlerle*yeniden adlandırın.
* Projeyi oluşturun.
* Proje klasöründeki bir komut isteminde aşağıdaki komutları çalıştırın:

  ```dotnetcli
  dotnet tool install --global dotnet-ef --version 3.0.0-*
  dotnet ef database update
  ```

* SQLite aracında şu SQL ifadesini çalıştırın:

  ```sql
  UPDATE Department SET RowVersion = randomblob(8)
  ```

* Veritabanını temel alarak projeyi çalıştırın.

---

## <a name="create-the-web-app-project"></a>Web uygulaması projesi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.
* Seçin **ASP.NET Core Web uygulaması**.
* Projeyi adlandırın *ContosoUniversity*. Büyük harfler de dahil olmak üzere bu tam adı kullanmak önemlidir, bu nedenle kod kopyalanıp yapıştırılırken ad alanları eşleşir.
* Açılan menüden **.NET Core** ve **3,0 ASP.NET Core** seçin ve ardından **Web uygulaması**' nı seçin.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Bir terminalde, proje klasörünün oluşturulması gereken klasöre gidin.

* Bir Razor Pages projesi ve `cd` yeni proje klasörü oluşturmak için aşağıdaki komutları çalıştırın:

  ```dotnetcli
  dotnet new webapp -o ContosoUniversity
  cd ContosoUniversity
  ```

---

## <a name="set-up-the-site-style"></a>Site stili Ayarla

*Sayfaları/paylaşılan/_Layout. cshtml*'yi güncelleştirerek site üst bilgisini, alt bilgisini ve menüsünü ayarlayın:

* "Contoso Üniversitesi" için "ContosoUniversity" her örneğini değiştirin. Üç örnekleri vardır.

* **Giriş** ve **Gizlilik** menü girişlerini silin ve **hakkında**, **öğrenciler**, **Kurslar**, **eğitmenler**ve **Departmanlar**için girişler ekleyin.

Değişiklikler vurgulanır.

[!code-cshtml[Main](intro/samples/cu30/Pages/Shared/_Layout.cshtml?highlight=6,14,21-35,49)]

*Pages/Index. cshtml*dosyasında, ASP.NET Core hakkındaki metni bu uygulamayla ilgili metinle değiştirmek için dosyanın içeriğini aşağıdaki kodla değiştirin:

[!code-cshtml[Main](intro/samples/cu30/Pages/Index.cshtml)]

Giriş sayfasının göründüğünü doğrulamak için uygulamayı çalıştırın.

## <a name="the-data-model"></a>Veri modeli

Aşağıdaki bölümler bir veri modeli oluşturur:

![Kurs kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

Bir öğrenci herhangi bir sayıda kursa kaydolabilir ve bir kurs, kayıtlı sayıda öğrenciye sahip olabilir.

## <a name="the-student-entity"></a>Öğrenci varlık

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

* Proje klasöründe bir *modeller* klasörü oluşturun. 

* Aşağıdaki kodla *modeller/öğrenci. cs* oluşturun:

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Student.cs)]

Özelliği `ID` , bu sınıfa karşılık gelen veritabanı tablosunun birincil anahtar sütunu olur. Varsayılan olarak EF Core adlı bir özellik yorumlar `ID` veya `classnameID` birincil anahtar olarak. Bu nedenle birincil anahtar `Student` sınıfı için alternatif otomatik olarak tanınan ad olur. `StudentID`

`Enrollments` Özelliği bir [gezinti özelliği](/ef/core/modeling/relationships). Gezinti özellikleri, bu varlıkla ilgili diğer varlıkları tutar. Bu durumda, `Enrollments` bir `Student` varlığın özelliği söz konusu öğrenciye ilişkin tüm varlıklarıbarındırır.`Enrollment` Örneğin, veritabanındaki bir öğrenci satırında iki ilişkili kayıt satırı varsa, `Enrollments` gezinti özelliği bu iki kayıt varlığını içerir. 

Veritabanında, bir kayıt satırı, Studentitıd sütunu öğrencinin ID değerini içeriyorsa bir öğrenci satırıyla ilgilidir. Örneğin, bir öğrenci satırının ID = 1 olduğunu varsayalım. İlgili kayıt satırları Studentitıd = 1 olacaktır. Studentitıd, kayıt tablosundaki bir *yabancı anahtardır* . 

Özelliği, birden çok ilgili `ICollection<Enrollment>` kayıt varlığı olabileceğinden, olarak tanımlanır. `Enrollments` `List<Enrollment>` Veya`HashSet<Enrollment>`gibi diğer koleksiyon türlerini kullanabilirsiniz. Zaman `ICollection<Enrollment>` olduğu EF Core kullanıldığında, oluşturur bir `HashSet<Enrollment>` varsayılan olarak koleksiyon.

## <a name="the-enrollment-entity"></a>Kayıt varlık

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

Aşağıdaki kodla *modeller/kayıt. cs* oluşturun:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

Bu özellik birincil anahtardır; bu varlık kendi başına değil, `classnameID` kendi modelini `ID` kullanır. `EnrollmentID` Bir üretim veri modeli için bir model seçin ve bunu tutarlı bir şekilde kullanın. Bu öğretici her ikisinin de yalnızca bir iş olduğunu göstermek için kullanır. `ID` Olmadan`classname` kullanmak, bazı veri modeli değişikliklerinin uygulanmasını kolaylaştırır.

`Grade` Özelliği bir `enum`. `Grade` Tür bildiriminden sonraki soru işareti, `Grade` özelliğin [null yapılabilir](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/)olduğunu gösterir. Null olan bir sınıf sıfır&mdash;bir sınıf null değerinden farklıdır veya henüz atanmamış olur.

`StudentID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`. Bir `Enrollment` varlıktır biriyle ilişkili `Student` tek bir özellik içerecek şekilde varlık `Student` varlık.

`CourseID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`. Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.

EF Core adlandırılmışsa, bu özellik bir yabancı anahtar olarak yorumlar `<navigation property name><primary key property name>`. Örneğin,`StudentID` nesnenin birincil anahtarı olduğundan `Student` `Student` `ID`, gezinti özelliği için yabancı anahtardır. Yabancı anahtar özellikleri de adı `<primary key property name>`. Örneğin, `CourseID` beri `Course` varlığın birincil anahtarı `CourseID`.

## <a name="the-course-entity"></a>Kurs varlık

![Kurs varlık diyagramı](intro/_static/course-entity.png)

Aşağıdaki kodla *modeller/kurs. cs* oluşturun:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

`Enrollments` Özelliktir bir gezinme özelliği. A `Course` varlık dilediğiniz sayıda ilgili olabileceğini `Enrollment` varlıklar.

`DatabaseGenerated` Özniteliği, uygulamanın veritabanını oluşturmak yerine birincil anahtarı belirtmesini sağlar.

Derleyici hatası olmadığını doğrulamak için projeyi derleyin.

## <a name="scaffold-student-pages"></a>Yapı iskelesi öğrenci sayfaları

Bu bölümde, oluşturmak için ASP.NET Core scafkatlama aracını kullanırsınız:

* EF Core *bağlamı* sınıfı. Bağlam, belirli bir veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır. `Microsoft.EntityFrameworkCore.DbContext` Sınıfından türetilir.
* `Student` Varlık için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini işleyen Razor sayfaları.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* *Sayfalar* klasöründe bir *öğrenciler* klasörü oluşturun.
* **Çözüm Gezgini**, *Sayfalar/öğrenciler* klasörüne sağ tıklayın ve **yeni yapı iskelesi** **Ekle** > öğesini seçin.
* İçinde **İskele Ekle** iletişim kutusunda **Entity Framework (CRUD) kullanarak Razor sayfaları** > **ekleme**.
* **Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunda:
  * İçinde **Model sınıfı** açılan listesinde, select **Öğrenci (ContosoUniversity.Models)** .
  * **Veri bağlamı sınıfı** satırında, **+** (artı) işaretini seçin.
  * *Contosouniversity. modeller. Contosoüniversıtycontext* olan veri bağlamı adını *Contosouniversity. Data. SchoolContext*olarak değiştirin.
  * **Add (Ekle)** seçeneğini belirleyin.

Aşağıdaki paketler otomatik olarak yüklenir:

* `Microsoft.VisualStudio.Web.CodeGeneration.Design`
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.Extensions.Logging.Debug`
* `Microsoft.EntityFrameworkCore.Tools`

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Gerekli NuGet paketlerini yüklemek için aşağıdaki .NET Core CLI komutlarını çalıştırın:

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.Tools --version 3.0.0-*
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
  dotnet add package Microsoft.Extensions.Logging.Debug --version 3.0.0-*
  ```

  Yapı iskelesi için Microsoft. VisualStudio. Web. CodeGeneration. Design paketi gereklidir. Uygulama SQL Server kullanmayabilse de, scafkatlama aracı SQL Server paketine ihtiyaç duyuyor.

* *Sayfalar/öğrenciler* klasörü oluşturun.

* [ASPNET-CodeGenerator scafkatlama aracı](xref:fundamentals/tools/dotnet-aspnet-codegenerator)'nı yüklemek için aşağıdaki komutu çalıştırın.

  ```dotnetcli
  dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
  ```

* Fkatlama öğrenci sayfalarını iskele almak için aşağıdaki komutu çalıştırın.

  **Windows 'da**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
  ```

  **MacOS veya Linux üzerinde**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
  ```

---

Önceki adımla ilgili bir sorununuz varsa, projeyi derleyin ve yapı iskelesi adımını yeniden deneyin.

Yapı iskelesi işlemi:

* *Sayfalar/öğrenciler* klasöründe Razor sayfaları oluşturur:
  * *. Cshtml* ve *Create.cshtml.cs* oluşturma
  * *Delete. cshtml* ve *delete.cshtml.cs*
  * *Details. cshtml* ve *details.cshtml.cs*
  * *. Cshtml* ve *Edit.cshtml.cs* Düzenle
  * *Index. cshtml* ve *Index.cshtml.cs*
* *Data/SchoolContext. cs*oluşturur.
* *Startup.cs*içinde bağımlılık eklenmesine bağlam ekler.
* *AppSettings. JSON*öğesine bir veritabanı bağlantı dizesi ekler.

## <a name="database-connection-string"></a>Veritabanı bağlantı dizesi

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Bağlantı dizesini belirtir [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb). 

[!code-json[Main](intro/samples/cu30/appsettings.json?highlight=11)]

LocalDB, SQL Server Express veritabanı Motoru'nu hafif bir sürümüdür ve uygulama geliştirme, üretim kullanımı için tasarlanmıştır. Varsayılan olarak, LocalDB `C:/Users/<user>` dizinde *. mdf* dosyaları oluşturur.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Bağlantı dizesini *cu. db*adlı bir SQLite veritabanı dosyasını işaret etmek üzere değiştirin:

[!code-json[Main](intro/samples/cu30/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a>Veritabanı bağlam sınıfını Güncelleştir

Belirli bir veri modeli için EF Core işlevselliğini koordine eden ana sınıf veritabanı bağlamı sınıfıdır. Bağlam [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir. Bağlam, veri modeline hangi varlıkların ekleneceğini belirtir. Bu projede adlı sınıfı `SchoolContext`.

Güncelleştirme *SchoolContext.cs* aşağıdaki kod ile:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

Vurgulanan kod oluşturur bir [olan DB\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) her varlık kümesi özelliği. EF Core terminolojisinde:

* Bir varlık kümesi, genellikle bir veritabanı tablosuna karşılık gelir.
* Bir varlık tablosunda bir satıra karşılık gelir.

Bir varlık kümesi birden çok varlık içerdiğinden, DBSet özellikleri çoğul adlar olmalıdır. Yapı iskelesi aracı bir`Student` dbset oluşturduğundan, bu adım çoğul `Students`olarak değişir. 

Razor Pages kodun yeni dbset adıyla eşleşmesini sağlamak için, tüm projesi `_context.Student` `_context.Students`genelinde genel bir değişiklik yapın.  8 oluşum vardır.

Derleyici hatası olmadığını doğrulamak için projeyi derleyin.

## <a name="startupcs"></a>Startup.cs

ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection). Hizmetler (EF Core veritabanı bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır. Bir veritabanı bağlamı örneğini alan Oluşturucu kodu öğreticide daha sonra gösterilmiştir.

Scafkatlama Aracı, bağlam sınıfını bağımlılık ekleme kapsayıcısına otomatik olarak kaydetti.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* ' `ConfigureServices`De, vurgulanan satırlar scaffolder tarafından eklenmiştir:

  [!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* ' `ConfigureServices`De, desteği tarafından eklenen kodun çağrılarından `UseSqlite`emin olun.

  [!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

---

Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.

## <a name="create-the-database"></a>Veritabanını oluşturma

Mevcut değilse veritabanını oluşturmak için *program.cs* güncelleştirin:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

Bağlam için bir veritabanı varsa, [Ensuyeniden](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) oluşturma yöntemi hiçbir eylemde bulunmaz. Veritabanı yoksa, veritabanını ve şemayı oluşturur. `EnsureCreated`veri modeli değişikliklerini işlemek için aşağıdaki iş akışını sunar:

* Veritabanını silin. Mevcut veriler kaybolur.
* Veri modelini değiştirin. Örneğin, bir `EmailAddress` alan ekleyin.
* Uygulamayı çalıştırın.
* `EnsureCreated`yeni şemaya sahip bir veritabanı oluşturur.

Bu iş akışı, verileri korumanıza gerek olmadığı sürece, şema hızlı bir şekilde gelişen zaman geliştirme aşamasında iyi bir şekilde gerçekleştirilir. Veritabanına girilen verilerin korunması gerektiğinde bu durum farklıdır. Bu durumda, geçişleri kullanın.

Öğretici serisinde daha sonra tarafından `EnsureCreated` oluşturulan veritabanını siler ve bunun yerine geçişleri kullanırsınız. Tarafından `EnsureCreated` oluşturulan bir veritabanı, geçişler kullanılarak güncelleştirilemiyor.

### <a name="test-the-app"></a>Uygulamayı test etme

* Uygulamayı çalıştırın.
* Seçin **Öğrenciler** bağlantısını ve ardından **Yeni Oluştur**.
* Ayrıntılar, düzenleme, test edin ve bağlantılarını silin.

## <a name="seed-the-database"></a>Veritabanının çekirdeğini oluşturma

`EnsureCreated` Yöntemi boş bir veritabanı oluşturur. Bu bölüm, veritabanını test verileriyle dolduran kodu ekler.

Aşağıdaki kodla *veri/Dbınizer. cs* oluşturun:

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  Kod, veritabanında herhangi bir öğrenci olup olmadığını denetler. Öğrenci yoksa, veritabanına test verileri ekler. Performansı iyileştirmek için `List<T>` koleksiyonlar yerine diziler halinde test verileri oluşturur.

* *Program.cs*içinde, `EnsureCreated` çağrıyı bir `DbInitializer.Initialize` çağrı ile değiştirin:

  ```csharp
  // context.Database.EnsureCreated();
  DbInitializer.Initialize(context);
  ````

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Çalışıyorsa uygulamayı durdurun ve **Paket Yöneticisi konsolunda** (PMC) aşağıdaki komutu çalıştırın:

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Çalışıyorsa uygulamayı durdurun ve *cu. db* dosyasını silin.

---

* Uygulamayı yeniden başlatın.

* Sağlanan verileri görmek için öğrenciler sayfasını seçin.

## <a name="view-the-database"></a>Veritabanını görüntüleme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Açık **SQL Server Nesne Gezgini** (SSOX) öğesinden **görünümü** Visual Studio'daki menü.
* SSOX 'te, **(LocalDB) \MSSQLLocalDB > veritabanları > SchoolContext-{GUID}** öğesini seçin. Veritabanı adı, daha önce belirttiğiniz bağlam adından ve bir tire ve bir GUID ile oluşturulur.
* Genişletin **tabloları** düğümü.
* Sağ **Öğrenci** tablosu ve'ı tıklatın **görünüm verilerini** oluşturulan sütunları ve tabloya eklenen satırları görebilirsiniz.
* `Student` Modelin `Student` tablo şemasına nasıl eşlendiğini görmek için **öğrenci** tablosuna sağ tıklayın ve **kodu görüntüle** ' ye tıklayın.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Veritabanı şemasını ve sağlanan verileri görüntülemek için SQLite aracınızı kullanın. Veritabanı dosyası *cu. db* olarak adlandırılır ve proje klasöründe bulunur.

---

## <a name="asynchronous-code"></a>Zaman uyumsuz kod

Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.

Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir. Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor. G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması. İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır. Sonuç olarak, zaman uyumsuz kod sunucu kaynaklarının daha verimli kullanılmasını sağlar ve sunucu gecikmeksizin daha fazla trafiği işleyebilir.

Zaman uyumsuz kod, çalışma zamanında az miktarda bir ek yükü sunar. Düşük trafiğe durumlar, performans düşüşüne yüksek trafik durumlar için göz ardı edilebilir, çalışırken, olası performans geliştirmesi önemli.

Aşağıdaki kodda, [zaman uyumsuz](/dotnet/csharp/language-reference/keywords/async) anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi zaman uyumsuz yürütülen kod olun.

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* `async` Anahtar sözcüğü, derleyiciye bildirir:
  * Yöntem gövdesini bölümleri için geri çağırmaları oluşturur.
  * Döndürülen [görev](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) nesnesini oluşturun.
* Dönüş `Task<T>` türü, devam eden işi temsil eder.
* `await` Anahtar sözcüğü, derleyicinin yöntemin iki parçalara bölmek neden olur. İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır. İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.
* `ToListAsync` zaman uyumsuz sürümüdür `ToList` genişletme yöntemi.

EF Core kullanan zaman uyumsuz kodu yazarken dikkat edilmesi gereken bazı noktalar şunlardır:

* Yalnızca sorguları veya komutlarının veritabanına gönderilmesine neden olan deyimler zaman uyumsuz olarak yürütülür. `ToListAsync` ,`SingleOrDefaultAsync`,, Ve`SaveChangesAsync`içerir. `FirstOrDefaultAsync` Yalnızca değiştirmek deyimleri içermeyen bir `IQueryable`, gibi `var students = context.Students.Where(s => s.LastName == "Davolio")`.
* EF Core bağlam iş parçacığı güvenli olmayan: paralel birden çok işlem yapmak yeniden denemeyin.
* Zaman uyumsuz kodun performans avantajlarından yararlanmak için, veritabanına sorgu gönderen EF Core yöntemleri çağırıyorsa kitaplık paketlerinin (örneğin, sayfalama için) zaman uyumsuz olarak kullanılacağını doğrulayın.

. NET'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz genel bakış](/dotnet/standard/async) ve [zaman uyumsuz programlama ile async ve await](/dotnet/csharp/programming-guide/concepts/async/).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="step-by-step"]
> [Sonraki öğretici](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Contoso University örnek web uygulamasını, Entity Framework (EF) çekirdek kullanarak bir ASP.NET Core Razor sayfalar uygulamasının nasıl oluşturulacağını gösterir.

Örnek uygulama, kurgusal Contoso üniversite için bir web sitesidir. Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir. Contoso University örnek uygulamasının nasıl oluşturulacağını açıklayan öğreticileri serisinin ilk sayfadır.

[İndirme veya tamamlanmış uygulamayı görüntüleyin.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Yükleme yönergeleri](xref:index#how-to-download-a-sample).

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE [](~/includes/2.1-SDK.md)]

---

Konusunda [Razor sayfaları](xref:razor-pages/index). Yeni programcılar tamamlamanız gereken [Razor sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) Bu seriyi başlatmadan önce.

## <a name="troubleshooting"></a>Sorun giderme

Bir sorunla karşılaşırsanız, çözümleyemiyor çalıştırırsanız, genel olarak çözüm kodunuzda karşılaştırarak bulabilirsiniz [projeyi](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples). Soru göndererek Yardım almak için en iyi yolu olan [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) için [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

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

Önceki adımlarda görüntüleri için bkz: [Razor web uygulaması oluşturma](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).
Uygulamayı çalıştırın.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```dotnetcli
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

---

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

`Enrollments` Özelliği bir [gezinti özelliği](/ef/core/modeling/relationships). Gezinti özellikleri bu varlıkla ilgili diğer varlıkları bağlayın. Bu durumda, `Enrollments` özelliği bir `Student entity` tüm tutan `Enrollment` olarak ilişkili varlıkları `Student`. Örneğin, bir öğrenci satır DB'de iki kayıt satırları ilgili olan `Enrollments` gezinti özelliği içerir, bu iki `Enrollment` varlıklar. İlgili `Enrollment` satırdır bu öğrencinin birincil anahtar değerini içeren bir satır `StudentID` sütun. Örneğin, Öğrenci kimlikli varsayalım = 1 olan iki satır `Enrollment` tablo. `Enrollment` Tablosunda var olan iki satır `StudentID` = 1. `StudentID` içinde bir yabancı anahtar `Enrollment` içinde Öğrenci belirten tablo `Student` tablo.

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

* İçinde **Model sınıfı** açılan listesinde, select **Öğrenci (ContosoUniversity.Models)** .
* İçinde **veri bağlamı sınıfının** satır, select **+** (artı) oturum açın ve oluşturulan bir adla değiştirin **ContosoUniversity.Models.SchoolContext**.
* İçinde **veri bağlamı sınıfının** açılan listesinde, select **ContosoUniversity.Models.SchoolContext**
* **Add (Ekle)** seçeneğini belirleyin.

![CRUD iletişim](intro/_static/s1.png)

Bkz: [film modeli iskelesini](xref:tutorials/razor-pages/model#scaffold-the-movie-model) önceki adımı ile ilgili bir sorun varsa.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Öğrenci modeli iskelesini için aşağıdaki komutları çalıştırın.

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

---

İskele işlem oluşturulur ve aşağıdaki dosya değişti:

### <a name="files-created"></a>Oluşturulan dosyalar

* *Sayfa/Öğrenciler* oluşturma, silme, Ayrıntılar, düzenleme, dizin.
* *Data/SchoolContext.cs*

### <a name="file-updates"></a>Dosya güncelleştirmeleri

* *Startup.cs* : Bu dosyada yapılan değişiklikler sonraki bölümde ayrıntılıdır.
* *appSettings. JSON* : Yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi eklenir.

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

Not: Önceki kod, yerine `Models` adalanı`namespace ContosoUniversity.Models`() için kullanır `Data`. `Models`, scaffolder tarafından oluşturulan kodla tutarlıdır. Daha fazla bilgi için bkz. [GitHub yapı iskelesi sorunu](https://github.com/aspnet/Scaffolding/issues/822).

Kod DB'de tüm Öğrenciler olup olmadığını denetler. DB'de Öğrenci varsa, bir veritabanı test verileri ile başlatılır. Diziye test verileri yükler yerine `List<T>` performansını iyileştirmek için koleksiyonları.

`EnsureCreated` Yöntemi DB bağlamı için bir veritabanı otomatik olarak oluşturur. Veritabanı varsa, `EnsureCreated` DB değiştirmeden döndürür.

İçinde *Program.cs*, değişiklik `Main` çağrılacak yöntem `Initialize`:

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Çalışıyorsa uygulamayı durdurun ve **Paket Yöneticisi konsolunda** (PMC) aşağıdaki komutu çalıştırın:

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Çalışıyorsa uygulamayı durdurun ve *cu. db* dosyasını silin.

---

## <a name="view-the-db"></a>DB görüntüleyin

Veritabanı adı, daha önce belirttiğiniz bağlam adından ve bir tire ve bir GUID ile oluşturulur. Bu nedenle, veritabanı adı "SchoolContext-{GUID}" olacaktır. GUID her kullanıcı için farklı olacaktır.
Açık **SQL Server Nesne Gezgini** (SSOX) öğesinden **görünümü** Visual Studio'daki menü.
SSOX 'te, **(LocalDB) \MSSQLLocalDB > veritabanları > SchoolContext-{GUID}** ' a tıklayın.

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

. NET'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz genel bakış](/dotnet/standard/async) ve [zaman uyumsuz programlama ile async ve await](/dotnet/csharp/programming-guide/concepts/async/).

Sonraki öğreticide, temel CRUD (oluşturma, okuma, güncelleştirme ve silme) işlemleri incelenir.



## <a name="additional-resources"></a>Ek kaynaklar

* [Bu öğreticinin YouTube sürümü](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [Next](xref:data/ef-rp/crud)

::: moniker-end
