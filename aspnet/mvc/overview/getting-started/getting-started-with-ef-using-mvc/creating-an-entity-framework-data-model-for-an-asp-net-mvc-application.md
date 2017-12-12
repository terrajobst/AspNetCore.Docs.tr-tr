---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: "Entity Framework 6 kod MVC 5 kullanarak ilk ile çalışmaya başlama | Microsoft Docs"
author: tdykstra
description: "Bu öğretici dizisinin yeni bir sürümü: ASP.NET Core ve Entity Framework Visual Studio 2015 kullanarak çekirdek ile başlayın. Contoso Universi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/22/2015
ms.topic: article
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 84ca4bbaebe401d14233131bcaa027debf7ea0f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>Entity Framework 6 kod MVC 5 kullanarak ilk ile çalışmaya başlama
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) veya [PDF indirin](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > Bu öğretici dizisinin yeni bir sürümü: [ASP.NET Core ve Entity Framework Visual Studio 2015 kullanarak çekirdek kullanmaya başlama](https://docs.asp.net/en/latest/data/ef-mvc/intro.html).
> 
> 
> Contoso University örnek web uygulaması Entity Framework 6 ve Visual Studio 2013 kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Bu öğretici Code First iş akışı kullanır. Code First, veritabanı ilk ve Model First arasında seçim yapma hakkında daha fazla bilgi için bkz: [Entity Framework geliştirme iş akışları](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf).
> 
> Örnek uygulama, kurgusal bir Contoso üniversite için bir web sitesidir. Öğrenci giriş, indirmelere oluşturma ve Eğitmen atamaları gibi işlevselliği içerir. Bu öğretici seri Contoso University örnek uygulamasının nasıl oluşturulacağını açıklar. Yapabilecekleriniz [tamamlanmış uygulamayı karşıdan](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
> 
> Can Brind tarafından çevrilen bir Visual Basic sürümü: [MVC 5 EF 6 Visual Basic'te](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) Mikesdotnetting sitesinde.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Entity Framework 6 (EntityFramework 6.1.0 NuGet paketi)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (isteğe bağlı)
>   
> 
> Öğreticinin ayrıca ile çalışması gerekir [için Visual Studio 2013 Express Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) veya Visual Studio 2012. [Windows Azure SDK'sını VS 2012 sürümünü](https://go.microsoft.com/fwlink/p/?linkid=323511) Visual Studio 2012 ile Windows Azure dağıtımı için gereklidir.
> 
> 
> ## <a name="tutorial-versions"></a>Eğitmen sürümleri
> 
> Bu öğreticinin önceki sürümleri için bkz: [EF 4.1 / MVC 3 e-kitap](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) ve [EF MVC 4 kullanarak 5 ile çalışmaya başlama](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx), [Entity Framework ve LINQ to Entities Forumu](https://social.msdn.microsoft.com/forums/en-US/adodotnetentityframework/threads/), veya [ StackOverflow.com](http://stackoverflow.com/).
> 
> Çözümlenemiyor bir sorunla karşılaşırsanız kodunuzu indirebilirsiniz projeyi karşılaştırarak sorunun çözümü genellikle bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [yaygın hatalar ve çözümleri veya bunları için geçici çözümler.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>Contoso University Web uygulaması

Bu öğreticiler oluşturmakta uygulama basit university web sitesidir.

Kullanıcıların görüntüleyebileceği ve Öğrenci, indirmelere ve Eğitmen bilgileri güncelleştirin. Oluşturacağınız ekranlar bazılarını aşağıda verilmiştir.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Öğrenci Düzenle](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Öğretici Entity Framework çoğunlukla nasıl kullanılacağı hakkında odaklanabilmeniz bu sitenin UI Stili yerleşik şablonlar tarafından oluşturulan yakın tutulmuştur.

## <a name="prerequisites"></a>Önkoşullar

Bkz: **yazılım sürümleri** sayfanın üst kısmındaki. Entity Framework 6 öğreticinin bir parçası olarak EF NuGet paketini yüklemek için bir önkoşul değil.

## <a name="create-an-mvc-web-application"></a>Bir MVC Web uygulaması oluşturma

Visual Studio'yu açın ve "ContosoUniversity" adlı yeni bir C# Web projesi oluşturun.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

İçinde **yeni ASP.NET projesi** iletişim kutusu seç **MVC** şablonu.

Varsa **bulutta Barındır** onay kutusuna **Microsoft Azure** bölüm seçildiğinde, temizleyin.

Tıklatın **değiştirmek kimlik doğrulama**.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

İçinde **kimlik doğrulamayı Değiştir** iletişim kutusunda **doğrulaması yok**ve ardından **Tamam**. Bu öğretici için kullanıcıların oturum açmasını gerektiren veya açmış göre erişimi kısıtlama olmaz.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Geri yeni ASP.NET projesi iletişim kutusunda, tıklatın **Tamam** projesi oluşturmak için.

## <a name="set-up-the-site-style"></a>Site stil ayarlama

Birkaç basit değişiklikler sitesi menüsü, Düzen ve giriş sayfası ayarlar.

Açık *görünümler/paylaşılan\\_Layout.cshtml*ve aşağıdaki değişiklikleri yapın:

- Her oluşumu "My ASP.NET Application" ve "Uygulama adı", "Contoso University" değiştirin.
- Öğrenciler, kurslar, eğitmen ve Departmanlar için menü girişlerini ekleyin ve kişi girişi silmek.

Değişiklikler vurgulanır.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

İçinde *Views\Home\Index.cshtml*, dosyanın içeriğini bu uygulamayla ilgili metin ile ASP.NET MVC hakkında metni değiştirmek için aşağıdaki kod ile değiştirin:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Site çalıştırmak için CTRL + F5 tuşuna basın. Ana menü ile giriş sayfasına bakın.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Entity Framework 6 yükleyin

Gelen **Araçları** menüsünü tıklatın **NuGet Paket Yöneticisi** ve ardından **Paket Yöneticisi Konsolu**.

İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutu girin:

`Install-Package EntityFramework`

![EF yüklü](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Görüntü yüklenen 6.0.0 gösterir, ancak NuGet 6.1.1 öğretici için en son güncelleştirme itibariyle olduğu Entity Framework (yayın öncesi sürümleri hariç), en son sürümünü yükler.

Bu adım, Bu öğretici, el ile yapmanız gerekiyor, ancak, otomatik olarak ASP.NET MVC yapı iskelesi özelliği tarafından yapılmış birkaç adım biridir. Entity Framework kullanmak için gerekli olan adımları görebilmeniz için bunları el ile yaptığınız. MVC denetleyicisi ve görünümleri oluşturmak için yapı iskelesi daha sonra kullanacaksınız. Otomatik olarak EF NuGet paketini yükleyin, veritabanı bağlamı sınıfının oluşturmak ve bağlantı dizesi oluştur yapı iskelesi izin vermek için kullanılan bir alternatiftir. Bu şekilde yapmak hazır olduğunuzda, yapmanız gereken tek şey bu adımları atlayın ve varlık sınıflarınızı oluşturduktan sonra MVC denetleyicisi iskele.

## <a name="create-the-data-model"></a>Veri modeli oluşturma

Sonraki Contoso University uygulama için varlık sınıfları oluşturacaksınız. Aşağıdaki üç varlıklarıyla başlayacaksınız:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Arasında bir-çok ilişkisi olduğundan `Student` ve `Enrollment` varlıkları ve arasında bir-çok ilişkisi olduğundan `Course` ve `Enrollment` varlıklar. Diğer bir deyişle, öğrencinin herhangi bir sayıda kurslar kaydedilebilir ve bir indirmelere içinde kayıtlı Öğrenciler herhangi bir sayıda olabilir.

Aşağıdaki bölümlerde bu varlıkların her biri için bir sınıf oluşturacaksınız.

> [!NOTE]
> Tüm bu varlık sınıfları oluşturma bitirmeden Projeyi derlemek çalışırsanız, derleyici hataları alırsınız.


### <a name="the-student-entity"></a>Öğrenci varlık

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

İçinde *modelleri* klasörünü adlı bir sınıf dosyası oluşturma *Student.cs* ve şablon kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

`ID` Özelliği, bu sınıfa karşılık gelen veritabanı tablosunun birincil anahtar sütunu hale gelir. Varsayılan olarak, Entity Framework adlı bir özellik yorumlar `ID` veya *classname* `ID` birincil anahtar olarak.

`Enrollments` Özelliği bir *gezinti özelliği*. Gezinti özellikleri bu varlıkla ilgili diğer varlıklar tutun. Bu durumda, `Enrollments` özelliği bir `Student` varlık tüm tutun `Enrollment` için ilişkili olan varlıkların `Student` varlık. Diğer bir deyişle, varsa bir verilen `Student` satır veritabanındaki sahip iki ilgili `Enrollment` satır (Bu öğrencinin birincil anahtar içeren satırları değeri kendi `StudentID` yabancı anahtar sütunu), o `Student` varlığın `Enrollments` gezinti özelliği Bu iki içerecek `Enrollment` varlıklar.

Gezinti özellikleri olarak tanımlanmış genellikle `virtual` böylece bunlar gibi bazı Entity Framework işlevleri avantajlarından yararlanmak *yavaş Yükleniyor*. (Yavaş yükleniyor verilecektir daha sonra [ilgili veri okunurken](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) bu serideki sonraki öğretici.)

Bir gezinti özelliği (çok- veya -çok ilişkileri) olduğu gibi birden çok varlık tutarsanız türünü hangi girişleri eklenebilir, silinen ve gibi güncelleştirilmiş bir listesi olmalıdır `ICollection`.

### <a name="the-enrollment-entity"></a>Kayıt varlık

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

İçinde *modelleri* klasörü oluşturmak *Enrollment.cs* ve var olan kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`EnrollmentID` Birincil anahtar özelliği olacaktır; bu varlığı kullanan *classname* `ID` yerine desen `ID` yazarken kendi başına gördüğünüz `Student` varlık. Normalde bir desen seçin ve veri modelinizi kullanmak. Burada, değişim ya da Desen kullanabileceğiniz gösterilmektedir. Bir sonraki öğreticide göreceğiniz nasıl `ID` olmadan `classname` devralma veri modelinde uygulamak kolaylaştırır.

`Grade` Özelliği bir [enum](https://msdn.microsoft.com/en-us/data/hh859576.aspx). Soru işareti sonra `Grade` türü bildirimi gösterir `Grade` özelliği [boş değer atanabilir](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx). Null bir düzeyde bir sıfır ataması farklı. — bir sınıf bilinen değil veya henüz atanmamış null anlamına gelir.

`StudentID` Özelliği bir yabancı anahtar ve karşılık gelen gezinme özelliğini `Student`. Bir `Enrollment` varlıktır biriyle ilişkili `Student` özelliği yalnızca tek bir tutmak için varlık `Student` varlık (aksine `Student.Enrollments` gezinti özelliği gördüğünüz daha önceki sürümlerde, birden çok tutabilir `Enrollment` varlıklar).

`CourseID` Özelliği bir yabancı anahtar ve karşılık gelen gezinme özelliğini `Course`. Bir `Enrollment` varlıktır biriyle ilişkili `Course` varlık.

Entity Framework onu ise bu özellik bir yabancı anahtar özellik olarak yorumlar  *&lt;gezinti özelliği adı&gt;&lt;birincil anahtar özelliği adı&gt;*  (örneğin, `StudentID`için `Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarının `ID`). Yabancı anahtar özellikleri de adlı aynı yalnızca  *&lt;birincil anahtar özelliği adı&gt;*  (örneğin, `CourseID` beri `Course` varlığın birincil anahtarının `CourseID`).

### <a name="the-course-entity"></a>İndirmelere varlık

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

İçinde *modelleri* klasörü oluşturmak *Course.cs*, şablonu kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

`Enrollments` Özelliği bir gezinti özelliğidir. A `Course` varlık herhangi bir sayıda için ilgili olabileceğini `Enrollment` varlıklar.

Daha fazla hakkında dediğimiz [DatabaseGenerated](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx) bu serideki sonraki öğretici özniteliği. Temel olarak, bu öznitelik indirmelere yerine için oluşturmak veritabanı birincil anahtarı girmenizi sağlar.

## <a name="create-the-database-context"></a>Veritabanı bağlamı oluşturma

Verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıf *veritabanı bağlamı* sınıfı. Bu sınıf türetme tarafından oluşturduğunuz [System.Data.Entity.DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx) sınıfı. Kodunuzda hangi varlıkların veri modelinde bulunan belirtin. Ayrıca, belirli bir Entity Framework davranış özelleştirebilirsiniz. Bu projede adlı sınıfı `SchoolContext`.

ContosoUniversity projesinde bir klasör oluşturmak için ' nde projeye sağ **Çözüm Gezgini** tıklatıp **Ekle**ve ardından **yeni klasör**. Yeni bir klasör adı *DAL* (için veri erişim katmanı). Bu klasörde adlı yeni bir sınıf dosyası oluşturun *SchoolContext.cs*ve şablon kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>Belirten varlık kümeleri

Bu kod oluşturur bir [DbSet](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset(v=VS.103).aspx) özelliği her bir varlık kümesi. Entity Framework terminoloji içinde bir *varlık kümesini* genellikle bir veritabanı tablosuna karşılık gelir ve bir *varlık* tablosunda bir satırı karşılık gelir.

> [!NOTE] 
> 
> Atlanmış `DbSet<Enrollment>` ve `DbSet<Course>` deyimleri ve çalışır. Entity Framework bunları örtük olarak dahil `Student` varlık başvuruları `Enrollment` varlık ve `Enrollment` varlık başvuruları `Course` varlık.


### <a name="specifying-the-connection-string"></a>Bağlantı dizesi belirtme

(Bu, Web.config dosyasında daha sonra eklersiniz) bağlantı dizesinin adını oluşturucuya geçirilen.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Bağlantı dizesindeki kendisi Web.config dosyasında depolanan bir adı yerine geçirebilirdiniz. Kullanılacak veritabanını belirtmek için seçenekleri hakkında daha fazla bilgi için bkz: [Entity Framework - bağlantıları ve modelleri](https://msdn.microsoft.com/en-us/data/jj592674).

Bir bağlantı dizesi veya birisinin adını açıkça belirtmezseniz, Entity Framework bağlantı dizesi adı sınıf adı ile aynı olduğunu varsayar. Bu örnekte varsayılan bağlantı dizesi adı sonra olacaktır `SchoolContext`, açıkça ne belirtme ile aynı.

### <a name="specifying-singular-table-names"></a>Tekil tablo adlarını belirtme

`modelBuilder.Conventions.Remove` Deyiminde [OnModelCreating](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) yöntemi, pluralized öğesinden tablo adları önler. Bunu siz yaparsanız, veritabanında oluşturulan tabloları sayfadayken `Students`, `Courses`, ve `Enrollments`. Bunun yerine, tablo adları olacaktır `Student`, `Course`, ve `Enrollment`. Geliştiriciler olup tablo adları veya pluralized hakkında katılmıyorum. Bu öğretici tekil kullanır, ancak önemli noktadır dahil olmak üzere veya bu kod satırı atlama tercih hangi formu seçebilirsiniz.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Test verileri veritabanını başlatılamadı EF ayarlayın

Entity Framework otomatik olarak oluşturun (veya bırakma yeniden oluşturmak için uygulama çalıştığında bir veritabanı ve). Bu, uygulamanızın her çalıştığında veya model var olan veritabanı eşit olduğunda yapılmalıdır belirtebilirsiniz. Ayrıca yazabilirsiniz bir `Seed` yöntemi test verileri ile doldurmak için veritabanı oluşturduktan sonra Entity Framework otomatik olarak çağırır.

Yalnızca bu değil mevcut (ve modeli değişti ve veritabanı zaten var. bir özel durum varsa) bir veritabanı oluşturmak için varsayılan davranıştır. Bu bölümde veritabanı bırakılacak ve model değiştiğinde yeniden oluşturulması gerektiğini belirtirsiniz. Veritabanını silmek, veri kaybına neden olur. Bu genellikle Tamam geliştirme sırasında çünkü `Seed` yöntemi veritabanı yeniden oluşturulduğunda ve test verileri yeniden oluşturacak çalışacak. Ancak üretim aşamasında, genellikle veritabanı şeması değiştirmek gereken her zaman tüm verilerinizi kaybetmek istemediğiniz. Daha sonra model değişiklikleri bırakarak ve veritabanını yeniden oluşturma yerine veritabanı şeması değiştirmek için Code First Migrations kullanılarak nasıl ele alınacağını görürsünüz.

DAL klasöründe adlı yeni bir sınıf dosyası oluşturma *SchoolInitializer.cs* ve şablon kod ile değiştirin  
ne zaman oluşturulması için bir veritabanı neden olan kod aşağıdaki gerekli ve yeni veritabanına test verileri yükler.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

`Seed` Yöntemi giriş parametresi olarak veritabanı bağlamı nesnesini alır ve yöntemindeki kod kullanır  
Yeni varlıklar veritabanına eklemek için bu nesne. Yeni bir koleksiyonunu her bir varlık türü için kod oluşturur  
 varlıklar, uygun ekler `DbSet` özellik ve değişiklikler veritabanına kaydeder. Öyle olmadığı  
Çağrı için gerekli `SaveChanges` yardımcı olur ancak bunun burada gerçekleştirilir gibi varlıkları her gruptan sonra yöntemi  
kod veritabanına yazma sırasında bir özel durum oluşursa, bir sorunun kaynağını bulun.

Başlatıcı sınıfını kullanmak için Entity Framework bildirmek için bir öğe olarak ekleme `entityFramework` uygulama öğesinde *Web.config* dosya (bir kök proje klasöründeki), aşağıdaki örnekte gösterildiği gibi:

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

`context type` Tam bağlamı sınıfı adı ve içinde derleme belirtir ve `databaseinitializer type` Başlatıcı sınıfı ve içinde derleme tam olarak nitelenmiş adını belirtir. (EF Başlatıcı kullanmak istemediğinizde üzerinde bir öznitelik ayarlayabilirsiniz `context` öğesi: `disableDatabaseInitialization="true"`.) Daha fazla bilgi için bkz: [Entity Framework - Config dosya ayarlarını](https://msdn.microsoft.com/en-us/data/jj556606).

Başlatıcı ayarı alternatif olarak *Web.config* dosyasıdır yapmak için bu kodda ekleyerek bir `Database.SetInitializer` ifadesine `Application_Start` yönteminde *Global.asax.cs* dosya. Daha fazla bilgi için bkz: [anlama veritabanı başlatıcıları Entity Framework Code First içinde](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

Uygulama artık ayarlama verilen yürütülmesi ilk kez veritabanına eriştiğinizde böylece ayarlama  
Uygulama, Entity Framework karşılaştırır model veritabanına (sizin `SchoolContext` ve varlık). Bir fark ise, uygulama bırakır ve veritabanını yeniden oluşturur.

> [!NOTE]
> Bir üretim web sunucusuna bir uygulamayı dağıttığınızda, kaldırın veya devre dışı bırakır ve veritabanını yeniden oluşturur kodu. Bu serideki sonraki öğretici, bu gerçekleştirirsiniz.


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>SQL Server Express LocalDB veritabanını kullanacak şekilde EF ayarlama

[Yerel veritabanı](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) SQL Server Express veritabanı Motoru'nu hafif bir sürümüdür. Yükleme ve yapılandırma çok kolaydır, isteğe bağlı olarak başlatılır ve kullanıcı modunda çalışır. Özel yürütme modunu veritabanları ile çalışmanıza olanak tanır SQL Server Express LocalDB çalışan *.mdf* dosyaları. Yerel veritabanı veritabanı dosyalarını koyabilirsiniz *uygulama\_veri* proje veritabanıyla kopyalamak istiyorsanız bir web projesi klasörü. SQL Server Express kullanıcı örneği özelliği de ile çalışmanıza olanak tanır *.mdf* dosyaları, ancak kullanıcı örneği özelliği kullanım dışıdır; bu nedenle, yerel veritabanı ile çalışmak için önerilir *.mdf* dosyaları. Visual Studio 2012 ve sonraki sürümlerinde LocalDB Visual Studio ile varsayılan olarak yüklenir.

Genellikle SQL Server Express üretim web uygulamaları için kullanılmaz. IIS ile çalışmak üzere tasarlanmadı çünkü LocalDB bir web uygulaması ile üretim kullanımı için özellikle önerilmez.

Bu öğreticide LocalDB ile çalışması. Uygulamayı açmak *Web.config* dosya ve ekleme bir `connectionStrings` öğesi önceki `appSettings` aşağıdaki örnekte gösterildiği gibi öğesi. (Güncelleştirdiğinizden emin olun *Web.config* kök proje klasöründeki dosya. Ayrıca bir *Web.config* dosyası *görünümleri* alt güncelleştirmeniz gerekmez.)

Visual Studio 2015 kullanıyorsanız, bağlantı dizesindeki "v11.0" "MSSQLLocalDB", varsayılan SQL Server örnek adı değiştiği değiştirin.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Entity Framework adlı bir yerel veritabanı veritabanı kullanacağını eklediğiniz bağlantı dizesini belirtir *ContosoUniversity1.mdf*. (Veritabanı henüz yok; EF oluşturur.) Oluşturulması veritabanına istediyseniz, *uygulama\_veri* klasör ekleyebilirsiniz `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` bağlantı dizesi. Bağlantı dizeleri hakkında daha fazla bilgi için bkz: [ASP.NET Web uygulamaları için SQL Server bağlantı dizelerini](https://msdn.microsoft.com/en-us/library/jj653752.aspx).

Gerçekte bir bağlantı dizesi olması gerekmez *Web.config* dosya. Bir bağlantı dizesi girmezseniz, Entity Framework bir bağlam sınıfınıza dayalı bir varsayılan kullanır. Daha fazla bilgi için bkz: [yeni bir veritabanına ilk kod](https://msdn.microsoft.com/en-us/data/jj193542).

## <a name="creating-a-student-controller-and-views"></a>Bir öğrenci denetleyicisi ve görünümler oluşturma

Verileri görüntülemek için bir web sayfası artık oluşturacaksınız ve veri isteme işlemini otomatik olarak tetiklenir  
veritabanı oluşturma. Yeni bir denetleyicisi oluşturarak başlarsınız. Ancak bunu yapmadan önce modeli ve bağlam sınıfları MVC denetleyicisi iskele kullanılabilir hale getirmek için projeyi oluşturun.

1. Sağ **denetleyicileri** klasöründe **Çözüm Gezgini**seçin **Ekle**ve ardından **yeni iskele kurulmuş öğe**.
- İçinde **İskele Ekle** iletişim kutusunda **Entity Framework kullanarak MVC 5 denetleyici, görünümleri olan**.

    ![İskele Ekle](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
- Denetleyici Ekle iletişim kutusunda, aşağıdaki seçimleri yapın ve ardından **Ekle**:

    - Model sınıfı: **Öğrenci (ContosoUniversity.Models)**. (Aşağı açılan listede bu seçeneği görmüyorsanız, projeyi oluşturun ve yeniden deneyin.)
    - Veri bağlamı sınıfı: **SchoolContext (ContosoUniversity.DAL)**.
    - Denetleyici adı: **StudentController** (StudentsController değil).
    - Diğer alanları varsayılan değerleri bırakın.

    ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    Tıkladığınızda **Ekle**, iskele kurucu StudentController.cs dosya ve Denetleyici ile çalışma (.cshtml dosyaları) görünümleri kümesi oluşturur. Gelecekte Entity Framework kullanan projeleri oluşturduğunuzda, ayrıca bazı ek iskele kurucu işlevselliğini yararlanabilirsiniz: yalnızca ilk model sınıfı oluşturmak, bir bağlantı dizesi oluşturma ve ardından **denetleyiciEkle** kutusunda yeni bağlam sınıfını belirtin. İskele kurucu oluşturacak, `DbContext` sınıfı ve bağlantınızı dize denetleyici ve görünüm yanı sıra.
- Visual Studio açılır *Controllers\StudentController.cs* dosya. Bir sınıf değişkeni, bir veritabanı bağlam nesnesi başlatır oluşturuldu bakın:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    `Index` Eylem yöntemi Öğrenciler listesini alır *Öğrenciler* varlık okuyarak kümesi `Students` veritabanı bağlam örneğinin özelliği:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    *Student\Index.cshtml* görünüm bu listesi bir tabloda görüntülenir:

    [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
- Projeyi çalıştırmak için CTRL + F5 tuşuna basın. (Bir "Gölge kopya oluşturulamıyor" hata alırsanız, tarayıcıyı kapatın ve yeniden deneyin.)

    Tıklatın **Öğrenciler** test verileri görmek için sekmesi, `Seed` eklenen yöntemi. Nasıl bağlı olarak, bir tarayıcı penceresi darsa, üst adres çubuğundaki Öğrenci sekmesini bağlantı göreceksiniz veya bağlantıyı görmek için sağ üst köşesinde tıklatmanız gerekir.

    ![Menü düğmesi](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    ![Öğrenci dizin sayfası](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Veritabanı görüntüleyin

Öğrenciler sayfa çalıştırılmış ve veritabanına erişmek uygulamayı denedi, EF hiçbir veritabanı vardı ve bir oluşturulduğu şekilde veritabanını verilerle doldurmak için seed yöntemi çalıştırılmadan gördünüz.

Kullanabilirsiniz **Sunucu Gezgini** veya **SQL Server Nesne Gezgini** (Visual Studio veritabanı görüntülemek için SSOX). Bu öğretici için kullanacağınız **Sunucu Gezgini**. (Visual Studio Express 2013'ten önceki sürümlerinde **Sunucu Gezgini** çağrılır **Database Explorer**.)

1. Tarayıcıyı kapatın.
2. İçinde **Sunucu Gezgini**, genişletin **veri bağlantıları**, genişletin **Okul bağlam (ContosoUniversity)**, genişletin ve ardından **tabloları** görmek için Yeni veritabanı tablolarında.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. Sağ **Öğrenci** tablosu ve'ı tıklatın **Show Table Data** oluşturulan sütunları ve tablosuna satır görmek için.

    ![Öğrenci tablosu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. Kapat **Sunucu Gezgini** bağlantı.

*ContosoUniversity1.mdf* ve *.ldf* veritabanı dosyaları olan `C:\Users\<yourusername>` klasör.

Kullanmakta olduğunuz çünkü `DropCreateDatabaseIfModelChanges` Başlatıcı, bir değişiklik şimdi olun `Student` sınıfı, uygulamayı yeniden çalıştırın ve veritabanı otomatik olarak değişikliğinizin eşleşecek şekilde yeniden oluşturulmuş olabilir. Örneğin, eklediğiniz bir `EmailAddress` özelliğine `Student` sınıfı, Öğrenciler sayfa yeniden çalıştırın ve ardından bakma tabloyu yeniden, görürsünüz: yeni bir `EmailAddress` sütun.

## <a name="conventions"></a>Kurallar

Sizin için tam bir veritabanı oluşturabilmek için Entity Framework sırayla yazma gerekiyordu kod miktarını kullanımı nedeniyle en az *kuralları*, veya Entity Framework yapar varsayımlar. Bunlardan bazıları zaten belirtilmediği veya bunları farkında olmadan kullanılmıştır:

- Varlık sınıfı adları pluralized biçimlerinin tablo adları olarak kullanılır.
- Varlık özellik adlarını sütun adları için kullanılır.
- Adlandırılmış varlık özellikleri `ID` veya *classname* `ID` birincil anahtar özelliği tanınır.
- Bu adlı bir özelliği bir yabancı anahtar özellik olarak yorumlanır  *&lt;gezinti özelliği adı&gt;&lt;birincil anahtar özelliği adı&gt;*  (örneğin, `StudentID` için`Student` gezinti özelliği bu yana `Student` varlığın birincil anahtarının `ID`). Yabancı anahtar özellikleri de adlı aynı yalnızca &lt;birincil anahtar özelliği adı&gt; (örneğin, `EnrollmentID` beri `Enrollment` varlığın birincil anahtarının `EnrollmentID`).

Kuralları geçersiz kılınabilir gördünüz. Örneğin, tablo adları döndürmemelidir pluralized ve daha sonra göreceksiniz belirtilen açıkça özelliği yabancı anahtar özelliği olarak işaretlemek nasıl. Kuralları ve bunları geçersiz kılma hakkında daha fazla bilgi edineceksiniz [daha fazla karmaşık bir veri modeli oluşturulurken](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) bu serideki sonraki öğretici. Kuralları hakkında daha fazla bilgi için bkz: [kod ilk kuralları](https://msdn.microsoft.com/en-us/data/jj679962).

## <a name="summary"></a>Özet

Şimdi, depolamak ve verileri görüntülemek için SQL Server Express LocalDB ve Entity Framework kullanan basit bir uygulama oluşturdunuz. Aşağıdaki öğreticide, temel CRUD gerçekleştirmeyi öğreneceksiniz (Oluştur, oku, Güncelleştir, Sil) işlemleri.

Lütfen geri bildirim, Bu öğretici beğendiğinizi nasıl ve ne biz artabileceğini bırakın. En yeni konular da isteğinde bulunabilirsiniz [Göster bana nasıl kodu ile](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access - kaynakları önerilen](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Sonraki](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
