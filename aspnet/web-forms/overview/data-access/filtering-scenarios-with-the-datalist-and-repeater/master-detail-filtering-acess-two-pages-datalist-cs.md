---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
title: Ana/ayrıntı filtreleme (C#) iki sayfada | Microsoft Docs
author: rick-anderson
description: Bu öğreticide iki sayfada ana/ayrıntı raporu nasıl atacağız. 'Ana' sayfasında categ listesini oluşturmak için Repeater denetimiyle kullan...
ms.author: riande
ms.date: 10/30/2010
ms.assetid: 68b8c023-92fa-4df6-9563-1764e16e4b04
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 6d4f38201eaa25097dcba589f35506204d3f73a3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756608"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>Ana/ayrıntı filtreleme iki sayfada (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_CS.exe) veya [PDF olarak indirin](master-detail-filtering-acess-two-pages-datalist-cs/_static/datatutorial34cs1.pdf)

> Bu öğreticide iki sayfada ana/ayrıntı raporu nasıl atacağız. "Ana" sayfasında kategori listesi, işlenecek tıklandığında, burada iki sütunlu DataList Seçili kategoriye ait olan ürünleri gösterir kullanıcı "Ayrıntıları" sayfasına götürür Repeater denetimiyle kullanın.


## <a name="introduction"></a>Giriş

İçinde [önceki öğretici](master-detail-filtering-with-a-dropdownlist-datalist-cs.md) tek web sayfasında "ana" kayıtları ve "ayrıntılarını." görüntülemek için bir DataList görüntülenecek DropDownList kullanarak ana/ayrıntı raporları görüntülemek nasıl gördük. Ana/ayrıntı raporları için kullanılan başka bir yaygın bir web sayfasındaki ana kayıtlarını ve diğer ayrıntıları modelidir. Önceki [ana/ayrıntı filtreleme arasında iki sayfa](../masterdetail/master-detail-filtering-across-two-pages-cs.md) Öğreticisi, biz incelenirken sağlayıcıların tümünü sistemde görüntülenecek GridView kullanarak bu deseni. Bu GridView boyunca geçirme ikinci sayfasında, bir bağlantı olarak işlenen bir HyperLinkField dahil `SupplierID` sorgu dizesi içinde. İkinci sayfasında GridView seçili sağlayıcısı tarafından sağlanan bu ürünlerin listelemek için kullanılır.

DataList ve Repeater denetimleri de kullanarak iki sayfalık ana/ayrıntı raporların gerçekleştirilebilir. Tek fark, ne DataList veya Repeater HyperLinkField denetim için destek sağlar. Bunun yerine, biz bir köprü Web denetimi veya rotasına bağlı HTML bağlayıcı öğesi eklemeniz gerekir (`<a>`) denetimin içindeki `ItemTemplate`. Köprü `NavigateUrl` özelliği veya yer işareti'nın `href` özniteliği sonra özelleştirilebilir bildirim temelli veya programlama yaklaşımları kullanarak.

Bu öğreticide, bir madde işaretli liste Repeater denetimiyle kullanarak tek bir sayfada kategorileri listeleyen örnek şunları keşfedeceğiz. Her liste öğesi, kategori adı ve açıklaması, ikinci bir sayfa için bir bağlantı olarak gösterilen kategori adı ile içerecektir. Bu bağlantıya tıklanması ikinci sayfasında, kullanıcıya bir DataList Seçili kategoriye ait bu ürünlerin burada gösterilir whisk.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>1. adım: bir madde işaretli listede kategorilerini görüntüleme

Herhangi bir ana/ayrıntı raporu oluşturmanın ilk adımı, "master" kayıtları görüntüleyerek başlatmaktır. Bu nedenle bizim ilk görev kategorileri "ana" sayfasında görüntülemek için gereklidir. Açık `CategoryListMaster.aspx` sayfasını `DataListRepeaterFiltering` klasöründe Repeater denetimiyle ekleyin ve akıllı etiketten yeni ObjectDataSource eklemek için iyileştirilmiş. Yeni ObjectDataSource yapılandırın, verilerden erişen `CategoriesBLL` sınıfın `GetCategories` metodu (bkz. Şekil 1).


[![ObjectDataSource CategoriesBLL sınıfın GetCategories yöntemi kullanmak üzere yapılandırma](master-detail-filtering-acess-two-pages-datalist-cs/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image1.png)

**Şekil 1**: ObjectDataSource kullanılacak yapılandırma `CategoriesBLL` sınıfın `GetCategories` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-acess-two-pages-datalist-cs/_static/image3.png))


