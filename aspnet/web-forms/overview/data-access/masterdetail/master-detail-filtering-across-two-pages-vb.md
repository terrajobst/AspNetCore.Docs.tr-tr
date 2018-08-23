---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
title: Ana/ayrıntı filtreleme (VB) iki sayfada | Microsoft Docs
author: rick-anderson
description: Bu öğreticide GridView kullanarak veritabanında tedarikçileri listelemek için Biz bu düzen uygulayacaksınız. Her GridView tedarikçi satırda bir görünümü içerecek...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 361d6a44-3f1f-4daf-85df-d4c2b8bf065d
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 290c4eba6f77a6006d424c3f05b77c1c128026b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756275"
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>Ana/ayrıntı filtreleme (VB) iki sayfada
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_9_VB.exe) veya [PDF olarak indirin](master-detail-filtering-across-two-pages-vb/_static/datatutorial09vb1.pdf)

> Bu öğreticide GridView kullanarak veritabanında tedarikçileri listelemek için Biz bu düzen uygulayacaksınız. Her GridView tedarikçi satıra tıklandığında, kullanıcıya ayrı bir sayfa sürer, bu ürünlerin için seçilen tedarikçi listeler, ürünleri görüntüle bağlantıyı içerir.


## <a name="introduction"></a>Giriş

Önceki iki öğreticilerde gördüğümüz tek web sayfasında "ana" kayıtları görüntülemek için DropDownList kullanarak ana/ayrıntı raporları görüntüleme ve [GridView](master-detail-filtering-with-a-dropdownlist-vb.md) veya [DetailsView](master-detail-filtering-with-two-dropdownlists-vb.md) denetiminin görüntülemek için " Ayrıntılar." Ana/Ayrıntılar raporlar için kullanılan başka bir yaygın düzen, bir web sayfası ve başka gösterilen ayrıntıları ana kayıtlarda olmasını sağlamaktır. Bir forum Web sitesi gibi [ASP.NET forumları](https://forums.asp.net/), uygulamada bu düzenin harika bir örnektir. ASP.NET forumları kullanmaya başlama, Forum Web Forms, veri sunu denetimleri, çeşitli oluşan ve benzeri. Birçok iş parçacıklarının her forum oluşur ve her bir iş parçacığı gönderilerin bir dizi oluşur. ASP.NET forumları giriş sayfasında Forumlara listelenir. Bir forumda tıklayarak, whisks size bir `ShowForum.aspx` sayfasında, iş parçacıkları bu forum için listeler. Benzer şekilde, bir iş parçacığında tıklayarak açılır `ShowPost.aspx`, gönderiler tıklandığını iş parçacığı için görüntüler.

Bu öğreticide GridView kullanarak veritabanında tedarikçileri listelemek için Biz bu düzen uygulayacaksınız. Her GridView tedarikçi satıra tıklandığında, kullanıcıya ayrı bir sayfa sürer, bu ürünlerin için seçilen tedarikçi listeler, ürünleri görüntüle bağlantıyı içerir.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>1. adım: Ekleme`SupplierListMaster.aspx`ve`ProductsForSupplierDetails.aspx`sayfaları için`Filtering`klasörü

Sayfa düzeni üçüncü öğreticide tanımlarken bir "Başlangıç" sayfa sayısı ekledik `BasicReporting`, `Filtering`, ve `CustomFormatting` klasörleri. Ancak, size bir başlangıç sayfası Bu öğretici için o anda eklemedi, bu nedenle için iki yeni sayfalar eklemek için bir dakikanızı ayırın `Filtering` klasör: `SupplierListMaster.aspx` ve `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` "ana" (Üreticiler) kayıt sırasında listeler `ProductsForSupplierDetails.aspx` ürünler için seçilen tedarikçi görüntülenir.

Ne zaman bu iki yeni sayfa oluşturma bunlarla ilişkilendirdiğiniz belirli `Site.master` ana sayfa.


![ProductsForSupplierDetails.aspx sayfaları ve SupplierListMaster.aspx filtreleme klasöre ekleyin.](master-detail-filtering-across-two-pages-vb/_static/image1.png)

**Şekil 1**: ekleme `SupplierListMaster.aspx` ve `ProductsForSupplierDetails.aspx` sayfaları için `Filtering` klasörü


Ayrıca, yeni sayfalar projeye eklerken, site haritası güncelleştirmeyi unutmayın `Web.sitemap`, buna göre. Bu öğretici için eklemeniz yeterlidir `SupplierListMaster.aspx` filtreleme raporlar alt sitesi olarak aşağıdaki XML içeriği kullanarak site haritası sayfasına `<siteMapNode>` öğesi:


[!code-xml[Main](master-detail-filtering-across-two-pages-vb/samples/sample1.xml)]

> [!NOTE]
> Yeni ASP.NET ekleme kullanarak sayfaları, site haritası güncelleştirme işlemini otomatikleştirmek [K. Scott Allen](http://odetocode.com/Blogs/scott/)kullanıcının ücretsiz Visual Studio [Site Haritası makrosu](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx).


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>2. adım: tedarikçi listesini görüntüleme`SupplierListMaster.aspx`

İle `SupplierListMaster.aspx` ve `ProductsForSupplierDetails.aspx` oluşturulan sayfaları, sonraki adımımız oluşturmaktır sağlayıcıları GridView `SupplierListMaster.aspx`. GridView sayfaya ekleyin ve için yeni bir ObjectDataSource bağlayın. Bu ObjectDataSource kullanması gereken `SuppliersBLL` sınıfın `GetSuppliers()` tüm Üreticiler döndürmek için yöntemi.


[![SuppliersBLL sınıfı seçin](master-detail-filtering-across-two-pages-vb/_static/image3.png)](master-detail-filtering-across-two-pages-vb/_static/image2.png)

**Şekil 2**: seçin `SuppliersBLL` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image4.png))


