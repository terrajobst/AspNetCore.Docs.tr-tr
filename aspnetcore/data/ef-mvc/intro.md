---
title: 'Öğretici: bir ASP.NET MVC web uygulamasında EF Core kullanmaya başlama'
description: Bu, Contoso Üniversitesi örnek uygulamasının sıfırdan nasıl oluşturulacağını açıklayan bir öğretici serisinin ilkisidir.
author: rick-anderson
ms.author: riande
ms.custom: mvc
ms.date: 02/06/2019
ms.topic: tutorial
uid: data/ef-mvc/intro
ms.openlocfilehash: fca9fdc425506ec8b4eec5c609237208f4c0d7b5
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511307"
---
# <a name="tutorial-get-started-with-ef-core-in-an-aspnet-mvc-web-app"></a>Öğretici: bir ASP.NET MVC web uygulamasında EF Core kullanmaya başlama

Bu öğretici 3,0 ASP.NET Core güncelleştirilmedi. [Razor Pages sürümü](xref:data/ef-rp/intro) güncelleştirildi. Kodun büyük bir çoğunluğu, Bu öğreticinin ASP.NET Core 3,0 ve sonraki bir sürümü için değişir:

* *Startup.cs* ve *program.cs* dosyalarıdır.
* [Razor Pages sürümünde](xref:data/ef-rp/intro)bulunabilir. 

Bunun ne zaman güncelleştirilemeyebilir hakkında bilgi edinmek için [Bu GitHub sorununa](https://github.com/dotnet/AspNetCore.Docs/issues/13920)bakın.

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc.md)]

Contoso Üniversitesi örnek Web uygulaması, Entity Framework (EF) Core 2,2 ve Visual Studio 2017 veya 2019 kullanarak ASP.NET Core 2,2 MVC web uygulamalarının nasıl oluşturulacağını gösterir.

Örnek uygulama, kurgusal bir Contoso Üniversitesi için bir Web sitesidir. Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir. Bu, Contoso Üniversitesi örnek uygulamasının sıfırdan nasıl oluşturulacağını açıklayan bir öğretici serisinin ilkisidir.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * ASP.NET Core MVC web uygulaması oluşturma
> * Site stili Ayarla
> * EF Core NuGet paketleri hakkında bilgi edinin
> * Veri modeli oluşturma
> * Veritabanı bağlamını oluşturma
> * Bağımlılık ekleme için bağlamı kaydetme
> * Test verileriyle veritabanını başlatma
> * Denetleyici ve görünüm oluşturma
> * Veritabanını görüntüleme

## <a name="prerequisites"></a>Önkoşullar

* [.NET Core SDK 2,2](https://dotnet.microsoft.com/download)
* Aşağıdaki iş yükleriyle [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) :
  * **ASP.net ve Web geliştirme** iş yükü
  * **.NET Core platformlar arası geliştirme** iş yükü

## <a name="troubleshooting"></a>Sorun giderme

Çözemiyoruz bir sorunla karşılaşırsanız, kodunuzun [Tamamlanan projeyle](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)karşılaştırılmasıyla genellikle çözümü bulabilirsiniz. Yaygın hataların bir listesi ve bunların nasıl çözüleceği için, [serideki son öğreticinin sorun giderme bölümüne](advanced.md#common-errors)bakın. İhtiyacınız olanları bulamazsanız, [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core)için bir soru gönderebilirsiniz.

> [!TIP]
> Bu, her biri daha önceki öğreticilerde gerçekleştirilen bir dizi 10 öğreticisidir. Her başarılı öğreticinin tamamlanmasından sonra projenin bir kopyasını kaydetmeyi göz önünde bulundurun. Daha sonra sorunlarla karşılaşırsanız, tüm serinin başlangıcına dönmek yerine önceki öğreticiden baştan başlayabilirsiniz.

## <a name="contoso-university-web-app"></a>Contoso Üniversitesi web uygulaması

Bu öğreticilerde oluşturacağınız uygulama basit bir üniversite web sitesidir.

Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin. Oluşturacağınız ekranların bazıları aşağıda verilmiştir.

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

![Öğrenciler düzenleme sayfası](intro/_static/student-edit.png)

## <a name="create-web-app"></a>Web uygulaması oluşturma

* Visual Studio'yu açın.

* **Dosya** menüsünden **Yeni > proje**' yi seçin.

* Sol bölmeden, **Visual C# > Web > yüklü**' i seçin.

* **ASP.NET Core Web uygulaması** proje şablonunu seçin.

* Ad olarak **Contosouniversity** yazın ve **Tamam**' a tıklayın.

  ![Yeni Proje iletişim kutusu](intro/_static/new-project2.png)

* **Yeni ASP.NET Core Web uygulaması** iletişim kutusunun görünmesini bekleyin.

* **.NET Core**, **ASP.NET Core 2,2** ve **Web uygulaması (Model-View-Controller)** şablonu ' nu seçin.

* **Kimlik doğrulamanın** **kimlik doğrulaması yok**olarak ayarlandığından emin olun.

* **Tamam**’ı seçin

  ![Yeni ASP.NET Core projesi iletişim kutusu](intro/_static/new-aspnet2.png)

## <a name="set-up-the-site-style"></a>Site stili Ayarla

Birkaç basit değişiklik, site menüsünü, düzeni ve giriş sayfasını ayarlar.

*Görünümler/paylaşılan/_Layout. cshtml* dosyasını açın ve aşağıdaki değişiklikleri yapın:

* "Contoso Üniversitesi" için "ContosoUniversity" her örneğini değiştirin. Üç örnekleri vardır.

* **Hakkında**, **öğrenciler**, **Kurslar**, **eğitmenler**ve **Departmanlar**için menü girişleri ekleyin ve **Gizlilik** menü girişini silin.

Değişiklikler vurgulanır.

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,34-48,63)]