Ardından, bir madde işaretli listedeki bir öğe olarak her kategori adını ve açıklamasını görüntüler gibi Repeater'ın şablonları tanımlayın. Şimdi henüz her bir kategoriye sahip hakkında endişe ayrıntıları sayfasına bağlantı. Aşağıdaki ObjectDataSource ve yineleyici için bildirim temelli bir biçimlendirme gösterilmektedir:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample1.aspx)]

Bu biçimlendirme tamamlandı, bir tarayıcı aracılığıyla bizim ilerleme durumunu görüntülemek için bir dakikanızı ayırın. Şekil 2 gösterildiği gibi yineleyici her kategorinin adını ve açıklamasını gösteren bir madde işaretli liste işler.


[![Her kategori bir madde işaretli liste öğesi görüntülenir](master-detail-filtering-acess-two-pages-datalist-cs/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image4.png)

**Şekil 2**: her kategori bir madde işaretli liste öğesi gösterilir ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-acess-two-pages-datalist-cs/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>2. adım: kategori adı ayrıntıları sayfasına bağlantı oturum açma.

Belirli bir kategorideki "details" bilgileri görüntülemek bir kullanıcı izin vermek için her madde işaretli liste öğesi, tıklanan, kullanıcı ikinci sayfasına götürür bağlantı eklemek ihtiyacımız (`ProductsForCategoryDetails.aspx`). Bu ikinci sayfa sonra bir DataList kullanarak seçili kategorideki ürünleri görüntüler. Bağlantısını tıklattığınız kategorisi belirlemek için tıklatılan kategorinin geçirilecek gerekiyor `CategoryID` bazı mekanizması aracılığıyla ikinci sayfada. Bu öğreticide kullanacağız seçeneği olan sorgu dizesi bir sayfadan diğerine skalar veri aktarmak için basit ve en kolay yolu var. Özellikle, `ProductsForCategoryDetails.aspx` sayfa beklediğiniz seçili *`categoryID`* adlı bir sorgu dizesi alanı geçirilecek değer `CategoryID`. Örneğin, İçecekler kategorisindeki ürünleri görüntülemek için sahip bir `CategoryID` 1, bir kullanıcının ziyaret `ProductsForCategoryDetails.aspx?CategoryID=1`.

Yineleyicideki ihtiyacımız ya da bir köprü Web denetimi veya bağlı HTML bağlayıcı öğesi eklemek için her bir madde işaretli liste öğesi için bir köprü oluşturmaya (`<a>`) için `ItemTemplate`. İçinde köprü olduğu senaryolar aynı her satır için gösterilen, her iki yöntemle yeterli olacaktır. Yineleyiciler için bağlayıcı bir öğe kullanmak istemiyorum. Bağlayıcı bir öğe kullanmak için Yineleyicinin'ın ItemTemplate için güncelleştirin:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample2.aspx)]

Unutmayın `CategoryID` doğrudan bağlantı öğenin içinde eklenen `href` özniteliği; ancak için böylece sınırlandırmak emin olmanız `href` beri kesme (ve Not tırnak işaretleri) özniteliğinin değeri `Eval` yöntemi içinde `href` özniteliği sınırlandırır, dize (`"CategoryID"`) tırnak işaretleri içinde yazın. Alternatif olarak, bunun yerine köprü Web denetimi kullanılabilir:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample3.aspx)]

