---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: "Web Forms Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 | Microsoft Docs"
author: tdykstra
description: "Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamaları oluşturmak nasıl gösteren..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: 5a852ec2301e63bde9a5ce99db80224dad7fb258
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Web Forms Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulama kurgusal bir Contoso üniversite için bir Web sitesidir. Öğrenci giriş, indirmelere oluşturma ve Eğitmen atamaları gibi işlevselliği içerir.
> 
> Öğretici örnekler C# dilinde gösterir. [İndirilebilir örnek](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) hem C# ve Visual Basic kodu içerir.
> 
> ## <a name="database-first"></a>İlk veritabanı
> 
> Entity Framework verilerle çalışma üç yolu vardır: *veritabanı ilk*, *Model First*, ve *Code First*. Bu öğretici ilk veritabanı için ' dir. Senaryonuz için en uygun olanı seçmeniz konusunda bu iş akışları ve Kılavuzu arasındaki farklar hakkında bilgi için bkz: [Entity Framework geliştirme iş akışları](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Forms
> 
> Bu öğretici seri ASP.NET Web Forms modeli kullanır ve ASP.NET Web Forms Visual Studio ile çalışmaya nasıl bildiğiniz varsayar. Görmüyorsanız [ASP.NET 4.5 Web formları ile çalışmaya başlama](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). ASP.NET MVC çerçevesi ile çalışmayı tercih ederseniz bkz [ASP.NET MVC kullanarak Entity Framework ile çalışmaya başlama](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Yazılım sürümleri
> 
> | **Öğreticide gösterilen** | **İle de çalışır** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Web için Visual Studio 2010 Express. Öğreticinin sonraki sürümlerini Visual Studio test edilmemiştir. Menü seçimleri, iletişim kutuları ve şablonları birçok farklılıklar vardır. |
> | .NET 4 | .NET 4.5, .NET 4 geriye dönük olarak uyumludur, ancak öğreticinin .NET 4.5 ile test edilmemiştir. |
> | Entity Framework 4 | Öğreticinin sonraki sürümlerini Entity Framework test edilmemiştir. Entity Framework 5 ile başlayarak, varsayılan olarak EF kullanan `DbContext API` EF 4.1 ile sunulmuştur. EntityDataSource denetimi kullanmak için tasarlanmış `ObjectContext` API. İle EntityDataSource kullanma hakkında bilgi için Denetim `DbContext` API, bkz: [bu blog gönderisine](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Sorular
> 
> Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET Entity Framework Forumu](https://forums.asp.net/1227.aspx), [Entity Framework ve LINQ to Entities Forumu](https://social.msdn.microsoft.com/forums/en-US/adodotnetentityframework/threads/), veya [ StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Genel Bakış

Bu öğreticiler oluşturmakta uygulama basit university Web sitesidir.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Kullanıcıların görüntüleyebileceği ve Öğrenci, indirmelere ve Eğitmen bilgileri güncelleştirin. Oluşturacağınız ekranlar bazılarını aşağıda verilmiştir.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Web uygulaması oluşturma

Başlangıç öğreticisi için Visual Studio'yu açın ve kullanarak yeni bir ASP.NET Web uygulaması projesi oluşturma **ASP.NET Web uygulaması** şablonu:

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Bu şablon zaten bir stil sayfası ve ana sayfalar içeren bir web uygulaması projesi oluşturur:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Açık *Site.Master* dosya ve "My ASP.NET Application" "Contoso University" için değiştirin.

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Bul *menü* adlı Denetim `NavigationMenu` ve menü öğelerini oluşturduğunuz sayfaları ekler aşağıdaki biçimlendirme ile değiştirin.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Açık *Default.aspx* sayfasında ve değiştirme `Content` adlı Denetim `BodyContent` bu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Artık, oluşturduğunuz çeşitli sayfalar için bağlantılar ile birlikte basit bir giriş sayfası vardır:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Veritabanı oluşturuluyor

Bu öğreticileri için otomatik olarak mevcut bir veritabanını temel alan veri modeli oluşturmak için Entity Framework veri modeli Tasarımcısı'nı kullanacaksınız (adlandırılırlar *veritabanı ilk* yaklaşım). Bu öğretici serisinde kapsanmayan bir veri modeli el ile oluşturmak ve ardından veritabanını oluşturmak Tasarımcı generate betikleri sağlamak için alternatiftir ( *modeli ilk* yaklaşım).

Bu öğreticide kullanılan veritabanı ilk yöntemi için sonraki adım bir veritabanı siteye eklemektir. Bu öğretici ile gider proje indirmek üzere kolay yoludur bakın. Ardından sağ *uygulama\_veri* klasöründe seçin **varolan öğeyi Ekle**seçip *School.mdf* indirilen projedeki veritabanı dosyası.

Kısmındaki yönergeleri izlemeniz alternatiftir [Okul örnek veritabanı oluşturma](https://msdn.microsoft.com/en-us/library/bb399731.aspx). Veritabanı indirin ya da oluşturmak isteyip kopyalama *School.mdf* şu klasörü dosyasına uygulamanızın *uygulama\_veri* klasörü:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Bu konumu *.mdf* dosya SQL Server 2008 Express kullandığınızı varsayar.)

Bir komut dosyasından veritabanı oluşturursanız, bir veritabanı diyagramı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. İçinde **Sunucu Gezgini**, genişletin **veri bağlantıları**, genişletin *School.mdf*, sağ **veritabanı diyagramları**, seçip**Yeni diyagrama ekleyin**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Tüm tabloları seçin ve ardından **Ekle**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server tablolar, sütunlar tablolarında ve tablolar arasındaki ilişkileri gösteren veritabanı diyagramı oluşturur. Ancak, istediğiniz yaklaşık düzenlemek için tabloları taşıyabilirsiniz.
3. Diyagramı "SchoolDiagram" olarak kaydedin ve kapatın.

Karşıdan yüklerseniz *School.mdf* Bu öğretici ile gider dosyasını çift tıklatarak veritabanı diyagramını görüntüleyebilirsiniz **SchoolDiagram** altında **veritabanı diyagramları** içinde **Sunucu Gezgini**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Diyagram şuna benzer (tablolar burada gösterilen öğesinden farklı konumlarda olabilir):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Entity Framework veri modeli oluşturma

Artık bu veritabanından bir Entity Framework veri modeli oluşturabilirsiniz. Uygulama kök klasöründe veri modeli oluşturabilirsiniz, ancak bu öğretici için onu adlı bir klasörde ekleyeceğiniz *DAL* (için veri erişim katmanı).

İçinde **Çözüm Gezgini**, adlandırılmış bir proje klasörü Ekle *DAL* (Çözüm altında proje altında olduğundan emin olun).

Sağ *DAL* klasörünü ve ardından **Ekle** ve **yeni öğe**. Altında **yüklü şablonlar**seçin **veri**seçin **ADO.NET varlık veri modeli** şablon adlandırın *SchoolModel.edmx*, ve ardından **Ekle**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Varlık veri modeli Sihirbazı başlar. İlk Sihirbazı adımda **veritabanından Oluştur** seçeneği, varsayılan olarak seçilidir. 
              **İleri**'ye tıklayın.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

İçinde **veri bağlantınızı** adım, varsayılan değerleri bırakın ve tıklayın **sonraki**. Okul veritabanı varsayılan olarak seçilidir ve bağlantı ayarı kaydedilir *Web.config* olarak dosya **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

İçinde **veritabanı nesnelerinizi** Sihirbaz adımı, tablolar hariç Tümünü Seç `sysdiagrams` (oluşturulduğu için daha önce oluşturulan diyagramı) ve ardından **son**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Model oluşturma tamamlandıktan sonra Visual Studio grafik gösterimi, veritabanı tablolarında karşılık Entity Framework nesnelerin (varlık) gösterir. (Veritabanı diyagramı olduğu gibi ile ayrı ayrı öğeler konumunu, resimde gördüklerinizden farklı olabilir. İsterseniz çizim yaklaşık eşleştirilecek öğeleri sürükleyebilirsiniz.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Entity Framework veri modeli keşfetme

Varlık diyagramı birkaç farklılıkla ile veritabanı diyagramı çok benzer görünür görebilirsiniz. Bir farktır ilişki türünü belirten simgeleri her bir ilişkilendirme sonunda eklenmesi (tablo ilişkileri çağrılır varlık ilişkilendirmeleri veri modelinde):

- Bir tane-sıfır-veya-bir ilişkisi "1" ve "0.. 1 çokluğa" gösterilir.

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    Bu durumda, bir `Person` varlık olabilir veya ile ilişkili olmayan bir `OfficeAssignment` varlık. Bir `OfficeAssignment` varlık ilişkili, ile bir `Person` varlık. Diğer bir deyişle, bir eğitmen olabilir veya bir office atanmamış olabilir ve tüm office için yalnızca bir eğitmen atanabilir.
- Bir-çok ilişkisi "1" temsil edilir ve "\*".

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    Bu durumda, bir `Person` varlık olabilir veya ilişkilendirilmemiş `StudentGrade` varlıklar. A `StudentGrade` varlık biriyle ilişkili olmalıdır `Person` varlık. `StudentGrade`Varlık, aslında bu veritabanında kayıtlı kurslar temsil eder; öğrencinin bir indirmelere kaydedilir ve hiçbir düzeyde henüz yoksa `Grade` özelliği null. Diğer bir deyişle, öğrencinin herhangi kurslar kayıtlı olmayan henüz, içinde bir indirmelere kaydedilebilir veya içinde birden çok kurslar kaydedilebilir. Kayıtlı bir seyrinde her düzeyde yalnızca bir öğrenci için geçerlidir.
- Bir çok-çok ilişkisi tarafından temsil edilen "\*"ve"\*".

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    Bu durumda, bir `Person` varlık olabilir veya ilişkilendirilmemiş `Course` varlıkları ve tersi de true: bir `Course` varlık olabilir veya ilişkilendirilmemiş `Person` varlıklar. Diğer bir deyişle, bir eğitmen birden çok kurslar öğretmek ve bir indirmelere tarafından birden çok Eğitmen öğrettin. (Bu veritabanında bu ilişki yalnızca eğitmen için geçerlidir; Öğrenciler kurslara bağlanmazsa. Öğrenciler kurslara StudentGrades tablosu tarafından bağlıdır.)

Başka bir veritabanı diyagramı ve arasındaki veri modelini ek farktır **Gezinti özellikleri** her varlık için bölüm. Bir gezinme özelliği bir varlığın ilgili varlıklar başvurur. Örneğin, `Courses` özelliğinde bir `Person` varlığı içeren bir koleksiyon tüm `Course` için ilişkili olan varlıkların `Person` varlık.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Başka bir veritabanı ve veri modeli birbirinden bildirilmesidir henüz `CourseInstructor` veritabanına bağlamak için kullanılan ilişkilendirme tablo `Person` ve `Course` bir çok-çok ilişkisi tablolarda. İlgili gezinme özelliklerini etkinleştirmeniz `Course` varlıklardan `Person` varlık ve ilgili `Person` varlıklardan `Course` varlık veri modeli ilişkilendirme tablosunda temsil gerek gelir.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Bu öğreticinin amaçları doğrultusunda varsayalım `FirstName` sütunu `Person` tablo gerçekte bir kişinin adı ve birlikte ikinci adı içerir. Bu yansıtacak şekilde alanın adını değiştirmek istediğiniz, ancak veritabanı değiştirmek veritabanı yöneticisi (DBA) istemeyebilirsiniz. Adını değiştirebilirsiniz `FirstName` veritabanını eşdeğer bırakarak sırasında veri modelindeki özelliği değiştirmeden.

Tasarımcısı'nda sağ **FirstName** içinde `Person` varlık ve ardından **yeniden adlandırma**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Yeni ad "FirstMidName" yazın. Bu kod sütununda veritabanı değiştirmeden başvuruda şeklini değiştirir.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

Modeli tarayıcısı veritabanı yapısı, veri modeli yapısını ve bunlar arasında eşleme görüntülemek için başka bir yol sağlar. Görmek için entity Designer'da boş bir alanı sağ tıklatın ve ardından **modeli tarayıcısı**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

**Modeli tarayıcısı** ağaç görünümü bölmesinde görüntülenir. ( **Modeli tarayıcısı** bölmesi yuvalanmış ile **Çözüm Gezgini** bölmesinde.) **SchoolModel** düğümü veri modeli yapısını temsil eder ve **SchoolModel.Store** düğüm veritabanı yapısını temsil eder.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Genişletme **SchoolModel.Store** tabloları görmek için genişletme **tabloları / görünümlere** tablolara bakın ve sonra genişletin **indirmelere** bir tablodaki sütun görmek için.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Genişletme **SchoolModel**, genişletin **varlık türleri**, genişletin ve ardından **indirmelere** varlıkları ve özellikleri varlıkları içinde görmek için düğüm.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

Her iki Tasarımcısı'nda veya **modeli tarayıcısı** bölmesinde Entity Framework iki model nesneleri ilişkilendirilme şekli görebilirsiniz. Sağ `Person` varlık ve select **Tablo eşleme**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Bu açılır **eşleme ayrıntıları** penceresi. Bu pencere, görmenize olanak tanır bildirimi veritabanı sütununun `FirstName` eşlenmiş `FirstMidName`, ne şekilde veri modelinde adlandırdığınız olduğu.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Entity Framework XML veritabanı, veri modeli ve bunları arasındaki eşlemeleri hakkında bilgi depolamak için kullanır. *SchoolModel.edmx* gerçekte bu bilgileri içeren bir XML dosyası bir dosyadır. Grafik biçiminde bilgi Tasarımcı oluşturur, ancak sağ tıklayarak XML olarak dosya görüntüleyebilir *.edmx* dosyasını **Çözüm Gezgini**a **birlikte Aç**, seçerek **(metin) XML Düzenleyicisi**. (Veri modeli Tasarımcısı'nı ve bir XML düzenleyicisini açma ve dosyayı aynı anda bir XML düzenleyicisinde açın Tasarımcı olamaz şekilde aynı dosyasıyla çalışma yalnızca iki farklı yöntemleridir.)

Bir Web sitesi, bir veritabanı ve veri modeli şimdi oluşturduğunuzu düşünün. Veri modeli ve ASP.NET kullanarak verileri çalışmaya başlamak sonraki kılavuzda `EntityDataSource` denetim.

>[!div class="step-by-step"]
[Sonraki](the-entity-framework-and-aspnet-getting-started-part-2.md)
