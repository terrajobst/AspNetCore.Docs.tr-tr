---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
title: Ana/ayrıntı DropDownList (VB) ile filtreleme | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, tek bir web sayfasındaki 'ana' kaydeder ve bir DataList arak Göster için görüntülenecek DropDownLists kullanarak ana/ayrıntı raporları görüntülemek nasıl bakın …
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: ad0f1014-1eff-465f-bdc6-93058de00e44
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: e4ece466319e268a74bbe8c4ed96ffc33cff432f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880693"
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Ana/ayrıntı DropDownList (VB) ile filtreleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_VB.exe) veya [PDF indirin](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/datatutorial33vb1.pdf)

> Bu öğreticide tek web sayfasında "ana" kaydeder ve "Ayrıntılar" görüntülemek için bir DataList görüntülenecek DropDownLists kullanarak ana/ayrıntı raporları görüntülemek nasıl bakın.


## <a name="introduction"></a>Giriş

GridView önceki kullanarak oluşturduğumuz ilk ana/ayrıntı raporu [ana/ayrıntı filtreleme ile bir DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) öğreticide, bazı "ana" kayıt kümesi göstererek başlar. Kullanıcı daha sonra ana kayıtları birine ana kayıt "ayrıntılarını." böylece görüntüleme ayrıntıya inebilir Ana/ayrıntı raporlar-çok ilişkileri Görselleştirme ve özellikle "geniş" tabloları (olanları sütunları pek çok) ayrıntılı bilgileri görüntülemek için ideal seçim içindir. Biz, önceki eğitimlerine GridView ve DetailsView denetimlerini kullanarak ana/ayrıntı raporları uygulamak nasıl incelediniz. Bu öğretici ve sonraki iki, biz durum, DataList kullanarak odaklanmasına ancak bu kavramlar yeniden gözden geçirin ve bunun yerine yineleyici denetler.

Bu öğreticide, bir DataList görüntülenen "Ayrıntılar" kayıt "ana" kayıtlarıyla içerecek şekilde bir DropDownList kullanarak göreceğiz.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>1. adım: ana/ayrıntı öğretici Web sayfaları ekleme

Şimdi biz Bu öğretici başlamadan önce ilk ASP.NET sayfaları biz Bu öğreticide ve DataList ve yineleyici denetimlerini kullanarak ana/ayrıntı raporları ile ilgili sonraki iki gerekir ve klasörü eklemek için bir dakikanızı ayırın. Başlangıç adlı projeye yeni bir klasör oluşturarak `DataListRepeaterFiltering`. Ardından, aşağıdaki beş ASP.NET sayfaları tümünün ana sayfa kullanacak şekilde yapılandırılmış olan bu klasöre eklemek `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![DataListRepeaterFiltering bir klasör oluşturun ve öğretici ASP.NET sayfaları ekleme](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image1.png)

**Şekil 1**: oluşturmak bir `DataListRepeaterFiltering` klasörü ve öğretici ASP.NET sayfaları ekleme


Ardından, açık `Default.aspx` sayfasında ve sürükleyin `SectionLevelTutorialListing.ascx` kullanıcı denetiminden `UserControls` tasarım yüzeyine klasör. Bu kullanıcı, içinde oluşturduğumuz denetimi [ana sayfalar ve Site gezintisi](../introduction/master-pages-and-site-navigation-vb.md) öğretici, site haritası numaralandırır ve madde işaretli listede geçerli bölümünden öğreticileri görüntüler.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image2.png)

**Şekil 2**: eklemek `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image4.png))


Madde işaretli liste görünümünü olması için biz oluşturma, ana/ayrıntı öğreticileri biz bunları site haritası eklemeniz gerekir. Açık `Web.sitemap` dosya ve sonra "Görüntüleyen veri ile DataList ve yineleyici" site haritası düğümü biçimlendirme aşağıdaki biçimlendirme ekleyin:

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample1.xml)]


![Yeni ASP.NET sayfaları dahil etmek için Site Haritası güncelleştir](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image5.png)

**Şekil 3**: yeni ASP.NET sayfaları dahil etmek için Site Haritası güncelleştir


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>2. adım: bir DropDownList kategorileri görüntüleme

Bizim ana/ayrıntı raporu görüntülenen seçilen liste öğesi'nin ürünleriyle bir DropDownList kategorilerde listeleyecek DataList sayfasında aşağı daha fazla. Ardından, bize, önünde ilk görev bir DropDownList görüntülenen kategorileri sağlamaktır. Başlangıç açarak `FilterByDropDownList.aspx` sayfasındaki `DataListRepeaterFiltering` klasörü ve sayfanın tasarımcıya araç kutusu'ndan bir DropDownList sürükleyin. Ardından, DropDownList's ayarlayın `ID` özelliğine `Categories`. DropDownList'ın Akıllı Etiket Veri Kaynağı Seç bağlantısından tıklayın ve adlı yeni bir ObjectDataSource oluşturma `CategoriesDataSource`.


