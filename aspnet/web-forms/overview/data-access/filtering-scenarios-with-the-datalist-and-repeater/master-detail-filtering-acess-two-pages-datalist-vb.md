---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
title: "Ana/ayrıntı iki sayfalardaki (VB) filtreleme | Microsoft Docs"
author: rick-anderson
description: "Bu öğreticide bir ana öğe/ayrıntı raporu iki sayfalara ayırmak nasıl ele. 'Ana' sayfasında categ listesini oluşturmak için bir yineleyici denetimi kullan..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2010
ms.topic: article
ms.assetid: f1a1be2c-6fd9-4a09-916e-aa1b98d5cf17
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f43fa998b81800cb1a2b7796ebb3922fc1caeb8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>Ana/ayrıntı iki sayfalardaki (VB) filtreleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_VB.exe) veya [PDF indirin](master-detail-filtering-acess-two-pages-datalist-vb/_static/datatutorial34vb1.pdf)

> Bu öğreticide bir ana öğe/ayrıntı raporu iki sayfalara ayırmak nasıl ele. "Ana" sayfasında tıklandığında, "Ayrıntılar" sayfasında kullanıcıya iki sütun DataList seçili kategorisine ait bu ürünlerden burada gösterir sürer kategori listesi, işlemek için bir yineleyici denetimi kullanın.


## <a name="introduction"></a>Giriş

İçinde [ana/ayrıntı filtreleme arasında iki sayfa](../masterdetail/master-detail-filtering-across-two-pages-vb.md) öğretici, biz incelenmesi sistemde sağlayıcıların tümünü görüntülemek için GridView kullanarak bu deseni. Bu GridView boyunca geçirme ikinci bir sayfaya bir bağlantı olarak işlenen bir HyperLinkField dahil `SupplierID` sorgu dizesi içinde. İkinci sayfa GridView seçili sağlayıcı tarafından sağlanan bu ürünleri listelemek için kullanılır.

Bu iki sayfalık ana/ayrıntı raporlar da DataList ve yineleyici denetimlerini kullanarak gerçekleştirilebilir. Tek fark DataList ne yineleyici desteği için HyperLinkField denetim sağlamasıdır. Bunun yerine, biz bir köprü Web denetimi veya HTML bağlayıcı bir öğe eklemelisiniz (`<a>`) denetimin içinde `ItemTemplate`. Köprü `NavigateUrl` özelliği veya bağlantı 's `href` özniteliği sonra özelleştirilebilir bildirim temelli veya programlama yaklaşımlar kullanılarak.

Bu öğreticide biz yineleyici denetimini kullanarak bir sayfada madde işaretli listede kategorilerini listeler örneği ele alacağız. Her liste öğesi kategorinin adını ve açıklamasını, ikinci bir sayfaya bir bağlantı olarak gösterilen kategori adı ile içerir. Bu bağlantıyı tıklatmak ikinci sayfasında, kullanıcıya bir DataList seçili kategorisine ait bu ürünlerden burada gösterecektir whisk.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>1. adım: kategorileri Madde işaretli listede görüntüleme

Ana/ayrıntı raporu oluşturma ilk adımı, "ana" kayıtları görüntüleyerek başlatmaktır. Bu nedenle, bizim ilk kategorileri "ana" sayfasında görüntülenecek bir görevdir. Açık `CategoryListMaster.aspx` sayfasındaki `DataListRepeaterFiltering` klasörü, yineleyici denetim ekleme ve yeni ObjectDataSource eklemek için akıllı etiketten opt. Böylece kendi verisinden erişen yeni ObjectDataSource yapılandırma `CategoriesBLL` sınıfının `GetCategories` yöntemi (bkz: Şekil 1).


[![ObjectDataSource CategoriesBLL sınıfının GetCategories yöntemi kullanmak üzere yapılandırma](master-detail-filtering-acess-two-pages-datalist-vb/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image1.png)

**Şekil 1**: ObjectDataSource kullanılacak yapılandırma `CategoriesBLL` sınıfının `GetCategories` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-acess-two-pages-datalist-vb/_static/image3.png))


Ardından, bir öğesiyle madde işaretli listede her kategori adı ve açıklaması görüntüler gibi yineleyici'nın şablonları tanımlayın. Şimdi henüz her kategori sahip hakkında endişelenmeniz ayrıntıları sayfasına bağlantı. Aşağıdaki Yineleyici ve ObjectDataSource için bildirim temelli biçimlendirme gösterir:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample1.aspx)]