*Görünümler/Home/Index. cshtml*'de, ASP.net ve MVC hakkındaki metni bu uygulamayla ilgili metinle değiştirmek için dosyanın içeriğini aşağıdaki kodla değiştirin:

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

Projeyi çalıştırmak için CTRL + F5 tuşlarına basın veya menüden **hata ayıklama > Başlat** ' ı seçin. Bu öğreticilerde oluşturacağınız sayfaların sekmelerini içeren giriş sayfasını görürsünüz.

![Contoso Üniversitesi ana sayfası](intro/_static/home-page.png)

## <a name="about-ef-core-nuget-packages"></a>NuGet paketleri EF Core hakkında

Bir projeye EF Core destek eklemek için hedeflemek istediğiniz veritabanı sağlayıcısını yükleyebilirsiniz. Bu öğretici SQL Server kullanır ve sağlayıcı paketi [Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)' dır. Bu paket [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur, bu nedenle pakete başvurmanız gerekmez.

EF SQL Server paketi ve bağımlılıkları (`Microsoft.EntityFrameworkCore` ve `Microsoft.EntityFrameworkCore.Relational`) EF için çalışma zamanı desteği sağlar. [Geçiş](migrations.md) öğreticisinde daha sonra bir araç paketi ekleyeceksiniz.

Entity Framework Core için kullanılabilen diğer veritabanı sağlayıcıları hakkında daha fazla bilgi için bkz. [veritabanı sağlayıcıları](/ef/core/providers/).

## <a name="create-the-data-model"></a>Veri modeli oluşturma

Daha sonra Contoso Üniversitesi uygulaması için varlık sınıfları oluşturacaksınız. Aşağıdaki üç varlıkla başlayacaksınız.

