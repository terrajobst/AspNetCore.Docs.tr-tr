---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
title: Özel veritabanı kaynaklı Site haritası sağlayıcısı (VB) oluşturma | Microsoft Docs
author: rick-anderson
description: Varsayılan site haritası sağlayıcısı, ASP.NET 2.0 statik bir XML dosyasından verisini alır. XML tabanlı sağlayıcı çok sayıda küçük ve orta boyut uygun olmasına rağmen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: f904cd2c-a408-4484-9324-8b8d7fe33893
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
msc.type: authoredcontent
ms.openlocfilehash: df295f1b8bf0b83647ffb90501936181894634d7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877976"
---
<a name="building-a-custom-database-driven-site-map-provider-vb"></a>Özel veritabanı kaynaklı Site haritası sağlayıcısı (VB) oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_VB.zip) veya [PDF indirin](building-a-custom-database-driven-site-map-provider-vb/_static/datatutorial62vb1.pdf)

> Varsayılan site haritası sağlayıcısı, ASP.NET 2.0 statik bir XML dosyasından verisini alır. XML tabanlı sağlayıcısı için çok sayıda küçük ve orta ölçekli Web siteleri uygun olsa da, büyük Web uygulamalarını daha dinamik site haritası gerektirir. İş mantığı katmanından verisini alır özel site haritası sağlayıcısı oluşturmak, Bu öğretici kapsamında, hangi sırayla veritabanından veri alır.


## <a name="introduction"></a>Giriş

ASP.NET 2.0 s site haritası özelliği bir XML dosyası gibi bir web uygulaması s site haritası bazı kalıcı ortamda tanımlamak bir sayfa Geliştirici sağlar. Bir kez tanımlandıktan sonra site haritası verileri programlı olarak aracılığıyla erişilebilen [ `SiteMap` sınıfı](https://msdn.microsoft.com/library/system.web.sitemap.aspx) içinde [ `System.Web` ad alanı](https://msdn.microsoft.com/library/system.web.aspx) veya gezinti çeşitli Web denetimleri gibi SiteMapPath, menü ve TreeView denetimleri. Site eşlemesi sistemini kullanan [sağlayıcı modeli](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) böylece farklı site haritası serileştirme uygulamaları oluşturulabilir ve bir web uygulamasına takılı. ASP.NET 2.0 ile birlikte gelen varsayılan site haritası sağlayıcısı, site haritası yapısı bir XML dosyasında devam ettirir. Geri [ana sayfalar ve Site gezintisi](../introduction/master-pages-and-site-navigation-vb.md) adlı bir dosya oluşturduğumuz öğretici `Web.sitemap` bu yapı yer alan ve onun XML her yeni bir öğretici bölümle güncelleştirme.

İyi site haritası s yapısı bu öğreticileri için oldukça, gibi statikse varsayılan XML tabanlı site haritası sağlayıcısı çalışır. Birçok senaryolarda, ancak daha dinamik site haritası gereklidir. Şekil 1, her kategori ve ürün nerede görüneceğini Web sitesi s yapısı bölümlerde olarak gösterilen site haritası göz önünde bulundurun. Bu site haritası, kök düğüme karşılık gelen web sayfasını ziyaret tüm kategorileri, belirli kategori s web sayfasını ziyaret bu kategori s ürünleri listeler ve belirli ürün s web sayfası görüntüleme, ürün s ayrıntıları gösterir ancak listesi.


[![Kategorileri ve ürünleri oluşma şekli Site Haritası s yapısı](building-a-custom-database-driven-site-map-provider-vb/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image1.png)

**Şekil 1**: kategorileri ve ürünleri oluşma şekli Site Haritası s yapısı ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image2.png))


Bu kategori ve ürün tabanlı yapı içine sabit kodlanmış olabilir ancak `Web.sitemap` dosya, dosyayı her zaman bir kategori güncellenmesi gerekiyor veya ürün eklenemez, kaldırılamaz veya yeniden adlandırılamaz. Yapısını veritabanından ya da ideal olarak, iş mantığı katmanından uygulama s mimarisinin alındığında, sonuç olarak, site haritası Bakımı büyük ölçüde basitleştirilmiş. Böylece, ürünler ve kategoriler eklenen gibi yeniden adlandırılmış veya silinmiş, site haritası otomatik olarak bu değişiklikleri yansıtacak şekilde güncelleştirmeniz gerekir.

ASP.NET 2.0 s site haritası serileştirme sağlayıcı modeli üzerinde kurulu olduğundan, veritabanı veya mimarisi gibi bir alternatif veri deposundaki verileri alan kendi özel site haritası sağlayıcısı oluşturabilir. Bu öğreticide BLL verileri alır özel bir sağlayıcı biz yapı. Let s başlayın!