[![ObjectDataSource GetSuppliers() yöntemi kullanmak üzere yapılandırma](master-detail-filtering-across-two-pages-vb/_static/image6.png)](master-detail-filtering-across-two-pages-vb/_static/image5.png)

**Şekil 3**: ObjectDataSource kullanılacak yapılandırma `GetSuppliers()` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image7.png))


Bir bağlantı eklemek ihtiyacımız ürünleri görüntüle her GridView satırında, tıklandığında başlıklı kullanıcıyı götürür `ProductsForSupplierDetails.aspx` seçilen satırın 's geçirme `SupplierID` aracılığıyla sorgu dizesi değeri. Örneğin, kullanıcı Tokyo Traders tedarikçi ürünleri görüntüle bağlantısına tıkladığında (bulunduğu bir `SupplierID` değeri 4), için gönderilmelidir `ProductsForSupplierDetails.aspx?SupplierID=4`.

Bunu gerçekleştirmek için ekleme bir [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) her GridView satır için bir köprü ekler GridView'a. GridView'ın akıllı etiketinde sütunları Düzenle bağlantısını tıklatarak başlatın. Ardından, sol üstteki listede HyperLinkField seçin ve HyperLinkField GridView'ın alan listesinde içermek için Ekle düğmesini tıklatın.


