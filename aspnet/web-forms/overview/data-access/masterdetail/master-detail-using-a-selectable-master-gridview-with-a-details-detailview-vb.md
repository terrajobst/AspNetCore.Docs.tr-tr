---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
title: "Ana/ayrıntı seçilebilir ana GridView ayrıntıları DetailView (VB) ile kullanma | Microsoft Docs"
author: rick-anderson
description: "Bu öğretici, satırları seçme düğmesi yanı sıra her ürünün fiyatı ve adını ekler GridView sahip olur. Bir particu için Seç düğmesini tıklatarak..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1d1a7c93-971d-4690-9c5e-dac0e5014a09
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
msc.type: authoredcontent
ms.openlocfilehash: eae9c07eff7780aab18346815ca410d687789d17
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-vb"></a>Ana/ayrıntı seçilebilir ana GridView ayrıntıları DetailView (VB) ile kullanma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_10_VB.exe) veya [PDF indirin](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/datatutorial10vb1.pdf)

> Bu öğretici, satırları seçme düğmesi yanı sıra her ürünün fiyatı ve adını ekler GridView sahip olur. Belirli bir ürün için Seç düğmesini tıklatarak tam ayrıntılarını aynı sayfada DetailsView denetimini görüntülenmesine neden olur.


## <a name="introduction"></a>Giriş

İçinde [önceki öğretici](master-detail-filtering-across-two-pages-vb.md) iki web sayfalarını kullanarak ana/ayrıntı raporu oluşturmak nasıl gördüğümüz: kendisinden biz görüntülenen Üreticiler; listesi "ana" web sayfası ve listede seçilen tarafından sağlanan bu ürünü "Ayrıntılar" web sayfası Sağlayıcı. Bu iki sayfalık rapor biçiminde bir sayfaya sıkıştırılmış. Bu öğretici, satırları seçme düğmesi yanı sıra her ürünün fiyatı ve adını ekler GridView sahip olur. Belirli bir ürün için Seç düğmesini tıklatarak tam ayrıntılarını aynı sayfada DetailsView denetimini görüntülenmesine neden olur.


[![Seç düğmesini tıklatarak ürünün ayrıntıları görüntüler.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image1.png)

**Şekil 1**: ürün ayrıntıları görüntüler için Seç düğmesini ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>1. adım: seçilebilir GridView oluşturma

Her ana kayıt köprü dahil rapor, iki sayfalık ana/ayrıntı geri çağırma, tıklatıldığında, kullanıcı tıklatılan sıranın geçirme Ayrıntılar sayfası gönderilen `SupplierID` sorgu dizesi değeri. Bu tür bir köprü bir HyperLinkField kullanarak her GridView satır eklendi. Tek sayfa ana/Ayrıntılar rapor için ihtiyacımız her GridView satır, tıklatıldığında bir düğme ayrıntılarını gösterir. GridView denetimi geri gönderimin neden olur ve bu satırın GridView 's işaretler her satır için Seç düğmesini içerecek biçimde yapılandırılabilir [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx).

GridView denetimine eklemeye başlayın `DetailsBySelecting.aspx` sayfasındaki `Filtering` ayarı klasörü, kendi `ID` özelliğine `ProductsGrid`. Adlı yeni bir ObjectDataSource ekleyeceğimize `AllProductsDataSource` , çağırır `ProductsBLL` sınıfının `GetProducts()` yöntemi.


[![AllProductsDataSource adlı bir ObjectDataSource oluşturma](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image4.png)

**Şekil 2**: ObjectDataSource adlı oluşturma `AllProductsDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image6.png))


[![ProductsBLL sınıf kullanma](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image7.png)

**Şekil 3**: kullanım `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image9.png))