> [!NOTE]
> Bu öğreticide oluşturduğunuz özel site haritası sağlayıcısı uygulama s mimarisi ve veri modeline sıkı şekilde bağlı. Jeff Prosise s [SQL Server'daki Site Haritaları depolama](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) ve [SQL Site haritası sağlayıcısı önceden beklediğinden](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) makaleleri SQL Server'da site haritası verileri depolamak için genelleştirilmiş bir yaklaşım inceleyin.


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>1. adım: özel Site haritası sağlayıcısı Web sayfaları oluşturma

Özel site haritası sağlayıcısı oluşturmaya başlamadan önce öncelikle Bu öğretici için yapmamız gereken ASP.NET sayfaları ekleme s olanak tanır. Adlı yeni bir klasör eklemeye başlayın `SiteMapProvider`. Ardından, o klasördeki her sayfayla ilişkilendirilecek emin olmak için aşağıdaki ASP.NET sayfaları ekleme `Site.master` ana sayfa:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Ayrıca bir `CustomProviders` alt klasöre `App_Code` klasör.


![Site haritası sağlayıcısı ile ilgili öğreticiler için ASP.NET sayfaları ekleme](building-a-custom-database-driven-site-map-provider-vb/_static/image2.gif)

**Şekil 2**: Site için ASP.NET sayfaları sağlayıcısı ile ilgili öğreticiler eşleme Ekle


Bu bölüm için yalnızca bir Öğreticisi olduğundan, biz t gerek güncelleştireceğinizi `Default.aspx` s bölümü öğreticiler listesi. Bunun yerine, `Default.aspx` kategorileri GridView denetiminde görüntülenir. Biz bu 2. adımda üstesinden gelmek.

Ardından, güncelleştirme `Web.sitemap` bir başvuru eklemek için `Default.aspx` sayfası. Özellikle, aşağıdaki biçimlendirme sonra önbelleğe alma ekleme `<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample1.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticileri Web sitesini görüntülemek için bir dakikanızı ayırın. Soldaki menüde artık tek site haritası Sağlayıcısı Öğreticisi için bir öğe içeriyor.


![Site Haritası artık Site Haritası Sağlayıcısı Öğreticisi için bir giriş içerir](building-a-custom-database-driven-site-map-provider-vb/_static/image3.gif)

**Şekil 3**: Site Haritası artık Site Haritası Sağlayıcısı Öğreticisi için bir giriş içerir


Bu öğretici s ana odak özel site haritası sağlayıcısı oluşturma ve bu sağlayıcıyı kullanmak için bir web uygulaması yapılandırma göstermektir. Özellikle, biz Şekil 1'de gösterildiği gibi her kategori hem de ürün için bir düğüm birlikte bir kök düğümü içeren site haritası döndüren bir sağlayıcı yapı. Genel olarak, site haritası içindeki her bir düğümün bir URL belirtebilirsiniz. Bizim site haritası için kök düğümü s URL olacaktır `~/SiteMapProvider/Default.aspx`, hangi listeler veritabanındaki kategorilerin tümünü. Her kategori düğümü site haritası işaret eden bir URL gerekir `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, hangi listeler tüm ürünleri belirtilen *adlı kullanıcı, Categoryıd'si*. Son olarak, her ürünün site haritası düğümü işaret edecek `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, belirli ürün s ayrıntıları görüntülenir.

Başlatmak için oluşturmamız gerekir `Default.aspx`, `ProductsByCategory.aspx`, ve `ProductDetails.aspx` sayfaları. Bu sayfaları adım 2, 3 ve 4 ' sırasıyla tamamlanır. Bu tür sayfalı ana/ayrıntı raporları Bu öğreticinin thrust site haritası sağlayıcıları olduğundan ve geçmiş öğreticiler oluşturma ele aldıktan sonra şu adımları 2'den 4'ten hurry. Birden fazla sayfayı dolduran ana/ayrıntı raporları oluşturma Yenileyici gerekiyorsa, geri bakın [ana/ayrıntı filtreleme arasında iki sayfa](../masterdetail/master-detail-filtering-across-two-pages-vb.md) Öğreticisi.

## <a name="step-2-displaying-a-list-of-categories"></a>2. adım: Kategoriler listesini görüntüleme

Açık `Default.aspx` sayfasındaki `SiteMapProvider` klasörü ve sürükleme GridView ayarı tasarımcıya araç kendi `ID` için `Categories`. İsteğe bağlı olarak GridView s akıllı etiketten adlı yeni bir ObjectDataSource bağlamak `CategoriesDataSource` ve verileri kullanarak alır şekilde yapılandırın `CategoriesBLL` s sınıfı `GetCategories` yöntemi. Bu GridView yalnızca kategorileri görüntüler ve verileri değiştirme olanağı sağlamadığını olduğundan, güncelleştirme, ekleme, açılan listeleri ayarlayın ve sekmeleri (hiçbiri) SİLİN.


[![ObjectDataSource GetCategories yöntemi kullanarak kategorileri döndürmek için yapılandırma](building-a-custom-database-driven-site-map-provider-vb/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image3.png)

**Şekil 4**: ObjectDataSource kategorileri kullanarak dönmek için yapılandırma `GetCategories` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image4.png))


[![Güncelleştirme, ekleme, açılan listeleri ayarlayın ve sekmeleri (hiçbiri) silme](building-a-custom-database-driven-site-map-provider-vb/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.png)

**Şekil 5**: aşağı açılan listeler güncelleştirme, ekleme ve silme sekmeler (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image6.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio için BoundField ekleyecektir `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, ve `BrochurePath`. GridView yalnızca içerecek şekilde düzenleyin `CategoryName` ve `Description` BoundFields ve güncelleştirme `CategoryName` BoundField s `HeaderText` kategorisine özelliği.

Ardından, bir HyperLinkField ekleyin ve bu nedenle getirin, BT'nin s en solundaki alan. Ayarlama `DataNavigateUrlFields` özelliğine `CategoryID` ve `DataNavigateUrlFormatString` özelliğine `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Ayarlama `Text` özelliğini ürünleri görüntüleyin.


![Kategoriler GridView bir HyperLinkField ekleyin](building-a-custom-database-driven-site-map-provider-vb/_static/image6.gif)

**Şekil 6**: bir HyperLinkField eklemek `Categories` GridView


ObjectDataSource oluşturup GridView s alanlarını özelleştirme sonra iki denetimi bildirim temelli biçimlendirme aşağıdaki gibi görünür:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample2.aspx)]