Bu biçimlendirme tam, bir tarayıcı aracılığıyla bizim ilerleme durumunu görüntülemek için bir dakikanızı ayırın. Şekil 2'de görüldüğü gibi yineleyici her kategorinin adını ve açıklamasını gösteren madde işaretli bir liste olarak işler.


[![Her kategori madde işaretli bir liste öğesi olarak görüntülenir](master-detail-filtering-acess-two-pages-datalist-vb/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image4.png)

**Şekil 2**: her kategori madde işaretli bir liste öğesi olarak görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-acess-two-pages-datalist-vb/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>2. adım: ayrıntıları sayfasına bir bağlantı içine kategori adı kapatma

Belirtilen kategori "Ayrıntılar" bilgilerini görüntülemek bir kullanıcının izin vermek için her madde işaretli liste öğesini, tıklandığında, sihirbazın ikinci sayfasında kullanıcıya sürer bağlantı eklemek ihtiyacımız (`ProductsForCategoryDetails.aspx`). Bu ikinci sayfasında sonra DataList kullanılarak seçilen kategori ürünleri görüntüler. Bağlantısını tıklattınız kategori belirlemek için tıklatılan kategorinin geçirmek ihtiyacımız `CategoryID` bazı mekanizması aracılığıyla ikinci sayfasına. Bu öğreticide kullanacağız seçeneği olduğu querystring bir sayfadan diğerine skaler veri aktarmak için basit ve en kolay yolu var. Özellikle, `ProductsForCategoryDetails.aspx` sayfa beklediğiniz seçili  *`categoryID`*  adlı bir sorgu dizesi alanı iletilecek değeri `CategoryID`. Örneğin, İçecekler kategorisini ürünleri görüntülemek için sahip olduğu bir `CategoryID` 1, bir kullanıcının ziyaret `ProductsForCategoryDetails.aspx?CategoryID=1`.

Ya da bir köprü Web denetimi veya bağlı HTML bağlayıcı öğesi eklemek için ihtiyacımız Yineleyicideki her madde işaretli liste öğesi için köprü oluşturma (`<a>`) için `ItemTemplate`. İçinde köprü olduğu senaryolar görüntülenen her satır için aynı, her iki yaklaşım yeterli olacaktır. Yineleyiciler için bağlayıcı bir öğe kullanarak tercih ediyorum. Bağlayıcı öğenin kullanmak için yineleyici'nın ItemTemplate güncelleştirin:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample2.aspx)]

Unutmayın `CategoryID` doğrudan bağlantı öğenin yerleştirilebilir `href` özniteliği; ancak, için böylece sınırlandırmak emin olmanız `href` itibaren kesme (ve Not tırnak işaretleri) özniteliğinin değeri `Eval` yöntemi içinde `href` özniteliği sınırlandırır, dize (`"CategoryID"`) tırnak işareti. Alternatif olarak, bunun yerine bir köprü Web denetimi kullanılabilir:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample3.aspx)]

Not nasıl URL'nin statik kısmı — `ProductsForCategoryDetails.aspx?CategoryID` — sonucuna eklenir `Eval("CategoryID")` dize birleştirme kullanarak doğrudan veri bağlama söz dizimi içinde.

Köprü denetim kullanmanın bir avantajı olan yineleyici 's program aracılığıyla erişilen `ItemDataBound` gerekirse olay işleyicisi. Örneğin, kategori adı metin olarak yerine bir bağlantı ile ilişkili ürün kategorileri için olarak görüntülemek isteyebilirsiniz. Bu tür bir denetimi içinde program aracılığıyla gerçekleştirilebilir `ItemDataBound` olay işleyicisi; ürünleri, köprüyü 's ilişkili hiçbir kategorileri için `NavigateUrl` özelliğinin böylece bu belirli kategori adı elde boş bir dizesine ayarlanması işleme düz metin olarak (yerine bağlantı olarak). Geri başvurmak [DataList ve bağlı yineleyici göre verileri biçimlendirme](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) programlı mantığı öğretici DataList ve yineleyici'nin içeriği biçimlendirme hakkında daha fazla bilgi için temel `ItemDataBound` olay işleyicisi.

İzliyorsanız, bağlayıcı bir öğe veya köprü denetimi yaklaşımını, sayfada kullanılacak çekinmeyin. Sayfayı her kategori adı işlenen bağlantı olarak bir tarayıcı aracılığıyla görüntülerken yaklaşım, bağımsız olarak `ProductsForCategoryDetails.aspx`, geçerli olarak geçen `CategoryID` değeri (bkz: Şekil 3).


