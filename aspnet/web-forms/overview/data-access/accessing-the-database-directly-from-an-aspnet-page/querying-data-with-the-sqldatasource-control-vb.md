---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
title: SqlDataSource denetimi (VB) ile veri sorgulama | Microsoft Docs
author: rick-anderson
description: Önceki öğreticilerde tam veri erişimi katmanı sunu katmanını ayırmak için ObjectDataSource Denetimi kullanılır. Bu tutor ile başlatılıyor...
ms.author: aspnetcontent
ms.date: 02/20/2007
ms.assetid: b12f752d-3502-40a4-b695-fc7b7d08cfd3
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 2fa26eeb640ddf096d5002b9bffbb48a87cfccd9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830538"
---
<a name="querying-data-with-the-sqldatasource-control-vb"></a>SqlDataSource denetimi (VB) ile veri sorgulama
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_VB.exe) veya [PDF olarak indirin](querying-data-with-the-sqldatasource-control-vb/_static/datatutorial47vb1.pdf)

> Önceki öğreticilerde tam veri erişimi katmanı sunu katmanını ayırmak için ObjectDataSource Denetimi kullanılır. Bu öğretici ile başlayarak, biz SqlDataSource denetimi sunu ve veri erişimi, katı ayırma gerektirmeyen basit uygulamalar için nasıl kullanılabileceğini öğrenin.


## <a name="introduction"></a>Giriş

Tüm öğreticileri biz şu ana kadar incelenir ve sunu, iş mantığı ve veri erişim katmanları oluşan katmanlı bir mimari kullanmış. Veri erişim katmanı (DAL) ilk öğreticide üretildi ([veri erişim katmanını oluşturma](../introduction/creating-a-data-access-layer-vb.md)) ve iş mantığı katmanı ikinci ([iş mantığı katmanı oluşturma](../introduction/creating-a-business-logic-layer-vb.md)). İle başlayarak [görüntüleyen veri ile ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) öğretici bildirimli olarak sunu katmanını mimariden ile arabirim oluşturmak için ASP.NET 2.0 s yeni ObjectDataSource Denetimi kullanmayı gördük.

Mimari öğreticileri kadarki tüm verilerle çalışmak için kullandığınız sırada da erişim, ekleme, güncelleştirme ve mimari atlayarak doğrudan bir ASP.NET sayfasından, veritabanı verilerini silin mümkündür. Bunun yapılması web sayfasında doğrudan belirli veritabanı sorgularını ve iş mantığı yerleştirir. Yeterince büyük veya karmaşık uygulamalar için katmanlı bir mimari kullanarak tasarlama ve uygulama başarı, Güncelleştirilebilirlik ve uygulama bakımı için oldukça önemli. Sağlam bir mimari geliştirmek, ancak verebilmesine basit, tek seferlik uygulamalar oluştururken, gereksiz olabilir.

ASP.NET 2.0 sağlayan beş yerleşik veri kaynağı denetimleri [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), ve [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). SqlDataSource erişmek ve Microsoft SQL Server, Microsoft Access, Oracle, MySQL ve diğerleri gibi bir ilişkisel veritabanına doğrudan, verileri değiştirmek için kullanılabilir. Bu öğretici ve sonraki üç SqlDataSource denetimi ile çalışmak için SqlDataSource kullanmayı yanı sıra nasıl sorgulanacağını ve filtre veritabanı veri keşfetme, ekleme, güncelleştirme ve verileri silmek nasıl inceleyeceğiz.


![ASP.NET 2.0, beş yerleşik veri kaynağı denetimleri içerir.](querying-data-with-the-sqldatasource-control-vb/_static/image1.gif)

**Şekil 1**: ASP.NET 2.0 beş yerleşik veri kaynaklarını içerir


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>SqlDataSource ve ObjectDataSource karşılaştırma

Kavramsal olarak, ObjectDataSource ve SqlDataSource denetimleri yalnızca verilere proxy'ler. Bölümünde açıklandığı gibi [görüntüleyen veri ile ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) öğreticide ObjectDataSource, verilere ve yöntemlere seçmek için çağrılacak ekleme, güncelleştirme ve verileri silmek sağlayan nesne türünü belirten özelliklere sahiptir temel alınan nesnenin türü. ObjectDataSource s özellikleri yapılandırdıktan sonra bir veri GridView, DetailsView veya DataList gibi Web denetimi denetime ObjectDataSource s kullanarak bağlanabilir `Select()`, `Insert()`, `Delete()`, ve `Update()` yöntemleri temel alınan mimarisinde ile etkileşim kurun.