[![GridView'a bir HyperLinkField Ekle](master-detail-filtering-across-two-pages-vb/_static/image9.png)](master-detail-filtering-across-two-pages-vb/_static/image8.png)

**Şekil 4**: bir HyperLinkField GridView'a ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image10.png))


Aynı metni HyperLinkField yapılandırılabilir veya URL bağlantıdaki her GridView satır değerleri veya bu değerleri her belirli bir satır için ilişkili veri değerlerini temel alabilir. Tüm satırlar boyunca değeri statik belirtmek için HyperLinkField'ın kullanın `Text` veya `NavigateUrl` özellikleri. HyperLinkField'ın ayarlanması bağlantı metni, tüm satırların aynı olmasını istiyoruz, `Text` özelliğini ürünleri görüntüle.


[![HyperLinkField'ın Text özelliğinin ayarlanacağı için ürünleri görüntüle](master-detail-filtering-across-two-pages-vb/_static/image12.png)](master-detail-filtering-across-two-pages-vb/_static/image11.png)

**Şekil 5**: HyperLinkField'ın ayarlamak `Text` ürünleri görüntüle özelliğini ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image13.png))


Metin veya GridView satır bağlı temel alınan verilerin dayanması için URL değerleri ayarlamak için metin veri alanları veya URL değerleri oluşan bir derleme içinden belirtin `DataTextField` veya `DataNavigateUrlFields` özellikleri. `DataTextField` yalnızca tek bir veri alanı için ayarlanabilir. `DataNavigateUrlFields`, ancak bir virgülle ayrılmış veri alanlarında listesine ayarlanabilir. Sık metin ya da geçerli sıranın veri alan değeri ve bazı static işaretleme birleşimi URL'yi temel ihtiyacımız var. Bu öğreticide, örneğin, URL HyperLinkField'ın bağlantı olmasını istiyoruz `ProductsForSupplierDetails.aspx?SupplierID=supplierID`burada *`supplierID`* her GridView'ın sıranın olan `SupplierID` değeri. Statik ihtiyacımız ve veri odaklı burada değerleri dikkat edin: `ProductsForSupplierDetails.aspx?SupplierID=` bağlantının URL'SİNDE bölümüdür statik ise *`supplierID`* bölümü verilerle her satırın kendi değerini olduğu gibi `SupplierID` değeri.

Statik ve veri odaklı değerlerinin bir birleşimini belirtmek için kullanın `DataTextFormatString` ve `DataNavigateUrlFormatString` özellikleri. Bu özellikler, static işaretleme gerektiği şekilde girin ve ardından işaret `{0}` belirtilen alanın değerini istediğiniz `DataTextField` veya `DataNavigateUrlFields` özellikler görünür. Varsa `DataNavigateUrlFields` özelliğine sahip birden çok alanlar belirtilen kullanım `{0}` eklenen ilk alan değeri istediğiniz yerde `{1}` ikinci alan değeri ve benzeri.

Bu öğreticisi için uygulama, ayarlanacak ihtiyacımız `DataNavigateUrlFields` özelliğini `SupplierID`, bu değer satır içi olarak özelleştirmek için ihtiyacımız veri alanı olduğundan ve `DataNavigateUrlFormatString` özelliğini `ProductsForSupplierDetails.aspx?SupplierID={0}`.


[![HyperLinkField satýrýnSupplierID göre uygun bağlantı URL'si eklemek için yapılandırma](master-detail-filtering-across-two-pages-vb/_static/image15.png)](master-detail-filtering-across-two-pages-vb/_static/image14.png)

**Şekil 6**: uygun bağlantı URL'si tabanlı bağlı içerecek şekilde HyperLinkField yapılandırma `SupplierID` ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image16.png))


HyperLinkField ekledikten sonra özelleştirme ve GridView'ın alanları yeniden sıralama çekinmeyin. Bazı küçük alan düzeyi özelleştirmeleri yapmış olduğunuz sonra aşağıdaki biçimlendirme GridView gösterir.


[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample2.aspx)]

Görüntülemek için bir dakikanızı ayırın `SupplierListMaster.aspx` tarayıcısından sayfası. Şekil 7 gösterildiği gibi sayfa şu anda tüm ürünleri görüntüle bağlantısının da tedarikçileri listeler. Görünüm ürünlerde tıklayarak bağlantı gideceksiniz `ProductsForSupplierDetails.aspx`, tedarikçi boyunca geçen `SupplierID` sorgu dizesi içinde.


[![Bir görünüm ürünleri bağlantısı her tedarikçi satır içerir](master-detail-filtering-across-two-pages-vb/_static/image18.png)](master-detail-filtering-across-two-pages-vb/_static/image17.png)

**Şekil 7**: görünüm ürünleri bağlantısı her tedarikçi satır içerir ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>3. adım: tedarikçi ürünleri listeleme`ProductsForSupplierDetails.aspx`

Bu noktada `SupplierListMaster.aspx` sayfa kullanıcılara gönderdiği `ProductsForSupplierDetails.aspx`, seçili tedarikçi geçirme `SupplierID` sorgu dizesi içinde. GridView içinde ürünleri görüntülemek için öğreticinin son adımı olan `ProductsForSupplierDetails.aspx` olan `SupplierID` eşittir `SupplierID` sorgu dizesinde geçirilen. GridView'a ekleyerek bu başlangıç yapmanın `ProductsForSupplierDetails.aspx` adlı yeni bir ObjectDataSource denetimi kullanarak, sayfa `ProductsBySupplierDataSource` , çağıran `GetProductsBySupplierID(supplierID)` yönteminden `ProductsBLL` sınıfı.


[![ProductsBySupplierDataSource adlı yeni bir ObjectDataSource Ekle](master-detail-filtering-across-two-pages-vb/_static/image21.png)](master-detail-filtering-across-two-pages-vb/_static/image20.png)

**Şekil 8**: yeni ObjectDataSource adlandırılmış ekleme `ProductsBySupplierDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image22.png))


[![ProductsBLL sınıfı seçin](master-detail-filtering-across-two-pages-vb/_static/image24.png)](master-detail-filtering-across-two-pages-vb/_static/image23.png)

**Şekil 9**: seçin `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image25.png))


[![GetProductsBySupplierID(supplierID) yöntemi Çağır ObjectDataSource sahip](master-detail-filtering-across-two-pages-vb/_static/image27.png)](master-detail-filtering-across-two-pages-vb/_static/image26.png)

**Şekil 10**: ObjectDataSource çağırma sahip `GetProductsBySupplierID(supplierID)` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image28.png))


Veri Kaynağı Yapılandırma Sihirbazı'nın son adım kaynağını sağlamamız ister `GetProductsBySupplierID(supplierID)` yöntemin *`supplierID`* parametresi. Sorgu dizesi değerini kullanmak için parametre kaynağı sorgu dizesine ayarlayın ve QueryStringField metin kutusunda kullanılacak sorgu dizesi değeri adını girin (`SupplierID`).


[![Parametre değeri SupplierID sorgu dizesi değerinden satýrýnSupplierID Doldur](master-detail-filtering-across-two-pages-vb/_static/image30.png)](master-detail-filtering-across-two-pages-vb/_static/image29.png)

**Şekil 11**: doldurmak *`supplierID`* parametresi değerinden `SupplierID` sorgu dizesi değeri ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image31.png))