[![Kategori adları şimdi ProductsForCategoryDetails.aspx için bağlantı](master-detail-filtering-acess-two-pages-datalist-vb/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image7.png)

**Şekil 3**: kategori adları şimdi bağlantı `ProductsForCategoryDetails.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-acess-two-pages-datalist-vb/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>3. adım: seçili kategorisine ait ürünleri listeleme

İle `CategoryListMaster.aspx` sayfası tümü, biz "Ayrıntılar" sayfasında, uygulama için uygulamamızla etkinleştirmek hazır `ProductsForCategoryDetails.aspx`. Bu sayfayı açın, araç tasarımcıya DataList sürükleyin ve ayarlayın, `ID` özelliğine `ProductsInCategory`. Ardından, yeni ObjectDataSource adlandırma sayfasına eklemek için DataList'ın akıllı etiketten seçin `ProductsInCategoryDataSource`. Çağırır gibi yapılandırma `ProductsBLL` sınıfının `GetProductsByCategoryID(categoryID)` yöntemi; aşağı açılan listeler (hiçbiri) INSERT, UPDATE ve DELETE sekmelerdeki kümesi.


[![ObjectDataSource ProductsBLL sınıfının GetProductsByCategoryID(categoryID) yöntemi kullanmak üzere yapılandırma](master-detail-filtering-acess-two-pages-datalist-vb/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image10.png)

**Şekil 4**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` sınıfının `GetProductsByCategoryID(categoryID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-acess-two-pages-datalist-vb/_static/image12.png))


Bu yana `GetProductsByCategoryID(categoryID)` yöntemi giriş parametresi kabul eder (*`categoryID`*), veri kaynağı Seç Sihirbazı'nı bize parametrenin kaynağı belirtmek için bir fırsat sunar. Parametre kaynağını QueryStringField kullanarak sorgu dizesine ayarlayın `CategoryID`.


[![Sorgu dizesi alanı adlı kullanıcı, Categoryıd'si parametrenin kaynağı olarak kullanın](master-detail-filtering-acess-two-pages-datalist-vb/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image13.png)

**Şekil 5**: sorgu dizesi alanı kullanmak `CategoryID` parametrenin kaynağı olarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-acess-two-pages-datalist-vb/_static/image15.png))


Veri Kaynağı Seç Sihirbazı'nı tamamladıktan sonra önceki eğitimlerine anlatıldığı gibi Visual Studio otomatik olarak oluşturur bir `ItemTemplate` her veri alanı adı ve değeri listeler DataList için. Bu şablon, yalnızca ürün adı, üretici ve fiyat listeleri bir değiştirin. Ayrıca, DataList's ayarlamak `RepeatColumns` özelliğini 2. Bu değişikliklerden sonra DataList ve ObjectDataSource'nın bildirim temelli biçimlendirmesi aşağıdakine benzer görünmelidir:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample4.aspx)]

Eylem bu sayfayı görüntülemek için başlangıç `CategoryListMaster.aspx` sayfasında; ardından, kategoriler madde işaretli listede bir bağlantıyı tıklatın. Bunun yapılması yönlendirilirsiniz için `ProductsForCategoryDetails.aspx`, boyunca geçen `CategoryID` aracılığıyla sorgu dizesi. `ProductsInCategoryDataSource` İçinde ObjectDataSource `ProductsForCategoryDetails.aspx` sonra belirtilen kategori için yalnızca bu ürünler ve onları satır başına iki ürün işler DataList görüntüleyebilirsiniz. Şekil 6 gösteren ekran görüntüsü `ProductsForCategoryDetails.aspx` Meşrubat görüntülerken.


[![Meşrubat görüntülenir, iki satır başına](master-detail-filtering-acess-two-pages-datalist-vb/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image16.png)

**Şekil 6**: Meşrubat görüntülenir, iki satır başına ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-acess-two-pages-datalist-vb/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>4. adım: ProductsForCategoryDetails.aspx üzerinde kategori bilgileri görüntüleme

Bir kullanıcı tıkladığında bir kategorideki üzerinde `CategoryListMaster.aspx`, için geçen `ProductsForCategoryDetails.aspx` ve seçili kategorisine ait ürünleri gösterilir. Bununla birlikte, `ProductsForCategoryDetails.aspx` hangi kategori seçilmedi konusunda hiçbir görsel ipuçları vardır. Meşrubat, ancak yanlışlıkla tıklatılan Condiments tıklatın yönelik bir kullanıcı, bunlar ulaştıktan sonra hiçbir şekilde kendi hata kullanıldıklarını sahip `ProductsForCategoryDetails.aspx`. Bu olası sorunu gidermek için şu seçilen kategori bilgilerini görüntüleyebilirsiniz — adını ve açıklamasını — en üstündeki `ProductsForCategoryDetails.aspx` sayfası.

Bunu başarmak için yukarıda yineleyici denetiminde FormView eklemek `ProductsForCategoryDetails.aspx`. Ardından, yeni ObjectDataSource adlı FormView'ın akıllı etiketten sayfasına ekleme `CategoryDataSource` ve kullanacak şekilde yapılandırın `CategoriesBLL` sınıfının `GetCategoryByCategoryID(categoryID)` yöntemi.


[![Kategori CategoriesBLL sınıfının GetCategoryByCategoryID(categoryID) yöntemi ile ilgili bilgilere erişim sağlar](master-detail-filtering-acess-two-pages-datalist-vb/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image19.png)

**Şekil 7**: kategori ile ilgili bilgilere erişim `CategoriesBLL` sınıfının `GetCategoryByCategoryID(categoryID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-acess-two-pages-datalist-vb/_static/image21.png))


