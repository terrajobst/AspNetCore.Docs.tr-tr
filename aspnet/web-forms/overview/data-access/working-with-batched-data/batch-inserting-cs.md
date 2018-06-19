---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
title: Toplu ekleme (C#) | Microsoft Docs
author: rick-anderson
description: Tek bir işlemde birden çok veritabanı kayıtları eklemek öğrenin. Kullanıcı arabirimi katmanında birden çok n girmesini izin vermek için GridView genişletmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: cf025e08-48fc-4385-b176-8610aa7b5565
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
msc.type: authoredcontent
ms.openlocfilehash: c8995592d9206fb17a7769414212369946304c54
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888415"
---
<a name="batch-inserting-c"></a>Toplu ekleme (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_CS.zip) veya [PDF indirin](batch-inserting-cs/_static/datatutorial66cs1.pdf)

> Tek bir işlemde birden çok veritabanı kayıtları eklemek öğrenin. Kullanıcı arabirimi katmanında birden çok yeni kayıtları girmek izin vermek için GridView genişletir. Veri erişim katmanı'ndaki tüm eklemeleri başarılı veya tüm eklemeleri geri emin olmak için bir işlem içinde birden çok ekleme işlemleri alın.


## <a name="introduction"></a>Giriş

İçinde [toplu güncelleştirme](batch-updating-cs.md) Aranan burada birden çok kayıt düzenlenebilir bir arabirim sunmak için GridView denetimini özelleştirme adresindeki öğretici. Sayfasını ziyaret kullanıcı, bir dizi değişikliği yapın ve sonra toplu güncelleştirme bir düğmeye tıklamayla gerçekleştirin. Kullanıcıların yaygın olarak güncelleştirme tek bir seferde çok sayıda kayıt durumlarda, böyle bir arabirim sayısız tıklama kaydedebilir ve varsayılan karşılaştırıldığında klavye ve fare İçerik Geçişi başına satır ilk geri incelediniz düzenleme özellikleri [bir Ekleme, güncelleştirme ve silme veri genel bakış](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) Öğreticisi.

Bu kavram kayıtları eklerken uygulanabilir. Düşünün burada Northwind Traders yaygın olarak sevkiyat belirli bir kategorideki ürünleri bir sayı içermesi sağlayıcılardan gelen aldığımız. Örnek olarak, biz sevkiyat altı farklı Çay ve kahve ürünleri Tokyo Traders ' alabilirsiniz. Bir kullanıcı aynı anda DetailsView denetimi ile altı ürünleri bir girerse, bunlar aynı değerlere çoğunu tekrar tekrar seçin gerekecektir: aynı kategori (Meşrubat) aynı üretici (Tokyo Traders) seçmeniz gerekir, aynı dönüştürülmesine değeri () False) ve aynı sıra değeri (0) birimlerde. Bu yinelenen veri girişi yalnızca zaman alıcıdır değildir, ancak hatalara açıktır.

Küçük bir iş ile kullanıcının tedarikçi kategori kez, bir dizi ürün adları ve birim fiyatları girin ve yeni ürünler veritabanına eklemek için düğmeyi tıklatın seçmesini sağlayan arabirim ekleme toplu iş oluşturabilir (bkz: Şekil 1). Her ürünün eklendikçe kendi `ProductName` ve `UnitPrice` veri alanları metin kutularına, girilen değerler atanır sırada kendi `CategoryID` ve `SupplierID` değerler değerleri üst fo formun en DropDownLists'ndan atanır. `Discontinued` Ve `UnitsOnOrder` değerler sabit kodlanmış değerler ayarlanır `false` ve 0, sırasıyla.


[![Toplu ekleme arabirimi](batch-inserting-cs/_static/image2.png)](batch-inserting-cs/_static/image1.png)

**Şekil 1**: toplu ekleme arabirimi ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-inserting-cs/_static/image3.png))


Bu öğreticide Şekil 1'de gösterilen arabirimi ekleme toplu uygulayan bir sayfa oluşturacağız. Olarak önceki iki öğreticileri ile biz eklemeleri kararlılık sağlamak için bir işlem kapsamı içinde kaydırılır. Let s başlayın!

## <a name="step-1-creating-the-display-interface"></a>1. adım: görüntü arabirimi oluşturma

