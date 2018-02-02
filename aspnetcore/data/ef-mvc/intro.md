---
title: "Entity Framework Çekirdek - 10 Öğreticisi 1 ile ASP.NET Core MVC"
author: tdykstra
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/intro
ms.openlocfilehash: 7de43a390ee0e11f6eda811b0774343ab330c53b
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-entity-framework-core-using-visual-studio-1-of-10"></a>ASP.NET Core MVC ve Entity Framework Visual Studio (1 / 10) kullanarak çekirdek ile çalışmaya başlama

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğretici Razor sayfalarının sürümü [burada](xref:data/ef-rp/intro). Razor sayfalarının sürümü izleyin daha kolaydır ve daha fazla EF özellikleri kapsar. Takip ettiğiniz öneririz [Razor sayfalarının sürüm bu öğreticinin](xref:data/ef-rp/intro).

Contoso University örnek web uygulaması Entity Framework (EF) çekirdek 2.0 ve Visual Studio 2017 kullanarak ASP.NET Core 2.0 MVC web uygulamalarının nasıl oluşturulacağını gösterir.

Örnek uygulama, kurgusal bir Contoso üniversite için bir web sitesidir. Öğrenci giriş, indirmelere oluşturma ve Eğitmen atamaları gibi işlevselliği içerir. Sıfırdan Contoso University örnek uygulamasının nasıl oluşturulacağını açıklayan eğitim serileri ilk budur.

