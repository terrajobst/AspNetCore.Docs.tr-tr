---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Entity Framework 6 Code MVC 5 kullanarak First ile çalışmaya başlama | Microsoft Docs
author: tdykstra
description: "Bu öğretici serisinin daha yeni bir sürüm: Visual Studio 2015'i kullanarak Entity Framework Core ve ASP.NET Core ile çalışmaya başlama. Contoso Universi..."
ms.author: riande
ms.date: 10/22/2015
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 29004ec2271dbf77395f07e030533e23662b67c1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756535"
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>MVC 5 Kullanarak Entity Framework 6 Code First ile Çalışmaya Başlama
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanmış projeyi indirmek](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) veya [PDF olarak indirin](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > Bu öğretici serisinin daha yeni bir sürümü kullanılabilir: [Visual Studio 2015'i kullanarak Entity Framework Core ve ASP.NET Core ile çalışmaya başlama](https://docs.asp.net/en/latest/data/ef-mvc/intro.html).
> 
> 
> Contoso University örnek web uygulaması Entity Framework 6 ve Visual Studio 2013 kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Bu öğretici, Code First iş akışını kullanır. Code First, ilk veritabanı ve Model ilk arasında seçim yapma hakkında daha fazla bilgi için bkz. [Entity Framework geliştirme iş akışlarının](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> Örnek, bir web sitesi için kurgusal Contoso üniversite uygulamasıdır. Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir. Bu öğretici serisinde Contoso University örnek uygulamanın nasıl oluşturulacağını açıklar. Yapabilecekleriniz [tamamlanmış uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
> 
> Mike Brind tarafından çevrilmiş bir Visual Basic sürüm: [MVC 5 ile Visual Basic'te EF 6](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) Mikesdotnetting sitesinde.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Entity Framework 6 (EntityFramework 6.1.0 NuGet paketi)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (isteğe bağlı)
>   
> 
> Öğretici ile da işe yaramalıdır [Visual Studio 2013 Express Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) veya Visual Studio 2012. [VS 2012 sürümü Windows Azure SDK'sının](https://go.microsoft.com/fwlink/p/?linkid=323511) Visual Studio 2012 ile Windows Azure dağıtımı için gereklidir.
> 
> 
> ## <a name="tutorial-versions"></a>Öğretici sürümleri
> 
> Bu öğreticinin önceki sürümler için bkz [EF 4.1 / MVC 3'e-kitap](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) ve [MVC 4 kullanarak EF 5 ile çalışmaya başlama](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx), [Entity Framework ve LINQ to Entities Forumu](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), veya [ StackOverflow.com](http://stackoverflow.com/).
> 
> Gideremezsiniz bir sorunla karşılaşırsanız, genellikle indirebileceğiniz tamamlanan proje kodunuzu karşılaştırarak soruna çözüm bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [sık karşılaşılan hatalar ve çözümleri veya geçici çözümleri için bunları.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>Contoso University Web uygulaması

Aşağıdaki öğreticilerde oluşturmakta uygulama basit university web sitesidir.

Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin. Oluşturacağınız ekranlar birkaçını aşağıda verilmiştir.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Öğrenciyi Düzenle](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Entity Framework ağırlıklı olarak nasıl kullanılacağı hakkında bir öğretici odaklanabilmeniz için kullanıcı Arabirimi stili bu sitenin yerleşik şablonları tarafından üretilen yakın tutulmuştur.

## <a name="prerequisites"></a>Önkoşullar

Bkz: **yazılım sürümleri** sayfanın üstünde. Entity Framework 6 öğreticinin bir parçası olarak EF NuGet paketini yüklemek için bir önkoşul değil.

## <a name="create-an-mvc-web-application"></a>Bir MVC Web uygulaması oluşturma

Visual Studio'yu açın ve "ContosoUniversity" adlı yeni bir C# Web projesi oluşturun.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

İçinde **yeni ASP.NET projesi** iletişim kutusu seç **MVC** şablonu.

Varsa **bulutta Barındır** onay kutusuna **Microsoft Azure** bölümü seçildiğinde, onay kutusunu temizleyin.

Tıklayın **kimlik doğrulamayı Değiştir**.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

İçinde **kimlik doğrulamayı Değiştir** iletişim kutusunda **kimlik doğrulaması yok**ve ardından **Tamam**. Bu öğretici için kullanıcıların oturum açmasını gerektiren veya açmış dayalı olarak erişimi kısıtlama olmaz.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Geri yeni ASP.NET projesi iletişim kutusunda, tıklayın **Tamam** projeyi oluşturmak için.

## <a name="set-up-the-site-style"></a>Site stili Ayarla

Birkaç basit değişiklikler site menü, Düzen ve giriş sayfasına ayarlar.

Açık *görünümler/paylaşılan\\_Layout.cshtml*ve aşağıdaki değişiklikleri yapın:

- "Contoso Üniversitesi" için "My ASP.NET Application" ve "Uygulama adı" her örneğini değiştirin.
- Öğrenciler, kurslara, eğitmenler ve Departmanlar için menü girişler ekleyin ve ilgili girişi silebilirsiniz.

Değişiklikler vurgulanır.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

İçinde *Views\Home\Index.cshtml*, dosyanın içeriğini bu uygulamayla ilgili metin ile ASP.NET ve MVC hakkında metnin değiştirmek için aşağıdaki kodla değiştirin:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Site çalıştırmak için CTRL + F5 tuşlarına basın. Ana menü ile giriş sayfası görürsünüz.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Entity Framework 6 yükleyin

Gelen **Araçları** menüsünü tıklatın **NuGet Paket Yöneticisi** ve ardından **Paket Yöneticisi Konsolu**.

İçinde **Paket Yöneticisi Konsolu** penceresine aşağıdaki komutu girin:

`Install-Package EntityFramework`

![EF yüklü](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

6.0.0'dan yüklenen görüntüyü gösterir, ancak NuGet 6.1.1 öğretici en son güncelleştirmesi itibarıyla olan Entity Framework (yayın öncesi sürümler hariç olmak üzere), en son sürümünü yükler.

Bu adım, Bu öğretici, el ile yapmanız var, ancak, otomatik olarak ASP.NET MVC iskele kurma özelliği tarafından yapılmış, birkaç adımda biridir. Entity Framework kullanmak için gerekli adımları görebilmeniz için bunları el ile yaptığınız. MVC denetleyicisi ve görünümleri oluşturmak için yapı iskelesi daha sonra kullanacaksınız. Otomatik olarak EF NuGet paketini yüklemek, veritabanı bağlamı sınıfının oluşturma ve bağlantı dizesini oluşturmak yapı iskelesi izin vermek için kullanılan bir alternatiftir. Bu şekilde yapmak hazır olduğunuzda, yapmanız gereken tek şey bu adımları atlayın ve varlık sınıflarınızı oluşturduktan sonra MVC denetleyicisi iskelesini.

## <a name="create-the-data-model"></a>Veri modeli oluşturma

Sonraki varlık sınıfları Contoso University uygulaması oluşturacaksınız. Aşağıdaki üç varlıklarla başlayacaksınız:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Arasında bir-çok ilişkisi `Student` ve `Enrollment` varlıkları ve bir-çok ilişkisi arasında `Course` ve `Enrollment` varlıklar. Diğer bir deyişle, bir öğrenci herhangi bir sayıda kursları kaydedilebilir ve bir kurs herhangi bir sayıda Öğrenciler içinde kayıtlı olabilir.

Aşağıdaki bölümlerde bu varlıkların her biri için bir sınıf oluşturacaksınız.

> [!NOTE]
> Tüm bu varlık sınıfları oluşturma tamamlanmadan önce projeyi derlemeyi denerseniz derleyici hataları alırsınız.


### <a name="the-student-entity"></a>Öğrenci varlık

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

İçinde *modelleri* klasöründe adlı bir sınıf dosyası oluşturma *Student.cs* ve şablon kodunu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

`ID` Özelliği, bu sınıf için karşılık gelen veritabanı tablosunun birincil anahtar sütunu olacak. Varsayılan olarak Entity Framework adlı bir özellik yorumlar `ID` veya *classname* `ID` birincil anahtar olarak.

`Enrollments` Özelliği bir *gezinti özelliği*. Gezinti özellikleri bu varlıkla ilgili diğer varlıkların tutun. Bu durumda, `Enrollments` özelliği bir `Student` varlık tüm tutun `Enrollment` olarak ilişkili varlıkları `Student` varlık. Diğer bir deyişle, varsa bir verilen `Student` satır veritabanında ilgili iki sahip `Enrollment` satırları (Bu öğrencinin birincil anahtarını içeren satır değerini kendi `StudentID` yabancı anahtar sütunu), bu `Student` varlığın `Enrollments` gezinme özelliği Bu iki içerecek `Enrollment` varlıklar.

Gezinti özellikleri olarak tanımlanmış genellikle `virtual` bunlar belirli Entity Framework işlevleri gibi yararlanabilir, böylece *yavaş Yükleniyor*. (Gecikmeli yükleme verilecektir daha sonra [ilgili verileri okuma](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) bu serideki sonraki öğretici.)

Bir gezinme özelliği (bire çok veya tek-çok ilişkilerde) olduğu gibi birden çok varlık tutarsanız, girişleri eklenebilir, silindi ve gibi güncelleştirilmiş bir listesi türü olmalıdır `ICollection`.

### <a name="the-enrollment-entity"></a>Kayıt varlık

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

İçinde *modelleri* klasör oluşturma *Enrollment.cs* ve varolan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`EnrollmentID` Birincil anahtar özelliği olacaktır; bu varlığı kullanan *classname* `ID` yerine desen `ID` gördüğünüz şekilde kendisi `Student` varlık. Genellikle bir düzen seçin ve veri modelinizi kullanılmakta. Burada, değişim ya da Desen kullanabilirsiniz gösterilmektedir. Bir sonraki öğreticide gördüğünüz kullanıldığında nasıl `ID` olmadan `classname` devralma veri modelinde uygulamak kolaylaştırır.

`Grade` Özelliği bir [enum](https://msdn.microsoft.com/data/hh859576.aspx). Sonra soru işareti `Grade` türü bildirimi gösterir `Grade` özelliği [boş değer atanabilir](https://msdn.microsoft.com/library/2cf62fcy.aspx). Boş bir sınıf bir sıfır sınıf farklıdır — bir sınıf bilinen değil veya henüz atanmamış null anlamına gelir.

`StudentID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`. Bir `Enrollment` varlıktır biriyle ilişkili `Student` özelliği yalnızca tek bir içerebileceği için varlık `Student` varlık (aksine `Student.Enrollments` gezinti özelliği gördüğünüz önceki sürümlerinde, birden çok tutabilir `Enrollment` varlıklar).

`CourseID` Özelliği olduğundan yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`. Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.

Entity Framework adlandırılmışsa, bu özellik bir yabancı anahtar özellik olarak yorumlar *&lt;gezinme özelliği adı&gt;&lt;birincil anahtar özelliği adı&gt;* (örneğin, `StudentID`için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`). Yabancı anahtar özellikleri de adı aynı yalnızca *&lt;birincil anahtar özelliği adı&gt;* (örneğin, `CourseID` beri `Course` varlığın birincil anahtarı `CourseID`).

### <a name="the-course-entity"></a>Kurs varlık

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

İçinde *modelleri* klasör oluşturma *Course.cs*, şablon kodunu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

`Enrollments` Özelliktir bir gezinme özelliği. A `Course` varlık dilediğiniz sayıda ilgili olabileceğini `Enrollment` varlıklar.

Daha fazla ilgili dediğimiz [DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx) bu serinin sonraki bir eğitimde özniteliği. Temel olarak, bu öznitelik kursu yerine için oluşturmak veritabanı birincil anahtarı girmenize olanak tanır.

## <a name="create-the-database-context"></a>Veritabanı bağlamı oluşturur

Verilen veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır *veritabanı bağlamı* sınıfı. Türeterek Bu sınıf oluşturduğunuz [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) sınıfı. Kodunuzda hangi varlıkları veri modelinde yer alan belirtin. Ayrıca, belirli bir Entity Framework davranış özelleştirebilirsiniz. Bu projede adlı sınıfı `SchoolContext`.

ContosoUniversity projeye bir klasör oluşturmak için projeye sağ **Çözüm Gezgini** tıklatıp **Ekle**ve ardından **yeni klasör**. Yeni klasör adı *DAL* (için veri erişim katmanı). Adlı yeni bir sınıf dosyası bu klasörde oluşturma *SchoolContext.cs*, şablonu kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>Varlık kümelerini belirtme

Bu kod oluşturur bir [olan DB](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) her varlık kümesi özelliği. Entity Framework terminolojisinde, bir *varlık kümesi* genellikle bir veritabanı tablosuna karşılık gelir ve bir *varlık* tablosunda bir satıra karşılık gelir.

> [!NOTE] 
> 
> Atlanmış `DbSet<Enrollment>` ve `DbSet<Course>` deyimleri ve aynı şekilde çalışır. Entity Framework bunları örtük olarak çünkü verilebilir `Student` varlık başvuruları `Enrollment` varlık ve `Enrollment` varlık başvuruları `Course` varlık.


### <a name="specifying-the-connection-string"></a>Bağlantı dizesi belirtme

(Bu, Web.config dosyasında daha sonra ekleyeceksiniz) bağlantı dizesinin adını oluşturucuya geçirilir.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Bağlantı dizesindeki kendisini Web.config dosyasında depolanan bir adı yerine geçirebiliriz. Veritabanını belirtmek için kullanılacak seçenekleri hakkında daha fazla bilgi için bkz. [Entity Framework - bağlantılar ve modelleri](https://msdn.microsoft.com/data/jj592674).

Bir bağlantı dizesi veya birisinin adını açıkça belirtmezseniz, Entity Framework bağlantı dizesi adı sınıfın adıyla aynı olduğu varsayılır. Bu örnekte varsayılan bağlantı dizesi adı ardından olacaktır `SchoolContext`, ne, açıkça belirtmekle aynı.

### <a name="specifying-singular-table-names"></a>Tekil tablo adlarını belirtme

`modelBuilder.Conventions.Remove` Deyiminde [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) yöntemi pluralized tablo adları engeller. Bunu yapmadıysanız, veritabanında oluşturulan tabloları sayfadayken `Students`, `Courses`, ve `Enrollments`. Bunun yerine, tablo adları olacaktır `Student`, `Course`, ve `Enrollment`. Geliştiriciler olup tablo adları veya pluralized hakkında katılmıyorum. Bu öğreticide tekil kullanır, ancak en önemli nokta dahil olmak üzere veya bu kod satırı atlama tercih hangi formu seçebilirsiniz.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Veritabanı test verileri ile başlatmak için EF ayarlayın

Entity Framework otomatik olarak oluşturabilir (veya drop yeniden oluşturmak için uygulama çalıştığında bir veritabanı ve). Bu, uygulama her çalıştırıldığında ya da model mevcut veritabanı ile eşitlenmemiş olduğunda yapılması gerektiğini belirtebilirsiniz. Ayrıca yazabileceğiniz bir `Seed` yöntemi test verileri ile doldurmak için veritabanını oluşturduktan sonra Entity Framework otomatik olarak çağırır.

Yalnızca olmayan mevcut (ve modeli değişti ve veritabanı zaten mevcut değilse bir özel durum halinde) bir veritabanı oluşturmak için varsayılan davranıştır. Bu bölümde veritabanı bırakılacak ve model değiştiğinde yeniden oluşturulması gerektiğini belirtirsiniz. Veritabanını silmek, tüm veri kaybına neden olur. Bu genellikle Tamam geliştirme sırasında çünkü `Seed` yöntemi çalıştırılacağı veritabanını yeniden oluşturulduğunda ve test verilerini yeniden oluşturun. Ancak, üretim ortamında genellikle veritabanı şemasını değiştirmeniz gereken her zaman tüm verilerinizi kaybetmek istemediğiniz. Daha sonra bırakarak ve veritabanını yeniden oluşturma yerine veritabanı şemasını değiştirmek için Code First Migrations'ı kullanarak model değişikliklerin üstesinden nasıl görürsünüz.

DAL klasöründe adlı yeni bir sınıf dosyası oluşturma *SchoolInitializer.cs* ve şablon kod ile değiştirin  
ne zaman oluşturulması için bir veritabanı kodlarda aşağıdaki gerekli ve test verilerini yeni veritabanına yükler.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

`Seed` Yöntemi giriş parametresi olarak veritabanı bağlam nesnesi alır ve yeni varlıklar eklemek için bu nesne yöntemindeki kodu kullanır. Her varlık türü için kodu yeni varlıklar koleksiyonu oluşturur, bunları uygun ekler `DbSet` özellik ve değişiklikleri veritabanına kaydeder. Çağrı için gerekli olmayan `SaveChanges` yöntemi her grubu varlıkların sonra olarak burada yapılır, ancak, bunu yardımcı olur, veritabanına kod yazarken bir özel durum oluşursa, bir sorunun kaynağını bulun.

Başlatıcı sınıfınıza kullanmak için Entity Framework bildirmek için bir öğeye eklediğiniz `entityFramework` uygulama öğesinde *Web.config* dosyasını (projenin kök klasöründe), aşağıdaki örnekte gösterildiği gibi:

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

`context type` Bağlam tam sınıf adı ve içinde derleme belirtir ve `databaseinitializer type` Başlatıcı sınıfı ve içinde derleme tam olarak nitelenmiş adını belirtir. (EF Başlatıcı kullanmak istemediğinizde, üzerinde bir öznitelik ayarlayabilirsiniz `context` öğesi: `disableDatabaseInitialization="true"`.) Daha fazla bilgi için [Entity Framework - Config dosyası ayarlarını](https://msdn.microsoft.com/data/jj556606).

Başlatıcı ayarı alternatif olarak *Web.config* dosyasıdır yapmak için kod ekleyerek bir `Database.SetInitializer` ifadesine `Application_Start` yönteminde *Global.asax.cs* dosya. Daha fazla bilgi için [anlama veritabanı başlatıcılar, Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

Uygulama artık ayarlı veritabanı belirli bir çalıştırma ilk kez eriştiğinizde, böylece ayarlama  
Uygulama, Entity Framework veritabanı modeline karşılaştırır (sizin `SchoolContext` ve varlık sınıfları). Bir fark varsa uygulamanın bırakır ve veritabanını yeniden oluşturur.

> [!NOTE]
> Bir üretim web sunucusu için bir uygulama dağıttığınızda, kaldırın veya devre dışı bırakır ve veritabanını yeniden oluşturan kodu. Bu, bir sonraki Öğreticide bu serideki gerçekleştirirsiniz.


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Bir SQL Server Express LocalDB veritabanını kullanacak şekilde EF ayarlama

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) SQL Server Express veritabanı Motoru'nu hafif bir sürümüdür. Yüklemek ve yapılandırmak kolay, isteğe bağlı olarak başlatılır ve kullanıcı modunda çalışır. Bir özel yürütme modu veritabanları ile çalışmanıza olanak tanır SQL Server Express LocalDB çalışan *.mdf* dosyaları. LocalDB veritabanı dosyaları koyabilirsiniz *uygulama\_veri* proje ile veritabanını kopyalamak isterseniz web projesinin klasörüne. SQL Server Express kullanıcı örneği özelliği de ile çalışmanıza olanak tanır *.mdf* dosyaları, ancak kullanıcı örneği özelliği kullanım dışıdır; bu nedenle, LocalDB ile çalışma için önerilir *.mdf* dosyaları. Visual Studio 2012 ve sonraki sürümlerinde, LocalDB Visual Studio ile varsayılan olarak yüklenir.

Genellikle SQL Server Express üretim web uygulamaları için kullanılmaz. IIS ile çalışmak üzere tasarlanmamıştır, çünkü LocalDB bir web uygulaması ile üretim kullanımı için özellikle önerilmez.

Bu öğreticide LocalDB ile çalışırsınız. Uygulamayı açmak *Web.config* dosya ve ekleme bir `connectionStrings` öğesi önceki `appSettings` öğesi, aşağıdaki örnekte gösterildiği gibi. (Güncelleştirdiğinizden emin olun *Web.config* kök proje klasöründeki dosya. Ayrıca bir *Web.config* dosyası *görünümleri* alt güncelleştirmeniz gerekmez.)

Visual Studio 2015 kullanıyorsanız, varsayılan SQL Server örneğinin adını değiştiği bağlantı dizesindeki "v11.0" ifadesini "MSSQLLocalDB ile", değiştirin.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Eklediğiniz bağlantı dizesini varlık çerçevesi adlı bir LocalDB veritabanına kullanacağını belirtir. *ContosoUniversity1.mdf*. (Veritabanı henüz yok; EF oluşturun.) Veritabanının oluşturulması istediyseniz, *uygulama\_veri* klasör ekleyebilirsiniz `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` bağlantı dizesi. Bağlantı dizeleri hakkında daha fazla bilgi için bkz. [ASP.NET Web uygulamaları için SQL Server bağlantı dizelerini](https://msdn.microsoft.com/library/jj653752.aspx).

Gerçekten de olmayan bir bağlantı dizesi gerekmez *Web.config* dosya. Bir bağlantı dizesi sağlamazsanız, Entity Framework bir bağlam sınıfınıza dayalı varsayılan kullanır. Daha fazla bilgi için [yeni veritabanına Code First](https://msdn.microsoft.com/data/jj193542).

## <a name="creating-a-student-controller-and-views"></a>Bir öğrenci denetleyicisi ve görünümler oluşturma

Artık verileri görüntülemek için bir web sayfası oluşturacak ve veri isteme işlemi otomatik olarak gerçekleştirir  
veritabanı oluşturma. Yeni bir denetleyici oluşturarak başlarsınız. Ancak bunu yapmadan önce modeli ve bağlam sınıfları MVC denetleyicisi yapı iskelesi kullanılabilir hale getirmek için projeyi derleyin.

1. Sağ **denetleyicileri** klasöründe **Çözüm Gezgini**seçin **Ekle**ve ardından **yeni iskele kurulmuş öğe**.
2. İçinde **İskele Ekle** iletişim kutusunda **MVC 5 denetleyici Entity Framework kullanarak görünümler ile**.

     ![Yapı İskelesi Ekle](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
3. Denetleyici Ekle iletişim kutusunda, aşağıdaki seçimleri yapın ve ardından **Ekle**:

   - Model sınıfı: **Öğrenci (ContosoUniversity.Models)**. (Aşağı açılan listede bu seçeneği görmüyorsanız, projeyi oluşturun ve yeniden deneyin.)
   - Veri bağlamı sınıfı: **SchoolContext (ContosoUniversity.DAL)**.
   - Denetleyici adı: **StudentController** (StudentsController değil).
   - Diğer alanları varsayılan değerleri bırakın.

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

     Tıkladığınızda **Ekle**, iskele kurucu StudentController.cs dosya ve görünümleri (.cshtml dosyaları) denetleyiciyle çalışan bir dizi oluşturur. Gelecekte, Entity Framework kullanan projeleri oluşturduğunuzda bazı ilave işlevler iskele kurucu, avantajlarından da yararlanabilirsiniz: yalnızca ilk model sınıfınızın oluşturmak, bir bağlantı dizesi oluşturma ve ardından **DenetleyiciEkle** kutusunda yeni bir bağlam sınıf belirtin. İskele kurucu oluşturacak, `DbContext` sınıfı, bağlantı dizesi denetleyici ve görünüm yanı sıra.
4. Visual Studio açılır *Controllers\StudentController.cs* dosya. Bir veritabanı bağlam nesnesi başlatan bir sınıf değişken oluşturuldu görürsünüz:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     `Index` Eylem yöntemine öğrencilerden listesini alır *Öğrenciler* varlık kümesi okuyarak `Students` veritabanı bağlam örneğinin özelliği:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     *Student\Index.cshtml* bu liste bir tabloda görüntüleyen:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Projeyi çalıştırmak için CTRL + F5 tuşlarına basın. (Bir "Gölge kopyası oluşturulamıyor" hata alırsanız, tarayıcıyı kapatın ve yeniden deneyin.)

     Tıklayın **Öğrenciler** test verilerini görmek için sekmesinde, `Seed` eklenen yöntemi. Nasıl bağlı olarak tarayıcı pencerenizin darsa, ilk adres çubuğundaki Öğrenci sekmesini bağlantı görürsünüz veya bağlantıyı görmek için sağ üst köşedeki tıklamanız gerekir.

     ![Menü düğmesi](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

     ![Öğrenci dizin sayfası](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Veritabanı görünümü

Öğrenciler sayfanın çalıştırdığınız ve uygulama veritabanına erişmeye EF veritabanı vardı ve oluşturduysanız, bu nedenle veritabanı veri ile doldurmak için seed yöntemi çalıştırılmadan gördünüz.

Kullanabilirsiniz **Sunucu Gezgini** veya **SQL Server Nesne Gezgini** (SSOX Visual Studio'daki veritabanını görüntülemek için). Bu öğretici için kullanacağınız **Sunucu Gezgini**. (Visual Studio Express 2013'den önceki sürümlerinde **Sunucu Gezgini** çağrılır **veritabanı Gezgini**.)

1. Tarayıcıyı kapatın.
2. İçinde **Sunucu Gezgini**, genişletme **veri bağlantıları**, genişletin **Okul bağlam (ContosoUniversity)** ve ardından **tabloları** görmek için Yeni veritabanınızdaki tablolar.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. Sağ **Öğrenci** tıklayın ve tablo **tablo verilerini Göster** oluşturulan sütunları ve tabloya eklenen satırları görebilirsiniz.

    ![Öğrenci tablosu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. Kapat **Sunucu Gezgini** bağlantı.

*ContosoUniversity1.mdf* ve *.ldf* veritabanı dosyalar, `C:\Users\<yourusername>` klasör.

Kullanmakta olduğunuz çünkü `DropCreateDatabaseIfModelChanges` Başlatıcı artık bir değişiklik için `Student` sınıfı, uygulamayı yeniden çalıştırın ve veritabanı otomatik olarak değişikliğiniz eşleşecek şekilde yeniden oluşturulması. Örneğin, eklediğiniz bir `EmailAddress` özelliğini `Student` sınıf, Öğrenciler sayfayı yeniden çalıştırın ve sonra Görünüm tabloya yeniden görürsünüz yeni `EmailAddress` sütun.

## <a name="conventions"></a>Kurallar

Sizin için tam bir veritabanı oluşturmak Entity Framework için sırayla yazmak için olan kod kullanımı nedeniyle en az *kuralları*, veya Entity Framework yapan varsayımlar. Bunlardan bazıları zaten belirtildiği veya bunları farkında olmadan kullanıldı:

- Varlık sınıfı adları pluralized formlar, tablo adları kullanılır.
- Varlık özellik adlarını sütun adları için kullanılır.
- Adlandırılmış varlık özellikleri `ID` veya *classname* `ID` birincil anahtar özellik olarak tanınır.
- Adlandırılmışsa, bu özellik bir yabancı anahtar özellik olarak yorumlanır *&lt;gezinme özelliği adı&gt;&lt;birincil anahtar özelliği adı&gt;* (örneğin, `StudentID` için`Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarı `ID`). Yabancı anahtar özellikleri de adı aynı yalnızca &lt;birincil anahtar özelliği adı&gt; (örneğin, `EnrollmentID` beri `Enrollment` varlığın birincil anahtarı `EnrollmentID`).

Kuralları geçersiz kılınabilir gördünüz. Örneğin, tablo adları olmamalıdır pluralized ve daha sonra göreceğiniz belirtilen nasıl açıkça bir özelliği bir yabancı anahtar özellik olarak işaretleyin. Kuralları ve bunları geçersiz kılma hakkında daha fazla bilgi edineceksiniz [daha karmaşık bir veri modeli oluşturma](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) bu serideki sonraki öğretici. Kuralları hakkında daha fazla bilgi için bkz. [kod öncelikli kurallar](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Özet

Depolamak ve verileri görüntülemek için SQL Server Express LocalDB ve Entity Framework kullanan basit bir uygulama oluşturdunuz. Aşağıdaki öğreticide temel CRUD gerçekleştirmeyi öğreneceksiniz (oluşturma, okuma, güncelleştirme ve silme) işlemleri.

Lütfen bu öğreticide sevmediğinizi nasıl ve ne geliştirebileceğimiz hakkında geri bildirim bırakın. Yeni konuları da isteyebilirsiniz [Show Me nasıl ile kod](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET veri erişimi - önerilen kaynaklar](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Next](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
