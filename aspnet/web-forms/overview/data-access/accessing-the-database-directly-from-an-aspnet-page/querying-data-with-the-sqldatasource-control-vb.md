---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
title: SqlDataSource denetimi (VB) ile veri sorgulama | Microsoft Docs
author: rick-anderson
description: "Önceki eğitimlerine sunu katmanı veri erişim katmanı tam olarak ayırmak için ObjectDataSource Denetimi kullanılır. Bu tutor ile başlatılıyor..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: b12f752d-3502-40a4-b695-fc7b7d08cfd3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
msc.type: authoredcontent
ms.openlocfilehash: a3832bd9847ec8e789b71d13b30a673c8779f4ac
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="querying-data-with-the-sqldatasource-control-vb"></a>SqlDataSource denetimi (VB) ile veri sorgulama
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_VB.exe) veya [PDF indirin](querying-data-with-the-sqldatasource-control-vb/_static/datatutorial47vb1.pdf)

> Önceki eğitimlerine sunu katmanı veri erişim katmanı tam olarak ayırmak için ObjectDataSource Denetimi kullanılır. Bu öğretici ile başlayarak, biz SqlDataSource denetimi sunu ve veri erişimi katı ayırma gerektirmeyen basit uygulamalar için nasıl kullanılacağını öğrenin.


## <a name="introduction"></a>Giriş

Tüm öğreticileri biz kadarki incelenmesi ve sunu, iş mantığı ve verileri erişim katmanları oluşan bir katmanlı mimarisi kullanmış. Veri erişim düzeyi (DAL) ilk öğreticide hazırlanmış ([veri erişim katmanı oluşturma](../introduction/creating-a-data-access-layer-vb.md)) ve iş mantığı katmanı ikinci ([bir iş mantığı katmanı oluşturma](../introduction/creating-a-business-logic-layer-vb.md)). İle başlayarak [veri ObjectDataSource ile görüntüleme](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) Öğreticisi, ASP.NET 2.0 s yeni ObjectDataSource Denetimi sunu katmanı mimarisinden ile bildirimli olarak arabirim için nasıl kullanılacağını gördük.

Mimari öğreticileri kadarki tüm verilerle çalışmak için kullanılan olsa da, aynı zamanda erişim, Ekle, Güncelleştir ve mimarisi atlayarak doğrudan bir ASP.NET sayfasından, veritabanı verilerini silmek mümkündür. Bunun yapılması belirli bir veritabanını sorgular ve iş mantığı doğrudan web sayfasındaki yerleştirir. Yeterince büyük veya karmaşık uygulamalar için katmanlı bir mimari kullanarak tasarlama ve uygulama oldukça başarı, Güncelleştirilebilirlik ve uygulama bakımı için önemlidir. Sağlam bir mimari geliştirme, ancak verebilmesine basit, tek seferlik uygulamaları oluştururken gereksiz olabilir.

ASP.NET 2.0 sağlar beş yerleşik veri kaynağı denetimleri [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), ve [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). SqlDataSource erişme ve Microsoft SQL Server, Microsoft Access, Oracle, MySQL ve diğerleri de dahil olmak üzere doğrudan bir ilişkisel veritabanı, verileri değiştirmek için kullanılabilir. Bu öğretici ve sonraki üç, biz SqlDataSource denetimi ile çalışmak üzere, SqlDataSource kullanma yanı sıra sorgulama ve filtre veritabanı veri araştırma, ekleme, güncelleştirme ve verileri silme inceleyeceğiz.


![ASP.NET 2.0 beş yerleşik veri kaynağı denetimleri içerir](querying-data-with-the-sqldatasource-control-vb/_static/image1.gif)

**Şekil 1**: ASP.NET 2.0, beş yerleşik veri kaynağı denetimleri içerir


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>ObjectDataSource ve SqlDataSource karşılaştırma

