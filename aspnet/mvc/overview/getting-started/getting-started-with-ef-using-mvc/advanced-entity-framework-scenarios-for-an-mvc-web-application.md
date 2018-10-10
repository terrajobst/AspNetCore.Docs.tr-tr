---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Bir MVC 5 Web uygulaması (12, 12) için Entity Framework 6 senaryoları Gelişmiş | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: riande
ms.date: 12/08/2014
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 0aa440e700c9bfb02aa5d55ebf481850a730febe
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912689"
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a>MVC 5 Web uygulaması (12, 12) için Gelişmiş Entity Framework 6 senaryoları
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Projeyi yükle](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Önceki öğreticide tablo başına hiyerarşi devralma uygulanır. Bu öğretici içerir, Entity Framework Code First kullanan ASP.NET web uygulamaları geliştirmenin temellerini gidin ne zaman uyumlu olması yararlı olan çeşitli konuları tanıtır. Adım adım yönergeler kod ve Visual Studio için aşağıdaki konulardan kullanarak rehberlik:

- [Ham SQL sorguları gerçekleştirme](#rawsql)
- [Hayır-izleme sorguları gerçekleştirme](#notracking)
- [Veritabanına gönderilen SQL İnceleme](#sql)

Öğreticinin daha fazla bilgi için kaynaklara bağlantılar tarafından izlenen kısa tanıtımları ile çeşitli konular sunar:

- [Depo ve iş birimi desenleri](#repo)
- [Proxy sınıfları](#proxies)
- [Otomatik değiştirme algılama](#changedetection)
- [Otomatik doğrulama](#validation)
- [Visual Studio için EF araçları](#tools)
- [Entity Framework kaynak kodu](#source)

Öğreticinin ayrıca aşağıdaki bölümleri içerir:

- [Özet](#summary)
- [İlgili kaynaklar](#acknowledgments)
- [VB hakkında bir Not](#vb)
- [Sık karşılaşılan hatalar ve çözümleri veya geçici çözümleri için bunları](#errors)

Bu konular çoğu için önceden oluşturulmuş sayfaları ile çalışırsınız. Ham SQL güncelleştirmeleri toplu olarak kullanmak için güncelleştirme veritabanındaki tüm kursları kredisi sayısı yeni bir sayfa oluşturacaksınız:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a>Ham SQL sorguları gerçekleştirme

Entity Framework kod ilk API SQL komutları doğrudan veritabanına geçirilecek sağlayan yöntemler içerir. Şu seçenekleriniz vardır:

- Kullanım [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) varlık türleri döndüren sorgular için yöntemi. Döndürülen nesneleri tarafından beklenen türde olmalıdır `DbSet` nesne ve otomatik olarak izlenir tarafından veritabanı bağlamı izlemeyi devre dışı sürece. (Aşağıdaki bölüme bakın [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) yöntemi.)
- Kullanım [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) varlıkları olmayan türleri döndüren sorgular için yöntemi. Varlık türleri almak için bu yöntem kullansanız bile döndürülen verileri veritabanı bağlamı tarafından izlenen değil.
- Kullanım [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) sorgu dışı komutları için.

Entity Framework kullanmanın avantajlarını önler, veri depolamanın çok yakından belirli bir yöntem, kodunuzu bağlamadan biridir. Bunu SQL sorgulara ve komutlara sizin için Ayrıca bunları kendiniz yazmak zorunda kalmanızı oluşturarak yapar. Ancak, el ile oluşturduğunuz belirli SQL sorguları çalıştırmak ihtiyacınız olduğunda olağanüstü senaryolar vardır ve bu yöntemler tür özel durumları işlemek için mümkün kılar.

Bir web uygulamasında SQL komutları yürütme her zaman true olduğu gibi sitenizi SQL ekleme saldırılarına karşı korumak için önlemler almanız gerekir. Bunu yapmanın bir yolu, bir web sayfası tarafından gönderilen dizeleri SQL komutları yorumlanamıyor emin olmak için parametreli sorgular kullanmaktır. Bu öğreticide, kullanıcı girişi bir sorguya tümleştirdiğinizde parametreli sorgular kullanacaksınız.

### <a name="calling-a-query-that-returns-entities"></a>Çağıran bir sorgu varlıklar döndürüyor

[Olan DB&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx) sınıf türünde bir varlık döndüren bir sorgu yürütmek için kullanabileceğiniz bir yöntem sağlar `TEntity`. Bu, nasıl çalıştığını görmek için kodda değiştireceksiniz `Details` yöntemi `Department` denetleyicisi.

İçinde *DepartmentController.cs*, `Details` yöntemi Değiştir `db.Departments.FindAsync` metot çağrısı ile bir `db.Departments.SqlQuery` yöntemi çağrısı, vurgulanan aşağıdaki kodda gösterildiği gibi:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Yeni kod düzgün çalıştığını doğrulamak için **Departmanlar** sekmesini ve ardından **ayrıntıları** Departmanlar biri için.

![Departman ayrıntıları](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Çağıran bir sorgu diğer nesne türlerini döndürür

Daha önce bir öğrenci istatistikleri kılavuz Öğrenci sayısı için her bir kayıt tarihi gösterdi hakkında sayfası için oluşturuldu. Bunu yapar kodu *HomeController.cs* LINQ kullanır:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Bu verileri doğrudan LINQ kullanmak yerine SQL alır kod yazmak istediğiniz varsayalım. Varlık nesnesi dışında bir şey döndüren bir sorgu çalıştırmak ihtiyacınız olan şeyi anlamına gelir, gerek kullanılacak [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) yöntemi.

İçinde *HomeController.cs*, LINQ deyiminde değiştirin `About` yöntemine aşağıdaki vurgulanmış kodu gösterildiği gibi bir SQL deyimi ile:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Hakkında sayfayı çalıştırın. Bu, daha önceki işlevlerini sürdürmektedir aynı verileri görüntüler.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a>Update sorgusu çağırma

Her kurs sonunda verilen kredi sayısı değiştirme gibi bu veritabanındaki toplu değişiklikleri gerçekleştirmek contoso University Yöneticiler istediğinizi varsayalım. University dersleri çok sayıda varsa, bunları tüm varlıklar olarak almak ve bunları tek tek değiştirmek için verimsiz olabilir. Bu bölümde, tüm kursları için kredi sayısını değiştirmek bir faktör belirtmesini sağlayan bir web sayfası uygulayacaksınız ve bir SQL yürüterek değişiklik yapacaksınız `UPDATE` deyimi. Web sayfasını aşağıdaki gibi görünür:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

İçinde *CourseContoller.cs*, ekleme `UpdateCourseCredits` yöntemleri `HttpGet` ve `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Denetleyici işlediğinde bir `HttpGet` isteği, hiçbir şey döndürülür içinde `ViewBag.RowsAffected` değişkeni ve görünüm görüntüler boş bir metin kutusu ve bir Gönder düğmesi önceki resimde gösterildiği gibi.

Zaman **güncelleştirme** düğmesine tıklandığında, `HttpPost` yöntemi çağrıldığında ve `multiplier` metin kutusuna girilen değere sahip. Kodu sonra dersleri güncelleştirir ve görünümüne etkilenen satır sayısını döndüren SQL yürütür `ViewBag.RowsAffected` değişkeni. Görünümü, bir değer aldığında değişken, metin kutuları yerine güncelleştirilen satır sayısını görüntüler ve Gönder düğmesi, aşağıdaki çizimde gösterildiği gibi:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

İçinde *CourseController.cs*, birine sağ tıklayın `UpdateCourseCredits` yöntemleri ve ardından **Görünüm Ekle**.

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

İçinde *Views\Course\UpdateCourseCredits.cshtml*, şablonu kodu aşağıdaki kodla değiştirin:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Çalıştırma `UpdateCourseCredits` yöntemi seçerek **kursları** sekmesini, ardından ekleme "/ UpdateCourseCredits" sonuna kadar tarayıcının adres çubuğuna URL'yi (örneğin: `http://localhost:50205/Course/UpdateCourseCredits`). Metin kutusuna bir sayı girin:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Tıklayın **güncelleştirme**. Etkilenen satır sayısını görürsünüz:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Tıklayın **listesine geri** kredi düzeltilmiş sayısı kurslarıyla listesini görmek için.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Ham SQL sorguları hakkında daha fazla bilgi için bkz. [ham SQL sorguları](https://msdn.microsoft.com/data/jj592907) MSDN'de.

<a id="notracking"></a>
## <a name="no-tracking-queries"></a>Hayır-izleme sorguları

Bir veritabanı bağlamını tablo satırları alır ve bunları temsil eden varlık nesnesi oluşturur, varsayılan olarak varlıkları bellekte veritabanında nedir ile eşitlenmiş durumda olup olmadığını izler. Bellekteki verileri bir önbellek olarak görev yapar ve bir varlık güncelleştirdiğinizde kullanılır. Bağlam olan genellikle kısa süreli (yeni bir tane oluşturulur ve elden her istek için) ve bağlam örnekleri için bu önbelleğe alma genellikle bir web uygulamasında gereksizdir okuyan bir varlık, varlığın yeniden kullanılmadan önce genellikle atıldı.

Kullanarak varlık nesnesi bellekte izleme devre dışı bırakabilirsiniz [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) yöntemi. Bunu yapmak isteyebilirsiniz tipik senaryolar aşağıdakileri içerir:

- Bir sorgu, izlemeyi açma performansı fark edilir derecede geliştiren veri büyük birim alır.
- Bir varlığı güncelleştirmek için eklemek istediğiniz, ancak farklı bir amaç için aynı varlık daha önce aldığınız. Varlık tarafından veritabanı bağlamı zaten izlenmekte olduğundan, değiştirmek istediğiniz varlığın eklenemiyor. Bu durumu yönetmek için tek bir yolu `AsNoTracking` önceki sorgu seçeneği.

Nasıl kullanılacağını gösteren bir örnek [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) yöntemi bkz [Bu öğreticinin önceki sürümüne](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Gerekli olmayan şekilde Bu öğreticiyi sürümü değiştirildi bayrağı düzenleme yöntemi, bir model bağlayıcı oluşturulan varlıkta ayarlayıp ayarlamadığını `AsNoTracking`.

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a>Veritabanına gönderilen SQL İnceleme

Bazen veritabanına gönderilen gerçek SQL sorguları görebilmeniz yararlıdır. Önceki bir öğreticide dinleyiciyi kodda bunu nasıl yapacağınız gördünüz; Şimdi, dinleyiciyi kod yazmaya gerek kalmadan yapmak için bazı yollar görürsünüz. Bunu denemek için basit bir sorgu arayın ve seçenekleri yükleniyor, filtreleme ve sıralama gibi eager ekledikçe ne olur ardından aramak.

İçinde *denetleyicileri/CourseController*, değiştirin `Index` istekli yükleme geçici olarak durdurmak için şu kod ile yöntemi:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Artık üzerinde bir kesme noktası ayarlamak `return` deyimi (imleç o satırın F9). Tuşuna **F5** projeyi hata ayıklama modunda çalıştırın ve kursu Dizin Sayfası'ı seçin. Kodu kesme noktasına ulaşıldığında, inceleyin `sql` değişkeni. SQL sunucusuna gönderilen sorgu görürsünüz. Basit bir `Select` deyimi.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Sorguda görmek için büyütece tıklayın **metin görselleştiricisi**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Artık kullanıcılar için belirli bir departmandaki filtre uygulayabilirsiniz böylece kursları dizin sayfasına açılır listede ekleyeceksiniz. Kursları başlığa göre sıralamak ve istekli yükleme için belirtirsiniz `Department` gezinme özelliği.

İçinde *CourseController.cs*, değiştirin `Index` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Kesme noktasına geri `return` deyimi.

Yöntem açılan listede seçili değerini alır `SelectedDepartment` parametresi. Hiçbir şey seçili değilse bu parametre null olacaktır.

A `SelectList` tüm bölümleri içeren bir koleksiyon için aşağı açılan liste görünümüne geçirilir. Geçirilen parametreleri `SelectList` oluşturucusu, seçili öğe değeri alan adı ve metin alan adı belirtin.

İçin `Get` yöntemi `Course` depo kodu belirtir bir filtre ifadesi bir sıralama düzeni ve için yükleme eager `Department` gezinme özelliği. Filtre ifadesi her zaman döndürür `true` hiçbir şey aşağı açılan listede seçili olup olmadığını (diğer bir deyişle, `SelectedDepartment` null).

İçinde *Views\Course\Index.cshtml*, açmadan önce hemen `table` etiketinde, aşağı açılan liste ve bir Gönder düğmesi oluşturmak için aşağıdaki kodu ekleyin:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Kesme noktası ile hala ayarlayın, kurs dizin sayfası çalıştırın. Tarayıcıda görüntülenen sayfa, kodu bir kesme noktası isabet ilk kez ile devam edin. Aşağı açılan listeden bir bölüm seçin ve tıklayın **filtre**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Bu süre, aşağı açılan liste için bölümler sorgu için ilk kesme noktasına olacaktır. Atlama ve görüntüleme `query` değişken sonraki kodu ulaştığında kesme noktası gördükleri için `Course` gibi görünüyor artık sorgu. Aşağıdaki gibi bir şey görürsünüz:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Sorgu şimdi olduğunu görebilirsiniz bir `JOIN` yükler sorgu `Department` ile birlikte veri `Course` veri ve onu içeren bir `WHERE` yan tümcesi.

Kaldırma `var sql = courses.ToString()` satır.

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a>Depo ve iş birimi desenleri

Birçok geliştiricinin depo ve iş birimi desenleri, Entity Framework ile çalışan kod çevresinde sarmalayıcı olarak uygulamak için kod yazın. Bu desenleri, veri erişim katmanı ve bir uygulamanın iş mantığı katmanı arasında bir Soyutlama Katmanı oluşturmak için tasarlanmıştır. Bu desenleri uygulama veri deposundaki değişiklikleri uygulamanızdan verenlerden yardımcı olabilir ve otomatik birim testi veya test odaklı geliştirme (TDD) kolaylaştırabilir. Ancak, bu desenleri uygulamak için ek kod yazmaya her zaman EF, çeşitli nedenlerle kullanan uygulamalar için en iyi seçenek değildir:

- EF bağlam sınıfını kendi veri deposu özel kod kodunuzdan korunmasını sağlar.
- EF bağlam sınıfını kullanarak EF bunu veritabanı için bir iş birimi sınıfı güncelleştirmeleri olarak hareket eder.
- Entity Framework 6'da sunulan özellikleri depo kod yazmaya gerek kalmadan TDD uygulamak kolaylaştırır.

Depo ve iş birimi desenleri uygulama hakkında daha fazla bilgi için bkz. [Entity Framework 5 sürümü Bu öğretici serisinin](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Entity Framework 6'da TDD uygulamak için yollar hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Sahte işlem DbSets EF6'nasıl daha kolay sağlar](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Sahte bir framework ile test etme](https://msdn.microsoft.com/data/dn314429)
- [Kendi test Double ile test etme](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a>Proxy sınıfları

Entity Framework varlık örnekleri (örneğin, bir sorgu yürütme), genellikle olarak varlık için bir proxy görevi gören dinamik olarak üretilen bir türetilmiş türün örneklerini oluşturur. Örneğin, aşağıdaki iki hata ayıklayıcı görüntülere bakın. İlk görüntüde gördüğünüz `student` değişkendir beklenen `Student` varlık örneği hemen sonra yazın. İkinci görüntüde sonra EF Öğrenci varlık veritabanından okumak için kullanılan proxy sınıfı görürsünüz.

![Önce Proxy sınıfı](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Sonra Proxy sınıfı](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Bu proxy sınıfı özelliği erişildiğinde eylemlerini otomatik olarak gerçekleştirmek için kancaları eklenecek varlık sanal bazı özelliklerini geçersiz kılar. Bu mekanizma için kullanılan bir işlev yavaş yükleniyor.

Çoğu zaman bu proxy'ler kullanımına dikkat etmeniz gerekmez, ancak özel durum vardır:

- Bazı senaryolarda Entity Framework proxy örnekleri oluşturmasını isteyebilirsiniz. Örneğin, varlıkları serileştirilirken genellikle POCO sınıfları olmayan proxy'si sınıfları istersiniz. Serileştirme sorunları önlemek için bir yoludur veri aktarımı nesneleri (Dto) yerine varlık nesneleri serileştirmek için gösterildiği [Entity Framework ile Web API kullanarak](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) öğretici. Başka bir yolu [proxy oluşturma devre dışı](https://msdn.microsoft.com/data/jj592886.aspx).
- Ne zaman örneği kullanarak bir varlık sınıfı `new` işleci, bir proxy örneği Al yok. Başka bir deyişle, yavaş yükleniyor ve otomatik değişiklik izleme gibi işlevselliği elde etmezsiniz. Bu genellikle Tamam, veritabanında olmayan yeni bir varlık oluşturduğumuzdan, genellikle yavaş yükleniyor, ihtiyacınız olmayan ve genellikle açıkça bir varlık olarak işaretleme durumunda değişiklik izleme gereksiniminiz yoksa `Added`. Ancak, yavaş yüklenmesi gerekir ve değişiklik izleme ihtiyacınız varsa, yeni varlık örneklerini kullanarak proxy'leriyle oluşturabileceğiniz [Oluştur](https://msdn.microsoft.com/library/gg679504.aspx) yöntemi `DbSet` sınıfı.
- Proxy türünden bir gerçek varlık türünü almak isteyebilirsiniz. Kullanabileceğiniz [GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) yöntemi `ObjectContext` bir proxy tür örneği gerçek varlık türünü almak için sınıf.

Daha fazla bilgi için [proxy ile çalışmayı](https://msdn.microsoft.com/data/JJ592886.aspx) MSDN'de.

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a>Otomatik değiştirme algılama

Entity Framework, bir varlığın geçerli değerleri özgün değerlerle karşılaştırarak, varlığın nasıl değiştiğini (ve bu nedenle hangi güncelleştirmelerin veritabanına gönderilmesi gerekir) belirler. Varlık sorgulanan ya da bağlı orijinal değerleri depolanır. Otomatik değiştirme algılama neden yöntemlerden bazıları aşağıda verilmiştir:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Aşağıdaki yöntemlerden birini bir döngüde birçok kez çağırmak ve çok sayıda varlık izliyorsunuz, önemli performans iyileştirmeleri otomatik değişiklik algılama kullanarak geçici olarak kapatarak alabilirsiniz [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) özelliği. Daha fazla bilgi için [değişiklikleri otomatik olarak algılama](https://msdn.microsoft.com/data/jj556205) MSDN'de.

<a id="validation"></a>
## <a name="automatic-validation"></a>Otomatik doğrulama

Çağırdığınızda `SaveChanges` yöntemi, varsayılan olarak Entity Framework doğrular değiştirilen tüm varlıkların tüm özelliklerini verileri veritabanını güncelleştirmeden önce. Çok sayıda varlık güncelleştirdik ve bu gereksiz çalışmadır zaten veri doğruladınız işlemini yapabilir değişiklikleri geçici olarak devre dışı doğrulama kapatarak daha az zaman alır. Bu kullanarak yapabileceğiniz [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) özelliği. Daha fazla bilgi için [doğrulama](https://msdn.microsoft.com/data/gg193959) MSDN'de.

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a>Entity Framework güç araçları

[Entity Framework güç araçları](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d) veri modeli diyagramları oluşturmak için kullanılan Visual Studio eklentisi aşağıdaki öğreticilerde gösterilir. Araçları, veritabanı Code First ile kullanabilmesi için varlık sınıfları üretmek gibi var olan bir veritabanında tabloları temel diğer işlevi de yapabilirsiniz. Araçları yüklendikten sonra bağlam menülerini bazı ek seçenekler görünür. Örneğin, sağ tıkladığınızda bağlam sınıfınızda **Çözüm Gezgini**, bir diyagram Oluştur seçeneğine sahip olursunuz. Code First kullanırken diyagram veri modelinde değiştiremezsiniz, ancak, şeyler anlamak daha kolay hale getirmek üzere gezinmeye.

![EF bağlam menüsü](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![EF diyagramı](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a>Entity Framework kaynak kodu

Entity Framework 6 için kaynak kodu kullanılabilir [GitHub](https://github.com/aspnet/EntityFramework6). Hataları dosya ve kendi geliştirmeleri EF kaynak koduna katkıda bulunabilir.

Kaynak kodu açık olsa da, Entity Framework tam olarak bir Microsoft ürünü olarak desteklenir. Microsoft Entity Framework takım denetim üzerinde katkılarını kabul tutar ve her bir yayının kalitesini sağlamak için tüm kod değişikliklerinin test eder.

<a id="summary"></a>
## <a name="summary"></a>Özet

Bu, Bu öğretici serisinde, Entity Framework kullanarak bir ASP.NET MVC uygulamasındaki tamamlar. Entity Framework kullanarak verilerle çalışma hakkında daha fazla bilgi için bkz. [MSDN belgeleri sayfasında EF](https://msdn.microsoft.com/data/ee712907) ve [ASP.NET veri erişimi - önerilen kaynaklar](../../../../whitepapers/aspnet-data-access-content-map.md).

Bunu oluşturduktan sonra web uygulamanızı dağıtma hakkında daha fazla bilgi için bkz. [ASP.NET Web dağıtımı - önerilen kaynaklar](../../../../whitepapers/aspnet-web-deployment-content-map.md) MSDN Kitaplığı'nda.

MVC için kimlik doğrulaması ve yetkilendirme gibi ilgili diğer konular hakkında bilgi için bkz. [ASP.NET MVC - önerilen kaynaklar](../recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>İlgili kaynaklar

- Tom Dykstra Bu öğreticinin özgün sürümle yazdım, EF 5 güncelleştirme yazarlarından ve EF 6 güncelleştirme yazdım. Tom Microsoft Web Platformu ve araçları içerik ekibi Kıdemli bir programlama yazardır.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) Eğitmen EF 5 ve MVC 4 için güncelleştirme çoğunu tamamladıysanız ve EF 6 güncelleştirme yazarlarından. Rick bir Microsoft Azure ve MVC odaklanmak için üst düzey programlama yazardır.
- [Rowan Miller](http://www.romiller.com) ve diğer Entity Framework takım üyeleri kod incelemeleriyle Yardımlı ve biz EF 5 ve EF 6 için öğreticiyi güncelleştirmekte olduğunuz, çıkan geçişleri ile birçok sorunlarında hata ayıklama yardımcı olmuştur.

<a id="vb"></a>
## <a name="vb"></a>VB

Öğretici, ilk olarak EF 4.1 için üretilen, biz tamamlanan indirme proje hem C# ve VB sürümleri sağlanır. Zaman sınırlaması ve çalışanlarınızın diğer önceliklere nedeniyle, bu sürüm için uyguladığımız değil. Varsa bu öğreticileri kullanarak bir VB projesi oluşturun ve başkalarıyla paylaşmak için lütfen bize bildirin iradeye sahip olması.

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a>Sık karşılaşılan hatalar ve çözümleri veya geçici çözümleri için bunları

### <a name="cannot-createshadow-copy"></a>Oluşturma/kopyalama gölge olamaz

Hata iletisi:

> Oluşturma/kopyalama gölge olamaz '&lt;filename&gt;', zaten mevcut.

Çözüm

Birkaç saniye bekleyin ve sayfayı yenileyin.

### <a name="update-database-not-recognized"></a>Veritabanını Güncelleştir tanınmıyor

Hata iletisi (gelen `Update-Database` PMC'yi komutunu):

> ' % S'terim 'Veritabanını Güncelleştir' cmdlet'i, işlev, komut dosyası veya çalıştırılabilir program adı olarak tanınmıyor. Adının yazımını denetleyin veya bir yol varsa, yolun doğru olduğundan emin olun ve yeniden deneyin.

Çözüm

Visual Studio'dan çıkın. Projeyi yeniden açıp yeniden deneyin.

### <a name="validation-failed"></a>Doğrulama başarısız oldu

Hata iletisi (gelen `Update-Database` PMC'yi komutunu):

> Bir veya daha fazla varlık için doğrulanamadı. Daha fazla ayrıntı için 'EntityValidationErrors' özelliğine bakın.

Çözüm

Bu sorunun bir nedeni olduğundan doğrulama hataları olduğunda `Seed` yöntemi çalışır. Bkz: [Seeding ve hata ayıklama Entity Framework (EF) Db'ler](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) hata ayıklama ipuçları için `Seed` yöntemi.

### <a name="http-50019-error"></a>500.19 HTTP hatası

Hata iletisi:

> HTTP Hatası 500.19 - iç sunucu hatası istenen sayfa sayfa için ilgili yapılandırma verileri geçersiz olduğundan erişilemez.

Çözüm

Bu hatayı alabileceğiniz bir çözüm, her biri aynı bağlantı noktası numarası kullanarak bunları birden çok kopyasını kalmamasını yoludur. Ayrıca, Visual Studio'nun tüm örneklerini çıkmadan, daha sonra üzerinde çalıştığınız projeyi yeniden genellikle bu sorunu çözebilirsiniz. Bu işe yaramazsa, bağlantı noktası numarasını değiştirmeyi deneyin. Proje dosyası üzerinde sağ tıklayın ve ardından Özellikler seçeneğine tıklayın. Seçin **Web** sekmesini ve sonra bağlantı noktası numarasını değiştirmelisiniz **proje URL'si** metin kutusu.

### <a name="error-locating-sql-server-instance"></a>Hata bulmayla SQL Server örneği

Hata iletisi:

> Bir SQL Server bağlantısı kurulurken ağla ilgili veya örneğe özel bir hata oluştu. Sunucu bulunamadı veya erişilebilir durumda değildi. Örnek adının doğru olduğundan ve SQL Server Uzak bağlantılara izin verecek şekilde yapılandırıldığını doğrulayın. (sağlayıcı: SQL ağ arabirimleri, hata: 26 - Server/örneği belirtilen hata bulma)

Çözüm

Bağlantı dizesini kontrol edin. El ile veritabanını sildiyseniz, yapım dizesinde veritabanının adını değiştirin.

> [!div class="step-by-step"]
> [Önceki](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
