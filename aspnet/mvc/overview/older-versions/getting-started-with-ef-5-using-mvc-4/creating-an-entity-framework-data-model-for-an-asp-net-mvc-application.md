---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Bir ASP.NET MVC uygulaması (1 / 10) için bir Entity Framework veri modeli oluşturma | Microsoft Docs
author: tdykstra
description: Visual Studio 2013, Entity Framework 6 ve MVC 5 Bu öğretici serisinin daha yeni bir sürümü kullanılabilir. Contoso University örnek web uygulaması gizle...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b691f718258f98e03513a089ca26b286f284765e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913244"
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Bir ASP.NET MVC uygulaması (1 / 10) için bir Entity Framework veri modeli oluşturma
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Projeyi yükle](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > A [Bu öğretici serisinin daha yeni sürümü](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) Visual Studio 2013, Entity Framework 6 ve MVC 5 için kullanılabilir.
> 
> 
> Contoso University örnek web uygulaması Entity Framework 5 ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Örnek, bir web sitesi için kurgusal Contoso üniversite uygulamasıdır. Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir. Bu öğretici serisinde Contoso University örnek uygulamanın nasıl oluşturulacağını açıklar. Yapabilecekleriniz [tamamlanmış uygulamayı karşıdan](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>İlk kod
> 
> Entity Framework verilerle çalışabilir üç yolu vardır: *veritabanı ilk*, *modeli ilk*, ve *Code First*. Bu öğretici için kod ilk bölümüdür. Senaryonuz için en uygun olanı seçin konusunda bu iş akışları ve rehberlik arasındaki farklar hakkında bilgi için bkz. [Entity Framework geliştirme iş akışlarının](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> Örnek uygulamanın üzerine kurulmuştur [ASP.NET MVC](../../../index.md). ASP.NET Web Forms modeli ile çalışmak üzere tercih ediyorsanız, bkz [Model bağlama ve Web Forms](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) öğretici serisinin ve [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Yazılım sürümleri
> 
> | **Öğreticide gösterilen** | **İle de çalışır.** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Web için Visual Studio 2012 Express. VS 2012 veya Web için VS 2012 Express zaten yoksa, Windows Azure SDK'sı tarafından otomatik olarak yüklenir. Visual Studio 2013'ün çalışması gerekir, ancak Eğitmeni ile test edilmemiştir ve bazı menü seçimlerini ve iletişim kutularında farklıdır. [VS 2013 sürümü Windows Azure SDK'sının](https://go.microsoft.com/fwlink/p/?linkid=323510) Windows Azure dağıtımı için gereklidir. |
> | .NET 4.5 | Gösterilen özelliklerin çoğu .NET 4'te çalışır, ancak bazı olmaz. Örneğin, .NET 4.5 EF enum desteği gerektirir. |
> | Entity Framework 5 |  |
> | [Windows Azure SDK'sını 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Windows Azure dağıtım adımı atlarsanız, SDK'sı gerekmez. SDK'sının yeni bir sürümü yayımlandığında, bağlantıyı yeni sürümü yükler. Bu durumda, yeni kullanıcı Arabirimi ve özellikleri için alan yönergelerden bazılarını uyum gerekebilir. |
> 
> ## <a name="questions"></a>Sorular
> 
> Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx), [Entity Framework ve LINQ to Entities Forumu](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), veya [ StackOverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>İlgili kaynaklar
> 
> Son öğretici serisinde bkz [onayları ve VB hakkında bir Not](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Öğreticinin orijinal sürüm
> 
> Öğreticinin orijinal sürüm kullanılabilir [EF 4.1 / MVC 3'e-kitap](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).


## <a name="the-contoso-university-web-application"></a>Contoso University Web uygulaması

Aşağıdaki öğreticilerde oluşturmakta uygulama basit university web sitesidir.

Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin. Oluşturacağınız ekranlar birkaçını aşağıda verilmiştir.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Entity Framework ağırlıklı olarak nasıl kullanılacağı hakkında bir öğretici odaklanabilmeniz için kullanıcı Arabirimi stili bu sitenin yerleşik şablonları tarafından üretilen yakın tutulmuştur.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticideki ekran görüntüleri ve yönergeleri, kullanmakta olduğunuz varsayılır [Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) veya [Visual Studio 2012 Express Web](https://go.microsoft.com/fwlink/?LinkID=275131), en yeni güncelleştirme ve Temmuz tarihinde yüklü .NET için Azure SDK'sı 2013. Tüm bu bağlantısıyla alabilirsiniz:

[.NET (Visual Studio 2012) için Azure SDK](https://go.microsoft.com/fwlink/?LinkId=254364)

Visual Studio yüklü değilse, yukarıdaki bağlantıyı eksik bileşenleri yükler. Visual Studio yoksa, Visual Studio 2012 Express Web için bağlantıyı yükler. Visual Studio 2013 kullanabilirsiniz, ancak bazı gerekli yordamlar ve ekranlar farklılık gösterir.

## <a name="create-an-mvc-web-application"></a>Bir MVC Web uygulaması oluşturma

Visual Studio'yu açın ve "ContosoUniversity" kullanarak adlı yeni bir C# projesi oluşturma **ASP.NET MVC 4 Web uygulaması** şablonu. Hedef emin **.NET Framework 4.5** (kullanacaksınız [ `enum` özellikleri](https://msdn.microsoft.com/data/hh859576.aspx), .NET 4.5 gerektirir).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusu seç **Internet uygulaması** şablonu.

Bırakın **Razor** seçili altyapısı görüntülemek ve bırakın **birim testi projesi oluşturma** onay kutusunu.

**Tamam**'ı tıklatın.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Site stili Ayarla

Birkaç basit değişiklikler site menü, Düzen ve giriş sayfasına ayarlar.

Açık *görünümler/paylaşılan\\_Layout.cshtml*, dosyanın içeriğini aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Bu kod, aşağıdaki değişiklikleri yapar:

- Şablon örnekleri "My ASP.NET MVC uygulaması" ve "logonuz buraya gelir", "Contoso Üniversitesi" ile değiştirir.
- Öğreticinin ilerleyen bölümlerinde kullanılan birkaç eylem bağlantıları ekler.

İçinde *Views\Home\Index.cshtml*, dosyanın içeriğini ASP.NET ve MVC şablonu paragrafları ortadan kaldırmak için aşağıdaki kodla değiştirin:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

İçinde *Controllers\HomeController.cs*, değerini `ViewBag.Message` içinde `Index` "Hoş Geldiniz Contoso University!", eylem yöntemine aşağıdaki örnekte gösterilen şekilde:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Site çalıştırmak için CTRL + F5 tuşlarına basın. Ana menü ile giriş sayfası görürsünüz.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Veri modeli oluşturma

Sonraki varlık sınıfları Contoso University uygulaması oluşturacaksınız. Aşağıdaki üç varlıklarla başlayacaksınız:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Arasında bir-çok ilişkisi `Student` ve `Enrollment` varlıkları ve bir-çok ilişkisi arasında `Course` ve `Enrollment` varlıklar. Diğer bir deyişle, bir öğrenci herhangi bir sayıda kursları kaydedilebilir ve bir kurs herhangi bir sayıda Öğrenciler içinde kayıtlı olabilir.

Aşağıdaki bölümlerde bu varlıkların her biri için bir sınıf oluşturacaksınız.

> [!NOTE]
> Tüm bu varlık sınıfları oluşturma tamamlanmadan önce projeyi derlemeyi denerseniz derleyici hataları alırsınız.


### <a name="the-student-entity"></a>Öğrenci varlık

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

İçinde *modelleri* klasör oluşturma *Student.cs* ve varolan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`StudentID` Özelliği, bu sınıf için karşılık gelen veritabanı tablosunun birincil anahtar sütunu olacak. Varsayılan olarak Entity Framework adlı bir özellik yorumlar `ID` veya *classname* `ID` birincil anahtar olarak.

`Enrollments` Özelliği bir *gezinti özelliği*. Gezinti özellikleri bu varlıkla ilgili diğer varlıkların tutun. Bu durumda, `Enrollments` özelliği bir `Student` varlık tüm tutun `Enrollment` olarak ilişkili varlıkları `Student` varlık. Diğer bir deyişle, varsa bir verilen `Student` satır veritabanında ilgili iki sahip `Enrollment` satırları (Bu öğrencinin birincil anahtarını içeren satır değerini kendi `StudentID` yabancı anahtar sütunu), bu `Student` varlığın `Enrollments` gezinme özelliği Bu iki içerecek `Enrollment` varlıklar.

Gezinti özellikleri olarak tanımlanmış genellikle `virtual` bunlar belirli Entity Framework işlevleri gibi yararlanabilir, böylece *yavaş Yükleniyor*. (Gecikmeli yükleme verilecektir daha sonra [ilgili verileri okuma](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) bu serideki sonraki öğretici.

Bir gezinme özelliği (bire çok veya tek-çok ilişkilerde) olduğu gibi birden çok varlık tutarsanız, girişleri eklenebilir, silindi ve gibi güncelleştirilmiş bir listesi türü olmalıdır `ICollection`.

### <a name="the-enrollment-entity"></a>Kayıt varlık

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

İçinde *modelleri* klasör oluşturma *Enrollment.cs* ve varolan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Sınıf özelliği bir [enum](https://msdn.microsoft.com/data/hh859576.aspx). Sonra soru işareti `Grade` türü bildirimi gösterir `Grade` özelliği [boş değer atanabilir](https://msdn.microsoft.com/library/2cf62fcy.aspx). Boş bir sınıf bir sıfır sınıf farklıdır — bir sınıf bilinen değil veya henüz atanmamış null anlamına gelir.

`StudentID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`. Bir `Enrollment` varlıktır biriyle ilişkili `Student` özelliği yalnızca tek bir içerebileceği için varlık `Student` varlık (aksine `Student.Enrollments` gezinti özelliği gördüğünüz önceki sürümlerinde, birden çok tutabilir `Enrollment` varlıklar).

`CourseID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`. Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.

### <a name="the-course-entity"></a>Kurs varlık

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

İçinde *modelleri* klasör oluşturma *Course.cs*, varolan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

`Enrollments` Özelliktir bir gezinme özelliği. A `Course` varlık dilediğiniz sayıda ilgili olabileceğini `Enrollment` varlıklar.

Daha fazla hakkında dediğimiz [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx).Hiçbiri)] sonraki öğreticide öznitelik. Temel olarak, bu öznitelik kursu yerine için oluşturmak veritabanı birincil anahtarı girmenize olanak tanır.

## <a name="create-the-database-context"></a>Veritabanı bağlamı oluşturur

Verilen veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır *veritabanı bağlamı* sınıfı. Türeterek Bu sınıf oluşturduğunuz [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) sınıfı. Kodunuzda hangi varlıkları veri modelinde yer alan belirtin. Ayrıca, belirli bir Entity Framework davranış özelleştirebilirsiniz. Bu projede adlı sınıfı `SchoolContext`.

Adlı bir klasör oluşturun *DAL* (için veri erişim katmanı). Adlı yeni bir sınıf dosyası bu klasörde oluşturma *SchoolContext.cs*, mevcut kodu şu kodla değiştirin:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Bu kod oluşturur bir [olan DB](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) her varlık kümesi özelliği. Entity Framework terminolojisinde, bir *varlık kümesi* genellikle bir veritabanı tablosuna karşılık gelir ve bir *varlık* tablosunda bir satıra karşılık gelir.

`modelBuilder.Conventions.Remove` Deyiminde [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) yöntemi pluralized tablo adları engeller. Bunu yapmadıysanız, oluşturulan tablolar sayfadayken `Students`, `Courses`, ve `Enrollments`. Bunun yerine, tablo adları olacaktır `Student`, `Course`, ve `Enrollment`. Geliştiriciler olup tablo adları veya pluralized hakkında katılmıyorum. Bu öğreticide tekil kullanır, ancak en önemli nokta dahil olmak üzere veya bu kod satırı atlama tercih hangi formu seçebilirsiniz.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) SQL Server Express Veritabanı Altyapısı'nın, isteğe bağlı olarak başlar ve kullanıcı modunda çalışan basit bir sürümüdür. Bir özel yürütme modu veritabanları ile çalışmanıza olanak tanır SQL Server Express LocalDB çalışan *.mdf* dosyaları. Genellikle, LocalDB veritabanı dosyaları saklanmaz *uygulama\_veri* web projesinin klasörüne. SQL Server Express kullanıcı örneği özelliği de ile çalışmanıza olanak tanır *.mdf* dosyaları, ancak kullanıcı örneği özelliği kullanım dışıdır; bu nedenle, LocalDB ile çalışma için önerilir *.mdf* dosyaları.

Genellikle SQL Server Express üretim web uygulamaları için kullanılmaz. IIS ile çalışmak üzere tasarlanmamıştır, çünkü LocalDB bir web uygulaması ile üretim kullanımı için özellikle önerilmez.

Visual Studio 2012 ve sonraki sürümlerinde, LocalDB Visual Studio ile varsayılan olarak yüklenir. Visual Studio 2010 ve önceki sürümlerde, SQL Server Express (LocalDB) olmadan Visual Studio ile varsayılan olarak yüklenir; Visual Studio 2010 kullanıyorsanız, el ile yüklemeniz gerekir.

Bu öğreticide, böylece veritabanı depolanabilir LocalDB ile çalışacaksınız *uygulama\_veri* klasörü olarak bir *.mdf* dosya. Açın *Web.config* dosya ve yeni bir bağlantı dizesi Ekle `connectionStrings` koleksiyonu, aşağıdaki örnekte gösterildiği gibi. (Güncelleştirdiğinizden emin olun *Web.config* kök proje klasöründeki dosya. Ayrıca bir *Web.config* dosyası *görünümleri* alt güncelleştirmeniz gerekmez.)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

Varsayılan olarak Entity Framework ile aynı adlı bir bağlantı dizesi arar `DbContext` sınıfı (`SchoolContext` bu proje için). Adlı bir LocalDB veritabanına eklediğiniz bağlantı dizesini belirtir *ContosoUniversity.mdf* bulunan *uygulama\_veri* klasör. Daha fazla bilgi için [ASP.NET Web uygulamaları için SQL Server bağlantı dizelerini](https://msdn.microsoft.com/library/jj653752.aspx).

Aslında, bağlantı dizesi belirtmeniz gerekmez. Bir bağlantı dizesi sağlamazsanız, Entity Framework sizin için oluşturur; Ancak, veritabanı içinde olmayabilir *uygulama\_veri* uygulamanızın klasör. Veritabanının oluşturulacağı hakkında daha fazla bilgi için bkz: [yeni veritabanına Code First](https://msdn.microsoft.com/data/jj193542).

`connectionStrings` Koleksiyon adlı bir bağlantı dizesi de sahip `DefaultConnection` üyelik veritabanı için kullanılır. Bu öğreticide, üyelik veritabanının kullanarak olmaz. İki bağlantı dizesini arasındaki tek fark, veritabanı adı ve ad özniteliği değeri ' dir.

## <a name="set-up-and-execute-a-code-first-migration"></a>Ayarlama ve kodu ilk geçişini Yürüt

Bir uygulama geliştirmek ilk kez başlattığınızda, verilerinizi değişiklikleri sık ve her veritabanı ile eşitlenmemiş alır model değişiklikleri model. Otomatik olarak bırakın ve veritabanı veri modeli değiştirdiğiniz her durumda yeniden oluşturmak için Entity Framework yapılandırabilirsiniz. Bu geliştirme aşamalarında bir sorun nedeniyle test verilerini kolayca yeniden oluşturulur, ancak üretim dağıttıktan sonra genellikle veritabanı şemasına veritabanı bırakmadan güncelleştirmek istediğiniz değildir. Bırakma ve yeniden oluşturulmadan veritabanını güncellemek Code First geçişleri özelliği sağlar. Geliştirme döngüsünün başlarında yeni bir proje içinde kullanmak isteyebilirsiniz [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) her bırakın, yeniden oluşturun ve yeniden veritabanının çekirdeğini oluşturma, modeli değişiklikleri saat. Bir uygulama dağıtmaya hazır olursunuz, geçişler yaklaşımı dönüştürebilirsiniz. Bu öğretici için yalnızca geçişleri kullanacaksınız. Daha fazla bilgi için [Code First Migrations](https://msdn.microsoft.com/data/jj591621) ve [geçişler yayını serisi](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Code First geçişleri etkinleştir

1. Gelen **Araçları** menüsünde tıklatın **NuGet Paket Yöneticisi** ardından **Paket Yöneticisi Konsolu**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. Adresindeki `PM>` istemine aşağıdaki komutu girin:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![geçişleri etkinleştir komutu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    Bu komut, oluşturur bir *geçişler* ContosoUniversity proje ve klasöre koyar, bu klasörde bir *Configuration.cs* geçişler yapılandırmak için düzenleyebileceğiniz bir dosya.

    ![Geçişleri klasörü](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    `Configuration` Sınıfı içeren bir `Seed` veritabanı oluşturulduğunda ve bir veri modeli sonra değişiklik güncelleştirildiğinde çağrılan yöntem.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    Bunun amacı, `Seed` yöntemdir sağlamak Code First oluşturur veya güncelleştirir, sonra test verileri veritabanına ekleyin.

### <a name="set-up-the-seed-method"></a>Seed yöntemi ayarlamak

[Çekirdek](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) yöntemi, Code First Migrations'ı bir veritabanı oluşturur ve en son geçiş için veritabanı güncelleştirmeleri her zaman çalışır. Seed yöntemi amacı, veritabanına sağlamak tablolarınızı uygulama önce veri eklemek ilk kez eriştiğinde budur.

Geçişleri bırakıldığını önce'Code First'ün önceki sürümlerinde yaygın olduğu `Seed` her model değişiklik geliştirme sırasında veritabanı tamamen silinir ve sıfırdan yeniden oluşturulması sahip olduğunuz için test verilerini eklemek için yöntemleri. Bu nedenle test verileri ile Code First Migrations, test verileri, veritabanı değişikliklerinden sonra korunur dahil [çekirdek](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) yöntemi genellikle gerekli değildir. Aslında, istemediğiniz `Seed` , geçişler veritabanı üretime dağıtmak için kullanacaksanız, test verileri eklemek için yöntemi `Seed` yöntemi, üretim ortamında çalıştırılır. Bu durumda, istediğiniz `Seed` üretimde eklenmesini istediğiniz verileri veritabanına eklemek için yöntemi. Örneğin, gerçek bölüm adlarında veritabanına isteyebileceğiniz `Department` uygulama üretimde kullanılabilir hale geldiğinde tablo.

Bu öğreticide, geçişler dağıtım için kullanacaksınız ancak sizin `Seed` yöntemi ekler test verilerini yine de çok fazla veri el ile eklemek zorunda kalmadan uygulama işlevselliğini nasıl çalıştığını görmek kolaylaştırmak.

1. Öğesinin içeriğini değiştirin *Configuration.cs* yeni veritabanına test verilerini yükler aşağıdaki kod ile dosya. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    [Çekirdek](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) yöntemi giriş parametresi olarak veritabanı bağlam nesnesi alır ve yeni varlıklar eklemek için bu nesne yöntemindeki kodu kullanır. Her varlık türü için kodu yeni varlıklar koleksiyonu oluşturur, bunları uygun ekler [olan DB](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) özellik ve değişiklikleri veritabanına kaydeder. Çağrı için gerekli olmayan [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) yöntemi her grubu varlıkların sonra olarak burada yapılır, ancak, bunu yardımcı olur, veritabanına kod yazarken bir özel durum oluşursa, bir sorunun kaynağını bulun.

    Veri INSERT deyimleri bazılarını [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) bir "upsert" işlemi gerçekleştirmek için yöntemi. Çünkü `Seed` yöntemi, her geçiş ile çalışır, eklemeye çalıştığınız satırların zaten var. veritabanı oluşturan ilk geçişten sonra olacağından, verileri yalnızca ekleyemezsiniz. "Upsert" işlemi zaten var olan bir satır, ancak eklemeye çalışırsanız, olacağını hataların ***geçersiz kılmalar*** , uygulamayı test ederken yaptığınız değişiklikler. Bazı tablolar test verileri, bunun gerçekleşmesi için istemeyebilirsiniz: Bazı durumlarda test ederken verileri değiştirdiğinizde değişikliklerinizi veritabanı güncelleştirmelerinden sonra kalmasını istiyor. Bu durumda koşullu ekleme işlemi yapmak istediğiniz: yalnızca zaten mevcut değilse bir satır ekleyin. Seed yöntemi her iki yaklaşım kullanır.

    Geçirilen ilk parametre [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) özelliği bir satır zaten mevcut olup olmadığını denetlemek için kullanılacak yöntemi belirtir. Sağlama, test Öğrenci verilerin `LastName` özelliği listedeki son her ad benzersiz olduğundan bu amaç için kullanılabilir:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Bu kod, son adlarının benzersiz olduğunu varsayar. Bir öğrenci bir yinelenen Soyadı ile el ile eklerseniz, sonraki açışınızda bir geçiş gerçekleştirmek şu özel durum elde edersiniz.

    Birden fazla öğe dizisi içeriyor

    Hakkında daha fazla bilgi için `AddOrUpdate` yöntemi bkz [EF 4.3 AddOrUpdate yöntemiyle ilgileniriz](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Julie Lerman'ın blogunda.

    Ekler kod `Enrollment` varlıkları kullanmaz `AddOrUpdate` yöntemi. Bir varlık zaten var ve mevcut değilse varlığı yerleştirir denetler. Bu yaklaşım, geçişler çalıştırdığınızda, bir kayıt ataması yaptığınız değişiklikleri korur. Kod her üyesi döngü `Enrollment` [listesi](https://msdn.microsoft.com/library/6sh2ey19.aspx) ve veritabanında kayıt bulunmazsa, kayıt veritabanına ekler. Her kayıt ekleyecek şekilde veritabanını güncelleştirmek ilk kez veritabanı boş olacaktır.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Hata ayıklama hakkında bilgi için `Seed` yöntemi ve "Alexander Carson" adlı iki Öğrenciler gibi gereksiz verilerin nasıl işleneceğini [Seeding ve hata ayıklama Entity Framework (EF) Db'ler](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) Rick Anderson'un blogunda.
2. Projeyi oluşturun.

### <a name="create-and-execute-the-first-migration"></a>Oluşturma ve ilk geçiş yürütme

1. Paket Yöneticisi konsolu penceresinde, aşağıdaki komutları girin: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    `add-migration` Komut ekler geçişler klasörüne bir *[tarih damgası]\_InitialCreate.cs* veritabanı oluşturan kodu içeren dosya. İlk parametre (`InitialCreate)` dosya için kullanılan ad ve istediğiniz olabilir; genellikle bir sözcük veya tümcecik geçiş yapıldığını özetleyen seçin. Örneğin, bir sonraki geçiş adını verebilirsiniz &quot;AddDepartmentTable&quot;.

    ![İlk geçiş geçişleri klasörü](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    `Up` Yöntemi `InitialCreate` sınıf veri modeli varlık kümeleri için karşılık gelen veritabanı tabloları oluşturur ve `Down` yöntemi bunları siler. Geçişleri çağrıları `Up` geçiş için veri modeli değişikliklerini uygulamak için yöntemi. Güncelleştirme, geçişler çağrıları geri almak için bir komutu girdiğinizde `Down` yöntemi. Aşağıdaki kod içeriği gösterilir `InitialCreate` dosyası:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    `update-database` Komutu çalıştırmaları `Up` veritabanını ve ardından yöntemini çalıştırır `Seed` veritabanını doldurmak için yöntemi.

Bir SQL Server veritabanı için veri modeliniz artık oluşturuldu. Veritabanının adıdır *ContosoUniversity*ve *.mdf* dosyasıdır projenizin *uygulama\_veri* klasör olarak belirttiğiniz olduğu için bağlantı dizesi.

Kullanabilirsiniz **Sunucu Gezgini** veya **SQL Server Nesne Gezgini** (SSOX Visual Studio'daki veritabanını görüntülemek için). Bu öğretici için kullanacağınız **Sunucu Gezgini**. Visual Studio Express 2012 Web, **Sunucu Gezgini** çağrılır **veritabanı Gezgini**.

1. Gelen **görünümü** menüsünde tıklatın **Sunucu Gezgini**.
2. Tıklayın **Bağlantı Ekle** simgesi.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. İle istenirse **veri kaynağı Seç** iletişim kutusunda, tıklayın **Microsoft SQL Server**ve ardından **devam**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. İçinde **Bağlantı Ekle** iletişim kutusuna **(localdb) \v11.0** için **sunucu adı**. Altında **bir veritabanı adı seçin veya girin**seçin **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. **Tamam**'a tıklayın.
6. Genişletin **SchoolContext** ve ardından **tabloları**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Sağ **Öğrenci** tıklayın ve tablo **tablo verilerini Göster** oluşturulan sütunları ve tabloya eklenen satırları görebilirsiniz.

    ![Öğrenci tablosu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Bir öğrenci denetleyicisi ve görünümler oluşturma

Sonraki adım bir ASP.NET MVC denetleyici ve görünüm bu tablolardan birinin ile çalışabilir, uygulamanızda oluşturmaktır.

1. Oluşturmak için bir `Student` denetleyicisi sağ **denetleyicileri** klasöründe **Çözüm Gezgini**seçin **Ekle**ve ardından **denetleyicisi** . İçinde **denetleyici Ekle** iletişim kutusunda, aşağıdaki seçimleri yapın ve ardından **Ekle**: 

   - Denetleyici adı: **StudentController**.
   - Şablonu: **okuma/yazma eylemleri ve Entity Framework kullanarak görünümler ile MVC denetleyicisi**.
   - Model sınıfı: **Öğrenci (ContosoUniversity.Models)**. (Aşağı açılan listede bu seçeneği görmüyorsanız, projeyi oluşturun ve yeniden deneyin.)
   - Veri bağlamı sınıfı: **SchoolContext (ContosoUniversity.Models)**.
   - Görünümler: **Razor (CSHTML)**. (Varsayılan)

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Visual Studio açılır *Controllers\StudentController.cs* dosya. Bir veritabanı bağlam nesnesi başlatan bir sınıf değişken oluşturuldu görürsünüz:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     `Index` Eylem yöntemine öğrencilerden listesini alır *Öğrenciler* varlık kümesi okuyarak `Students` veritabanı bağlam örneğinin özelliği:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     *Student\Index.cshtml* bu liste bir tabloda görüntüleyen:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Projeyi çalıştırmak için CTRL + F5 tuşlarına basın.

     Tıklayın **Öğrenciler** test verilerini görmek için sekmesinde, `Seed` eklenen yöntemi.

     ![Öğrenci dizin sayfası](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Kurallar

Sizin için tam bir veritabanı oluşturmak Entity Framework için sırayla yazmak için olan kod kullanımı nedeniyle en az *kuralları*, veya Entity Framework yapan varsayımlar. Bunlardan bazıları zaten Not:

- Varlık sınıfı adları pluralized formlar, tablo adları kullanılır.
- Varlık özellik adlarını sütun adları için kullanılır.
- Adlandırılmış varlık özellikleri `ID` veya *classname* `ID` birincil anahtar özellik olarak tanınır.

Kuralları geçersiz kılınabilir gördüğünüze göre (örneğin, tablo adları pluralized olmamalıdır belirttiğiniz), ve kuralları ve bunları geçersiz kılma hakkında daha fazla bilgi edineceksiniz [daha karmaşık bir veri modeli oluşturma](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) Öğreticisi Daha sonra bu dizide. Daha fazla bilgi için [kod öncelikli kurallar](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Özet

Depolamak ve verileri görüntülemek için Entity Framework ve SQL Server Express kullanan basit bir uygulama oluşturdunuz. Aşağıdaki öğreticide temel CRUD gerçekleştirmeyi öğreneceksiniz (oluşturma, okuma, güncelleştirme ve silme) işlemleri. Bu sayfanın sonundaki geri bildirim bırakabilir. Lütfen nasıl öğreticinin bu bölümü sevmediğinizi ve nasıl geliştirebileceğimiz bize bildirin.

Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Next](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