Tüm İşte bu kadar kolay! Şekil 12 gösterir `ProductsForSupplierDetails.aspx` sayfasında Tokyo Traders bağlantıyı tıklatarak ziyaret edildiğinde `SupplierListMaster.aspx`.


[![Tokyo Traders tarafından sağlanan ürün gösterilir](master-detail-filtering-across-two-pages-vb/_static/image33.png)](master-detail-filtering-across-two-pages-vb/_static/image32.png)

**Şekil 12**: Tokyo Traders tarafından sağlanan ürün gösterilir ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Sağlayıcı bilgileri görüntüleme`ProductsForSupplierDetails.aspx`

Şekil 12 gösterildiği gibi `ProductsForSupplierDetails.aspx` sayfası, yalnızca tarafından sağlanan ürünleri listeler `SupplierID` sorgu dizesinde belirtilen. Birisi bu sayfasına doğrudan gönderilir ancak Şekil 12 Tokyo Traders ürünleri gösterildiğini bilmez. Bu sorunu gidermek için biz sağlayıcı bilgileri bu sayfasında görüntüleyebilirsiniz.

Bir FormView'da GridView ürünleri yukarıda ekleyerek başlayın. Adlı yeni bir ObjectDataSource denetimi oluşturma `SuppliersDataSource` , çağıran `SuppliersBLL` sınıfın `GetSupplierBySupplierID(supplierID)` yöntemi.


