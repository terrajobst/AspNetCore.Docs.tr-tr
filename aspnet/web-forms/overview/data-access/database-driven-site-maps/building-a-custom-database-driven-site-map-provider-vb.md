---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
title: Bir özel veritabanı odaklı Site haritası sağlayıcısı (VB) oluşturma | Microsoft Docs
author: rick-anderson
description: ASP.NET 2.0 varsayılan site haritası sağlayıcısı statik bir XML dosyasından verilerini alır. XML-tabanlı sağlayıcı birçok küçük ve orta boyut için uygun olsa da...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: f904cd2c-a408-4484-9324-8b8d7fe33893
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
msc.type: authoredcontent
ms.openlocfilehash: 8e041a5a9163c7f9fe55c6aa06f35301cbdb48a8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393976"
---
<a name="building-a-custom-database-driven-site-map-provider-vb"></a>(VB) bir özel veritabanı odaklı Site haritası sağlayıcısı oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_VB.zip) veya [PDF olarak indirin](building-a-custom-database-driven-site-map-provider-vb/_static/datatutorial62vb1.pdf)

> ASP.NET 2.0 varsayılan site haritası sağlayıcısı statik bir XML dosyasından verilerini alır. XML-tabanlı sağlayıcı birçok küçük ve orta ölçekli Web sitelerine uygun olsa da, daha büyük Web uygulamalarını daha dinamik site haritası gerektirir. Bu öğreticide, verileri iş mantığı katmanından alır bir özel site haritası sağlayıcısı oluşturacağız hangi sırayla veritabanından veri alır.


## <a name="introduction"></a>Giriş