Bu öğretici iki bölgelere ayrılmış tek bir sayfayla oluşur: bir görüntü bölgesi ve bir ekleme bölgesi. Bu adımda oluşturacağız, görüntü arabirimi bir GridView ürünlerini gösterir ve işlem ürün sevkiyat başlıklı bir düğme içerir. Bu düğmeye tıklandığında, görüntü arabirimi Şekil 1'de gösterilen ekleme arabirimi ile değiştirilir. Görüntü arabirimi sonra ürün ekleme yapılan sevkiyat döndürür veya İptal düğmeleri tıklattınız. 2. adımda ekleme arabirimi oluşturacağız.

Bir sayfa oluşturma yalnızca biri aynı anda görünür, iki arabirim olduğunda her bir arabirime içinde genellikle yerleştirilir bir [Masası Web denetimi](http://www.w3schools.com/aspnet/control_panel.asp), diğer denetimler için bir kapsayıcı olarak sunar. Bu nedenle, sayfamızı iki Panel denetimleri bir her arabirim için olacaktır.

Başlangıç açarak `BatchInsert.aspx` sayfasındaki `BatchData` klasörü ve araç tasarımcıya sürükleyin bir Panel (bkz: Şekil 2). S Masası belirlenen `ID` özelliğine `DisplayInterface`. Bölmenin Designer'a eklerken, `Height` ve `Width` özellikler ayarlanır 50px ve 125px, sırasıyla. Bu özellik değerlerini Özellikler penceresinde Temizle.


[![Bir Panel tasarımcıya araç çubuğuna sürükleyin](batch-inserting-cs/_static/image5.png)](batch-inserting-cs/_static/image4.png)

**Şekil 2**: bir panelin tasarımcıya araç çubuğuna sürükleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-inserting-cs/_static/image6.png))


Ardından, bir düğmeyi ve GridView denetimi paneline sürükleyin. S düğmesi ayarlamak `ID` özelliğine `ProcessShipment` ve kendi `Text` işlem ürün sevkiyat özelliğine. GridView s ayarlamak `ID` özelliğine `ProductsGrid` ve akıllı etiketten adlı yeni bir ObjectDataSource bağlayın `ProductsDataSource`. ObjectDataSource kendi verisinden isteyecek şekilde yapılandırma `ProductsBLL` s sınıfı `GetProducts` yöntemi. Bu GridView, yalnızca verileri görüntülemek için kullanıldığından, güncelleştirme, ekleme, açılan listeleri ayarlayın ve sekmeleri (hiçbiri) SİLİN. Veri Kaynağı Yapılandırma Sihirbazı'nı tamamlamak için Son'u tıklatın.


[![S ProductsBLL sınıfı GetProducts yönteminden döndürülen verileri görüntüleme](batch-inserting-cs/_static/image8.png)](batch-inserting-cs/_static/image7.png)

**Şekil 3**: döndürülen veriler görüntülemek `ProductsBLL` s sınıfı `GetProducts` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-inserting-cs/_static/image9.png))


[![Güncelleştirme, ekleme, açılan listeleri ayarlayın ve sekmeleri (hiçbiri) silme](batch-inserting-cs/_static/image11.png)](batch-inserting-cs/_static/image10.png)

**Şekil 4**: aşağı açılan listeler güncelleştirme, ekleme ve silme sekmeler (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-inserting-cs/_static/image12.png))


ObjectDataSource Sihirbazı'nı tamamladıktan sonra Visual Studio BoundFields ve ürün veri alanları için CheckBoxField ekleyeceksiniz. Kaldırma dışındaki tüm `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`, ve `Discontinued` alanları. Estetik tüm özelleştirmeler çekinmeyin. Biçimlendirme karar `UnitPrice` alan bir para birimi değeri olarak alanları kaldırılmasında ve bazı alanlar yeniden adlandırıldı `HeaderText` değerleri. Ayrıca disk belleği ve Destek GridView s akıllı etiket etkinleştirmek disk belleği ve etkinleştirme sıralama onay kutularını işaretleyerek sıralama dahil olmak üzere GridView yapılandırın.

Paneli, düğmesi, GridView ve ObjectDataSource denetimler ekleme ve GridView s alanlarını özelleştirme sonra sayfa s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](batch-inserting-cs/samples/sample1.aspx)]

Düğmesini ve GridView için biçimlendirme görünür ve açma ve kapatma içinde Not `<asp:Panel>` etiketler. Bu denetimler içinde olduğundan `DisplayInterface` paneli, biz Gizle bunları s paneli ayarlayarak `Visible` özelliğine `false`. 3. adım arar s paneli programlı adresindeki `Visible` düğmesine yanıtında özelliği diğer gizleme çalışırken bir arabirim göster için tıklayın.

