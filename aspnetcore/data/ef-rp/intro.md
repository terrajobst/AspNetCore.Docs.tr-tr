---
title: ASP.NET Core Entity Framework Core ile Razor Pages-1-8 öğretici
author: rick-anderson
description: Entity Framework Core kullanarak Razor Pages uygulamasının nasıl oluşturulacağını gösterir
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 09/26/2019
uid: data/ef-rp/intro
ms.openlocfilehash: 01e507326ddd57057aa178ad3909fd4027a013fd
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259368"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>ASP.NET Core Entity Framework Core ile Razor Pages-1-8 öğretici

, [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

::: moniker range=">= aspnetcore-3.0"

Bu, bir [ASP.NET Core Razor Pages](xref:razor-pages/index) uygulamasında ENTITY Framework (EF) çekirdeğini nasıl kullanacağınızı gösteren bir öğretici serisinin ilkisidir. Öğreticiler, kurgusal bir Contoso Üniversitesi için bir Web sitesi oluşturur. Site, öğrenci giriş, kurs oluşturma ve eğitmen atamaları gibi işlevleri içerir.

[Tamamlanmış uygulamayı indirin veya görüntüleyin.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Yönergeleri indirin](xref:index#how-to-download-a-sample).

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

Bu öğreticilerde oluşturulan uygulama, temel bir üniversite web sitesidir. Kullanıcılar öğrenci, kurs ve eğitmen bilgilerini görüntüleyebilir ve güncelleştirebilir. Öğreticide oluşturulan ekranlardan bazıları aşağıda verilmiştir.

![Öğrenciler Dizin sayfası](intro/_static/students-index30.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit30.png)

Bu sitenin kullanıcı arabirimi stili yerleşik proje şablonlarına dayalıdır. Öğreticinin odağı, Kullanıcı arabirimini nasıl özelleştireceğinizi değil EF Core kullanma konusunda yer alır.

Tamamlanan projenin kaynak kodunu almak için sayfanın üst kısmındaki bağlantıyı izleyin. *Cu30* klasörü, öğreticinin ASP.NET Core 3,0 sürümü için kod içerir. 1-7 öğreticileri için kodun durumunu yansıtan dosyalar *cu30snapshots* klasöründe bulunabilir.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Tamamlanmış projeyi indirdikten sonra uygulamayı çalıştırmak için:

* Üç dosyayı ve ad içinde *SQLite* içeren bir klasörü silin.
* Projeyi derleyin.
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
* Projeyi derleyin.
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

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.
* **ASP.NET Core Web uygulaması**' nı seçin.
* Projeyi *Contosouniversity*olarak adlandırın. Büyük harfler de dahil olmak üzere bu tam adı kullanmak önemlidir, bu nedenle kod kopyalanıp yapıştırılırken ad alanları eşleşir.
* Açılan menüden **.NET Core** ve **3,0 ASP.NET Core** seçin ve ardından **Web uygulaması**' nı seçin.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Bir terminalde, proje klasörünün oluşturulması gereken klasöre gidin.

* Bir Razor Pages projesi oluşturmak için aşağıdaki komutları çalıştırın ve yeni proje klasörüne `cd`:

  ```dotnetcli
  dotnet new webapp -o ContosoUniversity
  cd ContosoUniversity
  ```

---

## <a name="set-up-the-site-style"></a>Site stilini ayarlayın

*Sayfaları/paylaşılan/_Layout. cshtml*'yi güncelleştirerek site üst bilgisini, alt bilgisini ve menüsünü ayarlayın:

* "ContosoUniversity" öğesinin her oluşumunu "Contoso Üniversitesi" olarak değiştirin. Üç oluşum vardır.

* **Giriş** ve **Gizlilik** menü girişlerini silin ve **hakkında**, **öğrenciler**, **Kurslar**, **eğitmenler**ve **Departmanlar**için girişler ekleyin.

Değişiklikler vurgulanır.

[!code-cshtml[Main](intro/samples/cu30/Pages/Shared/_Layout.cshtml?highlight=6,14,21-35,49)]

*Pages/Index. cshtml*dosyasında, ASP.NET Core hakkındaki metni bu uygulamayla ilgili metinle değiştirmek için dosyanın içeriğini aşağıdaki kodla değiştirin:

[!code-cshtml[Main](intro/samples/cu30/Pages/Index.cshtml)]

Giriş sayfasının göründüğünü doğrulamak için uygulamayı çalıştırın.

## <a name="the-data-model"></a>Veri modeli

Aşağıdaki bölümler bir veri modeli oluşturur:

![Kurs-kayıt-öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

Bir öğrenci herhangi bir sayıda kursa kaydolabilir ve bir kurs, kayıtlı sayıda öğrenciye sahip olabilir.

## <a name="the-student-entity"></a>Öğrenci varlığı

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

* Proje klasöründe bir *modeller* klasörü oluşturun. 

* Aşağıdaki kodla *modeller/öğrenci. cs* oluşturun:

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Student.cs)]

@No__t-0 özelliği, bu sınıfa karşılık gelen veritabanı tablosunun birincil anahtar sütunu olur. Varsayılan olarak, EF Core `ID` veya `classnameID` adlı bir özelliği birincil anahtar olarak yorumlar. Bu nedenle, `Student` sınıfı birincil anahtarı için otomatik olarak tanınan ad `StudentID` ' dir.

@No__t-0 özelliği bir [Gezinti özelliğidir](/ef/core/modeling/relationships). Gezinti özellikleri, bu varlıkla ilgili diğer varlıkları tutar. Bu durumda, bir `Student` varlığının `Enrollments` özelliği, söz konusu öğrenciye ilişkin tüm @no__t 2 varlıklarını barındırır. Örneğin, veritabanındaki bir öğrenci satırında iki ilişkili kayıt satırı varsa, `Enrollments` gezinti özelliği bu iki kayıt varlığını içerir. 

Veritabanında, bir kayıt satırı, Studentitıd sütunu öğrencinin ID değerini içeriyorsa bir öğrenci satırıyla ilgilidir. Örneğin, bir öğrenci satırının ID = 1 olduğunu varsayalım. İlgili kayıt satırları Studentitıd = 1 olacaktır. Studentitıd, kayıt tablosundaki bir *yabancı anahtardır* . 

@No__t-0 özelliği, birden çok ilgili kayıt varlığı olabileceğinden, `ICollection<Enrollment>` olarak tanımlanır. @No__t-0 veya `HashSet<Enrollment>` gibi diğer koleksiyon türlerini kullanabilirsiniz. @No__t-0 kullanıldığında, EF Core varsayılan olarak bir `HashSet<Enrollment>` koleksiyonu oluşturur.

## <a name="the-enrollment-entity"></a>Kayıt varlığı

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

Aşağıdaki kodla *modeller/kayıt. cs* oluşturun:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

@No__t-0 özelliği birincil anahtardır; Bu varlık, `ID` yerine `classnameID` modelini kullanır. Bir üretim veri modeli için bir model seçin ve bunu tutarlı bir şekilde kullanın. Bu öğretici her ikisinin de yalnızca bir iş olduğunu göstermek için kullanır. @No__t-1 olmadan @no__t kullanılması, bazı veri modeli değişikliklerinin uygulanmasını kolaylaştırır.

@No__t-0 özelliği bir `enum` ' dir. @No__t-0 tür bildiriminden sonraki soru işareti, `Grade` özelliğinin [null yapılabilir](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/)olduğunu gösterir. Null olan bir sınıf, 0 ' dan farklı bir şekilde ayarlanır @ no__t-0null, bir sınıf bilinmiyor veya henüz atanmamış anlamına gelir.

@No__t-0 özelliği bir yabancı anahtardır ve karşılık gelen gezinti özelliği `Student` ' dir. @No__t-0 bir varlık bir `Student` varlıkla ilişkilendirilir, bu nedenle özellik tek bir `Student` varlığı içerir.

@No__t-0 özelliği bir yabancı anahtardır ve karşılık gelen gezinti özelliği `Course` ' dir. @No__t-0 varlığı, bir `Course` varlığıyla ilişkilidir.

EF Core, `<navigation property name><primary key property name>` olarak adlandırılmışsa bir özelliği yabancı anahtar olarak yorumlar. Örneğin, `StudentID`, `Student` varlığının birincil anahtarı `ID` olduğundan, `Student` gezinti özelliği için yabancı anahtardır. Yabancı anahtar özellikleri, `<primary key property name>` olarak da adlandırılabilir. Örneğin, `Course` varlığının birincil anahtarı `CourseID` olduğundan `CourseID`.

## <a name="the-course-entity"></a>Kurs varlığı

![Kurs varlık diyagramı](intro/_static/course-entity.png)

Aşağıdaki kodla *modeller/kurs. cs* oluşturun:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

@No__t-0 özelliği bir gezinti özelliğidir. @No__t-0 varlığı, herhangi bir sayıda `Enrollment` varlıkla ilişkili olabilir.

@No__t-0 özniteliği, uygulamanın veritabanını oluşturmak yerine birincil anahtarı belirtmesini sağlar.

Derleyici hatası olmadığını doğrulamak için projeyi derleyin.

## <a name="scaffold-student-pages"></a>Yapı iskelesi öğrenci sayfaları

Bu bölümde, oluşturmak için ASP.NET Core scafkatlama aracını kullanırsınız:

* EF Core *bağlamı* sınıfı. Bağlam, belirli bir veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır. @No__t-0 sınıfından türetilir.
* @No__t-0 varlığı için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini işleyen Razor sayfaları.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* *Sayfalar* klasöründe bir *öğrenciler* klasörü oluşturun.
* **Çözüm Gezgini**, *Sayfalar/öğrenciler* klasörüne sağ tıklayın ve **Ekle** > **yeni yapı iskelesi öğesini**seçin.
* **Yapı Iskelesi Ekle** iletişim kutusunda, **Entity Framework (crud)** > **Ekle**' yi kullanarak Razor Pages seçin.
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

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

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

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Bağlantı dizesi [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)belirtir. 

[!code-json[Main](intro/samples/cu30/appsettings.json?highlight=11)]

LocalDB, SQL Server Express veritabanı altyapısının hafif bir sürümüdür ve üretim kullanımı için değil uygulama geliştirmeye yöneliktir. Varsayılan olarak, LocalDB `C:/Users/<user>` dizininde *. mdf* dosyaları oluşturur.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Bağlantı dizesini *cu. db*adlı bir SQLite veritabanı dosyasını işaret etmek üzere değiştirin:

[!code-json[Main](intro/samples/cu30/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a>Veritabanı bağlam sınıfını Güncelleştir

Belirli bir veri modeli için EF Core işlevselliğini koordine eden ana sınıf veritabanı bağlamı sınıfıdır. Bağlam [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir. Bağlam, veri modeline hangi varlıkların ekleneceğini belirtir. Bu projede, sınıfı `SchoolContext` olarak adlandırılmıştır.

Aşağıdaki kodla *SchoolContext.cs* güncelleştirin:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

Vurgulanan kod, her bir varlık kümesi için bir [Dbset @ no__t-1TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur. EF Core terminoloji:

* Bir varlık kümesi, genellikle bir veritabanı tablosuna karşılık gelir.
* Bir varlık, tablodaki bir satıra karşılık gelir.

Bir varlık kümesi birden çok varlık içerdiğinden, DBSet özellikleri çoğul adlar olmalıdır. Scafkatlama aracı bir @ no__t-0 DBSet oluşturduğundan, bu adım bunu plural `Students` olarak değiştirir. 

Razor Pages kodun yeni DBSet adıyla eşleşmesini sağlamak için, `_context.Student` ' ın tüm projesi genelinde bir global değişiklik yapın `_context.Students`.  8 oluşum vardır.

Derleyici hatası olmadığını doğrulamak için projeyi derleyin.

## <a name="startupcs"></a>Startup.cs

ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur. Hizmetler (EF Core veritabanı bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu hizmetleri gerektiren bileşenler (örneğin Razor Pages), bu hizmetleri Oluşturucu parametreleri aracılığıyla sağlamaktadır. Bir veritabanı bağlamı örneğini alan Oluşturucu kodu öğreticide daha sonra gösterilmiştir.

Scafkatlama Aracı, bağlam sınıfını bağımlılık ekleme kapsayıcısına otomatik olarak kaydetti.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* @No__t-0 ' da, vurgulanan satırlar scaffolder tarafından eklenmiştir:

  [!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* @No__t-0 ' da, desteği tarafından eklenen kodun `UseSqlite` ' i çağırıyor olduğundan emin olun.

  [!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

---

Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.

## <a name="create-the-database"></a>Veritabanını oluşturma

Mevcut değilse veritabanını oluşturmak için *program.cs* güncelleştirin:

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

Bağlam için bir veritabanı varsa, [Ensuyeniden](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) oluşturma yöntemi hiçbir eylemde bulunmaz. Veritabanı yoksa, veritabanını ve şemayı oluşturur. `EnsureCreated`, veri modeli değişikliklerini işlemek için aşağıdaki iş akışına izin vermez:

* Veritabanını silin. Mevcut veriler kaybolur.
* Veri modelini değiştirin. Örneğin, `EmailAddress` alanı ekleyin.
* Uygulamayı çalıştırın.
* `EnsureCreated`, yeni şemaya sahip bir veritabanı oluşturur.

Bu iş akışı, verileri korumanıza gerek olmadığı sürece, şema hızlı bir şekilde gelişen zaman geliştirme aşamasında iyi bir şekilde gerçekleştirilir. Veritabanına girilen verilerin korunması gerektiğinde bu durum farklıdır. Bu durumda, geçişleri kullanın.

Öğretici serisinde daha sonra, `EnsureCreated` tarafından oluşturulan veritabanını silin ve bunun yerine geçişleri kullanın. @No__t-0 tarafından oluşturulan bir veritabanı, geçişler kullanılarak güncelleştirilemiyor.

### <a name="test-the-app"></a>Uygulamayı test edin

* Uygulamayı çalıştırın.
* **Öğrenciler** bağlantısını seçin ve ardından **Yeni oluştur**.
* Düzenleme, Ayrıntılar ve silme bağlantılarını test edin.

## <a name="seed-the-database"></a>Veritabanını çekirdek

@No__t-0 yöntemi boş bir veritabanı oluşturur. Bu bölüm, veritabanını test verileriyle dolduran kodu ekler.

Aşağıdaki kodla *veri/Dbınizer. cs* oluşturun:

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  Kod, veritabanında herhangi bir öğrenci olup olmadığını denetler. Öğrenci yoksa, veritabanına test verileri ekler. Performansı iyileştirmek için `List<T>` koleksiyonları yerine diziler halinde test verileri oluşturur.

* *Program.cs*' de `EnsureCreated` çağrısını `DbInitializer.Initialize` çağrısıyla değiştirin:

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

* Visual Studio 'daki **Görünüm** menüsünden **SQL Server Nesne Gezgini** (ssox) öğesini açın.
* SSOX 'te, **(LocalDB) \MSSQLLocalDB > veritabanları > SchoolContext-{GUID}** öğesini seçin. Veritabanı adı, daha önce belirttiğiniz bağlam adından ve bir tire ve bir GUID ile oluşturulur.
* **Tables** düğümünü genişletin.
* Oluşturulan sütunları ve tabloya yerleştirilen satırları görmek için **öğrenci** tablosuna sağ tıklayın ve **verileri görüntüle** ' ye tıklayın.
* @No__t-2 modelinin `Student` tablo şemasına nasıl eşlendiğini görmek için **öğrenci** tablosuna sağ tıklayın ve **kodu görüntüle** ' ye tıklayın.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Veritabanı şemasını ve sağlanan verileri görüntülemek için SQLite aracınızı kullanın. Veritabanı dosyası *cu. db* olarak adlandırılır ve proje klasöründe bulunur.

---

## <a name="asynchronous-code"></a>Zaman uyumsuz kod

Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.

Web sunucusunda sınırlı sayıda iş parçacığı bulunur ve yüksek yük durumlarında tüm kullanılabilir iş parçacıkları kullanımda olabilir. Bu durumda, sunucu, iş parçacıkları boşaltılana kadar yeni istekleri işleyemez. Zaman uyumlu kodla, çok sayıda iş parçacığı, g/ç 'nin tamamlanmasını beklediği için aslında herhangi bir iş yapmadıklarında bağlı olabilir. Zaman uyumsuz kod ile, bir işlem g/ç 'yi tamamlanmayı beklerken, sunucunun diğer istekleri işlemek için kullanması için iş parçacığı serbest bırakılır. Sonuç olarak, zaman uyumsuz kod sunucu kaynaklarının daha verimli kullanılmasını sağlar ve sunucu gecikmeksizin daha fazla trafiği işleyebilir.

Zaman uyumsuz kod, çalışma zamanında az miktarda yük getirir. Düşük trafik durumlarında, performans artışı göz ardı edilebilir, ancak yüksek trafik durumları için olası performans iyileştirmesi oldukça önemlidir.

Aşağıdaki kodda, [Async](/dotnet/csharp/language-reference/keywords/async) anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi kodu zaman uyumsuz olarak yürütür.

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* @No__t-0 anahtar sözcüğü derleyiciye şunu söyler:
  * Yöntem gövdesinin parçaları için geri çağrılar oluşturun.
  * Döndürülen [görev](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) nesnesini oluşturun.
* @No__t-0 dönüş türü devam eden işi temsil eder.
* @No__t-0 anahtar sözcüğü, derleyicinin yöntemi iki parçaya böetmesine neden olur. İlk bölüm, zaman uyumsuz olarak başlatılan işlemle biter. İkinci bölüm, işlem tamamlandığında çağrılan bir geri çağırma yöntemine konur.
* `ToListAsync`, `ToList` Genişletme yönteminin zaman uyumsuz sürümüdür.

EF Core kullanan zaman uyumsuz kodu yazarken dikkat edilmesi gereken bazı şeyler:

* Yalnızca sorguları veya komutlarının veritabanına gönderilmesine neden olan deyimler zaman uyumsuz olarak yürütülür. Bu `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` ve `SaveChangesAsync` içerir. Yalnızca `var students = context.Students.Where(s => s.LastName == "Davolio")` gibi @no__t (0) değiştiren deyimler içermez.
* EF Core bağlamı iş parçacığı açısından güvenli değildir: paralel olarak birden çok işlem yapmayı denemeyin.
* Zaman uyumsuz kodun performans avantajlarından yararlanmak için, veritabanına sorgu gönderen EF Core yöntemleri çağırıyorsa kitaplık paketlerinin (örneğin, sayfalama için) zaman uyumsuz olarak kullanılacağını doğrulayın.

.NET 'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. Async [and await ile](/dotnet/csharp/programming-guide/concepts/async/)zaman uyumsuz [genel bakış](/dotnet/standard/async) ve zaman uyumsuz programlama

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="step-by-step"]
> [Sonraki öğretici](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Contoso Üniversitesi örnek Web uygulaması, Entity Framework (EF) çekirdeğini kullanarak bir ASP.NET Core Razor Pages uygulamasının nasıl oluşturulacağını gösterir.

Örnek uygulama, kurgusal bir Contoso Üniversitesi için bir Web sitesidir. Öğrenci giriş, kurs oluşturma ve eğitmen atamaları gibi işlevleri içerir. Bu sayfa, Contoso Üniversitesi örnek uygulamasının nasıl oluşturulacağını açıklayan bir öğretici serisinin ilkisidir.

[Tamamlanmış uygulamayı indirin veya görüntüleyin.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Yönergeleri indirin](xref:index#how-to-download-a-sample).

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE [](~/includes/2.1-SDK.md)]

---

[Razor Pages](xref:razor-pages/index)hakkında benzerlik. Yeni programcılar, bu seriyi başlatmadan önce [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) ' i tamamlamalıdır.

## <a name="troubleshooting"></a>Sorun giderme

Çözemiyoruz bir sorunla karşılaşırsanız, kodunuzun [Tamamlanan projeyle](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)karşılaştırılmasıyla genellikle çözümü bulabilirsiniz. [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core) [için bir](https://stackoverflow.com/questions/tagged/asp.net-core) soru göndererek yardım almanın iyi bir yolu.

## <a name="the-contoso-university-web-app"></a>Contoso Üniversitesi web uygulaması

Bu öğreticilerde oluşturulan uygulama, temel bir üniversite web sitesidir.

Kullanıcılar öğrenci, kurs ve eğitmen bilgilerini görüntüleyebilir ve güncelleştirebilir. Öğreticide oluşturulan ekranlardan bazıları aşağıda verilmiştir.

![Öğrenciler Dizin sayfası](intro/_static/students-index.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit.png)

Bu sitenin kullanıcı arabirimi stili yerleşik şablonlar tarafından üretilme kadar yakın. Öğretici odağı, kullanıcı arabiriminden değil, Razor Pages EF Core.

## <a name="create-the-contosouniversity-razor-pages-web-app"></a>ContosoUniversity Razor Pages Web uygulaması oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.
* Yeni bir ASP.NET Core Web uygulaması oluşturun. Projeyi **Contosouniversity**olarak adlandırın. Kod kopyalama/yapıştırma olduğunda, ad alanlarının eşleşmesi için *Contosouniversity* projesini adlandırmak önemlidir.
* Açılan listede **ASP.NET Core 2,1** ' i seçin ve ardından **Web uygulaması**' nı seçin.

Yukarıdaki adımların görüntüleri için bkz. [Razor Web uygulaması oluşturma](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).
Uygulamayı çalıştırın.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```dotnetcli
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

---

## <a name="set-up-the-site-style"></a>Site stilini ayarlayın

Site menüsünü, düzeni ve giriş sayfasını birkaç değişiklik ayarlar. *Pages/Shared/_Layout. cshtml* dosyasını aşağıdaki değişikliklerle güncelleştirin:

* "ContosoUniversity" öğesinin her oluşumunu "Contoso Üniversitesi" olarak değiştirin. Üç oluşum vardır.

* **Öğrenciler**, **Kurslar**, **eğitmenler**ve **Departmanlar**için menü girişleri ekleyin ve **kişi** menü girişini silin.

Değişiklikler vurgulanır. (Tüm *biçimlendirme gösterilmez.* )

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

*Pages/Index. cshtml*dosyasında, ASP.net ve MVC hakkındaki metni bu uygulamayla ilgili metinle değiştirmek için dosyanın içeriğini aşağıdaki kodla değiştirin:

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a>Veri modeli oluşturma

Contoso Üniversitesi uygulaması için varlık sınıfları oluşturma. Aşağıdaki üç varlıkla başlayın:

![Kurs-kayıt-öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

@No__t-0 ve `Enrollment` varlıkları arasında bire çok ilişki vardır. @No__t-0 ve `Enrollment` varlıkları arasında bire çok ilişki vardır. Bir öğrenci herhangi bir sayıda kursa kaydolabilir. Bir kurs, kayıtlı sayıda öğrenciye sahip olabilir.

Aşağıdaki bölümlerde, bu varlıkların her biri için bir sınıf oluşturulur.

### <a name="the-student-entity"></a>Öğrenci varlığı

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

*Modeller* klasörü oluşturun. *Modeller* klasöründe, *Student.cs* adlı bir sınıf dosyasını aşağıdaki kodla oluşturun:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

@No__t-0 özelliği, bu sınıfa karşılık gelen veritabanı (DB) tablosunun birincil anahtar sütunu olur. Varsayılan olarak, EF Core `ID` veya `classnameID` adlı bir özelliği birincil anahtar olarak yorumlar. @No__t-0 ' da, `classname` ' in sınıfın adıdır. Diğer otomatik olarak tanınan birincil anahtar, önceki örnekte `StudentID` ' dır.

@No__t-0 özelliği bir [Gezinti özelliğidir](/ef/core/modeling/relationships). Gezinti özellikleri bu varlıkla ilgili diğer varlıkların bağlantısını sağlar. Bu durumda, bir `Student entity` ' in `Enrollments` özelliği, bu `Student` ile ilgili @no__t 2 varlıkların tümünü barındırır. Örneğin, VERITABANıNDAKI bir öğrenci satırında iki ilişkili kayıt satırı varsa `Enrollments` gezinti özelliği bu iki `Enrollment` varlığını içerir. İlgili `Enrollment` satırı, `StudentID` sütununda öğrencinin birincil anahtar değerini içeren bir satırdır. Örneğin, ID = 1 olan öğrencinin `Enrollment` tablosunda iki satıra sahip olduğunu varsayalım. @No__t-0 tablosunda `StudentID` = 1 olan iki satır vardır. `StudentID`, `Student` tablosunda öğrenci belirten `Enrollment` tablosundaki bir yabancı anahtardır.

Bir gezinti özelliği birden çok varlık tutabileceğinden, gezinti özelliği `ICollection<T>` gibi bir liste türü olmalıdır. `ICollection<T>` veya `List<T>` veya `HashSet<T>` gibi bir tür olabilir. @No__t-0 kullanıldığında, EF Core varsayılan olarak bir `HashSet<T>` koleksiyonu oluşturur. Birden çok varlığı tutan gezinti özellikleri, çoktan çoğa ve bire çok ilişkilerden gelir.

### <a name="the-enrollment-entity"></a>Kayıt varlığı

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

*Modeller* klasöründe, aşağıdaki kodla *enrollment.cs* oluşturun:

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

@No__t-0 özelliği birincil anahtardır. Bu varlık `Student` varlığı gibi `ID` yerine `classnameID` modelini kullanır. Genellikle geliştiriciler bir model seçer ve bunu veri modeli boyunca kullanır. Daha sonraki bir öğreticide, veri modelinde devralmayı daha kolay hale getirmek için ClassName olmadan ID kullanımı gösterilmemiştir.

@No__t-0 özelliği bir `enum` ' dir. @No__t-0 tür bildiriminden sonraki soru işareti, `Grade` özelliğinin null yapılabilir olduğunu gösterir. Null olan bir sınıf sıfır bir sınıf ile farklıdır--null, henüz bir sınıf bilinmediğini veya henüz atanmadığını belirtir.

@No__t-0 özelliği bir yabancı anahtardır ve karşılık gelen gezinti özelliği `Student` ' dir. @No__t-0 bir varlık bir `Student` varlıkla ilişkilendirilir, bu nedenle özellik tek bir `Student` varlığı içerir. @No__t-0 varlığı, birden çok `Enrollment` varlık içeren `Student.Enrollments` gezinti özelliğinden farklıdır.

@No__t-0 özelliği bir yabancı anahtardır ve karşılık gelen gezinti özelliği `Course` ' dir. @No__t-0 varlığı, bir `Course` varlığıyla ilişkilidir.

EF Core, `<navigation property name><primary key property name>` olarak adlandırılmışsa bir özelliği yabancı anahtar olarak yorumlar. Örneğin, `Student` varlığının birincil anahtarı `ID` olduğundan, `Student` gezinti özelliği için `StudentID`. Yabancı anahtar özellikleri, `<primary key property name>` olarak da adlandırılabilir. Örneğin, `Course` varlığının birincil anahtarı `CourseID` olduğundan `CourseID`.

### <a name="the-course-entity"></a>Kurs varlığı

![Kurs varlık diyagramı](intro/_static/course-entity.png)

*Modeller* klasöründe, aşağıdaki kodla *Course.cs* oluşturun:

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

@No__t-0 özelliği bir gezinti özelliğidir. @No__t-0 varlığı, herhangi bir sayıda `Enrollment` varlıkla ilişkili olabilir.

@No__t-0 özniteliği, uygulamanın DB 'nin oluşturmasını sağlamak yerine birincil anahtarı belirtmesini sağlar.

## <a name="scaffold-the-student-model"></a>Öğrenci modelini dolandırın

Bu bölümde öğrenci modeli scafkatdır. Diğer bir deyişle, scafkatlama aracı öğrenci modeli için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri için sayfalar üretir.

* Projeyi derleyin.
* *Sayfalar/öğrenciler* klasörünü oluşturun.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Çözüm Gezgini**, *Sayfalar/öğrenciler* klasörüne sağ tıklayarak > **yeni yapı iskelesi** **ekleyin** >.
* **Yapı Iskelesi Ekle** iletişim kutusunda, **Entity Framework (crud)** > **Ekle**' yi kullanarak Razor Pages seçin.

**Entity Framework kullanarak Razor Pages Ekle (CRUD)** iletişim kutusunu doldurun:

* **Model sınıfı** açılır penceresinde **öğrenci (Contosouniversity. modeller)** öğesini seçin.
* **Veri bağlamı sınıfı** satırında **+** (artı) işaretini seçin ve üretilen adı **Contosouniversity. modeller. SchoolContext**olarak değiştirin.
* **Veri bağlamı sınıfı** açılır penceresinde **Contosouniversity. modeller. SchoolContext** öğesini seçin.
* **Add (Ekle)** seçeneğini belirleyin.

![CRUD iletişim kutusu](intro/_static/s1.png)

Önceki adımla ilgili bir sorununuz varsa [Film modeli](xref:tutorials/razor-pages/model#scaffold-the-movie-model) ' ne bakın.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Öğrenci modelini iskele almak için aşağıdaki komutları çalıştırın.

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

---

Yapı iskelesi işlemi oluşturulur ve aşağıdaki dosyaları değiştirdi:

### <a name="files-created"></a>Oluşturulan dosyalar

* *Sayfalar/öğrenciler* Oluşturma, silme, ayrıntılar, düzenleme, dizin oluşturma.
* *Data/SchoolContext. cs*

### <a name="file-updates"></a>Dosya güncelleştirmeleri

* *Startup.cs* : Bu dosyadaki değişiklikler sonraki bölümde ayrıntılıdır.
* *appSettings. JSON* : yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi eklenir.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Bağımlılık ekleme ile kaydedilen bağlamı inceleyin

ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection)ile oluşturulmuştur. Hizmetler (EF Core DB bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu hizmetleri gerektiren bileşenler (örneğin Razor Pages), bu hizmetleri Oluşturucu parametreleri aracılığıyla sağlamaktadır. Bir DB bağlam örneğini alan Oluşturucu kodu öğreticide daha sonra gösterilmiştir.

Scafkatlama aracı otomatik olarak bir DB bağlamı oluşturup bağımlılık ekleme kapsayıcısına kaydettirdi.

*Startup.cs*içinde `ConfigureServices` yöntemini inceleyin. Vurgulanan satır, scaffolder tarafından eklendi:

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.

## <a name="update-main"></a>Ana güncelleştirme

*Program.cs*' de `Main` yöntemini aşağıdaki şekilde değiştirin:

* Bağımlılık ekleme kapsayıcısından bir DB bağlam örneği alın.
* Yeniden [oluşturulmasını](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated)çağırın.
* @No__t-0 yöntemi tamamlandığında bağlamı atın.

Aşağıdaki kod güncelleştirilmiş *program.cs* dosyasını gösterir.

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

`EnsureCreated`, bağlam veritabanının mevcut olmasını sağlar. Varsa, hiçbir eylem yapılmaz. Yoksa, veritabanı ve tüm şeması oluşturulur. `EnsureCreated`, veritabanını oluşturmak için geçişleri kullanmaz. @No__t-0 ile oluşturulan bir veritabanı daha sonra geçişler kullanılarak güncelleştirilemez.

`EnsureCreated`, uygulama başlatma sırasında çağrılır ve bu, aşağıdaki iş akışına izin verir:

* VERITABANıNı silin.
* DB şemasını değiştirin (örneğin, `EmailAddress` alanı ekleyin).
* Uygulamayı çalıştırın.
* `EnsureCreated`, @ no__t-1 sütunuyla bir VERITABANı oluşturur.

şema hızlı bir şekilde gelişmede `EnsureCreated`, geliştirmede daha erken bir yoldur. Öğreticide daha sonra DB silinir ve geçişler kullanılır.

### <a name="test-the-app"></a>Uygulamayı test edin

Uygulamayı çalıştırın ve tanımlama bilgisi ilkesini kabul edin. Bu uygulama, kişisel bilgileri saklar. [Ab genel veri koruma yönetmeliği (GDPR) desteğiyle](xref:security/gdpr)ilgili tanımlama bilgisi İlkesi hakkında bilgi edinebilirsiniz.

* **Öğrenciler** bağlantısını seçin ve ardından **Yeni oluştur**.
* Düzenleme, Ayrıntılar ve silme bağlantılarını test edin.

## <a name="examine-the-schoolcontext-db-context"></a>SchoolContext DB bağlamını inceleyin

Belirli bir veri modeli için EF Core işlevselliğini koordine eden ana sınıf DB bağlam sınıfıdır. Veri bağlamı [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir. Veri bağlamı, veri modeline hangi varlıkların ekleneceğini belirtir. Bu projede, sınıfı `SchoolContext` olarak adlandırılmıştır.

Aşağıdaki kodla *SchoolContext.cs* güncelleştirin:

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

Vurgulanan kod, her bir varlık kümesi için bir [Dbset @ no__t-1TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur. EF Core terminoloji:

* Bir varlık kümesi, genellikle bir DB tablosuna karşılık gelir.
* Bir varlık, tablodaki bir satıra karşılık gelir.

`DbSet<Enrollment>` ve `DbSet<Course>` atlanamaz. EF Core, `Student` varlığı `Enrollment` varlığına başvurduğundan ve `Enrollment` varlığı `Course` varlığına başvurduğundan bunları örtülü olarak içerir. Bu öğreticide `DbSet<Enrollment>` ve `DbSet<Course>` ' i `SchoolContext` ' de tutun.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Bağlantı dizesi [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)belirtir. LocalDB, SQL Server Express veritabanı altyapısının hafif bir sürümüdür ve üretim kullanımı için değil uygulama geliştirmeye yöneliktir. LocalDB, istek üzerine başlar ve kullanıcı modunda çalışır, bu nedenle karmaşık bir yapılandırma yoktur. Varsayılan olarak, LocalDB `C:/Users/<user>` dizininde *. mdf* DB dosyaları oluşturur.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Test verileriyle VERITABANıNı başlatmak için kod ekleme

EF Core boş bir VERITABANı oluşturur. Bu bölümde, test verileriyle doldurmak için bir `Initialize` yöntemi yazılır.

*Veri* klasöründe, *DbInitializer.cs* adlı yeni bir sınıf dosyası oluşturun ve aşağıdaki kodu ekleyin:

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

Note: Yukarıdaki kod `Data` yerine ad alanı için (`namespace ContosoUniversity.Models`) `Models` kullanır. `Models`, scaffolder tarafından oluşturulan kodla tutarlıdır. Daha fazla bilgi için bkz. [GitHub yapı iskelesi sorunu](https://github.com/aspnet/Scaffolding/issues/822).

Kod, VERITABANıNDA herhangi bir öğrenci olup olmadığını denetler. Veritabanında hiç öğrenci yoksa, DB test verileriyle başlatılır. Performansı iyileştirmek için, test verilerini `List<T>` koleksiyonları yerine dizilere yükler.

@No__t-0 yöntemi DB bağlamı için otomatik olarak DB oluşturur. VERITABANı varsa, DB 'yi değiştirmeden `EnsureCreated` döndürür.

*Program.cs*' de `Main` yöntemini `Initialize` ' i çağırmak üzere değiştirin:

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Çalışıyorsa uygulamayı durdurun ve **Paket Yöneticisi konsolunda** (PMC) aşağıdaki komutu çalıştırın:

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Çalışıyorsa uygulamayı durdurun ve *cu. db* dosyasını silin.

---

## <a name="view-the-db"></a>VERITABANıNı görüntüleme

Veritabanı adı, daha önce belirttiğiniz bağlam adından ve bir tire ve bir GUID ile oluşturulur. Bu nedenle, veritabanı adı "SchoolContext-{GUID}" olacaktır. GUID her kullanıcı için farklı olacaktır.
Visual Studio 'daki **Görünüm** menüsünden **SQL Server Nesne Gezgini** (ssox) öğesini açın.
SSOX 'te, **(LocalDB) \MSSQLLocalDB > veritabanları > SchoolContext-{GUID}** ' a tıklayın.

**Tables** düğümünü genişletin.

Oluşturulan sütunları ve tabloya yerleştirilen satırları görmek için **öğrenci** tablosuna sağ tıklayın ve **verileri görüntüle** ' ye tıklayın.

## <a name="asynchronous-code"></a>Zaman uyumsuz kod

Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.

Web sunucusunda sınırlı sayıda iş parçacığı bulunur ve yüksek yük durumlarında tüm kullanılabilir iş parçacıkları kullanımda olabilir. Bu durumda, sunucu, iş parçacıkları boşaltılana kadar yeni istekleri işleyemez. Zaman uyumlu kodla, çok sayıda iş parçacığı, g/ç 'nin tamamlanmasını beklediği için aslında herhangi bir iş yapmadıklarında bağlı olabilir. Zaman uyumsuz kod ile, bir işlem g/ç 'yi tamamlanmayı beklerken, sunucunun diğer istekleri işlemek için kullanması için iş parçacığı serbest bırakılır. Sonuç olarak, zaman uyumsuz kod sunucu kaynaklarının daha verimli kullanılmasını sağlar ve sunucu, gecikme olmadan daha fazla trafiği işlemeye etkinleştirilir.

Zaman uyumsuz kod, çalışma zamanında az miktarda yük getirir. Düşük trafik durumlarında, performans artışı göz ardı edilebilir, ancak yüksek trafik durumları için olası performans iyileştirmesi oldukça önemlidir.

Aşağıdaki kodda, [Async](/dotnet/csharp/language-reference/keywords/async) anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi kodu zaman uyumsuz olarak yürütür.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* @No__t-0 anahtar sözcüğü derleyiciye şunu söyler:
  * Yöntem gövdesinin parçaları için geri çağrılar oluşturun.
  * Döndürülen [görev](/dotnet/api/system.threading.tasks.task) nesnesini otomatik olarak oluşturun. Daha fazla bilgi için bkz. [görev dönüş türü](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* @No__t-0 örtülü dönüş türü, devam eden işi temsil eder.
* @No__t-0 anahtar sözcüğü, derleyicinin yöntemi iki parçaya böetmesine neden olur. İlk bölüm, zaman uyumsuz olarak başlatılan işlemle biter. İkinci bölüm, işlem tamamlandığında çağrılan bir geri çağırma yöntemine konur.
* `ToListAsync`, `ToList` Genişletme yönteminin zaman uyumsuz sürümüdür.

EF Core kullanan zaman uyumsuz kodu yazarken dikkat edilmesi gereken bazı şeyler:

* Yalnızca sorguları veya komutlarının VERITABANıNA gönderilmesine neden olan deyimler zaman uyumsuz olarak yürütülür. Bu, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` ve `SaveChangesAsync` içerir. Yalnızca `var students = context.Students.Where(s => s.LastName == "Davolio")` gibi @no__t (0) değiştiren deyimler içermez.
* EF Core bağlamı iş parçacığı açısından güvenli değildir: paralel olarak birden çok işlem yapmayı denemeyin.
* Zaman uyumsuz kodun performans avantajlarından yararlanmak için, VERITABANıNA sorgu gönderen EF Core yöntemlerini çağırıyorsa kitaplık paketlerinin (örneğin, sayfalama için) zaman uyumsuz olarak kullanılacağını doğrulayın.

.NET 'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. Async [and await ile](/dotnet/csharp/programming-guide/concepts/async/)zaman uyumsuz [genel bakış](/dotnet/standard/async) ve zaman uyumsuz programlama

Sonraki öğreticide, temel CRUD (oluşturma, okuma, güncelleştirme, silme) işlemleri incelenir.



## <a name="additional-resources"></a>Ek kaynaklar

* [Bu öğreticinin YouTube sürümü](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [İleri](xref:data/ef-rp/crud)

::: moniker-end