ASP.NET 2.0 s site haritası özelliği gibi bir XML dosyasındaki bir web uygulaması s site haritası bazı kalıcı ortamda tanımlamak bir sayfa Geliştirici sağlar. Tanımlandıktan sonra site haritası verileri program aracılığıyla aracılığıyla erişilebilen [ `SiteMap` sınıfı](https://msdn.microsoft.com/library/system.web.sitemap.aspx) içinde [ `System.Web` ad alanı](https://msdn.microsoft.com/library/system.web.aspx) veya gezinti çeşitli aracılığıyla Web denetimleri gibi SiteMapPath, menü ve TreeView denetimleri. Site haritası sisteminin kullandığı [sağlayıcı modeli](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) böylece farklı site haritası serileştirme uygulamaları oluşturulabilir ve bir web uygulamasına takılı. ASP.NET 2.0 ile birlikte gelen varsayılan site haritası sağlayıcısı, site haritası yapısı bir XML dosyasında devam ettirir. Geri [ana sayfalar ve Site gezintisi](../introduction/master-pages-and-site-navigation-vb.md) adlı bir dosya oluşturduğumuz öğretici `Web.sitemap` bu yapı bulunan ve kendi XML her yeni Öğretici bölüm ile güncelleştirme.

Site haritası s yapısı için bu öğreticileri gibi oldukça statik ise de, varsayılan XML tabanlı site haritası sağlayıcısı çalışır. Birçok senaryoda, ancak daha dinamik site haritası gereklidir. Şekil 1, her bir kategoriye ve ürün nerede görüneceğini olarak bölümlerde Web sitesi s yapısını gösterilen site haritası göz önünde bulundurun. Bu site haritası ile kök düğümüne karşılık gelen web sayfasını ziyaret ederek tüm kategorileri belirli ürün s web sayfası görüntüleme, ürün s ayrıntıları gösterir ve belirli kategori s web sayfasını ziyaret ederek bu kategori s ürünler listesi listesi.


[![Kategorilerini ve ürünler düzenini Site Haritası s yapısı](building-a-custom-database-driven-site-map-provider-vb/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image1.png)

**Şekil 1**: kategorilerini ve ürünler düzenini Site Haritası s yapısı ([tam boyutlu görüntüyü görmek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image2.png))


Bu kategori ve ürün tabanlı yapı içinde sabit kodlanmış olabilir ancak `Web.sitemap` dosya, dosyayı her seferinde bir kategori güncelleştirilmesi gerekir veya ürün eklenen, kaldırıldı veya yeniden adlandırılamaz. Yapısını veritabanından veya ideal olarak, uygulama s mimarinin iş mantığı katmanı alındı, sonuç olarak, site haritası Bakımı önemli ölçüde basitleştirilmiş. Bu şekilde, ürünler ve kategoriler eklenmiş olarak yeniden adlandırılması veya silinmesi, site haritası otomatik olarak bu değişiklikleri yansıtacak şekilde güncelleştirmeniz gerekir.

ASP.NET 2.0 s site haritası serileştirme sağlayıcı modeli kurulu olduğundan, kendi veri mimarisi ve veritabanı gibi bir alternatif bir veri deposundan Dallarınızla kendi özel site haritası sağlayıcısı oluşturabiliriz. Bu öğreticide BLL verileri alır, özel bir sağlayıcı oluşturacağız. Let s başlayın!

> [!NOTE]
> Bu öğreticide oluşturulan özel site haritası sağlayıcısı, uygulama s mimarisi ve veri modelini sıkı şekilde bağlı. Jeff Prosise s [depolama, SQL Server'daki Site Haritaları](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) ve [önceden beklemekte SQL Site haritası sağlayıcısı](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) SQL Server'da site haritası verileri depolamak için genelleştirilmiş bir yaklaşım makaleleri inceleyin.


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>1. adım: özel Site haritası sağlayıcısı Web sayfaları oluşturma

Özel site haritası sağlayıcısı oluşturma başlamadan önce ilk yapmamız Bu öğretici için gereken ASP.NET sayfaları ekleyin s olanak tanır. Başlangıç adlı yeni bir klasör ekleyerek `SiteMapProvider`. Ardından, o klasördeki her bir sayfayla ilişkilendirilecek emin olmak için aşağıdaki ASP.NET sayfaları ekleyin `Site.master` ana sayfa:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Ayrıca bir `CustomProviders` alt `App_Code` klasör.


![Site haritası sağlayıcısı ile ilgili öğreticiler için ASP.NET sayfaları ekleme](building-a-custom-database-driven-site-map-provider-vb/_static/image2.gif)

**Şekil 2**: Site için ASP.NET sayfaları sağlayıcısı ile ilgili öğreticiler eşleme Ekle


Bu bölümün yalnızca bir öğretici olduğundan, şu t gerek ki `Default.aspx` s bölümü öğreticiler listesi. Bunun yerine, `Default.aspx` kategorilerini GridView denetiminde görüntüler. Biz bu 2. adımda üstesinden.

Ardından, güncelleştirme `Web.sitemap` bir başvuru eklemek için `Default.aspx` sayfası. Özellikle, aşağıdaki biçimlendirme sonra önbelleğe alma ekleme `<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample1.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticiler Web sitesini görüntülemek için bir dakikanızı ayırın. Sol taraftaki menüden, artık tek site haritası sağlayıcısı öğretici için bir öğe içeriyor.


![Site Haritası artık Site haritası sağlayıcısı öğretici için bir giriş içerir](building-a-custom-database-driven-site-map-provider-vb/_static/image3.gif)

**Şekil 3**: Site Haritası artık Site haritası sağlayıcısı öğretici için bir giriş içerir


Bu öğretici s odaklandığı bir özel site haritası sağlayıcısı oluşturma ve bu sağlayıcıyı kullanmak için bir web uygulaması yapılandırma göstermek sağlamaktır. Şekil 1'de gösterildiği gibi bir kök düğümü her kategori ve ürünü için bir düğüm ile birlikte içeren bir site haritası döndüren bir sağlayıcı özellikle oluşturacağız. Genel olarak, her site haritası düğümünde bir URL belirtebilir. Bizim site haritası için kök düğümü s URL'si olacaktır `~/SiteMapProvider/Default.aspx`, hangi listeler tüm kategorilerin veritabanını. Site haritası her kategori düğüme işaret eden bir URL gerekir `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, hangi listeler tüm ürünleri belirtilen *CategoryID*. Son olarak, her ürünün site haritası düğümü işaret edecek `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, belirli bir ürün s ayrıntılarını görüntüleyin.

Başlamak için oluşturmamız gerekir `Default.aspx`, `ProductsByCategory.aspx`, ve `ProductDetails.aspx` sayfaları. Bu sayfalar, sırasıyla 2, 3 ve 4. adımda tamamlanır. Bu tür bir çok sayfalı ana/ayrıntı raporları Bu öğreticinin thrust site haritası sağlayıcıları üzerinde olduğundan ve geçmiş öğreticiler oluşturma kapsamına olduğundan şu adımları 2'den 4'ten hurry. Birden fazla sayfayı dolduran ana/ayrıntı raporları oluşturma bilgilerinizi tazelemeniz gerekiyorsa kiracıurl [ana/ayrıntı filtreleme arasında iki sayfa](../masterdetail/master-detail-filtering-across-two-pages-vb.md) öğretici.

## <a name="step-2-displaying-a-list-of-categories"></a>2. adım: kategori listesi görüntüleme

Açık `Default.aspx` sayfasını `SiteMapProvider` klasörü ve ayar Tasarımcısı araç kutusundan sürükleyip GridView kendi `ID` için `Categories`. İsteğe bağlı olarak GridView s akıllı etiketten adlı yeni bir ObjectDataSource bağlama `CategoriesDataSource` ve verileri kullanarak alır şekilde yapılandırın `CategoriesBLL` s sınıfı `GetCategories` yöntemi. Bu GridView yalnızca kategorileri görüntüler ve veri değiştirme özelliklerini sağlamaz olduğundan, güncelleştirme, ekleme, açılan listeler ayarlayın ve sekme (hiçbiri) SİLİN.


[![ObjectDataSource GetCategories yöntemi kullanarak kategorileri döndürmek için yapılandırma](building-a-custom-database-driven-site-map-provider-vb/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image3.png)

**Şekil 4**: ObjectDataSource kategorileri kullanarak döndürmek için yapılandırma `GetCategories` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image4.png))


[![Güncelleştirme, ekleme, açılan listeler ayarlayın ve sekmeleri (hiçbiri) silme](building-a-custom-database-driven-site-map-provider-vb/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.png)

**Şekil 5**: aşağı açılan listeler güncelleştirme, ekleme ve silme sekmeler (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görmek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image6.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio için bir BoundField ekleyeceksiniz `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, ve `BrochurePath`. GridView yalnızca içeren Düzen `CategoryName` ve `Description` BoundFields ve güncelleştirme `CategoryName` BoundField s `HeaderText` kategoriye özelliği.

Ardından, bir HyperLinkField ekleyin ve bu nedenle konumlandırın BT'nin s en soldaki alan. Ayarlama `DataNavigateUrlFields` özelliğini `CategoryID` ve `DataNavigateUrlFormatString` özelliğini `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Ayarlama `Text` özelliğini ürünleri görüntüle.


![Bir HyperLinkField kategorileri GridView'a Ekle](building-a-custom-database-driven-site-map-provider-vb/_static/image6.gif)

**Şekil 6**: eklemek için bir HyperLinkField `Categories` GridView


ObjectDataSource oluşturma ve GridView s alanları özelleştirdikten sonra iki denetimi bildirim temelli biçimlendirmeyi aşağıdaki gibi görünür:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample2.aspx)]

Şekil 7 gösterir `Default.aspx` bir tarayıcıdan görüntülendiğinde. Bir kategori s ürünleri görüntüle tıklayarak bağlantı açılır `ProductsByCategory.aspx?CategoryID=categoryID`, adım 3'te oluşturulacak.


[![Bir görünüm ürünleri bağlantısı ile birlikte listelenen her kategorisi olan](building-a-custom-database-driven-site-map-provider-vb/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image7.png)

**Şekil 7**: her kategori olan bir görünüm ürünleri bağlantısı ile birlikte listelenen ([tam boyutlu görüntüyü görmek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>3. adım:, seçilen kategori s ürünleri listeleme

Açık `ProductsByCategory.aspx` sayfasında ve GridView adlandırma, ekleme `ProductsByCategory`. Akıllı, etiketten GridView adlı yeni bir ObjectDataSource bağlama `ProductsByCategoryDataSource`. ObjectDataSource kullanmak için yapılandırma `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi ve kümesi açılan listeler (hiçbiri) UPDATE, INSERT ve DELETE sekmeleri.


[![ProductsBLL sınıfı s GetProductsByCategoryID(categoryID) yöntemi kullanın](building-a-custom-database-driven-site-map-provider-vb/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image9.png)

**Şekil 8**: kullanım `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image10.png))


Veri Kaynağı Yapılandırma Sihirbazı'nda son adım için bir parametre kaynağı ister *CategoryID*. Bu bilgiler, sorgu dizesi alanı geçirilir beri `CategoryID`, sorgu dizesi aşağı açılan listeden seçin ve Şekil 9'da gösterildiği gibi CategoryID QueryStringField metin kutusuna girin. Sihirbazı tamamlamak için Son'u tıklatın.


[![' % S'CategoryID parametresi için CategoryID Querystring alanını kullan](building-a-custom-database-driven-site-map-provider-vb/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image11.png)

**Şekil 9**: kullanım `CategoryID` sorgu dizesi alanı *CategoryID* parametre ([tam boyutlu görüntüyü görmek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image12.png))


Sihirbazı tamamladıktan sonra Visual Studio karşılık gelen BoundFields ve bir CheckBoxField ürün veri alanları için GridView ekler. Kaldırma dışındaki tüm `ProductName`, `UnitPrice`, ve `SupplierName` BoundFields. Bu üç BoundFields özelleştirme `HeaderText` ürün, fiyat ve tedarikçi, sırasıyla okunacak özellik. Biçim `UnitPrice` BoundField bir para birimi olarak.

Ardından, bir HyperLinkField ekleyin ve en soldaki konuma taşıyın. Ayarlayın, `Text` ayrıntılarını görüntüleme özelliğine, `DataNavigateUrlFields` özelliğini `ProductID`ve kendi `DataNavigateUrlFormatString` özelliğini `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.


![ProductDetails.aspx için işaret eden bir görünümü Ayrıntılar HyperLinkField Ekle](building-a-custom-database-driven-site-map-provider-vb/_static/image10.gif)

**Şekil 10**: görünüm işaret HyperLinkField ayrıntıları ekleyin `ProductDetails.aspx`


Bu özelleştirme yapıldıktan sonra GridView ve ObjectDataSource s bildirim temelli biçimlendirme şuna benzemelidir:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample3.aspx)]

Dönüş görüntülemeye `Default.aspx` İçecekler için bağlantı üzerinden tarayıcı ve ürünleri Görüntüle'e tıklayın. Bu gideceksiniz `ProductsByCategory.aspx?CategoryID=1`, İçecekler kategorisine ait Northwind veritabanı adları, fiyatları ve ürünlerin tedarikçileri görüntüleme (bkz. Şekil 11). Kullanıcılar kategori listesi sayfasına geri dönmek için bir bağlantı eklemek için bu sayfayı geliştirebilir çekinmeyin (`Default.aspx`) ve Seçili kategoriyi s adı ve açıklamayı görüntüleyen bir DetailsView veya FormView denetimi.


[![İçecekler adları, fiyatları ve tedarikçileri görüntülenir](building-a-custom-database-driven-site-map-provider-vb/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image13.png)

**Şekil 11**: İçecekler adları, fiyatları ve tedarikçileri görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>4. adım: bir ürün s ayrıntıları gösterme

Son sayfa `ProductDetails.aspx`, seçili ürünlerin ayrıntılarını görüntüler. Açık `ProductDetails.aspx` ve bir DetailsView tasarımcı araç kutusundan sürükleyin. DetailsView s ayarlamak `ID` özelliğini `ProductInfo` ve temizleyin, `Height` ve `Width` özellik değerleri. Akıllı, etiketten DetailsView adlı yeni bir ObjectDataSource bağlama `ProductDataSource`, kendi verileri çekmek için ObjectDataSource yapılandırma `ProductsBLL` s sınıfı `GetProductByProductID(productID)` yöntemi. Önceki adımları 2 ve 3'te oluşturulan web sayfaları gibi güncelleştirme, ekleme, açılan listeler ayarlayın ve sekmeleri (hiçbiri) SİLİN.


[![ObjectDataSource GetProductByProductID(productID) yöntemi kullanmak üzere yapılandırma](building-a-custom-database-driven-site-map-provider-vb/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.png)

**Şekil 12**: ObjectDataSource kullanılacak yapılandırma `GetProductByProductID(productID)` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image16.png))


Veri Kaynağı Yapılandırma Sihirbazı'nın son adım kaynak için ister *ProductID* parametresi. Bu veriler sorgu dizesi alanı geldiğinden `ProductID`, aşağı açılan liste QueryString ProductID QueryStringField TextBox'a ayarlayın. Son olarak, Sihirbazı tamamlamak için Son düğmesini tıklatın.


[![ProductID parametre değerini ProductID sorgu dizesi alanı çekmek için yapılandırma](building-a-custom-database-driven-site-map-provider-vb/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image17.png)

**Şekil 13**: yapılandırma *ProductID* değerini çekmek için parametre `ProductID` sorgu dizesi alanı ([tam boyutlu görüntüyü görmek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image18.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio karşılık gelen BoundFields ve bir CheckBoxField ürün veri alanları için DetailsView oluşturacaktır. Kaldırma `ProductID`, `SupplierID`, ve `CategoryID` BoundFields ve gördüğünüz gibi geri kalan alanları yapılandırın. Birkaç estetik yapılandırmaları sonra my DetailsView ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdaki gibi görünüyordu:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample4.aspx)]

Bu sayfayı test etmek için iade `Default.aspx` ve İçecekler kategorisindeki için üzerinde ürünleri Görüntüle'ye tıklayın. İçecek ürünleri listeleme yapmaya Chai Çay için ayrıntıları görüntüle bağlantısına tıklayın. Bu gideceksiniz `ProductDetails.aspx?ProductID=1`, Ayrıntılar (bkz. Şekil 14) Chai Çay s gösterir.


[![Chai Çay s tedarikçi, kategori, fiyat ve diğer bilgileri görüntülenir](building-a-custom-database-driven-site-map-provider-vb/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image19.png)

**Şekil 14**: Chai Çay s tedarikçi, kategori, fiyat ve diğer bilgileri görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>5. adım: bir Site haritası sağlayıcısı iç işleyişini anlama

Site haritası web sunucusu s bellekte bir koleksiyonu olarak temsil edilen `SiteMapNode` örnekleriyle bir hiyerarşi oluşturur. Tam olarak bir kök olmalıdır, tüm kök olmayan düğüm tam olarak bir üst düğüme sahip olmalıdır ve tüm düğümleri tercihe bağlı sayıda alt öğeleri olabilir. Her `SiteMapNode` nesnesi bir bölümde bir Web sitesi s yapısını temsil eder; Bu bölümler yaygın olarak karşılık gelen bir web sayfası vardır. Sonuç olarak, [ `SiteMapNode` sınıfı](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) gibi özelliklere sahip `Title`, `Url`, ve `Description`, bölüm için bilgiler sağladığınız `SiteMapNode` temsil eder. Ayrıca bir `Key` her benzersiz olarak tanımlayan özellik `SiteMapNode` bu hiyerarşiyi'kurmak için kullanılan özellikleri yanı sıra hiyerarşi içinde `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`ve böyle devam eder.

Şekil 15, Şekil 1, ancak daha ayrıntılı olarak ince ince uygulama ayrıntılarını genel site haritası yapısı gösterilmektedir.


[![Her SiteMapNode özellikleri gibi başlık, Url, anahtar vb. vardır.](building-a-custom-database-driven-site-map-provider-vb/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.gif)

**Şekil 15**: her `SiteMapNode` özellikleri gibi sahip `Title`, `Url`, `Key`ve benzeri ([tam boyutlu görüntüyü görmek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image17.gif))


Site haritası aracılığıyla erişilebilir [ `SiteMap` sınıfı](https://msdn.microsoft.com/library/system.web.sitemap.aspx) içinde [ `System.Web` ad alanı](https://msdn.microsoft.com/library/system.web.aspx). Bu sınıf s `RootNode` özelliği site haritası s kök döndürüyor `SiteMapNode` örnek; `CurrentNode` döndürür `SiteMapNode` olan `Url` özelliği şu anda istenen sayfanın URL'sini eşleşir. Bu sınıf, ASP.NET 2.0 s Gezinti Web denetimleri tarafından dahili olarak kullanılır.

Zaman `SiteMap` s sınıfı özellikleri erişilen, bunu kalıcı bazı Orta site haritası yapısından belleğe seri gerekir. Ancak, site haritası serileştirme mantığı uygulamasına kodlanmış sabit değil `SiteMap` sınıfı. Bunun yerine, çalışma zamanında `SiteMap` sınıfı belirleyen hangi site haritası *sağlayıcısı* seri hale getirme için kullanılacak. Varsayılan olarak, [ `XmlSiteMapProvider` sınıfı](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) kullanılır, doğru biçimlendirilmiş bir XML dosyasından site haritası s yapısı okur. Ancak, biraz iş ile kendi özel site haritası sağlayıcısı oluşturabiliriz.

Tüm site haritası sağlayıcıları nesnesinden türetilmesi [ `SiteMapProvider` sınıfı](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), gerekli yöntemleri içerir ve site için gereken özellikleri harita sağlayıcıları, ancak çoğu uygulama ayrıntılarını atlar. İkinci bir sınıf [ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), genişletir `SiteMapProvider` sınıfı ve gerekli işlevi daha sağlam bir uygulamasını içerir. Dahili olarak `StaticSiteMapProvider` depolar `SiteMapNode` site örneklerini harita içinde bir `Hashtable` ve yöntemleri ister sağlar `AddNode(child, parent)`, `RemoveNode(siteMapNode),` ve `Clear()` ekleme ve kaldırma `SiteMapNode` s iç `Hashtable`. `XmlSiteMapProvider` türetilen `StaticSiteMapProvider`.

Ne zaman bir özel site haritası sağlayıcısı oluşturma genişletir `StaticSiteMapProvider`, geçersiz kılınması, soyut iki yöntem vardır: [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) ve [ `GetRootNodeCore` ](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, adından da anlaşılacağı gibi kalıcı depolama alanından site haritası yapısı yükleniyor ve bellekte oluşturmak için sorumludur. `GetRootNodeCore` kök düğümü site eşlemesinde döndürür.

Önce bir web uygulaması uygulama s yapılandırmasında kaydedilmelidir site haritası sağlayıcısı kullanabilirsiniz. Varsayılan olarak, `XmlSiteMapProvider` sınıfı adıyla kayıtlı `AspNetXmlSiteMapProvider`. Ek site haritası sağlayıcıları kaydetmek için aşağıdaki işaretlemede ekleme `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample5.xml)]

*Adı* değer sağlayıcı okunabilir bir ad atar *türü* site haritası sağlayıcısı tam olarak nitelenmiş tür adını belirtir. Somut değerlerini şunları keşfedeceğiz *adı* ve *türü* değerleri adım 7'de, sonra ve oluşturduğumuz bizim Özel site haritası sağlayıcısı.

Site haritası sağlayıcısı sınıfı, erişilen ilk kez örneği `SiteMap` sınıfı ve ömrü web uygulaması için bellekte kalır. Birden çok, eşzamanlı web sitesi ziyaretçilerden çağırılabilirler site haritası sağlayıcısı yalnızca bir örneği olduğundan, s sağlayıcısı yöntemleri olmasını zorunlu *iş parçacığı açısından güvenli*.

Performans ve ölçeklenebilirlik, onu s biz bellek içi sitenin önbelleğe önemli yapısı ve her seferinde yeniden yerine yapısı bu önbelleğe dönüş `BuildSiteMap` yöntemi çağrılır. `BuildSiteMap` Her kullanıcı için sayfa ve site haritası yapısı derinliğini kullanımda Gezinti denetimlere bağlı olarak, sayfa istek başına birden çok kez çağrılabilir. Biz site haritası yapısında önbelleğe alma, her durumda, `BuildSiteMap` çağrıldığında her zaman biz (bu sorguda, veritabanına neden olur) mimarisi ürün ve kategori bilgilerini yeniden almaya gerekir. Önbelleğe alınmış verileri önbelleğe alma önceki öğreticilerdeki ele aldığımız gibi eski haline gelebilir. Bu sorunla başa çıkmak için zaman veya SQL önbellek bağımlılık tabanlı expiries kullanabiliriz.

> [!NOTE]
> İsteğe bağlı olarak bir site haritası sağlayıcısı geçersiz kılabilir [ `Initialize` yöntemi](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` site haritası sağlayıcısı ilk örneği ve sağlayıcı atanmış herhangi bir özel özniteliği geçirilen çağrılır `Web.config` içinde `<add>` öğesi gibi: `<add name="name" type="type" customAttribute="value" />`. Sağlayıcı s kodu değiştirmek zorunda kalmadan, çeşitli site haritası sağlayıcısı ile ilgili ayarları belirtmek bir sayfa Geliştirici izin vermek istiyorsanız yararlıdır. Biz kategorisi ve products verilerine doğrudan başlangıcı yerine sonundan veritabanı mimarisi d biz aracılığıyla büyük olasılıkla okuma Örneğin, veritabanı bağlantı dizesi aracılığıyla belirtin sayfasında Geliştirici izin vermek istiyorsanız `Web.config` sabit kodlanmış kullanmak yerine Sağlayıcı s kodu değeri. Ekleriz adım 6'da özel site haritası sağlayıcısı bu kılmaz `Initialize` yöntemi. Kullanma örneği için `Initialize` yöntemi başvurmak [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [depolama, SQL Server'daki Site Haritaları](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) makalesi.


## <a name="step-6-creating-the-custom-site-map-provider"></a>6. adım: özel Site haritası sağlayıcısı oluşturma

Kategorileri ve ürünleri Northwind veritabanındaki site haritası oluşturan bir özel site haritası sağlayıcısı oluşturma için genişleten bir sınıfı oluşturmak ihtiyacımız `StaticSiteMapProvider`. Adım 1'ı eklemek için sorulan bir `CustomProviders` klasöründe `App_Code` klasörü - yeni bir sınıf adlı bu klasöre ekleyin `NorthwindSiteMapProvider`. Aşağıdaki kodu ekleyin `NorthwindSiteMapProvider` sınıfı:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample6.vb)]

Bu sınıf s keşfe s izin `BuildSiteMap` yöntemi ile başlayan bir [ `lock` deyimi](https://msdn.microsoft.com/library/c5kehkcz.aspx). `lock` Deyimi yalnızca sağlayan bir iş parçacığı girmek için bir defada böylece kendi kod erişim seri hale getirme ve başka bir s parmaklarının adımlamasını iki eş zamanlı iş parçacığı engelleme.

Sınıf düzeyi `SiteMapNode` değişkeni `root` site haritası yapısı önbelleğe alınması amacıyla kullanılır. Site haritası ilk kez ya da temel alınan verileri değiştirildikten sonra ilk kez oluşturulduğunda `root` olacaktır `Nothing` ve site haritası yapısı oluşturulur. Site haritası s kök düğümüne atanır `root` sonraki bu yöntem, yapım sırasında işlem çağrılırken, `root` değişmeyecek `Nothing`. Sonuç olarak, sürece `root` değil `Nothing` yeniden oluşturmak zorunda kalmadan, site haritası yapısı çağırana döndürülür.

Kök ise `Nothing`, ürün ve kategori bilgileri site haritası yapısı oluşturulur. Site haritası oluşturarak yerleşik `SiteMapNode` örnekleri ve ardından yapılan çağrılar aracılığıyla hiyerarşisi oluşturan `StaticSiteMapProvider` s sınıfı `AddNode` yöntemi. `AddNode` çeşitli depolama iç muhasebe gerçekleştirir `SiteMapNode` içinde örnekler bir `Hashtable`. Hiyerarşi oluşturma başlamadan önce arayarak Başlat `Clear` iç öğeleri kullanıma temizler yöntemi `Hashtable`. Ardından, `ProductsBLL` s sınıfı `GetProducts` yöntemi ve ortaya çıkan `ProductsDataTable` yerel değişkenlerle depolanır.

Site haritası s yapım kök düğümü oluşturarak ve giderek başlar `root`. Aşırı yüklemesini [ `SiteMapNode` s Oluşturucusu](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) burada ve bu boyunca kullanılan `BuildSiteMap` aşağıdaki bilgileri geçirilir:

- Site haritası sağlayıcısı başvuru (`Me`).
- `SiteMapNode` s `Key`. Bu değer, her biri için benzersiz olmalıdır, gerekli `SiteMapNode`.
- `SiteMapNode` s `Url`. `Url` isteğe bağlıdır, ancak sağlanırsa, her `SiteMapNode` s `Url` değeri benzersiz olmalıdır.
- `SiteMapNode` s `Title`, gerekli olduğu.

`AddNode(root)` Yöntem çağrısının ekler `SiteMapNode` `root` kök olarak site haritası için. Ardından, her `ProductRow` içinde `ProductsDataTable` numaralandırılmış alan şeklinde. Varsa zaten bir `SiteMapNode` geçerli s ürün kategorisi için başvuru. Aksi takdirde, yeni bir `SiteMapNode` kategorisi oluşturuldu ve bir alt öğesi olarak eklenen `SiteMapNode``root` aracılığıyla `AddNode(categoryNode, root)` yöntem çağrısı. Uygun kategoriyi sonra `SiteMapNode` düğüm bulunamadı veya oluşturduğunuz bir `SiteMapNode` geçerli ürün için oluşturulur ve alt kategorisi olarak eklenen `SiteMapNode` aracılığıyla `AddNode(productNode, categoryNode)`. Unutmayın kategori `SiteMapNode` s `Url` özellik değeri `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` while ürün `SiteMapNode` s `Url` özelliği atandığı `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Bir veritabanına sahip bu ürünlerin `NULL` değerini kendi `CategoryID` kategorisi altında gruplandırılır `SiteMapNode` olan `Title` özelliği yok ve özelliği ayarlandığında `Url` özelliği boş dize olarak ayarlayın. Ayarlama karar `Url` beri boş bir dize `ProductBLL` s sınıfı `GetProductsByCategory(categoryID)` yöntemi yalnızca ürünleriyle döndürülecek yeteneği şu anda eksik bir `NULL` `CategoryID` değeri. Ayrıca, nasıl gezinti denetimlerini işlemeye göstermek istediğiniz bir `SiteMapNode` için bir değer eksik kendi `Url` özelliği. Bu öğreticide genişletmek için öneriyoruz böylece hiçbiri `SiteMapNode` s `Url` özelliği işaret `ProductsByCategory.aspx`, henüz ürünlerle yalnızca görüntüler `NULL` `CategoryID` değerleri.


Site haritası oluşturduktan sonra rastgele bir nesne üzerinde bir SQL önbellek bağımlılık kullanarak veri önbelleğini eklenir `Categories` ve `Products` aracılığıyla tablolar bir `AggregateCacheDependency` nesne. Önceki öğreticide, SQL önbellek bağımlılıklarını kullanma incelediniz *SQL önbellek bağımlılıklarını kullanma*. Özel site haritası sağlayıcısı, ancak veri önbelleğini s kullanmaktadır `Insert` yöntemi olduğumuz ve henüz keşfetmek için. Bu aşırı yüklemesi, nesneyi önbellekten kaldırıldığında çağrılan bir temsilciyi son giriş parametresi olarak kabul eder. Özellikle, size yeni bir geçirmek [ `CacheItemRemovedCallback` temsilci](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) işaret eden `OnSiteMapChanged` yöntemi aşağıya içinde tanımlanan `NorthwindSiteMapProvider` sınıfı.

> [!NOTE]
> Site haritası bellek içi temsili sınıf düzeyi değişkenleri önbelleğe alınmış `root`. Web uygulamasında bu örneğe tüm iş parçacıkları arasında paylaşılan olduğundan ve özel site haritası sağlayıcısı sınıfı yalnızca bir örneği olduğundan, bu sınıf değişken bir önbellek olarak görev yapar. `BuildSiteMap` Yöntemi de kullanan veri önbelleğini temel alınan veritabanı verileri bildirim almak için yalnızca bir yol olarak `Categories` veya `Products` tabloları değişiklikler. Veri önbelleğe koyulur değeri yalnızca geçerli tarih ve saat olduğuna dikkat edin. Gerçek site haritası veri *değil* veri önbelleğindeki yerleştirin.


`BuildSiteMap` Yöntemi tamamlandığında site haritası kök düğümü döndürerek.

Kalan yöntemler oldukça basittir. `GetRootNodeCore` kök düğümü döndürmekten sorumludur. Bu yana `BuildSiteMap` kök dizinini döndürür `GetRootNodeCore` yalnızca döndürür `BuildSiteMap` s değeri döndürür. `OnSiteMapChanged` Yöntemi kümeleri `root` geri `Nothing` önbellek öğesinin kaldırıldığında. Kök döndürülmek ile `Nothing`, sonraki açışınızda `BuildSiteMap` olan çağrılır, site haritası yapısı yeniden oluşturulacak. Son olarak, `CachedDate` özelliği bu tür bir değer varsa veri önbelleğinde depolanan tarih ve saat değerini döndürür. Bu özellik sayfasında geliştirici tarafından site haritası verileri en son ne zaman önbelleğe ilişkili belirlemek için kullanılabilir.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>7. adım: Kaydetme`NorthwindSiteMapProvider`

Kullanılacak web uygulamamız için sırayla `NorthwindSiteMapProvider` site haritası sağlayıcısı, adım 6'da oluşturulan ihtiyacımız içine kaydetmek `<siteMap>` bölümünü `Web.config`. Özellikle, aşağıdaki biçimlendirme içinde eklemeniz `<system.web>` öğesinde `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample7.xml)]

Bu işaretleme şu iki işlemi yapar: ilk olarak, gösteren yerleşik `AspNetXmlSiteMapProvider` varsayılan site haritası sağlayıcısı; ikinci olarak, bu adım 6'da oluşturulan Northwind İnsan kolay adı ile özel site haritası sağlayıcısı olarak kaydeder.

> [!NOTE]
> Uygulama s'te bulunan site haritası sağlayıcıları için `App_Code` klasörü, değerini `type` özniteliktir yalnızca sınıf adı. Alternatif olarak, özel site haritası sağlayıcısı ayrı bir sınıf kitaplığı projesi ile web uygulaması s'te yerleştirilen derlenmiş bütünleştirilmiş kodu oluşturulmuş `/Bin` dizin. Bu durumda, `type` öznitelik değeri olacak *Namespace*. *ClassName*, *AssemblyName* .


Güncelleştirdikten sonra `Web.config`, herhangi bir sayfadan öğreticileri bir tarayıcıda görüntülemek için bir dakikanızı ayırın. Sol taraftaki gezinti arabirimi bölümleri görüntülenmeye devam eder ve öğreticiler tanımlanan unutmayın `Web.sitemap`. Kaldığımız olmasıdır `AspNetXmlSiteMapProvider` varsayılan sağlayıcı olarak. Kullanan bir gezinti kullanıcı arabirimi öğesi oluşturmak için `NorthwindSiteMapProvider`, size Northwind site haritası sağlayıcısı kullanılması gerektiğini açıkça belirtmeniz gerekir. Adım 8'de bunun nasıl göreceğiz.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>8. adım: özel Site haritası sağlayıcısı'nı kullanarak Site Haritası bilgilerini görüntüleme

Özel site haritası sağlayıcısı oluşturulur ve kayıtlı `Web.config`, biz Gezinti denetimler eklemek için hazır re `Default.aspx`, `ProductsByCategory.aspx`, ve `ProductDetails.aspx` içinde sayfa `SiteMapProvider` klasör. Başlangıç açarak `Default.aspx` sürükleyin ve sayfa bir `SiteMapPath` tasarımcı araç kutusundan. Araç Kutusu Gezinti bölümde SiteMapPath denetimi bulunur.


[![Bir SiteMapPath için Default.aspx Ekle](building-a-custom-database-driven-site-map-provider-vb/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image18.gif)

**Şekil 16**: eklemek için bir SiteMapPath `Default.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image20.gif))


Site haritası içindeki geçerli sayfa s konumunu belirten bir içerik haritası SiteMapPath denetimi görüntüler. Bir SiteMapPath ana sayfaya dön ekledik geri *ana sayfalar ve Site gezintisi* öğretici.

Bir tarayıcı aracılığıyla bu sayfayı görüntülemek için bir dakikanızı ayırın. Şekil 16 eklenen SiteMapPath kendi verileri çekerek varsayılan site haritası sağlayıcısı kullanan `Web.sitemap`. Bu nedenle, içerik haritası giriş gösterir &gt; sağ üst köşedeki içerik haritası gibi Site Haritası özelleştirme.


[![İçerik haritası varsayılan Site haritası sağlayıcısı kullanır.](building-a-custom-database-driven-site-map-provider-vb/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.gif)

**Şekil 17**: içerik haritası varsayılan Site haritası sağlayıcısı kullanır ([tam boyutlu görüntüyü görmek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image23.gif))


Adım 6'da oluşturduğumuz özel site haritası sağlayıcısı kullanan Şekil 16 eklenen SiteMapPath olacak şekilde ayarlanmış kendi [ `SiteMapProvider` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) Northwind için ad size atanan `NorthwindSiteMapProvider` içinde `Web.config`. Ne yazık ki, Tasarımcı varsayılan site haritası sağlayıcısı kullanmaya devam eder, ancak bu özellik değişikliğini yaptıktan sonra bir tarayıcı aracılığıyla sayfasını ziyaret edin, içerik haritası artık özel site haritası sağlayıcısı kullandığını görürsünüz.


[![İçerik haritası özel Site haritası sağlayıcısı NorthwindSiteMapProvider artık kullanır.](building-a-custom-database-driven-site-map-provider-vb/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image24.gif)

**Şekil 18**: içerik haritası artık özel Site haritası sağlayıcısı kullanıyor `NorthwindSiteMapProvider` ([tam boyutlu görüntüyü görmek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image26.gif))


SiteMapPath denetimi daha işlevsel bir kullanıcı arabiriminde görüntüler `ProductsByCategory.aspx` ve `ProductDetails.aspx` sayfaları. Bir SiteMapPath ayarı bu sayfalara ekleme `SiteMapProvider` Northwind hem de bir özellik. Gelen `Default.aspx` İçecekler ürünleri görüntüle bağlantısına ve sonra Chai Çay için ayrıntıları görüntüle bağlantısına tıklayın. Şekil 19 gösterildiği gibi geçerli site eşlemesi bölümü (Chai Çay) ve alt öğelerinden içerik haritası içerir: İçecekler ve tüm kategorileri.


[![İçerik haritası özel Site haritası sağlayıcısı NorthwindSiteMapProvider artık kullanır.](building-a-custom-database-driven-site-map-provider-vb/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.png)

**Şekil 19**: içerik haritası artık özel Site haritası sağlayıcısı kullanıyor `NorthwindSiteMapProvider` ([tam boyutlu görüntüyü görmek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image22.png))


Menü ve TreeView denetimleri gibi SiteMapPath yanı sıra başka gezinme kullanıcı arabirimi öğeleri kullanılabilir. `Default.aspx`, `ProductsByCategory.aspx`, Ve `ProductDetails.aspx` Bu öğretici için örneğin, indirme sayfaları tüm içerir (bkz. Şekil 20) menü denetimleri. Bkz: [İnceleme ASP.NET 2.0 s Site gezintisi özelliklerinde](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) ve [kullanarak Site Gezinti denetimlerinin](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) bölümünü [ASP.NET 2.0 hızlı Başlangıçlar](https://quickstarts.asp.net/QuickStartv20/aspnet/) daha derinlemesine göz atmak için Gezinti denetimlerinin ve site sistemi ASP.NET 2.0 eşleyin.


[![Menü denetimi her kategorisi ve ürünleri listeler](building-a-custom-database-driven-site-map-provider-vb/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image28.gif)

**Şekil 20**: menü denetimi listeler her kategorilerini ve ürünler ([tam boyutlu görüntüyü görmek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image30.gif))


Bu öğreticide daha önce belirtildiği gibi site haritası yapısı ile programlı olarak erişilebilir `SiteMap` sınıfı. Aşağıdaki kod kök dizinini döndürür `SiteMapNode` varsayılan sağlayıcı:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample8.vb)]

Bu yana `AspNetXmlSiteMapProvider` varsayılan sağlayıcıdır uygulamamız için yukarıdaki kod içinde tanımlanan kök düğümü döndürecekti `Web.sitemap`. Site haritası sağlayıcısı varsayılan dışındaki başvurmak için kullanma `SiteMap` s sınıfı [ `Providers` özelliği](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) şu şekilde:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample9.vb)]

Burada *adı* özel site haritası sağlayıcısı'nı (web uygulamamız için Northwind) adıdır.

Bir site haritası sağlayıcısı için belirli bir üyesine erişmek için kullanmanız `SiteMap.Providers["name"]` sağlayıcı örneğini almak ve ardından uygun türe dönüştürme. Örneğin, görüntülenecek `NorthwindSiteMapProvider` s `CachedDate` özelliği bir ASP.NET sayfasında, aşağıdaki kodu kullanın:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample10.vb)]

> [!NOTE]
> SQL önbellek bağımlılık özelliği test etmeyi unutmayın. Ziyaret sonra `Default.aspx`, `ProductsByCategory.aspx`, ve `ProductDetails.aspx` sayfaları, düzenleme, ekleme ve bölüm silme öğreticilerde birine gidin ve bir kategori veya ürün adını düzenleyin. Ardından sayfalarında birine iade `SiteMapProvider` klasör. Yoklama mekanizması, temel alınan veritabanına değişiklik gerektirdiğini belirtmek için yeterli zaman geçtiğini varsayarak, site haritası yeni ürün veya kategori adını gösterecek şekilde güncelleştirilmesi gerekir.


## <a name="summary"></a>Özet

ASP.NET 2.0 s site haritası özellikleri içeren bir `SiteMap` sınıfı, bir dizi yerleşik gezinti Web denetler ve site haritası bilgileri bekliyor. bir varsayılan site haritası sağlayıcısı bir XML dosyasına kalıcı. Site Haritası bilgileri başka bir kaynaktan gibi bir veritabanı, uygulama s mimarisi ya da bir özel site haritası sağlayıcısı oluşturma ihtiyacımız uzak bir Web hizmeti kullanmak için. Bu, doğrudan veya dolaylı olarak türeyen bir sınıf oluşturulmasını ilgilendirir gelen `SiteMapProvider` sınıfı.

Bu öğreticide uygulama mimariden culled ürün ve kategori bilgileri site haritası temel alan özel bir site haritası sağlayıcısı oluşturma gördük. Genişletilmiş bizim sağlayıcısı `StaticSiteMapProvider` sınıfı ve entailed oluşturma bir `BuildSiteMap` veri alınan yöntemi site haritası hiyerarşisi oluşturulur ve elde edilen sınıf düzeyi değişkenleri yapı önbelleğe alınmış. SQL önbellek bağımlılık ile bir geri çağırma işlevini önbelleğe alınan geçersiz kılmak için kullandığımız ne zaman yapısı temel alınan `Categories` veya `Products` değiştirilmiş verileri.

Mutlu programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [SQL Server'da Site Haritaları depolamak](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) ve [önceden beklemekte SQL Site haritası sağlayıcısı](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [ASP.NET 2.0 göz s sağlayıcı modeli](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Sağlayıcı Araç Seti](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [ASP.NET 2.0 İnceleme s Site Gezinti özellikleri](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Dave Gardner, Zack Jones, Teresa Murphy ve Bernadette Leigh yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](building-a-custom-database-driven-site-map-provider-cs.md)
