---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
title: GridView'ın alt bilgisinde (C#) Özet bilgiler görüntüleme | Microsoft Docs
author: rick-anderson
description: Özet bilgileri genellikle özet satırı raporda sayfanın alt kısmında görüntülenir. GridView denetiminde çekme isteği mümkün olan hücrelere altbilgi satırı ekleyebilirsiniz...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: d50edc31-9286-4c6a-8635-be09e72752a4
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: 34d5dbf4a019f798714f80964789cd85a466b43e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818815"
---
<a name="displaying-summary-information-in-the-gridviews-footer-c"></a>GridView'ın alt bilgisinde (C#) Özet bilgiler görüntüleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_15_CS.exe) veya [PDF olarak indirin](displaying-summary-information-in-the-gridview-s-footer-cs/_static/datatutorial15cs1.pdf)

> Özet bilgileri genellikle özet satırı raporda sayfanın alt kısmında görüntülenir. GridView denetiminde olan hücrelere biz program aracılığıyla veri toplama ekleyebilir altbilgi satır içerebilir. Bu öğreticide bu alt satırdaki verileri görüntüleme göreceğiz.


## <a name="introduction"></a>Giriş

Her ürünlerin Fiyatlar, stoktaki birimleri, birimleri sırası ve yeniden sıralama düzeyleri görmenin yanı sıra, bir kullanıcı de ortalama fiyatını, stoktaki birimleri toplam sayısı gibi toplama bilgileri ilginizi ve benzeri. Gibi özet bilgileri genellikle özet satırı raporda sayfanın alt kısmında görüntülenir. GridView denetiminde olan hücrelere biz program aracılığıyla veri toplama ekleyebilir altbilgi satır içerebilir.

Bu görevi, bize üç zorluklar sunar:

1. GridView'ın alt bilgi satırına görüntülemek için yapılandırma
2. Özet verilerini belirleme; diğer bir deyişle, nasıl biz ortalama fiyat veya stoktaki birimleri toplam işlem?
3. Alt satırın uygun hücrelere özet verilerini ekleme

Bu öğreticide bu zorluklarının üstesinden nasıl göreceğiz. Özellikle, GridView içinde görüntülenen seçili kategorinin ürünleri ile bir açılan listedeki kategorileri listeleyen bir sayfa oluşturacağız. GridView toplam birim sayısı ve ortalama fiyatını, stok ve sipariş bu kategoriye giren ürünlerin gösteren bir alt bilgi satır içerir.