[![SuppliersBLL sınıfı seçin](master-detail-filtering-across-two-pages-vb/_static/image36.png)](master-detail-filtering-across-two-pages-vb/_static/image35.png)

**Şekil 13**: seçin `SuppliersBLL` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image37.png))


[![GetSupplierBySupplierID(supplierID) yöntemi Çağır ObjectDataSource sahip](master-detail-filtering-across-two-pages-vb/_static/image39.png)](master-detail-filtering-across-two-pages-vb/_static/image38.png)

**Şekil 14**: ObjectDataSource çağırma sahip `GetSupplierBySupplierID(supplierID)` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image40.png))


Olduğu gibi `ProductsBySupplierDataSource`, sahip *`supplierID`* parametre değerini atanmış `SupplierID` sorgu dizesi değeri.


[![Parametre değeri SupplierID sorgu dizesi değerinden satýrýnSupplierID Doldur](master-detail-filtering-across-two-pages-vb/_static/image42.png)](master-detail-filtering-across-two-pages-vb/_static/image41.png)

**Şekil 15**: doldurmak *`supplierID`* parametresi değerinden `SupplierID` sorgu dizesi değeri ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image43.png))


FormView Tasarım görünümünde ObjectDataSource bağlanırken, Visual Studio otomatik olarak FormView oluşturma `ItemTemplate`, `InsertItemTemplate`, ve `EditItemTemplate` tarafından döndürülen veri alanların her biri için herhangi bir etiket ve metin kutusu Web denetimlerle ObjectDataSource. Biz yalnızca bir sağlayıcı bilgileri kullanımında kaldırmak ücretsiz görüntülemek istediğiniz beri `InsertItemTemplate` ve `EditItemTemplate`. Ardından, tedarikçi şirket adını görüntüler ItemTemplate düzenleyin bir `<h3>` öğesi ve adres, şehir, ülke ve telefon numarası şirket adı altında. FormView alternatif olarak, el ile ayarlayabilirsiniz `DataSourceID` oluşturup `ItemTemplate` biçimlendirme geri yaptığımız gibi "[görüntüleyen veri ile ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)" öğretici.

FormView bildirim temelli biçimlendirme bu düzenlemeler sonra aşağıdakine benzer görünmelidir:


[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample3.aspx)]

Şekil 16 gösteren ekran görüntüsü `ProductsForSupplierDetails.aspx` sayfasında sonra yukarıda ayrıntılı tedarikçi bilgiler eklenmiştir.


[![Tedarikçi hakkında bir Özet ürünlerin listesini içerir](master-detail-filtering-across-two-pages-vb/_static/image45.png)](master-detail-filtering-across-two-pages-vb/_static/image44.png)

**Şekil 16**: ürünlerin listesini içeren bir Özet hakkında tedarikçi ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>En son uygulama dokunduğu için`ProductsForSupplierDetails.aspx`kullanıcı Arabirimi

Deneyimi var. Bu rapor için kullanıcı artırmak için birkaç biz yapmanız gereken eklemeleri olan `ProductsForSupplierDetails.aspx` sayfası. Şu an bir kullanıcı Git tek yolu `ProductsForSupplierDetails.aspx` sağlayıcıları listesi sayfasına geri gelir, tarayıcınızın geri düğmesine tıklayın. Bir HyperLink denetimi için ekleyelim `ProductsForSupplierDetails.aspx` bağlantıları geri sayfa `SupplierListMaster.aspx`, kullanıcının ana listesine dönmek başka bir yol sağlama.


