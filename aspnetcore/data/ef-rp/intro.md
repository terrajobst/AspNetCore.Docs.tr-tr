---
title: ASP.NET Core - Öğreticisi 1. 8'de Entity Framework Core ile Razor sayfaları
author: rick-anderson
description: Entity Framework Core kullanan bir Razor sayfaları uygulamasının nasıl oluşturulacağını gösterir
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 09/26/2019
uid: data/ef-rp/intro
ms.openlocfilehash: 94783aa9014aef4c5f775fc8f36a2c3a7715e4b6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656823"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>ASP.NET Core - Öğreticisi 1. 8'de Entity Framework Core ile Razor sayfaları

, [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

::: moniker range=">= aspnetcore-3.0"

Bu, bir [ASP.NET Core Razor Pages](xref:razor-pages/index) uygulamasında ENTITY Framework (EF) çekirdeğini nasıl kullanacağınızı gösteren bir öğretici serisinin ilkisidir. Öğreticiler, kurgusal bir Contoso Üniversitesi için bir Web sitesi oluşturur. Site, öğrenci giriş, kurs oluşturma ve eğitmen atamaları gibi işlevleri içerir. Öğretici ilk kod yaklaşımını kullanır. Bu öğreticiyi veritabanı ilk yaklaşımı kullanarak takip eden bilgiler için [Bu GitHub sorununa](https://github.com/dotnet/AspNetCore.Docs/issues/16897)bakın.

[Tamamlanmış uygulamayı indirin veya görüntüleyin.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Yönergeleri indirin](xref:index#how-to-download-a-sample).

## <a name="prerequisites"></a>Önkoşullar

* Razor Pages yeni başladıysanız, bu duruma başlamadan önce Razor Pages öğretici serisini [kullanmaya](xref:tutorials/razor-pages/razor-pages-start) başlayın.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[VS prereqs](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[VS Code prereqs](~/includes/net-core-prereqs-vsc-3.0.md)]

---

## <a name="database-engines"></a>Veritabanı altyapıları

Visual Studio yönergeleri, yalnızca Windows üzerinde çalışan bir SQL Server Express sürümü olan [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)'yi kullanır.

Visual Studio Code yönergeler, platformlar arası bir veritabanı altyapısı olan [SQLite](https://www.sqlite.org/)kullanır.

SQLite kullanmayı seçerseniz, SQLite [Için DB tarayıcısı](https://sqlitebrowser.org/)gibi bir SQLite veritabanını yönetmek ve görüntülemek için üçüncü taraf bir araç indirip yükleyin.

## <a name="troubleshooting"></a>Sorun giderme

Giderebileceğiniz bir sorunla karşılaşırsanız, kodunuzu [Tamamlanan projeyle](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)karşılaştırın. Yardım almanın iyi bir yolu, [ASP.NET Core etiketi](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core etiketi](https://stackoverflow.com/questions/tagged/entity-framework-core)kullanılarak StackOverflow.com 'e bir soru göndererek.

## <a name="the-sample-app"></a>Örnek uygulama

Aşağıdaki öğreticilerde oluşturulan bir uygulamayı bir temel university web sitesidir. Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin. Öğreticide oluşturulan ekranlar birkaçını aşağıda verilmiştir.

![Öğrenciler dizin sayfası](intro/_static/students-index30.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit30.png)

Bu sitenin kullanıcı arabirimi stili yerleşik proje şablonlarına dayalıdır. Öğreticinin odağı, Kullanıcı arabirimini nasıl özelleştireceğinizi değil EF Core kullanma konusunda yer alır.

Tamamlanan projenin kaynak kodunu almak için sayfanın üst kısmındaki bağlantıyı izleyin. *Cu30* klasörü, öğreticinin ASP.NET Core 3,0 sürümü için kod içerir. 1-7 öğreticileri için kodun durumunu yansıtan dosyalar *cu30snapshots* klasöründe bulunabilir.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Tamamlanmış projeyi indirdikten sonra uygulamayı çalıştırmak için:

* Üç dosyayı ve ad içinde *SQLite* içeren bir klasörü silin.
* Projeyi oluşturun.
* Paket Yöneticisi konsolu 'nda (PMC) aşağıdaki komutu çalıştırın:

  ```powershell
  Update-Database
  ```

* Veritabanını temel alarak projeyi çalıştırın.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Tamamlanmış projeyi indirdikten sonra uygulamayı çalıştırmak için:

* *Contosouniversity. csproj*öğesini silin ve *Contosoüniversıtysqlite. csproj* öğesini *contosouniversity. csproj*olarak yeniden adlandırın.
* *Startup.cs*silin ve *StartupSQLite.cs* öğesini *Startup.cs*olarak yeniden adlandırın.
* *AppSettings. JSON*öğesini silin ve *Appsettingssqlite. JSON* öğesini *appSettings. JSON*olarak yeniden adlandırın.
* *Geçişler* klasörünü silin ve *migrationssql* öğesini *geçişlerle*yeniden adlandırın.
* Projeyi oluşturun.
* Proje klasöründeki bir komut isteminde aşağıdaki komutları çalıştırın:

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet ef database update
  ```

* SQLite aracında şu SQL ifadesini çalıştırın:

  ```sql
  UPDATE Department SET RowVersion = randomblob(8)
  ```

* Veritabanını temel alarak projeyi çalıştırın.

---

## <a name="create-the-web-app-project"></a>Web uygulaması projesi oluşturma

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio **Dosya** menüsünden **Yeni** > **projesi**' ni seçin.
* **ASP.NET Core Web uygulaması**' nı seçin.
* Projeyi *Contosouniversity*olarak adlandırın. Büyük harfler de dahil olmak üzere bu tam adı kullanmak önemlidir, bu nedenle kod kopyalanıp yapıştırılırken ad alanları eşleşir.
* Açılan menüden **.NET Core** ve **3,0 ASP.NET Core** seçin ve ardından **Web uygulaması**' nı seçin.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Bir terminalde, proje klasörünün oluşturulması gereken klasöre gidin.

* Razor Pages bir proje oluşturmak ve yeni proje klasörüne `cd` için aşağıdaki komutları çalıştırın:

  ```dotnetcli
  dotnet new webapp -o ContosoUniversity
  cd ContosoUniversity
  ```

---

## <a name="set-up-the-site-style"></a>Site stili Ayarla

*Sayfa/paylaşılan/_Layout. cshtml*'yi güncelleştirerek site üst bilgisini, alt bilgisini ve menüsünü ayarlayın:

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

`ID` özelliği, bu sınıfa karşılık gelen veritabanı tablosunun birincil anahtar sütunu olur. Varsayılan olarak, EF Core `ID` veya `classnameID` adında bir özelliği birincil anahtar olarak yorumlar. Bu nedenle, `Student` sınıfı birincil anahtarı için otomatik olarak tanınan ad `StudentID`.

`Enrollments` özelliği bir [Gezinti özelliğidir](/ef/core/modeling/relationships). Gezinti özellikleri, bu varlıkla ilgili diğer varlıkları tutar. Bu durumda, bir `Student` varlığının `Enrollments` özelliği söz konusu öğrenci ile ilgili `Enrollment` varlıkların tümünü barındırır. Örneğin, veritabanındaki bir öğrenci satırında iki ilişkili kayıt satırı varsa, `Enrollments` gezinti özelliği bu iki kayıt varlığını içerir. 

Veritabanında, bir kayıt satırı, Studentitıd sütunu öğrencinin ID değerini içeriyorsa bir öğrenci satırıyla ilgilidir. Örneğin, bir öğrenci satırının ID = 1 olduğunu varsayalım. İlgili kayıt satırları Studentitıd = 1 olacaktır. Studentitıd, kayıt tablosundaki bir *yabancı anahtardır* . 

Birden çok ilgili kayıt varlığı olabileceğinden `Enrollments` özelliği `ICollection<Enrollment>` olarak tanımlanır. `List<Enrollment>` veya `HashSet<Enrollment>`gibi başka koleksiyon türleri de kullanabilirsiniz. `ICollection<Enrollment>` kullanıldığında EF Core varsayılan olarak bir `HashSet<Enrollment>` koleksiyonu oluşturur.

## <a name="the-enrollment-entity"></a>Kayıt varlık

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

Aşağıdaki kodla *modeller/kayıt. cs* oluşturun:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

`EnrollmentID` özelliği birincil anahtardır; Bu varlık, `ID` yerine `classnameID` modelini kullanır. Bir üretim veri modeli için bir model seçin ve bunu tutarlı bir şekilde kullanın. Bu öğretici her ikisinin de yalnızca bir iş olduğunu göstermek için kullanır. `classname` olmadan `ID` kullanmak, bazı veri modeli değişikliklerinin uygulanmasını kolaylaştırır.

`Grade` özelliği bir `enum`. `Grade` türü bildiriminden sonraki soru işareti, `Grade` özelliğinin [null yapılabilir](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/)olduğunu gösterir. Null olan bir sınıf sıfır bir sınıfa göre farklılık gösterir&mdash;null, henüz bir sınıf bilinmediğini veya henüz atanmadığını belirtir.

`StudentID` özelliği bir yabancı anahtardır ve ilgili gezinti özelliği `Student`. `Enrollment` varlık bir `Student` varlığıyla ilişkilendirilir, bu nedenle özellik tek bir `Student` varlığı içerir.

`CourseID` özelliği bir yabancı anahtardır ve ilgili gezinti özelliği `Course`. Bir `Enrollment` varlığı bir `Course` varlığıyla ilişkilendirilir.

EF Core, `<navigation property name><primary key property name>`adında bir özelliği yabancı anahtar olarak yorumlar. Örneğin, `Student` varlığın birincil anahtarı `ID`olduğundan, `Student` gezinti özelliği için`StudentID` yabancı anahtardır. Yabancı anahtar özelliklerine de `<primary key property name>`adı verebilirsiniz. Örneğin, `Course` varlığın birincil anahtarı `CourseID`olduğundan `CourseID`.

## <a name="the-course-entity"></a>Kurs varlık

![Kurs varlık diyagramı](intro/_static/course-entity.png)

Aşağıdaki kodla *modeller/kurs. cs* oluşturun:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

`Enrollments` özelliği bir gezinti özelliğidir. `Course` bir varlık, herhangi bir sayıda `Enrollment` varlıkla ilişkili olabilir.

`DatabaseGenerated` özniteliği, uygulamanın veritabanını oluşturmak yerine birincil anahtarı belirtmesini sağlar.

Derleyici hatası olmadığını doğrulamak için projeyi derleyin.

## <a name="scaffold-student-pages"></a>Yapı iskelesi öğrenci sayfaları

Bu bölümde, oluşturmak için ASP.NET Core scafkatlama aracını kullanırsınız:

* EF Core *bağlamı* sınıfı. Bağlam, belirli bir veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır. `Microsoft.EntityFrameworkCore.DbContext` sınıfından türetilir.
* `Student` varlık için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini işleyen Razor sayfaları.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* *Sayfalar* klasöründe bir *öğrenciler* klasörü oluşturun.
* **Çözüm Gezgini**, *Sayfalar/öğrenciler* klasörüne sağ tıklayın ve > **yeni yapı iskelesi öğesi** **Ekle** ' yi seçin.
* **Yapı Iskelesi Ekle** iletişim kutusunda **Razor Pages Entity Framework (crud)** > **Ekle**' yi seçin.
* **Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunda:
  * **Model sınıfı** açılır penceresinde **öğrenci (Contosouniversity. modeller)** öğesini seçin.
  * **Veri bağlamı sınıfı** satırında **+** (artı) işaretini seçin.
  * *Contosouniversity. modeller. Contosoüniversıtycontext* olan veri bağlamı adını *Contosouniversity. Data. SchoolContext*olarak değiştirin.
  * **Add (Ekle)** seçeneğini belirleyin.

Aşağıdaki paketler otomatik olarak yüklenir:

* `Microsoft.VisualStudio.Web.CodeGeneration.Design`
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.Extensions.Logging.Debug`
* `Microsoft.EntityFrameworkCore.Tools`

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Gerekli NuGet paketlerini yüklemek için aşağıdaki .NET Core CLI komutlarını çalıştırın:
<!-- TO DO  After testing, Replace with
[!INCLUDE[](~/includes/includes/add-EF-NuGet-SQLite-CLI.md)]
remove dotnet tool install --global  below
 -->
  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SQLite
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet add package Microsoft.EntityFrameworkCore.Tools
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet add package Microsoft.Extensions.Logging.Debug
  ```

  Yapı iskelesi için Microsoft. VisualStudio. Web. CodeGeneration. Design paketi gereklidir. Uygulama SQL Server kullanmayabilse de, scafkatlama aracı SQL Server paketine ihtiyaç duyuyor.

* *Sayfalar/öğrenciler* klasörü oluşturun.

* [ASPNET-CodeGenerator scafkatlama aracı](xref:fundamentals/tools/dotnet-aspnet-codegenerator)'nı yüklemek için aşağıdaki komutu çalıştırın.

  ```dotnetcli
  dotnet tool install --global dotnet-aspnet-codegenerator
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

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Bağlantı dizesi [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)belirtir. 

[!code-json[Main](intro/samples/cu30/appsettings.json?highlight=11)]

LocalDB, SQL Server Express veritabanı Motoru'nu hafif bir sürümüdür ve uygulama geliştirme, üretim kullanımı için tasarlanmıştır. Varsayılan olarak, LocalDB `C:/Users/<user>` dizininde *. mdf* dosyaları oluşturur.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Bağlantı dizesini *cu. db*adlı bir SQLite veritabanı dosyasını işaret etmek üzere değiştirin:

[!code-json[Main](intro/samples/cu30/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a>Veritabanı bağlam sınıfını Güncelleştir

Belirli bir veri modeli için EF Core işlevselliğini koordine eden ana sınıf veritabanı bağlamı sınıfıdır. Bağlam [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir. Bağlam, veri modeline hangi varlıkların ekleneceğini belirtir. Bu projede, sınıfı `SchoolContext`olarak adlandırılır.

Aşağıdaki kodla *SchoolContext.cs* güncelleştirin:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

Vurgulanan kod, her bir varlık kümesi için bir [Dbset\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur. EF Core terminolojisinde:

* Bir varlık kümesi, genellikle bir veritabanı tablosuna karşılık gelir.
* Bir varlık tablosunda bir satıra karşılık gelir.

Bir varlık kümesi birden çok varlık içerdiğinden, DBSet özellikleri çoğul adlar olmalıdır. Scafkatlama aracı bir`Student` DBSet oluşturduğundan, bu adım bunu plural `Students`olarak değiştirir. 

Razor Pages kodun yeni DBSet adıyla eşleşmesini sağlamak için, tüm `_context.Student` projesi genelinde `_context.Students`için genel bir değişiklik yapın.  8 oluşum vardır.

Derleyici hatası olmadığını doğrulamak için projeyi derleyin.

## <a name="startupcs"></a>Startup.cs

ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur. Hizmetler (EF Core veritabanı bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır. Bir veritabanı bağlamı örneğini alan Oluşturucu kodu öğreticide daha sonra gösterilmiştir.

Scafkatlama Aracı, bağlam sınıfını bağımlılık ekleme kapsayıcısına otomatik olarak kaydetti.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* `ConfigureServices`, vurgulanan satırlar scaffolder tarafından eklenmiştir:

  [!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* `ConfigureServices`, desteği tarafından eklenen kodun `UseSqlite`çağırdığınızdan emin olun.

  [!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

---

Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.

## <a name="create-the-database"></a>Veritabanını oluşturma

Mevcut değilse veritabanını oluşturmak için *program.cs* güncelleştirin:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

Bağlam için bir veritabanı varsa, [Ensuyeniden](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) oluşturma yöntemi hiçbir eylemde bulunmaz. Veritabanı yoksa, veritabanını ve şemayı oluşturur. `EnsureCreated`, veri modeli değişikliklerini işlemek için aşağıdaki iş akışına izin vermez:

* Veritabanını silin. Mevcut veriler kaybolur.
* Veri modelini değiştirin. Örneğin, bir `EmailAddress` alanı ekleyin.
* Uygulamayı çalıştırın.
* `EnsureCreated` yeni şemaya sahip bir veritabanı oluşturur.

Bu iş akışı, verileri korumanıza gerek olmadığı sürece, şema hızlı bir şekilde gelişen zaman geliştirme aşamasında iyi bir şekilde gerçekleştirilir. Veritabanına girilen verilerin korunması gerektiğinde bu durum farklıdır. Bu durumda, geçişleri kullanın.

Öğretici serisinde daha sonra, `EnsureCreated` tarafından oluşturulan veritabanını silin ve bunun yerine geçişleri kullanın. `EnsureCreated` tarafından oluşturulan bir veritabanı, geçişler kullanılarak güncelleştirilemiyor.

### <a name="test-the-app"></a>Uygulamayı test edin

* Uygulamayı çalıştırın.
* **Öğrenciler** bağlantısını seçin ve ardından **Yeni oluştur**.
* Ayrıntılar, düzenleme, test edin ve bağlantılarını silin.

## <a name="seed-the-database"></a>Veritabanının çekirdeğini oluşturma

`EnsureCreated` yöntemi boş bir veritabanı oluşturur. Bu bölüm, veritabanını test verileriyle dolduran kodu ekler.

Aşağıdaki kodla *veri/Dbınizer. cs* oluşturun:

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  Kod, veritabanında herhangi bir öğrenci olup olmadığını denetler. Öğrenci yoksa, veritabanına test verileri ekler. Performansı iyileştirmek için `List<T>` koleksiyonları yerine diziler halinde test verileri oluşturur.

* *Program.cs*' de `EnsureCreated` çağrısını `DbInitializer.Initialize` çağrısıyla değiştirin:

  ```csharp
  // context.Database.EnsureCreated();
  DbInitializer.Initialize(context);
  ```

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Çalışıyorsa uygulamayı durdurun ve **Paket Yöneticisi konsolunda** (PMC) aşağıdaki komutu çalıştırın:

```powershell
Drop-Database
```

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Çalışıyorsa uygulamayı durdurun ve *cu. db* dosyasını silin.

---

* Uygulamayı yeniden başlatın.

* Sağlanan verileri görmek için öğrenciler sayfasını seçin.

## <a name="view-the-database"></a>Veritabanını görüntüleme

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio 'daki **Görünüm** menüsünden **SQL Server Nesne Gezgini** (ssox) öğesini açın.
* SSOX 'te, **(LocalDB) \MSSQLLocalDB > veritabanları > SchoolContext-{GUID}** öğesini seçin. Veritabanı adı, daha önce belirttiğiniz bağlam adından ve bir tire ve bir GUID ile oluşturulur.
* **Tables** düğümünü genişletin.
* Oluşturulan sütunları ve tabloya yerleştirilen satırları görmek için **öğrenci** tablosuna sağ tıklayın ve **verileri görüntüle** ' ye tıklayın.
* `Student` modelinin `Student` tablo şemasına nasıl eşlendiğini görmek için **öğrenci** tablosuna sağ tıklayın ve **kodu görüntüle** ' ye tıklayın.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Veritabanı şemasını ve sağlanan verileri görüntülemek için SQLite aracınızı kullanın. Veritabanı dosyası *cu. db* olarak adlandırılır ve proje klasöründe bulunur.

---

## <a name="asynchronous-code"></a>Zaman uyumsuz kod

Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.

Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir. Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor. G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması. İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır. Sonuç olarak, zaman uyumsuz kod sunucu kaynaklarının daha verimli kullanılmasını sağlar ve sunucu gecikmeksizin daha fazla trafiği işleyebilir.

Zaman uyumsuz kod, çalışma zamanında az miktarda bir ek yükü sunar. Düşük trafiğe durumlar, performans düşüşüne yüksek trafik durumlar için göz ardı edilebilir, çalışırken, olası performans geliştirmesi önemli.

Aşağıdaki kodda, [Async](/dotnet/csharp/language-reference/keywords/async) anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` metodu kodu zaman uyumsuz olarak yürütür.

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* `async` anahtar sözcüğü derleyiciye şunu söyler:
  * Yöntem gövdesini bölümleri için geri çağırmaları oluşturur.
  * Döndürülen [görev](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) nesnesini oluşturun.
* `Task<T>` dönüş türü devam eden çalışmayı temsil eder.
* `await` anahtar sözcüğü, derleyicinin yöntemi iki parçaya böetmesine neden olur. İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır. İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.
* `ToListAsync`, `ToList` uzantısı yönteminin zaman uyumsuz sürümüdür.

EF Core kullanan zaman uyumsuz kodu yazarken dikkat edilmesi gereken bazı noktalar şunlardır:

* Yalnızca sorguları veya komutlarının veritabanına gönderilmesine neden olan deyimler zaman uyumsuz olarak yürütülür. Bu `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`ve `SaveChangesAsync`içerir. `var students = context.Students.Where(s => s.LastName == "Davolio")`gibi yalnızca bir `IQueryable`değiştiren deyimler içermez.
* EF Core bağlam iş parçacığı güvenli olmayan: paralel birden çok işlem yapmak yeniden denemeyin.
* Zaman uyumsuz kodun performans avantajlarından yararlanmak için, veritabanına sorgu gönderen EF Core yöntemleri çağırıyorsa kitaplık paketlerinin (örneğin, sayfalama için) zaman uyumsuz olarak kullanılacağını doğrulayın.

.NET 'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. Async [and await ile](/dotnet/csharp/programming-guide/concepts/async/)zaman uyumsuz [genel bakış](/dotnet/standard/async) ve zaman uyumsuz programlama

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="step-by-step"]
> [Sonraki öğretici](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Contoso University örnek web uygulamasını, Entity Framework (EF) çekirdek kullanarak bir ASP.NET Core Razor sayfalar uygulamasının nasıl oluşturulacağını gösterir.

Örnek uygulama, kurgusal Contoso üniversite için bir web sitesidir. Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir. Contoso University örnek uygulamasının nasıl oluşturulacağını açıklayan öğreticileri serisinin ilk sayfadır.

[Tamamlanmış uygulamayı indirin veya görüntüleyin.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Yönergeleri indirin](xref:index#how-to-download-a-sample).

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE [](~/includes/2.1-SDK.md)]

---

[Razor Pages](xref:razor-pages/index)hakkında benzerlik. Yeni programcılar, bu seriyi başlatmadan önce [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) ' i tamamlamalıdır.

## <a name="troubleshooting"></a>Sorun giderme

Çözemiyoruz bir sorunla karşılaşırsanız, kodunuzun [Tamamlanan projeyle](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)karşılaştırılmasıyla genellikle çözümü bulabilirsiniz. [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core) [için bir](https://stackoverflow.com/questions/tagged/asp.net-core) soru göndererek yardım almanın iyi bir yolu.

## <a name="the-contoso-university-web-app"></a>Contoso University web uygulaması

Aşağıdaki öğreticilerde oluşturulan bir uygulamayı bir temel university web sitesidir.

Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin. Öğreticide oluşturulan ekranlar birkaçını aşağıda verilmiştir.

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit.png)

Bu sitenin UI Stili yerleşik şablonları tarafından üretilen yakın ' dir. EF çekirdekli Razor sayfaları, UI ile öğretici odağı açıktır.

## <a name="create-the-contosouniversity-razor-pages-web-app"></a>ContosoUniversity Razor sayfaları web uygulaması oluşturma

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio **Dosya** menüsünden **Yeni** > **projesi**' ni seçin.
* Yeni bir ASP.NET Core Web uygulaması oluşturun. Projeyi **Contosouniversity**olarak adlandırın. Kod kopyalama/yapıştırma olduğunda, ad alanlarının eşleşmesi için *Contosouniversity* projesini adlandırmak önemlidir.
* Açılan listede **ASP.NET Core 2,1** ' i seçin ve ardından **Web uygulaması**' nı seçin.

Yukarıdaki adımların görüntüleri için bkz. [Razor Web uygulaması oluşturma](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).
Uygulamayı çalıştırın.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```dotnetcli
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

---

## <a name="set-up-the-site-style"></a>Site stili Ayarla

Birkaç değişiklik site menü, Düzen ve giriş sayfası ayarlayın. *Sayfaları/paylaşılan/_Layout. cshtml* 'yi aşağıdaki değişikliklerle güncelleştirin:

* "Contoso Üniversitesi" için "ContosoUniversity" her örneğini değiştirin. Üç örnekleri vardır.

* **Öğrenciler**, **Kurslar**, **eğitmenler**ve **Departmanlar**için menü girişleri ekleyin ve **kişi** menü girişini silin.

Değişiklikler vurgulanır. (Tüm *biçimlendirme gösterilmez.* )

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

*Pages/Index. cshtml*dosyasında, ASP.net ve MVC hakkındaki metni bu uygulamayla ilgili metinle değiştirmek için dosyanın içeriğini aşağıdaki kodla değiştirin:

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a>Veri modeli oluşturma

Varlık sınıflarının Contoso University uygulama oluşturun. Aşağıdaki üç varlıklarla başlayın:

![Kurs kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

`Student` ve `Enrollment` varlıkları arasında bire çok ilişki vardır. `Course` ve `Enrollment` varlıkları arasında bire çok ilişki vardır. Bir öğrenci herhangi bir sayıda kursları kaydedebilirsiniz. Bir kurs herhangi bir sayıda Öğrenciler içinde kayıtlı olabilir.

Aşağıdaki bölümlerde, bu varlıkların her biri için bir sınıf oluşturulur.

### <a name="the-student-entity"></a>Öğrenci varlık

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

*Modeller* klasörü oluşturun. *Modeller* klasöründe, *Student.cs* adlı bir sınıf dosyasını aşağıdaki kodla oluşturun:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

`ID` özelliği, bu sınıfa karşılık gelen veritabanı (DB) tablosunun birincil anahtar sütunu olur. Varsayılan olarak, EF Core `ID` veya `classnameID` adında bir özelliği birincil anahtar olarak yorumlar. `classnameID`, `classname` sınıfın adıdır. Diğer otomatik olarak tanınan birincil anahtar, önceki örnekte `StudentID`.

`Enrollments` özelliği bir [Gezinti özelliğidir](/ef/core/modeling/relationships). Gezinti özellikleri bu varlıkla ilgili diğer varlıkları bağlayın. Bu durumda, bir `Student entity` `Enrollments` özelliği, bu `Student`ilgili `Enrollment` varlıkların tümünü barındırır. Örneğin, VERITABANıNDAKI bir öğrenci satırında iki ilişkili kayıt satırı varsa `Enrollments` gezinti özelliği bu iki `Enrollment` varlığını içerir. İlgili `Enrollment` satırı, `StudentID` sütununda öğrencinin birincil anahtar değerini içeren bir satırdır. Örneğin, ID = 1 olan öğrencinin `Enrollment` tablosunda iki satır olduğunu varsayalım. `Enrollment` tablosu `StudentID` = 1 olan iki satıra sahiptir. `StudentID`, `Student` tablosundaki öğrencisi belirten `Enrollment` tablosundaki bir yabancı anahtardır.

Bir gezinti özelliği birden çok varlık tutabileceğinden, gezinti özelliği `ICollection<T>`gibi bir liste türü olmalıdır. `ICollection<T>` belirtilebilir veya `List<T>` veya `HashSet<T>`gibi bir tür olabilir. `ICollection<T>` kullanıldığında EF Core varsayılan olarak bir `HashSet<T>` koleksiyonu oluşturur. Birden çok varlık tutun Gezinti özellikleri çoktan çoğa ve bire çok ilişkileri gelir.

### <a name="the-enrollment-entity"></a>Kayıt varlık

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

*Modeller* klasöründe, aşağıdaki kodla *enrollment.cs* oluşturun:

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` özelliği birincil anahtardır. Bu varlık `Student` varlığı gibi `ID` yerine `classnameID` modelini kullanır. Genellikle geliştiriciler bir düzen seçin ve veri modelini kullanın. Bir sonraki öğreticide, classname Kimliğini kullanarak, veri modelinde aktarma uygulamak daha kolay hale getirmek için gösterilir.

`Grade` özelliği bir `enum`. `Grade` türü bildiriminden sonraki soru işareti, `Grade` özelliğinin null yapılabilir olduğunu gösterir. Boş bir sınıf bir sıfır sınıf farklı--null anlamına gelir bir sınıf bilinen değil veya henüz atanmamış.

`StudentID` özelliği bir yabancı anahtardır ve ilgili gezinti özelliği `Student`. `Enrollment` varlık bir `Student` varlığıyla ilişkilendirilir, bu nedenle özellik tek bir `Student` varlığı içerir. `Student` varlığı, birden çok `Enrollment` varlığı içeren `Student.Enrollments` gezinti özelliğinden farklıdır.

`CourseID` özelliği bir yabancı anahtardır ve ilgili gezinti özelliği `Course`. Bir `Enrollment` varlığı bir `Course` varlığıyla ilişkilendirilir.

EF Core, `<navigation property name><primary key property name>`adında bir özelliği yabancı anahtar olarak yorumlar. Örneğin, `Student` varlığının birincil anahtarı `ID`olduğundan, `Student` gezinti özelliği için`StudentID`. Yabancı anahtar özelliklerine de `<primary key property name>`adı verebilirsiniz. Örneğin, `Course` varlığın birincil anahtarı `CourseID`olduğundan `CourseID`.

### <a name="the-course-entity"></a>Kurs varlık

![Kurs varlık diyagramı](intro/_static/course-entity.png)

*Modeller* klasöründe, aşağıdaki kodla *Course.cs* oluşturun:

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

`Enrollments` özelliği bir gezinti özelliğidir. `Course` bir varlık, herhangi bir sayıda `Enrollment` varlıkla ilişkili olabilir.

`DatabaseGenerated` özniteliği, uygulamanın DB oluşturmak yerine birincil anahtarı belirtmesini sağlar.

## <a name="scaffold-the-student-model"></a>İskele Öğrenci modeli

Bu bölümde, Öğrenci modeli iskele kurulmuş. Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik Öğrenci modeli oluşturur.

* Projeyi oluşturun.
* *Sayfalar/öğrenciler* klasörünü oluşturun.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Çözüm Gezgini**, *sayfalar/öğrenciler* > klasörüne sağ tıklayıp **Yeni > Scalankatan öğe** **Ekle** ' ye tıklayın.
* **Yapı Iskelesi Ekle** iletişim kutusunda **Razor Pages Entity Framework (crud)** > **Ekle**' yi seçin.

**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:

* **Model sınıfı** açılır penceresinde **öğrenci (Contosouniversity. modeller)** öğesini seçin.
* **Veri bağlamı sınıfı** satırında **+** (artı) işaretini seçin ve üretilen adı **Contosouniversity. modeller. SchoolContext**olarak değiştirin.
* **Veri bağlamı sınıfı** açılır penceresinde **Contosouniversity. modeller. SchoolContext** öğesini seçin.
* **Add (Ekle)** seçeneğini belirleyin.

![CRUD iletişim](intro/_static/s1.png)

Önceki adımla ilgili bir sorununuz varsa [Film modeli](xref:tutorials/razor-pages/model#scaffold-the-movie-model) ' ne bakın.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Öğrenci modeli iskelesini için aşağıdaki komutları çalıştırın.

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

---

İskele işlem oluşturulur ve aşağıdaki dosya değişti:

### <a name="files-created"></a>Oluşturulan dosyalar

* *Sayfalar/öğrenciler* Oluşturma, silme, ayrıntılar, düzenleme, dizin oluşturma.
* *Data/SchoolContext. cs*

### <a name="file-updates"></a>Dosya güncelleştirmeleri

* *Startup.cs* : Bu dosyadaki değişiklikler sonraki bölümde ayrıntılıdır.
* *appSettings. JSON* : yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi eklenir.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Bağımlılık ekleme ile kayıtlı bağlamını İnceleme

ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur. Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır. Bir db bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.

Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.

*Startup.cs*içinde `ConfigureServices` yöntemini inceleyin. Vurgulanan satırı iskele kurucu tarafından eklendi:

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.

## <a name="update-main"></a>Ana güncelleştir

*Program.cs*' de, aşağıdakileri yapmak için `Main` yöntemini değiştirin:

* Bir DB bağlamı örneği bağımlılık ekleme kapsayıcısını alın.
* Yeniden [oluşturulmasını](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated)çağırın.
* `EnsureCreated` yöntemi tamamlandığında bağlamı atın.

Aşağıdaki kod güncelleştirilmiş *program.cs* dosyasını gösterir.

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

`EnsureCreated`, bağlam veritabanının mevcut olmasını sağlar. Varsa, hiçbir işlem yapılmaz. Yoksa, veritabanı ve tüm şema oluşturulur. `EnsureCreated`, veritabanını oluşturmak için geçişleri kullanmaz. `EnsureCreated` ile oluşturulan bir veritabanı daha sonra geçişler kullanılarak güncelleştirilemez.

`EnsureCreated`, aşağıdaki iş akışına izin veren uygulama başlatma sırasında çağrılır:

* DB silin.
* DB şemasını değiştirin (örneğin, bir `EmailAddress` alanı ekleyin).
* Uygulamayı çalıştırın.
* `EnsureCreated`,`EmailAddress` sütunuyla bir VERITABANı oluşturur.

şema hızlı bir şekilde gelişen zaman geliştirme sürecinde `EnsureCreated`. Daha sonra öğreticide DB silinir ve geçişler kullanılır.

### <a name="test-the-app"></a>Uygulamayı test edin

Uygulamayı çalıştırın ve tanımlama bilgisi ilkesini kabul edin. Bu uygulama, kişisel bilgileri tutmak değil. [Ab genel veri koruma yönetmeliği (GDPR) desteğiyle](xref:security/gdpr)ilgili tanımlama bilgisi İlkesi hakkında bilgi edinebilirsiniz.

* **Öğrenciler** bağlantısını seçin ve ardından **Yeni oluştur**.
* Ayrıntılar, düzenleme, test edin ve bağlantılarını silin.

## <a name="examine-the-schoolcontext-db-context"></a>SchoolContext DB bağlamını İnceleme

Verilen veri modeli için EF Core işlevselliği koordine eden ana DB bağlamı sınıfının sınıftır. Veri bağlamı [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir. Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir. Bu projede, sınıfı `SchoolContext`olarak adlandırılır.

Aşağıdaki kodla *SchoolContext.cs* güncelleştirin:

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

Vurgulanan kod, her bir varlık kümesi için bir [Dbset\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur. EF Core terminolojisinde:

* Bir varlık kümesini genellikle DB tabloya karşılık gelir.
* Bir varlık tablosunda bir satıra karşılık gelir.

`DbSet<Enrollment>` ve `DbSet<Course>` atlanamaz. EF Core, `Student` varlık `Enrollment` varlığına başvurduğundan ve `Enrollment` varlığı `Course` varlığına başvurduğundan bunları örtülü olarak içerir. Bu öğretici için `SchoolContext``DbSet<Enrollment>` ve `DbSet<Course>` tutun.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Bağlantı dizesi [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)belirtir. LocalDB, SQL Server Express veritabanı Motoru'nu hafif bir sürümüdür ve uygulama geliştirme, üretim kullanımı için tasarlanmıştır. LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır. Varsayılan olarak, LocalDB `C:/Users/<user>` dizininde *. mdf* DB dosyaları oluşturur.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Bir veritabanı test verileri ile başlatmak için kod ekleyin

EF Core boş bir veritabanı oluşturur. Bu bölümde, test verileriyle doldurmak için bir `Initialize` yöntemi yazılır.

*Veri* klasöründe, *DbInitializer.cs* adlı yeni bir sınıf dosyası oluşturun ve aşağıdaki kodu ekleyin:

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

Note: Yukarıdaki kod `Data`yerine ad alanı (`namespace ContosoUniversity.Models`) için `Models` kullanır. `Models`, scaffolder tarafından oluşturulan kodla tutarlıdır. Daha fazla bilgi için bkz. [GitHub yapı iskelesi sorunu](https://github.com/aspnet/Scaffolding/issues/822).

Kod DB'de tüm Öğrenciler olup olmadığını denetler. DB'de Öğrenci varsa, bir veritabanı test verileri ile başlatılır. Performansı iyileştirmek için test verilerini `List<T>` koleksiyonlar yerine dizilere yükler.

`EnsureCreated` yöntemi DB bağlamı için otomatik olarak DB oluşturur. VERITABANı varsa, `EnsureCreated` DB 'yi değiştirmeden döndürür.

*Program.cs*içinde, `Main` yöntemini `Initialize`çağrısı olacak şekilde değiştirin:

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Çalışıyorsa uygulamayı durdurun ve **Paket Yöneticisi konsolunda** (PMC) aşağıdaki komutu çalıştırın:

```powershell
Drop-Database
```

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Çalışıyorsa uygulamayı durdurun ve *cu. db* dosyasını silin.

---

## <a name="view-the-db"></a>DB görüntüleyin

Veritabanı adı, daha önce belirttiğiniz bağlam adından ve bir tire ve bir GUID ile oluşturulur. Bu nedenle, veritabanı adı "SchoolContext-{GUID}" olacaktır. GUID her kullanıcı için farklı olacaktır.
Visual Studio 'daki **Görünüm** menüsünden **SQL Server Nesne Gezgini** (ssox) öğesini açın.
SSOX 'te, **(LocalDB) \MSSQLLocalDB > veritabanları > SchoolContext-{GUID}** ' a tıklayın.

**Tables** düğümünü genişletin.

Oluşturulan sütunları ve tabloya yerleştirilen satırları görmek için **öğrenci** tablosuna sağ tıklayın ve **verileri görüntüle** ' ye tıklayın.

## <a name="asynchronous-code"></a>Zaman uyumsuz kod

Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.

Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir. Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor. G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması. İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır. Sonuç olarak, sunucu kaynaklarının daha etkin kullanılması zaman uyumsuz kod sağlar ve sunucu gecikmeler olmadan daha fazla trafik işlemek için etkinleştirilir.

Zaman uyumsuz kod, çalışma zamanında az miktarda bir ek yükü sunar. Düşük trafiğe durumlar, performans düşüşüne yüksek trafik durumlar için göz ardı edilebilir, çalışırken, olası performans geliştirmesi önemli.

Aşağıdaki kodda, [Async](/dotnet/csharp/language-reference/keywords/async) anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` metodu kodu zaman uyumsuz olarak yürütür.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* `async` anahtar sözcüğü derleyiciye şunu söyler:
  * Yöntem gövdesini bölümleri için geri çağırmaları oluşturur.
  * Döndürülen [görev](/dotnet/api/system.threading.tasks.task) nesnesini otomatik olarak oluşturun. Daha fazla bilgi için bkz. [görev dönüş türü](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* Örtük dönüş türü `Task` devam eden işi temsil eder.
* `await` anahtar sözcüğü, derleyicinin yöntemi iki parçaya böetmesine neden olur. İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır. İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.
* `ToListAsync`, `ToList` uzantısı yönteminin zaman uyumsuz sürümüdür.

EF Core kullanan zaman uyumsuz kodu yazarken dikkat edilmesi gereken bazı noktalar şunlardır:

* Sorguları veya Veritabanına gönderilecek komutları neden deyimleri zaman uyumsuz olarak yürütülür. Bu, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`ve `SaveChangesAsync`içerir. `var students = context.Students.Where(s => s.LastName == "Davolio")`gibi yalnızca bir `IQueryable`değiştiren deyimler içermez.
* EF Core bağlam iş parçacığı güvenli olmayan: paralel birden çok işlem yapmak yeniden denemeyin.
* Zaman uyumsuz kodun performans avantajlarından yararlanmak için bunlar için bir veritabanı sorguları göndermek EF Core yöntemleri çağırırsanız kitaplığı paketlerinin (disk belleği sunamıyoruz gibi) zaman uyumsuz kullandığını doğrulayın.

.NET 'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. Async [and await ile](/dotnet/csharp/programming-guide/concepts/async/)zaman uyumsuz [genel bakış](/dotnet/standard/async) ve zaman uyumsuz programlama

Sonraki öğreticide, temel CRUD (oluşturma, okuma, güncelleştirme ve silme) işlemleri incelenir.



## <a name="additional-resources"></a>Ek kaynaklar

* [Bu öğreticinin YouTube sürümü](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [Next](xref:data/ef-rp/crud)

::: moniker-end