[![ObjectDataSource GetProducts() yöntemini çağırmak için yapılandırma](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image10.png)

**Şekil 4**: çağırma ObjectDataSource yapılandırma `GetProducts()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image12.png))


Kaldırma GridView'ın alanları Düzenle dışındaki tüm `ProductName` ve `UnitPrice` BoundFields. Ayrıca, bu BoundFields biçimlendirme gibi gerektiği gibi özelleştirin çekinmeyin `UnitPrice` bir para birimi olarak BoundField ve değiştirme `HeaderText` BoundFields özelliklerini. GridView'ın akıllı etiket Düzenle sütunları bağlantısından tıklayarak veya bildirim temelli söz dizimini el ile yapılandırma adımları grafik gerçekleştirilebilir.


[![Tüm ProductName ve UnitPrice BoundFields Kaldır](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image13.png)

**Şekil 5**: kaldırmak dışındaki tüm `ProductName` ve `UnitPrice` BoundFields ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image15.png))


GridView için son biçimlendirme aşağıdaki gibidir:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample1.aspx)]

Ardından, her satır için Seç düğmesini ekler GridView seçilebilir, olarak işaretlemek ihtiyacımız. Bunu gerçekleştirmek için GridView'ın akıllı etiket Seçimi Etkinleştir onay kutusunu işaretlemeniz yeterli.


[![GridView'ın satır seçilebilir yapma](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image16.png)

**Şekil 6**: GridView ait satır seçilebilir yapma ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image18.png))


Seçim etkinleştirme seçeneği denetimi ekler için CommandField `ProductsGrid` GridView ile kendi `ShowSelectButton` özelliği True olarak ayarlanmış. Şekil 6 gösterildiği gibi bu GridView her satır için Seç düğmesini sonuçlanır. Varsayılan olarak, Select düğmeleri LinkButtons işlenir, ancak düğmeler veya ImageButtons yerine CommandField kişinin kullanabilirsiniz `ButtonType` özelliği.


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample2.aspx)]

GridView satırın Seç düğmesine tıklandığında geri gönderimin ensues ve GridView's `SelectedRow` özelliği güncelleştirilir. Ek olarak `SelectedRow` GridView özelliği sağlar [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx), ve [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) özellikleri. `SelectedIndex` Özelliği, ancak seçilen satırın dizinini döndürür `SelectedValue` ve `SelectedDataKey` özellikleri dönüş değerleri GridView ait temel [DataKeyNames özelliğini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx).

`DataKeyNames` Özelliği bir ilişkilendirmek için kullanılır veya daha fazla veri alan değerleri her satırın ve her GridView satır temel alınan verilerle benzersiz şekilde tanımlayan bilgileri özniteliği için yaygın olarak kullanılır. `SelectedValue` Özelliği ilk değerini döndürür `DataKeyNames` seçili olan satır için veri alanı olarak nereye `SelectedDataKey` özelliği döndürür seçilen sıranın `DataKey` tüm için belirtilen veri anahtar alanları için değerleri içeren nesne Bu satır.

`DataKeyNames` GridView, DetailsView veya FormView Tasarımcısı aracılığıyla bir veri kaynağı bağladığınızda özellik için benzersiz olarak tanımlayan veri alanlarını otomatik olarak ayarlanır. Bu özellik bize için otomatik olarak önceki eğitimlerine ayarlanmış olsa da, örnekler olmadan çalıştıktan `DataKeyNames` belirtilen özelliği. İçinde biz inceleniyor ekleme, güncelleştirme ve silme, gelecekteki öğreticileri yanı sıra, ancak bu öğreticide seçilebilir GridView için `DataKeyNames` özelliği düzgün şekilde ayarlanmalıdır. GridView's emin olmak için bir dakikanızı ayırın `DataKeyNames` özelliği ayarlanmış `ProductID`.

Şimdi bir tarayıcı aracılığıyla bugüne kadarki bizim ilerlemeyi görüntüleyebilirsiniz. GridView adını ve tüm ürünleri seçin LinkButton birlikte için fiyat listeleri unutmayın. Seç düğmesini tıklatarak geri gönderimin neden olur. 2. adımda seçilen ürün ayrıntılarını görüntüleyerek DetailsView yanıt bu geri gönderme için nasıl göreceğiz.


[![Her ürün satır Select LinkButton içerir](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image19.png)

**Şekil 7**: her ürün satır seçin LinkButton içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image21.png))


## <a name="highlighting-the-selected-row"></a>Seçilen satırın vurgulama

`ProductsGrid` GridView sahip bir `SelectedRowStyle` seçili olan satır için görsel stil dikte için kullanılan özellik. Düzgün bir şekilde kullanılan, bu kullanıcının deneyimini daha fazla açıkça GridView öğesinin hangi satırının seçili göstererek iyileştirebilir. Bu öğretici için şimdi sarı bir arka plan ile vurgulanmasını seçili satır var.

Önceki öğreticilerimizi gibi ile şimdi CSS sınıfları olarak tanımlanan estetik ile ilgili ayarları korumak çalışmalar yapılmaktadır. Bu nedenle, yeni bir CSS sınıf oluşturmak `Styles.css` adlı `SelectedRowStyle`.


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample3.css)]

Bu CSS sınıfı uygulamak için `SelectedRowStyle` özelliği *tüm* bizim öğretici serisinde GridViews Düzenle `GridView.skin` içinde dış görünüm `DataWebControls` içerecek şekilde tema `SelectedRowStyle` aşağıda gösterildiği gibi ayarları:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample4.aspx)]

Bu eklenmesiyle seçili GridView satır şimdi sarı arka plan rengiyle vurgulanır.


[![GridView's SelectedRowStyle özelliğini kullanarak seçilen sıranın görünümünü özelleştirme](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image22.png)

**Şekil 8**: seçilen satırın görünümünü kullanarak GridView's özelleştirin `SelectedRowStyle` özelliği ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>2. adım: bir DetailsView'da seçili ürünün ayrıntılarını görüntüleme

İle `ProductsGrid` GridView tamamlandı, tüm kalan tek şey seçili belirli ürün bilgilerini görüntüler DetailsView eklemek için. GridView yukarıda DetailsView denetimini ekleme ve adlı yeni bir ObjectDataSource oluşturma `ProductDetailsDataSource`. Seçili ürün hakkındaki belirli bilgileri görüntülemek için bu DetailsView istiyoruz olduğundan, yapılandırma `ProductDetailsDataSource` kullanmak için `ProductsBLL` sınıfının `GetProductByProductID(productID)` yöntemi.


[![ProductsBLL sınıfının GetProductByProductID(productID) yöntemi çağırma](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image25.png)

**Şekil 9**: çağırma `ProductsBLL` sınıfının `GetProductByProductID(productID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image27.png))