Bir tarayıcı aracılığıyla bizim ilerleme durumunu görüntülemek için bir dakikanızı ayırın. Şekil 5 gösterildiği gibi aynı anda on ürünleri listeler GridView işlem ürün sevkiyat düğmesini görmeniz gerekir.


[![GridView ürünleri listeler ve sıralama ve yetenekleri disk belleği sunar](batch-inserting-cs/_static/image14.png)](batch-inserting-cs/_static/image13.png)

**Şekil 5**: GridView ürünleri ve sunar sıralama ve disk belleği özellikleri listelenmektedir ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-inserting-cs/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>2. adım: ekleme arabirimi oluşturma

Tam ekran arabirimiyle biz ekleme oluşturmak için hazır re arabirim. Bu öğretici için bir üretici ve kategori için tek bir değer ister ve en fazla beş ürün adları ve birim fiyat değerleri girmesini sağlayan bir ekleme arabirim oluşturmak s olanak tanır. Bu arabirim, kullanıcının tüm aynı kategoriye ve tedarikçi paylaşır, ancak benzersiz ürün adları ve fiyatlarını içeren bir ile beş yeni ürünler ekleyebilirsiniz.

Araç kutusu varolan altındaki yerleştirme tasarımcıya'ndan bir Panel sürükleyerek başlangıç `DisplayInterface` paneli. Ayarlama `ID` bu özelliği yeni eklenen paneline `InsertingInterface` ve kendi `Visible` özelliğine `false`. Ayarlar kodu ekleyeceğiz `InsertingInterface` Masası s `Visible` özelliğine `true` adım 3'te. Ayrıca s panelini dışarı temizleyin `Height` ve `Width` özellik değerleri.

Ardından, biz Şekil 1'de gösterilen ekleme arabirimi oluşturmanız gerekir. Bu arabirim HTML teknikleri çeşitli oluşturulabilir ancak oldukça basit bir kullanacağız: dört sütunlu, yedi satır bir tablo.

> [!NOTE]
> İşaretleme için HTML girerken `<table>` öğeleri tercih ediyorum kaynak görünümü kullanmak. Visual Studio Araçları eklemek için varken `<table>` öğelerini Tasarımcısı aracılığıyla Tasarımcı görünüyor eklemesine tüm çok istekli için sorulmamış `style` işaretleme içine ayarları. I oluşturduktan sonra `<table>` biçimlendirme, ı genellikle dönüş Web denetimleri ekleme ve bunların özelliklerini ayarlamak için Designer'a. Önceden belirlenen sütunları ve satırları tablo oluştururken, statik HTML kullanarak tercih ediyorum yerine [Tablo Web denetimi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) yerleştirilmiş bir tablo Web denetimi tüm Web denetimleri yalnızca kullanılarak erişilebilir olduğundan `FindControl("controlID")` düzeni. Ancak, Tablo Web denetimleri tablolar için (yalnızca satır veya sütun bazı veritabanı veya kullanıcı tarafından belirtilen ölçütleri temel olanları), dinamik olarak ölçekli tablo denetim programlı olarak oluşturulabilir Web itibaren kullanabilirim.


İçinde aşağıdaki biçimlendirme girin `<asp:Panel>` etiketlerinin `InsertingInterface` paneli:


[!code-html[Main](batch-inserting-cs/samples/sample2.html)]

Bu `<table>` biçimlendirme herhangi bir Web denetim içermez henüz bu bir anlık olarak ekleyeceğiz. Unutmayın, her `<tr>` öğe belirli bir CSS sınıfı ayarı içeriyor: `BatchInsertHeaderRow` burada Tedarikçi ve kategori DropDownLists çıkar; üstbilgisi satır için `BatchInsertFooterRow` burada sevkiyat ve İptal düğmeleri ekleme ürünleri çıkar; altbilgi satır ve değişen `BatchInsertRow` ve `BatchInsertAlternatingRow` ürünü ve birimi içeren satırları değerlerini fiyat TextBox denetimleri. T ve karşılık gelen CSS sınıfları oluşturulan `Styles.css` ekleme arabirimi GridView ve DetailsView benzer bir görünüm vermek için dosya Biz bu öğreticileri kullanılan ve denetler. Bu CSS sınıfları aşağıda verilmiştir.


[!code-css[Main](batch-inserting-cs/samples/sample3.css)]

Girilen bu biçimlendirme ile Tasarım görünümüne dönün. Bu `<table>` Şekil 6 gösterildiği gibi dört sütunlu, yedi satır bir Tablo Tasarımcısı'nda olarak göstermesi gerekir.


