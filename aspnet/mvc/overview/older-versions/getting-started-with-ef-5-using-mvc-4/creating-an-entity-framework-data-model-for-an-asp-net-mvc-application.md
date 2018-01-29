---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: "Bir ASP.NET MVC uygulaması (1 / 10) için bir Entity Framework veri modeli oluşturma | Microsoft Docs"
author: tdykstra
description: "Visual Studio 2013, Entity Framework 6 ve MVC 5 için Bu öğretici seri daha yeni bir sürümü kullanılabilir. Contoso University örnek web uygulaması de..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 223dd48bb996de527f20291e4701e7d1b60a539d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Bir ASP.NET MVC uygulaması (1 / 10) için bir Entity Framework veri modeli oluşturma
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > A [sürümü Bu öğretici seri](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) Visual Studio 2013, Entity Framework 6 ve MVC 5 için kullanılabilir.
> 
> 
> Contoso University örnek web uygulaması Entity Framework 5 ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulama, kurgusal bir Contoso üniversite için bir web sitesidir. Öğrenci giriş, indirmelere oluşturma ve Eğitmen atamaları gibi işlevselliği içerir. Bu öğretici seri Contoso University örnek uygulamasının nasıl oluşturulacağını açıklar. Yapabilecekleriniz [tamamlanmış uygulamayı karşıdan](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>İlk kod
> 
> Entity Framework verilerle çalışma üç yolu vardır: *veritabanı ilk*, *Model First*, ve *Code First*. Bu öğretici için Code First ' dir. Senaryonuz için en uygun olanı seçmeniz konusunda bu iş akışları ve Kılavuzu arasındaki farklar hakkında bilgi için bkz: [Entity Framework geliştirme iş akışları](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> Örnek uygulama yerleşik [ASP.NET MVC](../../../index.md). ASP.NET Web Forms modeli ile çalışmayı tercih ederseniz bkz [Model bağlama ve Web Forms](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) öğretici serisi ve [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Yazılım sürümleri
> 
> | **Öğreticide gösterilen** | **İle de çalışır** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Web için Visual Studio 2012 Express. VS 2012 veya Web için VS 2012 Express zaten yoksa, bu Windows Azure SDK'sı tarafından otomatik olarak yüklenir. Visual Studio 2013 çalışması gerekir, ancak öğreticinin ile test edilmemiştir ve bazı menü seçimlerini ve iletişim kutuları farklı. [Windows Azure SDK'sını VS 2013 sürümü](https://go.microsoft.com/fwlink/p/?linkid=323510) Windows Azure dağıtımı için gereklidir. |
> | .NET 4.5 | .NET 4'te gösterilen özelliklerin çoğunu çalışır, ancak bazı olmaz. Örneğin, .NET 4.5 EF enum desteği gerektirir. |
> | Entity Framework 5 |  |
> | [Windows Azure SDK 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Windows Azure dağıtım adımı atlarsanız, SDK gerekmez. SDK'sının yeni bir sürümü yayımlandığında, bağlantının ardından yeni sürümü yükleyin. Bu durumda, yeni kullanıcı Arabirimi ve özellikleri yönergeleri bazıları uyum gerekebilir. |
> 
> ## <a name="questions"></a>Sorular
> 
> Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx), [Entity Framework ve LINQ to Entities Forumu](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), veya [ StackOverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>İlgili kaynaklar
> 
> Seri için son öğreticide gördüğünüz [ilgili kaynaklar ve VB hakkında bir Not](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Öğreticinin özgün sürüm
> 
> Öğreticinin orijinal sürüm kullanılabilir [EF 4.1 / MVC 3 e-kitap](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).


## <a name="the-contoso-university-web-application"></a>Contoso University Web uygulaması

Bu öğreticiler oluşturmakta uygulama basit university web sitesidir.

Kullanıcıların görüntüleyebileceği ve Öğrenci, indirmelere ve Eğitmen bilgileri güncelleştirin. Oluşturacağınız ekranlar bazılarını aşağıda verilmiştir.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Öğretici Entity Framework çoğunlukla nasıl kullanılacağı hakkında odaklanabilmeniz bu sitenin UI Stili yerleşik şablonlar tarafından oluşturulan yakın tutulmuştur.

## <a name="prerequisites"></a>Önkoşullar

Yönergeleri ve ekran görüntüleri Bu öğreticide, kullanmakta olduğunuz varsayılır [Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) veya [için Visual Studio 2012 Express Web](https://go.microsoft.com/fwlink/?LinkID=275131), en son güncelleştirme ve Temmuz itibariyle yüklü .NET için Azure SDK ile 2013. Tüm bunlar aşağıdaki bağlantıyla alabilirsiniz:

[.NET (Visual Studio 2012) için Azure SDK](https://go.microsoft.com/fwlink/?LinkId=254364)

Visual Studio yüklüyse, yukarıdaki bağlantıyı eksik bileşenleri yükleyin. Visual Studio yoksa, bağlantı için Visual Studio 2012 Express Web yükler. Visual Studio 2013 kullanabilirsiniz, ancak bazı gerekli yordamlar ve ekranları farklılık gösterir.

## <a name="create-an-mvc-web-application"></a>Bir MVC Web uygulaması oluşturma

Visual Studio'yu açın ve "ContosoUniversity" kullanarak adlı yeni bir C# projesi oluşturun **ASP.NET MVC 4 Web uygulaması** şablonu. Hedef emin olun **.NET Framework 4.5** (kullanmaya başlayacağınız [ `enum` özellikleri](https://msdn.microsoft.com/data/hh859576.aspx), .NET 4.5 gerektirir).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusu seç **Internet uygulama** şablonu.

Bırakın **Razor** görünümünde seçili altyapısı ve bırakın **birim testi projesi oluşturma** onay kutusunu.

**Tamam**'ı tıklatın.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Site stil ayarlama

Birkaç basit değişiklikler sitesi menüsü, Düzen ve giriş sayfası ayarlar.

Açık *görünümler/paylaşılan\\_Layout.cshtml*ve dosyasının içeriğini aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Bu kod, aşağıdaki değişiklikleri yapar:

- "ASP.NET MVC Uygulamam" ve "Buraya logonuz konacak" şablonu örnekleri "Contoso University" ile değiştirir.
- Öğreticide daha sonra kullanılacak birkaç eylem bağlantıları ekler.

İçinde *Views\Home\Index.cshtml*, dosyanın içeriğini ASP.NET MVC hakkında şablon paragrafları ortadan kaldırmak için aşağıdaki kod ile değiştirin:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

İçinde *Controllers\HomeController.cs*, değerini değiştirin `ViewBag.Message` içinde `Index` "Hoş Geldiniz Contoso University!", eylem yöntemine aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Site çalıştırmak için CTRL + F5 tuşuna basın. Ana menü ile giriş sayfasına bakın.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Veri modeli oluşturma

Sonraki Contoso University uygulama için varlık sınıfları oluşturacaksınız. Aşağıdaki üç varlıklarıyla başlayacaksınız:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Arasında bir-çok ilişkisi olduğundan `Student` ve `Enrollment` varlıkları ve arasında bir-çok ilişkisi olduğundan `Course` ve `Enrollment` varlıklar. Diğer bir deyişle, öğrencinin herhangi bir sayıda kurslar kaydedilebilir ve bir indirmelere içinde kayıtlı Öğrenciler herhangi bir sayıda olabilir.

Aşağıdaki bölümlerde bu varlıkların her biri için bir sınıf oluşturacaksınız.

> [!NOTE]
> Tüm bu varlık sınıfları oluşturma bitirmeden Projeyi derlemek çalışırsanız, derleyici hataları alırsınız.


### <a name="the-student-entity"></a>Öğrenci varlık

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

İçinde *modelleri* klasörü oluşturmak *Student.cs* ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`StudentID` Özelliği, bu sınıfa karşılık gelen veritabanı tablosunun birincil anahtar sütunu hale gelir. Varsayılan olarak, Entity Framework adlı bir özellik yorumlar `ID` veya *classname* `ID` birincil anahtar olarak.

`Enrollments` Özelliği bir *gezinti özelliği*. Gezinti özellikleri bu varlıkla ilgili diğer varlıklar tutun. Bu durumda, `Enrollments` özelliği bir `Student` varlık tüm tutun `Enrollment` için ilişkili olan varlıkların `Student` varlık. Diğer bir deyişle, varsa bir verilen `Student` satır veritabanındaki sahip iki ilgili `Enrollment` satır (Bu öğrencinin birincil anahtar içeren satırları değeri kendi `StudentID` yabancı anahtar sütunu), o `Student` varlığın `Enrollments` gezinti özelliği Bu iki içerecek `Enrollment` varlıklar.

Gezinti özellikleri olarak tanımlanmış genellikle `virtual` böylece bunlar gibi bazı Entity Framework işlevleri avantajlarından yararlanmak *yavaş Yükleniyor*. (Yavaş yükleniyor verilecektir daha sonra [ilgili veri okunurken](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) bu serideki sonraki öğretici.

Bir gezinti özelliği (çok- veya -çok ilişkileri) olduğu gibi birden çok varlık tutarsanız türünü hangi girişleri eklenebilir, silinen ve gibi güncelleştirilmiş bir listesi olmalıdır `ICollection`.

### <a name="the-enrollment-entity"></a>Kayıt varlık

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

İçinde *modelleri* klasörü oluşturmak *Enrollment.cs* ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Sınıf özelliği bir [enum](https://msdn.microsoft.com/data/hh859576.aspx). Soru işareti sonra `Grade` türü bildirimi gösterir `Grade` özelliği [boş değer atanabilir](https://msdn.microsoft.com/library/2cf62fcy.aspx). Null bir düzeyde bir sıfır ataması farklı. — bir sınıf bilinen değil veya henüz atanmamış null anlamına gelir.

`StudentID` Özelliği bir yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`. Bir `Enrollment` varlıktır biriyle ilişkili `Student` özelliği yalnızca tek bir tutmak için varlık `Student` varlık (aksine `Student.Enrollments` gezinti özelliği gördüğünüz daha önceki sürümlerde, birden çok tutabilir `Enrollment` varlıklar).

`CourseID` Özelliği bir yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`. Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.

### <a name="the-course-entity"></a>İndirmelere varlık

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

İçinde *modelleri* klasörü oluşturmak *Course.cs*, var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

`Enrollments` Özelliği bir gezinti özelliğidir. A `Course` varlık herhangi bir sayıda için ilgili olabileceğini `Enrollment` varlıklar.

Daha fazla hakkında dediğimiz [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx).Hiçbiri)] sonraki öğreticide öznitelik. Temel olarak, bu öznitelik indirmelere yerine için oluşturmak veritabanı birincil anahtarı girmenizi sağlar.

## <a name="create-the-database-context"></a>Veritabanı bağlamı oluşturma

Verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıf *veritabanı bağlamı* sınıfı. Bu sınıf türetme tarafından oluşturduğunuz [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) sınıfı. Kodunuzda hangi varlıkların veri modelinde bulunan belirtin. Ayrıca, belirli bir Entity Framework davranış özelleştirebilirsiniz. Bu projede adlı sınıfı `SchoolContext`.

Adlı bir klasör oluşturun *DAL* (için veri erişim katmanı). Bu klasörde adlı yeni bir sınıf dosyası oluşturun *SchoolContext.cs*ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Bu kod oluşturur bir [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) özelliği her bir varlık kümesi. Entity Framework terminoloji içinde bir *varlık kümesini* genellikle bir veritabanı tablosuna karşılık gelir ve bir *varlık* tablosunda bir satırı karşılık gelir.

`modelBuilder.Conventions.Remove` Deyiminde [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) yöntemi, pluralized öğesinden tablo adları önler. Bunu siz yaparsanız, oluşturulan tablolar sayfadayken `Students`, `Courses`, ve `Enrollments`. Bunun yerine, tablo adları olacaktır `Student`, `Course`, ve `Enrollment`. Geliştiriciler olup tablo adları veya pluralized hakkında katılmıyorum. Bu öğretici tekil kullanır, ancak önemli noktadır dahil olmak üzere veya bu kod satırı atlama tercih hangi formu seçebilirsiniz.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[Yerel veritabanı](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) basit bir sürümü SQL Server Express Veritabanı Altyapısı'nın, isteğe bağlı olarak başlatılır ve kullanıcı modunda çalışır. Özel yürütme modunu veritabanları ile çalışmanıza olanak tanır SQL Server Express LocalDB çalışan *.mdf* dosyaları. Genellikle, yerel veritabanı veritabanı dosyaları saklanmaz *uygulama\_veri* bir web projesi klasörü. SQL Server Express kullanıcı örneği özelliği de ile çalışmanıza olanak tanır *.mdf* dosyaları, ancak kullanıcı örneği özelliği kullanım dışıdır; bu nedenle, yerel veritabanı ile çalışmak için önerilir *.mdf* dosyaları.

Genellikle SQL Server Express üretim web uygulamaları için kullanılmaz. IIS ile çalışmak üzere tasarlanmadı çünkü LocalDB bir web uygulaması ile üretim kullanımı için özellikle önerilmez.

Visual Studio 2012 ve sonraki sürümlerinde LocalDB Visual Studio ile varsayılan olarak yüklenir. Visual Studio 2010 ve önceki sürümlerinde, SQL Server Express (LocalDB) olmadan Visual Studio ile varsayılan olarak yüklenir; Visual Studio 2010 kullanıyorsanız, el ile yüklemeniz gerekir.

Bu öğreticide böylece veritabanı depolanabilir LocalDB ile karşılaşmayacağınızı *uygulama\_veri* klasörü olarak bir *.mdf* dosya. Kök açmak *Web.config* dosya ve yeni bir bağlantı dizesi eklemek `connectionStrings` aşağıdaki örnekte gösterildiği gibi koleksiyonu. (Güncelleştirdiğinizden emin olun *Web.config* kök proje klasöründeki dosya. Ayrıca bir *Web.config* dosyası *görünümleri* alt güncelleştirmeniz gerekmez.)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

Varsayılan olarak, aynı adlı bir bağlantı dizesi Entity Framework arar `DbContext` sınıfı (`SchoolContext` bu proje için). Adlı bir yerel veritabanı veritabanı eklediğiniz bağlantı dizesini belirtir *ContosoUniversity.mdf* bulunan *uygulama\_veri* klasör. Daha fazla bilgi için bkz: [ASP.NET Web uygulamaları için SQL Server bağlantı dizelerini](https://msdn.microsoft.com/library/jj653752.aspx).

Gerçekte bağlantı dizesi belirtmeniz gerekmez. Bir bağlantı dizesi girmezseniz, Entity Framework sizin için oluşturur; Ancak, veritabanı içinde olmayabilir *uygulama\_veri* , uygulamanızın klasör. Veritabanının oluşturulacağı hakkında daha fazla bilgi için bkz: [yeni bir veritabanına ilk kod](https://msdn.microsoft.com/data/jj193542).

`connectionStrings` De gruplandırmasında adlı bir bağlantı dizesi `DefaultConnection` üyelik veritabanı için kullanılır. Bu öğreticide üyelik veritabanının kullanarak olmaz. İki bağlantı dizesini arasındaki tek fark, veritabanı adı ve ad öznitelik değeri ' dir.

## <a name="set-up-and-execute-a-code-first-migration"></a>Ayarlama ve bir kod ilk geçiş yürütme

Uygulama geliştirme ilk kez başlattığınızda, verilerinizi değişikliklerini sık sık ve her zaman veritabanı ile eşitlenmemiş alır model değişiklikleri model. Otomatik olarak bırakın ve veritabanı veri modeli değiştirdiğinizde yeniden oluşturmak için Entity Framework yapılandırabilirsiniz. Bu sorunun erken geliştirme test verileri kolayca yeniden oluşturulur, ancak üretim dağıttıktan sonra genellikle veritabanı şeması veritabanı bırakmadan güncelleştirmek istediğiniz değildir. Bırakma ve yeniden oluşturma veritabanını güncelleştirmek Code First geçişleri özelliği sağlar. Erken yeni bir proje geliştirme döngüsü kullanmak isteyebilirsiniz [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) bırakma, yeniden oluşturun ve veritabanını yeniden Çekirdek değer her zaman model değişiklikleri. Uygulamanızı dağıtmak hazır edinebileceğinizi, geçişler yaklaşımı dönüştürebilirsiniz. Bu öğretici için geçişler yalnızca kullanacağız. Daha fazla bilgi için bkz: [Code First Migrations](https://msdn.microsoft.com/data/jj591621) ve [geçişler ekran kaydı serisi](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Code First geçişleri etkinleştir

1. Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi** ve ardından **Paket Yöneticisi Konsolu**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. Konumundaki `PM>` istemine aşağıdaki komutu girin:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![Enable-migrations komutunu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    Bu komut oluşturur bir *geçişler* ContosoUniversity proje ve bu klasöre koyar o klasördeki bir *Configuration.cs* geçişler yapılandırmak için düzenleyebilirsiniz dosya.

    ![Geçişler klasörü](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    `Configuration` Sınıfı içeren bir `Seed` veritabanı oluşturulduğunda ve bir veri modeli sonra değişiklik güncelleştirilir edildiğinde çağrılan yöntemi.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    Bunun amacı, `Seed` yöntemidir, Code First oluşturur veya güncelleştirdiği sonra test verileri veritabanına eklemek etkinleştirmek için.

### <a name="set-up-the-seed-method"></a>Set Seed yöntemi

[Çekirdek](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) yöntemi Code First Migrations veritabanı oluşturduğunda ve veritabanı için en son geçiş güncelleştirmeleri her zaman çalışır. Seed yöntemi amacı, uygulamanın önce tabloları veri eklemek etkinleştirmek için ilk kez veritabanına erişir budur.

Geçişler serbest önce önceki sürümlerinde Code First, bu yaygın `Seed` her modeli değişiklik geliştirme sırasında veritabanı tamamen silinir ve baştan yeniden oluşturulması sahip olduğunuz için test verileri eklemek için yöntem. Code First Migrations, veriler, veritabanı değişikliklerinden sonra tutulur test şekilde test verileri dahil olmak üzere [çekirdek](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) yöntemi genellikle gerekli değildir. Aslında, istemediğiniz `Seed` , geçişler veritabanı üretime dağıtmak için kullanacaksanız test verileri eklemek için yöntemi `Seed` yöntemi üretimde çalışır. Bu durumda, istediğiniz `Seed` üretimde eklenmesini istediğiniz verileri veritabanına eklemek için yöntem. Örneğin, gerçek bölüm adlarında veritabanına isteyebilirsiniz `Department` uygulama üretimde kullanılabilir hale geldiğinde tablo.

Bu öğretici için geçiş dağıtımı için kullanıyor olmanız, ancak, `Seed` yöntemi ekler test verileri yine de çok miktarda veri el ile eklemek zorunda kalmadan uygulama işlevlerinin nasıl çalıştığını görmek kolaylaştırmak için.

1. Değiştir *Configuration.cs* test verileri yeni veritabanına yükler aşağıdaki kod ile dosya. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    [Çekirdek](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) yöntemi giriş parametresi olarak veritabanı bağlamı nesnesini alır ve yeni varlıklar veritabanına eklemek için söz konusu nesne yöntemindeki kod kullanır. Her bir varlık türü için kod yeni varlıklar koleksiyonu oluşturur, uygun ekler [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) özellik ve değişiklikler veritabanına kaydeder. Çağrı için gerekli olmayan [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) yöntemi her grubu varlıkların sonra olarak burada yapılır, ancak, bunu yardımcı olan kod veritabanına yazma sırasında bir özel durum oluşursa, bir sorunun kaynağını bulun.

    Bazı veri INSERT deyimleri kullanmak [örnek](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) bir "upsert" işlem yapmak için yöntemi. Çünkü `Seed` yöntemi her geçiş ile çalışır, eklemeye çalıştığınız satır zaten var. bir veritabanı oluşturur ilk geçişten sonra olacağından, verileri, yalnızca ekleyemezsiniz. "Upsert" işlemi zaten bir satır, ancak eklemeye çalışırsanız, olacağını hatalarını önler ***geçersiz kılmaları*** uygulamayı test ederken yapmış olabileceğiniz veri değişiklikleri. Bazı tablolardaki test verilerle, gerçekleşmesi için istemeyebilirsiniz: Bazı durumlarda test ederken verileri değiştirdiğinizde değişikliklerinizi sonra veritabanı güncelleştirmeleri kalmasını istiyor. Bu durumda bir koşullu ekleme işlemi yapmak istiyor: yalnızca zaten yoksa bir satır ekleyin. Seed yöntemi her iki yaklaşımın kullanır.

    Geçirilen ilk parametre [örnek](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) yöntemi bir satır zaten var olup olmadığını denetlemek için kullanılacak özellik belirtir. Sağlama, test Öğrenci veriler için `LastName` özelliği listedeki son her ad benzersiz olduğundan bu amaç için kullanılabilir:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Bu kod, son adlarının benzersiz olduğunu varsayar. Yinelenen bir soyadı ile öğrencinin el ile eklerseniz, şu özel bir geçiş gerçekleştirmek sonraki açışınızda elde edersiniz.

    Sıra birden fazla öğe içeriyor

    Hakkında daha fazla bilgi için `AddOrUpdate` yöntemi, bkz: [ilgilenebilmek EF 4.3 örnek yöntemiyle](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Julie Lerman'ın blogunda.

    Ekleyen kod `Enrollment` olmayan varlıklar kullanmak `AddOrUpdate` yöntemi. Bir varlık zaten var ve yoksa varlık ekler olmadığını denetler. Bu yaklaşım geçişler çalıştırdığınızda, bir kayıt ataması yaptığınız değişiklikler korur. Kod döngüler her üye `Enrollment` [listesi](https://msdn.microsoft.com/library/6sh2ey19.aspx) ve kayıt veritabanında bulunmazsa, veritabanına kayıt ekler. Her kayıt ekleyecek şekilde veritabanını güncelleştirmek ilk kez veritabanı boş olacaktır.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Hata ayıklama hakkında bilgi için `Seed` yöntemi ve "Alexander Cihangir'in, Benli" adlı iki Öğrenciler gibi gereksiz verileri nasıl ele alınacağını bkz [Seeding ve hata ayıklama Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) Rick Anderson'un blogunda.
2. Projeyi oluşturun.

### <a name="create-and-execute-the-first-migration"></a>Oluşturma ve ilk geçiş yürütme

1. Paket Yöneticisi konsolu penceresinde aşağıdaki komutları girin: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    `add-migration` Komut ekler geçişler klasörüne bir *[tarih damgası]\_InitialCreate.cs* veritabanı oluşturan kodu içeren dosya. İlk parametre (`InitialCreate)` dosyası için kullanılan ad ve istediğiniz olabilir; genellikle bir sözcük veya tümcecik geçişte yapıldığını özetler seçin. Örneğin, bir sonraki geçiş adlandırabilirsiniz &quot;AddDepartmentTable&quot;.

    ![İlk geçiş ile geçişler klasörü](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    `Up` Yöntemi `InitialCreate` sınıf veri modeli varlık kümeleri için karşılık gelen veritabanı tabloları oluşturur ve `Down` yöntemi bunları siler. Geçişler çağrıları `Up` geçiş için veri modeli değişikliklerini uygulamak için yöntem. Geri alma güncelleştirme, geçişler çağrıları komutu girdiğinizde `Down` yöntemi. Aşağıdaki kod içeriğini gösterir `InitialCreate` dosyası:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    `update-database` Komutu çalıştırır `Up` çalışan veritabanını ve ardından oluşturmak için yöntemini `Seed` veritabanını doldurmak için yöntem.

Bir SQL Server veritabanı için veri modelinizi şimdi oluşturdu. Veritabanı adı *ContosoUniversity*ve *.mdf* dosyasıdır projenizin içinde *uygulama\_veri* klasörü içinde belirttiğiniz olduğu için bağlantı dizesi.

Kullanabilirsiniz **Sunucu Gezgini** veya **SQL Server Nesne Gezgini** (Visual Studio veritabanı görüntülemek için SSOX). Bu öğretici için kullanacağınız **Sunucu Gezgini**. Visual Studio Express 2012 için Web içinde **Sunucu Gezgini** çağrılır **Database Explorer**.

1. Gelen **Görünüm** menüsünde tıklatın **Sunucu Gezgini**.
2. Tıklatın **Bağlantı Ekle** simgesi.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. İle istenirse **veri kaynağı Seç** iletişim kutusunda, tıklatın **Microsoft SQL Server**ve ardından **devam**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. İçinde **Bağlantı Ekle** iletişim kutusunda, girin **(localdb) \v11.0** için **sunucu adı**. Altında **seçin veya bir veritabanı adı girin**seçin **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. **Tamam**'a tıklayın.
6. Genişletme **SchoolContext** genişletin ve ardından **tabloları**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Sağ **Öğrenci** tablosu ve'ı tıklatın **Show Table Data** oluşturulan sütunları ve tablosuna satır görmek için.

    ![Öğrenci tablosu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Bir öğrenci denetleyicisi ve görünümler oluşturma

Sonraki adım bir ASP.NET MVC denetleyicisi ve görünümler bu tablolar biri ile çalışabilir, uygulamanızda oluşturmaktır.

1. Oluşturmak için bir `Student` denetleyicisi sağ **denetleyicileri** klasöründe **Çözüm Gezgini**seçin **Ekle**ve ardından **denetleyicisi** . İçinde **denetleyici Ekle** iletişim kutusunda, aşağıdaki seçimleri yapın ve ardından **Ekle**: 

    - Denetleyici adı: **StudentController**.
    - Şablonu: **okuma/yazma eylemleri ve Entity Framework kullanarak görünümleri ile MVC denetleyicisi**.
    - Model sınıfı: **Öğrenci (ContosoUniversity.Models)**. (Aşağı açılan listede bu seçeneği görmüyorsanız, projeyi oluşturun ve yeniden deneyin.)
    - Veri bağlamı sınıfı: **SchoolContext (ContosoUniversity.Models)**.
    - Görünümleri: **Razor (CSHTML)**. (Varsayılan)

    ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
- Visual Studio açılır *Controllers\StudentController.cs* dosya. Bir sınıf değişkeni, bir veritabanı bağlam nesnesi başlatır oluşturuldu bakın:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

    `Index` Eylem yöntemi Öğrenciler listesini alır *Öğrenciler* varlık okuyarak kümesi `Students` veritabanı bağlam örneğinin özelliği:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

    *Student\Index.cshtml* görünüm bu listesi bir tabloda görüntülenir:

    [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
- Projeyi çalıştırmak için CTRL + F5 tuşuna basın.

    Tıklatın **Öğrenciler** test verileri görmek için sekmesi, `Seed` eklenen yöntemi.

    ![Öğrenci dizin sayfası](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Kurallar

Sizin için tam bir veritabanı oluşturabilmek için Entity Framework sırayla yazma gerekiyordu kod miktarını kullanımı nedeniyle en az *kuralları*, veya Entity Framework yapar varsayımlar. Bunlardan bazıları zaten Not:

- Varlık sınıfı adları pluralized biçimlerinin tablo adları olarak kullanılır.
- Varlık özellik adlarını sütun adları için kullanılır.
- Adlandırılmış varlık özellikleri `ID` veya *classname* `ID` birincil anahtar özelliği tanınır.

Kuralları geçersiz kılınabilir gördünüz (örneğin, tablo adları pluralized döndürmemelidir belirttiğiniz), ve kuralları ve bunları geçersiz kılma hakkında daha fazla bilgi edineceksiniz [daha fazla karmaşık bir veri modeli oluşturulurken](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) Öğreticisi Daha sonra bu dizide. Daha fazla bilgi için bkz: [kod ilk kuralları](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Özet

Şimdi, depolamak ve verileri görüntülemek için Entity Framework ve SQL Server Express kullanan basit bir uygulama oluşturdunuz. Aşağıdaki öğreticide, temel CRUD gerçekleştirmeyi öğreneceksiniz (Oluştur, oku, Güncelleştir, Sil) işlemleri. Bu sayfanın sonundaki geri bildirim bırakabilirsiniz. Lütfen nasıl öğreticinin bu bölümü beğendiğinizi ve biz bunu nasıl geliştirebileceğimiz bize bildirin.

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Next](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