Sahip  *`productID`*  GridView denetiminin elde edilen parametrenin değeri `SelectedValue` özelliği. Biz GridView'ın daha önce bahsedildiği gibi `SelectedValue` özelliği, ilk veri anahtar seçili olan satır için değerini döndürür. Bu nedenle, zorunludur, GridView's `DataKeyNames` özelliği ayarlanmış `ProductID`, böylece seçilen satırın `ProductID` tarafından döndürülen değer `SelectedValue`.


[![GridView's SelectedValue özelliği parametre ProductID ayarlayın](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image28.png)

**Şekil 10**: ayarlamak  *`productID`*  GridView'ın parametre `SelectedValue` özelliği ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image30.png))


Bir kez `productDetailsDataSource` ObjectDataSource doğru şekilde yapılandırılmış ve DetailsView'a bağlı, Bu öğretici tamamlandıktan! Sayfa ilk sitesini ziyaret ettiğinizde herhangi bir sütun seçiliyse, böylece GridView'ın `SelectedValue` özelliği döndürür `Nothing`. Hiçbir ürünleriyle olduğundan bir `NULL` `ProductID` değeri, hiçbir kayıt tarafından döndürülür `GetProductByProductID(productID)` DetailsView görüntülenmiyor anlamı yöntemi (bkz. Şekil 11). GridView satırın Seç düğmesini tıklatarak, üzerine geri gönderimin ensues ve DetailsView yenilenir. Bu sefer GridView's `SelectedValue` özelliği döndürür `ProductID` seçilen satırın `GetProductByProductID(productID)` yöntemi döndürür bir `ProductsDataTable` , belirli ürün ve DetailsView hakkında bilgi içeren bu Ayrıntılar gösterilir (bkz. Şekil 12).


[![İlk ziyaret, yalnızca GridView görüntülendiğinde](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image31.png)

**Şekil 11**: ilk sitesini ziyaret ettiğinizde, yalnızca GridView görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image33.png))


[![Bir satır seçtikten sonra ürünün ayrıntıları görüntülenir](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image34.png)

**Şekil 12**: bir satır seçtikten sonra ürünün ayrıntıları görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image36.png))


## <a name="summary"></a>Özet

Bu ve önceki üç öğreticiler bir dizi ana/ayrıntı raporları görüntülemek için teknik gördük. Biz incelenmesi Bu öğreticide seçilebilir GridView ana kaydeder ve aynı sayfa üzerinde seçili ana kaydı hakkındaki ayrıntıları görüntülemek için bir DetailsView barındırmak için kullanma. Önceki eğitimlerine DropDownLists kullanarak ve bir web sayfası ve ayrıntı kayıtları başka bir ana kayıtları görüntüleme ana/ayrıntı raporları görüntülemek nasıl inceledik.

Ana/ayrıntı raporları bizim incelendiğinde Bu öğreticiyi sonlandırır. Sonraki öğretici ile başlayan GridView, DetailsView ve FormView özelleştirilmiş biçimlendirme bizim araştırması başlarsınız. Kendilerine bağlanmış verileri temel alan bu denetimlerin görünümünü özelleştirmek nasıl, GridView'ın altbilgisindeki özetlemek nasıl ve bir büyük ölçüde düzeni üzerinde denetim elde etmek için şablonları kullanma göreceğiz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Hilton Giesenow oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](master-detail-filtering-across-two-pages-vb.md)