Şekil 7 gösterir `Default.aspx` bir tarayıcıdan görüntülendiğinde. Bir kategori s ürünleri görüntüleyin tıklamak bağlantıyı size götürür `ProductsByCategory.aspx?CategoryID=categoryID`, hangi adım 3'te oluşturulmasını sağlar.


[![Bir görünümü ürünleri bağlantısı ile boyunca listelenen her kategori olduğu](building-a-custom-database-driven-site-map-provider-vb/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image7.png)

**Şekil 7**: her kategorisidir listelenen boyunca bir görünüm ürünleri bağlantıyla ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>3. adım: seçilen kategori s ürünleri listeleme

Açık `ProductsByCategory.aspx` sayfasında ve adlandırma GridView, ekleme `ProductsByCategory`. Akıllı etiketten GridView adlı yeni bir ObjectDataSource bağlama `ProductsByCategoryDataSource`. ObjectDataSource kullanmak için yapılandırma `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi ve kümesi aşağı açılan listeler (hiçbiri) güncelleştirme, ekleme ve silme sekmeleri.


[![ProductsBLL sınıfı s GetProductsByCategoryID(categoryID) yöntemi kullanın.](building-a-custom-database-driven-site-map-provider-vb/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image9.png)

**Şekil 8**: kullanım `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image10.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı son adımda ister bir parametre kaynağına ilişkin *adlı kullanıcı, Categoryıd'si*. Bu bilgiler sorgu dizesi alanı geçirilen beri `CategoryID`, sorgu dizesi aşağı açılan listeden seçin ve Şekil 9'da gösterildiği gibi adlı kullanıcı, Categoryıd'si QueryStringField metin kutusuna girin. Sihirbazı tamamlamak için Son'u tıklatın.


[![Adlı kullanıcı, Categoryıd'si parametresi için adlı kullanıcı, Categoryıd'si Querystring alanını kullan](building-a-custom-database-driven-site-map-provider-vb/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image11.png)

**Şekil 9**: kullanım `CategoryID` sorgu dizesi alanı için *adlı kullanıcı, Categoryıd'si* parametre ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image12.png))


Sihirbazı tamamladıktan sonra Visual Studio karşılık gelen BoundFields ve bir CheckBoxField ürün veri alanları GridView ekleyeceksiniz. Kaldırma dışındaki tüm `ProductName`, `UnitPrice`, ve `SupplierName` BoundFields. Bu üç BoundFields özelleştirme `HeaderText` ürün, fiyat ve tedarikçi, sırasıyla okumak için özellikler. Biçim `UnitPrice` BoundField bir para birimi olarak.

Ardından, bir HyperLinkField ekleyin ve en solundaki konuma taşıyın. Ayarlama, `Text` ayrıntılarını görüntüleme özelliğini kendi `DataNavigateUrlFields` özelliğine `ProductID`ve kendi `DataNavigateUrlFormatString` özelliğine `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.


![İçin ProductDetails.aspx gösteren bir görünüm ayrıntıları HyperLinkField Ekle](building-a-custom-database-driven-site-map-provider-vb/_static/image10.gif)

**Şekil 10**: bir görünüm ayrıntıları işaret HyperLinkField Ekle `ProductDetails.aspx`


Bu özelleştirmeler yaptıktan sonra GridView ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdakine benzemelidir:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample3.aspx)]

Dönüş görüntülemeye `Default.aspx` Meşrubat için bağlantı aracılığıyla bir tarayıcı ve ürünleri görüntüleyin'ı tıklatın. Bu, olanak sürecek `ProductsByCategory.aspx?CategoryID=1`, Meşrubat kategorisine ait Northwind veritabanı adları, Fiyatlar ve ürünleri tedarikçileri görüntüleme (bkz. Şekil 11). Daha fazla kullanıcı kategorisi listesi sayfasına dönmek için bir bağlantı eklemek için bu sayfayı geliştirmek çekinmeyin (`Default.aspx`) ve seçilen kategori s ad ve açıklama görüntüleyen bir DetailsView veya FormView denetim.


[![Meşrubat adları, Fiyatlar ve Üreticiler görüntülenir](building-a-custom-database-driven-site-map-provider-vb/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image13.png)

**Şekil 11**: Meşrubat adları, Fiyatlar ve Üreticiler görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>4. adım: bir ürün s ayrıntıları gösterme

Son sayfa `ProductDetails.aspx`, seçilen ürün ayrıntılarını görüntüler. Açık `ProductDetails.aspx` ve bir DetailsView tasarımcıya araç çubuğuna sürükleyin. DetailsView s ayarlamak `ID` özelliğine `ProductInfo` ve temizleyin, `Height` ve `Width` özellik değerleri. Akıllı etiketten DetailsView adlı yeni bir ObjectDataSource bağlamak `ProductDataSource`, kendi verisinden çekmesini ObjectDataSource yapılandırma `ProductsBLL` s sınıfı `GetProductByProductID(productID)` yöntemi. Önceki web sayfalarıyla adımları 2 ve 3'te oluşturulan gibi güncelleştirme, ekleme, açılan listeleri ayarlayın ve sekmeleri (hiçbiri) SİLİN.


[![ObjectDataSource GetProductByProductID(productID) yöntemi kullanmak üzere yapılandırma](building-a-custom-database-driven-site-map-provider-vb/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.png)

**Şekil 12**: ObjectDataSource kullanılacak yapılandırma `GetProductByProductID(productID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image16.png))


Veri Kaynağı Yapılandırma Sihirbazı'nın son adımı ister kaynağı için *ProductID* parametresi. Bu veriler sorgu dizesi alanı gelir beri `ProductID`, aşağı açılan liste QueryString ve ProductID için QueryStringField textbox olarak ayarlayın. Son olarak, Sihirbazı tamamlamak için Son'u tıklatın.


[![Parametre değerini ProductID Querystring alanından çekme ProductID yapılandırın](building-a-custom-database-driven-site-map-provider-vb/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image17.png)

**Şekil 13**: yapılandırmak *ProductID* değerinden çekmesini parametresi `ProductID` Querystring alan ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image18.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio karşılık gelen BoundFields ve bir CheckBoxField ürün veri alanları için DetailsView'da oluşturur. Kaldırma `ProductID`, `SupplierID`, ve `CategoryID` BoundFields ve geri kalan alanları uygun gördüğünüz şekilde yapılandırın. My DetailsView ve ObjectDataSource s bildirim temelli biçimlendirmesi estetik yapılandırmaları sayıda sonra aşağıdaki gibi Aranan:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample4.aspx)]

Bu sayfayı test etmek için dönmek `Default.aspx` ve görünüm ürünleri İçecekler kategorisini tıklatın. Listenin içecek ürünleri gelen Chai Çay ayrıntılarını görüntüle bağlantısını tıklayın. Bu, olanak sürecek `ProductDetails.aspx?ProductID=1`, (bkz. Şekil 14) ayrıntıları Chai Çay s gösterir.


[![Chai Çay s tedarikçi, kategori, fiyat ve diğer bilgileri görüntülenir](building-a-custom-database-driven-site-map-provider-vb/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image19.png)

**Şekil 14**: Chai Çay s tedarikçi, kategori, fiyat ve diğer bilgileri görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>5. adım: bir Site haritası sağlayıcısı çalışmalar anlama

Site haritasını web sunucusu s bellekte koleksiyonu olarak temsil edilir `SiteMapNode` bir hiyerarşi form örnekleri. Tam olarak bir kök olmalıdır, tüm kök olmayan düğüm tam olarak bir üst düğüme sahip olmalıdır ve tüm düğümleri alt rastgele bir sayı olabilir. Her `SiteMapNode` nesnesi, Web sitesi s yapısında bölümüne temsil eder; Bu bölümleri yaygın olarak karşılık gelen bir web sayfası vardır. Sonuç olarak, [ `SiteMapNode` sınıfı](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) gibi özelliklere sahip `Title`, `Url`, ve `Description`, bölüm için bilgiler sağladığınız `SiteMapNode` temsil eder. Ayrıca bir `Key` her benzersiz olarak tanımlayan özellik `SiteMapNode` Bu hiyerarşi kurmak için kullanılan özellikleri yanı sıra hiyerarşi içinde `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`, vb.

Şekil 15 genel site haritası yapısı Şekil 1'den ancak ince ayrıntılı olarak ince ince uygulama ayrıntılarını gösterir.


[![Her SiteMapNode özellikler gibi başlık, Url, anahtar vb. vardır](building-a-custom-database-driven-site-map-provider-vb/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.gif)

**Şekil 15**: her `SiteMapNode` özellikler gibi sahip `Title`, `Url`, `Key`ve benzeri ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image17.gif))


Site haritasını üzerinden erişilebilir [ `SiteMap` sınıfı](https://msdn.microsoft.com/library/system.web.sitemap.aspx) içinde [ `System.Web` ad alanı](https://msdn.microsoft.com/library/system.web.aspx). Bu sınıf s `RootNode` özelliği döndürür site haritası s kök `SiteMapNode` örnek; `CurrentNode` döndürür `SiteMapNode` , `Url` özelliği şu anda istenen sayfanın URL'sini eşleşir. Bu sınıf, ASP.NET 2.0 s Gezinti Web denetimleri tarafından dahili olarak kullanılır.

Zaman `SiteMap` s sınıfı özellikleri erişilir, onu bazı kalıcı Orta site haritası yapısından belleğe seri gerekir. Ancak, site haritası serileştirme mantığı uygulamasına kodlanmış sabit değil `SiteMap` sınıfı. Bunun yerine, çalışma zamanında `SiteMap` sınıfı belirler hangi site haritası *sağlayıcı* seri hale getirme için kullanılacak. Varsayılan olarak, [ `XmlSiteMapProvider` sınıfı](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) kullanılır, doğru biçimlendirilmiş XML dosyasından site haritası s yapısı okur. Ancak, biraz iş ile kendi özel site haritası sağlayıcısı oluşturabilir.

Tüm site haritası sağlayıcıları öğesinden türetilmelidir [ `SiteMapProvider` sınıfı](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), temel yöntemleri içerir ve site için gereken özellikleri eşlemek sağlayıcıları, ancak çoğu uygulama ayrıntılarını atlar. İkinci sınıfı [ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), genişletir `SiteMapProvider` sınıfı ve gerekli işlevselliği, daha sağlam bir uygulama içerir. Dahili olarak, `StaticSiteMapProvider` depolar `SiteMapNode` site örneklerini harita bir `Hashtable` ve yöntemleri ister sağlar `AddNode(child, parent)`, `RemoveNode(siteMapNode),` ve `Clear()` ekleme ve kaldırma `SiteMapNode` iç s `Hashtable`. `XmlSiteMapProvider` türetilmiş `StaticSiteMapProvider`.

Özel site haritası sağlayıcısı oluşturma zaman genişletir `StaticSiteMapProvider`, geçersiz kılınmalıdır soyut iki yöntem vardır: [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) ve [ `GetRootNodeCore` ](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, adından da anlaşılacağı gibi site haritası yapısının kalıcı depolama biriminden yüklenmesi ve bellekte oluşturma sorumludur. `GetRootNodeCore` kök düğüm site eşlemesinde döndürür.

Önce bir web uygulaması uygulama s yapılandırmasında kaydedilmelidir site haritası sağlayıcısı kullanabilir. Varsayılan olarak, `XmlSiteMapProvider` sınıf adını kullanarak kayıtlı `AspNetXmlSiteMapProvider`. Ek site haritası sağlayıcıları kaydetmek için aşağıdaki biçimlendirme eklemek `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample5.xml)]

*Adı* değer sağlayıcı okunabilir bir ad atar *türü* site haritası sağlayıcısı tam olarak nitelenmiş tür adını belirtir. Somut değerlerini ele alacağız *adı* ve *türü* değerleri adım 7'de, sonra ullanıcı oluşturduğumuz bizim Özel site haritası sağlayıcısı.

Erişilen gelen ilk kez site haritası sağlayıcı sınıfının örneği `SiteMap` sınıfı ve web uygulamasının kullanım süresi için bellekte kalır. Yalnızca bir örneği birden çok, eşzamanlı web sitesi ziyaretçilerden çağrılan site haritası sağlayıcısı olduğundan, s sağlayıcısı yöntemleri olmasını kesinlik temelli *iş parçacığı*.

Performans ve ölçeklenebilirlik, onu s biz bellek içi sitenin önbelleğe önemli eşleme yapısı ve bu önbelleğe her zaman yeniden yerine yapısı dönmek `BuildSiteMap` yöntemi çağrılır. `BuildSiteMap` Sayfa ve site haritası yapısının derinliği kullanımda Gezinti denetimlere bağlı olarak kullanıcı başına sayfa isteği başına birkaç kez çağrılabilir. Biz site haritası yapısında önbelleğe alma, her durumda, `BuildSiteMap` çalıştırıldığında her zaman biz (hangi sorguda veritabanına neden olur) mimarisi ürün ve kategori bilgilerini yeniden almaya gerekir. Biz önceki önbelleğe alma eğitimlerine açıklandığı gibi önbelleğe alınan veriler eski haline gelebilir. Bu mücadele etmek için size zaman - ya da SQL önbellek bağımlılık tabanlı expiries kullanabilirsiniz.

> [!NOTE]
> Bir site haritası sağlayıcısı isteğe bağlı olarak geçersiz kılabilir [ `Initialize` yöntemi](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` site haritası sağlayıcısı ilk örneği ve sağlayıcı atanan özel öznitelikleri geçirilen değiştiğinde çağrılan `Web.config` içinde `<add>` öğesi gibi: `<add name="name" type="type" customAttribute="value" />`. Sağlayıcı s kodu değiştirmek zorunda kalmadan çeşitli site haritası sağlayıcısı ile ilgili ayarları belirtmek bir sayfa Geliştirici izin vermek istiyorsanız yararlıdır. Biz kategori ve ürünleri verileri doğrudan veritabanından tersine mimarisi d biz yoluyla büyük olasılıkla okuma Örneğin, veritabanı bağlantı dizesi aracılığıyla belirtin sayfasında Geliştirici izin vermek istiyorsanız `Web.config` sabit kodlanmış kullanarak yerine Sağlayıcı s kodu değeri. Biz adım 6'yapı özel site haritası sağlayıcısı bu kılmaz `Initialize` yöntemi. Kullanma örneği için `Initialize` yöntemi, başvurmak [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [SQL Server'daki Site Haritaları depolama](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) makalesi.


## <a name="step-6-creating-the-custom-site-map-provider"></a>6. adım: özel Site haritası sağlayıcısı oluşturma

Kategoriler ve ürünler Northwind veritabanı, site haritası derlemeler özel site haritası sağlayıcısı oluşturmak için genişleten bir sınıfı oluşturmak ihtiyacımız `StaticSiteMapProvider`. 1. Adım'ı eklemek için sorulan bir `CustomProviders` klasöründe `App_Code` klasör - adlı bu klasöre yeni bir sınıf ekleyin `NorthwindSiteMapProvider`. Aşağıdaki kodu ekleyin `NorthwindSiteMapProvider` sınıfı:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample6.vb)]

Bu sınıf s keşfe s izin `BuildSiteMap` ile başlayan yöntemi bir [ `lock` deyimi](https://msdn.microsoft.com/library/c5kehkcz.aspx). `lock` Deyimi yalnızca tek bir iş parçacığı girmek için aynı anda böylece, kodu erişim seri hale getirme ve sağlar birbirine s parmaklarının atlama iki eş zamanlı iş parçacıkları engelliyor.

Sınıf düzeyi `SiteMapNode` değişkeni `root` site haritası yapısı önbelleğe almak için kullanılır. Site haritasını ilk kez veya temel alınan veri değiştirildikten sonra ilk kez oluşturulduğunda `root` olacaktır `Nothing` ve site haritası yapısı oluşturulur. Site haritası s kök düğümü atandığı `root` sonraki bu yöntem, oluşturma sırasında işlem çağrılırken, `root` değişmeyecek `Nothing`. Sonuç olarak, sürece `root` değil `Nothing` yeniden oluşturmak zorunda kalmadan, site haritası yapısı çağırana döndürülür.

Kök ise `Nothing`, ürün ve kategori bilgileri site haritası yapısı oluşturulur. Site haritasını oluşturarak yerleşik `SiteMapNode` örnekleri ve hiyerarşisi aracılığıyla çağrı oluşturuluyor `StaticSiteMapProvider` s sınıfı `AddNode` yöntemi. `AddNode` getirilebilir depolama iç muhasebe gerçekleştirir `SiteMapNode` içinde örnekler bir `Hashtable`. Hiyerarşi oluşturma başlamadan önce arayarak Başlat `Clear` iç öğeleri çıkışı temizler yöntemi `Hashtable`. Ardından, `ProductsBLL` s sınıfı `GetProducts` yöntemi ve elde edilen `ProductsDataTable` yerel değişkenleri depolanır.

Kök düğüm oluşturarak ve atanarak site haritası s yapım başlar `root`. Aşırı yüklemesini [ `SiteMapNode` s Oluşturucusu](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) burada ve bu boyunca kullanılan `BuildSiteMap` aşağıdaki bilgileri geçirilir:

- Site haritası sağlayıcısı başvuru (`Me`).
- `SiteMapNode` s `Key`. Bu değer her biri için benzersiz olmalıdır, gerekli `SiteMapNode`.
- `SiteMapNode` s `Url`. `Url` isteğe bağlıdır, ancak verdiyse, her `SiteMapNode` s `Url` değer benzersiz olmalıdır.
- `SiteMapNode` s `Title`, hangi gereklidir.

`AddNode(root)` Yöntem çağrısı ekler `SiteMapNode` `root` kök olarak site haritası için. Ardından, her `ProductRow` içinde `ProductsDataTable` numaralandırılır. Zaten var. bir `SiteMapNode` için geçerli ürün s kategorisi, başvurulan. Aksi takdirde, yeni bir `SiteMapNode` kategori oluşturulur ve bir alt öğesi olarak eklenir için `SiteMapNode``root` aracılığıyla `AddNode(categoryNode, root)` yöntem çağrısı. Uygun kategoriyi sonra `SiteMapNode` düğüm bulunamadı veya oluşturduğunuz bir `SiteMapNode` geçerli ürün için oluşturulur ve bir alt kategori öğesi olarak eklenir `SiteMapNode` aracılığıyla `AddNode(productNode, categoryNode)`. Unutmayın kategori `SiteMapNode` s `Url` özellik değeri `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` while ürün `SiteMapNode` s `Url` özelliği atandığında `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Bir veritabanına sahip bu ürünlerden `NULL` değerini kendi `CategoryID` kategorisi altında gruplanmış `SiteMapNode` , `Title` özelliği yok ve özelliği ayarlanmış `Url` özelliği boş bir dize olarak ayarlayın. Ayarlama karar `Url` bu yana boş bir dizesine `ProductBLL` s sınıfı `GetProductsByCategory(categoryID)` yöntemi şu anda yalnızca bu ürünlerle döndürülecek özelliği eksik bir `NULL` `CategoryID` değeri. Ayrıca, nasıl gezinme denetimlerini işlemeye göstermek istediğinizde bir `SiteMapNode` için bir değer eksik kendi `Url` özelliği. Bu öğretici genişletmenizi teşvik böylece hiçbiri `SiteMapNode` s `Url` özelliği işaret `ProductsByCategory.aspx`, henüz ürünleriyle yalnızca görüntüler `NULL` `CategoryID` değerleri.


Site haritası oluşturduktan sonra rasgele bir nesne üzerinde bir SQL önbellek bağımlılığı kullanarak veri önbelleği eklenen `Categories` ve `Products` aracılığıyla tabloları bir `AggregateCacheDependency` nesnesi. Önceki öğreticide SQL önbellek bağımlılıkları kullanarak keşfedilen *kullanarak SQL önbellek bağımlılıkları*. Özel site haritası sağlayıcısı ancak, bir aşırı yüklemesini veri önbelleği s kullanır `Insert` yöntemi görüyoruz henüz keşfetmek için ve. Bu aşırı nesneyi önbellekten kaldırıldığında çağrılan bir temsilciyi son giriş parametresi olarak kabul eder. Özellikle, size yeni bir geçirmek [ `CacheItemRemovedCallback` temsilci](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) işaret `OnSiteMapChanged` yöntemi aşağıya içinde tanımlanan `NorthwindSiteMapProvider` sınıfı.

> [!NOTE]
> Site haritasını bellek içi gösterimini sınıf düzeyi değişkeni önbelleğe `root`. Özel site haritası sağlayıcısı sınıfı ve bu örnek web uygulamasındaki tüm iş parçacıkları arasında paylaşıldığından yalnızca bir örnek olduğundan, bu sınıf değişkeni önbellek olarak görev yapar. `BuildSiteMap` Yöntemi de kullandığı veri önbelleği temel veritabanı verilerde bildirim almak için yalnızca bir yol olarak `Categories` veya `Products` tabloları değişiklikleri. Veri önbelleğe koyulur değeri yalnızca geçerli tarih ve saat olduğuna dikkat edin. Gerçek site haritası verileri *değil* veri önbelleğindeki yerleştirin.


`BuildSiteMap` Yöntemi tamamlayan site haritası kök düğümünün döndürerek.

Kalan yöntemleri oldukça basittir. `GetRootNodeCore` kök düğüm döndürmek için sorumludur. Bu yana `BuildSiteMap` kökünü döndürür `GetRootNodeCore` yalnızca döndürür `BuildSiteMap` s dönüş değeri. `OnSiteMapChanged` Yöntemi kümeleri `root` başa `Nothing` önbellek öğesi kaldırıldığında. Döndürülmek kök ile `Nothing`, sonraki `BuildSiteMap` olan çağrılır, site haritası yapısı yeniden oluşturulacak. Son olarak, `CachedDate` özelliği, bu tür bir değeri varsa, veri önbelleğinde depolanan tarih ve saat değeri döndürür. Bu özellik sayfasında geliştirici tarafından site haritası verileri en son ne zaman önbelleğe ilişkili belirlemek için kullanılabilir.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>7. adım: Kaydetme`NorthwindSiteMapProvider`

Kullanılacak web uygulamamız için sırayla `NorthwindSiteMapProvider` site haritası sağlayıcısı, adım 6'oluşturulan ihtiyacımız içine kaydettirmek `<siteMap>` bölümünü `Web.config`. Özellikle, aşağıdaki biçimlendirmesi içinde eklemeniz `<system.web>` öğesinde `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample7.xml)]

Bu biçimlendirme şu iki işlemi yapar: gösterir ilk olarak, yerleşik `AspNetXmlSiteMapProvider` varsayılan site haritası sağlayıcısı; ikinci olarak, adım 6 ' İnsan kolay adı Northwind oluşturulan özel site haritası sağlayıcısı kaydeder.

> [!NOTE]
> Uygulama s içinde bulunan site haritası sağlayıcıları için `App_Code` klasörü, değeri `type` yalnızca sınıf adı bir özniteliktir. Alternatif olarak, özel site haritası sağlayıcısı ayrı bir sınıf kitaplığı projesi ile web uygulaması s yerleştirilen derlenmiş derleme oluşturulmuş `/Bin` dizin. Bu durumda, `type` öznitelik değeri olacaktır *Namespace*. *ClassName*, *AssemblyName* .


Güncelleştirdikten sonra `Web.config`, herhangi bir sayfasından öğreticiler bir tarayıcıda görüntülemek için bir dakikanızı ayırın. Sol taraftaki gezinti arabirimi bölümleri görüntülenmeye devam eder ve öğreticiler tanımlanan Not `Web.sitemap`. Biz sol olmasıdır `AspNetXmlSiteMapProvider` varsayılan sağlayıcı olarak. Kullanan bir gezinti kullanıcı arabirimi öğesi oluşturmak için `NorthwindSiteMapProvider`, biz Northwind site haritası sağlayıcısı kullanılması gerektiğini açıkça belirtmeniz gerekir. Bu adım 8'de nasıl göreceğiz.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>8. adım: özel Site haritası sağlayıcısı kullanarak Site Haritası bilgilerini görüntüleme

Özel site haritası sağlayıcısı oluşturulan ve kayıtlı `Web.config`, biz Gezinti denetimler eklemek için hazır re `Default.aspx`, `ProductsByCategory.aspx`, ve `ProductDetails.aspx` içinde sayfaları `SiteMapProvider` klasör. Başlangıç açarak `Default.aspx` sayfasında ve sürükleyin bir `SiteMapPath` tasarımcıya araç. ASP araç gezinti bölümünde bulunur.


[![Bir SiteMapPath için Default.aspx ekleme](building-a-custom-database-driven-site-map-provider-vb/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image18.gif)

**Şekil 16**: bir SiteMapPath eklemek `Default.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image20.gif))


ASP site haritası içindeki geçerli sayfa s konumunu belirten bir içerik haritası görüntüler. Ana sayfaya dön bir SiteMapPath eklediğimiz geri *ana sayfalar ve Site gezintisi* Öğreticisi.

Bir tarayıcı aracılığıyla bu sayfayı görüntülemek için bir dakikanızı ayırın. Şekil 16'eklenen SiteMapPath kendi verisinden çekme varsayılan site haritası sağlayıcısı kullanan `Web.sitemap`. Bu nedenle, içerik haritası giriş gösterir &gt; sağ üst köşesinde haritasındaki gibi Site Haritası özelleştirme.


[![Varsayılan Site haritası sağlayıcısı içerik haritası kullanır](building-a-custom-database-driven-site-map-provider-vb/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.gif)

**Şekil 17**: içerik haritası varsayılan Site haritası sağlayıcısı kullanır ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image23.gif))


Adım 6'oluşturduğumuz özel site haritası sağlayıcısı kullanın Şekil 16'eklenen SiteMapPath olacak şekilde ayarlanmış kendi [ `SiteMapProvider` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) Northwind için ad size atanan `NorthwindSiteMapProvider` içinde `Web.config`. Ne yazık ki, Tasarımcı varsayılan site haritası sağlayıcısı kullanmaya devam eder, ancak bu özellik değişikliği yaptıktan sonra bir tarayıcı aracılığıyla sayfasını ziyaret edin, içerik haritası şimdi özel site haritası sağlayıcısı kullandığını görürsünüz.


[![İçerik haritası artık özel Site haritası sağlayıcısı NorthwindSiteMapProvider kullanır](building-a-custom-database-driven-site-map-provider-vb/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image24.gif)

**Şekil 18**: içerik haritası artık özel Site haritası sağlayıcısı kullanıyor `NorthwindSiteMapProvider` ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image26.gif))


ASP daha işlevsel bir kullanıcı arabirimi görüntüler `ProductsByCategory.aspx` ve `ProductDetails.aspx` sayfaları. Bir SiteMapPath ayarı bu sayfalara eklemek `SiteMapProvider` Northwind için hem de özelliği. Gelen `Default.aspx` içecek ürünleri görüntüleyin bağlantısını ve ardından Chai Çay ayrıntılarını görüntüle bağlantısını tıklayın. Şekil 19 gösterildiği gibi geçerli site eşlemesi bölümü (Chai Çay) ve onun üst öğelerinin içerik haritası içerir: Meşrubat ve tüm kategoriler.


[![İçerik haritası artık özel Site haritası sağlayıcısı NorthwindSiteMapProvider kullanır](building-a-custom-database-driven-site-map-provider-vb/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.png)

**Şekil 19**: içerik haritası artık özel Site haritası sağlayıcısı kullanıyor `NorthwindSiteMapProvider` ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image22.png))


Menü ve TreeView denetimleri gibi SiteMapPath yanı sıra diğer gezinti kullanıcı arabirimi öğeleri kullanılabilir. `Default.aspx`, `ProductsByCategory.aspx`, Ve `ProductDetails.aspx` Bu öğretici, örneğin, indirme sayfalarında, tüm içerir (bkz. Şekil 20) menü denetimleri. Bkz: [inceleniyor ASP.NET 2.0 s Site Gezinti özellikleri](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) ve [kullanarak Site Gezinti denetimlerinin](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) bölümünü [ASP.NET 2.0 QuickStarts](https://quickstarts.asp.net/QuickStartv20/aspnet/) daha kapsamlı bir bakış için Gezinti denetimleri ve site sistemi, ASP.NET 2.0 eşleyin.


[![Menü denetimi her kategori ve ürünleri listeler](building-a-custom-database-driven-site-map-provider-vb/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image28.gif)

**Şekil 20**: menü denetim listeleri her kategori ve ürünleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-a-custom-database-driven-site-map-provider-vb/_static/image30.gif))


Bu öğreticide daha önce belirtildiği gibi site haritası yapısı aracılığıyla programlı olarak erişilebilir `SiteMap` sınıfı. Aşağıdaki kod kök döndürür `SiteMapNode` varsayılan sağlayıcısı:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample8.vb)]

Bu yana `AspNetXmlSiteMapProvider` varsayılan sağlayıcı uygulamamız için tanımlanan kök düğümü Yukarıdaki kod döndürecekti `Web.sitemap`. Bir site haritası sağlayıcısı varsayılan dışındaki başvurmak için `SiteMap` s sınıfı [ `Providers` özelliği](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) sözlüğüdür:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample9.vb)]

Burada *adı* özel site haritası sağlayıcısı (web uygulamamız için Northwind) adıdır.

Bir site haritası sağlayıcısı için belirli bir üye erişmek için `SiteMap.Providers["name"]` sağlayıcı örneğini almak ve uygun türe dönüştürülemiyor. Örneğin, görüntülemek için `NorthwindSiteMapProvider` s `CachedDate` bir ASP.NET sayfası özelliğinde şu kodu kullanın:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample10.vb)]

> [!NOTE]
> SQL önbellek bağımlılık özelliği test etmek emin olun. Ziyaret sonra `Default.aspx`, `ProductsByCategory.aspx`, ve `ProductDetails.aspx` sayfaları, düzenleme, ekleme ve bölüm silme eğitimlerine birine gidin ve bir kategori veya ürün adını düzenleyin. Sayfalarında birine dönmek `SiteMapProvider` klasör. Temel alınan veritabanı değişikliği not etmek yoklama mekanizması için yeterli süre geçti varsayıldığında, site haritası yeni ürün veya kategori adı göstermek için güncelleştirilmesi gerekir.


## <a name="summary"></a>Özet

ASP.NET 2.0 s site haritası özellikleri içeren bir `SiteMap` sınıfı, bir dizi yerleşik gezinti Web denetler ve site haritası bilgileri bekliyor bir varsayılan site haritası sağlayıcısı bir XML dosyasına kalıcı. Site Eşlemesi bilgileri kaynağından bazı diğer gibi bir veritabanı, uygulama s mimarisi ya da özel site haritası sağlayıcısı oluşturmak için ihtiyacımız uzak bir Web hizmeti kullanmak için. Bu, doğrudan veya dolaylı olarak, türeyen bir sınıf oluşturursunuz gelen `SiteMapProvider` sınıfı.

Bu öğreticide uygulama mimarisinden culled ürün ve kategori bilgileri site haritası dayanarak bir özel site haritası sağlayıcısı oluşturma gördük. Genişletilmiş bizim sağlayıcısı `StaticSiteMapProvider` sınıfı ve entailed oluşturma bir `BuildSiteMap` veri alınan yöntemi site haritası hiyerarşisi oluşturulan ve sonuçta elde edilen sınıf düzeyinde bir değişkene yapısında önbelleğe alınmış. Bir SQL önbellek bağımlılığı bir geri çağırma işlevini ile önbelleğe alınmış geçersiz kılmak için kullandık ne zaman yapısı arka plandaki `Categories` veya `Products` veri değiştirdi.

Mutluluk programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [SQL Server'da Site Haritaları depolama](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) ve [SQL Site haritası sağlayıcısı önceden beklediğinden](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [ASP.NET 2.0 göz s sağlayıcı modeli](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Sağlayıcı Araç Seti](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [ASP.NET 2.0 inceleniyor s Site Gezinti özellikleri](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Dave Gardner, Zack Can, Teresa Murphy ve Bernadette Leigh yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](building-a-custom-database-driven-site-map-provider-cs.md)