[![GridView'ın alt satırda Özet bilgiler görüntülenir](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image1.png)

**Şekil 1**: Özet bilgi GridView'ın alt bilgi satırı görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image3.png))


Bu öğreticiyle ürünleri ana/ayrıntı arabirimine kategori önceki bölümlerinde ele kavramları bağlı derlemeler [ana/ayrıntı filtreleme ile bir DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) öğretici. Lütfen önceki öğreticide henüz çalıştıysanız, bu raporla etmeden önce bunu.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>1. adım: ürünleri GridView ve kategorileri DropDownList ekleme

GridView'ın alt için Özet bilgiler eklemeye kendimize ilgili önce öncelikle yalnızca ana/ayrıntı raporu oluşturalım. Biz bu ilk adım tamamlandıktan sonra ilgili özet bilgileri içerecek şekilde nasıl inceleyeceğiz.

Başlangıç açarak `SummaryDataInFooter.aspx` sayfasını `CustomFormatting` klasör. Bir DropDownList denetimi ekleyin ve ayarlayın, `ID` için `Categories`. Ardından, akıllı etiket DropDownList'ın veri kaynağı Seç bağlantıdan tıklayın ve adlı yeni bir ObjectDataSource eklemek için iyileştirilmiş `CategoriesDataSource` , çağıran `CategoriesBLL` sınıfın `GetCategories()` yöntemi.


[![CategoriesDataSource adlı yeni bir ObjectDataSource Ekle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image4.png)

**Şekil 2**: yeni ObjectDataSource adlandırılmış ekleme `CategoriesDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image6.png))


[![CategoriesBLL sınıfın GetCategories() yöntemi Çağır ObjectDataSource sahip](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image7.png)

**Şekil 3**: ObjectDataSource çağırma sahip `CategoriesBLL` sınıfın `GetCategories()` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image9.png))


ObjectDataSource yapılandırdıktan sonra sihirbazın bize DropDownList ait veri kaynağı Yapılandırma Sihirbazı'nı hangi veri alan değeri belirtmek ihtiyacımız görüntülenmesi gerekir ve hangisinin DropDownList'ın değerinekarşılıkgelmelidirdöndürür`ListItem` s. Sahip `CategoryName` görüntülenen alan ve kullanım `CategoryID` değeri.


[![CategoryName CategoryID alanları olarak ve değeri ListItems ve metin sırasıyla kullanın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image10.png)

**Şekil 4**: kullanım `CategoryName` ve `CategoryID` olarak alanları `Text` ve `Value` için `ListItem` s, sırasıyla ([tam boyutlu görüntüyü görmek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image12.png))


Bu noktada bir DropDownList sahibiz (`Categories`), sistemde kategorilerini listeler. Şimdi seçilen kategoriye ait bu ürünlerin listeleyen GridView eklemek ihtiyacımız var. Yine de yaptığımız önce DropDownList'ın akıllı etiket AutoPostBack etkinleştir onay kutusunu denetlemek için bir dakikanızı ayırarak. Bölümünde açıklandığı gibi *ana/ayrıntı filtreleme ile bir DropDownList* DropDownList'ın ayarlayarak öğretici `AutoPostBack` özelliğini `true` sayfa DropDownList değeri değiştirilir her zaman geri yayımlanacak. Bu yeni seçilen kategori için bu ürünlerin gösteren GridView yenilenmesi neden olur. Varsa `AutoPostBack` özelliği `false` (varsayılan), kategoriyi değiştirme geri göndermeye neden olmaz ve bu nedenle listelenen ürünleri güncelleştirmek gerekmez.


[![Akıllı etiket DropDownList'ın etkinleştirme AutoPostBack onay kutusunu işaretleyin](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image13.png)

**Şekil 5**: etkinleştirme AutoPostBack DropDownList'ın akıllı etiketinde onay ([tam boyutlu görüntüyü görmek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image15.png))


Bir GridView denetimi, seçilen kategori ürünleri görüntülemek için sayfasına ekleyin. GridView'ın ayarlamak `ID` için `ProductsInCategory` ve adlı yeni bir ObjectDataSource bağlama `ProductsInCategoryDataSource`.


[![ProductsInCategoryDataSource adlı yeni bir ObjectDataSource Ekle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image16.png)

**Şekil 6**: yeni ObjectDataSource adlandırılmış ekleme `ProductsInCategoryDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image18.png))


ObjectDataSource yapılandırın çağırdığı `ProductsBLL` sınıfın `GetProductsByCategoryID(categoryID)` yöntemi.


[![GetProductsByCategoryID(categoryID) yöntemi Çağır ObjectDataSource sahip](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image19.png)

**Şekil 7**: ObjectDataSource çağırma sahip `GetProductsByCategoryID(categoryID)` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image21.png))


Bu yana `GetProductsByCategoryID(categoryID)` yöntemi bir giriş parametresi alan sihirbazının son adımı, parametre değerinin kaynak biz da belirtebilirsiniz. Seçili kategoriyi bu ürünleri görüntülemek için çekilen parametresi sahip `Categories` DropDownList.