[![Kullanıcı için SupplierListMaster.aspx geri almak için köprü denetim ekleme](master-detail-filtering-across-two-pages-vb/_static/image48.png)](master-detail-filtering-across-two-pages-vb/_static/image47.png)

**Şekil 17**: kullanıcı geri almak için bir köprü denetimini ekleme `SupplierListMaster.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image49.png))


Kullanıcı için herhangi bir ürün yüklü olmayan bir tedarikçi ürünleri görüntüle bağlantısına tıklarsa `ProductsBySupplierDataSource` ObjectDataSource içinde `ProductsForSupplierDetails.aspx` herhangi bir sonuç döndürmek olmaz. ObjectDataSource için bağlı GridView kullanıcının tarayıcısında sayfasında boş bir bölgede kaynaklanan tüm biçimlendirme işlemeyeceğini. Seçili sağlayıcı ile ilişkili ürün olduğunu kullanıcıya daha net bir şekilde kurmak biz GridView'ın ayarlayabilirsiniz `EmptyDataText` özelliğini iletinin sağlanamadığı duyduğunuzda görüntülenen istiyoruz. "Bu sağlayıcısı tarafından sağlanan ürün bulunur" için bu özelliği ayarladım

Varsayılan olarak, en az bir ürün kategoriye veritabanındaki tüm Üreticiler sağlar. Ancak, Bu öğretici için el ile değiştirdim `Products` böylece Escargots Nouveaux tedarikçi artık tüm ürünleri ile ilişkili olmayan tablo. Bu değişiklik yapıldıktan sonra Şekil 18 Escargots Nouveaux için Ayrıntılar sayfası gösterilir.


[![Tedarikçi ürünlerden sağlamaz kullanıcılar bilgilendirildi](master-detail-filtering-across-two-pages-vb/_static/image51.png)](master-detail-filtering-across-two-pages-vb/_static/image50.png)

**Şekil 18**: kullanıcılar bilgilendirildi tedarikçi ürünlerden sağlamaz ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-across-two-pages-vb/_static/image52.png))


## <a name="summary"></a>Özet

Ana/ayrıntı raporları, tek bir sayfaya hem ana hem de ayrıntılı kayıtları görüntüleyebilirsiniz, ancak birçok Web Siteleri'nde, iki web sayfaları arasında ayrılır. Bu öğreticide nasıl GridView "ana" web sayfasında listelenen üreticiler ve ilişkili ürünleri "Ayrıntılar" sayfasında listelenen sağlayarak böyle bir ana/ayrıntı raporu uygulanacağı inceledik. Sıranın geçirilen ayrıntıları sayfasına bağlantı ana web sayfasını her tedarikçi satırda bulunan `SupplierID` değeri. GridView'ın HyperLinkField kullanarak satır özgü bağlantılarını kolayca eklenebilir.

Ayrıntılar sayfasında belirtilen sağlayıcı için bu ürünlerin alınırken çağırarak gerçekleştirilmiştir `ProductsBLL` sınıfın `GetProductsBySupplierID(supplierID)` yöntemi. *`supplierID`* Bildirimli olarak parametre kaynağı olarak bir sorgu dizesi kullanarak parametre değeri belirtildi. Ayrıca Ayrıntılar sayfasından bir FormView'da kullanma tedarikçi ayrıntılarını görüntülemek nasıl inceledik.

Bizim [sonraki öğreticiye](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) ana/ayrıntı raporlarda son sunucudur. GridView seçme düğmesi sahip olduğu her satır ürünlerin listesini görüntülemek nasıl inceleyeceğiz. Select düğmesine tıklayarak bu ürün uygulamasının Ayrıntılar aynı sayfa üzerinde bir DetailsView denetiminde görüntüler.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Hilton Giesenow oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](master-detail-filtering-with-two-dropdownlists-vb.md)
> [İleri](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)