SqlDataSource aynı işlevselliği sağlar, ancak Nesne Kitaplığı yerine bir ilişkisel veritabanına karşı çalışır. SqlDataSource ile veritabanı bağlantı dizesi ve geçici SQL sorguları belirtmeniz gerekir veya saklı yordam, ekleme, güncelleştirme, silme ve verileri almak için çalıştırılacak. SqlDataSource s `Select()`, `Insert()`, `Update()`, ve `Delete()` çağrıldığında, yöntem, belirtilen veritabanına bağlanmak ve uygun SQL sorgusu yayınlanacak. Aşağıdaki diyagramda gösterildiği gibi bu yöntemleri sonuçları döndüren bir veritabanına bağlanma ve bir sorgu verme grunt çalışma yapın.


![SqlDataSource veritabanına bir Proxy işlevi görür](querying-data-with-the-sqldatasource-control-vb/_static/image2.gif)

**Şekil 2**: SqlDataSource veritabanına bir Proxy işlevi görür


> [!NOTE]
> Bu öğreticide veritabanından veri alınırken odaklanacağız. İçinde [ekleme, güncelleştirme ve SqlDataSource denetimi ile verilerde siliniyor](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md) Öğreticisi, biz SqlDataSource ekleme, güncelleştirme ve silme destekleyecek şekilde yapılandırma konusunda göreceksiniz.


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>SqlDataSource ve AccessDataSource denetimleri

SqlDataSource denetimi ek olarak, ASP.NET 2.0 AccessDataSource denetimi de içerir. Bu iki farklı denetimler, birçok geliştiricinin ASP.NET 2. 0'ı AccessDataSource denetimi yalnızca Microsoft SQL Server ile çalışmak üzere tasarlanmış SqlDataSource denetimi ile özel olarak Microsoft Access ile çalışmak için tasarlandığını yükselmiyor yeni müşteri adayı. SqlDataSource denetimi ile AccessDataSource özellikle Microsoft Access ile çalışmak için tasarlanmış olsa da çalışır *herhangi* .NET erişilebilen bir ilişkisel veritabanı. Bu, tüm OleDb veya ODBC uyumlu gibi veri depolarında, Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL ve PostgreSQL, birçok diğerlerinin yanı sıra içerir.

AccessDataSource ve SqlDataSource denetimleri arasındaki tek fark, veritabanı bağlantı bilgilerini nasıl belirtildiğine ' dir. Access veritabanı dosyası yalnızca dosya yoluna AccessDataSource denetim gerekir. SqlDataSource Öte yandan, bir tam bağlantı dizesi gerektirir.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>1. adım: SqlDataSource Web sayfaları oluşturma

SqlDataSource denetimi kullanarak doğrudan veritabanını verilerle çalışmak nasıl araştırma başlamadan önce ilk ASP.NET sayfaları ve bu öğreticinin sonraki üç yapmamız gereken bizim Web sitesi projesi oluşturmak için bir dakikanızı ayırarak s olanak tanır. Başlangıç adlı yeni bir klasör ekleyerek `SqlDataSource`. Ardından, o klasördeki her bir sayfayla ilişkilendirilecek emin olmak için aşağıdaki ASP.NET sayfaları ekleyin `Site.master` ana sayfa:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![SqlDataSource ile ilgili öğreticiler için ASP.NET sayfaları ekleme](querying-data-with-the-sqldatasource-control-vb/_static/image3.gif)

**Şekil 3**: SqlDataSource ile ilgili öğreticiler için ASP.NET sayfaları ekleme


Diğer klasörler gibi `Default.aspx` içinde `SqlDataSource` klasörü kendi bölümünde öğreticileri listeler. Bu geri çağırma `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu nedenle, bu kullanıcı denetimine ekleme `Default.aspx` sayfaya s Tasarım görünümü Çözüm Gezgini'nde sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](querying-data-with-the-sqldatasource-control-vb/_static/image5.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image4.gif)

**Şekil 4**: ekleme `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](querying-data-with-the-sqldatasource-control-vb/_static/image6.gif))