[![Ekleme arabirimi oluşan bir dört sütunlu, yedi satırlı bir tablo,](batch-inserting-cs/_static/image17.png)](batch-inserting-cs/_static/image16.png)

**Şekil 6**: ekleme arabirimi oluşur bir dört sütunlu, yedi satırlı bir tablo, ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-inserting-cs/_static/image18.png))


Şu anda yeniden Web denetimleri ekleme arabirimi eklemek için hazır. İki DropDownLists Araç Kutusu'ndan tablosundaki bir üretici ve kategori için uygun hücrelere sürükleyin.

DropDownList s sağlayıcısına ayarlamak `ID` özelliğine `Suppliers` ve adlı yeni bir ObjectDataSource bağlama `SuppliersDataSource`. Kendi verileri almak için yeni ObjectDataSource yapılandırma `SuppliersBLL` s sınıfı `GetSuppliers` yöntemi ve küme güncelleştirme sekmesinde s aşağı açılan listesi (hiçbiri). Sihirbazı tamamlamak için Son'u tıklatın.


[![ObjectDataSource s SuppliersBLL sınıfı GetSuppliers yöntemi kullanmak üzere yapılandırma](batch-inserting-cs/_static/image20.png)](batch-inserting-cs/_static/image19.png)

**Şekil 7**: ObjectDataSource kullanılacak yapılandırma `SuppliersBLL` s sınıfı `GetSuppliers` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-inserting-cs/_static/image21.png))


Sahip `Suppliers` DropDownList görüntü `CompanyName` veri alan ve kullanım `SupplierID` veri alanı olarak kendi `ListItem` s değerleri.


[![ŞirketAdı veri alanını görüntüler ve SupplierID değeri olarak kullanır.](batch-inserting-cs/_static/image23.png)](batch-inserting-cs/_static/image22.png)

**Şekil 8**: görüntü `CompanyName` veri alan ve kullanım `SupplierID` değeri olarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-inserting-cs/_static/image24.png))


İkinci DropDownList ad `Categories` ve adlı yeni bir ObjectDataSource bağlama `CategoriesDataSource`. Yapılandırma `CategoriesDataSource` kullanılacak ObjectDataSource `CategoriesBLL` s sınıfı `GetCategories` yöntemi; aşağı açılan listeler UPDATE ve DELETE sekmeler (hiçbiri) ve tıklatın kümesi son Sihirbazı tamamlayın. Son olarak, DropDownList görüntü olması `CategoryName` veri alan ve kullanım `CategoryID` değeri olarak.

Bu iki DropDownLists eklenir ve uygun şekilde yapılandırılmış ObjectDataSources bağlı sonra ekranınızın Şekil 9'a benzer görünmelidir.


[![Üstbilgi satırındaki şimdi üreticiler ve kategorileri DropDownLists içerir](batch-inserting-cs/_static/image26.png)](batch-inserting-cs/_static/image25.png)

**Şekil 9**: üstbilgisi satır şimdi içerir `Suppliers` ve `Categories` DropDownLists ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-inserting-cs/_static/image27.png))


Biz şimdi adı ve yeni her ürün için fiyat toplamak için metin kutuları oluşturmanız gerekir. TextBox denetimi tasarımcıya araç her beş ürün adı ve fiyat satır sürükleyin. Ayarlama `ID` kutularındaki özelliklerini `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`ve benzeri.

Her birim fiyat ayarı metin kutuları, sonra bir CompareValidator ekleyin `ControlToValidate` özelliğine uygun `ID`. Ayrıca `Operator` özelliğine `GreaterThanEqual`, `ValueToCompare` 0 olarak ve `Type` için `Currency`. Bu ayarları fiyat girdiyseniz, büyük veya sıfıra eşit bir geçerli para birimi değeri olduğundan emin olmak için CompareValidator isteyin. Ayarlama `Text` özelliğine \*, ve `ErrorMessage` fiyata değerinden büyük veya sıfıra eşit olmalıdır. Ayrıca, lütfen hiçbir para birimi simgesini yok sayın.

> [!NOTE]
> Ekleme arabirimi tüm RequiredFieldValidator denetimlerinin rağmen içermeyen `ProductName` alanındaki `Products` veritabanı tablosu izin verme `NULL` değerleri. En fazla beş ürünleri girmesine olanak istiyoruz olmasıdır. Kullanıcı için ilk üç satır ürün adı ve birim fiyat sağlamak için olsaydı, örneğin, son iki satırını boş bırakarak d şu üç yeni ürünler sisteme eklemeniz yeterlidir. Bu yana `ProductName` olan gerekli, ancak biz olması durumunda bir birim fiyat emin olmak için girilen karşılık gelen bir ürün adı değeri sağladığınız programlı olarak denetlemek gerekir. Biz bu onay adım 4'te üstesinden gelmek.