![Kurs kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

`Student` ve `Enrollment` varlıkları arasında bire çok ilişki vardır ve `Course` ile `Enrollment` varlıkları arasında bire çok bir ilişki vardır. Diğer bir deyişle, bir öğrenci herhangi bir sayıda kursa kaydedilebilir ve bir kurs, kayıtlı sayıda öğrenciye sahip olabilir.

Aşağıdaki bölümlerde, bu varlıkların her biri için bir sınıf oluşturacaksınız.

### <a name="the-student-entity"></a>Öğrenci varlık

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

*Modeller* klasöründe, *Student.cs* adlı bir sınıf dosyası oluşturun ve şablon kodunu aşağıdaki kodla değiştirin.

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID` özelliği, bu sınıfa karşılık gelen veritabanı tablosunun birincil anahtar sütunu olacak. Varsayılan olarak, Entity Framework `ID` veya `classnameID` adında bir özelliği birincil anahtar olarak yorumlar.

`Enrollments` özelliği bir [Gezinti özelliğidir](/ef/core/modeling/relationships). Gezinti özellikleri, bu varlıkla ilgili diğer varlıkları tutar. Bu durumda, bir `Student entity` `Enrollments` özelliği, bu `Student` varlıkla ilgili `Enrollment` varlıkların tümünü tutacaktır. Diğer bir deyişle, veritabanında verilen bir öğrenci satırı, iki ilişkili kayıt satırına (bu öğrencinin birincil anahtar değerini kendi StudentID yabancı anahtar sütununda içeren satırlar) sahipse, `Student` varlığın `Enrollments` gezinti özelliği bu iki `Enrollment` varlığını içerir.

Bir gezinti özelliği birden çok varlığı tutabileceiyorsa (çok-çok veya bire çok ilişkilerde olduğu gibi), türü `ICollection<T>`gibi girişlerin eklenebileceği, silinebileceği ve güncelleştirilemeyebilir bir liste olmalıdır. `ICollection<T>` veya `List<T>` veya `HashSet<T>`gibi bir tür belirtebilirsiniz. `ICollection<T>`belirtirseniz, EF varsayılan olarak bir `HashSet<T>` koleksiyonu oluşturur.

### <a name="the-enrollment-entity"></a>Kayıt varlık

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

*Modeller* klasöründe *enrollment.cs* oluşturun ve mevcut kodu şu kodla değiştirin:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` özelliği birincil anahtar olacaktır; Bu varlık, `Student` varlığında gördüğünüz gibi kendisini `ID` yerine `classnameID` modelini kullanır. Normalde tek bir model seçip veri modeliniz genelinde kullanabilirsiniz. Burada, değişim, her iki stili de kullanabileceğinizi gösterir. [Daha sonraki bir öğreticide](inheritance.md), ID 'yi ClassName olmadan kullanarak, veri modelinde devralma uygulamayı daha kolay hale getirir.

`Grade` özelliği bir `enum`. `Grade` türü bildiriminden sonraki soru işareti, `Grade` özelliğinin null yapılabilir olduğunu gösterir. Boş bir sınıf bir sıfır sınıf farklı--null anlamına gelir bir sınıf bilinen değil veya henüz atanmamış.

`StudentID` özelliği bir yabancı anahtardır ve ilgili gezinti özelliği `Student`. `Enrollment` bir varlık bir `Student` varlığıyla ilişkilendirilir, bu nedenle özellik yalnızca tek bir `Student` varlığı tutabilir (daha önce gördüğünüz `Student.Enrollments` gezinti özelliğinden farklı olarak, birden çok `Enrollment` varlığı tutabilir).

`CourseID` özelliği bir yabancı anahtardır ve ilgili gezinti özelliği `Course`. Bir `Enrollment` varlığı bir `Course` varlığıyla ilişkilendirilir.

Entity Framework, `<navigation property name><primary key property name>` adında bir özelliği yabancı anahtar özelliği olarak Yorumlar (örneğin, `Student` varlığının birincil anahtarı `ID`olduğundan `Student` gezinti özelliği için `StudentID`). Yabancı anahtar özelliklerine de yalnızca `<primary key property name>` (örneğin `CourseID`, `Course` varlığının birincil anahtarı `CourseID`olduğundan) adlandırılmış olabilir.

### <a name="the-course-entity"></a>Kurs varlık

![Kurs varlık diyagramı](intro/_static/course-entity.png)

*Modeller* klasöründe *Course.cs* oluşturun ve mevcut kodu şu kodla değiştirin:

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments` özelliği bir gezinti özelliğidir. `Course` bir varlık, herhangi bir sayıda `Enrollment` varlıkla ilişkili olabilir.

Bu serinin [sonraki bir öğreticide](complex-data-model.md) `DatabaseGenerated` özniteliği hakkında daha fazla bilgi edineceksiniz. Temel olarak bu öznitelik, veritabanının oluşturması yerine kursa ait birincil anahtarı girmenize olanak sağlar.

## <a name="create-the-database-context"></a>Veritabanı bağlamını oluşturma

Belirli bir veri modeli için Entity Framework işlevselliğini koordine eden ana sınıf veritabanı bağlamı sınıfıdır. Bu sınıfı, `Microsoft.EntityFrameworkCore.DbContext` sınıfından türeterek oluşturursunuz. Kodunuzda, veri modeline hangi varlıkların ekleneceğini belirtirsiniz. Ayrıca, belirli Entity Framework davranışlarını özelleştirebilirsiniz. Bu projede, sınıfı `SchoolContext`olarak adlandırılır.

Proje klasöründe, *veri*adlı bir klasör oluşturun.

*Veri* klasöründe, *SchoolContext.cs*adlı yeni bir sınıf dosyası oluşturun ve şablon kodunu şu kodla değiştirin:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Bu kod, her varlık kümesi için bir `DbSet` özelliği oluşturur. Entity Framework terimlerinde, genellikle bir varlık kümesi bir veritabanı tablosuna karşılık gelir ve bir varlık tablodaki bir satıra karşılık gelir.

`DbSet<Enrollment>` ve `DbSet<Course>` deyimlerini atlamış olabilirsiniz ve aynı şekilde çalışır. Entity Framework, `Student` varlık `Enrollment` varlığına başvurduğundan ve `Enrollment` varlığı `Course` varlığına başvurduğundan bunları örtülü olarak içerebilir.

Veritabanı oluşturulduğunda EF, `DbSet` Özellik adlarıyla aynı adlara sahip tablolar oluşturur. Koleksiyonlar için özellik adları genellikle plural (öğrenci yerine öğrenciler), ancak geliştiriciler tablo adlarının plurulululmasını kabul etmez. Bu öğreticiler için DbContext içinde tekil tablo adları belirterek varsayılan davranışı geçersiz kılarsınız. Bunu yapmak için, son DbSet özelliğinden sonra aşağıdaki vurgulanmış kodu ekleyin.

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-schoolcontext"></a>SchoolContext kaydetme

ASP.NET Core, varsayılan olarak [bağımlılık ekleme](../../fundamentals/dependency-injection.md) işlemini uygular. Hizmetler (EF veritabanı bağlamı gibi) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu hizmetleri gerektiren bileşenler (MVC denetleyicileri gibi) bu hizmetleri Oluşturucu parametreleri aracılığıyla sağlamaktadır. Bu öğreticide daha sonra bir bağlam örneği alan denetleyici Oluşturucu kodunu görürsünüz.

`SchoolContext` bir hizmet olarak kaydetmek için *Startup.cs*açın ve vurgulanan satırları `ConfigureServices` yöntemine ekleyin.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=9-10)]

Bağlantı dizesinin adı, `DbContextOptionsBuilder` nesnesi üzerinde bir yöntem çağırarak bağlama geçirilir. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.

`ContosoUniversity.Data` ve `Microsoft.EntityFrameworkCore` ad alanları için `using` deyimleri ekleyin ve ardından projeyi derleyin.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

*AppSettings. JSON* dosyasını açın ve aşağıdaki örnekte gösterildiği gibi bir bağlantı dizesi ekleyin.

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Bağlantı dizesi bir SQL Server LocalDB veritabanı belirtir. LocalDB, SQL Server Express veritabanı altyapısının hafif bir sürümüdür ve üretim kullanımı için değil uygulama geliştirmeye yöneliktir. LocalDB, isteğe bağlı olarak başlar ve karmaşık yapılandırma olduğundan kullanıcı modunda çalışır. Varsayılan olarak, LocalDB `C:/Users/<user>` dizininde *. mdf* veritabanı dosyaları oluşturur.

## <a name="initialize-db-with-test-data"></a>Test verileriyle VERITABANıNı başlatma

Entity Framework, sizin için boş bir veritabanı oluşturacaktır. Bu bölümde, test verileriyle doldurmak için veritabanı oluşturulduktan sonra çağrılan bir yöntem yazarsınız.

Burada, veritabanını otomatik olarak oluşturmak için `EnsureCreated` yöntemini kullanacaksınız. Sonraki bir [öğreticide](migrations.md) , veritabanını bırakıp yeniden oluşturmak yerine veritabanı şemasını değiştirmek için Code First Migrations kullanarak model değişikliklerini nasıl işleyeceğinizi göreceksiniz.

*Veri* klasöründe, *DbInitializer.cs* adlı yeni bir sınıf dosyası oluşturun ve şablon kodunu, gerektiğinde bir veritabanının oluşturulmasına neden olan aşağıdaki kodla değiştirin ve test verilerini yeni veritabanına yükler.

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

Kod, veritabanında herhangi bir öğrenci olup olmadığını denetler ve yoksa, veritabanının yeni olduğunu ve test verileriyle hazırlanması gerektiğini varsayar. Performansı iyileştirmek için test verilerini `List<T>` koleksiyonlar yerine dizilere yükler.

*Program.cs*' de, uygulama başlangıcında aşağıdakini yapmak için `Main` yöntemini değiştirin:

* Bağımlılık ekleme kapsayıcısından bir veritabanı bağlamı örneği alın.
* Temel yöntemi çağırın ve bu yönteme geçerek bağlamı geçer.
* Çekirdek yöntemi tamamlandığında bağlamı atın.

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

`using` deyimleri ekle:

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

Daha eski öğreticilerde, *Startup.cs*içinde `Configure` yönteminde benzer bir kod görebilirsiniz. Yalnızca istek ardışık düzenini ayarlamak için `Configure` yöntemini kullanmanızı öneririz. Uygulama başlangıç kodu `Main` yöntemine aittir.

Uygulamayı ilk kez çalıştırdığınızda veritabanı oluşturulur ve test verileriyle birlikte gösterilir. Veri modelinizi her değiştirdiğinizde, veritabanını silebilir, çekirdek yönteminizi güncelleştirebilir ve yeni bir veritabanıyla aynı şekilde bir baştan başlatabilirsiniz. Sonraki öğreticilerde, veri modeli değiştiğinde ve yeniden oluşturmadan veritabanını nasıl değiştireceğiniz hakkında bilgi edineceksiniz.

## <a name="create-controller-and-views"></a>Denetleyici ve görünüm oluşturma

Daha sonra, verileri sorgulamak ve kaydetmek için EF 'i kullanacak bir MVC denetleyicisi ve görünümleri eklemek üzere Visual Studio 'da scafkatlama altyapısını kullanacaksınız.

CRUD eylem yöntemlerinin ve görünümlerinin otomatik olarak oluşturulması, yapı iskelesi olarak bilinir. Yapı iskelesi, yapı oluşturma işleminden farklı olarak, kendi gereksinimlerinize uyacak şekilde değiştirebileceğiniz bir başlangıç noktası ve genellikle oluşturulan kodu değiştirmezsiniz. Oluşturulan kodu özelleştirmeniz gerektiğinde, kısmi sınıfları kullanırsınız veya işlemler değiştiğinde kodu yeniden oluşturmanız gerekir.

* **Çözüm Gezgini** ' de **denetleyiciler** klasörüne sağ tıklayın ve **> yeni iskele öğe Ekle**' yi seçin.

* **Yapı Iskelesi Ekle** iletişim kutusunda:

  * **Entity Framework kullanarak, görünümlerle MVC denetleyicisi ' ni**seçin.

  * **Ekle**'ye tıklayın. **Görünümler Ile MVC denetleyicisi ekleme, Entity Framework kullanma** iletişim kutusu görüntülenir.

    ![Yapı iskelesi öğrenci](intro/_static/scaffold-student2.png)

  * **Model sınıfı** ' nda **öğrenci**' yi seçin.

  * **Veri bağlamı sınıfında** **SchoolContext**öğesini seçin.

  * Varsayılan **Studentscontroller** adını olarak kabul edin.

  * **Ekle**'ye tıklayın.

  **Ekle**' ye tıkladığınızda, Visual Studio yapı iskelesi altyapısı, denetleyicisiyle birlikte çalışan bir *StudentsController.cs* dosyası ve bir dizi görünüm ( *. cshtml* dosyası) oluşturur.

(Yapı iskelesi altyapısı, daha önce Bu öğreticide yaptığınız gibi, daha önce el ile oluşturmazsanız, sizin için veritabanı bağlamını de oluşturabilir. **Veri bağlam sınıfının**sağ tarafındaki artı Işaretine tıklayarak **Denetleyici Ekle** kutusunda yeni bir bağlam sınıfı belirtebilirsiniz.  Daha sonra Visual Studio, `DbContext` sınıfınızın yanı sıra denetleyiciyi ve görünümleri de oluşturacaktır.)

Denetleyicinin bir `SchoolContext` Oluşturucu parametresi olarak aldığını fark edeceksiniz.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

ASP.NET Core bağımlılığı ekleme, denetleyiciye `SchoolContext` bir örneği geçirmekten yararlanır. Bunu *Startup.cs* dosyasında daha önce yapılandırdınız.

Denetleyici, veritabanındaki tüm öğrencileri görüntüleyen bir `Index` Action yöntemi içerir. Yöntemi, veritabanı bağlamı örneğinin `Students` özelliğini okuyarak öğrenciler varlık kümesinden öğrencilerin bir listesini alır:

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

Öğreticide daha sonra bu kodda zaman uyumsuz programlama öğeleri hakkında bilgi edineceksiniz.

*Views/öğrenciler/Index. cshtml* görünümü bu listeyi bir tabloda görüntüler:

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

Projeyi çalıştırmak için CTRL + F5 tuşlarına basın veya menüden **hata ayıklama > Başlat** ' ı seçin.

`DbInitializer.Initialize` yönteminin eklendiği test verilerini görmek için öğrenciler sekmesine tıklayın. Tarayıcı pencerenizin ne kadar dar olduğuna bağlı olarak, sayfanın üst kısmında `Students` Tab bağlantısını görürsünüz veya bağlantıyı görmek için sağ üst köşedeki gezinti simgesine tıklamanız gerekir.

![Contoso Üniversitesi giriş sayfası dar](intro/_static/home-page-narrow.png)

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

## <a name="view-the-database"></a>Veritabanını görüntüleme

Uygulamayı başlattığınızda, `DbInitializer.Initialize` yöntemi `EnsureCreated`çağırır. EF, bir veritabanı olmadığını ve bu nedenle bir tane oluşturduğunu gördük, `Initialize` yöntemi kodunun geri kalanı veritabanını verilerle doldurmuş. Visual Studio 'da veritabanını görüntülemek için **SQL Server Nesne Gezgini** (ssox) kullanabilirsiniz.

Tarayıcıyı kapatın.

SSOX penceresi henüz açık değilse, Visual Studio 'daki **Görünüm** menüsünden bunu seçin.

SSOX 'te **(LocalDB) \MSSQLLocalDB > veritabanları**' na tıklayın ve ardından *appSettings. JSON* dosyanızdaki bağlantı dizesinde bulunan veritabanı adı için girişe tıklayın.

Veritabanınızdaki tabloları görmek için **Tablolar** düğümünü genişletin.

![SSOX içindeki tablolar](intro/_static/ssox-tables.png)

Oluşturulan sütunları ve tabloya eklenmiş satırları görmek için **öğrenci** tablosuna sağ tıklayın ve **verileri görüntüle** ' ye tıklayın.

![SSOX 'te öğrenci tablosu](intro/_static/ssox-student-table.png)

*. Mdf* ve *. ldf* veritabanı dosyaları, *C:\Users\\\<YourUserName >* klasöründedir.

Uygulama başlatma sırasında çalışan Başlatıcı yönteminde `EnsureCreated` çağırırken, artık `Student` sınıfında bir değişiklik yapabilir, veritabanını silebilir, uygulamayı yeniden çalıştırabilirsiniz ve veritabanı, değişikliklerinizi eşleştirmek için otomatik olarak yeniden oluşturulur. Örneğin, `Student` sınıfına bir `EmailAddress` özelliği eklerseniz, yeniden oluşturulan tabloda yeni bir `EmailAddress` sütunu görürsünüz.

## <a name="conventions"></a>Kurallar

Entity Framework, kuralların kullanımı veya Entity Framework varsayımlarıyla ilgili olarak en az bir veritabanı oluşturabilmesini sağlamak için yazmanız gereken kod miktarı.

* `DbSet` özelliklerinin adları tablo adı olarak kullanılır. `DbSet` özelliği tarafından başvurulmayan varlıklar için, varlık sınıfı adları tablo adı olarak kullanılır.

* Varlık özelliği adları, sütun adları için kullanılır.

* ID veya Classnameıd olarak adlandırılan varlık özellikleri, birincil anahtar özellikleri olarak tanınır.

* Bir özellik, bir yabancı anahtar özelliği olarak yorumlanır *\<gezinti özelliği adı >\<birincil anahtar özellik adı >* (örneğin, `StudentID`, birincil anahtarı `Student` olduğundan `Student` gezinti özelliği için `ID`). Yabancı anahtar özellikleri, *\<birincil anahtar özellik adı >* olarak da adlandırılabilir (örneğin, `Enrollment` varlığının birincil anahtarı `EnrollmentID`olduğundan `EnrollmentID`).

Geleneksel davranış geçersiz kılınabilir. Örneğin, bu öğreticide daha önce gördüğünüz gibi tablo adlarını açıkça belirtebilirsiniz. Ayrıca, bu serinin [sonraki bir öğreticide](complex-data-model.md) göreceğiniz gibi, sütun adlarını ayarlayabilir ve herhangi bir özelliği birincil anahtar veya yabancı anahtar olarak ayarlayabilirsiniz.

## <a name="asynchronous-code"></a>Zaman uyumsuz kod

Zaman uyumsuz programlama, ASP.NET Core ve EF Core için varsayılan moddur.

Sınırlı sayıda iş parçacığı kullanılabilir bir web sunucusuna sahip ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir. Bu durum oluştuğunda, sunucunun iş parçacıklarının serbest bırakılana kadar yeni istekleri işleyemiyor. G/ç tamamlanması bekleniyor çünkü bunlar herhangi bir iş gerçekten yapmamanız sırasında eş zamanlı kod ile birçok iş parçacığı bağlanması. İşlemi tamamlamak, g/ç için beklerken zaman uyumsuz kod ile diğer istekleri işlemek için kullanılacak sunucuyu için kendi iş parçacığı serbest bırakılır. Sonuç olarak, sunucu kaynaklarının daha etkin kullanılması zaman uyumsuz kod sağlar ve sunucu gecikmeler olmadan daha fazla trafik işlemek için etkinleştirilir.

Zaman uyumsuz kod, çalışma zamanında az miktarda yük getirir, ancak düşük trafik durumlarında performans artışı göz ardı edilebilir, ancak yüksek trafik durumları için olası performans iyileştirmesi oldukça önemlidir.

Aşağıdaki kodda `async` anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` metodu kodu zaman uyumsuz olarak yürütür.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* `async` anahtar sözcüğü, derleyiciye Yöntem gövdesinin parçaları için geri çağrılar oluşturmasını ve döndürülen `Task<IActionResult>` nesnesini otomatik olarak oluşturmasını söyler.

* `Task<IActionResult>` dönüş türü, `IActionResult`türünde bir sonuçla devam eden işi temsil eder.

* `await` anahtar sözcüğü, derleyicinin yöntemi iki parçaya böetmesine neden olur. İlk bölüm ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır. İkinci bölümü, işlemi tamamlandıktan sonra çağrılan bir geri çağırma yöntemi yerleştirilir.

* `ToListAsync`, `ToList` uzantısı yönteminin zaman uyumsuz sürümüdür.

Entity Framework kullanan zaman uyumsuz kod yazarken dikkat edilmesi gereken bazı şeyler:

* Yalnızca sorguları veya komutlarının veritabanına gönderilmesine neden olan deyimler zaman uyumsuz olarak yürütülür. Bu, örneğin, `ToListAsync`, `SingleOrDefaultAsync`ve `SaveChangesAsync`içerir. Örneğin, `var students = context.Students.Where(s => s.LastName == "Davolio")`gibi yalnızca bir `IQueryable`değiştiren deyimler dahil değildir.

* EF bağlamı iş parçacığı açısından güvenli değildir: paralel olarak birden çok işlem yapmayı denemeyin. Herhangi bir zaman uyumsuz EF yöntemini çağırdığınızda her zaman `await` anahtar sözcüğünü kullanın.

* Zaman uyumsuz kodun performans avantajlarından yararlanmak isterseniz, kullanmakta olduğunuz tüm kitaplık paketlerinin (örneğin, sayfalama için), sorguların veritabanına gönderilmesine neden olan herhangi bir Entity Framework yöntemini çağırıyorsa, zaman uyumsuz olarak da kullanılmasını sağlayın.

.NET ' te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz. [Async Overview](/dotnet/articles/standard/async).

## <a name="get-the-code"></a>Kodu alma

[Tamamlanmış uygulamayı indirin veya görüntüleyin.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * ASP.NET Core MVC web uygulaması oluşturuldu
> * Site stili Ayarla
> * EF Core NuGet paketleri hakkında bilgi edinildi
> * Veri modeli oluşturuldu
> * Veritabanı bağlamı oluşturuldu
> * SchoolContext kaydedildi
> * Test verileriyle VERITABANı başlatıldı
> * Denetleyici ve görünümler oluşturuldu
> * Veritabanı görüntüleniyor

Aşağıdaki öğreticide, temel CRUD (oluşturma, okuma, güncelleştirme, silme) işlemlerini nasıl gerçekleştireceğinizi öğreneceksiniz.

Temel CRUD (oluşturma, okuma, güncelleştirme, silme) işlemlerini nasıl gerçekleştireceğinizi öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Temel CRUD işlevlerini uygulama](crud.md)