Son olarak, girişleri olarak bu dört sayfalar ekleme `Web.sitemap` dosya. Özellikle, aşağıdaki biçimlendirme DataList ve Repeater ekleme özel düğmeler sonra eklemeniz `<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample1.sql)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticiler Web sitesini görüntülemek için bir dakikanızı ayırın. Sol taraftaki menüden, artık düzenleme, ekleme ve silme öğreticiler için öğeleri içerir.


![Site Haritası girişleri SqlDataSource öğreticileri için artık içerir.](querying-data-with-the-sqldatasource-control-vb/_static/image7.gif)

**Şekil 5**: Site Haritası girişleri SqlDataSource öğreticileri için artık içerir.


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>2. adım: Ekleme ve SqlDataSource denetimi yapılandırma

Başlangıç açarak `Querying.aspx` sayfasını `SqlDataSource` klasörü ve Tasarım görünümüne geç. SqlDataSource denetimi kümesi ve Tasarımcısı araç kutusundan sürükleyin, `ID` için `ProductsDataSource`. SqlDataSource ObjectDataSource olduğu gibi işlenmiş herhangi bir çıktı üretmez ve bu nedenle tasarım yüzeyinde gri bir kutu olarak görünür. SqlDataSource s akıllı etiket yapılandırmak veri kaynağı bağlantısından SqlDataSource yapılandırmak için tıklayın.


![Tıklayarak veri kaynağı bağlantısından SqlDataSource s akıllı etiket yapılandırma](querying-data-with-the-sqldatasource-control-vb/_static/image8.gif)

**Şekil 6**: tıklayarak veri kaynağı bağlantısından SqlDataSource s akıllı etiket yapılandırma


Bu SqlDataSource denetimi s veri kaynağı Yapılandırma Sihirbazı ' getirir. Sihirbaz s adımlarını ObjectDataSource Denetimi s farklılık gösterir, ancak son hedef ayrıntıları alma, ekleme, güncelleştirme ve veri kaynağı aracılığıyla verileri silmek sağlamak için aynıdır. SqlDataSource için bunu kullanmak için temel alınan veritabanı belirtme ve geçici SQL deyimlerinin veya saklı yordamların sağlama kapsar.

Sihirbaz ilk adımı veritabanı için bize ister. Açılır listede bulunan web uygulaması s'te bu veritabanlarını içerir `App_Data` klasörü ve Sunucu Gezgininde veri bağlantıları için eklenmiştir. Biz bu yana zaten bir bağlantı dizesi için eklediyseniz `NORTHWIND.MDF` veritabanını `App_Data` Projemizin s klasörüne `Web.config` dosyası, aşağı açılan listesi, bu bağlantı dizesi, bir başvuru içerir `NORTHWINDConnectionString`. Aşağı açılan listeden bu öğeyi seçin ve İleri'ye tıklayın.


![Aşağı açılan listeden NORTHWINDConnectionString seçin](querying-data-with-the-sqldatasource-control-vb/_static/image9.gif)

**Şekil 7**: seçin `NORTHWINDConnectionString` aşağı açılan listeden


Veritabanını seçtikten sonra sihirbaz verileri döndürmek sorgu için sorar. Biz, sütunlar bir tablo veya Görünüm döndürmek özel bir SQL deyimi girin veya bir saklı yordam belirtin ya da belirtebilirsiniz. Bu seçeneği belirtin aracılığıyla özel bir SQL deyimi veya saklı yordam ve bir tablodaki sütunların belirtin geçiş veya radyo düğmeleri görüntüleyin.

> [!NOTE]
> Bu ilk örnekte, s belirtin sütunlar bir tablo veya Görünüm seçeneğini kullanın olanak tanır. Biz, bu öğreticide daha sonra sihirbaza dönmek ve özel bir SQL deyimi veya saklı yordam seçeneği belirtin keşfedin.


Belirtin sütunlar bir tablo veya Görünüm radyo düğmesi seçili Şekil 8 Select deyimini ekran yapılandırma gösterilir. Açılır listede, tablolar ve görünümler ile seçilen tablo veya Görünüm s sütunları onay kutusu listesinde görüntülenen Northwind veritabanındaki kümesi içerir. Bu örnekte, let s dönüş `ProductID`, `ProductName`, ve `UnitPrice` sütunlarından `Products` tablo. Sihirbazın bu seçim yapmadan elde edilen SQL deyimi görüntüledikten sonra Şekil 8 gösterildiği gibi `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.


![Ürünleri tablo verilerini döndürür](querying-data-with-the-sqldatasource-control-vb/_static/image10.gif)

**Şekil 8**: dönüş verileri kaynağı `Products` tablo