Değer bir para birimi simgesini içeriyorsa s kullanıcı girişini doğrulama olduğunda CompareValidator geçersiz veri bildirir. Her birim fiyat para birimi simgesini fiyatı girerken atlamak için kullanıcı yönlendiren bir görsel ipucu hizmet vermek için metin kutuları önünde $ ekleyin.

Son olarak, içinde bir ValidationSummary denetimi ekleme `InsertingInterface` paneli, ayarları kendi `ShowMessageBox` özelliğine `true` ve kendi `ShowSummary` özelliğine `false`. Bu ayarlara sahip kullanıcı geçersiz birim fiyat değeri girerse, soruna neden olan TextBox denetimleri yanında bir yıldız işareti görünür ve daha önce belirtilen hata iletisini gösteren bir istemci-tarafı messagebox ValidationSummary görüntüler.

Bu noktada, ekranınızın Şekil 10'a benzer görünmelidir.


[![Ekleme arabirim, ürünler için metin kutuları içerir adları ve fiyatlarını](batch-inserting-cs/_static/image29.png)](batch-inserting-cs/_static/image28.png)

**Şekil 10**: ekleme arabirimi şimdi içeren metin kutuları fiyatları ve ürün adları için ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-inserting-cs/_static/image30.png))


Sonraki biz altbilgi satırın sevkiyat ve İptal düğmeleri ürün ekleme eklemeniz gerekir. Sürükleme iki düğmesi denetimleri Araç Kutusu'ndan ekleme arabirimi altbilgi ayarlama düğmeleri `ID` özelliklerine `AddProducts` ve `CancelButton` ve `Text` ürünler sevkiyat ve iptal, sırasıyla eklemek için özellikler. Buna ek olarak, `CancelButton` denetim s `CausesValidation` özelliğine `false`.

Son olarak, size iki arabirimleri için durum iletilerini görüntüler bir etiket Web denetimi eklemeniz gerekir. Kullanıcı başarıyla ürünlerin yeni bir sevkiyat eklediğinde, örneğin, görüntü arabirimine döner ve bir onay iletisi görüntüler istiyoruz. Ancak, ürün adı devre dışı bırakır, ancak yeni bir ürün için bir fiyat kullanıcı sağlarsa, bu yana bir uyarı iletisi görüntülenecek ihtiyacımız `ProductName` alan gereklidir. Bu ileti için her iki arabirimde görüntülemek için ihtiyacımız olduğundan, sayfanın üst kısmındaki panelleri dışında yerleştirin.

Bir etiket Web denetimi sayfanın üst kısmındaki Tasarımcısı'nda Araç Kutusu'ndan sürükleyin. Ayarlama `ID` özelliğine `StatusLabel`, kullanıma açık `Text` özelliği ve kümesi `Visible` ve `EnableViewState` özelliklerine `false`. Önceki eğitimlerine anlatıldığı gibi ayarı `EnableViewState` özelliğine `false` bize programlı olarak etiket s özellik değerlerini değiştirmek ve bunları otomatik olarak geri sonraki geri gönderme kendi varsayılanlarına geri dönmek izin verir. Bu yanıt sonraki geri göndermede kaybolur bazı kullanıcı eylemi olarak bir durum iletisi gösteren kodunu kolaylaştırır. Son olarak, ayarlamak `StatusLabel` denetim s `CssClass` bir CSS sınıfı adı uyarı özelliğine tanımlanan `Styles.css` büyük, italik, kalın, kırmızı yazı tipiyle metni görüntüler.

Şekil 11 etiketi ekledikten ve yapılandırılmış sonra Visual Studio tasarımcısı gösterir.