[İndirme veya tamamlanan uygulama görüntüleyin.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

EF çekirdek 2.0 EF en son sürümü ancak henüz EF tüm özelliklerini yok 6.x. EF arasında seçim yapma hakkında bilgi için bkz: 6.x ve EF çekirdek [EF çekirdek vs. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/). EF seçerseniz 6.x, bkz: [Bu öğretici seri'nın önceki sürümünü](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

> [!NOTE]
> * Bu öğretici ASP.NET Core 1.1 sürümü için bkz: [PDF biçimi Bu öğreticide VS 2017 güncelleştirme 2 sürümü](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).
> * Visual Studio 2015 sürümünde Bu öğretici için bkz: [ASP.NET Core belgeleri PDF biçimli VS 2015 sürümü](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a>Sorun giderme

Olamaz çözmek bir sorunla çalıştırırsanız, genellikle çözümün kodunuzu karşılaştırarak bulabilirsiniz [projeyi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final). Sık karşılaşılan hataları ve bunları çözmek nasıl listesi için bkz: [serideki son Öğreticisi sorun giderme bölümüne](advanced.md#common-errors). Var. gerekenler bulamazsanız, bir soru için StackOverflow.com nakledebilirsiniz [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) veya [EF çekirdek](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP] 
> Bu, her biri önceki eğitimlerine bitti üzerinde derlemeler 10 öğreticileri dizisidir. Her başarılı öğretici tamamlandıktan sonra projeyi bir kopyasını kaydetme göz önünde bulundurun. Sorunlarla karşılaşırsanız, daha sonra yeniden tüm dizileri başlangıcına geri dönerseniz yerine önceki öğreticiden başlayabilirsiniz.

## <a name="the-contoso-university-web-application"></a>Contoso University web uygulaması

Bu öğreticiler oluşturmakta uygulama basit university web sitesidir.

Kullanıcıların görüntüleyebileceği ve Öğrenci, indirmelere ve Eğitmen bilgileri güncelleştirin. Oluşturacağınız ekranlar bazılarını aşağıda verilmiştir.

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

![Öğrenciler Düzenle sayfası](intro/_static/student-edit.png)

Öğretici Entity Framework çoğunlukla nasıl kullanılacağı hakkında odaklanabilmeniz bu sitenin UI Stili yerleşik şablonlar tarafından oluşturulan yakın tutulmuştur.

## <a name="create-an-aspnet-core-mvc-web-application"></a>Bir ASP.NET Core MVC web uygulaması oluşturma

Visual Studio'yu açın ve "ContosoUniversity" adlı yeni bir ASP.NET Core C# web projesi oluşturun.

* Gelen **dosya** menüsünde, select **yeni > Proje**.

* Sol bölmeden seçin **yüklü > Visual C# > Web**.

* Seçin **ASP.NET çekirdek Web uygulaması** proje şablonu.

* Girin **ContosoUniversity** tıklatın ve adı olarak **Tamam**.

  ![Yeni Proje iletişim kutusu](intro/_static/new-project.png)

* Bekle **yeni ASP.NET çekirdek Web uygulaması (.NET Core)** görünmesi iletişim

* Seçin **ASP.NET Core 2.0** ve **Web uygulaması (Model-View-Controller)** şablonu.

  **Not:** ASP.NET Core 2.0 ve EF çekirdek 2.0 veya üstü--emin olun, Bu öğretici gerektirir **ASP.NET Core 1.1** seçilmez.

* Emin olun **kimlik doğrulaması** ayarlanır **doğrulaması yok**.

* **Tamam**’a tıklayın.

  ![Yeni ASP.NET projesi iletişim kutusu](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a>Site stil ayarlama

Birkaç basit değişiklikler sitesi menüsü, Düzen ve giriş sayfası ayarlar.

Açık *Views/Shared/_Layout.cshtml* ve aşağıdaki değişiklikleri yapın:

* Her oluşumu "ContosoUniversity", "Contoso University" değiştirin. Üç oluşum vardır.

* Menü girişlerini eklemek **Öğrenciler**, **kurslar**, **Eğitmen**, ve **Departmanlar**ve silme **kişi** menü girişi.

Değişiklikler vurgulanır.

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

İçinde *Views/Home/Index.cshtml*, dosyanın içeriğini bu uygulamayla ilgili metin ile ASP.NET MVC hakkında metni değiştirmek için aşağıdaki kod ile değiştirin:

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

Projeyi çalıştırın veya seçmek için CTRL + F5 tuşuna basın **hata ayıklama > hata ayıklama olmadan Başlat** menüsünde. Bu öğreticiler oluşturacaksınız sayfalar için sekmelerle giriş sayfasına bakın.

![Contoso University giriş sayfası](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a>Entity Framework Core NuGet paketleri

Bir projeye EF çekirdek desteği eklemek için hedeflemek istediğiniz veritabanı sağlayıcısı yükleyin. Bu öğreticide SQL Server kullanır ve sağlayıcı paketi [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/). Bu paket dahil [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) yüklemek zorunda kalmamak için metapackage.

Bu paketi ve bağımlılıklarını (`Microsoft.EntityFrameworkCore` ve `Microsoft.EntityFrameworkCore.Relational`) EF çalışma zamanı desteği sağlar. Bir araç paketi ekleyeceksiniz daha sonra [geçişler](migrations.md) Öğreticisi. 

Entity Framework Çekirdek için kullanılabilir olan diğer veritabanı sağlayıcıları hakkında daha fazla bilgi için bkz: [veritabanı sağlayıcıları](https://docs.microsoft.com/ef/core/providers/).

## <a name="create-the-data-model"></a>Veri modeli oluşturma

Sonraki Contoso University uygulama için varlık sınıfları oluşturacaksınız. Aşağıdaki üç varlık ile başlayacağız.

![İndirmelere kayıt Öğrenci veri modeli diyagramı](intro/_static/data-model-diagram.png)

Arasında bir-çok ilişkisi olduğundan `Student` ve `Enrollment` varlıkları ve arasında bir-çok ilişkisi olduğundan `Course` ve `Enrollment` varlıklar. Diğer bir deyişle, öğrencinin herhangi bir sayıda kurslar kaydedilebilir ve bir indirmelere içinde kayıtlı Öğrenciler herhangi bir sayıda olabilir.

Aşağıdaki bölümlerde bu varlıkların her biri için bir sınıf oluşturacaksınız.

### <a name="the-student-entity"></a>Öğrenci varlık

![Öğrenci varlık diyagramı](intro/_static/student-entity.png)

İçinde *modelleri* klasörünü adlı bir sınıf dosyası oluşturma *Student.cs* ve şablon kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID` Özelliği, bu sınıfa karşılık gelen veritabanı tablosunun birincil anahtar sütunu hale gelir. Varsayılan olarak, Entity Framework adlı bir özellik yorumlar `ID` veya `classnameID` birincil anahtar olarak.

`Enrollments` Özelliği bir gezinti özelliğidir. Gezinti özellikleri bu varlıkla ilgili diğer varlıklar tutun. Bu durumda, `Enrollments` özelliği bir `Student entity` tüm tutacak `Enrollment` için ilişkili olan varlıkların `Student` varlık. Diğer bir deyişle, belirli bir öğrenci satır, veritabanındaki iki kayıt (birincil anahtar değerini kendi StudentID yabancı anahtar sütunu, öğrencinin içeren satırları) ilişkili satırları sahiptir, `Student` varlığın `Enrollments` gezinti özelliği olanlar içerecek iki `Enrollment` varlıklar.

Bir gezinti özelliği (çok- veya -çok ilişkileri) olduğu gibi birden çok varlık tutarsanız türünü hangi girişleri eklenebilir, silinen ve gibi güncelleştirilmiş bir listesi olmalıdır `ICollection<T>`. Belirleyebileceğiniz `ICollection<T>` veya gibi bir tür `List<T>` veya `HashSet<T>`. Belirtirseniz `ICollection<T>`, EF oluşturur bir `HashSet<T>` varsayılan koleksiyon.

### <a name="the-enrollment-entity"></a>Kayıt varlık

![Kayıt varlık diyagramı](intro/_static/enrollment-entity.png)

İçinde *modelleri* klasörü oluşturmak *Enrollment.cs* ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` Birincil anahtar özelliği olacaktır; bu varlığı kullanan `classnameID` yerine desen `ID` yazarken kendi başına gördüğünüz `Student` varlık. Normalde bir desen seçin ve veri modelinizi kullanmak. Burada, değişim ya da Desen kullanabileceğiniz gösterilmektedir. İçinde bir [sonraki öğretici](inheritance.md), nasıl classname Kimliğini kullanarak, veri modelinde devralma uygulamak kolaylaştırır görürsünüz.

`Grade` Özelliği bir `enum`. Soru işareti sonra `Grade` türü bildirimi gösterir `Grade` özelliği boş değer atanabilir. Null bir düzeyde bir sıfır ataması farklı.--null anlamına gelir bir düzeyde bilinen değil veya henüz atanmamış.

`StudentID` Özelliği bir yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`. Bir `Enrollment` varlıktır biriyle ilişkili `Student` özelliği yalnızca tek bir tutmak için varlık `Student` varlık (aksine `Student.Enrollments` gezinti özelliği gördüğünüz daha önceki sürümlerde, birden çok tutabilir `Enrollment` varlıklar).

`CourseID` Özelliği bir yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`. Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.

Entity Framework onu ise bu özellik bir yabancı anahtar özellik olarak yorumlar `<navigation property name><primary key property name>` (örneğin, `StudentID` için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarının `ID`). Yabancı anahtar özellikleri de adlı yalnızca `<primary key property name>` (örneğin, `CourseID` beri `Course` varlığın birincil anahtarının `CourseID`).

### <a name="the-course-entity"></a>İndirmelere varlık

![İndirmelere varlık diyagramı](intro/_static/course-entity.png)

İçinde *modelleri* klasörü oluşturmak *Course.cs* ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments` Özelliği bir gezinti özelliğidir. A `Course` varlık herhangi bir sayıda için ilgili olabileceğini `Enrollment` varlıklar.

Daha fazla hakkında dediğimiz `DatabaseGenerated` özniteliğini bir [sonraki öğretici](complex-data-model.md) bu serideki. Temel olarak, bu öznitelik indirmelere yerine için oluşturmak veritabanı birincil anahtarı girmenizi sağlar.

## <a name="create-the-database-context"></a>Veritabanı bağlamı oluşturma

Verilen veri modeli için Entity Framework işlevselliği koordinatları ana veritabanı bağlamı sınıfının sınıftır. Bu sınıf türetme tarafından oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı. Kodunuzda hangi varlıkların veri modelinde bulunan belirtin. Ayrıca, belirli bir Entity Framework davranış özelleştirebilirsiniz. Bu projede adlı sınıfı `SchoolContext`.

Proje klasöründe adlı bir klasör oluşturun *veri*.

İçinde *veri* klasörü oluşturun adlı yeni bir sınıf dosya *SchoolContext.cs*ve şablon kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Bu kod oluşturur bir `DbSet` özelliği her bir varlık kümesi. Entity Framework terminoloji, bir varlık kümesine genellikle bir veritabanı tablosuna karşılık gelir ve bir varlık tablosunda bir satırı karşılık gelir.

Atlanmış `DbSet<Enrollment>` ve `DbSet<Course>` deyimleri ve çalışır. Entity Framework bunları örtük olarak dahil `Student` varlık başvuruları `Enrollment` varlık ve `Enrollment` varlık başvuruları `Course` varlık.

Veritabanı oluşturulduğunda EF aynı adlara sahip tablolar oluşturur `DbSet` özellik adları. Koleksiyonlar için özellik adları olan genellikle çoğul (Öğrenciler yerine Öğrenci), ancak geliştiriciler katılmıyorum olup tablo adları veya pluralized hakkında. Bu öğreticileri için DbContext tekil tablo adları belirterek varsayılan davranışı geçersiz kılma. Bunu yapmak için en son DbSet özellik sonra aşağıdaki vurgulanmış kodu ekleyin.

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Bağlam bağımlılık ekleme ile kaydetme

ASP.NET Core uygulayan [bağımlılık ekleme](../../fundamentals/dependency-injection.md) varsayılan olarak. Hizmetleri (örneğin, EF veritabanı bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu Hizmetleri (örneğin, MVC denetleyicileri) gerektiren bileşenler bu hizmetlere Oluşturucu parametreleri yoluyla sağlanır. Daha sonra Bu öğreticide bir bağlam örneğini alır denetleyicisi Oluşturucusu kod görürsünüz.

Kaydetmek için `SchoolContext` bir hizmet olarak açmak *haline*, vurgulanan satırlar ekleyin `ConfigureServices` yöntemi.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

Bağlantı dizesinin adını bağlamına üzerinde bir yöntemini çağırarak geçirilen bir `DbContextOptionsBuilder` nesnesi. Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.

Ekleme `using` deyimleri için `ContosoUniversity.Data` ve `Microsoft.EntityFrameworkCore` ad alanları ve projeyi oluşturun.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Açık *appsettings.json* dosyası ve bir bağlantı dizesi aşağıdaki örnekte gösterildiği gibi ekleyin.

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Bir SQL Server yerel veritabanı veritabanı bağlantı dizesini belirtir. Yerel veritabanı, SQL Server Express veritabanı Motoru'nu hafif sürümüdür ve uygulama geliştirme, üretim kullanımı için amaçlanmıştır. Yerel veritabanı isteğe bağlı olarak başlar ve bu yüzden karmaşık yapılandırma kullanıcı modunda çalışır. Varsayılan olarak, yerel veritabanı oluşturur *.mdf* veritabanı dosyası `C:/Users/<user>` dizin.

## <a name="add-code-to-initialize-the-database-with-test-data"></a>Test verileri veritabanıyla başlatmak için kod ekleme

Entity Framework boş bir veritabanı oluşturur. Bu bölümde, test verileri ile doldurmak için veritabanı oluşturulduktan sonra çağrılan yöntemi yazın.

Burada kullanacağınız `EnsureCreated` otomatik olarak veritabanını oluşturmak için yöntem. İçinde bir [sonraki öğretici](migrations.md) model değişiklikleri bırakarak ve veritabanını yeniden oluşturma yerine veritabanı şeması değiştirmek için Code First Migrations kullanılarak nasıl ele alınacağını görürsünüz.

İçinde *veri* klasörünü adlı yeni bir sınıf dosyası oluşturun *DbInitializer.cs* şablonu kodu gerekli olduğunda oluşturulacak bir veritabanı neden olan aşağıdaki kodla değiştirin ve yükleri test yeni verileri Veritabanı.

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

Kod veritabanındaki tüm Öğrenciler varsa ve bu değilse, veritabanını yeni ve test verilerle sağlanmış gerekiyor, varsayar denetler. Diziye test verileri yükler yerine `List<T>` performansı iyileştirmek için koleksiyonları.

İçinde *Program.cs*, değişiklik `Main` yöntemi uygulama başlangıcında üzerinde aşağıdakileri yapın:

* Bir veritabanı bağlamını örneği bağımlılık ekleme kapsayıcıdan alın.
* İçin bağlamı geçirme çekirdek yöntemini çağırın.
* Seed yöntemi yapıldığında bağlamını siler.

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

Ekleme `using` deyimleri:

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Usings)]

Eski eğitimlerine benzer bir kod görebilirsiniz `Configure` yönteminde *haline*. Kullanmanızı öneririz `Configure` yalnızca istek ardışık düzenini ayarlamak için yöntem. Uygulama başlangıç koduna ait `Main` yöntemi.

Şimdi uygulamayı çalıştırın ilk kez veritabanı oluşturulur ve test verilerle sağlanmış. Veri modelinizi değiştirdiğinizde veritabanını silin, seed yöntemi güncelleştirin ve yeni bir veritabanı ile aynı şekilde afresh başlatın. Sonraki öğreticileri, veritabanı veri değişiklikleri, model silinmesi ve yeniden oluşturma, değiştirme görürsünüz.

## <a name="create-a-controller-and-views"></a>Bir denetleyici ve görünümler oluşturma

Ardından, MVC denetleyicisi ve EF sorgu ve veri kaydetmek için kullanacağı görünümleri eklemek için Visual Studio'da yapı iskelesi altyapısı kullanacağınız.

CRUD eylem yöntemleri ve görünümler otomatik olarak oluşturulmasını yapı iskelesi olarak bilinir. İskele kurulmuş kodu oluşturulan kod genellikle değiştirmeyin ancak kendi gereksinimlerinize uyacak şekilde değiştirebilirsiniz bir başlangıç noktası kod oluşturma farklıdır. Oluşturulan kodun özelleştirme gerektiğinde, değişiklik olduğunda kodu yeniden oluşturmak veya kısmi sınıflar kullanın.

* Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle > Yeni iskele kurulmuş öğe**.

Varsa **MVC bağımlılıkları ekleyin** iletişim kutusu görüntülenir:

* [Visual Studio en son sürüme güncelleştirin](https://www.visualstudio.com/downloads/). Visual Studio sürümleri 15,5 önce bu iletişim kutusunu göster.
* Update yapılamıyor, seçin **Ekle**ve ardından Ekle denetleyicisi adımları yeniden uygulayın.

* İçinde **İskele Ekle** iletişim kutusunda:

  * Seçin **Entity Framework kullanarak görünümleri ile MVC denetleyicisi**.

  * **Ekle**'yi tıklatın.

* İçinde **denetleyici Ekle** iletişim kutusunda:

  * İçinde **Model sınıfı** seçin **Öğrenci**.

  * İçinde **veri bağlamı sınıfı** seçin **SchoolContext**.

  * Varsayılanı kabul **StudentsController** adı.

  * **Ekle**'yi tıklatın.

  ![İskele Öğrenci](intro/_static/scaffold-student.png)

  Tıkladığınızda **Ekle**, Visual Studio yapı iskelesi motoru oluşturur bir *StudentsController.cs* dosyası ve bir görünüm kümesi (*.cshtml* dosyaları) Denetleyici ile çalışır.

(El ile oluşturmayın varsa yapı iskelesi altyapısı ayrıca veritabanı bağlamı için oluşturabilirsiniz önce bu öğreticide daha önce yaptığınız gibi. Yeni bir bağlam sınıfta belirtebilirsiniz **denetleyici Ekle** kutusunun sağ tarafındaki artı işaretine tıklayarak **veri bağlamı sınıfı**.  Visual Studio sonra oluşturacak, `DbContext` sınıfı denetleyici ve görünüm yanı sıra.)

Denetleyici sürdüğünü fark edeceksiniz bir `SchoolContext` Oluşturucusu parametre olarak.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

ASP.NET bağımlılık ekleme örneğini geçirerek dikkatli `SchoolContext` denetleyicisi içine. Uygulamasında yapılandırılmış *haline* önceki dosya.

Denetleyicisi içeren bir `Index` veritabanındaki tüm Öğrenciler görüntüler eylem yöntemi. Öğrenciler listesini yöntemi Öğrenciler varlık okuyarak kümesini alır `Students` veritabanı bağlam örneğinin özelliği:

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

Öğreticide daha sonra bu kodu zaman uyumsuz programlama öğeleri hakkında öğreneceksiniz.

*Views/Students/Index.cshtml* görünüm bu listesi bir tabloda görüntülenir:

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

Projeyi çalıştırın veya seçmek için CTRL + F5 tuşuna basın **hata ayıklama > hata ayıklama olmadan Başlat** menüsünde.

Test verileri görmek için Öğrenciler sekmesini tıklatın, `DbInitializer.Initialize` eklenen yöntemi. Bağlı olarak nasıl dar tarayıcı pencerenizi, göreceğiniz `Student` sekmesini bağlantı sayfası veya üstündeki bağlantıyı görmek için sağ üst köşesindeki gezinti simgeyi tıklatın zorunda.

![Contoso University giriş sayfası dar](intro/_static/home-page-narrow.png)

![Öğrenciler dizin sayfası](intro/_static/students-index.png)

## <a name="view-the-database"></a>Veritabanı görüntüleyin

Uygulama başlatıldığında `DbInitializer.Initialize` yöntem çağrılarını `EnsureCreated`. EF gördüğünüz hiçbir veritabanı oluştu ve bu nedenle onu oluşturan bir sonra geri kalan `Initialize` yöntemi kodu veritabanı verilerle doldurulur. Kullanabileceğiniz **SQL Server Nesne Gezgini** (Visual Studio veritabanı görüntülemek için SSOX).

Tarayıcıyı kapatın.

SSOX penceresi açık değilse, dosyayı seçin **Görünüm** Visual Studio menüsünde.

SSOX içinde tıklatın **(localdb) \MSSQLLocalDB > veritabanları**, bağlantı dizesinde yer veritabanı adı için giriş'a tıklayın, *appsettings.json* dosya.

Genişletme **tabloları** veritabanınızdaki tablolar görmek için düğüm.

![SSOX tablolarında](intro/_static/ssox-tables.png)

Sağ **Öğrenci** tablosu ve'ı tıklatın **görünüm verilerini** oluşturulan sütunları ve tablosuna satır görmek için.

![SSOX Öğrenci tabloda](intro/_static/ssox-student-table.png)

*.Mdf* ve *.ldf* veritabanı dosyaları olan *C:\Users\<kullanıcıadınız >* klasör.

Aradığınız çünkü `EnsureCreated` uygulama başlangıç çalıştıran Başlatıcı yönteminde artık bir değişiklik yapabilir `Student` sınıfı, veritabanını silin, uygulamayı yeniden çalıştırın ve veritabanı otomatik olarak değişikliğinizin eşleşecek şekilde yeniden oluşturulmuş olabilir. Örneğin, eklerseniz bir `EmailAddress` özelliğine `Student` sınıfı, göreceksiniz yeni bir `EmailAddress` yeniden oluşturulan tablonun sütun.

## <a name="conventions"></a>Kurallar

Sizin için tam bir veritabanı oluşturabilmek için Entity Framework sırayla yazma gerekiyordu kod miktarını kuralları veya Entity Framework yapar varsayımlar kullanımı nedeniyle düzeydedir.

* Adlarını `DbSet` özellikleri Tablo adları olarak kullanılır. Tarafından başvurulan olmayan varlıklar için bir `DbSet` özelliği, varlık sınıfı adları tablo adları olarak kullanılır.

* Varlık özellik adlarını sütun adları için kullanılır.

* Kimliği veya classnameID adlı varlık özellikleri birincil anahtar özelliği kabul edilir.

* Bu adlı bir özelliği bir yabancı anahtar özellik olarak yorumlanır  *<navigation property name> <primary key property name>*  (örneğin, `StudentID` için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtar `ID`). Yabancı anahtar özellikleri de adlı yalnızca  *<primary key property name>*  (örneğin, `EnrollmentID` beri `Enrollment` varlığın birincil anahtarının `EnrollmentID`).

Geleneksel davranışı geçersiz kılınabilir. Örneğin, bu öğreticide daha önce anlatıldığı gibi tablo adları açıkça belirtebilirsiniz. Sütun adları Ayarla ve içinde anlatıldığı gibi herhangi bir özelliği birincil anahtarı veya yabancı anahtar olarak ayarlayın ve bir [sonraki öğretici](complex-data-model.md) bu serideki.

## <a name="asynchronous-code"></a>Zaman uyumsuz kodu

Zaman uyumsuz programlama ASP.NET Core ve EF çekirdek için varsayılan moddur.

Bir web sunucusu kullanılabilir iş parçacıklarının sınırlı sayıda sahiptir ve yüksek yük durumlarda tüm kullanılabilir iş parçacıklarının kullanımda olabilir. Bu durum oluştuğunda sunucu yukarı serbest iş parçacığı kadar yeni istekleri işleyemiyor. G/ç tamamlamak bekliyorsunuz çünkü bunlar herhangi bir iş gerçekte yapmamanız sırasında zaman uyumlu koduyla birçok iş parçacığı bağlıdır. Bir işlemin g/ç tamamlanması beklenirken zaman uyumsuz koduyla diğer istekleri işlemek için kullanılacak sunucuyu kaydınızı kendi iş parçacığı serbest bırakılır. Sonuç olarak, zaman uyumsuz kod sunucu kaynaklarını daha verimli bir şekilde kullanılmak üzere etkinleştirir ve sunucu gecikme olmadan daha fazla trafik işlemek için etkinleştirilir.

Zaman uyumsuz kod yükünü az miktarda çalışma zamanında sağladıkları gerektirmez, ancak performans isabet yüksek trafik durumlarda, çalışırken edilebilir olduğu düşünülerek trafiği düşük durumlarda, olası performans geliştirmesi önemli.

Aşağıdaki kodda, `async` anahtar sözcüğü, `Task<T>` dönüş değeri, `await` anahtar sözcüğü ve `ToListAsync` yöntemi zaman uyumsuz yürütme kodu olun.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* `async` Anahtar sözcüğü söyler yöntemi gövde bölümlerinin geri aramalar oluşturun ve otomatik olarak oluşturmak için derleyici `Task<IActionResult>` döndürülen nesne.

* Dönüş türü `Task<IActionResult>` temsil eden bir sonuç türü ile devam eden iş `IActionResult`.

* `await` Anahtar sözcüğü yöntemi iki parçalara bölmek derleyici neden olur. İlk bölümü ile zaman uyumsuz olarak başlatıldığında işlemi sonlandırır. İkinci bölümü işlem tamamlandığında çağrılan bir geri çağırma yöntemi konur.

* `ToListAsync`zaman uyumsuz sürümü `ToList` genişletme yöntemi.

Entity Framework kullanan zaman uyumsuz kod zaman yazıyorsanız dikkat edilecek bazı noktalar:

* Sorguları ya da veritabanına gönderilen komutları neden deyimleri zaman uyumsuz olarak çalıştırılır. İçeren, örneğin, `ToListAsync`, `SingleOrDefaultAsync`, ve `SaveChangesAsync`. Bu, örneğin, yalnızca değiştirme deyimleri içermeyen bir `IQueryable`, gibi `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* İş parçacığı içinde korumalı bir EF bağlamı değil: paralel birden çok işlemleri yapmak denemeyin. Herhangi bir zaman uyumsuz EF yöntemini çağırdığınızda, her zaman kullanabilirsiniz `await` anahtar sözcüğü.

* Zaman uyumsuz kod performans yararlarını yararlanmak isterseniz herhangi bir kitaplığı paketleri emin olun (örneğin disk belleği için olduğu gibi) kullanıyorsanız, veritabanına gönderilmek üzere sorgular neden herhangi bir Entity Framework yöntem çağırırsanız zaman uyumsuz da kullanın.

. NET'te zaman uyumsuz programlama hakkında daha fazla bilgi için bkz: [zaman uyumsuz genel bakış](https://docs.microsoft.com/dotnet/articles/standard/async).

## <a name="summary"></a>Özet

Şimdi, depolamak ve verileri görüntülemek için Entity Framework Çekirdek ve SQL Server Express LocalDB kullanan basit bir uygulama oluşturdunuz. Aşağıdaki öğreticide, temel CRUD gerçekleştirme öğreneceksiniz (Oluştur, oku, Güncelleştir, Sil) işlemleri.

>[!div class="step-by-step"]
[Next](crud.md)
