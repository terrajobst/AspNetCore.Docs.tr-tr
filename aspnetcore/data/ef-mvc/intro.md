---
title: Entity Framework Core - öğretici 1 / 10 ile ASP.NET Core MVC
author: rick-anderson
description: ''
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-mvc/intro
ms.openlocfilehash: 6f867875ed670065ef76728342d3854281f08ae4
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50091073"
---
# <a name="aspnet-core-mvc-with-entity-framework-core---tutorial-1-of-10"></a>Entity Framework Core - öğretici 1 / 10 ile ASP.NET Core MVC

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc.md)]

Contoso University örnek web uygulaması, Entity Framework (EF) Core 2.0 ve Visual Studio 2017 kullanarak ASP.NET Core 2.0 MVC web uygulamalarının nasıl oluşturulacağını gösterir.

Örnek, bir web sitesi için kurgusal Contoso üniversite uygulamasıdır. Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir. Sıfırdan Contoso University örnek uygulamanın nasıl oluşturulacağını açıklayan öğreticileri serisinin ilk budur.

[İndirme veya tamamlanmış uygulamanın görüntüleyin.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

EF Core 2.0 EF en son sürümü, ancak henüz EF özelliklerinin tümünü yok 6.x. EF arasında seçim yapma hakkında bilgi için bkz: 6.x ve EF Core [EF Core vs. EF6.x](/ef/efcore-and-ef6/). EF seçerseniz 6.x, bkz: [Bu öğretici serisinin önceki sürümünü](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

> [!NOTE]
> Bu öğreticide ASP.NET Core 1.1 sürümü için bkz: [VS 2017 güncelleştirme 2 sürüm PDF biçimindeki bu öğreticinin](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="troubleshooting"></a>Sorun giderme

Bir sorunla karşılaşırsanız, çözümleyemiyor çalıştırırsanız, genel olarak çözüm kodunuzda karşılaştırarak bulabilirsiniz [projeyi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final). Sık karşılaşılan hatalar ve bunları çözmek nasıl bir listesi için bkz. [serinin son Öğreticisi sorun giderme bölümünü](advanced.md#common-errors). Var. aradığınızı bulamazsanız, StackOverflow.com için için soru gönderebildiği [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP]
> Önceki öğreticilerde bitti üzerinde her biri yapılar 10 öğreticiler, bir dizi budur. Her öğretici Kurulum başarıyla tamamlandıktan sonra projenin bir kopyasını kaydedebilirsiniz. Sorunlarla karşılaşırsanız, ardından yeniden yukarıda bahsedilen tüm dizileri başlangıcına yerine önceki öğreticiden başlayabilirsiniz.

## <a name="the-contoso-university-web-application"></a>Contoso University web uygulaması

Aşağıdaki öğreticilerde oluşturmakta uygulama basit university web sitesidir.

Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin. Oluşturacağınız ekranlar birkaçını aşağıda verilmiştir.

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit.png)

Entity Framework ağırlıklı olarak nasıl kullanılacağı hakkında bir öğretici odaklanabilmeniz için kullanıcı Arabirimi stili bu sitenin yerleşik şablonları tarafından üretilen yakın tutulmuştur.

## <a name="create-an-aspnet-core-mvc-web-application"></a>Bir ASP.NET Core MVC web uygulaması oluşturma

Visual Studio'yu açın ve "ContosoUniversity" adlı yeni bir ASP.NET Core C# web projesi oluşturun.

* Gelen **dosya** menüsünde **yeni > Proje**.

* Sol bölmeden **yüklü > Visual C# > Web**.

* Seçin **ASP.NET Core Web uygulaması** proje şablonu.

* Girin **ContosoUniversity** tıklayın ve adı olarak **Tamam**.

  ![Yeni Proje iletişim kutusu](intro/_static/new-project.png)

* Bekle **yeni ASP.NET Core Web uygulaması (.NET Core)** görüntülenecek iletişim

* Seçin **ASP.NET Core 2.0** ve **Web uygulaması (Model-View-Controller)** şablonu.

  **Not:** Bu öğretici, ASP.NET Core 2.0 ve EF Core 2.0 veya sonraki--emin gerektirir **ASP.NET Core 1.1** seçilmez.

* Emin **kimlik doğrulaması** ayarlanır **kimlik doğrulaması yok**.

* **Tamam**’a tıklayın.

  ![Yeni ASP.NET Core projesi iletişim kutusu](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a>Site stili Ayarla

Birkaç basit değişiklikler site menü, Düzen ve giriş sayfasına ayarlar.

Açık *Views/Shared/_Layout.cshtml* ve aşağıdaki değişiklikleri yapın:

* "Contoso Üniversitesi" için "ContosoUniversity" her örneğini değiştirin. Üç örnekleri vardır.

* Menü girdileri eklemek **Öğrenciler**, **kursları**, **Eğitmenler**, ve **Departmanlar**ve silme **başvurun** menüsü girişi.

Değişiklikler vurgulanır.

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

İçinde *Views/Home/Index.cshtml*, dosyanın içeriğini bu uygulamayla ilgili metin ile ASP.NET ve MVC hakkında metnin değiştirmek için aşağıdaki kodla değiştirin:

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

Projeyi çalıştırmak veya seçmek için CTRL + F5 tuşlarına basın **hata ayıklama > hata ayıklama olmadan Başlat** menüsünde. Aşağıdaki öğreticilerde oluşturacaksınız sayfaları için sekmeli giriş sayfası görürsünüz.

![Contoso University giriş sayfası](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a>Entity Framework Core NuGet paketleri

EF Core desteği için bir proje eklemek için hedeflemek istediğiniz veritabanı sağlayıcısı yükleyin. Bu öğreticide SQL Server kullanır ve sağlayıcı paketi [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/). Bu paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), uygulamanız için bir paket başvurusu varsa paket başvurusu yapmak zorunda kalmazsınız `Microsoft.AspNetCore.App` paket.

Bu paketi ve bağımlılıkları (`Microsoft.EntityFrameworkCore` ve `Microsoft.EntityFrameworkCore.Relational`) EF çalışma zamanı desteği sağlar. Bir araç paketi ekleyeceksiniz daha sonra [geçişler](migrations.md) öğretici.

Entity Framework Core için kullanılabilen diğer veritabanı sağlayıcıları hakkında daha fazla bilgi için bkz. [veritabanı sağlayıcıları](/ef/core/providers/).

## <a name="create-the-data-model"></a>Veri modeli oluşturma

Sonraki varlık sınıfları Contoso University uygulaması oluşturacaksınız. Aşağıdaki üç varlıklar ile başlayacağız.

![Kurs kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

Arasında bir-çok ilişkisi `Student` ve `Enrollment` varlıkları ve bir-çok ilişkisi arasında `Course` ve `Enrollment` varlıklar. Diğer bir deyişle, bir öğrenci herhangi bir sayıda kursları kaydedilebilir ve bir kurs herhangi bir sayıda Öğrenciler içinde kayıtlı olabilir.

Aşağıdaki bölümlerde bu varlıkların her biri için bir sınıf oluşturacaksınız.

### <a name="the-student-entity"></a>Öğrenci varlık

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

İçinde *modelleri* klasöründe adlı bir sınıf dosyası oluşturma *Student.cs* ve şablon kodunu aşağıdaki kodla değiştirin.

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID` Özelliği, bu sınıf için karşılık gelen veritabanı tablosunun birincil anahtar sütunu olacak. Varsayılan olarak Entity Framework adlı bir özellik yorumlar `ID` veya `classnameID` birincil anahtar olarak.

`Enrollments` Özelliği bir [gezinti özelliği](/ef/core/modeling/relationships). Gezinti özellikleri bu varlıkla ilgili diğer varlıkların tutun. Bu durumda, `Enrollments` özelliği bir `Student entity` tümünü tutacak `Enrollment` olarak ilişkili varlıkları `Student` varlık. Diğer bir deyişle, belirli bir öğrenci satır veritabanında iki kayıt satırları (birincil anahtar değeri kendi StudentID yabancı anahtar sütunu, Öğrenci içeren satırlar) ilgili olan, `Student` varlığın `Enrollments` gezinti özelliği bu içerecek iki `Enrollment` varlıklar.

Bir gezinme özelliği (bire çok veya tek-çok ilişkilerde) olduğu gibi birden çok varlık tutarsanız, girişleri eklenebilir, silindi ve gibi güncelleştirilmiş bir listesi türü olmalıdır `ICollection<T>`. Belirtebileceğiniz `ICollection<T>` ya da bir tür gibi `List<T>` veya `HashSet<T>`. Belirtirseniz `ICollection<T>`, EF oluşturur bir `HashSet<T>` varsayılan olarak koleksiyon.

### <a name="the-enrollment-entity"></a>Kayıt varlık

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

İçinde *modelleri* klasör oluşturma *Enrollment.cs* ve varolan kodu aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` Birincil anahtar özelliği olacaktır; bu varlığı kullanan `classnameID` yerine desen `ID` gördüğünüz şekilde kendisi `Student` varlık. Genellikle bir düzen seçin ve veri modelinizi kullanılmakta. Burada, değişim ya da Desen kullanabilirsiniz gösterilmektedir. İçinde bir [sonraki öğreticide](inheritance.md), nasıl classname Kimliğini kullanarak, veri modelinde devralma uygulamak kolaylaştırır görürsünüz.

`Grade` Özelliği bir `enum`. Sonra soru işareti `Grade` türü bildirimi gösterir `Grade` özelliği boş değer atanabilir. Boş bir sınıf bir sıfır sınıf farklı--null anlamına gelir bir sınıf bilinen değil veya henüz atanmamış.

`StudentID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`. Bir `Enrollment` varlıktır biriyle ilişkili `Student` özelliği yalnızca tek bir içerebileceği için varlık `Student` varlık (aksine `Student.Enrollments` gezinti özelliği gördüğünüz önceki sürümlerinde, birden çok tutabilir `Enrollment` varlıklar).

`CourseID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`. Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.

Entity Framework adlandırılmışsa, bu özellik bir yabancı anahtar özellik olarak yorumlar `<navigation property name><primary key property name>` (örneğin, `StudentID` için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`). Yabancı anahtar özellikleri de adı yalnızca `<primary key property name>` (örneğin, `CourseID` beri `Course` varlığın birincil anahtarı `CourseID`).

### <a name="the-course-entity"></a>Kurs varlık

![Kurs varlık diyagramı](intro/_static/course-entity.png)

İçinde *modelleri* klasör oluşturma *Course.cs* ve varolan kodu aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments` Özelliktir bir gezinme özelliği. A `Course` varlık dilediğiniz sayıda ilgili olabileceğini `Enrollment` varlıklar.

Daha fazla ilgili dediğimiz `DatabaseGenerated` özniteliğini bir [sonraki öğreticide](complex-data-model.md) bu seri. Temel olarak, bu öznitelik kursu yerine için oluşturmak veritabanı birincil anahtarı girmenize olanak tanır.

## <a name="create-the-database-context"></a>Veritabanı bağlamı oluşturur

Verilen veri modeli için Entity Framework işlevselliği koordine eden ana veritabanı bağlamı sınıfının sınıftır. Türeterek Bu sınıf oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı. Kodunuzda hangi varlıkları veri modelinde yer alan belirtin. Ayrıca, belirli bir Entity Framework davranış özelleştirebilirsiniz. Bu projede adlı sınıfı `SchoolContext`.

Proje klasöründe adlı bir klasör oluşturun *veri*.

İçinde *veri* klasör oluşturma adlı yeni bir sınıf dosyası *SchoolContext.cs*, şablonu kodu aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Bu kod oluşturur bir `DbSet` her varlık kümesi özelliği. Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir ve bir varlık tablosunda bir satıra karşılık gelir.

Atlanmış `DbSet<Enrollment>` ve `DbSet<Course>` deyimleri ve aynı şekilde çalışır. Entity Framework bunları örtük olarak çünkü verilebilir `Student` varlık başvuruları `Enrollment` varlık ve `Enrollment` varlık başvuruları `Course` varlık.

Veritabanı oluşturulduğunda EF aynı adlara sahip tablolar oluşturur `DbSet` özellik adları. Koleksiyonlar için özellik adları olan genellikle çoğul (Öğrenciler yerine Öğrenci), ancak geliştiriciler katılmıyorum olup tablo adları veya pluralized hakkında. Bu öğreticiler için bir DbContext tekil tablo adları belirterek varsayılan davranışı geçersiz kılabilir. Bunu yapmak için son olan DB özelliğinden sonra aşağıdaki vurgulanmış kodu ekleyin.

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Bağımlılık ekleme bağlam kaydı

ASP.NET Core uygulayan [bağımlılık ekleme](../../fundamentals/dependency-injection.md) varsayılan olarak. Hizmetler (örneğin, EF veritabanı bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu hizmetler (örneğin, MVC denetleyicileri) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır. Bu öğreticide daha sonra bir bağlam örneğini alır denetleyici Oluşturucu kodunu görürsünüz.

Kaydedilecek `SchoolContext` bir hizmet olarak açmak *Startup.cs*ve vurgulanan satırları ekleyin `ConfigureServices` yöntemi.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir `DbContextOptionsBuilder` nesne. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.

Ekleme `using` deyimleri için `ContosoUniversity.Data` ve `Microsoft.EntityFrameworkCore` ad alanları ve ardından projeyi derleyin.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Açık *appsettings.json* dosya ve aşağıdaki örnekte gösterildiği gibi bir bağlantı dizesi ekleyin.

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Bir SQL Server LocalDB veritabanına bağlantı dizesini belirtir. LocalDB, SQL Server Express veritabanı Motoru'nu hafif bir sürümüdür ve uygulama geliştirme, üretim kullanımı için tasarlanmıştır. LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır. Varsayılan olarak LocalDB oluşturur *.mdf* veritabanı dosyası `C:/Users/<user>` dizin.

## <a name="add-code-to-initialize-the-database-with-test-data"></a>Veritabanı test verileri ile başlatmak için kod ekleyin

Entity Framework sizin için boş bir veritabanı oluşturur. Bu bölümde, test verileri ile doldurmak için veritabanı oluşturulduktan sonra çağrılan yöntemi yazın.

Burada, kullanacağınız `EnsureCreated` otomatik olarak veritabanı oluşturmak için yöntemi. İçinde bir [sonraki öğreticide](migrations.md) bırakarak ve veritabanını yeniden oluşturma yerine veritabanı şemasını değiştirmek için Code First Migrations'ı kullanarak model değişikliklerin üstesinden nasıl görürsünüz.

İçinde *veri* klasöründe adlı yeni bir sınıf dosyası oluşturma *DbInitializer.cs* şablon kodunu gerektiğinde oluşturulması için bir veritabanı neden aşağıdaki kodla değiştirin ve yeni veri yüklerini test Veritabanı.

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

Kod, veritabanındaki tüm Öğrenciler vardır ve aksi durumda, veritabanını yeni ve test verileri ile sağlanmış gerekiyor, varsayar denetler. Diziye test verileri yükler yerine `List<T>` performansını iyileştirmek için koleksiyonları.

İçinde *Program.cs*, değişiklik `Main` yöntemi uygulama başlangıcında aşağıdakileri yapın:

* Veritabanı bağlamı örneği bağımlılık ekleme kapsayıcısını alın.
* Bağlam iletmeden seed yöntemi çağırın.
* Seed yöntemi tamamlandığında bağlamını siler.

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

Ekleme `using` ifadeleri:

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

Önceki öğreticilerde, benzer bir kod görebilirsiniz `Configure` yönteminde *Startup.cs*. Kullanmanızı öneririz `Configure` yöntemi yalnızca istek işlem hattı ayarlayın. Uygulama başlangıç koduna ait içinde `Main` yöntemi.

Şimdi uygulamayı çalıştırdığınız ilk kez veritabanı oluşturulacak ve test verileri ile çekirdek değeri oluşturulmuş. Veri modelinizi değiştirdiğinizde, veritabanını silin, çekirdek yönteminizi güncelleştirin ve yeni bir veritabanı ile aynı şekilde parçalarından başlatın. Sonraki öğreticilerde, silinmesi ve yeniden oluşturulmadan değişiklik veri modelini kullanırken, veritabanı değiştirme görürsünüz.

## <a name="create-a-controller-and-views"></a>Bir denetleyici ve görünümler oluşturma

Ardından, bir MVC denetleyicisi ve EF sorgu ve veri kaydetmek için kullanacağı görünümleri eklemek için Visual Studio yapı iskelesi altyapısı kullanacaksınız.

CRUD eylem yöntemleri ve görünümler otomatik olarak oluşturulmasını, yapı iskelesi olarak bilinir. Yapı iskelesi kod oluşturma farklıdır iskele kurulan kodu ise, oluşturulan kod genellikle değiştirmeyin, kendi gereksinimlerinize uyacak şekilde değiştirebileceği bir başlangıç noktasıdır. Oluşturulan kod özelleştirmeniz gerektiğinde, kısmi sınıflar kullanın veya şeyler değiştirdiğinizde, kodu yeniden oluştur.

* Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle > Yeni iskele kurulmuş öğe**.

Varsa **MVC bağımlılıkları Ekle** iletişim kutusu görüntülenir:

* [Visual Studio en son sürüme güncelleştirme](https://www.visualstudio.com/downloads/). Visual Studio sürümlerini 15.5 önce bu iletişim kutusunu göster.
* Güncelleştiremiyorsanız, seçin **Ekle**ve ekleme denetleyicisi adımları tekrar uygulayın.

* İçinde **İskele Ekle** iletişim kutusunda:

  * Seçin **Entity Framework kullanarak görünümler ile MVC denetleyicisi**.

  * **Ekle**'yi tıklatın.

* İçinde **denetleyici Ekle** iletişim kutusunda:

  * İçinde **Model sınıfı** seçin **Öğrenci**.

  * İçinde **veri bağlamı sınıfının** seçin **SchoolContext**.

  * Varsayılan değerleri kabul **StudentsController** adı.

  * **Ekle**'yi tıklatın.

  ![İskele Öğrenci](intro/_static/scaffold-student.png)

  Tıkladığınızda **Ekle**, Visual Studio yapı iskelesi motoru oluşturur bir *StudentsController.cs* dosyası ve bir görünüm kümesi (*.cshtml* dosyaları) Denetleyici ile çalışır.

(El ile oluşturmayın, yapı iskelesi altyapısı ayrıca veritabanı bağlamı için oluşturabilirsiniz önce bu öğreticide daha önce yaptığınız gibi. Yeni bir bağlam sınıfında belirttiğiniz **denetleyici Ekle** kutusunun sağındaki artı işaretine tıklayarak **veri bağlamı sınıfının**.  Visual Studio ardından oluşturur, `DbContext` sınıf denetleyici ve görünüm yanı sıra.)

Denetleyici aldığını fark edeceksiniz bir `SchoolContext` Oluşturucu parametresi olarak.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

ASP.NET Core bağımlılık ekleme örneğini geçirerek üstlenir `SchoolContext` denetleyici içinde. Yapılandırdığınız *Startup.cs* daha önce dosya.

Denetleyici içeren bir `Index` veritabanındaki tüm Öğrenciler görüntüleyen eylem yöntemi. Yöntemi, okuyarak ayarlamak Öğrenciler varlıktan Öğrenciler listesini alır. `Students` veritabanı bağlam örneğinin özelliği:

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

Bu kod zaman uyumsuz programlama öğeleri öğreticinin ilerleyen bölümlerinde öğreneceksiniz.

*Views/Students/Index.cshtml* bu liste bir tabloda görüntüleyen:

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

Projeyi çalıştırmak veya seçmek için CTRL + F5 tuşlarına basın **hata ayıklama > hata ayıklama olmadan Başlat** menüsünde.

Test verileri görmek için Öğrenciler sekmesine tıklayın, `DbInitializer.Initialize` eklenen yöntemi. Bağlı nasıl dar tarayıcı pencerenizin, gördüğünüz `Student` Gezinti bağlantıyı görmek için sağ üst köşedeki simgeyi tıklatın sayfanın veya üst kısmındaki sekme bağlantısına sahip olacaksınız.

![Dar contoso University giriş sayfası](intro/_static/home-page-narrow.png)

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

## <a name="view-the-database"></a>Veritabanı görünümü

Uygulama başlatıldığında `DbInitializer.Initialize` yöntem çağrılarını `EnsureCreated`. EF gördüğünüz veritabanı vardı ve bu nedenle oluşturulan bir sonra geri kalanında `Initialize` yöntemi kod veritabanı verilerle doldurulur. Kullanabileceğiniz **SQL Server Nesne Gezgini** (SSOX Visual Studio'daki veritabanını görüntülemek için).

Tarayıcıyı kapatın.

SSOX pencere zaten açık değilse, seçim **görünümü** Visual Studio'daki menü.

SSOX içinde tıklayın **(localdb) \MSSQLLocalDB > veritabanları**ve ardından bağlantı dizesinde veritabanının adını girişini tıklatın, *appsettings.json* dosya.

Genişletin **tabloları** veritabanınızdaki tablolar görmek için düğümü.

![SSOX tablolarında](intro/_static/ssox-tables.png)

Sağ **Öğrenci** tablosu ve'ı tıklatın **görünüm verilerini** oluşturulan sütunları ve tabloya eklenen satırları görebilirsiniz.

![Öğrenci tabloda SSOX](intro/_static/ssox-student-table.png)

<em>.Mdf</em> ve <em>.ldf</em> veritabanı dosyalar, <em>C:\Users\\ <yourusername> </em> klasör.

Aradığınız çünkü `EnsureCreated` uygulama başlatıldığında çalıştırılan Başlatıcı yönteminde artık bir değişiklik yapabileceğiniz `Student` sınıfı, veritabanını silin, uygulamayı yeniden çalıştırın ve veritabanı otomatik olarak değişikliğiniz eşleşecek şekilde yeniden oluşturulması. Örneğin, eklediğiniz bir `EmailAddress` özelliğini `Student` sınıfı göreceksiniz yeni bir `EmailAddress` yeniden oluşturulan tablodaki sütun.

## <a name="conventions"></a>Kurallar

Sizin için tam bir veritabanı oluşturmak Entity Framework için sırayla yazmanız gerekirdi kod kuralları veya Entity Framework yapan varsayımlar kullanımı nedeniyle en az olur.

* Adlarını `DbSet` özellikleri, tablo adları kullanılır. Varlık tarafından başvurulmayan bir `DbSet` özelliği, varlık sınıfı adları, tablo adları kullanılır.

* Varlık özellik adlarını sütun adları için kullanılır.

* Kimliği veya Classnameıd adlı varlık özellikleri birincil anahtar özellik olarak kabul edilir.

* Adlandırılmışsa, bu özellik bir yabancı anahtar özellik olarak yorumlanır *<navigation property name> <primary key property name>* (örneğin, `StudentID` için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`). Yabancı anahtar özellikleri de adı yalnızca *<primary key property name>* (örneğin, `EnrollmentID` beri `Enrollment` varlığın birincil anahtarı `EnrollmentID`).

Geleneksel davranışını geçersiz kılınabilir. Örneğin, bu öğreticide daha önce bahsettiğim gibi tablo adları açıkça belirtebilirsiniz. Sütun adları ayarlayın ve görüşelim gibi herhangi bir özelliği birincil anahtar veya yabancı anahtar olarak ayarlayın ve bir [sonraki öğreticide](complex-data-model.md) bu seri.

## <a name="asynchronous-code"></a>Zaman uyumsuz kod

Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.

Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir. Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor. G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması. İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır. Sonuç olarak, sunucu kaynaklarının daha etkin kullanılması zaman uyumsuz kod sağlar ve sunucu gecikmeler olmadan daha fazla trafik işlemek için etkinleştirilir.

Zaman uyumsuz kod çalışma zamanında az miktarda bir ek yükü sağladıkları gerektirmez, ancak olası performans artışını uğradığı performans düşüşünü yüksek trafik durumlar için göz ardı edilebilir, çalışırken, düşük trafiğe durumlar için önemli.

Aşağıdaki kodda, `async` anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi zaman uyumsuz yürütülen kod olun.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* `async` Anahtar sözcüğü derleyiciye geri çağırmaları yöntem gövdesini bölümleri için oluşturulacak ve otomatik olarak oluşturmak için `Task<IActionResult>` döndürülen nesne.

* Dönüş türü `Task<IActionResult>` türünün bir sonucu ile devam eden çalışmayı temsil `IActionResult`.

* `await` Anahtar sözcüğü, derleyicinin yöntemin iki parçalara bölmek neden olur. İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır. İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.

* `ToListAsync` zaman uyumsuz sürümüdür `ToList` genişletme yöntemi.

Entity Framework kullanan zaman uyumsuz kod zaman yazıyorsanız dikkat edilecek bazı noktalar:

* Sorguları veya veritabanına gönderilecek komutları neden deyimleri zaman uyumsuz olarak yürütülür. Örneğin, içeren `ToListAsync`, `SingleOrDefaultAsync`, ve `SaveChangesAsync`. Bu, örneğin, yalnızca değiştirmek deyimleri içermeyen bir `IQueryable`, gibi `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* EF bağlam iş parçacığı güvenli olmayan: paralel birden çok işlem yapmak yeniden denemeyin. Herhangi bir zaman uyumsuz EF yöntemi çağırdığınızda, her zaman kullanabilirsiniz `await` anahtar sözcüğü.

* Zaman uyumsuz kodun performans avantajlarından yararlanmak istiyorsanız herhangi bir kitaplığı paketleri emin olun (örneğin, disk belleği) kullanıyorsanız, bunlar sorgular veritabanına neden herhangi bir Entity Framework yöntem çağırırsanız zaman uyumsuz de kullanın.

. NET'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [zaman uyumsuz genel bakış](/dotnet/articles/standard/async).

## <a name="summary"></a>Özet

Depolamak ve verileri görüntülemek için Entity Framework Core ve SQL Server Express LocalDB kullanan basit bir uygulama oluşturdunuz. Aşağıdaki öğreticide, temel CRUD gerçekleştirmeyi öğreneceksiniz (oluşturma, okuma, güncelleştirme ve silme) işlemleri.

::: moniker-end

> [!div class="step-by-step"]
> [Next](crud.md)
