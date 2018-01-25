---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
title: "Ana/ayrıntı ile DropDownList (C#) filtreleme | Microsoft Docs"
author: rick-anderson
description: "Bu öğreticide master kayıtları DropDownList denetimi ve GridView içinde seçilen liste öğesinin ayrıntılarını görüntülemek nasıl göreceğiz."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 53e659cc-eefb-40c1-a1dc-559481c99443
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
msc.type: authoredcontent
ms.openlocfilehash: cf3058ac095bc2ed728a716e70f962e260eef5a2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Ana/ayrıntı filtreleme ile DropDownList (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_7_CS.exe) veya [PDF indirin](master-detail-filtering-with-a-dropdownlist-cs/_static/datatutorial07cs1.pdf)

> Bu öğreticide master kayıtları DropDownList denetimi ve GridView içinde seçilen liste öğesinin ayrıntılarını görüntülemek nasıl göreceğiz.


## <a name="introduction"></a>Giriş

Rapor genel bir tür *ana/ayrıntı raporu*, içinde rapor bazı "ana" kayıt kümesi göstererek başlar. Kullanıcı daha sonra ana kayıtları birine ana kayıt "ayrıntılarını." böylece görüntüleme ayrıntıya inebilir Ana/ayrıntı raporu gibi bir-çok ilişkileri görselleştirme için ideal seçim kategorilerin tümünü gösteren ve belirli bir kategori seçin ve ilişkili ürünlerinden görüntülemek bir kullanıcı izin vererek bildirir. Ayrıca, ana/ayrıntı raporları "geniş" özellikle tabloları (olanları sütunları pek çok) ayrıntılı bilgileri görüntülemek için yararlıdır. Örneğin, bir ana öğe/ayrıntı raporu "ana" düzeyini veritabanında yalnızca ürün adı ve birim fiyat ürünlerin gösterebilir ve belirli bir üründe araştırıp ek ürün alanları gösterir (kategori, tedarikçi, birim başına miktar ve vb.).

