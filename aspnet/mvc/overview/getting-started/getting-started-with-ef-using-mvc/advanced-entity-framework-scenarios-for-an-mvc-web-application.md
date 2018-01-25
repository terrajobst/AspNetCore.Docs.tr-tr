---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: "Gelişmiş bir MVC 5 Web uygulaması (12 / 12) için Entity Framework 6 senaryoları | Microsoft Docs"
author: tdykstra
description: "Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 85276377671b96e65406639c8584d9ebf8d77ff7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a>MVC 5 Web uygulaması (12 / 12) için Gelişmiş Entity Framework 6 senaryoları
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) veya [PDF indirin](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio 2013 kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Önceki öğreticide tablo başına hiyerarşisi devralma uygulanmadı. Bu öğretici içeren Entity Framework Code First kullanan ASP.NET web uygulamaları geliştirme temellerini gittiğiniz zaman uyumlu olması için kullanışlı olan çeşitli konular sunar. Adım adım yönergeler, kod ve Visual Studio için aşağıdaki konulara kullanarak size yol:

- [Ham SQL sorguları gerçekleştirme](#rawsql)
- [Hayır-izleme sorgular gerçekleştirme](#notracking)
- [Veritabanına gönderilen SQL inceleniyor](#sql)

Öğretici çeşitli konular kaynaklara daha fazla bilgi için bağlantılar tarafından izlenen kısa tanıtımları ile sunar:

- [Depo ve iş desenleri birimi](#repo)
- [Proxy sınıfları](#proxies)
- [Otomatik değiştirme algılama](#changedetection)
- [Otomatik doğrulama](#validation)
- [Visual Studio için EF araçları](#tools)
- [Entity Framework kaynak kodu](#source)

Öğreticinin ayrıca aşağıdaki bölümleri içerir:

- [Özet](#summary)
- [İlgili kaynaklar](#acknowledgments)
- [VB hakkında bir Not](#vb)
- [Sık karşılaşılan hatalar ve çözümleri veya bunları için geçici çözümler](#errors)

Bu konular çoğu için daha önce oluşturduğunuz sayfaları ile çalışmak. Ham SQL güncelleştirmeleri toplu olarak kullanmak için güncelleştirme veritabanındaki tüm kursları krediler sayısı yeni bir sayfa oluşturursunuz:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a>Gerçekleştirme ham SQL sorguları

Entity Framework kod ilk API, SQL komutlarını veritabanına doğrudan geçirmenizi sağlayan yöntemler içerir. Şu seçenekleriniz vardır:

- Kullanım [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) varlık türleri döndüren sorgular için yöntem. Döndürülen nesneleri tarafından beklenen türde olmalıdır `DbSet` nesne ve otomatik olarak izlenir tarafından veritabanı bağlamı izleme kapatmak sürece. (Aşağıdaki bölüme bakın [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) yöntemi.)
- Kullanım [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) varlıkları olmayan türleri döndüren sorgular için yöntem. Varlık türlerini almak için bu yöntemi kullanmak olsa bile döndürülen verileri veritabanı bağlamı tarafından izlenen değil.
- Kullanım [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) sorgu dışı komutları için.

Entity Framework kullanmanın yararları, veri depolamanın çok yakından belirli bir yöntem kodunuzu bağlamadan önler biridir. SQL sorguları ve komutlar, ayrıca bunları kendiniz yazmak zorunda kalmaktan boşaltır oluşturarak bunu yapar. Ancak el ile oluşturduğunuz belirli SQL sorguları çalıştırmak gerektiğinde olağanüstü senaryolar vardır ve bu yöntemler, bu tür özel durumları işleme mümkün kılar.

Bir web uygulamasında SQL komutlarını yürüttüğünüzde her zaman true olarak, siteniz SQL ekleme saldırılarına karşı korumak için önlemler almanız gerekir. Bunu yapmanın bir yolu, bir web sayfası tarafından gönderilen dizeleri SQL komutlarını yorumlanan emin olmak için parametreli sorgular kullanmaktır. Bu öğreticide kullanıcı girişi bir sorgu tümleştirdiğinizde parametreli sorgular kullanacaksınız.

### <a name="calling-a-query-that-returns-entities"></a>Bir sorgu çağırma varlıklar döndürüyor

[DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx) sınıfı türünde bir varlık döndüren bir sorgu çalıştırmak için kullanabileceğiniz bir yöntem sağlar `TEntity`. Bu, nasıl çalıştığını görmek için kodda değiştireceğiz `Details` yöntemi `Department` denetleyicisi.

İçinde *DepartmentController.cs*, `Details` yöntemi Değiştir `db.Departments.FindAsync` yöntemi çağrısı ile bir `db.Departments.SqlQuery` vurgulanan aşağıdaki kodda gösterildiği gibi yöntem çağrısı:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Yeni kod düzgün çalıştığını doğrulamak için seçin **Departmanlar** sekmesini ve ardından **ayrıntıları** Departmanlar biri için.

![Departman ayrıntıları](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Bir sorgu çağırma diğer nesne türlerini döndürür

Daha önce bir öğrenci istatistikleri kılavuz Öğrenciler sayısı için her kayıt tarihi gösterdi hakkında sayfası için oluşturduğunuz. Bunu yapan kod *HomeController.cs* LINQ kullanır:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Veriyi doğrudan LINQ kullanmak yerine SQL alır kod yazmaya istediğinizi varsayalım. Varlık nesnesi dışında bir şey döndüren bir sorgu çalıştırması gerekecek yapmak için anlamına kullanmanızı gerektiren [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) yöntemi.

İçinde *HomeController.cs*, LINQ deyiminde Değiştir `About` vurgulanan aşağıdaki kodda gösterildiği gibi bir SQL deyimi yöntemiyle:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Hakkında sayfayı çalıştırın. Uygulama önceden olduğu aynı verileri görüntüler.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a>Güncelleştirme sorgusu çağırma

Contoso University yöneticiler her indirmelere için iadeleri sayısını değiştirme veritabanındaki toplu değişiklikler gerçekleştirebilecek istediğinizi varsayalım. Çok sayıda kurslar university varsa, tüm varlıklar almak ve bunları tek tek değiştirmek için verimsiz olacaktır. Bu bölümde, tüm kurslara krediler sayısını değiştirmek bir faktör belirtmesini sağlayan bir web sayfası uygulamak ve bir SQL yürüterek değişiklik `UPDATE` deyimi. Web sayfasını aşağıdaki gibi görünmelidir:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

İçinde *CourseContoller.cs*, ekleme `UpdateCourseCredits` yöntemleri için `HttpGet` ve `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Denetleyici işlediğinde bir `HttpGet` isteği, hiçbir şey döndürülür içinde `ViewBag.RowsAffected` değişkeni ve görünümü görüntüler boş bir metin kutusu ve bir gönderme düğmesi Yukarıdaki çizimde gösterildiği gibi.

Zaman **güncelleştirme** düğmesine tıklandığında, `HttpPost` yöntemi çağrılır ve `multiplier` metin kutusuna girilen değere sahip. Kodu sonra kurslar güncelleştirir ve görünümüne etkilenen satır sayısını döndürür SQL yürütür `ViewBag.RowsAffected` değişkeni. Görünümü, bir değer aldığında değişken, bunu yerine metin kutusu güncelleştirilmiş satır sayısını görüntüler ve Gönder düğmesi, aşağıdaki çizimde gösterildiği gibi:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

İçinde *CourseController.cs*, birine sağ tıklayın `UpdateCourseCredits` yöntemleri ve ardından **Görünüm Ekle**.

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

In *Views\Course\UpdateCourseCredits.cshtml*, replace the template code with the following code:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Çalıştırma `UpdateCourseCredits` seçerek yöntemi **kurslar** sonra ekleyerek sekmesinde, "/ UpdateCourseCredits" sonuna kadar tarayıcının adres çubuğundaki URL'yi (örneğin: `http://localhost:50205/Course/UpdateCourseCredits`). Metin kutusuna bir sayı girin:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Tıklatın **güncelleştirme**. Etkilenen satırların sayısını bakın:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Tıklatın **listesine dön** krediler düzenlenen sayısıyla kurslar listesini görmek için.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Ham SQL sorguları hakkında daha fazla bilgi için bkz: [ham SQL sorguları](https://msdn.microsoft.com/data/jj592907) konusuna bakın.

<a id="notracking"></a>
## <a name="no-tracking-queries"></a>Hayır-izleme sorguları

Veritabanı bağlamı tablo satırları alır ve bunları temsil eden varlık nesnesi oluşturur, varsayılan olarak veritabanında nedir ile varlıkları bellekte eşit olup olmadığını izler. Bellekteki veri önbelleği olarak davranır ve bir varlık güncelleştirdiğinizde kullanılır. Bağlam olan genellikle kısa süreli (yeni bir tane oluşturulur ve atıldı her istek için) ve bağlam örnekleri için bu önbelleğe alma genellikle bir web uygulamasında gereksizdir okuyan bir varlığın varlık yeniden kullanılmadan önce genellikle atıldı.

Kullanarak varlık nesnesi bellekte izleme devre dışı bırakabilirsiniz [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) yöntemi. Bunu yapmak isteyebilirsiniz tipik senaryolar aşağıdakileri içerir:

- Bir sorgu izleme devre dışı kapatma performansı önemli ölçüde artırmak verilerin büyük hacimli alır.
- Güncelleştirmek için bir varlık eklemek için kullanmak istediğiniz, ancak farklı bir amaç için daha önce aynı varlık alınır. Varlık tarafından veritabanı bağlamı zaten izlenmekte olduğundan, değiştirmek istediğiniz varlık eklenemiyor. Bu durumu yönetmek için tek yolu `AsNoTracking` önceki sorgu seçeneği.

Nasıl kullanılacağını gösteren bir örnek için [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) yöntemi, bkz: [Bu öğreticinin önceki sürüm](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Gerekli olmayan şekilde Bu öğreticiyi sürümü değiştirilen bayrağı Edit yöntemi bir model bağlayıcı oluşturulan varlıkta ayarlamaz `AsNoTracking`.

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a>Veritabanına gönderilen SQL inceleniyor

Bazen veritabanına gönderilen gerçek SQL sorguları görebilmek için yararlıdır. Bir önceki öğreticide dinleyiciyi kodda nasıl gördüğünüz; Şimdi, dinleyiciyi kod yazmadan yapmak için bazı yollar görürsünüz. Bu sorunu anlamak denemek için basit bir sorgu arayın ve yüklenirken, filtreleme ve sıralama böyle eager seçenekleri ekledikçe için olanlar adresindeki Ara.

İçinde *denetleyicileri/CourseController*, yerine `Index` istekli yükleme geçici olarak durdurmak için aşağıdaki kod ile yöntemi:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Şimdi üzerinde bir kesme noktası belirleyerek `return` deyimi (F9 imleci satırdaki ile). Projeyi hata ayıklama modunda çalıştırmak ve indirmelere dizin sayfası seçmek için F5 tuşuna basın. Kod kesme noktasına ulaştığında, inceleyin `sql` değişkeni. SQL Server'a gönderilen sorgu bakın. Basit bir olduğu `Select` deyimi.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Sorguda görmek için büyütme derecesini sınıfı tıklatın **metin Görselleştirici**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Şimdi böylece kullanıcılar için belirli bir bölüm filtreleyebilirsiniz kurslar dizin sayfasına açılan listesini ekleyeceksiniz. Kurslar başlığa göre sıralamak ve istekli yükleme için belirtirsiniz `Department` gezinti özelliği.

İçinde *CourseController.cs*, yerine `Index` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Kesme noktası geri `return` deyimi.

Aşağı açılan listesinde seçilen değeri yöntemi alır `SelectedDepartment` parametresi. Hiçbir şey seçili ise, bu parametre null olur.

A `SelectList` tüm bölümleri içeren bir koleksiyon için aşağı açılan liste görünümüne geçirilir. Parametreleri geçirilen `SelectList` Oluşturucusu değerini alan adını, metin alanı adı ve seçilen öğeyi belirtin.

İçin `Get` yöntemi `Course` deposu, kod belirtir bir filtre ifadesi, sıralama ve için yükleme eager `Department` gezinti özelliği. Filtre ifadesi her zaman döndürür `true` hiçbir şey aşağı açılan listesinde seçili olup olmadığını (diğer bir deyişle, `SelectedDepartment` null).

İçinde *Views\Course\Index.cshtml*, açmadan önce hemen `table` etiketi, aşağı açılan liste ve bir gönderme düğmesi oluşturmak için aşağıdaki kodu ekleyin:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Kesme noktası hala ayarlayın, indirmelere dizin sayfası çalıştırın. Böylece sayfasını tarayıcıda görüntülenen kodu bir kesme noktası isabet ilk kez devam edin. Aşağı açılan listeden bir bölüm seçin ve tıklatın **filtre**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Bu süre, aşağı açılan liste Departmanlar sorgu için ilk kesme olacaktır. Atlayın ve görüntülemek `query` değişken sonraki kod ulaştığında kesme gördükleri için `Course` gibi görünüyor artık sorgu. Aşağıdakine benzer bir şey görürsünüz:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Sorgu artık olduğunu görebilirsiniz bir `JOIN` yükler sorgu `Department` veri ile birlikte `Course` veri ve onu içeren bir `WHERE` yan tümcesi.

Kaldırma `var sql = courses.ToString()` satır.

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a>Depo ve iş desenleri birimi

Çoğu geliştiricinin depo ve iş desenleri biriminin Entity Framework ile çalışan kod çevresinde bir sarmalayıcı olarak uygulamak için kod yazma. Bu düzenleri, veri erişim katmanı ve bir uygulamanın iş mantığı katmanı arasındaki bir Soyutlama Katmanı oluşturmak üzere tasarlanmıştır. Bu desenleri uygulama veri deposunda değişiklik uygulamanızdan verenlerden yardımcı olabilir ve otomatik birim testi veya teste dayalı geliştirme (TDD) kolaylaştırabilir. Ancak, bu desenleri uygulamak için ek kod yazma her zaman en iyi seçenek EF, çeşitli nedenlerle kullanan uygulamalar için değil:

- EF bağlamı sınıfının kendisi veri deposu özel kod kodunuzdan korunmasını sağlar.
- EF bağlam sınıfını EF kullanarak bunu veritabanı için bir iş birimi sınıf güncelleştirmeleri olarak davranamaz.
- Entity Framework 6 sunulan özellikler deposu kod yazmadan TDD uygulamak kolaylaştırır.

Depo ve iş desenleri ölçü uygulama hakkında daha fazla bilgi için bkz: [Bu öğretici seri Entity Framework 5 sürümünü](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Entity Framework 6 TDD uygulamak için yolları hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Nasıl EF6 Mocking DbSets daha kolay sağlar](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Mocking framework ile test etme](https://msdn.microsoft.com/data/dn314429)
- [Kendi test çiftleri ile test etme](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a>Proxy sınıfları

Entity Framework varlık örnekleri (örneğin, bir sorgu yürütülürken), bu genellikle varlık için bir proxy görevi gören dinamik olarak üretilen bir türetilen tür örneği olarak oluşturur. Örneğin, aşağıdaki iki hata ayıklayıcı görüntü bakın. İlk görüntüde, gördüğünüz `student` değişkenidir beklenen `Student` varlık örneği hemen sonra yazın. İkinci görüntüde EF Öğrenci varlık veritabanından okumak için kullanılan sonra proxy sınıfı bakın.

![Önce Proxy sınıfı](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Sonra Proxy sınıfı](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Bu proxy sınıfı özelliği erişildiğinde eylemleri otomatik olarak gerçekleştirmek için kancaları eklenecek varlık sanal bazı özelliklerini geçersiz kılar. Bu mekanizma için kullanılan bir yavaş yükleniyor işlevdir.

Çoğu zaman bu proxy'leri kullanımını farkında olmanız gerekmez, ancak özel durum vardır:

- Bazı senaryolarda proxy örnekleri oluşturma Entity Framework engellemek isteyebilirsiniz. Örneğin, varlıkları serileştirilirken genellikle POCO sınıfları, proxy sınıfları istersiniz. Seri hale getirme sorunları önlemek için bir yoldur varlık nesnesi yerine veri aktarımı nesneleri (DTOs) serileştirmek için gösterildiği gibi [Entity Framework ile Web API kullanarak](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) Öğreticisi. Başka bir yolu [proxy oluşturmayı devre dışı bırakmak](https://msdn.microsoft.com/data/jj592886.aspx).
- Ne zaman örneği kullanarak bir varlık sınıfı `new` işleci, bir proxy örneği Al yok. Bu, yavaş yükleniyor ve otomatik değişiklik izleme gibi işlevselliği elde etmezsiniz anlamına gelir. Bu genellikle Tamam olur; veritabanında olmayan yeni bir varlık oluşturmakta olduğunuz çünkü genellikle yavaş yükleniyor, gerekli olmayan ve genellikle açıkça bir varlık olarak işaretleme varsa izleme değişiklik gerekmez `Added`. Ancak, yavaş yüklenmesi gereken ve değişiklik izleme gereksiniminiz varsa, yeni varlık örneklerini kullanarak proxy'leri oluşturabileceğiniz [oluşturma](https://msdn.microsoft.com/library/gg679504.aspx) yöntemi `DbSet` sınıfı.
- Gerçek bir varlık türü bir proxy türünden almak isteyebilirsiniz. Kullanabileceğiniz [GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) yöntemi `ObjectContext` sınıfı bir proxy türü örneğinin gerçek bir varlık türü alınamadı.

Daha fazla bilgi için bkz: [proxy'leri çalışma](https://msdn.microsoft.com/data/JJ592886.aspx) konusuna bakın.

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a>Otomatik değiştirme algılama

Entity Framework bir varlık nasıl değiştiğini (ve bu nedenle hangi veritabanına gönderilmesi gereken güncelleştirmeler) geçerli bir varlık değerleri özgün değerlerle karşılaştırarak belirler. Varlık sorgulanan ya da bağlı olduğunda özgün değerler depolanır. Otomatik değiştirme algılama neden yöntemlerin bazıları şunlardır:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Çok sayıda varlık takip ettiğiniz ve aşağıdaki yöntemlerden birini birçok kez bir döngüde çağırmanız, algılama otomatik değişiklik kullanarak geçici olarak kapatarak önemli performans geliştirmeleri alabilirsiniz [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) özelliği. Daha fazla bilgi için bkz: [değişiklikleri otomatik olarak algılama](https://msdn.microsoft.com/data/jj556205) konusuna bakın.

<a id="validation"></a>
## <a name="automatic-validation"></a>Otomatik doğrulama

Çağırdığınızda `SaveChanges` yöntemi, Entity Framework varsayılan doğrular tüm değiştirilen varlıkların tüm özellikleri verilerde veritabanını güncelleştirmeden önce. Çok sayıda varlık güncelleştirdik ve bu iş gereksiz verileri zaten doğruladıktan ve kaydetme işlemi yapabilir değişiklikler geçici olarak doğrulama devre dışı bırakarak daha az zaman olur. Bu kullanarak yapabilirsiniz [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) özelliği. Daha fazla bilgi için bkz: [doğrulama](https://msdn.microsoft.com/data/gg193959) konusuna bakın.

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a>Entity Framework güç araçları

[Entity Framework güç araçları](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d) veri modeli diyagramları oluşturmak için kullanılan Visual Studio eklentisi bu öğreticileri gösterilir. Araçlar, veritabanı Code First ile kullanabilmesi için sınıflar oluşturmak gibi varolan bir veritabanını tablolarda göre diğer işlevi da yapabilirsiniz. Araçları yüklendikten sonra bazı ek seçenekleri de bağlam menülerini görünür. Örneğin, sağ bağlam sınıfınızda **Çözüm Gezgini**, bir diyagram oluşturmak için bir seçenek alın. Code First kullanırken diyagram veri modelinde değiştiremezsiniz, ancak siz şeyler anlamak daha kolay hale getirmek için taşıyabilirsiniz.

![Bağlam menüsünde EF](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![EF diyagramı](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a>Entity Framework kaynak kodu

Entity Framework 6 için kaynak kodunu şu adresten edinilebilir [GitHub](https://github.com/aspnet/EntityFramework6). Hatalar dosya ve kendi geliştirmeler EF kaynak kodu katkıda bulunabilir.

Kaynak kodu açık olsa da, Entity Framework tam bir Microsoft ürünü desteklenir. Microsoft Entity Framework takım üzerinde Katkıları kabul edilen denetim tutar ve her sürümle kalitesini emin olmak için tüm kod değişiklikleri sınar.

<a id="summary"></a>
## <a name="summary"></a>Özet

Bu, bir ASP.NET MVC uygulamasındaki Entity Framework kullanma öğreticileri bu dizi tamamlar. Entity Framework kullanarak verilerle çalışma hakkında daha fazla bilgi için bkz: [EF belge sayfasının MSDN'de](https://msdn.microsoft.com/data/ee712907) ve [ASP.NET Data Access - kaynakları önerilen](../../../../whitepapers/aspnet-data-access-content-map.md).

Temel aldık sonra web uygulamanızı dağıtma hakkında daha fazla bilgi için bkz: [ASP.NET Web dağıtımı - kaynakları önerilen](../../../../whitepapers/aspnet-web-deployment-content-map.md) MSDN Kitaplığı'nda.

MVC için kimlik doğrulama ve yetkilendirme gibi ilgili diğer konular hakkında bilgi için bkz: [ASP.NET MVC - kaynakları önerilen](../recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>İlgili kaynaklar

- Zel Dykstra özgün sürümü bu öğreticinin yazdı EF 5 güncelleştirme birlikte yazılan ve EF 6 güncelleştirme yazıldı. Microsoft Web Platformu ve araçları içerik ekibi Kıdemli bir programlama yazıcısını zel olur.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) Eğitmen EF 5 ve MVC 4 için güncelleştirme işlerin çoğunu vermedi ve EF 6 güncelleştirme birlikte yazıldı. Rick Microsoft Azure ve MVC odaklanan için üst düzey bir programlama yazıcı ' dir.
- [Rowan Mert](http://www.romiller.com) ve diğer Entity Framework ekibi üyelerinin kod incelemeleri destekli biz EF 5 ve EF 6 öğretici güncelleştirmekte olduğunuz sırada çıkan geçişler ile birçok hatalarını ayıklamanıza yardımcı oldu.

<a id="vb"></a>
## <a name="vb"></a>VB

Öğretici, ilk olarak EF 4.1 için üretilen, biz tamamlanan yükleme proje hem C# ve VB sürümleri sağlamıştır. Zaman sınırlamaları ve diğer öncelikleri nedeniyle biz, bu sürüm için yapmadıysanız. Varsa bu öğreticileri kullanarak bir VB projeyi oluşturun ve diğer kişilerle paylaşmak için lütfen bize bildirin istekli olacaktır.

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a>Sık karşılaşılan hatalar ve çözümleri veya bunları için geçici çözümler

### <a name="cannot-createshadow-copy"></a>Oluşturma/kopyalama gölge olamaz

Hata iletisi:

> Oluşturma/kopyalama gölge olamaz '&lt;filename&gt;' ne zaman zaten mevcut.


Çözüm

Birkaç saniye bekleyin ve sayfayı yenileyin.

### <a name="update-database-not-recognized"></a>Update-Database tanınmıyor

Hata iletisi (gelen `Update-Database` PMC komutunu):

> 'Update-Database' terimi, bir cmdlet, işlev, komut dosyası veya çalıştırılabilir program adı olarak tanınmıyor. Adının yazımını denetleyin veya bir yol dahilse, yolun doğru olduğundan emin olun ve yeniden deneyin.


Çözüm

Visual Studio'dan çıkın. Projeyi yeniden açın ve yeniden deneyin.

### <a name="validation-failed"></a>Doğrulama başarısız oldu

Hata iletisi (gelen `Update-Database` PMC komutunu):

> Bir veya daha fazla varlıklar için doğrulanamadı. Daha fazla ayrıntı için 'EntityValidationErrors' özelliğine bakın.


Çözüm

Bir bu sorunun nedeni doğrulama hataları olduğunda `Seed` yöntemi çalışır. Bkz: [Seeding ve hata ayıklama Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) hata ayıklama ipuçları için `Seed` yöntemi.

### <a name="http-50019-error"></a>HTTP 500.19 hata

Hata iletisi:

> HTTP Hatası 500.19 - iç sunucu hatası  
> Sayfayla ilgili yapılandırma verileri geçersiz olduğundan istenen sayfaya erişilemiyor.


Çözüm

Bu hatayı alabilirsiniz bir çözüm, bunların aynı bağlantı noktası numarası kullanan her biri birden çok kopyasını kalmaktan yoludur. Genellikle, Visual Studio tüm örneklerini çıkma ardından üzerinde çalıştığınız projeyi yeniden başlatılması bu sorunu çözebilirsiniz. Bu işe yaramazsa, bağlantı noktası numarasını değiştirmeyi deneyin. Proje dosyasını sağ tıklatın ve ardından Özellikler'i tıklatın. Seçin **Web** sekmesini ve sonra bağlantı noktası numarasını değiştirin **proje URL'sini** metin kutusu.

### <a name="error-locating-sql-server-instance"></a>Hata bulmayla SQL Server örneği

Hata iletisi:

> SQL Server bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir değildi. Örnek adının doğru olduğundan ve SQL Server Uzak bağlantılara izin verecek şekilde yapılandırıldığından emin olun. (sağlayıcısı: SQL ağ arabirimleri, hata: 26 - Server/örnek belirtilen hata bulma)


Çözüm

Bağlantı dizesini kontrol edin. Veritabanını el ile sildiyseniz, yapı dizesinde veritabanı adını değiştirin.

>[!div class="step-by-step"]
[Önceki](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
