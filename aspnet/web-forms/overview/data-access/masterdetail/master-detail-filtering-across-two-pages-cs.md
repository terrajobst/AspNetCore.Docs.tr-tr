---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: Ana/ayrıntı iki sayfalara (C#) filtreleme | Microsoft Docs
author: rick-anderson
description: Bu öğreticide veritabanında tedarikçileri listelemek için GridView kullanarak Biz bu deseni uygulamanız. GridView tedarikçi her satırda bir görünümü içerecek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 0858bf9c8dc380898647293825145654ac1dbc34
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887795"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>Ana/ayrıntı filtreleme iki sayfalara (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe) veya [PDF indirin](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> Bu öğreticide veritabanında tedarikçileri listelemek için GridView kullanarak Biz bu deseni uygulamanız. GridView her tedarikçi satır tıklandığında, ayrı bir sayfa kullanıcıya sürer, bu ürünler için seçilen tedarikçi listeler, bir görünüm ürünleri bağlantı içerir.


## <a name="introduction"></a>Giriş

Önceki iki eğitimlerine gördüğümüz nasıl [tek web sayfasında DropDownLists kullanarak ana/ayrıntı raporları görüntülemek](master-detail-filtering-with-a-dropdownlist-cs.md) için ["ana" kaydeder ve bir GridView veya DetailsView denetimi görüntüleme](master-detail-filtering-with-two-dropdownlists-cs.md) görüntülemek için " Ayrıntıları." Başka bir yaygın olarak kullanılan deseni ana/ayrıntı raporları için kullanılan bir web sayfası ve başka gösterilen ayrıntıları Yöneticisi kayıtlarda olmalıdır. Bir forum Web sitesi gibi [ASP.NET forumları](https://forums.asp.net/), bu yöntem modelinde harika bir örneğidir. ASP.NET forumlar, Başlarken, forumlar Web Forms, veri sunu denetimleri, çeşitli oluşurlar ve benzeri. Birçok iş parçacıklarının her Forumu oluşur ve her iş parçacığı gönderileri sayısı oluşur. ASP.NET forumları giriş sayfasında forumları listelenir. Bir forumda tıklatarak, whisks size bir `ShowForum.aspx` iş parçacıklarını bu forumunu listeler sayfası. Benzer şekilde, bir iş parçacığında tıklamak, götürür `ShowPost.aspx`, postaları tıklandığını iş parçacığı için görüntüler.

Bu öğreticide veritabanında tedarikçileri listelemek için GridView kullanarak Biz bu deseni uygulamanız. GridView her tedarikçi satır tıklandığında, ayrı bir sayfa kullanıcıya sürer, bu ürünler için seçilen tedarikçi listeler, bir görünüm ürünleri bağlantı içerir.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>1. adım: Ekleme`SupplierListMaster.aspx`ve`ProductsForSupplierDetails.aspx`sayfaları için`Filtering`klasörü

Sayfa düzeni üçüncü öğreticide tanımlarken "starter" sayfa sayısı eklediğimiz `BasicReporting`, `Filtering`, ve `CustomFormatting` klasörler. Ancak, size bir başlangıç sayfası Bu öğretici için o anda eklemediniz, böylece iki yeni sayfalara eklemek için bir dakikanızı ayırın `Filtering` klasörü: `SupplierListMaster.aspx` ve `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` durumdayken (tedarikçileri) "ana" kayıt listeleyecek `ProductsForSupplierDetails.aspx` ürünleri için seçilen tedarikçi görüntülenir.

Ne zaman bu iki yeni sayfa oluşturma bunları ile ilişkilendirmek belirli `Site.master` ana sayfa.


![SupplierListMaster.aspx ve ProductsForSupplierDetails.aspx sayfalar filtreleme klasörüne ekleyin](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**Şekil 1**: eklemek `SupplierListMaster.aspx` ve `ProductsForSupplierDetails.aspx` sayfaları için `Filtering` klasörü


Yeni sayfa projeye eklerken, ayrıca, site haritası dosyasını güncelleştirdiğinizden emin olun `Web.sitemap`, buna göre. Bu öğretici için eklemeniz yeterlidir `SupplierListMaster.aspx` filtreleme raporları bir alt öğesi olarak aşağıdaki XML içeriği kullanarak site haritası sayfasına `<siteMapNode>` öğe:


[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> Yeni ASP.NET ekleme sayfası kullanma, site haritası güncelleştirilmesi işlemini otomatikleştirmek [K. Scott Allen](http://odetocode.com/Blogs/scott/)kullanıcının ücretsiz Visual Studio [Site Haritası makrosu](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx).


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>2. adım: tedarikçi listesinde görüntüleme`SupplierListMaster.aspx`

İle `SupplierListMaster.aspx` ve `ProductsForSupplierDetails.aspx` oluşturulan sayfaları, bizim sonraki adımdır sağlayıcıları GridView oluşturmak için `SupplierListMaster.aspx`. GridView sayfaya eklemek ve yeni ObjectDataSource için bağlayın. Bu ObjectDataSource kullanması gereken `SuppliersBLL` sınıfının `GetSuppliers()` tüm tedarikçileri döndürülecek yöntemi.


[![SuppliersBLL sınıfı seçin](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**Şekil 2**: seçin `SuppliersBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image4.png))


[![ObjectDataSource GetSuppliers() yöntemi kullanmak üzere yapılandırma](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**Şekil 3**: ObjectDataSource kullanılacak yapılandırma `GetSuppliers()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image7.png))


Bir bağlantı eklemek ihtiyacımız ürünleri görüntüleyin her GridView satır, tıklatıldığında başlıklı kullanıcıyı götürür `ProductsForSupplierDetails.aspx` seçilen satırın 's geçirme `SupplierID` aracılığıyla sorgu dizesi değeri. Örneğin, kullanıcı Tokyo Traders tedarikçi ürünleri görüntüleyin bağlantısına tıklarsa (olan bir `SupplierID` değeri 4), bunlar gönderilmesi gereken `ProductsForSupplierDetails.aspx?SupplierID=4`.

Bunu başarmak için add bir [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) her GridView satır köprü ekler GridView için. GridView'ın akıllı etiket sütunları düzenleme bağlantısını tıklatarak başlatın. Ardından, sol üst köşedeki listeden HyperLinkField seçin ve HyperLinkField GridView'ın alan listesine eklemek için Ekle'yi tıklatın.


[![GridView bir HyperLinkField ekleyin](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**Şekil 4**: bir HyperLinkField GridView ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image10.png))


HyperLinkField aynı metni kullanmak üzere yapılandırılabilir veya URL bağlantıdaki her GridView satır değerleri veya bu değerleri belirli her satır için ilişkili veri değerlerini temel alabilir. Tüm satırlar boyunca değeri statik belirtmek için HyperLinkField's kullanın `Text` veya `NavigateUrl` özellikleri. HyperLinkField's ayarlama tüm satırlar için aynı olması için bağlantı metni istiyoruz, `Text` özelliğini ürünleri görüntüleyin.


[![Görünüm ürünleri HyperLinkField'ın metin özelliğini ayarlayın](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**Şekil 5**: HyperLinkField's ayarlamak `Text` ürünleri görüntüleyin özelliğine ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image13.png))


Metin veya temel alınan veri GridView satırın bağlı temel almasını URL değerleri ayarlamak için veri metin alanları veya URL değerleri çekilen de nereden belirtin `DataTextField` veya `DataNavigateUrlFields` özellikleri. `DataTextField` yalnızca bir tek veri alanına ayarlanabilir; `DataNavigateUrlFields`, Bununla birlikte, virgülle ayrılmış listesini veri alanları için ayarlanabilir. Kimliğinizi sık metin ya da geçerli sıranın veri alan değeri ve bazı static işaretleme bileşimini URL'yi temel gerekiyor. Bu öğreticide, örneğin, olmasını HyperLinkField'ın bağlantıları URL'sini istiyoruz `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, burada *`supplierID`* her GridView'ın sıranın olduğu `SupplierID` değeri. Hem statik ihtiyacımız ve veri temelli burada değerleri dikkat edin: `ProductsForSupplierDetails.aspx?SupplierID=` bağlantının URL'SİNDE bölümüdür statik, ancak *`supplierID`* bölümü verilere her satırın kendi değeri olduğu gibi `SupplierID` değeri.

Statik ve veri temelli bir değerler birleşimi belirtmek için kullanın `DataTextFormatString` ve `DataNavigateUrlFormatString` özellikleri. Bu özellikler, static işaretleme gerektiği gibi girin ve sonra işaretçiyi kullanın `{0}` belirtilen alanın değerini istediğiniz `DataTextField` veya `DataNavigateUrlFields` özellikleri görüntülenir. Varsa `DataNavigateUrlFields` özelliğine sahip birden çok alanları belirtilen kullanım `{0}` eklenen ilk alan değeri istediğiniz yerde `{1}` ikinci alan değeri ve benzeri için.

Bu bizim öğreticisi için uygulama, ayarlamak ihtiyacımız `DataNavigateUrlFields` özelliğine `SupplierID`, bu değeri bir satır başına temelinde özelleştirmek için ihtiyacımız veri alanı olduğundan ve `DataNavigateUrlFormatString` özelliğine `ProductsForSupplierDetails.aspx?SupplierID={0}`.


[![HyperLinkField SupplierID göre düzgün bağlantı URL'si içerecek şekilde yapılandırın](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**Şekil 6**: uygun bağlantı URL'si dayalı bağlı içerecek şekilde HyperLinkField yapılandırma `SupplierID` ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image16.png))


HyperLinkField ekledikten sonra özelleştirme ve GridView'ın alanları yeniden sıralamak çekinmeyin. Bazı küçük alan düzeyindeki özelleştirmeler yapmış olduğunuz sonra aşağıdaki biçimlendirme GridView gösterir.


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

Görüntülemek için bir dakikanızı ayırın `SupplierListMaster.aspx` bir tarayıcı aracılığıyla sayfası. Şekil 7'de görüldüğü gibi sayfa şu anda tüm ürünleri görüntüleyin bağlantı sağlayıcıların listeler. Görünüm ürünleri tıklatarak bağlantı gideceksiniz `ProductsForSupplierDetails.aspx`, tedarikçi boyunca geçen `SupplierID` sorgu dizesi içinde.


[![Bir görünümü ürünleri bağlantısı her tedarikçi satır içerir](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**Şekil 7**: görünümü ürünleri bağlantısı her tedarikçi satır içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>3. adım: tedarikçi ürünlerinde listeleme`ProductsForSupplierDetails.aspx`

Bu noktada `SupplierListMaster.aspx` sayfa kullanıcılara gönderme `ProductsForSupplierDetails.aspx`, seçili tedarikçi geçirme `SupplierID` sorgu dizesi içinde. Öğreticinin son adımı ürünleri içinde GridView içinde görüntülemektir `ProductsForSupplierDetails.aspx` , `SupplierID` eşittir `SupplierID` sorgu dizesi geçirilen. GridView'ekleyerek bu başlangıç gerçekleştirmek için `ProductsForSupplierDetails.aspx` sayfasında, adında yeni bir ObjectDataSource denetimi kullanarak `ProductsBySupplierDataSource` , çağırır `GetProductsBySupplierID(supplierID)` yönteminden `ProductsBLL` sınıfı.


[![ProductsBySupplierDataSource adlı yeni bir ObjectDataSource ekleme](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**Şekil 8**: yeni ObjectDataSource adlandırılmış eklemek `ProductsBySupplierDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image22.png))


[![ProductsBLL sınıfı seçin](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**Şekil 9**: seçin `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image25.png))


[![GetProductsBySupplierID(supplierID) yöntemi çağırma ObjectDataSource sahip](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**Şekil 10**: ObjectDataSource çağırma sahip `GetProductsBySupplierID(supplierID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image28.png))


Veri Kaynağı Yapılandırma Sihirbazı'nın son adım kaynağı sağlamamız ister `GetProductsBySupplierID(supplierID)` yöntemin *`supplierID`* parametresi. Sorgu dizesi değerini kullanmak için parametre kaynağı sorgu dizesine ayarlayın ve QueryStringField metin kutusuna kullanmak üzere sorgu dizesi değeri adını girin (`SupplierID`).


[![Parametre değeri SupplierID sorgu dizesi değerinden SupplierID doldurma](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**Şekil 11**: doldurmak *`supplierID`* parametre değerinin `SupplierID` sorgu dizesi değeri ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image31.png))


Tüm olan İşte bu kadar! Şekil 12 gösterir `ProductsForSupplierDetails.aspx` sayfasında Tokyo Traders bağlantıyı tıklatarak sitesini ziyaret ettiğinizde `SupplierListMaster.aspx`.


[![Tokyo Traders tarafından sağlanan ürünleri gösterilir](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**Şekil 12**: ürünleri verdiği Tokyo Traders gösterilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Sağlayıcı bilgileri görüntüleme`ProductsForSupplierDetails.aspx`

Şekil 12 gösterildiği gibi `ProductsForSupplierDetails.aspx` sayfa basitçe tarafından sağlanan ürünleri listeler `SupplierID` sorgu dizesinde belirtilen. Birisi bu sayfasına doğrudan gönderilen ancak, Şekil 12 Tokyo Traders ürünleri gösterildiğini bilmez. Bu sorunu gidermek için şu Tedarikçi bilgilerini bu sayfasında görüntüleyebilirsiniz.

FormView GridView ürünleri yukarıda ekleyerek başlayın. Adlı yeni bir ObjectDataSource denetimi oluşturma `SuppliersDataSource` , çağırır `SuppliersBLL` sınıfının `GetSupplierBySupplierID(supplierID)` yöntemi.


[![SuppliersBLL sınıfı seçin](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**Şekil 13**: seçin `SuppliersBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image37.png))


[![GetSupplierBySupplierID(supplierID) yöntemi çağırma ObjectDataSource sahip](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**Şekil 14**: ObjectDataSource çağırma sahip `GetSupplierBySupplierID(supplierID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image40.png))


İle `ProductsBySupplierDataSource`, sahip *`supplierID`* parametre değeri atanan `SupplierID` sorgu dizesi değeri.


[![Parametre değeri SupplierID sorgu dizesi değerinden SupplierID doldurma](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**Şekil 15**: doldurmak *`supplierID`* parametre değerinin `SupplierID` sorgu dizesi değeri ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image43.png))


FormView Tasarım görünümünde ObjectDataSource bağlanırken, Visual Studio FormView's otomatik olarak oluşturacak `ItemTemplate`, `InsertItemTemplate`, ve `EditItemTemplate` tarafından döndürülen veri alanların her biri için etiket ve metin kutusuna Web denetimleri ile ObjectDataSource. Biz yalnızca tedarikçi bilgi kullanımında kaldırmak ücretsiz görüntülemek istediğiniz beri `InsertItemTemplate` ve `EditItemTemplate`. Ardından, tedarikçi şirket adını görüntüler ItemTemplate düzenleyin bir `<h3>` öğesi ve adres, şehir, ülke ve şirket adı altındaki telefon numarası. FormView's alternatif olarak, el ile ayarlayabilirsiniz `DataSourceID` ve oluşturma `ItemTemplate` biçimlendirme geri yaptığımız gibi "[veri ObjectDataSource ile görüntüleme](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)" öğretici.

Sonra bu düzenlemeler FormView'ın bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

Şekil 16 gösteren ekran görüntüsü `ProductsForSupplierDetails.aspx` yukarıdaki ayrıntılı Tedarikçi bilgilerini eklenmiştir sonra sayfa.


[![Tedarikçi hakkında bir Özet ürünlerin listesini içerir](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**Şekil 16**: Özet hakkında tedarikçi ürünlerin listesini içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>En son uygulama dokunur`ProductsForSupplierDetails.aspx`kullanıcı Arabirimi

Deneyimi var. Bu rapor için kullanıcı geliştirmek için birkaç biz gelmelidir yapmak için eklemeleri olduğu `ProductsForSupplierDetails.aspx` sayfası. Şu an bir kullanıcı Git tek yolu `ProductsForSupplierDetails.aspx` Üreticiler listesine geri sayfasıdır kendi tarayıcınızın Geri düğmesini tıklatın. Köprü denetimi ekleyelim `ProductsForSupplierDetails.aspx` bağlantılar geri sayfa `SupplierListMaster.aspx`, kullanıcının ana listesine dönmek başka bir yol sağlayarak.


[![Kullanıcı için SupplierListMaster.aspx geri almak için köprü denetim ekleme](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**Şekil 17**: kullanıcı geri almak için bir köprü denetim eklemek `SupplierListMaster.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image49.png))


Kullanıcı herhangi bir ürünü sahip olmayan bir sağlayıcının görünümü ürünleri bağlantısına tıklarsa `ProductsBySupplierDataSource` içinde ObjectDataSource `ProductsForSupplierDetails.aspx` tüm sonuçları döndüren olmaz. ObjectDataSource bağlı GridView, kullanıcının tarayıcısının sayfasında boş bir bölgede kaynaklanan herhangi biçimlendirmesi oluşturmak olmaz. Kullanıcıya'nın seçili sağlayıcı ile ilişkili hiç ürün yok daha net bir şekilde iletişim kurmak için biz GridView's ayarlayabilirsiniz `EmptyDataText` özelliği böyle bir durum duyduğunuzda görüntülenen istiyoruz iletisi. "Bu sağlayıcısı tarafından sağlanan hiç ürün yok" için bu özelliği ayarlayın

Varsayılan olarak, en az bir ürün kategoriye veritabanındaki tüm tedarikçileri sağlar. Ancak, Bu öğretici için el ile değiştirdim `Products` tablo Escargots Nouveaux tedarikçi artık tüm ürünleri ile ilişkili olmasını sağlayın. Bu değişiklik yapıldıktan sonra Şekil 18 Escargots Nouveaux için Ayrıntılar sayfası gösterilir.


[![Kullanıcıların tedarikçi ürünlerden sağlamaz bildirilir](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**Şekil 18**: kullanıcıları bilgilendirmek tedarikçi ürünlerden sağlamaz ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-across-two-pages-cs/_static/image52.png))


## <a name="summary"></a>Özet

Ana/ayrıntı raporları tek bir sayfada hem ana hem de ayrıntı kayıtlarını görüntüleyebilirsiniz, ancak birçok Web sitelerinde bunlar arasında iki web sayfaları ayrılır. Bu öğreticide "Ayrıntılar" sayfasında listelenen ilişkili ürünler ve GridView "ana" web sayfasında listelenen tedarikçileri sağlayarak böyle bir ana öğe/ayrıntı raporu uygulamak nasıl inceledik. Sıranın geçirilen ayrıntıları sayfasına bir bağlantı her tedarikçi satır ana web sayfasında bulunan `SupplierID` değeri. Bu tür satır özgü bağlantıları kolayca GridView'ın HyperLinkField kullanılarak eklenebilir.

Ayrıntılar sayfasında bu ürünler için belirtilen tedarikçi alma çağırarak gerçekleştirilmiştir `ProductsBLL` sınıfının `GetProductsBySupplierID(supplierID)` yöntemi. *`supplierID`* Bildirimli olarak sorgu dizesi parametresi kaynak olarak kullanarak bir parametre belirtildi. Ayrıca Ayrıntılar sayfasında bir FormView kullanarak sağlayıcı ayrıntıları görüntülemek nasıl inceledik.

Bizim [sonraki öğretici](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) ana/ayrıntı raporlarda son sunucudur. Her satır seçme düğmesi sahip olduğu bir GridView ürünlerin listesini görüntülemek nasıl inceleyeceğiz. Select düğmesini tıklatarak bu ürünün ayrıntıları DetailsView denetimini aynı sayfada görüntülenir.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Hilton Giesenow oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](master-detail-filtering-with-two-dropdownlists-cs.md)
> [sonraki](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