İle bir ana öğe/ayrıntı raporu uygulanabileceği birçok yolu vardır. Bu ve sonraki üç öğreticileri biz ana/ayrıntı raporları çeşitli göreceğiz. Bu öğreticide ana kayıtları görüntülemek nasıl göreceğiz bir [DropDownList denetimi](https://msdn.microsoft.com/library/dtx91y0z.aspx) ve GridView içinde seçilen liste öğesinin Ayrıntıları. Özellikle, bu öğreticinin ana/ayrıntı raporu kategori ve ürün bilgilerini listeler.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>1. adım: bir DropDownList kategorileri görüntüleme

Bizim ana/ayrıntı raporu görüntülenen seçilen liste öğesi'nin ürünleriyle bir DropDownList kategorilerde listeleyecek GridView sayfasında aşağı daha fazla. Ardından, bize, önünde ilk görev bir DropDownList görüntülenen kategorileri sağlamaktır. Açık `FilterByDropDownList.aspx` sayfasındaki `Filtering` klasörü üzerinde DropDownList sayfanın tasarımcıya araç çubuğuna sürükleyin ve ayarlayın, `ID` özelliğine `Categories`. Ardından, DropDownList'ın Akıllı Etiket Veri Kaynağı Seç bağlantıyı tıklayın. Bu veri kaynağı Yapılandırma Sihirbazı'nı görüntüler.


[![DropDownList'ın veri kaynağını belirtin](master-detail-filtering-with-a-dropdownlist-cs/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image1.png)

**Şekil 1**: DropDownList'ın veri kaynağını belirtin ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-cs/_static/image3.png))


Adlı yeni bir ObjectDataSource eklemek için `CategoriesDataSource` , çağırır `CategoriesBLL` sınıfının `GetCategories()` yöntemi.


[![CategoriesDataSource adlı yeni bir ObjectDataSource ekleme](master-detail-filtering-with-a-dropdownlist-cs/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image4.png)

**Şekil 2**: yeni ObjectDataSource adlandırılmış eklemek `CategoriesDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-cs/_static/image6.png))


[![CategoriesBLL sınıfını kullanmak seçin](master-detail-filtering-with-a-dropdownlist-cs/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image7.png)

**Şekil 3**: kullanımına seçin `CategoriesBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-cs/_static/image9.png))


[![ObjectDataSource GetCategories() yöntemi kullanmak üzere yapılandırma](master-detail-filtering-with-a-dropdownlist-cs/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image10.png)

**Şekil 4**: ObjectDataSource kullanılacak yapılandırma `GetCategories()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-cs/_static/image12.png))


Yine DropDownList hangi veri kaynağı alanı görüntülenmesi gerekir ve hangi belirtmek için ihtiyacımız ObjectDataSource yapılandırdıktan sonra bir liste öğesi için değer olarak ilişkili olmalıdır. Sahip `CategoryName` görüntü olarak alan ve `CategoryID` her liste öğesi için değer olarak.


[![CategoryName alan ve kullanım adlı kullanıcı, Categoryıd'si DropDownList görünen değere sahip](master-detail-filtering-with-a-dropdownlist-cs/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image13.png)

**Şekil 5**: DropDownList görüntü sahip `CategoryName` alan ve kullanım `CategoryID` değeri olarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-cs/_static/image15.png))


Kayıtlardan doldurulur DropDownList denetimi bu noktada sahibiz `Categories` tablosu (tüm yaklaşık altı saniye içinde gerçekleştirilir). Şekil 6 bizim ilerleme bugüne kadarki bir tarayıcıdan görüntülendiğinde gösterir.


[![Geçerli kategorileri bir aşağı açılır listeler](master-detail-filtering-with-a-dropdownlist-cs/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image16.png)

**Şekil 6**: A aşağı açılan listeler geçerli kategorileri ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-cs/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>2. adım: ürünleri GridView ekleme

Bu son bizim ana/ayrıntı raporu seçili kategoriyle ilişkili ürünleri listelemek için adımdır. Bunu başarmak için sayfaya GridView ekleyin ve adlı yeni bir ObjectDataSource oluşturma `productsDataSource`. Sahip `productsDataSource` denetim cull kendi verisinden `ProductsBLL` sınıfının `GetProductsByCategoryID(categoryID)` yöntemi.


[![GetProductsByCategoryID(categoryID) yöntemini seçin](master-detail-filtering-with-a-dropdownlist-cs/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image19.png)

**Şekil 7**: seçin `GetProductsByCategoryID(categoryID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-cs/_static/image21.png))


Bu yöntem seçtikten sonra ObjectDataSource Sihirbazı bize değeri yöntemin için ister  *`categoryID`*  parametresi. Seçili değerini kullanacak şekilde `categories` DropDownList öğesi kümesine parametre kaynak denetimi ve ControlId `Categories`.


[![Adlı kullanıcı, Categoryıd'si parametresi kategorileri DropDownList değerine ayarlayın](master-detail-filtering-with-a-dropdownlist-cs/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image22.png)

**Şekil 8**: ayarlamak  *`categoryID`*  parametre değerine `Categories` DropDownList ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-cs/_static/image24.png))


Bir tarayıcıda bizim ilerleme kullanıma için bir dakikanızı ayırın. İlk sitesini ziyaret ettiğinde bu ürünler seçilen kategoriye ait (Meşrubat) (Şekil 9'da gösterildiği gibi) görüntülenir, ancak DropDownList değiştirme veri güncelleştirilmiyor. Geri gönderimin GridView güncelleştirmek oluşması gereken olmasıdır. Bunu gerçekleştirmek için şu (kümelesini herhangi bir kod yazmadan gerektirir) iki seçeneğiniz vardır:

- **Kategorilerini DropDownList's**[AutoPostBack özelliğini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**true.** (Bunu DropDownList'ın akıllı etiket AutoPostBack etkinleştir seçeneğini işaretleyerek gerçekleştirebilirsiniz.) DropDownList'ın seçili olduğunda, bu geri gönderimin tetikleyecek öğesi kullanıcı tarafından değiştirildi. Bu nedenle, kullanıcı DropDownList yeni bir kategori seçtiğinde geri gönderimin olun ve yeni seçilen kategori için ürünlerle GridView güncelleştirilir. (Bu öğreticide kullanmış olduğunuz yaklaşım budur.)
- **Bir düğme Web denetimi DropDownList yanındaki ekleyin.** Ayarlama, `Text` yenileme özelliğine veya buna benzer. Bu yaklaşımda, kullanıcının yeni bir kategori seçin ve ardından düğmesini gerekir. Düğmesini tıklatarak geri gönderimin neden ve bu ürünler seçili kategorinin listelemek için GridView güncelleştirin.

Şekil 9 ve 10 eylem ana/ayrıntı raporu gösterilmektedir.


[![İlk sitesini ziyaret ettiğinde içecek ürünleri görüntülenir](master-detail-filtering-with-a-dropdownlist-cs/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image25.png)

**Şekil 9**: ilk sitesini ziyaret ettiğinde içecek ürünleri görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-cs/_static/image27.png))


[![Yeni bir ürün (üretim) otomatik olarak seçme GridView güncelleştirme geri gönderme neden olur.](master-detail-filtering-with-a-dropdownlist-cs/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image28.png)

**Şekil 10**: yeni bir ürün (üretim) otomatik olarak belirlenmesi GridView güncelleştirme geri gönderme neden olur ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-cs/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>"--Bir kategori seçin--" liste öğesi ekleme

İlk kez ziyaret eden `FilterByDropDownList.aspx` DropDownList'ın ilk liste öğesi (Meşrubat) GridView içecek ürünleri gösteren varsayılan olarak, seçili kategorileri sayfa. Bunun yerine bir DropDownList öğesi olmasını isteyebilirsiniz ilk kategorinin ürünleri gösteren seçili yerine aşağıdakine benzer "--bir kategori seçin--" söyler.

DropDownList için yeni bir liste öğesi eklemek için Özellikler penceresini gidin ve içinde üç noktaya tıklayın `Items` özelliği. İle yeni bir liste öğesi ekleme `Text` "--bir kategori seçin--" ve `Value` `-1`.


[![Ekleme bir--bir kategori--liste öğesi seçin](master-detail-filtering-with-a-dropdownlist-cs/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image31.png)

**Şekil 11**: ekleme bir--bir kategori--liste öğesi seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-cs/_static/image33.png))


Alternatif olarak, aşağıdaki biçimlendirme DropDownList ekleyerek liste öğesi ekleyebilirsiniz:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample1.aspx)]