[![CategoriesDataSource adlı yeni bir ObjectDataSource ekleme](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image6.png)

**Şekil 4**: yeni ObjectDataSource adlandırılmış eklemek `CategoriesDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image8.png))


Yeni ObjectDataSource çağırılır gibi yapılandırma `CategoriesBLL` sınıfının `GetCategories()` yöntemi. Hala hangi veri kaynağı alanı DropDownList görüntüleneceğini ve hangi belirtmek için ihtiyacımız ObjectDataSource yapılandırdıktan sonra bir her liste öğesi için değer olarak ilişkili olmalıdır. Sahip `CategoryName` görüntü olarak alan ve `CategoryID` her liste öğesi için değer olarak.


[![CategoryName alan ve kullanım adlı kullanıcı, Categoryıd'si DropDownList görünen değere sahip](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image9.png)

**Şekil 5**: DropDownList görüntü sahip `CategoryName` alan ve kullanım `CategoryID` değeri olarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image11.png))


Kayıtlardan doldurulur DropDownList denetimi bu noktada sahibiz `Categories` tablosu (tüm yaklaşık altı saniye içinde gerçekleştirilir). Şekil 6 bizim ilerleme bugüne kadarki bir tarayıcıdan görüntülendiğinde gösterir.


[![Geçerli kategorileri bir aşağı açılır listeler](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image12.png)

**Şekil 6**: A aşağı açılan listeler geçerli kategorileri ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>2. adım: ürünleri DataList ekleme

Bizim ana/ayrıntı raporu son adımda seçilen kategori ile ilişkili ürün listesi var. Bunu başarmak için sayfaya bir DataList ekleyin ve adlı yeni bir ObjectDataSource oluşturma `ProductsByCategoryDataSource`. Sahip `ProductsByCategoryDataSource` denetim almak, verileri `ProductsBLL` sınıfının `GetProductsByCategoryID(categoryID)` yöntemi. Bu ana/ayrıntı raporu salt okunur olduğundan, (hiçbiri) INSERT, UPDATE ve DELETE sekmeleri seçeneğini seçin.


[![GetProductsByCategoryID(categoryID) yöntemini seçin](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image15.png)

**Şekil 7**: seçin `GetProductsByCategoryID(categoryID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image17.png))


İleri'yi tıklatmadan sonra ObjectDataSource Sihirbazı bize kaynağı değeri ile ister `GetProductsByCategoryID(categoryID)` yöntemin *`categoryID`* parametresi. Seçili değerini kullanacak şekilde `categories` DropDownList öğesi kümesine parametre kaynak denetimi ve ControlId `Categories`.


[![Adlı kullanıcı, Categoryıd'si parametresi kategorileri DropDownList değerine ayarlayın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image18.png)

**Şekil 8**: ayarlamak *`categoryID`* parametre değerine `Categories` DropDownList ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image20.png))


Veri Kaynağı Yapılandırma Sihirbazı tamamlandıktan sonra Visual Studio otomatik olarak oluşturacak bir `ItemTemplate` adını ve her veri alanı değerini görüntüler DataList için. Şimdi kullanmayı DataList geliştirmek bir `ItemTemplate` yalnızca ürün adı, kategori, tedarikçi, birim ve fiyat ile birlikte başına miktar görüntüleyen bir `SeparatorTemplate` , yerleştirir bir `<hr>` her bir öğe arasındaki öğesi. I kullanacağım `ItemTemplate` gelen bir örnekte [DataList ve yineleyici denetimleri ile veri görüntüleme](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb.md) öğretici ancak kullanımında en görsel olarak çekici Bul ne olursa olsun şablon biçimlendirme kullanmak boş.

Bu değişiklikleri yaptıktan sonra DataList ve alt ObjectDataSource'nın biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample2.aspx)]

Bir tarayıcıda bizim ilerleme kullanıma için bir dakikanızı ayırın. İlk sayfasını ziyaret ettiğinizde, seçilen kategoriye (Meşrubat) ait bu ürünlerden (Şekil 9'da gösterildiği gibi) görüntülenir, ancak DropDownList değiştirme veri güncelleştirmez. Geri gönderimin DataList güncelleştirmek oluşması gereken olmasıdır. Bunu ya da geçebiliriz gerçekleştirmek için DropDownList's ayarlamak `AutoPostBack` özelliğine `true` veya bir düğme Web denetimi sayfasına ekleyin. Bu öğretici için ı DropDownList's ayarlamak için tercih `AutoPostBack` özelliğine `true`.

Şekil 9 ve 10 eylem ana/ayrıntı raporu gösterilmektedir.


[![İlk sitesini ziyaret ettiğinde içecek ürünleri görüntülenir](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image21.png)

**Şekil 9**: ilk sitesini ziyaret ettiğinde içecek ürünleri görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image23.png))