Kavramsal olarak, ObjectDataSource ve SqlDataSource denetimleri yalnızca proxy'leri verilere bağlıdır. ' Da anlatıldığı gibi [veri ObjectDataSource ile görüntüleme](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) öğretici, ObjectDataSource sahip veri ve seçmek için çağrılacak yöntemleri eklemek, güncelleştirme ve verileri silme sağlayan nesne türü belirtmek özellikleri temel alınan nesnenin türü. ObjectDataSource özelliklerinde yapılandırdıktan sonra bir veri GridView, DetailsView veya DataList gibi Web denetimi ObjectDataSource s kullanarak denetimine bağlı olabilir `Select()`, `Insert()`, `Delete()`, ve `Update()` yöntemleri temel alınan mimarisinde ile etkileşim.

SqlDataSource aynı işlevselliği sunar, ancak Nesne Kitaplığı yerine ilişkisel bir veritabanına karşı çalışır. SqlDataSource ile veritabanı bağlantı dizesi ve geçici SQL sorguları belirtmeniz gerekir veya saklı yordamları ekleme, güncelleştirme, silme ve verileri almak için çalıştırılacak. SqlDataSource s `Select()`, `Insert()`, `Update()`, ve `Delete()` çağrıldığında, yöntem, belirtilen veritabanına bağlanmak ve uygun SQL sorgusu yayınlanacak. Aşağıdaki diyagramda gösterildiği gibi bu yöntemleri bir veritabanına bağlanma, bir sorgu verme ve sonuçları döndüren grunt çalışma yapın.


![SqlDataSource veritabanı için bir Proxy olarak görev yapar](querying-data-with-the-sqldatasource-control-vb/_static/image2.gif)

**Şekil 2**: SqlDataSource veritabanı için bir Proxy olarak görev yapar


> [!NOTE]
> Bu öğreticide biz veritabanından veri alınırken odak. İçinde [ekleme, güncelleştirme ve SqlDataSource denetimi silinirken verilerle](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md) Öğreticisi, biz SqlDataSource ekleme, güncelleştirme ve silme destekleyecek şekilde yapılandırmak nasıl göreceksiniz.


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>SqlDataSource ve AccessDataSource denetimleri

SqlDataSource denetimi ek olarak, ASP.NET 2.0 bir AccessDataSource denetimi de içerir. Bu iki farklı denetim çoğu geliştiricinin ASP.NET 2.0 AccessDataSource denetimi yalnızca Microsoft SQL Server ile birlikte çalışmak üzere tasarlanmış SqlDataSource denetimi yalnızca Microsoft Access ile çalışmak üzere tasarlanmıştır şüpheleniyorsanız yeni yol açar. AccessDataSource özellikle Microsoft Access ile çalışmak için tasarlanmış olsa da, SqlDataSource denetimi ile çalışır *herhangi* .NET erişilebilen ilişkisel veritabanı. Bu, tüm OleDb veya ODBC uyumlu veri depoları, Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL ve PostgreSQL, gibi diğer birçok arasında içerir.

AccessDataSource ve SqlDataSource denetimleri arasındaki tek fark, veritabanı bağlantı bilgilerini nasıl belirtildiğinden ' dir. AccessDataSource denetimi yalnızca Access veritabanı dosyasının dosya yolu gerekiyor. SqlDataSource Öte yandan, bir tam bağlantı dizesi gerektirir.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>1. adım: SqlDataSource Web sayfaları oluşturma

SqlDataSource denetimi kullanarak doğrudan veritabanını verilerle çalışmak nasıl keşfetme başlamadan önce öncelikle Bu öğretici ve sonraki üç yapmamız gereken Web sitesi Projemizin ASP.NET sayfaları oluşturmak için bir dakikanızı ayırın s olanak tanır. Adlı yeni bir klasör eklemeye başlayın `SqlDataSource`. Ardından, o klasördeki her sayfayla ilişkilendirilecek emin olmak için aşağıdaki ASP.NET sayfaları ekleme `Site.master` ana sayfa:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![ASP.NET sayfaları için SqlDataSource ile ilgili öğreticiler ekleme](querying-data-with-the-sqldatasource-control-vb/_static/image3.gif)

