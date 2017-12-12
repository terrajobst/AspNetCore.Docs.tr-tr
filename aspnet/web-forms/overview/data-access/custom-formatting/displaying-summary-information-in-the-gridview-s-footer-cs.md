---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
title: "GridView'ın altbilgisinde (C#) özet bilgilerini görüntüleme | Microsoft Docs"
author: rick-anderson
description: "Özet bilgileri genellikle özet satırı raporda sonundaki görüntülenir. GridView denetiminin pr geçebiliriz olan hücrelere bir alt satır içerebilir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d50edc31-9286-4c6a-8635-be09e72752a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: 0d3df976181a4641dbfffe77875989c77ece059d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="displaying-summary-information-in-the-gridviews-footer-c"></a>GridView'ın altbilgisinde (C#) özet bilgileri görüntüleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_15_CS.exe) veya [PDF indirin](displaying-summary-information-in-the-gridview-s-footer-cs/_static/datatutorial15cs1.pdf)

> Özet bilgileri genellikle özet satırı raporda sonundaki görüntülenir. GridView denetiminin, hücrelere program aracılığıyla veri toplama ekliyoruz bir alt satır içerebilir. Bu öğreticide bu altbilgi satırda birleşik verileri görüntülemek nasıl göreceğiz.


## <a name="introduction"></a>Giriş

Her ürünlerin Fiyatlar, stoktaki birimler, birimler, sipariş ve sipariş düzeyleri görmesini yanı sıra, bir kullanıcı de ortalama fiyat, stok, toplam sayısı gibi toplama bilgileri ilginizi ve benzeri. Gibi özet bilgileri genellikle özet satırı raporda sonundaki görüntülenir. GridView denetiminin, hücrelere program aracılığıyla veri toplama ekliyoruz bir alt satır içerebilir.

Bu görev bize üç zorluklar sunar:

1. GridView, altbilgi satırı görüntülemek için yapılandırma
2. Özet verilerini belirleme; diğer bir deyişle, nasıl biz ortalama fiyat veya stoktaki birimlerinin toplam işlem?
3. Altbilgi satırı uygun hücrelere özet verilerini injecting

Bu öğreticide bu güçlüklerini nasıl göreceğiz. Özellikle, bir GridView görüntülenen seçili kategorinin ürünlerle aşağı açılan liste kategorilerini listeler. bir sayfa oluşturacağız. GridView birimleri toplam sayısı ve ortalama fiyat stock ve o kategorideki ürünler için sipariş gösteren altbilgi satırı içerir.