[![Yeni bir ürün (üretim) otomatik olarak seçme DataList güncelleştirme geri gönderme neden olur.](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image24.png)

**Şekil 10**: yeni bir ürün (üretim) otomatik olarak belirlenmesi DataList güncelleştirme geri gönderme neden olur ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>"--Bir kategori seçin--" liste öğesi ekleme

İlk kez ziyaret eden `FilterByDropDownList.aspx` DropDownList'ın ilk liste öğesi (Meşrubat) DataList içecek ürünleri gösteren varsayılan olarak, seçili kategorileri sayfa. İçinde *ana/ayrıntı filtreleme ile bir DropDownList* öğreticide eklediğimiz "--bir kategori seçin--" seçeneği, varsayılan olarak seçilidir ve, seçili olduğunda, görüntülenen DropDownList *tüm* , veritabanındaki ürün. Her ürün satır küçük bir miktar ekran Gayrimenkul sürdü gibi bu tür bir yaklaşım GridView, ürünlerinde listelerken yönetilebilir. DataList ile ancak, bir çok daha büyük öbek ekranın her ürünün bilgileri kullanır. Hala şimdi "--bir kategori seçin--" seçeneğini ekleyin ve varsayılan olarak seçili olması, ancak hiç ürün gösterir, tüm ürünleri göster sahip olmak yerine seçili olduğunda, şimdi onu şekilde yapılandırın.

DropDownList için yeni bir liste öğesi eklemek için Özellikler penceresini gidin ve içinde üç noktaya tıklayın `Items` özelliği. İle yeni bir liste öğesi ekleme `Text` "--bir kategori seçin--" ve `Value` `0`.


![Ekleme bir](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image27.png)

**Şekil 11**: "--bir kategori seçin--" liste öğesi ekleme


Alternatif olarak, aşağıdaki biçimlendirme DropDownList ekleyerek liste öğesi ekleyebilirsiniz:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample3.aspx)]

Ayrıca, DropDownList denetimin ayarlamak ihtiyacımız `AppendDataBoundItems` için `true` çünkü bunu ayarlanmışsa `false` (varsayılan), kategoriler DropDownList ObjectDataSource bağlı herhangi bir el ile eklenen listeyi üzerine öğeler.


![AppendDataBoundItems özelliğini True olarak ayarlayın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image28.png)

**Şekil 12**: ayarlamak `AppendDataBoundItems` özelliğinin True


Değer seçtik nedeni `0` değerini sistemiyle kategori yok olduğundan "--bir kategori seçin--" listesi için öğedir `0`, "--bir kategori seçin--" liste öğesi seçildiğinde bu nedenle hiçbir ürün kaydı döndürülür. Bunu doğrulamak için bir tarayıcı aracılığıyla sayfasını ziyaret edin için bir dakikanızı ayırın. Şekil 13 gösterildiği başlangıçta sayfanın görüntüleme "--bir kategori seçin--" liste öğesi seçildiğinde ve hiç ürün görüntülenir.


[![Zaman](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image29.png)

**Şekil 13**: "--bir kategori seçin--" liste öğesi seçildiğinde, yok ürünleri görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image31.png))


Bunun yerine görüntüleyecektir varsa *tüm* "--bir kategori seçin--" seçeneği seçildiğinde, ürünlerin değeri kullanın `-1` yerine. Akıllı duruma okuyucu o arka planda geri çağırma *ana/ayrıntı filtreleme ile bir DropDownList* biz güncelleştirilmiş öğretici `ProductsBLL` sınıfının `GetProductsByCategoryID(categoryID)` yöntemi böylece, bir *`categoryID`* değeri `-1` dönen kayıt tüm ürün, geçirildi.

## <a name="summary"></a>Özet

Hiyerarşik olarak ilgili verileri görüntülerken, bu genellikle alınacağı kullanıcı hiyerarşinin en üst verilerden harcadığı başlatmak ve ayrıntılara detaya ana/ayrıntı raporları kullanarak verileri sunmak için yardımcı olur. Bu öğreticide seçili kategorinin ürünleri gösteren bir basit ana/ayrıntı raporu oluşturma incelendi. Bu, kategoriler ve seçili kategorisine ait ürünleri için DataList listesi için bir DropDownList kullanarak gerçekleştirilmiştir.

Sonraki öğreticide ana ve ayrıntı kayıtları iki sayfalara ayırarak adresindeki göreceğiz. İlk sayfa "ana" kayıtların listesini, ayrıntılarını görüntülemek için bir bağlantı görüntülenir. Bağlantıyı tıklatmak seçili ana kaydı için ayrıntıları görüntüleyecek ikinci sayfasında, kullanıcıya whisk.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler...

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Randy Etikan oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
> [sonraki](master-detail-filtering-acess-two-pages-datalist-vb.md)