[![İki Panel denetimleri yukarıda StatusLabel'i denetimi yerleştirin](batch-inserting-cs/_static/image32.png)](batch-inserting-cs/_static/image31.png)

**Şekil 11**: yer `StatusLabel` denetim Yukarıdaki iki Panel denetimleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-inserting-cs/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>3. adım: görüntü arasında geçiş yapma ve arabirimleri ekleme

Bu noktada biz bizim görüntüleme ve ekleme arabirimleri, ancak biz re hala iki görevlerle sol işaretleme tamamladınız:

- Görüntü arasında geçiş yapma ve arabirimleri ekleme
- Veritabanı sevkiyata ürünleri ekleme

Şu anda, görüntü arabirimdir görünür, ancak ekleme arabirimi gizlenir. Bunun nedeni, `DisplayInterface` Masası s `Visible` özelliği ayarlanmış `true` (varsayılan değer) sırada `InsertingInterface` Masası s `Visible` özelliği ayarlanmış `false`. Yalnızca ihtiyacımız her denetim s geçiş yapmak için iki arabirim arasında geçiş yapmak için `Visible` özellik değeri.

İşlem ürün sevkiyat düğmesine tıklandığında görüntü arabiriminden ekleme arabirimine taşımak istiyoruz. Bu nedenle, bu düğme s'için bir olay işleyicisi oluşturun `Click` aşağıdaki kodu içeren olay:


[!code-csharp[Main](batch-inserting-cs/samples/sample4.cs)]

Bu kod yalnızca gizler `DisplayInterface` paneli ve gösterir `InsertingInterface` paneli.

Ardından, olay işleyicileri ekleme arabiriminde sevkiyat ve iptal düğmesi denetimleri ekleme ürünlerinden oluşturun. Bu düğmeleri birini tıklandığında, görüntü arabirimi geri gerekir. Oluşturma `Click` her ikisi için olay işleyicileri düğmesi denetler çağrıda buldukları `ReturnToDisplayInterface`, kısa bir süre içinde eklenecektir bir yöntem. Gizleme yanı sıra `InsertingInterface` paneli ve gösteren `DisplayInterface` paneli, `ReturnToDisplayInterface` yöntemi gereken Web denetimleri önceden düzenleme durumlarına geri dönün. Bu DropDownLists ayarı içerir `SelectedIndex` 0 ve temizleme özelliklerine `Text` TextBox denetimleri özelliklerini.

> [!NOTE]
> Ne gerçekleşebilir düşünün, biz etmedi t dönüş denetimleri önceden düzenleme durumlarına görüntü arabirimine döndürmeden önce. Bir kullanıcı, işlem ürün sevkiyat düğmesini tıklatın, ürünleri yapılan sevkiyat girin ve ürünleri sevkiyat Ekle'ye tıklayın. Bu ürünleri ekleyin ve kullanıcının görünen arabirimine döndürür. Bu noktada kullanıcı başka bir sevkiyat eklemek isteyebilirsiniz. Ekleme arabirimi ancak DropDownList döndürecektir işlem ürün sevkiyat düğme tıklatıldığında seçim ve metin değerleri hala önceki değerlerine ile doldurulması.


[!code-csharp[Main](batch-inserting-cs/samples/sample5.cs)]

Her ikisi de `Click` olay işleyicileri yalnızca çağrısı `ReturnToDisplayInterface` yöntemi, ürün ekleme yapılan sevkiyat getireceğiz rağmen `Click` olay işleyicisini adım 4 ve ürünleri kaydetmek için kodu ekleyin. `ReturnToDisplayInterface` döndürerek başlatır `Suppliers` ve `Categories` DropDownLists kendi ilk seçenekleri. İki sabit `firstControlID` ve `lastControlID` işareti başlangıç ve bitiş ekleme içinde kutularındaki arayüzü ve sınırları içinde kullanılan ürün adı ve birim fiyat adlandırma kullanılan denetim dizin değerlerini `for` Ayarlardöngü`Text`TextBox denetimleri özelliklerini geri boş bir dize. Son olarak, paneller `Visible` Özellikler ekleme arabirimi gizlenmesi ve gösterilen görüntü arabirimi sıfırlanır.

Bu sayfayı bir tarayıcıda çıkışı test etmek için bir dakikanızı ayırın. İlk sitesini ziyaret ettiğinde görüntü arabirimi Şekil 5'te gösterildiği gibi görmeniz gerekir. İşlem ürün sevkiyat düğmesini tıklatın. Sayfaya geri gönderilir ve şimdi ekleme arabirimi Şekil 12'de gösterildiği gibi görmeniz gerekir. Her iki eklemek ürünlerinden sevkiyat veya İptal düğmeleri tıklatarak görüntü arabirimine döndürür.

> [!NOTE]
> Ekleme arabirimi görüntülerken, birim fiyat kutularındaki CompareValidators çıkışı test etmek için bir dakikanızı ayırın. Ürün ekleme sevkiyat düğmesi geçersiz para birimi değerleri ile veya bir değer sıfırdan fiyatlarıyla tıklatıldığında uyarı istemci-tarafı messagebox görmeniz gerekir.


[![İşlem ürün sevkiyat düğmesine tıkladıktan sonra ekleme arabirimi görüntülenir](batch-inserting-cs/_static/image35.png)](batch-inserting-cs/_static/image34.png)

**Şekil 12**: ekleme arabirimi işlem ürün sevkiyat düğmesine tıkladıktan sonra görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-inserting-cs/_static/image36.png))