[![Özet bilgileri GridView's altbilgi satırda görüntülenir](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image1.png)

**Şekil 1**: Özet bilgi GridView's altbilgi satırda görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image3.png))


Ürünler Ana/ayrıntı arabirimine kendi kategorisiyle Bu öğretici, daha önce ele alınan kavramlar bağlı derlemeler [ana/ayrıntı filtreleme ile bir DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) Öğreticisi. Lütfen önceki öğreticide henüz çalıştıysanız, bu dosyayla etmeden önce bunu.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>1. adım: kategorileri DropDownList ve ürünleri GridView ekleme

Özet bilgileri GridView'ın altbilgi ekleme ile kendisini ilgili önce şimdi ilk yalnızca ana/ayrıntı raporu oluşturun. Biz bu ilk adımı tamamladıktan sonra Özet verileri içerecek şekilde nasıl tümleştirildiği incelenmektedir.

Başlangıç açarak `SummaryDataInFooter.aspx` sayfasındaki `CustomFormatting` klasör. Bir DropDownList denetimi ekleyin ve ayarlayın, `ID` için `Categories`. Ardından, DropDownList'ın Akıllı Etiket Veri Kaynağı Seç bağlantısından tıklayın ve adlı yeni bir ObjectDataSource eklemek için kabul `CategoriesDataSource` , çağırır `CategoriesBLL` sınıfının `GetCategories()` yöntemi.


[![CategoriesDataSource adlı yeni bir ObjectDataSource ekleme](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image4.png)

**Şekil 2**: yeni ObjectDataSource adlandırılmış eklemek `CategoriesDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image6.png))


[![CategoriesBLL sınıfının GetCategories() yöntemini çağırma ObjectDataSource sahip](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image7.png)

**Şekil 3**: ObjectDataSource çağırma sahip `CategoriesBLL` sınıfının `GetCategories()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image9.png))


ObjectDataSource yapılandırdıktan sonra sihirbazın bize DropDownList ait veri kaynağı Yapılandırma Sihirbazı hangi veri alan değeri belirtmek ihtiyacımız görüntülenmesi gerekir ve hangisinin DropDownList'ın değerinekarşılıkgelmelidirdöndürür`ListItem` s. Sahip `CategoryName` görüntülenen alan ve kullanım `CategoryID` değeri olarak.


[![Adlı kullanıcı, Categoryıd'si alanları ve CategoryName ListItems değeri ve metin olarak sırasıyla kullanın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image10.png)

**Şekil 4**: kullanım `CategoryName` ve `CategoryID` olarak alanları `Text` ve `Value` için `ListItem` s, sırasıyla ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image12.png))


Bu noktada bir DropDownList sahibiz (`Categories`) sistemde kategorileri listeler. Biz şimdi seçili kategorisine ait bu ürünleri listeler GridView eklemeniz gerekir. Bunu, yine de yapmadan önce DropDownList'ın akıllı etiket AutoPostBack etkinleştir onay için bir dakikanızı ayırın. Bölümünde açıklandığı gibi *ana/ayrıntı filtreleme ile bir DropDownList* DropDownList's ayarlayarak öğretici `AutoPostBack` özelliğine `true` sayfa DropDownList değeri değiştirildiğinde her zaman geri nakledilir. Bu yeni seçilen kategori için bu ürünleri gösteren yenilenmesi GridView neden olur. Varsa `AutoPostBack` özelliği ayarlanmış `false` (varsayılan), kategoriyi değiştirme geri gönderimin neden olmaz ve bu nedenle listelenen ürünler güncelleştirme olmaz.


[![DropDownList'ın akıllı etiket etkinleştirme AutoPostBack onay kutusunu işaretleyin](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image13.png)

**Şekil 5**: etkinleştirmek AutoPostBack DropDownList kullanıcının akıllı etiket içinde onay ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image15.png))


GridView denetimini seçilen kategori ürünleri görüntülemek için sayfasına ekleyin. GridView's ayarlamak `ID` için `ProductsInCategory` ve adlı yeni bir ObjectDataSource bağlama `ProductsInCategoryDataSource`.


[![ProductsInCategoryDataSource adlı yeni bir ObjectDataSource ekleme](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image16.png)

**Şekil 6**: yeni ObjectDataSource adlandırılmış eklemek `ProductsInCategoryDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image18.png))


ObjectDataSource çağırılır şekilde yapılandırmanız `ProductsBLL` sınıfının `GetProductsByCategoryID(categoryID)` yöntemi.


[![GetProductsByCategoryID(categoryID) yöntemi çağırma ObjectDataSource sahip](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image19.png)

**Şekil 7**: ObjectDataSource çağırma sahip `GetProductsByCategoryID(categoryID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image21.png))


Bu yana `GetProductsByCategoryID(categoryID)` yöntemi bir giriş parametresi alır sihirbazın son adımda biz parametre değeri kaynak belirtebilirsiniz. Seçilen kategori bu ürünlerden görüntülemek için gelen çekilen parametresi sahip `Categories` DropDownList.


[![Adlı kullanıcı, Categoryıd'si parametre değeri seçili kategorileri DropDownList Al](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image22.png)

**Şekil 8**: Get  *`categoryID`*  seçili kategorileri DropDownList parametre değerinin ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image24.png))


Sihirbazı tamamladıktan sonra GridView bir BoundField her ürün özellikleri için sahip olur. Bu nedenle, yalnızca bu BoundFields şimdi temizlemek `ProductName`, `UnitPrice`, `UnitsInStock`, ve `UnitsOnOrder` BoundFields görüntülenir. Herhangi bir alan düzeyindeki ayarı kalan BoundFields eklemekten çekinmeyin (biçimlendirme gibi `UnitPrice` bir para birimi olarak). Bu değişiklikleri yaptıktan sonra GridView'ın bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample1.aspx)]

Bu noktada seçilen kategoriye ait bu ürünler için sipariş adı, birim fiyat, stoktaki birim ve birim gösteren tümüyle işlevsel bir ana/ayrıntı raporunu vardır.


[![Adlı kullanıcı, Categoryıd'si parametre değeri seçili kategorileri DropDownList Al](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image25.png)

**Şekil 9**: Get  *`categoryID`*  seçili kategorileri DropDownList parametre değerinin ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>2. adım: altbilgi GridView görüntüleme

GridView denetimi üstbilgi ve altbilgi satır görüntüleyebilirsiniz. Bu satır değerlerine bağlı olarak görüntüleniyor `ShowHeader` ve `ShowFooter` özellikleri, sırasıyla ile `ShowHeader` varsayarak `true` ve `ShowFooter` için `false`. Altbilgi GridView yalnızca içerecek şekilde kendi `ShowFooter` özelliğine `true`.


[![GridView's ShowFooter özelliğini true olarak ayarlayın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image28.png)

**Şekil 10**: GridView's ayarlamak `ShowFooter` özelliğine `true` ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image30.png))


Altbilgi satırı GridView tanımlanan alanların her biri için bir hücre içeren; Ancak, bu hücreler varsayılan olarak boştur. Bir tarayıcıda bizim ilerleme durumunu görüntülemek için bir dakikanızı ayırın. İle `ShowFooter` özelliği şimdi kümesine `true`, GridView boş altbilgi satır içerir.


[![GridView şimdi altbilgi satırı içerir](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image31.png)

**Şekil 11**: GridView artık altbilgi satırı içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image33.png))


Beyaz bir arka plana sahip olarak Şekil 11 altbilgi satırında, bildirimde değil. Oluşturalım bir `FooterStyle` CSS sınıfı `Styles.css` koyu kırmızı arka plan belirtir ve ardından yapılandırın `GridView.skin` kaplama dosyasında `DataWebControls` GridView'ın bu CSS sınıfı atamak için tema `FooterStyle`'s `CssClass` özelliği. Geri başvurmak kaplamaları ve Temalar tazelemek gerekiyorsa, [veri ObjectDataSource ile görüntüleme](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) Öğreticisi.

Aşağıdaki CSS sınıfı eklemeye başlayın `Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample2.css)]

`FooterStyle` İçin stilde CSS sınıfı benzer `HeaderStyle` rağmen sınıf `HeaderStyle`ait arka plan rengi subtlety koyu ve bunun metni kalın yazı tipiyle görüntülenir. Ayrıca, altbilgi metinde sağ üst bilginin metnini ortalanmış ancak hizalanır.

Ardından, bu CSS sınıfı her GridView'ın altbilgi ile ilişkilendirmek için açık `GridView.skin` dosyasını `DataWebControls` tema ve kümesi `FooterStyle`'s `CssClass` özelliği. Bu ek sonra dosyanın biçimlendirme gibi görünmelidir:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample3.aspx)]

Ekran görüntüsü gösterildiği gibi bu değişikliği altbilgi yapar. daha fazla bilgi açıkça bekleyin.


[![GridView's altbilgi satırı Kırmızımsı arka plan rengi artık sahiptir.](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image34.png)

**Şekil 12**: GridView'ın altbilgi satırı artık Kırmızımsı arka plan rengi sahiptir ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>3. adım: Özet verilerini hesaplama

Görüntülenen GridView'ın altbilgi ile bize karşılıklı sonraki sınama özet verilerini işlem şeklidir. Bu birleşik bilgi işlem için iki yol vardır:

1. Bir SQL sorgu aracılığıyla biz özet verileri belirli bir kategorideki hesaplamak için veritabanına ek bir sorgu verebilir. SQL toplama işlevleri ile birlikte içeren bir `GROUP BY` üzerinde verileri özetlenen verileri belirtmek için yan tümcesi. Aşağıdaki SQL sorgusunu geri gerekli bilgileri getirmek:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample4.sql)]

    Kuşkusuz bu sorguyu doğrudan vermek istemezsiniz `SummaryDataInFooter.aspx` bir yöntem oluşturarak ancak bunun yerine, sayfa `ProductsTableAdapter` ve `ProductsBLL`.
2. Bu bilgiler, GridView anlatıldığı gibi eklenen gibi işlem [özel biçimlendirme göre bağlı verileri](custom-formatting-based-upon-data-cs.md) öğreticinin, GridView `RowDataBound` olay işleyicisi harekete kez sonra GridView eklenmekte olan her satır için kendi edildi veriye bağlı. Bu olay için bir olay işleyicisi oluşturarak biz çalışan bir tutabilirsiniz istediğimizi değerlerin toplamını toplama. Sonra en son veri satırı toplamları ve ortalamayı hesaplamak için gereken bilgileri sahibiz GridView bağlandı.

I genellikle İkinci yaklaşımı veritabanı ve veri erişim katmanı ve iş mantığı katmanı Özet işlevleri gerçekleştirmek için gereken çaba bir seyahat kaydeder, ancak her iki yaklaşım yeterli gibi kullanın. Bu öğretici için Şimdi ikinci seçeneğini kullanın ve toplam kullanarak izlemek `RowDataBound` olay işleyicisi.

Oluşturma bir `RowDataBound` GridView Tasarımcısı'nda, Özellikler penceresinde Şimşek Cıvata simgesine tıklayarak seçip çift GridView için olay işleyicisini `RowDataBound` olay. Bu adlı yeni bir olay işleyicisi oluşturur `ProductsInCategory_RowDataBound` içinde `SummaryDataInFooter.aspx` sayfanın arka plandaki kod sınıfı.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample5.cs)]

Çalışan bir toplam korumak için olay işleyicisini kapsamı dışında değişkenlerini tanımlamak ihtiyacımız. Aşağıdaki dört sayfa düzeyi değişkenleri oluşturun:

- `_totalUnitPrice`, türü`decimal`
- `_totalNonNullUnitPriceCount`, türü`int`
- `_totalUnitsInStock`, türü`int`
- `_totalUnitsOnOrder`, türü`int`

Ardından, her veri satırı karşılaştı için bu üç değişkenleri artırmak için kod yazma `RowDataBound` olay işleyicisi.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample6.cs)]

`RowDataBound` Olay işleyicisini başlatır biz DataRow ile ilgilenen sağlayarak. Kurulmuş sonra `Northwind.ProductsRow` yalnızca bağlı örneği `GridViewRow` nesnesinde `e.Row` değişkeninde depolanan `product`. Ardından, çalışan toplam değişkenleri geçerli ürünün karşılık gelen değerlerle artar (bir veritabanı içermeyen varsayılarak `NULL` değeri). Biz hem çalışmasını izlemek `UnitPrice` toplam ve sayı olmayan,`NULL` `UnitPrice` ortalama fiyat iki sayının bölümünü bu olduğundan kaydeder.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>4. adım: altbilgisinde özet verilerini görüntüleme

Özet veriler toplanır GridView'ın altbilgi satırda görüntülemek son adımdır. Bu görev çok, bir program aracılığıyla aracılığıyla gerçekleştirilebilir `RowDataBound` olay işleyicisi. Sözcüğünün `RowDataBound` olay işleyicisi harekete *her* altbilgi satırı dahil olmak üzere GridView için bağlı olan bir satır. Bu nedenle, biz altbilgi satırın aşağıdaki kodu kullanarak verileri görüntülemek için olay işleyicisi genişletebilirsiniz:


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample7.cs)]

Tüm veri satırları eklendikten sonra GridView altbilgi satırı ekleneceğinden biz zamanına göre biz toplam hesaplamaları tamamlamış oldunuz altbilgisinde özet verilerini görüntülemek için hazır olduğunu emin olabilir. Son adım, daha sonra bu değerleri altbilgi hücrelerde ayarlamaktır.

Belirli altbilgi hücrede metin görüntülemek için kullanın `e.Row.Cells[index].Text = value`, burada `Cells` dizin 0 başlar. Aşağıdaki kod ortalama fiyat (Toplam Fiyat ürünleri numarasına göre bölünmüş) hesaplar ve birimlerin toplam sayısını birlikte stock ve GridView uygun altbilgi hücrelerinin sırayla birimlerde görüntüler.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample8.cs)]

Bu kod eklendikten sonra Şekil 13 rapor gösterir. Not nasıl `ToString("c")` gibi bir para birimi biçimlendirilmesi ortalama fiyat özet bilgileri neden olur.


[![GridView's altbilgi satırı Kırmızımsı arka plan rengi artık sahiptir.](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image37.png)

**Şekil 13**: GridView'ın altbilgi satırı artık Kırmızımsı arka plan rengi sahiptir ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image39.png))


## <a name="summary"></a>Özet

Özet verileri görüntüleyen bir ortak rapor gereksinimdir ve GridView denetimini kendi altbilgi satırda gibi bilgileri içerecek şekilde kolaylaştırır. Altbilgi satırında görüntülenen zaman GridView's `ShowFooter` özelliği ayarlanmış `true` ve metin hücrelerinden üzerinden programlı olarak ayarlanmış olabilir `RowDataBound` olay işleyicisi. Özet verilerini bilgi işlem ya da veritabanını yeniden sorgulama veya kod özet verileri programlı olarak hesaplamak için ASP.NET sayfa arka plan kodu sınıfında kullanılarak yapılabilir.

GridView, DetailsView ve FormView denetimleriyle özel biçimlendirme bizim incelemelerini kullanarak bu öğreticiyi sonlandırır. Bizim sonraki öğretici ekleme, güncelleştirme ve bu aynı denetimlerini kullanarak verileri silme bizim araştırması başlatır.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Önceki](using-the-formview-s-templates-cs.md)
[sonraki](custom-formatting-based-upon-data-vb.md)