**Şekil 3**: SqlDataSource ilgili öğreticileri için ASP.NET sayfaları ekleme


Diğer klasörler gibi `Default.aspx` içinde `SqlDataSource` klasörü öğreticileri kendi bölümünde listeler. Sözcüğünün `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu nedenle, bu kullanıcı denetimi Ekle `Default.aspx` s Tasarım görünümü sayfaya Çözüm Gezgini'nden sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](querying-data-with-the-sqldatasource-control-vb/_static/image5.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image4.gif)

**Şekil 4**: eklemek `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](querying-data-with-the-sqldatasource-control-vb/_static/image6.gif))


Son olarak, bu dört sayfaları girişlere ekleyin `Web.sitemap` dosya. Özellikle, aşağıdaki biçimlendirme sonra ekleme özel düğmeler DataList yineleyici ekleyin ve `<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample1.sql)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticileri Web sitesini görüntülemek için bir dakikanızı ayırın. Soldaki menüde artık düzenleme, ekleme ve öğreticiler silmek için öğeler içerir.


![Site Haritasını girişleri SqlDataSource öğreticileri için şimdi içerir.](querying-data-with-the-sqldatasource-control-vb/_static/image7.gif)

**Şekil 5**: Site Haritası girişleri SqlDataSource öğreticileri için şimdi içerir.


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>2. adım: Ekleme ve SqlDataSource denetimi yapılandırma

Başlangıç açarak `Querying.aspx` sayfasındaki `SqlDataSource` klasörü ve Tasarım görünümüne geçin. Araç kutusu Tasarımcısı ve kümesi üzerine SqlDataSource denetimini sürükleyin kendi `ID` için `ProductsDataSource`. ObjectDataSource olduğu gibi SqlDataSource işlenmiş herhangi bir çıktı üretmez ve bu nedenle tasarım yüzeyine gri kutu olarak görünür. SqlDataSource yapılandırmak için SqlDataSource s akıllı etiket yapılandırma veri kaynağı bağlantısını tıklayın.


![Tıklayın SqlDataSource s akıllı etiket veri kaynağı bağlantısını yapılandırın](querying-data-with-the-sqldatasource-control-vb/_static/image8.gif)

**Şekil 6**: tıklayın SqlDataSource s akıllı etiket veri kaynağı bağlantısını yapılandırın


Bu SqlDataSource denetim s veri kaynağı Yapılandırma Sihirbazı'nı getirir. ObjectDataSource Denetimi s Sihirbazı s adımları farklılık gösterir, ancak son hedef almak, Ekle, Güncelleştir ve veri kaynağı üzerinden verileri silmek nasıl ayrıntılarını sağlamak için aynıdır. SqlDataSource için bu, temel alınan veritabanının kullanılacağını belirtme ve saklı yordamları veya geçici SQL deyimleri sağlayarak kapsar.

İlk sihirbaz adımı bize veritabanı için ister. Bu veritabanı web uygulaması s bulunamadı aşağı açılan listeyi içeren `App_Data` klasörü ve Sunucu Gezgininde veri bağlantıları için eklenmiştir. Bu yana ullanıcı zaten bir bağlantı dizesi eklediğimiz `NORTHWIND.MDF` veritabanını `App_Data` Projemizin s klasörüne `Web.config` dosyası, açılan listesinden, bu bağlantı dizesi için bir başvuru içerir `NORTHWINDConnectionString`. Bu öğeyi aşağı açılan listeden seçin ve İleri'yi tıklatın.


![NORTHWINDConnectionString aşağı açılan listeden seçin](querying-data-with-the-sqldatasource-control-vb/_static/image9.gif)

**Şekil 7**: seçin `NORTHWINDConnectionString` aşağı açılan listeden


Veritabanını seçtikten sonra sihirbaz sorgu verileri döndürmek sorar. Şu tablo veya Görünüm dönün veya özel bir SQL deyimi girebilir veya saklı yordam belirtmek için sütunlarının ya da belirtebilirsiniz. Bu seçeneği belirtin aracılığıyla arasında özel bir SQL deyimi veya saklı yordam ve bir tablo belirtin sütunlarından geçiş veya radyo düğmeleri görüntüleyin.

> [!NOTE]
> Bu ilk Örneğin, bir tablo veya görünüm seçeneği belirtin sütunlarından kullanmak s olanak tanır. Biz, daha sonra Bu öğreticide sihirbaza geri dönün ve özel bir SQL deyimi veya saklı yordam seçeneği belirtin keşfedin.


Bir tablo veya Görünüm radyo düğmesi belirt sütunlarından seçildiğinde Şekil 8 yapılandırma Select deyimi ekran gösterir. Aşağı açılan liste tabloları ve görünümleri Northwind veritabanındaki seçili tabloyu veya görünümü s sütunları onay kutusu listesinde görüntülenen kümesini içerir. Bu örnekte, let s dönmek `ProductID`, `ProductName`, ve `UnitPrice` sütunlarından `Products` tablo. Sihirbazın bu seçim yapmadan elde edilen SQL deyimini gösterir sonra Şekil 8 gösterildiği gibi `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.