Döndürülecek Sihirbazı'nı yapılandırdıktan sonra `ProductID`, `ProductName`, ve `UnitPrice` sütunlarından `Products` tablo, İleri düğmesine tıklayın. Bu son ekran önceki adımda yapılandırılmış sorgu sonuçlarını incelemek için bir fırsat sağlar. Test sorgusu düğmeye tıklandığında yürütür yapılandırılmış `SELECT` deyimi ve sonuçları bir kılavuz içinde görüntüler.


![SELECT sorgunuz gözden geçirmek için Test sorgu düğmesine tıklayın](querying-data-with-the-sqldatasource-control-vb/_static/image11.gif)

**Şekil 9**: gözden geçirilmesi gereken Test sorgu düğmesine tıklayın, `SELECT` sorgu


Sihirbazı tamamlamak için Son'u tıklatın.

ObjectDataSource ile SqlDataSource s Sihirbazı'nı yalnızca değerleri denetim s, yani özelliklerine gibi [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) ve [ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) özellikleri. Sihirbazı tamamladıktan sonra SqlDataSource denetimi s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample2.aspx)]

`ConnectionString` Özelliği, veritabanına bağlanma hakkında bilgi sağlar. Bu özellik, bir tam, sabit kodlanmış bağlantı dizesi değeri atanabilir veya bir bağlantı dizesine işaret edebilir `Web.config`. Web.config dosyasında bir bağlantı dizesi değerindeki başvurmak için söz dizimini kullanın `<%$ expressionPrefix:expressionValue %>`. Genellikle, *expressionPrefix* ConnectionStrings olduğu ve *expressionValue* adı bağlantı dizesinde `Web.config` [ `<connectionStrings>` bölümü](https://msdn.microsoft.com/library/bf7sd233.aspx). Ancak, söz dizimi kullanılabilir başvurusuna `<appSettings>` öğeleri veya içerik kaynak dosyalarından. Bkz: [ASP.NET ifadeleri genel bakış](https://msdn.microsoft.com/library/d5bd1tad.aspx) bu söz dizimi hakkında daha fazla bilgi için.

`SelectCommand` Geçici SQL deyiminin veya saklı yordam verileri döndürmek için yürütülecek özelliği belirtir.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>3. adım: veri Web denetim ekleme ve SqlDataSource için bağlama

SqlDataSource yapılandırıldıktan sonra bir veri GridView veya DetailsView gibi Web denetimi bağlanabilir. Bu öğreticide, verileri görüntülemek GridView s olanak tanır. Araç kutusundan GridView sayfaya sürükleyin ve öğeyi `ProductsDataSource` veri kaynağı GridView s akıllı etiket aşağı açılan listeden seçerek SqlDataSource.


[![SqlDataSource denetimi bağlamak ve GridView Ekle](querying-data-with-the-sqldatasource-control-vb/_static/image13.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image12.gif)

**Şekil 10**: GridView ekleyin ve SqlDataSource denetimi bağlamak ([tam boyutlu görüntüyü görmek için tıklatın](querying-data-with-the-sqldatasource-control-vb/_static/image14.gif))


Önceden SqlDataSource denetimi, aşağı açılan listeden GridView s akıllı etiket seçildikten sonra Visual Studio otomatik olarak BoundField veya CheckBoxField GridView'a her veri kaynak denetimi tarafından döndürülecek olan sütunları ekler. SqlDataSource üç veritabanı sütunları döndürdüğünden `ProductID`, `ProductName`, ve `UnitPrice` GridView içinde üç alan vardır.

GridView s üç yapılandırmak için birkaç dakikanızı BoundFields. Değişiklik `ProductName` alan s `HeaderText` özelliğini ürün adına ve `UnitPrice` alan s fiyatı. Ayrıca `UnitPrice` bir para birimi olarak alan. Bu değişiklikleri yaptıktan sonra GridView s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample3.aspx)]

Bir tarayıcı aracılığıyla bu sayfasını ziyaret edin. Şekil 11 gösterildiği gibi her ürün s GridView listeler `ProductID`, `ProductName`, ve `UnitPrice` değerleri.


