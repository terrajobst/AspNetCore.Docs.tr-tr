---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
title: Ana/ayrıntı filtreleme (VB) ile bir DropDownList | Microsoft Docs
author: rick-anderson
description: Bu öğreticide tek bir web sayfasındaki 'master' kayıtları ve DataList arak Göster için görüntülenecek DropDownList kullanarak ana/ayrıntı raporları görüntülemek nasıl görüyoruz...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: ad0f1014-1eff-465f-bdc6-93058de00e44
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: e88a7459529e003b73ef5a42456de3501e3db461
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829327"
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Ana/ayrıntı filtreleme (VB) bir DropDownList ile
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_VB.exe) veya [PDF olarak indirin](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/datatutorial33vb1.pdf)

> Bu öğreticide tek web sayfasında "ana" kayıtları ve "details" görüntülemek için bir DataList görüntülenecek DropDownList kullanarak ana/ayrıntı raporları görüntülemek nasıl görüyoruz.


## <a name="introduction"></a>Giriş

GridView önceki bölümlerinde kullanarak oluşturduğumuz ilk ana/ayrıntı raporu [ana/ayrıntı filtreleme ile bir DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) öğretici, bazı "ana" kayıt kümesini göstererek başlar. Kullanıcı daha sonra bir ana kayıtlar, böylece ana kayıt "ayrıntılarını." görüntüleme detaya gidebilirsiniz Ana/ayrıntı raporları bir-çok ilişkileri görselleştirmek için ve özellikle "geniş" tabloları (olanları çok sütun) öğesinden ayrıntılı bilgiler görüntülemek için ideal seçenektir. Biz nasıl uygulanacağını önceki öğreticilerde GridView ve DetailsView denetimlerini kullanarak ana/ayrıntı raporları incelediniz. Bu öğreticide ve sonraki iki, biz DataList kullanma odak ancak bu kavramlar edilemeyeceğini ve yineleyici yerine denetler.

Bu öğreticide, içinde bir DataList görüntülenen "details" kayıt "ana" kayıtlarla içerecek şekilde bir DropDownList kullanarak inceleyeceğiz.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>1. adım: ana/ayrıntı Eğitmen Web sayfaları ekleme

Biz bu öğreticiye başlamadan önce öncelikle Bu öğretici ve DataList ve Repeater denetimleri kullanarak ana/ayrıntı raporlarla ilgili sonraki iki gerekir ASP.NET sayfaları ve klasörü eklemek için bir zaman ayırabiliriz. Adlı projede yeni bir klasör oluşturarak başlayın `DataListRepeaterFiltering`. Ardından, tüm bunları ana sayfaya kullanacak şekilde yapılandırılmış olması ve bu klasörü için aşağıdaki beş ASP.NET sayfaları ekleyin `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![DataListRepeaterFiltering bir klasör oluşturun ve öğretici ASP.NET sayfaları ekleme](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image1.png)

**Şekil 1**: oluşturma bir `DataListRepeaterFiltering` klasörü ve öğretici ASP.NET sayfaları ekleyin


Ardından, açık `Default.aspx` sürükleyin ve sayfa `SectionLevelTutorialListing.ascx` kullanıcı denetimi `UserControls` tasarım yüzeyine klasör. Bu kullanıcı, oluşturduğumuz denetimini [ana sayfalar ve Site gezintisi](../introduction/master-pages-and-site-navigation-vb.md) öğretici, site haritası numaralandırır ve madde işaretli listede geçerli bölümdeki öğreticiler görüntüler.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image2.png)

**Şekil 2**: ekleme `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image4.png))


Madde işaretli liste görünümünü sahip olmak için biz oluşturursunuz, ana/ayrıntı öğreticiler site eşlemesinin ekleneceği ihtiyacımız var. Açık `Web.sitemap` dosya ve sonra "Görüntüleyen veri ile DataList ve Repeater" site haritası düğüm biçimlendirmeyi aşağıdaki işaretlemeyi ekleyin:

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample1.xml)]


![Yeni ASP.NET sayfaları dahil etmek için Site Haritası güncelleştir](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image5.png)

**Şekil 3**: yeni ASP.NET sayfaları dahil etmek için Site Haritası güncelleştir


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>2. adım: bir DropDownList içinde kategorilerini görüntüleme

Bir DropDownList kategorileri görüntülenen seçili liste öğesinin ürünleri ile ana/ayrıntı raporumuzun listeler başka bir DataList sayfasında aşağı. Ardından bize önce ilk görev bir DropDownList içinde görüntülenen kategorileri sağlamaktır. Başlangıç açarak `FilterByDropDownList.aspx` sayfasını `DataListRepeaterFiltering` klasör ve bir DropDownList sayfanın Tasarımcısı araç kutusundan sürükleyin. Ardından, DropDownList'ın ayarlamak `ID` özelliğini `Categories`. Akıllı etiket DropDownList'ın veri kaynağı Seç bağlantıdan tıklayın ve adlı yeni bir ObjectDataSource oluşturma `CategoriesDataSource`.


