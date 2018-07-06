---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Web Forms, Entity Framework 4.0 Database First çalışmaya başlama ve ASP.NET 4 | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: b5c2ad33452cacdc1d3aca99c13b8c51e8b58b7c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810582"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Web Forms, Entity Framework 4.0 Database First çalışmaya başlama ve ASP.NET 4
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek, kurgusal Contoso üniversite için bir Web uygulamasıdır. Öğrenci giriş, kurs oluşturma ve Eğitmen atamaları gibi işlevleri içerir.
> 
> Öğreticide örnekler C# ' de gösterilmektedir. [İndirilebilir örnek](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) hem C# ve Visual Basic kodunu içerir.
> 
> ## <a name="database-first"></a>İlk veritabanı
> 
> Entity Framework verilerle çalışabilir üç yolu vardır: *veritabanı ilk*, *modeli ilk*, ve *Code First*. Bu öğretici için veritabanı ilk bölümüdür. Senaryonuz için en uygun olanı seçin konusunda bu iş akışları ve rehberlik arasındaki farklar hakkında bilgi için bkz. [Entity Framework geliştirme iş akışlarının](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Forms
> 
> Bu öğretici serisinde, ASP.NET Web Forms modeli kullanır ve Visual Studio'da ASP.NET Web Forms ile nasıl çalışılacağını bildiğiniz varsayılır. Görmüyorsanız [ASP.NET 4.5 Web Forms ile çalışmaya başlama](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). ASP.NET MVC çerçevesiyle çalışmak istiyorsanız, bkz. [ASP.NET MVC kullanarak Entity Framework ile çalışmaya başlama](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Yazılım sürümleri
> 
> | **Öğreticide gösterilen** | **İle de çalışır.** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Web için Visual Studio 2010 Express. Öğretici Visual Studio'nun daha yeni sürümlerini test edilmemiştir. Menü seçimleri, iletişim kutuları ve şablonları pek çok fark vardır. |
> | .NET 4 | .NET 4.5 .NET 4 ile geriye dönük uyumludur, ancak öğreticide .NET 4.5 ile test edilmemiştir. |
> | Entity Framework 4 | Öğreticinin sonraki sürümlerinde Entity Framework ile test edilmemiştir. Entity Framework 5 ile başlayarak, varsayılan olarak EF kullanan `DbContext API` ile EF 4.1 tanıtılmıştır. EntityDataSource denetimi kullanmak için tasarlanmış `ObjectContext` API. EntityDataSource kullanma hakkında daha fazla bilgi için denetimini `DbContext` API bakın [bu blog gönderisini](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Sorular
> 
> Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx), [Entity Framework ve LINQ to Entities Forumu](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), veya [ StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Genel Bakış

Aşağıdaki öğreticilerde oluşturmakta uygulama, bir basit university Web sitesidir.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Kullanıcılar görüntüleyebilir ve Öğrenci, kurs ve Eğitmen bilgileri güncelleştirin. Oluşturacağınız ekranlar birkaçını aşağıda gösterilmektedir.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Web uygulaması oluşturma

Başlangıç öğreticisi için Visual Studio'yu açın ve ardından kullanarak yeni bir ASP.NET Web uygulaması projesi oluşturma **ASP.NET Web uygulaması** şablonu:

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Bu şablon zaten bir stil sayfası ve ana sayfalar içeren bir web uygulaması projesi oluşturur:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Açık *Site.Master* dosya ve "My ASP.NET Application" için "Contoso Üniversitesi" değiştirin.

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Bulma *menü* adlı Denetim `NavigationMenu` oluşturmakta sayfaları için menü öğeleri ekler aşağıdaki işaretlemeyle değiştirin.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Açık *Default.aspx* sayfasında ve değiştirme `Content` adlı Denetim `BodyContent` bu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Artık, oluşturduğunuz çeşitli sayfalara bağlantılar ile basit bir giriş sayfası vardır:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Veritabanı oluşturma

Bu öğreticiler için Entity Framework veri modeli Tasarımcısı otomatik olarak varolan bir veritabanını temel alan veri modeli oluşturmak için kullanacağınız (genellikle adlı *veritabanı ilk* yaklaşım). Bu öğretici serisinde kapsanmayan bir veri modeline el ile oluşturmanız ve ardından veritabanını oluşturmak Tasarımcı oluşturma betikleri alternatiftir ( *model-first* yaklaşım).

Bu öğreticide kullanılan veritabanı ilk yöntem için sonraki adım bir veritabanı için site eklemektir. Bu öğreticiyle giden projeyi indirmek üzere en kolay yolu olan. Ardından sağ tıklayarak *uygulama\_veri* klasörüne **varolan öğeyi Ekle**seçip *School.mdf* indirilen projedeki veritabanı dosyasını.

Yönergeleri alternatiftir [School örnek veritabanını oluşturma](https://msdn.microsoft.com/library/bb399731.aspx). Veritabanı indirin veya oluşturmak isteyip kopyalama *School.mdf* uygulamanızın aşağıdaki klasöründeki dosyaya *uygulama\_veri* klasörü:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Bu konuma *.mdf* dosya SQL Server 2008 Express kullandığınız varsayılır.)

Bir komut dosyasından veritabanı oluşturursanız, bir veritabanı diyagramı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. İçinde **Sunucu Gezgini**, genişletme **veri bağlantıları**, genişletin *School.mdf*, sağ **veritabanı diyagramları**, seçip**Yeni Diyagram Ekle**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Tüm tabloları seçin ve ardından **Ekle**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server tabloları, sütunları tablolarda ve tablolar arasındaki ilişkileri gösteren bir veritabanı diyagramı oluşturur. Yaklaşık istediğiniz ancak bunları düzenlemek için tabloları taşıyabilirsiniz.
3. Diyagram "SchoolDiagram" olarak kaydedin ve kapatın.

Karşıdan yüklerseniz *School.mdf* bu öğreticiyle giden dosyasını çift tıklatarak veritabanı diyagramını görüntüleyebilirsiniz **SchoolDiagram** altında **veritabanı diyagramları** içinde **Sunucu Gezgini**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Diyagram aşağıdakine benzer (tabloları burada gösterilen öğesinden farklı konumlarda olabilir):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Entity Framework veri modeli oluşturma

Artık bu veritabanından bir Entity Framework veri modeli oluşturabilirsiniz. Uygulama kök klasöründe bulunan veri modeli oluşturabilirsiniz, ancak bu öğretici için bu adlı bir klasörde yer *DAL* (için veri erişim katmanı).

İçinde **Çözüm Gezgini**, adlı bir proje klasörü Ekle *DAL* (Çözüm altında projesi altında olduğundan emin olun).

Sağ *DAL* klasörünü ve ardından **Ekle** ve **yeni öğe**. Altında **yüklü şablonlar**seçin **veri**seçin **ADO.NET varlık veri modeli** şablon adlandırın *SchoolModel.edmx*, ve ardından **Ekle**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Bu varlık veri modeli Sihirbazı başlatır. Sihirbazı ilk adımda **veritabanından Oluştur** seçeneği, varsayılan olarak seçilidir. **İleri**'ye tıklayın.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

İçinde **veri bağlantınızı seçin** adım, varsayılan değerleri değiştirmeyin ve tıklayın **sonraki**. School veritabanını varsayılan olarak seçilidir ve bu bağlantı ayarı kaydedilir *Web.config* Dosyala **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

İçinde **veritabanı nesnelerinizi seçin** Sihirbazı adımı, tablolar hariç Tümünü Seç `sysdiagrams` (daha önce oluşturulan diyagram için oluşturulan) ve ardından **son**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Modeli oluşturma tamamlandıktan sonra Visual Studio grafik gösterimi, veritabanı tablolarında karşılık gelen Entity Framework nesneleri (varlıklar) gösterir. (Veritabanı diyagramıyla olduğu gibi tek tek öğelerine konumunu, resimde gördüklerinizden farklı olabilir. İsterseniz çizim yaklaşık eşleştirilecek öğeleri sürükleyebilirsiniz.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Entity Framework veri modeli ile keşfetme

Varlık diyagramda birkaç farklılıkla ile veritabanı diyagramını çok benzer görünür olduğunu görebilirsiniz. Bir farktır ilişki türünü belirten simgeleri her bir ilişkilendirme sonunda eklenmesi (tablo ilişkileri varlık ilişkileri veri modelinde adı):

- Bir sıfır-veya-bir ilişkilendirme "1" ve "0..1" tarafından temsil edilir.

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    Bu durumda, bir `Person` varlık olabilir veya ile ilişkili olmayan bir `OfficeAssignment` varlık. Bir `OfficeAssignment` varlık ilişkilendirilmelidir bir `Person` varlık. Diğer bir deyişle, bir eğitmen olabilir veya bir office atanamaz ve office için yalnızca bir eğitmen atanabilir.
- Bir-çok ilişkisi "1" tarafından temsil edilir ve "\*".

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    Bu durumda, bir `Person` varlık ilişkilendirilmemiş veya olabilir `StudentGrade` varlıklar. A `StudentGrade` varlık biriyle ilişkili olmalıdır `Person` varlık. `StudentGrade` Varlık, aslında bu veritabanında kayıtlı kursları temsil eder; bir öğrenci kurs kaydedilir ve hiçbir sınıf henüz yoksa `Grade` özelliği null. Diğer bir deyişle, bir öğrenci kurslar kayıtlı olmayan henüz bir kurs kaydedilebilir veya birden fazla eğitim kaydedilebilir. Kayıtlı bir kursta her sınıf için yalnızca bir öğrenci geçerlidir.
- Çoktan çoğa ilişki tarafından temsil edilen "\*"ve"\*".

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    Bu durumda, bir `Person` varlık ilişkilendirilmemiş veya olabilir `Course` varlıkları ve tersi de true ise: bir `Course` varlık ilişkilendirilmemiş veya olabilir `Person` varlıklar. Diğer bir deyişle, bir eğitmen birden çok kursları öğretin ve bir kurs birden çok Eğitmenler tarafından verilen. (Bu veritabanında bu ilişki sadece eğitmen için geçerlidir; Öğrenciler kurslara bağlanmazsa. Öğrenciler kurslara StudentGrades tablo tarafından bağlıdır.)

Başka bir veri modeli ile veritabanı diyagramını arasındaki ek farktır **Gezinti özellikleri** her varlık için bölüm. Bir varlığın bir gezinti özelliği, ilgili varlıkları başvuruyor. Örneğin, `Courses` özelliğinde bir `Person` varlığı içeren bir koleksiyon tüm `Course` olarak ilişkili varlıkları `Person` varlık.

[![image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Başka bir veritabanı ve veri modeli birbirinden bildirilmesidir henüz `CourseInstructor` veritabanına bağlanmak için kullanılan ilişkilendirme tablo `Person` ve `Course` tablolarında bir çoktan çoğa ilişki. Gezinme özelliklerini ilgili olanak `Course` varlıklardan `Person` varlık ve ilgili `Person` varlıklardan `Course` veri modelindeki ilişkilendirme tablosunu temsil gerek olduğundan varlık.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Bu öğreticinin amaçları doğrultusunda, varsayalım `FirstName` sütununun `Person` tablo gerçekten bir kişinin adı ve birlikte ikinci adı içerir. Bunu yansıtmak için alanın adını değiştirmek istediğiniz, ancak veritabanı değiştirmek veritabanı yöneticisi (DBA) istemeyebilirsiniz. Adını değiştirebilirsiniz `FirstName` veritabanını eşdeğer bırakarak sırasında veri modeli, özellik açısından bir farklılık göstermez.

Tasarımcıda sağ **FirstName** içinde `Person` varlık tıklayın ve ardından **Yeniden Adlandır**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Yeni ad "FirstMidName" yazın. Bu kod sütununda veritabanı değiştirmeden başvurmanız şeklini değiştirir.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

Model tarayıcı veritabanı yapısı, veri modeli yapısını ve bunlar arasında eşleme görüntülemek için başka bir yol sağlar. Bunu görmek için varlık Tasarımcısı'nda boş bir alana sağ tıklayın ve ardından **Model tarayıcı**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

**Model tarayıcı** ağaç görünümü bölmesinde görüntülenir. ( **Model tarayıcı** bölmesi yuvalanmış ile **Çözüm Gezgini** bölmesinde.) **SchoolModel** düğümü veri modeli yapısını temsil eder ve **SchoolModel.Store** düğüm veritabanı yapısını temsil eder.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Genişletin **SchoolModel.Store** tabloları görmek için genişletin **tablolar / görünümler** tablolara bakın ve ardından **kurs** tablodaki sütunları görmek için.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Genişletin **SchoolModel**, genişletme **varlık türleri**ve ardından **kurs** varlıklar ve varlıkların içinde özelliklerini görmek için düğümü.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

Her iki Tasarımcısı'nda veya **Model tarayıcı** bölmesinde Entity Framework iki model nesnelerin nasıl ilişkili olduğunu görebilirsiniz. Sağ `Person` varlık ve select **Tablo eşleme**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Bu açılır **eşleşme ayrıntıları** penceresi. Bu pencere, görmenize olanak tanır dikkat edin veritabanı sütununun `FirstName` eşlenmiş `FirstMidName`, ne şekilde veri modelinde adlandırdığınız olduğu.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Entity Framework, XML veritabanı, veri modeli ve bunlar arasındaki eşlemeler hakkındaki bilgileri depolamak için kullanır. *SchoolModel.edmx* aslında bu bilgileri içeren bir XML dosyası dosyasıdır. Tasarımcı grafik biçiminde bilgileri oluşturur, ancak sağ tıklayarak dosyayı XML olarak da görüntüleyebilirsiniz *.edmx* dosyası **Çözüm Gezgini**a **birlikte Aç**, seçerek **XML (metin) Düzenleyicisi**. (Veri modeli Tasarımcısı ve bir XML Düzenleyicisi'ni açarak ve dosyayı aynı anda bir XML düzenleyicisinde açın. Tasarımcı olamaz. Bu nedenle aynı dosya ile çalışma yalnızca iki farklı yöntemlerdir.)

Bir veri modeli bir Web sitesi ve bir veritabanı oluşturdunuz. Sonraki izlenecek yolda veri modelini ve ASP.NET'i kullanarak verilerle çalışmaya başlarsınız `EntityDataSource` denetimi.

> [!div class="step-by-step"]
> [Next](the-entity-framework-and-aspnet-getting-started-part-2.md)