İle `ProductsInCategoryDataSource` ObjectDataSource eklenen adım 3'te `CategoryDataSource`ait veri kaynağı Yapılandırma Sihirbazı için bir kaynak için bize ister `GetCategoryByCategoryID(categoryID)` giriş parametresi yöntemini. Parametre kaynağı QueryString ve QueryStringField değerine ayarlamayı tam aynı ayarları önce kullanmak `CategoryID` (Şekil 5'e yeniden bakın).

Sihirbazı tamamladıktan sonra Visual Studio otomatik olarak oluşturur bir `ItemTemplate`, `EditItemTemplate`, ve `InsertItemTemplate` FormView için. Bir salt okunur arabirimi sağlamakta olduğumuz beri kaldırmak çekinmeyin `EditItemTemplate` ve `InsertItemTemplate`. Ayrıca, FormView's özelleştirme çekinmeyin `ItemTemplate`. Gereksiz tüm şablonları kaldırarak ve ItemTemplate özelleştirme sonra FormView ve ObjectDataSource'nın bildirim temelli biçimlendirmesi aşağıdakine benzer görünmelidir:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample5.aspx)]

Şekil 8 Bu sayfayı bir tarayıcı aracılığıyla görüntülerken ekran gösterir.

> [!NOTE]
> FormView yanı sıra, aynı zamanda bir köprü denetim kategori listesi, kullanıcıya geri sürer FormView yukarıda ekledim (`CategoryListMaster.aspx`). Bu bağlantıyı başka bir yere koyun veya tamamen geçmek için çekinmeyin.