## <a name="step-4-adding-the-products"></a>4. adım: ürün ekleme

Bu öğretici ürünleri ürün ekleme veritabanında sevkiyat düğmesi s'den kaydetmek için kalan tüm `Click` olay işleyicisi. Bu oluşturarak gerçekleştirilebilir bir `ProductsDataTable` ve ekleyerek bir `ProductsRow` sağlanan ürün adlarının her biri için örneği. Bir kez bunlar `ProductsRow` s biz yapılan bir çağrı yapacak eklenmiştir `ProductsBLL` s sınıfı `UpdateWithTransaction` tümleştirilmesidir yöntemi `ProductsDataTable`. Sözcüğünün `UpdateWithTransaction` geri oluşturulan yöntemi [kaydırma veritabanı değişiklikleri bir işlem içinde](wrapping-database-modifications-within-a-transaction-cs.md) Öğreticisi, geçişleri `ProductsDataTable` için `ProductsTableAdapter` s `UpdateWithTransaction` yöntemi. Buradan, bir ADO.NET hareket başlatıldığından ve TableAdatper sorunları bir `INSERT` her eklenen için veritabanı deyimi `ProductsRow` DataTable tablosundaki. İşlem hata eklenen tüm ürünleri varsayıldığında, aksi takdirde, geri alınır.

Sevkiyat düğmesi s ürün ekleme kodunu `Click` olay işleyicisi biraz hata denetimi gerçekleştirmek de gerekir. Ekleme arabiriminde kullanılan hiçbir RequiredFieldValidators olduğundan, bir kullanıcı adı içermeden bir ürün için bir fiyat girebilirsiniz. Ürün s adı gerektiğinden bir koşul açılan biz kullanıcıyı uyarmak ve ile ekler devam değil gerekir. Tam `Click` olay işleyici kod izler:


[!code-csharp[Main](batch-inserting-cs/samples/sample6.cs)]

Olay işleyicisi, sağlayarak başlatır `Page.IsValid` özellik değeri döndürür `true`. Döndürürse `false`, ardından bir anlamına CompareValidators birkaçını geçersiz veri raporlama; böyle bir durumda biz girilen Ürün eklemeye çalışırsanız istemiyorsanız veya şu özel durumla kullanıcı tarafından girilen birim fiyat atamak çalışırken elde edersiniz değerin `ProductsRow` s `UnitPrice` özelliği.

Sonraki, yeni bir `ProductsDataTable` örneği oluşturulur (`products`). A `for` döngü, ürün adı ve birim fiyat kutularındaki yinelemek için kullanılır ve `Text` özelliklerini yerel değişkenler okuma `productName` ve `unitPrice`. Birim Fiyat ancak karşılık gelen ürün adı için bir değer kullanıcının girdiği varsa `StatusLabel` bir birim fiyat sağlarsanız, ileti gerekir de içerir ürünün adını ve olay işleyicisi çıkıldı.

Bir ürün adı, yeni bir belirtildiyse `ProductsRow` örneği kullanılarak oluşturulan `ProductsDataTable` s `NewProductsRow` yöntemi. Bu yeni `ProductsRow` örneği s `ProductName` özelliği için geçerli ürün ayarlanmış TextBox sırasında ad `SupplierID` ve `CategoryID` özellikleri atanır `SelectedValue` ekleme arabirimi s üstbilgisinde DropDownLists özelliklerini. Kullanıcı için ürün s Fiyat değeri girdiyseniz, atandığı `ProductsRow` örneği s `UnitPrice` özellik; Aksi takdirde özelliğidir sonuçlanır atanmamış, sol bir `NULL` değerini `UnitPrice` veritabanındaki. Son olarak, `Discontinued` ve `UnitsOnOrder` özellikler için sabit kodlanmış değerler atanır `false` ve 0, sırasıyla.

Özellikler için atandıktan sonra `ProductsRow` eklenir örneği `ProductsDataTable`.