Not nasıl statik bölümü URL'SİNİN — `ProductsForCategoryDetails.aspx?CategoryID` — sonucuna eklenir `Eval("CategoryID")` dize birleştirme kullanarak doğrudan veri bağlama söz dizimi içinde.

Köprü denetimini kullanmanın yararlarından biri olan Repeater 's programlı olarak erişilebilir `ItemDataBound` gerekirse olay işleyicisi. Örneğin, kategori adı metin yerine bir bağlantı ile ilişkili ürün kategorileri için olarak görüntülemek isteyebilirsiniz. Bu tür bir denetimi içinde program aracılığıyla gerçekleştirilebilir `ItemDataBound` olay işleyicisi; ürünleri, köprü 's ilişkili olmayan kategoriler için `NavigateUrl` özellik ayarlanabilir, belirli bir kategori adı böylece elde boş bir dize işleme düz metin (yerine bağlantı olarak). Kiracıurl [DataList ve Repeater göre verileri üzerine biçimlendirme](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) öğretici DataList ve Repeater'ın içeriklerini biçimlendirme hakkında daha fazla bilgi için programlama mantığını temel `ItemDataBound` olay işleyicisi.

İzliyorsanız, bağlayıcı bir öğe veya köprü denetim yaklaşım sayfanızın kullanmaktan çekinmeyin. Her kategori adı bir bağlantı olarak işlenecek bir tarayıcı aracılığıyla sayfayı görüntülerken yaklaşım bakılmaksızın `ProductsForCategoryDetails.aspx`, geçerli olarak geçen `CategoryID` değeri (bkz: Şekil 3).


[![Kategori adları için ProductsForCategoryDetails.aspx şimdi Bağla](master-detail-filtering-acess-two-pages-datalist-cs/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image7.png)

**Şekil 3**: kategori adları artık bağlantı `ProductsForCategoryDetails.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-acess-two-pages-datalist-cs/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>3. adım: seçilen bir kategoriye ait ürünleri listeleme

İle `CategoryListMaster.aspx` sayfası tümü, biz için "Ayrıntılar" sayfasında, uygulama ilgilenmemiz hazır `ProductsForCategoryDetails.aspx`. Bu sayfayı açın, tasarımcı araç kutusundan bir DataList sürükleyin ve ayarlayın, `ID` özelliğini `ProductsInCategory`. Ardından, yeni ObjectDataSource adlandırma sayfasına eklemek DataList'in akıllı etiketi seçin `ProductsInCategoryDataSource`. Çağırır gibi yapılandırma `ProductsBLL` sınıfın `GetProductsByCategoryID(categoryID)` yöntemi; açılan listeler INSERT, UPDATE ve DELETE sekmelerdeki (hiçbiri) kümesi.


[![ObjectDataSource ProductsBLL sınıfın GetProductsByCategoryID(categoryID) yöntemi kullanmak üzere yapılandırma](master-detail-filtering-acess-two-pages-datalist-cs/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image10.png)

**Şekil 4**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` sınıfın `GetProductsByCategoryID(categoryID)` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-acess-two-pages-datalist-cs/_static/image12.png))


Bu yana `GetProductsByCategoryID(categoryID)` yöntemi kabul giriş parametresi (*`categoryID`*), veri kaynağı Seç Sihirbazı'nı bize parametrenin kaynağını belirtmek için bir fırsat sunar. Parametre kaynağıyla QueryStringField kullanarak sorgu dizesine ayarlayın `CategoryID`.


[![Sorgu dizesi alanı CategoryID parametrenin kaynağı olarak kullanma](master-detail-filtering-acess-two-pages-datalist-cs/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image13.png)

**Şekil 5**: sorgu dizesi alanı kullanın `CategoryID` parametrenin kaynağı olarak ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-acess-two-pages-datalist-cs/_static/image15.png))