Ayrıca, DropDownList denetimin ayarlamak ihtiyacımız `AppendDataBoundItems` kategorileri DropDownList ObjectDataSource bağlı olduğunda bunlar herhangi bir el ile eklenen liste öğesini üzerine çünkü true `AppendDataBoundItems` True değil.


![AppendDataBoundItems özelliğini True olarak ayarlayın](master-detail-filtering-with-a-dropdownlist-cs/_static/image34.png)

**Şekil 12**: ayarlamak `AppendDataBoundItems` özelliğinin True


Bu değişiklikler sonra ilk sitesini ziyaret ettiğinde "--bir kategori seçin--" seçeneğinin seçildiğinden ve hiç ürün görüntülenir.


[![İlk sayfa yükü hiç ürün görüntülenir](master-detail-filtering-with-a-dropdownlist-cs/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image35.png)

**Şekil 13**: üzerinde ilk sayfa Hayır ürünleri görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-cs/_static/image37.png))


Değeri olduğu için hiç ürün "--bir kategori seçin--" öğesini seçilmediğinden zaman görüntülenme nedeni `-1` ve veritabanında hiç ürün yok bir `CategoryID` , `-1`. Bu noktada bitirdikten sonra istediğiniz davranış varsa! Ancak görüntülemek istiyorsanız, *tüm* "--bir kategori seçin--" liste öğesi seçildiğinde kategorilerini dönmek `ProductsBLL` sınıfı ve özelleştirme `GetProductsByCategoryID(categoryID)` onun çağırır şekilde yöntemi `GetProducts()` yöntemi varsa geçirilen içinde  *`categoryID`*  parametresi sıfırdan küçük:

[!code-csharp[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample2.cs)]

Burada kullanılan teknikleri kullandık tüm tedarikçileri görüntülenecek yaklaşım benzer geri [bildirim temelli parametreleri](../basic-reporting/declarative-parameters-cs.md) öğretici, bu örnek için biz değerini kullanmakta olduğunuz rağmen `-1` tüm kayıtları olması gerektiğini belirtmek için tersine alınan `null`. Bunun nedeni,  *`categoryID`*  parametresinin `GetProductsByCategoryID(categoryID)` yöntemi, tamsayı değeri geçirilen gibi bildirim temelli parametreleri öğreticide biz dize giriş parametresi geçirme ancak bekliyor.

Şekil 14 gösteren ekran görüntüsü `FilterByDropDownList.aspx` "--bir kategori seçin--" seçeneği seçildiğinde. Burada, tüm ürünleri varsayılan olarak görüntülenir ve kullanıcı belirli bir kategorideki seçerek görüntü daraltabilirsiniz.


[![Ürünler artık listelenen varsayılan olarak tümü](master-detail-filtering-with-a-dropdownlist-cs/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image38.png)

**Şekil 14**: ürünleri artık listelenen varsayılan olarak tümü ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-a-dropdownlist-cs/_static/image40.png))


## <a name="summary"></a>Özet

Hiyerarşik olarak ilgili verileri görüntülerken, bu genellikle alınacağı kullanıcı hiyerarşinin en üst verilerden harcadığı başlatmak ve ayrıntılara detaya ana/ayrıntı raporları kullanarak verileri sunmak için yardımcı olur. Bu öğreticide seçili kategorinin ürünleri gösteren bir basit ana/ayrıntı raporu oluşturma incelendi. Bu, kategoriler ve seçili kategorisine ait ürünleri için GridView listesi için bir DropDownList kullanarak gerçekleştirilmiştir.

İçinde [sonraki öğretici](master-detail-filtering-with-two-dropdownlists-cs.md) biz DropDownList arabirimi bir adım Ayrıca, iki DropDownLists kullanarak sürer.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Next](master-detail-filtering-with-two-dropdownlists-cs.md)