![Veri ürünleri bir tablo döndürür](querying-data-with-the-sqldatasource-control-vb/_static/image10.gif)

**Şekil 8**: dönüş verileri `Products` tablosu


Döndürülecek Sihirbazı'nı yapılandırdıktan sonra `ProductID`, `ProductName`, ve `UnitPrice` sütunlarından `Products` tablo, İleri düğmesine tıklayın. Bu son ekran önceki adımda yapılandırılmış sorgu sonuçlarını İnceleme fırsatı sağlar. Test Query düğmesini tıklatarak yürütür yapılandırılmış `SELECT` deyimi ve kılavuzda sonuçları görüntüler.


![SEÇME sorgusu gözden geçirmek için Test sorgu düğmesini tıklatın](querying-data-with-the-sqldatasource-control-vb/_static/image11.gif)

**Şekil 9**: gözden geçirme Test sorgu düğmesine tıklayın, `SELECT` sorgu


Sihirbazı tamamlamak için Son'u tıklatın.

ObjectDataSource ile SqlDataSource s Sihirbazı'nı yalnızca değerleri denetim s, yani özelliklerine gibi [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) ve [ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) özellikleri. Sihirbazı tamamladıktan sonra SqlDataSource denetimi s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample2.aspx)]

`ConnectionString` Özelliği veritabanına bağlanma hakkında bilgi sağlar. Bu özellik bir tam, sabit kodlanmış bağlantı dizesi değer atanabilir veya bir bağlantı dizesinde işaret edebilir `Web.config`. Web.config bağlantı dizesi değerindeki başvurmak için söz dizimini kullanın `<%$ expressionPrefix:expressionValue %>`. Genellikle, *expressionPrefix* ConnectionStrings olduğu ve *expressionValue* bağlantı dizesinde adıdır `Web.config` [ `<connectionStrings>` bölüm](https://msdn.microsoft.com/library/bf7sd233.aspx). Ancak, sözdizimi kullanılabilir başvuru `<appSettings>` öğeleri veya içerik kaynak dosyalarından. Bkz: [ASP.NET ifadeleri genel bakış](https://msdn.microsoft.com/library/d5bd1tad.aspx) Bu sözdizimi hakkında daha fazla bilgi için.

`SelectCommand` Özelliği, geçici SQL deyimi veya verileri döndürmek için saklı yordam belirtir.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>3. adım: veri Web denetim ekleme ve SqlDataSource bağlama

SqlDataSource yapılandırıldıktan sonra bir veri GridView veya DetailsView gibi Web denetimi bağlanabilir. Bu öğreticide, verileri bir GridView görüntülemek s olanak tanır. Araç Kutusu'ndan GridView sayfaya sürükleyin ve onu bağladıktan `ProductsDataSource` GridView s akıllı etiket aşağı açılan listeden veri kaynağını seçerek SqlDataSource.


[![GridView ekleyin ve SqlDataSource denetimine bağlama](querying-data-with-the-sqldatasource-control-vb/_static/image13.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image12.gif)

**Şekil 10**: GridView ekleyin ve SqlDataSource denetimi bağlamak ([tam boyutlu görüntüyü görüntülemek için tıklatın](querying-data-with-the-sqldatasource-control-vb/_static/image14.gif))


GridView s akıllı etiket aşağı açılan listeden SqlDataSource denetimi önceden seçildikten sonra Visual Studio otomatik olarak bir BoundField veya CheckBoxField GridView için veri kaynağı denetimi tarafından döndürülen sütunların her biri için ekler. SqlDataSource üç veritabanı sütunları döndürdüğünden `ProductID`, `ProductName`, ve `UnitPrice` GridView üç alan vardır.

GridView s üç yapılandırmak için bir dakikanızı ayırın BoundFields. Değişiklik `ProductName` alan s `HeaderText` Product Name özelliğine ve `UnitPrice` fiyat alan s. Ayrıca biçimlendirmek `UnitPrice` bir para birimi olarak alan. Bu değişiklikleri yaptıktan sonra GridView s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample3.aspx)]