Önceki öğreticilerde, veri kaynağı Seç Sihirbazı tamamladıktan sonra anlatıldığı gibi Visual Studio otomatik olarak oluşturur bir `ItemTemplate` her veri alanı adını ve değerini listeler DataList için. Bu şablon, yalnızca ürün adı, tedarikçi ve fiyat listeleri biriyle değiştirin. Ayrıca DataList'in ayarlayın `RepeatColumns` özelliği 2. DataList ve ObjectDataSource bildirim temelli biçimlendirme bu değişikliklerden sonra aşağıdakine benzer görünmelidir:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample4.aspx)]

Bu sayfa görüntüleme için başlangıç `CategoryListMaster.aspx` sayfasında; ardından, kategoriler madde işaretli listedeki bir bağlantıya tıklayın. Bunun yapılması yönlendirilirsiniz için `ProductsForCategoryDetails.aspx`, geçen boyunca `CategoryID` aracılığıyla sorgu dizesi. `ProductsInCategoryDataSource` ObjectDataSource içinde `ProductsForCategoryDetails.aspx` sonra belirtilen kategori için yalnızca bu ürünlere ve her satır iki ürün işler DataList görüntüleyebilirsiniz. Şekil 6 gösteren ekran görüntüsü `ProductsForCategoryDetails.aspx` İçecekler görüntülerken.


[![İçecekler görüntülenir, iki satır başına](master-detail-filtering-acess-two-pages-datalist-cs/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image16.png)

**Şekil 6**: İçecekler görüntülenir, iki satır başına ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-acess-two-pages-datalist-cs/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>4. adım: ProductsForCategoryDetails.aspx kategori bilgileri görüntüleme

Kullanıcı tıkladığında içinde bir kategorinin `CategoryListMaster.aspx`, seçtiklerinde `ProductsForCategoryDetails.aspx` ve seçilen bir kategoriye ait ürünleri gösterilir. Bununla birlikte, `ProductsForCategoryDetails.aspx` hangi kategori seçildi için hiçbir görsel ipuçları vardır. Bunlar ulaştıktan sonra hiçbir şekilde kendi hata taahhüdünü gerçekleştirmeye İçecekler, ancak yanlışlıkla tıklandı Çeşniler tıklayın yönelik bir kullanıcının sahip `ProductsForCategoryDetails.aspx`. Bu olası bir sorunu gidermek için seçilen kategori hakkında bilgi görüntüleriz — adını ve açıklamasını — en üstündeki `ProductsForCategoryDetails.aspx` sayfası.

Bunu yapmak için yukarıda Repeater denetiminde bir FormView'da ekleme `ProductsForCategoryDetails.aspx`. Ardından, yeni ObjectDataSource adlı FormView akıllı etiketten sayfaya ekleyin `CategoryDataSource` ve kullanacak şekilde yapılandırma `CategoriesBLL` sınıfın `GetCategoryByCategoryID(categoryID)` yöntemi.


[![Kategori CategoriesBLL sınıfın GetCategoryByCategoryID(categoryID) yöntemi ile ilgili bilgilere erişim](master-detail-filtering-acess-two-pages-datalist-cs/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image19.png)

**Şekil 7**: kategori ile ilgili bilgilere erişim `CategoriesBLL` sınıfın `GetCategoryByCategoryID(categoryID)` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-acess-two-pages-datalist-cs/_static/image21.png))


Olduğu gibi `ProductsInCategoryDataSource` ObjectDataSource eklenen adım 3'te `CategoryDataSource`ait veri kaynağı Yapılandırma Sihirbazı için bir kaynak için bize ister `GetCategoryByCategoryID(categoryID)` yöntemi giriş parametresi. Parametre kaynağıyla QueryString ve QueryStringField değere ayarlanması tamamen aynı ayarları olarak önce kullanmak `CategoryID` (Şekil 5'e yeniden bakın).