[![Şimdi görüntülenen sayfanın üst kısmındaki kategori bilgileri değil](master-detail-filtering-acess-two-pages-datalist-vb/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image22.png)

**Şekil 8**: Kategori bilgileri olduğu şimdi görüntülenen sayfanın üst kısmındaki ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-acess-two-pages-datalist-vb/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>5. adım: Seçilen kategoriye yüklenecek hiç ürün ait bir ileti görüntüleme

`CategoryListMaster.aspx` Sayfa bakılmaksızın sistemdeki tüm kategorileri listeler olup ilişkili ürünler. Bir kullanıcı DataList ilişkili hiçbir ürünlerle kategorisine tıklarsa `ProductsForCategoryDetails.aspx` kendi veri kaynağı herhangi bir öğeyi sahip olmadığınız, işlenmez. Geçmiş eğitimlerine anlatıldığı gibi GridView sağlayan bir `EmptyDataText` kendi veri kaynağında hiç kayıt varsa görüntülemek için bir kısa mesaj belirtmek için kullanılan özellik. Ne yazık ki, ne DataList ne de yineleyici böyle bir özellik vardır.

Seçilen kategori için eşleşen hiç ürün yok, eklemek ihtiyacımız kullanıcı bildiren bir ileti görüntülemek için bir etiket kontrol sayfasına özelliği `Text` özelliği vardır gerektiğinde, eşleşen hiç ürün görüntülenecek iletiyi atanır. Ardından program aracılığıyla ayarlamak ihtiyacımız kendi `Visible` özelliğinin temel DataList tüm öğeleri desteklemediğini içerir.

Bunu gerçekleştirmek için bir etiket DataList altına ekleyerek başlatın. Ayarlama, `ID` özelliğine `NoProductsMessage` ve kendi `Text` "... seçilen kategori için hiç ürün yok" özelliği Ardından, bu etiketin programlı olarak ayarlamak ihtiyacımız `Visible` özelliği dayalı olsun veya olmasın herhangi bir veri bağlıydı `ProductsInCategory` DataList. Bu atama verileri DataList bağlı sonra yapılması gerekir. GridView, DetailsView ve FormView için denetim için bir olay işleyicisi oluşturun `DataBound` databinding tamamlandıktan sonra ateşlenir olayı. Ancak, DataList ne yineleyici sahip bir `DataBound` olay kullanılabilir.

Söz konusu örnekte biz etiketin atayabilirsiniz `Visible` özelliğinde `Page_Load` veri DataList sayfanın önce atanmış beri olay işleyicisi `Load` olay. Ancak, bu yaklaşım ObjectDataSource verilerden DataList denetimini daha sonra sayfanın yaşam döngüsü için bağlı olması gibi genel durumda çalışır değil. "Ana" kayıtları tutmak için bir DropDownList kullanarak ana/ayrıntı raporu görüntülerken, olduğu gibi görüntülenen verileri başka bir denetimdeki değeri temel aldığı, örneğin, verileri veri Web denetimine kadar DataSet'e değil `PreRender` içinde hazırlama sayfanın yaşam döngüsü.

Tüm durumlarda çalışacak bir çözümüdür atamak için `Visible` özelliğine `False` DataList's içinde `ItemDataBound` (veya `ItemCreated`) bir öğe türü bağlama sırasında olay işleyicisi `Item` veya `AlternatingItem`. Olduğunu biliyoruz böyle bir durumda, en az bir veri öğesi veri kaynağında'ı ve bu nedenle gizleyebilirsiniz `NoProductsMessage` etiketi. Bu olay işleyicisi ek olarak, aynı zamanda bir olay işleyicisi DataList için 's ihtiyacımız `DataBinding` etiketin biz başlatma burada olay `Visible` özelliğine `True`. Bu yana `DataBinding` olay harekete önce `ItemDataBound` olayları, etiket 's `Visible` özelliği başlangıçta şekilde ayarlanacak `True`; tüm veri öğeler varsa ancak onu ayarlanacak `False`. Aşağıdaki kod bu mantığı uygular:

[!code-vb[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample6.vb)]

Tüm kategorilerini Northwind veritabanındaki bir veya daha fazla ürünleri ile ilişkilendirilir. Bu özelliği test etmek için ı el ile Northwind veritabanı Bu öğretici için ürün kategorisi ile ilişkili tüm ürünleri yeniden atama ayarlamalar yaptınız (`CategoryID` = 7) Deniz ürünleri kategorisine (`CategoryID` = 8). Bu Sunucu Gezgini'nden yeni sorgu seçme ve aşağıdaki kullanılarak gerçekleştirilebilir `UPDATE` deyimi:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample7.sql)]

Veritabanı uygun şekilde güncelleştirdikten sonra geri dönüp `CategoryListMaster.aspx` sayfasında ve üretim bağlantıyı tıklatın. Artık olduğundan ürettiği kategorisine ait herhangi bir ürün, görmeniz gerekir "... seçilen kategori için hiç ürün yok" Şekil 9'da gösterildiği gibi ileti.


[![Seçilen kategori yok ürünleri ait varsa bir ileti görüntülenir](master-detail-filtering-acess-two-pages-datalist-vb/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image25.png)

**Şekil 9**: Hayır ürünleri ait seçilen kategori varsa bir ileti görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-acess-two-pages-datalist-vb/_static/image27.png))


## <a name="summary"></a>Özet

Ana/ayrıntı raporları tek bir sayfada hem ana hem de ayrıntı kayıtlarını görüntüleyebilirsiniz, ancak birçok Web sitelerinde bunlar arasında iki web sayfaları ayrılır. Bu öğreticide nasıl "ana" web sayfasında bir yineleyici kullanarak madde işaretli listede listelenen kategoriler ve "Ayrıntılar" sayfasında listelenen ilişkili ürünler sağlayarak böyle bir ana öğe/ayrıntı raporu uygulanacağı inceledik. Sıranın geçirilen ayrıntıları sayfasına bir bağlantı her liste öğesi ana web sayfasında bulunan `CategoryID` değeri.

Ayrıntılar sayfasında bu ürünler için belirtilen tedarikçi alma yoluyla gerçekleştirilmiştir `ProductsBLL` sınıfının `GetProductsByCategoryID(categoryID)` yöntemi.  *`categoryID`*  Parametre değeri bildirimli olarak kullanarak belirtilen `CategoryID` parametresi kaynağı olarak sorgu dizesi değeri. Ayrıca bir FormView kullanarak Ayrıntılar sayfasında kategori ayrıntıları görüntülemek nasıl ve seçili kategorisine ait hiç ürün olsaydı bir ileti görüntülemek nasıl inceledik.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler...

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Zack Can ve Liz Shulok yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
[sonraki](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)