[![Seçili kategorileri DropDownList CategoryID parametresi değeri Al](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image22.png)

**Şekil 8**: alma *`categoryID`* seçili kategorileri DropDownList parametresi değerinden ([tam boyutlu görüntüyü görmek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image24.png))


Sihirbazı tamamladıktan sonra GridView bir BoundField her ürün özellikleri için sahip olur. Şimdi, yalnızca bu BoundFields temiz `ProductName`, `UnitPrice`, `UnitsInStock`, ve `UnitsOnOrder` BoundFields görüntülenir. Alan düzeyindeki ayarları için kalan BoundFields eklemekten çekinmeyin (biçimlendirme gibi `UnitPrice` bir para birimi olarak). Bu değişiklikleri yaptıktan sonra bildirim temelli GridView'ın işaretleme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample1.aspx)]

Bu noktada seçilen kategoriye ait bu ürünlere yönelik sipariş adı, birim fiyatı, stoktaki birim ve birim gösteren tam olarak işlevsel bir ana/ayrıntı raporunu sahibiz.


[![Seçili kategorileri DropDownList CategoryID parametresi değeri Al](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image25.png)

**Şekil 9**: alma *`categoryID`* seçili kategorileri DropDownList parametresi değerinden ([tam boyutlu görüntüyü görmek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>2. adım: alt bilgi GridView içinde görüntüleme

GridView denetiminde bir üstbilgi ve altbilgi satır görüntüleyebilirsiniz. Bu satır değerlerine bağlı olarak görüntüleniyor `ShowHeader` ve `ShowFooter` özellikleri sırasıyla `ShowHeader` varsayarak `true` ve `ShowFooter` için `false`. Alt bilgi GridView yalnızca içerecek şekilde kendi `ShowFooter` özelliğini `true`.


[![GridView'ın ShowFooter özelliği true olarak ayarlayın.](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image28.png)

**Şekil 10**: GridView'ın ayarlamak `ShowFooter` özelliğini `true` ([tam boyutlu görüntüyü görmek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image30.png))


Alt satır GridView içinde tanımlanan alanların her biri için bir hücre sahiptir; Ancak, bu hücreleri varsayılan olarak boştur. İlerlememizin bir tarayıcıda görüntülemek için bir dakikanızı ayırın. İle `ShowFooter` özelliği şimdi ayarlayın `true`, GridView bir boş alt satır içerir.


[![GridView şimdi bir alt bilgi satır içerir.](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image31.png)

**Şekil 11**: GridView artık bir alt bilgi satırı içeriyor ([tam boyutlu görüntüyü görmek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image33.png))


Beyaz arka plan olduğu gibi alt satırın Şekil 11'de, bekleme değil. Oluşturalım bir `FooterStyle` CSS sınıfı içinde `Styles.css` koyu kırmızı arka plan belirtir ve ardından yapılandırma `GridView.skin` dış görünüm dosyası içinde `DataWebControls` GridView'ın bu CSS sınıfı atamak için tema `FooterStyle`'s `CssClass` özelliği. Dış Görünüm ve tema tazelemek istiyorsanız kiracıurl [görüntüleyen veri ile ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) öğretici.

Başlamak için aşağıdaki CSS sınıfının ekleyerek `Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample2.css)]

`FooterStyle` Stili için CSS sınıfı benzer `HeaderStyle` olsa da, sınıf `HeaderStyle`ait arka plan rengini koyu subtlety ve metni kalın yazı tipiyle görüntülenir. Ayrıca, altbilgi metni sağa ortalanmış başlığının metin ise hizalanır.

Ardından, bu CSS sınıfı her GridView'ın alt ile ilişkilendirilecek açın `GridView.skin` dosyası `DataWebControls` teması ve kümesi `FooterStyle`'s `CssClass` özelliği. Sonra bu ek dosya biçimlendirme gibi görünmelidir:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample3.aspx)]

Ekran görüntüsü gösterildiği gibi bu değişikliği altbilgi yapar açıkça öne çıkarmak.


[![GridView'ın alt satır Kırmızımsı arka plan rengi artık sahiptir.](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image34.png)

**Şekil 12**: GridView'ın alt satır Kırmızımsı arka plan rengi artık sahiptir ([tam boyutlu görüntüyü görmek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>3. adım: Özet verilerini bilgi işlem

Görüntülenen GridView'ın alt ile bize karşılıklı sonraki işlem özet verileri nasıl zorluktur. Bu toplu bilgi işlem için iki yol vardır:

1. Bir SQL sorgusu biz işlem belirli bir kategori için Özet verileri için veritabanı için ek bir sorgu verebilir. SQL toplama işlevleri ile birlikte bir dizi içeren bir `GROUP BY` üzerinde özetlenmiş veriler verileri belirtmek için yan tümcesi. Aşağıdaki SQL sorgusunu, gerekli bilgileri geri getirir:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample4.sql)]

    Elbette bu sorgu doğrudan vermek istemezsiniz `SummaryDataInFooter.aspx` sayfasında, bir yöntem oluşturarak ancak bunun yerine `ProductsTableAdapter` ve `ProductsBLL`.
2. Bu bilgiler, GridView'a anlatıldığı gibi eklendiğinden gibi işlem [özel biçimlendirme sırasında verileri](custom-formatting-based-upon-data-cs.md) öğreticide GridView'ın `RowDataBound` olay işleyicisi harekete kez GridView sonra eklenen her satır için kendi bırakıldı sınırlama. Bu olay için bir olay işleyicisi oluşturarak biz çalışan bir tutabilirsiniz istediğimiz değerlerin toplamını toplama. Sonra en son veri satırı, toplamları ve ortalamayı hesaplamak için gereken bilgileri sahibiz GridView bağlandı.

Ben genellikle ikinci yaklaşım veritabanına ve veri erişim katmanı ve iş mantığı katmanı özeti işlevselliğini uygulamak için gereken çabayı bir seyahat kaydeder, ancak her iki yöntemle yeterli gibi kullanın. Bu öğretici için Şimdi ikinci seçenek kullanın ve toplam kullanarak izlemek `RowDataBound` olay işleyicisi.

Oluşturma bir `RowDataBound` GridView Tasarımcısı'nda, Özellikler penceresinde Şimşek simgesi tıklayarak seçip çift GridView için olay işleyicisi `RowDataBound` olay. Bu adlı yeni bir olay işleyicisi oluşturur `ProductsInCategory_RowDataBound` içinde `SummaryDataInFooter.aspx` sayfa arka plan kod sınıfı.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample5.cs)]

Değişen Toplam sürdürmek için biz değişkenleri olay işleyicisinin kapsamı dışında tanımlamanız gerekir. Aşağıdaki dört sayfa düzeyi değişkenleri oluşturun:

- `_totalUnitPrice`, türü `decimal`
- `_totalNonNullUnitPriceCount`, türü `int`
- `_totalUnitsInStock`, türü `int`
- `_totalUnitsOnOrder`, türü `int`

Ardından, her veri satırı ile karşılaşılırsa için bu üç değişkenin artırmak için kod yazma `RowDataBound` olay işleyicisi.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample6.cs)]

`RowDataBound` Olay işleyicisi, biz DataRow ile ilgilenen sağlayarak başlatır. Kurulmuş sonra `Northwind.ProductsRow` yalnızca bağlı örnek `GridViewRow` nesnesine `e.Row` değişkeninde depolanan `product`. Sonra çalışan toplam değişkenleri geçerli ürün uygulamasının karşılık gelen değerlerine göre artırılır (bir veritabanı içermeyen varsayarak `NULL` değeri). Biz hem çalışmasını izlemek `UnitPrice` toplam ve olmayan sayısını`NULL` `UnitPrice` ortalama fiyat bu iki sayının bölünme değerini olduğundan kaydeder.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>4. adım: alt bilgisinde özet verilerini görüntüleme

Özet verilerini toplamının GridView'ın alt satırda görüntüleme son adımdır. Bu görev, bir program aracılığıyla aracılığıyla gerçekleştirilebilir `RowDataBound` olay işleyicisi. Bu geri çağırma `RowDataBound` olay işleyicisi harekete için *her* alt bilgi satırı dahil olmak üzere GridView'a bağlı satır. Bu nedenle, biz bizim olay işleyicisine aşağıdaki kodu kullanarak alt satırdaki verileri görüntülemek için genişletebilirsiniz:


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample7.cs)]

Tüm veri satırları eklendikten sonra GridView'a altbilgi satır ekleneceğinden biz zamanına göre toplam hesaplamalar tamamlamış olmanız alt bilgisinde özet verileri görüntülemek hazırız emin olabilirsiniz. Son adım, daha sonra bu değerleri footer'ın hücrelerde ayarlamaktır.

Belirli alt hücresinde metni görüntülemek için kullanın `e.Row.Cells[index].Text = value`burada `Cells` dizin 0 başlar. Aşağıdaki kod ortalama fiyat (Toplam Fiyat ürünlerin sayıya bölünen) hesaplar ve bölümde stoktaki miktar ve sipariş GridView uygun altbilgi hücrelerinde birimlerde bu birimleri toplam sayısını görüntüler.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample8.cs)]

Bu kod eklendikten sonra rapor Şekil 13 gösterir. Not nasıl `ToString("c")` bir para birimi gibi Biçimlendirilecek ortalama fiyat özet bilgilerini neden olur.


[![GridView'ın alt satır Kırmızımsı arka plan rengi artık sahiptir.](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image37.png)

**Şekil 13**: GridView'ın alt satır Kırmızımsı arka plan rengi artık sahiptir ([tam boyutlu görüntüyü görmek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image39.png))


## <a name="summary"></a>Özet

Ortak bir rapor gereksinim olan Özet verilerini görüntüleme ve GridView denetiminde alt bilgi satırına gibi bilgileri içerecek şekilde kolaylaştırır. Altbilgi satırında görüntülenen zaman GridView'ın `ShowFooter` özelliği `true` ve metin hücrelerinden üzerinden programlı olarak ayarlama olabilir `RowDataBound` olay işleyicisi. Özet verilerini bilgi işlem ya da veritabanını yeniden sorgulama veya kod özet verileri program aracılığıyla işlem ASP.NET sayfa arka plan kod sınıfı kullanılarak yapılabilir.

Bu öğreticiyi FormView GridView ve DetailsView denetimlerini ile özel biçimlendirme bizim İnceleme sonlandırır. Sonraki ekleme, güncelleştirme ve silme aynı bu denetimleri kullanarak verileri bizim araştırma başlatıyor.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](using-the-formview-s-templates-cs.md)
> [İleri](custom-formatting-based-upon-data-vb.md)