Bir tarayıcı aracılığıyla bu sayfasını ziyaret edin. Şekil 11 gösterildiği gibi her ürünün s GridView listeler `ProductID`, `ProductName`, ve `UnitPrice` değerleri.


[![GridView, her ürün s ProductID, ProductName ve UnitPrice değerleri görüntüler](querying-data-with-the-sqldatasource-control-vb/_static/image16.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image15.gif)

**Şekil 11**: GridView görüntüler her ürün s `ProductID`, `ProductName`, ve `UnitPrice` değerleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](querying-data-with-the-sqldatasource-control-vb/_static/image17.gif))


Sayfasını ziyaret kendi veri kaynağı denetimi s GridView çağırır `Select()` yöntemi. ObjectDataSource Denetimi kullanmakta olduğunuz olduğunda bu çağrılır `ProductsBLL` s sınıfı `GetProducts()` yöntemi. SqlDataSource, ancak ile `Select()` yöntemi belirtilen veritabanı ve sorunları için bir bağlantı kurar `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, bu örnekte). SqlDataSource GridView ardından, döndürülen her veritabanı kaydı için GridView, bir satır oluşturmayı numaralandırır sonuçlarını döndürür.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Yerleşik veri Web denetimi özellikleri ve SqlDataSource denetimi

Genel olarak, Web denetimleri disk belleği, sıralama, düzenleme, verileri devralınmış özellikler silme, ekleme ve benzeri veri Web denetimine özeldir ve kullanılan veri kaynağı denetimi bağımlı değildir. Diğer bir deyişle, GridView, disk belleği, sıralama, düzenleme ve bir ObjectDataSource veya bir SqlDataSource bağlı olup olmadığını silme yerleşik kullanabilirler. Ancak, belirli veri Web denetimi özellikleri kullanılan veri kaynağı denetimi veya veri kaynağı denetim s yapılandırması duyarlıdır.

Örneğin, [verimli bir şekilde disk belleği üzerinden büyük miktarlarda veri](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) ele nasıl, varsayılan olarak, Web veriler için disk belleği mantığı naively döndürür denetimleri öğretici *tüm* temeldeki kayıtları veri kaynağı ve yalnızca uygun kayıtların alt kümesini verilen geçerli sayfa dizini ve sayfa başına görüntülenecek kayıt sayısını görüntüler. Bu model, yeterli büyüklükte sonuç kümeleri sayfalama yüksek oranda verimli değildir. Neyse ki, ObjectDataSource görüntülenecek kayıt yalnızca kesin kısmı döndürür özel sayfalama destekleyecek şekilde yapılandırılabilir. SqlDataSource denetimi ancak, özel sayfalama uygulamak için özellikler eksik.

Disk belleği ve sıralama ile başka bir subtlety SqlDataSource ile ortaya çıkar. Varsayılan olarak, SqlDataSource döndürülen verilerin disk belleği veya GridView sıralanır. Bunu göstermek için GridView s akıllı etiket etkinleştirmek disk belleği ve etkinleştirme sıralama seçeneklerinde denetleyin `Querying.aspx` ve beklendiği gibi çalıştığını doğrulayın.

SqlDataSource geniş yazılmış bir veri kümesine ilişkin Veritabanı verilerinin getireceğinden sıralama ve disk belleği çalışır. Toplam disk belleği uygulama için temel bir yönü sorgu tarafından döndürülen kayıt sayısı kümesinden saptanabilen. Ayrıca, veri kümesi s sonuçları DataView ile sıralanabilir. GridView istekleri disk belleğine alınan ya da veri sıralanmış olduğunda bu özellikler otomatik olarak SqlDataSource tarafından kullanılır.

SqlDataSource değiştirerek DataReader bir veri kümesi yerine döndürülecek yapılandırılabilir kendi [ `DataSourceMode` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) gelen `DataSet` (varsayılan) için `DataReader`. DataReader kullanarak durumlarda SqlDataSource s sonuçları DataReader beklediği var olan kodu geçirilirken tercih edilen. Ayrıca, DataReader veri kümeleri daha önemli ölçüde daha basit nesne olduğundan, daha iyi performans sunar. Bu değişiklik yaparsanız, ancak veri Web denetimi, sıralayabilirsiniz veya SqlDataSource kaç kayıtları sorgu tarafından döndürülen onaylaması veya DataReader olmadığından bu yana sayfa döndürülen verileri sıralamak için tüm teknikler sunar.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>4. adım: özel SQL deyimi kullanarak veya saklı yordam

SqlDataSource denetimi yapılandırırken, veri döndürmek için kullanılan sorgu iki yaklaşım birinde özel bir SQL deyimi veya saklı yordam veya varolan bir tablo veya Görünüm sütunlarından olarak belirtilebilir. Biz incelenmesi 2. adımda sütunlarından seçerek `Products` tablo. Özel bir SQL deyimi kullanarak ara s olanak tanır.

Başka bir GridView denetimine ekleme `Querying.aspx` sayfasında ve akıllı etiket aşağı açılan listeden yeni bir veri kaynağı oluşturmak için seçin. Ardından, belirtmek verileri veritabanından bu çekebilir, yeni bir SqlDataSource denetim oluşturur. Denetim adı `ProductsWithCategoryInfoDataSource`.


![ProductsWithCategoryInfoDataSource adlı yeni bir SqlDataSource denetimi oluşturma](querying-data-with-the-sqldatasource-control-vb/_static/image18.gif)

**Şekil 12**: adlı yeni bir SqlDataSource denetimi oluşturma`ProductsWithCategoryInfoDataSource`


Sonraki ekranda bize veritabanını belirtmek için sorar. Şekil 7'de yaptığımız gibi seçin `NORTHWINDConnectionString` açılan dan listesinde ve İleri'yi tıklatın. Select deyimi ekran yapılandırma, belirt özel bir SQL deyimi veya saklı yordam radyo düğmesini seçin ve İleri'yi tıklatın. Bu seçim, güncelleştirme, ekleme ve silme etiketli sekmeleri sağlayan özel deyimleri tanımlayın veya saklı yordamlar ekranını, getirir. Her bir sekmede özel bir SQL deyimi aşağıdaki metin kutusuna girin ya da aşağı açılan listeden bir saklı yordam seçin. Bu öğreticide özel bir SQL deyimi girme adresindeki arar; sonraki öğretici bir saklı yordam kullanan bir örnek içerir.


![Özel SQL deyimi girin ya da bir saklı yordam seçin](querying-data-with-the-sqldatasource-control-vb/_static/image19.gif)

**Şekil 13**: özel SQL deyimi girin ya da bir saklı yordam seçin


Özel bir SQL deyimi aşağıdaki metin kutusuna elle girilebilir veya Sorgu Oluşturucusu düğmesine tıklayarak grafik oluşturulabilir. Sorgu Oluşturucusu'nu veya metin kutusu altından döndürmek için aşağıdaki sorguyu kullanın `ProductID` ve `ProductName` alanlarını `Products` kullanarak tablo bir `JOIN` s ürün almak için `CategoryName` gelen `Categories` tablosu:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample4.sql)]


![Grafik sorgu kullanarak Sorgu Oluşturucusu oluşturabileceğiniz](querying-data-with-the-sqldatasource-control-vb/_static/image20.gif)

**Şekil 14**: Sorgu Oluşturucusu'nu kullanarak sorgu grafik oluşturmak için


Sorgu belirttikten sonra Test sorgusu ekranına devam etmek için İleri'yi tıklatın. SqlDataSource Sihirbazı'nı tamamlamak için Son'u tıklatın.

Sihirbazı tamamladıktan sonra GridView üç BoundFields görüntüleme kendisine eklenmiş olacaktır `ProductID`, `ProductName`, ve `CategoryName` sorgudan döndürülen ve aşağıdaki bildirim temelli biçimlendirmede kaynaklanan sütunlar:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample5.aspx)]


[![Her ürün s Kimliğini, adını ve ilişkili kategori adı GridView gösterir](querying-data-with-the-sqldatasource-control-vb/_static/image22.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image21.gif)

**Şekil 15**: GridView gösterir her ürün s kimliği, ad ve ilişkili kategori adı ([tam boyutlu görüntüyü görüntülemek için tıklatın](querying-data-with-the-sqldatasource-control-vb/_static/image23.gif))


## <a name="summary"></a>Özet

Bu öğreticide sorgulamak ve SqlDataSource denetimi kullanarak verileri görüntülemek nasıl gördük. ObjectDataSource gibi SqlDataSource verilere erişmek için bildirim temelli bir yaklaşım sağlayan bir proxy olarak görev yapar. Bağlanmak için veritabanı ve SQL özelliklerini belirtin `SELECT` yürütülecek sorgu; Özellikler penceresini aracılığıyla veya veri kaynağı Yapılandırma Sihirbazı kullanılarak belirtilebilir.

`SELECT` Biz Bu öğreticide incelenmesi sorgu örnekler tüm kayıtlar belirtilen sorgudan döndürülen. SqlDataSource denetimi ancak içerebilir bir `WHERE` değerleri programlı olarak atanmış veya otomatik olarak belirtilen bir kaynaktan çekilen parametreleriyle yan tümcesi. Oluşturma ve sonraki öğreticide parametreli sorgular kullanmak inceleyeceğiz!

Mutluluk programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [İlişkisel veritabanı verilerine erişme](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [SqlDataSource denetimine genel bakış](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [ASP.NET Quickstart öğreticiler: SqlDataSource denetimi](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Web.config `<connectionStrings>` öğesi](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Veritabanı bağlantı dizesi başvurusu](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Çiğdem Connery, Bernadette Leigh ve David Suru yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
[sonraki](using-parameterized-queries-with-the-sqldatasource-vb.md)