Sihirbazı tamamladıktan sonra Visual Studio otomatik olarak oluşturur bir `ItemTemplate`, `EditItemTemplate`, ve `InsertItemTemplate` FormView için. Salt okunur bir arabirim sağlayarak bu yana kaldırmak çekinmeyin `EditItemTemplate` ve `InsertItemTemplate`. Ayrıca, FormView özelleştirme rahatça `ItemTemplate`. FormView ve ObjectDataSource bildirim temelli biçimlendirme, gereksiz şablonlarını kaldırma ve ItemTemplate özelleştirdikten sonra aşağıdakine benzer görünmelidir:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample5.aspx)]

Şekil 8, bir tarayıcı aracılığıyla bu sayfayı görüntülerken ekran gösterilmiştir.

> [!NOTE]
> FormView yanı sıra, ayrıca bir köprü denetimini kategori listesi kullanıcıya geri kazanır FormView yukarıda ekledim (`CategoryListMaster.aspx`). Bu bağlantıyı başka bir yere yerleştirmeniz veya tamamen atlamak için çekinmeyin.


[![Şimdi görüntülenen sayfanın üstündeki kategorisi olan](master-detail-filtering-acess-two-pages-datalist-cs/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image22.png)

**Şekil 8**: Kategori bilgilerdir şimdi görüntülenen sayfanın üstündeki ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-acess-two-pages-datalist-cs/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>5. adım: Bir ileti ürün yok, seçilen kategoriye ait görüntüleme

`CategoryListMaster.aspx` Sayfası bağımsız olarak, sistemdeki tüm kategorileri listeler olup ilişkili ürünleri. Bir kullanıcı bir kategori içinde DataList ilişkili hiçbir ürünlerle tıklatırsa `ProductsForCategoryDetails.aspx` kendi veri kaynağı herhangi bir öğe sahip olmadığınız, işlenmez. Önceki öğreticilerde anlatıldığı gibi GridView sağlar bir `EmptyDataText` kendi veri kaynağında kayıt varsa görüntülemek için bir kısa mesaj belirtmek için kullanılan özellik. Ne yazık ki, ne DataList veya Repeater böyle bir özellik vardır.

Kullanıcı, seçilen kategori için eşleşen ürün yok, eklemek ihtiyacımız bildiren bir ileti görüntülemek için bir etiket denetimi sayfasına ayarlanmış `Text` özelliği vardır olay, eşleşen ürün yok görüntülenecek iletiyi atanır. Daha sonra program üzerinden ayarlamak ihtiyacımız kendi `Visible` özelliği temel DataList herhangi bir öğe olup olmadığını içerir.

Bunu gerçekleştirmek için DataList altına bir etiket ekleyerek başlayın. Ayarlama, `ID` özelliğini `NoProductsMessage` ve kendi `Text` "Olan... seçilen kategori için bir ürün yok" özelliği Ardından, bu etiketin program üzerinden ayarlamak ihtiyacımız `Visible` özelliği temel olsun veya olmasın herhangi bir veri bağlıydı `ProductsInCategory` DataList. Bu atama için DataList veri bağlandıktan sonra yapılması gerekir. GridView DetailsView ve FormView için denetim için bir olay işleyicisi oluşturabilir `DataBound` veri bağlama tamamlandıktan sonra tetiklenen olayı. DataList veya Repeater ne ancak sahip bir `DataBound` olay kullanılabilir.

Söz konusu örnekte biz etiketin atayabilirsiniz `Visible` özelliğinde `Page_Load` olay işleyicisi, verileri DataList sayfanın önce atanmış beri `Load` olay. Ancak, bu yaklaşım, ObjectDataSource verilerden daha sonra sayfa yaşam döngüsü içinde DataList için bağlı olması gibi genel durumda çalışmıyordu. "Ana" kayıtlarını tutacak bir DropDownList kullanarak ana/ayrıntı rapor görüntülenirken olarak görüntülenen verileri başka bir denetimin değeri temel alıyorsa, örneğin, verileri kadar veri Web denetimi için DataSet'e değil `PreRender` içinde hazırlama Sayfa yaşam döngüsü.

Tüm durumlarda çalışır, tek bir çözüm olan atamak `Visible` özelliğini `False` DataList'in içinde `ItemDataBound` (veya `ItemCreated`) bir öğe türü, bağlama sırasında olay işleyicisi `Item` veya `AlternatingItem`. Olduğunu biliyoruz. böyle bir durumda, en az bir veri öğesi veri kaynağında ve bu nedenle gizleyebilirsiniz `NoProductsMessage` etiketi. Bu olay işleyicisi yanı sıra aynı zamanda bir olay işleyicisi DataList'in için ihtiyacımız `DataBinding` etiketin biz başlatmak burada olay `Visible` özelliğini `True`. Bu yana `DataBinding` olayı tetikler önce `ItemDataBound` olayları, etiket 's `Visible` özelliği başlangıçta ayarlanır `True`; herhangi bir veri öğesi yoksa ancak bunu ayarlanacak `False`. Aşağıdaki kod bu mantığı uygular:

[!code-csharp[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample6.cs)]

Tüm kategorileri Northwind veritabanındaki bir veya daha çok ürünlerin ile ilişkilidir. Bu özelliği test etmek için el ile Northwind veritabanı Bu öğretici için üretim kategorisiyle ilişkili tüm ürünleri elemanına ayarladım (`CategoryID` = 7) Deniz ürünleri kategorisine (`CategoryID` = 8). Bu Sunucu Gezgini'nden yeni sorgu seçme ve aşağıdakileri kullanarak gerçekleştirilebilir `UPDATE` deyimi:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample7.sql)]

Veritabanı buna göre güncelleştirdikten sonra geri dönmek `CategoryListMaster.aspx` sayfasında ve üretim bağlantıya tıklayın. Artık tüm ürünler ürün kategorisine ait olduğundan, Şekil 9'da gösterildiği gibi "Ürün yok... seçilen kategori için bulunur" iletisini görmeniz gerekir.


[![Kategori seçildi Hayır ürünleri ait varsa bir ileti görüntülenir.](master-detail-filtering-acess-two-pages-datalist-cs/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image25.png)

**Şekil 9**: No ürünleri ait kategori seçildi varsa bir ileti görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](master-detail-filtering-acess-two-pages-datalist-cs/_static/image27.png))