[![GridView her ürün s ProductID, ProductName ve UnitPrice değerleri görüntüler.](querying-data-with-the-sqldatasource-control-vb/_static/image16.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image15.gif)

**Şekil 11**: GridView görüntüler her ürün s `ProductID`, `ProductName`, ve `UnitPrice` değerleri ([tam boyutlu görüntüyü görmek için tıklatın](querying-data-with-the-sqldatasource-control-vb/_static/image17.gif))


Sayfayı ziyaret edildiğinde, veri kaynağı denetimi s GridView çağırır `Select()` yöntemi. Kullandığımız ObjectDataSource Denetimi, bu adı `ProductsBLL` s sınıfı `GetProducts()` yöntemi. SqlDataSource, ancak ile `Select()` yöntemi sorunları ve belirtilen veritabanı bir bağlantı kurar `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, bu örnekte). SqlDataSource GridView ardından, döndürülen her veritabanı kaydı için GridView satır oluşturma numaralandırır sonuçlarını döndürür.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Yerleşik veri Web denetimi özellikleri ve SqlDataSource denetimi

Genel olarak, disk belleği, sıralama, düzenleme, Web denetimleri verilere devralınan özellikler silme, ekleme ve benzeri veri Web denetimi için özeldir ve kullanılan veri kaynak denetimine bağımlı değildir. Diğer bir deyişle, GridView disk belleği, sıralama, düzenleme ve bir ObjectDataSource veya bir SqlDataSource bağlı olup olmadığını silme yerleşik kullanabilir. Ancak, bazı verileri Web denetimi kullanılan veri kaynak denetimi veya veri kaynağı denetimi s yapılandırmasını duyarlıdır.

Örneğin, [verimli bir şekilde sayfalama aracılığıyla büyük miktarda veri](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) nasıl, varsayılan olarak, Web veri sayfalama mantığını naively döndürür denetimleri ele almıştık öğretici *tüm* temeldeki kayıtları veri kaynağı ve yalnızca uygun alt kayıtları verilen geçerli sayfa dizini ve sayfa başına görüntülenecek kayıt sayısını görüntüler. Bu model yeterince büyük sonuç kümeleri aracılığıyla sayfalama son derece verimsizdir. Neyse ki, ObjectDataSource yalnızca kesin alt görüntülenecek kayıt döndüren özel disk belleği destekleyecek şekilde yapılandırılabilir. SqlDataSource denetimi, ancak özel disk belleği uygulamak için özellikler eksiktir.

Sayfalama ve sıralama ile başka bir subtlety SqlDataSource ile ortaya çıkar. Varsayılan olarak, bir SqlDataSource döndürülen veriler disk belleğine alınan veya GridView sıralanır. Bunu göstermek için denetimi etkinleştirme sayfalama ve sıralamayı etkinleştir seçenekleri GridView s akıllı etiketinde `Querying.aspx` ve beklendiği gibi çalıştığını doğrulayın.

SqlDataSource geniş yazılmış bir veri kümesine ilişkin Veritabanı verilerinin getireceğinden sayfalama ve sıralama çalışır. Toplam disk belleği uygulamak için temel bir yönü sorgu tarafından döndürülen kayıt sayısını veri kümesinden saptanabilen. Ayrıca, veri kümesi s sonuçları bir DataView ile sıralanabilir. GridView istekleri disk belleğine alınan ya da veriler bu özellikler otomatik olarak SqlDataSource tarafından kullanılır.

SqlDataSource değiştirerek bir veri kümesi yerine bir DataReader döndürmek için yapılandırılabilir, [ `DataSourceMode` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) gelen `DataSet` (varsayılan) için `DataReader`. DataReader kullanarak durumlarda SqlDataSource s sonuçları DataReader için mevcut kod geçerken tercih edilen. Ayrıca, veri kümeleri daha önemli ölçüde daha basit nesneler DataReaders olduğundan, daha iyi performans sunar. Bu değişiklik yaparsanız, ancak veri Web denetimi diğerinden sıralayabilirsiniz veya SqlDataSource kaç tane kaydın sorgu tarafından döndürülen öğrenmenize de DataReader çalışmaz olduğundan sayfa döndürülen verileri sıralamak için herhangi bir teknikleri sunar.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>4. adım: özel SQL deyimini kullanarak veya saklı yordam

SqlDataSource denetimi yapılandırırken, veri döndürmek için kullanılan sorgu iki yaklaşımdan birini özel bir SQL deyimi veya saklı yordam veya varolan bir tablo veya Görünüm sütunları olarak belirtilebilir. Biz incelenirken 2. adımda sütunları seçerek `Products` tablo. Özel bir SQL deyimi kullanarak ara s olanak tanır.

Başka bir GridView denetimi için ekleme `Querying.aspx` sayfasında ve akıllı etiket aşağı açılan listeden yeni bir veri kaynağı oluşturmayı seçin. Ardından, belirtmek veriler bir veritabanından bu çekilir, yeni bir SqlDataSource denetimi oluşturur. Denetim adı `ProductsWithCategoryInfoDataSource`.


![ProductsWithCategoryInfoDataSource adlı yeni bir SqlDataSource denetimi oluşturma](querying-data-with-the-sqldatasource-control-vb/_static/image18.gif)

**Şekil 12**: adlı yeni bir SqlDataSource denetimi oluşturma `ProductsWithCategoryInfoDataSource`


Sonraki ekranda bize veritabanını belirtmek için sorar. Şekil 7'de yaptığımız gibi seçin `NORTHWINDConnectionString` açılır listeden listesinde ve İleri'ye tıklayın. Select deyimi ekran yapılandırma, belirle özel bir SQL deyimi veya saklı yordam radyo düğmesini seçin ve İleri'ye tıklayın. Bu sekme seçin, UPDATE, INSERT ve DELETE etiketli sunan özel deyimleri tanımlayın veya saklı yordamlar ekranını, getirir. Her sekmede TextBox'a özel bir SQL deyimi girin veya açılır listeden bir saklı yordam seçin. Bu öğreticide özel bir SQL ifadesi girerek görünecektir; sonraki öğreticide, bir saklı yordam kullanan bir örnek içerir.


![Özel bir SQL deyimi girin ya da bir saklı yordam seçin](querying-data-with-the-sqldatasource-control-vb/_static/image19.gif)

**Şekil 13**: özel SQL deyimi girin ya da bir saklı yordam seçin


Özel bir SQL deyimi TextBox'a el ile girilebilir veya grafik Sorgu Tasarımcısı düğmesine tıklayarak oluşturulabilir. Sorgu Oluşturucu veya metin kutusu, döndürmek için aşağıdaki sorguyu kullanın `ProductID` ve `ProductName` alanlarını `Products` kullanarak tablo bir `JOIN` s ürün almak için `CategoryName` gelen `Categories` tablosu:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample4.sql)]


![Grafik sorgu kullanarak Sorgu Oluşturucusu oluşturabilirsiniz](querying-data-with-the-sqldatasource-control-vb/_static/image20.gif)

**Şekil 14**: Sorgu tasarımcısını kullanarak sorgu grafik oluşturmak için


Sorgu belirttikten sonra Test sorgusu ekrana devam etmek için İleri'ye tıklayın. SqlDataSource Sihirbazı'nı tamamlamak için Son'u tıklatın.

Sihirbazı tamamladıktan sonra GridView eklenmiş görüntüleme üç BoundFields olacaktır `ProductID`, `ProductName`, ve `CategoryName` sorgusundan döndürülen ve bildirim temelli aşağıdaki biçimlendirmede kaynaklanan sütunlar:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample5.aspx)]


[![GridView her ürün s Kimliğini, adını ve ilişkili kategori adını gösterir](querying-data-with-the-sqldatasource-control-vb/_static/image22.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image21.gif)

**Şekil 15**: GridView gösterir her ürün kimliği, adı ve kategori adı ilişkili ([tam boyutlu görüntüyü görmek için tıklatın](querying-data-with-the-sqldatasource-control-vb/_static/image23.gif))


## <a name="summary"></a>Özet

Bu öğreticide sorgulamak ve SqlDataSource denetimi kullanarak verileri görüntülemek nasıl gördük. ObjectDataSource gibi SqlDataSource verilere erişmek için bildirim temelli bir yaklaşım sağlayan bir proxy işlevi görür. Bağlanmak için veritabanı ve SQL özelliklerini belirtin `SELECT` yürütülecek sorgu; veri kaynağı Yapılandırma Sihirbazı'nı kullanarak veya Özellikler penceresi aracılığıyla belirtilebilir.

`SELECT` Biz Bu öğreticide incelenirken sorgu örnekleri tüm kayıtlar belirtilen sorgusundan döndürülen. SqlDataSource denetimi, ancak dahil edebilirsiniz bir `WHERE` yan tümcesiyle parametre değerlerini programlı olarak atanan veya belirli bir kaynaktan otomatik olarak alınır. Oluşturma ve sonraki öğreticide parametreli sorgular kullanma inceleyeceğiz!

Mutlu programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [İlişkisel veritabanı verilerine erişme](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [SqlDataSource denetimine genel bakış](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [ASP.NET hızlı başlangıç öğreticileri: SqlDataSource denetimi](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Web.config `<connectionStrings>` öğesi](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Veritabanı bağlantı dizesi başvurusu](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Susan Connery Bernadette Leigh ve David Suru yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
> [İleri](using-parameterized-queries-with-the-sqldatasource-vb.md)