[![CategoriesDataSource adlı yeni bir ObjectDataSource Ekle](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image6.png)

**Şekil 4**: yeni ObjectDataSource adlandırılmış ekleme `CategoriesDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image8.png))


Bu çağırır gibi yeni ObjectDataSource yapılandırma `CategoriesBLL` sınıfın `GetCategories()` yöntemi. Biz yine de hangi veri kaynağı alanı DropDownList içinde görüntülenmesi gerekir ve hangi belirtmenize gerek ObjectDataSource yapılandırdıktan sonra bir her liste öğesi için bir değer olarak ilişkili olmalıdır. Sahip `CategoryName` görüntü olarak alan ve `CategoryID` değeri her liste öğesi olarak.


[![CategoryName alan ve kullanım CategoryID DropDownList görünen değere sahip](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image9.png)

**Şekil 5**: DropDownList görüntülemesi `CategoryName` alan ve kullanım `CategoryID` değeri ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image11.png))


Kayıtlardan doldurulur bir DropDownList denetimi bu noktada sahibiz `Categories` tablo (tümü yaklaşık altı saniyeler içinde gerçekleştirilir). Şekil 6 ilerlememizin şimdiye kadarki bir tarayıcıdan görüntülendiğinde gösterir.


[![Bir açılan geçerli kategorileri listeler](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image12.png)

**Şekil 6**: bir açılan listeler geçerli kategorilerin ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>2. adım: ürünleri DataList ekleme

Ana/ayrıntı raporumuzun son adımda, seçilen kategori ile ilişkili ürün listesi sağlamaktır. Bunu gerçekleştirmek için bir DataList sayfaya ekleyin ve adlı yeni bir ObjectDataSource oluşturma `ProductsByCategoryDataSource`. Sahip `ProductsByCategoryDataSource` denetimi alma, verileri `ProductsBLL` sınıfın `GetProductsByCategoryID(categoryID)` yöntemi. Bu ana/ayrıntı raporu salt okunur olduğundan, INSERT, UPDATE ve DELETE sekmeleri (hiçbiri) seçeneğini belirleyin.


[![GetProductsByCategoryID(categoryID) yöntemi seçin](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image15.png)

**Şekil 7**: seçin `GetProductsByCategoryID(categoryID)` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image17.png))


İleri'yi tıklatmadan sonra ObjectDataSource Sihirbazı'nı bize değeri kaynağını ister `GetProductsByCategoryID(categoryID)` yöntemin *`categoryID`* parametresi. Seçili değerini kullanacak şekilde `categories` DropDownList öğesi denetimi ve ControlId için parametre kaynağı ayarla `Categories`.


[![CategoryID parametresi kategorileri DropDownList değerine ayarlayın.](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image18.png)

**Şekil 8**: ayarlayın *`categoryID`* parametre değerine `Categories` DropDownList ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image20.png))


Veri Kaynağı Yapılandırma Sihirbazı tamamlandıktan sonra Visual Studio otomatik olarak oluşturacak bir `ItemTemplate` adını ve her veri alanının değerini görüntüler DataList için. Şimdi kullanmayı DataList geliştiren bir `ItemTemplate` yalnızca ürün adı, kategori, tedarikçi, birim ve fiyat ile birlikte başına miktarını görüntüler bir `SeparatorTemplate` , ekler bir `<hr>` her bir öğe arasındaki öğesi. Kullanmak şuraya atlıyorum `ItemTemplate` içinde bir örnekten [DataList ve Repeater denetimleri ile verileri görüntüleme](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb.md) Öğreticisi, ancak genel görünüm en görsel olarak çekici bulmak istediğiniz şablon biçimlendirme ücretsiz.

Bu değişiklikleri yaptıktan sonra DataList ve kendi ObjectDataSource biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample2.aspx)]

Bir tarayıcıda ilerlememizin kullanıma için bir dakikanızı ayırın. Sayfa ilk ziyaret edildiğinde, seçilen kategori (İçecekler) ait bu ürünlerin (Şekil 9'da gösterildiği gibi) görüntülenir, ancak veri DropDownList değiştirme güncelleştirmez. DataList güncelleştirmek bir geri gönderme gerçekleşmelidir olmasıdır. Ya da yapabiliriz bunu sağlamak için DropDownList'ın ayarlamak `AutoPostBack` özelliğini `true` veya düğme Web Denetimi sayfaya ekleyin. Bu öğreticide, ı DropDownList'ın ayarlanacak bıraktınız `AutoPostBack` özelliğini `true`.

Şekil 9 ve 10 eylem ana/ayrıntı raporu gösterilmektedir.


[![Sayfa ilk ziyaret edildiğinde, içecek ürünleri görüntülenir](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image21.png)

**Şekil 9**: sayfa ilk ziyaret edildiğinde, içecek ürünleri görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image23.png))


[![DataList güncelleştiriliyor, bir geri gönderme neden yeni bir ürün (ürün) otomatik olarak seçme](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image24.png)

**Şekil 10**: yeni bir ürün (ürün) otomatik olarak seçilmesi DataList güncelleştiriliyor, bir geri gönderme neden olur ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>"--Bir kategori seçin--" liste öğesi ekleme

İlk ziyaret edildiğinde `FilterByDropDownList.aspx` DropDownList'ın ilk liste öğesinin (İçecekler) içinde DataList içecek ürünleri gösteren varsayılan olarak seçili kategorileri sayfa. İçinde *ana/ayrıntı filtreleme ile bir DropDownList* öğreticide eklediğimiz bir "--bir kategori seçin--" seçeneği, varsayılan olarak seçilidir ve, seçildiğinde görüntülenen DropDownList *tüm* , veritabanında ürünleri. Küçük bir miktar ekran gerçek boyutunuzu her ürün satır sürdü gibi bu tür bir yaklaşım, GridView ürünleri listelerken yönetilebilir. DataList'i ile ancak çok daha büyük öbek ekranın her ürünün bilgi tüketir. Yine de şimdi "--bir kategori seçin--" bir seçenek ekleyin ve varsayılan olarak seçili olması, ancak hiçbir ürünleri gösterir şekilde tüm ürünleri göster yerine seçili olduğunda, şimdi yapılandırın.

DropDownList'e yeni bir liste öğesi eklemek için özellikler penceresine gidin ve içinde üç noktaya tıklayarak `Items` özelliği. İle yeni bir liste öğesi ekleme `Text` "--bir kategori seçin--" ve `Value` `0`.


![Ekleme bir](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image27.png)

**Şekil 11**: "--bir kategori seçin--" liste öğesi ekleme


Alternatif olarak, aşağıdaki biçimlendirme DropDownList'e ekleyerek liste öğesi ekleyebilirsiniz:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample3.aspx)]

Ayrıca, DropDownList denetimin ayarlamak ihtiyacımız `AppendDataBoundItems` için `true` çünkü bu ayarlanırsa `false` (varsayılan), kategorileri ObjectDataSource DropDownList'e bağlandığında bunlar herhangi bir el ile eklenen listeyi şunun üzerine yazacağız öğeleri.


![AppendDataBoundItems özelliğini True olarak ayarlayın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image28.png)

**Şekil 12**: ayarlayın `AppendDataBoundItems` özelliği true


Değer seçtik nedeni `0` değerini sistemiyle kategori olduğundan için "--bir kategori seçin--" listesi öğesidir `0`, "--bir kategori seçin--" liste öğesi seçildiğinde bu nedenle hiçbir ürün kayıtlar döndürülür. Bunu doğrulamak için bir tarayıcı aracılığıyla sayfayı ziyaret etmek için bir dakikanızı ayırarak. Şekil 13 gösterildiği başlangıçta sayfa görüntüleme "--bir kategori seçin--" liste öğesi seçildiğinden ve ürün görüntülenir.


[![Zaman](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image29.png)

**Şekil 13**: "--bir kategori seçin--" liste öğesi seçildiğinde, yok ürünleri görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image31.png))


Bunun yerine görüntüleyebilir, *tüm* "--bir kategori seçin--" seçeneği seçildiğinde, ürünlerin, değerini kullanın. `-1` yerine. Kurnaz Okuyucu, arka planda geri çağırma *ana/ayrıntı filtreleme ile bir DropDownList* güncelleştirdik öğretici `ProductsBLL` sınıfın `GetProductsByCategoryID(categoryID)` yöntemi için bir *`categoryID`* değerini `-1` , tüm ürün kayıtları döndürüldü geçirildi.

## <a name="summary"></a>Özet

Hiyerarşik olarak ilgili verileri görüntülerken, bu genellikle, kullanıcı verileri hiyerarşisinin üstünde harcadığı başlatabilir ve aşağı detaylarına ana/ayrıntı raporları kullanarak verileri sunmak için yardımcı olur. Bu öğreticide, seçilen kategori ürünleri gösteren bir basit ana/ayrıntı rapor oluşturmaya incelenir. Bu bir DropDownList kategorileri ve DataList Seçili kategoriye ait olan ürünlerin listesini kullanarak gerçekleştirilebilir.

İki sayfada ana ve ayrıntıları kayıtları ayırarak sonraki öğreticide inceleyeceğiz. İlk sayfa, "ana" kayıtlarını içeren bir liste, ayrıntılarını görüntülemek için bir bağlantı ile görüntülenir. Bağlantısına tıklayarak, seçili ana kaydın ayrıntılarını görüntüler ikinci sayfasında kullanıcıya whisk.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel performanstan...

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Randy Etikan oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
> [İleri](master-detail-filtering-acess-two-pages-datalist-vb.md)