## <a name="summary"></a>Özet

Ana/ayrıntı raporları, tek bir sayfaya hem ana hem de ayrıntılı kayıtları görüntüleyebilirsiniz, ancak birçok Web Siteleri'nde, iki web sayfaları arasında ayrılır. Bu öğreticide "ana" web sayfasında bir yineleyici kullanarak bir madde işaretli liste listelenen kategorileri ve ilişkili ürünleri "Ayrıntılar" sayfasında listelenen sağlayarak böyle bir ana/ayrıntı raporu uygulamak nasıl inceledik. Her liste öğesi ana web sayfasında yer alan sıranın geçirilen ayrıntıları sayfasına bağlantı `CategoryID` değeri.

Ayrıntılar sayfasında belirtilen sağlayıcı için bu ürünlerin alınırken aracılığıyla gerçekleştirilmiştir `ProductsBLL` sınıfın `GetProductsByCategoryID(categoryID)` yöntemi. *`categoryID`* Bildirimli olarak kullanarak bir parametre belirtildi `CategoryID` parametre kaynağı olarak sorgu dizesi değeri. Ayrıca Ayrıntılar sayfasından bir FormView'da kullanma Kategori ayrıntılarını görüntülemek nasıl ve seçilen bir kategoriye ait bir ürün yok varsa bir ileti görüntülemek nasıl inceledik.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel performanstan...

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Zack Jones ve Liz Shulok yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)
> [İleri](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