Tamamlandığında `for` döngü, biz denetleyin ürünlerden eklenip eklenmediğini. Kullanıcı tüm ürün adlarını veya fiyatları girmeden önce sevkiyat eklemek ürünlerinden tıklattınız olabilir. En az bir üründe olduğunda `ProductsDataTable`, `ProductsBLL` s sınıfı `UpdateWithTransaction` yöntemi çağrılır. Ardından, veriler için DataSet'e `ProductsGrid` GridView böylece yeni eklenen ürünleri görüntü arabiriminde görüntülenir. `StatusLabel` Bir onay iletisi görüntülenecek güncelleştirilir ve `ReturnToDisplayInterface` , arabirim ekleme ve görüntüleme arabirimi gösteren gizleme çağrılır.

Hiç ürün girilirse, ekleme arabirimi hiç ürün eklenen ileti görüntülenmeye devam eder. Lütfen ürün adlarını girin ve birim fiyatları kutularındaki görüntülenir.

Şekil s 13, 14 ve 15 ekleme Göster ve arabirimleri eylemi görüntüler. Şekil 13'te kullanıcı karşılık gelen bir ürün adı olmayan bir birim fiyat değer girmiştir. Şekil 14 görüntü arabirim üç sonra yeni Şekil 15 iki yeni eklenen ürünlerin (üçüncü bir önceki sayfada) GridView içinde gösterirken ürünler başarıyla eklendi gösterir.


[![Bir ürün adı gerekli olduğunda girilmesi birim fiyat olur](batch-inserting-cs/_static/image38.png)](batch-inserting-cs/_static/image37.png)

**Şekil 13**: bir ürün adı olan gerekli olduğunda girerek bir birim fiyat ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-inserting-cs/_static/image39.png))


[![Üç yeni Veggies tedarikçi için eklenmiştir Mayumi s](batch-inserting-cs/_static/image41.png)](batch-inserting-cs/_static/image40.png)

**Şekil 14**: üç yeni Veggies eklenmiştir tedarikçi Mayumi s ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-inserting-cs/_static/image42.png))


[![Yeni ürünler GridView son sayfasında bulunabilir.](batch-inserting-cs/_static/image44.png)](batch-inserting-cs/_static/image43.png)

**Şekil 15**: yeni ürünler bulunabilir GridView son sayfasına ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-inserting-cs/_static/image45.png))


> [!NOTE]
> Bu öğreticide kullanılan mantık ekleme toplu işlem kapsamı içinde ekler sarmalar. Bunu doğrulamak için bir veritabanı düzeyi hatası var tanıtır. Örneğin, yeni atama yerine `ProductsRow` örneği s `CategoryID` seçili değerine özellik `Categories` DropDownList, bir değere ister Ata `i * 5`. Burada `i` döngü dizin oluşturucu olup 1 ile 5 arasında değişen değerler içerir. Bu nedenle, ne zaman iki veya daha fazla ürün toplu işlemindeki ilk Ürün Ekle ekleme olacaktır geçerli bir `CategoryID` değeri (5), ancak sonraki ürünleri olacaktır `CategoryID` en fazla ile eşleşmiyor değerleri `CategoryID` değerler `Categories` tablo. Net etkisi olan sırasında ilk `INSERT` başarılı olur, sonraki olanları bir yabancı anahtar kısıtlaması ihlali ile başarısız olur. Toplu yerleştirme atomik, olduğundan ilk `INSERT` toplu eklemeden önce işlem durumu veritabanına başlangıcından döndürme geri alınacak.


## <a name="summary"></a>Özet

Bu ve önceki iki öğreticileri güncelleştirmek için silme, izin arabirimleri oluşturduk ve toplu veri ekleme, her biri için veri erişim katmanı'ndaki eklediğimiz işlem desteği kullanılan [veritabanı değişiklikleri sarmalama bir işlem içinde](wrapping-database-modifications-within-a-transaction-cs.md) Öğreticisi. Belirli senaryolar, bu tür toplu işleme kullanıcı arabirimleri büyük ölçüde son kullanıcı verimliliğini keserek temel alınan veri bütünlüğünü de koruyarak tıklama, Geri göndermeler ve klavye ve fare içerik geçişi sayısını artırmak.

Bu öğretici toplu verileri ile çalışma bizim göz tamamlar. Öğreticiler bir sonraki kümesini DAL, şifreleme bağlantı dizeleri ve daha fazla bağlantı ve komut düzeyi ayarlarını yapılandırma TableAdapter s yöntemler saklı yordamları kullanma dahil olmak üzere, Gelişmiş Veri erişim katmanı senaryoları çeşitli araştırır!

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Gözden geçirenler Hilton Giesenow ve S ren Jacob Lauritsen olan Bu öğretici için yol açar. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](batch-deleting-cs.md)
> [sonraki](wrapping-database-modifications-within-a-transaction-vb.md)
